---
title: Rejestrowanie nowego urządzenia usługi Azure IoT Edge (CLI) | Dokumentacja firmy Microsoft
description: Użyj rozszerzenia IoT dla interfejsu wiersza polecenia platformy Azure, aby zarejestrować nowe urządzenie usługi IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/27/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ee5e68d45c7d966619238312dabedc1628a4bf61
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998036"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli"></a>Rejestrowanie nowego urządzenia usługi Azure IoT Edge przy użyciu wiersza polecenia platformy Azure

Zanim użyjesz urządzeń IoT za pomocą usługi Azure IoT Edge, musisz zarejestrować ich przy użyciu usługi IoT hub. Po zarejestrowaniu urządzenia, pojawi się parametry połączenia, który może służyć do konfigurowania urządzenia pod kątem obciążeń krawędzi. 

[Interfejs wiersza polecenia Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) jest typu open-source wielu platformach narzędzia wiersza polecenia do zarządzania zasobami platformy Azure, takich jak usługi IoT Edge. Umożliwia zarządzanie zasobami usługi Azure IoT Hub, wystąpieniami usługi device provisioning i połączonymi centrami po gotowych. Nowe rozszerzenie IoT uzupełnia interfejs wiersza polecenia platformy Azure przy użyciu funkcji, takich jak zarządzanie urządzeniami i pełne możliwości usługi IoT Edge.

W tym artykule pokazano, jak zarejestrować nowe urządzenie usługi IoT Edge przy użyciu wiersza polecenia platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne

* [Usługi IoT hub](../iot-hub/iot-hub-create-using-cli.md) w subskrypcji platformy Azure. 
* [Interfejs wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) w danym środowisku. Co najmniej z wiersza polecenia platformy Azure musi być w wersji 2.0.24 lub nowszej. Użyj polecenia `az –-version` w celu przeprowadzenia weryfikacji. Ta wersja obsługuje polecenia rozszerzenia az i wprowadza platformę poleceń Knack. 
* [Rozszerzenia IoT dla interfejsu wiersza polecenia platformy Azure](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Tworzenie urządzenia

Użyj następującego polecenia, aby utworzyć nową tożsamość urządzenia w usłudze IoT hub: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Tego polecenia obejmuje trzy parametry:
* **Identyfikator urządzenia**: wprowadź opisową nazwę, która jest unikatowa w Centrum IoT.
* **Nazwa koncentratora**: Podaj nazwę Centrum IoT Hub.
* **włączone usługi Edge**: ten parametr deklaruje, że urządzenie jest przeznaczona do użytku z usługą IoT Edge.

   ![Tworzenie urządzenia usługi IoT Edge](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Wyświetl wszystkie urządzenia

Użyj następującego polecenia, aby wyświetlić wszystkie urządzenia w usłudze IoT hub:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Każde urządzenie, które są zarejestrowane jako urządzenia usługi IoT Edge będzie mieć ustawioną właściwość **capabilities.iotEdge** równa **true**. 

## <a name="retrieve-the-connection-string"></a>Pobieranie parametrów połączenia

Gdy wszystko będzie gotowe skonfigurować urządzenie, należy parametry połączenia, która łączy urządzenie fizyczne za pomocą jej tożsamości w usłudze IoT hub. Zwraca ciąg połączenia na jednym urządzeniu, należy użyć następującego polecenia:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

Parametr id urządzeń jest uwzględniana wielkość liter. Nie należy kopiować znaków cudzysłowu wokół parametry połączenia. 

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się, jak [wdrażać moduły na urządzeniu z wiersza polecenia platformy Azure](how-to-deploy-modules-cli.md)
