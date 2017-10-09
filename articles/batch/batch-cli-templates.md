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
# <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a>Använda Azure Batch CLI-mallar och filöverföring (förhandsversion)

Med hello Azure CLI är det möjligt toorun batchjobb utan att skriva kod.

Mallfilerna kan skapas och används med hello Azure CLI som tillåter Batch pooler, jobb och uppgifter toobe skapas. Jobbet indatafiler kan lätt överföras till hello storage-konto som är associerade med hello Batch-konto och jobbet utdatafilerna hämtas.

## <a name="overview"></a>Översikt

Ett tillägg toohello Azure CLI kan Batch toobe som används för slutpunkt till slutpunkt av användare som inte är utvecklare. Du kan skapa en pool, indata överförs, jobb och förknippade aktiviteter som skapats och hello resulterande utdata hämtas – ingen kod som krävs, hello CLI som används direkt eller integreras i skript.

Batch-mallar som bygger på hello [befintliga Batch-stöd i hello Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) som tillåter JSON-filer toospecify egenskapsvärden för hello skapa pooler, projekt, uppgifter och andra objekt. Med Batch mallar hello följande funktioner har lagts till via vad du kan göra med hello JSON-filer:

-   Du kan definiera parametrar. Om hello mall används är endast hello parametervärden angivna toocreate hello objektet med andra objekt egenskapsvärden som anges i hello mallen brödtext. En användare som förstår Batch och toobe för program som körs av Batch kan skapa mallar, ange pool, jobb och egenskapsvärden för aktiviteten. En användare mindre känner till Batch och/eller program behöver bara toospecify hello värden för hello definierade parametrar.

-   Jobbet uppgiften fabriker skapar du en eller flera uppgifter som är kopplad till ett jobb, undvika hello behöver för många uppgiften definitioner toobe skapas och avsevärt förenklad jobbet.


Indata-filer måste toobe som angetts för jobb och utdata datafiler produceras ofta. Ett lagringskonto associeras som standard, med varje Batch-konto och filer kan vara enkelt överförda tooand från det här lagringskontot med hjälp av CLI med ingen kodning och inga autentiseringsuppgifter för lagring krävs.

Till exempel [ffmpeg](http://ffmpeg.org/) är ett populärt program som bearbetar ljud- och bildfiler. hello Azure Batch CLI kan vara används tooinvoke ffmpeg tootranscode källa videofiler toodifferent lösningar.

-   En pool mall skapas. hello-användaren som skapar hello mallen vet hur toocall hello ffmpeg programmet och dess krav. de ange hello lämpliga OS, VM storlek, hur ffmpeg är installerat (från ett programpaket eller med Pakethanteraren, till exempel) och andra poolen egenskapsvärden. Parametrar skapas när hello mallen används endast hello programpools-id och antal virtuella datorer måste toobe anges.

-   Ett jobb skapas. hello skapar hello användarmallen vet hur ffmpeg behov toobe anropas tootranscode källa video tooa annan upplösning och anger hello kommandorad för uppgift; de vet att det finns en mapp som innehåller hello källa videofiler, med en aktivitet som behövs per indatafilen.

-   En användare med en uppsättning videofiler tootranscode först skapar en pool med hello poolen mall, ange endast hello programpools-id och antal virtuella datorer som krävs. De kan sedan överföra hello källa filer tootranscode. Ett jobb kan sedan skickas med hello jobbmallen anger endast hello programpools-id och platsen för källfiler för hello har överförts. hello Batch-jobbet har skapats med en aktivitet per indatafilen som genereras. Slutligen kan hello kodas utdatafilerna hämtas.

## <a name="installation"></a>Installation

hello mall och fil överföring funktioner kräver ett tillägg toobe installerad.

Anvisningar för hur tooinstall hello Azure CLI finns [installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

En gång hello Azure CLI har installerats, hello Batch kan installeras med följande CLI-kommando:

```azurecli
az component update --add batch-extensions --allow-third-party
```

Läs mer om hello Batch tillägget [Microsoft Azure Batch CLI-tillägg för Windows, Mac och Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).

## <a name="templates"></a>Mallar

hello Azure Batch CLI kan objekt, till exempel pooler, jobb och uppgifter toobe skapas genom att ange en JSON-fil som innehåller egenskapsnamn och värden. Exempel:

```azurecli
az batch pool create –-json-file AppPool.json
```

Azure Batch-mallar är liknande tooAzure Resource Manager-mallar, funktionalitet och syntax. De är JSON-filer som innehåller egenskapsnamn och värden, men lägga till följande huvudkoncepten hello:

-   **Parametrar**

    -   Tillåt egenskapen värden toobe anges i ett body-avsnitt med bara parametervärden som behöver toobe angavs när hello mall används. Till exempel kan hello klar definition för en pool placeras i hello brödtext och endast en parameter som definierats för pool-id endast en pool med id-sträng måste därför toobe angivna toocreate poolen.
        
    -   hello mallen brödtext kan skapas av någon med kunskap om Batch och hello program toobe kör av Batch; endast värden för hello författare definierade parametrar måste anges om hello mall används. En användare utan hello djupgående Batch och/eller program knowledge kan därför använda mallarna.

-   **Variabler**

    -   Tillåt enkla eller avancerade parametern värden toobe anges i en enda plats och på en eller flera platser i hello mallen brödtext. Variabler kan förenkla och minska hello storlek hello mallen samt göra den mer hanterbar genom att ha ett toochange platsegenskaper vars värde ändras.

-   **Konstruktioner på högre nivå**

    -   Vissa på högre nivå konstruktioner är tillgängliga i hello-mallen som inte ännu är tillgängliga i hello Batch-API: er. Exempelvis kan en aktivitet fabrik definieras i en jobbmall för som skapar flera uppgifter för hello jobb med hjälp av en gemensam aktivitetsdefinition. Dessa konstruktioner Undvik hello måste toocode för att dynamiskt skapa flera JSON-filer, till exempel en fil per aktivitet, samt skapa skript filer tooinstall program via en Pakethanteraren, t.ex.

    -   Vid en punkt, om tillämpligt, dessa konstruktioner kanske toothe Batch-tjänsten och finns tillgänglig i hello Batch-API: er, användargränssnitt och så vidare.

### <a name="pool-templates"></a>Pool-mallar

Dessutom stöds toohello standardmall funktioner för parametrar och variabler hello följande konstruktioner på högre nivå av hello poolen mallen:

-   **Paketet refererar till**

    -   Alternativt kan programvara toobe kopieras toopool noder med hjälp av paketet chefer. hello package manager och paket-id har angetts. Ett eller flera paket som kan toodeclare undviker hello måste toocreate ett skript som hämtar hello krävs paket, installera hello skript och kör hello skript på varje nod i poolen.

hello följande är ett exempel på en mall som skapar en pool av virtuella Linux-datorer med ffmpeg installerade och endast kräver en pool-id sträng och hello antal VMs toobe angivna toouse:

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

Om hello mallfilen hette _pool ffmpeg.json_, och sedan hello mallen skulle anropas på följande sätt:

```azurecli
az batch pool create --template pool-ffmpeg.json
```

### <a name="job-templates"></a>Jobbmallar

Dessutom stöds toohello standardmall funktioner för parametrar och variabler hello följande konstruktioner på högre nivå av hello jobbmallen:

-   **Uppgiften fabriken**

    -   Skapar flera uppgifter för ett jobb från en aktivitetsdefinition. Tre typer av aktiviteten fabriken stöds – parametrisk Sopa aktivitet per fil och uppgift samling.

hello följande är ett exempel på en mall som skapar ett jobb som använder ffmpeg till omkoda MP4-filer tooone av två lägre lösningar med en aktivitet som skapats per video källfilen:

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

Om hello mallfilen hette _jobbet ffmpeg.json_, och sedan hello mallen skulle anropas på följande sätt:

```azurecli
az batch job create --template job-ffmpeg.json
```

## <a name="file-groups-and-file-transfer"></a>Filgrupper och filöverföring

De flesta jobb och uppgifter kräver att inkommande filer och skapar utdatafilerna. Både inkommande filer och utdatafiler måste vanligtvis toobe överförs från hello klienten toohello nod eller från hello nod toohello klienten. hello Azure Batch CLI tillägget avlägsnar bort filöverföring och använder hello storage-konto som skapas som standard för varje Batch-kontot.

En filgrupp är lika tooa behållare som skapas i hello Azure storage-konto. hello filgrupp kan ha undermappar.

hello Batch CLI tillägget innehåller kommandon för att ladda upp filer från klientgrupp tooa angivna filen och hämta filer från hello angivna filen grupp tooa klient.

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

Poolen och jobb med mallar kan filer som lagras i filen grupper toobe som angetts för kopia till poolen noder eller inaktivera poolen noder bakåt tooa filgrupp. Till exempel i jobbmallen angav tidigare, har hello filen grupp ”ffmpeg inmatning” angetts för hello uppgiften factory som hello platsen för hello video källfilerna kopieras ned till hello noden för omkodning; hello filen grupp ”ffmpeg-utdata” används som hello platsen där hello kodas utdatafilerna kopieras toofrom hello nod som kör varje aktivitet.

## <a name="summary"></a>Sammanfattning

Stöd för överföring av mall och fil för närvarande lades till endast toohello Azure CLI. hello målet är tooexpand hello målgruppen som kan använda Batch toousers som inte behöver toodevelop kod med hello Batch-API: er, till exempel forskare, IT-användare och så vidare. Utan kodning, kan användare med kunskaper om Azure Batch och hello program toobe kör av Batch skapa mallar för att skapa en pool och jobb. Användare utan detaljerade kunskaper om Batch- och hello program kan använda hello mallar med mallparametrar.

Prova hello Batch-tillägget för hello Azure CLI och ge oss feedback och förslag, antingen i hello kommentarer för den här artikeln eller via hello [Azure Batch-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).

## <a name="next-steps"></a>Nästa steg

- Finns hello Batch mallar blogginlägg: [kör Azure Batch-jobb med hjälp av hello Azure CLI – ingen kod krävs](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).
- Utförlig information om installation och användning, exempel och källkoden är tillgängliga i hello [Azure GitHub-lagringsplatsen](https://github.com/Azure/azure-batch-cli-extensions).
