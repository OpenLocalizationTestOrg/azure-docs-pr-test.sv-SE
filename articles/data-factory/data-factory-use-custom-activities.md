---
title: aaaUse anpassade aktiviteter i ett Azure Data Factory-pipelinen
description: "Lär dig hur toocreate anpassade aktiviteter och Använd dem i ett Azure Data Factory-pipelinen."
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
ms.openlocfilehash: 23e33727b2160541ab40938ffd911fdd484b3daa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a><span data-ttu-id="1b7a4-103">Use custom activities in an Azure Data Factory pipeline (Använda anpassade aktiviteter i en Azure Data Factory-pipeline)</span><span class="sxs-lookup"><span data-stu-id="1b7a4-103">Use custom activities in an Azure Data Factory pipeline</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="1b7a4-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="1b7a4-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="1b7a4-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="1b7a4-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="1b7a4-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="1b7a4-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="1b7a4-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="1b7a4-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="1b7a4-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="1b7a4-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="1b7a4-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="1b7a4-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="1b7a4-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="1b7a4-114">Det finns två typer av aktiviteter som du kan använda i ett Azure Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-114">There are two types of activities that you can use in an Azure Data Factory pipeline.</span></span>

- <span data-ttu-id="1b7a4-115">[Data Movement aktiviteter](data-factory-data-movement-activities.md) toomove data mellan [stöds källa och mottagare datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-115">[Data Movement Activities](data-factory-data-movement-activities.md) toomove data between [supported source and sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>
- <span data-ttu-id="1b7a4-116">[Data Transformation aktiviteter](data-factory-data-transformation-activities.md) tootransform data med hjälp av compute-tjänster som Azure HDInsight, Azure Batch och Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-116">[Data Transformation Activities](data-factory-data-transformation-activities.md) tootransform data using compute services such as Azure HDInsight, Azure Batch, and Azure Machine Learning.</span></span> 

<span data-ttu-id="1b7a4-117">Skapa toomove data till/från ett dataarkiv som Data Factory inte stöder en **anpassad aktivitet** med dina egna data movement logik och Använd hello aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-117">toomove data to/from a data store that Data Factory does not support, create a **custom activity** with your own data movement logic and use hello activity in a pipeline.</span></span> <span data-ttu-id="1b7a4-118">På samma sätt kan skapa en anpassad aktivitet med dina egna data transformation logik tootransform/bearbetning av data på ett sätt som inte stöds av Data Factory och använda hello aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-118">Similarly, tootransform/process data in a way that isn't supported by Data Factory, create a custom activity with your own data transformation logic and use hello activity in a pipeline.</span></span> 

<span data-ttu-id="1b7a4-119">Du kan konfigurera en anpassad aktivitet toorun på en **Azure Batch** poolen med virtuella datorer eller en Windows-baserad **Azure HDInsight** klustret.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-119">You can configure a custom activity toorun on an **Azure Batch** pool of virtual machines or a Windows-based **Azure HDInsight** cluster.</span></span> <span data-ttu-id="1b7a4-120">När du använder Azure Batch kan använda du endast en befintlig Azure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-120">When using Azure Batch, you can use only an existing Azure Batch pool.</span></span> <span data-ttu-id="1b7a4-121">När du använder HDInsight kan du använda ett befintligt HDInsight-kluster eller ett kluster som skapas automatiskt för du på-begäran vid körning.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-121">Whereas, when using HDInsight, you can use an existing HDInsight cluster or a cluster that is automatically created for you on-demand at runtime.</span></span>  

<span data-ttu-id="1b7a4-122">hello innehåller följande genomgång stegvisa instruktioner för att skapa en anpassad .NET-aktivitet och använder hello anpassad aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-122">hello following walkthrough provides step-by-step instructions for creating a custom .NET activity and using hello custom activity in a pipeline.</span></span> <span data-ttu-id="1b7a4-123">hello genomgången använder en **Azure Batch** länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-123">hello walkthrough uses an **Azure Batch** linked service.</span></span> <span data-ttu-id="1b7a4-124">toouse ett Azure HDInsight i stället länkade tjänsten skapar du en länkad tjänst av typen **HDInsight** (egna HDInsight-kluster) eller **HDInsightOnDemand** (Data Factory skapar ett HDInsight-kluster på begäran).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-124">toouse an Azure HDInsight linked service instead, you create a linked service of type **HDInsight** (your own HDInsight cluster) or **HDInsightOnDemand** (Data Factory creates an HDInsight cluster on-demand).</span></span> <span data-ttu-id="1b7a4-125">Konfigurera sedan anpassad aktivitet toouse hello länkad HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-125">Then, configure custom activity toouse hello HDInsight linked service.</span></span> <span data-ttu-id="1b7a4-126">Se [Använd Azure HDInsight länkade tjänster](#use-hdinsight-compute-service) information om hur du använder Azure HDInsight toorun hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-126">See [Use Azure HDInsight linked services](#use-hdinsight-compute-service) section for details on using Azure HDInsight toorun hello custom activity.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="1b7a4-127">hello anpassade .NET aktiviteter körs bara på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-127">hello custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="1b7a4-128">En lösning för den här begränsningen är toouse hello kartan minska aktiviteten toorun anpassad Java-kod på en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-128">A workaround for this limitation is toouse hello Map Reduce Activity toorun custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="1b7a4-129">Ett annat alternativ är toouse Azure Batch-pool för virtuella datorer toorun anpassade aktiviteter i stället för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-129">Another option is toouse an Azure Batch pool of VMs toorun custom activities instead of using a HDInsight cluster.</span></span>
> - <span data-ttu-id="1b7a4-130">Det är inte möjligt toouse en Data Management Gateway från en anpassad aktivitet tooaccess lokala datakällor.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-130">It is not possible toouse a Data Management Gateway from a custom activity tooaccess on-premises data sources.</span></span> <span data-ttu-id="1b7a4-131">För närvarande [Data Management Gateway](data-factory-data-management-gateway.md) stöder endast hello kopia och lagrade proceduraktiviteten i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-131">Currently, [Data Management Gateway](data-factory-data-management-gateway.md) supports only hello copy activity and stored procedure activity in Data Factory.</span></span>   

## <a name="walkthrough-create-a-custom-activity"></a><span data-ttu-id="1b7a4-132">Genomgång: skapa en anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-132">Walkthrough: create a custom activity</span></span>
### <a name="prerequisites"></a><span data-ttu-id="1b7a4-133">Krav</span><span class="sxs-lookup"><span data-stu-id="1b7a4-133">Prerequisites</span></span>
* <span data-ttu-id="1b7a4-134">Visual Studio 2012/2013/2015</span><span class="sxs-lookup"><span data-stu-id="1b7a4-134">Visual Studio 2012/2013/2015</span></span>
* <span data-ttu-id="1b7a4-135">Ladda ned och installera [Azure .NET SDK](https://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="1b7a4-135">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/)</span></span>

### <a name="azure-batch-prerequisites"></a><span data-ttu-id="1b7a4-136">Krav för Azure Batch</span><span class="sxs-lookup"><span data-stu-id="1b7a4-136">Azure Batch prerequisites</span></span>
<span data-ttu-id="1b7a4-137">I hello genomgången kan du köra dina anpassade .NET-aktiviteter med hjälp av Azure Batch som en resurs för beräkning.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-137">In hello walkthrough, you run your custom .NET activities using Azure Batch as a compute resource.</span></span> <span data-ttu-id="1b7a4-138">**Azure Batch** är en plattform för tjänsten för att köra storskaliga parallellt och högpresterande datorbearbetning (HPC) program effektivt i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-138">**Azure Batch** is a platform service for running large-scale parallel and high-performance computing (HPC) applications efficiently in hello cloud.</span></span> <span data-ttu-id="1b7a4-139">Azure Batch schemalägger beräkningsintensiva arbete toorun på en hanterad **samling av virtuella datorer**, och kan automatiskt skala beräkna resurser toomeet hello behoven hos dina jobb.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-139">Azure Batch schedules compute-intensive work toorun on a managed **collection of virtual machines**, and can automatically scale compute resources toomeet hello needs of your jobs.</span></span> <span data-ttu-id="1b7a4-140">Se [grunderna i Azure Batch] [ batch-technical-overview] artikel en detaljerad översikt över hello Azure Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-140">See [Azure Batch basics][batch-technical-overview] article for a detailed overview of hello Azure Batch service.</span></span>

<span data-ttu-id="1b7a4-141">Hello genomgång kan skapa ett Azure Batch-konto med en pool för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-141">For hello tutorial, create an Azure Batch account with a pool of VMs.</span></span> <span data-ttu-id="1b7a4-142">Här är hello steg:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-142">Here are hello steps:</span></span>

1. <span data-ttu-id="1b7a4-143">Skapa en **Azure Batch-kontot** med hello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-143">Create an **Azure Batch account** using hello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="1b7a4-144">Se [skapa och hantera Azure Batch-kontot] [ batch-create-account] artikel anvisningar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-144">See [Create and manage an Azure Batch account][batch-create-account] article for instructions.</span></span>
2. <span data-ttu-id="1b7a4-145">Notera hello Azure Batch-kontonamn, kontonyckel, URI: N och namnet på programpoolen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-145">Note down hello Azure Batch account name, account key, URI, and pool name.</span></span> <span data-ttu-id="1b7a4-146">Du behöver dem toocreate en Azure Batch länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-146">You need them toocreate an Azure Batch linked service.</span></span>
    1. <span data-ttu-id="1b7a4-147">Hello hemsidan för Azure Batch-kontot som du ser en **URL** i hello följande format: `https://myaccount.westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-147">On hello home page for Azure Batch account, you see a **URL** in hello following format: `https://myaccount.westus.batch.azure.com`.</span></span> <span data-ttu-id="1b7a4-148">I det här exemplet **MITTKONTO** är hello namnet på hello Azure Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-148">In this example, **myaccount** is hello name of hello Azure Batch account.</span></span> <span data-ttu-id="1b7a4-149">URI som du använder i hello länkade tjänstdefinitionen är hello URL utan hello namnet på hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-149">URI you use in hello linked service definition is hello URL without hello name of hello account.</span></span> <span data-ttu-id="1b7a4-150">Till exempel: `https://<region>.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-150">For example: `https://<region>.batch.azure.com`.</span></span>
    2. <span data-ttu-id="1b7a4-151">Klicka på **nycklar** på hello vänstra menyn och kopiera hello **primära ÅTKOMSTNYCKELN**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-151">Click **Keys** on hello left menu, and copy hello **PRIMARY ACCESS KEY**.</span></span>
    3. <span data-ttu-id="1b7a4-152">toouse en befintlig adresspool klickar du på **pooler** på hello-menyn och Skriv ner hello **ID** för hello pool.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-152">toouse an existing pool, click **Pools** on hello menu, and note down hello **ID** of hello pool.</span></span> <span data-ttu-id="1b7a4-153">Om du inte har en befintlig adresspool kan du flytta toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-153">If you don't have an existing pool, move toohello next step.</span></span>     
2. <span data-ttu-id="1b7a4-154">Skapa en **Azure Batch-pool**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-154">Create an **Azure Batch pool**.</span></span>

   1. <span data-ttu-id="1b7a4-155">I hello [Azure-portalen](https://portal.azure.com), klickar du på **Bläddra** i hello vänstra menyn och klickar på **Batch-konton**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-155">In hello [Azure portal](https://portal.azure.com), click **Browse** in hello left menu, and click **Batch Accounts**.</span></span>
   2. <span data-ttu-id="1b7a4-156">Välj din Azure Batch-kontot tooopen hello **Batch-kontot** bladet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-156">Select your Azure Batch account tooopen hello **Batch Account** blade.</span></span>
   3. <span data-ttu-id="1b7a4-157">Klicka på **pooler** panelen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-157">Click **Pools** tile.</span></span>
   4. <span data-ttu-id="1b7a4-158">I hello **pooler** bladet, klickar du på knappen Lägg till på hello verktygsfältet tooadd poolen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-158">In hello **Pools** blade, click Add button on hello toolbar tooadd a pool.</span></span>
      1. <span data-ttu-id="1b7a4-159">Ange ett ID för hello-pool (Pool-ID).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-159">Enter an ID for hello pool (Pool ID).</span></span> <span data-ttu-id="1b7a4-160">Obs hello **ID för hello pool**; du behöver den när du skapar hello Data Factory-lösning.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-160">Note hello **ID of hello pool**; you need it when creating hello Data Factory solution.</span></span>
      2. <span data-ttu-id="1b7a4-161">Ange **Windows Server 2012 R2** för hello Operativsystemsfamilj inställningen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-161">Specify **Windows Server 2012 R2** for hello Operating System Family setting.</span></span>
      3. <span data-ttu-id="1b7a4-162">Välj en **nodprisnivå**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-162">Select a **node pricing tier**.</span></span>
      4. <span data-ttu-id="1b7a4-163">Ange **2** som värde för hello **mål dedikerade** inställningen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-163">Enter **2** as value for hello **Target Dedicated** setting.</span></span>
      5. <span data-ttu-id="1b7a4-164">Ange **2** som värde för hello **maximalt antal uppgifter per nod** inställningen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-164">Enter **2** as value for hello **Max tasks per node** setting.</span></span>
   5. <span data-ttu-id="1b7a4-165">Klicka på **OK** toocreate hello poolen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-165">Click **OK** toocreate hello pool.</span></span>
   6. <span data-ttu-id="1b7a4-166">Skriv ner hello **ID** för hello pool.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-166">Note down hello **ID** of hello pool.</span></span> 



### <a name="high-level-steps"></a><span data-ttu-id="1b7a4-167">Anvisningar</span><span class="sxs-lookup"><span data-stu-id="1b7a4-167">High-level steps</span></span>
<span data-ttu-id="1b7a4-168">Här följer hello två övergripande steg du utför som en del av den här genomgången:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-168">Here are hello two high-level steps you perform as part of this walkthrough:</span></span> 

1. <span data-ttu-id="1b7a4-169">Skapa en anpassad aktivitet som innehåller enkla data transformation/standardbearbetningslogiken.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-169">Create a custom activity that contains simple data transformation/processing logic.</span></span>
2. <span data-ttu-id="1b7a4-170">Skapa ett Azure data factory med en pipeline som använder hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-170">Create an Azure data factory with a pipeline that uses hello custom activity.</span></span>

### <a name="create-a-custom-activity"></a><span data-ttu-id="1b7a4-171">Skapa en anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-171">Create a custom activity</span></span>
<span data-ttu-id="1b7a4-172">toocreate en anpassad aktivitet för .NET, skapa en **.NET-klassbibliotek** projekt med en klass som implementerar som **IDotNetActivity** gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-172">toocreate a .NET custom activity, create a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="1b7a4-173">Det här gränssnittet har endast en metod: [kör](https://msdn.microsoft.com/library/azure/mt603945.aspx) och signaturen är:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-173">This interface has only one method: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) and its signature is:</span></span>

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


<span data-ttu-id="1b7a4-174">hello-metoden har fyra parametrar:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-174">hello method takes four parameters:</span></span>

- <span data-ttu-id="1b7a4-175">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-175">**linkedServices**.</span></span> <span data-ttu-id="1b7a4-176">Den här egenskapen är en enumerable lista över datalager som är länkade tjänster som refereras av in-/ utdata-datauppsättningar för hello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-176">This property is an enumerable list of Data Store linked services referenced by input/output datasets for hello activity.</span></span>   
- <span data-ttu-id="1b7a4-177">**datauppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-177">**datasets**.</span></span> <span data-ttu-id="1b7a4-178">Den här egenskapen är en enumerable lista av in-/ utdata-datauppsättningar för hello aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-178">This property is an enumerable list of input/output datasets for hello activity.</span></span> <span data-ttu-id="1b7a4-179">Du kan använda den här parametern tooget hello platser och scheman som definieras av inkommande och utgående datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-179">You can use this parameter tooget hello locations and schemas defined by input and output datasets.</span></span>
- <span data-ttu-id="1b7a4-180">**aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-180">**activity**.</span></span> <span data-ttu-id="1b7a4-181">Denna egenskap representerar hello pågående aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-181">This property represents hello current activity.</span></span> <span data-ttu-id="1b7a4-182">Det kan vara används tooaccess utökade egenskaper som är associerade med hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-182">It can be used tooaccess extended properties associated with hello custom activity.</span></span> <span data-ttu-id="1b7a4-183">Se [åtkomst utökade egenskaper](#access-extended-properties) mer information.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-183">See [Access extended properties](#access-extended-properties) for details.</span></span>
- <span data-ttu-id="1b7a4-184">**loggaren**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-184">**logger**.</span></span> <span data-ttu-id="1b7a4-185">Det här objektet kan du skriva debug kommentarer som yta i hello användarinloggning för hello pipelinen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-185">This object lets you write debug comments that surface in hello user log for hello pipeline.</span></span>

<span data-ttu-id="1b7a4-186">hello-metoden returnerar en ordlista som kan vara tillsammans används toochain anpassade aktiviteter i framtiden hello.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-186">hello method returns a dictionary that can be used toochain custom activities together in hello future.</span></span> <span data-ttu-id="1b7a4-187">Den här funktionen har inte implementerats ännu, så returnerar ett tomt Ordbok från hello-metod.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-187">This feature is not implemented yet, so return an empty dictionary from hello method.</span></span>  

### <a name="procedure"></a><span data-ttu-id="1b7a4-188">Procedur</span><span class="sxs-lookup"><span data-stu-id="1b7a4-188">Procedure</span></span>
1. <span data-ttu-id="1b7a4-189">Skapa en **.NET-klassbibliotek** projekt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-189">Create a **.NET Class Library** project.</span></span>
   <ol type="a">
     <li><span data-ttu-id="1b7a4-190">Starta <b>Visual Studio 2017</b> eller <b>Visual Studio 2015</b> eller <b>Visual Studio 2013</b> eller <b>Visual Studio 2012</b>.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-190">Launch <b>Visual Studio 2017</b> or <b>Visual Studio 2015</b> or <b>Visual Studio 2013</b> or <b>Visual Studio 2012</b>.</span></span></li>
     <li><span data-ttu-id="1b7a4-191">Klicka på <b>filen</b>, peka för<b>ny</b>, och klicka på <b>projekt</b>.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-191">Click <b>File</b>, point too<b>New</b>, and click <b>Project</b>.</span></span></li>
     <li><span data-ttu-id="1b7a4-192">Expandera <b>Mallar</b> och välj <b>Visual C#</b>.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-192">Expand <b>Templates</b>, and select <b>Visual C#</b>.</span></span> <span data-ttu-id="1b7a4-193">I den här genomgången ska du använda C#, men du kan använda alla .NET språk toodevelop hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-193">In this walkthrough, you use C#, but you can use any .NET language toodevelop hello custom activity.</span></span></li>
     <li><span data-ttu-id="1b7a4-194">Välj <b>klassbiblioteket</b> hello listan över projekttyper på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-194">Select <b>Class Library</b> from hello list of project types on hello right.</span></span> <span data-ttu-id="1b7a4-195">Välj i VS 2017 <b>Class Library (.NET Framework)</b> </span><span class="sxs-lookup"><span data-stu-id="1b7a4-195">In VS 2017, choose <b>Class Library (.NET Framework)</b> </span></span></li>
     <li><span data-ttu-id="1b7a4-196">Ange <b>MyDotNetActivity</b> för hello <b>namn</b>.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-196">Enter <b>MyDotNetActivity</b> for hello <b>Name</b>.</span></span></li>
     <li><span data-ttu-id="1b7a4-197">Välj <b>C:\ADFGetStarted</b> för hello <b>plats</b>.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-197">Select <b>C:\ADFGetStarted</b> for hello <b>Location</b>.</span></span></li>
     <li><span data-ttu-id="1b7a4-198">Klicka på <b>OK</b> toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-198">Click <b>OK</b> toocreate hello project.</span></span></li>
   </ol><span data-ttu-id="1b7a4-199">
2.Klicka på **verktyg**, peka för**NuGet Package Manager**, och klicka på **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-199">
2. Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
<span data-ttu-id="1b7a4-200">3.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-200">3.</span></span> <span data-ttu-id="1b7a4-201">Kör följande kommando tooimport hello i hello Package Manager-konsolen, **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-201">In hello Package Manager Console, execute hello following command tooimport **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="1b7a4-202">Importera hello **Azure Storage** NuGet-paketet i toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-202">Import hello **Azure Storage** NuGet package in toohello project.</span></span>

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="1b7a4-203">Startprogram för data Factory-tjänsten kräver hello 4.3 version av WindowsAzure.Storage.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-203">Data Factory service launcher requires hello 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="1b7a4-204">Om du lägger till en referens tooa senare version av Azure Storage sammansättning i projektet anpassad aktivitet visas ett fel när hello aktiviteten körs.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-204">If you add a reference tooa later version of Azure Storage assembly in your custom activity project, you see an error when hello activity executes.</span></span> <span data-ttu-id="1b7a4-205">tooresolve hello fel finns [Appdomain isolering](#appdomain-isolation) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-205">tooresolve hello error, see [Appdomain isolation](#appdomain-isolation) section.</span></span> 
5. <span data-ttu-id="1b7a4-206">Lägg till följande hello **med** instruktioner toohello källfilen i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-206">Add hello following **using** statements toohello source file in hello project.</span></span>

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
6. <span data-ttu-id="1b7a4-207">Ändra hello namnet på hello **namnområde** för**MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-207">Change hello name of hello **namespace** too**MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="1b7a4-208">Ändra hello namnet på hello-klass för**MyDotNetActivity** och härledd från hello **IDotNetActivity** gränssnitt som visas i följande kodutdrag hello:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-208">Change hello name of hello class too**MyDotNetActivity** and derive it from hello **IDotNetActivity** interface as shown in hello following code snippet:</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="1b7a4-209">Implementera (Lägg till) hello **kör** metod för hello **IDotNetActivity** gränssnitt toohello **MyDotNetActivity** klass och kopiera hello följande exempel kod toohello metod.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-209">Implement (Add) hello **Execute** method of hello **IDotNetActivity** interface toohello **MyDotNetActivity** class and copy hello following sample code toohello method.</span></span>

    <span data-ttu-id="1b7a4-210">hello räknar följande exempel hello antalet förekomster av hello sökterm (”Microsoft”) i varje blob som är associerade med en datasektorn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-210">hello following sample counts hello number of occurrences of hello search term (“Microsoft”) in each blob associated with a data slice.</span></span>

    ```csharp
    /// <summary>
    /// Execute method is hello only method of IDotNetActivity interface you must implement.
    /// In this sample, hello method invokes hello Calculate method tooperform hello core logic.  
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
    
        // toolog information, use hello logger object
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

        // get hello input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables toohold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from hello dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and hello other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get hello first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using hello same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get hello connection string in hello linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get hello folder path from hello input dataset definition
        string folderPath = GetFolderPath(inputDataset);
        string output = string.Empty; // for use later.
    
        // create storage client for input. Pass hello connection string.
        CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
        CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
        // initialize hello continuation token before using it in hello do-while loop.
        BlobContinuationToken continuationToken = null;
        do
        {   // get hello list of input blobs from hello input storage client object.
            BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                     true,
                                     BlobListingDetails.Metadata,
                                     null,
                                     continuationToken,
                                     null,
                                     null);
    
            // Calculate method returns hello number of occurrences of
            // hello search term (“Microsoft”) in each blob associated
               // with hello data slice. definition of hello method is shown in hello next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for hello output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get hello folder path from hello output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log hello output folder path 
        logger.Write("Writing blob toohello folder: {0}", folderPath);
    
        // create a storage object for hello output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write hello name of hello file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log hello output file name
        logger.Write("output blob URI: {0}", outputBlobUri.ToString());

        // create a blob and upload hello output text.
        CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
        logger.Write("Writing {0} toohello output blob", output);
        outputBlob.UploadText(output);
    
        // hello dictionary can be used toochain custom activities together in hello future.
        // This feature is not implemented yet, so just return an empty dictionary.  
    
        return new Dictionary<string, string>();
    }
    ```
9. <span data-ttu-id="1b7a4-211">Lägg till följande hjälpmetoder hello:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-211">Add hello following helper methods:</span></span> 

    ```csharp
    /// <summary>
    /// Gets hello folderPath value from hello input/output dataset.
    /// </summary>
    
    private static string GetFolderPath(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }

        // get type properties of hello dataset 
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello folder path found in hello type properties
        return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets hello fileName value from hello input/output dataset.   
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
        if (dataArtifact == null || dataArtifact.Properties == null)
        {
            return null;
        }
    
        // get type properties of hello dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello blob/file name in hello type properties
        return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in hello folder, counts hello number of instances of search term in hello file,
    /// and prepares hello output text that is written toohello output blob.
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
                output += string.Format("{0} occurrences(s) of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
            }
        }
        return output;
    }
    ```

    <span data-ttu-id="1b7a4-212">Hej GetFolderPath metoden returnerar hello toohello sökväg som hello dataset pekar tooand hello GetFileName metoden returnerar hello namnet på hello blob/fil hello dataset pekar på.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-212">hello GetFolderPath method returns hello path toohello folder that hello dataset points tooand hello GetFileName method returns hello name of hello blob/file that hello dataset points to.</span></span> <span data-ttu-id="1b7a4-213">Om du havefolderPath definierar använda variabler, till exempel {Year}, {Month}, {Day} osv., hello-metoden returnerar hello sträng eftersom den utan att ersätta dem med runtime-värden.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-213">If you havefolderPath defines using variables such as {Year}, {Month}, {Day} etc., hello method returns hello string as it is without replacing them with runtime values.</span></span> <span data-ttu-id="1b7a4-214">Se [åtkomst utökade egenskaper](#access-extended-properties) information om att komma åt SliceStart, SliceEnd osv.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-214">See [Access extended properties](#access-extended-properties) section for details on accessing SliceStart, SliceEnd, etc.</span></span>    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    <span data-ttu-id="1b7a4-215">hello Calculate metoden beräknar hello antal instanser av nyckelordet Microsoft i hello indatafiler (blobbar i hello mapp).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-215">hello Calculate method calculates hello number of instances of keyword Microsoft in hello input files (blobs in hello folder).</span></span> <span data-ttu-id="1b7a4-216">hello sökordet (”Microsoft”) är hårdkodat i hello kod.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-216">hello search term (“Microsoft”) is hard-coded in hello code.</span></span>
10. <span data-ttu-id="1b7a4-217">Kompilera hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-217">Compile hello project.</span></span> <span data-ttu-id="1b7a4-218">Klicka på **skapa** från hello-menyn och klicka på **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-218">Click **Build** from hello menu and click **Build Solution**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="1b7a4-219">Ange 4.5.2 version av .NET Framework som hello Målversionen av framework för ditt projekt: Högerklicka på hello-projektet och klicka på **egenskaper** tooset hello Målversionen av framework.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-219">Set 4.5.2 version of .NET Framework as hello target framework for your project: right-click hello project, and click **Properties** tooset hello target framework.</span></span> <span data-ttu-id="1b7a4-220">Data Factory stöder inte anpassade aktiviteter kompileras mot .NET Framework-versioner senare än 4.5.2.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-220">Data Factory does not support custom activities compiled against .NET Framework versions later than 4.5.2.</span></span>

11. <span data-ttu-id="1b7a4-221">Starta **Windows Explorer**, och navigera för**bin\debug** eller **bin\release** beroende på hello typ av version.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-221">Launch **Windows Explorer**, and navigate too**bin\debug** or **bin\release** folder depending on hello type of build.</span></span>
12. <span data-ttu-id="1b7a4-222">Skapa en zip-fil **MyDotNetActivity.zip** som innehåller alla hello-binärfiler i hello <project folder>\bin\Debug mapp.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-222">Create a zip file **MyDotNetActivity.zip** that contains all hello binaries in hello <project folder>\bin\Debug folder.</span></span> <span data-ttu-id="1b7a4-223">Inkludera hello **MyDotNetActivity.pdb** filen så att du får ytterligare information, till exempel radnumret i hello källkod som har orsakat problemet hello om ett fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-223">Include hello **MyDotNetActivity.pdb** file so that you get additional details such as line number in hello source code that caused hello issue if there was a failure.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="1b7a4-224">Alla hello filer i hello zip-filen för hello anpassad aktivitet måste vara på hello **övre nivå** med inga undermappar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-224">All hello files in hello zip file for hello custom activity must be at hello **top level** with no sub folders.</span></span>

    ![Binära filer](./media/data-factory-use-custom-activities/Binaries.png)
14. <span data-ttu-id="1b7a4-226">Skapa en blobbbehållare med namnet **customactivitycontainer** om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-226">Create a blob container named **customactivitycontainer** if it does not already exist.</span></span> 
15. <span data-ttu-id="1b7a4-227">Ladda upp MyDotNetActivity.zip som en blob toohello customactivitycontainer i en **allmänna** Azure blob-lagring (hot inte/lågfrekvent Blob-lagring) som refereras av AzureStorageLinkedService.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-227">Upload MyDotNetActivity.zip as a blob toohello customactivitycontainer in a **general-purpose** Azure blob storage (not hot/cool Blob storage) that is referred by AzureStorageLinkedService.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="1b7a4-228">Om du lägger till .NET aktivitet projektet tooa lösningen i Visual Studio som innehåller ett projekt som Data Factory och lägga till en referens too.NET aktivitet projekt från hello Data Factory-programprojekt, behöver du inte tooperform hello två sista stegen för att skapa manuellt hello zip-filen och överföra den toohello allmänna Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-228">If you add this .NET activity project tooa solution in Visual Studio that contains a Data Factory project, and add a reference too.NET activity project from hello Data Factory application project, you do not need tooperform hello last two steps of manually creating hello zip file and uploading it toohello general-purpose Azure blob storage.</span></span> <span data-ttu-id="1b7a4-229">När du publicerar Data Factory-enheter med hjälp av Visual Studio, utförs automatiskt de här stegen av hello publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-229">When you publish Data Factory entities using Visual Studio, these steps are automatically done by hello publishing process.</span></span> <span data-ttu-id="1b7a4-230">Mer information finns i [Data Factory-projekt i Visual Studio](#data-factory-project-in-visual-studio) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-230">For more information, see [Data Factory project in Visual Studio](#data-factory-project-in-visual-studio) section.</span></span>

## <a name="create-a-pipeline-with-custom-activity"></a><span data-ttu-id="1b7a4-231">Skapa en pipeline med anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-231">Create a pipeline with custom activity</span></span>
<span data-ttu-id="1b7a4-232">Du har skapat en anpassad aktivitet och överföra hello zip-filen med binärfiler tooa blob-behållare i en **allmänna** Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-232">You have created a custom activity and uploaded hello zip file with binaries tooa blob container in a **general-purpose** Azure Storage Account.</span></span> <span data-ttu-id="1b7a4-233">I det här avsnittet skapar du ett Azure data factory med en pipeline som använder hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-233">In this section, you create an Azure data factory with a pipeline that uses hello custom activity.</span></span>

<span data-ttu-id="1b7a4-234">hello inkommande datamängden för hello anpassad aktivitet representerar blobbar (filer) i hello customactivityinput mapp för adftutorial behållare i hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-234">hello input dataset for hello custom activity represents blobs (files) in hello customactivityinput folder of adftutorial container in hello blob storage.</span></span> <span data-ttu-id="1b7a4-235">Hej utdatauppsättningen för hello aktivitet representerar utdata blobbar i hello customactivityoutput mapp för adftutorial behållare i hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-235">hello output dataset for hello activity represents output blobs in hello customactivityoutput folder of adftutorial container in hello blob storage.</span></span>

<span data-ttu-id="1b7a4-236">Skapa **fil.txt** fil med hello följande innehåll och ladda upp det för**customactivityinput** för hello **adftutorial** behållare.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-236">Create **file.txt** file with hello following content and upload it too**customactivityinput** folder of hello **adftutorial** container.</span></span> <span data-ttu-id="1b7a4-237">Skapa hello adftutorial behållaren om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-237">Create hello adftutorial container if it does not exist already.</span></span> 

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="1b7a4-238">hello inkommande mappen motsvarar tooa segment i Azure Data Factory även om hello-mappen har två eller flera filer.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-238">hello input folder corresponds tooa slice in Azure Data Factory even if hello folder has two or more files.</span></span> <span data-ttu-id="1b7a4-239">När varje segment bearbetas av hello pipeline går hello anpassad aktivitet igenom alla hello BLOB i hello inkommande mapp för sektorn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-239">When each slice is processed by hello pipeline, hello custom activity iterates through all hello blobs in hello input folder for that slice.</span></span>

<span data-ttu-id="1b7a4-240">Du ser en utdatafil med i hello adftutorial\customactivityoutput mapp med en eller flera rader (samma som antal blobbar i hello inkommande mapp):</span><span class="sxs-lookup"><span data-stu-id="1b7a4-240">You see one output file with in hello adftutorial\customactivityoutput folder with one or more lines (same as number of blobs in hello input folder):</span></span>

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
```


<span data-ttu-id="1b7a4-241">Här är hello steg du utför i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-241">Here are hello steps you perform in this section:</span></span>

1. <span data-ttu-id="1b7a4-242">Skapa en **datafabriken**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-242">Create a **data factory**.</span></span>
2. <span data-ttu-id="1b7a4-243">Skapa **länkade tjänster** för hello Azure Batch-pool för virtuella datorer på vilka hello anpassad aktivitet körs och hello Azure Storage där hello-i/o-BLOB.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-243">Create **Linked services** for hello Azure Batch pool of VMs on which hello custom activity runs and hello Azure Storage that holds hello input/output blobs.</span></span>
3. <span data-ttu-id="1b7a4-244">Skapa ingående och utgående **datauppsättningar** som representerar indata och utdata för hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-244">Create input and output **datasets** that represent input and output of hello custom activity.</span></span>
4. <span data-ttu-id="1b7a4-245">Skapa en **pipeline** som använder hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-245">Create a **pipeline** that uses hello custom activity.</span></span>

> [!NOTE]
> <span data-ttu-id="1b7a4-246">Skapa hello **fil.txt** och ladda upp den tooa blob-behållaren om du inte redan gjort det..</span><span class="sxs-lookup"><span data-stu-id="1b7a4-246">Create hello **file.txt** and upload it tooa blob container if you haven't already done so.</span></span> <span data-ttu-id="1b7a4-247">Se anvisningarna i föregående avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-247">See instructions in hello preceding section.</span></span>   

### <a name="step-1-create-hello-data-factory"></a><span data-ttu-id="1b7a4-248">Steg 1: Skapa hello data factory</span><span class="sxs-lookup"><span data-stu-id="1b7a4-248">Step 1: Create hello data factory</span></span>
1. <span data-ttu-id="1b7a4-249">Efter inloggningen toohello Azure-portalen hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-249">After logging in toohello Azure portal, do hello following steps:</span></span>
   1. <span data-ttu-id="1b7a4-250">Klicka på **ny** på hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-250">Click **NEW** on hello left menu.</span></span>
   2. <span data-ttu-id="1b7a4-251">Klicka på **Data + analys** i hello **ny** bladet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-251">Click **Data + Analytics** in hello **New** blade.</span></span>
   3. <span data-ttu-id="1b7a4-252">Klicka på **Datafabriken** på hello **dataanalys** bladet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-252">Click **Data Factory** on hello **Data analytics** blade.</span></span>
   
    ![Nya Azure Data Factory-menyn](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. <span data-ttu-id="1b7a4-254">I hello **nya datafabriken** bladet ange **CustomActivityFactory** för hello namn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-254">In hello **New data factory** blade, enter **CustomActivityFactory** for hello Name.</span></span> <span data-ttu-id="1b7a4-255">hello namn i hello Azure data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-255">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="1b7a4-256">Om felmeddelandet hello: **datafabriksnamnet ”CustomActivityFactory” är inte tillgänglig**, ändra hello namn i hello data factory (till exempel **yournameCustomActivityFactory**) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-256">If you receive hello error: **Data factory name “CustomActivityFactory” is not available**, change hello name of hello data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>

    ![Nya Azure Data Factory-bladet](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. <span data-ttu-id="1b7a4-258">Klicka på **RESURSGRUPPENS namn**, och välj en befintlig resursgrupp eller skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-258">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="1b7a4-259">Kontrollera att du använder rätt hello **prenumeration** och **region** där du vill att hello data factory toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-259">Verify that you are using hello correct **subscription** and **region** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="1b7a4-260">Klicka på **skapa** på hello **nya data factory** bladet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-260">Click **Create** on hello **New data factory** blade.</span></span>
6. <span data-ttu-id="1b7a4-261">Du ser hello data factory som skapas i hello **instrumentpanelen** av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-261">You see hello data factory being created in hello **Dashboard** of hello Azure portal.</span></span>
7. <span data-ttu-id="1b7a4-262">När hello datafabriken har skapats, visas hello Data Factory blad som visar hello innehållet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-262">After hello data factory has been created successfully, you see hello Data Factory blade, which shows you hello contents of hello data factory.</span></span>
    
    ![Bladet Datafabrik](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a><span data-ttu-id="1b7a4-264">Steg 2: Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="1b7a4-264">Step 2: Create linked services</span></span>
<span data-ttu-id="1b7a4-265">Länkade tjänster länka datalager eller compute services tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-265">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="1b7a4-266">I det här steget kan länka du ditt Azure Storage-konto och Azure Batch-kontot tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-266">In this step, you link your Azure Storage account and Azure Batch account tooyour data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="1b7a4-267">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="1b7a4-267">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="1b7a4-268">Klicka på hello **författare och distribuera** panelen på hello **DATAFABRIKEN** bladet för **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-268">Click hello **Author and deploy** tile on hello **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="1b7a4-269">Du kan se hello Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-269">You see hello Data Factory Editor.</span></span>
2. <span data-ttu-id="1b7a4-270">Klicka på **Nytt datalager** hello kommandofältet och välj **Azure storage**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-270">Click **New data store** on hello command bar and choose **Azure storage**.</span></span> <span data-ttu-id="1b7a4-271">Du bör se hello JSON-skript för att skapa ett Azure Storage länkade tjänsten i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-271">You should see hello JSON script for creating an Azure Storage linked service in hello editor.</span></span>
    
    ![Nytt datalager - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. <span data-ttu-id="1b7a4-273">Ersätt `<accountname>` med namnet på ditt Azure storage-konto och `<accountkey>` med åtkomstnyckeln för hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-273">Replace `<accountname>` with name of your Azure storage account and `<accountkey>` with access key of hello Azure storage account.</span></span> <span data-ttu-id="1b7a4-274">toolearn hur tooget lagringen åt nyckeln finns [visa, kopiera och generera lagring åtkomstnycklar](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-274">toolearn how tooget your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

    ![Azure Storage tyckte om tjänsten](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. <span data-ttu-id="1b7a4-276">Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-276">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="1b7a4-277">Skapa Azure Batch länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="1b7a4-277">Create Azure Batch linked service</span></span>
1. <span data-ttu-id="1b7a4-278">Hello Data Factory-redigeraren, klicka på **... Flera** på hello kommandofältet klickar du på **nya beräkning**, och välj sedan **Azure Batch** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-278">In hello Data Factory Editor, click **... More** on hello command bar, click **New compute**, and then select **Azure Batch** from hello menu.</span></span>

    ![Nya beräknings - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. <span data-ttu-id="1b7a4-280">Gör följande ändringar toohello JSON-skript hello:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-280">Make hello following changes toohello JSON script:</span></span>

   1. <span data-ttu-id="1b7a4-281">Ange Azure Batch-kontonamnet för hello **accountName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-281">Specify Azure Batch account name for hello **accountName** property.</span></span> <span data-ttu-id="1b7a4-282">Hej **URL** från hello **Azure Batch-kontoblad** i hello följande format: `http://accountname.region.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-282">hello **URL** from hello **Azure Batch account blade** is in hello following format: `http://accountname.region.batch.azure.com`.</span></span> <span data-ttu-id="1b7a4-283">För hello **batchUri** egenskap i hello JSON, måste du tooremove `accountname.` från hello URL och Använd hello `accountname` för hello `accountName` JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-283">For hello **batchUri** property in hello JSON, you need tooremove `accountname.` from hello URL and use hello `accountname` for hello `accountName` JSON property.</span></span>
   2. <span data-ttu-id="1b7a4-284">Ange hello Azure Batch-kontonyckel för hello **accessKey** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-284">Specify hello Azure Batch account key for hello **accessKey** property.</span></span>
   3. <span data-ttu-id="1b7a4-285">Ange hello namn för hello-pool som du har skapat som en del av krav för hello **poolName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-285">Specify hello name of hello pool you created as part of prerequisites for hello **poolName** property.</span></span> <span data-ttu-id="1b7a4-286">Du kan också ange hello-ID för hello poolen i stället för hello namnet på hello-adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-286">You can also specify hello ID of hello pool instead of hello name of hello pool.</span></span>
   4. <span data-ttu-id="1b7a4-287">Ange Azure Batch-URI för hello **batchUri** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-287">Specify Azure Batch URI for hello **batchUri** property.</span></span> <span data-ttu-id="1b7a4-288">Exempel: `https://westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-288">Example: `https://westus.batch.azure.com`.</span></span>  
   5. <span data-ttu-id="1b7a4-289">Ange hello **AzureStorageLinkedService** för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-289">Specify hello **AzureStorageLinkedService** for hello **linkedServiceName** property.</span></span>

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

       <span data-ttu-id="1b7a4-290">För hello **poolName** egenskap, du kan också ange hello-ID för hello poolen i stället för hello namnet på hello-adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-290">For hello **poolName** property, you can also specify hello ID of hello pool instead of hello name of hello pool.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="1b7a4-291">hello Data Factory-tjänsten stöder inte ett alternativ på begäran för Azure Batch som för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-291">hello Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="1b7a4-292">Du kan bara använda din egen Azure Batch-pool i ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-292">You can only use your own Azure Batch pool in an Azure data factory.</span></span>   
    

### <a name="step-3-create-datasets"></a><span data-ttu-id="1b7a4-293">Steg 3: Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="1b7a4-293">Step 3: Create datasets</span></span>
<span data-ttu-id="1b7a4-294">I det här steget Skapa datauppsättningar toorepresent indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-294">In this step, you create datasets toorepresent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="1b7a4-295">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="1b7a4-295">Create input dataset</span></span>
1. <span data-ttu-id="1b7a4-296">I hello **Editor** hello Data Factory, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj sedan **Azure Blob storage** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-296">In hello **Editor** for hello Data Factory, click **... More** on hello command bar, click **New dataset**, and then select **Azure Blob storage** from hello drop-down menu.</span></span>
2. <span data-ttu-id="1b7a4-297">Ersätt hello JSON i hello högra med hello följande JSON-fragment:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-297">Replace hello JSON in hello right pane with hello following JSON snippet:</span></span>

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

   <span data-ttu-id="1b7a4-298">Du skapar en pipeline senare i den här genomgången med starttid: 2016-11-16T00:00:00Z-och Sluttid: 2016-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-298">You create a pipeline later in this walkthrough with start time: 2016-11-16T00:00:00Z and end time: 2016-11-16T05:00:00Z.</span></span> <span data-ttu-id="1b7a4-299">Det är schemalagda tooproduce data varje timma, så att det finns fem i/o-segment (mellan **00**: 00:00 -> **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-299">It is scheduled tooproduce data hourly, so there are five input/output slices (between **00**:00:00 -> **05**:00:00).</span></span>

   <span data-ttu-id="1b7a4-300">Hej **frekvens** och **intervall** hello inkommande dataset är inställd för**timme** och **1**, vilket innebär att hello indata sektorn är tillgängliga varje timme.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-300">hello **frequency** and **interval** for hello input dataset is set too**Hour** and **1**, which means that hello input slice is available hourly.</span></span> <span data-ttu-id="1b7a4-301">I det här exemplet är det hello samma fil (fil.txt) i hello intputfolder.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-301">In this sample, it is hello same file (file.txt) in hello intputfolder.</span></span>

   <span data-ttu-id="1b7a4-302">Här följer hello starttider för varje segment som representeras av SliceStart systemvariabel i hello ovan JSON-fragment.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-302">Here are hello start times for each slice, which is represented by SliceStart system variable in hello above JSON snippet.</span></span>
3. <span data-ttu-id="1b7a4-303">Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-303">Click **Deploy** on hello toolbar toocreate and deploy hello **InputDataset**.</span></span> <span data-ttu-id="1b7a4-304">Bekräfta att du ser hello **tabell skapas har** meddelandet på hello namnlist hello Editor.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-304">Confirm that you see hello **TABLE CREATED SUCCESSFULLY** message on hello title bar of hello Editor.</span></span>

#### <a name="create-an-output-dataset"></a><span data-ttu-id="1b7a4-305">Skapa en datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="1b7a4-305">Create an output dataset</span></span>
1. <span data-ttu-id="1b7a4-306">I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj sedan **Azure Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-306">In hello **Data Factory editor**, click **... More** on hello command bar, click **New dataset**, and then select **Azure Blob storage**.</span></span>
2. <span data-ttu-id="1b7a4-307">Ersätt hello JSON-skript i hello högra med hello följande JSON-skript:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-307">Replace hello JSON script in hello right pane with hello following JSON script:</span></span>

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

     <span data-ttu-id="1b7a4-308">Platsen är **adftutorial/customactivityoutput/** och utdata-filnamn är åååå-MM-dd-HH.txt där åååå-MM-dd-HH är hello år, månad, datum och timmes hello segment som skapas.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-308">Output location is **adftutorial/customactivityoutput/** and output file name is yyyy-MM-dd-HH.txt where yyyy-MM-dd-HH is hello year, month, date, and hour of hello slice being produced.</span></span> <span data-ttu-id="1b7a4-309">Se [för utvecklare] [ adf-developer-reference] mer information.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-309">See [Developer Reference][adf-developer-reference] for details.</span></span>

    <span data-ttu-id="1b7a4-310">En blob/utdatafil genereras för varje inkommande segment.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-310">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="1b7a4-311">Här är hur en utdatafil heter för varje segment.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-311">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="1b7a4-312">Alla hello utdatafiler skapas i en utdatamapp: **adftutorial\customactivityoutput**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-312">All hello output files are generated in one output folder: **adftutorial\customactivityoutput**.</span></span>

   | <span data-ttu-id="1b7a4-313">Sektorn</span><span class="sxs-lookup"><span data-stu-id="1b7a4-313">Slice</span></span> | <span data-ttu-id="1b7a4-314">Starttid</span><span class="sxs-lookup"><span data-stu-id="1b7a4-314">Start time</span></span> | <span data-ttu-id="1b7a4-315">Utdatafil</span><span class="sxs-lookup"><span data-stu-id="1b7a4-315">Output file</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="1b7a4-316">1</span><span class="sxs-lookup"><span data-stu-id="1b7a4-316">1</span></span> |<span data-ttu-id="1b7a4-317">2016-11-16T00:00:00</span><span class="sxs-lookup"><span data-stu-id="1b7a4-317">2016-11-16T00:00:00</span></span> |<span data-ttu-id="1b7a4-318">2016-11-16-00.txt</span><span class="sxs-lookup"><span data-stu-id="1b7a4-318">2016-11-16-00.txt</span></span> |
   | <span data-ttu-id="1b7a4-319">2</span><span class="sxs-lookup"><span data-stu-id="1b7a4-319">2</span></span> |<span data-ttu-id="1b7a4-320">2016-11-16T01:00:00</span><span class="sxs-lookup"><span data-stu-id="1b7a4-320">2016-11-16T01:00:00</span></span> |<span data-ttu-id="1b7a4-321">2016-11-16-01.txt</span><span class="sxs-lookup"><span data-stu-id="1b7a4-321">2016-11-16-01.txt</span></span> |
   | <span data-ttu-id="1b7a4-322">3</span><span class="sxs-lookup"><span data-stu-id="1b7a4-322">3</span></span> |<span data-ttu-id="1b7a4-323">2016-11-16T02:00:00</span><span class="sxs-lookup"><span data-stu-id="1b7a4-323">2016-11-16T02:00:00</span></span> |<span data-ttu-id="1b7a4-324">2016-11-16-02.txt</span><span class="sxs-lookup"><span data-stu-id="1b7a4-324">2016-11-16-02.txt</span></span> |
   | <span data-ttu-id="1b7a4-325">4</span><span class="sxs-lookup"><span data-stu-id="1b7a4-325">4</span></span> |<span data-ttu-id="1b7a4-326">2016-11-16T03:00:00</span><span class="sxs-lookup"><span data-stu-id="1b7a4-326">2016-11-16T03:00:00</span></span> |<span data-ttu-id="1b7a4-327">2016-11-16-03.txt</span><span class="sxs-lookup"><span data-stu-id="1b7a4-327">2016-11-16-03.txt</span></span> |
   | <span data-ttu-id="1b7a4-328">5</span><span class="sxs-lookup"><span data-stu-id="1b7a4-328">5</span></span> |<span data-ttu-id="1b7a4-329">2016-11-16T04:00:00</span><span class="sxs-lookup"><span data-stu-id="1b7a4-329">2016-11-16T04:00:00</span></span> |<span data-ttu-id="1b7a4-330">2016-11-16-04.txt</span><span class="sxs-lookup"><span data-stu-id="1b7a4-330">2016-11-16-04.txt</span></span> |

    <span data-ttu-id="1b7a4-331">Kom ihåg att alla hello-filer i en inkommande mapp är en del av ett segment med hello starttiderna som nämns ovan.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-331">Remember that all hello files in an input folder are part of a slice with hello start times mentioned above.</span></span> <span data-ttu-id="1b7a4-332">När den här sektorn behandlas hello anpassad aktivitet söker igenom varje fil och ger en rad i hello utdatafil med hello antal förekomster av sökterm (”Microsoft”).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-332">When this slice is processed, hello custom activity scans through each file and produces a line in hello output file with hello number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="1b7a4-333">Om det finns tre filer i hello inputfolder, det finns tre rader i hello utdatafil för varje timme segment: 2016-11-16-00.txt 2016-11-16:01:00:00.txt, etc.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-333">If there are three files in hello inputfolder, there are three lines in hello output file for each hourly slice: 2016-11-16-00.txt, 2016-11-16:01:00:00.txt, etc.</span></span>
3. <span data-ttu-id="1b7a4-334">toodeploy hello **OutputDataset**, klickar du på **distribuera** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-334">toodeploy hello **OutputDataset**, click **Deploy** on hello command bar.</span></span>

### <a name="create-and-run-a-pipeline-that-uses-hello-custom-activity"></a><span data-ttu-id="1b7a4-335">Skapa och köra en pipeline som använder hello anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-335">Create and run a pipeline that uses hello custom activity</span></span>
1. <span data-ttu-id="1b7a4-336">Hello Data Factory-redigeraren, klicka på **... Flera**, och välj sedan **ny pipeline** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-336">In hello Data Factory Editor, click **... More**, and then select **New pipeline** on hello command bar.</span></span> 
2. <span data-ttu-id="1b7a4-337">Ersätt hello JSON i hello högra med hello följande JSON-skript:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-337">Replace hello JSON in hello right pane with hello following JSON script:</span></span>

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

    <span data-ttu-id="1b7a4-338">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-338">Note hello following points:</span></span>

   * <span data-ttu-id="1b7a4-339">**Concurrency** har angetts för**2** så att två segment bearbetas parallellt med 2 virtuella datorer i hello Azure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-339">**Concurrency** is set too**2** so that two slices are processed in parallel by 2 VMs in hello Azure Batch pool.</span></span>
   * <span data-ttu-id="1b7a4-340">Det finns en aktivitet i området för hello aktiviteter och är av typen: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-340">There is one activity in hello activities section and it is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="1b7a4-341">**AssemblyName** anges toohello namnet på hello DLL: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-341">**AssemblyName** is set toohello name of hello DLL: **MyDotnetActivity.dll**.</span></span>
   * <span data-ttu-id="1b7a4-342">**EntryPoint** har angetts för**MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-342">**EntryPoint** is set too**MyDotNetActivityNS.MyDotNetActivity**.</span></span>
   * <span data-ttu-id="1b7a4-343">**PackageLinkedService** har angetts för**AzureStorageLinkedService** som pekar toohello blob-lagring som innehåller hello anpassad aktivitet zip-filen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-343">**PackageLinkedService** is set too**AzureStorageLinkedService** that points toohello blob storage that contains hello custom activity zip file.</span></span> <span data-ttu-id="1b7a4-344">Om du använder olika Azure Storage-konton för i/o filer och hello zip-filen för anpassad aktivitet du skapar en annan länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-344">If you are using different Azure Storage accounts for input/output files and hello custom activity zip file, you create another Azure Storage linked service.</span></span> <span data-ttu-id="1b7a4-345">Den här artikeln förutsätter att du använder hello samma Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-345">This article assumes that you are using hello same Azure Storage account.</span></span>
   * <span data-ttu-id="1b7a4-346">**PackageFile** har angetts för**customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-346">**PackageFile** is set too**customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="1b7a4-347">Den är i hello format: containerforthezip/nameofthezip.zip.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-347">It is in hello format: containerforthezip/nameofthezip.zip.</span></span>
   * <span data-ttu-id="1b7a4-348">hello anpassad aktivitet tar **InputDataset** som indata och **OutputDataset** som utdata.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-348">hello custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="1b7a4-349">Hej linkedServiceName-egenskapen för hello anpassad aktivitet pekar toohello **AzureBatchLinkedService**, som talar om Azure Data Factory som hello anpassade aktiviteten måste toorun på Azure Batch virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-349">hello linkedServiceName property of hello custom activity points toohello **AzureBatchLinkedService**, which tells Azure Data Factory that hello custom activity needs toorun on Azure Batch VMs.</span></span>
   * <span data-ttu-id="1b7a4-350">**isPaused** egenskapen för**FALSKT** som standard.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-350">**isPaused** property is set too**false** by default.</span></span> <span data-ttu-id="1b7a4-351">hello pipelinen körs direkt i det här exemplet eftersom hello segment starta i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-351">hello pipeline runs immediately in this example because hello slices start in hello past.</span></span> <span data-ttu-id="1b7a4-352">Du kan ange den här egenskapen tootrue toopause hello pipeline och ange den bakre toofalse toorestart.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-352">You can set this property tootrue toopause hello pipeline and set it back toofalse toorestart.</span></span>
   * <span data-ttu-id="1b7a4-353">Hej **starta** tid och **end** tider är **fem** timmar från varandra och segment produceras varje timma, så att fem segment som produceras av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-353">hello **start** time and **end** times are **five** hours apart and slices are produced hourly, so five slices are produced by hello pipeline.</span></span>
3. <span data-ttu-id="1b7a4-354">toodeploy hello pipeline, klickar du på **distribuera** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-354">toodeploy hello pipeline, click **Deploy** on hello command bar.</span></span>

### <a name="monitor-hello-pipeline"></a><span data-ttu-id="1b7a4-355">Övervakaren hello pipeline</span><span class="sxs-lookup"><span data-stu-id="1b7a4-355">Monitor hello pipeline</span></span>
1. <span data-ttu-id="1b7a4-356">I hello Data Factory-bladet i hello Azure-portalen klickar du på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-356">In hello Data Factory blade in hello Azure portal, click **Diagram**.</span></span>

    ![Ikonen Diagram](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. <span data-ttu-id="1b7a4-358">Klicka på hello OutputDataset i hello diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-358">In hello Diagram View, now click hello OutputDataset.</span></span>

    ![Diagramvy](./media/data-factory-use-custom-activities/diagram.png)
3. <span data-ttu-id="1b7a4-360">Du bör se att hello fem utdata segment är i tillståndet för hello Ready.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-360">You should see that hello five output slices are in hello Ready state.</span></span> <span data-ttu-id="1b7a4-361">Om de inte är i tillståndet för hello Ready har inte de producerats ännu.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-361">If they are not in hello Ready state, they haven't been produced yet.</span></span> 

   ![Utdata segment](./media/data-factory-use-custom-activities/OutputSlices.png)
4. <span data-ttu-id="1b7a4-363">Kontrollera att hello utdatafilerna genereras i hello blob storage i hello **adftutorial** behållare.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-363">Verify that hello output files are generated in hello blob storage in hello **adftutorial** container.</span></span>

   ![utdata från anpassad aktivitet][image-data-factory-ouput-from-custom-activity]
5. <span data-ttu-id="1b7a4-365">Om du öppnar hello utdatafilen, bör du se hello utdata liknande toohello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-365">If you open hello output file, you should see hello output similar toohello following output:</span></span>

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
    ```
6. <span data-ttu-id="1b7a4-366">Använd hello [Azure-portalen] [ azure-preview-portal] eller Azure PowerShell-cmdlets toomonitor din data factory, rörledningar och datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-366">Use hello [Azure portal][azure-preview-portal] or Azure PowerShell cmdlets toomonitor your data factory, pipelines, and data sets.</span></span> <span data-ttu-id="1b7a4-367">Du kan ta emot meddelanden från hello **ActivityLogger** i hello koden för hello anpassad aktivitet i hello-loggar (särskilt användar-0.log) som du kan hämta från hello-portalen eller med hjälp av cmdlet: ar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-367">You can see messages from hello **ActivityLogger** in hello code for hello custom activity in hello logs (specifically user-0.log) that you can download from hello portal or using cmdlets.</span></span>

   ![Hämta loggar från anpassad aktivitet][image-data-factory-download-logs-from-custom-activity]

<span data-ttu-id="1b7a4-369">Se [övervaka och hantera Pipelines](data-factory-monitor-manage-pipelines.md) detaljerade anvisningar för att övervaka datamängd och pipelinor.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-369">See [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md) for detailed steps for monitoring datasets and pipelines.</span></span>      

## <a name="data-factory-project-in-visual-studio"></a><span data-ttu-id="1b7a4-370">Data Factory-projekt i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b7a4-370">Data Factory project in Visual Studio</span></span>  
<span data-ttu-id="1b7a4-371">Du kan skapa och publicera Data Factory-enheter med hjälp av Visual Studio istället för att använda Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-371">You can create and publish Data Factory entities by using Visual Studio instead of using Azure portal.</span></span> <span data-ttu-id="1b7a4-372">För detaljerad information om hur du skapar och publicerar Data Factory-enheter med hjälp av Visual Studio finns [skapa din första pipeline med Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) och [kopiera data från Azure Blob tooAzure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) artiklar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-372">For detailed information about creating and publishing Data Factory entities by using Visual Studio, See [Build your first pipeline using Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) and [Copy data from Azure Blob tooAzure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) articles.</span></span>

<span data-ttu-id="1b7a4-373">Hello följande ytterligare steg om du skapar Data Factory-projekt i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-373">Do hello following additional steps if you are creating Data Factory project in Visual Studio:</span></span>
 
1. <span data-ttu-id="1b7a4-374">Lägg till hello Data Factory-projekt toohello Visual Studio-lösning som innehåller hello anpassad aktivitet projekt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-374">Add hello Data Factory project toohello Visual Studio solution that contains hello custom activity project.</span></span> 
2. <span data-ttu-id="1b7a4-375">Lägg till en referens toohello .NET aktivitet projektet från hello Data Factory-projekt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-375">Add a reference toohello .NET activity project from hello Data Factory project.</span></span> <span data-ttu-id="1b7a4-376">Högerklicka på Data Factory-projektet, peka för**Lägg till**, och klicka sedan på **referens**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-376">Right-click Data Factory project, point too**Add**, and then click **Reference**.</span></span> 
3. <span data-ttu-id="1b7a4-377">I hello **Lägg till referens** dialogrutan, Välj hello **MyDotNetActivity** projektet och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-377">In hello **Add Reference** dialog box, select hello **MyDotNetActivity** project, and click **OK**.</span></span>
4. <span data-ttu-id="1b7a4-378">Skapa och publicera hello lösning.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-378">Build and publish hello solution.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="1b7a4-379">När du publicerar Data Factory-entiteter, en zip-fil skapas automatiskt för dig och är överförda toohello blob-behållare: customactivitycontainer.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-379">When you publish Data Factory entities, a zip file is automatically created for you and is uploaded toohello blob container: customactivitycontainer.</span></span> <span data-ttu-id="1b7a4-380">Om hello blob-behållaren inte finns, skapas den automatiskt för.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-380">If hello blob container does not exist, it is automatically created too.</span></span>  


## <a name="data-factory-and-batch-integration"></a><span data-ttu-id="1b7a4-381">Data Factory och Batch-integrering</span><span class="sxs-lookup"><span data-stu-id="1b7a4-381">Data Factory and Batch integration</span></span>
<span data-ttu-id="1b7a4-382">hello Data Factory-tjänsten skapar ett jobb i Azure Batch med hello namn: **poolname adf: jobbet xxx**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-382">hello Data Factory service creates a job in Azure Batch with hello name: **adf-poolname: job-xxx**.</span></span> <span data-ttu-id="1b7a4-383">Klicka på **jobb** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-383">Click **Jobs** from hello left menu.</span></span> 

![Azure Data Factory - batchjobb](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

<span data-ttu-id="1b7a4-385">En aktivitet skapas för varje aktivitet kör för ett segment.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-385">A task is created for each activity run of a slice.</span></span> <span data-ttu-id="1b7a4-386">Om det finns fem segment-redo toobe bearbetas, skapas fem uppgifter i jobbet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-386">If there are five slices ready toobe processed, five tasks are created in this job.</span></span> <span data-ttu-id="1b7a4-387">Om det finns flera compute-noder i hello Batch-pool, kan två eller flera segment köras parallellt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-387">If there are multiple compute nodes in hello Batch pool, two or more slices can run in parallel.</span></span> <span data-ttu-id="1b7a4-388">Om hello maximala uppgifter per compute-nod har angetts för > 1, du kan också ha fler än ett segment som körs på hello samma beräkning.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-388">If hello maximum tasks per compute node is set too> 1, you can also have more than one slice running on hello same compute.</span></span>

![Azure Data Factory - jobbet batchaktiviteter](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

<span data-ttu-id="1b7a4-390">hello följande diagram illustrerar hello relationen mellan Azure Data Factory och Batch-aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-390">hello following diagram illustrates hello relationship between Azure Data Factory and Batch tasks.</span></span>

![Data Factory & Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a><span data-ttu-id="1b7a4-392">Felsöka fel</span><span class="sxs-lookup"><span data-stu-id="1b7a4-392">Troubleshoot failures</span></span>
<span data-ttu-id="1b7a4-393">Felsökning består av några grundläggande tekniker:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-393">Troubleshooting consists of a few basic techniques:</span></span>

1. <span data-ttu-id="1b7a4-394">Om du ser hello följande fel kan kanske du använder en aktiv/lågfrekvent blob-lagring i stället för med ett allmänt Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-394">If you see hello following error, you may be using a Hot/Cool blob storage instead of using a general-purpose Azure blob storage.</span></span> <span data-ttu-id="1b7a4-395">Överför hello zip-filen tooa **allmänna Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-395">Upload hello zip file tooa **general-purpose Azure Storage Account**.</span></span> 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of hello specified Azure Blob(s).
    ``` 
2. <span data-ttu-id="1b7a4-396">Om du ser följande fel hello bekräfta hello namnet på hello klass i hello CS matchar hello det angivna filnamnet för hello **EntryPoint** egenskap i hello pipeline-JSON.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-396">If you see hello following error, confirm that hello name of hello class in hello CS file matches hello name you specified for hello **EntryPoint** property in hello pipeline JSON.</span></span> <span data-ttu-id="1b7a4-397">I hello genomgången hello klassen heter: MyDotNetActivity och hello EntryPoint i hello JSON: MyDotNetActivityNS. **MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-397">In hello walkthrough, name of hello class is: MyDotNetActivity, and hello EntryPoint in hello JSON is: MyDotNetActivityNS.**MyDotNetActivity**.</span></span>

    ```
    MyDotNetActivity assembly does not exist or doesn't implement hello type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   <span data-ttu-id="1b7a4-398">Om hello namnen stämmer överens, bekräfta att alla hello binärfiler hello **rotmappen** av hello zip-filen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-398">If hello names do match, confirm that all hello binaries are in hello **root folder** of hello zip file.</span></span> <span data-ttu-id="1b7a4-399">När du öppnar hello zip-filen som är bör du se alla hello-filer i rotmappen för hello, inte i alla undermappar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-399">That is, when you open hello zip file, you should see all hello files in hello root folder, not in any sub folders.</span></span>   
3. <span data-ttu-id="1b7a4-400">Om hello inkommande sektorn inte har angetts för**klar**, bekräfta att hello inkommande mappstrukturen är korrekt och **fil.txt** finns i hello inkommande mappar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-400">If hello input slice is not set too**Ready**, confirm that hello input folder structure is correct and **file.txt** exists in hello input folders.</span></span>
3. <span data-ttu-id="1b7a4-401">I hello **kör** metod för din anpassade aktivitet, Använd hello **IActivityLogger** objekt toolog information som hjälper dig att felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-401">In hello **Execute** method of your custom activity, use hello **IActivityLogger** object toolog information that helps you troubleshoot issues.</span></span> <span data-ttu-id="1b7a4-402">hälsningsmeddelande loggas visas i loggfilerna för hello användare (en eller flera filer med namnet: användaren 0.log, användaren 1.log, användaren 2.log osv.).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-402">hello logged messages show up in hello user log files (one or more files named: user-0.log, user-1.log, user-2.log, etc.).</span></span>

   <span data-ttu-id="1b7a4-403">I hello **OutputDataset** bladet, klickar du på hello sektorn toosee hello **DATASEKTORN** bladet för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-403">In hello **OutputDataset** blade, click hello slice toosee hello **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="1b7a4-404">Du ser **aktivitetskörningar** för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-404">You see **activity runs** for that slice.</span></span> <span data-ttu-id="1b7a4-405">Du bör se en aktivitet som körs för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-405">You should see one activity run for hello slice.</span></span> <span data-ttu-id="1b7a4-406">Om du klickar på Kör i kommandofältet hello kan du starta en annan aktivitet som körs för hello samma segment.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-406">If you click Run in hello command bar, you can start another activity run for hello same slice.</span></span>

   <span data-ttu-id="1b7a4-407">När du klickar på hello aktivitet kör du ser hello **kör AKTIVITETSINFORMATION** bladet med en lista över loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-407">When you click hello activity run, you see hello **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="1b7a4-408">Du ser loggade meddelanden i hello user_0.log-filen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-408">You see logged messages in hello user_0.log file.</span></span> <span data-ttu-id="1b7a4-409">När ett fel uppstår finns tre aktivitetskörningar eftersom hello återförsöksvärde anges too3 i hello pipeline/aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-409">When an error occurs, you see three activity runs because hello retry count is set too3 in hello pipeline/activity JSON.</span></span> <span data-ttu-id="1b7a4-410">När du klickar på hello aktivitet kör se du hello loggfiler att du kan granska tootroubleshoot hello fel.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-410">When you click hello activity run, you see hello log files that you can review tootroubleshoot hello error.</span></span>

   <span data-ttu-id="1b7a4-411">Hello loggfiler, klicka på hello **användare 0.log**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-411">In hello list of log files, click hello **user-0.log**.</span></span> <span data-ttu-id="1b7a4-412">Hello högra panelen är hello resultatet av att använda hello **IActivityLogger.Write** metod.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-412">In hello right panel are hello results of using hello **IActivityLogger.Write** method.</span></span> <span data-ttu-id="1b7a4-413">Om du inte ser alla meddelanden, kontrollera om du har flera loggfiler med namnet: user_1.log, user_2.log osv. Annars kanske hello kod inte när hello senast loggade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-413">If you don't see all messages, check if you have more log files named: user_1.log, user_2.log etc. Otherwise, hello code may have failed after hello last logged message.</span></span>

   <span data-ttu-id="1b7a4-414">Du kan också kontrollera **system 0.log** för alla system felmeddelanden och undantag.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-414">In addition, check **system-0.log** for any system error messages and exceptions.</span></span>
4. <span data-ttu-id="1b7a4-415">Inkludera hello **PDB** filen i hello zip-filen så att hello felinformation har information som **anropsstacken** när ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-415">Include hello **PDB** file in hello zip file so that hello error details have information such as **call stack** when an error occurs.</span></span>
5. <span data-ttu-id="1b7a4-416">Alla hello filer i hello zip-filen för hello anpassad aktivitet måste vara på hello **övre nivå** med inga undermappar.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-416">All hello files in hello zip file for hello custom activity must be at hello **top level** with no sub folders.</span></span>
6. <span data-ttu-id="1b7a4-417">Se till att hello **assemblyName** (MyDotNetActivity.dll) **entryPoint**(MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip) och **packageLinkedService** (ska peka toohello **allmänna**Azure blob-lagring som innehåller hello zip-filen) har angetts toocorrect värden.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-417">Ensure that hello **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point toohello **general-purpose**Azure blob storage that contains hello zip file) are set toocorrect values.</span></span>
7. <span data-ttu-id="1b7a4-418">Om du har löst ett fel och vill tooreprocess hello segment Högerklicka hello sektor i hello **OutputDataset** bladet och klicka på **kör**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-418">If you fixed an error and want tooreprocess hello slice, right-click hello slice in hello **OutputDataset** blade and click **Run**.</span></span>
8. <span data-ttu-id="1b7a4-419">Om du ser följande fel hello använder du hello Azure Storage-paketet med version > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-419">If you see hello following error, you are using hello Azure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="1b7a4-420">Startprogram för data Factory-tjänsten kräver hello 4.3 version av WindowsAzure.Storage.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-420">Data Factory service launcher requires hello 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="1b7a4-421">Se [Appdomain isolering](#appdomain-isolation) avsnittet för en arbete runt om du måste använda hello senare version av Azure Storage-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-421">See [Appdomain isolation](#appdomain-isolation) section for a work-around if you must use hello later version of Azure Storage assembly.</span></span> 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by hello target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    <span data-ttu-id="1b7a4-422">Om du kan använda hello 4.3.0 versionen av Azure Storage, ta bort hello befintliga referens tooAzure lagring paketet med version > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-422">If you can use hello 4.3.0 version of Azure Storage package, remove hello existing reference tooAzure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="1b7a4-423">Kör sedan följande kommando från NuGet Package Manager-konsolen hello.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-423">Then, run hello following command from NuGet Package Manager Console.</span></span> 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    <span data-ttu-id="1b7a4-424">Skapa hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-424">Build hello project.</span></span> <span data-ttu-id="1b7a4-425">Ta bort Azure.Storage sammansättningen av version > 4.3.0 från hello bin\Debug mapp.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-425">Delete Azure.Storage assembly of version > 4.3.0 from hello bin\Debug folder.</span></span> <span data-ttu-id="1b7a4-426">Skapa en zip-fil med binärfiler och hello PDB-filen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-426">Create a zip file with binaries and hello PDB file.</span></span> <span data-ttu-id="1b7a4-427">Ersätta hello gamla zip-filen med den här i hello blob-behållaren (customactivitycontainer).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-427">Replace hello old zip file with this one in hello blob container (customactivitycontainer).</span></span> <span data-ttu-id="1b7a4-428">Kör hello sektorer som inte kunde (Högerklicka segment och klicka på Kör).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-428">Rerun hello slices that failed (right-click slice, and click Run).</span></span>   
8. <span data-ttu-id="1b7a4-429">hello anpassad aktivitet använder inte hello **app.config** filen från paketet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-429">hello custom activity does not use hello **app.config** file from your package.</span></span> <span data-ttu-id="1b7a4-430">Därför om koden läser eventuella anslutningssträngar från hello konfigurationsfil, fungerar det inte vid körning.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-430">Therefore, if your code reads any connection strings from hello configuration file, it does not work at runtime.</span></span> <span data-ttu-id="1b7a4-431">Hej bäst när med hjälp av Azure Batch är toohold alla hemligheter i en **Azure KeyVault**, använda en certifikatbaserad service principal tooprotect hello **keyvault**, och distribuera hello certifikat tooAzure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-431">hello best practice when using Azure Batch is toohold any secrets in an **Azure KeyVault**, use a certificate-based service principal tooprotect hello **keyvault**, and distribute hello certificate tooAzure Batch pool.</span></span> <span data-ttu-id="1b7a4-432">hello kan .NET anpassad aktivitet sedan använda hemligheter från hello KeyVault vid körning.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-432">hello .NET custom activity then can access secrets from hello KeyVault at runtime.</span></span> <span data-ttu-id="1b7a4-433">Den här lösningen är en allmän lösning och kan skala tooany typ av hemlighet, inte bara anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-433">This solution is a generic solution and can scale tooany type of secret, not just connection string.</span></span>

   <span data-ttu-id="1b7a4-434">Det finns en enklare lösning (men inte rekommenderas): du kan skapa en **Azure SQL länkade tjänsten** med strängen anslutningsinställningar, skapa en datamängd som använder hello länkad tjänst och kedjan hello datamängd som en dummy inkommande datauppsättning toohello anpassad .NET-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-434">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses hello linked service, and chain hello dataset as a dummy input dataset toohello custom .NET activity.</span></span> <span data-ttu-id="1b7a4-435">Du kan sedan åtkomst hello länkade tjänstens anslutningssträngen i hello anpassad Aktivitetskod.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-435">You can then access hello linked service's connection string in hello custom activity code.</span></span>  

## <a name="update-custom-activity"></a><span data-ttu-id="1b7a4-436">Uppdatera anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="1b7a4-436">Update custom activity</span></span>
<span data-ttu-id="1b7a4-437">Om du uppdaterar hello koden för hello anpassad aktivitet skapar det och överför hello zip-fil som innehåller nya binärfiler toohello blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-437">If you update hello code for hello custom activity, build it, and upload hello zip file that contains new binaries toohello blob storage.</span></span>

## <a name="appdomain-isolation"></a><span data-ttu-id="1b7a4-438">AppDomain isolering</span><span class="sxs-lookup"><span data-stu-id="1b7a4-438">Appdomain isolation</span></span>
<span data-ttu-id="1b7a4-439">Se [mellan AppDomain-exempel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) som visar hur toocreate en anpassad aktivitet som inte är begränsad tooassembly-versioner som används av hello Data Factory programstart (exempel: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x osv.).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-439">See [Cross AppDomain Sample](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) that shows you how toocreate a custom activity that is not constrained tooassembly versions used by hello Data Factory launcher (example: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, etc.).</span></span>

## <a name="access-extended-properties"></a><span data-ttu-id="1b7a4-440">Åtkomst till utökade egenskaper</span><span class="sxs-lookup"><span data-stu-id="1b7a4-440">Access extended properties</span></span>
<span data-ttu-id="1b7a4-441">Du kan deklarera utökade egenskaper i hello aktivitets-JSON som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-441">You can declare extended properties in hello activity JSON as shown in hello following sample:</span></span>

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


<span data-ttu-id="1b7a4-442">Det finns två utökade egenskaper i hello exempelvis: **SliceStart** och **DataFactoryName**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-442">In hello example, there are two extended properties: **SliceStart** and **DataFactoryName**.</span></span> <span data-ttu-id="1b7a4-443">hello-värdet för SliceStart baseras på hello SliceStart systemvariabeln.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-443">hello value for SliceStart is based on hello SliceStart system variable.</span></span> <span data-ttu-id="1b7a4-444">Se [systemvariabler](data-factory-functions-variables.md) för en lista över variabler system som stöds.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-444">See [System Variables](data-factory-functions-variables.md) for a list of supported system variables.</span></span> <span data-ttu-id="1b7a4-445">hello-värdet för DataFactoryName är hårdkodat tooCustomActivityFactory.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-445">hello value for DataFactoryName is hard-coded tooCustomActivityFactory.</span></span>

<span data-ttu-id="1b7a4-446">tooaccess dessa utökade egenskaper i hello **Execute** metod, Använd koden liknande toohello följande kod:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-446">tooaccess these extended properties in hello **Execute** method, use code similar toohello following code:</span></span>

```csharp
// tooget extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// toolog all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a><span data-ttu-id="1b7a4-447">Automatisk skalning av Azure Batch</span><span class="sxs-lookup"><span data-stu-id="1b7a4-447">Auto-scaling of Azure Batch</span></span>
<span data-ttu-id="1b7a4-448">Du kan också skapa en Azure Batch-pool med **Autoskala** funktion.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-448">You can also create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="1b7a4-449">Du kan till exempel skapa en azure batch-pool med 0 dedikerade virtuella datorer och en Autoskala formel baserat på hello antal väntande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-449">For example, you could create an azure batch pool with 0 dedicated VMs and an autoscale formula based on hello number of pending tasks.</span></span> 

<span data-ttu-id="1b7a4-450">här hello formeln uppnår hello följande beteende: när hello poolen skapas, det börjar med 1 VM.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-450">hello sample formula here achieves hello following behavior: When hello pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="1b7a4-451">$PendingTasks mått definierar hello antalet aktiviteter i körs + aktiv (i kö) tillstånd.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-451">$PendingTasks metric defines hello number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="1b7a4-452">hello formeln hittar hello Genomsnittligt antal väntande aktiviteter i hello senaste 180 sekunder och anger därefter TargetDedicated.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-452">hello formula finds hello average number of pending tasks in hello last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="1b7a4-453">Det garanterar att TargetDedicated aldrig är mer omfattande än 25 virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-453">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="1b7a4-454">Så som nya aktiviteter skickas pool växer automatiskt och som slutförda uppgifter, virtuella datorer blir ledigt i taget och hello autoskalning krymper dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-454">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and hello autoscaling shrinks those VMs.</span></span> <span data-ttu-id="1b7a4-455">startingNumberOfVMs och maxNumberofVMs kan vara justerade tooyour behov.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-455">startingNumberOfVMs and maxNumberofVMs can be adjusted tooyour needs.</span></span>

<span data-ttu-id="1b7a4-456">Autoskala formel:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-456">Autoscale formula:</span></span>

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

<span data-ttu-id="1b7a4-457">Se [automatiskt skala compute-noder i en Azure Batch-pool](../batch/batch-automatic-scaling.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-457">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

<span data-ttu-id="1b7a4-458">Om hello pool använder hello standard [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch-tjänsten kan ta 15 30 minuter tooprepare hello VM innan du kör hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-458">If hello pool is using hello default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch service could take 15-30 minutes tooprepare hello VM before running hello custom activity.</span></span>  <span data-ttu-id="1b7a4-459">Om hello pool använder en annan autoScaleEvaluationInterval, kan hello Batch-tjänsten ta autoScaleEvaluationInterval + 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-459">If hello pool is using a different autoScaleEvaluationInterval, hello Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>

## <a name="use-hdinsight-compute-service"></a><span data-ttu-id="1b7a4-460">Använda HDInsight beräknings-tjänst</span><span class="sxs-lookup"><span data-stu-id="1b7a4-460">Use HDInsight compute service</span></span>
<span data-ttu-id="1b7a4-461">I hello genomgången används Azure Batch beräkning toorun hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-461">In hello walkthrough, you used Azure Batch compute toorun hello custom activity.</span></span> <span data-ttu-id="1b7a4-462">Du kan också använda egna Windows-baserade HDInsight-kluster eller har Data Factory skapar på-begäran-Windows-baserade HDInsight-kluster och har hello anpassad aktivitet som körs på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-462">You can also use your own Windows-based HDInsight cluster or have Data Factory create an on-demand Windows-based HDInsight cluster and have hello custom activity run on hello HDInsight cluster.</span></span> <span data-ttu-id="1b7a4-463">Här följer hello anvisningar för att använda ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-463">Here are hello high-level steps for using an HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1b7a4-464">hello anpassade .NET aktiviteter körs bara på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-464">hello custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="1b7a4-465">En lösning för den här begränsningen är toouse hello kartan minska aktiviteten toorun anpassad Java-kod på en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-465">A workaround for this limitation is toouse hello Map Reduce Activity toorun custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="1b7a4-466">Ett annat alternativ är toouse Azure Batch-pool för virtuella datorer toorun anpassade aktiviteter i stället för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-466">Another option is toouse an Azure Batch pool of VMs toorun custom activities instead of using a HDInsight cluster.</span></span>
 

1. <span data-ttu-id="1b7a4-467">Skapa en Azure HDInsight länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-467">Create an Azure HDInsight linked service.</span></span>   
2. <span data-ttu-id="1b7a4-468">Använd HDInsight länkade tjänsten i stället för **AzureBatchLinkedService** i hello pipeline-JSON.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-468">Use HDInsight linked service in place of **AzureBatchLinkedService** in hello pipeline JSON.</span></span>

<span data-ttu-id="1b7a4-469">Om du vill tootest med hello genomgången ändra **starta** och **end** tidsgränsen för hello pipelinen, så att du kan testa hello scenario med hello Azure HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-469">If you want tootest it with hello walkthrough, change **start** and **end** times for hello pipeline so that you can test hello scenario with hello Azure HDInsight service.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="1b7a4-470">Skapa en Azure HDInsight-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="1b7a4-470">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="1b7a4-471">hello Azure Data Factory-tjänsten stöder skapandet av ett kluster på begäran och använder det tooprocess inkommande tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-471">hello Azure Data Factory service supports creation of an on-demand cluster and use it tooprocess input tooproduce output data.</span></span> <span data-ttu-id="1b7a4-472">Du kan också använda ditt eget kluster tooperform hello samma.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-472">You can also use your own cluster tooperform hello same.</span></span> <span data-ttu-id="1b7a4-473">När du använder HDInsight-kluster på begäran, skapas ett kluster för varje segment.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-473">When you use on-demand HDInsight cluster, a cluster gets created for each slice.</span></span> <span data-ttu-id="1b7a4-474">Om du använder egna HDInsight-kluster hello klustret är klar hello tooprocess sektorn omedelbart.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-474">Whereas, if you use your own HDInsight cluster, hello cluster is ready tooprocess hello slice immediately.</span></span> <span data-ttu-id="1b7a4-475">Därför när du använder kluster på begäran måste kanske du inte visas hello utdata så snabbt som när du använder ditt eget kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-475">Therefore, when you use on-demand cluster, you may not see hello output data as quickly as when you use your own cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="1b7a4-476">Vid körning körs en instans av en .NET-aktiviteten bara på en worker-nod i hello HDInsight-klustret. inte kan skaländras toorun på flera noder.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-476">At runtime, an instance of a .NET activity runs only on one worker node in hello HDInsight cluster; it cannot be scaled toorun on multiple nodes.</span></span> <span data-ttu-id="1b7a4-477">Flera instanser av .NET-aktiviteten kan köras parallellt på olika noder i hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-477">Multiple instances of .NET activity can run in parallel on different nodes of hello HDInsight cluster.</span></span>
>
>

##### <a name="toouse-an-on-demand-hdinsight-cluster"></a><span data-ttu-id="1b7a4-478">toouse ett HDInsight-kluster på begäran</span><span class="sxs-lookup"><span data-stu-id="1b7a4-478">toouse an on-demand HDInsight cluster</span></span>
1. <span data-ttu-id="1b7a4-479">I hello **Azure-portalen**, klickar du på **författare och distribution** i hello Data Factory-startsidan.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-479">In hello **Azure portal**, click **Author and Deploy** in hello Data Factory home page.</span></span>
2. <span data-ttu-id="1b7a4-480">I hello Data Factory-redigeraren, klickar du på **nya beräkning** från hello kommandot och välj **på begäran HDInsight-kluster** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-480">In hello Data Factory Editor, click **New compute** from hello command bar and select **On-demand HDInsight cluster** from hello menu.</span></span>
3. <span data-ttu-id="1b7a4-481">Gör följande ändringar toohello JSON-skript hello:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-481">Make hello following changes toohello JSON script:</span></span>

   1. <span data-ttu-id="1b7a4-482">För hello **clusterSize** -egenskapen anger hello storleken på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-482">For hello **clusterSize** property, specify hello size of hello HDInsight cluster.</span></span>
   2. <span data-ttu-id="1b7a4-483">För hello **timeToLive** -egenskapen anger hur länge hello kunden kan vara inaktiv innan den tas bort.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-483">For hello **timeToLive** property, specify how long hello customer can be idle before it is deleted.</span></span>
   3. <span data-ttu-id="1b7a4-484">För hello **version** egenskap, ange hello HDInsight-version som du vill använda toouse.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-484">For hello **version** property, specify hello HDInsight version you want toouse.</span></span> <span data-ttu-id="1b7a4-485">Om du utesluter den här egenskapen används hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-485">If you exclude this property, hello latest version is used.</span></span>  
   4. <span data-ttu-id="1b7a4-486">För hello **linkedServiceName**, ange **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-486">For hello **linkedServiceName**, specify **AzureStorageLinkedService**.</span></span>

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
    > <span data-ttu-id="1b7a4-487">hello anpassade .NET aktiviteter körs bara på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-487">hello custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="1b7a4-488">En lösning för den här begränsningen är toouse hello kartan minska aktiviteten toorun anpassad Java-kod på en Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-488">A workaround for this limitation is toouse hello Map Reduce Activity toorun custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="1b7a4-489">Ett annat alternativ är toouse Azure Batch-pool för virtuella datorer toorun anpassade aktiviteter i stället för ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-489">Another option is toouse an Azure Batch pool of VMs toorun custom activities instead of using a HDInsight cluster.</span></span>

4. <span data-ttu-id="1b7a4-490">Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-490">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

##### <a name="toouse-your-own-hdinsight-cluster"></a><span data-ttu-id="1b7a4-491">toouse egna HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-491">toouse your own HDInsight cluster:</span></span>
1. <span data-ttu-id="1b7a4-492">I hello **Azure-portalen**, klickar du på **författare och distribution** i hello Data Factory-startsidan.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-492">In hello **Azure portal**, click **Author and Deploy** in hello Data Factory home page.</span></span>
2. <span data-ttu-id="1b7a4-493">I hello **Data Factory-redigeraren**, klickar du på **nya beräkning** från hello kommandot och välj **HDInsight-kluster** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-493">In hello **Data Factory Editor**, click **New compute** from hello command bar and select **HDInsight cluster** from hello menu.</span></span>
3. <span data-ttu-id="1b7a4-494">Gör följande ändringar toohello JSON-skript hello:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-494">Make hello following changes toohello JSON script:</span></span>

   1. <span data-ttu-id="1b7a4-495">För hello **clusterUri** egenskap, ange hello URL för din HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-495">For hello **clusterUri** property, enter hello URL for your HDInsight.</span></span> <span data-ttu-id="1b7a4-496">Till exempel: https://<clustername>.azurehdinsight.net/</span><span class="sxs-lookup"><span data-stu-id="1b7a4-496">For example: https://<clustername>.azurehdinsight.net/</span></span>     
   2. <span data-ttu-id="1b7a4-497">För hello **användarnamn** egenskap, ange hello användarnamn som har åtkomst toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-497">For hello **UserName** property, enter hello user name who has access toohello HDInsight cluster.</span></span>
   3. <span data-ttu-id="1b7a4-498">För hello **lösenord** egenskap, ange hello lösenord för hello användaren.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-498">For hello **Password** property, enter hello password for hello user.</span></span>
   4. <span data-ttu-id="1b7a4-499">För hello **LinkedServiceName** egenskap, ange **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-499">For hello **LinkedServiceName** property, enter **AzureStorageLinkedService**.</span></span>
4. <span data-ttu-id="1b7a4-500">Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-500">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

<span data-ttu-id="1b7a4-501">Se [Compute länkade tjänster](data-factory-compute-linked-services.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-501">See [Compute linked services](data-factory-compute-linked-services.md) for details.</span></span>

<span data-ttu-id="1b7a4-502">I hello **pipeline-JSON**, använda HDInsight (på begäran eller egna) länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="1b7a4-502">In hello **pipeline JSON**, use HDInsight (on-demand or your own) linked service:</span></span>

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

## <a name="create-a-custom-activity-by-using-net-sdk"></a><span data-ttu-id="1b7a4-503">Skapa en anpassad aktivitet med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="1b7a4-503">Create a custom activity by using .NET SDK</span></span>
<span data-ttu-id="1b7a4-504">I hello genomgången i den här artikeln skapa en datafabrik med en pipeline som använder hello anpassad aktivitet genom att använda hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-504">In hello walkthrough in this article, you create a data factory with a pipeline that uses hello custom activity by using hello Azure portal.</span></span> <span data-ttu-id="1b7a4-505">hello följande kod visar hur toocreate hello data factory med hjälp av .NET SDK i stället.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-505">hello following code shows you how toocreate hello data factory by using .NET SDK instead.</span></span> <span data-ttu-id="1b7a4-506">Du hittar mer information om hur du använder SDK tooprogrammatically skapa pipelines i hello [skapar en pipeline med kopieringsaktiviteten med hjälp av .NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-506">You can find more details about using SDK tooprogrammatically create pipelines in hello [create a pipeline with copy activity by using .NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md) article.</span></span> 

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

            // TODO: replace ADFTutorialResourceGroup with hello name of your resource group.
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

                            // Initial value for pipeline's active period. With this, you won't need tooset slice status
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

            throw new InvalidOperationException("Failed tooacquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a><span data-ttu-id="1b7a4-507">Felsöka anpassad aktivitet i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1b7a4-507">Debug custom activity in Visual Studio</span></span>
<span data-ttu-id="1b7a4-508">Hej [Azure Data Factory - lokal miljö](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) prov på GitHub innehåller ett verktyg som gör att du toodebug anpassad .NET-aktiviteter i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-508">hello [Azure Data Factory - local environment](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) sample on GitHub includes a tool that allows you toodebug custom .NET activities within Visual Studio.</span></span>  


## <a name="sample-custom-activities-on-github"></a><span data-ttu-id="1b7a4-509">Exempel anpassade aktiviteter på GitHub</span><span class="sxs-lookup"><span data-stu-id="1b7a4-509">Sample custom activities on GitHub</span></span>
| <span data-ttu-id="1b7a4-510">Exempel</span><span class="sxs-lookup"><span data-stu-id="1b7a4-510">Sample</span></span> | <span data-ttu-id="1b7a4-511">Vilka anpassade aktiviteten har</span><span class="sxs-lookup"><span data-stu-id="1b7a4-511">What custom activity does</span></span> |
| --- | --- |
| <span data-ttu-id="1b7a4-512">[HTTP-Data Installationshämtaren](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-512">[HTTP Data Downloader](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span></span> |<span data-ttu-id="1b7a4-513">Hämtar data från en HTTP-slutpunkt tooAzure Blob Storage med hjälp av anpassade C# aktiviteten i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-513">Downloads data from an HTTP Endpoint tooAzure Blob Storage using custom C# Activity in Data Factory.</span></span> |
| [<span data-ttu-id="1b7a4-514">Twitter-Sentiment Analysis-exempel</span><span class="sxs-lookup"><span data-stu-id="1b7a4-514">Twitter Sentiment Analysis sample</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |<span data-ttu-id="1b7a4-515">Anropar en Azure ML-modell och vill sentiment analys, bedömningen, förutsägelse osv.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-515">Invokes an Azure ML model and do sentiment analysis, scoring, prediction etc.</span></span> |
| <span data-ttu-id="1b7a4-516">[Kör R-skriptet](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span><span class="sxs-lookup"><span data-stu-id="1b7a4-516">[Run R Script](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span></span> |<span data-ttu-id="1b7a4-517">Anropar R-skriptet genom att köra RScript.exe på ditt HDInsight-kluster som redan har R installerat på den.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-517">Invokes R script by running RScript.exe on your HDInsight cluster that already has R Installed on it.</span></span> |
| [<span data-ttu-id="1b7a4-518">Mellan AppDomain .NET-aktiviteten</span><span class="sxs-lookup"><span data-stu-id="1b7a4-518">Cross AppDomain .NET Activity</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |<span data-ttu-id="1b7a4-519">Använder olika sammansättningen versioner från de som används av hello Data Factory programstart</span><span class="sxs-lookup"><span data-stu-id="1b7a4-519">Uses different assembly versions from ones used by hello Data Factory launcher</span></span> |
| [<span data-ttu-id="1b7a4-520">Bearbeta en modell i Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="1b7a4-520">Reprocess a model in Azure Analysis Services</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  <span data-ttu-id="1b7a4-521">Ombearbeta en modell i Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="1b7a4-521">Reprocesses a model in Azure Analysis Services.</span></span> |

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
