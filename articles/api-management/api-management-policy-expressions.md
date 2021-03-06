---
title: Wyrażenia zasad w usłudze Azure API Management | Dokumentacja firmy Microsoft
description: Więcej informacji na temat wyrażeń zasad w usłudze Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 771ec7713c989025635e585b7bb511986e71cda9
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024773"
---
# <a name="api-management-policy-expressions"></a>Wyrażenia zasad usługi API Management
W tym artykule omówiono wyrażenia zasad składnia jest C# 7. Każde wyrażenie ma dostęp do niejawnie podana [kontekstu](api-management-policy-expressions.md#ContextVariables) zmienną i dozwolonych [podzbioru](api-management-policy-expressions.md#CLRTypes) dla typów programu .NET Framework.  

Więcej informacji:

- Zobacz, jak Podaj informacje o kontekście do usługi zaplecza. Użyj [Ustaw parametr ciągu zapytania](api-management-transformation-policies.md#SetQueryStringParameter) i [nagłówka HTTP ustaw](api-management-transformation-policies.md#SetHTTPheader) zasady muszą podać tę informację.
- Zobacz, jak używać [weryfikacji tokenu JWT](api-management-access-restriction-policies.md#ValidateJWT) zasad do wstępnej autoryzacji dostępu do operacji na podstawie tokenu oświadczeń.   
- Zobacz, jak używać [inspektora interfejsów API](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) śledzenia, aby zobaczyć, jak zasady są oceniane i wyniki tych ocen.  
- Zobacz, jak używać wyrażeń z [uzyskać z pamięci podręcznej](api-management-caching-policies.md#GetFromCache) i [Store do pamięci podręcznej](api-management-caching-policies.md#StoreToCache) zasad można skonfigurować buforowanie odpowiedzi usługi API Management. Ustaw czas trwania, który pasuje do buforowania odpowiedzi usługi wewnętrznej bazy danych określony przez usługę kopii `Cache-Control` dyrektywy.  
- Zobacz, jak przeprowadzić filtrowanie zawartości. Usuń elementy danych z odpowiedź odebraną z zaplecza za pomocą [przepływ sterowania](api-management-advanced-policies.md#choose) i [ustawić treść](api-management-transformation-policies.md#SetBody) zasad. 
- Aby pobrać deklaracji zasad, zobacz [--samples/zasady usługi api management](https://github.com/Azure/api-management-samples/tree/master/policies) repozytorium github.  
  
  
##  <a name="Syntax"></a> Składnia  
 Pojedynczej instrukcji wyrażenia są ujęte w `@(expression)`, gdzie `expression` jest poprawnie sformułowany C# instrukcji wyrażenia.  
  
 Wyrażenia instrukcji wielu są ujęte w `@{expression}`. Wszystkie ścieżki kodu w ramach wielu instrukcji wyrażenia musi się kończyć `return` instrukcji.  
  
##  <a name="PolicyExpressionsExamples"></a> Przykłady  
  
```  
@(true)  
  
@((1+1).ToString())  
  
@("Hi There".Length)  
  
@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)  
  
@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)  
  
@{   
  string value;   
  if (context.Request.Headers.TryGetValue("Authorization", out value))   
  {   
    return Encoding.UTF8.GetString(Convert.FromBase64String(value));  
  }   
  else   
  {   
    return null;  
  }  
}  
```  
  
##  <a name="PolicyExpressionsUsage"></a> Sposób użycia  
 Wyrażenia mogą być używane jako wartości atrybutów lub wartości tekstowe w dowolnej usługi API Management [zasady](api-management-policies.md) (chyba że informacje o zasadach określono inaczej).  
  
> [!IMPORTANT]
>  Korzystając z wyrażenia zasad ma tylko ograniczoną weryfikacji wyrażenia zasad po zdefiniowaniu zasad. Wyrażenia są wykonywane przez bramę w czasie wykonywania, wyjątki generowane przez zasady wynik wyrażenia w błąd w czasie wykonywania.  
  
##  <a name="CLRTypes"></a> Typów programu .NET framework, które mogą w wyrażeniach zasad  
 Poniższa tabela zawiera listę typów programu .NET Framework i ich elementów członkowskich, które są dozwolone w wyrażeniach zasad.  
  
|Typ CLR|Obsługiwane elementy członkowskie|  
|--------------|-----------------------|  
|Newtonsoft.Json.Linq.Extensions|Wszyscy|  
|Newtonsoft.Json.Linq.JArray|Wszyscy|  
|Newtonsoft.Json.Linq.JConstructor|Wszyscy|  
|Newtonsoft.Json.Linq.JContainer|Wszyscy|  
|Newtonsoft.Json.Linq.JObject|Wszyscy|  
|Newtonsoft.Json.Linq.JProperty|Wszyscy|  
|Newtonsoft.Json.Linq.JRaw|Wszyscy|  
|Newtonsoft.Json.Linq.JToken|Wszyscy|  
|Newtonsoft.Json.Linq.JTokenType|Wszyscy|  
|Newtonsoft.Json.Linq.JValue|Wszyscy|  
|System.Collections.Generic.IReadOnlyCollection<T\>|Wszyscy|  
|System.Collections.Generic.IReadOnlyDictionary<TKey,  TValue>|Wszyscy|  
|System.Collections.Generic.ISet<TKey, TValue>|Wszyscy|  
|System.Collections.Generic.KeyValuePair<TKey,  TValue>|Klucz-wartość|  
|System.Collections.Generic.List<TKey, TValue>|Wszyscy|  
|System.Collections.Generic.Queue<TKey, TValue>|Wszyscy|  
|System.Collections.Generic.Stack<TKey, TValue>|Wszyscy|  
|System.Convert|Wszyscy|  
|System.DateTime|Wszyscy|  
|System.DateTimeKind|UTC|  
|System.DateTimeOffset|Wszyscy|  
|System.Decimal|Wszyscy|  
|System.Double|Wszyscy|  
|System.Guid|Wszyscy|  
|System.IEnumerable<T\>|Wszyscy|  
|System.IEnumerator<T\>|Wszyscy|  
|System.Int16|Wszyscy|  
|System.Int32|Wszyscy|  
|System.Int64|Wszyscy|  
|System.Linq.Enumerable<T\>|Wszyscy|  
|System.Math|Wszyscy|  
|System.MidpointRounding|Wszyscy|
|System.Net.WebUtility|Wszyscy|
|System.Nullable<T\>|Wszyscy|  
|System.Random|Wszyscy|  
|System.SByte|Wszyscy|  
|System.Security.Cryptography. HMACSHA384|Wszyscy|  
|System.Security.Cryptography. HMACSHA512|Wszyscy|  
|System.Security.Cryptography.HashAlgorithm|Wszyscy|  
|System.Security.Cryptography.HMAC|Wszyscy|  
|System.Security.Cryptography.HMACMD5|Wszyscy|  
|System.Security.Cryptography.HMACSHA1|Wszyscy|  
|System.Security.Cryptography.HMACSHA256|Wszyscy|  
|System.Security.Cryptography.KeyedHashAlgorithm|Wszyscy|  
|System.Security.Cryptography.MD5|Wszyscy|  
|System.Security.Cryptography.RNGCryptoServiceProvider|Wszyscy|  
|System.Security.Cryptography.SHA1|Wszyscy|  
|System.Security.Cryptography.SHA1Managed|Wszyscy|  
|System.Security.Cryptography.SHA256|Wszyscy|  
|System.Security.Cryptography.SHA256Managed|Wszyscy|  
|System.Security.Cryptography.SHA384|Wszyscy|  
|System.Security.Cryptography.SHA384Managed|Wszyscy|  
|System.Security.Cryptography.SHA512|Wszyscy|  
|System.Security.Cryptography.SHA512Managed|Wszyscy|  
|System.Single|Wszyscy|  
|System.String|Wszyscy|  
|System.StringSplitOptions|Wszyscy|  
|System.Text.Encoding|Wszyscy|  
|System.Text.RegularExpressions.Capture|Wartość indeksu, długość,|  
|System.Text.RegularExpressions.CaptureCollection|Liczba elementów|  
|System.Text.RegularExpressions.Group|Przechwytywanie, Powodzenie|  
|System.Text.RegularExpressions.GroupCollection|Liczba elementów|  
|System.Text.RegularExpressions.Match|Wynik jest pusta, grup,|  
|System.Text.RegularExpressions.Regex|(Konstruktor) IsMatch, dopasowanie, są zgodne, Zastąp|  
|System.Text.RegularExpressions.RegexOptions|Compiled, IgnoreCase, IgnorePatternWhitespace, Multiline, None, RightToLeft, Singleline|  
|System.TimeSpan|Wszyscy|  
|System.Tuple|Wszyscy|  
|System.UInt16|Wszyscy|  
|System.UInt32|Wszyscy|  
|System.UInt64|Wszyscy|  
|System.Uri|Wszyscy|  
|System.Xml.Linq.Extensions|Wszyscy|  
|System.Xml.Linq.XAttribute|Wszyscy|  
|System.Xml.Linq.XCData|Wszyscy|  
|System.Xml.Linq.XComment|Wszyscy|  
|System.Xml.Linq.XContainer|Wszyscy|  
|System.Xml.Linq.XDeclaration|Wszyscy|  
|System.Xml.Linq.XDocument|Wszyscy|  
|System.Xml.Linq.XDocumentType|Wszyscy|  
|System.Xml.Linq.XElement|Wszyscy|  
|System.Xml.Linq.XName|Wszyscy|  
|System.Xml.Linq.XNamespace|Wszyscy|  
|System.Xml.Linq.XNode|Wszyscy|  
|System.Xml.Linq.XNodeDocumentOrderComparer|Wszyscy|  
|System.Xml.Linq.XNodeEqualityComparer|Wszyscy|  
|System.Xml.Linq.XObject|Wszyscy|  
|System.Xml.Linq.XProcessingInstruction|Wszyscy|  
|System.Xml.Linq.XText|Wszyscy|  
|System.Xml.XmlNodeType|Wszyscy|  
  
##  <a name="ContextVariables"></a> Zmiennej kontekstowej  
 Zmiennej o nazwie `context` jest niejawnie dostępne we wszystkich zasadach [wyrażenie](api-management-policy-expressions.md#Syntax). Jej członków zawierają informacje przydatne do `\request`. Wszystkie `context` elementy członkowskie są tylko do odczytu.  
  
|Zmiennej kontekstowej|Dozwolone metody, właściwości i wartości parametrów|  
|----------------------|-------------------------------------------------------|  
|Kontekst|Interfejs API: IApi<br /><br /> Wdrożenie<br /><br /> Upłynęło: TimeSpan — odstęp czasu między wartość sygnatury czasowej i bieżący czas<br /><br /> LastError<br /><br /> Operacja<br /><br /> Product (Produkt)<br /><br /> Żądanie<br /><br /> Identyfikatora RequestId: Identyfikator Guid - żądanie Unikatowy identyfikator<br /><br /> Odpowiedź<br /><br /> Subskrypcja<br /><br /> Sygnatura czasowa: Data i godzina — punktu w czasie, gdy otrzymano żądanie<br /><br /> Śledzenie: bool — wskazuje, jeśli śledzenie jest włączone czy wyłączone <br /><br /> Użytkownik<br /><br /> Zmienne: IReadOnlyDictionary < string, object ><br /><br /> void Trace(message: string)|  
|context.Api|Identyfikator: ciąg<br /><br /> IsCurrentRevision: bool<br /><br />  Nazwa: ciąg<br /><br /> Ścieżka: ciąg<br /><br /> Poprawka: ciąg<br /><br /> ServiceUrl: IUrl<br /><br /> Wersja: ciąg |  
|context.Deployment|Region: ciąg<br /><br /> ServiceName: ciąg<br /><br /> Certyfikaty: IReadOnlyDictionary < string, X509Certificate2 >|  
|kontekst. LastError|Źródło: ciąg<br /><br /> Przyczyna: ciąg<br /><br /> Komunikat: ciąg<br /><br /> Zakres: ciąg<br /><br /> Sekcja: ciąg<br /><br /> Ścieżka: ciąg<br /><br /> PolicyId: ciąg<br /><br /> Aby uzyskać więcej informacji na temat kontekstu. LastError, zobacz [obsługi błędów](api-management-error-handling-policies.md).|  
|kontekst. Operacja|Identyfikator: ciąg<br /><br /> Metoda: ciąg<br /><br /> Nazwa: ciąg<br /><br /> UrlTemplate: ciąg|  
|context.Product|Interfejsy API: IEnumerable < IApi\><br /><br /> ApprovalRequired: bool<br /><br /> Grupy: IEnumerable < IGroup\><br /><br /> Identyfikator: ciąg<br /><br /> Nazwa: ciąg<br /><br /> Stan: wyliczenia ProductState {NotPublished, opublikowano}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|  
|kontekst. Żądanie|Treść: IMessageBody<br /><br /> Certificate: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Nagłówki: IReadOnlyDictionary < string, string [] ><br /><br /> Adres IP: ciąg<br /><br /> MatchedParameters: IReadOnlyDictionary < string, string ><br /><br /> Metoda: ciąg<br /><br /> OriginalUrl:IUrl<br /><br /> Adres URL: IUrl|  
|Parametry kontekstu. Request.Headers.GetValueOrDefault (headerName: string, defaultValue: ciąg)|headerName: ciąg<br /><br /> defaultValue: ciąg<br /><br /> Zwraca wartości nagłówka żądania rozdzielonych przecinkami lub `defaultValue` Jeśli nagłówek nie zostanie znaleziony.|  
|kontekst. Odpowiedź|Treść: IMessageBody<br /><br /> Nagłówki: IReadOnlyDictionary < string, string [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: ciąg|  
|Parametry kontekstu. Response.Headers.GetValueOrDefault (headerName: string, defaultValue: ciąg)|headerName: ciąg<br /><br /> defaultValue: ciąg<br /><br /> Zwraca wartości nagłówka odpowiedzi rozdzielonych przecinkami lub `defaultValue` Jeśli nagłówek nie zostanie znaleziony.|  
|kontekst. Subskrypcja|Wartością CreatedTime: Data i godzina<br /><br /> EndDate: Data i godzina?<br /><br /> Identyfikator: ciąg<br /><br /> Klucz: ciąg<br /><br /> Nazwa: ciąg<br /><br /> PrimaryKey: ciąg<br /><br /> Klucz pomocniczy: ciąg<br /><br /> Oprócz parametru StartDate: Data i godzina?|  
|context.User|Adres e-mail: ciąg<br /><br /> Imię: ciąg<br /><br /> Grupy: IEnumerable < IGroup\><br /><br /> Identyfikator: ciąg<br /><br /> Tożsamości: Typ IEnumerable < IUserIdentity\><br /><br /> Nazwisko: ciąg<br /><br /> Uwaga: ciąg<br /><br /> RegistrationDate: Data i godzina|  
|IApi|Identyfikator: ciąg<br /><br /> Nazwa: ciąg<br /><br /> Ścieżka: ciąg<br /><br /> Protokoły: IEnumerable < string\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|  
|IGroup|Identyfikator: ciąg<br /><br /> Nazwa: ciąg|  
|IMessageBody|Jako < T\>(preserveContent: bool = false): gdy T: ciągu, JObject, JToken, JArray, XNode XElement i XDocument<br /><br /> `context.Request.Body.As<T>` i `context.Response.Body.As<T>` metody są używane do odczytu, żądań i odpowiedzi, treści wiadomości w określonym typie `T`. Domyślnie metoda korzysta z oryginalnego strumienia treści komunikatu i renderuje ona niedostępna po zwraca. Aby uniknąć przez metody działają na kopię w strumieniu treści, należy ustawić `preserveContent` parametr `true`. Przejdź [tutaj](api-management-transformation-policies.md#SetBody) aby zobaczyć przykład.|  
|IUrl|Host: ciąg<br /><br /> Ścieżka: ciąg<br /><br /> Port: int<br /><br /> Zapytań: IReadOnlyDictionary < string, string [] ><br /><br /> Ciąg zapytania: ciąg<br /><br /> Schemat: ciąg|  
|IUserIdentity|Identyfikator: ciąg<br /><br /> Dostawca: ciąg|  
|ISubscriptionKeyParameterNames|Nagłówek: ciąg<br /><br /> Zapytanie: ciąg|  
|ciąg IUrl.Query.GetValueOrDefault (queryParameterName: string, defaultValue: ciąg)|queryParameterName: ciąg<br /><br /> defaultValue: ciąg<br /><br /> Zwraca wartości parametrów zapytania rozdzielonych przecinkami lub `defaultValue` Jeśli parametr nie zostanie znaleziony.|  
|Kontekst T. Variables.GetValueOrDefault < T\>(nazwa_zmiennej: string, defaultValue: T)|nazwa_zmiennej: ciąg<br /><br /> defaultValue: T<br /><br /> Zwraca wartość zmiennej rzutowane na typ `T` lub `defaultValue` Jeśli zmienna nie zostanie znaleziony.<br /><br /> Ta metoda zgłasza wyjątek, jeśli określony typ jest niezgodny z rzeczywisty typ zwracany zmiennej.|  
|BasicAuthCredentials AsBasic(input: this string)|wprowadzania: ciąg<br /><br /> Jeśli parametr wejściowy zawiera prawidłową wartość nagłówka żądania uwierzytelnienia podstawowe uwierzytelnianie HTTP, metoda zwraca obiekt typu `BasicAuthCredentials`; w przeciwnym razie metoda zwraca wartość null.|  
|wartość logiczna TryParseBasic (dane wejściowe: ten ciąg, wynik: się BasicAuthCredentials)|wprowadzania: ciąg<br /><br /> wynik: limit BasicAuthCredentials<br /><br /> Jeśli parametr wejściowy zawiera podstawowe uwierzytelnianie HTTP autoryzacji prawidłową w nagłówku żądania metoda zwraca `true` i parametr wynik zawiera wartość typu `BasicAuthCredentials`; w przeciwnym razie metoda zwraca `false`.|  
|BasicAuthCredentials|Hasło: ciąg<br /><br /> Identyfikator użytkownika: ciąg|  
|Token Jwt AsJwt(input: this string)|wprowadzania: ciąg<br /><br /> Jeśli parametr wejściowy zawiera prawidłową wartość tokenu JWT, metoda zwraca obiekt typu `Jwt`; w przeciwnym razie metoda zwraca `null`.|  
|bool TryParseJwt (dane wejściowe: ten ciąg, wynik: limit Jwt)|wprowadzania: ciąg<br /><br /> wynik: limit Jwt<br /><br /> Jeśli parametr wejściowy zawiera prawidłową wartość tokenu JWT, metoda zwraca `true` i parametr wynik zawiera wartość typu `Jwt`; w przeciwnym razie metoda zwraca `false`.|  
|Jwt|Algorytm: ciąg<br /><br /> Odbiorcy: IEnumerable < string\><br /><br /> Oświadczenia: IReadOnlyDictionary < string, string [] ><br /><br /> Czas wygaśnięcia: Data i godzina?<br /><br /> Identyfikator: ciąg<br /><br /> Wystawca: ciąg<br /><br /> Nie wcześniej niż: Data i godzina?<br /><br /> Podmiot: ciąg<br /><br /> Typ: ciąg|  
|string Jwt.Claims.GetValueOrDefault(claimName: string, defaultValue: string)|claimName: ciąg<br /><br /> defaultValue: ciąg<br /><br /> Zwraca rozdzielonych przecinkami wartości oświadczeń lub `defaultValue` Jeśli nagłówek nie zostanie znaleziony.|
|byte [] Szyfruj (wejściowych: ten byte [], algorytmu: string, klucz: byte [], iv:byte[])|dane wejściowe - zwykłego tekstu do zaszyfrowania<br /><br />algorytmu podpisu — Nazwa algorytmu szyfrowania symetrycznego<br /><br />klucz — klucz szyfrowania<br /><br />IV - wektor inicjowania<br /><br />Zwraca postaci zwykłego tekstu.|
|byte [] Szyfruj (wejściowych: ten byte [], algorytmu: System.Security.Cryptography.SymmetricAlgorithm)|dane wejściowe - zwykłego tekstu do zaszyfrowania<br /><br />algorytmu podpisu — algorytm szyfrowania<br /><br />Zwraca postaci zwykłego tekstu.|
|byte [] Szyfruj (wejściowych: ten byte [], algorytmu: System.Security.Cryptography.SymmetricAlgorithm, klucz: byte [], iv:byte[])|dane wejściowe - zwykłego tekstu do zaszyfrowania<br /><br />algorytmu podpisu — algorytm szyfrowania<br /><br />klucz — klucz szyfrowania<br /><br />IV - wektor inicjowania<br /><br />Zwraca postaci zwykłego tekstu.|
|byte [] / odszyfrowywania (wejściowych: ten byte [], algorytmu: string, klucz: byte [], iv:byte[])|dane wejściowe — tekst szyfrowania do odszyfrowania<br /><br />algorytmu podpisu — Nazwa algorytmu szyfrowania symetrycznego<br /><br />klucz — klucz szyfrowania<br /><br />IV - wektor inicjowania<br /><br />Zwraca zwykłego tekstu.|
|byte [] / odszyfrowywania (wejściowych: ten byte [], algorytmu: System.Security.Cryptography.SymmetricAlgorithm)|dane wejściowe — tekst szyfrowania do odszyfrowania<br /><br />algorytmu podpisu — algorytm szyfrowania<br /><br />Zwraca zwykłego tekstu.|
|byte [] / odszyfrowywania (wejściowych: ten byte [], algorytmu: System.Security.Cryptography.SymmetricAlgorithm, klucz: byte [], iv:byte[])|Tekst wejściowy szyfrowania - wejściowych — do odszyfrowania<br /><br />algorytmu podpisu — algorytm szyfrowania<br /><br />klucz — klucz szyfrowania<br /><br />IV - wektor inicjowania<br /><br />Zwraca zwykłego tekstu.|


## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji, w pracy z tymi zasadami zobacz:

+ [Zasady usługi API Management](api-management-howto-policies.md)
+ [Przekształć interfejsy API](transform-api.md)
+ [Informacje o zasadach](api-management-policy-reference.md) pełną listę zasad i ich ustawienia
+ [Przykłady zasad](policy-samples.md)   
