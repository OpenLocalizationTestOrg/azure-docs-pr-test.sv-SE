---
title: "Use custom activities in an Azure Data Factory pipeline (Använda anpassade aktiviteter i en Azure Data Factory-pipeline)"
description: "Lär dig hur du skapar anpassade aktiviteter och använda dem i ett Azure Data Factory-pipelinen."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: f3d265f31cb653d32076747e586383d67bbccc41
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a><span data-ttu-id="c11fd-103">Use custom activities in an Azure Data Factory pipeline (Använda anpassade aktiviteter i en Azure Data Factory-pipeline)</span><span class="sxs-lookup"><span data-stu-id="c11fd-103">Use custom activities in an Azure Data Factory pipeline</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="c11fd-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="c11fd-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="c11fd-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="c11fd-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="c11fd-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="c11fd-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="c11fd-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="c11fd-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="c11fd-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="c11fd-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="c11fd-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="c11fd-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="c11fd-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="c11fd-114">Det finns två typer av aktiviteter som du kan använda i ett Azure Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-114">There are two types of activities that you can use in an Azure Data Factory pipeline.</span></span>

- <span data-ttu-id="c11fd-115">[Data Movement aktiviteter](data-factory-data-movement-activities.md) att flytta data mellan [stöds källa och mottagare datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="c11fd-115">[Data Movement Activities](data-factory-data-movement-activities.md) to move data between [supported source and sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>
- <span data-ttu-id="c11fd-116">[Data Transformation aktiviteter](data-factory-data-transformation-activities.md) för att omvandla data med hjälp av compute-tjänster som Azure HDInsight, Azure Batch och Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c11fd-116">[Data Transformation Activities](data-factory-data-transformation-activities.md) to transform data using compute services such as Azure HDInsight, Azure Batch, and Azure Machine Learning.</span></span> 

<span data-ttu-id="c11fd-117">Om du vill flytta data till/från ett dataarkiv som Data Factory inte har stöd för, skapa en **anpassad aktivitet** med dina egna data movement logik och Använd aktiviteten i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="c11fd-117">To move data to/from a data store that Data Factory does not support, create a **custom activity** with your own data movement logic and use the activity in a pipeline.</span></span> <span data-ttu-id="c11fd-118">På samma sätt för att transformera/bearbeta data på ett sätt som inte stöds av Data Factory, skapa en anpassad aktivitet med dina egna data transformation logik och använda aktiviteten i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="c11fd-118">Similarly, to transform/process data in a way that isn't supported by Data Factory, create a custom activity with your own data transformation logic and use the activity in a pipeline.</span></span> 

<span data-ttu-id="c11fd-119">Du kan konfigurera en anpassad aktivitet ska köras på en **Azure Batch** poolen med virtuella datorer eller en Windows-baserad **Azure HDInsight** klustret.</span><span class="sxs-lookup"><span data-stu-id="c11fd-119">You can configure a custom activity to run on an **Azure Batch** pool of virtual machines or a Windows-based **Azure HDInsight** cluster.</span></span> <span data-ttu-id="c11fd-120">När du använder Azure Batch kan använda du endast en befintlig Azure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="c11fd-120">When using Azure Batch, you can use only an existing Azure Batch pool.</span></span> <span data-ttu-id="c11fd-121">När du använder HDInsight kan du använda ett befintligt HDInsight-kluster eller ett kluster som skapas automatiskt för du på-begäran vid körning.</span><span class="sxs-lookup"><span data-stu-id="c11fd-121">Whereas, when using HDInsight, you can use an existing HDInsight cluster or a cluster that is automatically created for you on-demand at runtime.</span></span>  

<span data-ttu-id="c11fd-122">Den här genomgången innehåller stegvisa instruktioner för att skapa en anpassad .NET-aktivitet och använda den anpassade aktiviteten i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="c11fd-122">The following walkthrough provides step-by-step instructions for creating a custom .NET activity and using the custom activity in a pipeline.</span></span> <span data-ttu-id="c11fd-123">Den här genomgången använder en **Azure Batch** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-123">The walkthrough uses an **Azure Batch** linked service.</span></span> <span data-ttu-id="c11fd-124">Att använda en Azure HDInsight länkade tjänsten i stället skapar du en länkad tjänst av typen **HDInsight** (egna HDInsight-kluster) eller **HDInsightOnDemand** (Data Factory skapar ett HDInsight-kluster på begäran).</span><span class="sxs-lookup"><span data-stu-id="c11fd-124">To use an Azure HDInsight linked service instead, you create a linked service of type **HDInsight** (your own HDInsight cluster) or **HDInsightOnDemand** (Data Factory creates an HDInsight cluster on-demand).</span></span> <span data-ttu-id="c11fd-125">Konfigurera sedan anpassad aktivitet om du vill använda HDInsight länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-125">Then, configure custom activity to use the HDInsight linked service.</span></span> <span data-ttu-id="c11fd-126">Se [Använd Azure HDInsight länkade tjänster](#use-hdinsight-compute-service) information om hur du använder Azure HDInsight för att köra den anpassade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-126">See [Use Azure HDInsight linked services](#use-hdinsight-compute-service) section for details on using Azure HDInsight to run the custom activity.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="c11fd-127">Anpassad .NET-aktiviteter körs bara på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-127">The custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="c11fd-128">En lösning för den här begränsningen är att använda aktiviteten mappa minska för att köra anpassad Java-kod på en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-128">A workaround for this limitation is to use the Map Reduce Activity to run custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="c11fd-129">Ett annat alternativ är att använda en Azure Batch-pool för virtuella datorer för att köra anpassade aktiviteter i stället för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-129">Another option is to use an Azure Batch pool of VMs to run custom activities instead of using a HDInsight cluster.</span></span>
> - <span data-ttu-id="c11fd-130">Det går inte att använda en Data Management Gateway från en anpassad aktivitet för att få åtkomst till lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="c11fd-130">It is not possible to use a Data Management Gateway from a custom activity to access on-premises data sources.</span></span> <span data-ttu-id="c11fd-131">För närvarande [Data Management Gateway](data-factory-data-management-gateway.md) stöder endast kopieringsaktiviteten och aktiviteten lagrad procedur i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c11fd-131">Currently, [Data Management Gateway](data-factory-data-management-gateway.md) supports only the copy activity and stored procedure activity in Data Factory.</span></span>   

## <a name="walkthrough-create-a-custom-activity"></a><span data-ttu-id="c11fd-132">Genomgång: skapa en anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-132">Walkthrough: create a custom activity</span></span>
### <a name="prerequisites"></a><span data-ttu-id="c11fd-133">Krav</span><span class="sxs-lookup"><span data-stu-id="c11fd-133">Prerequisites</span></span>
* <span data-ttu-id="c11fd-134">Visual Studio 2012/2013/2015</span><span class="sxs-lookup"><span data-stu-id="c11fd-134">Visual Studio 2012/2013/2015</span></span>
* <span data-ttu-id="c11fd-135">Ladda ned och installera [Azure .NET SDK](https://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="c11fd-135">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/)</span></span>

### <a name="azure-batch-prerequisites"></a><span data-ttu-id="c11fd-136">Krav för Azure Batch</span><span class="sxs-lookup"><span data-stu-id="c11fd-136">Azure Batch prerequisites</span></span>
<span data-ttu-id="c11fd-137">I den här genomgången kan du köra dina anpassade .NET-aktiviteter med hjälp av Azure Batch som en resurs för beräkning.</span><span class="sxs-lookup"><span data-stu-id="c11fd-137">In the walkthrough, you run your custom .NET activities using Azure Batch as a compute resource.</span></span> <span data-ttu-id="c11fd-138">**Azure Batch** är en plattform som tjänsten för att köra storskaliga parallellt och högpresterande datorbearbetning (HPC) program effektivt i molnet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-138">**Azure Batch** is a platform service for running large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="c11fd-139">Azure Batch schemalägger beräkningsintensiva arbete ska köras på en hanterad **samling av virtuella datorer**, och kan automatiskt skala beräkningsresurser för att möta behoven hos dina jobb.</span><span class="sxs-lookup"><span data-stu-id="c11fd-139">Azure Batch schedules compute-intensive work to run on a managed **collection of virtual machines**, and can automatically scale compute resources to meet the needs of your jobs.</span></span> <span data-ttu-id="c11fd-140">Se [grunderna i Azure Batch] [ batch-technical-overview] artikel en detaljerad översikt över Azure Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-140">See [Azure Batch basics][batch-technical-overview] article for a detailed overview of the Azure Batch service.</span></span>

<span data-ttu-id="c11fd-141">Skapa ett Azure Batch-konto med en pool för virtuella datorer för självstudierna.</span><span class="sxs-lookup"><span data-stu-id="c11fd-141">For the tutorial, create an Azure Batch account with a pool of VMs.</span></span> <span data-ttu-id="c11fd-142">Här är stegen:</span><span class="sxs-lookup"><span data-stu-id="c11fd-142">Here are the steps:</span></span>

1. <span data-ttu-id="c11fd-143">Skapa en **Azure Batch-kontot** med hjälp av den [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c11fd-143">Create an **Azure Batch account** using the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="c11fd-144">Se [skapa och hantera Azure Batch-kontot] [ batch-create-account] artikel anvisningar.</span><span class="sxs-lookup"><span data-stu-id="c11fd-144">See [Create and manage an Azure Batch account][batch-create-account] article for instructions.</span></span>
2. <span data-ttu-id="c11fd-145">Notera Azure Batch-kontonamn, kontonyckel, URI: N och namnet på programpoolen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-145">Note down the Azure Batch account name, account key, URI, and pool name.</span></span> <span data-ttu-id="c11fd-146">Du behöver dem för att skapa en Azure Batch länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="c11fd-146">You need them to create an Azure Batch linked service.</span></span>
    1. <span data-ttu-id="c11fd-147">På startsidan för Azure Batch-kontot som du ser en **URL** i följande format: `https://myaccount.westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-147">On the home page for Azure Batch account, you see a **URL** in the following format: `https://myaccount.westus.batch.azure.com`.</span></span> <span data-ttu-id="c11fd-148">I det här exemplet **MITTKONTO** är namnet på Azure Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="c11fd-148">In this example, **myaccount** is the name of the Azure Batch account.</span></span> <span data-ttu-id="c11fd-149">URI som du använder i länkade tjänstdefinitionen är URL: en utan att namnet på kontot.</span><span class="sxs-lookup"><span data-stu-id="c11fd-149">URI you use in the linked service definition is the URL without the name of the account.</span></span> <span data-ttu-id="c11fd-150">Till exempel: `https://<region>.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-150">For example: `https://<region>.batch.azure.com`.</span></span>
    2. <span data-ttu-id="c11fd-151">Klicka på **nycklar** på den vänstra menyn och kopiera den **primära ÅTKOMSTNYCKELN**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-151">Click **Keys** on the left menu, and copy the **PRIMARY ACCESS KEY**.</span></span>
    3. <span data-ttu-id="c11fd-152">Om du vill använda en befintlig adresspool **pooler** på menyn och Skriv ner den **ID** för poolen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-152">To use an existing pool, click **Pools** on the menu, and note down the **ID** of the pool.</span></span> <span data-ttu-id="c11fd-153">Om du inte har en befintlig adresspool går vidare till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="c11fd-153">If you don't have an existing pool, move to the next step.</span></span>     
2. <span data-ttu-id="c11fd-154">Skapa en **Azure Batch-pool**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-154">Create an **Azure Batch pool**.</span></span>

   1. <span data-ttu-id="c11fd-155">I den [Azure-portalen](https://portal.azure.com), klickar du på **Bläddra** i den vänstra menyn och klicka på **Batch-konton**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-155">In the [Azure portal](https://portal.azure.com), click **Browse** in the left menu, and click **Batch Accounts**.</span></span>
   2. <span data-ttu-id="c11fd-156">Välj Azure Batch-konto för att öppna den **Batch-kontot** bladet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-156">Select your Azure Batch account to open the **Batch Account** blade.</span></span>
   3. <span data-ttu-id="c11fd-157">Klicka på **pooler** panelen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-157">Click **Pools** tile.</span></span>
   4. <span data-ttu-id="c11fd-158">I den **pooler** bladet, klickar du på knappen Lägg till i verktygsfältet för att lägga till en pool.</span><span class="sxs-lookup"><span data-stu-id="c11fd-158">In the **Pools** blade, click Add button on the toolbar to add a pool.</span></span>
      1. <span data-ttu-id="c11fd-159">Ange ett ID för poolen (Pool-ID).</span><span class="sxs-lookup"><span data-stu-id="c11fd-159">Enter an ID for the pool (Pool ID).</span></span> <span data-ttu-id="c11fd-160">Observera den **ID för poolen**; du behöver den när du skapar Data Factory-lösning.</span><span class="sxs-lookup"><span data-stu-id="c11fd-160">Note the **ID of the pool**; you need it when creating the Data Factory solution.</span></span>
      2. <span data-ttu-id="c11fd-161">Ange **Windows Server 2012 R2** för operativsystemsfamiljen inställningen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-161">Specify **Windows Server 2012 R2** for the Operating System Family setting.</span></span>
      3. <span data-ttu-id="c11fd-162">Välj en **nodprisnivå**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-162">Select a **node pricing tier**.</span></span>
      4. <span data-ttu-id="c11fd-163">Ange **2** som värde för den **mål dedikerade** inställningen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-163">Enter **2** as value for the **Target Dedicated** setting.</span></span>
      5. <span data-ttu-id="c11fd-164">Ange **2** som värde för den **maximalt antal uppgifter per nod** inställningen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-164">Enter **2** as value for the **Max tasks per node** setting.</span></span>
   5. <span data-ttu-id="c11fd-165">Klicka på **OK** för att skapa poolen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-165">Click **OK** to create the pool.</span></span>
   6. <span data-ttu-id="c11fd-166">Notera den **ID** för poolen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-166">Note down the **ID** of the pool.</span></span> 



### <a name="high-level-steps"></a><span data-ttu-id="c11fd-167">Anvisningar</span><span class="sxs-lookup"><span data-stu-id="c11fd-167">High-level steps</span></span>
<span data-ttu-id="c11fd-168">Här är de två övergripande steg du utför som en del av den här genomgången:</span><span class="sxs-lookup"><span data-stu-id="c11fd-168">Here are the two high-level steps you perform as part of this walkthrough:</span></span> 

1. <span data-ttu-id="c11fd-169">Skapa en anpassad aktivitet som innehåller enkla data transformation/standardbearbetningslogiken.</span><span class="sxs-lookup"><span data-stu-id="c11fd-169">Create a custom activity that contains simple data transformation/processing logic.</span></span>
2. <span data-ttu-id="c11fd-170">Skapa ett Azure data factory med en pipeline som använder den anpassade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-170">Create an Azure data factory with a pipeline that uses the custom activity.</span></span>

### <a name="create-a-custom-activity"></a><span data-ttu-id="c11fd-171">Skapa en anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-171">Create a custom activity</span></span>
<span data-ttu-id="c11fd-172">Om du vill skapa en anpassad aktivitet för .NET, skapa en **.NET-klassbibliotek** projekt med en klass som implementerar som **IDotNetActivity** gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="c11fd-172">To create a .NET custom activity, create a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="c11fd-173">Det här gränssnittet har endast en metod: [kör](https://msdn.microsoft.com/library/azure/mt603945.aspx) och signaturen är:</span><span class="sxs-lookup"><span data-stu-id="c11fd-173">This interface has only one method: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) and its signature is:</span></span>

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


<span data-ttu-id="c11fd-174">Metoden som använder fyra parametrar:</span><span class="sxs-lookup"><span data-stu-id="c11fd-174">The method takes four parameters:</span></span>

- <span data-ttu-id="c11fd-175">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-175">**linkedServices**.</span></span> <span data-ttu-id="c11fd-176">Den här egenskapen är en enumerable lista över datalager som är länkade tjänster som refereras av in-/ utdata-datauppsättningar för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-176">This property is an enumerable list of Data Store linked services referenced by input/output datasets for the activity.</span></span>   
- <span data-ttu-id="c11fd-177">**datauppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-177">**datasets**.</span></span> <span data-ttu-id="c11fd-178">Den här egenskapen är en enumerable lista över datauppsättningar in-/ utdata för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-178">This property is an enumerable list of input/output datasets for the activity.</span></span> <span data-ttu-id="c11fd-179">Du kan använda den här parametern för att få platser och scheman som definierats av inkommande och utgående datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="c11fd-179">You can use this parameter to get the locations and schemas defined by input and output datasets.</span></span>
- <span data-ttu-id="c11fd-180">**aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-180">**activity**.</span></span> <span data-ttu-id="c11fd-181">Denna egenskap representerar den aktuella aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-181">This property represents the current activity.</span></span> <span data-ttu-id="c11fd-182">Den kan användas för att få åtkomst till utökade egenskaper som är associerade med den anpassade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-182">It can be used to access extended properties associated with the custom activity.</span></span> <span data-ttu-id="c11fd-183">Se [åtkomst utökade egenskaper](#access-extended-properties) mer information.</span><span class="sxs-lookup"><span data-stu-id="c11fd-183">See [Access extended properties](#access-extended-properties) for details.</span></span>
- <span data-ttu-id="c11fd-184">**loggaren**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-184">**logger**.</span></span> <span data-ttu-id="c11fd-185">Det här objektet kan du skriva debug kommentarer som yta i användarinloggning för pipeline.</span><span class="sxs-lookup"><span data-stu-id="c11fd-185">This object lets you write debug comments that surface in the user log for the pipeline.</span></span>

<span data-ttu-id="c11fd-186">Metoden returnerar en ordlista som kan användas för att kedja anpassade aktiviteter tillsammans i framtiden.</span><span class="sxs-lookup"><span data-stu-id="c11fd-186">The method returns a dictionary that can be used to chain custom activities together in the future.</span></span> <span data-ttu-id="c11fd-187">Den här funktionen har inte implementerats ännu, så returneras en tom ordlista från metoden.</span><span class="sxs-lookup"><span data-stu-id="c11fd-187">This feature is not implemented yet, so return an empty dictionary from the method.</span></span>  

### <a name="procedure"></a><span data-ttu-id="c11fd-188">Procedur</span><span class="sxs-lookup"><span data-stu-id="c11fd-188">Procedure</span></span>
1. <span data-ttu-id="c11fd-189">Skapa en **.NET-klassbibliotek** projekt.</span><span class="sxs-lookup"><span data-stu-id="c11fd-189">Create a **.NET Class Library** project.</span></span>
   <ol type="a">
     <li><span data-ttu-id="c11fd-190">Starta <b>Visual Studio 2017</b> eller <b>Visual Studio 2015</b> eller <b>Visual Studio 2013</b> eller <b>Visual Studio 2012</b>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-190">Launch <b>Visual Studio 2017</b> or <b>Visual Studio 2015</b> or <b>Visual Studio 2013</b> or <b>Visual Studio 2012</b>.</span></span></li>
     <li><span data-ttu-id="c11fd-191">Klicka på <b>Arkiv</b>, peka på <b>Nytt</b> och klicka på <b>Projekt</b>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-191">Click <b>File</b>, point to <b>New</b>, and click <b>Project</b>.</span></span></li>
     <li><span data-ttu-id="c11fd-192">Expandera <b>Mallar</b> och välj <b>Visual C#</b>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-192">Expand <b>Templates</b>, and select <b>Visual C#</b>.</span></span> <span data-ttu-id="c11fd-193">I den här genomgången ska du använda C#, men du kan använda alla .NET-språk för att utveckla anpassade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-193">In this walkthrough, you use C#, but you can use any .NET language to develop the custom activity.</span></span></li>
     <li><span data-ttu-id="c11fd-194">Välj <b>klassbiblioteket</b> från listan över projekttyper till höger.</span><span class="sxs-lookup"><span data-stu-id="c11fd-194">Select <b>Class Library</b> from the list of project types on the right.</span></span> <span data-ttu-id="c11fd-195">Välj i VS 2017 <b>Class Library (.NET Framework)</b> </span><span class="sxs-lookup"><span data-stu-id="c11fd-195">In VS 2017, choose <b>Class Library (.NET Framework)</b> </span></span></li>
     <li><span data-ttu-id="c11fd-196">Ange <b>MyDotNetActivity</b> för den <b>namn</b>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-196">Enter <b>MyDotNetActivity</b> for the <b>Name</b>.</span></span></li>
     <li><span data-ttu-id="c11fd-197">Välj <b>C:\ADFGetStarted</b> för den <b>plats</b>.</span><span class="sxs-lookup"><span data-stu-id="c11fd-197">Select <b>C:\ADFGetStarted</b> for the <b>Location</b>.</span></span></li>
     <li><span data-ttu-id="c11fd-198">Klicka på <b>OK</b> för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-198">Click <b>OK</b> to create the project.</span></span></li>
   </ol><span data-ttu-id="c11fd-199">
2.Klicka på **verktyg**, peka på **NuGet Package Manager**, och klicka på **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-199">
2. Click **Tools**, point to **NuGet Package Manager**, and click **Package Manager Console**.</span></span>
<span data-ttu-id="c11fd-200">3.</span><span class="sxs-lookup"><span data-stu-id="c11fd-200">3.</span></span> <span data-ttu-id="c11fd-201">I Package Manager-konsolen kör du följande kommando för att importera **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-201">In the Package Manager Console, execute the following command to import **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="c11fd-202">Importera den **Azure Storage** NuGet-paketet i i projektet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-202">Import the **Azure Storage** NuGet package in to the project.</span></span>

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="c11fd-203">Startprogram för data Factory-tjänsten kräver 4.3 version av WindowsAzure.Storage.</span><span class="sxs-lookup"><span data-stu-id="c11fd-203">Data Factory service launcher requires the 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="c11fd-204">Om du lägger till en referens till en senare version av Azure Storage sammansättning i projektet anpassad aktivitet finns ett fel när aktiviteten körs.</span><span class="sxs-lookup"><span data-stu-id="c11fd-204">If you add a reference to a later version of Azure Storage assembly in your custom activity project, you see an error when the activity executes.</span></span> <span data-ttu-id="c11fd-205">Du kan lösa problemet, se [Appdomain isolering](#appdomain-isolation) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c11fd-205">To resolve the error, see [Appdomain isolation](#appdomain-isolation) section.</span></span> 
5. <span data-ttu-id="c11fd-206">Lägg till följande **med** instruktioner till källfilen i projektet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-206">Add the following **using** statements to the source file in the project.</span></span>

    ```csharp

    // Comment these lines if using VS 2017
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    // --------------------

    // Comment these lines if using <= VS 2015
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    // ---------------------

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. <span data-ttu-id="c11fd-207">Ändra namnet på den **namnområde** till **MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-207">Change the name of the **namespace** to **MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="c11fd-208">Ändra namnet på klassen som **MyDotNetActivity** och härleds från den **IDotNetActivity** gränssnitt som visas i följande kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="c11fd-208">Change the name of the class to **MyDotNetActivity** and derive it from the **IDotNetActivity** interface as shown in the following code snippet:</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="c11fd-209">Implementera (Lägg till) på **kör** metod för den **IDotNetActivity** gränssnitt till den **MyDotNetActivity** klassen och kopiera följande exempelkod till metoden.</span><span class="sxs-lookup"><span data-stu-id="c11fd-209">Implement (Add) the **Execute** method of the **IDotNetActivity** interface to the **MyDotNetActivity** class and copy the following sample code to the method.</span></span>

    <span data-ttu-id="c11fd-210">I följande exempel räknar antal förekomster av sökordet (”Microsoft”) i varje blob som är associerade med en datasektorn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-210">The following sample counts the number of occurrences of the search term (“Microsoft”) in each blob associated with a data slice.</span></span>

    ```csharp
    /// <summary>
    /// Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    
    public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
    {
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // to log information, use the logger object
        // log all extended properties            
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }
    
        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get the input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables to hold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from the dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and the other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get the first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using the same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get the connection string in the linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get the folder path from the input dataset definition
        string folderPath = GetFolderPath(inputDataset);
        string output = string.Empty; // for use later.
    
        // create storage client for input. Pass the connection string.
        CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
        CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
        // initialize the continuation token before using it in the do-while loop.
        BlobContinuationToken continuationToken = null;
        do
        {   // get the list of input blobs from the input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                     true,
                                     BlobListingDetails.Metadata,
                                     null,
                                     continuationToken,
                                     null,
                                     null);
    
            // Calculate method returns the number of occurrences of
            // the search term (“Microsoft”) in each blob associated
               // with the data slice. definition of the method is shown in the next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for the output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get the folder path from the output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log the output folder path   
        logger.Write("Writing blob to the folder: {0}", folderPath);
    
        // create a storage object for the output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log the output file name
        logger.Write("output blob URI: {0}", outputBlobUri.ToString());

        // create a blob and upload the output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} to the output blob", output);
        outputBlob.UploadText(output);
    
        // The dictionary can be used to chain custom activities together in the future.
        // This feature is not implemented yet, so just return an empty dictionary.  
    
        return new Dictionary<string, string>();
    }
    ```
9. <span data-ttu-id="c11fd-211">Lägg till följande helper-metoder:</span><span class="sxs-lookup"><span data-stu-id="c11fd-211">Add the following helper methods:</span></span> 

    ```csharp
    /// <summary>
    /// Gets the folderPath value from the input/output dataset.
    /// </summary>
    
    private static string GetFolderPath(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }

        // get type properties of the dataset   
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the folder path found in the type properties
        return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets the fileName value from the input/output dataset.   
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }
    
        // get type properties of the dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the blob/file name in the type properties
        return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
    /// and prepares the output text that is written to the output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
        string output = string.Empty;
        logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
        foreach (IListBlobItem listBlobItem in Bresult.Results)
        {
            CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
            if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
            {
                string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
                logger.Write("input blob text: {0}", blobText);
                string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
                var matchQuery = from word in source
                                 where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                 select word;
                int wordCount = matchQuery.Count();
                output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
            }
        }
        return output;
    }
    ```

    <span data-ttu-id="c11fd-212">Metoden GetFolderPath returnerar sökvägen till mappen som dataset pekar på, och metoden GetFileName Returnerar namnet på blob/fil som dataset pekar på.</span><span class="sxs-lookup"><span data-stu-id="c11fd-212">The GetFolderPath method returns the path to the folder that the dataset points to and the GetFileName method returns the name of the blob/file that the dataset points to.</span></span> <span data-ttu-id="c11fd-213">Om du havefolderPath definierar använda variabler, till exempel {Year}, {Month}, {Day} osv., metoden returnerar strängen eftersom den är utan att ersätta dem med runtime-värden.</span><span class="sxs-lookup"><span data-stu-id="c11fd-213">If you havefolderPath defines using variables such as {Year}, {Month}, {Day} etc., the method returns the string as it is without replacing them with runtime values.</span></span> <span data-ttu-id="c11fd-214">Se [åtkomst utökade egenskaper](#access-extended-properties) information om att komma åt SliceStart, SliceEnd osv.</span><span class="sxs-lookup"><span data-stu-id="c11fd-214">See [Access extended properties](#access-extended-properties) section for details on accessing SliceStart, SliceEnd, etc.</span></span>    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    <span data-ttu-id="c11fd-215">Metoden Calculate beräknar antalet instanser av nyckelordet Microsoft i indatafiler (blobbar i mappen).</span><span class="sxs-lookup"><span data-stu-id="c11fd-215">The Calculate method calculates the number of instances of keyword Microsoft in the input files (blobs in the folder).</span></span> <span data-ttu-id="c11fd-216">Sökordet (”Microsoft”) är hårdkodat i koden.</span><span class="sxs-lookup"><span data-stu-id="c11fd-216">The search term (“Microsoft”) is hard-coded in the code.</span></span>
10. <span data-ttu-id="c11fd-217">Kompilera projektet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-217">Compile the project.</span></span> <span data-ttu-id="c11fd-218">Klicka på **skapa** i menyn och klickar på **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-218">Click **Build** from the menu and click **Build Solution**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c11fd-219">Ange 4.5.2 version av .NET Framework som målramverk för ditt projekt: Högerklicka på projektet och klicka på **egenskaper** ange Målversionen av framework.</span><span class="sxs-lookup"><span data-stu-id="c11fd-219">Set 4.5.2 version of .NET Framework as the target framework for your project: right-click the project, and click **Properties** to set the target framework.</span></span> <span data-ttu-id="c11fd-220">Data Factory stöder inte anpassade aktiviteter kompileras mot .NET Framework-versioner senare än 4.5.2.</span><span class="sxs-lookup"><span data-stu-id="c11fd-220">Data Factory does not support custom activities compiled against .NET Framework versions later than 4.5.2.</span></span>

11. <span data-ttu-id="c11fd-221">Starta **Windows Explorer**, och navigera till **bin\debug** eller **bin\release** beroende på vilken typ av version.</span><span class="sxs-lookup"><span data-stu-id="c11fd-221">Launch **Windows Explorer**, and navigate to **bin\debug** or **bin\release** folder depending on the type of build.</span></span>
12. <span data-ttu-id="c11fd-222">Skapa en zip-fil **MyDotNetActivity.zip** som innehåller alla binärfiler i den <project folder>\bin\Debug mapp.</span><span class="sxs-lookup"><span data-stu-id="c11fd-222">Create a zip file **MyDotNetActivity.zip** that contains all the binaries in the <project folder>\bin\Debug folder.</span></span> <span data-ttu-id="c11fd-223">Inkludera den **MyDotNetActivity.pdb** filen så att du får ytterligare information, till exempel radnumret i källkoden som har orsakat problemet om ett fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="c11fd-223">Include the **MyDotNetActivity.pdb** file so that you get additional details such as line number in the source code that caused the issue if there was a failure.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="c11fd-224">Alla filer i zip-filen för den anpassade aktiviteten måste vara på den **översta nivån** utan undermappar.</span><span class="sxs-lookup"><span data-stu-id="c11fd-224">All the files in the zip file for the custom activity must be at the **top level** with no sub folders.</span></span>

    ![Binära filer](./media/data-factory-use-custom-activities/Binaries.png)
14. <span data-ttu-id="c11fd-226">Skapa en blobbbehållare med namnet **customactivitycontainer** om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="c11fd-226">Create a blob container named **customactivitycontainer** if it does not already exist.</span></span> 
15. <span data-ttu-id="c11fd-227">Ladda upp MyDotNetActivity.zip som en blobb till customactivitycontainer i en **allmänna** Azure blob-lagring (hot inte/lågfrekvent Blob-lagring) som refereras av AzureStorageLinkedService.</span><span class="sxs-lookup"><span data-stu-id="c11fd-227">Upload MyDotNetActivity.zip as a blob to the customactivitycontainer in a **general-purpose** Azure blob storage (not hot/cool Blob storage) that is referred by AzureStorageLinkedService.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="c11fd-228">Om du lägger till det här projektet för .NET-aktiviteten en lösning i Visual Studio som innehåller ett projekt som Data Factory och lägga till en referens till projektet för .NET-aktiviteten från Data Factory-programprojekt, behöver du inte utföra de två sista stegen för att manuellt skapa zip-filen och överföra den till allmänna Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="c11fd-228">If you add this .NET activity project to a solution in Visual Studio that contains a Data Factory project, and add a reference to .NET activity project from the Data Factory application project, you do not need to perform the last two steps of manually creating the zip file and uploading it to the general-purpose Azure blob storage.</span></span> <span data-ttu-id="c11fd-229">När du publicerar Data Factory-enheter med hjälp av Visual Studio, utförs automatiskt de här stegen av publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-229">When you publish Data Factory entities using Visual Studio, these steps are automatically done by the publishing process.</span></span> <span data-ttu-id="c11fd-230">Mer information finns i [Data Factory-projekt i Visual Studio](#data-factory-project-in-visual-studio) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c11fd-230">For more information, see [Data Factory project in Visual Studio](#data-factory-project-in-visual-studio) section.</span></span>

## <a name="create-a-pipeline-with-custom-activity"></a><span data-ttu-id="c11fd-231">Skapa en pipeline med anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-231">Create a pipeline with custom activity</span></span>
<span data-ttu-id="c11fd-232">Du har skapat en anpassad aktivitet och upp zip-filen med binärfiler till en blobbbehållare i en **allmänna** Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c11fd-232">You have created a custom activity and uploaded the zip file with binaries to a blob container in a **general-purpose** Azure Storage Account.</span></span> <span data-ttu-id="c11fd-233">I det här avsnittet skapar du ett Azure data factory med en pipeline som använder den anpassade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-233">In this section, you create an Azure data factory with a pipeline that uses the custom activity.</span></span>

<span data-ttu-id="c11fd-234">Den inkommande datamängden för den anpassade aktiviteten representerar blobbar (filer) i mappen customactivityinput för adftutorial behållare i blob storage.</span><span class="sxs-lookup"><span data-stu-id="c11fd-234">The input dataset for the custom activity represents blobs (files) in the customactivityinput folder of adftutorial container in the blob storage.</span></span> <span data-ttu-id="c11fd-235">Datamängd för utdata för aktiviteten representerar utdata blobbar i mappen customactivityoutput för adftutorial behållare i blob storage.</span><span class="sxs-lookup"><span data-stu-id="c11fd-235">The output dataset for the activity represents output blobs in the customactivityoutput folder of adftutorial container in the blob storage.</span></span>

<span data-ttu-id="c11fd-236">Skapa **fil.txt** filen med följande innehåll och överföra den till **customactivityinput** mappen för den **adftutorial** behållare.</span><span class="sxs-lookup"><span data-stu-id="c11fd-236">Create **file.txt** file with the following content and upload it to **customactivityinput** folder of the **adftutorial** container.</span></span> <span data-ttu-id="c11fd-237">Skapa adftutorial behållaren om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="c11fd-237">Create the adftutorial container if it does not exist already.</span></span> 

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="c11fd-238">Mappen inkommande motsvarar en sektor i Azure Data Factory även om mappen har två eller flera filer.</span><span class="sxs-lookup"><span data-stu-id="c11fd-238">The input folder corresponds to a slice in Azure Data Factory even if the folder has two or more files.</span></span> <span data-ttu-id="c11fd-239">När varje segment bearbetas av pipelinen går anpassad aktivitet igenom alla blobbar i mappen inkommande för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-239">When each slice is processed by the pipeline, the custom activity iterates through all the blobs in the input folder for that slice.</span></span>

<span data-ttu-id="c11fd-240">Du ser en utdatafil med i mappen adftutorial\customactivityoutput med en eller flera rader (samma som antal blobbar i mappen inkommande):</span><span class="sxs-lookup"><span data-stu-id="c11fd-240">You see one output file with in the adftutorial\customactivityoutput folder with one or more lines (same as number of blobs in the input folder):</span></span>

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
```


<span data-ttu-id="c11fd-241">Här är vad du behöver göra i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="c11fd-241">Here are the steps you perform in this section:</span></span>

1. <span data-ttu-id="c11fd-242">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-242">Create a **data factory**.</span></span>
2. <span data-ttu-id="c11fd-243">Skapa **länkade tjänster** för Azure Batch-pool för virtuella datorer som den anpassade aktiviteten körs och Azure-lagring som innehåller in-/ utdata-BLOB.</span><span class="sxs-lookup"><span data-stu-id="c11fd-243">Create **Linked services** for the Azure Batch pool of VMs on which the custom activity runs and the Azure Storage that holds the input/output blobs.</span></span>
3. <span data-ttu-id="c11fd-244">Skapa ingående och utgående **datauppsättningar** som representerar indata och utdata för den anpassade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-244">Create input and output **datasets** that represent input and output of the custom activity.</span></span>
4. <span data-ttu-id="c11fd-245">Skapa en **pipeline** som använder den anpassade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-245">Create a **pipeline** that uses the custom activity.</span></span>

> [!NOTE]
> <span data-ttu-id="c11fd-246">Skapa den **fil.txt** och överföra den till en blob-behållare, om du inte redan gjort det..</span><span class="sxs-lookup"><span data-stu-id="c11fd-246">Create the **file.txt** and upload it to a blob container if you haven't already done so.</span></span> <span data-ttu-id="c11fd-247">Se anvisningarna i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c11fd-247">See instructions in the preceding section.</span></span>   

### <a name="step-1-create-the-data-factory"></a><span data-ttu-id="c11fd-248">Steg 1: Skapa datafabriken</span><span class="sxs-lookup"><span data-stu-id="c11fd-248">Step 1: Create the data factory</span></span>
1. <span data-ttu-id="c11fd-249">När du loggar in på Azure-portalen, gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="c11fd-249">After logging in to the Azure portal, do the following steps:</span></span>
   1. <span data-ttu-id="c11fd-250">Klicka på **NYTT** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-250">Click **NEW** on the left menu.</span></span>
   2. <span data-ttu-id="c11fd-251">Klicka på **Data + analys** i den **ny** bladet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-251">Click **Data + Analytics** in the **New** blade.</span></span>
   3. <span data-ttu-id="c11fd-252">Klicka på **Data Factory** på bladet **Dataanalys**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-252">Click **Data Factory** on the **Data analytics** blade.</span></span>
   
    ![Nya Azure Data Factory-menyn](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. <span data-ttu-id="c11fd-254">I den **nya datafabriken** bladet ange **CustomActivityFactory** för namnet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-254">In the **New data factory** blade, enter **CustomActivityFactory** for the Name.</span></span> <span data-ttu-id="c11fd-255">Namnet på Azure Data Factory måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="c11fd-255">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="c11fd-256">Om du får felmeddelandet: **datafabriksnamnet ”CustomActivityFactory” är inte tillgänglig**, byta namn på datafabriken (till exempel **yournameCustomActivityFactory**) och försöker skapa igen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-256">If you receive the error: **Data factory name “CustomActivityFactory” is not available**, change the name of the data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>

    ![Nya Azure Data Factory-bladet](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. <span data-ttu-id="c11fd-258">Klicka på **RESURSGRUPPENS namn**, och välj en befintlig resursgrupp eller skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c11fd-258">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="c11fd-259">Kontrollera att du använder rätt **prenumeration** och **region** där du vill att datafabriken ska skapas.</span><span class="sxs-lookup"><span data-stu-id="c11fd-259">Verify that you are using the correct **subscription** and **region** where you want the data factory to be created.</span></span>
5. <span data-ttu-id="c11fd-260">Klicka på **Skapa** på bladet **Ny datafabrik**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-260">Click **Create** on the **New data factory** blade.</span></span>
6. <span data-ttu-id="c11fd-261">Du ser datafabriken som skapas i den **instrumentpanelen** på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-261">You see the data factory being created in the **Dashboard** of the Azure portal.</span></span>
7. <span data-ttu-id="c11fd-262">När datafabriken har skapats, kan du se Data Factory-bladet, som visar innehållet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="c11fd-262">After the data factory has been created successfully, you see the Data Factory blade, which shows you the contents of the data factory.</span></span>
    
    ![Bladet Datafabrik](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a><span data-ttu-id="c11fd-264">Steg 2: Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="c11fd-264">Step 2: Create linked services</span></span>
<span data-ttu-id="c11fd-265">Länkade tjänster länkar datalager eller beräkningstjänster till en Azure-datafabrik.</span><span class="sxs-lookup"><span data-stu-id="c11fd-265">Linked services link data stores or compute services to an Azure data factory.</span></span> <span data-ttu-id="c11fd-266">I det här steget kan länka du ditt Azure Storage-konto och Azure Batch-konto till din data factory.</span><span class="sxs-lookup"><span data-stu-id="c11fd-266">In this step, you link your Azure Storage account and Azure Batch account to your data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="c11fd-267">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="c11fd-267">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="c11fd-268">Klicka på den **författare och distribuera** panelen på den **DATAFABRIKEN** bladet för **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-268">Click the **Author and deploy** tile on the **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="c11fd-269">Du kan se Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="c11fd-269">You see the Data Factory Editor.</span></span>
2. <span data-ttu-id="c11fd-270">Klicka på **Nytt datalager** på kommandoraden och välj **Azure storage**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-270">Click **New data store** on the command bar and choose **Azure storage**.</span></span> <span data-ttu-id="c11fd-271">Du bör se JSON-skriptet för att skapa en länkad Azure-lagringstjänst i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="c11fd-271">You should see the JSON script for creating an Azure Storage linked service in the editor.</span></span>
    
    ![Nytt datalager - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. <span data-ttu-id="c11fd-273">Ersätt `<accountname>` med namnet på ditt Azure storage-konto och `<accountkey>` med åtkomstnyckeln för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c11fd-273">Replace `<accountname>` with name of your Azure storage account and `<accountkey>` with access key of the Azure storage account.</span></span> <span data-ttu-id="c11fd-274">Information om hur du hämtar din lagringsåtkomstnyckel finns i [Visa, kopiera och återskapa lagringsåtkomstnycklar](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="c11fd-274">To learn how to get your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

    ![Azure Storage tyckte om tjänsten](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. <span data-ttu-id="c11fd-276">Klicka på **Distribuera** i kommandofältet för att distribuera den länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-276">Click **Deploy** on the command bar to deploy the linked service.</span></span>

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="c11fd-277">Skapa Azure Batch länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="c11fd-277">Create Azure Batch linked service</span></span>
1. <span data-ttu-id="c11fd-278">I den Data Factory-redigeraren, klicka på **... Flera** i kommandofältet klickar du på **nya beräkning**, och välj sedan **Azure Batch** på menyn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-278">In the Data Factory Editor, click **... More** on the command bar, click **New compute**, and then select **Azure Batch** from the menu.</span></span>

    ![Nya beräknings - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. <span data-ttu-id="c11fd-280">Gör följande ändringar i JSON-skriptet:</span><span class="sxs-lookup"><span data-stu-id="c11fd-280">Make the following changes to the JSON script:</span></span>

   1. <span data-ttu-id="c11fd-281">Ange Azure Batch-kontonamnet för den **accountName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-281">Specify Azure Batch account name for the **accountName** property.</span></span> <span data-ttu-id="c11fd-282">Den **URL** från den **Azure Batch-kontoblad** är i följande format: `http://accountname.region.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-282">The **URL** from the **Azure Batch account blade** is in the following format: `http://accountname.region.batch.azure.com`.</span></span> <span data-ttu-id="c11fd-283">För den **batchUri** egenskap i JSON som du behöver ta bort `accountname.` från URL: en och använder den `accountname` för den `accountName` JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-283">For the **batchUri** property in the JSON, you need to remove `accountname.` from the URL and use the `accountname` for the `accountName` JSON property.</span></span>
   2. <span data-ttu-id="c11fd-284">Ange nyckel för Azure Batch-kontot för den **accessKey** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-284">Specify the Azure Batch account key for the **accessKey** property.</span></span>
   3. <span data-ttu-id="c11fd-285">Ange namnet på poolen som du har skapat som en del av krav för den **poolName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-285">Specify the name of the pool you created as part of prerequisites for the **poolName** property.</span></span> <span data-ttu-id="c11fd-286">Du kan också ange ID för poolen i stället för namnet på poolen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-286">You can also specify the ID of the pool instead of the name of the pool.</span></span>
   4. <span data-ttu-id="c11fd-287">Ange Azure Batch-URI för den **batchUri** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-287">Specify Azure Batch URI for the **batchUri** property.</span></span> <span data-ttu-id="c11fd-288">Exempel: `https://westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c11fd-288">Example: `https://westus.batch.azure.com`.</span></span>  
   5. <span data-ttu-id="c11fd-289">Ange den **AzureStorageLinkedService** för den **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-289">Specify the **AzureStorageLinkedService** for the **linkedServiceName** property.</span></span>

        ```json
        {
         "name": "AzureBatchLinkedService",
         "properties": {
           "type": "AzureBatch",
           "typeProperties": {
             "accountName": "myazurebatchaccount",
             "batchUri": "https://westus.batch.azure.com",
             "accessKey": "<yourbatchaccountkey>",
             "poolName": "myazurebatchpool",
             "linkedServiceName": "AzureStorageLinkedService"
           }
         }
        }
        ```

       <span data-ttu-id="c11fd-290">För den **poolName** egenskap, du kan också ange ID för poolen i stället för namnet på poolen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-290">For the **poolName** property, you can also specify the ID of the pool instead of the name of the pool.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="c11fd-291">Data Factory-tjänsten stöder inte ett alternativ på begäran för Azure Batch som för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c11fd-291">The Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="c11fd-292">Du kan bara använda din egen Azure Batch-pool i ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="c11fd-292">You can only use your own Azure Batch pool in an Azure data factory.</span></span>   
    

### <a name="step-3-create-datasets"></a><span data-ttu-id="c11fd-293">Steg 3: Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="c11fd-293">Step 3: Create datasets</span></span>
<span data-ttu-id="c11fd-294">I det här steget skapar du datauppsättningar som representerar indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="c11fd-294">In this step, you create datasets to represent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="c11fd-295">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="c11fd-295">Create input dataset</span></span>
1. <span data-ttu-id="c11fd-296">I **redigeringsprogrammet** för Data Factory klickar du på **... Flera** i kommandofältet klickar du på **ny datamängd**, och välj sedan **Azure Blob storage** från den nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-296">In the **Editor** for the Data Factory, click **... More** on the command bar, click **New dataset**, and then select **Azure Blob storage** from the drop-down menu.</span></span>
2. <span data-ttu-id="c11fd-297">Ersätt JSON i den högra rutan med följande kodavsnitt i JSON:</span><span class="sxs-lookup"><span data-stu-id="c11fd-297">Replace the JSON in the right pane with the following JSON snippet:</span></span>

    ```json
    {
     "name": "InputDataset",
     "properties": {
         "type": "AzureBlob",
         "linkedServiceName": "AzureStorageLinkedService",
         "typeProperties": {
             "folderPath": "adftutorial/customactivityinput/",
             "format": {
                 "type": "TextFormat"
             }
         },
         "availability": {
             "frequency": "Hour",
             "interval": 1
         },
         "external": true,
         "policy": {}
     }
    }
    ```

   <span data-ttu-id="c11fd-298">Du skapar en pipeline senare i den här genomgången med starttid: 2016-11-16T00:00:00Z-och Sluttid: 2016-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="c11fd-298">You create a pipeline later in this walkthrough with start time: 2016-11-16T00:00:00Z and end time: 2016-11-16T05:00:00Z.</span></span> <span data-ttu-id="c11fd-299">Den är schemalagd att generera data varje timma, så att det finns fem i/o-segment (mellan **00**: 00:00 -> **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="c11fd-299">It is scheduled to produce data hourly, so there are five input/output slices (between **00**:00:00 -> **05**:00:00).</span></span>

   <span data-ttu-id="c11fd-300">Den **frekvens** och **intervall** för inkommande datamängden har värdet **timme** och **1**, vilket innebär att inkommande segmentet finns varje timme.</span><span class="sxs-lookup"><span data-stu-id="c11fd-300">The **frequency** and **interval** for the input dataset is set to **Hour** and **1**, which means that the input slice is available hourly.</span></span> <span data-ttu-id="c11fd-301">I det här exemplet är det samma fil (fil.txt) på intputfolder.</span><span class="sxs-lookup"><span data-stu-id="c11fd-301">In this sample, it is the same file (file.txt) in the intputfolder.</span></span>

   <span data-ttu-id="c11fd-302">Här följer starttider för varje segment som representeras av SliceStart systemvariabel i ovanstående JSON-fragment.</span><span class="sxs-lookup"><span data-stu-id="c11fd-302">Here are the start times for each slice, which is represented by SliceStart system variable in the above JSON snippet.</span></span>
3. <span data-ttu-id="c11fd-303">Klicka på **distribuera** i verktygsfältet för att skapa och distribuera den **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-303">Click **Deploy** on the toolbar to create and deploy the **InputDataset**.</span></span> <span data-ttu-id="c11fd-304">Kontrollera att du ser meddelandet **TABELLEN HAR SKAPATS** i namnlisten i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="c11fd-304">Confirm that you see the **TABLE CREATED SUCCESSFULLY** message on the title bar of the Editor.</span></span>

#### <a name="create-an-output-dataset"></a><span data-ttu-id="c11fd-305">Skapa en datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="c11fd-305">Create an output dataset</span></span>
1. <span data-ttu-id="c11fd-306">I den **Data Factory-redigeraren**, klickar du på **... Flera** i kommandofältet klickar du på **ny datamängd**, och välj sedan **Azure Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-306">In the **Data Factory editor**, click **... More** on the command bar, click **New dataset**, and then select **Azure Blob storage**.</span></span>
2. <span data-ttu-id="c11fd-307">Ersätt JSON-skript i den högra rutan med följande JSON-skript:</span><span class="sxs-lookup"><span data-stu-id="c11fd-307">Replace the JSON script in the right pane with the following JSON script:</span></span>

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
                "partitionedBy": [
                    {
                        "name": "slice",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy-MM-dd-HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

     <span data-ttu-id="c11fd-308">Platsen är **adftutorial/customactivityoutput/** och utdata-filnamn är åååå-MM-dd-HH.txt där åååå-MM-dd-HH är år, månad, datum och timme av sektorn som skapas.</span><span class="sxs-lookup"><span data-stu-id="c11fd-308">Output location is **adftutorial/customactivityoutput/** and output file name is yyyy-MM-dd-HH.txt where yyyy-MM-dd-HH is the year, month, date, and hour of the slice being produced.</span></span> <span data-ttu-id="c11fd-309">Se [för utvecklare] [ adf-developer-reference] mer information.</span><span class="sxs-lookup"><span data-stu-id="c11fd-309">See [Developer Reference][adf-developer-reference] for details.</span></span>

    <span data-ttu-id="c11fd-310">En blob/utdatafil genereras för varje inkommande segment.</span><span class="sxs-lookup"><span data-stu-id="c11fd-310">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="c11fd-311">Här är hur en utdatafil heter för varje segment.</span><span class="sxs-lookup"><span data-stu-id="c11fd-311">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="c11fd-312">Alla utdatafiler som skapas i en utdatamapp: **adftutorial\customactivityoutput**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-312">All the output files are generated in one output folder: **adftutorial\customactivityoutput**.</span></span>

   | <span data-ttu-id="c11fd-313">Sektorn</span><span class="sxs-lookup"><span data-stu-id="c11fd-313">Slice</span></span> | <span data-ttu-id="c11fd-314">Starttid</span><span class="sxs-lookup"><span data-stu-id="c11fd-314">Start time</span></span> | <span data-ttu-id="c11fd-315">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="c11fd-315">Output file</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="c11fd-316">1</span><span class="sxs-lookup"><span data-stu-id="c11fd-316">1</span></span> |<span data-ttu-id="c11fd-317">2016-11-16T00:00:00</span><span class="sxs-lookup"><span data-stu-id="c11fd-317">2016-11-16T00:00:00</span></span> |<span data-ttu-id="c11fd-318">2016-11-16-00.txt</span><span class="sxs-lookup"><span data-stu-id="c11fd-318">2016-11-16-00.txt</span></span> |
   | <span data-ttu-id="c11fd-319">2</span><span class="sxs-lookup"><span data-stu-id="c11fd-319">2</span></span> |<span data-ttu-id="c11fd-320">2016-11-16T01:00:00</span><span class="sxs-lookup"><span data-stu-id="c11fd-320">2016-11-16T01:00:00</span></span> |<span data-ttu-id="c11fd-321">2016-11-16-01.txt</span><span class="sxs-lookup"><span data-stu-id="c11fd-321">2016-11-16-01.txt</span></span> |
   | <span data-ttu-id="c11fd-322">3</span><span class="sxs-lookup"><span data-stu-id="c11fd-322">3</span></span> |<span data-ttu-id="c11fd-323">2016-11-16T02:00:00</span><span class="sxs-lookup"><span data-stu-id="c11fd-323">2016-11-16T02:00:00</span></span> |<span data-ttu-id="c11fd-324">2016-11-16-02.txt</span><span class="sxs-lookup"><span data-stu-id="c11fd-324">2016-11-16-02.txt</span></span> |
   | <span data-ttu-id="c11fd-325">4</span><span class="sxs-lookup"><span data-stu-id="c11fd-325">4</span></span> |<span data-ttu-id="c11fd-326">2016-11-16T03:00:00</span><span class="sxs-lookup"><span data-stu-id="c11fd-326">2016-11-16T03:00:00</span></span> |<span data-ttu-id="c11fd-327">2016-11-16-03.txt</span><span class="sxs-lookup"><span data-stu-id="c11fd-327">2016-11-16-03.txt</span></span> |
   | <span data-ttu-id="c11fd-328">5</span><span class="sxs-lookup"><span data-stu-id="c11fd-328">5</span></span> |<span data-ttu-id="c11fd-329">2016-11-16T04:00:00</span><span class="sxs-lookup"><span data-stu-id="c11fd-329">2016-11-16T04:00:00</span></span> |<span data-ttu-id="c11fd-330">2016-11-16-04.txt</span><span class="sxs-lookup"><span data-stu-id="c11fd-330">2016-11-16-04.txt</span></span> |

    <span data-ttu-id="c11fd-331">Kom ihåg att alla filer i en inkommande mapp är en del av ett segment med starttiderna som nämns ovan.</span><span class="sxs-lookup"><span data-stu-id="c11fd-331">Remember that all the files in an input folder are part of a slice with the start times mentioned above.</span></span> <span data-ttu-id="c11fd-332">När den här sektorn behandlas den anpassade aktiviteten söker igenom varje fil och ger en rad i filen med antalet förekomster av sökordet (”Microsoft”).</span><span class="sxs-lookup"><span data-stu-id="c11fd-332">When this slice is processed, the custom activity scans through each file and produces a line in the output file with the number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="c11fd-333">Om det finns tre filer i inputfolder, det finns tre rader i filen för varje timme segment: 2016-11-16-00.txt 2016-11-16:01:00:00.txt, etc.</span><span class="sxs-lookup"><span data-stu-id="c11fd-333">If there are three files in the inputfolder, there are three lines in the output file for each hourly slice: 2016-11-16-00.txt, 2016-11-16:01:00:00.txt, etc.</span></span>
3. <span data-ttu-id="c11fd-334">Att distribuera den **OutputDataset**, klickar du på **distribuera** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-334">To deploy the **OutputDataset**, click **Deploy** on the command bar.</span></span>

### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a><span data-ttu-id="c11fd-335">Skapa och köra en pipeline som använder den anpassade aktiviteten</span><span class="sxs-lookup"><span data-stu-id="c11fd-335">Create and run a pipeline that uses the custom activity</span></span>
1. <span data-ttu-id="c11fd-336">I den Data Factory-redigeraren, klicka på **... Flera**, och välj sedan **ny pipeline** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-336">In the Data Factory Editor, click **... More**, and then select **New pipeline** on the command bar.</span></span> 
2. <span data-ttu-id="c11fd-337">Ersätt JSON i den högra rutan med följande JSON-skript:</span><span class="sxs-lookup"><span data-stu-id="c11fd-337">Replace the JSON in the right pane with the following JSON script:</span></span>

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    <span data-ttu-id="c11fd-338">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="c11fd-338">Note the following points:</span></span>

   * <span data-ttu-id="c11fd-339">**Concurrency** är inställd på **2** så att två segment bearbetas parallellt med 2 virtuella datorer i Azure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="c11fd-339">**Concurrency** is set to **2** so that two slices are processed in parallel by 2 VMs in the Azure Batch pool.</span></span>
   * <span data-ttu-id="c11fd-340">Det finns en aktivitet i avsnittet aktiviteter och är av typen: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-340">There is one activity in the activities section and it is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="c11fd-341">**AssemblyName** är inställd på namnet på DLL: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-341">**AssemblyName** is set to the name of the DLL: **MyDotnetActivity.dll**.</span></span>
   * <span data-ttu-id="c11fd-342">**EntryPoint** är inställd på **MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-342">**EntryPoint** is set to **MyDotNetActivityNS.MyDotNetActivity**.</span></span>
   * <span data-ttu-id="c11fd-343">**PackageLinkedService** är inställd på **AzureStorageLinkedService** som pekar till blob-lagring som innehåller anpassad aktivitet zip-filen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-343">**PackageLinkedService** is set to **AzureStorageLinkedService** that points to the blob storage that contains the custom activity zip file.</span></span> <span data-ttu-id="c11fd-344">Om du använder olika Azure Storage-konton för i/o-filer och anpassad aktivitet zip-filen kan skapa du en annan länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c11fd-344">If you are using different Azure Storage accounts for input/output files and the custom activity zip file, you create another Azure Storage linked service.</span></span> <span data-ttu-id="c11fd-345">Den här artikeln förutsätter att du använder samma Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c11fd-345">This article assumes that you are using the same Azure Storage account.</span></span>
   * <span data-ttu-id="c11fd-346">**PackageFile** är inställd på **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-346">**PackageFile** is set to **customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="c11fd-347">Det är i formatet: containerforthezip/nameofthezip.zip.</span><span class="sxs-lookup"><span data-stu-id="c11fd-347">It is in the format: containerforthezip/nameofthezip.zip.</span></span>
   * <span data-ttu-id="c11fd-348">Den anpassade aktiviteten tar **InputDataset** som indata och **OutputDataset** som utdata.</span><span class="sxs-lookup"><span data-stu-id="c11fd-348">The custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="c11fd-349">LinkedServiceName-egenskapen för den anpassade aktiviteten pekar på den **AzureBatchLinkedService**, som talar om Azure Data Factory som den anpassade aktiviteten ska köras på virtuella datorer i Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="c11fd-349">The linkedServiceName property of the custom activity points to the **AzureBatchLinkedService**, which tells Azure Data Factory that the custom activity needs to run on Azure Batch VMs.</span></span>
   * <span data-ttu-id="c11fd-350">**isPaused** egenskap är inställd på **FALSKT** som standard.</span><span class="sxs-lookup"><span data-stu-id="c11fd-350">**isPaused** property is set to **false** by default.</span></span> <span data-ttu-id="c11fd-351">Pipelinen körs direkt i det här exemplet eftersom sektorerna starta tidigare.</span><span class="sxs-lookup"><span data-stu-id="c11fd-351">The pipeline runs immediately in this example because the slices start in the past.</span></span> <span data-ttu-id="c11fd-352">Du kan ange egenskapen till true för att pausa pipeline och ställa tillbaka till false om du vill starta om.</span><span class="sxs-lookup"><span data-stu-id="c11fd-352">You can set this property to true to pause the pipeline and set it back to false to restart.</span></span>
   * <span data-ttu-id="c11fd-353">Den **starta** tid och **end** tider är **fem** timmar från varandra och segment produceras varje timma, så att fem segment produceras av pipeline.</span><span class="sxs-lookup"><span data-stu-id="c11fd-353">The **start** time and **end** times are **five** hours apart and slices are produced hourly, so five slices are produced by the pipeline.</span></span>
3. <span data-ttu-id="c11fd-354">Om du vill distribuera pipeline, klickar du på **distribuera** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-354">To deploy the pipeline, click **Deploy** on the command bar.</span></span>

### <a name="monitor-the-pipeline"></a><span data-ttu-id="c11fd-355">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="c11fd-355">Monitor the pipeline</span></span>
1. <span data-ttu-id="c11fd-356">I bladet Data Factory i Azure-portalen klickar du på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-356">In the Data Factory blade in the Azure portal, click **Diagram**.</span></span>

    ![Ikonen Diagram](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. <span data-ttu-id="c11fd-358">Klicka på OutputDataset i diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-358">In the Diagram View, now click the OutputDataset.</span></span>

    ![Diagramvy](./media/data-factory-use-custom-activities/diagram.png)
3. <span data-ttu-id="c11fd-360">Du bör se att fem utdata-segment är i tillståndet redo.</span><span class="sxs-lookup"><span data-stu-id="c11fd-360">You should see that the five output slices are in the Ready state.</span></span> <span data-ttu-id="c11fd-361">Om de inte är i tillståndet klar inte har de producerats ännu.</span><span class="sxs-lookup"><span data-stu-id="c11fd-361">If they are not in the Ready state, they haven't been produced yet.</span></span> 

   ![Utdata segment](./media/data-factory-use-custom-activities/OutputSlices.png)
4. <span data-ttu-id="c11fd-363">Kontrollera att utdatafilerna genereras i blobblagring i den **adftutorial** behållare.</span><span class="sxs-lookup"><span data-stu-id="c11fd-363">Verify that the output files are generated in the blob storage in the **adftutorial** container.</span></span>

   ![utdata från anpassad aktivitet][image-data-factory-ouput-from-custom-activity]
5. <span data-ttu-id="c11fd-365">Om du öppnar filen bör du se utdata som liknar följande utdata:</span><span class="sxs-lookup"><span data-stu-id="c11fd-365">If you open the output file, you should see the output similar to the following output:</span></span>

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
    ```
6. <span data-ttu-id="c11fd-366">Använd den [Azure-portalen] [ azure-preview-portal] eller Azure PowerShell-cmdletar för att övervaka din data factory, rörledningar och datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="c11fd-366">Use the [Azure portal][azure-preview-portal] or Azure PowerShell cmdlets to monitor your data factory, pipelines, and data sets.</span></span> <span data-ttu-id="c11fd-367">Du kan ta emot meddelanden från den **ActivityLogger** i koden för den anpassade aktiviteten i loggarna (särskilt användar-0.log) som du kan hämta från portalen eller med cmdlet: ar.</span><span class="sxs-lookup"><span data-stu-id="c11fd-367">You can see messages from the **ActivityLogger** in the code for the custom activity in the logs (specifically user-0.log) that you can download from the portal or using cmdlets.</span></span>

   ![Hämta loggar från anpassad aktivitet][image-data-factory-download-logs-from-custom-activity]

<span data-ttu-id="c11fd-369">Se [övervaka och hantera Pipelines](data-factory-monitor-manage-pipelines.md) detaljerade anvisningar för att övervaka datamängd och pipelinor.</span><span class="sxs-lookup"><span data-stu-id="c11fd-369">See [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md) for detailed steps for monitoring datasets and pipelines.</span></span>      

## <a name="data-factory-project-in-visual-studio"></a><span data-ttu-id="c11fd-370">Data Factory-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c11fd-370">Data Factory project in Visual Studio</span></span>  
<span data-ttu-id="c11fd-371">Du kan skapa och publicera Data Factory-enheter med hjälp av Visual Studio istället för att använda Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-371">You can create and publish Data Factory entities by using Visual Studio instead of using Azure portal.</span></span> <span data-ttu-id="c11fd-372">För detaljerad information om hur du skapar och publicerar Data Factory-enheter med hjälp av Visual Studio finns [skapa din första pipeline med Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) och [kopiera data från Azure Blob till Azure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="c11fd-372">For detailed information about creating and publishing Data Factory entities by using Visual Studio, See [Build your first pipeline using Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) and [Copy data from Azure Blob to Azure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) articles.</span></span>

<span data-ttu-id="c11fd-373">Gör följande om du skapar Data Factory-projekt i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c11fd-373">Do the following additional steps if you are creating Data Factory project in Visual Studio:</span></span>
 
1. <span data-ttu-id="c11fd-374">Lägga till Data Factory-projekt i Visual Studio-lösning som innehåller anpassad aktivitet projektet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-374">Add the Data Factory project to the Visual Studio solution that contains the custom activity project.</span></span> 
2. <span data-ttu-id="c11fd-375">Lägg till en referens till projektet .NET aktivitet från Data Factory-projekt.</span><span class="sxs-lookup"><span data-stu-id="c11fd-375">Add a reference to the .NET activity project from the Data Factory project.</span></span> <span data-ttu-id="c11fd-376">Högerklicka på Data Factory-projektet, peka på **Lägg till**, och klicka sedan på **referens**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-376">Right-click Data Factory project, point to **Add**, and then click **Reference**.</span></span> 
3. <span data-ttu-id="c11fd-377">I den **Lägg till referens** dialogrutan markerar du den **MyDotNetActivity** projektet och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-377">In the **Add Reference** dialog box, select the **MyDotNetActivity** project, and click **OK**.</span></span>
4. <span data-ttu-id="c11fd-378">Skapa och publicera lösningen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-378">Build and publish the solution.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c11fd-379">När du publicerar Data Factory-entiteter, en zip-fil skapas automatiskt för dig och överförs till blob-behållaren: customactivitycontainer.</span><span class="sxs-lookup"><span data-stu-id="c11fd-379">When you publish Data Factory entities, a zip file is automatically created for you and is uploaded to the blob container: customactivitycontainer.</span></span> <span data-ttu-id="c11fd-380">Om blob-behållaren inte finns, skapas den automatiskt för.</span><span class="sxs-lookup"><span data-stu-id="c11fd-380">If the blob container does not exist, it is automatically created too.</span></span>  


## <a name="data-factory-and-batch-integration"></a><span data-ttu-id="c11fd-381">Data Factory och Batch-integrering</span><span class="sxs-lookup"><span data-stu-id="c11fd-381">Data Factory and Batch integration</span></span>
<span data-ttu-id="c11fd-382">Data Factory-tjänsten skapar ett jobb med namnet i Azure Batch: **poolname adf: jobb-xxx**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-382">The Data Factory service creates a job in Azure Batch with the name: **adf-poolname: job-xxx**.</span></span> <span data-ttu-id="c11fd-383">Klicka på **jobb** i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-383">Click **Jobs** from the left menu.</span></span> 

![Azure Data Factory - batchjobb](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

<span data-ttu-id="c11fd-385">En aktivitet skapas för varje aktivitet kör för ett segment.</span><span class="sxs-lookup"><span data-stu-id="c11fd-385">A task is created for each activity run of a slice.</span></span> <span data-ttu-id="c11fd-386">Om det finns fem segment som är redo att bearbetas, skapas fem uppgifter i jobbet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-386">If there are five slices ready to be processed, five tasks are created in this job.</span></span> <span data-ttu-id="c11fd-387">Om det finns flera compute-noder i Batch-pool, kan två eller flera segment köras parallellt.</span><span class="sxs-lookup"><span data-stu-id="c11fd-387">If there are multiple compute nodes in the Batch pool, two or more slices can run in parallel.</span></span> <span data-ttu-id="c11fd-388">Du kan också ha fler än ett segment som körs på samma beräkningar om de maximala uppgifterna per compute-nod är > 1.</span><span class="sxs-lookup"><span data-stu-id="c11fd-388">If the maximum tasks per compute node is set to > 1, you can also have more than one slice running on the same compute.</span></span>

![Azure Data Factory - jobbet batchaktiviteter](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

<span data-ttu-id="c11fd-390">Följande diagram illustrerar förhållandet mellan Azure Data Factory och Batch uppgifter.</span><span class="sxs-lookup"><span data-stu-id="c11fd-390">The following diagram illustrates the relationship between Azure Data Factory and Batch tasks.</span></span>

![Data Factory & Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a><span data-ttu-id="c11fd-392">Felsöka fel</span><span class="sxs-lookup"><span data-stu-id="c11fd-392">Troubleshoot failures</span></span>
<span data-ttu-id="c11fd-393">Felsökning består av några grundläggande tekniker:</span><span class="sxs-lookup"><span data-stu-id="c11fd-393">Troubleshooting consists of a few basic techniques:</span></span>

1. <span data-ttu-id="c11fd-394">Om du ser följande fel kan kanske du använder en aktiv/lågfrekvent blob-lagring i stället för med ett allmänt Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="c11fd-394">If you see the following error, you may be using a Hot/Cool blob storage instead of using a general-purpose Azure blob storage.</span></span> <span data-ttu-id="c11fd-395">Ladda upp zip-filen till en **allmänna Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-395">Upload the zip file to a **general-purpose Azure Storage Account**.</span></span> 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of the specified Azure Blob(s).
    ``` 
2. <span data-ttu-id="c11fd-396">Om du ser följande fel, bekräfta att namnet på klassen i filen CS matchar det namn du angav för den **EntryPoint** egenskap i pipeline-JSON.</span><span class="sxs-lookup"><span data-stu-id="c11fd-396">If you see the following error, confirm that the name of the class in the CS file matches the name you specified for the **EntryPoint** property in the pipeline JSON.</span></span> <span data-ttu-id="c11fd-397">I den här genomgången är namnet på klassen: MyDotNetActivity och EntryPoint i JSON: MyDotNetActivityNS. **MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-397">In the walkthrough, name of the class is: MyDotNetActivity, and the EntryPoint in the JSON is: MyDotNetActivityNS.**MyDotNetActivity**.</span></span>

    ```
    MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   <span data-ttu-id="c11fd-398">Om namnen stämmer överens, bekräfta att alla binärfiler i **rotmappen** i zip-filen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-398">If the names do match, confirm that all the binaries are in the **root folder** of the zip file.</span></span> <span data-ttu-id="c11fd-399">När du öppnar zip-filen som är bör du se alla filer i rotmappen, inte i alla undermappar.</span><span class="sxs-lookup"><span data-stu-id="c11fd-399">That is, when you open the zip file, you should see all the files in the root folder, not in any sub folders.</span></span>   
3. <span data-ttu-id="c11fd-400">Om inkommande segmentet är inte **klar**, bekräfta att inkommande mappstrukturen är korrekt och **fil.txt** finns i mapparna inkommande.</span><span class="sxs-lookup"><span data-stu-id="c11fd-400">If the input slice is not set to **Ready**, confirm that the input folder structure is correct and **file.txt** exists in the input folders.</span></span>
3. <span data-ttu-id="c11fd-401">I den **kör** metod för din anpassade aktivitet, Använd den **IActivityLogger** objekt att logga information som hjälper dig att felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="c11fd-401">In the **Execute** method of your custom activity, use the **IActivityLogger** object to log information that helps you troubleshoot issues.</span></span> <span data-ttu-id="c11fd-402">De loggade meddelandena visas i loggfilerna för användare (en eller flera filer med namnet: användaren 0.log, användaren 1.log, användaren 2.log osv.).</span><span class="sxs-lookup"><span data-stu-id="c11fd-402">The logged messages show up in the user log files (one or more files named: user-0.log, user-1.log, user-2.log, etc.).</span></span>

   <span data-ttu-id="c11fd-403">I den **OutputDataset** bladet, klickar du på sektorn att se den **DATASEKTORN** bladet för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-403">In the **OutputDataset** blade, click the slice to see the **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="c11fd-404">Du ser **aktivitetskörningar** för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-404">You see **activity runs** for that slice.</span></span> <span data-ttu-id="c11fd-405">Du bör se en aktivitet som körs för sektorn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-405">You should see one activity run for the slice.</span></span> <span data-ttu-id="c11fd-406">Om du klickar på Kör i kommandofältet kan du starta en annan aktivitet som körs för samma segment.</span><span class="sxs-lookup"><span data-stu-id="c11fd-406">If you click Run in the command bar, you can start another activity run for the same slice.</span></span>

   <span data-ttu-id="c11fd-407">När du klickar på aktiviteten kör du ser den **kör AKTIVITETSINFORMATION** bladet med en lista över loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="c11fd-407">When you click the activity run, you see the **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="c11fd-408">Du ser loggade meddelanden i filen user_0.log.</span><span class="sxs-lookup"><span data-stu-id="c11fd-408">You see logged messages in the user_0.log file.</span></span> <span data-ttu-id="c11fd-409">När ett fel uppstår finns tre aktivitetskörningar eftersom antalet nya försök är inställd på 3 i pipeline/aktivitet JSON.</span><span class="sxs-lookup"><span data-stu-id="c11fd-409">When an error occurs, you see three activity runs because the retry count is set to 3 in the pipeline/activity JSON.</span></span> <span data-ttu-id="c11fd-410">När du klickar på kör aktiviteten finns i loggfilerna som du kan granska för att felsöka felet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-410">When you click the activity run, you see the log files that you can review to troubleshoot the error.</span></span>

   <span data-ttu-id="c11fd-411">I listan över loggfiler, klickar du på den **användare 0.log**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-411">In the list of log files, click the **user-0.log**.</span></span> <span data-ttu-id="c11fd-412">I den högra panelen är resultatet av att använda den **IActivityLogger.Write** metod.</span><span class="sxs-lookup"><span data-stu-id="c11fd-412">In the right panel are the results of using the **IActivityLogger.Write** method.</span></span> <span data-ttu-id="c11fd-413">Om du inte ser alla meddelanden, kontrollera om du har flera loggfiler med namnet: user_1.log, user_2.log osv. Koden kan annars kan ha misslyckats efter senaste loggat meddelandet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-413">If you don't see all messages, check if you have more log files named: user_1.log, user_2.log etc. Otherwise, the code may have failed after the last logged message.</span></span>

   <span data-ttu-id="c11fd-414">Du kan också kontrollera **system 0.log** för alla system felmeddelanden och undantag.</span><span class="sxs-lookup"><span data-stu-id="c11fd-414">In addition, check **system-0.log** for any system error messages and exceptions.</span></span>
4. <span data-ttu-id="c11fd-415">Inkludera den **PDB** filen i zip-filen så att felinformation har information som **anropsstacken** när ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="c11fd-415">Include the **PDB** file in the zip file so that the error details have information such as **call stack** when an error occurs.</span></span>
5. <span data-ttu-id="c11fd-416">Alla filer i zip-filen för den anpassade aktiviteten måste vara på den **översta nivån** utan undermappar.</span><span class="sxs-lookup"><span data-stu-id="c11fd-416">All the files in the zip file for the custom activity must be at the **top level** with no sub folders.</span></span>
6. <span data-ttu-id="c11fd-417">Se till att den **assemblyName** (MyDotNetActivity.dll) **entryPoint**(MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer/MyDotNetActivity.zip) och **packageLinkedService** (måste peka på den **allmänna**Azure blob-lagring som innehåller zip-filen) är inställda på rätt värden.</span><span class="sxs-lookup"><span data-stu-id="c11fd-417">Ensure that the **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point to the **general-purpose**Azure blob storage that contains the zip file) are set to correct values.</span></span>
7. <span data-ttu-id="c11fd-418">Om du har korrigerat ett fel och vill bearbeta sektorn på nytt, högerklickar du på sektorn i **OutputDataset**-bladet och klickar på **Kör**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-418">If you fixed an error and want to reprocess the slice, right-click the slice in the **OutputDataset** blade and click **Run**.</span></span>
8. <span data-ttu-id="c11fd-419">Om du ser följande fel använder du Azure Storage-paketet med version > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="c11fd-419">If you see the following error, you are using the Azure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="c11fd-420">Startprogram för data Factory-tjänsten kräver 4.3 version av WindowsAzure.Storage.</span><span class="sxs-lookup"><span data-stu-id="c11fd-420">Data Factory service launcher requires the 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="c11fd-421">Se [Appdomain isolering](#appdomain-isolation) avsnittet för en arbete runt om du måste använda den senare versionen av Azure Storage-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-421">See [Appdomain isolation](#appdomain-isolation) section for a work-around if you must use the later version of Azure Storage assembly.</span></span> 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    <span data-ttu-id="c11fd-422">Om du kan använda 4.3.0 versionen av Azure Storage, ta bort befintliga referensen till Azure Storage-paketet med version > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="c11fd-422">If you can use the 4.3.0 version of Azure Storage package, remove the existing reference to Azure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="c11fd-423">Kör sedan följande kommando från NuGet Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-423">Then, run the following command from NuGet Package Manager Console.</span></span> 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    <span data-ttu-id="c11fd-424">Bygga projektet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-424">Build the project.</span></span> <span data-ttu-id="c11fd-425">Ta bort Azure.Storage sammansättningen av version > 4.3.0 från mappen bin\Debug.</span><span class="sxs-lookup"><span data-stu-id="c11fd-425">Delete Azure.Storage assembly of version > 4.3.0 from the bin\Debug folder.</span></span> <span data-ttu-id="c11fd-426">Skapa en zip-fil med binärfiler och PDB-filen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-426">Create a zip file with binaries and the PDB file.</span></span> <span data-ttu-id="c11fd-427">Ersätt den gamla zipfilen med den här i blob-behållaren (customactivitycontainer).</span><span class="sxs-lookup"><span data-stu-id="c11fd-427">Replace the old zip file with this one in the blob container (customactivitycontainer).</span></span> <span data-ttu-id="c11fd-428">Köra sektorerna misslyckades (Högerklicka segment och klicka på Kör).</span><span class="sxs-lookup"><span data-stu-id="c11fd-428">Rerun the slices that failed (right-click slice, and click Run).</span></span>   
8. <span data-ttu-id="c11fd-429">Den anpassade aktiviteten använder inte den **app.config** filen från paketet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-429">The custom activity does not use the **app.config** file from your package.</span></span> <span data-ttu-id="c11fd-430">Därför om koden läser eventuella anslutningssträngar i konfigurationsfilen, fungerar det inte vid körning.</span><span class="sxs-lookup"><span data-stu-id="c11fd-430">Therefore, if your code reads any connection strings from the configuration file, it does not work at runtime.</span></span> <span data-ttu-id="c11fd-431">Bästa praxis och när du använder Azure Batch är att hålla alla hemligheter i en **Azure KeyVault**, använda en certifikatbaserad tjänstens huvudnamn för att skydda den **keyvault**, och distribuera certifikat till Azure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="c11fd-431">The best practice when using Azure Batch is to hold any secrets in an **Azure KeyVault**, use a certificate-based service principal to protect the **keyvault**, and distribute the certificate to Azure Batch pool.</span></span> <span data-ttu-id="c11fd-432">Den anpassade .NET-aktiviteten kan sedan komma åt hemligheter i KeyVault vid körning.</span><span class="sxs-lookup"><span data-stu-id="c11fd-432">The .NET custom activity then can access secrets from the KeyVault at runtime.</span></span> <span data-ttu-id="c11fd-433">Den här lösningen är en allmän lösning och kan skalas till alla typer av hemlighet, inte bara anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-433">This solution is a generic solution and can scale to any type of secret, not just connection string.</span></span>

   <span data-ttu-id="c11fd-434">Det finns en enklare lösning (men inte rekommenderas): du kan skapa en **Azure SQL länkade tjänsten** skapa en datamängd som använder den länkade tjänsten med inställningar för sträng och kedja datamängden som en dummy inkommande datauppsättning för anpassad .NET-aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-434">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses the linked service, and chain the dataset as a dummy input dataset to the custom .NET activity.</span></span> <span data-ttu-id="c11fd-435">Du kan sedan komma åt den länkade tjänsten anslutningssträngen i koden anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c11fd-435">You can then access the linked service's connection string in the custom activity code.</span></span>  

## <a name="update-custom-activity"></a><span data-ttu-id="c11fd-436">Uppdatera anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="c11fd-436">Update custom activity</span></span>
<span data-ttu-id="c11fd-437">Om du uppdaterar koden för den anpassade aktiviteten, skapa den och ladda upp zip-filen som innehåller nya binärfiler till blob storage.</span><span class="sxs-lookup"><span data-stu-id="c11fd-437">If you update the code for the custom activity, build it, and upload the zip file that contains new binaries to the blob storage.</span></span>

## <a name="appdomain-isolation"></a><span data-ttu-id="c11fd-438">AppDomain isolering</span><span class="sxs-lookup"><span data-stu-id="c11fd-438">Appdomain isolation</span></span>
<span data-ttu-id="c11fd-439">Se [mellan AppDomain-exempel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) som visar hur du skapar en anpassad aktivitet som inte är begränsad till sammansättningen versioner som används av Data Factory-programstart (exempel: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x osv.).</span><span class="sxs-lookup"><span data-stu-id="c11fd-439">See [Cross AppDomain Sample](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) that shows you how to create a custom activity that is not constrained to assembly versions used by the Data Factory launcher (example: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, etc.).</span></span>

## <a name="access-extended-properties"></a><span data-ttu-id="c11fd-440">Åtkomst till utökade egenskaper</span><span class="sxs-lookup"><span data-stu-id="c11fd-440">Access extended properties</span></span>
<span data-ttu-id="c11fd-441">Du kan deklarera utökade egenskaper i aktivitets-JSON som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c11fd-441">You can declare extended properties in the activity JSON as shown in the following sample:</span></span>

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```


<span data-ttu-id="c11fd-442">Det här exemplet finns det två utökade egenskaper: **SliceStart** och **DataFactoryName**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-442">In the example, there are two extended properties: **SliceStart** and **DataFactoryName**.</span></span> <span data-ttu-id="c11fd-443">Värdet för SliceStart baseras på variabeln SliceStart system.</span><span class="sxs-lookup"><span data-stu-id="c11fd-443">The value for SliceStart is based on the SliceStart system variable.</span></span> <span data-ttu-id="c11fd-444">Se [systemvariabler](data-factory-functions-variables.md) för en lista över variabler system som stöds.</span><span class="sxs-lookup"><span data-stu-id="c11fd-444">See [System Variables](data-factory-functions-variables.md) for a list of supported system variables.</span></span> <span data-ttu-id="c11fd-445">Värdet för DataFactoryName är hårdkodat till CustomActivityFactory.</span><span class="sxs-lookup"><span data-stu-id="c11fd-445">The value for DataFactoryName is hard-coded to CustomActivityFactory.</span></span>

<span data-ttu-id="c11fd-446">Åtkomst till dessa utökade egenskaper i den **Execute** metod, Använd koden liknar följande kod:</span><span class="sxs-lookup"><span data-stu-id="c11fd-446">To access these extended properties in the **Execute** method, use code similar to the following code:</span></span>

```csharp
// to get extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// to log all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a><span data-ttu-id="c11fd-447">Automatisk skalning av Azure Batch</span><span class="sxs-lookup"><span data-stu-id="c11fd-447">Auto-scaling of Azure Batch</span></span>
<span data-ttu-id="c11fd-448">Du kan också skapa en Azure Batch-pool med **Autoskala** funktion.</span><span class="sxs-lookup"><span data-stu-id="c11fd-448">You can also create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="c11fd-449">Du kan till exempel skapa en azure batch-pool med 0 dedikerade virtuella datorer och en Autoskala formel baserat på antalet väntande åtgärder.</span><span class="sxs-lookup"><span data-stu-id="c11fd-449">For example, you could create an azure batch pool with 0 dedicated VMs and an autoscale formula based on the number of pending tasks.</span></span> 

<span data-ttu-id="c11fd-450">Exempel formeln här uppnår på följande: När poolen skapas, det börjar med 1 VM.</span><span class="sxs-lookup"><span data-stu-id="c11fd-450">The sample formula here achieves the following behavior: When the pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="c11fd-451">$PendingTasks mått definierar antalet aktiviteter i körs + aktiv (i kö) tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c11fd-451">$PendingTasks metric defines the number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="c11fd-452">Formeln hittar det genomsnittliga antalet väntande aktiviteter under de senaste 180 sekunderna och anger därefter TargetDedicated.</span><span class="sxs-lookup"><span data-stu-id="c11fd-452">The formula finds the average number of pending tasks in the last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="c11fd-453">Det garanterar att TargetDedicated aldrig är mer omfattande än 25 virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c11fd-453">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="c11fd-454">Så när nya aktiviteter skickas pool växer automatiskt och som slutförda uppgifter, virtuella datorer blir ledigt i taget och autoskalning krymper dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c11fd-454">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and the autoscaling shrinks those VMs.</span></span> <span data-ttu-id="c11fd-455">startingNumberOfVMs och maxNumberofVMs kan justeras efter dina behov.</span><span class="sxs-lookup"><span data-stu-id="c11fd-455">startingNumberOfVMs and maxNumberofVMs can be adjusted to your needs.</span></span>

<span data-ttu-id="c11fd-456">Autoskala formel:</span><span class="sxs-lookup"><span data-stu-id="c11fd-456">Autoscale formula:</span></span>

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

<span data-ttu-id="c11fd-457">Se [automatiskt skala compute-noder i en Azure Batch-pool](../batch/batch-automatic-scaling.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="c11fd-457">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

<span data-ttu-id="c11fd-458">Om poolen använder standard [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), Batch-tjänsten kan ta 15 30 minuter att förbereda den virtuella datorn innan du kör den anpassade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-458">If the pool is using the default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), the Batch service could take 15-30 minutes to prepare the VM before running the custom activity.</span></span>  <span data-ttu-id="c11fd-459">Om poolen använder en annan autoScaleEvaluationInterval, kan Batch-tjänsten ta autoScaleEvaluationInterval + 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="c11fd-459">If the pool is using a different autoScaleEvaluationInterval, the Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>

## <a name="use-hdinsight-compute-service"></a><span data-ttu-id="c11fd-460">Använda HDInsight beräknings-tjänst</span><span class="sxs-lookup"><span data-stu-id="c11fd-460">Use HDInsight compute service</span></span>
<span data-ttu-id="c11fd-461">I den här genomgången används Azure Batch beräkning för att köra den anpassade aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-461">In the walkthrough, you used Azure Batch compute to run the custom activity.</span></span> <span data-ttu-id="c11fd-462">Du kan också använda egna Windows-baserade HDInsight-kluster eller har Data Factory skapar på-begäran-Windows-baserade HDInsight-kluster och har den anpassade aktiviteten körs på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="c11fd-462">You can also use your own Windows-based HDInsight cluster or have Data Factory create an on-demand Windows-based HDInsight cluster and have the custom activity run on the HDInsight cluster.</span></span> <span data-ttu-id="c11fd-463">Här följer anvisningar för att använda ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-463">Here are the high-level steps for using an HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c11fd-464">Anpassad .NET-aktiviteter körs bara på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-464">The custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="c11fd-465">En lösning för den här begränsningen är att använda aktiviteten mappa minska för att köra anpassad Java-kod på en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-465">A workaround for this limitation is to use the Map Reduce Activity to run custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="c11fd-466">Ett annat alternativ är att använda en Azure Batch-pool för virtuella datorer för att köra anpassade aktiviteter i stället för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-466">Another option is to use an Azure Batch pool of VMs to run custom activities instead of using a HDInsight cluster.</span></span>
 

1. <span data-ttu-id="c11fd-467">Skapa en Azure HDInsight länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="c11fd-467">Create an Azure HDInsight linked service.</span></span>   
2. <span data-ttu-id="c11fd-468">Använd HDInsight länkade tjänsten i stället för **AzureBatchLinkedService** i pipeline-JSON.</span><span class="sxs-lookup"><span data-stu-id="c11fd-468">Use HDInsight linked service in place of **AzureBatchLinkedService** in the pipeline JSON.</span></span>

<span data-ttu-id="c11fd-469">Om du vill testa den med den här genomgången, ändra **starta** och **end** tidsgränsen för pipeline så att du kan testa scenariot med tjänsten Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c11fd-469">If you want to test it with the walkthrough, change **start** and **end** times for the pipeline so that you can test the scenario with the Azure HDInsight service.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="c11fd-470">Skapa en Azure HDInsight-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="c11fd-470">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="c11fd-471">Azure Data Factory-tjänsten stöder skapandet av ett kluster på begäran och använda den för att behandla indata för att gav inga utdata.</span><span class="sxs-lookup"><span data-stu-id="c11fd-471">The Azure Data Factory service supports creation of an on-demand cluster and use it to process input to produce output data.</span></span> <span data-ttu-id="c11fd-472">Du kan också använda ditt eget kluster för att utföra samma.</span><span class="sxs-lookup"><span data-stu-id="c11fd-472">You can also use your own cluster to perform the same.</span></span> <span data-ttu-id="c11fd-473">När du använder HDInsight-kluster på begäran, skapas ett kluster för varje segment.</span><span class="sxs-lookup"><span data-stu-id="c11fd-473">When you use on-demand HDInsight cluster, a cluster gets created for each slice.</span></span> <span data-ttu-id="c11fd-474">Om du använder egna HDInsight-kluster i klustret är redo att utföra sektorn omedelbart.</span><span class="sxs-lookup"><span data-stu-id="c11fd-474">Whereas, if you use your own HDInsight cluster, the cluster is ready to process the slice immediately.</span></span> <span data-ttu-id="c11fd-475">Därför när du använder kluster på begäran måste kanske du inte visas utdata så snabbt som när du använder ditt eget kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-475">Therefore, when you use on-demand cluster, you may not see the output data as quickly as when you use your own cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="c11fd-476">Vid körning körs en instans av en .NET-aktiviteten bara på en worker-nod i HDInsight-klustret; den kan inte skalas för att köras på flera noder.</span><span class="sxs-lookup"><span data-stu-id="c11fd-476">At runtime, an instance of a .NET activity runs only on one worker node in the HDInsight cluster; it cannot be scaled to run on multiple nodes.</span></span> <span data-ttu-id="c11fd-477">Flera instanser av .NET-aktiviteten kan köras parallellt på olika noder i HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="c11fd-477">Multiple instances of .NET activity can run in parallel on different nodes of the HDInsight cluster.</span></span>
>
>

##### <a name="to-use-an-on-demand-hdinsight-cluster"></a><span data-ttu-id="c11fd-478">Att använda ett HDInsight-kluster på begäran</span><span class="sxs-lookup"><span data-stu-id="c11fd-478">To use an on-demand HDInsight cluster</span></span>
1. <span data-ttu-id="c11fd-479">I den **Azure-portalen**, klickar du på **författare och distribution** i Data Factory-startsidan.</span><span class="sxs-lookup"><span data-stu-id="c11fd-479">In the **Azure portal**, click **Author and Deploy** in the Data Factory home page.</span></span>
2. <span data-ttu-id="c11fd-480">I den Data Factory-redigeraren, klicka på **nya beräkning** kommandofältet och välj **på begäran HDInsight-kluster** på menyn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-480">In the Data Factory Editor, click **New compute** from the command bar and select **On-demand HDInsight cluster** from the menu.</span></span>
3. <span data-ttu-id="c11fd-481">Gör följande ändringar i JSON-skriptet:</span><span class="sxs-lookup"><span data-stu-id="c11fd-481">Make the following changes to the JSON script:</span></span>

   1. <span data-ttu-id="c11fd-482">För den **clusterSize** -egenskapen anger storleken på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="c11fd-482">For the **clusterSize** property, specify the size of the HDInsight cluster.</span></span>
   2. <span data-ttu-id="c11fd-483">För den **timeToLive** -egenskapen anger hur länge kunden kan vara inaktiv innan den tas bort.</span><span class="sxs-lookup"><span data-stu-id="c11fd-483">For the **timeToLive** property, specify how long the customer can be idle before it is deleted.</span></span>
   3. <span data-ttu-id="c11fd-484">För den **version** egenskap, ange HDInsight-version som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="c11fd-484">For the **version** property, specify the HDInsight version you want to use.</span></span> <span data-ttu-id="c11fd-485">Om du utesluter den här egenskapen används den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="c11fd-485">If you exclude this property, the latest version is used.</span></span>  
   4. <span data-ttu-id="c11fd-486">För den **linkedServiceName**, ange **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-486">For the **linkedServiceName**, specify **AzureStorageLinkedService**.</span></span>

        ```JSON
        {
           "name": "HDInsightOnDemandLinkedService",
           "properties": {
               "type": "HDInsightOnDemand",
               "typeProperties": {
                   "clusterSize": 4,
                   "timeToLive": "00:05:00",
                   "osType": "Windows",
                   "linkedServiceName": "AzureStorageLinkedService",
               }
           }
        }
        ```

    > [!IMPORTANT]
    > <span data-ttu-id="c11fd-487">Anpassad .NET-aktiviteter körs bara på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-487">The custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="c11fd-488">En lösning för den här begränsningen är att använda aktiviteten mappa minska för att köra anpassad Java-kod på en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-488">A workaround for this limitation is to use the Map Reduce Activity to run custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="c11fd-489">Ett annat alternativ är att använda en Azure Batch-pool för virtuella datorer för att köra anpassade aktiviteter i stället för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="c11fd-489">Another option is to use an Azure Batch pool of VMs to run custom activities instead of using a HDInsight cluster.</span></span>

4. <span data-ttu-id="c11fd-490">Klicka på **Distribuera** i kommandofältet för att distribuera den länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-490">Click **Deploy** on the command bar to deploy the linked service.</span></span>

##### <a name="to-use-your-own-hdinsight-cluster"></a><span data-ttu-id="c11fd-491">Använda din egen HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="c11fd-491">To use your own HDInsight cluster:</span></span>
1. <span data-ttu-id="c11fd-492">I den **Azure-portalen**, klickar du på **författare och distribution** i Data Factory-startsidan.</span><span class="sxs-lookup"><span data-stu-id="c11fd-492">In the **Azure portal**, click **Author and Deploy** in the Data Factory home page.</span></span>
2. <span data-ttu-id="c11fd-493">I den **Data Factory-redigeraren**, klickar du på **nya beräkning** kommandofältet och välj **HDInsight-kluster** på menyn.</span><span class="sxs-lookup"><span data-stu-id="c11fd-493">In the **Data Factory Editor**, click **New compute** from the command bar and select **HDInsight cluster** from the menu.</span></span>
3. <span data-ttu-id="c11fd-494">Gör följande ändringar i JSON-skriptet:</span><span class="sxs-lookup"><span data-stu-id="c11fd-494">Make the following changes to the JSON script:</span></span>

   1. <span data-ttu-id="c11fd-495">För den **clusterUri** egenskap, ange URL för din HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c11fd-495">For the **clusterUri** property, enter the URL for your HDInsight.</span></span> <span data-ttu-id="c11fd-496">Till exempel: https://<clustername>.azurehdinsight.net/</span><span class="sxs-lookup"><span data-stu-id="c11fd-496">For example: https://<clustername>.azurehdinsight.net/</span></span>     
   2. <span data-ttu-id="c11fd-497">För den **användarnamn** egenskap, ange namnet på användaren som har åtkomst till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="c11fd-497">For the **UserName** property, enter the user name who has access to the HDInsight cluster.</span></span>
   3. <span data-ttu-id="c11fd-498">För den **lösenord** egenskap, ange lösenordet för användaren.</span><span class="sxs-lookup"><span data-stu-id="c11fd-498">For the **Password** property, enter the password for the user.</span></span>
   4. <span data-ttu-id="c11fd-499">För den **LinkedServiceName** egenskap, ange **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="c11fd-499">For the **LinkedServiceName** property, enter **AzureStorageLinkedService**.</span></span>
4. <span data-ttu-id="c11fd-500">Klicka på **Distribuera** i kommandofältet för att distribuera den länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c11fd-500">Click **Deploy** on the command bar to deploy the linked service.</span></span>

<span data-ttu-id="c11fd-501">Se [Compute länkade tjänster](data-factory-compute-linked-services.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="c11fd-501">See [Compute linked services](data-factory-compute-linked-services.md) for details.</span></span>

<span data-ttu-id="c11fd-502">I den **pipeline-JSON**, använda HDInsight (på begäran eller egna) länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="c11fd-502">In the **pipeline JSON**, use HDInsight (on-demand or your own) linked service:</span></span>

```JSON
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "HDInsightOnDemandLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00Z",
    "end": "2016-11-16T05:00:00Z",
    "isPaused": false
  }
}
```

## <a name="create-a-custom-activity-by-using-net-sdk"></a><span data-ttu-id="c11fd-503">Skapa en anpassad aktivitet med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c11fd-503">Create a custom activity by using .NET SDK</span></span>
<span data-ttu-id="c11fd-504">I den här genomgången i den här artikeln skapa en datafabrik med en pipeline som använder den anpassade aktiviteten med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="c11fd-504">In the walkthrough in this article, you create a data factory with a pipeline that uses the custom activity by using the Azure portal.</span></span> <span data-ttu-id="c11fd-505">Följande kod visar hur du skapar datafabriken med hjälp av .NET SDK i stället.</span><span class="sxs-lookup"><span data-stu-id="c11fd-505">The following code shows you how to create the data factory by using .NET SDK instead.</span></span> <span data-ttu-id="c11fd-506">Du hittar mer information om hur du använder SDK för att skapa programmässigt pipelines i den [skapar en pipeline med kopieringsaktiviteten med hjälp av .NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c11fd-506">You can find more details about using SDK to programmatically create pipelines in the [create a pipeline with copy activity by using .NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md) article.</span></span> 

```csharp
using System;
using System.Configuration;
using System.Collections.ObjectModel;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.DataFactories;
using Microsoft.Azure.Management.DataFactories.Models;
using Microsoft.Azure.Management.DataFactories.Common.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Collections.Generic;

namespace DataFactoryAPITestApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // create data factory management client

            // TODO: replace ADFTutorialResourceGroup with the name of your resource group.
            string resourceGroupName = "ADFTutorialResourceGroup";

            // TODO: replace APITutorialFactory with a name that is globally unique. For example: APITutorialFactory04212017
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader().Result);

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties()
                    }
                }
            );

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: Replace <accountname> and <accountkey> with name and key of your Azure Storage account.
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure Batch linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureBatchLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: replace <batchaccountname> and <yourbatchaccountkey> with name and key of your Azure Batch account
                            new AzureBatchLinkedService("<batchaccountname>", "https://westus.batch.azure.com", "<yourbatchaccountkey>", "myazurebatchpool", "AzureStorageLinkedService")
                        )
                    }
                }
            );

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "InputDataset";
            string Dataset_Destination = "OutputDataset";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Source,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FolderPath = "adftutorial/customactivityinput/",
                                Format = new TextFormat()
                            },
                            External = true,
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },

                            Policy = new Policy() { }
                        }
                    }
                });

            Console.WriteLine("Creating output dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FileName = "{slice}.txt",
                                FolderPath = "adftutorial/customactivityoutput/",
                                PartitionedBy = new List<Partition>()
                                {
                                    new Partition()
                                    {
                                        Name = "slice",
                                        Value = new DateTimePartitionValue()
                                        {
                                            Date = "SliceStart",
                                            Format = "yyyy-MM-dd-HH"
                                        }
                                    }
                                }
                            },
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

            Console.WriteLine("Creating a custom activity pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2017, 3, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipelineCustom";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Use custom activity",

                            // Initial value for pipeline's active period. With this, you won't need to set slice status
                            Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,
                            IsPaused = false,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "MyDotNetActivity",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    LinkedServiceName = "AzureBatchLinkedService",
                                    TypeProperties = new DotNetActivity()
                                    {
                                        AssemblyName = "MyDotNetActivity.dll",
                                        EntryPoint = "MyDotNetActivityNS.MyDotNetActivity",
                                        PackageLinkedService = "AzureStorageLinkedService",
                                        PackageFile = "customactivitycontainer/MyDotNetActivity.zip",
                                        ExtendedProperties = new Dictionary<string, string>()
                                        {
                                            { "SliceStart", "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"}
                                        }
                                    },
                                    Policy = new ActivityPolicy()
                                    {
                                        Concurrency = 2,
                                        ExecutionPriorityOrder = "OldestFirst",
                                        Retry = 3,
                                        Timeout = new TimeSpan(0,0,30,0),
                                        Delay = new TimeSpan()
                                    }
                                }
                            }
                        }
                    }
                });
        }

        public static async Task<string> GetAuthorizationHeader()
        {
            AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
            ClientCredential credential = new ClientCredential(
                ConfigurationManager.AppSettings["ApplicationId"],
                ConfigurationManager.AppSettings["Password"]);
            AuthenticationResult result = await context.AcquireTokenAsync(
                resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                clientCredential: credential);

            if (result != null)
                return result.AccessToken;

            throw new InvalidOperationException("Failed to acquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a><span data-ttu-id="c11fd-507">Felsöka anpassad aktivitet i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c11fd-507">Debug custom activity in Visual Studio</span></span>
<span data-ttu-id="c11fd-508">Den [Azure Data Factory - lokal miljö](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) prov på GitHub innehåller ett verktyg som hjälper dig att felsöka anpassade .NET-aktiviteter i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c11fd-508">The [Azure Data Factory - local environment](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) sample on GitHub includes a tool that allows you to debug custom .NET activities within Visual Studio.</span></span>  


## <a name="sample-custom-activities-on-github"></a><span data-ttu-id="c11fd-509">Exempel anpassade aktiviteter på GitHub</span><span class="sxs-lookup"><span data-stu-id="c11fd-509">Sample custom activities on GitHub</span></span>
| <span data-ttu-id="c11fd-510">Exempel</span><span class="sxs-lookup"><span data-stu-id="c11fd-510">Sample</span></span> | <span data-ttu-id="c11fd-511">Vilka anpassade aktiviteten har</span><span class="sxs-lookup"><span data-stu-id="c11fd-511">What custom activity does</span></span> |
| --- | --- |
| <span data-ttu-id="c11fd-512">[HTTP-Data Installationshämtaren](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span><span class="sxs-lookup"><span data-stu-id="c11fd-512">[HTTP Data Downloader](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span></span> |<span data-ttu-id="c11fd-513">Hämtar data från en HTTP-slutpunkt till Azure Blob Storage med hjälp av anpassade C# aktiviteten i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c11fd-513">Downloads data from an HTTP Endpoint to Azure Blob Storage using custom C# Activity in Data Factory.</span></span> |
| [<span data-ttu-id="c11fd-514">Twitter-Sentiment Analysis-exempel</span><span class="sxs-lookup"><span data-stu-id="c11fd-514">Twitter Sentiment Analysis sample</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |<span data-ttu-id="c11fd-515">Anropar en Azure ML-modell och vill sentiment analys, bedömningen, förutsägelse osv.</span><span class="sxs-lookup"><span data-stu-id="c11fd-515">Invokes an Azure ML model and do sentiment analysis, scoring, prediction etc.</span></span> |
| <span data-ttu-id="c11fd-516">[Kör R-skriptet](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span><span class="sxs-lookup"><span data-stu-id="c11fd-516">[Run R Script](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span></span> |<span data-ttu-id="c11fd-517">Anropar R-skriptet genom att köra RScript.exe på ditt HDInsight-kluster som redan har R installerat på den.</span><span class="sxs-lookup"><span data-stu-id="c11fd-517">Invokes R script by running RScript.exe on your HDInsight cluster that already has R Installed on it.</span></span> |
| [<span data-ttu-id="c11fd-518">Mellan AppDomain .NET-aktiviteten</span><span class="sxs-lookup"><span data-stu-id="c11fd-518">Cross AppDomain .NET Activity</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |<span data-ttu-id="c11fd-519">Använder olika sammansättningen versioner från de som används av Data Factory-starta</span><span class="sxs-lookup"><span data-stu-id="c11fd-519">Uses different assembly versions from ones used by the Data Factory launcher</span></span> |
| [<span data-ttu-id="c11fd-520">Bearbeta en modell i Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="c11fd-520">Reprocess a model in Azure Analysis Services</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  <span data-ttu-id="c11fd-521">Ombearbeta en modell i Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="c11fd-521">Reprocesses a model in Azure Analysis Services.</span></span> |

[batch-net-library]: ../batch/batch-dotnet-get-started.md
[batch-create-account]: ../batch/batch-account-create-portal.md
[batch-technical-overview]: ../batch/batch-technical-overview.md
[batch-get-started]: ../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
