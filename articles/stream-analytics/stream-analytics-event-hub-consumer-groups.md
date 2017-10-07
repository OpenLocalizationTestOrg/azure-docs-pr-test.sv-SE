---
title: med event hub mottagare aaaDebug Azure Stream Analytics | Microsoft Docs
description: "Fråga bästa praxis för att bedöma Händelsehubbar konsumentgrupper i Stream Analytics-jobb."
keywords: "Event hub gränsen konsumentgrupp"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 89821e6273151de43b5e42d907e547c939e24824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a><span data-ttu-id="95613-104">Felsöka Azure Stream Analytics med event hub mottagare</span><span class="sxs-lookup"><span data-stu-id="95613-104">Debug Azure Stream Analytics with event hub receivers</span></span>

<span data-ttu-id="95613-105">Du kan använda Azure Händelsehubbar i Azure Stream Analytics tooingest eller utdata från ett jobb.</span><span class="sxs-lookup"><span data-stu-id="95613-105">You can use Azure Event Hubs in Azure Stream Analytics tooingest or output data from a job.</span></span> <span data-ttu-id="95613-106">Det är bra för att använda Händelsehubbar toouse flera konsumentgrupper, tooensure jobbet skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="95613-106">A best practice for using Event Hubs is toouse multiple consumer groups, tooensure job scalability.</span></span> <span data-ttu-id="95613-107">En orsak är att hello antalet läsare i hello Stream Analytics-jobbet för specifika indata påverkar hello antalet läsare i en enskild konsument-grupp.</span><span class="sxs-lookup"><span data-stu-id="95613-107">One reason is that hello number of readers in hello Stream Analytics job for a specific input affects hello number of readers in a single consumer group.</span></span> <span data-ttu-id="95613-108">hello exakta antal mottagare baseras på interna implementeringsdetaljerna för hello skalbar topologi logik.</span><span class="sxs-lookup"><span data-stu-id="95613-108">hello precise number of receivers is based on internal implementation details for hello scale-out topology logic.</span></span> <span data-ttu-id="95613-109">hello antal mottagare exponeras inte externt.</span><span class="sxs-lookup"><span data-stu-id="95613-109">hello number of receivers is not exposed externally.</span></span> <span data-ttu-id="95613-110">hello antalet läsare kan ändra vid hello jobbets starttid eller under uppgraderingar av jobbet.</span><span class="sxs-lookup"><span data-stu-id="95613-110">hello number of readers can change either at hello job start time or during job upgrades.</span></span>

> [!NOTE]
> <span data-ttu-id="95613-111">När hello antalet läsare ändras under en uppgradering av jobb, skrivs tillfälligt varningar tooaudit loggar.</span><span class="sxs-lookup"><span data-stu-id="95613-111">When hello number of readers changes during a job upgrade, transient warnings are written tooaudit logs.</span></span> <span data-ttu-id="95613-112">Stream Analytics-jobb återskapa automatiskt från dessa tillfälliga problem.</span><span class="sxs-lookup"><span data-stu-id="95613-112">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a><span data-ttu-id="95613-113">Antalet läsare per partition överskrider Händelsehubbar gräns på fem</span><span class="sxs-lookup"><span data-stu-id="95613-113">Number of readers per partition exceeds Event Hubs limit of five</span></span>

<span data-ttu-id="95613-114">Scenarier i vilka hello överskrider antalet läsare per partition hello Händelsehubbar gräns på fem inkludera hello följande:</span><span class="sxs-lookup"><span data-stu-id="95613-114">Scenarios in which hello number of readers per partition exceeds hello Event Hubs limit of five include hello following:</span></span>

* <span data-ttu-id="95613-115">Flera SELECT-satser: Om du använder flera SELECT-satser som finns för**samma** händelsehubb indata, varje SELECT-sats orsakar en ny mottagare toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="95613-115">Multiple SELECT statements: If you use multiple SELECT statements that refer too**same** event hub input, each SELECT statement causes a new receiver toobe created.</span></span>
* <span data-ttu-id="95613-116">UNION: När du använder en UNION, det är möjligt toohave flera indata som refererar toohello **samma** händelsegruppen NAV- och konsumenter.</span><span class="sxs-lookup"><span data-stu-id="95613-116">UNION: When you use a UNION, it's possible toohave multiple inputs that refer toohello **same** event hub and consumer group.</span></span>
* <span data-ttu-id="95613-117">SJÄLVKOPPLING: När du använder en SELF JOIN-åtgärd, det är möjligt toorefer toohello **samma** händelsehubb flera gånger.</span><span class="sxs-lookup"><span data-stu-id="95613-117">SELF JOIN: When you use a SELF JOIN operation, it's possible toorefer toohello **same** event hub multiple times.</span></span>

## <a name="solution"></a><span data-ttu-id="95613-118">Lösning</span><span class="sxs-lookup"><span data-stu-id="95613-118">Solution</span></span>

<span data-ttu-id="95613-119">hello kan följande säkerhetsmetoder minimera scenarier i vilka hello överskrider antalet läsare per partition hello Händelsehubbar gräns på fem.</span><span class="sxs-lookup"><span data-stu-id="95613-119">hello following best practices can help mitigate scenarios in which hello number of readers per partition exceeds hello Event Hubs limit of five.</span></span>

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a><span data-ttu-id="95613-120">Dela din fråga i flera steg med hjälp av en WITH-satsen</span><span class="sxs-lookup"><span data-stu-id="95613-120">Split your query into multiple steps by using a WITH clause</span></span>

<span data-ttu-id="95613-121">hello WITH-satsen anger en tillfällig namngivna resultatmängd som kan refereras av en FROM-sats i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="95613-121">hello WITH clause specifies a temporary named result set that can be referenced by a FROM clause in hello query.</span></span> <span data-ttu-id="95613-122">Du definierar hello WITH-satsen i hello körning omfånget för en enstaka SELECT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="95613-122">You define hello WITH clause in hello execution scope of a single SELECT statement.</span></span>

<span data-ttu-id="95613-123">Till exempel i stället för den här frågan:</span><span class="sxs-lookup"><span data-stu-id="95613-123">For example, instead of this query:</span></span>

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

<span data-ttu-id="95613-124">Använd den här frågan:</span><span class="sxs-lookup"><span data-stu-id="95613-124">Use this query:</span></span>

```
WITH input (
   SELECT * FROM inputEventHub
) as data

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-toodifferent-consumer-groups"></a><span data-ttu-id="95613-125">Kontrollera att indata binda toodifferent konsumentgrupper</span><span class="sxs-lookup"><span data-stu-id="95613-125">Ensure that inputs bind toodifferent consumer groups</span></span>

<span data-ttu-id="95613-126">För frågor som är tre eller flera inmatningar anslutna toohello samma Händelsehubbar konsumenten, skapa separata konsumentgrupper.</span><span class="sxs-lookup"><span data-stu-id="95613-126">For queries in which three or more inputs are connected toohello same Event Hubs consumer group, create separate consumer groups.</span></span> <span data-ttu-id="95613-127">Detta kräver hello skapandet av ytterligare Stream Analytics-indata.</span><span class="sxs-lookup"><span data-stu-id="95613-127">This requires hello creation of additional Stream Analytics inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="95613-128">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="95613-128">Get help</span></span>
<span data-ttu-id="95613-129">Mer hjälp, försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="95613-129">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="95613-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95613-130">Next steps</span></span>
* [<span data-ttu-id="95613-131">Introduktion tooStream Analytics</span><span class="sxs-lookup"><span data-stu-id="95613-131">Introduction tooStream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="95613-132">Kom igång med Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="95613-132">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="95613-133">Skala Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="95613-133">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="95613-134">Språkreferens för Stream Analytics-fråga</span><span class="sxs-lookup"><span data-stu-id="95613-134">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="95613-135">Strömma Analytics management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="95613-135">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
