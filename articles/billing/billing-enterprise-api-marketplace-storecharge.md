---
title: Azure Billing Enterprise API - Marketplace-debiteringar | Microsoft Docs
description: "Läs mer om Reporting API: erna som gör att företag Azure-kunder att dra förbrukningsdata programmässigt."
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
ms.openlocfilehash: 5539623f7ae35e14b6dafe6fdf9efe4bcaba4fd3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a>Reporting API: er för företagskunder - Marketplace Store kostnad

Marketplace Store kostnad API returnerar användningsbaserad marketplace avgifter sammanställning per dag för fakturering tidsperioden eller start- och slutdatum (en gång avgifter inte ingår).

##<a name="request"></a>Förfrågan 
Allmänna rubrikegenskaper för som ska läggas till anges [här](billing-enterprise-api.md). Om en faktureringsperiod anges returnerade data för den aktuella faktureringsperioden. Anpassade tidsintervall kan anges med början och slutar datumparametrar som är i formatet ÅÅÅÅ-MM-dd, högsta tidsintervallet som stöds är 36 månader.  

|Metod | URI-begäran|
|-|-|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacecharges|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / marketplacecharges|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 10-01-2017|

> [!Note]
> Ersätt v2 med v1 i URL: en ovan om du vill använda förhandsversionen av API.
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
|id|Sträng|Unikt Id för kostnad marketplace-objektet|
|subscriptionGuid|GUID|Guid för prenumeration|
|SubscriptionName|Sträng|Prenumerationsnamnet|
|meterId|Sträng|ID för den angivna mätaren|
|usageStartDate|Datum och tid|Starttid för posten i användning|
|usageEndDate|Datum och tid|Sluttid för posten användning|
|offerName|Sträng|Namnet på erbjudande|
|resourceGroup|Sträng|Resursen grupp|
|InstanceId|Sträng|Instans-Id|
|additionalInfo|Sträng|Ytterligare information JSON-strängen|
|tags|Sträng|Taggen JSON-strängen|
|orderNumber|Sträng|Ordningstalet|
|unitOfMeasure|Sträng|Enheten för mätaren|
|CostCenter|Sträng|Kostnadsställe|
|accountId|int|Konto-Id|
|Kontonamn|Sträng |Namnet på kontot|
|accountOwnerId|Sträng|Ägar-Id för kontot|
|departmentId|int|Avdelning Id|
|DepartmentName|Sträng|Ett avdelningsnamn|
|publisherName|Sträng|Utgivarens namn|
|planName|Sträng|Namn för energischema|
|consumedQuantity|Decimal|Förbrukade antal under den här tiden|
|resourceRate|Decimal|Enhetspriset för mätaren|
|extendedCost|Decimal|Beräknad kostnad som baseras på Förbrukat antal och utökad kostnad|
<br/>
## <a name="see-also"></a>Se även

* [Fakturering punkter API](billing-enterprise-api-billing-periods.md)

* [Information om användning av API](billing-enterprise-api-usage-detail.md) 

* [Belastningsutjämning och sammanfattning API](billing-enterprise-api-balance-summary.md)

* [Price Sheet API](billing-enterprise-api-pricesheet.md)