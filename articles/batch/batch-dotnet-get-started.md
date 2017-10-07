---
title: "aaaTutorial - Använd hello Azure Batch-klientbibliotek för .NET | Microsoft Docs"
description: "Lär dig hello grundläggande begrepp för Azure Batch och skapa en enkel lösning med hjälp av .NET."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 76cb9807-cbc1-405a-8136-d1e53e66e82b
ms.service: batch
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06062b3886a8081bd9a831824a981503ef55f9b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-building-solutions-with-hello-batch-client-library-for-net"></a>Komma igång med att skapa lösningar med hello Batch-klientbiblioteket för .NET

> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

Hello grundläggande information om hur [Azure Batch] [ azure_batch] och hello [Batch .NET] [ net_api] bibliotek i den här artikeln som diskuterar vi ett C#-exempel programmet steg av steg. Vi titta på hur hello exempelprogrammet utnyttjar hello Batch-tjänsten tooprocess en parallell arbetsbelastning i hello molnet och hur den samverkar med [Azure Storage](../storage/common/storage-introduction.md) för filen mellanlagrings- och hämtning. Du lär dig ett arbetsflöde för vanliga Batch-program och få en grundläggande förståelse för hello viktiga komponenter av Batch, till exempel projekt, uppgifter, pooler och compute-noder.

![Arbetsflöde för Batch-lösning (grundläggande)][11]<br/>

## <a name="prerequisites"></a>Krav
I den här artikeln förutsätter vi att du har erfarenhet av att arbeta med C# och Visual Studio. Det förutsätts även att du är kan toosatisfy hello skapa krav som anges nedan för Azure och hello Batch- och lagringstjänster.

### <a name="accounts"></a>Konton
* **Azure-konto**: Om du inte redan har en Azure-prenumeration kan du [skapa ett kostnadsfritt Azure-konto][azure_free_account].
* **Batch-konto**: När du har skaffat en Azure-prenumeration [skapar du ett Azure Batch-konto](batch-account-create-portal.md).
* **Lagringskonto**: Se [skapar ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) i [Om Azure-lagringskonton](../storage/common/storage-create-storage-account.md).

> [!IMPORTANT]
> Batch för närvarande stöder *endast* hello **allmänna** lagringskontotypen, enligt beskrivningen i steg #5 [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) i [om Azure Storage-konton](../storage/common/storage-create-storage-account.md).
>
>

### <a name="visual-studio"></a>Visual Studio
Du måste ha **Visual Studio 2015 eller nyare** toobuild hello exempelprojektet. Du kan hitta gratis och utvärderingsversion versioner av Visual Studio i hello [översikt över Visual Studio produkter][visual_studio].

### <a name="dotnettutorial-code-sample"></a>*DotNetTutorial*kodexempel
Hej [DotNetTutorial] [ github_dotnettutorial] exempel är en av många Batch-kodexempel hittades i hello hello [azure-batch-samples] [ github_samples] databasen på GitHub. Du kan hämta alla hello prover genom att klicka på **kloning eller hämta > hämta ZIP** på startsidan för hello databasen eller genom att klicka på hello [azure-batch-exempel-master.zip] [ github_samples_zip]direkt hämtningslänken. När du har extraherat hello innehållet i hello ZIP-fil hittar hello lösning i hello följande mapp:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure Batch Explorer (valfritt)
Hej [Azure Batch Explorer] [ github_batchexplorer] är ett kostnadsfritt verktyg som ingår i hello [azure-batch-samples] [ github_samples] databasen på GitHub. När krävs inte toocomplete självstudiekursen, det kan vara användbart när du skapar och felsöker Batch-lösningar.

## <a name="dotnettutorial-sample-project-overview"></a>Översikt över DotNetTutorial-exempelprojekt
Hej *DotNetTutorial* kodexempel är en Visual Studio-lösning som består av två projekt: **DotNetTutorial** och **TaskApplication**.

* **DotNetTutorial** är hello klientprogram som interagerar med hello Batch- och Storage services tooexecute en parallell arbetsbelastning på compute-noder (virtuella datorer). DotNetTutorial körs på den lokala arbetsstationen.
* **TaskApplication** är hello-program som körs på beräkningsnoder i Azure tooperform hello verkligt arbete. I exemplet hello `TaskApplication.exe` Parsar hello text i en fil som hämtas från Azure Storage (hello indatafilen). Sedan den genererar en textfil (hello utdata-fil) som innehåller en lista över hello de tre översta ord som visas i hello indatafilen. När den skapar hello utdatafilen filöverföringar TaskApplication hello filen tooAzure lagring. Detta gör att det tillgängliga toohello klientprogrammet för hämtning. TaskApplication körs parallellt på flera beräkningsnoder i hello Batch-tjänsten.

hello följande diagram illustrerar hello primära åtgärder som utförs av hello klientprogrammet *DotNetTutorial*, och hello-program som körs av hello uppgifter *TaskApplication*. Detta grundläggande arbetsflöde är typiskt för många beräkningslösningar som skapas med Batch. När den inte visar alla funktioner som är tillgängliga i hello Batch-tjänsten, omfattar nästan alla Batch-scenario delar av det här arbetsflödet.

![Batch-exempelarbetsflöde][8]<br/>

[**Steg 1.**](#step-1-create-storage-containers) Skapa **behållare** i Azure Blob Storage.<br/>
[**Steg 2.**](#step-2-upload-task-application-and-data-files) Överför uppgiften programfiler och indatafilerna toocontainers.<br/>
[**Steg 3.**](#step-3-create-batch-pool) Skapa en Batch-**pool**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** Hej poolen **startuppgift har ställts** hämtningar hello uppgiften binära filer (TaskApplication) toonodes som de gå hello poolen.<br/>
[**Steg 4.**](#step-4-create-batch-job) Skapa ett Batch-**jobb**.<br/>
[**Steg 5.**](#step-5-add-tasks-to-job) Lägg till **uppgifter** toohello jobb.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** hello uppgifter som är schemalagda tooexecute på noder.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Varje aktivitet hämtar sina indata från Azure Storage och börjar sedan köra.<br/>
[**Steg 6.**](#step-6-monitor-tasks) Övervaka aktiviteter.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** PowerPoint, överför de sina utdata data tooAzure lagring.<br/>
[**Step 7.**](#step-7-download-task-output) Hämta aktiviteternas utdata från Storage.

Som tidigare nämnts kan inte varje Batch-lösning utför stegen exakt, och kan inkludera många fler, men hello *DotNetTutorial* -exempelprogrammet demonstrerar vanliga processer som hittades i en Batch-lösningen.

## <a name="build-hello-dotnettutorial-sample-project"></a>Skapa hello *DotNetTutorial* exempelprojektet
Innan hello exempel kan köras, måste du ange både Batch och lagring autentiseringsuppgifter i hello *DotNetTutorial* projektets `Program.cs` fil. Om du redan inte har gjort det, öppna hello lösningen i Visual Studio genom att dubbelklicka på hello `DotNetTutorial.sln` lösningsfilen. Eller öppna den från Visual Studio med hjälp av hello **Arkiv > Öppna > projekt/lösning** menyn.

Öppna `Program.cs` inom hello *DotNetTutorial* projekt. Lägg sedan till dina autentiseringsuppgifter som anges i hello övre delen av filen hello:

```csharp
// Update hello Batch and Storage account credential strings below with hello values
// unique tooyour accounts. These are used when constructing connection strings
// for hello Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [!IMPORTANT]
> Som nämnts ovan är du för närvarande måste ange hello autentiseringsuppgifter för en **allmänna** storage-konto i Azure Storage. Batch-program använda blob storage inom hello **allmänna** storage-konto. Ange inte hello autentiseringsuppgifter för ett lagringskonto som har skapats genom att välja hello *Blob storage* kontotyp.
>
>

Du hittar Batch- och autentiseringsuppgifterna för ditt konto i hello kontoblad för varje tjänst i hello [Azure-portalen][azure_portal]:

![Batch-autentiseringsuppgifter i hello portal][9]
![lagring autentiseringsuppgifter i hello-portalen][10]<br/>

Nu när du har uppdaterat hello-projekt med dina autentiseringsuppgifter, högerklicka på hello lösningen i Solution Explorer och klicka på **skapa lösning**. Om du uppmanas bekräfta att hello återställas av NuGet-paket.

> [!TIP]
> Om hello NuGet-paket inte återställs automatiskt, eller om du får felmeddelanden om ett fel toorestore hello-paket, kontrollera att du har hello [NuGet Package Manager] [ nuget_packagemgr] installerad. Aktivera sedan hello hämtning av saknade paket. Se [aktiverar paketet återställa under skapa] [ nuget_restore] tooenable ladda ned paket.
>
>

I följande avsnitt hello, vi dela upp hello exempelprogrammet på hello steg utförs tooprocess en arbetsbelastning i hello Batch-tjänsten och diskutera dessa steg i detalj. Vi rekommenderar att du toorefer toohello öppna lösningen i Visual Studio medan du gå igenom hello resten av den här artikeln eftersom inte alla kodrad i hello exempel beskrivs.

Navigera toohello överkant hello `MainAsync` metod i hello *DotNetTutorial* projektets `Program.cs` filen toostart med steg 1. Varje steg nedan och ungefär sätt hello utvecklingen av metoden anropar `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Steg 1: Skapa Storage-behållare
![Skapa behållare i Azure Storage][1]
<br/>

Batch har inbyggt stöd för integrering med Azure Storage. Behållare på ditt lagringskonto tillhandahåller hello-filer som behövs i hello aktiviteter som körs i Batch-kontot. hello behållare också tillhandahålla en plats toostore hello utdata som producerar hello uppgifter. hello först öppna hello *DotNetTutorial* klientprogrammet kan skapa tre behållare i [Azure Blob Storage](../storage/common/storage-introduction.md):

* **programmet**: den här behållaren lagrar hello-program som körs av hello uppgifter, samt någon av dess beroenden, till exempel DLL-filer.
* **inkommande**: uppgifter hämtar hello data filer tooprocess från hello *inkommande* behållare.
* **utdata**: när uppgifter bearbeta indatafilen, kommer de att överföra hello resultat toohello *utdata* behållare.

I ordning toointeract med en Storage-kontot och skapa behållare, använder vi hello [Azure Storage-klientbibliotek för .NET][net_api_storage]. Vi skapar ett konto, referens toohello [CloudStorageAccount][net_cloudstorageaccount], och som skapar du en [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct hello Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve hello storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create hello blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Vi använder hello `blobClient` referera i hela programmet hello och skicka den som en parameter tooseveral metoder. Ett exempel på detta är i hello kodblock som följer omedelbart hello ovan, där vi kallar `CreateContainerIfNotExistAsync` tooactually skapa hello behållare.

```csharp
// Use hello blob client toocreate hello containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

När du har skapat hello behållare hello program kan ladda upp hello-filer som ska användas av hello uppgifter.

> [!TIP]
> [Hur toouse Blob Storage från .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) ger en bra översikt av arbetet med Azure Storage-behållare och blobbar. Det bör vara hello övre delen av listan läsning när du börjar arbeta med Batch.
>
>

## <a name="step-2-upload-task-application-and-data-files"></a>Steg 2: Ladda upp filer för aktivitetsprogram och indata
![Överför aktivitet program och indata (data) filer toocontainers][2]
<br/>

Överför åtgärden, i filen hello *DotNetTutorial* först definiera samlingar av **program** och **inkommande** filen sökvägar som de finns på hello lokal dator. Sedan överför dessa filer toohello behållare som du skapade i föregående steg i hello.

```csharp
// Paths toohello executable and its dependencies that will be executed by hello tasks
List<string> applicationFilePaths = new List<string>
{
    // hello DotNetTutorial project includes a project reference tooTaskApplication,
    // allowing us toodetermine hello path of hello task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// hello collection of data files that are toobe processed by hello tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload hello application and its dependencies tooAzure Storage. This is the
// application that will process hello data files, and will be executed by each
// of hello tasks on hello compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload hello data files. This is hello data that will be processed by each of
// hello tasks that are executed on hello compute nodes within hello pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Det finns två metoder i `Program.cs` som är inblandade i hello överföringsprocessen:

* `UploadFilesToContainerAsync`: Den här metoden returnerar en mängd [ResourceFile] [ net_resourcefile] objekt (diskuteras nedan) och internt anrop `UploadFileToContainerAsync` tooupload varje fil som skickades i hello *filePaths* parameter.
* `UploadFileToContainerAsync`: Det här är hello-metod som faktiskt utför hello filöverföringen och skapar hello [ResourceFile] [ net_resourcefile] objekt. Efter överföring hello filen erhåller en signatur för delad åtkomst (SAS) för hello-filen och returnerar ett ResourceFile-objekt som representerar den. Signaturer för delad åtkomst beskrivs nedan.

```csharp
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} toocontainer [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath);

        // Set hello expiry time and permissions for hello blob shared access signature.
        // In this case, no start time is specified, so hello shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct hello SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles
En [ResourceFile] [ net_resourcefile] innehåller aktiviteter i en Batch med hello URL tooa-fil i Azure Storage som är hämtade tooa beräkningsnod innan aktiviteten körs. Hej [ResourceFile.BlobSource] [ net_resourcefile_blobsource] egenskapen anger hello fullständig URL till hello fil eftersom den finns i Azure Storage. hello-URL kan även innehålla en signatur för delad åtkomst (SAS) som tillhandahåller säker åtkomst till toohello-fil. De flesta typer av aktiviteter i Batch .NET innehåller en *ResourceFiles*-egenskap, inklusive:

* [CloudTask][net_task]
* [StartTask][net_pool_starttask]
* [JobPreparationTask][net_jobpreptask]
* [JobReleaseTask][net_jobreltask]

Hej DotNetTutorial exempelprogrammet använder inte hello JobPreparationTask eller JobReleaseTask aktivitetstyperna, men du kan läsa mer om dem i [kör jobbet förberedelse och slutförande uppgifter på Azure Batch-beräkningsnoder](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Signatur för delad åtkomst (SAS)
Delad åtkomst signaturer är strängar som, när ingår som en del av en URL, ger säker åtkomst toocontainers och blobbar i Azure Storage. hello DotNetTutorial programmet använder både blob och behållare delad åtkomst signatur URL: er och visar hur dessa delade tooobtain åt signatur strängar från hello Storage-tjänst.

* **BLOB-signaturer för delad åtkomst**: hello poolen startuppgift har ställts i DotNetTutorial använder signaturer för delad blob åtkomst vid nedladdning av hello binärfiler och indata filer från lagring (se steg #3 nedan). Hej `UploadFileToContainerAsync` metod i Dotnettutorial's `Program.cs` innehåller hello-kod som hämtar varje blob signatur för delad åtkomst. Detta sker genom anrop till [CloudBlob.GetSharedAccessSignature][net_sas_blob].
* **Behållaren signaturer för delad åtkomst**: eftersom varje aktivitet har slutförts på hello beräkningsnod, laddar upp dess utdata filen toohello *utdata* behållare i Azure Storage. toodo så TaskApplication använder en signatur för delad åtkomst behållare som ger skrivbehörighet toohello behållare som en del av hello sökvägen när den överför hello-fil. Signatur för delad åtkomst ges hello behållaren görs på ett liknande sätt som när erhålla hello blob delad åtkomstsignatur. I DotNetTutorial, hittar du den hello `GetContainerSasUrl` helper metodanrop [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] toodo så. Du kan läsa mer om hur TaskApplication använder hello behållaren delad åtkomstsignatur i ”steg 6: övervaka aktiviteter”.

> [!TIP]
> Checka ut hello tvådelat serien på signaturer för delad åtkomst [del 1: Förstå hello delade signatur (SAS) modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md) och [del 2: skapa och använda en signatur för delad åtkomst (SAS) med Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn mer om hur du ger säker åtkomst toodata i ditt lagringskonto.
>
>

## <a name="step-3-create-batch-pool"></a>Steg 3: Skapa en Batch-pool
![Skapa en Batch-pool][3]
<br/>

En Batch-**pool** är en samling beräkningsnoder (virtuella datorer) där Batch utför aktiviteterna i ett jobb.

Efter överföring hello program och data filer toohello Storage-konto med Azure Storage API: er, *DotNetTutorial* börjar att göra anrop toohello Batch-tjänsten med API: er som tillhandahålls av hello Batch .NET-biblioteket. hello kod skapar först ett [BatchClient][net_batchclient]:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Därefter hello exemplet skapas en pool med compute-noder i hello Batch-kontot med ett anrop för`CreatePoolIfNotExistsAsync`. `CreatePoolIfNotExistsAsync`använder hello [BatchClient.PoolOperations.CreatePool] [ net_pool_create] metoden toocreate en ny pool i hello Batch-tjänsten:

```csharp
private static async Task CreatePoolIfNotExistAsync(BatchClient batchClient, string poolId, IList<ResourceFile> resourceFiles)
{
    CloudPool pool = null;
    try
    {
        Console.WriteLine("Creating pool [{0}]...", poolId);

        // Create hello unbound pool. Until we call CloudPool.Commit() or CommitAsync(), no pool is actually created in the
        // Batch service. This CloudPool instance is therefore considered "unbound," and we can modify its properties.
        pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicatedComputeNodes: 3,                                             // 3 compute nodes
            virtualMachineSize: "small",                                                // single-core, 1.75 GB memory, 225 GB disk
            cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));   // Windows Server 2012 R2

        // Create and assign hello StartTask that will be executed when compute nodes join hello pool.
        // In this case, we copy hello StartTask's resource files (that will be automatically downloaded
        // toohello node by hello StartTask) into hello shared directory that all tasks will have access to.
        pool.StartTask = new StartTask
        {
            // Specify a command line for hello StartTask that copies hello task application files toothe
            // node's shared directory. Every compute node in a Batch pool is configured with a number
            // of pre-defined environment variables that can be referenced by commands or applications
            // run by tasks.

            // Since a successful execution of robocopy can return a non-zero exit code (e.g. 1 when one or
            // more files were successfully copied) we need toomanually exit with a 0 for Batch toorecognize
            // StartTask execution success.
            CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
            ResourceFiles = resourceFiles,
            WaitForSuccess = true
        };

        await pool.CommitAsync();
    }
    catch (BatchException be)
    {
        // Swallow hello specific error code PoolExists since that is expected if hello pool already exists
        if (be.RequestInformation?.BatchError != null && be.RequestInformation.BatchError.Code == BatchErrorCodeStrings.PoolExists)
        {
            Console.WriteLine("hello pool {0} already existed when we tried toocreate it", poolId);
        }
        else
        {
            throw; // Any other exception is unexpected
        }
    }
}
```

När du skapar en pool med [CreatePool][net_pool_create], du anger flera parametrar, till exempel hello många compute-noder hello [storlek hello noder](../cloud-services/cloud-services-sizes-specs.md), och hello nodernas operativsystem system. I *DotNetTutorial*, använder vi [CloudServiceConfiguration] [ net_cloudserviceconfiguration] toospecify Windows Server 2012 R2 från [molntjänster](../cloud-services/cloud-services-guestos-update-matrix.md). 

Du kan också skapa pooler för compute-noder som är Azure virtuella datorer (VM) genom att ange hello [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] för din pool. Du kan skapa en pool med VM-beräkningsnoder från antingen Windows- eller [Linux-avbildningar](batch-linux-nodes.md). hello källa för VM-avbildningar kan vara antingen:

- Hej [Azure virtuella datorer Marketplace][vm_marketplace], som innehåller både Windows- och Linux-avbildningar som är färdiga att använda. 
- En anpassad avbildning som du förbereder och tillhandahåller. Mer information om anpassade avbildningar finns i [Utveckla storskaliga parallella beräkningslösningar med Batch](batch-api-basics.md#pool).

> [!IMPORTANT]
> Du debiteras för beräkningsresurser i Batch. toominimize kostnader, kan du sänka `targetDedicatedComputeNodes` too1 innan du kör hello exempel.
>
>

Tillsammans med egenskaperna fysisk nod kan du också ange en [startuppgift har ställts] [ net_pool_starttask] för hello poolen. hello startuppgift har ställts körs på varje nod som noden ansluter hello poolen och varje gång en nod startas. hello startuppgift har ställts är särskilt användbar för att installera program på compute-noder tidigare toohello körningen av uppgifter. Om aktiviteterna bearbeta data med hjälp av Python-skript kan använda du till exempel en startuppgift har ställts tooinstall Python på hello compute-noder.

I det här exempelprogrammet hello startuppgift har ställts kopierar hello-filer som den laddar ned från lagring (som anges med hello [startuppgift har ställts][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] egenskapen) från hello startuppgift har ställts directory toohello delade arbetskatalogen som *alla* aktiviteter som körs på hello nod kan komma åt. Då kopieras `TaskApplication.exe` och dess beroenden toohello delad katalog på varje nod som hello nod ansluter till hello poolen, så att alla aktiviteter som körs på hello nod kan komma åt den.

> [!TIP]
> Hej **programpaket** funktion i Azure Batch tillhandahåller ett annat sätt tooget tillämpningsprogrammet till hello beräkningsnoder i poolen. Se [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md) mer information.
>
>

Mest kända i hello kodstycke ovan är också hello användning av två miljövariabler i hello *CommandLine* -egenskapen för hello startuppgift har ställts: `%AZ_BATCH_TASK_WORKING_DIR%` och `%AZ_BATCH_NODE_SHARED_DIR%`. Varje compute-nod i ett Batch-pool konfigureras med flera miljövariabler som är specifika tooBatch automatiskt. En process som körs av en aktivitet har åtkomst toothese miljövariabler.

> [!TIP]
> toofind mer information om hello miljövariabler som är tillgängliga på beräkningsnoder i en Batch-pool och information om aktiviteten fungerande kataloger finns hello [miljöinställningar för uppgifter](batch-api-basics.md#environment-settings-for-tasks) och [filer och kataloger ](batch-api-basics.md#files-and-directories) avsnitt i hello [Batch funktionsöversikt för utvecklare](batch-api-basics.md).
>
>

## <a name="step-4-create-batch-job"></a>Steg 4: Skapa ett Batch-jobb
![Skapa ett Batch-jobb][4]<br/>

Ett Batch-**jobb** är en samling aktiviteter och associeras med en pool av beräkningsnoder. hello aktiviteter i ett projekt köra på hello associerade poolens beräkningsnoder.

Du kan använda ett jobb inte bara för att ordna och spåra uppgifter i relaterade arbetsbelastningar, men också för införande av vissa villkor – till exempel hello maximal körningstid för hello jobb (och därmed dess aktiviteter) samt prioritet i relationen tooother jobb i hello Batch konto. I det här exemplet är hello jobbet dock endast kopplade till hello-pool som skapades i steg #3. Inga ytterligare egenskaper har konfigurerats.

Alla Batch-jobb är associerade med en specifik pool. Den här associeringen anger vilka noder hello jobbuppgifter körs på. Du kan ange detta med hjälp av hello [CloudJob.PoolInformation] [ net_job_poolinfo] egenskapen enligt hello kodfragmentet nedan.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Nu när ett jobb har skapats läggs aktiviteterna tooperform hello arbete.

## <a name="step-5-add-tasks-toojob"></a>Steg 5: Lägg till uppgifter toojob
![Lägga till uppgifter toojob][5]<br/>
*(1) uppgifter läggs toohello jobb, (2) hello aktiviteter är schemalagda toorun på noder och (3) hello uppgifter hämta hello data filer tooprocess*

Batch **uppgifter** är hello enskilda arbetsenheter som kör på hello compute-noder. En aktivitet har en kommandorad och kör hello skript eller körbara filer som du anger att kommandoraden.

tooactually utföra arbetet, aktiviteter måste läggas till tooa jobb. Varje [CloudTask] [ net_task] konfigureras med hjälp av en kommandoradsegenskapen och [ResourceFiles] [ net_task_resourcefiles] (som i hello poolen startuppgift har ställts) som hello-aktiviteten hämtar toohello nod innan dess kommandoraden körs automatiskt. I hello *DotNetTutorial* exempelprojektet varje uppgift bearbetar endast en fil. Det betyder att dess ResourceFiles-samling kan innehålla ett enda element.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks toojob [{1}]...", inputFiles.Count, jobId);

    // Create a collection toohold hello tasks that we'll be adding toohello job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of hello tasks. Because we copied hello task application toothe
    // node's shared directory with hello pool's StartTask, we can access it via
    // hello shared directory on hello node that hello task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add hello tasks as a collection, as opposed tooissuing a separate AddTask call
    // for each. Bulk task submission helps tooensure efficient underlying API calls
    // toohello Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [!IMPORTANT]
> När de har åtkomst till miljövariabler som `%AZ_BATCH_NODE_SHARED_DIR%` eller köra ett program som inte hittades i hello nod `PATH`, kommandorader för aktiviteten måste föregås av `cmd /c`. Detta uttryckligen köra hello Kommandotolken och instruera tooterminate efter att utföra kommandot. Det här kravet är inte nödvändigt om aktiviteterna köra ett program i hello nod `PATH` (exempelvis *robocopy.exe* eller *powershell.exe*) och inga miljövariabler som används.
>
>

Inom hello `foreach` loop i hello kodstycke ovan visas hello kommandoraden för hello aktivitet skapas så att tre kommandoradsargument som skickas för*TaskApplication.exe*:

1. Hej **första argumentet** är hello sökvägen hello filen tooprocess. Detta är hello lokal sökväg toohello fil eftersom den finns på hello-nod. När hello ResourceFile objekt i `UploadFileToContainerAsync` skapades ovan, hello filnamn har använts för den här egenskapen (som en parameter toohello ResourceFile konstruktor). Detta anger att hello-filen finns i hello samma katalog som *TaskApplication.exe*.
2. Hej **andra argumentet** anger att hello upp *N* ord ska skrivas toohello utdatafilen. I exemplet hello är detta hårdkodat så att hello översta tre ord skrivs toohello utdatafilen.
3. Hej **tredje argumentet** är hello signatur för delad åtkomst (SAS) som tillhandahåller skrivbehörighet toohello **utdata** behållare i Azure Storage. *TaskApplication.exe* använder detta delad åtkomst till URL: en signatur när den laddas upp hello utdata filen tooAzure lagring. Du hittar hello koden för den här i hello `UploadFileToContainer` metod i hello TaskApplication projekt `Program.cs` fil:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference toohello container using hello SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload hello file (as a new blob) toohello container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when hello Batch service
                // sets hello CloudTask.ExecutionInformation.ExitCode for hello task that
                // executed this application, it properly indicates that there was a
                // problem with hello task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Steg 6: Övervaka aktiviteter
![Övervaka aktiviteter][6]<br/>
*hello klienten program (1) Övervakare hello uppgifter för slutförande och status slutfört och (2) hello uppgifter överför resultat data tooAzure lagring*

När uppgifter läggs tooa jobbet kommer de automatiskt i kö och schemalagda för körning på datornoderna inom hello poolen som är associerade med hello jobb. Batch hanterar baserat på hello-inställningar som du anger, alla uppgiften queuing, schemaläggning, försöker igen och andra uppgifter för administration av aktivitet för dig.

Det finns flera metoder toomonitoring för körning av aktiviteten. DotNetTutorial visar ett enkelt exempel som endast rapporterar om aktiviteterna har slutförts eller misslyckats, samt aktiviteternas status. Inom hello `MonitorTasks` metod i Dotnettutorial's `Program.cs`, det finns tre Batch .NET-begrepp som backar upp diskussion. De förklaras nedan i den ordning som de förekommer:

1. **ODATADetailLevel**: Det är viktigt att du anger [ODATADetailLevel][net_odatadetaillevel] i liståtgärder (till exempel när du hämtar en lista över aktiviteterna i ett jobb) för att upprätthålla Batch-programmets prestanda. Lägg till [fråga hello Azure Batch-tjänsten effektivt](batch-efficient-list-queries.md) tooyour läsa listan om du planerar att göra någon form av övervakning av innehållsstatus i Batch-program.
2. **TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor] tillhandahåller .NET-program för Batch med verktyg som hjälper dig att övervaka aktiviteternas status. I `MonitorTasks`, *DotNetTutorial* väntar på att alla aktiviteter tooreach [TaskState.Completed] [ net_taskstate] inom en tidsgräns. Därefter avslutas hello jobb.
3. **TerminateJobAsync**: avslutar ett jobb med [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (eller hello blockerar JobOperations.TerminateJob) markerar jobbet som slutförd. Det är viktigt toodo så om Batch-lösningen använder en [JobReleaseTask][net_jobreltask]. Det här är en särskild typ av aktivitet, som beskrivs i [Aktiviteter för att förbereda och slutföra jobb](batch-job-prep-release.md).

Hej `MonitorTasks` metod från *DotNetTutorial*'s `Program.cs` visas nedan:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed tooreach hello Completed state within hello timeout period.";

    // Obtain hello collection of tasks currently managed by hello job. Note that we use
    // a detail level too specify that only hello "id" property of each task should be
    // populated. Using a detail level for all list operations helps toolower
    // response time from hello Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor toomonitor hello state of our tasks. In this case, we
    // will wait for all tasks tooreach hello Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached hello "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property tooensure that it did not encounter a failure
    // or return a non-zero exit code.

    // Update hello detail level toopopulate only hello task id and executionInfo
    // properties. We refresh hello tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate hello task's properties with hello latest info from hello Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.Result == TaskExecutionResult.Failure)
        {
            // A task with failure information set indicates there was a problem with hello task. It is important toonote that
            // hello task's state can be "Completed," yet still have encountered a failure.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a failure: {1}", task.Id, task.ExecutionInformation.FailureInformation.Message);
            if (task.ExecutionInformation.ExitCode != 0)
            {
                // A non-zero exit code may indicate that hello application executed by hello task encountered an error
                // during execution. As not every application returns non-zero on failure by default (e.g. robocopy),
                // your implementation of error checking may differ from this example.

                Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
            }
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within hello specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Steg 7: Hämta aktivitetsutdata
![Hämta aktivitetsutdata från Storage][7]<br/>

Nu när hello jobbet har slutförts kan hello utdata från hello uppgifter laddas ned från Azure Storage. Detta görs med ett anrop för`DownloadBlobsFromContainerAsync` i *DotNetTutorial*'s `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference tooa previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all hello block blobs in hello specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference toohello current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents tooa file in hello specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded too{0}", directoryPath);
}
```

> [!NOTE]
> Hej anrop för`DownloadBlobsFromContainerAsync` i hello *DotNetTutorial* programmet anger hello filer att hämtade tooyour `%TEMP%` mapp. Känna sig fria toomodify detta utdata plats.
>
>

## <a name="step-8-delete-containers"></a>Steg 8: Ta bort behållare
Eftersom du debiteras för data som finns i Azure Storage är alltid en bra idé tooremove blob som inte längre behövs för Batch-jobb. I Dotnettutorial's `Program.cs`, detta görs med tre anrop toohello hjälpmetod `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

själva hello metoden bara hämtar en referens toohello behållare och anropar sedan [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a>Steg 9: Ta bort hello jobbet och hello pool
I hello sista steget är du tillfrågas toodelete hello jobbet och hello pool som har skapats av hello DotNetTutorial program. Även om du inte debiteras för själva jobben och aktiviteterna *debiteras* du för beräkningsnoder. Vi rekommenderar därför att du endast allokerar noder efter behov. Borttagning av oanvända pooler kan ingå i din underhållsrutin.

Hej BatchClient [JobOperations] [ net_joboperations] och [PoolOperations] [ net_pooloperations] båda har motsvarande borttagning metoder, som kallas om hello användaren bekräftar borttagning:

```csharp
// Clean up hello resources we've created in hello Batch account if hello user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [!IMPORTANT]
> Tänk på att du debiteras för beräkningsresurser – du kan minimera kostnaden genom att ta bort oanvända pooler. Tänk också på att poolen tar du bort alla beräkningsnoder i poolen och alla data på hello noder kommer att återställas när hello poolen har tagits bort.
>
>

## <a name="run-hello-dotnettutorial-sample"></a>Kör hello *DotNetTutorial* exempel
När du kör hello exempelprogrammet blir hello konsolens utdata liknande toohello följande. Under körning och du får en paus på `Awaiting task completion, timeout in 00:30:00...` medan hello poolen compute-noder startas. Använd hello [Azure-portalen] [ azure_portal] toomonitor din pool, compute-noder, jobb och uppgifter under och efter körning. Använd hello [Azure-portalen] [ azure_portal] eller hello [Azure Lagringsutforskaren] [ storage_explorers] tooview hello lagringsresurser (behållare och blobbar) som är skapat av hello program.

Vanliga körningstid är **cirka 5 minuter** när du kör programmet hello i standardkonfigurationen.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe toocontainer [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll toocontainer [application]...
Uploading file ..\..\taskdata1.txt toocontainer [input]...
Uploading file ..\..\taskdata2.txt toocontainer [input]...
Uploading file ..\..\taskdata3.txt toocontainer [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks toojob [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within hello specified timeout period.
Downloading all files from container [output]...
All files downloaded tooC:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a>Nästa steg
Känna sig fria toomake ändringar för*DotNetTutorial* och *TaskApplication* tooexperiment med olika compute scenarier. Till exempel försök att lägga till en körning fördröjning för*TaskApplication*, till exempel med [Thread.Sleep][net_thread_sleep], toosimulate tidskrävande uppgifter och övervaka dem i hello-portalen. Försök att lägga till fler aktiviteter eller justera hello antalet compute-noder. Lägg till logik toocheck för och Tillåt hello användning av en befintlig programpool toospeed körningstid (*tipset*: kolla `ArticleHelpers.cs` i hello [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] projektet i [azure-batch-samples][github_samples]).

Nu när du är bekant med grundläggande hello-arbetsflöde i en Batch-lösning är tid toodig i toohello ytterligare funktioner för hello Batch-tjänsten.

* Granska hello [översikt över Azure Batch funktioner](batch-api-basics.md) artikel som vi rekommenderar att om du är ny toohello tjänst.
* Start på hello andra artiklar för utveckling av Batch under **Development djupgående** i hello [Batch Utbildningsväg][batch_learning_path].
* Checka ut en annan implementering av bearbetning hello ”främsta orden” arbetsbelastning med Batch i hello [TopNWords] [ github_topnwords] exempel.
* Granska hello Batch .NET [viktig information](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) för hello senaste ändringarna i hello-biblioteket.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/vs/
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Skapa behållare i Azure Storage"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Överför aktivitet program och indata (data) filer toocontainers"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Skapa en Batch-pool"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Skapa ett Batch-jobb"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Lägga till uppgifter toojob"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Övervaka aktiviteter"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Hämta aktivitetsutdata från Storage"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Arbetsflödet i en Batch-lösning (fullständigt diagram)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Batch-autentiseringsuppgifter på portalen"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Storage-autentiseringsuppgifter på portalen"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Arbetsflödet i en Batch-lösning (minimalt diagram)"
