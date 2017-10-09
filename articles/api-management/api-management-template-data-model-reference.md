---
title: aaaAzure API Management-mall datamodell referens | Microsoft Docs
description: "Läs mer om hello entitet och typ garantier för vanliga objekt som används i hello datamodeller för hello developer portal mallar i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 7d049d8ecc9e597cf48ce0c820c172798bcf86de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-template-data-model-reference"></a>Azure API Management-mall datamodell referens
Det här avsnittet beskriver hello entitet och typ garantier för vanliga objekt som används i hello datamodeller för hello developer portal mallar i Azure API Management.  
  
 Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
-   [API](#API)  
-   [API-översikten](#APISummary)  
-   [Programmet](#Application)  
-   [Bifogad fil](#Attachment)  
-   [Kodexempel](#Sample)  
-   [Kommentar](#Comment)  
-   [Filtrering](#Filtering)  
-   [Huvudet](#Header)  
-   [HTTP-begäran](#HTTPRequest)  
-   [HTTP-svar](#HTTPResponse)  
-   [Problemet](#Issue)  
-   [Åtgärd](#Operation)  
-   [Åtgärd-menyn](#Menu)  
-   [Åtgärden menyobjekt](#MenuItem)  
-   [Växling](#Paging)  
-   [Parametern](#Parameter)  
-   [Produkten](#Product)  
-   [Leverantör](#Provider)  
-   [Representation](#Representation)  
-   [Prenumeration](#Subscription)  
-   [Prenumerationen sammanfattning](#SubscriptionSummary)  
-   [Information om användarkonto](#UserAccountInfo)  
-   [Användaren logga in](#UseSignIn)  
-   [Användaren loggar in](#UserSignUp)  
  
##  <a name="API"></a>API  
 Hej `API` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|id|Sträng|Resursidentifieraren. Identifierar hello API inom hello aktuella API Management service-instans. hello-värdet är en giltig relativ URL i hello-format för `apis/{id}` där `{id}` är en API-identifierare. Den här egenskapen är skrivskyddad.|  
|namn|Sträng|Namnet på hello API. Får inte vara tomt. Maximal längd är 100 tecken.|  
|description|Sträng|Beskrivning av hello API. Får inte vara tomt. Kan innehålla HTML-kod. Maximal längd är 1000 tecken.|  
|serviceUrl|Sträng|Absolut URL för hello serverdelstjänst implementera detta API.|  
|Sökväg|Sträng|Relativ URL som unikt identifierar denna API och alla dess resurs sökvägar i hello API Management service-instans. Det läggs toohello API-slutpunkten bas-URL anges när hello service-instans skapas tooform en offentlig URL för detta API.|  
|Protokoll|matris med tal|Beskriver på vilket protokoll hello åtgärder i detta API kan anropas. Tillåtna värden är `1 - http` och `2 - https`, eller båda.|  
|authenticationSettings|[Auktoriseringsinställningar server-autentisering](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Samling autentiseringsinställningar som ingår i detta API.|  
|subscriptionKeyParameterNames|Objektet|Egenskapen som kan använda toospecify anpassade namn för frågan och/eller huvudet parametrar som innehåller hello prenumeration nyckel. När den här egenskapen är finns måste det innehålla minst en av följande egenskaper för hello två.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a>API-översikten  
 Hej `API summary` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|id|Sträng|Resursidentifieraren. Identifierar hello API inom hello aktuella API Management service-instans. hello-värdet är en giltig relativ URL i hello-format för `apis/{id}` där `{id}` är en API-identifierare. Den här egenskapen är skrivskyddad.|  
|namn|Sträng|Namnet på hello API. Får inte vara tomt. Maximal längd är 100 tecken.|  
|description|Sträng|Beskrivning av hello API. Får inte vara tomt. Kan innehålla HTML-kod. Maximal längd är 1000 tecken.|  
  
##  <a name="Application"></a>Programmet  
 Hej `application` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Id|Sträng|hello Unik identifierare för programmet hello.|  
|Rubrik|Sträng|hello titel hello program.|  
|Beskrivning|Sträng|hello beskrivning av programmet hello.|  
|URL|URI: N|hello URI för hello program.|  
|Version|Sträng|Versionsinformation för hello program.|  
|Krav|Sträng|En beskrivning av kraven för programmet hello.|  
|Status|Antal|hello aktuella tillstånd hello program.<br /><br /> -0 - registrerad<br /><br /> -1 - har skickats<br /><br /> -2 - publicerade<br /><br /> -3 - avvisade<br /><br /> -4 - Opublicerat|  
|RegistrationDate|Datum och tid|hello datum och tid hello-programmet har registrerats.|  
|CategoryId|Antal|hello kategori av programmet hello (ekonomi, underhållning osv.)|  
|DeveloperId|Sträng|hello Unik identifierare för hello utvecklare som skickats hello program.|  
|Bifogade filer|Samling av [bilaga](#Attachment) entiteter.|Eventuella bifogade filer för hello program, till exempel skärmdumpar eller ikoner.|  
|Ikon|[Bifogad fil](#Attachment)|hello ikonen hello för hello program.|  
  
##  <a name="Attachment"></a>Bifogad fil  
 Hej `attachment` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Unikt ID|Sträng|hello Unik identifierare för hello bifogad fil.|  
|URL|Sträng|hello-URL för hello resurs.|  
|Typ|Sträng|hello typ av bifogade filer.|  
|ContentType|Sträng|hello medietyp för hello bifogad fil.|  
  
##  <a name="Sample"></a>Kodexempel  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Rubrik|Sträng|hello namnet på hello igen.|  
|fragment|Sträng|Den här egenskapen är inaktuell och bör inte användas.|  
|pensel|Sträng|Som code syntax färgläggning mallen toobe används för att visa hello kodexempel. Tillåtna värden är `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, och `csharp`.|  
|mall|Sträng|hello namnet på den här koden exempelmall.|  
|Brödtext|Sträng|En platshållare för hello kod exempel del av hello fragment.|  
|Metoden|Sträng|hello HTTP hello funktionssätt.|  
|schemat|Sträng|hello protokollet toouse för hello åtgärden begäran.|  
|Sökväg|Sträng|hello sökväg hello igen.|  
|DocumentDB|Sträng|Exempel på frågan sträng med definierade parametrar.|  
|värden|Sträng|hello-URL för hello API Management service-gateway för hello API som innehåller den här åtgärden.|  
|Rubriker|Samling av [huvud](#Header) entiteter.|Huvuden för den här åtgärden.|  
|parameters|Samling av [parametern](#Parameter) entiteter.|Parametrar som definierats för den här åtgärden.|  
  
##  <a name="Comment"></a>Kommentar  
 Hej `API` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Id|Antal|hello-id för hello kommentar.|  
|CommentText|Sträng|hello brödtext hello kommentar. Kan innehålla HTML.|  
|DeveloperCompany|Sträng|hello namn hello-utvecklare.|  
|PostedOn|Datum och tid|hello datum och tid hello kommentar anslogs.|  
  
##  <a name="Issue"></a>Problemet  
 Hej `issue` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Id|Sträng|hello Unik identifierare för hello problemet.|  
|ApiID|Sträng|hello-id för hello API för vilken det här problemet har rapporterats.|  
|Rubrik|Sträng|Rubrik på hello problemet.|  
|Beskrivning|Sträng|Beskrivning av hello problemet.|  
|SubscriptionDeveloperName|Sträng|Hello-utvecklare som rapporterats hello problemet förnamn.|  
|IssueState|Sträng|hello aktuella tillstånd hello problemet. Möjliga värden är Proposed, öppnade, stängd.|  
|ReportedOn|Datum och tid|hello datum och tid hello problemet har rapporterats.|  
|Kommentarer|Samling av [kommentar](#Comment) entiteter.|Kommentarer om det här problemet.|  
|Bifogade filer|Samling av [bilaga](api-management-template-data-model-reference.md#Attachment) entiteter.|Eventuella bifogade filer toohello problem.|  
|Tjänster|Samling av [API](#API) entiteter.|hello API: er prenumererar tooby hello användaren som skapade hello problemet.|  
  
##  <a name="Filtering"></a>Filtrering  
 Hej `filtering` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Mönstret|Sträng|hello aktuella sökterm; eller `null` om det finns ingen sökterm.|  
|Platshållare|Sträng|hello text toodisplay i sökrutan hello när det finns ingen sökterm som angetts.|  
  
##  <a name="Header"></a>Huvudet  
 Det här avsnittet beskrivs hello `parameter` representation.  
  
|Egenskap|Beskrivning|Typ|  
|--------------|-----------------|----------|  
|namn|Sträng|Parameternamnet.|  
|description|Sträng|Parameterbeskrivning.|  
|värde|Sträng|Huvudets värde.|  
|TypeName|Sträng|Datatypen för huvudvärde.|  
|alternativ|Sträng|Alternativ.|  
|Krävs|Booleskt värde|Om hello-huvudet måste anges.|  
|Skrivskyddad|Booleskt värde|Om hello-huvud är skrivskyddad.|  
  
##  <a name="HTTPRequest"></a>HTTP-begäran  
 Det här avsnittet beskrivs hello `request` representation.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|description|Sträng|Beskrivning av åtgärden begäran.|  
|Rubriker|matris med [huvud](#Header) entiteter.|Huvuden för begäran.|  
|parameters|matris med [Parameter](#Parameter)|De parametrar som begäran igen.|  
|garantier|matris med [Representation](#Representation)|Åtgärden begäran garantier samling.|  
  
##  <a name="HTTPResponse"></a>HTTP-svar  
 Det här avsnittet beskrivs hello `response` representation.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|statusCode|Positivt heltal|Åtgärden Svarets statuskod.|  
|description|Sträng|Beskrivning av åtgärden svar.|  
|garantier|matris med [Representation](#Representation)|Åtgärden svar garantier samling.|  
  
##  <a name="Operation"></a>Åtgärden  
 Hej `operation` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|id|Sträng|Resursidentifieraren. Identifierar hello åtgärden inom hello aktuella API Management service-instans. hello-värdet är en giltig relativ URL i hello-format för `apis/{aid}/operations/{id}` där `{aid}` är en API-identifierare och `{id}` är en identifierare för åtgärden. Den här egenskapen är skrivskyddad.|  
|namn|Sträng|Namnet på hello igen. Får inte vara tomt. Maximal längd är 100 tecken.|  
|description|Sträng|Beskrivning av hello igen. Får inte vara tomt. Kan innehålla HTML-kod. Maximal längd är 1000 tecken.|  
|schemat|Sträng|Beskriver på vilket protokoll hello åtgärder i detta API kan anropas. Tillåtna värden är `http`, `https`, eller båda `http` och `https`.|  
|uriTemplate|Sträng|Relativa URL: en mall identifierar hello målresursen för den här åtgärden. Kan innehålla parametrar. Exempel:`customers/{cid}/orders/{oid}/?date={date}`|  
|värden|Sträng|hello API Management gateway URL som är värd för hello API.|  
|HttpMethod|Sträng|HTTP-åtgärdsmetod.|  
|Begäran|[HTTP-begäran](#HTTPRequest)|En entitet som innehåller detaljer om förfrågan.|  
|svar|matris med [HTTP-svar](#HTTPResponse)|Matris med åtgärden [HTTP-svar](#HTTPResponse) entiteter.|  
  
##  <a name="Menu"></a>Åtgärd-menyn  
 Hej `operation menu` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|ApiId|Sträng|hello-id för hello aktuella API.|  
|CurrentOperationId|Sträng|hello-id för hello den aktuella åtgärden.|  
|Åtgärd|Sträng|Hej Menytyp.|  
|MenuItems|Samling av [åtgärden menyalternativet](#MenuItem) entiteter.|hello åtgärder för hello aktuella API.|  
  
##  <a name="MenuItem"></a>Åtgärden menyobjekt  
 Hej `operation menu item` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Id|Sträng|hello-id för hello igen.|  
|Rubrik|Sträng|hello beskrivning av hello igen.|  
|HttpMethod|Sträng|hello Http hello funktionssätt.|  
  
##  <a name="Paging"></a>Växling  
 Hej `paging` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Sidan|Antal|hello aktuella sidnumret.|  
|PageSize|Antal|Hej Max antal resultat toobe visas på en sida.|  
|TotalItemCount|Antal|hello antal element för visning.|  
|{ShowAll|Booleskt värde|Om alla toosho resulterar på en sida.|  
|PageCount|Antal|hello antalet sidor i resultaten.|  
  
##  <a name="Parameter"></a>Parametern  
 Det här avsnittet beskrivs hello `parameter` representation.  
  
|Egenskap|Beskrivning|Typ|  
|--------------|-----------------|----------|  
|namn|Sträng|Parameternamnet.|  
|description|Sträng|Parameterbeskrivning.|  
|värde|Sträng|Parametervärdet.|  
|alternativ|Strängmatris|Värden som definierats för frågeparametervärden.|  
|Krävs|Booleskt värde|Anger om parametern är obligatorisk.|  
|typ|Antal|Om den här parametern är en sökvägsparameter (1) eller en querystring-parameter (2).|  
|TypeName|Sträng|Parametertypen.|  
  
##  <a name="Product"></a>Produkten  
 Hej `product` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Id|Sträng|Resursidentifieraren. Identifierar hello produkt i hello aktuella API Management service-instans. hello-värdet är en giltig relativ URL i hello-format för `products/{pid}` där `{pid}` är en produktidentifierare. Den här egenskapen är skrivskyddad.|  
|Rubrik|Sträng|Namnet på hello produkten. Får inte vara tomt. Maximal längd är 100 tecken.|  
|Beskrivning|Sträng|Beskrivning av hello produkten. Får inte vara tomt. Kan innehålla HTML-kod. Maximal längd är 1000 tecken.|  
|Villkor|Sträng|Produkten villkor för användning. Utvecklare som försöker toosubscribe toohello produkten visas och krävs tooaccept villkoren innan de kan slutföras hello prenumeration.|  
|ProductState|Antal|Anger om hello produkten är publicerad eller inte. Publicerade produkter är kan upptäckas av utvecklare på hello developer-portalen. Icke-publicerade produkter är synliga endast tooadministrators.<br /><br /> hello tillåtna värden för Produkttillstånd är:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|Booleskt värde|Anger om en användare kan ha flera prenumerationer toothis produkten vid hello samtidigt.|  
|MultipleSubscriptionsCount|Antal|hello antalet prenumerationer toothis produkten av hello aktuella användaren.|  
  
##  <a name="Provider"></a>Providern  
 Hej `provider` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Egenskaper|sträng ordlista|Egenskaper för den här autentiseringsprovider.|  
|AuthenticationType|Sträng|hello providertyp. (Azure Active Directory, Facebook-inloggningar, Google-konto, Account, Twitter).|  
|Beskrivning|Sträng|Visningsnamnet för hello-providern.|  
  
##  <a name="Representation"></a>Representation  
 Det här avsnittet beskrivs en `representation`.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|ContentType|Sträng|Anger en registrerad eller anpassat innehåll representation, t.ex. `application/xml`.|  
|exempel|Sträng|Ett exempel på hello representation.|  
  
##  <a name="Subscription"></a>Prenumerationen  
 Hej `subscription` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Id|Sträng|Resursidentifieraren. Identifierar hello prenumeration inom hello aktuella API Management service-instans. hello-värdet är en giltig relativ URL i hello-format för `subscriptions/{sid}` där `{sid}` är ett prenumerations-ID. Den här egenskapen är skrivskyddad.|  
|ProductId|Sträng|hello produkten resursidentifieraren för hello abonnemang produkt. hello-värdet är en giltig relativ URL i hello-format för `products/{pid}` där `{pid}` är en produktidentifierare.|  
|ProductTitle|Sträng|Namnet på hello produkten. Får inte vara tomt. Maximal längd är 100 tecken.|  
|Produktbeskrivning|Sträng|Beskrivning av hello produkten. Får inte vara tomt. Kan innehålla HTML-kod. Maximal längd är 1000 tecken.|  
|ProductDetailsUrl|Sträng|Relativ URL toohello-produktinformation.|  
|state|Sträng|hello tillståndet för hello prenumeration. Möjliga tillstånd är:<br /><br /> - `0 - suspended`– hello prenumeration är blockerat och hello prenumeranten kan inte anropa alla API: er för hello produkten.<br /><br /> - `1 - active`– hello prenumeration är aktiv.<br /><br /> - `2 - expired`– hello prenumerationen uppnåtts dess förfallodatum och har inaktiverats.<br /><br /> - `3 - submitted`– hello prenumerationsbegäran har gjorts av hello utvecklare, men har ännu inte godkänts eller avvisats.<br /><br /> - `4 - rejected`– hello prenumerationsbegäran har nekats av en administratör.<br /><br /> - `5 - cancelled`– hello prenumerationen har avbrutits av hello utvecklare eller administratör.|  
|Visningsnamn|Sträng|Visningsnamnet för hello prenumeration.|  
|CreatedDate|Datum och tid|hello datum hello prenumeration skapades i ISO 8601-format: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|Booleskt värde|Om hello prenumeration avbrytas av hello aktuella användaren.|  
|IsAwaitingApproval|Booleskt värde|Om hello prenumerationen väntar på godkännande.|  
|Startdatum|Datum och tid|hello startdatum för hello-prenumeration i ISO 8601-format: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|Datum och tid|hello utgångsdatum för hello-prenumeration i ISO 8601-format: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Datum och tid|hello datum för hello-prenumeration i ISO 8601-format: `2014-06-24T16:25:00Z`.|  
|primaryKey|Sträng|hello primära prenumeration nyckel. Maximal längd är 256 tecken.|  
|sekundär nyckel|Sträng|hello sekundära prenumeration nyckel. Maximal längd är 256 tecken.|  
|CanBeRenewed|Booleskt värde|Om hello prenumerationen förnyas av hello aktuella användaren.|  
|HasExpired|Booleskt värde|Om hello prenumerationen har gått ut.|  
|IsRejected|Booleskt värde|Om hello prenumerationsbegäran nekades.|  
|CancelUrl|Sträng|hello relativ Url toocancel hello prenumeration.|  
|RenewUrl|Sträng|hello relativ Url toorenew hello prenumeration.|  
  
##  <a name="SubscriptionSummary"></a>Prenumerationen sammanfattning  
 Hej `subscription summary` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Id|Sträng|Resursidentifieraren. Identifierar hello prenumeration inom hello aktuella API Management service-instans. hello-värdet är en giltig relativ URL i hello-format för `subscriptions/{sid}` där `{sid}` är ett prenumerations-ID. Den här egenskapen är skrivskyddad.|  
|Visningsnamn|Sträng|hello visningsnamn för hello prenumeration|  
  
##  <a name="UserAccountInfo"></a>Information om användarkonto  
 Hej `user account info` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Förnamn|Sträng|Förnamn. Får inte vara tomt. Maximal längd är 100 tecken.|  
|Efternamn|Sträng|Efternamn. Får inte vara tomt. Maximal längd är 100 tecken.|  
|E-post|Sträng|E-postadress. Får inte vara tomt och måste vara unika inom hello tjänstinstansen. Maximal längd är 254 tecken.|  
|Lösenord|Sträng|Lösenordet för användarkontot.|  
|NameIdentifier|Sträng|Konto-ID, hello samma som hello användarens e-postadress.|  
|ProviderName|Sträng|Providernamn för autentisering.|  
|IsBasicAccount|Booleskt värde|TRUE om det här kontot har registrerats med hjälp av e-post och lösenord. FALSKT om hello-konto har registrerats med hjälp av en provider.|  
  
##  <a name="UseSignIn"></a>Användaren logga in  
 Hej `user sign in` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|E-post|Sträng|E-postadress. Får inte vara tomt och måste vara unika inom hello tjänstinstansen. Maximal längd är 254 tecken.|  
|Lösenord|Sträng|Lösenordet för användarkontot.|  
|ReturnUrl|Sträng|hello URL till hello sida där hello användaren klickat på Logga in.|  
|RememberMe|Booleskt värde|Om toosave hello aktuella användarens information.|  
|RegistrationEnabled|Booleskt värde|Om registrering är aktiverat.|  
|DelegationEnabled|Booleskt värde|Om delegerade inloggning är aktiverat.|  
|DelegationUrl|Sträng|hello delegerade tecken i URL: en, om aktiverat.|  
|SsoSignUpUrl|Sträng|hello enkel inloggning URL för hello användaren, om den finns.|  
|AuxServiceUrl|Sträng|Om hello aktuell användare är administratör är en länk toohello tjänstinstans i hello klassiska Azure-portalen.|  
|Leverantörer|Samling av [Provider](#Provider) entiteter|hello autentiseringsproviders för den här användaren.|  
|UserRegistrationTerms|Sträng|Villkoren som en användare måste acceptera toobefore logga in.|  
|UserRegistrationTermsEnabled|Booleskt värde|Om villkoren är aktiverade.|  
  
##  <a name="UserSignUp"></a>Användaren loggar in  
 Hej `user sign up` entiteten har hello följande egenskaper.  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|LösenordBekräfta|Booleskt värde|Värdet som används av hello [anmälan](api-management-page-controls.md#sign-up)anmälan kontroll.|  
|Lösenord|Sträng|Lösenordet för användarkontot.|  
|PasswordVerdictLevel|Antal|Värdet som används av hello [anmälan](api-management-page-controls.md#sign-up)anmälan kontroll.|  
|UserRegistrationTerms|Sträng|Villkoren som en användare måste acceptera toobefore logga in.|  
|UserRegistrationTermsOptions|Antal|Värdet som används av hello [anmälan](api-management-page-controls.md#sign-up)anmälan kontroll.|  
|ConsentAccepted|Booleskt värde|Värdet som används av hello [anmälan](api-management-page-controls.md#sign-up)anmälan kontroll.|  
|E-post|Sträng|E-postadress. Får inte vara tomt och måste vara unika inom hello tjänstinstansen. Maximal längd är 254 tecken.|  
|Förnamn|Sträng|Förnamn. Får inte vara tomt. Maximal längd är 100 tecken.|  
|Efternamn|Sträng|Efternamn. Får inte vara tomt. Maximal längd är 100 tecken.|  
|UserData|Sträng|Värdet som används av hello [anmälan](api-management-page-controls.md#sign-up) kontroll.|  
|NameIdentifier|Sträng|Värdet som används av hello [anmälan](api-management-page-controls.md#sign-up)anmälan kontroll.|  
|ProviderName|Sträng|Providernamn för autentisering.|

## <a name="next-steps"></a>Nästa steg
Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).
