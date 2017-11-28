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
# <a name="get-started-building-solutions-with-hello-batch-client-library-for-net"></a><span data-ttu-id="dcec6-103">Komma igång med att skapa lösningar med hello Batch-klientbiblioteket för .NET</span><span class="sxs-lookup"><span data-stu-id="dcec6-103">Get started building solutions with hello Batch client library for .NET</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="dcec6-104">.NET</span><span class="sxs-lookup"><span data-stu-id="dcec6-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="dcec6-105">Python</span><span class="sxs-lookup"><span data-stu-id="dcec6-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="dcec6-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="dcec6-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="dcec6-107">Hello grundläggande information om hur [Azure Batch] [ azure_batch] och hello [Batch .NET] [ net_api] bibliotek i den här artikeln som diskuterar vi ett C#-exempel programmet steg av steg.</span><span class="sxs-lookup"><span data-stu-id="dcec6-107">Learn hello basics of [Azure Batch][azure_batch] and hello [Batch .NET][net_api] library in this article as we discuss a C# sample application step by step.</span></span> <span data-ttu-id="dcec6-108">Vi titta på hur hello exempelprogrammet utnyttjar hello Batch-tjänsten tooprocess en parallell arbetsbelastning i hello molnet och hur den samverkar med [Azure Storage](../storage/common/storage-introduction.md) för filen mellanlagrings- och hämtning.</span><span class="sxs-lookup"><span data-stu-id="dcec6-108">We look at how hello sample application leverages hello Batch service tooprocess a parallel workload in hello cloud, and how it interacts with [Azure Storage](../storage/common/storage-introduction.md) for file staging and retrieval.</span></span> <span data-ttu-id="dcec6-109">Du lär dig ett arbetsflöde för vanliga Batch-program och få en grundläggande förståelse för hello viktiga komponenter av Batch, till exempel projekt, uppgifter, pooler och compute-noder.</span><span class="sxs-lookup"><span data-stu-id="dcec6-109">You'll learn a common Batch application workflow and gain a base understanding of hello major components of Batch such as jobs, tasks, pools, and compute nodes.</span></span>

<span data-ttu-id="dcec6-110">![Arbetsflöde för Batch-lösning (grundläggande)][11]</span><span class="sxs-lookup"><span data-stu-id="dcec6-110">![Batch solution workflow (basic)][11]</span></span><br/>

## <a name="prerequisites"></a><span data-ttu-id="dcec6-111">Krav</span><span class="sxs-lookup"><span data-stu-id="dcec6-111">Prerequisites</span></span>
<span data-ttu-id="dcec6-112">I den här artikeln förutsätter vi att du har erfarenhet av att arbeta med C# och Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcec6-112">This article assumes that you have a working knowledge of C# and Visual Studio.</span></span> <span data-ttu-id="dcec6-113">Det förutsätts även att du är kan toosatisfy hello skapa krav som anges nedan för Azure och hello Batch- och lagringstjänster.</span><span class="sxs-lookup"><span data-stu-id="dcec6-113">It also assumes that you're able toosatisfy hello account creation requirements that are specified below for Azure and hello Batch and Storage services.</span></span>

### <a name="accounts"></a><span data-ttu-id="dcec6-114">Konton</span><span class="sxs-lookup"><span data-stu-id="dcec6-114">Accounts</span></span>
* <span data-ttu-id="dcec6-115">**Azure-konto**: Om du inte redan har en Azure-prenumeration kan du [skapa ett kostnadsfritt Azure-konto][azure_free_account].</span><span class="sxs-lookup"><span data-stu-id="dcec6-115">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account][azure_free_account].</span></span>
* <span data-ttu-id="dcec6-116">**Batch-konto**: När du har skaffat en Azure-prenumeration [skapar du ett Azure Batch-konto](batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="dcec6-116">**Batch account**: Once you have an Azure subscription, [create an Azure Batch account](batch-account-create-portal.md).</span></span>
* <span data-ttu-id="dcec6-117">**Lagringskonto**: Se [skapar ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) i [Om Azure-lagringskonton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="dcec6-117">**Storage account**: See [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dcec6-118">Batch för närvarande stöder *endast* hello **allmänna** lagringskontotypen, enligt beskrivningen i steg #5 [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) i [om Azure Storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="dcec6-118">Batch currently supports *only* hello **general-purpose** storage account type, as described in step #5 [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
>
>

### <a name="visual-studio"></a><span data-ttu-id="dcec6-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dcec6-119">Visual Studio</span></span>
<span data-ttu-id="dcec6-120">Du måste ha **Visual Studio 2015 eller nyare** toobuild hello exempelprojektet.</span><span class="sxs-lookup"><span data-stu-id="dcec6-120">You must have **Visual Studio 2015 or newer** toobuild hello sample project.</span></span> <span data-ttu-id="dcec6-121">Du kan hitta gratis och utvärderingsversion versioner av Visual Studio i hello [översikt över Visual Studio produkter][visual_studio].</span><span class="sxs-lookup"><span data-stu-id="dcec6-121">You can find free and trial versions of Visual Studio in hello [overview of Visual Studio products][visual_studio].</span></span>

### <a name="dotnettutorial-code-sample"></a><span data-ttu-id="dcec6-122">*DotNetTutorial*kodexempel</span><span class="sxs-lookup"><span data-stu-id="dcec6-122">*DotNetTutorial* code sample</span></span>
<span data-ttu-id="dcec6-123">Hej [DotNetTutorial] [ github_dotnettutorial] exempel är en av många Batch-kodexempel hittades i hello hello [azure-batch-samples] [ github_samples] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="dcec6-123">hello [DotNetTutorial][github_dotnettutorial] sample is one of hello many Batch code samples found in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="dcec6-124">Du kan hämta alla hello prover genom att klicka på **kloning eller hämta > hämta ZIP** på startsidan för hello databasen eller genom att klicka på hello [azure-batch-exempel-master.zip] [ github_samples_zip]direkt hämtningslänken.</span><span class="sxs-lookup"><span data-stu-id="dcec6-124">You can download all hello samples by clicking  **Clone or download > Download ZIP** on hello repository home page, or by clicking hello [azure-batch-samples-master.zip][github_samples_zip] direct download link.</span></span> <span data-ttu-id="dcec6-125">När du har extraherat hello innehållet i hello ZIP-fil hittar hello lösning i hello följande mapp:</span><span class="sxs-lookup"><span data-stu-id="dcec6-125">Once you've extracted hello contents of hello ZIP file, you can find hello solution in hello following folder:</span></span>

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a><span data-ttu-id="dcec6-126">Azure Batch Explorer (valfritt)</span><span class="sxs-lookup"><span data-stu-id="dcec6-126">Azure Batch Explorer (optional)</span></span>
<span data-ttu-id="dcec6-127">Hej [Azure Batch Explorer] [ github_batchexplorer] är ett kostnadsfritt verktyg som ingår i hello [azure-batch-samples] [ github_samples] databasen på GitHub.</span><span class="sxs-lookup"><span data-stu-id="dcec6-127">hello [Azure Batch Explorer][github_batchexplorer] is a free utility that is included in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="dcec6-128">När krävs inte toocomplete självstudiekursen, det kan vara användbart när du skapar och felsöker Batch-lösningar.</span><span class="sxs-lookup"><span data-stu-id="dcec6-128">While not required toocomplete this tutorial, it can be useful while developing and debugging your Batch solutions.</span></span>

## <a name="dotnettutorial-sample-project-overview"></a><span data-ttu-id="dcec6-129">Översikt över DotNetTutorial-exempelprojekt</span><span class="sxs-lookup"><span data-stu-id="dcec6-129">DotNetTutorial sample project overview</span></span>
<span data-ttu-id="dcec6-130">Hej *DotNetTutorial* kodexempel är en Visual Studio-lösning som består av två projekt: **DotNetTutorial** och **TaskApplication**.</span><span class="sxs-lookup"><span data-stu-id="dcec6-130">hello *DotNetTutorial* code sample is a Visual Studio solution that consists of two projects: **DotNetTutorial** and **TaskApplication**.</span></span>

* <span data-ttu-id="dcec6-131">**DotNetTutorial** är hello klientprogram som interagerar med hello Batch- och Storage services tooexecute en parallell arbetsbelastning på compute-noder (virtuella datorer).</span><span class="sxs-lookup"><span data-stu-id="dcec6-131">**DotNetTutorial** is hello client application that interacts with hello Batch and Storage services tooexecute a parallel workload on compute nodes (virtual machines).</span></span> <span data-ttu-id="dcec6-132">DotNetTutorial körs på den lokala arbetsstationen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-132">DotNetTutorial runs on your local workstation.</span></span>
* <span data-ttu-id="dcec6-133">**TaskApplication** är hello-program som körs på beräkningsnoder i Azure tooperform hello verkligt arbete.</span><span class="sxs-lookup"><span data-stu-id="dcec6-133">**TaskApplication** is hello program that runs on compute nodes in Azure tooperform hello actual work.</span></span> <span data-ttu-id="dcec6-134">I exemplet hello `TaskApplication.exe` Parsar hello text i en fil som hämtas från Azure Storage (hello indatafilen).</span><span class="sxs-lookup"><span data-stu-id="dcec6-134">In hello sample, `TaskApplication.exe` parses hello text in a file downloaded from Azure Storage (hello input file).</span></span> <span data-ttu-id="dcec6-135">Sedan den genererar en textfil (hello utdata-fil) som innehåller en lista över hello de tre översta ord som visas i hello indatafilen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-135">Then it produces a text file (hello output file) that contains a list of hello top three words that appear in hello input file.</span></span> <span data-ttu-id="dcec6-136">När den skapar hello utdatafilen filöverföringar TaskApplication hello filen tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="dcec6-136">After it creates hello output file, TaskApplication uploads hello file tooAzure Storage.</span></span> <span data-ttu-id="dcec6-137">Detta gör att det tillgängliga toohello klientprogrammet för hämtning.</span><span class="sxs-lookup"><span data-stu-id="dcec6-137">This makes it available toohello client application for download.</span></span> <span data-ttu-id="dcec6-138">TaskApplication körs parallellt på flera beräkningsnoder i hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dcec6-138">TaskApplication runs in parallel on multiple compute nodes in hello Batch service.</span></span>

<span data-ttu-id="dcec6-139">hello följande diagram illustrerar hello primära åtgärder som utförs av hello klientprogrammet *DotNetTutorial*, och hello-program som körs av hello uppgifter *TaskApplication*.</span><span class="sxs-lookup"><span data-stu-id="dcec6-139">hello following diagram illustrates hello primary operations that are performed by hello client application, *DotNetTutorial*, and hello application that is executed by hello tasks, *TaskApplication*.</span></span> <span data-ttu-id="dcec6-140">Detta grundläggande arbetsflöde är typiskt för många beräkningslösningar som skapas med Batch.</span><span class="sxs-lookup"><span data-stu-id="dcec6-140">This basic workflow is typical of many compute solutions that are created with Batch.</span></span> <span data-ttu-id="dcec6-141">När den inte visar alla funktioner som är tillgängliga i hello Batch-tjänsten, omfattar nästan alla Batch-scenario delar av det här arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="dcec6-141">While it does not demonstrate every feature available in hello Batch service, nearly every Batch scenario includes portions of this workflow.</span></span>

<span data-ttu-id="dcec6-142">![Batch-exempelarbetsflöde][8]</span><span class="sxs-lookup"><span data-stu-id="dcec6-142">![Batch example workflow][8]</span></span><br/>

[<span data-ttu-id="dcec6-143">**Steg 1.**</span><span class="sxs-lookup"><span data-stu-id="dcec6-143">**Step 1.**</span></span>](#step-1-create-storage-containers) <span data-ttu-id="dcec6-144">Skapa **behållare** i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="dcec6-144">Create **containers** in Azure Blob Storage.</span></span><br/><span data-ttu-id="dcec6-145">
[**Steg 2.**](#step-2-upload-task-application-and-data-files)</span><span class="sxs-lookup"><span data-stu-id="dcec6-145">
[**Step 2.**](#step-2-upload-task-application-and-data-files)</span></span> <span data-ttu-id="dcec6-146">Överför uppgiften programfiler och indatafilerna toocontainers.</span><span class="sxs-lookup"><span data-stu-id="dcec6-146">Upload task application files and input files toocontainers.</span></span><br/><span data-ttu-id="dcec6-147">
[**Steg 3.**](#step-3-create-batch-pool)</span><span class="sxs-lookup"><span data-stu-id="dcec6-147">
[**Step 3.**](#step-3-create-batch-pool)</span></span> <span data-ttu-id="dcec6-148">Skapa en Batch-**pool**.</span><span class="sxs-lookup"><span data-stu-id="dcec6-148">Create a Batch **pool**.</span></span><br/>
  <span data-ttu-id="dcec6-149">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span><span class="sxs-lookup"><span data-stu-id="dcec6-149">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span></span> <span data-ttu-id="dcec6-150">Hej poolen **startuppgift har ställts** hämtningar hello uppgiften binära filer (TaskApplication) toonodes som de gå hello poolen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-150">hello pool **StartTask** downloads hello task binary files (TaskApplication) toonodes as they join hello pool.</span></span><br/><span data-ttu-id="dcec6-151">
[**Steg 4.**](#step-4-create-batch-job)</span><span class="sxs-lookup"><span data-stu-id="dcec6-151">
[**Step 4.**](#step-4-create-batch-job)</span></span> <span data-ttu-id="dcec6-152">Skapa ett Batch-**jobb**.</span><span class="sxs-lookup"><span data-stu-id="dcec6-152">Create a Batch **job**.</span></span><br/><span data-ttu-id="dcec6-153">
[**Steg 5.**](#step-5-add-tasks-to-job)</span><span class="sxs-lookup"><span data-stu-id="dcec6-153">
[**Step 5.**](#step-5-add-tasks-to-job)</span></span> <span data-ttu-id="dcec6-154">Lägg till **uppgifter** toohello jobb.</span><span class="sxs-lookup"><span data-stu-id="dcec6-154">Add **tasks** toohello job.</span></span><br/>
  <span data-ttu-id="dcec6-155">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span><span class="sxs-lookup"><span data-stu-id="dcec6-155">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span></span> <span data-ttu-id="dcec6-156">hello uppgifter som är schemalagda tooexecute på noder.</span><span class="sxs-lookup"><span data-stu-id="dcec6-156">hello tasks are scheduled tooexecute on nodes.</span></span><br/>
    <span data-ttu-id="dcec6-157">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span><span class="sxs-lookup"><span data-stu-id="dcec6-157">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span></span> <span data-ttu-id="dcec6-158">Varje aktivitet hämtar sina indata från Azure Storage och börjar sedan köra.</span><span class="sxs-lookup"><span data-stu-id="dcec6-158">Each task downloads its input data from Azure Storage, then begins execution.</span></span><br/><span data-ttu-id="dcec6-159">
[**Steg 6.**](#step-6-monitor-tasks)</span><span class="sxs-lookup"><span data-stu-id="dcec6-159">
[**Step 6.**](#step-6-monitor-tasks)</span></span> <span data-ttu-id="dcec6-160">Övervaka aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="dcec6-160">Monitor tasks.</span></span><br/>
  <span data-ttu-id="dcec6-161">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span><span class="sxs-lookup"><span data-stu-id="dcec6-161">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span></span> <span data-ttu-id="dcec6-162">PowerPoint, överför de sina utdata data tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="dcec6-162">As tasks are completed, they upload their output data tooAzure Storage.</span></span><br/><span data-ttu-id="dcec6-163">
[**Step 7.**](#step-7-download-task-output)</span><span class="sxs-lookup"><span data-stu-id="dcec6-163">
[**Step 7.**](#step-7-download-task-output)</span></span> <span data-ttu-id="dcec6-164">Hämta aktiviteternas utdata från Storage.</span><span class="sxs-lookup"><span data-stu-id="dcec6-164">Download task output from Storage.</span></span>

<span data-ttu-id="dcec6-165">Som tidigare nämnts kan inte varje Batch-lösning utför stegen exakt, och kan inkludera många fler, men hello *DotNetTutorial* -exempelprogrammet demonstrerar vanliga processer som hittades i en Batch-lösningen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-165">As mentioned, not every Batch solution performs these exact steps, and may include many more, but hello *DotNetTutorial* sample application demonstrates common processes found in a Batch solution.</span></span>

## <a name="build-hello-dotnettutorial-sample-project"></a><span data-ttu-id="dcec6-166">Skapa hello *DotNetTutorial* exempelprojektet</span><span class="sxs-lookup"><span data-stu-id="dcec6-166">Build hello *DotNetTutorial* sample project</span></span>
<span data-ttu-id="dcec6-167">Innan hello exempel kan köras, måste du ange både Batch och lagring autentiseringsuppgifter i hello *DotNetTutorial* projektets `Program.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="dcec6-167">Before you can successfully run hello sample, you must specify both Batch and Storage account credentials in hello *DotNetTutorial* project's `Program.cs` file.</span></span> <span data-ttu-id="dcec6-168">Om du redan inte har gjort det, öppna hello lösningen i Visual Studio genom att dubbelklicka på hello `DotNetTutorial.sln` lösningsfilen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-168">If you have not done so already, open hello solution in Visual Studio by double-clicking hello `DotNetTutorial.sln` solution file.</span></span> <span data-ttu-id="dcec6-169">Eller öppna den från Visual Studio med hjälp av hello **Arkiv > Öppna > projekt/lösning** menyn.</span><span class="sxs-lookup"><span data-stu-id="dcec6-169">Or open it from within Visual Studio by using hello **File > Open > Project/Solution** menu.</span></span>

<span data-ttu-id="dcec6-170">Öppna `Program.cs` inom hello *DotNetTutorial* projekt.</span><span class="sxs-lookup"><span data-stu-id="dcec6-170">Open `Program.cs` within hello *DotNetTutorial* project.</span></span> <span data-ttu-id="dcec6-171">Lägg sedan till dina autentiseringsuppgifter som anges i hello övre delen av filen hello:</span><span class="sxs-lookup"><span data-stu-id="dcec6-171">Then add your credentials as specified near hello top of hello file:</span></span>

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
> <span data-ttu-id="dcec6-172">Som nämnts ovan är du för närvarande måste ange hello autentiseringsuppgifter för en **allmänna** storage-konto i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dcec6-172">As mentioned above, you must currently specify hello credentials for a **general-purpose** storage account in Azure Storage.</span></span> <span data-ttu-id="dcec6-173">Batch-program använda blob storage inom hello **allmänna** storage-konto.</span><span class="sxs-lookup"><span data-stu-id="dcec6-173">Your Batch applications use blob storage within hello **general-purpose** storage account.</span></span> <span data-ttu-id="dcec6-174">Ange inte hello autentiseringsuppgifter för ett lagringskonto som har skapats genom att välja hello *Blob storage* kontotyp.</span><span class="sxs-lookup"><span data-stu-id="dcec6-174">Do not specify hello credentials for a Storage account that was created by selecting hello *Blob storage* account type.</span></span>
>
>

<span data-ttu-id="dcec6-175">Du hittar Batch- och autentiseringsuppgifterna för ditt konto i hello kontoblad för varje tjänst i hello [Azure-portalen][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="dcec6-175">You can find your Batch and Storage account credentials within hello account blade of each service in hello [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="dcec6-176">![Batch-autentiseringsuppgifter i hello portal][9]
![lagring autentiseringsuppgifter i hello-portalen][10]</span><span class="sxs-lookup"><span data-stu-id="dcec6-176">![Batch credentials in hello portal][9]
![Storage credentials in hello portal][10]</span></span><br/>

<span data-ttu-id="dcec6-177">Nu när du har uppdaterat hello-projekt med dina autentiseringsuppgifter, högerklicka på hello lösningen i Solution Explorer och klicka på **skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="dcec6-177">Now that you've updated hello project with your credentials, right-click hello solution in Solution Explorer and click **Build Solution**.</span></span> <span data-ttu-id="dcec6-178">Om du uppmanas bekräfta att hello återställas av NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="dcec6-178">Confirm hello restoration of any NuGet packages, if you're prompted.</span></span>

> [!TIP]
> <span data-ttu-id="dcec6-179">Om hello NuGet-paket inte återställs automatiskt, eller om du får felmeddelanden om ett fel toorestore hello-paket, kontrollera att du har hello [NuGet Package Manager] [ nuget_packagemgr] installerad.</span><span class="sxs-lookup"><span data-stu-id="dcec6-179">If hello NuGet packages are not automatically restored, or if you see errors about a failure toorestore hello packages, ensure that you have hello [NuGet Package Manager][nuget_packagemgr] installed.</span></span> <span data-ttu-id="dcec6-180">Aktivera sedan hello hämtning av saknade paket.</span><span class="sxs-lookup"><span data-stu-id="dcec6-180">Then enable hello download of missing packages.</span></span> <span data-ttu-id="dcec6-181">Se [aktiverar paketet återställa under skapa] [ nuget_restore] tooenable ladda ned paket.</span><span class="sxs-lookup"><span data-stu-id="dcec6-181">See [Enabling Package Restore During Build][nuget_restore] tooenable package download.</span></span>
>
>

<span data-ttu-id="dcec6-182">I följande avsnitt hello, vi dela upp hello exempelprogrammet på hello steg utförs tooprocess en arbetsbelastning i hello Batch-tjänsten och diskutera dessa steg i detalj.</span><span class="sxs-lookup"><span data-stu-id="dcec6-182">In hello following sections, we break down hello sample application into hello steps that it performs tooprocess a workload in hello Batch service, and discuss those steps in detail.</span></span> <span data-ttu-id="dcec6-183">Vi rekommenderar att du toorefer toohello öppna lösningen i Visual Studio medan du gå igenom hello resten av den här artikeln eftersom inte alla kodrad i hello exempel beskrivs.</span><span class="sxs-lookup"><span data-stu-id="dcec6-183">We encourage you toorefer toohello open solution in Visual Studio while you work your way through hello rest of this article, since not every line of code in hello sample is discussed.</span></span>

<span data-ttu-id="dcec6-184">Navigera toohello överkant hello `MainAsync` metod i hello *DotNetTutorial* projektets `Program.cs` filen toostart med steg 1.</span><span class="sxs-lookup"><span data-stu-id="dcec6-184">Navigate toohello top of hello `MainAsync` method in hello *DotNetTutorial* project's `Program.cs` file toostart with Step 1.</span></span> <span data-ttu-id="dcec6-185">Varje steg nedan och ungefär sätt hello utvecklingen av metoden anropar `MainAsync`.</span><span class="sxs-lookup"><span data-stu-id="dcec6-185">Each step below then roughly follows hello progression of method calls in `MainAsync`.</span></span>

## <a name="step-1-create-storage-containers"></a><span data-ttu-id="dcec6-186">Steg 1: Skapa Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="dcec6-186">Step 1: Create Storage containers</span></span>
<span data-ttu-id="dcec6-187">![Skapa behållare i Azure Storage][1]
</span><span class="sxs-lookup"><span data-stu-id="dcec6-187">![Create containers in Azure Storage][1]
</span></span><br/>

<span data-ttu-id="dcec6-188">Batch har inbyggt stöd för integrering med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dcec6-188">Batch includes built-in support for interacting with Azure Storage.</span></span> <span data-ttu-id="dcec6-189">Behållare på ditt lagringskonto tillhandahåller hello-filer som behövs i hello aktiviteter som körs i Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="dcec6-189">Containers in your Storage account will provide hello files needed by hello tasks that run in your Batch account.</span></span> <span data-ttu-id="dcec6-190">hello behållare också tillhandahålla en plats toostore hello utdata som producerar hello uppgifter.</span><span class="sxs-lookup"><span data-stu-id="dcec6-190">hello containers also provide a place toostore hello output data that hello tasks produce.</span></span> <span data-ttu-id="dcec6-191">hello först öppna hello *DotNetTutorial* klientprogrammet kan skapa tre behållare i [Azure Blob Storage](../storage/common/storage-introduction.md):</span><span class="sxs-lookup"><span data-stu-id="dcec6-191">hello first thing hello *DotNetTutorial* client application does is create three containers in [Azure Blob Storage](../storage/common/storage-introduction.md):</span></span>

* <span data-ttu-id="dcec6-192">**programmet**: den här behållaren lagrar hello-program som körs av hello uppgifter, samt någon av dess beroenden, till exempel DLL-filer.</span><span class="sxs-lookup"><span data-stu-id="dcec6-192">**application**: This container will store hello application run by hello tasks, as well as any of its dependencies, such as DLLs.</span></span>
* <span data-ttu-id="dcec6-193">**inkommande**: uppgifter hämtar hello data filer tooprocess från hello *inkommande* behållare.</span><span class="sxs-lookup"><span data-stu-id="dcec6-193">**input**: Tasks will download hello data files tooprocess from hello *input* container.</span></span>
* <span data-ttu-id="dcec6-194">**utdata**: när uppgifter bearbeta indatafilen, kommer de att överföra hello resultat toohello *utdata* behållare.</span><span class="sxs-lookup"><span data-stu-id="dcec6-194">**output**: When tasks complete input file processing, they will upload hello results toohello *output* container.</span></span>

<span data-ttu-id="dcec6-195">I ordning toointeract med en Storage-kontot och skapa behållare, använder vi hello [Azure Storage-klientbibliotek för .NET][net_api_storage].</span><span class="sxs-lookup"><span data-stu-id="dcec6-195">In order toointeract with a Storage account and create containers, we use hello [Azure Storage Client Library for .NET][net_api_storage].</span></span> <span data-ttu-id="dcec6-196">Vi skapar ett konto, referens toohello [CloudStorageAccount][net_cloudstorageaccount], och som skapar du en [CloudBlobClient][net_cloudblobclient]:</span><span class="sxs-lookup"><span data-stu-id="dcec6-196">We create a reference toohello account with [CloudStorageAccount][net_cloudstorageaccount], and from that create a [CloudBlobClient][net_cloudblobclient]:</span></span>

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

<span data-ttu-id="dcec6-197">Vi använder hello `blobClient` referera i hela programmet hello och skicka den som en parameter tooseveral metoder.</span><span class="sxs-lookup"><span data-stu-id="dcec6-197">We use hello `blobClient` reference throughout hello application and pass it as a parameter tooseveral methods.</span></span> <span data-ttu-id="dcec6-198">Ett exempel på detta är i hello kodblock som följer omedelbart hello ovan, där vi kallar `CreateContainerIfNotExistAsync` tooactually skapa hello behållare.</span><span class="sxs-lookup"><span data-stu-id="dcec6-198">An example of this is in hello code block that immediately follows hello above, where we call `CreateContainerIfNotExistAsync` tooactually create hello containers.</span></span>

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

<span data-ttu-id="dcec6-199">När du har skapat hello behållare hello program kan ladda upp hello-filer som ska användas av hello uppgifter.</span><span class="sxs-lookup"><span data-stu-id="dcec6-199">Once hello containers have been created, hello application can now upload hello files that will be used by hello tasks.</span></span>

> [!TIP]
> <span data-ttu-id="dcec6-200">[Hur toouse Blob Storage från .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) ger en bra översikt av arbetet med Azure Storage-behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="dcec6-200">[How toouse Blob Storage from .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) provides a good overview of working with Azure Storage containers and blobs.</span></span> <span data-ttu-id="dcec6-201">Det bör vara hello övre delen av listan läsning när du börjar arbeta med Batch.</span><span class="sxs-lookup"><span data-stu-id="dcec6-201">It should be near hello top of your reading list as you start working with Batch.</span></span>
>
>

## <a name="step-2-upload-task-application-and-data-files"></a><span data-ttu-id="dcec6-202">Steg 2: Ladda upp filer för aktivitetsprogram och indata</span><span class="sxs-lookup"><span data-stu-id="dcec6-202">Step 2: Upload task application and data files</span></span>
<span data-ttu-id="dcec6-203">![Överför aktivitet program och indata (data) filer toocontainers][2]
</span><span class="sxs-lookup"><span data-stu-id="dcec6-203">![Upload task application and input (data) files toocontainers][2]
</span></span><br/>

<span data-ttu-id="dcec6-204">Överför åtgärden, i filen hello *DotNetTutorial* först definiera samlingar av **program** och **inkommande** filen sökvägar som de finns på hello lokal dator.</span><span class="sxs-lookup"><span data-stu-id="dcec6-204">In hello file upload operation, *DotNetTutorial* first defines collections of **application** and **input** file paths as they exist on hello local machine.</span></span> <span data-ttu-id="dcec6-205">Sedan överför dessa filer toohello behållare som du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="dcec6-205">Then it uploads these files toohello containers that you created in hello previous step.</span></span>

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

<span data-ttu-id="dcec6-206">Det finns två metoder i `Program.cs` som är inblandade i hello överföringsprocessen:</span><span class="sxs-lookup"><span data-stu-id="dcec6-206">There are two methods in `Program.cs` that are involved in hello upload process:</span></span>

* <span data-ttu-id="dcec6-207">`UploadFilesToContainerAsync`: Den här metoden returnerar en mängd [ResourceFile] [ net_resourcefile] objekt (diskuteras nedan) och internt anrop `UploadFileToContainerAsync` tooupload varje fil som skickades i hello *filePaths* parameter.</span><span class="sxs-lookup"><span data-stu-id="dcec6-207">`UploadFilesToContainerAsync`: This method returns a collection of [ResourceFile][net_resourcefile] objects (discussed below) and internally calls `UploadFileToContainerAsync` tooupload each file that is passed in hello *filePaths* parameter.</span></span>
* <span data-ttu-id="dcec6-208">`UploadFileToContainerAsync`: Det här är hello-metod som faktiskt utför hello filöverföringen och skapar hello [ResourceFile] [ net_resourcefile] objekt.</span><span class="sxs-lookup"><span data-stu-id="dcec6-208">`UploadFileToContainerAsync`: This is hello method that actually performs hello file upload and creates hello [ResourceFile][net_resourcefile] objects.</span></span> <span data-ttu-id="dcec6-209">Efter överföring hello filen erhåller en signatur för delad åtkomst (SAS) för hello-filen och returnerar ett ResourceFile-objekt som representerar den.</span><span class="sxs-lookup"><span data-stu-id="dcec6-209">After uploading hello file, it obtains a shared access signature (SAS) for hello file and returns a ResourceFile object that represents it.</span></span> <span data-ttu-id="dcec6-210">Signaturer för delad åtkomst beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="dcec6-210">Shared access signatures are also discussed below.</span></span>

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

### <a name="resourcefiles"></a><span data-ttu-id="dcec6-211">ResourceFiles</span><span class="sxs-lookup"><span data-stu-id="dcec6-211">ResourceFiles</span></span>
<span data-ttu-id="dcec6-212">En [ResourceFile] [ net_resourcefile] innehåller aktiviteter i en Batch med hello URL tooa-fil i Azure Storage som är hämtade tooa beräkningsnod innan aktiviteten körs.</span><span class="sxs-lookup"><span data-stu-id="dcec6-212">A [ResourceFile][net_resourcefile] provides tasks in Batch with hello URL tooa file in Azure Storage that is downloaded tooa compute node before that task is run.</span></span> <span data-ttu-id="dcec6-213">Hej [ResourceFile.BlobSource] [ net_resourcefile_blobsource] egenskapen anger hello fullständig URL till hello fil eftersom den finns i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dcec6-213">hello [ResourceFile.BlobSource][net_resourcefile_blobsource] property specifies hello full URL of hello file as it exists in Azure Storage.</span></span> <span data-ttu-id="dcec6-214">hello-URL kan även innehålla en signatur för delad åtkomst (SAS) som tillhandahåller säker åtkomst till toohello-fil.</span><span class="sxs-lookup"><span data-stu-id="dcec6-214">hello URL may also include a shared access signature (SAS) that provides secure access toohello file.</span></span> <span data-ttu-id="dcec6-215">De flesta typer av aktiviteter i Batch .NET innehåller en *ResourceFiles*-egenskap, inklusive:</span><span class="sxs-lookup"><span data-stu-id="dcec6-215">Most tasks types within Batch .NET include a *ResourceFiles* property, including:</span></span>

* <span data-ttu-id="dcec6-216">[CloudTask][net_task]</span><span class="sxs-lookup"><span data-stu-id="dcec6-216">[CloudTask][net_task]</span></span>
* <span data-ttu-id="dcec6-217">[StartTask][net_pool_starttask]</span><span class="sxs-lookup"><span data-stu-id="dcec6-217">[StartTask][net_pool_starttask]</span></span>
* <span data-ttu-id="dcec6-218">[JobPreparationTask][net_jobpreptask]</span><span class="sxs-lookup"><span data-stu-id="dcec6-218">[JobPreparationTask][net_jobpreptask]</span></span>
* <span data-ttu-id="dcec6-219">[JobReleaseTask][net_jobreltask]</span><span class="sxs-lookup"><span data-stu-id="dcec6-219">[JobReleaseTask][net_jobreltask]</span></span>

<span data-ttu-id="dcec6-220">Hej DotNetTutorial exempelprogrammet använder inte hello JobPreparationTask eller JobReleaseTask aktivitetstyperna, men du kan läsa mer om dem i [kör jobbet förberedelse och slutförande uppgifter på Azure Batch-beräkningsnoder](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="dcec6-220">hello DotNetTutorial sample application does not use hello JobPreparationTask or JobReleaseTask task types, but you can read more about them in [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md).</span></span>

### <a name="shared-access-signature-sas"></a><span data-ttu-id="dcec6-221">Signatur för delad åtkomst (SAS)</span><span class="sxs-lookup"><span data-stu-id="dcec6-221">Shared access signature (SAS)</span></span>
<span data-ttu-id="dcec6-222">Delad åtkomst signaturer är strängar som, när ingår som en del av en URL, ger säker åtkomst toocontainers och blobbar i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dcec6-222">Shared access signatures are strings which—when included as part of a URL—provide secure access toocontainers and blobs in Azure Storage.</span></span> <span data-ttu-id="dcec6-223">hello DotNetTutorial programmet använder både blob och behållare delad åtkomst signatur URL: er och visar hur dessa delade tooobtain åt signatur strängar från hello Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="dcec6-223">hello DotNetTutorial application uses both blob and container shared access signature URLs, and demonstrates how tooobtain these shared access signature strings from hello Storage service.</span></span>

* <span data-ttu-id="dcec6-224">**BLOB-signaturer för delad åtkomst**: hello poolen startuppgift har ställts i DotNetTutorial använder signaturer för delad blob åtkomst vid nedladdning av hello binärfiler och indata filer från lagring (se steg #3 nedan).</span><span class="sxs-lookup"><span data-stu-id="dcec6-224">**Blob shared access signatures**: hello pool's StartTask in DotNetTutorial uses blob shared access signatures when it downloads hello application binaries and input data files from Storage (see Step #3 below).</span></span> <span data-ttu-id="dcec6-225">Hej `UploadFileToContainerAsync` metod i Dotnettutorial's `Program.cs` innehåller hello-kod som hämtar varje blob signatur för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dcec6-225">hello `UploadFileToContainerAsync` method in DotNetTutorial's `Program.cs` contains hello code that obtains each blob's shared access signature.</span></span> <span data-ttu-id="dcec6-226">Detta sker genom anrop till [CloudBlob.GetSharedAccessSignature][net_sas_blob].</span><span class="sxs-lookup"><span data-stu-id="dcec6-226">It does so by calling [CloudBlob.GetSharedAccessSignature][net_sas_blob].</span></span>
* <span data-ttu-id="dcec6-227">**Behållaren signaturer för delad åtkomst**: eftersom varje aktivitet har slutförts på hello beräkningsnod, laddar upp dess utdata filen toohello *utdata* behållare i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dcec6-227">**Container shared access signatures**: As each task finishes its work on hello compute node, it uploads its output file toohello *output* container in Azure Storage.</span></span> <span data-ttu-id="dcec6-228">toodo så TaskApplication använder en signatur för delad åtkomst behållare som ger skrivbehörighet toohello behållare som en del av hello sökvägen när den överför hello-fil.</span><span class="sxs-lookup"><span data-stu-id="dcec6-228">toodo so, TaskApplication uses a container shared access signature that provides write access toohello container as part of hello path when it uploads hello file.</span></span> <span data-ttu-id="dcec6-229">Signatur för delad åtkomst ges hello behållaren görs på ett liknande sätt som när erhålla hello blob delad åtkomstsignatur.</span><span class="sxs-lookup"><span data-stu-id="dcec6-229">Obtaining hello container shared access signature is done in a similar fashion as when obtaining hello blob shared access signature.</span></span> <span data-ttu-id="dcec6-230">I DotNetTutorial, hittar du den hello `GetContainerSasUrl` helper metodanrop [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] toodo så.</span><span class="sxs-lookup"><span data-stu-id="dcec6-230">In DotNetTutorial, you will find that hello `GetContainerSasUrl` helper method calls [CloudBlobContainer.GetSharedAccessSignature][net_sas_container] toodo so.</span></span> <span data-ttu-id="dcec6-231">Du kan läsa mer om hur TaskApplication använder hello behållaren delad åtkomstsignatur i ”steg 6: övervaka aktiviteter”.</span><span class="sxs-lookup"><span data-stu-id="dcec6-231">You'll read more about how TaskApplication uses hello container shared access signature in "Step 6: Monitor Tasks."</span></span>

> [!TIP]
> <span data-ttu-id="dcec6-232">Checka ut hello tvådelat serien på signaturer för delad åtkomst [del 1: Förstå hello delade signatur (SAS) modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md) och [del 2: skapa och använda en signatur för delad åtkomst (SAS) med Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn mer om hur du ger säker åtkomst toodata i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="dcec6-232">Check out hello two-part series on shared access signatures, [Part 1: Understanding hello shared access signature (SAS) model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) and [Part 2: Create and use a shared access signature (SAS) with Blob storage](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn more about providing secure access toodata in your Storage account.</span></span>
>
>

## <a name="step-3-create-batch-pool"></a><span data-ttu-id="dcec6-233">Steg 3: Skapa en Batch-pool</span><span class="sxs-lookup"><span data-stu-id="dcec6-233">Step 3: Create Batch pool</span></span>
<span data-ttu-id="dcec6-234">![Skapa en Batch-pool][3]
</span><span class="sxs-lookup"><span data-stu-id="dcec6-234">![Create a Batch pool][3]
</span></span><br/>

<span data-ttu-id="dcec6-235">En Batch-**pool** är en samling beräkningsnoder (virtuella datorer) där Batch utför aktiviteterna i ett jobb.</span><span class="sxs-lookup"><span data-stu-id="dcec6-235">A Batch **pool** is a collection of compute nodes (virtual machines) on which Batch executes a job's tasks.</span></span>

<span data-ttu-id="dcec6-236">Efter överföring hello program och data filer toohello Storage-konto med Azure Storage API: er, *DotNetTutorial* börjar att göra anrop toohello Batch-tjänsten med API: er som tillhandahålls av hello Batch .NET-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="dcec6-236">After uploading hello application and data files toohello Storage account with Azure Storage APIs, *DotNetTutorial* begins making calls toohello Batch service with APIs provided by hello Batch .NET library.</span></span> <span data-ttu-id="dcec6-237">hello kod skapar först ett [BatchClient][net_batchclient]:</span><span class="sxs-lookup"><span data-stu-id="dcec6-237">hello code first creates a [BatchClient][net_batchclient]:</span></span>

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

<span data-ttu-id="dcec6-238">Därefter hello exemplet skapas en pool med compute-noder i hello Batch-kontot med ett anrop för`CreatePoolIfNotExistsAsync`.</span><span class="sxs-lookup"><span data-stu-id="dcec6-238">Next, hello sample creates a pool of compute nodes in hello Batch account with a call too`CreatePoolIfNotExistsAsync`.</span></span> <span data-ttu-id="dcec6-239">`CreatePoolIfNotExistsAsync`använder hello [BatchClient.PoolOperations.CreatePool] [ net_pool_create] metoden toocreate en ny pool i hello Batch-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="dcec6-239">`CreatePoolIfNotExistsAsync` uses hello [BatchClient.PoolOperations.CreatePool][net_pool_create] method toocreate a new pool in hello Batch service:</span></span>

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

<span data-ttu-id="dcec6-240">När du skapar en pool med [CreatePool][net_pool_create], du anger flera parametrar, till exempel hello många compute-noder hello [storlek hello noder](../cloud-services/cloud-services-sizes-specs.md), och hello nodernas operativsystem system.</span><span class="sxs-lookup"><span data-stu-id="dcec6-240">When you create a pool with [CreatePool][net_pool_create], you specify several parameters such as hello number of compute nodes, hello [size of hello nodes](../cloud-services/cloud-services-sizes-specs.md), and hello nodes' operating system.</span></span> <span data-ttu-id="dcec6-241">I *DotNetTutorial*, använder vi [CloudServiceConfiguration] [ net_cloudserviceconfiguration] toospecify Windows Server 2012 R2 från [molntjänster](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="dcec6-241">In *DotNetTutorial*, we use [CloudServiceConfiguration][net_cloudserviceconfiguration] toospecify Windows Server 2012 R2 from [Cloud Services](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span> 

<span data-ttu-id="dcec6-242">Du kan också skapa pooler för compute-noder som är Azure virtuella datorer (VM) genom att ange hello [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] för din pool.</span><span class="sxs-lookup"><span data-stu-id="dcec6-242">You can also create pools of compute nodes that are Azure Virtual Machines (VMs) by specifying hello [VirtualMachineConfiguration][net_virtualmachineconfiguration] for your pool.</span></span> <span data-ttu-id="dcec6-243">Du kan skapa en pool med VM-beräkningsnoder från antingen Windows- eller [Linux-avbildningar](batch-linux-nodes.md).</span><span class="sxs-lookup"><span data-stu-id="dcec6-243">You can create a pool of VM compute nodes from either Windows or [Linux images](batch-linux-nodes.md).</span></span> <span data-ttu-id="dcec6-244">hello källa för VM-avbildningar kan vara antingen:</span><span class="sxs-lookup"><span data-stu-id="dcec6-244">hello source for your VM images can be either:</span></span>

- <span data-ttu-id="dcec6-245">Hej [Azure virtuella datorer Marketplace][vm_marketplace], som innehåller både Windows- och Linux-avbildningar som är färdiga att använda.</span><span class="sxs-lookup"><span data-stu-id="dcec6-245">hello [Azure Virtual Machines Marketplace][vm_marketplace], which provides both Windows and Linux images that are ready-to-use.</span></span> 
- <span data-ttu-id="dcec6-246">En anpassad avbildning som du förbereder och tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="dcec6-246">A custom image that you prepare and provide.</span></span> <span data-ttu-id="dcec6-247">Mer information om anpassade avbildningar finns i [Utveckla storskaliga parallella beräkningslösningar med Batch](batch-api-basics.md#pool).</span><span class="sxs-lookup"><span data-stu-id="dcec6-247">For more details about custom images, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#pool).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dcec6-248">Du debiteras för beräkningsresurser i Batch.</span><span class="sxs-lookup"><span data-stu-id="dcec6-248">You are charged for compute resources in Batch.</span></span> <span data-ttu-id="dcec6-249">toominimize kostnader, kan du sänka `targetDedicatedComputeNodes` too1 innan du kör hello exempel.</span><span class="sxs-lookup"><span data-stu-id="dcec6-249">toominimize costs, you can lower `targetDedicatedComputeNodes` too1 before you run hello sample.</span></span>
>
>

<span data-ttu-id="dcec6-250">Tillsammans med egenskaperna fysisk nod kan du också ange en [startuppgift har ställts] [ net_pool_starttask] för hello poolen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-250">Along with these physical node properties, you may also specify a [StartTask][net_pool_starttask] for hello pool.</span></span> <span data-ttu-id="dcec6-251">hello startuppgift har ställts körs på varje nod som noden ansluter hello poolen och varje gång en nod startas.</span><span class="sxs-lookup"><span data-stu-id="dcec6-251">hello StartTask executes on each node as that node joins hello pool, and each time a node is restarted.</span></span> <span data-ttu-id="dcec6-252">hello startuppgift har ställts är särskilt användbar för att installera program på compute-noder tidigare toohello körningen av uppgifter.</span><span class="sxs-lookup"><span data-stu-id="dcec6-252">hello StartTask is especially useful for installing applications on compute nodes prior toohello execution of tasks.</span></span> <span data-ttu-id="dcec6-253">Om aktiviteterna bearbeta data med hjälp av Python-skript kan använda du till exempel en startuppgift har ställts tooinstall Python på hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="dcec6-253">For example, if your tasks process data by using Python scripts, you could use a StartTask tooinstall Python on hello compute nodes.</span></span>

<span data-ttu-id="dcec6-254">I det här exempelprogrammet hello startuppgift har ställts kopierar hello-filer som den laddar ned från lagring (som anges med hello [startuppgift har ställts][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] egenskapen) från hello startuppgift har ställts directory toohello delade arbetskatalogen som *alla* aktiviteter som körs på hello nod kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="dcec6-254">In this sample application, hello StartTask copies hello files that it downloads from Storage (which are specified by using hello [StartTask][net_starttask].[ResourceFiles][net_starttask_resourcefiles] property) from hello StartTask working directory toohello shared directory that *all* tasks running on hello node can access.</span></span> <span data-ttu-id="dcec6-255">Då kopieras `TaskApplication.exe` och dess beroenden toohello delad katalog på varje nod som hello nod ansluter till hello poolen, så att alla aktiviteter som körs på hello nod kan komma åt den.</span><span class="sxs-lookup"><span data-stu-id="dcec6-255">Essentially, this copies `TaskApplication.exe` and its dependencies toohello shared directory on each node as hello node joins hello pool, so that any tasks that run on hello node can access it.</span></span>

> [!TIP]
> <span data-ttu-id="dcec6-256">Hej **programpaket** funktion i Azure Batch tillhandahåller ett annat sätt tooget tillämpningsprogrammet till hello beräkningsnoder i poolen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-256">hello **application packages** feature of Azure Batch provides another way tooget your application onto hello compute nodes in a pool.</span></span> <span data-ttu-id="dcec6-257">Se [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="dcec6-257">See [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md) for details.</span></span>
>
>

<span data-ttu-id="dcec6-258">Mest kända i hello kodstycke ovan är också hello användning av två miljövariabler i hello *CommandLine* -egenskapen för hello startuppgift har ställts: `%AZ_BATCH_TASK_WORKING_DIR%` och `%AZ_BATCH_NODE_SHARED_DIR%`.</span><span class="sxs-lookup"><span data-stu-id="dcec6-258">Also notable in hello code snippet above is hello use of two environment variables in hello *CommandLine* property of hello StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` and `%AZ_BATCH_NODE_SHARED_DIR%`.</span></span> <span data-ttu-id="dcec6-259">Varje compute-nod i ett Batch-pool konfigureras med flera miljövariabler som är specifika tooBatch automatiskt.</span><span class="sxs-lookup"><span data-stu-id="dcec6-259">Each compute node within a Batch pool is automatically configured with several environment variables that are specific tooBatch.</span></span> <span data-ttu-id="dcec6-260">En process som körs av en aktivitet har åtkomst toothese miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="dcec6-260">Any process that is executed by a task has access toothese environment variables.</span></span>

> [!TIP]
> <span data-ttu-id="dcec6-261">toofind mer information om hello miljövariabler som är tillgängliga på beräkningsnoder i en Batch-pool och information om aktiviteten fungerande kataloger finns hello [miljöinställningar för uppgifter](batch-api-basics.md#environment-settings-for-tasks) och [filer och kataloger ](batch-api-basics.md#files-and-directories) avsnitt i hello [Batch funktionsöversikt för utvecklare](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="dcec6-261">toofind out more about hello environment variables that are available on compute nodes in a Batch pool, and information on task working directories, see hello [Environment settings for tasks](batch-api-basics.md#environment-settings-for-tasks) and [Files and directories](batch-api-basics.md#files-and-directories) sections in hello [Batch feature overview for developers](batch-api-basics.md).</span></span>
>
>

## <a name="step-4-create-batch-job"></a><span data-ttu-id="dcec6-262">Steg 4: Skapa ett Batch-jobb</span><span class="sxs-lookup"><span data-stu-id="dcec6-262">Step 4: Create Batch job</span></span>
<span data-ttu-id="dcec6-263">![Skapa ett Batch-jobb][4]</span><span class="sxs-lookup"><span data-stu-id="dcec6-263">![Create Batch job][4]</span></span><br/>

<span data-ttu-id="dcec6-264">Ett Batch-**jobb** är en samling aktiviteter och associeras med en pool av beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="dcec6-264">A Batch **job** is a collection of tasks, and is associated with a pool of compute nodes.</span></span> <span data-ttu-id="dcec6-265">hello aktiviteter i ett projekt köra på hello associerade poolens beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="dcec6-265">hello tasks in a job execute on hello associated pool's compute nodes.</span></span>

<span data-ttu-id="dcec6-266">Du kan använda ett jobb inte bara för att ordna och spåra uppgifter i relaterade arbetsbelastningar, men också för införande av vissa villkor – till exempel hello maximal körningstid för hello jobb (och därmed dess aktiviteter) samt prioritet i relationen tooother jobb i hello Batch konto.</span><span class="sxs-lookup"><span data-stu-id="dcec6-266">You can use a job not only for organizing and tracking tasks in related workloads, but also for imposing certain constraints--such as hello maximum runtime for hello job (and by extension, its tasks) as well as job priority in relation tooother jobs in hello Batch account.</span></span> <span data-ttu-id="dcec6-267">I det här exemplet är hello jobbet dock endast kopplade till hello-pool som skapades i steg #3.</span><span class="sxs-lookup"><span data-stu-id="dcec6-267">In this example, however, hello job is associated only with hello pool that was created in step #3.</span></span> <span data-ttu-id="dcec6-268">Inga ytterligare egenskaper har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="dcec6-268">No additional properties are configured.</span></span>

<span data-ttu-id="dcec6-269">Alla Batch-jobb är associerade med en specifik pool.</span><span class="sxs-lookup"><span data-stu-id="dcec6-269">All Batch jobs are associated with a specific pool.</span></span> <span data-ttu-id="dcec6-270">Den här associeringen anger vilka noder hello jobbuppgifter körs på.</span><span class="sxs-lookup"><span data-stu-id="dcec6-270">This association indicates which nodes hello job's tasks will execute on.</span></span> <span data-ttu-id="dcec6-271">Du kan ange detta med hjälp av hello [CloudJob.PoolInformation] [ net_job_poolinfo] egenskapen enligt hello kodfragmentet nedan.</span><span class="sxs-lookup"><span data-stu-id="dcec6-271">You specify this by using hello [CloudJob.PoolInformation][net_job_poolinfo] property, as shown in hello code snippet below.</span></span>

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

<span data-ttu-id="dcec6-272">Nu när ett jobb har skapats läggs aktiviteterna tooperform hello arbete.</span><span class="sxs-lookup"><span data-stu-id="dcec6-272">Now that a job has been created, tasks are added tooperform hello work.</span></span>

## <a name="step-5-add-tasks-toojob"></a><span data-ttu-id="dcec6-273">Steg 5: Lägg till uppgifter toojob</span><span class="sxs-lookup"><span data-stu-id="dcec6-273">Step 5: Add tasks toojob</span></span>
<span data-ttu-id="dcec6-274">![Lägga till uppgifter toojob][5]</span><span class="sxs-lookup"><span data-stu-id="dcec6-274">![Add tasks toojob][5]</span></span><br/><span data-ttu-id="dcec6-275">
*(1) uppgifter läggs toohello jobb, (2) hello aktiviteter är schemalagda toorun på noder och (3) hello uppgifter hämta hello data filer tooprocess*</span><span class="sxs-lookup"><span data-stu-id="dcec6-275">
*(1) Tasks are added toohello job, (2) hello tasks are scheduled toorun on nodes, and (3) hello tasks download hello data files tooprocess*</span></span>

<span data-ttu-id="dcec6-276">Batch **uppgifter** är hello enskilda arbetsenheter som kör på hello compute-noder.</span><span class="sxs-lookup"><span data-stu-id="dcec6-276">Batch **tasks** are hello individual units of work that execute on hello compute nodes.</span></span> <span data-ttu-id="dcec6-277">En aktivitet har en kommandorad och kör hello skript eller körbara filer som du anger att kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="dcec6-277">A task has a command line and runs hello scripts or executables that you specify in that command line.</span></span>

<span data-ttu-id="dcec6-278">tooactually utföra arbetet, aktiviteter måste läggas till tooa jobb.</span><span class="sxs-lookup"><span data-stu-id="dcec6-278">tooactually perform work, tasks must be added tooa job.</span></span> <span data-ttu-id="dcec6-279">Varje [CloudTask] [ net_task] konfigureras med hjälp av en kommandoradsegenskapen och [ResourceFiles] [ net_task_resourcefiles] (som i hello poolen startuppgift har ställts) som hello-aktiviteten hämtar toohello nod innan dess kommandoraden körs automatiskt.</span><span class="sxs-lookup"><span data-stu-id="dcec6-279">Each [CloudTask][net_task] is configured by using a command-line property and [ResourceFiles][net_task_resourcefiles] (as with hello pool's StartTask) that hello task downloads toohello node before its command line is automatically executed.</span></span> <span data-ttu-id="dcec6-280">I hello *DotNetTutorial* exempelprojektet varje uppgift bearbetar endast en fil.</span><span class="sxs-lookup"><span data-stu-id="dcec6-280">In hello *DotNetTutorial* sample project, each task processes only one file.</span></span> <span data-ttu-id="dcec6-281">Det betyder att dess ResourceFiles-samling kan innehålla ett enda element.</span><span class="sxs-lookup"><span data-stu-id="dcec6-281">Thus, its ResourceFiles collection contains a single element.</span></span>

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
> <span data-ttu-id="dcec6-282">När de har åtkomst till miljövariabler som `%AZ_BATCH_NODE_SHARED_DIR%` eller köra ett program som inte hittades i hello nod `PATH`, kommandorader för aktiviteten måste föregås av `cmd /c`.</span><span class="sxs-lookup"><span data-stu-id="dcec6-282">When they access environment variables such as `%AZ_BATCH_NODE_SHARED_DIR%` or execute an application not found in hello node's `PATH`, task command lines must be prefixed with `cmd /c`.</span></span> <span data-ttu-id="dcec6-283">Detta uttryckligen köra hello Kommandotolken och instruera tooterminate efter att utföra kommandot.</span><span class="sxs-lookup"><span data-stu-id="dcec6-283">This will explicitly execute hello command interpreter and instruct it tooterminate after carrying out your command.</span></span> <span data-ttu-id="dcec6-284">Det här kravet är inte nödvändigt om aktiviteterna köra ett program i hello nod `PATH` (exempelvis *robocopy.exe* eller *powershell.exe*) och inga miljövariabler som används.</span><span class="sxs-lookup"><span data-stu-id="dcec6-284">This requirement is unnecessary if your tasks execute an application in hello node's `PATH` (such as *robocopy.exe* or *powershell.exe*) and no environment variables are used.</span></span>
>
>

<span data-ttu-id="dcec6-285">Inom hello `foreach` loop i hello kodstycke ovan visas hello kommandoraden för hello aktivitet skapas så att tre kommandoradsargument som skickas för*TaskApplication.exe*:</span><span class="sxs-lookup"><span data-stu-id="dcec6-285">Within hello `foreach` loop in hello code snippet above, you can see that hello command line for hello task is constructed such that three command-line arguments are passed too*TaskApplication.exe*:</span></span>

1. <span data-ttu-id="dcec6-286">Hej **första argumentet** är hello sökvägen hello filen tooprocess.</span><span class="sxs-lookup"><span data-stu-id="dcec6-286">hello **first argument** is hello path of hello file tooprocess.</span></span> <span data-ttu-id="dcec6-287">Detta är hello lokal sökväg toohello fil eftersom den finns på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="dcec6-287">This is hello local path toohello file as it exists on hello node.</span></span> <span data-ttu-id="dcec6-288">När hello ResourceFile objekt i `UploadFileToContainerAsync` skapades ovan, hello filnamn har använts för den här egenskapen (som en parameter toohello ResourceFile konstruktor).</span><span class="sxs-lookup"><span data-stu-id="dcec6-288">When hello ResourceFile object in `UploadFileToContainerAsync` was first created above, hello file name was used for this property (as a parameter toohello ResourceFile constructor).</span></span> <span data-ttu-id="dcec6-289">Detta anger att hello-filen finns i hello samma katalog som *TaskApplication.exe*.</span><span class="sxs-lookup"><span data-stu-id="dcec6-289">This indicates that hello file can be found in hello same directory as *TaskApplication.exe*.</span></span>
2. <span data-ttu-id="dcec6-290">Hej **andra argumentet** anger att hello upp *N* ord ska skrivas toohello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-290">hello **second argument** specifies that hello top *N* words should be written toohello output file.</span></span> <span data-ttu-id="dcec6-291">I exemplet hello är detta hårdkodat så att hello översta tre ord skrivs toohello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-291">In hello sample, this is hard-coded so that hello top three words are written toohello output file.</span></span>
3. <span data-ttu-id="dcec6-292">Hej **tredje argumentet** är hello signatur för delad åtkomst (SAS) som tillhandahåller skrivbehörighet toohello **utdata** behållare i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dcec6-292">hello **third argument** is hello shared access signature (SAS) that provides write access toohello **output** container in Azure Storage.</span></span> <span data-ttu-id="dcec6-293">*TaskApplication.exe* använder detta delad åtkomst till URL: en signatur när den laddas upp hello utdata filen tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="dcec6-293">*TaskApplication.exe* uses this shared access signature URL when it uploads hello output file tooAzure Storage.</span></span> <span data-ttu-id="dcec6-294">Du hittar hello koden för den här i hello `UploadFileToContainer` metod i hello TaskApplication projekt `Program.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="dcec6-294">You can find hello code for this in hello `UploadFileToContainer` method in hello TaskApplication project's `Program.cs` file:</span></span>

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

## <a name="step-6-monitor-tasks"></a><span data-ttu-id="dcec6-295">Steg 6: Övervaka aktiviteter</span><span class="sxs-lookup"><span data-stu-id="dcec6-295">Step 6: Monitor tasks</span></span>
<span data-ttu-id="dcec6-296">![Övervaka aktiviteter][6]</span><span class="sxs-lookup"><span data-stu-id="dcec6-296">![Monitor tasks][6]</span></span><br/><span data-ttu-id="dcec6-297">
*hello klienten program (1) Övervakare hello uppgifter för slutförande och status slutfört och (2) hello uppgifter överför resultat data tooAzure lagring*</span><span class="sxs-lookup"><span data-stu-id="dcec6-297">
*hello client application (1) monitors hello tasks for completion and success status, and (2) hello tasks upload result data tooAzure Storage*</span></span>

<span data-ttu-id="dcec6-298">När uppgifter läggs tooa jobbet kommer de automatiskt i kö och schemalagda för körning på datornoderna inom hello poolen som är associerade med hello jobb.</span><span class="sxs-lookup"><span data-stu-id="dcec6-298">When tasks are added tooa job, they are automatically queued and scheduled for execution on compute nodes within hello pool associated with hello job.</span></span> <span data-ttu-id="dcec6-299">Batch hanterar baserat på hello-inställningar som du anger, alla uppgiften queuing, schemaläggning, försöker igen och andra uppgifter för administration av aktivitet för dig.</span><span class="sxs-lookup"><span data-stu-id="dcec6-299">Based on hello settings you specify, Batch handles all task queuing, scheduling, retrying, and other task administration duties for you.</span></span>

<span data-ttu-id="dcec6-300">Det finns flera metoder toomonitoring för körning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="dcec6-300">There are many approaches toomonitoring task execution.</span></span> <span data-ttu-id="dcec6-301">DotNetTutorial visar ett enkelt exempel som endast rapporterar om aktiviteterna har slutförts eller misslyckats, samt aktiviteternas status.</span><span class="sxs-lookup"><span data-stu-id="dcec6-301">DotNetTutorial shows a simple example that reports only on completion and task failure or success states.</span></span> <span data-ttu-id="dcec6-302">Inom hello `MonitorTasks` metod i Dotnettutorial's `Program.cs`, det finns tre Batch .NET-begrepp som backar upp diskussion.</span><span class="sxs-lookup"><span data-stu-id="dcec6-302">Within hello `MonitorTasks` method in DotNetTutorial's `Program.cs`, there are three Batch .NET concepts that warrant discussion.</span></span> <span data-ttu-id="dcec6-303">De förklaras nedan i den ordning som de förekommer:</span><span class="sxs-lookup"><span data-stu-id="dcec6-303">They are listed below in their order of appearance:</span></span>

1. <span data-ttu-id="dcec6-304">**ODATADetailLevel**: Det är viktigt att du anger [ODATADetailLevel][net_odatadetaillevel] i liståtgärder (till exempel när du hämtar en lista över aktiviteterna i ett jobb) för att upprätthålla Batch-programmets prestanda.</span><span class="sxs-lookup"><span data-stu-id="dcec6-304">**ODATADetailLevel**: Specifying [ODATADetailLevel][net_odatadetaillevel] in list operations (such as obtaining a list of a job's tasks) is essential in ensuring Batch application performance.</span></span> <span data-ttu-id="dcec6-305">Lägg till [fråga hello Azure Batch-tjänsten effektivt](batch-efficient-list-queries.md) tooyour läsa listan om du planerar att göra någon form av övervakning av innehållsstatus i Batch-program.</span><span class="sxs-lookup"><span data-stu-id="dcec6-305">Add [Query hello Azure Batch service efficiently](batch-efficient-list-queries.md) tooyour reading list if you plan on doing any sort of status monitoring within your Batch applications.</span></span>
2. <span data-ttu-id="dcec6-306">**TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor] tillhandahåller .NET-program för Batch med verktyg som hjälper dig att övervaka aktiviteternas status.</span><span class="sxs-lookup"><span data-stu-id="dcec6-306">**TaskStateMonitor**: [TaskStateMonitor][net_taskstatemonitor] provides Batch .NET applications with helper utilities for monitoring task states.</span></span> <span data-ttu-id="dcec6-307">I `MonitorTasks`, *DotNetTutorial* väntar på att alla aktiviteter tooreach [TaskState.Completed] [ net_taskstate] inom en tidsgräns.</span><span class="sxs-lookup"><span data-stu-id="dcec6-307">In `MonitorTasks`, *DotNetTutorial* waits for all tasks tooreach [TaskState.Completed][net_taskstate] within a time limit.</span></span> <span data-ttu-id="dcec6-308">Därefter avslutas hello jobb.</span><span class="sxs-lookup"><span data-stu-id="dcec6-308">Then it terminates hello job.</span></span>
3. <span data-ttu-id="dcec6-309">**TerminateJobAsync**: avslutar ett jobb med [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (eller hello blockerar JobOperations.TerminateJob) markerar jobbet som slutförd.</span><span class="sxs-lookup"><span data-stu-id="dcec6-309">**TerminateJobAsync**: Terminating a job with [JobOperations.TerminateJobAsync][net_joboperations_terminatejob] (or hello blocking JobOperations.TerminateJob) marks that job as completed.</span></span> <span data-ttu-id="dcec6-310">Det är viktigt toodo så om Batch-lösningen använder en [JobReleaseTask][net_jobreltask].</span><span class="sxs-lookup"><span data-stu-id="dcec6-310">It is essential toodo so if your Batch solution uses a [JobReleaseTask][net_jobreltask].</span></span> <span data-ttu-id="dcec6-311">Det här är en särskild typ av aktivitet, som beskrivs i [Aktiviteter för att förbereda och slutföra jobb](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="dcec6-311">This is a special type of task, which is described in [Job preparation and completion tasks](batch-job-prep-release.md).</span></span>

<span data-ttu-id="dcec6-312">Hej `MonitorTasks` metod från *DotNetTutorial*'s `Program.cs` visas nedan:</span><span class="sxs-lookup"><span data-stu-id="dcec6-312">hello `MonitorTasks` method from *DotNetTutorial*'s `Program.cs` appears below:</span></span>

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

## <a name="step-7-download-task-output"></a><span data-ttu-id="dcec6-313">Steg 7: Hämta aktivitetsutdata</span><span class="sxs-lookup"><span data-stu-id="dcec6-313">Step 7: Download task output</span></span>
<span data-ttu-id="dcec6-314">![Hämta aktivitetsutdata från Storage][7]</span><span class="sxs-lookup"><span data-stu-id="dcec6-314">![Download task output from Storage][7]</span></span><br/>

<span data-ttu-id="dcec6-315">Nu när hello jobbet har slutförts kan hello utdata från hello uppgifter laddas ned från Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="dcec6-315">Now that hello job is completed, hello output from hello tasks can be downloaded from Azure Storage.</span></span> <span data-ttu-id="dcec6-316">Detta görs med ett anrop för`DownloadBlobsFromContainerAsync` i *DotNetTutorial*'s `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="dcec6-316">This is done with a call too`DownloadBlobsFromContainerAsync` in *DotNetTutorial*'s `Program.cs`:</span></span>

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
> <span data-ttu-id="dcec6-317">Hej anrop för`DownloadBlobsFromContainerAsync` i hello *DotNetTutorial* programmet anger hello filer att hämtade tooyour `%TEMP%` mapp.</span><span class="sxs-lookup"><span data-stu-id="dcec6-317">hello call too`DownloadBlobsFromContainerAsync` in hello *DotNetTutorial* application specifies that hello files should be downloaded tooyour `%TEMP%` folder.</span></span> <span data-ttu-id="dcec6-318">Känna sig fria toomodify detta utdata plats.</span><span class="sxs-lookup"><span data-stu-id="dcec6-318">Feel free toomodify this output location.</span></span>
>
>

## <a name="step-8-delete-containers"></a><span data-ttu-id="dcec6-319">Steg 8: Ta bort behållare</span><span class="sxs-lookup"><span data-stu-id="dcec6-319">Step 8: Delete containers</span></span>
<span data-ttu-id="dcec6-320">Eftersom du debiteras för data som finns i Azure Storage är alltid en bra idé tooremove blob som inte längre behövs för Batch-jobb.</span><span class="sxs-lookup"><span data-stu-id="dcec6-320">Because you are charged for data that resides in Azure Storage, it's always a good idea tooremove blobs that are no longer needed for your Batch jobs.</span></span> <span data-ttu-id="dcec6-321">I Dotnettutorial's `Program.cs`, detta görs med tre anrop toohello hjälpmetod `DeleteContainerAsync`:</span><span class="sxs-lookup"><span data-stu-id="dcec6-321">In DotNetTutorial's `Program.cs`, this is done with three calls toohello helper method `DeleteContainerAsync`:</span></span>

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

<span data-ttu-id="dcec6-322">själva hello metoden bara hämtar en referens toohello behållare och anropar sedan [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:</span><span class="sxs-lookup"><span data-stu-id="dcec6-322">hello method itself merely obtains a reference toohello container, and then calls [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:</span></span>

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

## <a name="step-9-delete-hello-job-and-hello-pool"></a><span data-ttu-id="dcec6-323">Steg 9: Ta bort hello jobbet och hello pool</span><span class="sxs-lookup"><span data-stu-id="dcec6-323">Step 9: Delete hello job and hello pool</span></span>
<span data-ttu-id="dcec6-324">I hello sista steget är du tillfrågas toodelete hello jobbet och hello pool som har skapats av hello DotNetTutorial program.</span><span class="sxs-lookup"><span data-stu-id="dcec6-324">In hello final step, you're prompted toodelete hello job and hello pool that were created by hello DotNetTutorial application.</span></span> <span data-ttu-id="dcec6-325">Även om du inte debiteras för själva jobben och aktiviteterna *debiteras* du för beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="dcec6-325">Although you're not charged for jobs and tasks themselves, you *are* charged for compute nodes.</span></span> <span data-ttu-id="dcec6-326">Vi rekommenderar därför att du endast allokerar noder efter behov.</span><span class="sxs-lookup"><span data-stu-id="dcec6-326">Thus, we recommend that you allocate nodes only as needed.</span></span> <span data-ttu-id="dcec6-327">Borttagning av oanvända pooler kan ingå i din underhållsrutin.</span><span class="sxs-lookup"><span data-stu-id="dcec6-327">Deleting unused pools can be part of your maintenance process.</span></span>

<span data-ttu-id="dcec6-328">Hej BatchClient [JobOperations] [ net_joboperations] och [PoolOperations] [ net_pooloperations] båda har motsvarande borttagning metoder, som kallas om hello användaren bekräftar borttagning:</span><span class="sxs-lookup"><span data-stu-id="dcec6-328">hello BatchClient's [JobOperations][net_joboperations] and [PoolOperations][net_pooloperations] both have corresponding deletion methods, which are called if hello user confirms deletion:</span></span>

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
> <span data-ttu-id="dcec6-329">Tänk på att du debiteras för beräkningsresurser – du kan minimera kostnaden genom att ta bort oanvända pooler.</span><span class="sxs-lookup"><span data-stu-id="dcec6-329">Keep in mind that you are charged for compute resources—deleting unused pools will minimize cost.</span></span> <span data-ttu-id="dcec6-330">Tänk också på att poolen tar du bort alla beräkningsnoder i poolen och alla data på hello noder kommer att återställas när hello poolen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="dcec6-330">Also, be aware that deleting a pool deletes all compute nodes within that pool, and that any data on hello nodes will be unrecoverable after hello pool is deleted.</span></span>
>
>

## <a name="run-hello-dotnettutorial-sample"></a><span data-ttu-id="dcec6-331">Kör hello *DotNetTutorial* exempel</span><span class="sxs-lookup"><span data-stu-id="dcec6-331">Run hello *DotNetTutorial* sample</span></span>
<span data-ttu-id="dcec6-332">När du kör hello exempelprogrammet blir hello konsolens utdata liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="dcec6-332">When you run hello sample application, hello console output will be similar toohello following.</span></span> <span data-ttu-id="dcec6-333">Under körning och du får en paus på `Awaiting task completion, timeout in 00:30:00...` medan hello poolen compute-noder startas.</span><span class="sxs-lookup"><span data-stu-id="dcec6-333">During execution, you will experience a pause at `Awaiting task completion, timeout in 00:30:00...` while hello pool's compute nodes are started.</span></span> <span data-ttu-id="dcec6-334">Använd hello [Azure-portalen] [ azure_portal] toomonitor din pool, compute-noder, jobb och uppgifter under och efter körning.</span><span class="sxs-lookup"><span data-stu-id="dcec6-334">Use hello [Azure portal][azure_portal] toomonitor your pool, compute nodes, job, and tasks during and after execution.</span></span> <span data-ttu-id="dcec6-335">Använd hello [Azure-portalen] [ azure_portal] eller hello [Azure Lagringsutforskaren] [ storage_explorers] tooview hello lagringsresurser (behållare och blobbar) som är skapat av hello program.</span><span class="sxs-lookup"><span data-stu-id="dcec6-335">Use hello [Azure portal][azure_portal] or hello [Azure Storage Explorer][storage_explorers] tooview hello Storage resources (containers and blobs) that are created by hello application.</span></span>

<span data-ttu-id="dcec6-336">Vanliga körningstid är **cirka 5 minuter** när du kör programmet hello i standardkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-336">Typical execution time is **approximately 5 minutes** when you run hello application in its default configuration.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="dcec6-337">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dcec6-337">Next steps</span></span>
<span data-ttu-id="dcec6-338">Känna sig fria toomake ändringar för*DotNetTutorial* och *TaskApplication* tooexperiment med olika compute scenarier.</span><span class="sxs-lookup"><span data-stu-id="dcec6-338">Feel free toomake changes too*DotNetTutorial* and *TaskApplication* tooexperiment with different compute scenarios.</span></span> <span data-ttu-id="dcec6-339">Till exempel försök att lägga till en körning fördröjning för*TaskApplication*, till exempel med [Thread.Sleep][net_thread_sleep], toosimulate tidskrävande uppgifter och övervaka dem i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="dcec6-339">For example, try adding an execution delay too*TaskApplication*, such as with [Thread.Sleep][net_thread_sleep], toosimulate long-running tasks and monitor them in hello portal.</span></span> <span data-ttu-id="dcec6-340">Försök att lägga till fler aktiviteter eller justera hello antalet compute-noder.</span><span class="sxs-lookup"><span data-stu-id="dcec6-340">Try adding more tasks or adjusting hello number of compute nodes.</span></span> <span data-ttu-id="dcec6-341">Lägg till logik toocheck för och Tillåt hello användning av en befintlig programpool toospeed körningstid (*tipset*: kolla `ArticleHelpers.cs` i hello [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] projektet i [azure-batch-samples][github_samples]).</span><span class="sxs-lookup"><span data-stu-id="dcec6-341">Add logic toocheck for and allow hello use of an existing pool toospeed execution time (*hint*: check out `ArticleHelpers.cs` in hello [Microsoft.Azure.Batch.Samples.Common][github_samples_common] project in [azure-batch-samples][github_samples]).</span></span>

<span data-ttu-id="dcec6-342">Nu när du är bekant med grundläggande hello-arbetsflöde i en Batch-lösning är tid toodig i toohello ytterligare funktioner för hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dcec6-342">Now that you're familiar with hello basic workflow of a Batch solution, it's time toodig in toohello additional features of hello Batch service.</span></span>

* <span data-ttu-id="dcec6-343">Granska hello [översikt över Azure Batch funktioner](batch-api-basics.md) artikel som vi rekommenderar att om du är ny toohello tjänst.</span><span class="sxs-lookup"><span data-stu-id="dcec6-343">Review hello [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new toohello service.</span></span>
* <span data-ttu-id="dcec6-344">Start på hello andra artiklar för utveckling av Batch under **Development djupgående** i hello [Batch Utbildningsväg][batch_learning_path].</span><span class="sxs-lookup"><span data-stu-id="dcec6-344">Start on hello other Batch development articles under **Development in-depth** in hello [Batch learning path][batch_learning_path].</span></span>
* <span data-ttu-id="dcec6-345">Checka ut en annan implementering av bearbetning hello ”främsta orden” arbetsbelastning med Batch i hello [TopNWords] [ github_topnwords] exempel.</span><span class="sxs-lookup"><span data-stu-id="dcec6-345">Check out a different implementation of processing hello "top N words" workload by using Batch in hello [TopNWords][github_topnwords] sample.</span></span>
* <span data-ttu-id="dcec6-346">Granska hello Batch .NET [viktig information](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) för hello senaste ändringarna i hello-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="dcec6-346">Review hello Batch .NET [release notes](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/Batch/DataPlane/changelog.md#azurebatch-release-notes) for hello latest changes in hello library.</span></span>

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
