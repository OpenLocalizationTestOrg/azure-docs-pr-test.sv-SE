---
title: aaaAzure fakturering Enterprise-API - fakturering punkter | Microsoft Docs
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
ms.openlocfilehash: d4e17f25b22729a7f213306fb019ee0dbeca87ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a>Reporting API: er för företagskunder - fakturering punkter

hello fakturering punkter API returnerar en lista över fakturering punkter som har förbrukningsdata för hello anges registrering i omvänd kronologisk ordning. Varje Period innehåller en egenskap som pekar toohello API väg för hello fyra datauppsättningar - BalanceSummary, UsageDetails, Marktplace kostnader och Prisdokument. Om hello-perioden inte har data är hello motsvarande egenskap null. 


##<a name="request"></a>Förfrågan 
Allmänna sidhuvudegenskaper för som behöver toobe läggs anges [här](billing-enterprise-api.md). 

|Metod | URI-begäran|
|-|-|
|HÄMTA| https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / billingperiods|

> [!Note]
> toouse hello förhandsversionen av API, Ersätt v2 med v1 hello ovan URL.
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
|billingPeriodId| Sträng| hello unikt Id som representerar en viss period för fakturering|
|billingStart| Datum och tid| ISO 8601-sträng som representerar hello startdatum|
|billingEnd| Datum och tid| ISO 8601-sträng som representerar hello slutdatum|
|balanceSummary| Sträng| hello URL-sökväg som dirigerar toohello saldo översiktsdata för denna period|
|usageDetails| Sträng| hello URL-sökväg som dirigerar toohello användningsinformation data för denna period|
|marketplaceCharges| Sträng| hello URL-sökväg som dirigerar toohello Marketplace-debiteringar data för denna period|
|Prisdokument| Sträng| hello URL-sökväg som dirigerar toohello Prisdokument data för denna period|

<br/>
## <a name="see-also"></a>Se även

* [Belastningsutjämning och sammanfattning API](billing-enterprise-api-balance-summary.md)

* [Information om användning av API](billing-enterprise-api-usage-detail.md) 

* [Marketplace Store kostnad API](billing-enterprise-api-marketplace-storecharge.md) 

* [Price Sheet API](billing-enterprise-api-pricesheet.md)