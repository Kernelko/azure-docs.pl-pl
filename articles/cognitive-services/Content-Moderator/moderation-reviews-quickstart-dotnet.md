---
title: 'Szybki start: tworzenie przeglądów przy użyciu platformy .NET — Content Moderator'
titlesuffix: Azure Cognitive Services
description: Jak tworzyć przeglądy za pomocą zestawu Azure Content Moderator SDK for .NET.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: ce90c5f691a0a8a333161f3135856d720d1de310
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226589"
---
# <a name="quickstart-create-reviews-using-net"></a>Szybki start: tworzenie przeglądów przy użyciu platformy .NET

Ten artykuł zawiera informacje i przykłady kodu, które pomogą Ci rozpocząć korzystanie z zestawu [Content Moderator SDK for .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) do następujących zastosowań:
 
- Tworzenie zestawu przeglądów dla moderatorów-ludzi
- Uzyskiwanie stanu istniejących przeglądów dla moderatorów-ludzi

Zawartość przechodzi przez automatyczną moderację przed zaplanowaniem przeglądu przez człowieka. W tym artykule opisano jak utworzyć przegląd do moderowania przez człowieka. Pełniejszy scenariusz można znaleźć w samouczkach [Moderowanie zawartości serwisu Facebook](facebook-post-moderation.md) i [Moderowanie katalogu handlu elektronicznego](ecommerce-retail-catalog-moderation.md).

W tym artykule założono, że znasz już program Visual Studio i język C#.

## <a name="sign-up-for-content-moderator"></a>Rejestracja w usłudze Content Moderator

Zanim użyjesz usług Content Moderator za pomocą interfejsu API REST lub zestawu SDK, potrzebujesz klucza subskrypcji.
Zapoznaj się [przewodnikiem Szybki start](quick-start.md), aby dowiedzieć się, jak można uzyskać klucz.

## <a name="sign-up-for-a-review-tool-account-if-not-completed-in-the-previous-step"></a>Zarejestruj konto narzędzia do przeprowadzania przeglądów, jeśli nie zostało to zrobione w poprzednim kroku

Jeśli używasz usługi Content Moderator w witrynie Azure Portal, także [zarejestruj konto narzędzia do przeprowadzania przeglądów](https://contentmoderator.cognitive.microsoft.com/) i utwórz zespół osób przeglądających. Do wywołania interfejsu API w celu uruchomienia zadania i wyświetlenia przeglądów w narzędziu do przeprowadzaniu przeglądów jest potrzebny identyfikator zespołu i narzędzie do przeprowadzania przeglądów.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>Upewnij się, że Twój klucz interfejsu API umożliwia wywołanie interfejsu API przeglądu w celu utworzenia przeglądu.

Po wykonaniu poprzednich kroków możesz mieć dwa klucze usługi Content Moderator, jeśli wykonywanie kroków rozpoczęto w witrynie Azure Portal. 

Jeśli planujesz użyć klucza interfejsu API platformy Azure w przykładzie zestawu SDK, wykonaj kroki opisane w sekcji [Używanie klucza platformy Azure przy użyciu interfejsu API przeglądu](review-tool-user-guide/credentials.md#use-the-azure-account-with-the-review-tool-and-review-api), aby umożliwić aplikacji wywołanie interfejsu API przeglądu i tworzenie przeglądów.

Jeśli używasz bezpłatnej wersji próbnej klucza wygenerowanej przez narzędzie do przeprowadzania przeglądów, narzędzie to będzie już znało klucz i dlatego dodatkowe kroki nie są wymagane.

## <a name="create-your-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

1. Dodaj nowy projekt **Aplikacja konsoli (.NET Framework)** do rozwiązania.

   W przykładowym kodzie nadaj projektowi nazwę **CreateReviews**.

1. Wybierz ten projekt jako pojedynczy projekt startowy rozwiązania.

### <a name="install-required-packages"></a>Instalowanie wymaganych pakietów

Zainstaluj następujące pakiety NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Aktualizowanie programu za pomocą instrukcji

Zmodyfikuj program za pomocą instrukcji.

    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="create-the-content-moderator-client"></a>Tworzenie klienta usługi Content Moderator

Dodaj następujący kod, aby utworzyć klienta usługi Content Moderator dla Twojej subskrypcji.

> [!IMPORTANT]
> Zaktualizuj pola **AzureRegion** i **CMSubscriptionKey** wartościami identyfikatora regionu i klucza subskrypcji.


    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "YOUR API REGION";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"https://{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "YOUR API KEY";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.Endpoint = AzureBaseURL;
            return client;
        }
    }

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>Tworzenie klasy w celu skojarzenia wewnętrznych informacji o zawartości z identyfikatorem przeglądu

Dodaj następującą klasę do klasy **Program**.
Użyj tej klasy do skojarzenia identyfikatora przeglądu z wewnętrznym Identyfikatorem zawartości elementu.

    /// <summary>
    /// Associates the review ID (assigned by the service) to the internal
    /// content ID of the item.
    /// </summary>
    public class ReviewItem
    {
        /// <summary>
        /// The media type for the item to review.
        /// </summary>
        public string Type;

        /// <summary>
        /// The URL of the item to review.
        /// </summary>
        public string Url;

        /// <summary>
        /// The internal content ID for the item to review.
        /// </summary>
        public string ContentId;

        /// <summary>
        /// The ID that the service assigned to the review.
        /// </summary>
        public string ReviewId;
    }

### <a name="initialize-application-specific-settings"></a>Inicjowanie ustawień specyficznych dla aplikacji

> [!NOTE]
> Klucz usługi Content Moderator ma limit liczby żądań na sekundę (RPS), a w razie przekroczenia tego limitu zestaw SDK zgłasza wyjątek z kodem błędu 429. 
>
> Limit klucza warstwy bezpłatnej wynosi 1 RPS.

#### <a name="add-the-following-constants-to-the-program-class-in-programcs"></a>Dodaj następujące stałe do klasy **Program** w pliku Program.cs.
    
    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Image List API.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const double latencyDelay = 45;

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Dodaj następujące stałe i pola statyczne do klasy **Program** w pliku Program.cs.

Zaktualizuj te wartości, tak aby zawierały informacje specyficzne dla subskrypcji i zespołu.

> [!NOTE]
> Stałą TeamName należy ustawić na nazwę użytą podczas tworzenia subskrypcji [narzędzia do przeprowadzania przeglądów usługi Content Moderator](https://contentmoderator.cognitive.microsoft.com/). Wartość TeamName możesz pobrać z sekcji **Poświadczenia** menu **Ustawienia** (koło zębate).
>
> Nazwa zespołu to wartość pola **Id** w sekcji **Interfejs API**.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Content Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "YOUR REVIEW TEAM ID";

    /// <summary>
    /// The optional name of the subteam to assign the review to.
    /// </summary>
    private const string Subteam = null;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Reviews show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "YOUR API ENDPOINT";

    /// <summary>
    /// The media type for the item to review.
    /// </summary>
    /// <remarks>Valid values are "image", "text", and "video".</remarks>
    private const string MediaType = "image";

    /// <summary>
    /// The URLs of the images to create review jobs for.
    /// </summary>
    private static readonly string[] ImageUrls = new string[] {
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg"
        // add more if you want
    };

    /// <summary>
    /// The metadata key to initially add to each review item.
    /// </summary>
    private const string MetadataKey = "sc";

    /// <summary>
    /// The metadata value to initially add to each review item.
    /// </summary>
    private const string MetadataValue = "true";

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>Dodaj następujące pola statyczne do klasy **Program** w pliku Program.cs.

Użyj tych pól do śledzenia stanu aplikacji.

    /// <summary>
    /// A static reference to the text writer to use for logging.
    /// </summary>
    private static TextWriter writer;

    /// <summary>
    /// The cached review information, associating a local content ID
    /// to the created review ID for each item.
    /// </summary>
    private static List<ReviewItem> reviewItems =
        new List<ReviewItem>();

## <a name="create-a-method-to-write-messages-to-the-log-file"></a>Utwórz metodę na potrzeby zapisywania komunikatów do pliku dziennika

Dodaj następującą metodę do klasy **Program**. 

    /// <summary>
    /// Writes a message to the log file, and optionally to the console.
    /// </summary>
    /// <param name="message">The message.</param>
    /// <param name="echo">if set to <c>true</c>, write the message to the console.</param>
    private static void WriteLine(string message = null, bool echo = false)
    {
        writer.WriteLine(message ?? String.Empty);

        if (echo)
        {
            Console.WriteLine(message ?? String.Empty);
        }
    }

## <a name="create-a-method-to-create-a-set-of-reviews"></a>Tworzenie metody do tworzenia zestawu przeglądów

Zazwyczaj istnieje jakaś logika biznesowa służąca do identyfikowania przychodzących obrazów, tekstu lub wideo, które wymagają przeglądu. Jednak tutaj użyj po prostu stałej listy obrazów.

Dodaj następującą metodę do klasy **Program**.

    /// <summary>
    /// Create the reviews using the fixed list of images.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void CreateReviews(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Creating reviews for the following images:", true);

        // Create the structure to hold the request body information.
        List<CreateReviewBodyItem> requestInfo =
            new List<CreateReviewBodyItem>();

        // Create some standard metadata to add to each item.
        List<CreateReviewBodyItemMetadataItem> metadata =
            new List<CreateReviewBodyItemMetadataItem>(
            new CreateReviewBodyItemMetadataItem[] {
                new CreateReviewBodyItemMetadataItem(
                    MetadataKey, MetadataValue)
            });

        // Populate the request body information and the initial cached review information.
        for (int i = 0; i < ImageUrls.Length; i++)
        {
            // Cache the local information with which to create the review.
            var itemInfo = new ReviewItem()
            {
                Type = MediaType,
                ContentId = i.ToString(),
                Url = ImageUrls[i],
                ReviewId = null
            };

            WriteLine($" - {itemInfo.Url}; with id = {itemInfo.ContentId}.", true);

            // Add the item informaton to the request information.
            requestInfo.Add(new CreateReviewBodyItem(
                itemInfo.Type, itemInfo.Url, itemInfo.ContentId,
                CallbackEndpoint, metadata));

            // Cache the review creation information.
            reviewItems.Add(itemInfo);
        }

        var reviewResponse = client.Reviews.CreateReviewsWithHttpMessagesAsync(
            "application/json", TeamName, requestInfo);

        // Update the local cache to associate the created review IDs with
        // the associated content.
        var reviewIds = reviewResponse.Result.Body;
        for (int i = 0; i < reviewIds.Count; i++)
        {
            Program.reviewItems[i].ReviewId = reviewIds[i];
        }

        WriteLine(JsonConvert.SerializeObject(
        reviewIds, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Tworzenie metody pobierania stanu istniejących przeglądów

Dodaj następującą metodę do klasy **Program**. 

> [!Note]
> W praktyce adres URL wywołania zwrotnego `CallbackEndpoint` zostałby ustawiony na adres URL, za pomocą którego są odbierane wyniki przeglądu ręcznego (dostarczane przy użyciu żądania POST HTTP).
> Możesz zmodyfikować tę metodę, aby sprawdzać stan oczekujących przeglądów.

    /// <summary>
    /// Gets the review details from the server.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void GetReviewDetails(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Getting review details:", true);
        foreach (var item in reviewItems)
        {
            var reviewDetail = client.Reviews.GetReviewWithHttpMessagesAsync(
                TeamName, item.ReviewId);

            WriteLine(
                $"Review {item.ReviewId} for item ID {item.ContentId} is " +
                $"{reviewDetail.Result.Body.Status}.", true);
            WriteLine(JsonConvert.SerializeObject(
                reviewDetail.Result.Body, Formatting.Indented));

            Thread.Sleep(throttleRate);
        }
    }

## <a name="add-code-to-create-a-set-of-reviews-and-check-its-status"></a>Dodawanie kodu tworzenia zestawu przeglądów i sprawdzania jego stanu

Dodaj następujący kod do metody **Main**.

Ten kod symuluje wiele operacji wykonywanych w ramach definiowania listy i zarządzania nią, a także przeglądanie obrazów za pomocą listy. Funkcje rejestrowania umożliwiają wyświetlenie obiektów odpowiedzi wygenerowanych przez wywołania usługi Content Moderator za pomocą zestawu SDK.

    using (TextWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        writer = outputWriter;
        using (var client = Clients.NewClient())
        {
            CreateReviews(client);
            GetReviewDetails(client);

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propigate.");
            Thread.Sleep(latencyDelay * 1000);

            GetReviewDetails(client);
        }

        writer = null;
        outputWriter.Flush();
        outputWriter.Close();
    }

    Console.WriteLine();
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();

## <a name="run-the-program-and-review-the-output"></a>Uruchamianie program i przeglądanie danych wyjściowych

Zobaczysz dane wyjściowe podobne do następujących:

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

Zaloguj się do narzędzia do przeprowadzania przeglądów usługi Content Moderator, aby zobaczyć oczekujący przegląd obrazu z etykietą **sc** ustawioną na wartość **true**. Są także widoczne domyślne tagi **a** i **r** oraz wszelkie tagi niestandardowe zdefiniowane w narzędziu do przeprowadzania przeglądów. 

Użyj przycisku **Dalej**, aby przesłać dane.

![Przeglądanie obrazu przez moderatorów-ludzi](images/moderation-reviews-quickstart-dotnet.PNG)

Następnie naciśnij dowolny klawisz, aby kontynuować.

    Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...

## <a name="check-out-the-following-output-in-the-log-file"></a>Sprawdź następujące dane wyjściowe w pliku dziennika.

> [!NOTE]
> W pliku danych wyjściowych ciągi „\{teamname}” i „\{callbackUrl}” przedstawiają wartości odpowiednio pól `TeamName` i `CallbackEndpoint`.

Identyfikatory przeglądów i adresy URL zawartości obrazu są różne przy każdym uruchomieniu aplikacji, a po zakończeniu przeglądu pole `reviewerResultTags` zawiera tagi określone przez osobę przeglądającą dla elementu.

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.
    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Pending",
        "reviewerResultTags": [],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="your-callback-url-if-provided-receives-this-response"></a>Ta odpowiedź jest wysyłana na adres URL wywołania zwrotnego, jeśli został określony

Zobaczysz odpowiedź podobną do następującej:

    {
        "ReviewId": "201801i48a2937e679a41c7966e838c92f5e649",
        "ModifiedOn": "2018-01-06T05:04:40.5525865Z",
        "ModifiedBy": "yourusername",
        "CallBackType": "Review",
        "ContentId": "0",
        "ContentType": "Image",
        "Metadata": {
            "sc": "true"
            },
        "ReviewerResultTags": {
            "a": "False",
            "r": "False",
        }
    }


## <a name="next-steps"></a>Następne kroki

Pobierz zestaw [Content Moderator .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) i [rozwiązanie programu Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) dla tego i innych przewodników Szybki start usługi Content Moderator dla platformy .NET i rozpocznij pracę nad integracją.
