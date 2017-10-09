---
title: "aaaReal tid för händelsebearbetning med Stream Analytics händelsebearbetning | Microsoft Docs"
description: "Lär dig hur en uppsättning Azure-tjänster kan samverka för att aktivera realtidsskydd händelsebearbetning och analyser."
keywords: "realtidsbearbetning, händelsebearbetning, Referensarkitektur"
services: stream-analytics,event-hubs,storage,sql-database
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: 
ms.assetid: 11af48bc-313c-4527-8c80-91088dc9f3c6
ms.service: stream-analytics
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: jeffstok
ms.openlocfilehash: a43c503d709609ba61e9932822d30bc2208906ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Referera arkitektur: realtid händelsebearbetning med Microsoft Azure Stream Analytics
hello Referensarkitektur för realtid händelsebearbetning med Azure Stream Analytics är avsedda tooprovide en allmän plan för att distribuera en realtid plattform som en tjänst (PaaS) stream-bearbetning med Microsoft Azure.

## <a name="summary"></a>Sammanfattning
Traditionellt baseras Analyslösningar på funktioner som ETL (extract, transform, load) och datalagring, där data är lagrade tidigare tooanalysis. Ändra krav, inklusive mer snabbt inkommande data trycker den befintliga modell toohello gränsen. När den inte är en ny funktion hello-metoden har inte tagits brett över alla branschen lodräta annonser hello möjlighet tooanalyze data i glidande dataströmmar tidigare toostorage är en lösning. 

Microsoft Azure tillhandahåller en omfattande katalog analytics tekniker som stöder ett flertal olika lösningsscenarier och krav. Att välja vilka Azure-tjänster toodeploy för en slutpunkt till slutpunkt-lösning kan vara en utmaning beroende erbjudanden hello bredd. Det här dokumentet är utformad toodescribe hello funktioner och interoperation för hello olika Azure-tjänster som stöder en lösning för direktuppspelning av händelsen. Här förklaras också hello scenarier där kunder kan dra nytta av den här typen av metoden.

## <a name="contents"></a>Innehåll
* Sammanfattning
* Introduktion tooReal tid Analytics
* Molntjänsters realtidsdata i Azure
* Vanliga scenarier för Realtidsanalys
* Arkitektur och komponenter
  * Datakällor
  * Dataintegrering lager
  * Realtidsanalys lager
  * Data lagringsskikt
  * Presentation / förbrukning lager
* Slutsats

**Skapad av:** Charles Feddersen, Lösningsarkitekt, insikter Datacenter av utmärkt, Microsoft Corporation

**Publicerad:** januari 2015

**Revision:** 1.0

**Hämta:** [realtid händelsebearbetning med Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

