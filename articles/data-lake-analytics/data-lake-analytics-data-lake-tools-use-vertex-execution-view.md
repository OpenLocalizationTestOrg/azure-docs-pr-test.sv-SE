---
title: "Använd vyn Vertex körning i Data Lake-verktyg för Visual Studio | Microsoft Docs"
description: "Lär dig använda Vertex körning vyn examen Data Lake Analytics-jobb."
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
ms.openlocfilehash: b788e7bc8ded86ebd49cc0be73e5b4e1bcbeaba3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a><span data-ttu-id="44f88-103">Använd vyn Vertex körning i Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="44f88-103">Use the Vertex Execution View in Data Lake Tools for Visual Studio</span></span>
<span data-ttu-id="44f88-104">Lär dig använda Vertex körning vyn examen Data Lake Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="44f88-104">Learn how to use the Vertex Execution View to exam Data Lake Analytics jobs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44f88-105">Krav</span><span class="sxs-lookup"><span data-stu-id="44f88-105">Prerequisites</span></span>

<span data-ttu-id="44f88-106">Du måste grundläggande kunskaper om med Data Lake-verktyg för Visual Studio för att utveckla U-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="44f88-106">You need basic knowledge of using Data Lake Tools for Visual Studio to develop U-SQL script.</span></span>  <span data-ttu-id="44f88-107">Se [Självstudier: utveckla U-SQL-skript med hjälp av Data Lake-verktyg för Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="44f88-107">See [Tutorial: develop U-SQL scripts using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>

## <a name="open-the-vertex-execution-view"></a><span data-ttu-id="44f88-108">Öppna vyn Vertex körning</span><span class="sxs-lookup"><span data-stu-id="44f88-108">Open the Vertex Execution View</span></span>
<span data-ttu-id="44f88-109">Öppna ett U-SQL-jobb i Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44f88-109">Open a U-SQL job in Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="44f88-110">Klicka på **Vertex vy** i det nedre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="44f88-110">Click **Vertex Execution View** in the bottom left corner.</span></span> <span data-ttu-id="44f88-111">Du kan uppmanas att först läsa in profiler och det kan ta lite tid beroende på nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="44f88-111">You may be prompted to load profiles first and it can take some time depending on your network connectivity.</span></span>

![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a><span data-ttu-id="44f88-113">Förstå Vertex vy</span><span class="sxs-lookup"><span data-stu-id="44f88-113">Understand Vertex Execution View</span></span>
<span data-ttu-id="44f88-114">Vyn Vertex körningen har tre delar:</span><span class="sxs-lookup"><span data-stu-id="44f88-114">The Vertex Execution View has three parts:</span></span>

![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

<span data-ttu-id="44f88-116">Den **Vertex selector** på vänster kan du välja formhörnen av funktioner (som topp 10 data läsa eller välja efter steget).</span><span class="sxs-lookup"><span data-stu-id="44f88-116">The **Vertex selector** on the left lets you select vertices by features (such as top 10 data read, or choose by stage).</span></span> <span data-ttu-id="44f88-117">En av de mest använda filter är att se den **formhörnen på kritiska**.</span><span class="sxs-lookup"><span data-stu-id="44f88-117">One of the most commonly-used filters is to see the **vertices on critical path**.</span></span> <span data-ttu-id="44f88-118">Den **kritiska** är den längsta kedjan av formhörnen ett U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="44f88-118">The **Critical path** is the longest chain of vertices of a U-SQL job.</span></span> <span data-ttu-id="44f88-119">Förstå viktiga sökvägen är användbart för att optimera dina jobb genom att kontrollera vilka vertex tar längst tid.</span><span class="sxs-lookup"><span data-stu-id="44f88-119">Understanding the critical path is useful for optimizing your jobs by checking which vertex takes the longest time.</span></span>
  
![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

<span data-ttu-id="44f88-121">Fönstret visas i mitten av **Körningsstatus för alla formhörnen**.</span><span class="sxs-lookup"><span data-stu-id="44f88-121">The top center pane shows the **running status of all the vertices**.</span></span>
  
![Data Lake Analytics verktyg Vertex körning visa](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

<span data-ttu-id="44f88-123">Längst ned i fönstret center visar information om varje vertex:</span><span class="sxs-lookup"><span data-stu-id="44f88-123">The bottom center pane shows information about each vertex:</span></span>
* <span data-ttu-id="44f88-124">Processnamnet: Namnet på vertex-instans.</span><span class="sxs-lookup"><span data-stu-id="44f88-124">Process Name: The name of the vertex instance.</span></span> <span data-ttu-id="44f88-125">Den består av olika delar i StageName | VertexName | VertexRunInstance.</span><span class="sxs-lookup"><span data-stu-id="44f88-125">It is composed of different parts in StageName|VertexName|VertexRunInstance.</span></span> <span data-ttu-id="44f88-126">Till exempel SV7_Split [62] .v1 vertex står för den andra körs instansen (.v1, index som börjar från 0) för Vertex tal 62 i steget SV7_Split.</span><span class="sxs-lookup"><span data-stu-id="44f88-126">For example, the SV7_Split[62].v1 vertex stands for the second running instance (.v1, index starting from 0) of Vertex number 62 in Stage SV7_Split.</span></span>
* <span data-ttu-id="44f88-127">Totala Data Läs/skriftliga: Data har lästs/skrivits av den här hörn.</span><span class="sxs-lookup"><span data-stu-id="44f88-127">Total Data Read/Written: The data was read/written by this vertex.</span></span>
* <span data-ttu-id="44f88-128">Tillstånd-/ utgång Status: Slutlig status när vertex avslutas.</span><span class="sxs-lookup"><span data-stu-id="44f88-128">State/Exit Status: The final status when the vertex is ended.</span></span>
* <span data-ttu-id="44f88-129">Avsluta felkod-fel typ: Felet när vertex misslyckades.</span><span class="sxs-lookup"><span data-stu-id="44f88-129">Exit Code/Failure Type: The error when the vertex failed.</span></span>
* <span data-ttu-id="44f88-130">Skapa orsak: Varför vertex skapades.</span><span class="sxs-lookup"><span data-stu-id="44f88-130">Creation Reason: Why the vertex was created.</span></span>
* <span data-ttu-id="44f88-131">Resursen latens/bearbeta latens/PN kön svarstid: tiden för vertex vänta resurser för att bearbeta data och för att stanna kvar i kön.</span><span class="sxs-lookup"><span data-stu-id="44f88-131">Resource Latency/Process Latency/PN Queue Latency: the time taken for the vertex to wait for resources, to process data, and to stay in the queue.</span></span>
* <span data-ttu-id="44f88-132">Processen/Creator GUID: GUID för den aktuella brytpunkten som körs eller dess creator.</span><span class="sxs-lookup"><span data-stu-id="44f88-132">Process/Creator GUID: GUID for the current running vertex or its creator.</span></span>
* <span data-ttu-id="44f88-133">Version: N: e-instansen körs vertex (systemet kan schemalägga nya instanser för ett hörn för många orsaker, till exempel redundans compute redundans, osv.)</span><span class="sxs-lookup"><span data-stu-id="44f88-133">Version: the N-th instance of the running vertex (the system might schedule new instances of a vertex for many reasons, for example failover, compute redundancy, etc.)</span></span>
* <span data-ttu-id="44f88-134">Version skapas en gång.</span><span class="sxs-lookup"><span data-stu-id="44f88-134">Version Created Time.</span></span>
* <span data-ttu-id="44f88-135">Bearbeta skapa Start tid/Process i kö tid/Process Start tid/Process slutförd tid: när vertex-processen börjar skapa; När processen vertex startar till kön; När vissa vertex processen startas; När vissa vertex har slutförts.</span><span class="sxs-lookup"><span data-stu-id="44f88-135">Process Create Start Time/Process Queued Time/Process Start Time/Process Complete Time: when the vertex process starts creation; when the vertex process starts to queue; when the certain vertex process starts; when the certain vertex is completed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44f88-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="44f88-136">Next steps</span></span>
* <span data-ttu-id="44f88-137">Information om hur du loggar diagnostikinformation finns i [Åtkomst till diagnostikloggar för Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span><span class="sxs-lookup"><span data-stu-id="44f88-137">To log diagnostics information, see [Accessing diagnostics logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)</span></span>
* <span data-ttu-id="44f88-138">Om du vill se en mer komplex fråga, se [Analysera webbplatsloggar med hjälp av Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="44f88-138">To see a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
* <span data-ttu-id="44f88-139">Om du vill visa jobbinformation [Använd jobbet webbläsare och visa jobb för Azure Data lake Analytics-jobb](data-lake-analytics-data-lake-tools-view-jobs.md)</span><span class="sxs-lookup"><span data-stu-id="44f88-139">To view job details, see [Use Job Browser and Job View for Azure Data lake Analytics jobs](data-lake-analytics-data-lake-tools-view-jobs.md)</span></span>
