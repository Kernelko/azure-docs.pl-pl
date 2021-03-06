---
title: Jak utworzyć procesor multimediów za pomocą usługi Azure Media Services SDK dla platformy .NET | Dokumentacja firmy Microsoft
description: Dowiedz się, jak utworzyć składnik procesor multimediów do kodowania, przekonwertowania formatu, szyfrowanie i odszyfrowywanie zawartości multimediów dla usług Azure Media Services. Przykłady kodu są napisane C# i użyj Media Services SDK dla platformy .NET.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: juliako
ms.openlocfilehash: 64e353bbb83c7696960fc1d2e478478afbc94241
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50249241"
---
# <a name="how-to-get-a-media-processor-instance"></a>Porady: pobieranie wystąpienia procesora multimediów
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Przegląd
W usłudze Media Services, który procesor multimediów jest składnikiem, który obsługuje zadanie przetwarzania specyficznego, takie jak kodowanie formatu konwersji, szyfrowania lub odszyfrowywania zawartości multimedialnej. Podczas tworzenia zadania kodowania, szyfrowania lub przekonwertowania formatu zawartości multimedialnej, zwykle tworzysz procesor multimediów.

## <a name="azure-media-processors"></a>Procesory multimediów Azure 

Poniższy temat zawiera listę procesory multimediów:

* [Kodowanie procesorów multimediów](scenarios-and-availability.md#encoding-media-processors)
* [Procesory multimediów usługi analizy](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a>Pobierz procesor multimediów

Poniższa metoda pokazuje, jak można pobrać wystąpienia procesora media. W przykładzie kodu założono użycie zmiennej poziom modułu o nazwie **_kontekst** można odwoływać się do kontekstu serwera zgodnie z opisem w sekcji [jak: nawiązać połączenie programowe z usługi Media](media-services-use-aad-auth-to-access-ams-api.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a>Ścieżki szkoleniowe dotyczące usługi Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Przekazywanie opinii
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Następne kroki
Teraz gdy wiesz, jak uzyskać wystąpienie procesora media, przejdź do [jak kodowanie elementu zawartości](media-services-dotnet-encode-with-media-encoder-standard.md) tematu, w której opisano, jak za pomocą usługi Media Encoder Standard kodowanie elementu zawartości.

