---
title: "Hur du skriver frågor i Stream Analytics | Microsoft Docs"
description: "Skriva frågor i Stream Analytics och fråga efter data | Learning sökvägssegment."
keywords: "hur du skriver frågor, fråga efter data, skriva en fråga, skriva frågor"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: b44b0658a06761a805708e7fdeba9e3b2cf9d3ab
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-write-queries-in-stream-analytics"></a><span data-ttu-id="31bce-104">Hur du skriver frågor i Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="31bce-104">How to write queries in Stream Analytics</span></span>
<span data-ttu-id="31bce-105">Skriva frågor för bearbetning av logiken i Azure Stream Analytics implementeras som en ”stående-fråga” som har definierats innan ett jobb startar och köras på data eftersom den når jobbet.</span><span class="sxs-lookup"><span data-stu-id="31bce-105">Writing queries for stream processing logic in Azure Stream Analytics is implemented as a "standing query" that is defined before a job starts and executed on data as it reaches the job.</span></span> <span data-ttu-id="31bce-106">Dataomvandling uttryckt i ett SQL-liknande frågespråk som i stort sett en delmängd av T-SQL med några läggs-tillägg som [fönsterhantering](https://msdn.microsoft.com/library/azure/dn835019.aspx) användas för att uttrycka temporal semantik.</span><span class="sxs-lookup"><span data-stu-id="31bce-106">The data transformation is expressed in a SQL-like query language, which is largely a subset of T-SQL with some added language extensions like [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) used to express temporal semantics.</span></span>

## <a name="writing-queries"></a><span data-ttu-id="31bce-107">Skriva frågor:</span><span class="sxs-lookup"><span data-stu-id="31bce-107">Writing Queries:</span></span>
1. <span data-ttu-id="31bce-108">I din Stream Analytics-jobbet i Azure-hanteringsportalen, klickar du på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="31bce-108">In your Stream Analytics Job in the Azure Management portal, click **Query**.</span></span>
   
    ![Välj fråga](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    <span data-ttu-id="31bce-110">I Azure-portalen klickar du på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="31bce-110">In the Azure Portal, click **Query**.</span></span>
   
    ![Välj Query Preview](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. <span data-ttu-id="31bce-112">Nya jobb har en fråga mall för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="31bce-112">New jobs have a query template to help get you started.</span></span> <span data-ttu-id="31bce-113">Frågemallen utför en ”” direktfråga som projekt alla fält från inkommande händelser i utdata.</span><span class="sxs-lookup"><span data-stu-id="31bce-113">The query template performs a "pass-through" query that projects all fields from input events into the output.</span></span>  
   
   * <span data-ttu-id="31bce-114">Om du har definierat minst ett indata och utdata för jobbet, kan du ersätta platshållaren ”[YourOutputAlias]” och ”[YourInputAlias]” fält med alias för indata och utdata som du vill använda först.</span><span class="sxs-lookup"><span data-stu-id="31bce-114">If you have defined at least one input and output for your job, you can replace the placeholder "[YourOutputAlias]" and "[YourInputAlias]" fields with the aliases of the input and output that you wish use first.</span></span> <span data-ttu-id="31bce-115">Dessutom kan du fortfarande skapa och testa din fråga i den klassiska Azure-portalen utan att definiera indata och utdata för jobbet.</span><span class="sxs-lookup"><span data-stu-id="31bce-115">In addition, you can still author and test your query in the Azure Classic Portal without defining inputs and outputs on the job.</span></span>
   * <span data-ttu-id="31bce-116">Om du vill utföra mer bearbetning än en enkel direkt kan du redigera frågedefinitionen.</span><span class="sxs-lookup"><span data-stu-id="31bce-116">If you wish to perform more processing than a simple pass-through, you can edit the query definition.</span></span> <span data-ttu-id="31bce-117">Om du vill komma igång med redigering av frågan, ta en titt på vissa vanlig fråga mönster fångas [här](stream-analytics-stream-analytics-query-patterns.md).</span><span class="sxs-lookup"><span data-stu-id="31bce-117">To get started with query authoring, take a look at some common query patterns are captured [here](stream-analytics-stream-analytics-query-patterns.md).</span></span>  
   
   ![Fråga efter data fönster](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a><span data-ttu-id="31bce-119">Om du vill validera frågedata fungerar:</span><span class="sxs-lookup"><span data-stu-id="31bce-119">To validate query data is working:</span></span>
<span data-ttu-id="31bce-120">Du kan testa att frågan fungerar som förväntat genom att köra det i webbläsaren över en eller flera lokala JSON-filer som innehåller testdata.</span><span class="sxs-lookup"><span data-stu-id="31bce-120">You can test that your query behaves as expected by running it in the browser over one or more local JSON files containing test data.</span></span> <span data-ttu-id="31bce-121">Det här inte starta jobbet eller ha alla fakturering konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="31bce-121">This will not start the job or have any billing implications.</span></span>

> [!NOTE]
> <span data-ttu-id="31bce-122">För närvarande stöds webbläsarbaserad frågetestning inte i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="31bce-122">Currently in-browser query testing is not supported in the Azure Portal.</span></span>  
> 
> 

1. <span data-ttu-id="31bce-123">Kontrollera att det inte finns några fel i frågan (annars knappen Testa inaktiveras) och klicka sedan på knappen Testa.</span><span class="sxs-lookup"><span data-stu-id="31bce-123">Make sure that there are no errors in the query (otherwise the Test button will be disabled) and then click the Test button.</span></span>  
   
   ![Fråga efter data Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. <span data-ttu-id="31bce-125">Du uppmanas att ange filer för varje indata som refereras i frågan.</span><span class="sxs-lookup"><span data-stu-id="31bce-125">You will be prompted to specify files for each of the inputs referenced in the query.</span></span> <span data-ttu-id="31bce-126">I det här exemplet lämnas mallen frågan som-är så att dialogrutan uppmanar för indata med namnet ”yourinputalias”.</span><span class="sxs-lookup"><span data-stu-id="31bce-126">In this example, the template query is left as-is, so the dialog is prompting for an input named "yourinputalias".</span></span>  
   
   ![Testa fråga](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. <span data-ttu-id="31bce-128">Bläddra till en testfil.</span><span class="sxs-lookup"><span data-stu-id="31bce-128">Browse to a test file.</span></span> <span data-ttu-id="31bce-129">Flera exempelfiler är tillgängliga på [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) och du kan också hämta exempeldata från din egen dataströmmen indata via funktionen exempeldata på fliken indata.</span><span class="sxs-lookup"><span data-stu-id="31bce-129">Several sample files are available on [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) and you can also retrieve sample data from your own data stream inputs via the Sample Data function on the inputs tab.</span></span>  
   
   ![Frågan indata](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. <span data-ttu-id="31bce-131">Frågan ska köras via testdata och resultaten längst ned på sidan frågan visas när du har stängt dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="31bce-131">After closing the dialog, your query will be run over the test data and you will see the results at the bottom of the Query page.</span></span>  
   
   ![Frågesammanfattning av](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a><span data-ttu-id="31bce-133">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="31bce-133">Get help</span></span>
<span data-ttu-id="31bce-134">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="31bce-134">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="31bce-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31bce-135">Next steps</span></span>
* [<span data-ttu-id="31bce-136">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="31bce-136">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="31bce-137">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="31bce-137">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="31bce-138">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="31bce-138">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="31bce-139">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="31bce-139">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="31bce-140">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="31bce-140">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

