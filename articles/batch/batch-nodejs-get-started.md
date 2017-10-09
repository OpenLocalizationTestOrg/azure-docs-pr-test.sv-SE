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
# <a name="get-started-with-batch-sdk-for-nodejs"></a>Kom igång med Batch SDK för Node.js

> [!div class="op_single_selector"]
> * [NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

Lär dig grunderna hello för att skapa en Batch-klient i Node.js med [Azure Batch Node.js SDK](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/). Vi går igenom ett scenario med ett batch-program, steg för steg, och utför sedan en konfigurering med en Node.js-klient.  

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har kunskaper om Node.js och att du är bekant med Linux. Det förutsätts även att du har en Azure-konto-installation med rättigheter toocreate Batch- och nätverksåtkomsttjänster.

Vi rekommenderar att läsa [teknisk översikt över Azure Batch](batch-technical-overview.md) innan du går igenom hello stegen som beskrivs i den här artikeln.

## <a name="hello-tutorial-scenario"></a>hello självstudiekursen scenario
Låt oss förstå hello batch arbetsflödet scenario. Vi har ett enkelt skript som skrivits i Python som hämtar alla csv-filer från ett Azure Blob storage-behållare och konverterar dem tooJSON. tooprocess flera lagring konto behållare parallellt kan vi distribuera hello skript som ett Azure Batch-jobb.

## <a name="azure-batch-architecture"></a>Azure Batch-arkitektur
hello visar följande diagram hur vi kan skala hello Python-skriptet med hjälp av Azure Batch och en Node.js-klient.

![Azure Batch-scenario](./media/batch-nodejs-get-started/BatchScenario.png)

Hej node.js klient distribuerar batchjobb med en jobbförberedelseuppgift (beskrivs i detalj senare) och en uppsättning uppgifter beroende på hello antal behållare i hello storage-konto. Du kan hämta hello skript från hello github-lagringsplatsen.

* [Node.js-klient](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/nodejs_batch_client_sample.js)
* [Förberedande aktivitet – kommandoskript](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/startup_prereq.sh)
* [Python csv tooJSON-processor](https://github.com/Azure/azure-batch-samples/blob/master/Node.js/GettingStarted/processcsv.py)

> [!TIP]
> hello Node.js-klienten i hello länk anges innehåller inte specifik kod toobe distribueras som en funktionsapp i Azure. Du kan se följande länkar för anvisningar toocreate en toohello.
> - [Skapa funktionsappar](../azure-functions/functions-create-first-azure-function.md)
> - [Skapa timerutlösare](../azure-functions/functions-bindings-timer.md)
>
>

## <a name="build-hello-application"></a>Skapa hello program

Låt oss Följ nu hello stegvis genom processen i Skapa hello Node.js-klienten:

### <a name="step-1-install-azure-batch-sdk"></a>Steg 1: Installera Azure Batch SDK

Du kan installera Azure Batch SDK för Node.js kommandot hello npm installation.

`npm install azure-batch`

Det här kommandot installerar hello senaste versionen av azure batch-nod SDK.

>[!Tip]
> I en app i Azure-funktion, kan du gå för ”Kudu” i hello Azure funktionen konsolinställningar fliken toorun hello npm installera kommandon. I det här fallet tooinstall Azure Batch-SDK för Node.js.
>
>

### <a name="step-2-create-an-azure-batch-account"></a>Steg 2: Skapa ett Azure Batch-konto

Du kan skapa från hello [Azure-portalen](batch-account-create-portal.md) eller från kommandoraden ([Powershell](batch-powershell-cmdlets-get-started.md) /[Azure cli](https://docs.microsoft.com/cli/azure/overview)).

Följande är hello kommandon toocreate en via Azure CLI.

Skapa en resursgrupp, hoppa över det här steget om du redan har en där du vill att toocreate hello Batch-kontot:

`az group create -n "<resource-group-name>" -l "<location>"`

Skapa ett Azure Batch-konto.

`az batch account create -l "<location>"  -g "<resource-group-name>" -n "<batch-account-name>"`

Varje Batch-konto har motsvarande åtkomstnycklar. Nycklarna är nödvändiga toocreate ytterligare resurser i Azure batch-kontot. En bra idé för produktionsmiljö är toouse Azure Key Vault toostore nycklarna. Du kan sedan skapa en tjänst huvudnamn för programmet hello. Med det här tjänstprogrammet huvudnamn hello kan skapa en OAuth-token tooaccess nycklar från hello nyckelvalvet.

`az batch account keys list -g "<resource-group-name>" -n "<batch-account-name>"`

Kopiera och spara hello viktiga toobe används i hello efterföljande steg.

### <a name="step-3-create-an-azure-batch-service-client"></a>Steg 3: Skapa en Azure Batch-tjänsteklient
Följande kodavsnitt först importerar hello azure batch Node.js-modulen och skapar sedan en Batch-tjänsten-klient. Du behöver toofirst skapa ett SharedKeyCredentials-objekt med hello Batch kontonyckel kopieras från hello föregående steg.

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

hello Azure Batch-URI kan hittas i hello översiktsflik av hello Azure-portalen. Det har hello format:

`https://accountname.location.batch.azure.com`

Se toohello skärmbild:

![URI för Azure Batch](./media/batch-nodejs-get-started/azurebatchuri.png)



### <a name="step-4-create-an-azure-batch-pool"></a>Steg 4: Skapa en Azure Batch-pool
En Azure Batch-pool består av flera virtuella datorer (även kallade batchnoder). Azure Batch-tjänsten distribuerar hello uppgifter på dessa noder och hanterar dem.. Du kan definiera hello följande konfigurationsparametrar för din pool.

* Typ av virtuell datoravbildning
* Storlek på de virtuella datornoderna
* Antal virtuella datornoder

> [!Tip]
> hello storlek och antalet virtuella noder beror i stort sett på hello antalet aktiviteter som du vill toorun i parallell och även själva hello-aktiviteten. Vi rekommenderar att du testar toodetermine hello bästa antal och storlek.
>
>

hello skapar nedanstående kodutdrag hello parametern konfigurationsobjekt.

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
> Hello lista över Linux VM-avbildningar som är tillgängliga för Azure Batch och deras SKU-ID, se [lista över virtuella datoravbildningar](batch-linux-nodes.md#list-of-virtual-machine-images).
>
>

Du kan skapa hello Azure Batch-pool när hello poolen konfiguration har definierats. hello Batch-pool-kommando skapar en virtuell dator i Azure-noder och förbereda dem toobe klar tooreceive uppgifter tooexecute. I alla efterföljande steg ska det finnas ett unikt referens-ID.

följande kodstycke hello skapar en Azure Batch-pool.

```nodejs
// Create a unique Azure Batch pool ID
var poolid = "pool" + customerDetails.customerid;
var poolConfig = {id:poolid, displayName:poolid,vmSize:vmSize,virtualMachineConfiguration:vmconfig,targetDedicatedComputeNodes:numVms,enableAutoScale:false };
// Creating hello Pool for hello specific customer
var pool = batch_client.pool.add(poolConfig,function(error,result){
    if(error!=null){console.log(error.response)};
});
```

Du kan kontrollera hello status för hello poolen skapats och att hello är i tillståndet ”aktiv” innan du går vidare med överföring av en jobbet toothat pool.

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

Följande är ett exempel resultatet-objekt som returnerats av hello pool.get.

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


### <a name="step-4-submit-an-azure-batch-job"></a>Steg 4: Skicka ett Azure Batch-jobb
Azure Batch-jobbet består av en logisk grupp av snarlika uppgifter. I vårt scenario är det ”processen csv tooJSON”. Varje aktivitet här kan bearbeta de CSV-filer som finns i respektive Azure Storage-behållare.

Dessa aktiviteter körs parallellt och distribueras över flera noder, styrd av hello Azure Batch-tjänsten.

> [!Tip]
> Du kan använda hello [maxTasksPerNode](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) egenskapen toospecify högsta antalet uppgifter som kan köras samtidigt på en enda nod.
>
>

#### <a name="preparation-task"></a>Förberedande aktivitet

hello VM noder som skapats är tomt Ubuntu-noder. Du behöver ofta tooinstall en uppsättning program som krav.
Du kan normalt ha ett kommandoskript som installerar hello krav innan du hello faktiska aktiviteter som körs för Linux-noder. Det kan röra sig om vilka körbara filer som helst.
Hej [shell skriptet](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/startup_prereq.sh) i det här exemplet installerar Python pip och hello Azure Storage SDK för Python.

Du kan ladda upp hello skriptet på ett Azure Storage-konto och generera ett SAS-URI tooaccess hello skript. Den här processen kan också automatiseras med hjälp av hello Azure Storage Node.js SDK.

> [!Tip]
> En jobbförberedelseuppgift för ett jobb körs bara på hello VM noder där hello viss uppgift måste toorun. Om du vill krav toobe installerad på alla noder oavsett hello aktiviteter som körs på den, du kan använda hello [startuppgift har ställts](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Pool.html#add) egenskapen när du lägger till en pool. Du kan använda hello följande förberedelser aktivitetsdefinitionen för referens.
>
>

En jobbförberedelseuppgift har angetts under hello överföringen av Azure Batch-jobbet. Följande är hello konfigurationsparametrar för förberedelse av aktivitet:

* **ID**: en unik identifierare för hello jobbförberedelseuppgift
* **commandLine**: kommandoraden tooexecute hello aktiviteten
* **resourceFiles**: matris med-objekt som innehåller information om filer som behövs toobe hämtas för den här uppgiften toorun.  Här visas alternativen
    - blobSource: hello hello filen SAS-URI
    - filePath: lokal sökväg toodownload och spara hello-filen
    - fileMode: fileMode har ett oktalt format med standardvärdet 0770 (gäller endast Linux-noder).
* **waitForSuccess**: om set tootrue, hello aktiviteten inte körs på misslyckade aktiviteter för förberedelser
* **runElevated**: Ange den tootrue om utökade privilegier krävs toorun hello aktivitet.

Följande kodavsnitt visar konfiguration för hello förberedelse uppgiften skriptexempel:

```nodejs
var job_prep_task_config = {id:"installprereq",commandLine:"sudo sh startup_prereq.sh > startup.log",resourceFiles:[{'blobSource':'Blob SAS URI','filePath':'startup_prereq.sh'}],waitForSuccess:true,runElevated:true}
```

Om det finns inga krav toobe som installerats för dina uppgifter toorun, kan du hoppa över hello förberedande uppgifter. Följande kod skapar ett jobb med visningsnamnet ”process csv files” (bearbeta CSV-filer).

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


### <a name="step-5-submit-azure-batch-tasks-for-a-job"></a>Steg 5: Skicka Azure Batch-aktiviteter för ett jobb

Nu när vi har skapat ett jobb för bearbetning av CSV-filer kan vi börja skapa aktiviteter för jobbet i fråga. Under förutsättning att vi har fyra behållare, har vi toocreate fyra aktiviteter, en för varje behållare.

Om vi tittar på hello [Python-skriptet](https://github.com/shwetams/azure-batchclient-sample-nodejs/blob/master/processcsv.py), den accepterar två parametrar:

* behållarnamn: hello lagringsfilerna behållaren toodownload från
* pattern: En valfri parameter för filnamnsmönster

Anta att vi har fyra behållare ”con1”, ”con2”, ”con3” och kod ”con4” följande visar skickar för uppgifter toohello Azure batch-jobbet ”processen csv” vi skapade tidigare.

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

hello koden lägger till flera uppgifter toohello pool. Och varje hello aktiviteter utförs på en nod i hello pool för virtuella datorer skapas. Om hello antalet aktiviteter som överskrider hello antal virtuella datorer i en pool eller hello maxTasksPerNode egenskap, vänta hello uppgifter tills en nod har frigjorts. Denna orkestrering hanteras automatiskt av Azure Batch.

hello-portalen har detaljerade vyer hello aktiviteter och jobbstatus. Du kan också använda hello lista och få funktioner i hello Azure nod SDK. Information finns i dokumentationen för hello [länk](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/Job.html).

## <a name="next-steps"></a>Nästa steg

- Granska hello [översikt över Azure Batch funktioner](batch-api-basics.md) artikel som vi rekommenderar att om du är ny toohello tjänst.
- Se hello [Batch Node.js referens](http://azure.github.io/azure-sdk-for-node/azure-batch/latest/) tooexplore hello Batch-API.

