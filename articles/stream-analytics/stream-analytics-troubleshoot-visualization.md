---
title: "aaaVisualize och felsöka Stream Analytics-jobb | Microsoft Docs"
description: "Lär dig hur toovisualize ett Stream Analytics-jobb i pipeline för självbetjäning felsökning med hjälp av hello diagnostik för diagram."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 8a6715be601fdc47b8d9caf4112da161dad22618
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualisera och felsöka Stream Analytics-jobb
I Stream Analytics, precis som med andra molnbaserad teknik är felsökning ibland nödvändiga toolook till varför ett jobb skapar inte hello förväntades utdata (eller inga utdata att). Med detta i åtanke ger Stream Analytics hello för visualisering av ett direktuppspelningsjobb. Detta är också användbar som ett verktyg för modellering och har hello sida-förmån för organisationer som behöver dokumentation av sitt arbete.

I hello visualiseringen panelen hello indata visas samt hello-fråga som körs och sedan alla hello utdata konfigureras. Problem med nätverksanslutningen eller konfiguration kan bli tydligare och det kan också vara användbara toosee en bild av din konfiguration.

## <a name="using-hello-diagnosis-diagram-tool"></a>Verktyget hello diagnos diagram
tooaccess som den här visualizer helt enkelt klicka på hello ”diagnos diagram”-knappen i hello ”inställningar” bladet för hello av hello Stream Analytics-jobbet.

![Stream-Analytics-Troubleshoot-Visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Varje inkommande och utgående är färgkodade tooindicate hello aktuell status för komponenten, som visas nedan.

![Stream-Analytics-Troubleshoot-Visualization-Input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

När hello användaren vill toolook på mellanliggande steg toounderstand hello data flödet frågemönster inuti ett jobb, visar hello visualiseringen verktyget hello uppdelning av hello frågan till dess komponenten steg och hello flödessekvens. Klicka på varje frågesteg visar hello motsvarande avsnitt i en fråga Redigera fönstret som visas. 

![Stream-Analytics-Troubleshoot-Visualization-Intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

