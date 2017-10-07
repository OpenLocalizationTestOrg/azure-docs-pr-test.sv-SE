---
title: "aaaUse aktivitet beroenden toorun aktiviteter baserat på hello slutförande av andra åtgärder - Azure Batch | Microsoft Docs"
description: "Skapa uppgifter som är beroende av hello slutförande av andra uppgifter för bearbetning av MapReduce format och liknande stordata för arbetsbelastningar i Azure Batch."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: b8d12db5-ca30-4c7d-993a-a05af9257210
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: faf08ec38cb30b1f66acd51e256c31aea6215c62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-task-dependencies-toorun-tasks-that-depend-on-other-tasks"></a>Skapa uppgift beroenden toorun uppgifter som är beroende av andra aktiviteter

Du kan definiera uppgiften beroenden toorun en aktivitet eller en uppsättning uppgifter som endast när en överordnad aktivitet har slutförts. Vissa scenarier där aktiviteten beroenden är användbara är:

* MapReduce-format arbetsbelastningar i hello molnet.
* Jobb vars databearbetningsuppgifter kan uttryckas som en riktat acykliskt diagram (DAG).
* Förrendering och efter återgivning processer, där varje uppgift måste slutföras innan du kan börja hello nästa uppgift.
* Ett annat jobb som underordnade aktiviteter beroende hello utdata från överordnad uppgifter.

Du kan skapa aktiviteter som är schemalagda för körning på datornoderna efter hello slutförande av en eller flera överordnade uppgifter med Batch uppgiften beroenden. Du kan till exempel skapa ett jobb som återger varje bildruta i en 3D-film med separat, parallella aktiviteter. hello sista aktivitet - hello ”merge task”--sammanslagningar hello renderas ramar hello fullständig film när alla ramar har gjorts.

Som standard är beroende aktiviteter schemalagda för körning endast när hello överordnade uppgiften har slutförts. Du kan ange ett beroende åtgärden toooverride hello standardbeteende och köra uppgifter om hello överordnade misslyckas. Se hello [beroende åtgärder](#dependency-actions) information.  

Du kan skapa uppgifter som är beroende av andra aktiviteter i en-till-en- eller en-till-många-relation. Du kan också skapa ett intervall beroende där en aktivitet beror på hello slutförande av en grupp aktiviteter inom ett angivet intervall för aktiviteten ID: N. Du kan kombinera dessa tre grundläggande scenarier toocreate många-till-många-relationer.

## <a name="task-dependencies-with-batch-net"></a>Beroenden för uppgift med Batch .NET
I den här artikeln tar vi upp hur tooconfigure samband med hjälp av hello [Batch .NET] [ net_msdn] bibliotek. Vi först att visa hur för[aktivera sambandet](#enable-task-dependencies) för dina projekt och sedan visa hur för[konfigurera en aktivitet med beroenden](#create-dependent-tasks). Vi beskriver också hur toospecify beroende åtgärden toorun beroende-uppgifter om hello överordnade misslyckas. Dessutom diskuterar vi hello [beroende scenarier](#dependency-scenarios) som har stöd för Batch.

## <a name="enable-task-dependencies"></a>Aktivera aktivitet beroenden
toouse aktivitet beroenden i Batch-program, måste du först konfigurera hello jobbet toouse aktivitet beroenden. I Batch .NET aktivera det på din [CloudJob] [ net_cloudjob] genom att ange dess [UsesTaskDependencies] [ net_usestaskdependencies] egenskapen för`true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

I föregående kodfragment hello, ”batchClient” är en instans av hello [BatchClient] [ net_batchclient] klass.

## <a name="create-dependent-tasks"></a>Skapa beroende aktiviteter
toocreate en aktivitet som är beroende av hello slutförande av en eller flera överordnade uppgifter du kan ange andra uppgifter som hello uppgiften ”beror på” hello. Konfigurera hello i Batch .NET [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] egenskap med en instans av hello [TaskDependencies] [ net_taskdependencies] klass:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Det här kodstycket skapas en beroende aktivitet med aktivitets-ID ”blommor”. hello beroende ”blommor” uppgiften aktiviteter ”regn” och ”Sun”. Uppgiften ”blommor” är schemalagd toorun på en beräkningsnod bara efter aktiviteter som ”regn” och ”Sun” har slutförts.

> [!NOTE]
> En aktivitet anses toobe har slutförts när den är i hello **slutförts** tillstånd och dess **slutkod** är `0`. I Batch .NET detta innebär en [CloudTask][net_cloudtask].[ Tillstånd] [ net_taskstate] egenskapsvärdet `Completed` och hello Cloudtask's [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] egenskapsvärdet är `0`.
> 
> 

## <a name="dependency-scenarios"></a>Beroende scenarier
Det finns tre grundläggande uppgiften beroende scenarier som du kan använda i Azure Batch: 1, en-till-många och aktivitets-ID vara beroende. Dessa kan vara kombinerade tooprovide en fjärde scenario, många-till-många.

| Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Exempel |  |
|:---:| --- | --- |
|  [1](#one-to-one) |*taskB* beror på *taskA* <p/> *taskB* kommer inte schemaläggas för körning tills *taskA* har slutförts |![Diagram: en uppgift beroende][1] |
|  [En-till-många](#one-to-many) |*aktivitetC* är beroende av både *aktivitetA* och *aktivitetB*. <p/> *taskC* kommer inte schemaläggas för körning förrän både *taskA* och *taskB* har slutförts |![Diagram: en-till-många-uppgiften beroende][2] |
|  [ID-intervall för aktiviteten](#task-id-range) |*taskD* beror på ett antal uppgifter <p/> *taskD* kommer inte schemaläggas för körning tills hello uppgifter med ID: N *1* via *10* har slutförts |![Diagram: ID-intervallet sambandet][3] |

> [!TIP]
> Du kan skapa **många-till-många** relationer, till exempel där uppgifter C, D, E och F varje beroende aktiviteter A och b Detta är användbart, till exempel i paralleliserad förbearbetning scenarier där underordnade aktiviteter är beroende av hello utdata från flera överordnade uppgifter.
> 
> I hello exemplen i det här avsnittet körs en beroende aktivitet endast när hello överordnade uppgifter slutföras. Det här beteendet är hello standardbeteendet för beroende aktivitet. Du kan köra en beroende aktivitet när en överordnad aktivitet inte genom att ange ett beroende åtgärden toooverride hello standardbeteendet. Se hello [beroende åtgärder](#dependency-actions) information.

### <a name="one-to-one"></a>1
En aktivitet beror på hello slutförande av en överordnad aktivitet i en-till-en-relation. toocreate Hej beroende, ange en enskild uppgift ID toohello [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] statisk metod när du fylla i hello [DependsOn] [ net_dependson] -egenskapen för [CloudTask] [ net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>En-till-många
En aktivitet beror på hello slutförande av flera överordnade uppgifter i en en-till-många-relation. toocreate Hej beroende, tillhandahåller en uppsättning aktivitet-ID: N toohello [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] statisk metod när du fylla i hello [DependsOn] [ net_dependson] -egenskapen för [CloudTask] [ net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
``` 

### <a name="task-id-range"></a>ID-intervall för aktiviteten
En aktivitet beror på hello hello slutförande av uppgifter vars ID-nummer som ligger inom ett intervall för ett beroende på ett intervall med överordnade uppgifter.
toocreate hello beroende ange hello första och sista uppgifts-ID i hello intervallet toohello [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] statisk metod när du fylla i hello [DependsOn] [ net_dependson] -egenskapen för [CloudTask] [net_cloudtask].

> [!IMPORTANT]
> När du använder aktiviteten ID-intervall för dina beroenden hello aktivitets-ID i hello intervallet *måste* vara en sträng som representerar heltalsvärden.
> 
> Varje aktivitet i hello intervallet måste uppfylla hello beroende av slutförs eller genom att fylla i med ett fel som är mappade tooa beroende åtgärd som har angetts för**Satisfy**. Se hello [beroende åtgärder](#dependency-actions) information.
>
>

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // toouse a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters tooTaskIdRange,
    // but their ids (above) are string representations of hello ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a>Beroende åtgärder

Som standard körs en beroende aktivitet eller uppsättning aktiviteter bara när en överordnad aktivitet har slutförts. I vissa fall kan kanske du vill toorun beroende aktiviteter även om hello överordnad aktivitet misslyckas. Du kan åsidosätta standardbeteendet för hello genom att ange en beroende-åtgärd. En beroende åtgärd anger om en beroende aktivitet är berättigad toorun, baserat på hello lyckad eller misslyckad hello överordnad aktivitet. 

Anta exempelvis att en beroende aktivitet väntar på data från hello hello överordnad aktivitet har slutförts. Om hello överordnade misslyckas kanske ändå hello beroende aktivitet kan toorun med äldre data. I det här fallet kan en beroende-åtgärd ange hello beroende aktiviteten berättigade toorun trots hello fel i hello överordnad aktivitet.

En beroende-åtgärden är baserad på ett avsluta-villkor för hello överordnade aktivitet. Du kan ange en beroende-åtgärd för någon av följande avslutningsvillkor; hello för .NET, se hello [ExitConditions] [ net_exitconditions] klass för ytterligare information:

- När en före bearbetning fel uppstår.
- Felet uppstår när en filöverföring. Om hello aktivitet avslutas med slutkoden som angetts via **exitCodes** eller **exitCodeRanges**, och det uppstår ett fel vid, hello åtgärden som anges av hello avsluta koden har företräde.
- När hello aktivitet avslutas med slutkoden definieras av hello **ExitCodes** egenskapen.
- När hello aktivitet avslutas med slutkoden som ligger inom ett intervall som anges av hello **ExitCodeRanges** egenskapen.
- Hej standardfallet, om hello aktivitet avslutas med slutkoden inte definieras av **ExitCodes** eller **ExitCodeRanges**, eller om hello aktivitet avslutas med en före bearbetning fel och hello **PreProcessingError**  egenskapen har inte angetts eller om hello uppgiften misslyckas med en fil överför fel och hello **FileUploadError** egenskapen har inte angetts. 

toospecify en beroende-åtgärd i .NET, ange hello [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] -egenskapen för hello avsluta-villkor. Hej **DependencyAction** egenskap tar ett av två värden:

- Inställningen hello **DependencyAction** egenskapen för**Satisfy** indikerar att beroende aktiviteter är berättigad toorun om hello överordnad aktivitet avslutas med ett angivet fel.
- Inställningen hello **DependencyAction** egenskapen för**Block** visar att beroende aktiviteter inte berättigad toorun.

Hej standardinställningen för hello **DependencyAction** egenskapen är **Satisfy** för slutkoden 0, och **Block** för alla andra avslutningsvillkor.

hello följande kodavsnitt anger hello **DependencyAction** -egenskapen för en överordnad aktivitet. Om hello överordnad aktivitet avslutas med ett före bearbetning fel eller med hello angivna felkoder hello beroende har aktivitet blockerats. Om hello överordnad aktivitet avslutas med ett noll-fel, är berättigad toorun hello beroende aktivitet.

```csharp
// Task A is hello parent task.
new CloudTask("A", "cmd.exe /c echo A")
{
    // Specify exit conditions for task A and their dependency actions.
    ExitConditions = new ExitConditions
    {
        // If task A exits with a pre-processing error, block any downstream tasks (in this example, task B).
        PreProcessingError = new ExitOptions
        {
            DependencyAction = DependencyAction.Block
        },
        // If task A exits with hello specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible toorun 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible toorun depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a>Kodexempel
Hej [TaskDependencies] [ github_taskdependencies] exempelprojektet är en av hello [kodexempel för Azure Batch] [ github_samples] på GitHub. Visual Studio-lösning visar:

- Hur tooenable uppgift beroende på ett jobb
- Hur toocreate uppgifter som är beroende av andra aktiviteter
- Hur tooexecute de aktiviteter på en pool med compute-noder.

## <a name="next-steps"></a>Nästa steg
### <a name="application-deployment"></a>Programdistribution
Hej [programpaket](batch-application-packages.md) funktion i Batch ger en enkelt distribuera tooboth och version hello-program som dina aktiviteter att köra compute-noder.

### <a name="installing-applications-and-staging-data"></a>Installera program och mellanlagring av data
Se [installera program och mellanlagring av data på Batch-beräkningsnoder] [ forum_post] i hello Azure Batch-forum för en översikt över metoderna för att förbereda din noder toorun uppgifter. Skrivs av en av hello Azure Batch-gruppmedlemmar, på det här är en bra introduktion på hello olika sätt toocopy program compute indata för aktivitet och andra filer tooyour-noder.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_exitconditions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitconditions
[net_exitoptions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions
[net_dependencyaction]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions#Microsoft_Azure_Batch_ExitOptions_DependencyAction
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: 1-samband"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: en-till-många-beroende"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: ID-intervallet sambandet"
