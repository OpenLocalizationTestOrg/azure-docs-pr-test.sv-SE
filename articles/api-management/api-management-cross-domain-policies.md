---
title: "aaaAzure API Management korsdomänprinciper | Microsoft Docs"
description: "Läs mer om hello korsdomänprinciper tillgängligt för användning i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7689d277-8abe-472a-a78c-e6d4bd43455d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: dd5ebfd65b92ebd0c1f589a2bac669a3928d40b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-cross-domain-policies"></a>Korsdomänprinciper för API Management
Det här avsnittet innehåller en referens för hello följande API Management-principer. Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CrossDomainPolicies"></a>Mellan domänprinciper  
  
-   [Tillåt mellan domäner kan anropa](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -gör hello API åtkomlig från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.  
  
-   [CORS](api-management-cross-domain-policies.md#CORS) -lägger till resursdelning för korsande ursprung (CORS) stöder tooan åtgärden eller en API tooallow domänerna anrop från webbläsarbaserade klienter.  
  
-   [Hanteras JSONP](api-management-cross-domain-policies.md#JSONP) – lägger till JSON med utfyllnad (hanteras JSONP) stöd tooan åtgärden eller en API tooallow domänerna anrop från JavaScript webbläsarbaserade klienter.  
  
##  <a name="AllowCrossDomainCalls"></a>Tillåta anrop mellan domäner  
 Använd hello `cross-domain` princip toomake hello API som är tillgängliga från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in hello Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a>Exempel  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|mellan domäner|Rotelementet. Underordnade element måste uppfylla toohello [Adobe domänerna filen principspecifikationen](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).|Ja|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** globala  
  
##  <a name="CORS"></a>CORS  
 Hej `cors` princip lägger till resursdelning för korsande ursprung (CORS) stöder tooan åtgärden eller en API tooallow domänerna anrop från webbläsarbaserade klienter.  
  
 CORS kan en webbläsare och en server toointeract och avgöra huruvida tooallow specifika cross-origin-begäranden (d.v.s. XMLHttpRequests anrop från JavaScript på en webbsida tooother domäner). Detta ger bättre flexibilitet än att bara tillåta samma origin-begäranden, men det är säkrare än att tillåta alla cross-origin-begäranden.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<cors allow-credentials="false|true">  
    <allowed-origins>  
        <origin>origin uri</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="number of seconds">  
        <method>http verb</method>  
    </allowed-methods>  
    <allowed-headers>  
        <header>header name</header>  
    </allowed-headers>  
    <expose-headers>  
        <header>header name</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="example"></a>Exempel  
 Det här exemplet visar hur toosupport före svarta begär, till exempel de som har anpassade huvuden eller andra metoder än GET och POST. toosupport anpassade sidhuvuden och ytterligare HTTP-verb, Använd hello `allowed-methods` och `allowed-headers` avsnitten som visas i följande exempel hello.  
  
```xml  
<cors allow-credentials="true">  
    <allowed-origins>  
        <!-- Localhost useful for development -->  
        <origin>http://localhost:8080/</origin>  
        <origin>http://example.com/</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="300">  
        <method>GET</method>  
        <method>POST</method>  
        <method>PATCH</method>  
        <method>DELETE</method>  
    </allowed-methods>  
    <allowed-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
        <header>x-zumo-version</header>  
        <header>x-zumo-auth</header>  
        <header>content-type</header>  
        <header>accept</header>  
    </allowed-headers>  
    <expose-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|cors|Rotelementet.|Ja|Saknas|  
|tillåtna ursprung|Innehåller `origin` element som beskriver hello tillåtna ursprung för domäner begäranden. `allowed-origins`kan innehålla antingen en enskild `origin` element som anger `*` tooallow alla ursprung, eller en eller flera `origin` element som innehåller en URI.|Ja|Saknas|  
|ursprung|hello värdet kan vara antingen `*` tooallow alla ursprung eller en URI som anger ett enda ursprung. hello URI måste innehålla ett schema, värd och port.|Ja|Om hello port utelämnas i en URI används port 80 för HTTP och port 443 används för HTTPS.|  
|tillåtna metoder|Det här elementet krävs om metoder än hämta eller POST tillåts. Innehåller `method` element som anger hello stöds HTTP-verb.|Nej|Om det här avsnittet inte finns, stöds GET och POST.|  
|Metoden|Anger ett HTTP-verb.|Minst en `method` -element krävs om hello `allowed-methods` avsnittet finns.|Saknas|  
|tillåtna rubriker|Det här elementet innehåller `header` element att ange namnen på hello-huvuden som kan ingå i hello-begäran.|Nej|Saknas|  
|exponera rubriker|Det här elementet innehåller `header` element att ange namnen på hello-huvuden som ska vara tillgängligt av hello-klienten.|Nej|Saknas|  
|sidhuvud|Anger ett namn för sidhuvudet.|Minst en `header` -element krävs i `allowed-headers` eller `expose-headers` om hello avsnittet finns.|Saknas|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|Tillåt autentiseringsuppgifter|Hej `Access-Control-Allow-Credentials` huvud hello preflight-svar kommer att ange toohello värdet för det här attributet och påverkar hello klientens möjlighet toosubmit autentiseringsuppgifter i domänerna begäranden.|Nej|FALSKT|  
|preflight-resultat-max-ålder|Hej `Access-Control-Max-Age` huvud hello preflight-svar kommer att ange toohello värdet för det här attributet och påverkar hello användaragenten möjlighet toocache före svarta svar.|Nej|0|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** API, åtgärden  
  
##  <a name="JSONP"></a>HANTERAS JSONP  
 Hej `jsonp` principen lägger till JSON med utfyllnad (hanteras JSONP) stöd tooan åtgärden eller en API tooallow mellan domäner-anrop från JavaScript-webbläsarbaserade klienter. Hanteras JSONP är en metod som används i JavaScript program toorequest data från en server i en annan domän. Hanteras JSONP kringgår hello begränsning som tillämpas av de flesta webbläsare där tooweb sidor måste vara i hello samma domän.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a>Exempel  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 Om du anropar hello metoden utan hello återanrop parametern? cb = XXX returneras oformaterade JSON (utan en funktionen anropet wrapper).  
  
 Om du lägger till hello återanrop parametern `?cb=XXX` hanteras JSONP resulterar i att wrapping hello ursprungliga JSON-resultaten runt hello Återanropsfunktionen som returneras`XYZ('<json result goes here>');`  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|hanteras jsonp|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|motringning parameternamn|hello domänerna JavaScript-anrop med prefixet hello fullständigt kvalificerade domännamnet där hello funktionen finns.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** utgående  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
## <a name="next-steps"></a>Nästa steg
Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).  