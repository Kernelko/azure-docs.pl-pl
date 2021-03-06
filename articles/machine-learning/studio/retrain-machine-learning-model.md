---
title: Ponowne szkolenie modelu uczenia maszynowego | Dokumentacja firmy Microsoft
description: Dowiedz się, jak ponowne szkolenie modelu i zaktualizować usługę sieci Web, aby używać nowo uczonego modelu w usłudze Azure Machine Learning.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.openlocfilehash: 46aa2c209f782706357f9a928ddbaa6321abdd77
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115531"
---
# <a name="retrain-a-machine-learning-model"></a>Ponowne szkolenie modelu uczenia maszynowego
W ramach procesu operacjonalizacji modeli uczenia maszynowego w usłudze Azure Machine Learning model jest uczony i zapisany. Możesz następnie użyć go do utworzenia predykcyjne usługi sieci Web. Usługi sieci Web mogą być następnie używane w witrynach sieci web, pulpity nawigacyjne i aplikacje mobilne. 

Modele utworzone za pomocą usługi Machine Learning zwykle nie są statyczne. Nowe dane staje się dostępny, lub gdy konsumenta interfejsu API ma swoje własne dane modelu musi być retrained. 

Ponowne szkolenie może często występować. Dzięki funkcji programowe ponowne Trenowanie interfejsu API możesz programowo Ponowne szkolenie modelu przy użyciu interfejsów API do ponownego trenowania i zaktualizować usługę sieci Web przy użyciu nowo trenowanego modelu. 

W tym dokumencie opisano proces ponownego trenowania i dowiesz się, jak używać interfejsów API do ponownego trenowania.

## <a name="why-retrain-defining-the-problem"></a>Dlaczego Ponowne szkolenie: Definiowanie problemu
W ramach uczenia procesu uczenia maszynowego model jest uczony przy użyciu zestawu danych. Modele utworzone za pomocą usługi Machine Learning zwykle nie są statyczne. Nowe dane staje się dostępny, lub gdy konsumenta interfejsu API ma swoje własne dane modelu musi być retrained.

W tych scenariuszach programowego interfejsu API zapewnia wygodny sposób lub konsumenta interfejsów API można utworzyć klienta, które mogą na podstawie jednorazowe lub regularne Ponowne szkolenie modelu przy użyciu własnych danych. Są następnie oceniać wyniki ponownego trenowania i zaktualizuj interfejs API usługi sieci Web do korzystania z nowo uczonego modelu.

> [!NOTE]
> Jeśli masz istniejące eksperymentu szkolenia i usługi sieci Web nowej można wyewidencjonować ponownego próbkowania istniejącej usługi internetowej predykcyjne zamiast wykonać czynności opisane w przewodniku wymienione w poniższej sekcji.
> 
> 

## <a name="end-to-end-workflow"></a>Kompletny przepływ pracy
Proces obejmuje następujące składniki: eksperymentu szkolenia i eksperyment predykcyjny publikowane jako usługi sieci Web. Aby włączyć ponownym szkoleniem trenowanego modelu, eksperymentu szkolenia muszą być publikowane jako usługi sieci Web z danymi wyjściowymi uczonego modelu. Dzięki temu dostęp do interfejsu API do modelu do ponownego trenowania. 

Poniższe kroki dotyczą zarówno nowe oraz klasyczne usługi sieci Web:

Tworzenie początkowej predykcyjną usługę sieci Web:

* Tworzenie eksperymentu szkolenia
* Tworzenie eksperymentu predykcyjnego sieci web
* Wdrażanie predykcyjna usługa internetowa

Ponowne szkolenie usługi sieci Web:

* Aktualizuj eksperymentu szkolenia, aby umożliwić ponowne trenowanie
* Wdrażanie ponownego trenowania usługi sieci web
* Ponowne szkolenie modelu przy użyciu kodu usługę wykonywania wsadowego

Aby uzyskać wskazówki poprzednich kroków, zobacz [Retrain Machine Learning models programowo](retrain-models-programmatically.md).

> [!NOTE] 
> Aby wdrożyć nową usługę sieci web musi masz wystarczające uprawnienia w ramach subskrypcji, do której możesz wdrażanie usługi sieci web. Aby uzyskać więcej informacji, zobacz [Zarządzanie usługą sieci Web przy użyciu portalu usług sieci Web Azure Machine Learning](manage-new-webservice.md). 

Jeśli wdrożono klasycznej usługi sieci Web:

* Utwórz nowy punkt końcowy na predykcyjną usługę sieci Web
* Stosowanie poprawek do adresu URL i kodu
* Użyj adresu URL poprawki, aby wskazywały nowy punkt końcowy retrained modelu 

Aby uzyskać wskazówki poprzednich kroków, zobacz [Ponowne szkolenie klasyczna usługa sieci Web](retrain-a-classic-web-service.md).

Jeśli napotkasz problemy, ponownego trenowania klasyczna usługa sieci Web, zobacz [Rozwiązywanie problemów z ponownym szkoleniem usługi Azure Machine Learning klasyczna usługa sieci Web](troubleshooting-retraining-models.md).

Jeśli wdrożono usługę sieci Web nowy:

* Zaloguj się do swojego konta usługi Azure Resource Manager
* Pobierz definicję usługi sieci Web
* Eksportowanie definicji usługi sieci Web jako dane JSON
* Aktualizuj odwołanie do `ilearner` obiektów blob w formacie JSON
* Importuj dane JSON w definicji usługi sieci Web
* Aktualizacja usługi sieci Web za pomocą nowych definicji usługi sieci Web

Aby uzyskać wskazówki poprzednich kroków, zobacz [Ponowne szkolenie przy użyciu poleceń cmdlet programu PowerShell usługi Machine Learning zarządzania usługi nowej sieci Web](retrain-new-web-service-using-powershell.md).

Proces konfigurowania szkoleniem klasyczna usługa sieci Web obejmuje następujące czynności:

![Ponowne szkolenie Przegląd procesu][1]

Proces konfigurowania szkoleniem usługi sieci Web nowej obejmuje następujące czynności:

![Ponowne szkolenie Przegląd procesu][7]

## <a name="other-resources"></a>Inne zasoby
* [Ponowne szkolenie i aktualizowania usługi Azure Machine Learning modeli przy użyciu usługi Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Tworzenie wielu modeli usługi Machine Learning i sieci web punktów końcowych usługi podstawie jednego eksperymentu przy użyciu programu PowerShell](create-models-and-endpoints-with-powershell.md)
* [AML ponowne Trenowanie modeli przy użyciu interfejsów API](https://www.youtube.com/watch?v=wwjglA8xllg) wideo pokazano, jak ponowne trenowanie modeli uczenia maszynowego utworzonych w usłudze Azure Machine Learning za pomocą ponownego trenowania interfejsów API i programu PowerShell.

<!--image links-->
[1]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

