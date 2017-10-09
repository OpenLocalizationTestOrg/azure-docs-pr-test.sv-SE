---
title: aaaOverview av Azure tid serien Insights | Microsoft Docs
description: "Introduktion tooAzure tid serien Insight, en ny tjänst för tid serien dataanalys och IoT-lösningar"
keywords: 
services: tsi
documentationcenter: 
author: op-ravi
manager: jhubbard
editor: 
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: omravi
ms.openlocfilehash: 8c022bf1fae88eddab3dbebc7bb36cc15a785172
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-time-series-insights"></a>Vad är Azure Time Series Insights?

Azure tid serien Insights är en hanterad molntjänst med lagring, analyser och visualisering komponenter som gör det enkelt tooingest, lagra, utforska och analysera händelser miljarder samtidigt. Med Time Series Insights får du en global vy över dina data som hjälper dig att snabbt validera dina IoT-lösningar och undvika kostsamma avbrott. Du får hjälp med att identifiera dolda trender, upptäcka avvikelser och utföra rotorsaksanalyser nästan i realtid. Tid serien insikter en time series-data från händelse-mäklare (t.ex. IoT-hubbar eller Händelsehubbar), indexerar hello data och upphör data baserat på en konfigurerbar bevarandeprincip. Användare använder hello data via en intuitiv UX eller REST API: er för frågan.

![Översikt över Time Series Insight](media/overview/time-series-insights-overview-flow.png)

## <a name="primary-scenarios"></a>Primära scenarier

* Övervaka och validera IoT-lösningar på några minuter.
* Visualisera och analysera IoT-data i större skala.
* Påskynda rotorsaksanalys och avvikelseidentifiering.
* Skapa en global översikt över flera enheter, anläggningar och data.

## <a name="capabilities-and-benefits"></a>Funktioner och fördelar

* **Enkelt tooget igång**: Azure tid serien insikter kräver inga direkta dataförberedelse och är otroligt snabbt. Ansluta toobillions händelser i dina Azure IoT-hubb eller Händelsehubb i minuter. När du är ansluten, visualisera och interagera med sensordata i sekunder tooquickly verifiera din IoT-lösningar. Tid serien insikter är enkelt toouse; Du kan interagera med dina data utan att skriva en enda rad kod.  Det finns inga nya språk toolearn; Tid serien insikter ger en detaljerad, fritext frågan yta för avancerade användare pekar och klickar på undersökning för alla.

* **Nära realtidsinsikter**: tid serien insikter kan mata in flera hundra miljoner sensor händelser per dag, med en minuts fördröjning så att du snabbt kan reagera toochanges. Med Time Series Insights får du djupare insikter om dina sensordata. Det beror på att du får hjälp med att upptäcka trender och avvikelser snabbt, vilket gör det möjligt att utföra komplicerade rotorsaksanalyser och undvika kostsamma avbrott. Genom att aktivera cross-korrelation mellan realtidsstatus och historisk data, hjälper tid serien Insights dig att låsa upp dolda trender i hello data.

* **Bygga anpassade lösningar**: Bädda in Azure Time Series Insights-data till dina befintliga program eller skapa nya anpassade lösningar med REST-API:er för Time Series Insights. Skapa och dela anpassade vyer som du kan dela för andra tooexplore din identifieringar.

* **Skalbarhet**: tid serien insikter är utformad toosupport IoT i större skala. I förhandsversionen kan den ingång från 1 miljon too100 miljoner händelser per dag, med en Standardkvarhållning span 31 dagar. Du kan visualisera och analysera livedataströmmar nästan i realtid tillsammans med stora mängder historiska data. Går vidare ökar ingång och kvarhållning tooaccommodate en någonsin växande företagsnivå.

## <a name="time-series-insights-glossary"></a>Ordlista för Time Series Insights

* **Miljö**: En miljö är en Azure-resurs med ingångs- och lagringskapacitet.  Kunder etablera miljöer genom hello Azure-portalen med kapaciteten som krävs.
* **Händelsekälla**: En händelsekälla härleds från en asynkron meddelandekö för händelser, som Azure Event Hubs.  Tid serien Insights ansluter direkt tooEvent källor, vill föra in hello dataströmmen utan att skriva någon kod. Tidserieinsikter stöder för närvarande, Azure Event Hubs och Azure IoT Hubs.
* **Referensdata**: tid serien insikter ger användarna hello möjlighet toojoin tid series-data med referensdata.  Referensdata kan innehålla metadata om enheter eller andra statiska data som ändras relativt sällan. Tid serien Insights ansluter hello referensdata med dataströmmar, vilket gör att användare toovisualize och analysera den här data nästan i realtid.
