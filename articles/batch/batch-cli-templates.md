---
title: "aaaRun Azure Batch jobb slutpunkt till slutpunkt utan att skriva kod (förhandsversion) | Microsoft Docs"
description: "Skapa mallfilerna för hello Azure CLI toocreate Batch pooler, jobb och uppgifter."
services: batch
author: mscurrell
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: na
ms.topic: article
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: markscu
ms.openlocfilehash: c0434d09766451f6ba516efbad949834711ee819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a><span data-ttu-id="7ea30-103">Använda Azure Batch CLI-mallar och filöverföring (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="7ea30-103">Use Azure Batch CLI Templates and File Transfer (Preview)</span></span>

<span data-ttu-id="7ea30-104">Med hello Azure CLI är det möjligt toorun batchjobb utan att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="7ea30-104">Using hello Azure CLI it is possible toorun Batch jobs without writing code.</span></span>

<span data-ttu-id="7ea30-105">Mallfilerna kan skapas och används med hello Azure CLI som tillåter Batch pooler, jobb och uppgifter toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="7ea30-105">Template files can be created and used with hello Azure CLI that allow Batch pools, jobs, and tasks toobe created.</span></span> <span data-ttu-id="7ea30-106">Jobbet indatafiler kan lätt överföras till hello storage-konto som är associerade med hello Batch-konto och jobbet utdatafilerna hämtas.</span><span class="sxs-lookup"><span data-stu-id="7ea30-106">Job input files can be easily uploaded to hello storage account associated with hello Batch account and job output files downloaded.</span></span>

## <a name="overview"></a><span data-ttu-id="7ea30-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="7ea30-107">Overview</span></span>

<span data-ttu-id="7ea30-108">Ett tillägg toohello Azure CLI kan Batch toobe som används för slutpunkt till slutpunkt av användare som inte är utvecklare.</span><span class="sxs-lookup"><span data-stu-id="7ea30-108">An extension toohello Azure CLI enables Batch toobe used end-to-end by users who are not developers.</span></span> <span data-ttu-id="7ea30-109">Du kan skapa en pool, indata överförs, jobb och förknippade aktiviteter som skapats och hello resulterande utdata hämtas – ingen kod som krävs, hello CLI som används direkt eller integreras i skript.</span><span class="sxs-lookup"><span data-stu-id="7ea30-109">A pool can be created, input data uploaded, jobs and associated tasks created, and hello resulting output data downloaded – no code required, hello CLI being used directly or being integrated into scripts.</span></span>

<span data-ttu-id="7ea30-110">Batch-mallar som bygger på hello [befintliga Batch-stöd i hello Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) som tillåter JSON-filer toospecify egenskapsvärden för hello skapa pooler, projekt, uppgifter och andra objekt.</span><span class="sxs-lookup"><span data-stu-id="7ea30-110">Batch templates build on hello [existing Batch support in hello Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) that allows JSON files toospecify property values for hello creation of pools, jobs, tasks, and other items.</span></span> <span data-ttu-id="7ea30-111">Med Batch mallar hello följande funktioner har lagts till via vad du kan göra med hello JSON-filer:</span><span class="sxs-lookup"><span data-stu-id="7ea30-111">With Batch templates, hello following capabilities are added over what is possible with hello JSON files:</span></span>

-   <span data-ttu-id="7ea30-112">Du kan definiera parametrar.</span><span class="sxs-lookup"><span data-stu-id="7ea30-112">Parameters can be defined.</span></span> <span data-ttu-id="7ea30-113">Om hello mall används är endast hello parametervärden angivna toocreate hello objektet med andra objekt egenskapsvärden som anges i hello mallen brödtext.</span><span class="sxs-lookup"><span data-stu-id="7ea30-113">When hello template is used, only hello parameter values are specified toocreate hello item, with other item property values being specified in hello template body.</span></span> <span data-ttu-id="7ea30-114">En användare som förstår Batch och toobe för program som körs av Batch kan skapa mallar, ange pool, jobb och egenskapsvärden för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7ea30-114">A user who understands Batch and the applications toobe run by Batch can create templates, specifying pool, job, and task property values.</span></span> <span data-ttu-id="7ea30-115">En användare mindre känner till Batch och/eller program behöver bara toospecify hello värden för hello definierade parametrar.</span><span class="sxs-lookup"><span data-stu-id="7ea30-115">A user less familiar with Batch and/or the applications only needs toospecify hello values for hello defined parameters.</span></span>

-   <span data-ttu-id="7ea30-116">Jobbet uppgiften fabriker skapar du en eller flera uppgifter som är kopplad till ett jobb, undvika hello behöver för många uppgiften definitioner toobe skapas och avsevärt förenklad jobbet.</span><span class="sxs-lookup"><span data-stu-id="7ea30-116">Job task factories create one or more tasks associated with a job, avoiding hello need for many task definitions toobe created and significantly simplifying job submission.</span></span>


<span data-ttu-id="7ea30-117">Indata-filer måste toobe som angetts för jobb och utdata datafiler produceras ofta.</span><span class="sxs-lookup"><span data-stu-id="7ea30-117">Input data files need toobe supplied for jobs and output data files are often produced.</span></span> <span data-ttu-id="7ea30-118">Ett lagringskonto associeras som standard, med varje Batch-konto och filer kan vara enkelt överförda tooand från det här lagringskontot med hjälp av CLI med ingen kodning och inga autentiseringsuppgifter för lagring krävs.</span><span class="sxs-lookup"><span data-stu-id="7ea30-118">A storage account is associated, by default, with each Batch account and files can be easily transferred tooand from this storage account using the CLI, with no coding and no storage credentials required.</span></span>

<span data-ttu-id="7ea30-119">Till exempel [ffmpeg](http://ffmpeg.org/) är ett populärt program som bearbetar ljud- och bildfiler.</span><span class="sxs-lookup"><span data-stu-id="7ea30-119">For example, [ffmpeg](http://ffmpeg.org/) is a popular application that processes audio and video files.</span></span> <span data-ttu-id="7ea30-120">hello Azure Batch CLI kan vara används tooinvoke ffmpeg tootranscode källa videofiler toodifferent lösningar.</span><span class="sxs-lookup"><span data-stu-id="7ea30-120">hello Azure Batch CLI can be used tooinvoke ffmpeg tootranscode source video files toodifferent resolutions.</span></span>

-   <span data-ttu-id="7ea30-121">En pool mall skapas.</span><span class="sxs-lookup"><span data-stu-id="7ea30-121">A pool template is created.</span></span> <span data-ttu-id="7ea30-122">hello-användaren som skapar hello mallen vet hur toocall hello ffmpeg programmet och dess krav. de ange hello lämpliga OS, VM storlek, hur ffmpeg är installerat (från ett programpaket eller med Pakethanteraren, till exempel) och andra poolen egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="7ea30-122">hello user creating hello template knows how toocall hello ffmpeg application and its requirements; they specify hello appropriate OS, VM size, how ffmpeg is installed (from an application package or using a package manager, for example), and other pool property values.</span></span> <span data-ttu-id="7ea30-123">Parametrar skapas när hello mallen används endast hello programpools-id och antal virtuella datorer måste toobe anges.</span><span class="sxs-lookup"><span data-stu-id="7ea30-123">Parameters are created so when hello template is used, only hello pool id and number of VMs need toobe specified.</span></span>

-   <span data-ttu-id="7ea30-124">Ett jobb skapas.</span><span class="sxs-lookup"><span data-stu-id="7ea30-124">A job template is created.</span></span> <span data-ttu-id="7ea30-125">hello skapar hello användarmallen vet hur ffmpeg behov toobe anropas tootranscode källa video tooa annan upplösning och anger hello kommandorad för uppgift; de vet att det finns en mapp som innehåller hello källa videofiler, med en aktivitet som behövs per indatafilen.</span><span class="sxs-lookup"><span data-stu-id="7ea30-125">hello user creating hello template knows how ffmpeg needs toobe invoked tootranscode source video tooa different resolution and specifies hello task command line; they also know that there is a folder containing hello source video files, with a task required per input file.</span></span>

-   <span data-ttu-id="7ea30-126">En användare med en uppsättning videofiler tootranscode först skapar en pool med hello poolen mall, ange endast hello programpools-id och antal virtuella datorer som krävs.</span><span class="sxs-lookup"><span data-stu-id="7ea30-126">An end user with a set of video files tootranscode first creates a pool using hello pool template, specifying only hello pool id and number of VMs required.</span></span> <span data-ttu-id="7ea30-127">De kan sedan överföra hello källa filer tootranscode.</span><span class="sxs-lookup"><span data-stu-id="7ea30-127">They can then upload hello source files tootranscode.</span></span> <span data-ttu-id="7ea30-128">Ett jobb kan sedan skickas med hello jobbmallen anger endast hello programpools-id och platsen för källfiler för hello har överförts.</span><span class="sxs-lookup"><span data-stu-id="7ea30-128">A job can then be submitted using hello job template, specifying only hello pool id and location of hello source files uploaded.</span></span> <span data-ttu-id="7ea30-129">hello Batch-jobbet har skapats med en aktivitet per indatafilen som genereras.</span><span class="sxs-lookup"><span data-stu-id="7ea30-129">hello Batch job is created, with one task per input file being generated.</span></span> <span data-ttu-id="7ea30-130">Slutligen kan hello kodas utdatafilerna hämtas.</span><span class="sxs-lookup"><span data-stu-id="7ea30-130">Finally, hello transcoded output files can be download.</span></span>

## <a name="installation"></a><span data-ttu-id="7ea30-131">Installation</span><span class="sxs-lookup"><span data-stu-id="7ea30-131">Installation</span></span>

<span data-ttu-id="7ea30-132">hello mall och fil överföring funktioner kräver ett tillägg toobe installerad.</span><span class="sxs-lookup"><span data-stu-id="7ea30-132">hello template and file transfer capabilities require an extension toobe installed.</span></span>

<span data-ttu-id="7ea30-133">Anvisningar för hur tooinstall hello Azure CLI finns [installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="7ea30-133">For instructions on how tooinstall hello Azure CLI see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="7ea30-134">En gång hello Azure CLI har installerats, hello Batch kan installeras med följande CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="7ea30-134">Once hello Azure CLI has been installed, hello Batch extension can be installed using the following CLI command:</span></span>

```azurecli
az component update --add batch-extensions --allow-third-party
```

<span data-ttu-id="7ea30-135">Läs mer om hello Batch tillägget [Microsoft Azure Batch CLI-tillägg för Windows, Mac och Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).</span><span class="sxs-lookup"><span data-stu-id="7ea30-135">For more information about hello Batch extension, see [Microsoft Azure Batch CLI Extensions for Windows, Mac and Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).</span></span>

## <a name="templates"></a><span data-ttu-id="7ea30-136">Mallar</span><span class="sxs-lookup"><span data-stu-id="7ea30-136">Templates</span></span>

<span data-ttu-id="7ea30-137">hello Azure Batch CLI kan objekt, till exempel pooler, jobb och uppgifter toobe skapas genom att ange en JSON-fil som innehåller egenskapsnamn och värden.</span><span class="sxs-lookup"><span data-stu-id="7ea30-137">hello Azure Batch CLI allows items such as pools, jobs, and tasks toobe created by specifying a JSON file containing property names and values.</span></span> <span data-ttu-id="7ea30-138">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7ea30-138">For example:</span></span>

```azurecli
az batch pool create –-json-file AppPool.json
```

<span data-ttu-id="7ea30-139">Azure Batch-mallar är liknande tooAzure Resource Manager-mallar, funktionalitet och syntax.</span><span class="sxs-lookup"><span data-stu-id="7ea30-139">Azure Batch templates are similar tooAzure Resource Manager templates, in functionality and syntax.</span></span> <span data-ttu-id="7ea30-140">De är JSON-filer som innehåller egenskapsnamn och värden, men lägga till följande huvudkoncepten hello:</span><span class="sxs-lookup"><span data-stu-id="7ea30-140">They are JSON files that contain item property names and values, but add hello following main concepts:</span></span>

-   <span data-ttu-id="7ea30-141">**Parametrar**</span><span class="sxs-lookup"><span data-stu-id="7ea30-141">**Parameters**</span></span>

    -   <span data-ttu-id="7ea30-142">Tillåt egenskapen värden toobe anges i ett body-avsnitt med bara parametervärden som behöver toobe angavs när hello mall används.</span><span class="sxs-lookup"><span data-stu-id="7ea30-142">Allow property values toobe specified in a body section, with only parameter values needing toobe supplied when hello template is used.</span></span> <span data-ttu-id="7ea30-143">Till exempel kan hello klar definition för en pool placeras i hello brödtext och endast en parameter som definierats för pool-id endast en pool med id-sträng måste därför toobe angivna toocreate poolen.</span><span class="sxs-lookup"><span data-stu-id="7ea30-143">For example, hello complete definition for a pool could be placed in hello body and only one parameter defined for pool id; only a pool id string therefore needs toobe supplied toocreate a pool.</span></span>
        
    -   <span data-ttu-id="7ea30-144">hello mallen brödtext kan skapas av någon med kunskap om Batch och hello program toobe kör av Batch; endast värden för hello författare definierade parametrar måste anges om hello mall används.</span><span class="sxs-lookup"><span data-stu-id="7ea30-144">hello template body can be authored by someone with knowledge of Batch and hello applications toobe run by Batch; only values for hello author-defined parameters must be supplied when hello template is used.</span></span> <span data-ttu-id="7ea30-145">En användare utan hello djupgående Batch och/eller program knowledge kan därför använda mallarna.</span><span class="sxs-lookup"><span data-stu-id="7ea30-145">A user without hello in-depth Batch and/or application knowledge can therefore use the templates.</span></span>

-   <span data-ttu-id="7ea30-146">**Variabler**</span><span class="sxs-lookup"><span data-stu-id="7ea30-146">**Variables**</span></span>

    -   <span data-ttu-id="7ea30-147">Tillåt enkla eller avancerade parametern värden toobe anges i en enda plats och på en eller flera platser i hello mallen brödtext.</span><span class="sxs-lookup"><span data-stu-id="7ea30-147">Allow simple or complex parameter values toobe specified in one place and used in one or more places in hello template body.</span></span> <span data-ttu-id="7ea30-148">Variabler kan förenkla och minska hello storlek hello mallen samt göra den mer hanterbar genom att ha ett toochange platsegenskaper vars värde ändras.</span><span class="sxs-lookup"><span data-stu-id="7ea30-148">Variables can simplify and reduce hello size of hello template, as well as make it more maintainable by having one location toochange properties whose value may change.</span></span>

-   <span data-ttu-id="7ea30-149">**Konstruktioner på högre nivå**</span><span class="sxs-lookup"><span data-stu-id="7ea30-149">**Higher-level constructs**</span></span>

    -   <span data-ttu-id="7ea30-150">Vissa på högre nivå konstruktioner är tillgängliga i hello-mallen som inte ännu är tillgängliga i hello Batch-API: er.</span><span class="sxs-lookup"><span data-stu-id="7ea30-150">Some higher-level constructs are available in hello template that are not yet available in hello Batch APIs.</span></span> <span data-ttu-id="7ea30-151">Exempelvis kan en aktivitet fabrik definieras i en jobbmall för som skapar flera uppgifter för hello jobb med hjälp av en gemensam aktivitetsdefinition.</span><span class="sxs-lookup"><span data-stu-id="7ea30-151">For example, a task factory can be defined in a job template that creates multiple tasks for hello job using a common task definition.</span></span> <span data-ttu-id="7ea30-152">Dessa konstruktioner Undvik hello måste toocode för att dynamiskt skapa flera JSON-filer, till exempel en fil per aktivitet, samt skapa skript filer tooinstall program via en Pakethanteraren, t.ex.</span><span class="sxs-lookup"><span data-stu-id="7ea30-152">These constructs avoid hello need toocode to dynamically create multiple JSON files, such as one file per task, as well as create script files tooinstall applications via a package manager, for example.</span></span>

    -   <span data-ttu-id="7ea30-153">Vid en punkt, om tillämpligt, dessa konstruktioner kanske toothe Batch-tjänsten och finns tillgänglig i hello Batch-API: er, användargränssnitt och så vidare.</span><span class="sxs-lookup"><span data-stu-id="7ea30-153">At some point, where applicable, these constructs may be added toothe Batch service and available in hello Batch APIs, UIs, etc.</span></span>

### <a name="pool-templates"></a><span data-ttu-id="7ea30-154">Pool-mallar</span><span class="sxs-lookup"><span data-stu-id="7ea30-154">Pool templates</span></span>

<span data-ttu-id="7ea30-155">Dessutom stöds toohello standardmall funktioner för parametrar och variabler hello följande konstruktioner på högre nivå av hello poolen mallen:</span><span class="sxs-lookup"><span data-stu-id="7ea30-155">In addition toohello standard template capabilities of parameters and variables, hello following higher-level constructs are supported by hello pool template:</span></span>

-   <span data-ttu-id="7ea30-156">**Paketet refererar till**</span><span class="sxs-lookup"><span data-stu-id="7ea30-156">**Package references**</span></span>

    -   <span data-ttu-id="7ea30-157">Alternativt kan programvara toobe kopieras toopool noder med hjälp av paketet chefer.</span><span class="sxs-lookup"><span data-stu-id="7ea30-157">Optionally allows software toobe copied toopool nodes by using package managers.</span></span> <span data-ttu-id="7ea30-158">hello package manager och paket-id har angetts.</span><span class="sxs-lookup"><span data-stu-id="7ea30-158">hello package manager and package id are specified.</span></span> <span data-ttu-id="7ea30-159">Ett eller flera paket som kan toodeclare undviker hello måste toocreate ett skript som hämtar hello krävs paket, installera hello skript och kör hello skript på varje nod i poolen.</span><span class="sxs-lookup"><span data-stu-id="7ea30-159">Being able toodeclare one or more packages avoids hello need toocreate a script that gets hello required packages, install hello script, and run hello script on each pool node.</span></span>

<span data-ttu-id="7ea30-160">hello följande är ett exempel på en mall som skapar en pool av virtuella Linux-datorer med ffmpeg installerade och endast kräver en pool-id sträng och hello antal VMs toobe angivna toouse:</span><span class="sxs-lookup"><span data-stu-id="7ea30-160">hello following is an example of a template that creates a pool of Linux VMs with ffmpeg installed and only requires a pool id string and hello number of VMs toobe supplied toouse:</span></span>

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "hello number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello pool id "
            }
        }
    },
    "pool": {
        "type": "Microsoft.Batch/batchAccounts/pools",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('poolId')]",
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "Canonical",
                    "offer": "UbuntuServer",
                    "sku": "16.04.0-LTS",
                    "version": "latest"
                },
                "nodeAgentSKUId": "batch.node.ubuntu 16.04"
            },
            "vmSize": "STANDARD_D3_V2",
            "targetDedicatedNodes": "[parameters('nodeCount')]",
            "enableAutoScale": false,
            "maxTasksPerNode": 1,
            "packageReferences": [
                {
                    "type": "aptPackage",
                    "id": "ffmpeg"
                }
            ]
        }
    }
}
```

<span data-ttu-id="7ea30-161">Om hello mallfilen hette _pool ffmpeg.json_, och sedan hello mallen skulle anropas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7ea30-161">If hello template file was named _pool-ffmpeg.json_, then hello template would be invoked as follows:</span></span>

```azurecli
az batch pool create --template pool-ffmpeg.json
```

### <a name="job-templates"></a><span data-ttu-id="7ea30-162">Jobbmallar</span><span class="sxs-lookup"><span data-stu-id="7ea30-162">Job templates</span></span>

<span data-ttu-id="7ea30-163">Dessutom stöds toohello standardmall funktioner för parametrar och variabler hello följande konstruktioner på högre nivå av hello jobbmallen:</span><span class="sxs-lookup"><span data-stu-id="7ea30-163">In addition toohello standard template capabilities of parameters and variables, hello following higher-level constructs are supported by hello job template:</span></span>

-   <span data-ttu-id="7ea30-164">**Uppgiften fabriken**</span><span class="sxs-lookup"><span data-stu-id="7ea30-164">**Task factory**</span></span>

    -   <span data-ttu-id="7ea30-165">Skapar flera uppgifter för ett jobb från en aktivitetsdefinition.</span><span class="sxs-lookup"><span data-stu-id="7ea30-165">Creates multiple tasks for a job from one task definition.</span></span> <span data-ttu-id="7ea30-166">Tre typer av aktiviteten fabriken stöds – parametrisk Sopa aktivitet per fil och uppgift samling.</span><span class="sxs-lookup"><span data-stu-id="7ea30-166">Three types of task factory are supported – parametric sweep, task per file, and task collection.</span></span>

<span data-ttu-id="7ea30-167">hello följande är ett exempel på en mall som skapar ett jobb som använder ffmpeg till omkoda MP4-filer tooone av två lägre lösningar med en aktivitet som skapats per video källfilen:</span><span class="sxs-lookup"><span data-stu-id="7ea30-167">hello following is an example of a template that creates a job that uses ffmpeg to transcode MP4 video files tooone of two lower resolutions, with one task created per source video file:</span></span>

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch pool which runs hello job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch job"
            }
        },
        "resolution": {
            "type": "string",
            "defaultValue": "428x240",
            "allowedValues": [
                "428x240",
                "854x480"
            ],
            "metadata": {
                "description": "Target video resolution"
            }
        }
    },
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('jobId')]",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "[parameters('poolId')]"
            },
            "taskFactory": {
                "type": "taskPerFile",
                "source": { 
                    "fileGroup": "ffmpeg-input"
                },
                "repeatTask": {
                    "commandLine": "ffmpeg -i {fileName} -y -s [parameters('resolution')] -strict -2 {fileNameWithoutExtension}_[parameters('resolution')].mp4",
                    "resourceFiles": [
                        {
                            "blobSource": "{url}",
                            "filePath": "{fileName}"
                        }
                    ],
                    "outputFiles": [
                        {
                            "filePattern": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                            "destination": {
                                "autoStorage": {
                                    "path": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                                    "fileGroup": "ffmpeg-output"
                                }
                            },
                            "uploadOptions": {
                                "uploadCondition": "TaskSuccess"
                            }
                        }
                    ]
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

<span data-ttu-id="7ea30-168">Om hello mallfilen hette _jobbet ffmpeg.json_, och sedan hello mallen skulle anropas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="7ea30-168">If hello template file was named _job-ffmpeg.json_, then hello template would be invoked as follows:</span></span>

```azurecli
az batch job create --template job-ffmpeg.json
```

## <a name="file-groups-and-file-transfer"></a><span data-ttu-id="7ea30-169">Filgrupper och filöverföring</span><span class="sxs-lookup"><span data-stu-id="7ea30-169">File groups and file transfer</span></span>

<span data-ttu-id="7ea30-170">De flesta jobb och uppgifter kräver att inkommande filer och skapar utdatafilerna.</span><span class="sxs-lookup"><span data-stu-id="7ea30-170">Most jobs and tasks require input files and produce output files.</span></span> <span data-ttu-id="7ea30-171">Både inkommande filer och utdatafiler måste vanligtvis toobe överförs från hello klienten toohello nod eller från hello nod toohello klienten.</span><span class="sxs-lookup"><span data-stu-id="7ea30-171">Both input files and output files typically need toobe transferred, either from hello client toohello node, or from hello node toohello client.</span></span> <span data-ttu-id="7ea30-172">hello Azure Batch CLI tillägget avlägsnar bort filöverföring och använder hello storage-konto som skapas som standard för varje Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="7ea30-172">hello Azure Batch CLI extension abstracts away file transfer and utilizes hello storage account that is created by default for each Batch account.</span></span>

<span data-ttu-id="7ea30-173">En filgrupp är lika tooa behållare som skapas i hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7ea30-173">A file group equates tooa container that is created in hello Azure storage account.</span></span> <span data-ttu-id="7ea30-174">hello filgrupp kan ha undermappar.</span><span class="sxs-lookup"><span data-stu-id="7ea30-174">hello file group may have subfolders.</span></span>

<span data-ttu-id="7ea30-175">hello Batch CLI tillägget innehåller kommandon för att ladda upp filer från klientgrupp tooa angivna filen och hämta filer från hello angivna filen grupp tooa klient.</span><span class="sxs-lookup"><span data-stu-id="7ea30-175">hello Batch CLI extension provides commands for uploading files from client tooa specified file group and downloading files from hello specified file group tooa client.</span></span>

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

<span data-ttu-id="7ea30-176">Poolen och jobb med mallar kan filer som lagras i filen grupper toobe som angetts för kopia till poolen noder eller inaktivera poolen noder bakåt tooa filgrupp.</span><span class="sxs-lookup"><span data-stu-id="7ea30-176">Pool and job templates allow files stored in file groups toobe specified for copy onto pool nodes or off pool nodes back tooa file group.</span></span> <span data-ttu-id="7ea30-177">Till exempel i jobbmallen angav tidigare, har hello filen grupp ”ffmpeg inmatning” angetts för hello uppgiften factory som hello platsen för hello video källfilerna kopieras ned till hello noden för omkodning; hello filen grupp ”ffmpeg-utdata” används som hello platsen där hello kodas utdatafilerna kopieras toofrom hello nod som kör varje aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7ea30-177">For example, in the job template specified previously, hello file group “ffmpeg-input” is specified for hello task factory as hello location of hello source video files copied down onto hello node for transcoding; hello file group “ffmpeg-output” is used as hello location where hello transcoded output files are copied toofrom hello node running each task.</span></span>

## <a name="summary"></a><span data-ttu-id="7ea30-178">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7ea30-178">Summary</span></span>

<span data-ttu-id="7ea30-179">Stöd för överföring av mall och fil för närvarande lades till endast toohello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="7ea30-179">Template and file transfer support have currently been added only toohello Azure CLI.</span></span> <span data-ttu-id="7ea30-180">hello målet är tooexpand hello målgruppen som kan använda Batch toousers som inte behöver toodevelop kod med hello Batch-API: er, till exempel forskare, IT-användare och så vidare.</span><span class="sxs-lookup"><span data-stu-id="7ea30-180">hello goal is tooexpand hello audience that can use Batch toousers who do not need toodevelop code using hello Batch APIs, such as researchers, IT users, and so on.</span></span> <span data-ttu-id="7ea30-181">Utan kodning, kan användare med kunskaper om Azure Batch och hello program toobe kör av Batch skapa mallar för att skapa en pool och jobb.</span><span class="sxs-lookup"><span data-stu-id="7ea30-181">Without coding, users with knowledge of Azure, Batch, and hello applications toobe run by Batch can create templates for pool and job creation.</span></span> <span data-ttu-id="7ea30-182">Användare utan detaljerade kunskaper om Batch- och hello program kan använda hello mallar med mallparametrar.</span><span class="sxs-lookup"><span data-stu-id="7ea30-182">With template parameters, users without detailed knowledge of Batch and hello applications can use hello templates.</span></span>

<span data-ttu-id="7ea30-183">Prova hello Batch-tillägget för hello Azure CLI och ge oss feedback och förslag, antingen i hello kommentarer för den här artikeln eller via hello [Azure Batch-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).</span><span class="sxs-lookup"><span data-stu-id="7ea30-183">Try out hello Batch extension for hello Azure CLI and provide us with any feedback or suggestions, either in hello comments for this article or via hello [Azure Batch forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ea30-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ea30-184">Next steps</span></span>

- <span data-ttu-id="7ea30-185">Finns hello Batch mallar blogginlägg: [kör Azure Batch-jobb med hjälp av hello Azure CLI – ingen kod krävs](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).</span><span class="sxs-lookup"><span data-stu-id="7ea30-185">See hello Batch templates blog post: [Running Azure Batch jobs using hello Azure CLI – no code required](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).</span></span>
- <span data-ttu-id="7ea30-186">Utförlig information om installation och användning, exempel och källkoden är tillgängliga i hello [Azure GitHub-lagringsplatsen](https://github.com/Azure/azure-batch-cli-extensions).</span><span class="sxs-lookup"><span data-stu-id="7ea30-186">Detailed installation and usage documentation, samples, and source code are available in hello [Azure GitHub repository](https://github.com/Azure/azure-batch-cli-extensions).</span></span>
