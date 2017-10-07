---
title: "aaaPersist projekt- och utdata tooAzure lagring med hello Azure Batch-tjänsten API | Microsoft Docs"
description: "Lär dig hur toouse API för Batch-tjänsten toopersist batchaktiviteten och jobbet utdata tooAzure lagring."
services: batch
author: tamram
manager: timlt
editor: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.openlocfilehash: 71b3f7c0dda2d2a9d8eb3eef83229873c70ca22c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="persist-task-data-tooazure-storage-with-hello-batch-service-api"></a>Spara aktiviteten data tooAzure lagring med hello API för Batch-tjänsten

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Från och med version 2017-05-01, stöder hello API för Batch-tjänsten bestående utgående data tooAzure lagring för aktiviteter och jobb manager aktiviteter som körs på pooler med hello konfiguration av virtuell dator. När du lägger till en aktivitet kan du ange en behållare i Azure Storage som hello mål för hello uppgiftens utdata. Hej Batch-tjänsten och sedan skriver en utgående data toothat behållare när hello åtgärden är klar.

En fördel toousing hello Batch service API toopersist uppgiftsutdata är att du inte behöver toomodify hello program som hello aktivitet körs. Med några enkla ändringar tooyour-klientprogram, kan du i stället kvarstår hello uppgiftens utdata från inom hello-kod som skapar hello aktiviteten.   

## <a name="when-do-i-use-hello-batch-service-api-toopersist-task-output"></a>När använder hello API för Batch-tjänsten toopersist uppgiftens utdata?

Azure Batch tillhandahåller mer än ett sätt toopersist uppgiftens utdata. Med hjälp av hello API för Batch-tjänsten är en praktisk metod som är bäst lämpade toothese scenarier:

- Du vill toowrite kod toopersist uppgiftens utdata från i ditt klientprogram utan att ändra hello-program som uppgiften körs.
- Vill du toopersist utdata från Batch-aktiviteter och jobb manager aktiviteter i pooler som har skapats med hello konfiguration av virtuell dator.
- Vill du toopersist utdata tooan Azure Storage-behållare med ett godtyckligt namn.
- Du vill toopersist utdata tooan Azure Storage-behållare med namnet bl.a toohello [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). 

Om ditt scenario skiljer sig från de som visas ovan, kanske du måste tooconsider en annan metod. Till exempel stöder hello API för Batch-tjänsten för närvarande inte strömmande utdata tooAzure lagring medan hello aktiviteten körs. toostream utdata, Överväg att använda hello Batch filen konventioner bibliotek, tillgängliga för .NET. För andra språk behöver du tooimplement din egen lösning. Mer information om andra alternativ för bestående uppgiftsutdata finns [spara projekt- och utdata tooAzure lagring](batch-task-output.md). 

## <a name="create-a-container-in-azure-storage"></a>Skapa en behållare i Azure Storage

toopersist aktivitet utdata tooAzure lagring måste du toocreate en behållare som fungerar som hello mål för utgående filer. Skapa hello behållare innan du kör uppgiften, helst innan du skickar ditt jobb. toocreate hello behållare, Använd hello lämplig Azure Storage-klientbibliotek eller SDK. Mer information om Azure Storage-API: er finns i hello [Azure Storage-dokumentationen](https://docs.microsoft.com/azure/storage/).

Till exempel om du skapar ditt program i C#, använda hello [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/). följande exempel visar hur hello toocreate en behållare:

```csharp
CloudBlobContainer container = storageAccount.CreateCloudBlobClient().GetContainerReference(containerName);
await conainer.CreateIfNotExists();
```

## <a name="get-a-shared-access-signature-for-hello-container"></a>Hämta en signatur för delad åtkomst för hello behållare

När du har skapat hello behållaren får du en signatur för delad åtkomst (SAS) med skrivbehörighet toohello behållare. En SAS ger delegerad åtkomst toohello behållare. hello SAS beviljar åtkomst med en angiven uppsättning behörigheter och under ett angivet tidsintervall. hello Batch-tjänsten måste en SAS med skriva behörigheter toowrite aktivitet utdata toohello behållare. Mer information om SAS finns [använder signaturer för delad åtkomst \(SAS\) i Azure Storage](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

När du får en SAS med hello Azure Storage-API: er returnerar hello API en SAS-token-sträng. Den här token strängen innehåller alla parametrar för hello SAS, inklusive hello behörigheter och hello intervall över vilka hello SAS är giltig. toouse hello SAS tooaccess en behållare i Azure Storage, behöver du tooappend hello SAS-token sträng toohello resurs-URI. hello resurs-URI, tillsammans med hello läggs SAS-token, ger autentiserad åtkomst tooAzure lagring.

hello följande exempel visar hur tooget en lässkyddad SAS token sträng för hello-behållaren och sedan lägger till hello SAS toohello behållare URI:

```csharp
string containerSasToken = container.GetSharedAccessSignature(new SharedAccessBlobPolicy()
{
    SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddDays(1),
    Permissions = SharedAccessBlobPermissions.Write
});

string containerSasUrl = container.Uri.AbsoluteUri + containerSasToken; 
```

## <a name="specify-output-files-for-task-output"></a>Ange utdatafilerna för uppgiftsutdata

toospecify utdatafilerna för en aktivitet, skapa en samling [utdatafil](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile) objekt och tilldela den toohello [CloudTask.OutputFiles](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudtask.outputfiles#Microsoft_Azure_Batch_CloudTask_OutputFiles) egenskapen när du skapar hello-aktivitet. 

hello följande kodexempel för .NET skapar en uppgift som skriver slumptal tooa fil med namnet `output.txt`. hello exempel skapas en utdatafil för `output.txt` toobe skrivs toohello behållare. hello exemplet skapar också utdatafilerna för alla loggfiler som matchar mönstret för hello filen `std*.txt` (_t.ex._, `stdout.txt` och `stderr.txt`). hello behållarens Webbadress kräver hello SAS som skapades tidigare för hello behållare. hello Batch-tjänsten använder hello SAS tooauthenticate åtkomst toohello behållare: 

```csharp
new CloudTask(taskId, "cmd /v:ON /c \"echo off && set && (FOR /L %i IN (1,1,100000) DO (ECHO !RANDOM!)) > output.txt\"")
{
    OutputFiles = new List<OutputFile>
    {
        new OutputFile(
            filePattern: @"..\std*.txt",
            destination: new OutputFileDestination(
         new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId)),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
        new OutputFile(
            filePattern: @"output.txt",
            destination: 
         new OutputFileDestination(new OutputFileBlobContainerDestination(
                    containerUrl: containerSasUrl,
                    path: taskId + @"\output.txt")),
            uploadOptions: new OutputFileUploadOptions(
            uploadCondition: OutputFileUploadCondition.TaskCompletion)),
}
```

### <a name="specify-a-file-pattern-for-matching"></a>Ange ett mönster för matchning för filen

När du anger en utdatafil, kan du använda hello [OutputFile.FilePattern](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfile.filepattern#Microsoft_Azure_Batch_OutputFile_FilePattern) egenskapen toospecify filen mönster för matchning. Hej filmönstret kan matcha noll filer, en fil eller en uppsättning filer som skapas av hello-aktivitet.

Hej **FilePattern** egenskapen stöder standard filesystem jokertecken som `*` (för icke-rekursiva matchar) och `**` (för rekursiv matchar). Till exempel hello kodexemplet ovan anger hello filen mönster toomatch `std*.txt` icke-rekursivt: 

`filePattern: @"..\std*.txt"`

tooupload en enskild fil, ange ett fil-mönster med jokertecken. Till exempel hello kodexemplet ovan anger hello filen mönster toomatch `output.txt`:

`filePattern: @"output.txt"`

### <a name="specify-an-upload-condition"></a>Ange ett villkor för överföring

Hej [OutputFileUploadOptions.UploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileuploadoptions.uploadcondition#Microsoft_Azure_Batch_OutputFileUploadOptions_UploadCondition) egenskapen tillåter villkorlig överföra utdatafilerna. Ett vanligt scenario är tooupload en uppsättning filer om hello aktiviteten lyckas och en annan uppsättning filer om den misslyckas. Exempelvis kanske tooupload utförlig loggfiler när hello uppgiften misslyckas och avslutas med slutkoden inte är noll. På liknande sätt kan kanske du vill tooupload resultatet filer endast om hello aktiviteten lyckas, eftersom dessa filer kanske saknas eller är ofullständig om hello misslyckas.

hello kodexemplen ovan anger hello **UploadCondition** egenskapen för**TaskCompletion**. Den här inställningen anger att hello-filen är toobe överförs när hello aktiviteter har slutförts, oavsett hello värdet för hello slutkod. 

`uploadCondition: OutputFileUploadCondition.TaskCompletion`

Andra inställningar finns i hello [OutputFileUploadCondition](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.outputfileuploadcondition) enum.

### <a name="disambiguate-files-with-hello-same-name"></a>Undvika tvetydigheten filer med hello samma namn

hello aktiviteter i ett projekt kan skapa filer som har hello samma namn. Till exempel `stdout.txt` och `stderr.txt` skapas för varje aktivitet som körs i ett jobb. Eftersom varje aktivitet körs i sin egen kontext, filerna inte är i konflikt i hello nodens filsystemet. Men när du överför filer från flera aktiviteter tooa delade behållaren måste toodisambiguate filer med hello samma namn.

Hej [OutputFileBlobContainerDestination.Path](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination.path#Microsoft_Azure_Batch_OutputFileBlobContainerDestination_Path) egenskapen anger hello mål blob eller virtuell katalog för utdatafilerna. Du kan använda hello **sökväg** egenskapen tooname hello blob eller virtuella katalogen så att utdata filer med samma namn är unikt namngivna i Azure Storage hello. Med hjälp av hello aktivitets-ID i hello sökväg är ett bra sätt tooensure unika namn och att identifiera lätt filer.

Om hello **FilePattern** egenskapen tooa jokerteckenuttryck så kan alla filer som matchar mönstret hello överförda toohello virtuella katalog som anges av hello **sökväg** egenskapen. Om hello behållare är exempelvis `mycontainer`, hello uppgiften ID är `mytask`, och hello filen mönstret är `..\std*.txt`, hello absolut URI: er toohello utdatafilerna i Azure Storage kommer att likna:

```
https://myaccount.blob.core.windows.net/mycontainer/mytask/stderr.txt
https://myaccount.blob.core.windows.net/mycontainer/mytask/stdout.txt
```

Om hello **FilePattern** egenskapen är uppsättningen toomatch ett enda filnamn, vilket innebär att den inte innehåller jokertecken, sedan hello värdet för hello **sökväg** egenskapen anger hello blob fullständigt kvalificerat namn . Om du tror namngivningskonflikter med en enda fil från flera aktiviteter innehålla hello namnet på hello virtuella katalogen som en del av hello filen namnet toodisambiguate dessa filer. Till exempel ange hello **sökväg** egenskapen tooinclude hello aktivitets-ID, hello avgränsningstecken (vanligtvis ett snedstreck) och hello filnamn:

`path: taskId + @"/output.txt"`

hello absolut URI: er toohello utdatafilerna för en uppsättning uppgifter ska likna följande:

```
https://myaccount.blob.core.windows.net/mycontainer/task1/output.txt
https://myaccount.blob.core.windows.net/mycontainer/task2/output.txt
```

Mer information om virtuella kataloger i Azure Storage finns [visa hello blobbar i en behållare](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container).


## <a name="diagnose-file-upload-errors"></a>Diagnostisera överför filfel

Om överföring av utgående filer tooAzure lagring misslyckas hello aktivitet och går sedan toohello **slutförd** tillstånd och hello [TaskExecutionInformation.FailureInformation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskexecutioninformation.failureinformation#Microsoft_Azure_Batch_TaskExecutionInformation_FailureInformation) -egenskapen anges. Granska hello **FailureInformation** egenskapen toodetermine vilka fel uppstod. Här är till exempel ett fel som uppstår på filöverföringen om hello behållare inte hittas: 

```
Category: UserError
Code: FileUploadContainerNotFound
Message: One of hello specified Azure container(s) was not found while attempting tooupload an output file
```

På varje filöverföringen Batch skriver två logga filer toohello compute-nod, `fileuploadout.txt` och `fileuploaderr.txt`. Du kan undersöka dessa loggen filer toolearn mer om ett specifikt fel. I fall där hello hämtningsförsöket aldrig gjordes, till exempel eftersom själva hello aktiviteten inte gick att köra sedan finns loggfilerna inte.

## <a name="diagnose-file-upload-performance"></a>Diagnostisera filen överföringsprestanda

Hej `fileuploadout.txt` filen loggar överföringen pågår. Du kan undersöka den här filen toolearn mer om hur länge din filöverföringar tar. Tänk på att det finns många bidragande faktorer tooupload prestanda, bland annat hello storlek hello nod, andra aktiviteter på hello nod när hello hello överför om hello Målbehållaren hello samma region som hello Batch-pool, hur många noder är Överför toohello storage-konto på hello samma tidpunkt, och så vidare.

## <a name="use-hello-batch-service-api-with-hello-batch-file-conventions-standard"></a>Använda hello API för Batch-tjänsten med hello Batch filen konventioner standard

När du bevara uppgiftens utdata med hello API för Batch-tjänsten kan namnge din målbehållare och blobbar du själv vill. Du kan också välja tooname dem enligt toohello [Batch filen konventioner standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions). hello filen konventioner standard avgör hello namnen på hello målbehållare och blob i Azure Storage för en given utdatafilen baserat på hello namnen på hello projekt- och. Om du använder hello filen konventioner standard för namngivning av filer och utgående filer är tillgängliga för visning i hello [Azure-portalen](https://portal.azure.com).

Om du utvecklar i C#, kan du använda hello metoder som är inbyggda i hello [filen konventioner för Batch-biblioteket för .NET](https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files). Det här biblioteket skapar hello korrekt angiven behållare och blob-sökvägar för dig. Du kan till exempel anropa hello API tooget hello rätt namn hello behållare, baserat på hello jobbnamn:

```csharp
string containerName = job.OutputStorageContainerName();
```

Du kan använda hello [CloudJobExtensions.GetOutputStorageContainerUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files.cloudjobextensions.getoutputstoragecontainerurl) metoden tooreturn en URL för delad åtkomst signatur (SAS) som är används toowrite toohello behållare. Du kan sedan överföra den här SAS-toohello [OutputFileBlobContainerDestination](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.outputfileblobcontainerdestination) konstruktor.

Om du utvecklar i ett annat språk än C#, behöver du tooimplement hello filen konventioner standard själv.

## <a name="code-sample"></a>Kodexempel

Hej [PersistOutputs] [ github_persistoutputs] exempelprojektet är en av hello [kodexempel för Azure Batch] [ github_samples] på GitHub. Den här Visual Studio-lösningen visar hur toouse hello Batch-klientbibliotek för .NET toopersist aktivitet utdata toodurable lagring. toorun hello exempel, följer du dessa steg:

1. Öppna hello-projekt i **Visual Studio 2015 eller senare**.
2. Lägg till grupp och lagring **kontoautentiseringsuppgifter** för**AccountSettings.settings** i hello Microsoft.Azure.Batch.Samples.Common projekt.
3. **Skapa** (men inte kör) hello lösning. Återställa NuGet-paket om du uppmanas till detta.
4. Använd hello Azure portal tooupload en [programpaket](batch-application-packages.md) för **PersistOutputsTask**. Inkludera hello `PersistOutputsTask.exe` och dess beroende sammansättningar i hello ZIP-paketet, ange hello program-ID för ”PersistOutputsTask” och programmet hello Paketversion för ”1.0”.
5. **Starta** (kör) hello **PersistOutputs** projekt.
6. När begärd toochoose hello beständiga teknik toouse körs hello exempel anger **2** toorun hello exempel med hjälp av hello API för Batch-tjänsten toopersist uppgiftens utdata.
7. Om du vill köra hello exemplet igen, ange **3** toopersist utdata med API för hello Batch-tjänsten och tooname hello-behållaren och blob målsökväg enligt toohello filen konventioner som standard.

## <a name="next-steps"></a>Nästa steg

- Mer information om bestående uppgiftens utdata med hello filen konventioner bibliotek för .NET finns [spara projekt- och data tooAzure lagring med hello Batch filen konventioner biblioteket för .NET-toopersist ](batch-task-output-file-conventions.md).
- Information om andra metoder för bestående av utdata i Azure Batch finns [spara projekt- och utdata tooAzure lagring](batch-task-output.md).

[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
