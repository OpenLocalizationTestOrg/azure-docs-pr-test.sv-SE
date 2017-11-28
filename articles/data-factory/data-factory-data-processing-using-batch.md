---
title: "aaaProcess stora datauppsättningar som använder Data Factory och Batch | Microsoft Docs"
description: "Beskriver hur tooprocess stora mängder data i ett Azure Data Factory pipeline med hjälp av kapaciteten för parallell bearbetning av Azure Batch."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 6788f02de555d2e9d6588cc990a39043866d7e97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a><span data-ttu-id="9272d-103">Bearbeta datauppsättningar i stor skala med Data Factory och Batch</span><span class="sxs-lookup"><span data-stu-id="9272d-103">Process large-scale datasets using Data Factory and Batch</span></span>
<span data-ttu-id="9272d-104">Den här artikeln beskriver en arkitektur på en exempellösning som flyttar och bearbetar stora datauppsättningar på automatisk och schemalagda sätt.</span><span class="sxs-lookup"><span data-stu-id="9272d-104">This article describes an architecture of a sample solution that moves and processes large-scale datasets in an automatic and scheduled manner.</span></span> <span data-ttu-id="9272d-105">Det ger också en slutpunkt till slutpunkt genomgången tooimplement hello-lösning med hjälp av Azure Data Factory och Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="9272d-105">It also provides an end-to-end walkthrough tooimplement hello solution using Azure Data Factory and Azure Batch.</span></span>

<span data-ttu-id="9272d-106">Den här artikeln är längre än våra vanliga artikeln eftersom den innehåller en genomgång av en lösning för hela exemplet.</span><span class="sxs-lookup"><span data-stu-id="9272d-106">This article is longer than our typical article because it contains a walkthrough of an entire sample solution.</span></span> <span data-ttu-id="9272d-107">Om du är ny tooBatch och Data Factory, du kan lära dig om dessa tjänster och hur de fungerar tillsammans.</span><span class="sxs-lookup"><span data-stu-id="9272d-107">If you are new tooBatch and Data Factory, you can learn about these services and how they work together.</span></span> <span data-ttu-id="9272d-108">Om du vet något om hello tjänster och designa/utforma en lösning, kan du fokusera just på hello [arkitektur avsnittet](#architecture-of-sample-solution) hello artikel och om du utvecklar en prototyp eller en lösning, du kan också tootry stegvisa instruktioner i hello [genomgången](#implementation-of-sample-solution).</span><span class="sxs-lookup"><span data-stu-id="9272d-108">If you know something about hello services and are designing/architecting a solution, you may focus just on hello [architecture section](#architecture-of-sample-solution) of hello article and if you are developing a prototype or a solution, you may also want tootry out step-by-step instructions in hello [walkthrough](#implementation-of-sample-solution).</span></span> <span data-ttu-id="9272d-109">Vi erbjuder kommentarer om det här innehållet och hur du använder den.</span><span class="sxs-lookup"><span data-stu-id="9272d-109">We invite your comments about this content and how you use it.</span></span>

<span data-ttu-id="9272d-110">Först ska vi titta på hur Data Factory och Batch-tjänster kan hjälpa med bearbetning av stora datamängder i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="9272d-110">First, let's look at how Data Factory and Batch services can help with processing large datasets in hello cloud.</span></span>     

## <a name="why-azure-batch"></a><span data-ttu-id="9272d-111">Varför Azure Batch?</span><span class="sxs-lookup"><span data-stu-id="9272d-111">Why Azure Batch?</span></span>
<span data-ttu-id="9272d-112">Azure Batch kan toorun storskaliga parallella och högpresterande datorbearbetning (HPC) program effektivt i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="9272d-112">Azure Batch enables you toorun large-scale parallel and high-performance computing (HPC) applications efficiently in hello cloud.</span></span> <span data-ttu-id="9272d-113">Det är en platform-tjänst som schemalägger beräkningsintensiva arbete toorun på en hanterad samling av virtuella datorer och kan automatiskt skala beräkna resurser toomeet hello behoven hos dina jobb.</span><span class="sxs-lookup"><span data-stu-id="9272d-113">It's a platform service that schedules compute-intensive work toorun on a managed collection of virtual machines, and can automatically scale compute resources toomeet hello needs of your jobs.</span></span>

<span data-ttu-id="9272d-114">Med hello Batch-tjänsten definierar du Azure compute resurser tooexecute dina program parallellt, och i större skala.</span><span class="sxs-lookup"><span data-stu-id="9272d-114">With hello Batch service, you define Azure compute resources tooexecute your applications in parallel, and at scale.</span></span> <span data-ttu-id="9272d-115">Du kan köra på begäran eller schemalagt jobb, och du behöver inte toomanually skapa, konfigurera och hantera ett HPC-kluster, enskilda virtuella datorer, virtuella nätverk eller ett komplexa jobb och uppgift schemaläggning infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="9272d-115">You can run on-demand or scheduled jobs, and you don't need toomanually create, configure, and manage an HPC cluster, individual virtual machines, virtual networks, or a complex job and task scheduling infrastructure.</span></span>

<span data-ttu-id="9272d-116">Se hello följande artiklar om du inte är bekant med Azure Batch eftersom det hjälper med att förstå hello arkitektur/implementering av hello-lösningen som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9272d-116">See hello following articles if you are not familiar with Azure Batch as it helps with understanding hello architecture/implementation of hello solution described in this article.</span></span>   

* [<span data-ttu-id="9272d-117">Grunderna i Azure Batch</span><span class="sxs-lookup"><span data-stu-id="9272d-117">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
* [<span data-ttu-id="9272d-118">Översikt över funktionen för batch</span><span class="sxs-lookup"><span data-stu-id="9272d-118">Batch feature overview</span></span>](../batch/batch-api-basics.md)

<span data-ttu-id="9272d-119">(valfritt) toolearn mer om Azure Batch finns hello [Utbildningsväg för Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).</span><span class="sxs-lookup"><span data-stu-id="9272d-119">(optional) toolearn more about Azure Batch, see hello [Learning path for Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).</span></span>

## <a name="why-azure-data-factory"></a><span data-ttu-id="9272d-120">Varför Azure Data Factory?</span><span class="sxs-lookup"><span data-stu-id="9272d-120">Why Azure Data Factory?</span></span>
<span data-ttu-id="9272d-121">Data Factory är en molnbaserad integration datatjänst som samordnar och automatiserar hello flytt och transformering av data.</span><span class="sxs-lookup"><span data-stu-id="9272d-121">Data Factory is a cloud-based data integration service that orchestrates and automates hello movement and transformation of data.</span></span> <span data-ttu-id="9272d-122">Med hello Data Factory-tjänsten kan du skapa hanterade data pipelines som flytta data från lokala och molnet lagrar tooa centraliserad data datalager (till exempel: Azure Blob Storage), och processen/Transformera data med tjänster som Azure HDInsight och Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9272d-122">Using hello Data Factory service, you can create managed data pipelines that move data from on-premises and cloud data stores tooa centralized data store (for example: Azure Blob Storage), and process/transform data using services such as Azure HDInsight and Azure Machine Learning.</span></span> <span data-ttu-id="9272d-123">Du kan också schemalägga data pipelines toorun i en schemalagd tid (varje timme, varje dag, varje vecka, etc.) och övervaka och hantera dem på en snabb överblick över tooidentify problem och vidta åtgärder.</span><span class="sxs-lookup"><span data-stu-id="9272d-123">You can also schedule data pipelines toorun in a scheduled manner (hourly, daily, weekly, etc.) and monitor and manage them at a glance tooidentify issues and take action.</span></span>

<span data-ttu-id="9272d-124">Se hello efter artiklar om du inte är bekant med Azure Data Factory eftersom det hjälper med att förstå hello arkitektur/implementering av hello-lösningen som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9272d-124">See hello following articles if you are not familiar with Azure Data Factory as it helps with understanding hello architecture/implementation of hello solution described in this article.</span></span>  

* [<span data-ttu-id="9272d-125">Införandet av Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="9272d-125">Introduction of Azure Data Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="9272d-126">Skapa din första pipeline för data</span><span class="sxs-lookup"><span data-stu-id="9272d-126">Build your first data pipeline</span></span>](data-factory-build-your-first-pipeline.md)   

<span data-ttu-id="9272d-127">(valfritt) toolearn mer om Azure Data Factory finns hello [för Azure Data Factory-Utbildningsväg](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="9272d-127">(optional) toolearn more about Azure Data Factory, see hello [Learning path for Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span></span>

## <a name="data-factory-and-batch-together"></a><span data-ttu-id="9272d-128">Data Factory och Batch tillsammans</span><span class="sxs-lookup"><span data-stu-id="9272d-128">Data Factory and Batch together</span></span>
<span data-ttu-id="9272d-129">Data Factory innehåller inbyggda aktiviteter, t.ex. Kopieringsaktiviteten toocopy/flytta data från en källdata tooa målarkiv data och Hive aktiviteten tooprocess data med Hadoop-kluster (HDInsight) på Azure.</span><span class="sxs-lookup"><span data-stu-id="9272d-129">Data Factory includes built-in activities such as Copy Activity toocopy/move data from a source data store tooa destination data store and Hive Activity tooprocess data using Hadoop clusters (HDInsight) on Azure.</span></span> <span data-ttu-id="9272d-130">Se [Data Transformation aktiviteter](data-factory-data-transformation-activities.md) en lista över aktiviteter för omvandling av stöds.</span><span class="sxs-lookup"><span data-stu-id="9272d-130">See [Data Transformation Activities](data-factory-data-transformation-activities.md) for a list of supported transformation activities.</span></span>

<span data-ttu-id="9272d-131">Dessutom kan du toocreate anpassade .NET aktiviteter toomove eller process data med egna logik och köra dessa aktiviteter på Azure HDInsight-kluster eller ett Azure Batch-pool för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9272d-131">It also allows you toocreate custom .NET activities toomove or process data with your own logic and run these activities on an Azure HDInsight cluster or on an Azure Batch pool of VMs.</span></span> <span data-ttu-id="9272d-132">När du använder Azure Batch, kan du konfigurera hello poolen tooauto skalning (lägga till eller ta bort virtuella datorer baserat på arbetsbelastning hello) utifrån en formel som du anger.</span><span class="sxs-lookup"><span data-stu-id="9272d-132">When you use Azure Batch, you can configure hello pool tooauto-scale (add or remove VMs based on hello workload) based on a formula you provide.</span></span>     

## <a name="architecture-of-sample-solution"></a><span data-ttu-id="9272d-133">Arkitektur för exempellösning</span><span class="sxs-lookup"><span data-stu-id="9272d-133">Architecture of sample solution</span></span>
<span data-ttu-id="9272d-134">Även om hello-arkitekturen som beskrivs i den här artikeln är avsedd för en enkel lösning, är det relevanta toocomplex scenarier, till exempel risk modellering av finansiella tjänster, bild bearbetning och återgivning och genom analys.</span><span class="sxs-lookup"><span data-stu-id="9272d-134">Even though hello architecture described in this article is for a simple solution, it is relevant toocomplex scenarios such as risk modeling by financial services, image processing and rendering, and genomic analysis.</span></span>

<span data-ttu-id="9272d-135">hello diagram illustrerar 1) hur Data Factory samordnar dataförflyttning och bearbetning och 2) hur Azure Batch bearbetar hello data på ett sätt som parallellt.</span><span class="sxs-lookup"><span data-stu-id="9272d-135">hello diagram illustrates 1) how Data Factory orchestrates data movement and processing and 2) how Azure Batch processes hello data in a parallel manner.</span></span> <span data-ttu-id="9272d-136">Hämta och skriva ut hello diagram för enkel referens (11 x 17 i.</span><span class="sxs-lookup"><span data-stu-id="9272d-136">Download and print hello diagram for easy reference (11 x 17 in.</span></span> <span data-ttu-id="9272d-137">eller A3-storlek): [HPC och data orchestration med hjälp av Azure Batch och Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).</span><span class="sxs-lookup"><span data-stu-id="9272d-137">or A3 size): [HPC and data orchestration using Azure Batch and Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).</span></span>

<span data-ttu-id="9272d-138">[![Diagram för storskalig databearbetning](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span><span class="sxs-lookup"><span data-stu-id="9272d-138">[![Large-scale data processing diagram](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span></span>

<span data-ttu-id="9272d-139">hello innehåller följande lista hello grundläggande stegen hello-processen.</span><span class="sxs-lookup"><span data-stu-id="9272d-139">hello following list provides hello basic steps of hello process.</span></span> <span data-ttu-id="9272d-140">hello lösningen innehåller koden och förklaringar toobuild hello slutpunkt till slutpunkt-lösning.</span><span class="sxs-lookup"><span data-stu-id="9272d-140">hello solution includes code and explanations toobuild hello end-to-end solution.</span></span>

1. <span data-ttu-id="9272d-141">**Konfigurera Azure Batch med en pool med compute-noder (VM)**.</span><span class="sxs-lookup"><span data-stu-id="9272d-141">**Configure Azure Batch with a pool of compute nodes (VMs)**.</span></span> <span data-ttu-id="9272d-142">Du kan ange hello antal noder och storleken på varje nod.</span><span class="sxs-lookup"><span data-stu-id="9272d-142">You can specify hello number of nodes and size of each node.</span></span>
2. <span data-ttu-id="9272d-143">**Skapa en instans av Azure Data Factory** som har konfigurerats med entiteter som representerar Azure blob-lagring, beräkning Azure Batch-tjänsten, i/o-data och arbetsflöde/pipeline med aktiviteter som att flytta och transformera data.</span><span class="sxs-lookup"><span data-stu-id="9272d-143">**Create an Azure Data Factory instance** that is configured with entities that represent Azure blob storage, Azure Batch compute service, input/output data, and a workflow/pipeline with activities that move and transform data.</span></span>
3. <span data-ttu-id="9272d-144">**Skapa en anpassad .NET-aktivitet i hello Data Factory-pipelinen**.</span><span class="sxs-lookup"><span data-stu-id="9272d-144">**Create a custom .NET activity in hello Data Factory pipeline**.</span></span> <span data-ttu-id="9272d-145">hello-aktiviteten är din användarkod som körs på hello Azure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="9272d-145">hello activity is your user code that runs on hello Azure Batch pool.</span></span>
4. <span data-ttu-id="9272d-146">**Lagra stora mängder inkommande data som blobbar i Azure storage**.</span><span class="sxs-lookup"><span data-stu-id="9272d-146">**Store large amounts of input data as blobs in Azure storage**.</span></span> <span data-ttu-id="9272d-147">Data är uppdelat i logiska segment (vanligtvis genom tid).</span><span class="sxs-lookup"><span data-stu-id="9272d-147">Data is divided into logical slices (usually by time).</span></span>
5. <span data-ttu-id="9272d-148">**Data Factory kopierar data som bearbetas parallellt** toohello sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="9272d-148">**Data Factory copies data that is processed in parallel** toohello secondary location.</span></span>
6. <span data-ttu-id="9272d-149">**Data Factory körs hello anpassad aktivitet använder hello allokerats av Batch**.</span><span class="sxs-lookup"><span data-stu-id="9272d-149">**Data Factory runs hello custom activity using hello pool allocated by Batch**.</span></span> <span data-ttu-id="9272d-150">Data Factory kan köra aktiviteter samtidigt.</span><span class="sxs-lookup"><span data-stu-id="9272d-150">Data Factory can run activities concurrently.</span></span> <span data-ttu-id="9272d-151">Varje aktivitet bearbetar ett segment av data.</span><span class="sxs-lookup"><span data-stu-id="9272d-151">Each activity processes a slice of data.</span></span> <span data-ttu-id="9272d-152">hello resultat lagras i Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="9272d-152">hello results are stored in Azure storage.</span></span>
7. <span data-ttu-id="9272d-153">**Data Factory flyttar hello slutresultatet tooa tredje plats**, antingen för distribution via en app eller för ytterligare bearbetning av andra verktyg.</span><span class="sxs-lookup"><span data-stu-id="9272d-153">**Data Factory moves hello final results tooa third location**, either for distribution via an app, or for further processing by other tools.</span></span>

## <a name="implementation-of-sample-solution"></a><span data-ttu-id="9272d-154">Implementering av exempellösning</span><span class="sxs-lookup"><span data-stu-id="9272d-154">Implementation of sample solution</span></span>
<span data-ttu-id="9272d-155">hello exempellösning är avsiktligt enkla och tooshow du hur toouse Data Factory och Batch tillsammans tooprocess datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="9272d-155">hello sample solution is intentionally simple and is tooshow you how toouse Data Factory and Batch together tooprocess datasets.</span></span> <span data-ttu-id="9272d-156">hello lösningen är helt enkelt hello antalet förekomster av en sökterm (”Microsoft”) i indatafiler ordnade i en tidsserie.</span><span class="sxs-lookup"><span data-stu-id="9272d-156">hello solution simply counts hello number of occurrences of a search term (“Microsoft”) in input files organized in a time series.</span></span> <span data-ttu-id="9272d-157">Den matar ut hello antal toooutput filer.</span><span class="sxs-lookup"><span data-stu-id="9272d-157">It outputs hello count toooutput files.</span></span>

<span data-ttu-id="9272d-158">**Tid**: Om du är bekant med grunderna i Azure Data Factory och Batch och ha slutfört hello krav som anges nedan, beräkna den här lösningen tar 1 – 2 timmar toocomplete.</span><span class="sxs-lookup"><span data-stu-id="9272d-158">**Time**: If you are familiar with basics of Azure, Data Factory, and Batch, and have completed hello prerequisites listed below, we estimate this solution takes 1-2 hours toocomplete.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="9272d-159">Krav</span><span class="sxs-lookup"><span data-stu-id="9272d-159">Prerequisites</span></span>
#### <a name="azure-subscription"></a><span data-ttu-id="9272d-160">Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9272d-160">Azure subscription</span></span>
<span data-ttu-id="9272d-161">Om du inte har en Azure-prenumeration kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="9272d-161">If you don't have an Azure subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="9272d-162">Se [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9272d-162">See [Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

#### <a name="azure-storage-account"></a><span data-ttu-id="9272d-163">Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="9272d-163">Azure storage account</span></span>
<span data-ttu-id="9272d-164">Du kan använda ett Azure storage-konto för att lagra hello data i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="9272d-164">You use an Azure storage account for storing hello data in this tutorial.</span></span> <span data-ttu-id="9272d-165">Om du inte har ett Azure storage-konto, se [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="9272d-165">If you don't have an Azure storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="9272d-166">hello exempellösning använder blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="9272d-166">hello sample solution uses blob storage.</span></span>

#### <a name="azure-batch-account"></a><span data-ttu-id="9272d-167">Azure Batch-kontot</span><span class="sxs-lookup"><span data-stu-id="9272d-167">Azure Batch account</span></span>
<span data-ttu-id="9272d-168">Skapa ett Azure Batch-konto med hello [Azure-portalen](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="9272d-168">Create an Azure Batch account using hello [Azure portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="9272d-169">Se [skapa och hantera Azure Batch-kontot](../batch/batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9272d-169">See [Create and manage an Azure Batch account](../batch/batch-account-create-portal.md).</span></span> <span data-ttu-id="9272d-170">Observera hello Azure Batch-kontot namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="9272d-170">Note hello Azure Batch account name and account key.</span></span> <span data-ttu-id="9272d-171">Du kan också använda [ny AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet toocreate Azure Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="9272d-171">You can also use [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet toocreate an Azure Batch account.</span></span> <span data-ttu-id="9272d-172">Se [Kom igång med Azure Batch-PowerShell-cmdlets](../batch/batch-powershell-cmdlets-get-started.md) detaljerade anvisningar om hur du använder denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9272d-172">See [Get started with Azure Batch PowerShell cmdlets](../batch/batch-powershell-cmdlets-get-started.md) for detailed instructions on using this cmdlet.</span></span>

<span data-ttu-id="9272d-173">hello exempellösning använder Azure Batch (indirekt via ett Azure Data Factory-pipelinen) tooprocess data i en parallell sätt i en pool med compute-noder (en hanterad samling med virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="9272d-173">hello sample solution uses Azure Batch (indirectly via an Azure Data Factory pipeline) tooprocess data in a parallel manner on a pool of compute nodes (a managed collection of virtual machines).</span></span>

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a><span data-ttu-id="9272d-174">Azure Batch-pool för virtuella datorer (VM)</span><span class="sxs-lookup"><span data-stu-id="9272d-174">Azure Batch pool of virtual machines (VMs)</span></span>
<span data-ttu-id="9272d-175">Skapa en **Azure Batch-pool** med minst 2 compute-noder.</span><span class="sxs-lookup"><span data-stu-id="9272d-175">Create an **Azure Batch pool** with at least 2 compute nodes.</span></span>

1. <span data-ttu-id="9272d-176">I hello [Azure-portalen](https://portal.azure.com), klickar du på **Bläddra** i hello vänstra menyn och klickar på **Batch-konton**.</span><span class="sxs-lookup"><span data-stu-id="9272d-176">In hello [Azure portal](https://portal.azure.com), click **Browse** in hello left menu, and click **Batch Accounts**.</span></span>
2. <span data-ttu-id="9272d-177">Välj din Azure Batch-kontot tooopen hello **Batch-kontot** bladet.</span><span class="sxs-lookup"><span data-stu-id="9272d-177">Select your Azure Batch account tooopen hello **Batch Account** blade.</span></span>
3. <span data-ttu-id="9272d-178">Klicka på **pooler** panelen.</span><span class="sxs-lookup"><span data-stu-id="9272d-178">Click **Pools** tile.</span></span>
4. <span data-ttu-id="9272d-179">I hello **pooler** bladet, klickar du på knappen Lägg till på hello verktygsfältet tooadd poolen.</span><span class="sxs-lookup"><span data-stu-id="9272d-179">In hello **Pools** blade, click Add button on hello toolbar tooadd a pool.</span></span>
   1. <span data-ttu-id="9272d-180">Ange ett ID för hello pool (**programpoolens ID**).</span><span class="sxs-lookup"><span data-stu-id="9272d-180">Enter an ID for hello pool (**Pool ID**).</span></span> <span data-ttu-id="9272d-181">Obs hello **ID för hello pool**; du behöver den när du skapar hello Data Factory-lösning.</span><span class="sxs-lookup"><span data-stu-id="9272d-181">Note hello **ID of hello pool**; you need it when creating hello Data Factory solution.</span></span>
   2. <span data-ttu-id="9272d-182">Ange **Windows Server 2012 R2** för hello Operativsystemsfamilj inställningen.</span><span class="sxs-lookup"><span data-stu-id="9272d-182">Specify **Windows Server 2012 R2** for hello Operating System Family setting.</span></span>
   3. <span data-ttu-id="9272d-183">Välj en **nodprisnivå**.</span><span class="sxs-lookup"><span data-stu-id="9272d-183">Select a **node pricing tier**.</span></span>
   4. <span data-ttu-id="9272d-184">Ange **2** som värde för hello **mål dedikerade** inställningen.</span><span class="sxs-lookup"><span data-stu-id="9272d-184">Enter **2** as value for hello **Target Dedicated** setting.</span></span>
   5. <span data-ttu-id="9272d-185">Ange **2** som värde för hello **maximalt antal uppgifter per nod** inställningen.</span><span class="sxs-lookup"><span data-stu-id="9272d-185">Enter **2** as value for hello **Max tasks per node** setting.</span></span>
   6. <span data-ttu-id="9272d-186">Klicka på **OK** toocreate hello poolen.</span><span class="sxs-lookup"><span data-stu-id="9272d-186">Click **OK** toocreate hello pool.</span></span>

#### <a name="azure-storage-explorer"></a><span data-ttu-id="9272d-187">Azure Lagringsutforskaren</span><span class="sxs-lookup"><span data-stu-id="9272d-187">Azure Storage Explorer</span></span>
<span data-ttu-id="9272d-188">[Azure Storage Explorer 6 (Verktyg)](https://azurestorageexplorer.codeplex.com/) eller [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (från ClumsyLeaf programvara).</span><span class="sxs-lookup"><span data-stu-id="9272d-188">[Azure Storage Explorer 6 (tool)](https://azurestorageexplorer.codeplex.com/) or [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (from ClumsyLeaf Software).</span></span> <span data-ttu-id="9272d-189">Du kan använda dessa verktyg för att kontrollera och ändra hello data i Azure Storage projekt inklusive hello-loggarna för din molnbaserade program.</span><span class="sxs-lookup"><span data-stu-id="9272d-189">You use these tools for inspecting and altering hello data in your Azure Storage projects including hello logs of your cloud-hosted applications.</span></span>

1. <span data-ttu-id="9272d-190">Skapa en behållare med namnet **minbehållare** med privat åtkomst (ingen anonym åtkomst)</span><span class="sxs-lookup"><span data-stu-id="9272d-190">Create a container named **mycontainer** with private access (no anonymous access)</span></span>
2. <span data-ttu-id="9272d-191">Om du använder **CloudXplorer**skapar mappar och undermappar med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="9272d-191">If you are using **CloudXplorer**, create folders and subfolders with hello following structure:</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   <span data-ttu-id="9272d-192">`Inputfolder`och `outputfolder` är mappar på högsta nivå i `mycontainer`.</span><span class="sxs-lookup"><span data-stu-id="9272d-192">`Inputfolder` and `outputfolder` are top-level folders in `mycontainer`.</span></span> <span data-ttu-id="9272d-193">Hej `inputfolder` har undermappar med datum-/ tidsstämplar (åååå-MM-DD-HH).</span><span class="sxs-lookup"><span data-stu-id="9272d-193">hello `inputfolder` has subfolders with date-time stamps (YYYY-MM-DD-HH).</span></span>

   <span data-ttu-id="9272d-194">Om du använder **Azure Lagringsutforskaren**, i hello nästa steg, behöver du tooupload filer med namn: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` och så vidare.</span><span class="sxs-lookup"><span data-stu-id="9272d-194">If you are using **Azure Storage Explorer**, in hello next step, you need tooupload files with names: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` and so on.</span></span> <span data-ttu-id="9272d-195">Det här steget skapar automatiskt hello mappar.</span><span class="sxs-lookup"><span data-stu-id="9272d-195">This step automatically creates hello folders.</span></span>
3. <span data-ttu-id="9272d-196">Skapa en textfil **fil.txt** på datorn med innehåll som har hello nyckelordet **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="9272d-196">Create a text file **file.txt** on your machine with content that has hello keyword **Microsoft**.</span></span> <span data-ttu-id="9272d-197">Till exempel: ”test anpassad aktivitet Microsoft test anpassad aktivitet Microsoft”.</span><span class="sxs-lookup"><span data-stu-id="9272d-197">For example: “test custom activity Microsoft test custom activity Microsoft”.</span></span>
4. <span data-ttu-id="9272d-198">Överför hello filen toohello efter inkommande mappar i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="9272d-198">Upload hello file toohello following input folders in Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   <span data-ttu-id="9272d-199">Om du använder **Azure Lagringsutforskaren**, ladda upp filen hello **fil.txt** för**minbehållare**.</span><span class="sxs-lookup"><span data-stu-id="9272d-199">If you are using **Azure Storage Explorer**, upload hello file **file.txt** too**mycontainer**.</span></span> <span data-ttu-id="9272d-200">Klicka på **kopiera** på hello verktygsfältet toocreate en kopia av hello-blob.</span><span class="sxs-lookup"><span data-stu-id="9272d-200">Click **Copy** on hello toolbar toocreate a copy of hello blob.</span></span> <span data-ttu-id="9272d-201">I hello **kopiera Blob** dialogrutan, ändra hello **blob målnamn** för`inputfolder/2015-11-16-00/file.txt`.</span><span class="sxs-lookup"><span data-stu-id="9272d-201">In hello **Copy Blob** dialog box, change hello **destination blob name** too`inputfolder/2015-11-16-00/file.txt`.</span></span> <span data-ttu-id="9272d-202">Upprepa det här steget toocreate `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` och så vidare.</span><span class="sxs-lookup"><span data-stu-id="9272d-202">Repeat this step toocreate `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` and so on.</span></span> <span data-ttu-id="9272d-203">Den här åtgärden skapar automatiskt hello mappar.</span><span class="sxs-lookup"><span data-stu-id="9272d-203">This action automatically creates hello folders.</span></span>
5. <span data-ttu-id="9272d-204">Skapa en annan behållare med namnet: `customactivitycontainer`.</span><span class="sxs-lookup"><span data-stu-id="9272d-204">Create another container named: `customactivitycontainer`.</span></span> <span data-ttu-id="9272d-205">Du kan överföra hello anpassad aktivitet zip-filen toothis behållare.</span><span class="sxs-lookup"><span data-stu-id="9272d-205">You upload hello custom activity zip file toothis container.</span></span>

#### <a name="visual-studio"></a><span data-ttu-id="9272d-206">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9272d-206">Visual Studio</span></span>
<span data-ttu-id="9272d-207">Installera Microsoft Visual Studio 2012 eller senare toocreate hello anpassade Batch aktiviteten toobe används i hello Data Factory-lösning.</span><span class="sxs-lookup"><span data-stu-id="9272d-207">Install Microsoft Visual Studio 2012 or later toocreate hello custom Batch activity toobe used in hello Data Factory solution.</span></span>

### <a name="high-level-steps-toocreate-hello-solution"></a><span data-ttu-id="9272d-208">Anvisningar toocreate hello lösning</span><span class="sxs-lookup"><span data-stu-id="9272d-208">High-level steps toocreate hello solution</span></span>
1. <span data-ttu-id="9272d-209">Skapa en anpassad aktivitet som innehåller hello databearbetning logik.</span><span class="sxs-lookup"><span data-stu-id="9272d-209">Create a custom activity that contains hello data processing logic.</span></span>
2. <span data-ttu-id="9272d-210">Skapa ett Azure data factory som använder hello anpassad aktivitet:</span><span class="sxs-lookup"><span data-stu-id="9272d-210">Create an Azure data factory that uses hello custom activity:</span></span>

### <a name="create-hello-custom-activity"></a><span data-ttu-id="9272d-211">Skapa hello anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="9272d-211">Create hello custom activity</span></span>
<span data-ttu-id="9272d-212">hello Data Factory anpassad aktivitet är hello hjärtat i det här exemplet lösning.</span><span class="sxs-lookup"><span data-stu-id="9272d-212">hello Data Factory custom activity is hello heart of this sample solution.</span></span> <span data-ttu-id="9272d-213">hello exempellösningen använder Azure Batch toorun hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9272d-213">hello sample solution uses Azure Batch toorun hello custom activity.</span></span> <span data-ttu-id="9272d-214">Se [använda anpassade aktiviteter i ett Azure Data Factory-pipelinen](data-factory-use-custom-activities.md) för hello grundläggande information toodevelop anpassade aktiviteter och Använd dem i Azure Data Factory rörledningar.</span><span class="sxs-lookup"><span data-stu-id="9272d-214">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) for hello basic information toodevelop custom activities and use them in Azure Data Factory pipelines.</span></span>

<span data-ttu-id="9272d-215">toocreate en anpassad .NET-aktivitet som du kan använda i ett Azure Data Factory-pipelinen, behöver du toocreate en **.NET-klassbibliotek** projekt med en klass som implementerar som **IDotNetActivity** gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="9272d-215">toocreate a .NET custom activity that you can use in an Azure Data Factory pipeline, you need toocreate a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="9272d-216">Det här gränssnittet har endast en metod: **kör**.</span><span class="sxs-lookup"><span data-stu-id="9272d-216">This interface has only one method: **Execute**.</span></span> <span data-ttu-id="9272d-217">Här är hello signaturen för hello-metoden:</span><span class="sxs-lookup"><span data-stu-id="9272d-217">Here is hello signature of hello method:</span></span>

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

<span data-ttu-id="9272d-218">hello-metoden innehåller några viktiga komponenter som du behöver toounderstand.</span><span class="sxs-lookup"><span data-stu-id="9272d-218">hello method has a few key components that you need toounderstand.</span></span>

* <span data-ttu-id="9272d-219">hello-metoden har fyra parametrar:</span><span class="sxs-lookup"><span data-stu-id="9272d-219">hello method takes four parameters:</span></span>

  1. <span data-ttu-id="9272d-220">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="9272d-220">**linkedServices**.</span></span> <span data-ttu-id="9272d-221">En enumerable lista över länkade tjänster som länkar i/o-datakällor (till exempel: Azure Blob Storage) toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="9272d-221">An enumerable list of linked services that link input/output data sources (for example: Azure Blob Storage) toohello data factory.</span></span> <span data-ttu-id="9272d-222">I det här exemplet finns bara en länkad tjänst av typen Azure Storage som används för både inkommande och utgående.</span><span class="sxs-lookup"><span data-stu-id="9272d-222">In this sample, there is only one linked service of type Azure Storage used for both input and output.</span></span>
  2. <span data-ttu-id="9272d-223">**datauppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="9272d-223">**datasets**.</span></span> <span data-ttu-id="9272d-224">Detta är en enumerable lista över datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="9272d-224">This is an enumerable list of datasets.</span></span> <span data-ttu-id="9272d-225">Du kan använda den här parametern tooget hello platser och scheman som definieras av inkommande och utgående datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="9272d-225">You can use this parameter tooget hello locations and schemas defined by input and output datasets.</span></span>
  3. <span data-ttu-id="9272d-226">**aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="9272d-226">**activity**.</span></span> <span data-ttu-id="9272d-227">Den här parametern representerar hello aktuella beräkning enhet – i det här fallet Azure Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9272d-227">This parameter represents hello current compute entity - in this case, an Azure Batch service.</span></span>
  4. <span data-ttu-id="9272d-228">**loggaren**.</span><span class="sxs-lookup"><span data-stu-id="9272d-228">**logger**.</span></span> <span data-ttu-id="9272d-229">hello loggaren kan du skriva debug kommentarer som yta som hello ”användare” i felloggen hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="9272d-229">hello logger lets you write debug comments that surface as hello “User” log for hello pipeline.</span></span>
* <span data-ttu-id="9272d-230">hello-metoden returnerar en ordlista som kan vara tillsammans används toochain anpassade aktiviteter i framtiden hello.</span><span class="sxs-lookup"><span data-stu-id="9272d-230">hello method returns a dictionary that can be used toochain custom activities together in hello future.</span></span> <span data-ttu-id="9272d-231">Den här funktionen har inte implementerats ännu, så returnerar ett tomt Ordbok från hello-metod.</span><span class="sxs-lookup"><span data-stu-id="9272d-231">This feature is not implemented yet, so return an empty dictionary from hello method.</span></span>

#### <a name="procedure-create-hello-custom-activity"></a><span data-ttu-id="9272d-232">Metod: Skapa hello anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="9272d-232">Procedure: Create hello custom activity</span></span>
1. <span data-ttu-id="9272d-233">Skapa ett .NET-klassbibliotek-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9272d-233">Create a .NET Class Library project in Visual Studio.</span></span>

   1. <span data-ttu-id="9272d-234">Starta **Visual Studio 2012**/**2013/2015**.</span><span class="sxs-lookup"><span data-stu-id="9272d-234">Launch **Visual Studio 2012**/**2013/2015**.</span></span>
   2. <span data-ttu-id="9272d-235">Klicka på **filen**, peka för**ny**, och klicka på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="9272d-235">Click **File**, point too**New**, and click **Project**.</span></span>
   3. <span data-ttu-id="9272d-236">Expandera **mallar**, och välj **Visual C\#**.</span><span class="sxs-lookup"><span data-stu-id="9272d-236">Expand **Templates**, and select **Visual C\#**.</span></span> <span data-ttu-id="9272d-237">I den här genomgången ska du använda C\#, men du kan använda alla .NET språk toodevelop hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9272d-237">In this walkthrough, you use C\#, but you can use any .NET language toodevelop hello custom activity.</span></span>
   4. <span data-ttu-id="9272d-238">Välj **klassbiblioteket** hello listan över projekttyper på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="9272d-238">Select **Class Library** from hello list of project types on hello right.</span></span>
   5. <span data-ttu-id="9272d-239">Ange **MyDotNetActivity** för hello **namn**.</span><span class="sxs-lookup"><span data-stu-id="9272d-239">Enter **MyDotNetActivity** for hello **Name**.</span></span>
   6. <span data-ttu-id="9272d-240">Välj **C:\\ADF** för hello **plats**.</span><span class="sxs-lookup"><span data-stu-id="9272d-240">Select **C:\\ADF** for hello **Location**.</span></span> <span data-ttu-id="9272d-241">Skapa hello mapp **ADF** om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="9272d-241">Create hello folder **ADF** if it does not exist.</span></span>
   7. <span data-ttu-id="9272d-242">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="9272d-242">Click **OK** toocreate hello project.</span></span>
2. <span data-ttu-id="9272d-243">Klicka på **verktyg**, peka för**NuGet Package Manager**, och klicka på **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="9272d-243">Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="9272d-244">I hello **Pakethanterarkonsolen**, kör följande kommando tooimport hello **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="9272d-244">In hello **Package Manager Console**, execute hello following command tooimport **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="9272d-245">Importera hello **Azure Storage** NuGet-paketet i toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="9272d-245">Import hello **Azure Storage** NuGet package in toohello project.</span></span> <span data-ttu-id="9272d-246">Du behöver det här paketet eftersom du använder hello Blob storage-API i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="9272d-246">You need this package because you use hello Blob storage API in this sample.</span></span>

    ```powershell
    Install-Package Azure.Storage
    ```
5. <span data-ttu-id="9272d-247">Lägg till följande hello **med** direktiven toohello källfilen i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="9272d-247">Add hello following **using** directives toohello source file in hello project.</span></span>

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. <span data-ttu-id="9272d-248">Ändra hello namnet på hello **namnområde** för**MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="9272d-248">Change hello name of hello **namespace** too**MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="9272d-249">Ändra hello namnet på hello-klass för**MyDotNetActivity** och härledd från hello **IDotNetActivity** gränssnitt som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="9272d-249">Change hello name of hello class too**MyDotNetActivity** and derive it from hello **IDotNetActivity** interface as shown below.</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="9272d-250">Implementera (Lägg till) hello **kör** metod för hello **IDotNetActivity** gränssnitt toohello **MyDotNetActivity** klass och kopiera hello följande exempel kod toohello metod.</span><span class="sxs-lookup"><span data-stu-id="9272d-250">Implement (Add) hello **Execute** method of hello **IDotNetActivity** interface toohello **MyDotNetActivity** class and copy hello following sample code toohello method.</span></span> <span data-ttu-id="9272d-251">Se hello [Kör metod](#execute-method) avsnittet förklaring för hello logik som används i den här metoden.</span><span class="sxs-lookup"><span data-stu-id="9272d-251">See hello [Execute Method](#execute-method) section for explanation for hello logic used in this method.</span></span>

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
    
       // declare types for input and output data stores
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // using First method instead of Single since we are using hello same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // toocreate an input storage client.
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
           // with hello data slice.
           //
           // definition of hello method is shown in hello next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob toohello folder: {0}", folderPath);
    
       // create a storage object for hello output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write hello name of hello file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
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
9. <span data-ttu-id="9272d-252">Lägg till följande hjälpklass metoder toohello hello.</span><span class="sxs-lookup"><span data-stu-id="9272d-252">Add hello following helper methods toohello class.</span></span> <span data-ttu-id="9272d-253">De här metoderna anropas av hello **Execute** metod.</span><span class="sxs-lookup"><span data-stu-id="9272d-253">These methods are invoked by hello **Execute** method.</span></span> <span data-ttu-id="9272d-254">Viktigast av allt hello **Calculate** metoden isolerar hello-kod som går igenom varje blob.</span><span class="sxs-lookup"><span data-stu-id="9272d-254">Most importantly, hello **Calculate** method isolates hello code that iterates through each blob.</span></span>

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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
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
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
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
    <span data-ttu-id="9272d-255">Hej **GetFolderPath** returnerar metoden hello toohello sökväg som hello dataset pekar tooand hello **GetFileName** metoden returnerar hello namnet på hello blob/fil hello dataset pekar på.</span><span class="sxs-lookup"><span data-stu-id="9272d-255">hello **GetFolderPath** method returns hello path toohello folder that hello dataset points tooand hello **GetFileName** method returns hello name of hello blob/file that hello dataset points to.</span></span>

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    <span data-ttu-id="9272d-256">Hej **Calculate** metoden beräknar hello antal instanser av nyckelordet **Microsoft** i hello indatafiler (blobbar i hello mapp).</span><span class="sxs-lookup"><span data-stu-id="9272d-256">hello **Calculate** method calculates hello number of instances of keyword **Microsoft** in hello input files (blobs in hello folder).</span></span> <span data-ttu-id="9272d-257">hello sökordet (”Microsoft”) är hårdkodat i hello kod.</span><span class="sxs-lookup"><span data-stu-id="9272d-257">hello search term (“Microsoft”) is hard-coded in hello code.</span></span>

1. <span data-ttu-id="9272d-258">Kompilera hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="9272d-258">Compile hello project.</span></span> <span data-ttu-id="9272d-259">Klicka på **skapa** från hello-menyn och klicka på **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="9272d-259">Click **Build** from hello menu and click **Build Solution**.</span></span>
2. <span data-ttu-id="9272d-260">Starta **Windows Explorer**, och navigera för**bin\\felsöka** eller **bin\\släpper** beroende på hello typ av version.</span><span class="sxs-lookup"><span data-stu-id="9272d-260">Launch **Windows Explorer**, and navigate too**bin\\debug** or **bin\\release** folder depending on hello type of build.</span></span>
3. <span data-ttu-id="9272d-261">Skapa en zip-fil **MyDotNetActivity.zip** som innehåller alla hello-binärfiler i hello  **\\bin\\felsöka** mapp.</span><span class="sxs-lookup"><span data-stu-id="9272d-261">Create a zip file **MyDotNetActivity.zip** that contains all hello binaries in hello **\\bin\\Debug** folder.</span></span> <span data-ttu-id="9272d-262">Du kanske vill tooinclude hello MyDotNetActivity. **pdb** filen så att du får ytterligare information, till exempel radnumret i hello källkod som orsakade hello problemet när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="9272d-262">You may want tooinclude hello MyDotNetActivity.**pdb** file so that you get additional details such as line number in hello source code that caused hello issue when a failure occurs.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. <span data-ttu-id="9272d-263">Överför **MyDotNetActivity.zip** som en blob toohello blob-behållare: `customactivitycontainer` i hello Azure blob storage som hello **StorageLinkedService** länkade tjänsten i hello  **ADFTutorialDataFactory** använder.</span><span class="sxs-lookup"><span data-stu-id="9272d-263">Upload **MyDotNetActivity.zip** as a blob toohello blob container: `customactivitycontainer` in hello Azure blob storage that hello **StorageLinkedService** linked service in hello **ADFTutorialDataFactory** uses.</span></span> <span data-ttu-id="9272d-264">Skapa hello blob-behållaren `customactivitycontainer` om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="9272d-264">Create hello blob container `customactivitycontainer` if it does not already exist.</span></span>

#### <a name="execute-method"></a><span data-ttu-id="9272d-265">Execute-metoden</span><span class="sxs-lookup"><span data-stu-id="9272d-265">Execute method</span></span>
<span data-ttu-id="9272d-266">Det här avsnittet innehåller mer information och information om hello koden i hello Execute-metoden.</span><span class="sxs-lookup"><span data-stu-id="9272d-266">This section provides more details and notes about hello code in hello Execute method.</span></span>

1. <span data-ttu-id="9272d-267">hello-medlemmar för att söka igenom inkommande hello-samling finns i hello [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) namnområde.</span><span class="sxs-lookup"><span data-stu-id="9272d-267">hello members for iterating through hello input collection are found in hello [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) namespace.</span></span> <span data-ttu-id="9272d-268">Söka igenom hello blob samling kräver att du använder hello **BlobContinuationToken** klass.</span><span class="sxs-lookup"><span data-stu-id="9272d-268">Iterating through hello blob collection requires using hello **BlobContinuationToken** class.</span></span> <span data-ttu-id="9272d-269">I princip måste du använda en gör-när loop med hello token som hello mekanism för att avsluta hello loop.</span><span class="sxs-lookup"><span data-stu-id="9272d-269">In essence, you must use a do-while loop with hello token as hello mechanism for exiting hello loop.</span></span> <span data-ttu-id="9272d-270">Mer information finns i [hur toouse Blob storage från .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="9272d-270">For more information, see [How toouse Blob storage from .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="9272d-271">En grundläggande loop visas här:</span><span class="sxs-lookup"><span data-stu-id="9272d-271">A basic loop is shown here:</span></span>

    ```csharp
    // Initialize hello continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get hello list of input blobs from hello input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   <span data-ttu-id="9272d-272">Hello i dokumentationen för hello [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) metod för information.</span><span class="sxs-lookup"><span data-stu-id="9272d-272">See hello documentation for hello [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) method for details.</span></span>
2. <span data-ttu-id="9272d-273">hello koden fungerar via hello uppsättning blobbar logiskt hamnar inom hello do-while-slinga.</span><span class="sxs-lookup"><span data-stu-id="9272d-273">hello code for working through hello set of blobs logically goes within hello do-while loop.</span></span> <span data-ttu-id="9272d-274">I hello **kör** metod, hello-medan loop skickar hello lista över BLOB tooa metod med namnet **beräkna**.</span><span class="sxs-lookup"><span data-stu-id="9272d-274">In hello **Execute** method, hello do-while loop passes hello list of blobs tooa method named **Calculate**.</span></span> <span data-ttu-id="9272d-275">hello-metoden returnerar en variabel med namnet **utdata** som hello resultatet av att ha hävdade via alla hello BLOB i hello segment.</span><span class="sxs-lookup"><span data-stu-id="9272d-275">hello method returns a string variable named **output** that is hello result of having iterated through all hello blobs in hello segment.</span></span>

   <span data-ttu-id="9272d-276">Den returnerar hello antalet förekomster av hello sökterm (**Microsoft**) i hello blob skickades toohello **Calculate** metod.</span><span class="sxs-lookup"><span data-stu-id="9272d-276">It returns hello number of occurrences of hello search term (**Microsoft**) in hello blob passed toohello **Calculate** method.</span></span>

    ```csharp
    output += string.Format("{0} occurrences of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. <span data-ttu-id="9272d-277">En gång hello **Calculate** metod har gjort hello arbete, den måste vara skriven tooa nya blob.</span><span class="sxs-lookup"><span data-stu-id="9272d-277">Once hello **Calculate** method has done hello work, it must be written tooa new blob.</span></span> <span data-ttu-id="9272d-278">Så att en ny blob kan skrivas med hello resultat för varje uppsättning blobbar som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="9272d-278">So for every set of blobs processed, a new blob can be written with hello results.</span></span> <span data-ttu-id="9272d-279">toowrite tooa ny blob, första hitta hello utdata datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="9272d-279">toowrite tooa new blob, first find hello output dataset.</span></span>

    ```csharp
    // Get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. <span data-ttu-id="9272d-280">hello koden anropar också en hjälpmetod: **GetFolderPath** tooretrieve hello mappsökvägen (hello namnet på lagringsbehållaren).</span><span class="sxs-lookup"><span data-stu-id="9272d-280">hello code also calls a helper method: **GetFolderPath** tooretrieve hello folder path (hello storage container name).</span></span>

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   <span data-ttu-id="9272d-281">Hej **GetFolderPath** webbsändningar hello DataSet-objektet tooan AzureBlobDataSet som har en egenskap med namnet FolderPath.</span><span class="sxs-lookup"><span data-stu-id="9272d-281">hello **GetFolderPath** casts hello DataSet object tooan AzureBlobDataSet, which has a property named FolderPath.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. <span data-ttu-id="9272d-282">hello koden anropar hello **GetFileName** metoden tooretrieve hello filnamn (blob-namn).</span><span class="sxs-lookup"><span data-stu-id="9272d-282">hello code calls hello **GetFileName** method tooretrieve hello file name (blob name).</span></span> <span data-ttu-id="9272d-283">hello koden är liknande toohello ovan kod tooget hello mappsökväg.</span><span class="sxs-lookup"><span data-stu-id="9272d-283">hello code is similar toohello above code tooget hello folder path.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. <span data-ttu-id="9272d-284">hello namnet på hello fil skrivs genom att skapa ett URI-objekt.</span><span class="sxs-lookup"><span data-stu-id="9272d-284">hello name of hello file is written by creating a URI object.</span></span> <span data-ttu-id="9272d-285">hello URI konstruktorn använder hello **BlobEndpoint** egenskapen tooreturn hello behållarens namn.</span><span class="sxs-lookup"><span data-stu-id="9272d-285">hello URI constructor uses hello **BlobEndpoint** property tooreturn hello container name.</span></span> <span data-ttu-id="9272d-286">hello mappen sökvägen och filnamnet läggs tooconstruct hello utdata blob-URI.</span><span class="sxs-lookup"><span data-stu-id="9272d-286">hello folder path and file name are added tooconstruct hello output blob URI.</span></span>  

    ```csharp
    // Write hello name of hello file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. <span data-ttu-id="9272d-287">hello namnet på hello-filen har skrivits och nu kan du skriva hello utdata-strängen från hello **Calculate** metoden tooa nya blob:</span><span class="sxs-lookup"><span data-stu-id="9272d-287">hello name of hello file has been written and now you can write hello output string from hello **Calculate** method tooa new blob:</span></span>

    ```csharp
    // Create a blob and upload hello output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} toohello output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-hello-data-factory"></a><span data-ttu-id="9272d-288">Skapa hello data factory</span><span class="sxs-lookup"><span data-stu-id="9272d-288">Create hello data factory</span></span>
<span data-ttu-id="9272d-289">I hello [skapa hello anpassad aktivitet](#create-the-custom-activity) avsnittet du har skapat en anpassad aktivitet och överförda hello zip-filen med binärfiler och hello PDB-filen tooan Azure blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="9272d-289">In hello [Create hello custom activity](#create-the-custom-activity) section, you created a custom activity and uploaded hello zip file with binaries and hello PDB file tooan Azure blob container.</span></span> <span data-ttu-id="9272d-290">I det här avsnittet skapar du en Azure **datafabriken** med en **pipeline** som använder hello **anpassad aktivitet**.</span><span class="sxs-lookup"><span data-stu-id="9272d-290">In this section, you create an Azure **data factory** with a **pipeline** that uses hello **custom activity**.</span></span>

<span data-ttu-id="9272d-291">Hej inkommande dataset för hello anpassad aktivitet representerar hello BLOB (filer) i hello inkommande mapp (`mycontainer\\inputfolder`) i blob storage.</span><span class="sxs-lookup"><span data-stu-id="9272d-291">hello input dataset for hello custom activity represents hello blobs (files) in hello input folder (`mycontainer\\inputfolder`) in blob storage.</span></span> <span data-ttu-id="9272d-292">Hej utdatauppsättningen för hello aktivitet representerar hello utdata blobbar i hello utdatamapp (`mycontainer\\outputfolder`) i blob storage.</span><span class="sxs-lookup"><span data-stu-id="9272d-292">hello output dataset for hello activity represents hello output blobs in hello output folder (`mycontainer\\outputfolder`) in blob storage.</span></span>

<span data-ttu-id="9272d-293">Ta bort en eller flera filer i hello inkommande mappar:</span><span class="sxs-lookup"><span data-stu-id="9272d-293">Drop one or more files in hello input folders:</span></span>

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

<span data-ttu-id="9272d-294">Till exempel ta bort en fil (fil.txt) med följande innehåll i mappar hello hello.</span><span class="sxs-lookup"><span data-stu-id="9272d-294">For example, drop one file (file.txt) with hello following content into each of hello folders.</span></span>

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="9272d-295">Varje inkommande mapp motsvarar tooa segment i Azure Data Factory även om hello-mappen har 2 eller flera filer.</span><span class="sxs-lookup"><span data-stu-id="9272d-295">Each input folder corresponds tooa slice in Azure Data Factory even if hello folder has 2 or more files.</span></span> <span data-ttu-id="9272d-296">När varje segment bearbetas av hello pipeline går hello anpassad aktivitet igenom alla hello BLOB i hello inkommande mapp för sektorn.</span><span class="sxs-lookup"><span data-stu-id="9272d-296">When each slice is processed by hello pipeline, hello custom activity iterates through all hello blobs in hello input folder for that slice.</span></span>

<span data-ttu-id="9272d-297">Du kan se utdata fem filer med hello samma innehåll.</span><span class="sxs-lookup"><span data-stu-id="9272d-297">You see five output files with hello same content.</span></span> <span data-ttu-id="9272d-298">Hello utdatafilen från att bearbeta hello-filen i mappen hello 2015-11-16-00 har till exempel hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="9272d-298">For example, hello output file from processing hello file in hello 2015-11-16-00 folder has hello following content:</span></span>

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
```

<span data-ttu-id="9272d-299">Om du släpper flera filer (fil.txt, file2.txt, file3.txt) med hello samma innehållsmapp toohello inkommande ser du hello följande innehåll i hello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="9272d-299">If you drop multiple files (file.txt, file2.txt, file3.txt) with hello same content toohello input folder, you see hello following content in hello output file.</span></span> <span data-ttu-id="9272d-300">Varje mapp (2015-11-16-00, etc.) motsvarar tooa sektor i det här exemplet, även om hello-mappen har flera inkommande filer.</span><span class="sxs-lookup"><span data-stu-id="9272d-300">Each folder (2015-11-16-00, etc.) corresponds tooa slice in this sample even though hello folder has multiple input files.</span></span>

```csharp
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file3.txt.
```

<span data-ttu-id="9272d-301">hello utdatafilen har tre rader nu, ett för varje indatafilen (blob) i hello mapp som associeras med hello sektorn (2015-11-16-00).</span><span class="sxs-lookup"><span data-stu-id="9272d-301">hello output file has three lines now, one for each input file (blob) in hello folder associated with hello slice (2015-11-16-00).</span></span>

<span data-ttu-id="9272d-302">En aktivitet skapas för varje aktivitet körs.</span><span class="sxs-lookup"><span data-stu-id="9272d-302">A task is created for each activity run.</span></span> <span data-ttu-id="9272d-303">I det här exemplet finns bara en aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="9272d-303">In this sample, there is only one activity in hello pipeline.</span></span> <span data-ttu-id="9272d-304">När ett segment bearbetas av hello pipeline körs hello anpassad aktivitet på Azure Batch tooprocess hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="9272d-304">When a slice is processed by hello pipeline, hello custom activity runs on Azure Batch tooprocess hello slice.</span></span> <span data-ttu-id="9272d-305">Eftersom det finns fem segment (varje segment kan ha flera blobbar eller -fil), det finns fem aktiviteter som skapats i Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="9272d-305">Since there are five slices (each slice can have multiple blobs or file), there are five tasks created in Azure Batch.</span></span> <span data-ttu-id="9272d-306">När en aktivitet körs på Batch är faktiskt anpassad aktivitet för hello som körs.</span><span class="sxs-lookup"><span data-stu-id="9272d-306">When a task runs on Batch, it is actually hello custom activity that is running.</span></span>

<span data-ttu-id="9272d-307">hello följande genomgång innehåller ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="9272d-307">hello following walkthrough provides additional details.</span></span>

#### <a name="step-1-create-hello-data-factory"></a><span data-ttu-id="9272d-308">Steg 1: Skapa hello data factory</span><span class="sxs-lookup"><span data-stu-id="9272d-308">Step 1: Create hello data factory</span></span>
1. <span data-ttu-id="9272d-309">När du loggar in toohello [Azure-portalen](https://portal.azure.com/), hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9272d-309">After logging in toohello [Azure portal](https://portal.azure.com/), do hello following steps:</span></span>

   1. <span data-ttu-id="9272d-310">Klicka på **ny** på hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="9272d-310">Click **NEW** on hello left menu.</span></span>
   2. <span data-ttu-id="9272d-311">Klicka på **Data + analys** i hello **ny** bladet.</span><span class="sxs-lookup"><span data-stu-id="9272d-311">Click **Data + Analytics** in hello **New** blade.</span></span>
   3. <span data-ttu-id="9272d-312">Klicka på **Datafabriken** på hello **dataanalys** bladet.</span><span class="sxs-lookup"><span data-stu-id="9272d-312">Click **Data Factory** on hello **Data analytics** blade.</span></span>
2. <span data-ttu-id="9272d-313">I hello **nya datafabriken** bladet ange **CustomActivityFactory** för hello namn.</span><span class="sxs-lookup"><span data-stu-id="9272d-313">In hello **New data factory** blade, enter **CustomActivityFactory** for hello Name.</span></span> <span data-ttu-id="9272d-314">hello namn i hello Azure data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="9272d-314">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="9272d-315">Om felmeddelandet hello: **datafabriksnamnet ”CustomActivityFactory” är inte tillgänglig**, ändra hello namn i hello data factory (till exempel **yournameCustomActivityFactory**) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="9272d-315">If you receive hello error: **Data factory name “CustomActivityFactory” is not available**, change hello name of hello data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>
3. <span data-ttu-id="9272d-316">Klicka på **RESURSGRUPPENS namn**, och välj en befintlig resursgrupp eller skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="9272d-316">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="9272d-317">Kontrollera att du använder rätt hello-prenumeration och region där du vill skapa hello data factory toobe.</span><span class="sxs-lookup"><span data-stu-id="9272d-317">Verify that you are using hello correct subscription and region where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="9272d-318">Klicka på **skapa** på hello **nya data factory** bladet.</span><span class="sxs-lookup"><span data-stu-id="9272d-318">Click **Create** on hello **New data factory** blade.</span></span>
6. <span data-ttu-id="9272d-319">Du ser hello data factory som skapas i hello **instrumentpanelen** av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9272d-319">You see hello data factory being created in hello **Dashboard** of hello Azure portal.</span></span>
7. <span data-ttu-id="9272d-320">När hello datafabriken har skapats, visas hello data factory sidan som visar hello innehållet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="9272d-320">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a><span data-ttu-id="9272d-321">Steg 2: Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="9272d-321">Step 2: Create linked services</span></span>
<span data-ttu-id="9272d-322">Länkade tjänster länka datalager eller compute services tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="9272d-322">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="9272d-323">I det här steget kan du länka din **Azure Storage** konto och **Azure Batch** konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="9272d-323">In this step, you link your **Azure Storage** account and **Azure Batch** account tooyour data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="9272d-324">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="9272d-324">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="9272d-325">Klicka på hello **författare och distribuera** panelen på hello **DATAFABRIKEN** bladet för **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="9272d-325">Click hello **Author and deploy** tile on hello **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="9272d-326">Du kan se hello Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="9272d-326">You see hello Data Factory Editor.</span></span>
2. <span data-ttu-id="9272d-327">Klicka på **Nytt datalager** hello kommandofältet och välj **Azure storage.**</span><span class="sxs-lookup"><span data-stu-id="9272d-327">Click **New data store** on hello command bar and choose **Azure storage.**</span></span> <span data-ttu-id="9272d-328">Du bör se hello JSON-skript för att skapa ett Azure Storage länkade tjänsten i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="9272d-328">You should see hello JSON script for creating an Azure Storage linked service in hello editor.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. <span data-ttu-id="9272d-329">Ersätt **kontonamn** med hello namnet på ditt Azure storage-konto och **kontonyckel** med hello snabbtangent av hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9272d-329">Replace **account name** with hello name of your Azure storage account and **account key** with hello access key of hello Azure storage account.</span></span> <span data-ttu-id="9272d-330">toolearn hur tooget lagringen åt nyckeln finns [visa, kopiera och generera lagring åtkomstnycklar](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="9272d-330">toolearn how tooget your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

4. <span data-ttu-id="9272d-331">Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9272d-331">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="9272d-332">Skapa Azure Batch länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="9272d-332">Create Azure Batch linked service</span></span>
<span data-ttu-id="9272d-333">I det här steget skapar du en länkad tjänst för din **Azure Batch** konto som används toorun hello Data Factory anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9272d-333">In this step, you create a linked service for your **Azure Batch** account that is used toorun hello Data Factory custom activity.</span></span>

1. <span data-ttu-id="9272d-334">Klicka på **nya beräkning** hello kommandofältet och välj **Azure Batch.**</span><span class="sxs-lookup"><span data-stu-id="9272d-334">Click **New compute** on hello command bar and choose **Azure Batch.**</span></span> <span data-ttu-id="9272d-335">Du bör se hello JSON-skript för att skapa ett Azure Batch länkade tjänsten i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="9272d-335">You should see hello JSON script for creating an Azure Batch linked service in hello editor.</span></span>
2. <span data-ttu-id="9272d-336">I hello JSON-skript:</span><span class="sxs-lookup"><span data-stu-id="9272d-336">In hello JSON script:</span></span>

   1. <span data-ttu-id="9272d-337">Ersätt **kontonamn** med hello namnet på Azure Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="9272d-337">Replace **account name** with hello name of your Azure Batch account.</span></span>
   2. <span data-ttu-id="9272d-338">Ersätt **åtkomstnyckeln** med hello snabbtangent av hello Azure Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="9272d-338">Replace **access key** with hello access key of hello Azure Batch account.</span></span>
   3. <span data-ttu-id="9272d-339">Ange hello-ID för hello pool för hello **poolName** egenskapen**.**</span><span class="sxs-lookup"><span data-stu-id="9272d-339">Enter hello ID of hello pool for hello **poolName** property**.**</span></span> <span data-ttu-id="9272d-340">För den här egenskapen kan du ange antingen namnet på programpoolen eller poolens ID.</span><span class="sxs-lookup"><span data-stu-id="9272d-340">For this property, you can specify either pool name or pool ID.</span></span>
   4. <span data-ttu-id="9272d-341">Ange hello batch URI för hello **batchUri** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="9272d-341">Enter hello batch URI for hello **batchUri** JSON property.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="9272d-342">Hej **URL** från hello **Azure Batch-kontoblad** i hello följande format: \<accountname\>.\< region\>. batch.azure.com. För hello **batchUri** egenskap i hello JSON du behöver för**ta bort ”accountname”.**</span><span class="sxs-lookup"><span data-stu-id="9272d-342">hello **URL** from hello **Azure Batch account blade** is in hello following format: \<accountname\>.\<region\>.batch.azure.com. For hello **batchUri** property in hello JSON, you need too**remove "accountname."**</span></span> <span data-ttu-id="9272d-343">från hello-URL.</span><span class="sxs-lookup"><span data-stu-id="9272d-343">from hello URL.</span></span> <span data-ttu-id="9272d-344">Exempel: `"batchUri": "https://eastus.batch.azure.com"`.</span><span class="sxs-lookup"><span data-stu-id="9272d-344">Example: `"batchUri": "https://eastus.batch.azure.com"`.</span></span>
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      <span data-ttu-id="9272d-345">För hello **poolName** egenskap, du kan också ange hello-ID för hello poolen i stället för hello namnet på hello-adresspoolen.</span><span class="sxs-lookup"><span data-stu-id="9272d-345">For hello **poolName** property, you can also specify hello ID of hello pool instead of hello name of hello pool.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9272d-346">hello Data Factory-tjänsten stöder inte ett alternativ på begäran för Azure Batch som för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="9272d-346">hello Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="9272d-347">Du kan bara använda din egen Azure Batch-pool i ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="9272d-347">You can only use your own Azure Batch pool in an Azure data factory.</span></span>
      >
      >
   5. <span data-ttu-id="9272d-348">Ange **StorageLinkedService** för hello **linkedServiceName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="9272d-348">Specify **StorageLinkedService** for hello **linkedServiceName** property.</span></span> <span data-ttu-id="9272d-349">Du har skapat den här länkade tjänsten i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="9272d-349">You created this linked service in hello previous step.</span></span> <span data-ttu-id="9272d-350">Den här används som ett mellanlagringsområde för filer och loggar.</span><span class="sxs-lookup"><span data-stu-id="9272d-350">This storage is used as a staging area for files and logs.</span></span>
3. <span data-ttu-id="9272d-351">Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9272d-351">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

#### <a name="step-3-create-datasets"></a><span data-ttu-id="9272d-352">Steg 3: Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="9272d-352">Step 3: Create datasets</span></span>
<span data-ttu-id="9272d-353">I det här steget Skapa datauppsättningar toorepresent indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="9272d-353">In this step, you create datasets toorepresent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="9272d-354">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="9272d-354">Create input dataset</span></span>
1. <span data-ttu-id="9272d-355">I hello **Editor** hello Data Factory, klickar du på **ny datamängd** hello verktygsfält och klicka på knappen **Azure Blob storage** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="9272d-355">In hello **Editor** for hello Data Factory, click **New dataset** button on hello toolbar and click **Azure Blob storage** from hello drop-down menu.</span></span>
2. <span data-ttu-id="9272d-356">Ersätt hello JSON i hello högra med hello följande JSON-fragment:</span><span class="sxs-lookup"><span data-stu-id="9272d-356">Replace hello JSON in hello right pane with hello following JSON snippet:</span></span>

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
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

    <span data-ttu-id="9272d-357">Du skapar en pipeline senare i den här genomgången med starttid: 2015-11-16T00:00:00Z-och Sluttid: 2015-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="9272d-357">You create a pipeline later in this walkthrough with start time: 2015-11-16T00:00:00Z and end time: 2015-11-16T05:00:00Z.</span></span> <span data-ttu-id="9272d-358">Det är schemalagda tooproduce data **varje timme**, så att det finns 5 i/o-segment (mellan **00**: 00:00 -\> **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="9272d-358">It is scheduled tooproduce data **hourly**, so there are 5 input/output slices (between **00**:00:00 -\> **05**:00:00).</span></span>

    <span data-ttu-id="9272d-359">Hej **frekvens** och **intervall** hello inkommande dataset är inställd för**timme** och **1**, vilket innebär att hello indata sektorn är tillgängliga varje timme.</span><span class="sxs-lookup"><span data-stu-id="9272d-359">hello **frequency** and **interval** for hello input dataset is set too**Hour** and **1**, which means that hello input slice is available hourly.</span></span>

    <span data-ttu-id="9272d-360">Här följer hello starttider för varje segment som representeras av **SliceStart** systemvariabel i hello ovan JSON-fragment.</span><span class="sxs-lookup"><span data-stu-id="9272d-360">Here are hello start times for each slice, which is represented by **SliceStart** system variable in hello above JSON snippet.</span></span>

    | <span data-ttu-id="9272d-361">**Sektorn**</span><span class="sxs-lookup"><span data-stu-id="9272d-361">**Slice**</span></span> | <span data-ttu-id="9272d-362">**Starttid**</span><span class="sxs-lookup"><span data-stu-id="9272d-362">**Start time**</span></span>          |
    |-----------|-------------------------|
    | <span data-ttu-id="9272d-363">1</span><span class="sxs-lookup"><span data-stu-id="9272d-363">1</span></span>         | <span data-ttu-id="9272d-364">2015-11-16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-364">2015-11-16T**00**:00:00</span></span> |
    | <span data-ttu-id="9272d-365">2</span><span class="sxs-lookup"><span data-stu-id="9272d-365">2</span></span>         | <span data-ttu-id="9272d-366">2015-11-16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-366">2015-11-16T**01**:00:00</span></span> |
    | <span data-ttu-id="9272d-367">3</span><span class="sxs-lookup"><span data-stu-id="9272d-367">3</span></span>         | <span data-ttu-id="9272d-368">2015-11-16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-368">2015-11-16T**02**:00:00</span></span> |
    | <span data-ttu-id="9272d-369">4</span><span class="sxs-lookup"><span data-stu-id="9272d-369">4</span></span>         | <span data-ttu-id="9272d-370">2015-11-16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-370">2015-11-16T**03**:00:00</span></span> |
    | <span data-ttu-id="9272d-371">5</span><span class="sxs-lookup"><span data-stu-id="9272d-371">5</span></span>         | <span data-ttu-id="9272d-372">2015-11-16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-372">2015-11-16T**04**:00:00</span></span> |

    <span data-ttu-id="9272d-373">Hej **folderPath** beräknas med hjälp av hello år, månad, dag och timme tillhör hello tidsintervallet för start (**SliceStart**).</span><span class="sxs-lookup"><span data-stu-id="9272d-373">hello **folderPath** is calculated by using hello year, month, day, and hour part of hello slice start time (**SliceStart**).</span></span> <span data-ttu-id="9272d-374">Här är därför hur en inkommande mappen är mappade tooa sektorn.</span><span class="sxs-lookup"><span data-stu-id="9272d-374">Therefore, here is how an input folder is mapped tooa slice.</span></span>

    | <span data-ttu-id="9272d-375">**Sektorn**</span><span class="sxs-lookup"><span data-stu-id="9272d-375">**Slice**</span></span> | <span data-ttu-id="9272d-376">**Starttid**</span><span class="sxs-lookup"><span data-stu-id="9272d-376">**Start time**</span></span>          | <span data-ttu-id="9272d-377">**Inkommande mapp**</span><span class="sxs-lookup"><span data-stu-id="9272d-377">**Input folder**</span></span>  |
    |-----------|-------------------------|-------------------|
    | <span data-ttu-id="9272d-378">1</span><span class="sxs-lookup"><span data-stu-id="9272d-378">1</span></span>         | <span data-ttu-id="9272d-379">2015-11-16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-379">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="9272d-380">2015-11-16-**00**</span><span class="sxs-lookup"><span data-stu-id="9272d-380">2015-11-16-**00**</span></span> |
    | <span data-ttu-id="9272d-381">2</span><span class="sxs-lookup"><span data-stu-id="9272d-381">2</span></span>         | <span data-ttu-id="9272d-382">2015-11-16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-382">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="9272d-383">2015-11-16-**01**</span><span class="sxs-lookup"><span data-stu-id="9272d-383">2015-11-16-**01**</span></span> |
    | <span data-ttu-id="9272d-384">3</span><span class="sxs-lookup"><span data-stu-id="9272d-384">3</span></span>         | <span data-ttu-id="9272d-385">2015-11-16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-385">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="9272d-386">2015-11-16-**02**</span><span class="sxs-lookup"><span data-stu-id="9272d-386">2015-11-16-**02**</span></span> |
    | <span data-ttu-id="9272d-387">4</span><span class="sxs-lookup"><span data-stu-id="9272d-387">4</span></span>         | <span data-ttu-id="9272d-388">2015-11-16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-388">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="9272d-389">2015-11-16-**03**</span><span class="sxs-lookup"><span data-stu-id="9272d-389">2015-11-16-**03**</span></span> |
    | <span data-ttu-id="9272d-390">5</span><span class="sxs-lookup"><span data-stu-id="9272d-390">5</span></span>         | <span data-ttu-id="9272d-391">2015-11-16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-391">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="9272d-392">2015-11-16-**04**</span><span class="sxs-lookup"><span data-stu-id="9272d-392">2015-11-16-**04**</span></span> |

1. <span data-ttu-id="9272d-393">Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **InputDataset** tabell.</span><span class="sxs-lookup"><span data-stu-id="9272d-393">Click **Deploy** on hello toolbar toocreate and deploy hello **InputDataset** table.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="9272d-394">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="9272d-394">Create output dataset</span></span>
<span data-ttu-id="9272d-395">I det här steget skapar du en annan dataset av typen AzureBlob toorepresent hello-utdata.</span><span class="sxs-lookup"><span data-stu-id="9272d-395">In this step, you create another dataset of type AzureBlob toorepresent hello output data.</span></span>

1. <span data-ttu-id="9272d-396">I hello **Editor** hello Data Factory, klickar du på **ny datamängd** hello verktygsfält och klicka på knappen **Azure Blob storage** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="9272d-396">In hello **Editor** for hello Data Factory, click **New dataset** button on hello toolbar and click **Azure Blob storage** from hello drop-down menu.</span></span>
2. <span data-ttu-id="9272d-397">Ersätt hello JSON i hello högra med hello följande JSON-fragment:</span><span class="sxs-lookup"><span data-stu-id="9272d-397">Replace hello JSON in hello right pane with hello following JSON snippet:</span></span>

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
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

    <span data-ttu-id="9272d-398">En blob/utdatafil genereras för varje inkommande segment.</span><span class="sxs-lookup"><span data-stu-id="9272d-398">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="9272d-399">Här är hur en utdatafil heter för varje segment.</span><span class="sxs-lookup"><span data-stu-id="9272d-399">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="9272d-400">Alla hello utdatafiler skapas i en utdatamapp: `mycontainer\\outputfolder`.</span><span class="sxs-lookup"><span data-stu-id="9272d-400">All hello output files are generated in one output folder: `mycontainer\\outputfolder`.</span></span>

    | <span data-ttu-id="9272d-401">**Sektorn**</span><span class="sxs-lookup"><span data-stu-id="9272d-401">**Slice**</span></span> | <span data-ttu-id="9272d-402">**Starttid**</span><span class="sxs-lookup"><span data-stu-id="9272d-402">**Start time**</span></span>          | <span data-ttu-id="9272d-403">**Utdatafil**</span><span class="sxs-lookup"><span data-stu-id="9272d-403">**Output file**</span></span>       |
    |-----------|-------------------------|-----------------------|
    | <span data-ttu-id="9272d-404">1</span><span class="sxs-lookup"><span data-stu-id="9272d-404">1</span></span>         | <span data-ttu-id="9272d-405">2015-11-16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-405">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="9272d-406">2015-11-16 -**00. txt**</span><span class="sxs-lookup"><span data-stu-id="9272d-406">2015-11-16-**00.txt**</span></span> |
    | <span data-ttu-id="9272d-407">2</span><span class="sxs-lookup"><span data-stu-id="9272d-407">2</span></span>         | <span data-ttu-id="9272d-408">2015-11-16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-408">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="9272d-409">2015-11-16 -**01. txt**</span><span class="sxs-lookup"><span data-stu-id="9272d-409">2015-11-16-**01.txt**</span></span> |
    | <span data-ttu-id="9272d-410">3</span><span class="sxs-lookup"><span data-stu-id="9272d-410">3</span></span>         | <span data-ttu-id="9272d-411">2015-11-16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-411">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="9272d-412">2015-11-16 -**02. txt**</span><span class="sxs-lookup"><span data-stu-id="9272d-412">2015-11-16-**02.txt**</span></span> |
    | <span data-ttu-id="9272d-413">4</span><span class="sxs-lookup"><span data-stu-id="9272d-413">4</span></span>         | <span data-ttu-id="9272d-414">2015-11-16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-414">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="9272d-415">2015-11-16 -**03. txt**</span><span class="sxs-lookup"><span data-stu-id="9272d-415">2015-11-16-**03.txt**</span></span> |
    | <span data-ttu-id="9272d-416">5</span><span class="sxs-lookup"><span data-stu-id="9272d-416">5</span></span>         | <span data-ttu-id="9272d-417">2015-11-16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="9272d-417">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="9272d-418">2015-11-16 -**04. txt**</span><span class="sxs-lookup"><span data-stu-id="9272d-418">2015-11-16-**04.txt**</span></span> |

    <span data-ttu-id="9272d-419">Kom ihåg att alla hello filer i en mapp för inkommande (till exempel: 2015-11-16-00) är en del av ett segment med hello starttiden: 2015-11-16-00.</span><span class="sxs-lookup"><span data-stu-id="9272d-419">Remember that all hello files in an input folder (for example: 2015-11-16-00) are part of a slice with hello start time: 2015-11-16-00.</span></span> <span data-ttu-id="9272d-420">När den här sektorn behandlas hello anpassad aktivitet söker igenom varje fil och ger en rad i hello utdatafil med hello antal förekomster av sökterm (”Microsoft”).</span><span class="sxs-lookup"><span data-stu-id="9272d-420">When this slice is processed, hello custom activity scans through each file and produces a line in hello output file with hello number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="9272d-421">Om det finns tre filer i mappen hello 2015-11-16-00, det finns tre rader i hello utdatafilen: 2015-11-16-00.txt.</span><span class="sxs-lookup"><span data-stu-id="9272d-421">If there are three files in hello folder 2015-11-16-00, there are three lines in hello output file: 2015-11-16-00.txt.</span></span>

1. <span data-ttu-id="9272d-422">Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="9272d-422">Click **Deploy** on hello toolbar toocreate and deploy hello **OutputDataset**.</span></span>

#### <a name="step-4-create-and-run-hello-pipeline-with-custom-activity"></a><span data-ttu-id="9272d-423">Steg 4: Skapa och köra hello pipeline med anpassad aktivitet</span><span class="sxs-lookup"><span data-stu-id="9272d-423">Step 4: Create and run hello pipeline with custom activity</span></span>
<span data-ttu-id="9272d-424">I det här steget skapar du en pipeline med en aktivitet, hello anpassad aktivitet som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9272d-424">In this step, you create a pipeline with one activity, hello custom activity you created earlier.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9272d-425">Om du inte har överfört hello **fil.txt** tooinput mappar i hello blob-behållaren göra det innan du skapar hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="9272d-425">If you haven't uploaded hello **file.txt** tooinput folders in hello blob container, do so before creating hello pipeline.</span></span> <span data-ttu-id="9272d-426">Hej **isPaused** egenskapen toofalse i pipeline-hello JSON, så hello pipeline körs omedelbart som hello **starta** datumet infaller under hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="9272d-426">hello **isPaused** property is set toofalse in hello pipeline JSON, so hello pipeline runs immediately as hello **start** date is in hello past.</span></span>
>
>

1. <span data-ttu-id="9272d-427">I hello Data Factory-redigeraren, klickar du på **ny pipeline** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="9272d-427">In hello Data Factory Editor, click **New pipeline** on hello command bar.</span></span> <span data-ttu-id="9272d-428">Om du inte ser hello kommando klickar du på **... (Tre punkter)**  toosee den.</span><span class="sxs-lookup"><span data-stu-id="9272d-428">If you do not see hello command, click **... (Ellipsis)** toosee it.</span></span>
2. <span data-ttu-id="9272d-429">Ersätt hello JSON i hello högra med hello följande JSON-skript:</span><span class="sxs-lookup"><span data-stu-id="9272d-429">Replace hello JSON in hello right pane with hello following JSON script:</span></span>

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   <span data-ttu-id="9272d-430">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="9272d-430">Note hello following points:</span></span>

   * <span data-ttu-id="9272d-431">Det finns bara en aktivitet i hello pipeline och som är av typen: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="9272d-431">There is only one activity in hello pipeline and that is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="9272d-432">**AssemblyName** anges toohello namnet på hello DLL: **MyDotNetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="9272d-432">**AssemblyName** is set toohello name of hello DLL: **MyDotNetActivity.dll**.</span></span>
   * <span data-ttu-id="9272d-433">**EntryPoint** har angetts för**MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="9272d-433">**EntryPoint** is set too**MyDotNetActivityNS.MyDotNetActivity**.</span></span> <span data-ttu-id="9272d-434">Det är i princip \<namnområde\>.\< ClassName\> i koden.</span><span class="sxs-lookup"><span data-stu-id="9272d-434">It is basically \<namespace\>.\<classname\> in your code.</span></span>
   * <span data-ttu-id="9272d-435">**PackageLinkedService** har angetts för**StorageLinkedService** som pekar toohello blob-lagring som innehåller hello anpassad aktivitet zip-filen.</span><span class="sxs-lookup"><span data-stu-id="9272d-435">**PackageLinkedService** is set too**StorageLinkedService** that points toohello blob storage that contains hello custom activity zip file.</span></span> <span data-ttu-id="9272d-436">Om du använder olika Azure Storage-konton för i/o-filer och hello anpassad aktivitet zip-filen, har du toocreate en annan Azure Storage länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9272d-436">If you are using different Azure Storage accounts for input/output files and hello custom activity zip file, you have toocreate another Azure Storage linked service.</span></span> <span data-ttu-id="9272d-437">Den här artikeln förutsätter att du använder hello samma Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9272d-437">This article assumes that you are using hello same Azure Storage account.</span></span>
   * <span data-ttu-id="9272d-438">**PackageFile** har angetts för**customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="9272d-438">**PackageFile** is set too**customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="9272d-439">Den är i hello format: \<containerforthezip\>/\<nameofthezip.zip\>.</span><span class="sxs-lookup"><span data-stu-id="9272d-439">It is in hello format: \<containerforthezip\>/\<nameofthezip.zip\>.</span></span>
   * <span data-ttu-id="9272d-440">hello anpassad aktivitet tar **InputDataset** som indata och **OutputDataset** som utdata.</span><span class="sxs-lookup"><span data-stu-id="9272d-440">hello custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="9272d-441">Hej **linkedServiceName** -egenskapen för hello anpassad aktivitet pekar toohello **AzureBatchLinkedService**, som talar om Azure Data Factory som hello anpassade aktiviteten måste toorun på Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="9272d-441">hello **linkedServiceName** property of hello custom activity points toohello **AzureBatchLinkedService**, which tells Azure Data Factory that hello custom activity needs toorun on Azure Batch.</span></span>
   * <span data-ttu-id="9272d-442">Hej **samtidighet** inställning är viktig.</span><span class="sxs-lookup"><span data-stu-id="9272d-442">hello **concurrency** setting is important.</span></span> <span data-ttu-id="9272d-443">Om du använder hello standardvärdet, vilket är 1, även om du har 2 eller mer compute-noder i hello Azure Batch-pool, hello segment bearbetas efter varandra.</span><span class="sxs-lookup"><span data-stu-id="9272d-443">If you use hello default value, which is 1, even if you have 2 or more compute nodes in hello Azure Batch pool, hello slices are processed one after another.</span></span> <span data-ttu-id="9272d-444">Du därför inte att utnyttja mätfunktioner på hello parallell bearbetning av Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="9272d-444">Therefore, you are not taking advantage of hello parallel processing capability of Azure Batch.</span></span> <span data-ttu-id="9272d-445">Om du ställer in **samtidighet** tooa högre värde, säg 2 innebär det att två sektorer (motsvarar tootwo aktiviteter i Azure Batch) kan bearbetas hos hello samma tid, i vilket fall båda hello virtuella datorer i hello Azure Batch-pool används.</span><span class="sxs-lookup"><span data-stu-id="9272d-445">If you set **concurrency** tooa higher value, say 2, it means that two slices (corresponds tootwo tasks in Azure Batch) can be processed at hello same time, in which case, both hello VMs in hello Azure Batch pool are utilized.</span></span> <span data-ttu-id="9272d-446">Därför egenskapen hello samtidighet på lämpligt sätt.</span><span class="sxs-lookup"><span data-stu-id="9272d-446">Therefore, set hello concurrency property appropriately.</span></span>
   * <span data-ttu-id="9272d-447">Endast en aktivitet (segment) körs på en virtuell dator när som helst som standard.</span><span class="sxs-lookup"><span data-stu-id="9272d-447">Only one task (slice) is executed on a VM at any point by default.</span></span> <span data-ttu-id="9272d-448">hello orsak är att, som standard hello **maximalt uppgifter per VM** anges too1 för en Azure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="9272d-448">hello reason is that, by default, hello **Maximum tasks per VM** is set too1 for an Azure Batch pool.</span></span> <span data-ttu-id="9272d-449">Som en del av krav, du har skapat en pool med den här egenskapen set too2 så två Data Factory-segment kan köras på en virtuell dator på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="9272d-449">As part of prerequisites, you created a pool with this property set too2, so two Data Factory slices can be running on a VM at hello same time.</span></span>

    -   <span data-ttu-id="9272d-450">**isPaused** egenskapen toofalse som standard.</span><span class="sxs-lookup"><span data-stu-id="9272d-450">**isPaused** property is set toofalse by default.</span></span> <span data-ttu-id="9272d-451">hello pipelinen körs direkt i det här exemplet eftersom hello segment starta i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="9272d-451">hello pipeline runs immediately in this example because hello slices start in hello past.</span></span> <span data-ttu-id="9272d-452">Du kan ange den här egenskapen tootrue toopause hello pipeline och ange den bakre toofalse toorestart.</span><span class="sxs-lookup"><span data-stu-id="9272d-452">You can set this property tootrue toopause hello pipeline and set it back toofalse toorestart.</span></span>

    -   <span data-ttu-id="9272d-453">Hej **starta** tid och **end** segment produceras varje timma, så att fem segment som produceras av hello pipeline och tider är fem timmar från varandra.</span><span class="sxs-lookup"><span data-stu-id="9272d-453">hello **start** time and **end** times are five hours apart and slices are produced hourly, so five slices are produced by hello pipeline.</span></span>

1. <span data-ttu-id="9272d-454">Klicka på **distribuera** på hello i kommandofältet toodeploy hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="9272d-454">Click **Deploy** on hello command bar toodeploy hello pipeline.</span></span>

#### <a name="step-5-test-hello-pipeline"></a><span data-ttu-id="9272d-455">Steg 5: Testa hello pipeline</span><span class="sxs-lookup"><span data-stu-id="9272d-455">Step 5: Test hello pipeline</span></span>
<span data-ttu-id="9272d-456">I det här steget kan testa du hello pipeline genom att släppa filer till hello inkommande mappar.</span><span class="sxs-lookup"><span data-stu-id="9272d-456">In this step, you test hello pipeline by dropping files into hello input folders.</span></span> <span data-ttu-id="9272d-457">Vi börjar med tester hello pipeline med en fil per inkommande mappar.</span><span class="sxs-lookup"><span data-stu-id="9272d-457">Let’s start with testing hello pipeline with one file per one input folder.</span></span>

1. <span data-ttu-id="9272d-458">I hello Data Factory-bladet i hello Azure-portalen klickar du på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="9272d-458">In hello Data Factory blade in hello Azure portal, click **Diagram**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. <span data-ttu-id="9272d-459">Dubbelklicka på inkommande dataset i hello diagramvyn: **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="9272d-459">In hello diagram view, double-click input dataset: **InputDataset**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. <span data-ttu-id="9272d-460">Du bör se hello **InputDataset** bladet med alla fem sektorer redo.</span><span class="sxs-lookup"><span data-stu-id="9272d-460">You should see hello **InputDataset** blade with all five slices ready.</span></span> <span data-ttu-id="9272d-461">Meddelande hello **sektorn starttid** och **sektorn SLUTTID** för varje segment.</span><span class="sxs-lookup"><span data-stu-id="9272d-461">Notice hello **SLICE START TIME** and **SLICE END TIME** for each slice.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. <span data-ttu-id="9272d-462">I hello **diagramvyn**, klicka på **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="9272d-462">In hello **Diagram View**, now click **OutputDataset**.</span></span>
5. <span data-ttu-id="9272d-463">Du bör se att hello fem utdata segment är i tillståndet för hello Ready om de redan har skapats.</span><span class="sxs-lookup"><span data-stu-id="9272d-463">You should see that hello five output slices are in hello Ready state if they have already been produced.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. <span data-ttu-id="9272d-464">Använd Azure portal tooview hello **uppgifter** som är associerade med hello **segment** och se vilka VM varje segment som kördes på.</span><span class="sxs-lookup"><span data-stu-id="9272d-464">Use Azure portal tooview hello **tasks** associated with hello **slices** and see what VM each slice ran on.</span></span> <span data-ttu-id="9272d-465">Se [Data Factory och Batch-integrering](#data-factory-and-batch-integration) information.</span><span class="sxs-lookup"><span data-stu-id="9272d-465">See [Data Factory and Batch integration](#data-factory-and-batch-integration) section for details.</span></span>
7. <span data-ttu-id="9272d-466">Du bör se hello utdatafilerna i hello `outputfolder` av `mycontainer` i din Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="9272d-466">You should see hello output files in hello `outputfolder` of `mycontainer` in your Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   <span data-ttu-id="9272d-467">Du bör se fem utdatafiler, en för varje inkommande segment.</span><span class="sxs-lookup"><span data-stu-id="9272d-467">You should see five output files, one for each input slice.</span></span> <span data-ttu-id="9272d-468">Var och en av hello utdata filen måste ha innehåll liknande toohello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="9272d-468">Each of hello output file should have content similar toohello following output:</span></span>

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
    ```
   <span data-ttu-id="9272d-469">hello följande diagram illustrerar hur hello Data Factory segment mappa tootasks i Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="9272d-469">hello following diagram illustrates how hello Data Factory slices map tootasks in Azure Batch.</span></span> <span data-ttu-id="9272d-470">I det här exemplet har ett segment endast en körning.</span><span class="sxs-lookup"><span data-stu-id="9272d-470">In this example, a slice has only one run.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. <span data-ttu-id="9272d-471">Nu ska vi försöker med flera filer i en mapp.</span><span class="sxs-lookup"><span data-stu-id="9272d-471">Now, let’s try with multiple files in a folder.</span></span> <span data-ttu-id="9272d-472">Skapa filer: **file2.txt**, **file3.txt**, **file4.txt**, och **file5.txt** med hello samma innehåll som fil.txt i hello mapp: **2015-11-06-01**.</span><span class="sxs-lookup"><span data-stu-id="9272d-472">Create files: **file2.txt**, **file3.txt**, **file4.txt**, and **file5.txt** with hello same content as in file.txt in hello folder: **2015-11-06-01**.</span></span>
9. <span data-ttu-id="9272d-473">I hello utdatamapp **ta bort** hello utdatafilen: **2015-11-16-01.txt**.</span><span class="sxs-lookup"><span data-stu-id="9272d-473">In hello output folder, **delete** hello output file: **2015-11-16-01.txt**.</span></span>
10. <span data-ttu-id="9272d-474">Nu i hello **OutputDataset** bladet, högerklicka på hello segmentet med **sektorn starttid** ställa in också**2015-11/16 01:00:00 AM**, och klicka på **kör**toorerun/återexport-process hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="9272d-474">Now, in hello **OutputDataset** blade, right-click hello slice with **SLICE START TIME** set too**11/16/2015 01:00:00 AM**, and click **Run** toorerun/re-process hello slice.</span></span> <span data-ttu-id="9272d-475">Nu har hello sektorn fem filer i stället för en fil.</span><span class="sxs-lookup"><span data-stu-id="9272d-475">Now, hello slice has five files instead of one file.</span></span>

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. <span data-ttu-id="9272d-476">När hello sektorn körs och dess status är **klar**, kontrollera hello innehållet i hello utdatafil för det här segmentet (**2015-11-16-01.txt**) i hello `outputfolder` av `mycontainer` i blob storage.</span><span class="sxs-lookup"><span data-stu-id="9272d-476">After hello slice runs and its status is **Ready**, verify hello content in hello output file for this slice (**2015-11-16-01.txt**) in hello `outputfolder` of `mycontainer` in your blob storage.</span></span> <span data-ttu-id="9272d-477">Det bör finnas en rad för varje fil i hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="9272d-477">There should be a line for each file of hello slice.</span></span>

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> <span data-ttu-id="9272d-478">Om du inte ta bort hello utdata filen 2015-11-16-01.txt innan du försöker med fem indatafiler, visas en rad från hello tidigare segmentet kör och fem rader från hello aktuellt segment kör.</span><span class="sxs-lookup"><span data-stu-id="9272d-478">If you did not delete hello output file 2015-11-16-01.txt before trying with five input files, you see one line from hello previous slice run and five lines from hello current slice run.</span></span> <span data-ttu-id="9272d-479">Som standard är hello innehåll läggs toooutput fil om den redan finns.</span><span class="sxs-lookup"><span data-stu-id="9272d-479">By default, hello content is appended toooutput file if it already exists.</span></span>
>
>

#### <a name="data-factory-and-batch-integration"></a><span data-ttu-id="9272d-480">Data Factory och Batch-integrering</span><span class="sxs-lookup"><span data-stu-id="9272d-480">Data Factory and Batch integration</span></span>
<span data-ttu-id="9272d-481">hello Data Factory-tjänsten skapar ett jobb i Azure Batch med hello namn: `adf-poolname:job-xxx`.</span><span class="sxs-lookup"><span data-stu-id="9272d-481">hello Data Factory service creates a job in Azure Batch with hello name: `adf-poolname:job-xxx`.</span></span>

![Azure Data Factory - batchjobb](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

<span data-ttu-id="9272d-483">En aktivitet i hello jobb skapas för varje aktivitet körning av ett segment.</span><span class="sxs-lookup"><span data-stu-id="9272d-483">A task in hello job is created for each activity run of a slice.</span></span> <span data-ttu-id="9272d-484">Om det finns 10 segment-redo toobe bearbetas, har 10 aktiviteter skapats i hello jobb.</span><span class="sxs-lookup"><span data-stu-id="9272d-484">If there are 10 slices ready toobe processed, 10 tasks are created in hello job.</span></span> <span data-ttu-id="9272d-485">Du kan ha fler än ett segment som körs parallellt om du har flera compute-noder i hello pool.</span><span class="sxs-lookup"><span data-stu-id="9272d-485">You can have more than one slice running in parallel if you have multiple compute nodes in hello pool.</span></span> <span data-ttu-id="9272d-486">Om hello maximala uppgifter per compute-nod har angetts för > 1, det kan finnas mer än en segmentera körs på hello samma beräkning.</span><span class="sxs-lookup"><span data-stu-id="9272d-486">If hello maximum tasks per compute node is set too> 1, there can be more than one slice running on hello same compute.</span></span>

<span data-ttu-id="9272d-487">I det här exemplet finns fem segment, så fem aktiviteter i Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="9272d-487">In this example, there are five slices, so five tasks in Azure Batch.</span></span> <span data-ttu-id="9272d-488">Med hello **samtidighet** ställa in också**5** i hello pipeline-JSON i Azure Data Factory och **maximalt uppgifter per VM** ställa in också**2** i Azure Batch med **2** virtuella datorer, hello aktiviteter körs snabb (kontrollera start- och sluttider för aktiviteter).</span><span class="sxs-lookup"><span data-stu-id="9272d-488">With hello **concurrency** set too**5** in hello pipeline JSON in Azure Data Factory and **Maximum tasks per VM** set too**2** in Azure Batch pool with **2** VMs, hello tasks runs fast (check start and end times for tasks).</span></span>

<span data-ttu-id="9272d-489">Använd hello portal tooview hello batchjobb och dess uppgifter som är associerade med hello **segment** och se vilka VM varje segment som kördes på.</span><span class="sxs-lookup"><span data-stu-id="9272d-489">Use hello portal tooview hello Batch job and its tasks that are associated with hello **slices** and see what VM each slice ran on.</span></span>

![Azure Data Factory - jobbet batchaktiviteter](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-hello-pipeline"></a><span data-ttu-id="9272d-491">Felsöka hello pipeline</span><span class="sxs-lookup"><span data-stu-id="9272d-491">Debug hello pipeline</span></span>
<span data-ttu-id="9272d-492">Felsökning består av några grundläggande metoder:</span><span class="sxs-lookup"><span data-stu-id="9272d-492">Debugging consists of a few basic techniques:</span></span>

1. <span data-ttu-id="9272d-493">Om hello inkommande sektorn inte har angetts för**klar**, bekräfta att hello inkommande mappstrukturen är korrekt och fil.txt finns i hello inkommande mappar.</span><span class="sxs-lookup"><span data-stu-id="9272d-493">If hello input slice is not set too**Ready**, confirm that hello input folder structure is correct and file.txt exists in hello input folders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. <span data-ttu-id="9272d-494">I hello **kör** metod för din anpassade aktivitet, Använd hello **IActivityLogger** objekt toolog information som hjälper dig att felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="9272d-494">In hello **Execute** method of your custom activity, use hello **IActivityLogger** object toolog information that helps you troubleshoot issues.</span></span> <span data-ttu-id="9272d-495">hälsningsmeddelande loggas visas i hello användaren\_0. loggfilen.</span><span class="sxs-lookup"><span data-stu-id="9272d-495">hello logged messages show up in hello user\_0.log file.</span></span>

   <span data-ttu-id="9272d-496">I hello **OutputDataset** bladet, klickar du på hello sektorn toosee hello **DATASEKTORN** bladet för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="9272d-496">In hello **OutputDataset** blade, click hello slice toosee hello **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="9272d-497">Du ser **aktivitetskörningar** för den sektorn.</span><span class="sxs-lookup"><span data-stu-id="9272d-497">You see **activity runs** for that slice.</span></span> <span data-ttu-id="9272d-498">Du bör se en aktivitet som körs för hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="9272d-498">You should see one activity run for hello slice.</span></span> <span data-ttu-id="9272d-499">Om du klickar på **kör** i kommandofältet hello börjar du en annan aktivitet som körs för hello samma segment.</span><span class="sxs-lookup"><span data-stu-id="9272d-499">If you click **Run** in hello command bar, you can start another activity run for hello same slice.</span></span>

   <span data-ttu-id="9272d-500">När du klickar på hello aktivitet kör du ser hello **kör AKTIVITETSINFORMATION** bladet med en lista över loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="9272d-500">When you click hello activity run, you see hello **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="9272d-501">Du ser loggade meddelanden i hello **användare\_0. loggen** fil.</span><span class="sxs-lookup"><span data-stu-id="9272d-501">You see logged messages in hello **user\_0.log** file.</span></span> <span data-ttu-id="9272d-502">När ett fel uppstår finns tre aktivitetskörningar eftersom hello återförsöksvärde anges too3 i hello pipeline/aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="9272d-502">When an error occurs, you see three activity runs because hello retry count is set too3 in hello pipeline/activity JSON.</span></span> <span data-ttu-id="9272d-503">När du klickar på hello aktivitet kör se du hello loggfiler att du kan granska tootroubleshoot hello fel.</span><span class="sxs-lookup"><span data-stu-id="9272d-503">When you click hello activity run, you see hello log files that you can review tootroubleshoot hello error.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   <span data-ttu-id="9272d-504">Hello loggfiler, klicka på hello **användare 0.log**.</span><span class="sxs-lookup"><span data-stu-id="9272d-504">In hello list of log files, click hello **user-0.log**.</span></span> <span data-ttu-id="9272d-505">Hello högra panelen är hello resultatet av att använda hello **IActivityLogger.Write** metod.</span><span class="sxs-lookup"><span data-stu-id="9272d-505">In hello right panel are hello results of using hello **IActivityLogger.Write** method.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   <span data-ttu-id="9272d-506">Kontrollera system-0.log för alla system felmeddelanden och undantag.</span><span class="sxs-lookup"><span data-stu-id="9272d-506">Check system-0.log for any system error messages and exceptions.</span></span>

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. <span data-ttu-id="9272d-507">Inkludera hello **PDB** filen i hello zip-filen så att hello felinformation har information som **anropsstacken** när ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="9272d-507">Include hello **PDB** file in hello zip file so that hello error details have information such as **call stack** when an error occurs.</span></span>
4. <span data-ttu-id="9272d-508">Alla hello filer i hello zip-filen för hello anpassad aktivitet måste vara på hello **övre nivå** med inga undermappar.</span><span class="sxs-lookup"><span data-stu-id="9272d-508">All hello files in hello zip file for hello custom activity must be at hello **top level** with no subfolders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. <span data-ttu-id="9272d-509">Se till att hello **assemblyName** (MyDotNetActivity.dll) **entryPoint** (MyDotNetActivityNS.MyDotNetActivity) **packageFile** (customactivitycontainer / MyDotNetActivity.zip) och **packageLinkedService** (bör punkt toohello Azure blob-lagring som innehåller hello zip-filen) har angetts toocorrect värden.</span><span class="sxs-lookup"><span data-stu-id="9272d-509">Ensure that hello **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point toohello Azure blob storage that contains hello zip file) are set toocorrect values.</span></span>
6. <span data-ttu-id="9272d-510">Om du har löst ett fel och vill tooreprocess hello segment Högerklicka hello sektor i hello **OutputDataset** bladet och klicka på **kör**.</span><span class="sxs-lookup"><span data-stu-id="9272d-510">If you fixed an error and want tooreprocess hello slice, right-click hello slice in hello **OutputDataset** blade and click **Run**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > <span data-ttu-id="9272d-511">Du ser en **behållare** i din Azure Blob storage med namnet: `adfjobs`.</span><span class="sxs-lookup"><span data-stu-id="9272d-511">You see a **container** in your Azure Blob storage named: `adfjobs`.</span></span> <span data-ttu-id="9272d-512">Den här behållaren tas inte bort automatiskt, men du kan ta bort när du är klar med testet hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="9272d-512">This container is not automatically deleted, but you can safely delete it after you are done testing hello solution.</span></span> <span data-ttu-id="9272d-513">På liknande sätt hello Data Factory lösning skapar ett Azure Batch **jobbet** med namnet: `adf-\<pool ID/name\>:job-0000000001`.</span><span class="sxs-lookup"><span data-stu-id="9272d-513">Similarly, hello Data Factory solution creates an Azure Batch **job** named: `adf-\<pool ID/name\>:job-0000000001`.</span></span> <span data-ttu-id="9272d-514">Du kan ta bort det här jobbet när du har testat hello lösning om du vill.</span><span class="sxs-lookup"><span data-stu-id="9272d-514">You can delete this job after you test hello solution if you like.</span></span>
   >
   >
7. <span data-ttu-id="9272d-515">hello anpassad aktivitet använder inte hello **app.config** filen från paketet.</span><span class="sxs-lookup"><span data-stu-id="9272d-515">hello custom activity does not use hello **app.config** file from your package.</span></span> <span data-ttu-id="9272d-516">Därför om koden läser eventuella anslutningssträngar från hello konfigurationsfil, fungerar det inte vid körning.</span><span class="sxs-lookup"><span data-stu-id="9272d-516">Therefore, if your code reads any connection strings from hello configuration file, it does not work at runtime.</span></span> <span data-ttu-id="9272d-517">Hej bäst när med hjälp av Azure Batch är toohold alla hemligheter i en **Azure KeyVault**, använda en certifikatbaserad service principal tooprotect hello keyvault och distribuera hello certifikat tooAzure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="9272d-517">hello best practice when using Azure Batch is toohold any secrets in an **Azure KeyVault**, use a certificate-based service principal tooprotect hello keyvault, and distribute hello certificate tooAzure Batch pool.</span></span> <span data-ttu-id="9272d-518">hello kan .NET anpassad aktivitet sedan använda hemligheter från hello KeyVault vid körning.</span><span class="sxs-lookup"><span data-stu-id="9272d-518">hello .NET custom activity then can access secrets from hello KeyVault at runtime.</span></span> <span data-ttu-id="9272d-519">Den här lösningen är en generisk och kan skalas tooany typ av hemlighet, inte bara anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="9272d-519">This solution is a generic one and can scale tooany type of secret, not just connection string.</span></span>

    <span data-ttu-id="9272d-520">Det finns en enklare lösning (men inte rekommenderas): du kan skapa en **Azure SQL länkade tjänsten** med strängen anslutningsinställningar, skapa en datamängd som använder hello länkad tjänst och kedjan hello datamängd som en dummy inkommande datauppsättning toohello anpassad .NET-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9272d-520">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses hello linked service, and chain hello dataset as a dummy input dataset toohello custom .NET activity.</span></span> <span data-ttu-id="9272d-521">Du kan sedan åtkomst hello länkade tjänstens anslutningssträngen i hello anpassad Aktivitetskod och det bör fungera bra vid körning.</span><span class="sxs-lookup"><span data-stu-id="9272d-521">You can then access hello linked service's connection string in hello custom activity code and it should work fine at runtime.</span></span>  

#### <a name="extend-hello-sample"></a><span data-ttu-id="9272d-522">Utöka hello-exempel</span><span class="sxs-lookup"><span data-stu-id="9272d-522">Extend hello sample</span></span>
<span data-ttu-id="9272d-523">Du kan utöka det här exemplet toolearn mer om Azure Data Factory och Azure Batch-funktioner.</span><span class="sxs-lookup"><span data-stu-id="9272d-523">You can extend this sample toolearn more about Azure Data Factory and Azure Batch features.</span></span> <span data-ttu-id="9272d-524">Till exempel tooprocess segment i ett annat tidsintervall hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9272d-524">For example, tooprocess slices in a different time range, do hello following steps:</span></span>

1. <span data-ttu-id="9272d-525">Lägg till följande undermappar i hello hello `inputfolder`: 2015-11-16-05 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 och placera inkommande filer i mapparna.</span><span class="sxs-lookup"><span data-stu-id="9272d-525">Add hello following subfolders in hello `inputfolder`: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08, 2015-11-16-09 and place input files in those folders.</span></span> <span data-ttu-id="9272d-526">Ändra hello sluttiden för pipelinen hello från `2015-11-16T05:00:00Z` för`2015-11-16T10:00:00Z`.</span><span class="sxs-lookup"><span data-stu-id="9272d-526">Change hello end time for hello pipeline from `2015-11-16T05:00:00Z` too`2015-11-16T10:00:00Z`.</span></span> <span data-ttu-id="9272d-527">I hello **diagramvyn**, dubbelklicka på hello **InputDataset**, och bekräfta att hello inkommande segment är klara.</span><span class="sxs-lookup"><span data-stu-id="9272d-527">In hello **Diagram View**, double-click hello **InputDataset**, and confirm that hello input slices are ready.</span></span> <span data-ttu-id="9272d-528">Dubbelklicka på **OuptutDataset** toosee hello tillståndet för utdata segment.</span><span class="sxs-lookup"><span data-stu-id="9272d-528">Double-click **OuptutDataset** toosee hello state of output slices.</span></span> <span data-ttu-id="9272d-529">Om de är i tillståndet Ready Kontrollera hello utdatamapp för hello utdatafilerna.</span><span class="sxs-lookup"><span data-stu-id="9272d-529">If they are in Ready state, check hello output folder for hello output files.</span></span>
2. <span data-ttu-id="9272d-530">Öka eller minska hello **samtidighet** inställningen toounderstand hur den påverkar hello prestandan för din lösning särskilt hello bearbetning som uppstår på Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="9272d-530">Increase or decrease hello **concurrency** setting toounderstand how it affects hello performance of your solution, especially hello processing that occurs on Azure Batch.</span></span> <span data-ttu-id="9272d-531">(Se steg 4: skapa och köra hello pipeline mer information om hello **samtidighet** inställningen.)</span><span class="sxs-lookup"><span data-stu-id="9272d-531">(See Step 4: Create and run hello pipeline for more on hello **concurrency** setting.)</span></span>
3. <span data-ttu-id="9272d-532">Skapa en pool med högre/nedre **maximalt uppgifter per VM**.</span><span class="sxs-lookup"><span data-stu-id="9272d-532">Create a pool with higher/lower **Maximum tasks per VM**.</span></span> <span data-ttu-id="9272d-533">toouse hello ny pool som du skapade uppdateringen hello Azure Batch länkad tjänst i hello Data Factory-lösning.</span><span class="sxs-lookup"><span data-stu-id="9272d-533">toouse hello new pool you created, update hello Azure Batch linked service in hello Data Factory solution.</span></span> <span data-ttu-id="9272d-534">(Se steg 4: skapa och köra hello pipeline mer information om hello **maximalt uppgifter per VM** inställningen.)</span><span class="sxs-lookup"><span data-stu-id="9272d-534">(See Step 4: Create and run hello pipeline for more on hello **Maximum tasks per VM** setting.)</span></span>
4. <span data-ttu-id="9272d-535">Skapa en Azure Batch-pool med **Autoskala** funktion.</span><span class="sxs-lookup"><span data-stu-id="9272d-535">Create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="9272d-536">Skala automatiskt beräkningsnoder i en pool med Azure Batch är hello justeras dynamiskt processorkraft som används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="9272d-536">Automatically scaling compute nodes in an Azure Batch pool is hello dynamic adjustment of processing power used by your application.</span></span> 

    <span data-ttu-id="9272d-537">här hello formeln uppnår hello följande beteende: när hello poolen skapas, det börjar med 1 VM.</span><span class="sxs-lookup"><span data-stu-id="9272d-537">hello sample formula here achieves hello following behavior: When hello pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="9272d-538">$PendingTasks mått definierar hello antalet aktiviteter i körs + aktiv (i kö) tillstånd.</span><span class="sxs-lookup"><span data-stu-id="9272d-538">$PendingTasks metric defines hello number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="9272d-539">hello formeln hittar hello Genomsnittligt antal väntande aktiviteter i hello senaste 180 sekunder och anger därefter TargetDedicated.</span><span class="sxs-lookup"><span data-stu-id="9272d-539">hello formula finds hello average number of pending tasks in hello last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="9272d-540">Det garanterar att TargetDedicated aldrig är mer omfattande än 25 virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9272d-540">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="9272d-541">Så som nya aktiviteter skickas pool växer automatiskt och som slutförda uppgifter, virtuella datorer blir ledigt i taget och hello autoskalning krymper dessa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9272d-541">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and hello autoscaling shrinks those VMs.</span></span> <span data-ttu-id="9272d-542">startingNumberOfVMs och maxNumberofVMs kan vara justerade tooyour behov.</span><span class="sxs-lookup"><span data-stu-id="9272d-542">startingNumberOfVMs and maxNumberofVMs can be adjusted tooyour needs.</span></span>
 
    <span data-ttu-id="9272d-543">Autoskala formel:</span><span class="sxs-lookup"><span data-stu-id="9272d-543">Autoscale formula:</span></span>

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   <span data-ttu-id="9272d-544">Se [automatiskt skala compute-noder i en Azure Batch-pool](../batch/batch-automatic-scaling.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="9272d-544">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

   <span data-ttu-id="9272d-545">Om hello pool använder hello standard [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch-tjänsten kan ta 15 30 minuter tooprepare hello VM innan du kör hello anpassad aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9272d-545">If hello pool is using hello default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch service could take 15-30 minutes tooprepare hello VM before running hello custom activity.</span></span>  <span data-ttu-id="9272d-546">Om hello pool använder en annan autoScaleEvaluationInterval, kan hello Batch-tjänsten ta autoScaleEvaluationInterval + 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="9272d-546">If hello pool is using a different autoScaleEvaluationInterval, hello Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>
5. <span data-ttu-id="9272d-547">I hello exempellösning hello **kör** metoden startar hello **Calculate** metod som bearbetar en indata sektorn tooproduce datasektorn en utdata.</span><span class="sxs-lookup"><span data-stu-id="9272d-547">In hello sample solution, hello **Execute** method invokes hello **Calculate** method that processes an input data slice tooproduce an output data slice.</span></span> <span data-ttu-id="9272d-548">Du kan skriva egna metoden tooprocess indata och Ersätt hello Calculate metodanrop i hello Execute-metoden med en anropet tooyour metod.</span><span class="sxs-lookup"><span data-stu-id="9272d-548">You can write your own method tooprocess input data and replace hello Calculate method call in hello Execute method with a call tooyour method.</span></span>

### <a name="next-steps-consume-hello-data"></a><span data-ttu-id="9272d-549">Nästa steg: använda hello data</span><span class="sxs-lookup"><span data-stu-id="9272d-549">Next steps: Consume hello data</span></span>
<span data-ttu-id="9272d-550">När du bearbetar data kan du använda den med online verktyg som **Microsoft Power BI**.</span><span class="sxs-lookup"><span data-stu-id="9272d-550">After you process data, you can consume it with online tools like **Microsoft Power BI**.</span></span> <span data-ttu-id="9272d-551">Här är länkar toohelp du förstår Power BI och hur toouse den i Azure:</span><span class="sxs-lookup"><span data-stu-id="9272d-551">Here are links toohelp you understand Power BI and how toouse it in Azure:</span></span>

* [<span data-ttu-id="9272d-552">Utforska en datamängd i Power BI</span><span class="sxs-lookup"><span data-stu-id="9272d-552">Explore a dataset in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [<span data-ttu-id="9272d-553">Komma igång med hello Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="9272d-553">Getting started with hello Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [<span data-ttu-id="9272d-554">Uppdatera data i Power BI</span><span class="sxs-lookup"><span data-stu-id="9272d-554">Refresh data in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [<span data-ttu-id="9272d-555">Azure och Power BI - översikt</span><span class="sxs-lookup"><span data-stu-id="9272d-555">Azure and Power BI - basic overview</span></span>](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a><span data-ttu-id="9272d-556">Referenser</span><span class="sxs-lookup"><span data-stu-id="9272d-556">References</span></span>
* [<span data-ttu-id="9272d-557">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="9272d-557">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

  * [<span data-ttu-id="9272d-558">Introduktion tooAzure Data Factory-tjänsten</span><span class="sxs-lookup"><span data-stu-id="9272d-558">Introduction tooAzure Data Factory service</span></span>](data-factory-introduction.md)
  * [<span data-ttu-id="9272d-559">Kom igång med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="9272d-559">Get started with Azure Data Factory</span></span>](data-factory-build-your-first-pipeline.md)
  * [<span data-ttu-id="9272d-560">Använd anpassade aktiviteter i en Azure Data Factory-pipeline</span><span class="sxs-lookup"><span data-stu-id="9272d-560">Use custom activities in an Azure Data Factory pipeline</span></span>](data-factory-use-custom-activities.md)
* [<span data-ttu-id="9272d-561">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="9272d-561">Azure Batch</span></span>](https://azure.microsoft.com/documentation/services/batch/)

  * [<span data-ttu-id="9272d-562">Grunderna i Azure Batch</span><span class="sxs-lookup"><span data-stu-id="9272d-562">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
  * [<span data-ttu-id="9272d-563">Översikt över Azure Batch-funktioner</span><span class="sxs-lookup"><span data-stu-id="9272d-563">Overview of Azure Batch features</span></span>](../batch/batch-api-basics.md)
  * [<span data-ttu-id="9272d-564">Skapa och hantera Azure Batch-kontot i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9272d-564">Create and manage Azure Batch account in hello Azure portal</span></span>](../batch/batch-account-create-portal.md)
  * [<span data-ttu-id="9272d-565">Kom igång med Azure Batch-biblioteket .NET</span><span class="sxs-lookup"><span data-stu-id="9272d-565">Get started with Azure Batch Library .NET</span></span>](../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
