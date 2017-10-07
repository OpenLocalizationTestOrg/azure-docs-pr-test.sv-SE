---
title: "aaa ”Azure Batch compute-nod miljövariabler | Microsoft Docs ”"
description: "Compute-nod miljö variabelreferens för Azure Batch Analytics."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/05/2017
ms.author: tamram
ms.openlocfilehash: 860f34b530579a81fbd5cf8ffa31df79d917c080
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-batch-compute-node-environment-variables"></a>Azure Batch compute-nod miljövariabler
Hej [Azure Batch-tjänsten](https://azure.microsoft.com/services/batch/) anger hello efter datornoderna miljövariabler. Du kan referera till dessa miljövariabler i aktiviteten kommandorader, och hello program och skript som körs av hello kommandorader.

Mer information om hur du använder miljövariabler med Batch finns [miljöinställningar för uppgifter](https://docs.microsoft.com/azure/batch/batch-api-basics#environment-settings-for-tasks).

## <a name="environment-variable-visibility"></a>Variabeln synlighet för miljö

De här miljövariablerna visas bara i hello hello **aktivitet användaren**, hello användarkonto på hello nod som en aktivitet körs. Du kommer *inte* se dessa om du [fjärransluta](https://azure.microsoft.com/documentation/articles/batch-api-basics/#connecting-to-compute-nodes) tooa compute-nod via Remote Desktop Protocol (RDP) eller SSH (Secure Shell) och listan hello miljövariabler. Detta beror på att hello-användarkonto som används för anslutning till inte hello samma som hello-konto som används av hello-aktivitet.

## <a name="command-line-expansion-of-environment-variables"></a>Kommandoradsverktyget expandering av miljövariabler

hello kommandorader som körs av uppgifter på compute-noder gör inte köras under ett gränssnitt. Därför kan dessa kommandorader internt kan inte dra nytta av shell-funktioner, till exempel expandering (Detta omfattar hello `PATH`). tootake nytta av dessa funktioner, måste du **anropa hello shell** hello-kommandoraden. Till exempel starta `cmd.exe` på Windows compute-noder eller `/bin/sh` på Linux-noder:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Miljövariabler

| Variabelnamn                     | Beskrivning                                                              | Tillgänglighet | Exempel |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | hello namnet på hello Batch-kontot som hello aktiviteten tillhör.                  | Alla aktiviteter.   | mybatchaccount |
| AZ_BATCH_CERTIFICATES_DIR       | En katalog i hello [arbetskatalog för aktiviteten] [ files_dirs] i vilka certifikat som lagras för Linux compute-noder. Observera att den här miljövariabeln inte gäller tooWindows compute-noder.                                                  | Alla aktiviteter.   |  /mnt/batch/Tasks/workitems/batchjob001/Job-1/task001/certs |
| AZ_BATCH_JOB_ID                 | hello-ID för hello jobb som hello aktiviteten tillhör. | Alla aktiviteter utom starta uppgiften. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | hello fullständig sökväg till hello jobbförberedelseuppgiften [aktivitet directory] [ files_dirs] på hello-nod. | Alla aktiviteter utom start och aktiviteten jobbet förberedelser. Endast tillgänglig om hello jobbet har konfigurerats med en förberedelse för projektaktivitet. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | hello fullständig sökväg till hello jobbförberedelseuppgiften [arbetskatalog för aktiviteten] [ files_dirs] på hello-nod. | Alla aktiviteter utom start och aktiviteten jobbet förberedelser. Endast tillgänglig om hello jobbet har konfigurerats med en förberedelse för projektaktivitet. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_NODE_ID                | hello-ID för hello-nod som hello aktivitet har tilldelats. | Alla aktiviteter. | TVM-1219235766_3-20160919t172711z |
| AZ_BATCH_NODE_ROOT_DIR          | hello fullständig sökväg till hello roten för alla [Batch-kataloger] [ files_dirs] på hello-nod. | Alla aktiviteter. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | hello fullständig sökväg till hello [delade katalogen] [ files_dirs] på hello-nod. Alla aktiviteter som utförs på en nod har full åtkomst toothis directory. Uppgifter som utförs på andra noder har inte fjärråtkomst toothis directory (det inte är en ”delad” nätverkskatalog). | Alla aktiviteter. | C:\user\tasks\shared |
| AZ_BATCH_NODE_STARTUP_DIR       | hello fullständig sökväg till hello [starta uppgiften directory] [ files_dirs] på hello-nod. | Alla aktiviteter. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | hello-ID för hello-pool som hello aktiviteten körs på. | Alla aktiviteter. | batchpool001 |
| AZ_BATCH_TASK_DIR               | hello fullständig sökväg till hello [aktivitet directory] [ files_dirs] på hello-nod. Den här katalogen innehåller hello `stdout.txt` och `stderr.txt` hello aktiviteten och hello AZ_BATCH_TASK_WORKING_DIR. | Alla aktiviteter. | C:\user\tasks\workitems\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | hello-ID för hello aktuella aktiviteten. | Alla aktiviteter utom starta uppgiften. | task001 |
| AZ_BATCH_TASK_WORKING_DIR       | hello fullständig sökväg till hello [arbetskatalog för aktiviteten] [ files_dirs] på hello-nod. hello körs aktiviteten har full åtkomst toothis directory. | Alla aktiviteter. | C:\user\tasks\workitems\batchjob001\job-1\task001\wd |
| CCP_NODES                       | hello listan över noder och antalet kärnor per nod som är allokerade tooa [flera instansuppgift][multi_instance]. Noder och kärnor visas i hello-format`numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, där hello antalet noder följs av en eller flera noden IP-adresser och hello antalet kärnor för varje. |  Flera instanser primära och underaktiviteter. |`2 10.0.0.4 1 10.0.0.5 1` |
| AZ_BATCH_NODE_LIST              | hello listan över noder som är allokerade tooa [flera instansuppgift] [ multi_instance] hello format `nodeIP;nodeIP`. | Flera instanser primära och underaktiviteter. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_HOST_LIST              | hello listan över noder som är allokerade tooa [flera instansuppgift] [ multi_instance] hello format `nodeIP,nodeIP`. | Flera instanser primära och underaktiviteter. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_MASTER_NODE            | hello IP-adressen och porten för hello-beräkningsnod i vilka hello främsta uppgiften för en [flera instansuppgift] [ multi_instance] körs. | Flera instanser primära och underaktiviteter. | `10.0.0.4:6000`|
| AZ_BATCH_TASK_SHARED_DIR | En sökväg som är identisk för hello primära aktivitet, och varje underaktivitet till en [flera instansuppgift][multi_instance]. hello sökväg finns på varje nod på som hello flera instanser aktiviteten körs och är tillgänglig toohello aktivitetskommandon som körs på noden för läsning och skrivning (både hello [samordning kommandot] [ coord_cmd] och hello [kommandot programmet][app_cmd]). Underaktiviteter eller en primär aktivitet som utförs på andra noder har inte fjärråtkomst toothis directory (det inte är en ”delad” nätverkskatalog). | Flera instanser primära och underaktiviteter. | C:\user\tasks\workitems\multiinstancesamplejob\job-1\multiinstancesampletask |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Anger om aktuella hello-noden är hello huvudnoden för en [flera instansuppgift][multi_instance]. Möjliga värden är `true` och `false`.| Flera instanser primära och underaktiviteter. | `true` |
| AZ_BATCH_NODE_IS_DEDICATED | Om `true`, hello aktuella noden är en dedikerad nod. Om `false`, är det en [låg prioritet nod](batch-low-pri-vms.md). | Alla aktiviteter. | `true` |

[files_dirs]: https://azure.microsoft.com/documentation/articles/batch-api-basics/#files-and-directories
[multi_instance]: https://azure.microsoft.com/documentation/articles/batch-mpi/
[coord_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#coordination-command
[app_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#application-command
