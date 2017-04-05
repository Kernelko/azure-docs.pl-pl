---
title: "Używanie tematów usługi Azure Service Bus z platformą .NET | Microsoft Docs"
description: "Dowiedz się, jak używać tematów i subskrypcji usługi Service Bus z platformą .NET na platformie Azure. Przykłady kodu są napisane dla aplikacji .NET."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 31d0bc29-6524-4b1b-9c7f-aa15d5a9d3b4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 03/23/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: bec18e91ef8798a791d4b1fe93bd529593197e01
ms.lasthandoff: 03/24/2017


---
# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Jak używać tematów i subskrypcji usługi Service Bus
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

W tym artykule opisano sposób używania tematów i subskrypcji usługi Service Bus. Przykłady są napisane w języku C# i używają interfejsów API platformy .NET. Omówione scenariusze obejmują tworzenie tematów i subskrypcji, tworzenie filtrów subskrypcji, wysyłanie komunikatów do tematów, otrzymywanie komunikatów z subskrypcji, a także usuwanie tematów i subskrypcji. Aby uzyskać więcej informacji o tematach i subskrypcjach, zobacz sekcję [Następne kroki](#next-steps).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Konfigurowanie aplikacji w taki sposób, aby używała usługi Service Bus
Gdy tworzysz aplikację, która używa usługi Service Bus, musisz dodać odwołanie do zestawu usługi Service Bus i dołączyć odpowiednie przestrzenie nazw. Najłatwiejszym sposobem na to jest pobranie odpowiedniego pakietu [NuGet](https://www.nuget.org).

## <a name="get-the-service-bus-nuget-package"></a>Uzyskiwanie pakietu NuGet usługi Service Bus
Dodanie [pakietu NuGet usługi Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus) jest najprostszym sposobem skonfigurowania aplikacji ze wszystkimi niezbędnymi zależnościami usługi Service Bus. Aby zainstalować pakiet NuGet usługi Service Bus w projekcie, wykonaj następujące kroki:

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy pozycję **Odwołania**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.
2. Kliknij pozycję **Przeglądaj**, wyszukaj nazwę „Azure Service Bus”, a następnie wybierz pozycję **Microsoft Azure Service Bus**. Kliknij pozycję **Zainstaluj**, aby ukończyć instalację, a następnie zamknij okno dialogowe:
   
   ![][7]

Teraz można przystąpić do pisania kodu dla usługi Service Bus.

## <a name="create-a-service-bus-connection-string"></a>Tworzenie parametrów połączenia usługi Service Bus
Usługa Service Bus używa parametrów połączenia do przechowywania punktów końcowych i poświadczeń. Parametry połączenia można umieścić w pliku konfiguracji zamiast kodować je na stałe:

* W przypadku korzystania z usług platformy Azure zalecane jest przechowywanie parametrów połączenia przy użyciu systemu konfiguracji usługi platformy Azure (pliki csdef i cscfg).
* W przypadku korzystania z usług Azure Websites lub Azure Virtual Machines zalecane jest przechowywanie parametrów połączenia przy użyciu systemu konfiguracji platformy .NET (na przykład plik Web.config).

W obu przypadkach można pobrać parametry połączenia za pomocą metody `CloudConfigurationManager.GetSetting`, jak pokazano w dalszej części tego artykułu.

### <a name="configure-your-connection-string"></a>Konfigurowanie parametrów połączenia
Mechanizm konfiguracji usługi umożliwia dynamiczną zmianę ustawień konfiguracji z poziomu witryny [Azure Portal][Azure portal] bez ponownego wdrażania aplikacji. Na przykład dodaj etykietę `Setting` do pliku definicji usługi (**csdef**), jak pokazano w następnym przykładzie.

```xml
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Następnie możesz określić wartości w pliku konfiguracji usługi (.cscfg).

```xml
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Użyj nazwy klucza i wartości klucza sygnatury dostępu współdzielonego pobranych z portalu zgodnie z opisem w poprzedniej sekcji.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Konfigurowanie parametrów połączenia w przypadku korzystania z usługi Azure Websites lub Azure Virtual Machines
W przypadku korzystania z witryn sieci Web lub usługi Virtual Machines zaleca się użycie systemu konfiguracji platformy .NET (na przykład Web.config). Do przechowywania parametrów połączenia służy element `<appSettings>`.

```xml
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Użyj nazwy i wartości klucza sygnatury dostępu współdzielonego pobranych z witryny [Azure Portal][Azure portal] zgodnie z opisem w poprzedniej sekcji.

## <a name="create-a-topic"></a>Tworzenie tematu
Operacje zarządzania tematami i subskrypcjami usługi Service Bus można wykonywać przy użyciu klasy [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager). Ta klasa dostarcza metody do tworzenia, wyliczania i usuwania tematów.

Poniższy przykład tworzy obiekt `NamespaceManager` przy użyciu klasy `CloudConfigurationManager` platformy Azure z użyciem parametrów połączenia składających się z adresu podstawowego przestrzeni nazw usługi Service Bus oraz odpowiednich poświadczeń sygnatury dostępu współdzielonego z uprawnieniami do zarządzania nim. Te parametry połączenia mają następującą postać:

```xml
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Skorzystaj z następującego przykładu, przyjmując ustawienia konfiguracji określone w poprzedniej sekcji.

```csharp
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Istnieją przeciążenia metody [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager) umożliwiające ustawienie właściwości tematu, na przykład w celu ustawienia wartości czasu wygaśnięcia do zastosowania dla komunikatów wysyłanych do tematu. Te ustawienia są stosowane przy użyciu klasy [TopicDescription](/dotnet/api/microsoft.servicebus.messaging.topicdescription). Poniższy przykład pokazuje, jak utworzyć temat o nazwie **TestTopic** o maksymalnym rozmiarze 5 GB i domyślnym czasie wygaśnięcia komunikatu wynoszącym 1 minutę.

```csharp
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [!NOTE]
> Można użyć metody [TopicExists](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_TopicExists_System_String_) na obiektach [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager), aby sprawdzić, czy temat o określonej nazwie już istnieje w przestrzeni nazw.
> 
> 

## <a name="create-a-subscription"></a>Tworzenie subskrypcji
Można również tworzyć subskrypcje tematu przy użyciu klasy [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager). Subskrypcje są nazywane i mogą zawierać opcjonalny filtr, który ogranicza zestaw komunikatów przesyłany do wirtualnej kolejki subskrypcji.

> [!IMPORTANT]
> Aby móc odbierać komunikaty w ramach subskrypcji, musisz utworzyć subskrypcję przed wysłaniem komunikatów do tematu. Jeśli z tematem nie są powiązane żadne subskrypcje, temat odrzuci te komunikaty.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Tworzenie subskrypcji z filtrem domyślnym (MatchAll)
Filtr **MatchAll** jest filtrem domyślnym, który jest używany, gdy podczas tworzenia nowej subskrypcji nie został określony żaden filtr. Kiedy używasz filtru **MatchAll**, wszystkie komunikaty opublikowane do tematu są umieszczane w wirtualnej kolejce subskrypcji. Poniższy przykład tworzy subskrypcję o nazwie „AllMessages” i używa domyślnego filtru **MatchAll**.

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Tworzenie subskrypcji z filtrami
Można również ustawiać filtry pozwalające określić, które komunikaty wysyłane do tematu powinny znajdować się w subskrypcji określonego tematu.

Najbardziej elastycznym typem filtru obsługiwanym przez subskrypcje jest klasa [SqlFilter][SqlFilter], która implementuje podzbiór standardu SQL92. Filtry SQL działają na właściwościach komunikatów, które są publikowane do tematu. Aby uzyskać więcej informacji na temat wyrażeń, które mogą być używane z filtrem SQL, zobacz składnię [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

Poniższy przykład tworzy subskrypcję o nazwie **HighMessages** z obiektem [SqlFilter][SqlFilter], który wybiera tylko komunikaty o właściwości niestandardowej **MessageNumber** wyższej niż 3.

```csharp
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageId > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Podobnie poniższy przykład tworzy subskrypcję o nazwie **LowMessages** z obiektem [SqlFilter][SqlFilter], który wybiera tylko komunikaty o właściwości **MessageNumber** równej 3 lub mniejszej.

```csharp
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageId <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

W takiej sytuacji komunikat wysłany do tematu `TestTopic` zawsze jest dostarczany do odbiorców mających subskrypcję **AllMessages** i selektywnie dostarczany do odbiorców mających subskrypcje **HighMessages** i **LowMessages** (w zależności od zawartości komunikatu).

## <a name="send-messages-to-a-topic"></a>Wysyłanie komunikatów do tematu
Aby wysłać komunikat do tematu usługi Service Bus, aplikacja tworzy obiekt [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient) przy użyciu parametrów połączenia.

Poniższy kod przedstawia sposób tworzenia obiektu [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient) dla tematu **TestTopic** utworzonego wcześniej przy użyciu wywołania interfejsu API [CreateFromConnectionString](/dotnet/api/microsoft.servicebus.messaging.topicclient#Microsoft_ServiceBus_Messaging_TopicClient_CreateFromConnectionString_System_String_System_String_).

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Komunikaty wysyłane do tematów usługi Service Bus są wystąpieniami klasy [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Obiekty **BrokeredMessage** mają zestaw właściwości standardowych (takich jak [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) i [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), słownik, który służy do przechowywania określonych właściwości niestandardowych aplikacji, oraz treść dowolnych danych aplikacji. Aplikacja może ustawić treść komunikatu przez przekazanie dowolnego obiektu podlegającego serializacji do konstruktora obiektu **BrokeredMessage**, a odpowiedni element **DataContractSerializer** jest następnie używany do serializacji obiektu. Alternatywnie można udostępnić obiekt **System.IO.Stream**.

W poniższym przykładzie pokazano sposób wysyłania pięciu testowych komunikatów do obiektu **TestTopic** [TopicClient](/dotnet/api/microsoft.servicebus.messaging.topicclient) uzyskanego w poprzednim przykładzie kodu. Należy pamiętać, że wartość właściwości [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) każdego komunikatu różni się w zależności od iteracji pętli (określa subskrypcje, które go otrzymają).

```csharp
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageId"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Tematy usługi Service Bus obsługują maksymalny rozmiar komunikatu 256 KB w [warstwie Standardowa](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera standardowe i niestandardowe właściwości aplikacji, może mieć maksymalny rozmiar 64 KB. Nie ma żadnego limitu liczby komunikatów w temacie, ale jest ograniczenie całkowitego rozmiaru komunikatów przechowywanych przez temat. Rozmiar tematu jest definiowany w czasie tworzenia, z górnym limitem 5 GB. Jeśli partycjonowanie jest włączone, górny limit jest większy. Aby uzyskać więcej informacji, zobacz [Partycjonowane jednostki do obsługi komunikatów](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Jak odbierać komunikaty z subskrypcji
Zalecanym sposobem odbierania komunikatów z subskrypcji jest użycie obiektu [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient). Obiekty **SubscriptionClient** mogą pracować w dwóch różnych trybach: [*ReceiveAndDelete* i *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode). Ustawienie domyślne to **PeekLock**.

W przypadku używania trybu **ReceiveAndDelete** odbieranie jest operacją pojedynczego zrzutu. Oznacza to, że kiedy usługa Service Bus odbiera żądanie odczytu komunikatu w subskrypcji, oznacza komunikat jako wykorzystywany i zwraca go do aplikacji. Tryb **ReceiveAndDelete** jest najprostszym modelem i działa najlepiej w scenariuszach, w których aplikacja może tolerować nieprzetworzenie komunikatu w razie awarii. Aby to zrozumieć, rozważmy scenariusz, w którym konsument wystawia żądanie odbioru, a następnie ulega awarii przed jego przetworzeniem. Usługa Service Bus oznaczyła komunikat jako wykorzystany, dlatego gdy aplikacja zostanie ponownie uruchomiona i ponownie rozpocznie korzystanie z komunikatów, pominie utracony komunikat, który został wykorzystany przed awarią.

W trybie **PeekLock** (trybie domyślnym) odbieranie staje się operacją dwuetapową, co umożliwia obsługę aplikacji, które nie tolerują brakujących komunikatów. Gdy usługa Service Bus odbiera żądanie, znajduje następny komunikat do wykorzystania, blokuje go w celu uniemożliwienia innym klientom odebrania go i zwraca go do aplikacji. Kiedy aplikacja zakończy przetwarzanie komunikatu (lub niezawodnie zapisze go w celu przyszłego przetwarzania), wykonuje drugi etap procesu odbierania przez wywołanie metody [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) dla odebranego komunikatu. Gdy wywołanie metody **Complete** stanie się widoczne dla usługi Service Bus, usługa oznaczy komunikat jako wykorzystany i usunie go z subskrypcji.

W poniższym przykładzie pokazano, jak komunikaty mogą być odbierane i przetwarzane przy użyciu domyślnego trybu **PeekLock**. Aby określić inną wartość właściwości [ReceiveMode](/dotnet/api/microsoft.servicebus.messaging.receivemode), można użyć innego przeciążenia metody [CreateFromConnectionString](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_CreateFromConnectionString_System_String_System_String_System_String_Microsoft_ServiceBus_Messaging_ReceiveMode_). W tym przykładzie użyto wywołania zwrotnego [OnMessage](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) do przetwarzania komunikatów przychodzących do subskrypcji **HighMessages**.

```csharp
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

Ten przykład konfiguruje wywołanie zwrotne metody [OnMessage](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__Microsoft_ServiceBus_Messaging_OnMessageOptions_) przy użyciu obiektu [OnMessageOptions](/dotnet/api/microsoft.servicebus.messaging.onmessageoptions). Właściwość [AutoComplete](/dotnet/api/microsoft.servicebus.messaging.onmessageoptions#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoComplete) jest ustawiona na **false** w celu umożliwienia ręcznej kontroli nad tym, kiedy wywołać metodę [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) na odebranym komunikacie. Właściwość [AutoRenewTimeout](/dotnet/api/microsoft.servicebus.messaging.onmessageoptions#Microsoft_ServiceBus_Messaging_OnMessageOptions_AutoRenewTimeout) jest ustawiona na 1 minutę, co powoduje, że klient oczekuje przez jedną minutę na komunikat, zanim upłynie limit czasu funkcji automatycznego odnawiania i klient wyśle nowe wywołanie w celu sprawdzenia komunikatów. Ta wartość właściwości zmniejsza liczbę przypadków, gdy klient wysyła wywołania, które mogą być obciążane i nie pobierają komunikatów.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Sposób obsługi awarii aplikacji i komunikatów niemożliwych do odczytania
Usługa Service Bus zapewnia funkcję ułatwiającą bezpieczne odzyskiwanie w razie błędów w aplikacji lub trudności z przetwarzaniem komunikatu. Jeśli aplikacja odbierająca z jakiegoś powodu nie można przetworzyć komunikatu, wówczas może wywołać metodę [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon_System_Collections_Generic_IDictionary_System_String_System_Object__) dla odebranego komunikatu (zamiast metody [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Powoduje to odblokowanie komunikatu w subskrypcji przez usługę Service Bus i ponowne udostępnienie go do odebrania przez tę samą lub inną odbierającą aplikację.

Istnieje również limit czasu skojarzony z komunikatem zablokowanym w subskrypcji i jeśli aplikacja nie może przetworzyć komunikatu przed upływem limitu czasu blokady (na przykład jeśli wystąpiła awaria aplikacji), usługa Service Bus automatycznie odblokowuje komunikat i udostępnia go do ponownego odbioru.

W przypadku gdy aplikacja przestaje działać po przetworzeniu komunikatu, ale przed wysłaniem żądania [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete), komunikat zostanie dostarczony do aplikacji po jej ponownym uruchomieniu. Jest to często określane jako *Przetwarzanie co najmniej raz*, co oznacza, że każdy komunikat jest przetwarzany co najmniej raz, ale w pewnych sytuacjach ten sam komunikat może być dostarczony ponownie. Jeśli scenariusz nie toleruje dwukrotnego przetwarzania, deweloperzy aplikacji powinni dodać dodatkową logikę do swojej aplikacji w celu obsługi dwukrotnego dostarczania komunikatów. Jest to często osiągane przy użyciu właściwości [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) komunikatu, która pozostaje stała między kolejnymi próbami dostarczenia.

## <a name="delete-topics-and-subscriptions"></a>Usuwanie tematów i subskrypcji
W poniższym przykładzie pokazano sposób usuwania tematu **TestTopic** z przestrzeni nazw usługi **HowToSample**.

```csharp
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Usunięcie tematu powoduje również usunięcie subskrypcji, które są zarejestrowane z tematem. Subskrypcje mogą być również usuwane niezależnie. Poniższy kod ilustruje sposób usuwania subskrypcji o nazwie **HighMessages** z tematu **TestTopic**.

```csharp
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz już podstawy tematów i subskrypcji usługi Service Bus, skorzystaj z poniższych linków, aby dowiedzieć się więcej.

* [Kolejki, tematy i subskrypcje][Queues, topics, and subscriptions].
* [Przykładowe filtry tematu][Topic filters sample]
* Dokumentacja interfejsów API dla elementu [SqlFilter][SqlFilter].
* Utwórz działającą aplikację, która wysyła komunikaty do kolejki usługi Service Bus i odbiera je z niej: [Samouczek dotyczący komunikatów obsługiwanych przez brokera w usłudze Service Bus dla platformy .NET][Service Bus brokered messaging .NET tutorial].
* Przykłady usługi Service Bus: pobierz ze strony [przykładów dla platformy Azure][Azure samples] lub zobacz [przegląd przykładów](service-bus-samples.md).

[Azure portal]: https://portal.azure.com

[7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Topic filters sample]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter#Microsoft_ServiceBus_Messaging_SqlFilter_SqlExpression
[Service Bus brokered messaging .NET tutorial]: service-bus-brokered-tutorial-dotnet.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2

