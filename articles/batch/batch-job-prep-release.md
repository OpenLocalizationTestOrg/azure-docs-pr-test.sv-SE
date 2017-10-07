---
title: "aaaCreate uppgifter tooprepare jobb och fullständig jobb på datornoderna - Azure Batch | Microsoft Docs"
description: "Använd jobbet nivå förberedelse uppgifter toominimize data transfer tooAzure Batch-beräkningsnoder och släpp uppgifter för rensning av noden på jobbet har slutförts."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fd5fb47ae6700281e63048c49a1241f4e935baba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a>Kör jobbförberedelse och jobbet versionen uppgifter på Batch-beräkningsnoder

 En Azure Batch-jobbet kräver ofta någon form av installationsprogrammet innan dess aktiviteter utförs och jobbet efter Underhåll när dess aktiviteter är slutförda. Du kan behöver toodownload vanliga uppgiften indata tooyour compute-noder eller överför uppgiften utgående data tooAzure lagring när hello jobbet har slutförts. Du kan använda **jobbet förberedelse** och **jobbet versionen** uppgifter tooperform dessa åtgärder.

## <a name="what-are-job-preparation-and-release-tasks"></a>Vad är jobbförberedelseuppgiften och viktiga uppgifter?
Innan du kör ett jobb uppgifter hello jobbet förberedelse aktiviteten körs på alla compute-noder schemalagda toorun åtminstone en aktivitet. När hello jobbet är slutfört, körs hello jobbet versionen aktiviteten på varje nod i hello-pool som körts minst en aktivitet. Du kan ange en toobe för kommandoraden som anropas när en jobbförberedelseuppgiften precis som med normal batchaktiviteter eller uppgiften körs.

Jobbet förberedelse och version aktiviteter finns bekant Batch aktivitet funktioner, till exempel Filhämtning ([resursfiler][net_job_prep_resourcefiles]), utökade körning, anpassade miljövariabler, varaktighet för maximal körning av, försök igen antal och kvarhållningstiden för filen.

I följande avsnitt hello, lär du dig hur toouse hello [JobPreparationTask] [ net_job_prep] och [JobReleaseTask] [ net_job_release] påträffades i hello [Batch .NET] [ api_net] bibliotek.

> [!TIP]
> Jobbet förberedelse och version aktiviteter är särskilt användbart i miljöer med ”delat poolen”, som en pool av datornoderna kvarstår mellan jobbkörningar och används av många jobb.
> 
> 

## <a name="when-toouse-job-preparation-and-release-tasks"></a>När toouse jobbet förberedelse och släpp aktiviteter
Jobbförberedelseuppgiften och versionen jobbuppgifter är passar bra för hello följande situationer:

**Hämta vanliga aktivitetsdata**

Batchjobb kräver ofta en gemensam uppsättning data som indata för hello jobbets uppgifter. Dagliga risk analys beräkningar är exempelvis marknaden data projektspecifika, men vanliga tooall uppgifter i hello-jobbet. Den här marknaden data, ofta flera gigabyte i storlek, bör vara hämtade tooeach beräkningsnod bara en gång så att alla aktiviteter som körs på hello kan använda den. Använd en **förberedelse projektaktivitet** toodownload data tooeach noden innan hello körningen av hello jobbet har andra aktiviteter.

**Ta bort jobb- och utdata**

I en ”delad pool” miljö, där en pool compute-noder inte är inaktiverade mellan jobb kan behöva du toodelete jobbdata mellan körs. Du kan behöva tooconserve ledigt diskutrymme på hello noder eller uppfyller organisationens säkerhetsprinciper. Använd en **versionen projektaktivitet** toodelete data som hämtas av en projektaktivitet förberedelse, eller genererats under körningen av aktiviteten.

**Kvarhållning av logg**

Du kanske vill tookeep en kopia av loggfiler som dina aktiviteter genererar eller kanske kraschdumpfiler som genereras av misslyckade program. Använd en **versionen projektaktivitet** i sådana fall toocompress och överföra den här informationen tooan [Azure Storage] [ azure_storage] konto.

> [!TIP]
> Ett annat sätt toopersist loggar och andra jobb- och utdata data är toouse hello [Azure Batch filen konventioner](batch-task-output.md) bibliotek.
> 
> 

## <a name="job-preparation-task"></a>Förberedelse för projektaktivitet
Innan körningen av ett jobb uppgifter utför Batch hello projektaktivitet förberedelse på varje beräkningsnod som är schemalagda toorun en aktivitet. Som standard väntar hello Batch-tjänsten för hello jobbet förberedelse uppgiften toobe slutförts innan du kör hello aktiviteter schemalagda tooexecute på hello-nod. Du kan dock konfigurera hello-tjänsten inte toowait. Om hello nod har startats om hello jobbet uppgiften körs igen, men du kan också inaktivera det här beteendet.

hello jobbet uppgiften körs bara på noder som är schemalagda toorun en aktivitet. Detta förhindrar hello onödiga körningen av en jobbförberedelseuppgift om en nod inte har tilldelats en aktivitet. Detta kan inträffa när hello antal uppgifter för ett jobb är mindre än hello antalet noder i en pool. Det gäller även när [samtidiga uppgiftskörningen](batch-parallel-node-tasks.md) är aktiverat, vilket lämnar vissa noder inaktiv om hello uppgiften antalet är lägre än hello Totalt antal möjliga samtidiga uppgifter. Genom att köra inte hello projektaktivitet förberedelse för inaktiv noder kan lägga du mindre pengar på kostnader för överföring av data.

> [!NOTE]
> [JobPreparationTask] [ net_job_prep_cloudjob] skiljer sig från [CloudPool.StartTask] [ pool_starttask] i att JobPreparationTask kör hello början av varje jobb medan startuppgift har ställts körs bara när en beräkningsnod först ansluter till en pool eller startar om.
> 
> 

## <a name="job-release-task"></a>Versionen för projektaktivitet
När ett jobb har markerats som slutförd, körs hello jobbet versionen uppgiften på varje nod i hello-pool som körts minst en aktivitet. Du kan markera ett jobb som slutförd genom att utfärda en avsluta begäran. Hej Batch-tjänsten och sedan anger hello jobbets status för*avslutar*, avbryts alla aktiva eller köra uppgifter som är kopplad till hello jobbet och kör hello projektaktivitet versionen. hello jobbet flyttar toohello *slutförts* tillstånd.

> [!NOTE]
> Borttagning av jobbet körs även hello projektaktivitet versionen. Men om ett jobb har redan avslutats, körs hello versionen inte aktiviteten en gång om jobbet hello senare tas bort.
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Jobbet prep och släpp aktiviteter med Batch .NET
toouse en projektaktivitet förberedelse, tilldela en [JobPreparationTask] [ net_job_prep] objekt tooyour jobbet [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] egenskapen . På liknande sätt kan initiera en [JobReleaseTask] [ net_job_release] och tilldela den tooyour jobbet [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] egenskapen tooset hello Jobbets uppgiften.

I det här kodstycket `myBatchClient` är en instans av [BatchClient][net_batch_client], och `myPool` är en befintlig adresspool inom hello Batch-kontot.

```csharp
// Create hello CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify hello command lines for hello job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign hello job preparation task toohello job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign hello job release task toohello job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Som tidigare nämnts körs hello versionen uppgiften när ett jobb avslutas eller tas bort. Avsluta ett jobb med [JobOperations.TerminateJobAsync][net_job_terminate]. Ta bort ett jobb med [JobOperations.DeleteJobAsync][net_job_delete]. Du vanligtvis avsluta eller ta bort ett jobb när dess aktiviteter har slutförts eller när en tidsgräns som du har definierat har uppnåtts.

```csharp
// Terminate hello job toomark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Kodexempel på GitHub
toosee förberedelse och version jobbuppgifter i åtgärden, kolla hello [JobPrepRelease] [ job_prep_release_sample] exempelprojektet på GitHub. Detta konsolprogram hello följande:

1. Skapar en pool med två noder som ”liten”.
2. Skapar ett jobb med jobbförberedelseuppgiften, version och standarduppgifter.
3. Kör hello förberedelse projektaktivitet som först skriver hello nod-ID tooa textfil i en nod ”delade” katalogen.
4. Kör en aktivitet på varje nod som skriver dess aktivitet ID toohello samma textfil.
5. När alla aktiviteter har slutförts (eller hello tidsgränsen uppnås), skriver du hello innehållet för varje nod text filen toohello konsol.
6. När hello jobbet har slutförts körs hello viktig uppgift toodelete hello fil från hello-nod.
7. Utskrifter hello slutkoder av hello jobbförberedelseuppgiften och släpp aktiviteter för varje nod som de körs.
8. Pausar körningen tooallow bekräftelse av jobb och/eller pool borttagning.

Utdata från hello exempelprogrammet är liknande toohello följande:

```
Attempting toocreate pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob tooreach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER tooexit...
```

> [!NOTE]
> På grund av toohello variabeln skapa och starta tiden för noder i en ny pool (vissa noder är redo för uppgifter före andra), kan du se olika utdata. Eftersom hello slutföras snabbt, kan en av noderna hello poolen specifikt köra alla hello jobbets uppgifter. Om detta händer ser du att hello jobbet prep och släpp aktiviteter finns inte för hello-nod som körs inga aktiviteter.
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-hello-azure-portal"></a>Granska jobbförberedelseuppgiften och versionen uppgifter i hello Azure-portalen
När du kör hello exempelprogrammet, kan du använda hello [Azure-portalen] [ portal] tooview hello egenskaper för hello jobb och dess uppgifter eller även hämta hello delade textfil som ändras av hello jobbets uppgifter.

hello skärmbilden nedan visar hello **förberedelse uppgifter bladet** i hello Azure-portalen efter en körning av hello exempelprogrammet. Navigera toohello *JobPrepReleaseSampleJob* egenskaper när aktiviteterna har slutförts (men innan du tar bort dina jobb och pool) och klicka på **förberedelseuppgifter** eller **viktiga uppgifter** tooview deras egenskaper.

![Förberedelse av jobbegenskaper i Azure-portalen][1]

## <a name="next-steps"></a>Nästa steg
### <a name="application-packages"></a>Programpaket
I tillägg toohello förberedelse projektaktivitet, kan du också använda hello [programpaket](batch-application-packages.md) funktion i Batch tooprepare compute-noder för körning av aktiviteten. Den här funktionen är särskilt användbar för att distribuera program som inte kräver kör ett installationsprogram, program som innehåller många (100 +) filer eller program som kräver strikt versionskontroll.

### <a name="installing-applications-and-staging-data"></a>Installera program och mellanlagring av data
Den här MSDN-foruminlägg innehåller en översikt över flera metoder för att förbereda din noder för pågående aktiviteter:

[Installera program och mellanlagring av data på Batch-beräkningsnoder][forum_post]

Skrivs av en av hello Azure Batch-gruppmedlemmar beskrivs den flera metoder som du kan använda toodeploy program och data toocompute noder.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
