---
title: aaaError hantering i Azure API Management-principer | Microsoft Docs
description: "Lär dig hur hello toorespond tooerror villkor som kan uppstå under bearbetning av begäranden i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 3c777964-02b2-4f55-8731-8c3bd3c0ae27
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 002c65f21e763fd644da61b6a11685ffd97488c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="error-handling-in-api-management-policies"></a>Hantera fel i API Management-principer
Azure API Management gör utgivare toorespond tooerror villkor som kan uppstå under hello bearbetning av begäranden toohello proxy genom att tillhandahålla en `ProxyError` objekt. Hej `ProxyError` objektet nås via hello [kontext. LastError](api-management-policy-expressions.md#ContextVariables) egenskap och kan användas av principer i hello `on-error` princip. Det här avsnittet innehåller en referens till hello fel funktioner för hantering i Azure API Management.  
  
## <a name="error-handling-in-api-management"></a>Hantera fel i API Management  
 Principer i Azure API Management är indelade i `inbound`, `backend`, `outbound`, och `on-error` avsnitten som visas i följande exempel hello.  
  
```xml  
<policies>  
  <inbound>  
    <!-- statements toobe applied toohello request go here -->  
  </inbound>  
  <backend>  
    <!-- statements toobe applied before hello request is   
         forwarded toohello backend service go here -->  
    </backend>  
    <outbound>  
      <!-- statements toobe applied toohello response go here -->  
    </outbound>  
    <on-error>  
        <!-- statements toobe applied if there is an error   
             condition go here -->  
  </on-error>  
</policies>  
```  
  
 Inbyggda steg utförs under hello bearbetningen av en begäran, tillsammans med alla principer som ingår i omfattningen för hello-begäran. Om ett fel inträffar bearbetning omedelbart hoppar toohello `on-error` princip. Hej `on-error` principavdelningen kan användas på ett scope och API-utgivare kan konfigurera anpassade beteende, till exempel loggning hello fel tooevent nav eller skapa en ny svar tooreturn toohello anropare.  
  
> [!NOTE]
>  Hej `on-error` avsnittet finns inte i principer som standard. tooadd hello `on-error` tooa princip, bläddra toohello önskad princip i Redigeraren för hello och lägger till den. Mer information om hur du konfigurerar principer finns [principer i API Management](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/).  
>   
>  Om det finns inga `on-error` avsnittet anropare får 400 eller 500 http-svarsmeddelanden om ett fel inträffar.  
  
### <a name="policies-allowed-in-on-error"></a>Principer som tillåts i vid fel  
 hello följande principer kan användas i hello `on-error` princip.  
  
-   [Välj](api-management-advanced-policies.md#choose)  
  
-   [Ange variabel](api-management-advanced-policies.md#set-variable)  
  
-   [Sök och Ersätt](api-management-transformation-policies.md#Findandreplacestringinbody)  
  
-   [returnera svar](api-management-advanced-policies.md#ReturnResponse)  
  
-   [set-huvud](api-management-transformation-policies.md#SetHTTPheader)  
  
-   [set-metod](api-management-advanced-policies.md#SetRequestMethod)  
  
-   [Ange status](api-management-advanced-policies.md#SetStatus)  
  
-   [Skicka begäran](api-management-advanced-policies.md#SendRequest)  
  
-   [en-sätt-begäran om att skicka](api-management-advanced-policies.md#SendOneWayRequest)  
  
-   [loggen till eventhub](api-management-advanced-policies.md#log-to-eventhub)  
  
-   [JSON-xml](api-management-transformation-policies.md#ConvertJSONtoXML)  
  
-   [XML-json](api-management-transformation-policies.md#ConvertXMLtoJSON)  
  
## <a name="lasterror"></a>LastError  
 När ett fel inträffar och kontroll hoppar toohello `on-error` principer för hello fel lagras i [kontext. LastError](api-management-policy-expressions.md#ContextVariables) -egenskap som kan användas av principer i hello `on-error` har hello följande egenskaper och fält.  
  
|Namn|Typ|Beskrivning|Krävs|  
|----------|----------|-----------------|--------------|  
|Källa|Sträng|Namn hello element där hello-fel uppstod. Kan vara antingen princip eller en inbyggda pipeline Stegnamn.|Ja|  
|Orsak|Sträng|Datorn eget felkod som kan användas i felhantering.|Nej|  
|Meddelande|Sträng|Läsbart felbeskrivning.|Ja|  
|Omfång|Sträng|Namnet på hello scope där hello-fel uppstod och kan vara någon av ”globala”, ”product”, ”api” eller ”åtgärden”|Nej|  
|Avsnittet|Sträng|Avsnittsnamnet där fel uppstod och kan något av ”inkommande”, ”serverdel”, ”utgående” eller ”på fel”.|Nej|  
|Sökväg|Sträng|Anger kapslade princip, t.ex ”. Välj [3] / när [2]”.|Nej|  
|PolicyId|Sträng|Värdet för hello `id` attributet, om anges av hello kund på hello princip där felet uppstod|Nej|  
  
> [!NOTE]
>  Alla principer som har en valfri `id` attribut som kan läggas till toohello rotelement i hello princip. Om det här attributet visas i en princip när ett fel inträffar hello värdet för hello-attributet kan hämtas med hello `context.LastError.PolicyId` egenskapen.  
  
## <a name="predefined-errors-for-built-in-steps"></a>Fördefinierade fel för inbyggda steg  
 hello är följande fel fördefinierade för felförhållanden som kan uppstå under hello utvärdering av inbyggda bearbetning steg.  
  
|Källa|Villkor|Orsak|Meddelande|  
|------------|---------------|------------|-------------|  
|konfiguration|URI: N matchar inte tooany Api eller åtgärd|OperationNotFound|Det går inte toomatch inkommande begäran tooan åtgärden.|  
|Auktorisering|Prenumerationen nyckeln har inte angetts|SubscriptionKeyNotFound|Åtkomst nekades på grund av toomissing prenumeration nyckel. Gör att tooinclude prenumeration nyckeln när du gör begäranden toothis API.|  
|Auktorisering|Nyckelvärdet för prenumerationen är ogiltig|SubscriptionKeyInvalid|Åtkomst nekades på grund av tooinvalid prenumeration nyckel. Se till att tooprovide en giltig nyckel för en aktiv prenumeration.|  
  
## <a name="predefined-errors-for-policies"></a>Fördefinierade fel för principer  
 hello är följande fel fördefinierade för felförhållanden som kan uppstå under utvärderingen.  
  
|Källa|Villkor|Orsak|Meddelande|  
|------------|---------------|------------|-------------|  
|gräns för överföringshastigheten|Hastigheten med vilken överskriden|RateLimitExceeded|Hastighetsbegränsning har överskridits|  
|kvot|Kvoten överskreds|QuotaExceeded|Slut på kvot för samtalsvolym. Kvoten fylls i xx:xx:xx. - eller - Out-of-bandbredd kvoten. Kvoten fylls i xx:xx:xx.|  
|hanteras jsonp|Motringning parametervärdet är ogiltigt (innehåller felaktiga tecken)|CallbackParameterInvalid|Värdet för parametern återanrop {--återanrop} är inte en giltig JavaScript-identifierare.|  
|IP-filter|Misslyckade tooparse anroparen IP från begäran|FailedToParseCallerIP|Det gick inte tooestablish IP-adress för hello anropare. Åtkomst nekades.|  
|IP-filter|Anroparen IP finns inte i listan över tillåtna|CallerIpNotAllowed|Anroparen IP-adress {ip-adress} är inte tillåtet. Åtkomst nekades.|  
|IP-filter|Anroparen IP finns i blockeringslistan|CallerIpBlocked|Anroparen IP-adress blockeras. Åtkomst nekades.|  
|Kontrollera-huvud|Obligatorisk rubrik som visas inte eller värde saknas|HeaderNotFound|Huvudet {huvudnamn} hittades inte i hello-begäran. Åtkomst nekades.|  
|Kontrollera-huvud|Obligatorisk rubrik som visas inte eller värde saknas|HeaderValueNotAllowed|Huvudvärde {huvudnamn} för {huvudvärde} är inte tillåtet. Åtkomst nekades.|  
|Validera jwt|Jwt-token saknas i begäran|TokenNotFound|JWT hittades inte i hello-begäran. Åtkomst nekades.|  
|Validera jwt|Signaturverifieringen misslyckades|TokenSignatureInvalid|< meddelande från jwt biblioteket\>. Åtkomst nekades.|  
|Validera jwt|Ogiltig målgrupp|TokenAudienceNotAllowed|< meddelande från jwt biblioteket\>. Åtkomst nekades.|  
|Validera jwt|Ogiltig utfärdare|TokenIssuerNotAllowed|< meddelande från jwt biblioteket\>. Åtkomst nekades.|  
|Validera jwt|Token har upphört att gälla|TokenExpired|< meddelande från jwt biblioteket\>. Åtkomst nekades.|  
|Validera jwt|Signaturnyckel löstes inte med id|TokenSignatureKeyNotFound|< meddelande från jwt biblioteket\>. Åtkomst nekades.|  
|Validera jwt|Det saknas nödvändiga anspråk från token|TokenClaimNotFound|JWT-token saknas hello följande anspråk: < c1\>, < c2\>,... Åtkomst nekades.|  
|Validera jwt|Felaktig matchning av anspråk värden|TokenClaimValueNotAllowed|Anspråksvärdet {anspråkets namn} för {Anspråksvärdet} är inte tillåtet. Åtkomst nekades.|  
|Validera jwt|Andra verifieringsfel|JwtInvalid|< meddelande från jwt-biblioteket\>|

## <a name="next-steps"></a>Nästa steg
Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).  