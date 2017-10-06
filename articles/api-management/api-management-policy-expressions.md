---
title: aaaAzure API Management principuttrycken | Microsoft Docs
description: "Lär dig mer om principen uttryck i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 79da0d6ca3963307ec811a33aaac3d63a7abd97d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-policy-expressions"></a>API Management principuttrycken
Principen uttryck syntax är C# 6.0. Varje uttryck har åtkomst toohello angetts implicit [kontexten](api-management-policy-expressions.md#ContextVariables) variabel och ett tillåtna [delmängd](api-management-policy-expressions.md#CLRTypes) av .NET Framework-typer.  
  
> [!NOTE]
>  Mer information om principen uttryck finns hello [Principuttrycken](https://azure.microsoft.com/documentation/videos/policy-expressions-in-azure-api-management/) video.  
>   
>  Demonstration av hur du konfigurerar principer med hjälp av principuttrycken finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Den här videon innehåller hello följa principen uttryck demonstrationer.  
>   
>  -   10:30 - se hur tooapply princip hello API nivå toosupply kontexten information toohello serverdelstjänst med hello [ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) och [ange HTTP-huvudet](api-management-transformation-policies.md#SetHTTPheader) principer. Det finns en demonstration av anropa en funktion i hello developer-portalen där du kan se dessa principer på arbetet vid 12:10.  
> -   13:50 – se hur toouse hello [Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) princip toopre-auktorisera åtkomst toooperations baserat på token anspråk. Vidarebefordra snabb too15:00 toosee hello principer som konfigurerats i hello redigeraren och sedan too18:50 för en demonstration av anropa en funktion från hello developer-portalen både med och utan hello krävs autentiseringstoken.  
> -   21:00 – se hur toouse en [API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) spåra toosee hur principer utvärderas och hello hello utvärderingsresultat.  
> -   25:25 - se hur toouse principuttrycken med hello [hämta från cache](api-management-caching-policies.md#GetFromCache) och [Store toocache](api-management-caching-policies.md#StoreToCache) principer tooconfigure API Management svaret som matchar hello svar cachelagring av hello cachevaraktighet serverdelstjänst som anges av hello säkerhetskopieras tjänstens `Cache-Control` direktiv.  
> -   34:30 - se hur tooperform innehåll filtrering genom att ta bort dataelement från hello svar togs emot från hello serverdelstjänst med hello [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) och [konfigurera brödtext](api-management-transformation-policies.md#SetBody) principer. Starta en översikt över vid 31:50 toosee [hello mörkt Sky prognos API](https://developer.forecast.io/) används för den här demon.  
> -   hanteringsprinciper för toodownload hello används i den här videon finns hello [api-hantering-exempel principer](https://github.com/Azure/api-management-samples/tree/master/policies) github-lagringsplatsen.  
  
  
##  <a name="Syntax"></a>Syntax  
 Uttryck för enskild instruktion innefattas i `@(expression)`, där `expression` är en giltig C#-uttrycket.  
  
 Flera instruktioner uttryck innefattas i `@{expression}`. Alla kodsökvägar inom flera instruktioner uttryck måste avslutas med en `return` instruktion.  
  
##  <a name="PolicyExpressionsExamples"></a>Exempel  
  
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
  
##  <a name="PolicyExpressionsUsage"></a>Användning  
 Uttryck kan användas som attributvärden eller textvärden i någon av hello API Management [principer](api-management-policies.md), såvida inte hello principreferens anger något annat.  
  
> [!IMPORTANT]
>  Observera att när du använder principuttrycken endast begränsad verifiering av hello principuttrycken när hello princip har definierats. Eftersom hello uttryck körs vid körning i hello inkommande eller utgående pipeline av hello gateway, resulterar körning undantag genereras av hello princip uttryck i ett körningsfel i hello API-anrop.  
  
##  <a name="CLRTypes"></a>.NET framework-typer som är tillåtna i uttryck för principen  
 hello följande tabell visas hello .NET Framework-typer och deras medlemmar som tillåts i princip uttryck.  
  
|CLR-typ|Metoder som stöds|  
|--------------|-----------------------|  
|Newtonsoft.Json.Linq.Extensions|Alla metoder som stöds|  
|Newtonsoft.Json.Linq.JArray|Alla metoder som stöds|  
|Newtonsoft.Json.Linq.JConstructor|Alla metoder som stöds|  
|Newtonsoft.Json.Linq.JContainer|Alla metoder som stöds|  
|Newtonsoft.Json.Linq.JObject|Alla metoder som stöds|  
|Newtonsoft.Json.Linq.JProperty|Alla metoder som stöds|  
|Newtonsoft.Json.Linq.JRaw|Alla metoder som stöds|  
|Newtonsoft.Json.Linq.JToken|Alla metoder som stöds|  
|Newtonsoft.Json.Linq.JTokenType|Alla metoder som stöds|  
|Newtonsoft.Json.Linq.JValue|Alla metoder som stöds|  
|System.Collections.Generic.IReadOnlyCollection < T\>|Alla|  
|System.Collections.Generic.IReadOnlyDictionary < TKey, TValue >|Alla|  
|System.Collections.Generic.ISet < TKey, TValue >|Alla|  
|System.Collections.Generic.KeyValuePair < TKey, TValue >|Nyckel/värde|  
|System.Collections.Generic.List < TKey, TValue >|Alla|  
|System.Collections.Generic.Queue < TKey, TValue >|Alla|  
|System.Collections.Generic.Stack < TKey, TValue >|Alla|  
|System.Convert|Alla|  
|System.DateTime|Alla|  
|System.DateTimeKind|UTC|  
|System.DateTimeOffset|Alla|  
|System.Decimal|Alla|  
|System.Double|Alla|  
|System.Guid|Alla|  
|System.IEnumerable < T\>|Alla|  
|System.IEnumerator < T\>|Alla|  
|System.Int16|Alla|  
|System.Int32|Alla|  
|System.Int64|Alla|  
|System.Linq.Enumerable < T\>|Alla metoder som stöds|  
|System.Math|Alla|  
|System.MidpointRounding|Alla|  
|System.Nullable < T\>|Alla|  
|System.Random|Alla|  
|System.SByte|Alla|  
|System.Security.Cryptography. HMACSHA384|Alla|  
|System.Security.Cryptography. HMACSHA512|Alla|  
|System.Security.Cryptography.HashAlgorithm|Alla|  
|System.Security.Cryptography.HMAC|Alla|  
|System.Security.Cryptography.HMACMD5|Alla|  
|System.Security.Cryptography.HMACSHA1|Alla|  
|System.Security.Cryptography.HMACSHA256|Alla|  
|System.Security.Cryptography.KeyedHashAlgorithm|Alla|  
|System.Security.Cryptography.MD5|Alla|  
|System.Security.Cryptography.RNGCryptoServiceProvider|Alla|  
|System.Security.Cryptography.SHA1|Alla|  
|System.Security.Cryptography.SHA1Managed|Alla|  
|System.Security.Cryptography.SHA256|Alla|  
|System.Security.Cryptography.SHA256Managed|Alla|  
|System.Security.Cryptography.SHA384|Alla|  
|System.Security.Cryptography.SHA384Managed|Alla|  
|System.Security.Cryptography.SHA512|Alla|  
|System.Security.Cryptography.SHA512Managed|Alla|  
|System.Single|Alla|  
|System.String|Alla|  
|System.StringSplitOptions|Alla|  
|System.Text.Encoding|Alla|  
|System.Text.RegularExpressions.Capture|Längd, indexvärde|  
|System.Text.RegularExpressions.CaptureCollection|Antal objekt|  
|System.Text.RegularExpressions.Group|Insamlingar, lyckades|  
|System.Text.RegularExpressions.GroupCollection|Antal objekt|  
|System.Text.RegularExpressions.Match|Tom, grupper, resultat|  
|System.Text.RegularExpressions.Regex|.ctor IsMatch, matchning matchar, Ersätt|  
|System.Text.RegularExpressions.RegexOptions|Kompileras IgnoreCase, IgnorePatternWhitespace, lambda, None, RightToLeft, Singleline|  
|System.TimeSpan|Alla|  
|System.Tuple|Alla|  
|System.UInt16|Alla|  
|System.UInt32|Alla|  
|System.UInt64|Alla|  
|System.Uri|Alla|  
|System.Xml.Linq.Extensions|Alla metoder som stöds|  
|System.Xml.Linq.XAttribute|Alla metoder som stöds|  
|System.Xml.Linq.XCData|Alla metoder som stöds|  
|System.Xml.Linq.XComment|Alla metoder som stöds|  
|System.Xml.Linq.XContainer|Alla metoder som stöds|  
|System.Xml.Linq.XDeclaration|Alla metoder som stöds|  
|System.Xml.Linq.XDocument|Alla metoder som stöds|  
|System.Xml.Linq.XDocumentType|Alla metoder som stöds|  
|System.Xml.Linq.XElement|Alla metoder som stöds|  
|System.Xml.Linq.XName|Alla metoder som stöds|  
|System.Xml.Linq.XNamespace|Alla metoder som stöds|  
|System.Xml.Linq.XNode|Alla metoder som stöds|  
|System.Xml.Linq.XNodeDocumentOrderComparer|Alla metoder som stöds|  
|System.Xml.Linq.XNodeEqualityComparer|Alla metoder som stöds|  
|System.Xml.Linq.XObject|Alla metoder som stöds|  
|System.Xml.Linq.XProcessingInstruction|Alla metoder som stöds|  
|System.Xml.Linq.XText|Alla metoder som stöds|  
|System.Xml.XmlNodeType|Alla|  
  
##  <a name="ContextVariables"></a>Kontexten variabel  
 En variabel med namnet `context` implicit finns i varje princip [uttryck](api-management-policy-expressions.md#Syntax). Dess medlemmar ger information relevanta toohello `\request`. Alla hello `context` medlemmar är skrivskyddad.  
  
|Kontexten variabel|Tillåtna metoder, egenskaper och parametervärden|  
|----------------------|-------------------------------------------------------|  
|Kontexten|API: IApi<br /><br /> Distribution<br /><br /> LastError<br /><br /> Åtgärd<br /><br /> Produkt<br /><br /> Förfrågan<br /><br /> Begärande-ID: sträng<br /><br /> Svar<br /><br /> Prenumeration<br /><br /> Spårning: bool<br /><br /> Användare<br /><br /> Variabler: IReadOnlyDictionary < sträng, objekt ><br /><br /> void Trace(message: string)|  
|kontexten. API|ID: sträng<br /><br /> Namn: sträng<br /><br /> Sökväg: sträng<br /><br /> ServiceUrl: IUrl|  
|kontexten. Distribution|Region: sträng<br /><br /> Namn på tjänst: sträng|  
|kontexten. LastError|Källa: sträng<br /><br /> Orsak: sträng<br /><br /> Meddelande: sträng<br /><br /> Omfång: sträng<br /><br /> Avsnittet: sträng<br /><br /> Sökväg: sträng<br /><br /> PolicyId: sträng<br /><br /> Mer information om kontexten. LastError, se [felhantering](api-management-error-handling-policies.md).|  
|kontexten. Åtgärden|ID: sträng<br /><br /> Metod: sträng<br /><br /> Namn: sträng<br /><br /> UrlTemplate: sträng|  
|kontexten. Produkten|API: er: IEnumerable < IApi\><br /><br /> ApprovalRequired: bool<br /><br /> Grupper: IEnumerable < IGroup\><br /><br /> ID: sträng<br /><br /> Namn: sträng<br /><br /> Tillstånd: enum ProductState {NotPublished, publicerad}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|  
|kontexten. Begäran|Brödtext: IMessageBody<br /><br /> Certifikat: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Huvuden: IReadOnlyDictionary < string, string [] ><br /><br /> IP-adress: sträng<br /><br /> MatchedParameters: IReadOnlyDictionary < sträng, sträng ><br /><br /> Metod: sträng<br /><br /> OriginalUrl:IUrl<br /><br /> URL: IUrl|  
|strängkontext. Request.Headers.GetValueOrDefault (headerName: sträng, defaultValue: sträng)|headerName: sträng<br /><br /> Standardvärde: sträng<br /><br /> Returnerar kommaseparerade värden i huvudet begäran eller `defaultValue` om hello huvudet inte kunde hittas.|  
|kontexten. Svar|Brödtext: IMessageBody<br /><br /> Huvuden: IReadOnlyDictionary < string, string [] ><br /><br /> StatusCode: int<br /><br /> StatusReason: sträng|  
|strängkontext. Response.Headers.GetValueOrDefault (headerName: sträng, defaultValue: sträng)|headerName: sträng<br /><br /> Standardvärde: sträng<br /><br /> Returnerar svaret fil med kommaavgränsade värden i huvudet eller `defaultValue` om hello huvudet inte kunde hittas.|  
|kontexten. Prenumerationen|CreatedTime: DateTime<br /><br /> EndDate: DateTime?<br /><br /> ID: sträng<br /><br /> Nyckel: sträng<br /><br /> Namn: sträng<br /><br /> PrimaryKey: sträng<br /><br /> Sekundär nyckel: sträng<br /><br /> StartDate: DateTime?|  
|kontexten. Användaren|E-post: sträng<br /><br /> Förnamn: sträng<br /><br /> Grupper: IEnumerable < IGroup\><br /><br /> ID: sträng<br /><br /> Identiteter: IEnumerable < IUserIdentity\><br /><br /> Efternamn: sträng<br /><br /> Obs: sträng<br /><br /> RegistrationDate: DateTime|  
|IApi|ID: sträng<br /><br /> Namn: sträng<br /><br /> Sökväg: sträng<br /><br /> Protokoll: IEnumerable < sträng\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|  
|IGroup|ID: sträng<br /><br /> Namn: sträng|  
|IMessageBody|Som < T\>(preserveContent: bool = false): där T: sträng, JObject, JToken, JArray, XNode, XElement, XDocument<br /><br /> Hej `context.Request.Body.As<T>` och `context.Response.Body.As<T>` metoder är används tooread ett meddelande om begäran och svar organ i en viss typ av `T`. Som standard hello metoden använder hello ursprungliga brödtext meddelandeströmmen och reneders tillgänglig efter det returneras. tooavoid som genom att låta hello-metoden fungerar på en kopia av hello brödtextströmmen, ange hello `preserveContent` parameter för`true`. Gå [här](api-management-transformation-policies.md#SetBody) toosee ett exempel.|  
|IUrl|Värden: sträng<br /><br /> Sökväg: sträng<br /><br /> Port: int<br /><br /> Fråga: IReadOnlyDictionary < string, string [] ><br /><br /> QueryString: sträng<br /><br /> Schema: sträng|  
|IUserIdentity|ID: sträng<br /><br /> Providern: sträng|  
|ISubscriptionKeyParameterNames|Rubrik: sträng<br /><br /> Fråga: sträng|  
|sträng IUrl.Query.GetValueOrDefault (queryParameterName: sträng, defaultValue: sträng)|queryParameterName: sträng<br /><br /> Standardvärde: sträng<br /><br /> Returnerar kommaavgränsade parametervärden för frågan eller `defaultValue` om hello parametern inte hittas.|  
|T-kontext. Variables.GetValueOrDefault < T\>(variabelnamn: sträng, defaultValue: T)|Variabelnamn: sträng<br /><br /> Standardvärde: T<br /><br /> Returnerar värdet på variabeln omvandlingen tootype `T` eller `defaultValue` om hello variabeln inte hittas.<br /><br /> Den här metoden genereras ett undantag om hello angivna typen inte matchar hello faktiska typen av hello returnerade variabeln.|  
|BasicAuthCredentials AsBasic(input: this string)|indata: sträng<br /><br /> Om hello Indataparametern innehåller ett giltigt grundläggande HTTP-autentisering auktorisering begäran sidhuvud värde, hello-metoden returnerar ett objekt av typen `BasicAuthCredentials`; annars hello-metoden returnerar null.|  
|bool TryParseBasic (indata: den här strängen, resultat: ut BasicAuthCredentials)|indata: sträng<br /><br /> resultatet: ut BasicAuthCredentials<br /><br /> Om hello Indataparametern innehåller ett giltigt grundläggande HTTP-autentisering auktorisering begäran sidhuvud värde, hello-metoden returnerar `true` och hello resultatet parametern innehåller ett värde av typen `BasicAuthCredentials`; annars hello-metoden returnerar `false`.|  
|BasicAuthCredentials|Lösenord: sträng<br /><br /> Användar-ID: sträng|  
|Jwt AsJwt(input: this string)|indata: sträng<br /><br /> Om hello Indataparametern innehåller ett giltigt JWT-token värde, hello-metoden returnerar ett objekt av typen `Jwt`; annars hello-metoden returnerar `null`.|  
|bool TryParseJwt (indata: den här strängen, resultat: ut Jwt)|indata: sträng<br /><br /> resultatet: ut Jwt<br /><br /> Om hello Indataparametern innehåller ett giltigt JWT-token värde, hello-metoden returnerar `true` och hello resultatet parametern innehåller ett värde av typen `Jwt`; annars hello-metoden returnerar `false`.|  
|Jwt|Algoritm: sträng<br /><br /> Målgrupp: IEnumerable < sträng\><br /><br /> Anspråk: IReadOnlyDictionary < string, string [] ><br /><br /> ExpirationTime: DateTime?<br /><br /> ID: sträng<br /><br /> Utfärdare: sträng<br /><br /> Inte före: DateTime?<br /><br /> Ämne: sträng<br /><br /> Typ: sträng|  
|sträng Jwt.Claims.GetValueOrDefault (claimName: sträng, defaultValue: sträng)|claimName: sträng<br /><br /> Standardvärde: sträng<br /><br /> Returnerar kommaavgränsad anspråksvärden eller `defaultValue` om hello huvudet inte kunde hittas.|

## <a name="next-steps"></a>Nästa steg
Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).  
