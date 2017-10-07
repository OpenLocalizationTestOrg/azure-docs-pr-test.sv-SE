---
title: "aaaHow toowrite frågor i Stream Analytics | Microsoft Docs"
description: "Skriva frågor i Stream Analytics och fråga efter data | Learning sökvägssegment."
keywords: "hur toowrite frågor, fråga efter data skriva en fråga, skriva frågor"
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
ms.openlocfilehash: b943c34f10afd2b21789afbd341c471a5f168729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowrite-queries-in-stream-analytics"></a><span data-ttu-id="b3438-104">Hur toowrite frågor i Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b3438-104">How toowrite queries in Stream Analytics</span></span>
<span data-ttu-id="b3438-105">Skriva frågor för bearbetning av logiken i Azure Stream Analytics implementeras som en ”stående-fråga” som har definierats innan ett jobb startar och köras på data eftersom den når hello jobb.</span><span class="sxs-lookup"><span data-stu-id="b3438-105">Writing queries for stream processing logic in Azure Stream Analytics is implemented as a "standing query" that is defined before a job starts and executed on data as it reaches hello job.</span></span> <span data-ttu-id="b3438-106">Hej Dataomvandling uttryckt i ett SQL-liknande frågespråk som i stort sett en delmängd av T-SQL med några läggs-tillägg som [fönsterhantering](https://msdn.microsoft.com/library/azure/dn835019.aspx) används tooexpress temporala semantik.</span><span class="sxs-lookup"><span data-stu-id="b3438-106">hello data transformation is expressed in a SQL-like query language, which is largely a subset of T-SQL with some added language extensions like [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) used tooexpress temporal semantics.</span></span>

## <a name="writing-queries"></a><span data-ttu-id="b3438-107">Skriva frågor:</span><span class="sxs-lookup"><span data-stu-id="b3438-107">Writing Queries:</span></span>
1. <span data-ttu-id="b3438-108">I din Stream Analytics-jobbet i hello Azure Management portal, klickar du på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="b3438-108">In your Stream Analytics Job in hello Azure Management portal, click **Query**.</span></span>
   
    ![Välj fråga](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    <span data-ttu-id="b3438-110">I hello Azure-portalen klickar du på **frågan**.</span><span class="sxs-lookup"><span data-stu-id="b3438-110">In hello Azure Portal, click **Query**.</span></span>
   
    ![Välj Query Preview](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. <span data-ttu-id="b3438-112">Nya jobb har en fråga mallen toohelp komma igång.</span><span class="sxs-lookup"><span data-stu-id="b3438-112">New jobs have a query template toohelp get you started.</span></span> <span data-ttu-id="b3438-113">hello frågan mallen utför en ”direktlagringsdisk” fråga att projekt alla fält från inkommande händelser i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="b3438-113">hello query template performs a "pass-through" query that projects all fields from input events into hello output.</span></span>  
   
   * <span data-ttu-id="b3438-114">Om du har definierat minst ett indata och utdata för jobbet, kan du ersätta hello platshållare ”[YourOutputAlias]” och ”[YourInputAlias]” fält med hello-alias för hello indata och utdata som du vill använda först.</span><span class="sxs-lookup"><span data-stu-id="b3438-114">If you have defined at least one input and output for your job, you can replace hello placeholder "[YourOutputAlias]" and "[YourInputAlias]" fields with hello aliases of hello input and output that you wish use first.</span></span> <span data-ttu-id="b3438-115">Dessutom kan du fortfarande skapa och testa din fråga i hello klassiska Azure-portalen utan att definiera indata och utdata för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="b3438-115">In addition, you can still author and test your query in hello Azure Classic Portal without defining inputs and outputs on hello job.</span></span>
   * <span data-ttu-id="b3438-116">Om du vill tooperform mer bearbetning än en enkel direkt kan redigera du hello frågedefinitionen.</span><span class="sxs-lookup"><span data-stu-id="b3438-116">If you wish tooperform more processing than a simple pass-through, you can edit hello query definition.</span></span> <span data-ttu-id="b3438-117">tooget igång med redigering av frågan ta en titt på vissa vanlig fråga mönster fångas [här](stream-analytics-stream-analytics-query-patterns.md).</span><span class="sxs-lookup"><span data-stu-id="b3438-117">tooget started with query authoring, take a look at some common query patterns are captured [here](stream-analytics-stream-analytics-query-patterns.md).</span></span>  
   
   ![Fråga efter data fönster](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="toovalidate-query-data-is-working"></a><span data-ttu-id="b3438-119">toovalidate fråga efter data fungerar:</span><span class="sxs-lookup"><span data-stu-id="b3438-119">toovalidate query data is working:</span></span>
<span data-ttu-id="b3438-120">Du kan testa att frågan fungerar som förväntat genom att köra det i hello webbläsare över en eller flera lokala JSON-filer som innehåller testdata.</span><span class="sxs-lookup"><span data-stu-id="b3438-120">You can test that your query behaves as expected by running it in hello browser over one or more local JSON files containing test data.</span></span> <span data-ttu-id="b3438-121">Det här inte starta hello jobbet eller ha någon faktureringsinformation konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="b3438-121">This will not start hello job or have any billing implications.</span></span>

> [!NOTE]
> <span data-ttu-id="b3438-122">För närvarande stöds webbläsarbaserad frågetestning inte i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b3438-122">Currently in-browser query testing is not supported in hello Azure Portal.</span></span>  
> 
> 

1. <span data-ttu-id="b3438-123">Kontrollera att det inte finns några fel i hello-fråga (annars hello testknappen inaktiveras) och klicka sedan på knappen för hello-Test.</span><span class="sxs-lookup"><span data-stu-id="b3438-123">Make sure that there are no errors in hello query (otherwise hello Test button will be disabled) and then click hello Test button.</span></span>  
   
   ![Fråga efter data Test](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. <span data-ttu-id="b3438-125">Du kommer att tillfrågas toospecify filer för varje hello indata som refereras till i hello frågan.</span><span class="sxs-lookup"><span data-stu-id="b3438-125">You will be prompted toospecify files for each of hello inputs referenced in hello query.</span></span> <span data-ttu-id="b3438-126">I det här exemplet hello mallen fråga lämnas som-är så hello dialogrutan uppmanar för indata med namnet ”yourinputalias”.</span><span class="sxs-lookup"><span data-stu-id="b3438-126">In this example, hello template query is left as-is, so hello dialog is prompting for an input named "yourinputalias".</span></span>  
   
   ![Testa fråga](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. <span data-ttu-id="b3438-128">Bläddra tooa Testa fil.</span><span class="sxs-lookup"><span data-stu-id="b3438-128">Browse tooa test file.</span></span> <span data-ttu-id="b3438-129">Flera exempelfiler är tillgängliga på [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) och du kan också hämta exempeldata från din egen dataströmmen indata via hello exempeldata funktionen hello indata på fliken.</span><span class="sxs-lookup"><span data-stu-id="b3438-129">Several sample files are available on [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) and you can also retrieve sample data from your own data stream inputs via hello Sample Data function on hello inputs tab.</span></span>  
   
   ![Frågan indata](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. <span data-ttu-id="b3438-131">När du har stängt dialogrutan hello frågan ska köras via hello testdata och du ser hello resultat på hello hello frågan sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="b3438-131">After closing hello dialog, your query will be run over hello test data and you will see hello results at hello bottom of hello Query page.</span></span>  
   
   ![Frågesammanfattning av](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a><span data-ttu-id="b3438-133">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="b3438-133">Get help</span></span>
<span data-ttu-id="b3438-134">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="b3438-134">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3438-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b3438-135">Next steps</span></span>
* [<span data-ttu-id="b3438-136">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b3438-136">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="b3438-137">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="b3438-137">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="b3438-138">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="b3438-138">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="b3438-139">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="b3438-139">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="b3438-140">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="b3438-140">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

