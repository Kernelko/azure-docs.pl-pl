---
title: 'Szybki start: tłumaczenie tekstu, Ruby — interfejs API tłumaczenia tekstu w usłudze Translator'
titleSuffix: Azure Cognitive Services
description: W tym przewodniku Szybki start przetłumaczysz tekst z jednego języka na inny przy użyciu interfejsu API tłumaczenia tekstu w usłudze Translator i języka Ruby.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/22/2018
ms.author: erhopf
ms.openlocfilehash: a36f873c08fee57ddb73f94b4a77dc4514422cb1
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645613"
---
# <a name="quickstart-translate-text-with-the-translator-text-rest-api-ruby"></a>Szybki start: tłumaczenie tekstu przy użyciu interfejsu API REST tłumaczenia tekstu w usłudze Translator (Ruby)

W tym przewodniku Szybki start przetłumaczysz tekst z jednego języka na inny przy użyciu interfejsu API tłumaczenia tekstu w usłudze Translator.

## <a name="prerequisites"></a>Wymagania wstępne

Aby uruchomić ten kod, potrzebne jest środowisko języka [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) lub jego nowsza wersja.

Aby korzystać z interfejsu API tłumaczenia tekstu w usłudze Translator, potrzebny jest również klucz subskrypcji. Zobacz [How to sign up for the Translator Text API (Jak zarejestrować się w interfejsie API tłumaczenia tekstu w usłudze Translator)](translator-text-how-to-signup.md).

## <a name="translate-request"></a>Żądanie Translate

Poniższy kod tłumaczy tekst źródłowy z jednego języka na inny przy użyciu metody [Translate](./reference/v3-0-translate.md).

1. Utwórz nowy projekt języka Ruby w ulubionym edytorze kodu.
2. Dodaj kod przedstawiony poniżej.
3. Zastąp wartość `key` kluczem dostępu właściwym dla Twojej subskrypcji.
4. Uruchom program.

```ruby
require 'net/https'
require 'uri'
require 'cgi'
require 'json'
require 'securerandom'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the key string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'https://api.cognitive.microsofttranslator.com'
path = '/translate?api-version=3.0'

# Translate to German and Italian.
params = '&to=de&to=it'

uri = URI (host + path + params)

text = 'Hello, world!'

content = '[{"Text" : "' + text + '"}]'

request = Net::HTTP::Post.new(uri)
request['Content-type'] = 'application/json'
request['Content-length'] = content.length
request['Ocp-Apim-Subscription-Key'] = key
request['X-ClientTraceId'] = SecureRandom.uuid
request.body = content

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request (request)
end

result = response.body.force_encoding("utf-8")

json = JSON.pretty_generate(JSON.parse(result))
puts json
```

## <a name="translate-response"></a>Odpowiedź na żądanie Translate

Po pomyślnym przetworzeniu żądania jest zwracana odpowiedź w formacie JSON, jak pokazano na poniższym przykładzie:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z kodem przykładowym z tego przewodnika Szybki start i innych, dotyczącym między innymi transliteracji i rozpoznawania języka, a także z innymi projektami przykładowymi dotyczącymi tłumaczenia tekstów w usłudze Translator w repozytorium GitHub.

> [!div class="nextstepaction"]
> [Poznaj przykłady dla języka Ruby w usłudze GitHub](https://aka.ms/TranslatorGitHub?type=&language=ruby)
