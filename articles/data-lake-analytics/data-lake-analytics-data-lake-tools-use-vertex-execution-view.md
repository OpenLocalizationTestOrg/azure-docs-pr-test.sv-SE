---
title: "aaaUse hello Vertex vy i Data Lake-verktyg för Visual Studio | Microsoft Docs"
description: "Lär dig hur toouse hello Vertex vy tooexam Data Lake Analytics-jobb."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/13/2016
ms.author: jgao
ms.openlocfilehash: fb54d2af8a32aa27a54ff50a73c1b4903677a21e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a><span data-ttu-id="ec01d-103">Använd hello Vertex vy i Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ec01d-103">Use hello Vertex Execution View in Data Lake Tools for Visual Studio</span></span>
<span data-ttu-id="ec01d-104">Lär dig hur toouse hello Vertex vy tooexam Data Lake Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="ec01d-104">Learn how toouse hello Vertex Execution View tooexam Data Lake Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec01d-105">Krav</span><span class="sxs-lookup"><span data-stu-id="ec01d-105">Prerequisites</span></span>

<span data-ttu-id="ec01d-106">Du måste grundläggande kunskaper om med Data Lake-verktyg för Visual Studio toodevelop U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="ec01d-106">You need basic knowledge of using Data Lake Tools for Visual Studio toodevelop U-SQL script.</span></span>  <span data-ttu-id="ec01d-107">Se [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ec01d-107">See [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>

## <a name="open-hello-vertex-execution-view"></a><span data-ttu-id="ec01d-108">Öppna hello Vertex vy</span><span class="sxs-lookup"><span data-stu-id="ec01d-108">Open hello Vertex Execution View</span></span>
<span data-ttu-id="ec01d-109">Öppna ett U-SQL-jobb i Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ec01d-109">Open a U-SQL job in Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="ec01d-110">Klicka på **Vertex vy** i hello nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="ec01d-110">Click **Vertex Execution View** in hello bottom left corner.</span></span> <span data-ttu-id="ec01d-111">Du kanske ange tooload profiler först och det kan ta lite tid beroende på nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="ec01d-111">You may be prompted tooload profiles first and it can take some time depending on your network connectivity.</span></span>

![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a><span data-ttu-id="ec01d-113">Förstå Vertex vy</span><span class="sxs-lookup"><span data-stu-id="ec01d-113">Understand Vertex Execution View</span></span>
<span data-ttu-id="ec01d-114">hello Vertex vy består av tre delar:</span><span class="sxs-lookup"><span data-stu-id="ec01d-114">hello Vertex Execution View has three parts:</span></span>

![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

<span data-ttu-id="ec01d-116">Hej **Vertex selector** på hello vänstra kan du välja formhörnen av funktioner (som topp 10 data läsa eller välja efter steget).</span><span class="sxs-lookup"><span data-stu-id="ec01d-116">hello **Vertex selector** on hello left lets you select vertices by features (such as top 10 data read, or choose by stage).</span></span> <span data-ttu-id="ec01d-117">En av hello mest använda filter är toosee hello **formhörnen på kritiska**.</span><span class="sxs-lookup"><span data-stu-id="ec01d-117">One of hello most commonly-used filters is toosee hello **vertices on critical path**.</span></span> <span data-ttu-id="ec01d-118">Hej **kritiska** är hello längsta kedja av formhörnen ett U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="ec01d-118">hello **Critical path** is hello longest chain of vertices of a U-SQL job.</span></span> <span data-ttu-id="ec01d-119">Förstå hello kritiska är användbart för att optimera dina jobb genom att kontrollera vilka vertex tar hello längsta tid.</span><span class="sxs-lookup"><span data-stu-id="ec01d-119">Understanding hello critical path is useful for optimizing your jobs by checking which vertex takes hello longest time.</span></span>
  
![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

<span data-ttu-id="ec01d-121">hello överst i mitten visar hello **Körningsstatus för alla hello formhörnen**.</span><span class="sxs-lookup"><span data-stu-id="ec01d-121">hello top center pane shows hello **running status of all hello vertices**.</span></span>
  
![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

<span data-ttu-id="ec01d-123">hello längst ned i fönstret center visar information om varje vertex:</span><span class="sxs-lookup"><span data-stu-id="ec01d-123">hello bottom center pane shows information about each vertex:</span></span>
* <span data-ttu-id="ec01d-124">Processnamnet: hello namn på hello vertex instans.</span><span class="sxs-lookup"><span data-stu-id="ec01d-124">Process Name: hello name of hello vertex instance.</span></span> <span data-ttu-id="ec01d-125">Den består av olika delar i StageName | VertexName | VertexRunInstance.</span><span class="sxs-lookup"><span data-stu-id="ec01d-125">It is composed of different parts in StageName|VertexName|VertexRunInstance.</span></span> <span data-ttu-id="ec01d-126">Till exempel hello SV7_Split [62] .v1 vertex står för hello andra instans som körs (.v1, index som börjar från 0) för Vertex tal 62 i steget SV7_Split.</span><span class="sxs-lookup"><span data-stu-id="ec01d-126">For example, hello SV7_Split[62].v1 vertex stands for hello second running instance (.v1, index starting from 0) of Vertex number 62 in Stage SV7_Split.</span></span>
* <span data-ttu-id="ec01d-127">Totala Data Läs/skriftliga: hello data var Läs/skrivs av den här hörn.</span><span class="sxs-lookup"><span data-stu-id="ec01d-127">Total Data Read/Written: hello data was read/written by this vertex.</span></span>
* <span data-ttu-id="ec01d-128">Status för status-/ utgång: hello slutlig status när hello vertex avslutas.</span><span class="sxs-lookup"><span data-stu-id="ec01d-128">State/Exit Status: hello final status when hello vertex is ended.</span></span>
* <span data-ttu-id="ec01d-129">Avsluta felkod-fel typ: hello fel vid hello vertex misslyckades.</span><span class="sxs-lookup"><span data-stu-id="ec01d-129">Exit Code/Failure Type: hello error when hello vertex failed.</span></span>
* <span data-ttu-id="ec01d-130">Skapa orsak: Varför hello vertex skapades.</span><span class="sxs-lookup"><span data-stu-id="ec01d-130">Creation Reason: Why hello vertex was created.</span></span>
* <span data-ttu-id="ec01d-131">Svarstid för resursen latens/bearbeta latens/PN kön: hello tid det tar för hello vertex toowait för resurser, tooprocess data och toostay i hello kö.</span><span class="sxs-lookup"><span data-stu-id="ec01d-131">Resource Latency/Process Latency/PN Queue Latency: hello time taken for hello vertex toowait for resources, tooprocess data, and toostay in hello queue.</span></span>
* <span data-ttu-id="ec01d-132">Processen/Creator GUID: GUID för hello aktuella körs vertex eller dess creator.</span><span class="sxs-lookup"><span data-stu-id="ec01d-132">Process/Creator GUID: GUID for hello current running vertex or its creator.</span></span>
* <span data-ttu-id="ec01d-133">Version: hello nte instans av hello kör vertex (hello system kan schemalägga nya instanser för ett hörn för många orsaker, till exempel redundans compute redundans, osv.)</span><span class="sxs-lookup"><span data-stu-id="ec01d-133">Version: hello N-th instance of hello running vertex (hello system might schedule new instances of a vertex for many reasons, for example failover, compute redundancy, etc.)</span></span>
* <span data-ttu-id="ec01d-134">Version skapas en gång.</span><span class="sxs-lookup"><span data-stu-id="ec01d-134">Version Created Time.</span></span>
* <span data-ttu-id="ec01d-135">Bearbeta skapa Start tid/Process i kö tid/Process Start tid/Process slutförd tid: när hello vertex processen börjar skapa; När hello vertex processen startar tooqueue; När hello startar vissa vertex; När hello vissa vertex har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ec01d-135">Process Create Start Time/Process Queued Time/Process Start Time/Process Complete Time: when hello vertex process starts creation; when hello vertex process starts tooqueue; when hello certain vertex process starts; when hello certain vertex is completed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec01d-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ec01d-136">Next steps</span></span>
* <span data-ttu-id="ec01d-137">toolog diagnostikinformation, se [åtkomst till diagnostik loggar för Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span><span class="sxs-lookup"><span data-stu-id="ec01d-137">toolog diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span></span>
* <span data-ttu-id="ec01d-138">toosee en mer komplex fråga, se [analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="ec01d-138">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="ec01d-139">tooview jobbinformation finns [Använd jobbet webbläsare och visa jobb för Azure Data lake Analytics-jobb](data-lake-analytics-data-lake-tools-view-jobs.md)</span><span class="sxs-lookup"><span data-stu-id="ec01d-139">tooview job details, see [Use Job Browser and Job View for Azure Data lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md)</span></span>
