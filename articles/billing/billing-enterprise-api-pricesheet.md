---
title: Azure Billing Enterprise API - Prisdokument | Microsoft Docs
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
ms.openlocfilehash: 2e7d6e883abe4cee13bc5f684baf2a1ea9c6c397
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a>Reporting API: er för företagskunder - Prisdokument

Price Sheet API ger den tillämpliga hastigheten för varje mätaren för den angivna registrering och fakturering Period.

##<a name="request"></a>Förfrågan
Allmänna rubrikegenskaper för som ska läggas till anges [här](billing-enterprise-api.md). Om en faktureringsperiod anges returnerade data för den aktuella faktureringsperioden.

|Metod | URI-begäran|
|-|-|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / prisdokument|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / prisdokument|

> [!Note]
> Ersätt v2 med v1 i URL: en ovan om du vill använda förhandsversionen av API.
>

## <a name="response"></a>Svar

    
        [
            {
                "id": "enrollments/57354989/billingperiods/201601/products/343/pricesheets",
                "billingPeriodId": "201704",
                "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
                "meterName": "A1 VM",
                "unitOfMeasure": "100 Hours",
                "includedQuantity": 0,
                "partNumber": "N7H-00015",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            {
                "id": "enrollments/57354989/billingperiods/201601/products/2884/pricesheets",
                "billingPeriodId": "201404",
                "meterId": "dc210ecb-97e8-4522-8134-5385494233c0",
                "meterName": "Locally Redundant Storage Premium Storage - Snapshots - AU East",
                "unitOfMeasure": "100 GB",
                "includedQuantity": 0,
                "partNumber": "N9H-00402",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            ...
        ]
    

> [!Note]
>MeterId fältet är inte tillgängligt om du använder API: et för förhandsgranskning.
>

**Svaret egenskapsdefinitioner**

|Egenskapsnamn| Typ| Beskrivning
|-|-|-|
|id| Sträng| Unikt Id som representerar ett visst Prisdokument objekt (mätaren av fakturering period)|
|billingPeriodId| Sträng| Unikt Id som representerar en viss period för fakturering|
|meterId| Sträng| Identifierare för mätaren. Det går att mappa till meterId för användning.|
|meterName| Sträng| Namnet på mätaren|
|unitOfMeasure| Sträng| Enhet för mätning av tjänsten|
|includedQuantity| Decimal| Antal som ingår |
|partNumber| Sträng| Artikelnumret som är associerad med mätaren|
|Enhetspris| Decimal| Enhetspriset för mätaren|
|currencyCode| Sträng| Koden för Enhetspris|
<br/>
## <a name="see-also"></a>Se även

* [Fakturering punkter API](billing-enterprise-api-billing-periods.md)

* [Information om användning av API](billing-enterprise-api-usage-detail.md)

* [Belastningsutjämning och sammanfattning API](billing-enterprise-api-balance-summary.md)

* [Marketplace Store kostnad API](billing-enterprise-api-marketplace-storecharge.md)
