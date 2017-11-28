---
title: "aaaHow toocreate data analytics bearbetning jobb för Stream Analytics | Microsoft Docs"
description: "Skapa ett data analytics-jobb för bearbetning för Stream Analytics | Learning sökvägssegment."
keywords: analytics databearbetning
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: d4a3c89d8862d59688d06a1719b063efa2ab1c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-data-analytics-processing-job-for-stream-analytics"></a><span data-ttu-id="cce13-104">Hur toocreate en analytics databearbetning jobbet för Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cce13-104">How toocreate a data analytics processing job for Stream Analytics</span></span>
<span data-ttu-id="cce13-105">hello översta resurs i Azure Stream Analytics är en Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="cce13-105">hello top-level resource in Azure Stream Analytics is a Stream Analytics Job.</span></span>  <span data-ttu-id="cce13-106">Det består av en eller flera ingående datakällor, en fråga uttrycka hello DTS och ett eller flera mål för utdata som resultat har skrivits till.</span><span class="sxs-lookup"><span data-stu-id="cce13-106">It consists of one or more input data sources, a query expressing hello data transformation, and one or more output targets that results are written to.</span></span> <span data-ttu-id="cce13-107">Dessa aktivera tillsammans hello användaren tooperform dataanalys för strömmande data bearbetades scenarier.</span><span class="sxs-lookup"><span data-stu-id="cce13-107">Together these enable hello user tooperform data analytics processing for streaming data scenarios.</span></span>

<span data-ttu-id="cce13-108">toostart med Stream Analytics, börja med att skapa ett nytt Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="cce13-108">toostart using Stream Analytics, begin by creating a new Stream Analytics job.</span></span>  <span data-ttu-id="cce13-109">Observera att den här åtgärden har ingen fakturering effekter tills hello påbörjades.</span><span class="sxs-lookup"><span data-stu-id="cce13-109">Note this action has no billing implications until hello job is started.</span></span>

1. <span data-ttu-id="cce13-110">Logga in på hello online [klassiska Azure-portalen](http://manage.windowsazure.com) eller hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="cce13-110">Sign in on hello online [Azure classic portal](http://manage.windowsazure.com) or hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="cce13-111">I hello portal: **klickar du på ny**, klicka på **datatjänster** eller **dataanalys** beroende på din portal och klicka sedan på **Azure Stream Analytics** eller **strömma Analytics** och sedan **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="cce13-111">In hello portal: **Click New**, then click **Data Services** or **Data Analytics** depending on your portal and then click **Azure Stream Analytics** or **Stream Analytics** and then **Quick Create**.</span></span>
   
   ![Guiden analytics databearbetning](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Skapa dataanalys bearbetning jobbet](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. <span data-ttu-id="cce13-114">Ange hello önskad konfiguration för hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="cce13-114">Specify hello desired configuration for hello Stream Analytics job.</span></span>
   
   * <span data-ttu-id="cce13-115">I hello **jobbnamn** anger du ett namn tooidentify hello Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="cce13-115">In hello **Job Name** box, enter a name tooidentify hello Stream Analytics job.</span></span> <span data-ttu-id="cce13-116">När hello **jobbnamn** har verifierats, visas en grön kryssmarkering i hello jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="cce13-116">When hello **Job Name** is validated, a green check mark appears in hello Job Name box.</span></span> <span data-ttu-id="cce13-117">Hej **jobbnamn** får bara innehålla alfanumeriska tecken och hello '-' tecken och måste vara mellan 3 och 63 tecken.</span><span class="sxs-lookup"><span data-stu-id="cce13-117">hello **Job Name** may contain only alphanumeric characters and hello '-' character, and must be between 3 and 63 characters.</span></span>
   * <span data-ttu-id="cce13-118">Använd **Region** i hello Azure-portalen eller **plats** i hello Azure portal toospecify hello geografiska plats där du vill toorun hello jobb.</span><span class="sxs-lookup"><span data-stu-id="cce13-118">Use **Region** in hello Azure portal or **Location** in hello Azure portal toospecify hello geographic location where you want toorun hello job.</span></span>
   * <span data-ttu-id="cce13-119">Om du använder hello Azure-portalen, Välj eller skapa en storage-konto toouse som hello **Lagringskonto för Regional övervakning**.</span><span class="sxs-lookup"><span data-stu-id="cce13-119">If using hello Azure portal, select or create a storage account toouse as hello **Regional Monitoring Storage Account**.</span></span> <span data-ttu-id="cce13-120">Det här lagringskontot är används toostore övervakningsdata för alla Stream Analytics-jobb som körs i den här regionen.</span><span class="sxs-lookup"><span data-stu-id="cce13-120">This storage account is used toostore monitoring data for all Stream Analytics jobs running in this region.</span></span>
   * <span data-ttu-id="cce13-121">Om du använder hello Azure-portalen, ange en ny eller befintlig **resursgruppen** toohold relaterade resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="cce13-121">If using hello Azure portal, specify a new or existing **Resource Group** toohold related resources for your application.</span></span>
4. <span data-ttu-id="cce13-122">När hello nya Stream Analytics-jobbalternativ har konfigurerats, klickar du på **skapa Stream Analytics-jobbet**.</span><span class="sxs-lookup"><span data-stu-id="cce13-122">Once hello new Stream Analytics job options are configured, click **Create Stream Analytics Job**.</span></span> <span data-ttu-id="cce13-123">Det kan ta några minuter för hello Stream Analytics-jobbet toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="cce13-123">It can take a few minutes for hello Stream Analytics job toobe created.</span></span> <span data-ttu-id="cce13-124">Du kan övervaka hello förlopp i hello meddelandehubben toocheck hello status.</span><span class="sxs-lookup"><span data-stu-id="cce13-124">toocheck hello status, you can monitor hello progress in hello Notifications hub.</span></span>
   
   ![Data analytics bearbetning jobbet meddelandehubben](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Azure portal dataanalys bearbetar jobbet Skapa jobb](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. <span data-ttu-id="cce13-127">hello nytt jobb visas status för **Skapad**.</span><span class="sxs-lookup"><span data-stu-id="cce13-127">hello new job will show a status of **Created**.</span></span> <span data-ttu-id="cce13-128">Observera att hello **starta** är inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="cce13-128">Notice that hello **Start** button is disabled.</span></span> <span data-ttu-id="cce13-129">Konfigurera hello jobbet indata-, fråge- och utdata innan du startar hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="cce13-129">Configure hello job input, query, and output before you start hello job.</span></span>
   
   ![Databearbetning analytics-jobbet Status](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure portal dataanalys bearbetning jobbstatus](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="cce13-132">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="cce13-132">Get help</span></span>
<span data-ttu-id="cce13-133">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="cce13-133">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="cce13-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cce13-134">Next steps</span></span>
* [<span data-ttu-id="cce13-135">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cce13-135">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="cce13-136">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="cce13-136">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="cce13-137">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="cce13-137">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="cce13-138">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="cce13-138">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="cce13-139">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="cce13-139">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

