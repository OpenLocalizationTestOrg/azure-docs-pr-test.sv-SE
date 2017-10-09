---
title: "aaaTutorial - Använd hello Azure Batch-klientbibliotek för Node.js | Microsoft Docs"
description: "Lär dig hello grundläggande begrepp för Azure Batch och skapa en enkel lösning med hjälp av Node.js."
services: batch
author: shwetams
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: nodejs
ms.topic: hero-article
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: shwetams
ms.openlocfilehash: d2b0ecbe764e7100affd7b02839aef3077b073cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-batch-sdk-for-nodejs"></a><span data-ttu-id="42730-103">Kom igång med Batch SDK för Node.js</span><span class="sxs-lookup"><span data-stu-id="42730-103">Get started with Batch SDK for Node.js</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="42730-104">NET</span><span class="sxs-lookup"><span data-stu-id="42730-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="42730-105">Python</span><span class="sxs-lookup"><span data-stu-id="42730-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="42730-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="42730-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="42730-107">Lär dig grunderna hello för att skapa en Batch-klient i Node.js med [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/).</span><span class="sxs-lookup"><span data-stu-id="42730-107">Learn hello basics of building a Batch client in Node.js using [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/).</span></span> <span data-ttu-id="42730-108">Vi går igenom ett scenario med ett batch-program, steg för steg, och utför sedan en konfigurering med en Node.js-klient.</span><span class="sxs-lookup"><span data-stu-id="42730-108">We take a step by step approach of understanding a scenario for a batch application and then setting it up using a Node.js client.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="42730-109">Krav</span><span class="sxs-lookup"><span data-stu-id="42730-109">Prerequisites</span></span>
<span data-ttu-id="42730-110">Den här artikeln förutsätter att du har kunskaper om Node.js och att du är bekant med Linux.</span><span class="sxs-lookup"><span data-stu-id="42730-110">This article assumes that you have a working knowledge of Node.js and familiarity with Linux.</span></span> <span data-ttu-id="42730-111">Det förutsätts även att du har en Azure-konto-installation med rättigheter toocreate Batch- och nätverksåtkomsttjänster.</span><span class="sxs-lookup"><span data-stu-id="42730-111">It also assumes that you have an Azure account setup with access rights toocreate Batch and Storage services.</span></span>

<span data-ttu-id="42730-112">Vi rekommenderar att läsa [teknisk översikt över Azure Batch](batch-technical-overview.md) innan du går igenom hello stegen som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="42730-112">We recommend reading [Azure Batch Technical Overview](batch-technical-overview.md) before you go through hello steps outlined this article.</span></span>

## <a name="hello-tutorial-scenario"></a><span data-ttu-id="42730-113">hello självstudiekursen scenario</span><span class="sxs-lookup"><span data-stu-id="42730-113">hello tutorial scenario</span></span>
<span data-ttu-id="42730-114">Låt oss förstå hello batch arbetsflödet scenario.</span><span class="sxs-lookup"><span data-stu-id="42730-114">Let us understand hello batch workflow scenario.</span></span> <span data-ttu-id="42730-115">Vi har ett enkelt skript som skrivits i Python som hämtar alla csv-filer från ett Azure Blob storage-behållare och konverterar dem tooJSON.</span><span class="sxs-lookup"><span data-stu-id="42730-115">We have a simple script written in Python that downloads all csv files from an Azure Blob storage container and converts them tooJSON.</span></span> <span data-ttu-id="42730-116">tooprocess flera lagring konto behållare parallellt kan vi distribuera hello skript som ett Azure Batch-jobb.</span><span class="sxs-lookup"><span data-stu-id="42730-116">tooprocess multiple storage account containers in parallel, we can deploy hello script as an Azure Batch job.</span></span>

## <a name="azure-batch-architecture"></a><span data-ttu-id="42730-117">Azure Batch-arkitektur</span><span class="sxs-lookup"><span data-stu-id="42730-117">Azure Batch Architecture</span></span>
<span data-ttu-id="42730-118">hello visar följande diagram hur vi kan skala hello Python-skriptet med hjälp av Azure Batch och en Node.js-klient.</span><span class="sxs-lookup"><span data-stu-id="42730-118">hello following diagram depicts how we can scale hello Python script using Azure Batch and a Node.js client.</span></span>

![Azure Batch-scenario](./media/batch-nodejs-get-started/BatchScenario.png)

<span data-ttu-id="42730-120">Hej node.js klient distribuerar batchjobb med en jobbförberedelseuppgift (beskrivs i detalj senare) och en uppsättning uppgifter beroende på hello antal behållare i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="42730-120">hello node.js client deploys a batch job with a preparation task (explained in detail later) and a set of tasks depending on hello number of containers in hello storage account.</span></span> <span data-ttu-id="42730-121">Du kan hämta hello skript från hello github-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="42730-121">You can download hello scripts from hello github repository.</span></span>

* [<span data-ttu-id="42730-122">Node.js-klient</span><span class="sxs-lookup"><span data-stu-id="42730-122">Node.js client</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/nodejs_batch_client_sample.js)
* [<span data-ttu-id="42730-123">Förberedande aktivitet – kommandoskript</span><span class="sxs-lookup"><span data-stu-id="42730-123">Preparation task shell scripts</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/startup_prereq.sh)
* [<span data-ttu-id="42730-124">Python csv tooJSON-processor</span><span class="sxs-lookup"><span data-stu-id="42730-124">Python csv tooJSON processor</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/processcsv.py)

> [!TIP]
> <span data-ttu-id="42730-125">hello Node.js-klienten i hello länk anges innehåller inte specifik kod toobe distribueras som en funktionsapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="42730-125">hello Node.js client in hello link specified does not contain specific code toobe deployed as an Azure function app.</span></span> <span data-ttu-id="42730-126">Du kan se följande länkar för anvisningar toocreate en toohello.</span><span class="sxs-lookup"><span data-stu-id="42730-126">You can refer toohello following links for instructions toocreate one.</span></span>
> - [<span data-ttu-id="42730-127">Skapa funktionsappar</span><span class="sxs-lookup"><span data-stu-id="42730-127">Create function app</span></span>](../azure-functions/functions-create-first-azure-function.md)
> - [<span data-ttu-id="42730-128">Skapa timerutlösare</span><span class="sxs-lookup"><span data-stu-id="42730-128">Create timer trigger function</span></span>](../azure-functions/functions-bindings-timer.md)
>
>

## <a name="build-hello-application"></a><span data-ttu-id="42730-129">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="42730-129">Build hello application</span></span>

<span data-ttu-id="42730-130">Låt oss Följ nu hello stegvis genom processen i Skapa hello Node.js-klienten:</span><span class="sxs-lookup"><span data-stu-id="42730-130">Now, let us follow hello process step by step into building hello Node.js client:</span></span>

### <a name="step-1-install-azure-batch-sdk"></a><span data-ttu-id="42730-131">Steg 1: Installera Azure Batch SDK</span><span class="sxs-lookup"><span data-stu-id="42730-131">Step 1: Install Azure Batch SDK</span></span>

<span data-ttu-id="42730-132">Du kan installera Azure Batch SDK för Node.js kommandot hello npm installation.</span><span class="sxs-lookup"><span data-stu-id="42730-132">You can install Azure Batch SDK for Node.js using hello npm install command.</span></span>

`npm install azure-batch`

<span data-ttu-id="42730-133">Det här kommandot installerar hello senaste versionen av azure batch-nod SDK.</span><span class="sxs-lookup"><span data-stu-id="42730-133">This command installs hello latest version of azure-batch node SDK.</span></span>

>[!Tip]
> <span data-ttu-id="42730-134">I en app i Azure-funktion, kan du gå för ”Kudu” i hello Azure funktionen konsolinställningar fliken toorun hello npm installera kommandon.</span><span class="sxs-lookup"><span data-stu-id="42730-134">In an Azure Function app, you can go too"Kudu Console" in hello Azure function's Settings tab toorun hello npm install commands.</span></span> <span data-ttu-id="42730-135">I det här fallet tooinstall Azure Batch-SDK för Node.js.</span><span class="sxs-lookup"><span data-stu-id="42730-135">In this case tooinstall Azure Batch SDK for Node.js.</span></span>
>
>

### <a name="step-2-create-an-azure-batch-account"></a><span data-ttu-id="42730-136">Steg 2: Skapa ett Azure Batch-konto</span><span class="sxs-lookup"><span data-stu-id="42730-136">Step 2: Create an Azure Batch account</span></span>

<span data-ttu-id="42730-137">Du kan skapa från hello [Azure-portalen](batch-account-create-portal.md) eller från kommandoraden ([Powershell](batch-powershell-cmdlets-get-started.md) /[Azure cli](https://docs.microsoft.com/cli/azure/overview)).</span><span class="sxs-lookup"><span data-stu-id="42730-137">You can create it from hello [Azure portal](batch-account-create-portal.md) or from command line ([Powershell](batch-powershell-cmdlets-get-started.md) /[Azure cli](https://docs.microsoft.com/cli/azure/overview)).</span></span>

<span data-ttu-id="42730-138">Följande är hello kommandon toocreate en via Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="42730-138">Following are hello commands toocreate one through Azure CLI.</span></span>

<span data-ttu-id="42730-139">Skapa en resursgrupp, hoppa över det här steget om du redan har en där du vill att toocreate hello Batch-kontot:</span><span class="sxs-lookup"><span data-stu-id="42730-139">Create a Resource Group, skip this step if you already have one where you want toocreate hello Batch Account:</span></span>

`az group create -n "<resource-group-name>" -l "<location>"`

<span data-ttu-id="42730-140">Skapa ett Azure Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="42730-140">Next, create an Azure Batch account.</span></span>

`az batch account create -l "<location>"  -g "<resource-group-name>" -n "<batch-account-name>"`

<span data-ttu-id="42730-141">Varje Batch-konto har motsvarande åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="42730-141">Each Batch account has its corresponding access keys.</span></span> <span data-ttu-id="42730-142">Nycklarna är nödvändiga toocreate ytterligare resurser i Azure batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="42730-142">These keys are needed toocreate further resources in Azure batch account.</span></span> <span data-ttu-id="42730-143">En bra idé för produktionsmiljö är toouse Azure Key Vault toostore nycklarna.</span><span class="sxs-lookup"><span data-stu-id="42730-143">A good practice for production environment is toouse Azure Key Vault toostore these keys.</span></span> <span data-ttu-id="42730-144">Du kan sedan skapa en tjänst huvudnamn för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="42730-144">You can then create a Service principal for hello application.</span></span> <span data-ttu-id="42730-145">Med det här tjänstprogrammet huvudnamn hello kan skapa en OAuth-token tooaccess nycklar från hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="42730-145">Using this service principal hello application can create an OAuth token tooaccess keys from hello key vault.</span></span>

`az batch account keys list -g "<resource-group-name>" -n "<batch-account-name>"`

<span data-ttu-id="42730-146">Kopiera och spara hello viktiga toobe används i hello efterföljande steg.</span><span class="sxs-lookup"><span data-stu-id="42730-146">Copy and store hello key toobe used in hello subsequent steps.</span></span>

### <a name="step-3-create-an-azure-batch-service-client"></a><span data-ttu-id="42730-147">Steg 3: Skapa en Azure Batch-tjänsteklient</span><span class="sxs-lookup"><span data-stu-id="42730-147">Step 3: Create an Azure Batch service client</span></span>
<span data-ttu-id="42730-148">Följande kodavsnitt först importerar hello azure batch Node.js-modulen och skapar sedan en Batch-tjänsten-klient.</span><span class="sxs-lookup"><span data-stu-id="42730-148">Following code snippet first imports hello azure-batch Node.js module and then creates a Batch Service client.</span></span> <span data-ttu-id="42730-149">Du behöver toofirst skapa ett SharedKeyCredentials-objekt med hello Batch kontonyckel kopieras från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="42730-149">You need toofirst create a SharedKeyCredentials object with hello Batch account key copied from hello previous step.</span></span>

```nodejs
// Initializing Azure Batch variables

var batch = require('azure-batch');

var accountName = '<azure-batch-account-name>';

var accountKey = '<account-key-downloaded>';

var accountUrl = '<account-url>'

// Create Batch credentials object using account name and account key

var credentials = new batch.SharedKeyCredentials(accountName,accountKey);

// Create Batch service client

var batch_client = new batch.ServiceClient(credentials,accountUrl);

```

<span data-ttu-id="42730-150">hello Azure Batch-URI kan hittas i hello översiktsflik av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="42730-150">hello Azure Batch URI can be found in hello Overview tab of hello Azure portal.</span></span> <span data-ttu-id="42730-151">Det har hello format:</span><span class="sxs-lookup"><span data-stu-id="42730-151">It is of hello format:</span></span>

`https://accountname.location.batch.azure.com`

<span data-ttu-id="42730-152">Se toohello skärmbild:</span><span class="sxs-lookup"><span data-stu-id="42730-152">Refer toohello screenshot:</span></span>

![URI för Azure Batch](./media/batch-nodejs-get-started/azurebatchuri.png)



### <a name="step-4-create-an-azure-batch-pool"></a><span data-ttu-id="42730-154">Steg 4: Skapa en Azure Batch-pool</span><span class="sxs-lookup"><span data-stu-id="42730-154">Step 4: Create an Azure Batch pool</span></span>
<span data-ttu-id="42730-155">En Azure Batch-pool består av flera virtuella datorer (även kallade batchnoder).</span><span class="sxs-lookup"><span data-stu-id="42730-155">An Azure Batch pool consists of multiple VMs (also known as Batch Nodes).</span></span> <span data-ttu-id="42730-156">Azure Batch-tjänsten distribuerar hello uppgifter på dessa noder och hanterar dem..</span><span class="sxs-lookup"><span data-stu-id="42730-156">Azure Batch service deploys hello tasks on these nodes and manages them.</span></span> <span data-ttu-id="42730-157">Du kan definiera hello följande konfigurationsparametrar för din pool.</span><span class="sxs-lookup"><span data-stu-id="42730-157">You can define hello following configuration parameters for your pool.</span></span>

* <span data-ttu-id="42730-158">Typ av virtuell datoravbildning</span><span class="sxs-lookup"><span data-stu-id="42730-158">Type of Virtual Machine image</span></span>
* <span data-ttu-id="42730-159">Storlek på de virtuella datornoderna</span><span class="sxs-lookup"><span data-stu-id="42730-159">Size of Virtual Machine nodes</span></span>
* <span data-ttu-id="42730-160">Antal virtuella datornoder</span><span class="sxs-lookup"><span data-stu-id="42730-160">Number of Virtual Machine nodes</span></span>

> [!Tip]
> <span data-ttu-id="42730-161">hello storlek och antalet virtuella noder beror i stort sett på hello antalet aktiviteter som du vill toorun i parallell och även själva hello-aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="42730-161">hello size and number of Virtual Machine nodes largely depend on hello number of tasks you want toorun in parallel and also hello task itself.</span></span> <span data-ttu-id="42730-162">Vi rekommenderar att du testar toodetermine hello bästa antal och storlek.</span><span class="sxs-lookup"><span data-stu-id="42730-162">We recommend testing toodetermine hello ideal number and size.</span></span>
>
>

<span data-ttu-id="42730-163">hello skapar nedanstående kodutdrag hello parametern konfigurationsobjekt.</span><span class="sxs-lookup"><span data-stu-id="42730-163">hello following code snippet creates hello configuration parameter objects.</span></span>

```nodejs
// Creating Image reference configuration for Ubuntu Linux VM
var imgRef = {publisher:"Canonical",offer:"UbuntuServer",sku:"14.04.2-LTS",version:"latest"}

// Creating hello VM configuration object with hello SKUID
var vmconfig = {imageReference:imgRef,nodeAgentSKUId:"batch.node.ubuntu 14.04"}

// Setting hello VM size tooStandard F4
var vmSize = "STANDARD_F4"

//Setting number of VMs in hello pool too4
var numVMs = 4
```

> [!Tip]
> <span data-ttu-id="42730-164">Hello lista över Linux VM-avbildningar som är tillgängliga för Azure Batch och deras SKU-ID, se [lista över virtuella datoravbildningar](batch-linux-nodes.md#list-of-virtual-machine-images).</span><span class="sxs-lookup"><span data-stu-id="42730-164">For hello list of Linux VM images available for Azure Batch and their SKU IDs, see [List of virtual machine images](batch-linux-nodes.md#list-of-virtual-machine-images).</span></span>
>
>

<span data-ttu-id="42730-165">Du kan skapa hello Azure Batch-pool när hello poolen konfiguration har definierats.</span><span class="sxs-lookup"><span data-stu-id="42730-165">Once hello pool configuration is defined, you can create hello Azure Batch pool.</span></span> <span data-ttu-id="42730-166">hello Batch-pool-kommando skapar en virtuell dator i Azure-noder och förbereda dem toobe klar tooreceive uppgifter tooexecute.</span><span class="sxs-lookup"><span data-stu-id="42730-166">hello Batch pool command creates Azure Virtual Machine nodes and prepares them toobe ready tooreceive tasks tooexecute.</span></span> <span data-ttu-id="42730-167">I alla efterföljande steg ska det finnas ett unikt referens-ID.</span><span class="sxs-lookup"><span data-stu-id="42730-167">Each pool should have a unique ID for reference in subsequent steps.</span></span>

<span data-ttu-id="42730-168">följande kodstycke hello skapar en Azure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="42730-168">hello following code snippet creates an Azure Batch pool.</span></span>

```nodejs
// Create a unique Azure Batch pool ID
var poolid = "pool" + customerDetails.customerid;
var poolConfig = {id:poolid, displayName:poolid,vmSize:vmSize,virtualMachineConfiguration:vmconfig,targetDedicatedComputeNodes:numVms,enableAutoScale:false };
// Creating hello Pool for hello specific customer
var pool = batch_client.pool.add(poolConfig,function(error,result){
    if(error!=null){console.log(error.response)};
});
```

<span data-ttu-id="42730-169">Du kan kontrollera hello status för hello poolen skapats och att hello är i tillståndet ”aktiv” innan du går vidare med överföring av en jobbet toothat pool.</span><span class="sxs-lookup"><span data-stu-id="42730-169">You can check hello status of hello pool created and ensure that hello state is in "active" before going ahead with submission of a Job toothat pool.</span></span>

```nodejs
var cloudPool = batch_client.pool.get(poolid,function(error,result,request,response){
        if(error == null)
        {

            if(result.state == "active")
            {
                console.log("Pool is active");
            }
        }
        else
        {
            if(error.statusCode==404)
            {
                console.log("Pool not found yet returned 404...");    

            }
            else
            {
                console.log("Error occurred while retrieving pool data");
            }
        }
        });
```

<span data-ttu-id="42730-170">Följande är ett exempel resultatet-objekt som returnerats av hello pool.get.</span><span class="sxs-lookup"><span data-stu-id="42730-170">Following is a sample result object returned by hello pool.get function.</span></span>

```
{ id: 'processcsv_201721152',
  displayName: 'processcsv_201721152',
  url: 'https://<batch-account-name>.centralus.batch.azure.com/pools/processcsv_201721152',
  eTag: '<eTag>',
  lastModified: 2017-03-27T10:28:02.398Z,
  creationTime: 2017-03-27T10:28:02.398Z,
  state: 'active',
  stateTransitionTime: 2017-03-27T10:28:02.398Z,
  allocationState: 'resizing',
  allocationStateTransitionTime: 2017-03-27T10:28:02.398Z,
  vmSize: 'standard_a1',
  virtualMachineConfiguration:
   { imageReference:
      { publisher: 'Canonical',
        offer: 'UbuntuServer',
        sku: '14.04.2-LTS',
        version: 'latest' },
     nodeAgentSKUId: 'batch.node.ubuntu 14.04' },
  resizeTimeout:
   { [Number: 900000]
     _milliseconds: 900000,
     _days: 0,
     _months: 0,
     _data:
      { milliseconds: 0,
        seconds: 0,
        minutes: 15,
        hours: 0,
        days: 0,
        months: 0,
        years: 0 },
     _locale:
      Locale {
        _calendar: [Object],
        _longDateFormat: [Object],
        _invalidDate: 'Invalid date',
        ordinal: [Function: ordinal],
        _ordinalParse: /\d{1,2}(th|st|nd|rd)/,
        _relativeTime: [Object],
        _months: [Object],
        _monthsShort: [Object],
        _week: [Object],
        _weekdays: [Object],
        _weekdaysMin: [Object],
        _weekdaysShort: [Object],
        _meridiemParse: /[ap]\.?m?\.?/i,
        _abbr: 'en',
        _config: [Object],
        _ordinalParseLenient: /\d{1,2}(th|st|nd|rd)|\d{1,2}/ } },
  currentDedicated: 0,
  targetDedicated: 4,
  enableAutoScale: false,
  enableInterNodeCommunication: false,
  maxTasksPerNode: 1,
  taskSchedulingPolicy: { nodeFillType: 'Spread' } }
```


### <a name="step-4-submit-an-azure-batch-job"></a><span data-ttu-id="42730-171">Steg 4: Skicka ett Azure Batch-jobb</span><span class="sxs-lookup"><span data-stu-id="42730-171">Step 4: Submit an Azure Batch job</span></span>
<span data-ttu-id="42730-172">Azure Batch-jobbet består av en logisk grupp av snarlika uppgifter.</span><span class="sxs-lookup"><span data-stu-id="42730-172">An Azure Batch job is a logical group of similar tasks.</span></span> <span data-ttu-id="42730-173">I vårt scenario är det ”processen csv tooJSON”.</span><span class="sxs-lookup"><span data-stu-id="42730-173">In our scenario, it is "Process csv tooJSON."</span></span> <span data-ttu-id="42730-174">Varje aktivitet här kan bearbeta de CSV-filer som finns i respektive Azure Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="42730-174">Each task here could be processing csv files present in each Azure Storage container.</span></span>

<span data-ttu-id="42730-175">Dessa aktiviteter körs parallellt och distribueras över flera noder, styrd av hello Azure Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="42730-175">These tasks would run in parallel and deployed across multiple nodes, orchestrated by hello Azure Batch service.</span></span>

> [!Tip]
> <span data-ttu-id="42730-176">Du kan använda hello [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) egenskapen toospecify högsta antalet uppgifter som kan köras samtidigt på en enda nod.</span><span class="sxs-lookup"><span data-stu-id="42730-176">You can use hello [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) property toospecify maximum number of tasks that can run concurrently on a single node.</span></span>
>
>

#### <a name="preparation-task"></a><span data-ttu-id="42730-177">Förberedande aktivitet</span><span class="sxs-lookup"><span data-stu-id="42730-177">Preparation task</span></span>

<span data-ttu-id="42730-178">hello VM noder som skapats är tomt Ubuntu-noder.</span><span class="sxs-lookup"><span data-stu-id="42730-178">hello VM nodes created are blank Ubuntu nodes.</span></span> <span data-ttu-id="42730-179">Du behöver ofta tooinstall en uppsättning program som krav.</span><span class="sxs-lookup"><span data-stu-id="42730-179">Often, you need tooinstall a set of programs as prerequisites.</span></span>
<span data-ttu-id="42730-180">Du kan normalt ha ett kommandoskript som installerar hello krav innan du hello faktiska aktiviteter som körs för Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="42730-180">Typically, for Linux nodes you can have a shell script that installs hello prerequisites before hello actual tasks run.</span></span> <span data-ttu-id="42730-181">Det kan röra sig om vilka körbara filer som helst.</span><span class="sxs-lookup"><span data-stu-id="42730-181">However it could be any programmable executable.</span></span>
<span data-ttu-id="42730-182">Hej [shell skriptet](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) i det här exemplet installerar Python pip och hello Azure Storage SDK för Python.</span><span class="sxs-lookup"><span data-stu-id="42730-182">hello [shell script](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) in this example installs Python-pip and hello Azure Storage SDK for Python.</span></span>

<span data-ttu-id="42730-183">Du kan ladda upp hello skriptet på ett Azure Storage-konto och generera ett SAS-URI tooaccess hello skript.</span><span class="sxs-lookup"><span data-stu-id="42730-183">You can upload hello script on an Azure Storage Account and generate a SAS URI tooaccess hello script.</span></span> <span data-ttu-id="42730-184">Den här processen kan också automatiseras med hjälp av hello Azure Storage Node.js SDK.</span><span class="sxs-lookup"><span data-stu-id="42730-184">This process can also be automated using hello Azure Storage Node.js SDK.</span></span>

> [!Tip]
> <span data-ttu-id="42730-185">En jobbförberedelseuppgift för ett jobb körs bara på hello VM noder där hello viss uppgift måste toorun.</span><span class="sxs-lookup"><span data-stu-id="42730-185">A preparation task for a job runs only on hello VM nodes where hello specific task needs toorun.</span></span> <span data-ttu-id="42730-186">Om du vill krav toobe installerad på alla noder oavsett hello aktiviteter som körs på den, du kan använda hello [startuppgift har ställts](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) egenskapen när du lägger till en pool.</span><span class="sxs-lookup"><span data-stu-id="42730-186">If you want prerequisites toobe installed on all nodes irrespective of hello tasks that run on it, you can use hello [startTask](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) property while adding a pool.</span></span> <span data-ttu-id="42730-187">Du kan använda hello följande förberedelser aktivitetsdefinitionen för referens.</span><span class="sxs-lookup"><span data-stu-id="42730-187">You can use hello following preparation task definition for reference.</span></span>
>
>

<span data-ttu-id="42730-188">En jobbförberedelseuppgift har angetts under hello överföringen av Azure Batch-jobbet.</span><span class="sxs-lookup"><span data-stu-id="42730-188">A preparation task is specified during hello submission of Azure Batch job.</span></span> <span data-ttu-id="42730-189">Följande är hello konfigurationsparametrar för förberedelse av aktivitet:</span><span class="sxs-lookup"><span data-stu-id="42730-189">Following are hello preparation task configuration parameters:</span></span>

* <span data-ttu-id="42730-190">**ID**: en unik identifierare för hello jobbförberedelseuppgift</span><span class="sxs-lookup"><span data-stu-id="42730-190">**ID**: A unique identifier for hello preparation task</span></span>
* <span data-ttu-id="42730-191">**commandLine**: kommandoraden tooexecute hello aktiviteten</span><span class="sxs-lookup"><span data-stu-id="42730-191">**commandLine**: Command line tooexecute hello task executable</span></span>
* <span data-ttu-id="42730-192">**resourceFiles**: matris med-objekt som innehåller information om filer som behövs toobe hämtas för den här uppgiften toorun.</span><span class="sxs-lookup"><span data-stu-id="42730-192">**resourceFiles**: Array of objects that provide details of files needed toobe downloaded for this task toorun.</span></span>  <span data-ttu-id="42730-193">Här visas alternativen</span><span class="sxs-lookup"><span data-stu-id="42730-193">Following are its options</span></span>
    - <span data-ttu-id="42730-194">blobSource: hello hello filen SAS-URI</span><span class="sxs-lookup"><span data-stu-id="42730-194">blobSource: hello SAS URI of hello file</span></span>
    - <span data-ttu-id="42730-195">filePath: lokal sökväg toodownload och spara hello-filen</span><span class="sxs-lookup"><span data-stu-id="42730-195">filePath: Local path toodownload and save hello file</span></span>
    - <span data-ttu-id="42730-196">fileMode: fileMode har ett oktalt format med standardvärdet 0770 (gäller endast Linux-noder).</span><span class="sxs-lookup"><span data-stu-id="42730-196">fileMode: Only applicable for Linux nodes, fileMode is in octal format with a default value of 0770</span></span>
* <span data-ttu-id="42730-197">**waitForSuccess**: om set tootrue, hello aktiviteten inte körs på misslyckade aktiviteter för förberedelser</span><span class="sxs-lookup"><span data-stu-id="42730-197">**waitForSuccess**: If set tootrue, hello task does not run on preparation task failures</span></span>
* <span data-ttu-id="42730-198">**runElevated**: Ange den tootrue om utökade privilegier krävs toorun hello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="42730-198">**runElevated**: Set it tootrue if elevated privileges are needed toorun hello task.</span></span>

<span data-ttu-id="42730-199">Följande kodavsnitt visar konfiguration för hello förberedelse uppgiften skriptexempel:</span><span class="sxs-lookup"><span data-stu-id="42730-199">Following code snippet shows hello preparation task script configuration sample:</span></span>

```nodejs
var job_prep_task_config = {id:"installprereq",commandLine:"sudo sh startup_prereq.sh > startup.log",resourceFiles:[{'blobSource':'Blob SAS URI','filePath':'startup_prereq.sh'}],waitForSuccess:true,runElevated:true}
```

<span data-ttu-id="42730-200">Om det finns inga krav toobe som installerats för dina uppgifter toorun, kan du hoppa över hello förberedande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="42730-200">If there are no prerequisites toobe installed for your tasks toorun, you can skip hello preparation tasks.</span></span> <span data-ttu-id="42730-201">Följande kod skapar ett jobb med visningsnamnet ”process csv files” (bearbeta CSV-filer).</span><span class="sxs-lookup"><span data-stu-id="42730-201">Following code creates a job with display name "process csv files."</span></span>

 ```nodejs
 // Setting up Batch pool configuration
 var pool_config = {poolId:poolid}
 // Setting up Job configuration along with preparation task
 var jobId = "processcsvjob"
 var job_config = {id:jobId,displayName:"process csv files",jobPreparationTask:job_prep_task_config,poolInfo:pool_config}
 // Adding Azure batch job toohello pool
 var job = batch_client.job.add(job_config,function(error,result){
     if(error != null)
     {
         console.log("Error submitting job : " + error.response);
     }});
```


### <a name="step-5-submit-azure-batch-tasks-for-a-job"></a><span data-ttu-id="42730-202">Steg 5: Skicka Azure Batch-aktiviteter för ett jobb</span><span class="sxs-lookup"><span data-stu-id="42730-202">Step 5: Submit Azure Batch tasks for a job</span></span>

<span data-ttu-id="42730-203">Nu när vi har skapat ett jobb för bearbetning av CSV-filer kan vi börja skapa aktiviteter för jobbet i fråga.</span><span class="sxs-lookup"><span data-stu-id="42730-203">Now that our process csv job is created, let us create tasks for that job.</span></span> <span data-ttu-id="42730-204">Under förutsättning att vi har fyra behållare, har vi toocreate fyra aktiviteter, en för varje behållare.</span><span class="sxs-lookup"><span data-stu-id="42730-204">Assuming we have four containers, we have toocreate four tasks, one for each container.</span></span>

<span data-ttu-id="42730-205">Om vi tittar på hello [Python-skriptet](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), den accepterar två parametrar:</span><span class="sxs-lookup"><span data-stu-id="42730-205">If we look at hello [Python script](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), it accepts two parameters:</span></span>

* <span data-ttu-id="42730-206">behållarnamn: hello lagringsfilerna behållaren toodownload från</span><span class="sxs-lookup"><span data-stu-id="42730-206">container name: hello Storage container toodownload files from</span></span>
* <span data-ttu-id="42730-207">pattern: En valfri parameter för filnamnsmönster</span><span class="sxs-lookup"><span data-stu-id="42730-207">pattern: An optional parameter of file name pattern</span></span>

<span data-ttu-id="42730-208">Anta att vi har fyra behållare ”con1”, ”con2”, ”con3” och kod ”con4” följande visar skickar för uppgifter toohello Azure batch-jobbet ”processen csv” vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="42730-208">Assuming we have four containers "con1", "con2", "con3","con4" following code shows submitting for tasks toohello Azure batch job "process csv" we created earlier.</span></span>

```nodejs
// storing container names in an array
var container_list = ["con1","con2","con3","con4"]
    container_list.forEach(function(val,index){           

           var container_name = val;
           var taskID = container_name + "_process";
           var task_config = {id:taskID,displayName:'process csv in ' + container_name,commandLine:'python processcsv.py --container ' + container_name,resourceFiles:[{'blobSource':'<blob SAS URI>','filePath':'processcsv.py'}]}
           var task = batch_client.task.add(poolid,task_config,function(error,result){
                if(error != null)
                {
                    console.log(error.response);     
                }
                else
                {
                    console.log("Task for container : " + container_name + "submitted successfully");
                }



           });

    });
```

<span data-ttu-id="42730-209">hello koden lägger till flera uppgifter toohello pool.</span><span class="sxs-lookup"><span data-stu-id="42730-209">hello code adds multiple tasks toohello pool.</span></span> <span data-ttu-id="42730-210">Och varje hello aktiviteter utförs på en nod i hello pool för virtuella datorer skapas.</span><span class="sxs-lookup"><span data-stu-id="42730-210">And each of hello tasks is executed on a node in hello pool of VMs created.</span></span> <span data-ttu-id="42730-211">Om hello antalet aktiviteter som överskrider hello antal virtuella datorer i en pool eller hello maxTasksPerNode egenskap, vänta hello uppgifter tills en nod har frigjorts.</span><span class="sxs-lookup"><span data-stu-id="42730-211">If hello number of tasks exceeds hello number of VMs in a pool or hello maxTasksPerNode property, hello tasks wait until a node is made available.</span></span> <span data-ttu-id="42730-212">Denna orkestrering hanteras automatiskt av Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="42730-212">This orchestration is handled by Azure Batch automatically.</span></span>

<span data-ttu-id="42730-213">hello-portalen har detaljerade vyer hello aktiviteter och jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="42730-213">hello portal has detailed views on hello tasks and job statuses.</span></span> <span data-ttu-id="42730-214">Du kan också använda hello lista och få funktioner i hello Azure nod SDK.</span><span class="sxs-lookup"><span data-stu-id="42730-214">You can also use hello list and get functions in hello Azure Node SDK.</span></span> <span data-ttu-id="42730-215">Information finns i dokumentationen för hello [länk](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).</span><span class="sxs-lookup"><span data-stu-id="42730-215">Details are provided in hello documentation [link](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="42730-216">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="42730-216">Next steps</span></span>

- <span data-ttu-id="42730-217">Granska hello [översikt över Azure Batch funktioner](batch-api-basics.md) artikel som vi rekommenderar att om du är ny toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="42730-217">Review hello [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new toohello service.</span></span>
- <span data-ttu-id="42730-218">Se hello [Batch Node.js referens](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) tooexplore hello Batch-API.</span><span class="sxs-lookup"><span data-stu-id="42730-218">See hello [Batch Node.js reference](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) tooexplore hello Batch API.</span></span>

