---
title: Samouczek pobieranie danych przy użyciu usługi Azure API raportowania usługi AD z certyfikatami | Dokumentacja firmy Microsoft
description: W tym samouczku opisano sposób użycia usługi Azure AD interfejsu API raportowania przy użyciu poświadczeń certyfikatu w celu pobrania danych z katalogów bez interwencji użytkownika.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: report-monitor
ms.date: 05/07/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 5c54af76fc1e145ea062c6bcb37f354a7de94781
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364180"
---
# <a name="tutorial-get-data-using-the-azure-active-directory-reporting-api-with-certificates"></a>Samouczek: Pobieranie danych przy użyciu usługi Azure Active Directory, interfejsu API raportowania przy użyciu certyfikatów

[Interfejsy API raportowania usługi Azure Active Directory (Azure AD)](concept-reporting-api.md) umożliwiają dostęp programowy do danych za pomocą zestawu interfejsów API opartych na architekturze REST. Te interfejsy API można wywoływać przy użyciu różnych języków i narzędzi do programowania. Jeśli chcesz uzyskać dostęp usługi Azure AD Reporting API bez interwencji użytkownika, należy skonfigurować dostęp do używania certyfikatów.

W tym samouczku dowiesz się, jak utworzyć certyfikat testowy i umożliwia dostęp do interfejsu API programu Graph MS dla raportowania. Nie zaleca się przy użyciu certyfikatu testowego w środowisku produkcyjnym. 

## <a name="prerequisites"></a>Wymagania wstępne

1. Najpierw wykonaj [wymagania wstępne dotyczące dostępu do usługi Azure Active Directory reporting API](howto-configure-prerequisites-for-reporting-api.md). 

2. Pobierz i zainstaluj [usługi Azure AD PowerShell V2](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/docs-conceptual/azureadps-2.0/install-adv2.md).

3. Zainstaluj [MSCloudIdUtils](https://www.powershellgallery.com/packages/MSCloudIdUtils/). Ten moduł zapewnia kilka poleceń cmdlet narzędzi, w tym:
    - Biblioteki ADAL, służące do uwierzytelniania
    - Tokeny dostępu użytkownika, kluczy aplikacji i certyfikatów korzystających z bibliotek ADAL
    - Stronicowane wyniki obsługi interfejsu API programu Graph

4. Jeśli po raz pierwszy przy użyciu modułu uruchamiania **MSCloudIdUtilsModule instalacji**, w przeciwnym razie importować go za pomocą **Import-Module** polecenia programu Powershell.

Sesja powinna wyglądać podobnie do tego ekranu:

  ![Windows Powershell](./media/tutorial-access-api-with-certificates/module-install.png)
  
## <a name="create-a-test-certificate"></a>Utwórz certyfikat testowy

1. Użyj **New-SelfSignedCertificate** polecenia cmdlet programu Powershell do utworzenia certyfikatu testowego.

   ```
   $cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
   ```

2. Użyj **eksportu certyfikatu** polecenia cmdlet, aby wyeksportować je do pliku certyfikatu.

   ```
   Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"

   ```

## <a name="register-the-certificate-in-your-app"></a>Zarejestruj certyfikat w swojej aplikacji

1. Przejdź do [witryny Azure portal](https://portal.azure.com), wybierz opcję **usługi Azure Active Directory**, a następnie wybierz **rejestracje aplikacji** i wybierz aplikację z listy. 

2. Wybierz **ustawienia** > **klucze** i wybierz **Przekaż klucz publiczny**.

3. Wybierz plik certyfikatu z poprzedniego kroku, a następnie wybierz **Zapisz**. 

4. Zanotuj identyfikator aplikacji i odcisk palca certyfikatu, które właśnie zostało zarejestrowane z aplikacją. Aby znaleźć odcisku palca, ze strony aplikacji w portalu, przejdź do **ustawienia** i kliknij przycisk **klucze**. Odcisk palca będzie wymieniony w obszarze **kluczy publicznych** listy.

5. Otwórz manifest aplikacji w edytorze manifestu w tekście i Zastąp *keyCredentials* właściwości z informacjami certyfikatu przy użyciu następującego schematu. 

   ```
   "keyCredentials": [
        {
            "customKeyIdentifier": "$base64Thumbprint", //base64 encoding of the certificate hash
            "keyId": "$keyid", //GUID to identify the key in the manifest
            "type": "AsymmetricX509Cert",
            "usage": "Verify",
            "value":  "$base64Value" //base64 encoding of the certificate raw data
        }
    ]
   ```

6. Umożliwia zapisanie manifestu. 
  
## <a name="get-an-access-token-for-ms-graph-api"></a>Uzyskiwanie tokenu dostępu do interfejsu API programu Graph MS

1. Użyj **Get MSCloudIdMSGraphAccessTokenFromCert** polecenia cmdlet z modułu MSCloudIdUtils PowerShell, przekazując Identyfikatora aplikacji i odcisk palca uzyskanego w poprzednim kroku. 

 ![Azure Portal](./media/tutorial-access-api-with-certificates/getaccesstoken.png)

## <a name="use-the-access-token-to-call-the-graph-api"></a>Wykorzystanie tokenu dostępu do wywołania interfejsu API programu Graph

1. Teraz służy tokenu dostępu za pomocą skryptu programu Powershell do wykonywania zapytań interfejsu API programu Graph. Użyj **Invoke MSCloudIdMSGraphQuery** polecenia cmdlet z MSCloudIDUtils wyliczyć plik logowań do i directoryAudits punkt końcowy. To polecenie cmdlet obsługuje wyniki wielostronicowe, a następnie wysyła te wyniki do potoku programu PowerShell.

2. Wyślij zapytanie do endpoint directoryAudits, aby pobrać dzienniki inspekcji. 
 ![Azure Portal](./media/tutorial-access-api-with-certificates/query-directoryAudits.png)

3. Wyślij zapytanie do endpoint plik logowań do można pobrać dzienników logowania.
 ![Azure Portal](./media/tutorial-access-api-with-certificates/query-signins.png)

4. Teraz możesz wyeksportować te dane do woluminu CSV i zapisywanie w systemie SIEM. Skrypt można również opakować w zaplanowane zadanie, aby okresowo uzyskiwać dane usługi Azure AD od dzierżawcy bez konieczności przechowywania kluczy aplikacji w kodzie źródłowym. 

## <a name="next-steps"></a>Kolejne kroki

* [Pierwsze wrażenie dotyczące interfejsów API raportowania](concept-reporting-api.md)
* [Dokumentacja interfejsu API inspekcji](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
* [Raport aktywności logowania, dokumentacja interfejsu API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)



