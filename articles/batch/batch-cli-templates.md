---
title: "Kör Azure Batch jobb slutpunkt till slutpunkt utan att skriva kod (förhandsversion) | Microsoft Docs"
description: "Skapa mallfilerna för Azure CLI att skapa Batch pooler, jobb och uppgifter."
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
ms.openlocfilehash: 6b91466da46d1f4ca9f25bf1718be783603efc58
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a><span data-ttu-id="db06a-103">Använda Azure Batch CLI-mallar och filöverföring (förhandsversion)</span><span class="sxs-lookup"><span data-stu-id="db06a-103">Use Azure Batch CLI Templates and File Transfer (Preview)</span></span>

<span data-ttu-id="db06a-104">Med Azure CLI är det möjligt att köra batchjobb utan att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="db06a-104">Using the Azure CLI it is possible to run Batch jobs without writing code.</span></span>

<span data-ttu-id="db06a-105">Mallfilerna kan skapas och används med Azure CLI som tillåter Batch pooler, jobb och uppgifter som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="db06a-105">Template files can be created and used with the Azure CLI that allow Batch pools, jobs, and tasks to be created.</span></span> <span data-ttu-id="db06a-106">Jobbet indatafiler kan lätt överföras till storage-konto som är associerade med Batch-konto och jobbet utdatafilerna hämtas.</span><span class="sxs-lookup"><span data-stu-id="db06a-106">Job input files can be easily uploaded to the storage account associated with the Batch account and job output files downloaded.</span></span>

## <a name="overview"></a><span data-ttu-id="db06a-107">Översikt</span><span class="sxs-lookup"><span data-stu-id="db06a-107">Overview</span></span>

<span data-ttu-id="db06a-108">Ett tillägg till Azure CLI kan Batch ska använda slutpunkt till slutpunkt av användare som inte är utvecklare.</span><span class="sxs-lookup"><span data-stu-id="db06a-108">An extension to the Azure CLI enables Batch to be used end-to-end by users who are not developers.</span></span> <span data-ttu-id="db06a-109">Du kan skapa en pool, indata har överförts, jobb och förknippade aktiviteter som skapats och resulterande utdata hämtas – ingen kod krävs CLI som används direkt eller integreras i skript.</span><span class="sxs-lookup"><span data-stu-id="db06a-109">A pool can be created, input data uploaded, jobs and associated tasks created, and the resulting output data downloaded – no code required, the CLI being used directly or being integrated into scripts.</span></span>

<span data-ttu-id="db06a-110">Batch-mallar som bygger på den [befintliga Batch-stöd i Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) som gör det möjligt för JSON-filer att ange egenskapsvärden för att skapa pooler, projekt, uppgifter och andra objekt.</span><span class="sxs-lookup"><span data-stu-id="db06a-110">Batch templates build on the [existing Batch support in the Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) that allows JSON files to specify property values for the creation of pools, jobs, tasks, and other items.</span></span> <span data-ttu-id="db06a-111">Följande funktioner läggs över vad du kan göra med JSON-filer med Batch-mallar:</span><span class="sxs-lookup"><span data-stu-id="db06a-111">With Batch templates, the following capabilities are added over what is possible with the JSON files:</span></span>

-   <span data-ttu-id="db06a-112">Du kan definiera parametrar.</span><span class="sxs-lookup"><span data-stu-id="db06a-112">Parameters can be defined.</span></span> <span data-ttu-id="db06a-113">När mallen används anges bara parametervärden till att skapa objektet med andra objekt egenskapsvärden som anges i brödtexten i mallen.</span><span class="sxs-lookup"><span data-stu-id="db06a-113">When the template is used, only the parameter values are specified to create the item, with other item property values being specified in the template body.</span></span> <span data-ttu-id="db06a-114">En användare som förstår Batch och program som ska köras av Batch kan skapa mallar, ange pool, jobb och egenskapsvärden för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="db06a-114">A user who understands Batch and the applications to be run by Batch can create templates, specifying pool, job, and task property values.</span></span> <span data-ttu-id="db06a-115">En användare mindre känner till Batch och/eller program behöver bara ange värden för de definierade parametrarna.</span><span class="sxs-lookup"><span data-stu-id="db06a-115">A user less familiar with Batch and/or the applications only needs to specify the values for the defined parameters.</span></span>

-   <span data-ttu-id="db06a-116">Jobbet uppgiften fabriker skapa en eller flera uppgifter som är kopplad till ett jobb, vilket minskar behovet av att många definitioner för uppgiften att skapas och förenkla avsevärt jobbet.</span><span class="sxs-lookup"><span data-stu-id="db06a-116">Job task factories create one or more tasks associated with a job, avoiding the need for many task definitions to be created and significantly simplifying job submission.</span></span>


<span data-ttu-id="db06a-117">Indata-filer måste anges för jobb och utdata datafiler produceras ofta.</span><span class="sxs-lookup"><span data-stu-id="db06a-117">Input data files need to be supplied for jobs and output data files are often produced.</span></span> <span data-ttu-id="db06a-118">Ett lagringskonto associeras som standard med varje Batch-kontot och filer kan lätt överföras till och från det här lagringskontot med hjälp av CLI med ingen kodning och inga autentiseringsuppgifter för lagring krävs.</span><span class="sxs-lookup"><span data-stu-id="db06a-118">A storage account is associated, by default, with each Batch account and files can be easily transferred to and from this storage account using the CLI, with no coding and no storage credentials required.</span></span>

<span data-ttu-id="db06a-119">Till exempel [ffmpeg](http://ffmpeg.org/) är ett populärt program som bearbetar ljud- och bildfiler.</span><span class="sxs-lookup"><span data-stu-id="db06a-119">For example, [ffmpeg](http://ffmpeg.org/) is a popular application that processes audio and video files.</span></span> <span data-ttu-id="db06a-120">Azure Batch CLI kan användas för att anropa ffmpeg att kodas om käll-filer till olika lösningar.</span><span class="sxs-lookup"><span data-stu-id="db06a-120">The Azure Batch CLI can be used to invoke ffmpeg to transcode source video files to different resolutions.</span></span>

-   <span data-ttu-id="db06a-121">En pool mall skapas.</span><span class="sxs-lookup"><span data-stu-id="db06a-121">A pool template is created.</span></span> <span data-ttu-id="db06a-122">Användaren som skapar mallen vet hur du anropar ffmpeg programmet och dess krav. de ange lämpliga OS, VM storlek, hur ffmpeg är installerat (från ett programpaket eller med Pakethanteraren, till exempel) och andra poolen egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="db06a-122">The user creating the template knows how to call the ffmpeg application and its requirements; they specify the appropriate OS, VM size, how ffmpeg is installed (from an application package or using a package manager, for example), and other pool property values.</span></span> <span data-ttu-id="db06a-123">Parametrar skapas när mallen används endast programpools-id och antal virtuella datorer måste anges.</span><span class="sxs-lookup"><span data-stu-id="db06a-123">Parameters are created so when the template is used, only the pool id and number of VMs need to be specified.</span></span>

-   <span data-ttu-id="db06a-124">Ett jobb skapas.</span><span class="sxs-lookup"><span data-stu-id="db06a-124">A job template is created.</span></span> <span data-ttu-id="db06a-125">Användaren som skapar mallen vet hur ffmpeg måste anropas för att kodas om källan video till en annan upplösning och anger vilken kommandorad för uppgift; de vet att det finns en mapp som innehåller källan videofiler, med en aktivitet som behövs per indatafilen.</span><span class="sxs-lookup"><span data-stu-id="db06a-125">The user creating the template knows how ffmpeg needs to be invoked to transcode source video to a different resolution and specifies the task command line; they also know that there is a folder containing the source video files, with a task required per input file.</span></span>

-   <span data-ttu-id="db06a-126">En användare med en uppsättning videofiler ska kunna koda först skapar en pool med mallen pool, ange bara programpools-id och antal virtuella datorer som krävs.</span><span class="sxs-lookup"><span data-stu-id="db06a-126">An end user with a set of video files to transcode first creates a pool using the pool template, specifying only the pool id and number of VMs required.</span></span> <span data-ttu-id="db06a-127">De kan sedan överföra källfilerna ska kunna koda.</span><span class="sxs-lookup"><span data-stu-id="db06a-127">They can then upload the source files to transcode.</span></span> <span data-ttu-id="db06a-128">Ett jobb kan sedan skickas med jobbmallen att ange pool-id och platsen för källfiler som har överförts.</span><span class="sxs-lookup"><span data-stu-id="db06a-128">A job can then be submitted using the job template, specifying only the pool id and location of the source files uploaded.</span></span> <span data-ttu-id="db06a-129">Batch-jobbet har skapats med en aktivitet per indatafilen som genereras.</span><span class="sxs-lookup"><span data-stu-id="db06a-129">The Batch job is created, with one task per input file being generated.</span></span> <span data-ttu-id="db06a-130">Slutligen kan utdatafilerna kodas hämtas.</span><span class="sxs-lookup"><span data-stu-id="db06a-130">Finally, the transcoded output files can be download.</span></span>

## <a name="installation"></a><span data-ttu-id="db06a-131">Installation</span><span class="sxs-lookup"><span data-stu-id="db06a-131">Installation</span></span>

<span data-ttu-id="db06a-132">Mall och fil överföring funktionerna kräver ett tillägg som ska installeras.</span><span class="sxs-lookup"><span data-stu-id="db06a-132">The template and file transfer capabilities require an extension to be installed.</span></span>

<span data-ttu-id="db06a-133">Anvisningar om hur du installerar Azure CLI finns [installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="db06a-133">For instructions on how to install the Azure CLI see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="db06a-134">När du har installerat Azure CLI installeras Batch-tillägg med följande CLI-kommando:</span><span class="sxs-lookup"><span data-stu-id="db06a-134">Once the Azure CLI has been installed, the Batch extension can be installed using the following CLI command:</span></span>

```azurecli
az component update --add batch-extensions --allow-third-party
```

<span data-ttu-id="db06a-135">Mer information om Batch-tillägg finns [Microsoft Azure Batch CLI-tillägg för Windows, Mac och Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).</span><span class="sxs-lookup"><span data-stu-id="db06a-135">For more information about the Batch extension, see [Microsoft Azure Batch CLI Extensions for Windows, Mac and Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).</span></span>

## <a name="templates"></a><span data-ttu-id="db06a-136">Mallar</span><span class="sxs-lookup"><span data-stu-id="db06a-136">Templates</span></span>

<span data-ttu-id="db06a-137">Azure Batch CLI kan objekt, till exempel pooler, jobb och uppgifter som ska skapas genom att ange en JSON-fil som innehåller egenskapsnamn och värden.</span><span class="sxs-lookup"><span data-stu-id="db06a-137">The Azure Batch CLI allows items such as pools, jobs, and tasks to be created by specifying a JSON file containing property names and values.</span></span> <span data-ttu-id="db06a-138">Exempel:</span><span class="sxs-lookup"><span data-stu-id="db06a-138">For example:</span></span>

```azurecli
az batch pool create –-json-file AppPool.json
```

<span data-ttu-id="db06a-139">Azure Batch-mallar liknar Azure Resource Manager-mallar i funktioner och syntax.</span><span class="sxs-lookup"><span data-stu-id="db06a-139">Azure Batch templates are similar to Azure Resource Manager templates, in functionality and syntax.</span></span> <span data-ttu-id="db06a-140">De är JSON-filer som innehåller egenskapsnamn och värden, men lägga till följande huvudkoncepten:</span><span class="sxs-lookup"><span data-stu-id="db06a-140">They are JSON files that contain item property names and values, but add the following main concepts:</span></span>

-   <span data-ttu-id="db06a-141">**Parametrar**</span><span class="sxs-lookup"><span data-stu-id="db06a-141">**Parameters**</span></span>

    -   <span data-ttu-id="db06a-142">Tillåt egenskapsvärden anges i ett body-avsnitt med bara parametervärden som ska anges när mallen används.</span><span class="sxs-lookup"><span data-stu-id="db06a-142">Allow property values to be specified in a body section, with only parameter values needing to be supplied when the template is used.</span></span> <span data-ttu-id="db06a-143">Till exempel kan fullständig definitionen för en pool placeras i brödtexten och endast en parameter som definierats för pool-id endast en pool med id-sträng måste därför anges för att skapa en pool.</span><span class="sxs-lookup"><span data-stu-id="db06a-143">For example, the complete definition for a pool could be placed in the body and only one parameter defined for pool id; only a pool id string therefore needs to be supplied to create a pool.</span></span>
        
    -   <span data-ttu-id="db06a-144">Brödtexten i mallen kan skapas av någon med kunskap om Batch och program som ska köras av Batch; endast värden för parametrarna författare definierats måste anges när mallen används.</span><span class="sxs-lookup"><span data-stu-id="db06a-144">The template body can be authored by someone with knowledge of Batch and the applications to be run by Batch; only values for the author-defined parameters must be supplied when the template is used.</span></span> <span data-ttu-id="db06a-145">En användare utan djupgående Batch och/eller program knowledge kan därför använda mallarna.</span><span class="sxs-lookup"><span data-stu-id="db06a-145">A user without the in-depth Batch and/or application knowledge can therefore use the templates.</span></span>

-   <span data-ttu-id="db06a-146">**Variabler**</span><span class="sxs-lookup"><span data-stu-id="db06a-146">**Variables**</span></span>

    -   <span data-ttu-id="db06a-147">Tillåt enkla eller avancerade parametervärden som ska anges i en enda plats och på en eller flera platser i brödtexten i mallen.</span><span class="sxs-lookup"><span data-stu-id="db06a-147">Allow simple or complex parameter values to be specified in one place and used in one or more places in the template body.</span></span> <span data-ttu-id="db06a-148">Variabler kan förenkla och minska storleken på mallen samt göra den mer hanterbar genom olika platser för att ändra egenskaper vars värde ändras.</span><span class="sxs-lookup"><span data-stu-id="db06a-148">Variables can simplify and reduce the size of the template, as well as make it more maintainable by having one location to change properties whose value may change.</span></span>

-   <span data-ttu-id="db06a-149">**Konstruktioner på högre nivå**</span><span class="sxs-lookup"><span data-stu-id="db06a-149">**Higher-level constructs**</span></span>

    -   <span data-ttu-id="db06a-150">Vissa på högre nivå konstruktioner är tillgängliga i den mall som inte är ännu tillgängliga i Batch-API: er.</span><span class="sxs-lookup"><span data-stu-id="db06a-150">Some higher-level constructs are available in the template that are not yet available in the Batch APIs.</span></span> <span data-ttu-id="db06a-151">Exempelvis kan en aktivitet fabrik definieras i en jobbmall för som skapar flera uppgifter för jobbet med en gemensam aktivitetsdefinition.</span><span class="sxs-lookup"><span data-stu-id="db06a-151">For example, a task factory can be defined in a job template that creates multiple tasks for the job using a common task definition.</span></span> <span data-ttu-id="db06a-152">Dessa konstruktioner undvika att behöva koden för att dynamiskt skapa flera JSON-filer, till exempel en fil per aktivitet, samt skapa skriptfiler för att installera program via en Pakethanteraren, t.ex.</span><span class="sxs-lookup"><span data-stu-id="db06a-152">These constructs avoid the need to code to dynamically create multiple JSON files, such as one file per task, as well as create script files to install applications via a package manager, for example.</span></span>

    -   <span data-ttu-id="db06a-153">På någon punkt i tillämpliga fall kanske dessa konstruktioner läggas till i Batch-tjänsten och är tillgängliga i Batch-API: er, användargränssnitt, osv.</span><span class="sxs-lookup"><span data-stu-id="db06a-153">At some point, where applicable, these constructs may be added to the Batch service and available in the Batch APIs, UIs, etc.</span></span>

### <a name="pool-templates"></a><span data-ttu-id="db06a-154">Pool-mallar</span><span class="sxs-lookup"><span data-stu-id="db06a-154">Pool templates</span></span>

<span data-ttu-id="db06a-155">Förutom funktionerna som standardmall för parametrar och variabler stöds följande på högre nivå konstruktioner av pool-mallen:</span><span class="sxs-lookup"><span data-stu-id="db06a-155">In addition to the standard template capabilities of parameters and variables, the following higher-level constructs are supported by the pool template:</span></span>

-   <span data-ttu-id="db06a-156">**Paketet refererar till**</span><span class="sxs-lookup"><span data-stu-id="db06a-156">**Package references**</span></span>

    -   <span data-ttu-id="db06a-157">Alternativt kan program som ska kopieras till poolen noder med hjälp av paketet chefer.</span><span class="sxs-lookup"><span data-stu-id="db06a-157">Optionally allows software to be copied to pool nodes by using package managers.</span></span> <span data-ttu-id="db06a-158">Package manager och paket-id har angetts.</span><span class="sxs-lookup"><span data-stu-id="db06a-158">The package manager and package id are specified.</span></span> <span data-ttu-id="db06a-159">Kunna deklarerar ett eller flera paket undanröjer behovet av att skapa ett skript som hämtar de nödvändiga paketen installera skriptet och kör skriptet på varje nod i poolen.</span><span class="sxs-lookup"><span data-stu-id="db06a-159">Being able to declare one or more packages avoids the need to create a script that gets the required packages, install the script, and run the script on each pool node.</span></span>

<span data-ttu-id="db06a-160">Följande är ett exempel på en mall som skapar en pool av virtuella Linux-datorer med ffmpeg installerade och endast kräver en pool-id-sträng och antalet virtuella datorer för att ange om du vill använda:</span><span class="sxs-lookup"><span data-stu-id="db06a-160">The following is an example of a template that creates a pool of Linux VMs with ffmpeg installed and only requires a pool id string and the number of VMs to be supplied to use:</span></span>

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "The number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The pool id "
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

<span data-ttu-id="db06a-161">Om mallfilen hette _pool ffmpeg.json_, och sedan mallen skulle anropas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="db06a-161">If the template file was named _pool-ffmpeg.json_, then the template would be invoked as follows:</span></span>

```azurecli
az batch pool create --template pool-ffmpeg.json
```

### <a name="job-templates"></a><span data-ttu-id="db06a-162">Jobbmallar</span><span class="sxs-lookup"><span data-stu-id="db06a-162">Job templates</span></span>

<span data-ttu-id="db06a-163">Förutom funktionerna som standardmall för parametrar och variabler stöds följande på högre nivå konstruktioner av jobbmallen:</span><span class="sxs-lookup"><span data-stu-id="db06a-163">In addition to the standard template capabilities of parameters and variables, the following higher-level constructs are supported by the job template:</span></span>

-   <span data-ttu-id="db06a-164">**Uppgiften fabriken**</span><span class="sxs-lookup"><span data-stu-id="db06a-164">**Task factory**</span></span>

    -   <span data-ttu-id="db06a-165">Skapar flera uppgifter för ett jobb från en aktivitetsdefinition.</span><span class="sxs-lookup"><span data-stu-id="db06a-165">Creates multiple tasks for a job from one task definition.</span></span> <span data-ttu-id="db06a-166">Tre typer av aktiviteten fabriken stöds – parametrisk Sopa aktivitet per fil och uppgift samling.</span><span class="sxs-lookup"><span data-stu-id="db06a-166">Three types of task factory are supported – parametric sweep, task per file, and task collection.</span></span>

<span data-ttu-id="db06a-167">Följande är ett exempel på en mall som skapar ett jobb som använder ffmpeg att kodas om MP4-filer till en av två lägre lösningar med en aktivitet som skapats per video källfilen:</span><span class="sxs-lookup"><span data-stu-id="db06a-167">The following is an example of a template that creates a job that uses ffmpeg to transcode MP4 video files to one of two lower resolutions, with one task created per source video file:</span></span>

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch pool which runs the job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "The name of Azure Batch job"
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

<span data-ttu-id="db06a-168">Om mallfilen hette _jobbet ffmpeg.json_, och sedan mallen skulle anropas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="db06a-168">If the template file was named _job-ffmpeg.json_, then the template would be invoked as follows:</span></span>

```azurecli
az batch job create --template job-ffmpeg.json
```

## <a name="file-groups-and-file-transfer"></a><span data-ttu-id="db06a-169">Filgrupper och filöverföring</span><span class="sxs-lookup"><span data-stu-id="db06a-169">File groups and file transfer</span></span>

<span data-ttu-id="db06a-170">De flesta jobb och uppgifter kräver att inkommande filer och skapar utdatafilerna.</span><span class="sxs-lookup"><span data-stu-id="db06a-170">Most jobs and tasks require input files and produce output files.</span></span> <span data-ttu-id="db06a-171">Både inkommande filer och utdatafilerna vanligtvis måste överföras från klienten till noden, eller från noden till klienten.</span><span class="sxs-lookup"><span data-stu-id="db06a-171">Both input files and output files typically need to be transferred, either from the client to the node, or from the node to the client.</span></span> <span data-ttu-id="db06a-172">Azure Batch CLI-tillägget avlägsnar bort filöverföring och använder det lagringskonto som skapas som standard för varje Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="db06a-172">The Azure Batch CLI extension abstracts away file transfer and utilizes the storage account that is created by default for each Batch account.</span></span>

<span data-ttu-id="db06a-173">En filgrupp som är lika med en behållare som skapats i Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="db06a-173">A file group equates to a container that is created in the Azure storage account.</span></span> <span data-ttu-id="db06a-174">Filgruppen kan ha undermappar.</span><span class="sxs-lookup"><span data-stu-id="db06a-174">The file group may have subfolders.</span></span>

<span data-ttu-id="db06a-175">Tillägget Batch CLI innehåller kommandon för att ladda upp filer från klienten till en angiven fil-grupp och hämta filer från den angivna filgruppen till en klient.</span><span class="sxs-lookup"><span data-stu-id="db06a-175">The Batch CLI extension provides commands for uploading files from client to a specified file group and downloading files from the specified file group to a client.</span></span>

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

<span data-ttu-id="db06a-176">Poolen och jobb med mallar kan filer som lagras i filgrupper anges för kopiering till poolen noder eller inaktivera poolen noder tillbaka till en filgrupp.</span><span class="sxs-lookup"><span data-stu-id="db06a-176">Pool and job templates allow files stored in file groups to be specified for copy onto pool nodes or off pool nodes back to a file group.</span></span> <span data-ttu-id="db06a-177">Till exempel i jobbmallen angav tidigare, har filen grupp ”ffmpeg-indata” angetts för uppgiften fabriken som plats för video källfilerna kopieras ned till noden för omkodning; filen grupp ”ffmpeg-utdata” används som den plats där utdatafiler kodas kopieras till från den nod som kör varje aktivitet.</span><span class="sxs-lookup"><span data-stu-id="db06a-177">For example, in the job template specified previously, the file group “ffmpeg-input” is specified for the task factory as the location of the source video files copied down onto the node for transcoding; the file group “ffmpeg-output” is used as the location where the transcoded output files are copied to from the node running each task.</span></span>

## <a name="summary"></a><span data-ttu-id="db06a-178">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="db06a-178">Summary</span></span>

<span data-ttu-id="db06a-179">Stöd för överföring av mall och fil lagts till Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="db06a-179">Template and file transfer support have currently been added only to the Azure CLI.</span></span> <span data-ttu-id="db06a-180">Målet är att expandera den målgrupp som kan använda Batch för användare som inte behöver utveckla kod med Batch-API, till exempel forskare, IT-användare och så vidare.</span><span class="sxs-lookup"><span data-stu-id="db06a-180">The goal is to expand the audience that can use Batch to users who do not need to develop code using the Batch APIs, such as researchers, IT users, and so on.</span></span> <span data-ttu-id="db06a-181">Utan kodning, kan användare med kunskaper om Azure Batch och program som ska köras av Batch skapa mallar för att skapa en pool och jobb.</span><span class="sxs-lookup"><span data-stu-id="db06a-181">Without coding, users with knowledge of Azure, Batch, and the applications to be run by Batch can create templates for pool and job creation.</span></span> <span data-ttu-id="db06a-182">Användare utan detaljerade kunskaper om Batch- och programmen kan använda mallarna med mallparametrar.</span><span class="sxs-lookup"><span data-stu-id="db06a-182">With template parameters, users without detailed knowledge of Batch and the applications can use the templates.</span></span>

<span data-ttu-id="db06a-183">Prova att använda Batch-tillägget i Azure CLI och ge oss feedback och förslag, antingen i kommentarer för den här artikeln eller via den [Azure Batch-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).</span><span class="sxs-lookup"><span data-stu-id="db06a-183">Try out the Batch extension for the Azure CLI and provide us with any feedback or suggestions, either in the comments for this article or via the [Azure Batch forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).</span></span>

## <a name="next-steps"></a><span data-ttu-id="db06a-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db06a-184">Next steps</span></span>

- <span data-ttu-id="db06a-185">Bloggposten Batch-mallar: [kör Azure Batch-jobb med hjälp av Azure CLI – ingen kod krävs](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).</span><span class="sxs-lookup"><span data-stu-id="db06a-185">See the Batch templates blog post: [Running Azure Batch jobs using the Azure CLI – no code required](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).</span></span>
- <span data-ttu-id="db06a-186">Utförlig information om installation och användning, exempel och källkoden är tillgängliga i den [Azure GitHub-lagringsplatsen](https://github.com/Azure/azure-batch-cli-extensions).</span><span class="sxs-lookup"><span data-stu-id="db06a-186">Detailed installation and usage documentation, samples, and source code are available in the [Azure GitHub repository](https://github.com/Azure/azure-batch-cli-extensions).</span></span>
