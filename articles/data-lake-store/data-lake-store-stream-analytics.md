---
title: "Strömma data från Stream Analytics i Data Lake Store | Microsoft Docs"
description: "Använd Azure Stream Analytics att sända data till Azure Data Lake Store"
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
ms.openlocfilehash: 90104aaacf24a5a7156900fc3848a27f60329814
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a><span data-ttu-id="d0f76-103">Strömma data från Azure Storage Blob till Data Lake Store med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d0f76-103">Stream data from Azure Storage Blob into Data Lake Store using Azure Stream Analytics</span></span>
<span data-ttu-id="d0f76-104">I den här artikeln får du lära dig hur du använder Azure Data Lake Store som utdata för ett Azure Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="d0f76-104">In this article you will learn how to use Azure Data Lake Store as an output for an Azure Stream Analytics job.</span></span> <span data-ttu-id="d0f76-105">Den här artikeln visar ett enkelt scenario som läser data från ett Azure Storage blob (indata) och skriver data till Data Lake Store (utdata).</span><span class="sxs-lookup"><span data-stu-id="d0f76-105">This article demonstrates a simple scenario that reads data from an Azure Storage blob (input) and writes the data to Data Lake Store (output).</span></span>

> [!NOTE]
> <span data-ttu-id="d0f76-106">För tillfället skapande och konfiguration av Data Lake Store matar ut för Stream Analytics stöds bara i den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="d0f76-106">At this time, creation and configuration of Data Lake Store outputs for Stream Analytics is supported only in the [Azure Classic Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="d0f76-107">Därför kan använder vissa delar av den här kursen den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d0f76-107">Hence, some parts of this tutorial will use the Azure Classic Portal.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="d0f76-108">Krav</span><span class="sxs-lookup"><span data-stu-id="d0f76-108">Prerequisites</span></span>
<span data-ttu-id="d0f76-109">Innan du påbörjar de här självstudierna måste du ha:</span><span class="sxs-lookup"><span data-stu-id="d0f76-109">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="d0f76-110">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-110">**An Azure subscription**.</span></span> <span data-ttu-id="d0f76-111">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d0f76-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="d0f76-112">**Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-112">**Azure Storage account**.</span></span> <span data-ttu-id="d0f76-113">Du använder en blob-behållare från detta konto för att mata in data för ett Stream Analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="d0f76-113">You will use a blob container from this account to input data for a Stream Analytics job.</span></span> <span data-ttu-id="d0f76-114">Den här självstudiekursen förutsätter att du har ett lagringskonto som kallas **storageforasa** och en behållare i kontot kallas **storageforasacontainer**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-114">For this tutorial, assume you have a storage account called **storageforasa** and a container within the account called **storageforasacontainer**.</span></span> <span data-ttu-id="d0f76-115">När du har skapat behållaren måste du ladda upp en exempeldatafil till den.</span><span class="sxs-lookup"><span data-stu-id="d0f76-115">Once you have created the container, upload a sample data file to it.</span></span> 
  
* <span data-ttu-id="d0f76-116">**Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-116">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="d0f76-117">Följ instruktionerna i [Kom igång med Azure Data Lake Store med hjälp av Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d0f76-117">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="d0f76-118">Antar vi att du har ett Data Lake Store-konto som kallas **asadatalakestore**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-118">Let's assume you have a Data Lake Store account called **asadatalakestore**.</span></span> 

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="d0f76-119">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="d0f76-119">Create a Stream Analytics Job</span></span>
<span data-ttu-id="d0f76-120">Börja med att skapa ett Stream Analytics-jobb som innehåller ingen källa och ett mål för utdata.</span><span class="sxs-lookup"><span data-stu-id="d0f76-120">You start by creating a Stream Analytics job that includes an input source and an output destination.</span></span> <span data-ttu-id="d0f76-121">I den här självstudien källan är en Azure blob-behållaren och målet är Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="d0f76-121">For this tutorial, the source is an Azure blob container and the destination is Data Lake Store.</span></span>

1. <span data-ttu-id="d0f76-122">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d0f76-122">Sign on to the [Azure Portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="d0f76-123">I den vänstra rutan klickar du på **Stream Analytics-jobb**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-123">From the left pane, click **Stream Analytics jobs**, and then click **Add**.</span></span>

    <span data-ttu-id="d0f76-124">![Skapa ett Stream Analytics-jobbet](./media/data-lake-store-stream-analytics/create.job.png "skapa ett Stream Analytics-jobb")</span><span class="sxs-lookup"><span data-stu-id="d0f76-124">![Create a Stream Analytics Job](./media/data-lake-store-stream-analytics/create.job.png "Create a Stream Analytics job")</span></span>

    > [!NOTE]
    > <span data-ttu-id="d0f76-125">Kontrollera att du skapar jobbet i samma region som lagringskontot eller du kan innebära ytterligare kostnader för att flytta data mellan regioner.</span><span class="sxs-lookup"><span data-stu-id="d0f76-125">Make sure you create job in the same region as the storage account or you will incur additional cost of moving data between regions.</span></span>
    >

## <a name="create-a-blob-input-for-the-job"></a><span data-ttu-id="d0f76-126">Skapa en Blob-inmatning för jobbet</span><span class="sxs-lookup"><span data-stu-id="d0f76-126">Create a Blob input for the job</span></span>

1. <span data-ttu-id="d0f76-127">Öppna den för Stream Analytics-jobbet från den vänstra rutan klickar du på den **indata** fliken och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-127">Open the page for the Stream Analytics job, from the left pane click the **Inputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="d0f76-128">![Lägga till indata till jobbet](./media/data-lake-store-stream-analytics/create.input.1.png "lägga till indata till dina jobb")</span><span class="sxs-lookup"><span data-stu-id="d0f76-128">![Add an input to your job](./media/data-lake-store-stream-analytics/create.input.1.png "Add an input to your job")</span></span>

2. <span data-ttu-id="d0f76-129">På den **nya indata** bladet, ange följande värden.</span><span class="sxs-lookup"><span data-stu-id="d0f76-129">On the **New input** blade, provide the following values.</span></span>

    <span data-ttu-id="d0f76-130">![Lägga till indata till jobbet](./media/data-lake-store-stream-analytics/create.input.2.png "lägga till indata till dina jobb")</span><span class="sxs-lookup"><span data-stu-id="d0f76-130">![Add an input to your job](./media/data-lake-store-stream-analytics/create.input.2.png "Add an input to your job")</span></span>

    * <span data-ttu-id="d0f76-131">För **indata alias**, ange ett unikt namn för det jobb som indata.</span><span class="sxs-lookup"><span data-stu-id="d0f76-131">For **Input alias**, enter a unique name for the job input.</span></span>
    * <span data-ttu-id="d0f76-132">För **typ av datakälla**väljer **dataströmmen**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-132">For **Source type**, select **Data stream**.</span></span>
    * <span data-ttu-id="d0f76-133">För **källa**väljer **Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-133">For **Source**, select **Blob storage**.</span></span>
    * <span data-ttu-id="d0f76-134">För **prenumeration**väljer **använda blob storage från aktuell prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-134">For **Subscription**, select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="d0f76-135">För **lagringskonto**, Välj lagringskonto som du har skapat som en del av förutsättningarna.</span><span class="sxs-lookup"><span data-stu-id="d0f76-135">For **Storage account**, select the storage account that you created as part of the prerequisites.</span></span> 
    * <span data-ttu-id="d0f76-136">För **behållare**, markera den behållare som du skapade i det valda lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="d0f76-136">For **Container**, select the container that you created in the selected storage account.</span></span>
    * <span data-ttu-id="d0f76-137">För **händelse serialiseringsformat**väljer **CSV**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-137">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="d0f76-138">För **avgränsare**väljer **fliken**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-138">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="d0f76-139">För **kodning**väljer **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-139">For **Encoding**, select **UTF-8**.</span></span>

    <span data-ttu-id="d0f76-140">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-140">Click **Create**.</span></span> <span data-ttu-id="d0f76-141">Portalen nu lägger till indata och testar anslutningen till den.</span><span class="sxs-lookup"><span data-stu-id="d0f76-141">The portal now adds the input and tests the connection to it.</span></span>


## <a name="create-a-data-lake-store-output-for-the-job"></a><span data-ttu-id="d0f76-142">Skapa ett Data Lake Store-utdata för jobbet</span><span class="sxs-lookup"><span data-stu-id="d0f76-142">Create a Data Lake Store output for the job</span></span>

1. <span data-ttu-id="d0f76-143">Öppna sidan för Stream Analytics-jobb, klicka på den **utdata** fliken och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-143">Open the page for the Stream Analytics job, click the **Outputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="d0f76-144">![Lägga till utdata till jobbet](./media/data-lake-store-stream-analytics/create.output.1.png "lägga till utdata till dina jobb")</span><span class="sxs-lookup"><span data-stu-id="d0f76-144">![Add an output to your job](./media/data-lake-store-stream-analytics/create.output.1.png "Add an output to your job")</span></span>

2. <span data-ttu-id="d0f76-145">På den **nya utdata** bladet, ange följande värden.</span><span class="sxs-lookup"><span data-stu-id="d0f76-145">On the **New output** blade, provide the following values.</span></span>

    <span data-ttu-id="d0f76-146">![Lägga till utdata till jobbet](./media/data-lake-store-stream-analytics/create.output.2.png "lägga till utdata till dina jobb")</span><span class="sxs-lookup"><span data-stu-id="d0f76-146">![Add an output to your job](./media/data-lake-store-stream-analytics/create.output.2.png "Add an output to your job")</span></span>

    * <span data-ttu-id="d0f76-147">För **kolumnalias**, ange ett unikt namn utdata för jobbet.</span><span class="sxs-lookup"><span data-stu-id="d0f76-147">For **Output alias**, enter a a unique name for the job output.</span></span> <span data-ttu-id="d0f76-148">Detta är ett eget namn som används i frågor för att dirigera utdata till denna Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="d0f76-148">This is a friendly name used in queries to direct the query output to this Data Lake Store.</span></span>
    * <span data-ttu-id="d0f76-149">För **Sink**väljer **Datasjölager**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-149">For **Sink**, select **Data Lake Store**.</span></span>
    * <span data-ttu-id="d0f76-150">Du uppmanas att godkänna åtkomst till Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="d0f76-150">You will be prompted to authorize access to Data Lake Store account.</span></span> <span data-ttu-id="d0f76-151">Klicka på **auktorisera**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-151">Click **Authorize**.</span></span>

3. <span data-ttu-id="d0f76-152">På den **nya utdata** bladet fortsätta att tillhandahålla följande värden.</span><span class="sxs-lookup"><span data-stu-id="d0f76-152">On the **New output** blade, continue to provide the following values.</span></span>

    <span data-ttu-id="d0f76-153">![Lägga till utdata till jobbet](./media/data-lake-store-stream-analytics/create.output.3.png "lägga till utdata till dina jobb")</span><span class="sxs-lookup"><span data-stu-id="d0f76-153">![Add an output to your job](./media/data-lake-store-stream-analytics/create.output.3.png "Add an output to your job")</span></span>

    * <span data-ttu-id="d0f76-154">För **kontonamn**, Välj Data Lake Store-konto som du redan skapat där du vill att jobbet utdata skickas till.</span><span class="sxs-lookup"><span data-stu-id="d0f76-154">For **Account name**, select the Data Lake Store account you already created where you want the job output to be sent to.</span></span>
    * <span data-ttu-id="d0f76-155">För **prefix sökvägar**, ange en filsökväg som används för att skriva filer inom det angivna Data Lake Store-kontot.</span><span class="sxs-lookup"><span data-stu-id="d0f76-155">For **Path prefix pattern**, enter a file path used to write your files within the specified Data Lake Store account.</span></span>
    * <span data-ttu-id="d0f76-156">För **datumformat**, om du har använt en datumtoken i sökvägen prefix, kan du välja datumformat där filerna ordnas.</span><span class="sxs-lookup"><span data-stu-id="d0f76-156">For **Date format**, if you used a date token in the prefix path, you can select the date format in which your files are organized.</span></span>
    * <span data-ttu-id="d0f76-157">För **tidsformat**, om du har använt en tid token i prefix-sökvägen, ange tidsformat där filerna ordnas.</span><span class="sxs-lookup"><span data-stu-id="d0f76-157">For **Time format**, if you used a time token in the prefix path, specify the time format in which your files are organized.</span></span>
    * <span data-ttu-id="d0f76-158">För **händelse serialiseringsformat**väljer **CSV**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-158">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="d0f76-159">För **avgränsare**väljer **fliken**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-159">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="d0f76-160">För **kodning**väljer **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-160">For **Encoding**, select **UTF-8**.</span></span>
    
    <span data-ttu-id="d0f76-161">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-161">Click **Create**.</span></span> <span data-ttu-id="d0f76-162">Portalen nu lägger till utdata och testar anslutningen till den.</span><span class="sxs-lookup"><span data-stu-id="d0f76-162">The portal now adds the output and tests the connection to it.</span></span>
    
## <a name="run-the-stream-analytics-job"></a><span data-ttu-id="d0f76-163">Kör Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="d0f76-163">Run the Stream Analytics job</span></span>

1. <span data-ttu-id="d0f76-164">Om du vill köra ett Stream Analytics-jobb, måste du köra en fråga från den **frågan** fliken.</span><span class="sxs-lookup"><span data-stu-id="d0f76-164">To run a Stream Analytics job, you must run a query from the **Query** tab.</span></span> <span data-ttu-id="d0f76-165">För den här självstudiekursen, du kan köra exempelfråga genom att ersätta platshållarna med jobbet indata och utdata alias, som visas i skärmdumpen nedan.</span><span class="sxs-lookup"><span data-stu-id="d0f76-165">For this tutorial, you can run the sample query by replacing the placeholders with the job input and output aliases, as shown in the screen capture below.</span></span>

    <span data-ttu-id="d0f76-166">![Kör frågan](./media/data-lake-store-stream-analytics/run.query.png "kör frågan")</span><span class="sxs-lookup"><span data-stu-id="d0f76-166">![Run query](./media/data-lake-store-stream-analytics/run.query.png "Run query")</span></span>

2. <span data-ttu-id="d0f76-167">Klicka på **spara** högst upp på skärmen och sedan från den **översikt** klickar du på **starta**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-167">Click **Save** from the top of the screen, and then from the **Overview** tab, click **Start**.</span></span> <span data-ttu-id="d0f76-168">Dialogrutan Välj **anpassad tid**, och sedan ange aktuellt datum och tid.</span><span class="sxs-lookup"><span data-stu-id="d0f76-168">From the dialog box, select **Custom Time**, and then set the current date and time.</span></span>

    <span data-ttu-id="d0f76-169">![Ange jobbtiden för](./media/data-lake-store-stream-analytics/run.query.2.png "ange jobbtiden för")</span><span class="sxs-lookup"><span data-stu-id="d0f76-169">![Set job time](./media/data-lake-store-stream-analytics/run.query.2.png "Set job time")</span></span>

    <span data-ttu-id="d0f76-170">Klicka på **starta** att starta jobbet.</span><span class="sxs-lookup"><span data-stu-id="d0f76-170">Click **Start** to start the job.</span></span> <span data-ttu-id="d0f76-171">Det kan ta ett par minuter att starta jobbet.</span><span class="sxs-lookup"><span data-stu-id="d0f76-171">It can take up to a couple minutes to start the job.</span></span>

3. <span data-ttu-id="d0f76-172">Kopiera en exempeldatafil till blob-behållaren för att starta jobbet för att hämta data från blob.</span><span class="sxs-lookup"><span data-stu-id="d0f76-172">To trigger the job to pick the data from the blob, copy a sample data file to the blob container.</span></span> <span data-ttu-id="d0f76-173">Du kan få en exempeldatafil från den [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span><span class="sxs-lookup"><span data-stu-id="d0f76-173">You can get a sample data file from the [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span></span> <span data-ttu-id="d0f76-174">Den här självstudiekursen kommer vi kopiera filen **vehicle1_09142014.csv**.</span><span class="sxs-lookup"><span data-stu-id="d0f76-174">For this tutorial, let's copy the file **vehicle1_09142014.csv**.</span></span> <span data-ttu-id="d0f76-175">Du kan använda olika klienter som [Azure Lagringsutforskaren](http://storageexplorer.com/), för att överföra data till en blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="d0f76-175">You can use various clients, such as [Azure Storage Explorer](http://storageexplorer.com/), to upload data to a blob container.</span></span>

4. <span data-ttu-id="d0f76-176">Från den **översikt** fliken, under **övervakning**, se hur data bearbetades.</span><span class="sxs-lookup"><span data-stu-id="d0f76-176">From the **Overview** tab, under **Monitoring**, see how the data was processed.</span></span>

    <span data-ttu-id="d0f76-177">![Övervakningsjobb](./media/data-lake-store-stream-analytics/run.query.3.png "Övervakningsjobb")</span><span class="sxs-lookup"><span data-stu-id="d0f76-177">![Monitor job](./media/data-lake-store-stream-analytics/run.query.3.png "Monitor job")</span></span>

5. <span data-ttu-id="d0f76-178">Slutligen kan du kontrollera att utdata för jobbet är tillgänglig i Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="d0f76-178">Finally, you can verify that the job output data is available in the Data Lake Store account.</span></span> 

    <span data-ttu-id="d0f76-179">![Kontrollera utdata](./media/data-lake-store-stream-analytics/run.query.4.png "Kontrollera utdata")</span><span class="sxs-lookup"><span data-stu-id="d0f76-179">![Verify output](./media/data-lake-store-stream-analytics/run.query.4.png "Verify output")</span></span>

    <span data-ttu-id="d0f76-180">I fönstret Data Explorer utgående meddelande som utdata skrivs till en sökväg som anges i Data Lake Store inställningar (`streamanalytics/job/output/{date}/{time}`).</span><span class="sxs-lookup"><span data-stu-id="d0f76-180">In the Data Explorer pane, notice that the output is written to a folder path as specified in the Data Lake Store output settings (`streamanalytics/job/output/{date}/{time}`).</span></span>  

## <a name="see-also"></a><span data-ttu-id="d0f76-181">Se även</span><span class="sxs-lookup"><span data-stu-id="d0f76-181">See also</span></span>
* [<span data-ttu-id="d0f76-182">Skapa ett HDInsight-kluster om du vill använda Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="d0f76-182">Create an HDInsight cluster to use Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
