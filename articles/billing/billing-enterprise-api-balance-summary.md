---
title: "Azure Billing Enterprise API - saldot och översikt | Microsoft Docs"
description: "Läs mer om Azure Billing-användning och RateCard APIs som används för att ge insikter om Azure resursförbrukning och trender."
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
ms.openlocfilehash: f6b149f0e656d2263705048aa5b644f5bb4a5712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a>Reporting API: er för företagskunder - saldot och sammanfattning

Ger information om saldon, nya inköp, Azure Marketplace serviceavgifter, justeringar och överförbrukning avgifter månatligen saldo och översikt över API.


##<a name="request"></a>Förfrågan 
Allmänna rubrikegenskaper för som ska läggas till anges [här](billing-enterprise-api.md). Om en faktureringsperiod anges returnerade data för den aktuella faktureringsperioden.

|Metod | URI-begäran|
|-|-|
|HÄMTA| https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / balancesummary|
|HÄMTA| https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / balancesummary|

> [!Note]
> Ersätt v2 med v1 i URL: en ovan om du vill använda förhandsversionen av API.
>

## <a name="response"></a>Svar

        {
            "id": "enrollments/100/billingperiods/201507/balancesummaries",
            "billingPeriodId": 201507,
            "currencyCode": "USD",
            "beginningBalance": 0,
            "endingBalance": 1.1,
            "newPurchases": 1,
            "adjustments": 1.1,
            "utilized": 1.1,
            "serviceOverage": 1,
            "chargesBilledSeparately": 1,
            "totalOverage": 1,
            "totalUsage": 1.1,
            "azureMarketplaceServiceCharges": 1,
            "newPurchasesDetails": [
                {
                "name": "",
                "value": 1
                }
            ],
            "adjustmentDetails": [
                {
                "name": "Promo Credit",
                "value": 1.1
                },
                {
                "name": "SIE Credit",
                "value": 1.0
                }
            ]
        }


**Svaret egenskapsdefinitioner**

|Egenskapsnamn| Typ| Beskrivning
|-|-|-|
|id|Sträng|Unikt Id för en specifik faktureringsperiod och registrering|
|billingPeriodId|Sträng |Faktureringsperiod Id|
|currencyCode|Sträng |Koden|
|beginningBalance|Decimal| Det ingående saldot för faktureringsperioden|
|endingBalance|Decimal| Den utgående balansen för faktureringsperioden (för öppna perioder detta uppdateras dagligen)|
|newPurchases|Decimal| Nya inköp Totalmängd|
|justering av|Decimal| Total justeringsbelopp|
|utnyttjade|Decimal| Total användning för åtagande|
|serviceOverage|Decimal| Överförbrukning för Azure-tjänster|
|chargesBilledSeparately|Decimal| Avgifter debiteras separat|
|totalOverage|Decimal| serviceOverage + chargesBilledSeparately|
|totalUsage|Decimal| Azure-tjänsten åtagande + total överförbrukning|
|azureMarketplaceServiceCharges|Decimal| Totalt antal kostnader för Azure Marketplace|
|newPurchasesDetails|JSON-Strängmatrisen av namn-värde-par|Lista över nya inköp|
|adjustmentDetails|JSON-Strängmatrisen av namn-värde-par|Lista över justeringar (kampanj kredit, SIE kredit osv.) |


<br/>
## <a name="see-also"></a>Se även

* [Fakturering punkter API](billing-enterprise-api-billing-periods.md)

* [Information om användning av API](billing-enterprise-api-usage-detail.md) 

* [Marketplace Store kostnad API](billing-enterprise-api-marketplace-storecharge.md) 

* [Price Sheet API](billing-enterprise-api-pricesheet.md)