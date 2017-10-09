---
title: aaaAzure fakturering Enterprise API - Prisdokument | Microsoft Docs
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
ms.openlocfilehash: 4cfe694c63fba266d117054b046d9caacb3b7197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a>Reporting API: er för företagskunder - Prisdokument

hello Price Sheet API ger hello tillämpliga hastighet för varje mätaren för hello registrering och fakturering Period.

##<a name="request"></a>Förfrågan
Allmänna sidhuvudegenskaper för som behöver toobe läggs anges [här](billing-enterprise-api.md). Om en faktureringsperiod anges sedan returneras data för hello aktuella fakturering tidsperiod.

|Metod | URI-begäran|
|-|-|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / prisdokument|
|HÄMTA|https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / prisdokument|

> [!Note]
> toouse hello förhandsversionen av API, Ersätt v2 med v1 hello ovan URL.
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
>Om du använder hello Preview API är meterId fältet inte tillgängligt.
>

**Svaret egenskapsdefinitioner**

|Egenskapsnamn| Typ| Beskrivning
|-|-|-|
|id| Sträng| hello unikt Id som representerar ett visst Prisdokument objekt (mätaren av fakturering period)|
|billingPeriodId| Sträng| hello unikt Id som representerar en viss period för fakturering|
|meterId| Sträng| hello identifierare för hello mätaren. Det kan vara mappade toohello användning meterId.|
|meterName| Sträng| hello mätaren namn|
|unitOfMeasure| Sträng| hello enhet för att mäta hello-tjänsten|
|includedQuantity| Decimal| Antal som ingår |
|partNumber| Sträng| hello artikelnumret som är associerade med hello mätare|
|Enhetspris| Decimal| hello enhetspriset för hello mätare|
|currencyCode| Sträng| hello valutakoden för hello Enhetspris|
<br/>
## <a name="see-also"></a>Se även

* [Fakturering punkter API](billing-enterprise-api-billing-periods.md)

* [Information om användning av API](billing-enterprise-api-usage-detail.md)

* [Belastningsutjämning och sammanfattning API](billing-enterprise-api-balance-summary.md)

* [Marketplace Store kostnad API](billing-enterprise-api-marketplace-storecharge.md)
