---
title: Tworzenie przezroczystej bramy za pomocą usługi Azure IoT Edge — Windows | Dokumentacja firmy Microsoft
description: Umożliwia tworzenie przezroczystej bramy, która pozwala na przetwarzanie informacji dla wielu urządzeń w usłudze Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/20/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: f584e2cdcb038c6f8e9fcdbeecc22fb957bd7f8d
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394818"
---
# <a name="create-a-windows-iot-edge-device-that-acts-as-a-transparent-gateway"></a>Utwórz urządzenie Windows IoT Edge, która pełni rolę przezroczystej bramy

Ten artykuł zawiera szczegółowe instrukcje dotyczące korzystania z urządzenia usługi IoT Edge rolę przezroczystej bramy. W pozostałej części tego artykułu termin *brama usługi IoT Edge* odwołuje się do urządzenia usługi IoT Edge używany jako przezroczystej bramy. Aby uzyskać więcej informacji, zobacz [jak IoT Edge urządzenia mogą być używane jako brama](./iot-edge-as-gateway.md), który zawiera omówienie pojęć.

>[!NOTE]
>Obecnie:
> * Jeśli brama zostanie odłączony od usługi IoT Hub, podrzędnym urządzenia nie mogą uwierzytelniać za pomocą bramy.
> * Urządzenia z włączoną obsługą usługi Edge nie można nawiązać połączenia bramy usługi IoT Edge. 
> * Podrzędne urządzenia nie mogą używać przekazywania plików.

Trudnym o tworzeniu przezroczystej bramy jest możliwość bezpiecznego łączenia z bramy do podrzędnego urządzeń. Usługa Azure IoT Edge umożliwia użycie infrastruktury kluczy publicznych do skonfigurowania nawiązywać bezpieczne połączenia TLS między tymi urządzeniami. W tym przypadku możemy zezwolenie podrzędnym urządzenia połączyć się z urządzenia usługi IoT Edge, działając jako przezroczystej bramy.  Aby zachować bezpieczeństwo uzasadnione, podrzędne urządzenia należy się upewnić tożsamość urządzenia usługi Edge, ponieważ mają tylko urządzenia nawiązywania połączenia z bramami i potencjalnie złośliwych bramy.

Można utworzyć żadnej infrastruktury certyfikatów, umożliwiająca zaufania wymagane dla topologii urządzenia bramy. W tym artykule przyjęto założenie, że tę samą konfigurację certyfikatu, który zostanie wykorzystany do włączenia [zabezpieczeń urzędu certyfikacji X.509](../iot-hub/iot-hub-x509ca-overview.md) w usłudze IoT Hub, który obejmuje certyfikat X.509 urzędu certyfikacji, powiązanych z określonej usługi IoT hub (IoT hub właściciel urzędu certyfikacji) i serii certyfikatów, zarejestrowana za pomocą tego urzędu certyfikacji i urzędu certyfikacji do urządzenia usługi Edge.

![Instalator bramy](./media/how-to-create-transparent-gateway/gateway-setup.png)

Brama przedstawia swój certyfikat urzędu certyfikacji urządzenia Edge na urządzeniu podrzędnego podczas inicjowania połączenia. Podrzędne urządzenie sprawdza upewnij się, że certyfikat urzędu certyfikacji urządzenia Edge jest podpisany przez właściciela certyfikatu urzędu certyfikacji. Ten proces umożliwia podrzędnym urządzenia upewnić się, że brama pochodzi z zaufanego źródła.

W poniższych krokach objaśniono proces tworzenia certyfikatów i instalowania ich w odpowiednich miejscach.

## <a name="prerequisites"></a>Wymagania wstępne
1. [Zainstalować środowisko uruchomieniowe usługi Azure IoT Edge](./how-to-install-iot-edge-windows-with-windows.md) na urządzeniu z systemem Windows ma być używany jako przezroczystej bramy.

1. Pobierz biblioteki OpenSSL dla Windows. Istnieje wiele sposobów, należy zainstalować protokół OpenSSL:

   >[!NOTE]
   >Jeśli masz już zainstalowane na urządzeniu z systemem Windows biblioteki OpenSSL, może pominąć ten krok, ale upewnij się, że `openssl.exe` jest dostępna w Twojej `%PATH%` zmiennej środowiskowej.

   * Pobierz i zainstaluj dowolne [plików binarnych biblioteki OpenSSL firm](https://wiki.openssl.org/index.php/Binaries), na przykład z [tego projektu na SourceForge](https://sourceforge.net/projects/openssl/).
   
   * Pobierz kod źródłowy biblioteki OpenSSL i kompilowania plików binarnych na komputerze, samodzielnie lub za pośrednictwem [vcpkg](https://github.com/Microsoft/vcpkg). Zgodnie z instrukcjami podanymi poniżej umożliwia vcpkg pobierania kodu źródłowego, kompilacji i zainstaluj OpenSSL na komputerze Windows za pomocą prostych krokach.

      1. Przejdź do katalogu, w którym chcesz zainstalować vcpkg. W tym miejscu na będzie nazywamy to $VCPKGDIR. Postępuj zgodnie z instrukcjami, aby pobrać i zainstalować [vcpkg](https://github.com/Microsoft/vcpkg).
   
      1. Po zainstalowaniu vcpkg w wierszu polecenia programu powershell uruchom następujące polecenie, aby zainstalować pakiet biblioteki OpenSSL dla Windows x64. Instalacja trwa zwykle około 5 minut.

         ```PowerShell
         .\vcpkg install openssl:x64-windows
         ```
      1. Dodaj `$VCPKGDIR\installed\x64-windows\tools\openssl` do Twojej `PATH` zmiennej środowiskowej, aby `openssl.exe` plik jest dostępny do wywołania.

1. Przejdź do katalogu, w którym chcesz pracować. W tym miejscu na będzie nazywamy to $WRKDIR.  Wszystkie pliki zostaną utworzone w tym katalogu.
   
   ciągłe dostarczanie $WRKDIR

1.  Uzyskaj skrypty w celu wygenerowania wymaganych certyfikatów nieprodukcyjnych przy użyciu następującego polecenia. Te skrypty pomocne podczas tworzenia wymagane certyfikaty, aby skonfigurować przezroczystej bramy.

      ```PowerShell
      git clone https://github.com/Azure/azure-iot-sdk-c.git
      ```

1. Skopiuj pliki konfiguracji i skrypt w katalogu roboczym. Ponadto można ustawić zmiennej env OPENSSL_CONF przy użyciu pliku konfiguracji openssl_root_ca.cnf.

   ```PowerShell
   copy azure-iot-sdk-c\tools\CACertificates\*.cnf .
   copy azure-iot-sdk-c\tools\CACertificates\ca-certs.ps1 .
   $env:OPENSSL_CONF = "$PWD\openssl_root_ca.cnf"
   ```

1. Włącz uruchamianie skryptów, uruchamiając następujące polecenie programu PowerShell

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy Unrestricted
   ```

1. Przenieś funkcje używane przez skrypty w globalnej przestrzeni nazw programu PowerShell, funkcja dot-sourcing za pomocą następującego polecenia
   
   ```PowerShell
   . .\ca-certs.ps1
   ```

1. Upewnij się, że została poprawnie zainstalowana OpenSSL i upewnij się, że nie będzie konfliktów nazw z istniejących certyfikatów, uruchamiając następujące polecenie. Jeśli wystąpią problemy, skrypt powinna opisywać sposób rozwiązać problemy w systemie.

   ```PowerShell
   Test-CACertsPrerequisites
   ```

## <a name="certificate-creation"></a>Tworzenie certyfikatu
1. Utwórz certyfikat urzędu certyfikacji właściciela i jeden certyfikat pośredniego. Certyfikaty są umieszczane w `$WRKDIR`.

      ```PowerShell
      New-CACertsCertChain rsa
      ```

1. Utwórz certyfikat urzędu certyfikacji urządzenia usługi Edge i klucza prywatnego za pomocą poniższego polecenia.

   >[!NOTE]
   > **NIE** Użyj nazwy, która jest taka sama jak nazwa hosta DNS bramy. To spowoduje, że certyfikatu klienta dla tych certyfikatów, nie powiedzie się.

   ```PowerShell
   New-CACertsEdgeDevice "<gateway device name>"
   ```

## <a name="certificate-chain-creation"></a>Tworzenie łańcucha certyfikatu
Tworzenie łańcucha certyfikatów od właściciela certyfikatu urzędu certyfikacji, pośredniego certyfikatu i certyfikat urzędu certyfikacji urządzeń brzegowych za pomocą poniższego polecenia. Umieszczenie ich w pliku łańcucha pozwala łatwo zainstalować na urządzeniu usługi Edge działający jako przezroczystej bramy.

   ```PowerShell
   Write-CACertsCertificatesForEdgeDevice "<gateway device name>"
   ```

   Skrypt tworzy następujące certyfikaty i klucza:
   * `$WRKDIR\certs\new-edge-device.*`
   * `$WRKDIR\private\new-edge-device.key.pem`
   * `$WRKDIR\certs\azure-iot-test-only.root.ca.cert.pem`

## <a name="installation-on-the-gateway"></a>Instalacja bramy
1. Skopiuj następujące pliki z $WRKDIR dowolne miejsce na urządzeniu usługi Edge, będziemy odnosić się do tego jako $CERTDIR. Jeśli na urządzeniu usługi Edge jest generowane certyfikaty, Pomiń ten krok.

   * Certyfikat dostępu Warunkowego do urządzeń —  `$WRKDIR\certs\new-edge-device-full-chain.cert.pem`
   * Klucz prywatny urzędu certyfikacji urządzenia — `$WRKDIR\private\new-edge-device.key.pem`
   * Właściciel CA- `$WRKDIR\certs\azure-iot-test-only.root.ca.cert.pem`

2. Ustaw `certificate` właściwości w pliku yaml konfiguracji demona zabezpieczeń do ścieżki gdzie umieścić pliki certyfikatu i klucza.

```yaml
certificates:
  device_ca_cert: "$CERTDIR\\certs\\new-edge-device-full-chain.cert.pem"
  device_ca_pk: "$CERTDIR\\private\\new-edge-device.key.pem"
  trusted_ca_certs: "$CERTDIR\\certs\\azure-iot-test-only.root.ca.cert.pem"
```
## <a name="deploy-edgehub-to-the-gateway"></a>Wdrażanie EdgeHub do bramy
Jedną z najważniejszych funkcji usługi Azure IoT Edge jest możliwość wdrażania modułów na urządzeniach usługi IoT Edge z poziomu chmury. Ta sekcja zawiera podczas tworzenia wdrożenia pozornie pusty; Jednak Centrum usługi Edge jest automatycznie dodawany do wszystkich wdrożeń, nawet jeśli nie mają żadnych modułów obecne. Centrum usługi Edge jest tylko moduł, który należy na urządzeniu usługi Edge ona pełnić rolę przezroczystej bramy, dzięki czemu Tworzenie pustego wdrożenia jest wystarczająca. 
1. W witrynie Azure Portal przejdź do centrum IoT Hub.
2. Przejdź do **usługi IoT Edge** i wybierz urządzenia usługi IoT Edge, która ma być używany jako brama.
3. Wybierz pozycję **Ustaw moduły**.
4. Wybierz opcję **Dalej**.
5. W kroku **Określanie tras** powinna być widoczna domyślna trasa, która wysyła wszystkie komunikaty ze wszystkich modułów do centrum IoT Hub. Jeśli tak nie jest, dodaj następujący kod, a następnie wybierz przycisk **Dalej**.
   ```JSON
   {
       "routes": {
           "route": "FROM /* INTO $upstream"
       }
   }
   ```
6. W kroku szablon recenzji wybierz **przesyłania**.

## <a name="installation-on-the-downstream-device"></a>Instalacja na urządzeniu podrzędne
Podrzędne urządzenie może pozostawać w dowolnej aplikacji przy użyciu [zestaw SDK urządzeń Azure IoT](../iot-hub/iot-hub-devguide-sdks.md), takie jak proste co opisano w [Podłącz urządzenie do Centrum IoT hub przy użyciu platformy .NET](../iot-hub/quickstart-send-telemetry-dotnet.md). Aplikacji urządzenia podrzędnego musi ufać **właściciela urzędu certyfikacji** certyfikatu w celu weryfikowania połączeń TLS do urządzenia bramy. Zazwyczaj ten krok można wykonać na dwa sposoby: na poziomie systemu operacyjnego lub (w przypadku niektórych języków) na poziomie aplikacji.

### <a name="os-level"></a>Poziom systemu operacyjnego
Instalacji tego certyfikatu w magazynie certyfikatów systemu operacyjnego pozwoli wszystkie aplikacje, aby użyć właściciela certyfikatu urzędu certyfikacji jako zaufanego certyfikatu.

* Ubuntu - poniżej przedstawiono przykładowy sposób zainstalować certyfikat urzędu certyfikacji na hoście systemu Ubuntu.

   ```cmd
   sudo cp $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem  /usr/local/share/ca-certificates/azure-iot-test-only.root.ca.cert.pem.crt
   sudo update-ca-certificates
   ```
 
    Powinien zostać wyświetlony komunikat z informacją "Aktualizowanie certyfikaty /etc/ssl/certs... Dodano 1, 0 usunięte; Gotowe".

* Windows — poniżej przedstawiono przykładowy sposób zainstalować certyfikat urzędu certyfikacji na hoście Windows.
  1. W menu start wpisz "Zarządzanie komputerem certyfikaty". To powinno wyświetlić narzędziem o nazwie `certlm`.
  2. Przejdź do **certyfikatów komputera lokalnego** > **zaufanych certyfikatów głównych** > **certyfikaty** > kliknij prawym przyciskiem myszy > **Wszystkie zadania** > **zaimportować** można uruchomić Kreatora importu certyfikatów.
  3. Postępuj zgodnie z instrukcjami, zgodnie z instrukcją i zaimportuj $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem pliku certyfikatu.
  4. Po zakończeniu wyświetlony komunikat "Pomyślnie zaimportowane".

### <a name="application-level"></a>Poziom aplikacji
W przypadku aplikacji .NET można dodać poniższy fragment kodu można traktować jako zaufany certyfikat w formacie PEM. Zainicjować zmienną `certPath` z `$CERTDIR\certs\azure-iot-test-only.root.ca.cert.pem`.

   ```
   using System.Security.Cryptography.X509Certificates;

   ...

   X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
   store.Open(OpenFlags.ReadWrite);
   store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
   store.Close();
   ```

## <a name="connect-the-downstream-device-to-the-gateway"></a>Podłącz urządzenie podrzędnego do bramy
Inicjowanie zestawu sdk urządzenia usługi IoT Hub przy użyciu parametrów połączenia, odnoszące się do nazwy hosta urządzenia bramy. Jest to realizowane przez dołączenie `GatewayHostName` właściwość parametrów połączenia urządzenia. Na przykład poniżej przedstawiono przykładowe parametry połączenia dla urządzenia dla urządzenia, do której firma Microsoft dołączany `GatewayHostName` właściwości:

   ```
   HostName=yourHub.azure-devices.net;DeviceId=yourDevice;SharedAccessKey=XXXYYYZZZ=;GatewayHostName=mygateway.contoso.com
   ```

   >[!NOTE]
   >To przykładowe polecenie, które testy, które wszystko, co zostało skonfiguruj poprawnie. Możesz sohuld powiedzenie komunikat "zweryfikowano OK".
   >
   >openssl s_client-connect - CAfile mygateway.contoso.com:8883 $CERTDIR/certs/azure-iot-test-only.root.ca.cert.pem - showcerts

## <a name="routing-messages-from-downstream-devices"></a>Routing komunikatów z urządzeń podrzędne
Komunikaty wysyłane z urządzeń podrzędne, podobnie jak komunikaty wysyłane przez moduły można kierować do środowiska uruchomieniowego usługi IoT Edge. Pozwala na przeprowadzanie analiz w module uruchomiona na bramie przed wysłaniem danych do chmury. Poniżej trasy będzie służyć do wysyłania komunikatów z urządzenia podrzędnego o nazwie `sensor` z nazwą modułu `ai_insights`.

   ```json
   { "routes":{ "sensorToAIInsightsInput1":"FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/ai_insights/inputs/input1\")", "AIInsightsToIoTHub":"FROM /messages/modules/ai_insights/outputs/output1 INTO $upstream" } }
   ```

Aby uzyskać więcej informacji na temat routingu wiadomości zobacz [budowy modułu](./module-composition.md).

[!INCLUDE [iot-edge-extended-ofline-preview](../../includes/iot-edge-extended-offline-preview.md)]

## <a name="next-steps"></a>Kolejne kroki
[Zrozumienie wymagań i narzędzia do tworzenia modułów usługi IoT Edge](module-development.md).
