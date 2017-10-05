---
title: "Skapa ett data analytics-jobb för bearbetning för Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 05fdf1e20efd129cdfc27e1d37bc9e124edf5dcd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a><span data-ttu-id="f8037-104">Skapa ett data analytics-jobb för bearbetning för Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f8037-104">How to create a data analytics processing job for Stream Analytics</span></span>
<span data-ttu-id="f8037-105">Resursen på den översta nivån i Azure Stream Analytics är en Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="f8037-105">The top-level resource in Azure Stream Analytics is a Stream Analytics Job.</span></span>  <span data-ttu-id="f8037-106">Det består av en eller flera inkommande datakällor, en fråga uttrycka data transformation och ett eller flera mål för utdata som resultat har skrivits till.</span><span class="sxs-lookup"><span data-stu-id="f8037-106">It consists of one or more input data sources, a query expressing the data transformation, and one or more output targets that results are written to.</span></span> <span data-ttu-id="f8037-107">Tillsammans gör dessa att användaren kan utföra dataanalys bearbetning för strömmande datascenarier.</span><span class="sxs-lookup"><span data-stu-id="f8037-107">Together these enable the user to perform data analytics processing for streaming data scenarios.</span></span>

<span data-ttu-id="f8037-108">Om du vill börja använda Stream Analytics, börjar du med att skapa ett nytt Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="f8037-108">To start using Stream Analytics, begin by creating a new Stream Analytics job.</span></span>  <span data-ttu-id="f8037-109">Observera att den här åtgärden har ingen fakturering effekter tills jobbet har startats.</span><span class="sxs-lookup"><span data-stu-id="f8037-109">Note this action has no billing implications until the job is started.</span></span>

1. <span data-ttu-id="f8037-110">Logga in på nätet [klassiska Azure-portalen](http://manage.windowsazure.com) eller [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f8037-110">Sign in on the online [Azure classic portal](http://manage.windowsazure.com) or the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="f8037-111">I portalen: **klickar du på ny**, klicka på **datatjänster** eller **dataanalys** beroende på din portal och klicka sedan på **Azure Stream Analytics** eller **Stream Analytics** och sedan **Snabbregistrering**.</span><span class="sxs-lookup"><span data-stu-id="f8037-111">In the portal: **Click New**, then click **Data Services** or **Data Analytics** depending on your portal and then click **Azure Stream Analytics** or **Stream Analytics** and then **Quick Create**.</span></span>
   
   ![Guiden analytics databearbetning](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Skapa dataanalys bearbetning jobbet](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. <span data-ttu-id="f8037-114">Ange önskad konfiguration för Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="f8037-114">Specify the desired configuration for the Stream Analytics job.</span></span>
   
   * <span data-ttu-id="f8037-115">I den **jobbnamn** anger du ett namn som identifierar Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="f8037-115">In the **Job Name** box, enter a name to identify the Stream Analytics job.</span></span> <span data-ttu-id="f8037-116">När den **jobbnamn** har verifierats, visas en grön kryssmarkering i jobbnamn.</span><span class="sxs-lookup"><span data-stu-id="f8037-116">When the **Job Name** is validated, a green check mark appears in the Job Name box.</span></span> <span data-ttu-id="f8037-117">Den **jobbnamn** får bara innehålla alfanumeriska tecken och '-' tecken och måste vara mellan 3 och 63 tecken.</span><span class="sxs-lookup"><span data-stu-id="f8037-117">The **Job Name** may contain only alphanumeric characters and the '-' character, and must be between 3 and 63 characters.</span></span>
   * <span data-ttu-id="f8037-118">Använd **Region** i Azure-portalen eller **plats** i Azure portal för att ange den geografiska plats där du vill köra jobbet.</span><span class="sxs-lookup"><span data-stu-id="f8037-118">Use **Region** in the Azure portal or **Location** in the Azure portal to specify the geographic location where you want to run the job.</span></span>
   * <span data-ttu-id="f8037-119">Om du använder Azure portal, Välj eller skapa ett lagringskonto som ska användas som den **Lagringskonto för Regional övervakning**.</span><span class="sxs-lookup"><span data-stu-id="f8037-119">If using the Azure portal, select or create a storage account to use as the **Regional Monitoring Storage Account**.</span></span> <span data-ttu-id="f8037-120">Det här lagringskontot används för att lagra övervakningsdata för alla Stream Analytics-jobb som körs i den här regionen.</span><span class="sxs-lookup"><span data-stu-id="f8037-120">This storage account is used to store monitoring data for all Stream Analytics jobs running in this region.</span></span>
   * <span data-ttu-id="f8037-121">Om använder Azure-portalen, ange en ny eller befintlig **resursgruppen** för relaterade resurser för ditt program.</span><span class="sxs-lookup"><span data-stu-id="f8037-121">If using the Azure portal, specify a new or existing **Resource Group** to hold related resources for your application.</span></span>
4. <span data-ttu-id="f8037-122">När de nya alternativ för Stream Analytics-jobbet har konfigurerats, klickar du på **skapa Stream Analytics-jobbet**.</span><span class="sxs-lookup"><span data-stu-id="f8037-122">Once the new Stream Analytics job options are configured, click **Create Stream Analytics Job**.</span></span> <span data-ttu-id="f8037-123">Det kan ta några minuter för Stream Analytics-jobbet ska skapas.</span><span class="sxs-lookup"><span data-stu-id="f8037-123">It can take a few minutes for the Stream Analytics job to be created.</span></span> <span data-ttu-id="f8037-124">Du kan övervaka förloppet på meddelandehubben för att kontrollera status.</span><span class="sxs-lookup"><span data-stu-id="f8037-124">To check the status, you can monitor the progress in the Notifications hub.</span></span>
   
   ![Data analytics bearbetning jobbet meddelandehubben](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Azure portal dataanalys bearbetar jobbet Skapa jobb](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. <span data-ttu-id="f8037-127">Nytt jobb visas status för **Skapad**.</span><span class="sxs-lookup"><span data-stu-id="f8037-127">The new job will show a status of **Created**.</span></span> <span data-ttu-id="f8037-128">Observera att den **starta** är inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="f8037-128">Notice that the **Start** button is disabled.</span></span> <span data-ttu-id="f8037-129">Konfigurera jobbet indata-, fråge- och utdata innan du startar jobbet.</span><span class="sxs-lookup"><span data-stu-id="f8037-129">Configure the job input, query, and output before you start the job.</span></span>
   
   ![Databearbetning analytics-jobbet Status](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure portal dataanalys bearbetning jobbstatus](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="f8037-132">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="f8037-132">Get help</span></span>
<span data-ttu-id="f8037-133">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="f8037-133">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8037-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8037-134">Next steps</span></span>
* [<span data-ttu-id="f8037-135">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f8037-135">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="f8037-136">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f8037-136">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="f8037-137">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="f8037-137">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="f8037-138">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="f8037-138">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="f8037-139">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="f8037-139">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

