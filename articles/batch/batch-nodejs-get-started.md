---
title: "Självstudiekurs – Använda Azure Batch-klientbiblioteket för Node.js | Microsoft Docs"
description: "Lär dig de grundläggande principerna för Azure Batch och skapa en enkel lösning med Node.js."
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
ms.openlocfilehash: c48171d8634a651718a0775183414f463c6a468c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-batch-sdk-for-nodejs"></a><span data-ttu-id="cca3e-103">Kom igång med Batch SDK för Node.js</span><span class="sxs-lookup"><span data-stu-id="cca3e-103">Get started with Batch SDK for Node.js</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="cca3e-104">NET</span><span class="sxs-lookup"><span data-stu-id="cca3e-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="cca3e-105">Python</span><span class="sxs-lookup"><span data-stu-id="cca3e-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="cca3e-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="cca3e-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="cca3e-107">Lär dig grunderna i att bygga en Batch-klient i Node.js med [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/).</span><span class="sxs-lookup"><span data-stu-id="cca3e-107">Learn the basics of building a Batch client in Node.js using [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/).</span></span> <span data-ttu-id="cca3e-108">Vi går igenom ett scenario med ett batch-program, steg för steg, och utför sedan en konfigurering med en Node.js-klient.</span><span class="sxs-lookup"><span data-stu-id="cca3e-108">We take a step by step approach of understanding a scenario for a batch application and then setting it up using a Node.js client.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="cca3e-109">Krav</span><span class="sxs-lookup"><span data-stu-id="cca3e-109">Prerequisites</span></span>
<span data-ttu-id="cca3e-110">Den här artikeln förutsätter att du har kunskaper om Node.js och att du är bekant med Linux.</span><span class="sxs-lookup"><span data-stu-id="cca3e-110">This article assumes that you have a working knowledge of Node.js and familiarity with Linux.</span></span> <span data-ttu-id="cca3e-111">Den förutsätter också att du har ett Azure-konto med behörighet att skapa batch- och lagringstjänster.</span><span class="sxs-lookup"><span data-stu-id="cca3e-111">It also assumes that you have an Azure account setup with access rights to create Batch and Storage services.</span></span>

<span data-ttu-id="cca3e-112">Vi rekommenderar att du läser [Azure Batch, teknisk översikt](batch-technical-overview.md) innan du går igenom stegen som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="cca3e-112">We recommend reading [Azure Batch Technical Overview](batch-technical-overview.md) before you go through the steps outlined this article.</span></span>

## <a name="the-tutorial-scenario"></a><span data-ttu-id="cca3e-113">Självstudiescenario</span><span class="sxs-lookup"><span data-stu-id="cca3e-113">The tutorial scenario</span></span>
<span data-ttu-id="cca3e-114">Vi börjar med att gå igenom själva scenariot för batch-arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="cca3e-114">Let us understand the batch workflow scenario.</span></span> <span data-ttu-id="cca3e-115">Vi har ett enkelt skript skrivet i Python som laddar ned alla CSV-filer från en Azure Blob Storage-behållare och konverterar dem till JSON-format.</span><span class="sxs-lookup"><span data-stu-id="cca3e-115">We have a simple script written in Python that downloads all csv files from an Azure Blob storage container and converts them to JSON.</span></span> <span data-ttu-id="cca3e-116">Om du vill bearbeta flera Storage- kontobehållare parallellt med varandra kan vi distribuera skriptet som ett Azure Batch-jobb.</span><span class="sxs-lookup"><span data-stu-id="cca3e-116">To process multiple storage account containers in parallel, we can deploy the script as an Azure Batch job.</span></span>

## <a name="azure-batch-architecture"></a><span data-ttu-id="cca3e-117">Azure Batch-arkitektur</span><span class="sxs-lookup"><span data-stu-id="cca3e-117">Azure Batch Architecture</span></span>
<span data-ttu-id="cca3e-118">Följande diagram visar hur vi kan skala Python-skriptet med Azure Batch och en Node.js-klient.</span><span class="sxs-lookup"><span data-stu-id="cca3e-118">The following diagram depicts how we can scale the Python script using Azure Batch and a Node.js client.</span></span>

![Azure Batch-scenario](./media/batch-nodejs-get-started/BatchScenario.png)

<span data-ttu-id="cca3e-120">Node.js-klienten distribuerar ett batch-jobb med en förberedande aktivitet (beskrivs i detalj senare) och en uppsättning aktiviteter beroende på antalet behållare i lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="cca3e-120">The node.js client deploys a batch job with a preparation task (explained in detail later) and a set of tasks depending on the number of containers in the storage account.</span></span> <span data-ttu-id="cca3e-121">Du kan ladda ned skripten från GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="cca3e-121">You can download the scripts from the github repository.</span></span>

* [<span data-ttu-id="cca3e-122">Node.js-klient</span><span class="sxs-lookup"><span data-stu-id="cca3e-122">Node.js client</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/nodejs_batch_client_sample.js)
* [<span data-ttu-id="cca3e-123">Förberedande aktivitet – kommandoskript</span><span class="sxs-lookup"><span data-stu-id="cca3e-123">Preparation task shell scripts</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/startup_prereq.sh)
* [<span data-ttu-id="cca3e-124">Processor för konvertering från CSV-format (Python) till JSON-format</span><span class="sxs-lookup"><span data-stu-id="cca3e-124">Python csv to JSON processor</span></span>](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/processcsv.py)

> [!TIP]
> <span data-ttu-id="cca3e-125">Node.js-klienten i den angivna länken innehåller ingen kod som kan distribueras som en Azure-funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="cca3e-125">The Node.js client in the link specified does not contain specific code to be deployed as an Azure function app.</span></span> <span data-ttu-id="cca3e-126">Se följande länkar för att få anvisningar om hur du skapar en sådan.</span><span class="sxs-lookup"><span data-stu-id="cca3e-126">You can refer to the following links for instructions to create one.</span></span>
> - [<span data-ttu-id="cca3e-127">Skapa funktionsappar</span><span class="sxs-lookup"><span data-stu-id="cca3e-127">Create function app</span></span>](../azure-functions/functions-create-first-azure-function.md)
> - [<span data-ttu-id="cca3e-128">Skapa timerutlösare</span><span class="sxs-lookup"><span data-stu-id="cca3e-128">Create timer trigger function</span></span>](../azure-functions/functions-bindings-timer.md)
>
>

## <a name="build-the-application"></a><span data-ttu-id="cca3e-129">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="cca3e-129">Build the application</span></span>

<span data-ttu-id="cca3e-130">Nu tittar vi närmare på hur man bygger Node.js-klienten, steg för steg:</span><span class="sxs-lookup"><span data-stu-id="cca3e-130">Now, let us follow the process step by step into building the Node.js client:</span></span>

### <a name="step-1-install-azure-batch-sdk"></a><span data-ttu-id="cca3e-131">Steg 1: Installera Azure Batch SDK</span><span class="sxs-lookup"><span data-stu-id="cca3e-131">Step 1: Install Azure Batch SDK</span></span>

<span data-ttu-id="cca3e-132">Du kan installera Azure Batch SDK för Node.js med hjälp av installationskommandot npm.</span><span class="sxs-lookup"><span data-stu-id="cca3e-132">You can install Azure Batch SDK for Node.js using the npm install command.</span></span>

`npm install azure-batch`

<span data-ttu-id="cca3e-133">Med hjälp av det här kommandot installerar du den senaste versionen av azure-batch node SDK.</span><span class="sxs-lookup"><span data-stu-id="cca3e-133">This command installs the latest version of azure-batch node SDK.</span></span>

>[!Tip]
> <span data-ttu-id="cca3e-134">Om du använder en Azure-funktionsapp går du till Kudu-konsolen på Azure-funktionens inställningsflik för att köra installationskommandot npm.</span><span class="sxs-lookup"><span data-stu-id="cca3e-134">In an Azure Function app, you can go to "Kudu Console" in the Azure function's Settings tab to run the npm install commands.</span></span> <span data-ttu-id="cca3e-135">I det här fallet är syftet att installera Azure Batch SDK för Node.js.</span><span class="sxs-lookup"><span data-stu-id="cca3e-135">In this case to install Azure Batch SDK for Node.js.</span></span>
>
>

### <a name="step-2-create-an-azure-batch-account"></a><span data-ttu-id="cca3e-136">Steg 2: Skapa ett Azure Batch-konto</span><span class="sxs-lookup"><span data-stu-id="cca3e-136">Step 2: Create an Azure Batch account</span></span>

<span data-ttu-id="cca3e-137">Du kan skapa ett konto i [Azure Portal](batch-account-create-portal.md) eller från kommandoraden ([Powershell](batch-powershell-cmdlets-get-started.md) /[Azure cli](https://docs.microsoft.com/cli/azure/overview)).</span><span class="sxs-lookup"><span data-stu-id="cca3e-137">You can create it from the [Azure portal](batch-account-create-portal.md) or from command line ([Powershell](batch-powershell-cmdlets-get-started.md) /[Azure cli](https://docs.microsoft.com/cli/azure/overview)).</span></span>

<span data-ttu-id="cca3e-138">Nedan beskrivs kommandon som kan användas för att skapa ett sådant med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="cca3e-138">Following are the commands to create one through Azure CLI.</span></span>

<span data-ttu-id="cca3e-139">Skapa en resursgrupp. Hoppa över det här steget om du redan har en på den plats där du vill skapa ett Batch-konto:</span><span class="sxs-lookup"><span data-stu-id="cca3e-139">Create a Resource Group, skip this step if you already have one where you want to create the Batch Account:</span></span>

`az group create -n "<resource-group-name>" -l "<location>"`

<span data-ttu-id="cca3e-140">Skapa ett Azure Batch-konto.</span><span class="sxs-lookup"><span data-stu-id="cca3e-140">Next, create an Azure Batch account.</span></span>

`az batch account create -l "<location>"  -g "<resource-group-name>" -n "<batch-account-name>"`

<span data-ttu-id="cca3e-141">Varje Batch-konto har motsvarande åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="cca3e-141">Each Batch account has its corresponding access keys.</span></span> <span data-ttu-id="cca3e-142">Dessa nycklar behövs för att skapa fler resurser i Azure Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="cca3e-142">These keys are needed to create further resources in Azure batch account.</span></span> <span data-ttu-id="cca3e-143">Om du arbetar med en produktionsmiljö är det en bra idé att lagra nycklarna i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cca3e-143">A good practice for production environment is to use Azure Key Vault to store these keys.</span></span> <span data-ttu-id="cca3e-144">Du kan sedan skapa ett huvudnamn för tjänsten för programmet.</span><span class="sxs-lookup"><span data-stu-id="cca3e-144">You can then create a Service principal for the application.</span></span> <span data-ttu-id="cca3e-145">Med den här tjänstens huvudnamn kan programmet skapa en OAuth-token för att komma åt åtkomstnycklarna i Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cca3e-145">Using this service principal the application can create an OAuth token to access keys from the key vault.</span></span>

`az batch account keys list -g "<resource-group-name>" -n "<batch-account-name>"`

<span data-ttu-id="cca3e-146">Kopiera och lagra nyckeln som ska användas i efterföljande steg.</span><span class="sxs-lookup"><span data-stu-id="cca3e-146">Copy and store the key to be used in the subsequent steps.</span></span>

### <a name="step-3-create-an-azure-batch-service-client"></a><span data-ttu-id="cca3e-147">Steg 3: Skapa en Azure Batch-tjänsteklient</span><span class="sxs-lookup"><span data-stu-id="cca3e-147">Step 3: Create an Azure Batch service client</span></span>
<span data-ttu-id="cca3e-148">Följande kodfragment importerar först azure-batch Node.js-modulen och skapar sedan en Batch-tjänsteklient.</span><span class="sxs-lookup"><span data-stu-id="cca3e-148">Following code snippet first imports the azure-batch Node.js module and then creates a Batch Service client.</span></span> <span data-ttu-id="cca3e-149">Du måste först skapa ett SharedKeyCredentials-objekt med hjälp av den nyckel för Batch-kontot som kopierades i det föregående steget.</span><span class="sxs-lookup"><span data-stu-id="cca3e-149">You need to first create a SharedKeyCredentials object with the Batch account key copied from the previous step.</span></span>

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

<span data-ttu-id="cca3e-150">URI:en för Azure Batch återfinns på översiktsfliken i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="cca3e-150">The Azure Batch URI can be found in the Overview tab of the Azure portal.</span></span> <span data-ttu-id="cca3e-151">Formatet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="cca3e-151">It is of the format:</span></span>

`https://accountname.location.batch.azure.com`

<span data-ttu-id="cca3e-152">Se skärmbilden:</span><span class="sxs-lookup"><span data-stu-id="cca3e-152">Refer to the screenshot:</span></span>

![URI för Azure Batch](./media/batch-nodejs-get-started/azurebatchuri.png)



### <a name="step-4-create-an-azure-batch-pool"></a><span data-ttu-id="cca3e-154">Steg 4: Skapa en Azure Batch-pool</span><span class="sxs-lookup"><span data-stu-id="cca3e-154">Step 4: Create an Azure Batch pool</span></span>
<span data-ttu-id="cca3e-155">En Azure Batch-pool består av flera virtuella datorer (även kallade batchnoder).</span><span class="sxs-lookup"><span data-stu-id="cca3e-155">An Azure Batch pool consists of multiple VMs (also known as Batch Nodes).</span></span> <span data-ttu-id="cca3e-156">Azure Batch-tjänsten distribuerar aktiviteterna på noderna och hanterar dem.</span><span class="sxs-lookup"><span data-stu-id="cca3e-156">Azure Batch service deploys the tasks on these nodes and manages them.</span></span> <span data-ttu-id="cca3e-157">Följande konfigurationsparametrar kan definieras för din pool.</span><span class="sxs-lookup"><span data-stu-id="cca3e-157">You can define the following configuration parameters for your pool.</span></span>

* <span data-ttu-id="cca3e-158">Typ av virtuell datoravbildning</span><span class="sxs-lookup"><span data-stu-id="cca3e-158">Type of Virtual Machine image</span></span>
* <span data-ttu-id="cca3e-159">Storlek på de virtuella datornoderna</span><span class="sxs-lookup"><span data-stu-id="cca3e-159">Size of Virtual Machine nodes</span></span>
* <span data-ttu-id="cca3e-160">Antal virtuella datornoder</span><span class="sxs-lookup"><span data-stu-id="cca3e-160">Number of Virtual Machine nodes</span></span>

> [!Tip]
> <span data-ttu-id="cca3e-161">Storlekar och antalet virtuella noder beror huvudsakligen på antalet aktiviteter som du vill köra parallellt samt själva uppgiften som ska utföras.</span><span class="sxs-lookup"><span data-stu-id="cca3e-161">The size and number of Virtual Machine nodes largely depend on the number of tasks you want to run in parallel and also the task itself.</span></span> <span data-ttu-id="cca3e-162">Vi rekommenderar tester för att bäst kunna avgöra det bästa antalet och perfekta storlekar.</span><span class="sxs-lookup"><span data-stu-id="cca3e-162">We recommend testing to determine the ideal number and size.</span></span>
>
>

<span data-ttu-id="cca3e-163">Följande kodfragment skapar konfigurationsparameterobjekten.</span><span class="sxs-lookup"><span data-stu-id="cca3e-163">The following code snippet creates the configuration parameter objects.</span></span>

```nodejs
// Creating Image reference configuration for Ubuntu Linux VM
var imgRef = {publisher:"Canonical",offer:"UbuntuServer",sku:"14.04.2-LTS",version:"latest"}

// Creating the VM configuration object with the SKUID
var vmconfig = {imageReference:imgRef,nodeAgentSKUId:"batch.node.ubuntu 14.04"}

// Setting the VM size to Standard F4
var vmSize = "STANDARD_F4"

//Setting number of VMs in the pool to 4
var numVMs = 4
```

> [!Tip]
> <span data-ttu-id="cca3e-164">En lista över virtuella datoravbildningar med Linux och deras SKU ID:n finns i [Lista över virtuella datoravbildningar](batch-linux-nodes.md#list-of-virtual-machine-images).</span><span class="sxs-lookup"><span data-stu-id="cca3e-164">For the list of Linux VM images available for Azure Batch and their SKU IDs, see [List of virtual machine images](batch-linux-nodes.md#list-of-virtual-machine-images).</span></span>
>
>

<span data-ttu-id="cca3e-165">När poolkonfigurationen har definierats kan du skapa Azure Batch-poolen.</span><span class="sxs-lookup"><span data-stu-id="cca3e-165">Once the pool configuration is defined, you can create the Azure Batch pool.</span></span> <span data-ttu-id="cca3e-166">Batch-poolkommandot skapar virtuella Azure-datornoder och förbereder dem för att kunna ta emot och köra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="cca3e-166">The Batch pool command creates Azure Virtual Machine nodes and prepares them to be ready to receive tasks to execute.</span></span> <span data-ttu-id="cca3e-167">I alla efterföljande steg ska det finnas ett unikt referens-ID.</span><span class="sxs-lookup"><span data-stu-id="cca3e-167">Each pool should have a unique ID for reference in subsequent steps.</span></span>

<span data-ttu-id="cca3e-168">Använd följande kodfragment för att skapa en Azure Batch-pool.</span><span class="sxs-lookup"><span data-stu-id="cca3e-168">The following code snippet creates an Azure Batch pool.</span></span>

```nodejs
// Create a unique Azure Batch pool ID
var poolid = "pool" + customerDetails.customerid;
var poolConfig = {id:poolid, displayName:poolid,vmSize:vmSize,virtualMachineConfiguration:vmconfig,targetDedicatedComputeNodes:numVms,enableAutoScale:false };
// Creating the Pool for the specific customer
var pool = batch_client.pool.add(poolConfig,function(error,result){
    if(error!=null){console.log(error.response)};
});
```

<span data-ttu-id="cca3e-169">Kontrollera status för den pool som har skapats och försäkra dig om att den är i ”aktivt” tillstånd innan du går vidare med att skicka jobb till poolen i fråga.</span><span class="sxs-lookup"><span data-stu-id="cca3e-169">You can check the status of the pool created and ensure that the state is in "active" before going ahead with submission of a Job to that pool.</span></span>

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

<span data-ttu-id="cca3e-170">Följande är ett exempel på ett resultatobjekt som returnerats av funktionen pool.get.</span><span class="sxs-lookup"><span data-stu-id="cca3e-170">Following is a sample result object returned by the pool.get function.</span></span>

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


### <a name="step-4-submit-an-azure-batch-job"></a><span data-ttu-id="cca3e-171">Steg 4: Skicka ett Azure Batch-jobb</span><span class="sxs-lookup"><span data-stu-id="cca3e-171">Step 4: Submit an Azure Batch job</span></span>
<span data-ttu-id="cca3e-172">Azure Batch-jobbet består av en logisk grupp av snarlika uppgifter.</span><span class="sxs-lookup"><span data-stu-id="cca3e-172">An Azure Batch job is a logical group of similar tasks.</span></span> <span data-ttu-id="cca3e-173">I vårt exempel är det ”Process csv to JSON” (konvertering från CSV-format till JSON-format).</span><span class="sxs-lookup"><span data-stu-id="cca3e-173">In our scenario, it is "Process csv to JSON."</span></span> <span data-ttu-id="cca3e-174">Varje aktivitet här kan bearbeta de CSV-filer som finns i respektive Azure Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="cca3e-174">Each task here could be processing csv files present in each Azure Storage container.</span></span>

<span data-ttu-id="cca3e-175">Dessa uppgifter körs parallellt och distribueras över flera noder, och allt detta samordnas av Azure Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cca3e-175">These tasks would run in parallel and deployed across multiple nodes, orchestrated by the Azure Batch service.</span></span>

> [!Tip]
> <span data-ttu-id="cca3e-176">Du kan använda egenskapen [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) för att ange högsta antal aktiviteter som kan köras samtidigt på en enda nod.</span><span class="sxs-lookup"><span data-stu-id="cca3e-176">You can use the [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) property to specify maximum number of tasks that can run concurrently on a single node.</span></span>
>
>

#### <a name="preparation-task"></a><span data-ttu-id="cca3e-177">Förberedande aktivitet</span><span class="sxs-lookup"><span data-stu-id="cca3e-177">Preparation task</span></span>

<span data-ttu-id="cca3e-178">De VM-noder som skapas är tomma Ubuntu-noder.</span><span class="sxs-lookup"><span data-stu-id="cca3e-178">The VM nodes created are blank Ubuntu nodes.</span></span> <span data-ttu-id="cca3e-179">Oftast måste du installera en obligatorisk uppsättning program.</span><span class="sxs-lookup"><span data-stu-id="cca3e-179">Often, you need to install a set of programs as prerequisites.</span></span>
<span data-ttu-id="cca3e-180">Om du använder Linux-noder har du normalt sett ett kommandoskript som installerar alla obligatoriska program innan de faktiska aktiviteterna körs.</span><span class="sxs-lookup"><span data-stu-id="cca3e-180">Typically, for Linux nodes you can have a shell script that installs the prerequisites before the actual tasks run.</span></span> <span data-ttu-id="cca3e-181">Det kan röra sig om vilka körbara filer som helst.</span><span class="sxs-lookup"><span data-stu-id="cca3e-181">However it could be any programmable executable.</span></span>
<span data-ttu-id="cca3e-182">[Kommandoskriptet](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) i det här exemplet installerar Python-pip och Azure Storage SDK för Python.</span><span class="sxs-lookup"><span data-stu-id="cca3e-182">The [shell script](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) in this example installs Python-pip and the Azure Storage SDK for Python.</span></span>

<span data-ttu-id="cca3e-183">Du kan ladda upp skriptet på Azure Storage-kontot och generera en SAS-URI för att komma åt skriptet.</span><span class="sxs-lookup"><span data-stu-id="cca3e-183">You can upload the script on an Azure Storage Account and generate a SAS URI to access the script.</span></span> <span data-ttu-id="cca3e-184">Den här processen kan också automatiseras med hjälp av Azure Storage Node.js SDK.</span><span class="sxs-lookup"><span data-stu-id="cca3e-184">This process can also be automated using the Azure Storage Node.js SDK.</span></span>

> [!Tip]
> <span data-ttu-id="cca3e-185">Förberedande aktiviteter för ett jobb kan endast köras på de virtuella datornoder där en viss aktivitet ska köras.</span><span class="sxs-lookup"><span data-stu-id="cca3e-185">A preparation task for a job runs only on the VM nodes where the specific task needs to run.</span></span> <span data-ttu-id="cca3e-186">Om du vill att de obligatoriska programmen ska installeras på alla noder, oavsett vilka aktiviteter som körs på dem, kan du använda egenskapen [startTask](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) när du lägger till poolen.</span><span class="sxs-lookup"><span data-stu-id="cca3e-186">If you want prerequisites to be installed on all nodes irrespective of the tasks that run on it, you can use the [startTask](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) property while adding a pool.</span></span> <span data-ttu-id="cca3e-187">Du kan använda följande definition för förberedande aktiviteter som referens.</span><span class="sxs-lookup"><span data-stu-id="cca3e-187">You can use the following preparation task definition for reference.</span></span>
>
>

<span data-ttu-id="cca3e-188">En förberedande aktivitet anges vid överföring av Azure Batch-jobbet.</span><span class="sxs-lookup"><span data-stu-id="cca3e-188">A preparation task is specified during the submission of Azure Batch job.</span></span> <span data-ttu-id="cca3e-189">Här följer konfigurationsparametrar för den förberedande aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="cca3e-189">Following are the preparation task configuration parameters:</span></span>

* <span data-ttu-id="cca3e-190">**ID**: En unik identifierare för den förberedande aktiviteten</span><span class="sxs-lookup"><span data-stu-id="cca3e-190">**ID**: A unique identifier for the preparation task</span></span>
* <span data-ttu-id="cca3e-191">**commandLine**: Den kommandorad som exekverar den körbara filen</span><span class="sxs-lookup"><span data-stu-id="cca3e-191">**commandLine**: Command line to execute the task executable</span></span>
* <span data-ttu-id="cca3e-192">**resourceFiles**: En uppsättning objekt som tillhandahåller detaljerad information om de filer som måste laddas ned innan aktiviteten kan köras.</span><span class="sxs-lookup"><span data-stu-id="cca3e-192">**resourceFiles**: Array of objects that provide details of files needed to be downloaded for this task to run.</span></span>  <span data-ttu-id="cca3e-193">Här visas alternativen</span><span class="sxs-lookup"><span data-stu-id="cca3e-193">Following are its options</span></span>
    - <span data-ttu-id="cca3e-194">blobSource: SAS-URI för filen.</span><span class="sxs-lookup"><span data-stu-id="cca3e-194">blobSource: The SAS URI of the file</span></span>
    - <span data-ttu-id="cca3e-195">filePath: Lokal sökväg för nedladdning och sparande av filen.</span><span class="sxs-lookup"><span data-stu-id="cca3e-195">filePath: Local path to download and save the file</span></span>
    - <span data-ttu-id="cca3e-196">fileMode: fileMode har ett oktalt format med standardvärdet 0770 (gäller endast Linux-noder).</span><span class="sxs-lookup"><span data-stu-id="cca3e-196">fileMode: Only applicable for Linux nodes, fileMode is in octal format with a default value of 0770</span></span>
* <span data-ttu-id="cca3e-197">**waitForSuccess**: Om värdet är satt till sant går det inte att köra aktiviteten om den förberedande aktiviteten misslyckas.</span><span class="sxs-lookup"><span data-stu-id="cca3e-197">**waitForSuccess**: If set to true, the task does not run on preparation task failures</span></span>
* <span data-ttu-id="cca3e-198">**runElevated**: Sätt värdet till sant om det krävs utökad behörighet för att få köra uppgiften.</span><span class="sxs-lookup"><span data-stu-id="cca3e-198">**runElevated**: Set it to true if elevated privileges are needed to run the task.</span></span>

<span data-ttu-id="cca3e-199">Följande kodfragment innehåller ett exempel på skriptkonfigurering för den förberedande aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="cca3e-199">Following code snippet shows the preparation task script configuration sample:</span></span>

```nodejs
var job_prep_task_config = {id:"installprereq",commandLine:"sudo sh startup_prereq.sh > startup.log",resourceFiles:[{'blobSource':'Blob SAS URI','filePath':'startup_prereq.sh'}],waitForSuccess:true,runElevated:true}
```

<span data-ttu-id="cca3e-200">Om det inte finns några obligatoriska program att installera före aktivitetskörningen kan du hoppa över de förberedande aktiviteterna.</span><span class="sxs-lookup"><span data-stu-id="cca3e-200">If there are no prerequisites to be installed for your tasks to run, you can skip the preparation tasks.</span></span> <span data-ttu-id="cca3e-201">Följande kod skapar ett jobb med visningsnamnet ”process csv files” (bearbeta CSV-filer).</span><span class="sxs-lookup"><span data-stu-id="cca3e-201">Following code creates a job with display name "process csv files."</span></span>

 ```nodejs
 // Setting up Batch pool configuration
 var pool_config = {poolId:poolid}
 // Setting up Job configuration along with preparation task
 var jobId = "processcsvjob"
 var job_config = {id:jobId,displayName:"process csv files",jobPreparationTask:job_prep_task_config,poolInfo:pool_config}
 // Adding Azure batch job to the pool
 var job = batch_client.job.add(job_config,function(error,result){
     if(error != null)
     {
         console.log("Error submitting job : " + error.response);
     }});
```


### <a name="step-5-submit-azure-batch-tasks-for-a-job"></a><span data-ttu-id="cca3e-202">Steg 5: Skicka Azure Batch-aktiviteter för ett jobb</span><span class="sxs-lookup"><span data-stu-id="cca3e-202">Step 5: Submit Azure Batch tasks for a job</span></span>

<span data-ttu-id="cca3e-203">Nu när vi har skapat ett jobb för bearbetning av CSV-filer kan vi börja skapa aktiviteter för jobbet i fråga.</span><span class="sxs-lookup"><span data-stu-id="cca3e-203">Now that our process csv job is created, let us create tasks for that job.</span></span> <span data-ttu-id="cca3e-204">Anta att vi har fyra behållare och vill skapa fyra aktiviteter – en för varje behållare.</span><span class="sxs-lookup"><span data-stu-id="cca3e-204">Assuming we have four containers, we have to create four tasks, one for each container.</span></span>

<span data-ttu-id="cca3e-205">Om vi tittar på [Python-skriptet](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py) så godtas två möjliga parametrar:</span><span class="sxs-lookup"><span data-stu-id="cca3e-205">If we look at the [Python script](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), it accepts two parameters:</span></span>

* <span data-ttu-id="cca3e-206">container name: Den Storage-behållare som du vill ladda ned filer från</span><span class="sxs-lookup"><span data-stu-id="cca3e-206">container name: The Storage container to download files from</span></span>
* <span data-ttu-id="cca3e-207">pattern: En valfri parameter för filnamnsmönster</span><span class="sxs-lookup"><span data-stu-id="cca3e-207">pattern: An optional parameter of file name pattern</span></span>

<span data-ttu-id="cca3e-208">Anta att vi har fyra behållare – ”con1”, ”con2”, ”con3” och ”con4”. Följande kod visar hur man skickar aktiviteter till Azure Batch-jobbet ”process csv” som vi skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="cca3e-208">Assuming we have four containers "con1", "con2", "con3","con4" following code shows submitting for tasks to the Azure batch job "process csv" we created earlier.</span></span>

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

<span data-ttu-id="cca3e-209">Koden lägger till flera aktiviteter i poolen.</span><span class="sxs-lookup"><span data-stu-id="cca3e-209">The code adds multiple tasks to the pool.</span></span> <span data-ttu-id="cca3e-210">Varje aktivitet körs på en nod i poolen med virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="cca3e-210">And each of the tasks is executed on a node in the pool of VMs created.</span></span> <span data-ttu-id="cca3e-211">Om antalet aktiviteter överskrider antalet virtuella datorer i en pool eller egenskapen maxTasksPerNode måste du vänta tills en nod blir ledig.</span><span class="sxs-lookup"><span data-stu-id="cca3e-211">If the number of tasks exceeds the number of VMs in a pool or the maxTasksPerNode property, the tasks wait until a node is made available.</span></span> <span data-ttu-id="cca3e-212">Denna orkestrering hanteras automatiskt av Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="cca3e-212">This orchestration is handled by Azure Batch automatically.</span></span>

<span data-ttu-id="cca3e-213">Portalen har detaljerade vyer för aktiviteter och jobbstatusar.</span><span class="sxs-lookup"><span data-stu-id="cca3e-213">The portal has detailed views on the tasks and job statuses.</span></span> <span data-ttu-id="cca3e-214">Du kan också använda listan och hämta funktioner från Azure Node SDK.</span><span class="sxs-lookup"><span data-stu-id="cca3e-214">You can also use the list and get functions in the Azure Node SDK.</span></span> <span data-ttu-id="cca3e-215">Detaljerad information kan fås via [dokumentationslänken](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).</span><span class="sxs-lookup"><span data-stu-id="cca3e-215">Details are provided in the documentation [link](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cca3e-216">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cca3e-216">Next steps</span></span>

- <span data-ttu-id="cca3e-217">Läs artikeln [Översikt över Azure Batch-funktioner](batch-api-basics.md), som vi  rekommenderar om du inte har använt tjänsten.</span><span class="sxs-lookup"><span data-stu-id="cca3e-217">Review the [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new to the service.</span></span>
- <span data-ttu-id="cca3e-218">Se [Batch Node.js-referens](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) om du vill utforska Batch API.</span><span class="sxs-lookup"><span data-stu-id="cca3e-218">See the [Batch Node.js reference](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) to explore the Batch API.</span></span>

