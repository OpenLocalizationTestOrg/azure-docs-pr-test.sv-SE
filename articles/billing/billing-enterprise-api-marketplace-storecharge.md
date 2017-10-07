---
title: aaaAzure fakturering Enterprise API - Marketplace-debiteringar | Microsoft Docs
description: "Läs mer om hello Reporting-API: er som gör att Azure Enterprise-kunder toopull förbrukningsdata programmässigt."
services: 
documentationcenter: 
author: aedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: cdf2836b52df06a4bf5ed71a476fe33662c5363c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a>Reporting API: er för företagskunder - Marketplace Store kostnad

hello Marketplace Store kostnad API returnerar hello användningsbaserad marketplace avgifter sammanställning per dag för hello angivna fakturering Period eller start- och slutdatum (en gång avgifter inte ingår).

##<a name="request"></a>Förfrågan 
Allmänna sidhuvudegenskaper för som behöver toobe läggs anges [här](billing-enterprise-api.md). Om en faktureringsperiod anges sedan returneras data för hello aktuella fakturering tidsperiod. Anpassade tidsintervall kan anges med hello start och sluta datum parametrarna i hello formatet ÅÅÅÅ-MM-dd hello maximala stöds tid intervallet är 36 månader.  

|Metod | URI-begäran|
|-|-|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacecharges|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / marketplacecharges|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 10-01-2017|

> [!Note]
> toouse hello förhandsversionen av API, Ersätt v2 med v1 hello ovan URL.
>

## <a name="response"></a>Svar
 
    
        [
            {
                "id": "id",
                "subscriptionGuid": "00000000-0000-0000-0000-000000000000",
                "subscriptionName": "subName",
                "meterId": "2core",
                "usageStartDate": "2015-09-17T00:00:00Z",
                "usageEndDate": "2015-09-17T23:59:59Z",
                "offerName": "Virtual LoadMaster™ (VLM) for Azure",
                "resourceGroup": "Res group",
                "instanceId": "id",
                "additionalInfo": "{\"ImageType\":null,\"ServiceType\":\"Medium\"}",
                "tags": "",
                "orderNumber": "order",
                "unitOfMeasure": "",
                "costCenter": "100",
                "accountId": 100,
                "accountName": "Account Name",
                "accountOwnerId": "account@live.com",
                "departmentId": 101,
                "departmentName": "Department 1",
                "publisherName": "Publisher 1",
                "planName": "Plan name",
                "consumedQuantity": 1.15,
                "resourceRate": 0.1,
                "extendedCost": 1.11
            },
            ...
        ]
    

**Svaret egenskapsdefinitioner**

|Egenskapsnamn| Typ| Beskrivning
|-|-|-|
|id|Sträng|Unikt Id för kostnad hello marketplace-objektet|
|subscriptionGuid|GUID|hello prenumeration Guid|
|SubscriptionName|Sträng|hello prenumerationsnamn|
|meterId|Sträng|ID för hello orsakat mätare|
|usageStartDate|Datum och tid|Starttid för hello användning post|
|usageEndDate|Datum och tid|Sluttid för hello användning post|
|offerName|Sträng|Hej erbjudandenamn|
|resourceGroup|Sträng|hello resurs grupp|
|InstanceId|Sträng|Instans-Id|
|additionalInfo|Sträng|Ytterligare information JSON-strängen|
|tags|Sträng|Taggen JSON-strängen|
|orderNumber|Sträng|hello ordningsnummer|
|unitOfMeasure|Sträng|Enheten för hello mätare|
|CostCenter|Sträng|hello kostnad center|
|accountId|int|hello konto-Id|
|Kontonamn|Sträng |hello kontonamn|
|accountOwnerId|Sträng|hello konto ägar-Id|
|departmentId|int|hello avdelning Id|
|DepartmentName|Sträng|hello avdelningsnamn|
|publisherName|Sträng|hello utgivarens namn|
|planName|Sträng|hello namn|
|consumedQuantity|Decimal|Förbrukade antal under den här tiden|
|resourceRate|Decimal|Enhetspriset för hello mätare|
|extendedCost|Decimal|Beräknad kostnad som baseras på Förbrukat antal och utökad kostnad|
<br/>
## <a name="see-also"></a>Se även

* [Fakturering punkter API](billing-enterprise-api-billing-periods.md)

* [Information om användning av API](billing-enterprise-api-usage-detail.md) 

* [Belastningsutjämning och sammanfattning API](billing-enterprise-api-balance-summary.md)

* [Price Sheet API](billing-enterprise-api-pricesheet.md)