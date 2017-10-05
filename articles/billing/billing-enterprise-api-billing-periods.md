---
title: "Azure API för fakturering Enterprise - fakturering punkter | Microsoft Docs"
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
ms.openlocfilehash: c6880b79189e0683387a7aafbd6fa4805b3b42ef
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a>Reporting API: er för företagskunder - fakturering punkter

API: et för fakturering perioder returnerar en lista över fakturering punkter som har förbrukningsdata för den angivna registreringen i omvänd kronologisk ordning. Varje Period innehåller en egenskap som pekar på API-väg för de fyra datauppsättningarna - BalanceSummary, UsageDetails, Marktplace kostnader och Prisdokument. Om perioden inte har data är motsvarande egenskap null. 


##<a name="request"></a>Förfrågan 
Allmänna rubrikegenskaper för som ska läggas till anges [här](billing-enterprise-api.md). 

|Metod | URI-begäran|
|-|-|
|HÄMTA| https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / billingperiods|

> [!Note]
> Ersätt v2 med v1 i URL: en ovan om du vill använda förhandsversionen av API.
>

## <a name="response"></a>Svar
 
    
    
      [
            {
                "billingPeriodId": "201704",
                "billingStart": "2017-04-01T00:00:00Z",
                "billingEnd": "2017-04-30T11:59:59Z",
                "balanceSummary": "/v1/enrollments/100/billingperiods/201704/balancesummary",
                "usageDetails": "/v1/enrollments/100/billingperiods/201704/usagedetails",
                "marketplaceCharges": "/v1/enrollments/100/billingperiods/201704/marketplacecharges",
                "priceSheet": "/v1/enrollments/100/billingperiods/201704/pricesheet"
            },          
            ....
      ]
    

**Svaret egenskapsdefinitioner**

|Egenskapsnamn| Typ| Beskrivning
|-|-|-|
|billingPeriodId| Sträng| Unikt Id som representerar en viss period för fakturering|
|billingStart| Datum och tid| ISO 8601-sträng som representerar perioden startdatum|
|billingEnd| Datum och tid| ISO 8601-sträng som representerar slutdatumet|
|balanceSummary| Sträng| URL-sökvägen som omdirigerar till saldo sammanfattning-data för den här perioden|
|usageDetails| Sträng| URL-sökvägen som omdirigerar till användningsinformation data för denna period|
|marketplaceCharges| Sträng| URL-sökvägen som omdirigerar till Marketplace-debiteringar data för denna period|
|Prisdokument| Sträng| URL-sökvägen som omdirigerar till Prisdokument data för denna period|

<br/>
## <a name="see-also"></a>Se även

* [Belastningsutjämning och sammanfattning API](billing-enterprise-api-balance-summary.md)

* [Information om användning av API](billing-enterprise-api-usage-detail.md) 

* [Marketplace Store kostnad API](billing-enterprise-api-marketplace-storecharge.md) 

* [Price Sheet API](billing-enterprise-api-pricesheet.md)