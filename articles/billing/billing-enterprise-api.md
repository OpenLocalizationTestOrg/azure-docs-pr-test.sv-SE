---
title: 'aaaAzure fakturering Enterprise-API: er | Microsoft Docs'
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
ms.openlocfilehash: 017cecc57ad6bdeb402b5d9d57fc95df9b033a42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>Översikt över Reporting API: er för Enterprise-kunder
hello Reporting API: er kan Azure Enterprise-kunder tooprogrammatically pull användnings- och faktureringsinformation i Verktyg för analys av önskade data. 

## <a name="enabling-data-access-toohello-api"></a>Aktivera data access toohello API: et
* **Generera eller hämta hello API-nyckel** - inloggningen toohello Enterprise portal och följ hello kursen under Hjälp - Reporting API: er. hello förklaras första under den här hjälpartikeln hur toogenerate eller hämta hello API-nyckel för hello angivna registreringen.
* **Skicka nycklar i hello API** -hello API-nyckeln måste toobe som skickades för varje anrop för autentisering och auktorisering. hello måste följande egenskap toobe toohello HTTP-huvuden

|Begäran sidhuvud nyckel | Värde|
|-|-|
|Auktorisering| Ange hello värde i det här formatet: **ägar {API_KEY}** <br/> Exempel: ägar eyr... 09|

## <a name="consumption-apis"></a>API: er för förbrukning
Swagger-slutpunkten är tillgänglig [här](https://consumption.azure.com/swagger/ui/index) i hello API: er som beskrivs nedan som ska aktivera enkel introspection hello API och hello möjlighet toogenerate klientenheter SDK: er med hjälp av [AutoRest](https://github.com/Azure/AutoRest) eller [ Swagger CodeGen](http://swagger.io/swagger-codegen/). Data från 1 maj 2014 är tillgängliga via den här API: T. 

* **Belastningsutjämning och sammanfattning** - hello [saldo och översikt över API](billing-enterprise-api-balance-summary.md) ger information om saldon, nya inköp, avgifter för Azure Marketplace, justeringar och överförbrukning avgifter månatligen.

* **Användningsinformation** - hello [användning detalj API](billing-enterprise-api-usage-detail.md) erbjuder en daglig sammanställning av förbrukade antalen och beräknade kostnader genom en registrering. hello resultatet innehåller också information om instanser, mätare och avdelningar. hello API kan efterfrågas med fakturering punkt eller genom att start- och slutdatum. 

* **Marketplace Store kostnad** - hello [Marketplace Store kostnad API](billing-enterprise-api-marketplace-storecharge.md) returnerar hello användningsbaserad marketplace avgifter sammanställning per dag för hello angivna fakturering Period eller start- och slutdatum (en gång avgifter inte ingår) .

* **Prisdokument** - hello [Price Sheet API](billing-enterprise-api-pricesheet.md) innehåller hello tillämpliga hastighet för varje mätaren för hello registrering och fakturering Period. 

## <a name="helper-apis"></a>Helper-API: er
 **Lista över fakturering punkter** - hello [fakturering punkter API](billing-enterprise-api-billing-periods.md) returnerar en lista över fakturering punkter som har förbrukningsdata för hello angivna registrering i omvänd kronologisk ordning. Varje Period innehåller en egenskap som pekar toohello API väg för hello fyra datauppsättningar - BalanceSummary, UsageDetails, Marketplace-debiteringar och Prisdokument.


## <a name="api-response-codes"></a>API-svarskoder  
|Svarets statuskod|Meddelande|Beskrivning|
|-|-|-|
|200| OKEJ|Inga fel|
|401| Behörighet saknas| API-nyckel hittades inte, ogiltig upphört att gälla osv.|
|404| Inte tillgänglig| Rapporten slutpunkten hittades inte|
|400| Felaktig begäran| Ogiltiga parametrar – datumintervall, EA siffror osv.|
|500| Serverfel| Unexoected fel vid bearbetning av begäran| 









