---
title: "aaaPersist projekt- och utdata tooAzure lagring med hello filen konventioner bibliotek för .NET - Azure Batch | Microsoft Docs"
description: "Lär dig hur toouse filen konventioner för Azure Batch-biblioteket för .NET toopersist batchaktiviteten och jobbet utdata tooAzure lagring och visa hello beständiga utdata i hello Azure-portalen."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cf2ac8632a13d32438c1bdcf11b4b9649de1e2b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="persist-job-and-task-data-tooazure-storage-with-hello-batch-file-conventions-library-for-net-toopersist"></a>Spara jobbet och uppgift data tooAzure lagring med hello Batch filen konventioner biblioteket för .NET toopersist 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Enkelriktade toopersist aktivitetsdata är toouse hello [filen konventioner för Azure Batch-biblioteket för .NET][nuget_package]. hello filen konventioner bibliotek förenklar hello processen att lagra uppgiftsutdata data tooAzure lagring och hämtning av den. Du kan använda hello filen konventioner biblioteket i både uppgiften och klienten kod &mdash; uppgiftskoden för spara filer och klienten Platskod toolist och hämta dem. Uppgiftskoden kan också använda hello biblioteket tooretrieve hello utdata överordnade uppgifter, som i en [uppgift beroenden](batch-task-dependencies.md) scenario. 

tooretrieve utdatafiler med hello filen konventioner bibliotek, du kan hitta hello filer för ett visst jobb eller en aktivitet genom att registrera dem med ID: T och syftet. Du behöver inte tooknow hello namn eller platser hello-filer. Du kan till exempel använda hello filen konventioner biblioteket toolist alla mellanliggande filer för en viss uppgift eller få en preview-fil för ett visst jobb.

> [!TIP]
> Från och med version 2017-05-01, stöder hello API för Batch-tjänsten bestående utgående data tooAzure lagring för aktiviteter och jobb manager aktiviteter som körs på pooler som har skapats med hello konfiguration av virtuell dator. hello API för Batch-tjänsten tillhandahåller ett enkelt sätt toopersist utdata från inom hello-kod som skapar en uppgift och fungerar som ett alternativt toohello filen konventioner bibliotek. Du kan ändra din Batch klienten program toopersist utdata utan att behöva tooupdate hello program som uppgiften körs. Mer information finns i [spara aktiviteten data tooAzure lagring med hello API för Batch-tjänsten](batch-task-output-files.md).
> 
> 

## <a name="when-do-i-use-hello-file-conventions-library-toopersist-task-output"></a>När använder hello filen konventioner biblioteket toopersist uppgiftens utdata?

Azure Batch tillhandahåller mer än ett sätt toopersist uppgiftens utdata. hello filen konventioner är bäst lämpade toothese scenarier:

- Du kan enkelt ändra hello koden för hello programmet att uppgiften körs toopersist filer med hjälp av hello filen konventioner bibliotek.
- Du vill toostream data tooAzure lagring medan hello aktivitet körs.
- Vill du toopersist data från poolen som skapats med hello molnet tjänstkonfiguration eller hello konfiguration av virtuell dator.
- Klientprogrammet eller andra uppgifter i hello jobb behov toolocate och hämta utdata aktivitetsfiler genom ID eller syfte. 
- Vill du tooview uppgiftens utdata i hello Azure-portalen.

Om ditt scenario skiljer sig från de som visas ovan, kanske du måste tooconsider en annan metod. Mer information om andra alternativ för bestående uppgiftsutdata finns [spara projekt- och utdata tooAzure lagring](batch-task-output.md). 

## <a name="what-is-hello-batch-file-conventions-standard"></a>Vad är hello Batch filen konventioner standard?

Hej [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) innehåller ett namngivningsschema för hello mål behållare och blob sökvägar toowhich skrivs utdatafiler. Filer beständiga tooAzure lagring som följer toohello filen konventioner som standard är automatiskt tillgängliga för visning i hello Azure-portalen. hello portalen är medveten om hello namngivningskonvention och därför kan visa filer som följer tooit.

hello filen konventioner biblioteket för .NET namn automatiskt din behållare för lagring och uppgiften utdatafilerna enligt toohello filen konventioner som standard. hello filen konventioner biblioteket innehåller också metoder tooquery utdatafilerna i Azure Storage enligt toojob ID, aktivitets-ID eller syfte.   

Om du utvecklar med ett annat språk än .NET, kan du implementera hello filen konventioner standard själv i ditt program. Mer information finns i [om hello Batch filen konventioner standard](batch-task-output.md#about-the-batch-file-conventions-standard).

## <a name="link-an-azure-storage-account-tooyour-batch-account"></a>Länka ett Azure Storage-konto tooyour Batch-kontot

toopersist utdata data tooAzure lagring med hjälp av hello filen konventioner bibliotek, måste du först koppla ett Azure Storage-konto tooyour Batch-kontot. Om du inte redan har gjort länka en Storage-konto tooyour Batch-kontot med hjälp av hello [Azure-portalen](https://portal.azure.com):

1. Navigera tooyour Batch-kontot i hello Azure-portalen. 
2. Under **inställningar**väljer **Lagringskonto**.
3. Om du inte redan har ett lagringskonto som är associerade med Batch-kontot, klickar du på **Storage-konto (ingen)**.
4. Välj ett lagringskonto hello listan för din prenumeration. För bästa prestanda använder du ett Azure Storage-konto som är i hello samma region som hello Batch-kontot var dina aktiviteter körs.

## <a name="persist-output-data"></a>Spara utdata

toopersist projekt- och utdata med hello filen konventioner bibliotek, skapa en behållare i Azure Storage och sedan spara hello utdata toohello behållare. Använd hello [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage) i din aktivitet kod tooupload hello uppgiften utdata toohello behållaren. 

Mer information om hur du arbetar med behållare och blobbar i Azure Storage finns [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).

> [!WARNING]
> Alla utdata för jobb- och beständiga med hello filen konventioner biblioteket lagras i hello samma behållare. Om ett stort antal uppgifter försök toopersist filer på hello samma tid, [lagring throttling-begränsning](../storage/common/storage-performance-checklist.md#blobs) eventuellt.
> 
> 

### <a name="create-storage-container"></a>Skapa lagringsbehållaren

toopersist aktivitet utdata tooAzure lagring, först skapa en behållare genom att anropa [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync]. Den här tilläggsmetoden tar en [CloudStorageAccount] [ net_cloudstorageaccount] objekt som parameter. En behållare med namnet bl.a toohello filen konventioner standard, så att innehållet är synliga genom hello Azure-portalen och hello hämtning av metoderna som beskrivs senare i artikeln hello skapas.

Du vanligtvis placera hello koden toocreate en behållare i ditt klientprogram &mdash; hello program som skapar din pooler, jobb och uppgifter.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference toohello linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create hello blob storage container for hello outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Lagra utdata för aktiviteten

Nu när du har skapat en behållare i Azure Storage uppgifter kan spara utdata toohello behållare med hello [TaskOutputStorage] [ net_taskoutputstorage] klass hittades i hello filen konventioner bibliotek.

I koden aktivitet, först skapa en [TaskOutputStorage] [ net_taskoutputstorage] objekt och sedan när hello har slutförts sitt arbete, anropa hello [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] metoden toosave dess utdata tooAzure lagring.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code tooprocess data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

Hej `kind` parametern för hello [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) metoden kategoriserar hello beständiga filer. Det finns fyra fördefinierade [TaskOutputKind] [ net_taskoutputkind] typer: `TaskOutput`, `TaskPreview`, `TaskLog`, och `TaskIntermediate.` du kan också definiera egna kategorier av utdata.

Dessa utdata-typer kan du toospecify vilken typ av utdata toolist när du senare frågar Batch för hello beständiga utdata för en viss uppgift. Med andra ord, när du listar hello utdata för en aktivitet, kan du filtrera hello på någon av hello utdata typer. Till exempel ”ge mig hello *preview* utdata för aktiviteten *109*”. Mer om lista och hämta utdata visas i [hämta utdata](#retrieve-output) senare i hello artikeln.

> [!TIP]
> hello utdata typ avgör också var i hello Azure portal en viss fil visas: *TaskOutput*-kategoriserade filer visas **aktivitet utdatafiler**, och *TaskLog*filer visas **uppgift loggar**.
> 
> 

### <a name="store-job-outputs"></a>Lagra utdata för jobbet

Dessutom toostoring resultat av åtgärder, kan du lagra hello utdata som är associerade med en hela jobbet. I hello merge uppgiften för ett jobb för återgivning av film kan du spara hello fullständigt renderas film som en jobbutdata. När jobbet har slutförts, ditt klientprogram kan visa och hämta hello utdata för hello jobb och har inte behöver tooquery hello enskilda aktiviteter.

Lagra jobbutdata genom att anropa hello [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] metod, och ange hello [JobOutputKind] [ net_joboutputkind] och filnamn:

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Precis som med hello **TaskOutputKind** typen för aktiviteten utdata, använder du hello [JobOutputKind] [ net_joboutputkind] typen toocategorize ett jobb har sparat filer. Den här parametern kan du fråga toolater (lista) en viss typ av utdata. Hej **JobOutputKind** typ innehåller både utdata och förhandsgranska kategorier och kan du skapa egna kategorier.

### <a name="store-task-logs"></a>Lagrar uppgiften loggar

Dessutom toopersisting en toodurable fillagring när en aktivitet eller jobbet har slutförts kan du behöva toopersist filer som uppdateras under hello körning av uppgiften &mdash; loggfiler eller `stdout.txt` och `stderr.txt`, till exempel. För detta ändamål hello Azure Batch filen konventioner-bibliotek innehåller hello [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] metod. Med [SaveTrackedAsync][net_savetrackedasync], kan du spåra uppdateringar tooa fil på hello nod (på ett intervall som du anger) och spara dessa uppdateringar tooAzure lagring.

I följande kodavsnitt hello, vi använda [SaveTrackedAsync] [ net_savetrackedasync] tooupdate `stdout.txt` i Azure Storage var 15: e sekund vid hello körning av aktiviteten hello:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// hello primary task logic is wrapped in a using statement that sends updates to
// hello stdout.txt blob in Storage every 15 seconds while hello task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code tooprocess data and produce output file(s) */

    // We are tracking hello disk file toosave our standard output, but the
    // node agent may take up too3 seconds tooflush hello stdout stream to
    // disk. So give hello file a moment toocatch up.
     await Task.Delay(stdoutFlushDelay);
}
```

hello kommenterats avsnittet `Code tooprocess data and produce output file(s)` är en platshållare för hello-kod som normalt skulle utföra uppgiften. Du kan till exempel ha kod som hämtar data från Azure Storage och utför transformering eller beräkning på den. hello viktig del av den här fragment visar hur du kan anpassa sådan kod i en `using` block tooperiodically uppdatera en fil med [SaveTrackedAsync][net_savetrackedasync].

hello nod agent är ett program som körs på varje nod i hello pool och tillhandahåller hello och kommandokontroll gränssnitt mellan hello nod och hello Batch-tjänsten. Hej `Task.Delay` anropet krävs hello slutet av det här `using` block tooensure som hello nod agent har tid tooflush hello innehållet i standard ut toohello stdout.txt fil på hello-nod. Utan den här fördröjningen är det möjligt toomiss hello senaste några sekunder för utdata. Den här fördröjningen kan inte krävas för alla filer.

> [!NOTE]
> När du aktiverar spårning med filen **SaveTrackedAsync**, endast *lägger till* toohello spårade filen är beständiga tooAzure lagring. Använd den här metoden endast för spårning av icke-rotera loggfiler eller andra filer som har skrivits toowith lägga till operations hello filens toohello slut.
> 
> 

## <a name="retrieve-output-data"></a>Hämta utdata

När du hämtar utdatan beständiga använder hello Azure Batch filen konventioner bibliotek, gör du det i en aktivitet till och jobbet-central sätt. Du kan begära hello utdata för det angivna aktivitets- eller utan att behöva tooknow dess sökväg i Azure Storage eller även dess filnamn. Du kan i stället begära utdatafilerna av aktivitet eller jobb-ID.

hello följande kodavsnitt går igenom ett jobb uppgifter, skriver ut information om hello utdatafilerna för hello aktivitet och hämtar sedan dess filer från lagring.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (OutputFileReference output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="view-output-files-in-hello-azure-portal"></a>Visa utdatafiler i hello Azure-portalen

hello Azure-portalen visar aktivitet utdatafilerna och loggar som är beständiga tooa länkad Azure Storage-konto med hello [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). Du kan också implementera dessa konventioner i hello ett annat språk eller du kan använda hello filen konventioner biblioteket i .NET-program.

tooenable hello visningen av dina utdatafiler i hello portal, måste du uppfylla hello följande krav:

1. [Koppla ett Azure Storage-konto](#requirement-linked-storage-account) tooyour Batch-kontot.
2. Följa toohello fördefinierade regler för namngivning av behållare för lagring och filer när beständighet utdata. Du hittar hello definition av dessa konventioner i hello filen konventioner biblioteket [viktigt][github_file_conventions_readme]. Om du använder hello [Azure Batch filen konventioner] [ nuget_package] biblioteket toopersist din utdata, dina filer sparas enligt toohello filen konventioner som standard.

tooview uppgiftsutdata filer och loggar in hello Azure-portalen, navigera toohello aktivitet vars utdata som du är intresserad av, klickar sedan på antingen **sparad utdatafilerna** eller **sparade loggarna**. Den här bilden visar hello **sparad utdatafilerna** för hello uppgiften med ID ”007”:

![Resultat av åtgärder bladet i hello Azure-portalen][2]

## <a name="code-sample"></a>Kodexempel

Hej [PersistOutputs] [ github_persistoutputs] exempelprojektet är en av hello [kodexempel för Azure Batch] [ github_samples] på GitHub. Den här Visual Studio-lösningen visar hur toouse hello Azure Batch filen konventioner biblioteket toopersist aktivitet utdata toodurable lagring. toorun hello exempel, följer du dessa steg:

1. Öppna hello-projekt i **Visual Studio 2015 eller senare**.
2. Lägg till grupp och lagring **kontoautentiseringsuppgifter** för**AccountSettings.settings** i hello Microsoft.Azure.Batch.Samples.Common projekt.
3. **Skapa** (men inte kör) hello lösning. Återställa NuGet-paket om du uppmanas till detta.
4. Använd hello Azure portal tooupload en [programpaket](batch-application-packages.md) för **PersistOutputsTask**. Inkludera hello `PersistOutputsTask.exe` och dess beroende sammansättningar i hello ZIP-paketet, ange hello program-ID för ”PersistOutputsTask” och programmet hello Paketversion för ”1.0”.
5. **Starta** (kör) hello **PersistOutputs** projekt.
6. När begärd toochoose hello beständiga teknik toouse körs hello exempel anger **1** toorun hello exempel med hjälp av hello filen konventioner biblioteket toopersist uppgiftens utdata. 

## <a name="next-steps"></a>Nästa steg

### <a name="get-hello-batch-file-conventions-library-for-net"></a>Hämta hello Batch filen konventioner bibliotek för .NET

hello Batch filen konventioner bibliotek för .NET är tillgängligt på [NuGet][nuget_package]. hello biblioteket utökar hello [CloudJob] [ net_cloudjob] och [CloudTask] [ net_cloudtask] klasser med nya metoder. Se även hello [refererar dokumentationen](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) för hello filen konventioner biblioteket.

Hej [källkod] [ github_file_conventions] för hello filen konventioner bibliotek är tillgängligt på GitHub i hello Microsoft Azure SDK för .NET-databasen. 

### <a name="explore-other-approaches-for-persisting-output-data"></a>Utforska andra metoder för bestående av utdata

- Se [spara projekt- och utdata tooAzure lagring](batch-task-output.md) för en översikt över bestående aktivitets- och data.
- Se [spara aktiviteten data tooAzure lagring med hello API för Batch-tjänsten](batch-task-output-files.md) toolearn hur toouse hello API för Batch-tjänsten toopersist utdata.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Sparad utdatafiler och sparade loggarna väljare i portalen"
[2]: ./media/batch-task-output/task-output-02.png "Resultat av åtgärder bladet i hello Azure-portalen"
