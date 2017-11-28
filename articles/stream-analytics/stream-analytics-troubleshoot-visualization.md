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
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a><span data-ttu-id="7f042-103">Visualisera och felsöka Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="7f042-103">Visualize and troubleshoot Stream Analytics jobs</span></span>
<span data-ttu-id="7f042-104">I Stream Analytics, precis som med andra molnbaserad teknik är felsökning ibland nödvändiga toolook till varför ett jobb skapar inte hello förväntades utdata (eller inga utdata att).</span><span class="sxs-lookup"><span data-stu-id="7f042-104">In Stream Analytics, as with other cloud-based technologies, troubleshooting is sometimes needed toolook into why a job does not produce hello expected output (or any output for that matter).</span></span> <span data-ttu-id="7f042-105">Med detta i åtanke ger Stream Analytics hello för visualisering av ett direktuppspelningsjobb.</span><span class="sxs-lookup"><span data-stu-id="7f042-105">With this in mind, Stream Analytics provides hello capability for visualizing a streaming job.</span></span> <span data-ttu-id="7f042-106">Detta är också användbar som ett verktyg för modellering och har hello sida-förmån för organisationer som behöver dokumentation av sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="7f042-106">This is also handy as a modeling tool and has hello side benefit for those requiring documentation of their work.</span></span>

<span data-ttu-id="7f042-107">I hello visualiseringen panelen hello indata visas samt hello-fråga som körs och sedan alla hello utdata konfigureras.</span><span class="sxs-lookup"><span data-stu-id="7f042-107">In hello visualization panel hello inputs are visible as well as hello query being executed and then all hello outputs configured.</span></span> <span data-ttu-id="7f042-108">Problem med nätverksanslutningen eller konfiguration kan bli tydligare och det kan också vara användbara toosee en bild av din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7f042-108">Connectivity or configuration issues can become more apparent and it can also be helpful toosee a visual representation of your configuration.</span></span>

## <a name="using-hello-diagnosis-diagram-tool"></a><span data-ttu-id="7f042-109">Verktyget hello diagnos diagram</span><span class="sxs-lookup"><span data-stu-id="7f042-109">Using hello diagnosis diagram tool</span></span>
<span data-ttu-id="7f042-110">tooaccess som den här visualizer helt enkelt klicka på hello ”diagnos diagram”-knappen i hello ”inställningar” bladet för hello av hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="7f042-110">tooaccess this visualizer, simply click on hello “Diagnosis diagram” button in hello “Settings” blade of hello of hello Stream Analytics job.</span></span>

![Stream-Analytics-Troubleshoot-Visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

<span data-ttu-id="7f042-112">Varje inkommande och utgående är färgkodade tooindicate hello aktuell status för komponenten, som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="7f042-112">Every input and output is color coded tooindicate hello current state of that component, as shown below.</span></span>

![Stream-Analytics-Troubleshoot-Visualization-Input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

<span data-ttu-id="7f042-114">När hello användaren vill toolook på mellanliggande steg toounderstand hello data flödet frågemönster inuti ett jobb, visar hello visualiseringen verktyget hello uppdelning av hello frågan till dess komponenten steg och hello flödessekvens.</span><span class="sxs-lookup"><span data-stu-id="7f042-114">When hello user wants toolook at intermediate query steps toounderstand hello data flow patterns inside a job, hello visualization tool provides a view of hello breakdown of hello query into its component steps and hello flow sequence.</span></span> <span data-ttu-id="7f042-115">Klicka på varje frågesteg visar hello motsvarande avsnitt i en fråga Redigera fönstret som visas.</span><span class="sxs-lookup"><span data-stu-id="7f042-115">Clicking on each query step will show hello corresponding section in a query editing pane as illustrated.</span></span> 

![Stream-Analytics-Troubleshoot-Visualization-Intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a><span data-ttu-id="7f042-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f042-117">Next steps</span></span>
* [<span data-ttu-id="7f042-118">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="7f042-118">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="7f042-119">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="7f042-119">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="7f042-120">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="7f042-120">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="7f042-121">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="7f042-121">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="7f042-122">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="7f042-122">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

