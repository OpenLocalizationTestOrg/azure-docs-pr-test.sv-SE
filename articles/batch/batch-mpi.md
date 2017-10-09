---
title: "flera instanser av aaaUse uppgifter toorun MPI program – Azure Batch | Microsoft Docs"
description: "Lär dig hur tooexecute Message Passing Interface (MPI) program med hjälp av hello flera instanser aktiviteten Skriv i Azure Batch."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: 5/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0e3295a6aeb76267c26d5504bcff59de3dc5e22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-multi-instance-tasks-toorun-message-passing-interface-mpi-applications-in-batch"></a>Använda flera instanser uppgifter toorun Message Passing Interface (MPI) program i Batch

Flera instanser uppgifter kan du toorun en Azure Batch-uppgift på flera compute-noder samtidigt. Dessa uppgifter gör det möjligt för höga prestanda scenarier som program för Message Passing Interface (MPI) i gruppen. I den här artikeln får du lära dig hur tooexecute flera instanser aktiviteter med hjälp av hello [Batch .NET] [ api_net] bibliotek.

> [!NOTE]
> Hello exemplen i den här artikeln fokuserar på Batch .NET, MS-MPI och datornoder för Windows är hello flera instanser uppgiften begrepp som beskrivs här tillämpliga tooother plattformar och tekniker (Python och Intel MPI för Linux-noder, till exempel).
>
>

## <a name="multi-instance-task-overview"></a>Översikt över uppgifter för flera instanser
I batchen, varje uppgift är normalt körs på en enda beräkningsnod--du skickar flera uppgifter tooa jobb, och hello Batch-tjänsten scheman för varje aktivitet för körning på en nod. Men genom att konfigurera en aktivitet **inställningar för flera instanser**, anger du Batch tooinstead skapa en primär aktivitet och flera underaktiviteter som sedan körs på flera noder.

![Översikt över uppgifter för flera instanser][1]

När du skickar en uppgift med flera instanser inställningar tooa jobbet utför Batch flera steg unikt toomulti-instans uppgifter:

1. hello Batch-tjänsten skapar en **primära** och flera **underaktiviteter** baserat på inställningarna för hello flera instanser. hello Totalt antal aktiviteter (primära plus alla underaktiviteter) stämmer hello antalet **instanser** (datornoder) du anger i inställningarna för hello flera instanser.
2. Batch anger en hello compute-noder som hello **master**, och scheman hello främsta uppgiften tooexecute på hello master. Den schemaläggs hello underaktiviteter tooexecute på hello resten av hello compute-noder allokerade toohello flera instanser aktivitet, en delaktivitet per nod.
3. hello primära och alla underaktiviteter hämta någon **vanliga resursfiler** du anger i inställningarna för hello flera instanser.
4. När du har laddats ned hello vanliga resursfiler, hello primära och underaktiviteter köra hello **samordning kommandot** du anger i inställningarna för hello flera instanser. hello samordning kommandot är brukar användas tooprepare noder för att köra hello-aktivitet. Detta kan inkludera startar Bakgrundstjänster (exempelvis [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) och verifiera att hello noder är klar tooprocess mellan noder meddelanden.
5. hello främsta uppgiften kör hello **kommandot programmet** på hello huvudnoden *när* hello samordning kommandot har slutförts av hello primära och alla underaktiviteter. kommandot för programmet hello är hello kommandoraden för hello flera instanser själva aktiviteten och körs bara av hello främsta uppgiften. I en [MS-MPI][msmpi_msdn]-baserad lösning, det är där du kör MPI-aktiverade appen med `mpiexec.exe`.

> [!NOTE]
> Även om det är funktionellt distinkta hello ”flera instanser task” är inte en unik Uppgiftstyp som hello [startuppgift har ställts] [ net_starttask] eller [JobPreparationTask] [ net_jobprep]. hello flera instanser aktivitet är helt enkelt en standard Batch-aktivitet ([CloudTask] [ net_task] i Batch .NET) vars flera instanser inställningar har konfigurerats. I den här artikeln finns vi toothis som hello **flera instanser uppgiften**.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Krav för flera instanser uppgifter
Flera instanser uppgifter kräver en pool med **kommunikationen mellan noder aktiverat**, och med **samtidiga uppgiftskörningen inaktiveras**. toodisable samtidiga uppgiftskörningen, ange hello [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) egenskapen too1.

Det här kodstycket visar hur toocreate poolen för flera instanser av åtgärder med hello Batch .NET-biblioteket.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> Om du försöker toorun en aktivitet med flera instanser i en pool med dessa kommunikation har inaktiverats eller med en *maxTasksPerNode* värde större än 1, hello aldrig schemaläggs--på obestämd tid förblir det hello ”aktiv”. 
>
> Flera instanser uppgifter kan köra endast på noder i pooler som skapats efter 14 December 2015.
>
>

### <a name="use-a-starttask-tooinstall-mpi"></a>Använd en startuppgift har ställts tooinstall MPI
toorun MPI program med flera instanser uppgiften, måste du först tooinstall en MPI-implementering (MS-MPI eller Intel MPI, till exempel) på hello datornoder i hello pool. Detta är ett bra tillfälle toouse en [startuppgift har ställts][net_starttask], som körs varje gång en nod ansluter till en pool eller startas. Det här kodstycket skapar en startuppgift har ställts som anger hello MS-MPI installationspaketet som en [resursfilen][net_resourcefile]. uppgiften hello-start kommandoraden körs efter hello resursfilen är hämtade toohello nod. I det här fallet utför hello kommandoraden en obevakad installation av MS-MPI.

```csharp
// Create a StartTask for hello pool which we use for installing MS-MPI on
// hello nodes as they join hello pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit hello fully configured pool toohello Batch service tooactually create
// hello pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Direktåtkomst till fjärrminne (RDMA)
När du väljer en [RDMA-kompatibla storlek](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) som A9 för hello compute-noder i Batch-pool, MPI-program kan dra nytta av nätverk för Azures hög prestanda, låg latens direkt fjärrminne access (RDMA).

Leta efter hello-storlekar som angetts som ”RDMA-kompatibla” i hello följande artiklar:

* **CloudServiceConfiguration** pooler

  * [Storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md) (endast Windows)
* **VirtualMachineConfiguration** pooler

  * [Storlekar för virtuella datorer i Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)
  * [Storlekar för virtuella datorer i Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)

> [!NOTE]
> tootake nytta av RDMA på [Linux datornoder](batch-linux-nodes.md), måste du använda **Intel MPI** på hello noder. Mer information om CloudServiceConfiguration och VirtualMachineConfiguration pooler finns hello avsnittet av hello [Batch funktionsöversikt](batch-api-basics.md).
>
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>Skapa en flerinstans-uppgift med Batch .NET
Nu när vi har omfattas hello poolen krav och MPI installationen kan vi skapa hello flera instanser aktivitet. I det här kodstycket vi skapa en standard [CloudTask][net_task], konfigurera dess [MultiInstanceSettings] [ net_multiinstance_prop] egenskapen. Som tidigare nämnts hello flera instansuppgift är inte en distinkta aktivitetstyp, men en standard Batch-aktivitet som konfigurerats med inställningar för flera instanser.

```csharp
// Create hello multi-instance task. Its command line is hello "application command"
// and will be executed *only* by hello primary, and only after hello primary and
// subtasks execute hello CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure hello task's MultiInstanceSettings. hello CoordinationCommandLine will be executed by
// hello primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit hello task toohello job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on hello nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primära aktiviteten och underaktiviteterna
När du skapar hello flera instansinställningarna för en uppgift kan du ange hello antal compute-noder som är tooexecute hello aktivitet. När du skickar hello uppgiften tooa jobb hello Batch-tjänsten skapar en **primära** aktiviteten och det finns tillräckligt med **underaktiviteter** som matchar tillsammans hello antalet noder som du angav.

Dessa aktiviteter har tilldelats ett heltal-id i hello intervallet 0 för*numberOfInstances* - 1. hello uppgift med id 0 är hello primära och alla andra ID: n är underaktiviteter. Till exempel om du skapar hello följande inställningar för flera instanser för en uppgift hello främsta uppgiften skulle ha ett id 0 och hello underaktiviteter skulle ha ID 1 till 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Huvudnod
När du skickar en aktivitet med flera instanser hello Batch-tjänsten anger en hello compute-noder som hello ”huvudnoden” och scheman hello främsta uppgiften tooexecute på hello huvudnod. hello underaktiviteter är schemalagda tooexecute på hello resten av hello noder allokeras toohello flera instanser aktivitet.

## <a name="coordination-command"></a>Samordning kommando
Hej **samordning kommandot** körs både hello primära och underaktiviteter.

hello anrop av hello samordning kommandot blockerar--batchen inte körs kommandot för programmet hello tills hello samordning kommandot har returnerat har på alla underaktiviteter. hello samordning kommandot bör därför startar alla nödvändiga Bakgrundstjänster, kontrollera att de är klara för användning och avsluta. Till exempel kommandot samordning för en lösning med hjälp av MS-MPI version 7 startar hello SMPD-tjänsten på hello noden och sedan avslutas:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Observera hello användning av `start` i det här kommandot samordning. Detta är nödvändigt eftersom hello `smpd.exe` program inte returnerar omedelbart efter körning. Utan hello hello [starta] [ cmd_start] kommandot samordning kommandot returnerar inte och därför skulle blockera hello programmet kommandot körs.

## <a name="application-command"></a>Kommandot programmet
När hello främsta uppgiften och alla underaktiviteter är klar hello samordning kommando körs hello flera instanser aktivitetens kommandoraden körs av hello främsta uppgiften *endast*. Vi kallar detta hello **kommandot programmet** toodistinguish från hello samordning kommando.

MS-MPI program, Använd hello programmet kommandot tooexecute din MPI-aktiverat program med `mpiexec.exe`. Här är till exempel ett programkommando för en lösning med hjälp av MS-MPI version 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> Eftersom MS-MPI `mpiexec.exe` använder hello `CCP_NODES` variabeln som standard (se [miljövariabler](#environment-variables)) hello-exemplet ovan kommandoraden för programmet utesluter den.
>
>

## <a name="environment-variables"></a>Miljövariabler
Batch skapar flera [miljövariabler] [ msdn_env_var] specifika toomulti-instans uppgifter på hello compute-noder allokeras tooa flera instanser aktivitet. De här miljövariablerna kan referera till din kommandorader samordning och program som kan hello skript och program som de körs.

hello skapas följande miljövariabler av hello Batch-tjänsten för användning av flera instanser uppgifter:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Fullständig information om detta och hello andra Batch compute-nod miljövariabler, inklusive innehållet och synlighet finns [Compute-nod miljövariabler][msdn_env_var].

> [!TIP]
> hello Batch Linux MPI kodexemplet innehåller ett exempel på hur många av de här miljövariablerna kan användas. Hej [samordning cmd] [ coord_cmd_example] Bash skript hämtar vanlig tillämpning och indatafilerna från Azure Storage, aktiverar en Network File System (NFS) nätverksresurs på hello huvudnod och konfigurerar hello andra noder allokerat toohello flera instanser aktivitet som NFS-klienterna.
>
>

## <a name="resource-files"></a>Resursfiler
Det finns två uppsättningar av resursen filer tooconsider för flera instanser uppgifter: **vanliga resursfiler** som *alla* uppgifter hämta (både primär och underaktiviteter), och hello **resursfiler** angetts för hello flera instanser uppgift sig själv, vilket *endast hello primära* uppgift hämtningar.

Du kan ange en eller flera **vanliga resursfiler** i inställningarna för hello flera instanser för en aktivitet. Dessa vanliga resursfiler hämtas från [Azure Storage](../storage/common/storage-introduction.md) på varje nod **aktivitet delade katalogen** genom hello primära och alla underaktiviteter. Du kan komma åt hello uppgiften delad katalog från program och samordning kommandorader med hello `AZ_BATCH_TASK_SHARED_DIR` miljövariabeln. Hej `AZ_BATCH_TASK_SHARED_DIR` sökvägen är identiska på varje nod allokerade toohello flera instanser aktivitet, vilket du kan dela ett enda samordning kommando mellan hello primära och alla underaktiviteter. Batch ”delar inte” hello katalogen i en mening för fjärråtkomst, men du kan använda den som en monteringspunkt eller dela punkt som nämnts tidigare i hello tips på miljövariabler.

Resursfiler som du anger för hello själva flera instanser aktiviteten är hämtade toohello aktivitet arbetskatalogen `AZ_BATCH_TASK_WORKING_DIR`, som standard. Som tidigare nämnts kan däremot toocommon resursfiler endast hello primära aktiviteten hämtar resursfiler som angetts för hello flera instanser själva aktiviteten.

> [!IMPORTANT]
> Använd alltid hello miljövariabler `AZ_BATCH_TASK_SHARED_DIR` och `AZ_BATCH_TASK_WORKING_DIR` toorefer toothese kataloger i din kommandorader. Försök inte tooconstruct hello sökvägar manuellt.
>
>

## <a name="task-lifetime"></a>Livslängd för aktiviteten
hello livstid hello främsta uppgiften kontroller hello livstid hello hela flera instanser aktivitet. När primära hello avslutas avslutas alla hello underaktiviteter. hello avslutningskoden hello primära är hello avslutningskoden hello aktivitet och är därför används toodetermine hello lyckad eller misslyckad hello aktivitet för försök igen.

Om någon av hello underaktiviteter misslyckas misslyckas avslutades med koden inte är noll, till exempel hello hela flera instanser. hello flera instansuppgift är sedan avslutas och igen in tooits gränsen för återförsök.

När du tar bort en aktivitet med flera instanser bort hello primära och alla underaktiviteter också av hello Batch-tjänsten. Alla underaktivitet kataloger och filer tas bort från hello compute-noder, precis som för en aktivitet som standard.

[TaskConstraints] [ net_taskconstraints] för en aktivitet med flera instanser, till exempel hello [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], och [RetentionTime] [ net_taskconstraint_retention] gäller egenskaper, som de är för en aktivitet som standard och tillämpa toohello primära servern och alla underaktiviteter. Men om du ändrar hello [RetentionTime] [ net_taskconstraint_retention] egenskapen när du lägger till hello flera instanser toohello aktiviteten, den här ändringen är tillämpade endast toohello främsta uppgiften. Alla hello underaktiviteter fortsätta toouse hello ursprungliga [RetentionTime][net_taskconstraint_retention].

En beräkningsnod senaste uppgiftslista visar hello-id för en delaktivitet om hello aktivitet var en del av en aktivitet med flera instanser.

## <a name="obtain-information-about-subtasks"></a>Hämta information om underaktiviteter
tooobtain information om underaktiviteter genom att använda hello Batch .NET-bibliotek, anrop hello [CloudTask.ListSubtasks] [ net_task_listsubtasks] metod. Den här metoden returnerar information om alla underaktiviteter och information om hello beräkningsnod som körts hello uppgifter. Från den här informationen kan bestämma du rotkatalogen för varje underaktivitet, hello programpools-id, det aktuella tillståndet, slutkod med mera. Du kan använda den här informationen i kombination med hello [PoolOperations.GetNodeFile] [ poolops_getnodefile] metoden tooobtain hello underaktivitets filer. Observera att den här metoden inte returnera information om hello primära aktivitet (id 0).

> [!NOTE]
> Om inte annat anges hello Batch .NET-metoder som fungerar på flera instanser [CloudTask] [ net_task] själva gäller *endast* toohello främsta uppgiften. Till exempel när du anropar hello [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metoden för en uppgift med flera instanser returneras endast hello främsta uppgiften filer.
>
>

hello följande kodavsnitt visar hur tooobtain underaktivitet information, samt begära filinnehållet från hello-noder som de körs.

```csharp
// Obtain hello job and hello multi-instance task from hello Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain hello list of subtasks for hello task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over hello subtasks and print their stdout and stderr
// output if hello subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Kodexempel
Hej [MultiInstanceTasks] [ github_mpi] kodexempel på GitHub visar hur toouse flera instanser uppgift toorun en [MS-MPI] [ msmpi_msdn] programmet på Batch-beräkningsnoder. Gör så hello i [förberedelse](#preparation) och [körning](#execution) toorun hello exempel.

### <a name="preparation"></a>Förberedelse
1. Följ hello två första stegen i [hur toocompile och köra ett enkelt program MS-MPI][msmpi_howto]. Det här uppfyller hello prerequesites för hello följande steg.
2. Skapa en *versionen* version av hello [MPIHelloWorld] [ helloworld_proj] MPI exempelprogrammet. Detta är hello-program som körs på datornoderna av hello flera instanser aktivitet.
3. Skapa en zip-filen med `MPIHelloWorld.exe` (som du skapat i steg 2) och `MSMpiSetup.exe` (som du hämtade i steg 1). Du måste ladda upp zip-fil som ett programpaket i hello nästa steg.
4. Använd hello [Azure-portalen] [ portal] toocreate en Batch [programmet](batch-application-packages.md) kallas ”MPIHelloWorld” och ange hello zip-filen som du skapade i föregående steg i hello versionsnummer ”1.0” hello programpaket. Se [ladda upp och hantera program](batch-application-packages.md#upload-and-manage-applications) för mer information.

> [!TIP]
> Skapa en *versionen* version av `MPIHelloWorld.exe` så att du inte har tooinclude eventuella ytterligare beroenden (till exempel `msvcp140d.dll` eller `vcruntime140d.dll`) i programpaketet.
>
>

### <a name="execution"></a>Körning
1. Hämta hello [azure-batch-samples] [ github_samples_zip] från GitHub.
2. Öppna hello MultiInstanceTasks **lösning** i Visual Studio 2015 eller senare. Hej `MultiInstanceTasks.sln` lösningsfilen finns:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. Ange Batch- och autentiseringsuppgifterna för ditt konto i `AccountSettings.settings` i hello **Microsoft.Azure.Batch.Samples.Common** projekt.
4. **Skapa och köra** hello MultiInstanceTasks lösning tooexecute hello MPI exempelprogrammet på compute-noder i en Batch-pool.
5. *Valfria*: Använd hello [Azure-portalen] [ portal] eller hello [Batch Explorer] [ batch_explorer] tooexamine hello exempel pool, jobb, och uppgiften (”MultiInstanceSamplePool”, ”MultiInstanceSampleJob”, ”MultiInstanceSampleTask”) innan du tar bort hello resurser.

> [!TIP]
> Du kan hämta [Visual Studio Community] [ visual_studio] kostnadsfritt om du inte har Visual Studio.
>
>

Utdata från `MultiInstanceTasks.exe` är liknande toohello följande:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] toojob [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks toocomplete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a>Nästa steg
* hello Microsoft HPC & Azure Batch-teamets blogg beskrivs [MPI stöd för Linux på Azure Batch][blog_mpi_linux], och innehåller information om hur du använder [OpenFOAM] [ openfoam] med Batch. Du kan hitta Python-kodexempel för hello [OpenFOAM exempel på GitHub][github_mpi].
* Lär dig hur för[skapa pooler med Linux datornoderna](batch-linux-nodes.md) för användning i Azure Batch MPI-lösningar.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Översikt över flera instanser"
