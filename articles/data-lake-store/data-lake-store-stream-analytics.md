---
title: "aaaStream data från Stream Analytics i Data Lake Store | Microsoft Docs"
description: "Använda Azure Stream Analytics toostream data i Azure Data Lake Store"
services: data-lake-store,stream-analytics
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 68c727d4807db0abe6efa90145d68c78902eb789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a><span data-ttu-id="32de3-103">Strömma data från Azure Storage Blob till Data Lake Store med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="32de3-103">Stream data from Azure Storage Blob into Data Lake Store using Azure Stream Analytics</span></span>
<span data-ttu-id="32de3-104">I den här artikeln du lära dig hur toouse Azure Data Lake lagrar som utdata för ett Azure Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="32de3-104">In this article you will learn how toouse Azure Data Lake Store as an output for an Azure Stream Analytics job.</span></span> <span data-ttu-id="32de3-105">Den här artikeln visar ett enkelt scenario som läser data från en Azure Storage blob (indata) och skrivningar hello-tooData Datasjölager (utdata).</span><span class="sxs-lookup"><span data-stu-id="32de3-105">This article demonstrates a simple scenario that reads data from an Azure Storage blob (input) and writes hello data tooData Lake Store (output).</span></span>

> [!NOTE]
> <span data-ttu-id="32de3-106">För tillfället skapande och konfiguration av Data Lake Store matar ut för Stream Analytics stöds endast i hello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="32de3-106">At this time, creation and configuration of Data Lake Store outputs for Stream Analytics is supported only in hello [Azure Classic Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="32de3-107">Därför kan använder vissa delar av den här kursen hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="32de3-107">Hence, some parts of this tutorial will use hello Azure Classic Portal.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="32de3-108">Krav</span><span class="sxs-lookup"><span data-stu-id="32de3-108">Prerequisites</span></span>
<span data-ttu-id="32de3-109">Innan du påbörjar den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="32de3-109">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="32de3-110">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="32de3-110">**An Azure subscription**.</span></span> <span data-ttu-id="32de3-111">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="32de3-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="32de3-112">**Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="32de3-112">**Azure Storage account**.</span></span> <span data-ttu-id="32de3-113">Du använder en blob-behållare från detta konto tooinput data för ett Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="32de3-113">You will use a blob container from this account tooinput data for a Stream Analytics job.</span></span> <span data-ttu-id="32de3-114">Den här självstudiekursen förutsätter att du har ett lagringskonto som kallas **storageforasa** och en behållare i hello kontot kallas **storageforasacontainer**.</span><span class="sxs-lookup"><span data-stu-id="32de3-114">For this tutorial, assume you have a storage account called **storageforasa** and a container within hello account called **storageforasacontainer**.</span></span> <span data-ttu-id="32de3-115">När du har skapat hello behållare, ladda upp en exempel data filen tooit.</span><span class="sxs-lookup"><span data-stu-id="32de3-115">Once you have created hello container, upload a sample data file tooit.</span></span> 
  
* <span data-ttu-id="32de3-116">**Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="32de3-116">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="32de3-117">Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="32de3-117">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="32de3-118">Antar vi att du har ett Data Lake Store-konto som kallas **asadatalakestore**.</span><span class="sxs-lookup"><span data-stu-id="32de3-118">Let's assume you have a Data Lake Store account called **asadatalakestore**.</span></span> 

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="32de3-119">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="32de3-119">Create a Stream Analytics Job</span></span>
<span data-ttu-id="32de3-120">Börja med att skapa ett Stream Analytics-jobb som innehåller ingen källa och ett mål för utdata.</span><span class="sxs-lookup"><span data-stu-id="32de3-120">You start by creating a Stream Analytics job that includes an input source and an output destination.</span></span> <span data-ttu-id="32de3-121">I den här självstudien hello källan är en Azure blob-behållaren och hello målet är Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="32de3-121">For this tutorial, hello source is an Azure blob container and hello destination is Data Lake Store.</span></span>

1. <span data-ttu-id="32de3-122">Logga in toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="32de3-122">Sign on toohello [Azure Portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="32de3-123">Hello till vänster och klicka på **Stream Analytics-jobb**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="32de3-123">From hello left pane, click **Stream Analytics jobs**, and then click **Add**.</span></span>

    <span data-ttu-id="32de3-124">![Skapa ett Stream Analytics-jobbet](./media/data-lake-store-stream-analytics/create.job.png "skapa ett Stream Analytics-jobb")</span><span class="sxs-lookup"><span data-stu-id="32de3-124">![Create a Stream Analytics Job](./media/data-lake-store-stream-analytics/create.job.png "Create a Stream Analytics job")</span></span>

    > [!NOTE]
    > <span data-ttu-id="32de3-125">Kontrollera att du skapar jobbet i hello samma region som hello storage-konto eller du påförs extra kostnader för att flytta data mellan regioner.</span><span class="sxs-lookup"><span data-stu-id="32de3-125">Make sure you create job in hello same region as hello storage account or you will incur additional cost of moving data between regions.</span></span>
    >

## <a name="create-a-blob-input-for-hello-job"></a><span data-ttu-id="32de3-126">Skapa en Blob-inmatning för hello jobbet</span><span class="sxs-lookup"><span data-stu-id="32de3-126">Create a Blob input for hello job</span></span>

1. <span data-ttu-id="32de3-127">Öppna hello för hello Stream Analytics-jobbet i hello vänstra rutan klickar du på hello **indata** fliken och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="32de3-127">Open hello page for hello Stream Analytics job, from hello left pane click hello **Inputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="32de3-128">![Lägg till ett jobb för inkommande tooyour](./media/data-lake-store-stream-analytics/create.input.1.png "lägga till ett inkommande tooyour-jobb")</span><span class="sxs-lookup"><span data-stu-id="32de3-128">![Add an input tooyour job](./media/data-lake-store-stream-analytics/create.input.1.png "Add an input tooyour job")</span></span>

2. <span data-ttu-id="32de3-129">På hello **nya indata** bladet ange hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="32de3-129">On hello **New input** blade, provide hello following values.</span></span>

    <span data-ttu-id="32de3-130">![Lägg till ett jobb för inkommande tooyour](./media/data-lake-store-stream-analytics/create.input.2.png "lägga till ett inkommande tooyour-jobb")</span><span class="sxs-lookup"><span data-stu-id="32de3-130">![Add an input tooyour job](./media/data-lake-store-stream-analytics/create.input.2.png "Add an input tooyour job")</span></span>

    * <span data-ttu-id="32de3-131">För **indata alias**, ange ett unikt namn för hello jobbet indata.</span><span class="sxs-lookup"><span data-stu-id="32de3-131">For **Input alias**, enter a unique name for hello job input.</span></span>
    * <span data-ttu-id="32de3-132">För **typ av datakälla**väljer **dataströmmen**.</span><span class="sxs-lookup"><span data-stu-id="32de3-132">For **Source type**, select **Data stream**.</span></span>
    * <span data-ttu-id="32de3-133">För **källa**väljer **Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="32de3-133">For **Source**, select **Blob storage**.</span></span>
    * <span data-ttu-id="32de3-134">För **prenumeration**väljer **använda blob storage från aktuell prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="32de3-134">For **Subscription**, select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="32de3-135">För **lagringskonto**, Välj hello storage-konto som du har skapat som en del av hello krav.</span><span class="sxs-lookup"><span data-stu-id="32de3-135">For **Storage account**, select hello storage account that you created as part of hello prerequisites.</span></span> 
    * <span data-ttu-id="32de3-136">För **behållare**väljer hello-behållare som du skapade i hello valt storage-konto.</span><span class="sxs-lookup"><span data-stu-id="32de3-136">For **Container**, select hello container that you created in hello selected storage account.</span></span>
    * <span data-ttu-id="32de3-137">För **händelse serialiseringsformat**väljer **CSV**.</span><span class="sxs-lookup"><span data-stu-id="32de3-137">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="32de3-138">För **avgränsare**väljer **fliken**.</span><span class="sxs-lookup"><span data-stu-id="32de3-138">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="32de3-139">För **kodning**väljer **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="32de3-139">For **Encoding**, select **UTF-8**.</span></span>

    <span data-ttu-id="32de3-140">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="32de3-140">Click **Create**.</span></span> <span data-ttu-id="32de3-141">hello portal nu lägger till hello indata och testar hello anslutning tooit.</span><span class="sxs-lookup"><span data-stu-id="32de3-141">hello portal now adds hello input and tests hello connection tooit.</span></span>


## <a name="create-a-data-lake-store-output-for-hello-job"></a><span data-ttu-id="32de3-142">Skapa ett Data Lake Store-utdata för hello jobb</span><span class="sxs-lookup"><span data-stu-id="32de3-142">Create a Data Lake Store output for hello job</span></span>

1. <span data-ttu-id="32de3-143">Öppna hello hello Stream Analytics-jobbet på sidan, klicka på hello **utdata** fliken och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="32de3-143">Open hello page for hello Stream Analytics job, click hello **Outputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="32de3-144">![Lägg till ett jobb för utdata tooyour](./media/data-lake-store-stream-analytics/create.output.1.png "lägga till en utgående tooyour jobb")</span><span class="sxs-lookup"><span data-stu-id="32de3-144">![Add an output tooyour job](./media/data-lake-store-stream-analytics/create.output.1.png "Add an output tooyour job")</span></span>

2. <span data-ttu-id="32de3-145">På hello **nya utdata** bladet ange hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="32de3-145">On hello **New output** blade, provide hello following values.</span></span>

    <span data-ttu-id="32de3-146">![Lägg till ett jobb för utdata tooyour](./media/data-lake-store-stream-analytics/create.output.2.png "lägga till en utgående tooyour jobb")</span><span class="sxs-lookup"><span data-stu-id="32de3-146">![Add an output tooyour job](./media/data-lake-store-stream-analytics/create.output.2.png "Add an output tooyour job")</span></span>

    * <span data-ttu-id="32de3-147">För **kolumnalias**, ange ett unikt namn för hello jobbutdata.</span><span class="sxs-lookup"><span data-stu-id="32de3-147">For **Output alias**, enter a a unique name for hello job output.</span></span> <span data-ttu-id="32de3-148">Detta är ett eget namn som används i frågor toodirect hello frågan utdata toothis Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="32de3-148">This is a friendly name used in queries toodirect hello query output toothis Data Lake Store.</span></span>
    * <span data-ttu-id="32de3-149">För **Sink**väljer **Datasjölager**.</span><span class="sxs-lookup"><span data-stu-id="32de3-149">For **Sink**, select **Data Lake Store**.</span></span>
    * <span data-ttu-id="32de3-150">Du kan ange tooauthorize komma åt tooData Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="32de3-150">You will be prompted tooauthorize access tooData Lake Store account.</span></span> <span data-ttu-id="32de3-151">Klicka på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="32de3-151">Click **Authorize**.</span></span>

3. <span data-ttu-id="32de3-152">På hello **nya utdata** bladet fortsätta tooprovide hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="32de3-152">On hello **New output** blade, continue tooprovide hello following values.</span></span>

    <span data-ttu-id="32de3-153">![Lägg till ett jobb för utdata tooyour](./media/data-lake-store-stream-analytics/create.output.3.png "lägga till en utgående tooyour jobb")</span><span class="sxs-lookup"><span data-stu-id="32de3-153">![Add an output tooyour job](./media/data-lake-store-stream-analytics/create.output.3.png "Add an output tooyour job")</span></span>

    * <span data-ttu-id="32de3-154">För **kontonamn**, Välj hello Data Lake Store-konto som du redan skapat där du vill att hello jobbet utdata toobe skickas till.</span><span class="sxs-lookup"><span data-stu-id="32de3-154">For **Account name**, select hello Data Lake Store account you already created where you want hello job output toobe sent to.</span></span>
    * <span data-ttu-id="32de3-155">För **prefix sökvägar**, ange en fil sökvägen toowrite filerna i hello angett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="32de3-155">For **Path prefix pattern**, enter a file path used toowrite your files within hello specified Data Lake Store account.</span></span>
    * <span data-ttu-id="32de3-156">För **datumformat**, om du har använt en datumtoken i hello prefix sökväg, kan du välja hello datumformat där filerna ordnas.</span><span class="sxs-lookup"><span data-stu-id="32de3-156">For **Date format**, if you used a date token in hello prefix path, you can select hello date format in which your files are organized.</span></span>
    * <span data-ttu-id="32de3-157">För **tidsformat**, om du har använt en tid token i hello prefix sökväg, ange hello tidsformat där filerna ordnas.</span><span class="sxs-lookup"><span data-stu-id="32de3-157">For **Time format**, if you used a time token in hello prefix path, specify hello time format in which your files are organized.</span></span>
    * <span data-ttu-id="32de3-158">För **händelse serialiseringsformat**väljer **CSV**.</span><span class="sxs-lookup"><span data-stu-id="32de3-158">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="32de3-159">För **avgränsare**väljer **fliken**.</span><span class="sxs-lookup"><span data-stu-id="32de3-159">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="32de3-160">För **kodning**väljer **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="32de3-160">For **Encoding**, select **UTF-8**.</span></span>
    
    <span data-ttu-id="32de3-161">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="32de3-161">Click **Create**.</span></span> <span data-ttu-id="32de3-162">hello portal nu lägger till hello utdata och testar hello anslutning tooit.</span><span class="sxs-lookup"><span data-stu-id="32de3-162">hello portal now adds hello output and tests hello connection tooit.</span></span>
    
## <a name="run-hello-stream-analytics-job"></a><span data-ttu-id="32de3-163">Kör hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="32de3-163">Run hello Stream Analytics job</span></span>

1. <span data-ttu-id="32de3-164">toorun Stream Analytics-jobbet, måste du köra en fråga från hello **frågan** fliken. Den här självstudiekursen kommer du kan köra hello exempelfråga genom att ersätta hello-platshållare med hello jobbet alias för inkommande och utgående, enligt hello skärmdumpen nedan.</span><span class="sxs-lookup"><span data-stu-id="32de3-164">toorun a Stream Analytics job, you must run a query from hello **Query** tab. For this tutorial, you can run hello sample query by replacing hello placeholders with hello job input and output aliases, as shown in hello screen capture below.</span></span>

    <span data-ttu-id="32de3-165">![Kör frågan](./media/data-lake-store-stream-analytics/run.query.png "kör frågan")</span><span class="sxs-lookup"><span data-stu-id="32de3-165">![Run query](./media/data-lake-store-stream-analytics/run.query.png "Run query")</span></span>

2. <span data-ttu-id="32de3-166">Klicka på **spara** från hello överkant hello-skärmen och sedan från hello **översikt** klickar du på **starta**.</span><span class="sxs-lookup"><span data-stu-id="32de3-166">Click **Save** from hello top of hello screen, and then from hello **Overview** tab, click **Start**.</span></span> <span data-ttu-id="32de3-167">Hello dialogrutan Välj **anpassad tid**, och sedan ange hello aktuellt datum och tid.</span><span class="sxs-lookup"><span data-stu-id="32de3-167">From hello dialog box, select **Custom Time**, and then set hello current date and time.</span></span>

    <span data-ttu-id="32de3-168">![Ange jobbtiden för](./media/data-lake-store-stream-analytics/run.query.2.png "ange jobbtiden för")</span><span class="sxs-lookup"><span data-stu-id="32de3-168">![Set job time](./media/data-lake-store-stream-analytics/run.query.2.png "Set job time")</span></span>

    <span data-ttu-id="32de3-169">Klicka på **starta** toostart hello jobb.</span><span class="sxs-lookup"><span data-stu-id="32de3-169">Click **Start** toostart hello job.</span></span> <span data-ttu-id="32de3-170">Det kan ta upp tooa några minuter toostart hello jobb.</span><span class="sxs-lookup"><span data-stu-id="32de3-170">It can take up tooa couple minutes toostart hello job.</span></span>

3. <span data-ttu-id="32de3-171">tootrigger hello toopick hello jobbdata från hello blob kopiera en exempel data filen toohello blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="32de3-171">tootrigger hello job toopick hello data from hello blob, copy a sample data file toohello blob container.</span></span> <span data-ttu-id="32de3-172">Du kan hämta en exempeldatafil från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span><span class="sxs-lookup"><span data-stu-id="32de3-172">You can get a sample data file from hello [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span></span> <span data-ttu-id="32de3-173">För den här kursen ska vi kopiera hello filen **vehicle1_09142014.csv**.</span><span class="sxs-lookup"><span data-stu-id="32de3-173">For this tutorial, let's copy hello file **vehicle1_09142014.csv**.</span></span> <span data-ttu-id="32de3-174">Du kan använda olika klienter som [Azure Lagringsutforskaren](http://storageexplorer.com/), tooupload data tooa blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="32de3-174">You can use various clients, such as [Azure Storage Explorer](http://storageexplorer.com/), tooupload data tooa blob container.</span></span>

4. <span data-ttu-id="32de3-175">Från hello **översikt** fliken, under **övervakning**, se hur hello data bearbetades.</span><span class="sxs-lookup"><span data-stu-id="32de3-175">From hello **Overview** tab, under **Monitoring**, see how hello data was processed.</span></span>

    <span data-ttu-id="32de3-176">![Övervakningsjobb](./media/data-lake-store-stream-analytics/run.query.3.png "Övervakningsjobb")</span><span class="sxs-lookup"><span data-stu-id="32de3-176">![Monitor job](./media/data-lake-store-stream-analytics/run.query.3.png "Monitor job")</span></span>

5. <span data-ttu-id="32de3-177">Slutligen kan du kontrollera att hello utdata för jobbet är tillgängliga i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="32de3-177">Finally, you can verify that hello job output data is available in hello Data Lake Store account.</span></span> 

    <span data-ttu-id="32de3-178">![Kontrollera utdata](./media/data-lake-store-stream-analytics/run.query.4.png "Kontrollera utdata")</span><span class="sxs-lookup"><span data-stu-id="32de3-178">![Verify output](./media/data-lake-store-stream-analytics/run.query.4.png "Verify output")</span></span>

    <span data-ttu-id="32de3-179">Observera att hello utdata skriftliga tooa sökvägen som anges i hello Data Lake Store utdata inställningar i hello Data Explorer-fönstret (`streamanalytics/job/output/{date}/{time}`).</span><span class="sxs-lookup"><span data-stu-id="32de3-179">In hello Data Explorer pane, notice that hello output is written tooa folder path as specified in hello Data Lake Store output settings (`streamanalytics/job/output/{date}/{time}`).</span></span>  

## <a name="see-also"></a><span data-ttu-id="32de3-180">Se även</span><span class="sxs-lookup"><span data-stu-id="32de3-180">See also</span></span>
* [<span data-ttu-id="32de3-181">Skapa ett HDInsight-kluster toouse Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="32de3-181">Create an HDInsight cluster toouse Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
