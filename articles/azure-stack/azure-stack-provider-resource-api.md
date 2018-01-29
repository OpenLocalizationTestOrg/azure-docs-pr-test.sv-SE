---
title: "Providern Resursanvändning API | Microsoft Docs"
description: "Referens för Resursanvändning API, som hämtar information om Azure-stacken användning"
services: azure-stack
documentationcenter: 
author: AlfredoPizzirani
manager: byronr
editor: 
ms.assetid: b6055923-b6a6-45f0-8979-225b713150ae
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: alfredop
ms.openlocfilehash: 0c45ce3bc93945ed8700464beebabcda07e8d77c
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2017
---
# <a name="provider-resource-usage-api"></a>API för providerresursanvändning
Termen *provider* gäller tjänstadministratören och inga delegerade providers. Azure Stack-operatorer och delegerad providers kan använda providern användnings-API för att visa användningen av sina direkta klienter. Till exempel som visas i diagrammet P0 kan anropa API för att hämta information om användning på P1's providern och P2's direkt användning och P1 kan anropa användningsinformation för P3 och P4.

![Konceptuell modell för hierarkin provider](media/azure-stack-provider-resource-api/image1.png)

## <a name="api-call-reference"></a>Referens för API-anrop
### <a name="request"></a>Förfrågan
Begäran hämtar information om förbrukningen för de begärda prenumerationerna och för den begärda tidsperioden. Det finns ingen brödtext i begäran.

Denna användning API är en leverantör API, så anroparen måste tilldelas en ägare, deltagare eller läsare roll i leverantörens prenumeration.

| **Metoden** | **URI-begäran** |
| --- | --- |
| HÄMTA |https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}& subscriberId = {sub1.1} & api-version 2015-06-01-preview = & continuationToken = {token värde} |

### <a name="arguments"></a>Argument
| **Argumentet** | **Beskrivning** |
| --- | --- |
| *armendpoint* |Azure Resource Manager-slutpunkten för din Azure Stack-miljö. Azure-stacken konventionen är att namnet på Azure Resource Manager-slutpunkten är i formatet `https://adminmanagement.{domain-name}`. Till exempel för development kit om domännamnet är *local.azurestack.external*, och sedan Resource Manager-slutpunkten är `https://adminmanagement.local.azurestack.external`. |
| *subId* |Prenumerations-ID för den användare som gör anropet. |
| *reportedStartTime* |Starttid för frågan. Värdet för *DateTime* ska vara i Coordinated Universal Time (UTC) och i början av timme, till exempel 13:00. Ange värdet till midnatt UTC-tid för daglig sammanställning. Formatet är *undantagstecken* ISO 8601. Till exempel *2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z*, där kolon hoppas till *% 3a* och hoppas på plustecknet till *% 2b* så att det är användarvänligt URI. |
| *reportedEndTime* |Sluttid för frågan. Begränsningar som gäller för *reportedStartTime* gäller även för det här argumentet. Värdet för *reportedEndTime* får inte vara i framtiden eller det aktuella datumet. Om det är sätts resultatet till ”behandlingen har inte slutförts”. |
| *aggregationGranularity* |Valfri parameter som har två separata möjliga värden: varje dag och varje timme. Eftersom värdena föreslår en returnerar data i timme och det andra är en upplösning på varje timme. Dagliga alternativet är standardinställningen. |
| *subscriberId* |Prenumerations-ID. Prenumerations-ID för en direkt innehavare av providern krävs för att hämta filtrerade data. Om ingen parameter för prenumerations-ID har angetts returnerar anropet användningsdata för alla leverantörens direkt klienter. |
| *API-version* |Version av det protokoll som används för att göra denna begäran. Det här värdet anges *2015-06-01-preview*. |
| *continuationToken* |Token hämtas från det senaste anropet till användnings-API-providern. Denna token krävs när ett svar är större än 1 000 rader och den fungerar som ett bokmärke för förloppet. Om token inte finns, data hämtas från början på dagen eller timme, baserat på Granulariteten skickades. |

### <a name="response"></a>Svar
Hämta /subscriptions/sub1/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = varje dag & subscriberId = sub1.1 & api-version = 1.0

```json
{
"value": [
{

"id":
"/subscriptions/sub1.1/providers/Microsoft.Commerce/UsageAggregate/sub1.1-

meterID1",
"name": "sub1.1-meterID1",
"type": "Microsoft.Commerce/UsageAggregate",

"properties": {
"subscriptionId":"sub1.1",
"usageStartTime": "2015-03-03T00:00:00+00:00",
"usageEndTime": "2015-03-04T00:00:00+00:00",
"instanceData":"{\"Microsoft.Resources\":{\"resourceUri\":\"resourceUri1\",\"location\":\"Alaska\",\"tags\":null,\"additionalInfo\":null}}",
"quantity":2.4000000000,
"meterId":"meterID1"

}
},

…
```

### <a name="response-details"></a>Svarsinformation
| **Argumentet** | **Beskrivning** |
| --- | --- |
| *ID* |Unikt ID för mängdfunktionen användning. |
| *Namn* |Namnet på mängdfunktionen användning. |
| *typ* |Resursdefinitionen. |
| *prenumerations-ID* |Prenumerations-ID för Azure Stack-användare. |
| *usageStartTime* |UTC starttid för en användning bucket som tillhör den här samlingen för användning.|
| *usageEndTime* |UTC-sluttid för usage-bucket som tillhör den här samlingen för användning. |
| *instanceData* |Nyckel-värdepar för detaljerad information om instansen (i ett nytt format):<br> *resourceUri*: fullständigt resurs-ID, vilket innefattar resursgrupper och instansnamnet. <br> *plats*: Region där den här tjänsten körs. <br> *taggar*: resurstaggar som anges av användaren. <br> *additionalInfo*: Mer information om den resurs som förbrukades, till exempel OS-version eller image typen. |
| *Antal* |Mängden resursförbrukning som uppstått i den här tidsintervall. |
| *meterId* |Unikt ID för den resurs som förbrukades (kallas även *ResourceID*). |

## <a name="next-steps"></a>Nästa steg
[Klient Resursanvändning API-referens](azure-stack-tenant-resource-usage-api.md)

[Användning-relaterade vanliga frågor och svar](azure-stack-usage-related-faq.md)
