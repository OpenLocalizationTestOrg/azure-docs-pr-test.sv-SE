---
title: "aaaInstall programpaket på datornoderna - Azure Batch | Microsoft Docs"
description: "Använd hello tillämpningsfunktion paket för Azure Batch tooeasily hantera flera program och versioner för installation på Batch-beräkningsnoder."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 683be7b7f1bd5db7835332016f6dccb72f45c3b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-applications-toocompute-nodes-with-batch-application-packages"></a><span data-ttu-id="959b0-103">Distribuera program toocompute noder med Batch-programpaket</span><span class="sxs-lookup"><span data-stu-id="959b0-103">Deploy applications toocompute nodes with Batch application packages</span></span>

<span data-ttu-id="959b0-104">hello programmet paket funktion i Azure Batch tillhandahåller enkel hantering av aktiviteten program och deras distribution toohello compute-noder i din pool.</span><span class="sxs-lookup"><span data-stu-id="959b0-104">hello application packages feature of Azure Batch provides easy management of task applications and their deployment toohello compute nodes in your pool.</span></span> <span data-ttu-id="959b0-105">Du kan använda programpaket, för att ladda upp och hantera flera versioner av hello program kör aktiviteter, inklusive tillhörande stödfiler.</span><span class="sxs-lookup"><span data-stu-id="959b0-105">With application packages, you can upload and manage multiple versions of hello applications your tasks run, including their supporting files.</span></span> <span data-ttu-id="959b0-106">Du kan sedan distribuera automatiskt en eller flera av dessa program toohello compute-noder i din pool.</span><span class="sxs-lookup"><span data-stu-id="959b0-106">You can then automatically deploy one or more of these applications toohello compute nodes in your pool.</span></span>

<span data-ttu-id="959b0-107">I den här artikeln får du lära dig hur tooupload och hantera programpaket i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="959b0-107">In this article, you will learn how tooupload and manage application packages in hello Azure portal.</span></span> <span data-ttu-id="959b0-108">Sedan får du lära dig hur tooinstall dem på en pool compute-noder med hello [Batch .NET] [ api_net] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="959b0-108">You will then learn how tooinstall them on a pool's compute nodes with hello [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="959b0-109">Programpaket kan användas för alla Batch-pooler som skapats efter 5 juli 2017.</span><span class="sxs-lookup"><span data-stu-id="959b0-109">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="959b0-110">De stöds i Batch-adresspooler skapa mellan 10 mars 2016 och 5 juli 2017 om hello poolen har skapats med en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="959b0-110">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if hello pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="959b0-111">Batch-pooler som skapats tidigare too10 mars 2016 stöder inte programpaket.</span><span class="sxs-lookup"><span data-stu-id="959b0-111">Batch pools created prior too10 March 2016 do not support application packages.</span></span>
>
> <span data-ttu-id="959b0-112">hello API: er för att skapa och hantera programpaket är en del av hello [Batch Management .NET] [[api_net_mgmt]] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="959b0-112">hello APIs for creating and managing application packages are part of hello [Batch Management .NET][[api_net_mgmt]] library.</span></span> <span data-ttu-id="959b0-113">hello API: er för att installera paket med program på en beräkningsnod är en del av hello [Batch .NET] [ api_net] bibliotek.</span><span class="sxs-lookup"><span data-stu-id="959b0-113">hello APIs for installing application packages on a compute node are part of hello [Batch .NET][api_net] library.</span></span>  
>
> <span data-ttu-id="959b0-114">hello paket tillämpningsfunktion beskrivs här ersätter hello Batch-appar funktion som är tillgänglig i tidigare versioner av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="959b0-114">hello application packages feature described here supersedes hello Batch Apps feature available in previous versions of hello service.</span></span>
> 
> 

## <a name="application-package-requirements"></a><span data-ttu-id="959b0-115">Paketet programkrav</span><span class="sxs-lookup"><span data-stu-id="959b0-115">Application package requirements</span></span>
<span data-ttu-id="959b0-116">toouse programpaket du behöver för[länka ett Azure Storage-konto](#link-a-storage-account) tooyour Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="959b0-116">toouse application packages, you need too[link an Azure Storage account](#link-a-storage-account) tooyour Batch account.</span></span>

<span data-ttu-id="959b0-117">Den här funktionen introducerades i [Batch REST API] [ api_rest] version 2015-12-01.2.2 och hello motsvarande [Batch .NET] [ api_net] version library 3.1.0.</span><span class="sxs-lookup"><span data-stu-id="959b0-117">This feature was introduced in [Batch REST API][api_rest] version 2015-12-01.2.2 and hello corresponding [Batch .NET][api_net] library version 3.1.0.</span></span> <span data-ttu-id="959b0-118">Vi rekommenderar att du alltid använder hello senaste API-versionen när du arbetar med Batch.</span><span class="sxs-lookup"><span data-stu-id="959b0-118">We recommend that you always use hello latest API version when working with Batch.</span></span>

> [!NOTE]
> <span data-ttu-id="959b0-119">Programpaket kan användas för alla Batch-pooler som skapats efter 5 juli 2017.</span><span class="sxs-lookup"><span data-stu-id="959b0-119">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="959b0-120">De stöds i Batch-adresspooler skapa mellan 10 mars 2016 och 5 juli 2017 om hello poolen har skapats med en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="959b0-120">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if hello pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="959b0-121">Batch-pooler som skapats tidigare too10 mars 2016 stöder inte programpaket.</span><span class="sxs-lookup"><span data-stu-id="959b0-121">Batch pools created prior too10 March 2016 do not support application packages.</span></span>
>
>

## <a name="about-applications-and-application-packages"></a><span data-ttu-id="959b0-122">Om program och programpaket</span><span class="sxs-lookup"><span data-stu-id="959b0-122">About applications and application packages</span></span>
<span data-ttu-id="959b0-123">I Azure Batch en *programmet* refererar tooa uppsättning version binärfiler som kan vara automatiskt hämtade toohello beräkningsnoder i din pool.</span><span class="sxs-lookup"><span data-stu-id="959b0-123">Within Azure Batch, an *application* refers tooa set of versioned binaries that can be automatically downloaded toohello compute nodes in your pool.</span></span> <span data-ttu-id="959b0-124">En *programpaket* refererar tooa *specifika* av dessa binärfiler och representerar en viss *version* av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="959b0-124">An *application package* refers tooa *specific set* of those binaries and represents a given *version* of hello application.</span></span>

<span data-ttu-id="959b0-125">![Översiktsdiagram över program och programpaket][1]</span><span class="sxs-lookup"><span data-stu-id="959b0-125">![High-level diagram of applications and application packages][1]</span></span>

### <a name="applications"></a><span data-ttu-id="959b0-126">Program</span><span class="sxs-lookup"><span data-stu-id="959b0-126">Applications</span></span>
<span data-ttu-id="959b0-127">Ett program i batchen innehåller en eller flera program paket och anger konfigurationsalternativ för hello program.</span><span class="sxs-lookup"><span data-stu-id="959b0-127">An application in Batch contains one or more application packages and specifies configuration options for hello application.</span></span> <span data-ttu-id="959b0-128">Ett program kan exempelvis ange hello standard programmet paketet version tooinstall på datornoder och om dess paket kan uppdateras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="959b0-128">For example, an application can specify hello default application package version tooinstall on compute nodes and whether its packages can be updated or deleted.</span></span>

### <a name="application-packages"></a><span data-ttu-id="959b0-129">Programpaket</span><span class="sxs-lookup"><span data-stu-id="959b0-129">Application packages</span></span>
<span data-ttu-id="959b0-130">Ett programpaket är en .zip-fil som innehåller hello binärfiler och stödfiler som krävs för uppgifter toorun hello programmet.</span><span class="sxs-lookup"><span data-stu-id="959b0-130">An application package is a .zip file that contains hello application binaries and supporting files that are required for your tasks toorun hello application.</span></span> <span data-ttu-id="959b0-131">Varje programpaket representerar en viss version av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="959b0-131">Each application package represents a specific version of hello application.</span></span>

<span data-ttu-id="959b0-132">Du kan ange programpaket på hello poolen och aktivitet.</span><span class="sxs-lookup"><span data-stu-id="959b0-132">You can specify application packages at hello pool and task levels.</span></span> <span data-ttu-id="959b0-133">Du kan ange en eller flera av dessa paket och (valfritt) en version när du skapar en pool eller aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="959b0-133">You can specify one or more of these packages and (optionally) a version when you create a pool or task.</span></span>

* <span data-ttu-id="959b0-134">**Poolen programpaket** distribueras för*varje* nod i hello pool.</span><span class="sxs-lookup"><span data-stu-id="959b0-134">**Pool application packages** are deployed too*every* node in hello pool.</span></span> <span data-ttu-id="959b0-135">Program distribueras när en nod ansluter till en pool och när den startas om eller avbildade.</span><span class="sxs-lookup"><span data-stu-id="959b0-135">Applications are deployed when a node joins a pool, and when it is rebooted or reimaged.</span></span>
  
    <span data-ttu-id="959b0-136">Poolen programpaket är lämplig när alla noder i en pool köra ett jobb aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="959b0-136">Pool application packages are appropriate when all nodes in a pool execute a job's tasks.</span></span> <span data-ttu-id="959b0-137">Du kan ange en eller flera programpaket när du skapar en pool och du kan lägga till eller uppdatera en befintlig adresspool paket.</span><span class="sxs-lookup"><span data-stu-id="959b0-137">You can specify one or more application packages when you create a pool, and you can add or update an existing pool's packages.</span></span> <span data-ttu-id="959b0-138">Om du uppdaterar en befintlig adresspool programpaket måste du starta om dess noder tooinstall hello nya paket.</span><span class="sxs-lookup"><span data-stu-id="959b0-138">If you update an existing pool's application packages, you must restart its nodes tooinstall hello new package.</span></span>
* <span data-ttu-id="959b0-139">**Uppgift programpaket** distribueras endast tooa beräkningsnod schemalagd toorun en aktivitet innan du kör kommandoraden hello aktivitet.</span><span class="sxs-lookup"><span data-stu-id="959b0-139">**Task application packages** are deployed only tooa compute node scheduled toorun a task, just before running hello task's command line.</span></span> <span data-ttu-id="959b0-140">Om hello anges programpaket och versionen finns redan på hello nod, det är inte omdistribueras och hello befintliga paketet används.</span><span class="sxs-lookup"><span data-stu-id="959b0-140">If hello specified application package and version is already on hello node, it is not redeployed and hello existing package is used.</span></span>
  
    <span data-ttu-id="959b0-141">Uppgiften programpaket är användbara i miljöer med delad pool, där olika jobben körs på en pool och hello poolen tas inte bort när ett jobb har slutförts.</span><span class="sxs-lookup"><span data-stu-id="959b0-141">Task application packages are useful in shared-pool environments, where different jobs are run on one pool, and hello pool is not deleted when a job is completed.</span></span> <span data-ttu-id="959b0-142">Om jobbet har färre uppgifter än noder i hello pool, kan aktivitet programpaket minimera dataöverföring eftersom programmet är distribuerade endast toohello noder som kör aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="959b0-142">If your job has fewer tasks than nodes in hello pool, task application packages can minimize data transfer since your application is deployed only toohello nodes that run tasks.</span></span>
  
    <span data-ttu-id="959b0-143">Andra scenarier kan dra nytta av aktiviteten programpaket finns jobb som kör ett stort program, men endast för några aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="959b0-143">Other scenarios that can benefit from task application packages are jobs that run a large application, but for only a few tasks.</span></span> <span data-ttu-id="959b0-144">Till exempel kan ett före bearbetning steg eller en merge-aktivitet, där hello föregående bearbetning eller merge-program är tunga, dra nytta av programpaket för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="959b0-144">For example, a pre-processing stage or a merge task, where hello pre-processing or merge application is heavyweight, may benefit from using task application packages.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="959b0-145">Det finns begränsningar på hello antal program och programpaket i ett batchkonto och hello största storlek för paketet.</span><span class="sxs-lookup"><span data-stu-id="959b0-145">There are restrictions on hello number of applications and application packages within a Batch account and on hello maximum application package size.</span></span> <span data-ttu-id="959b0-146">Se [kvoter och gränser för hello Azure Batch-tjänsten](batch-quota-limit.md) mer information om dessa begränsningar.</span><span class="sxs-lookup"><span data-stu-id="959b0-146">See [Quotas and limits for hello Azure Batch service](batch-quota-limit.md) for details about these limits.</span></span>
> 
> 

### <a name="benefits-of-application-packages"></a><span data-ttu-id="959b0-147">Fördelarna med programpaket</span><span class="sxs-lookup"><span data-stu-id="959b0-147">Benefits of application packages</span></span>
<span data-ttu-id="959b0-148">Programpaket kan förenkla hello koden i Batch-lösningen och lägre hello administration krävs toomanage hello program som körs i dina uppgifter.</span><span class="sxs-lookup"><span data-stu-id="959b0-148">Application packages can simplify hello code in your Batch solution and lower hello overhead required toomanage hello applications that your tasks run.</span></span>

<span data-ttu-id="959b0-149">Med programpaket saknar din pool startuppgift toospecify en lång lista med en enskild resurs filer tooinstall på hello noder.</span><span class="sxs-lookup"><span data-stu-id="959b0-149">With application packages, your pool's start task doesn't have toospecify a long list of individual resource files tooinstall on hello nodes.</span></span> <span data-ttu-id="959b0-150">Du har inte toomanually hantera flera versioner av dina filer i Azure Storage, eller på noderna.</span><span class="sxs-lookup"><span data-stu-id="959b0-150">You don't have toomanually manage multiple versions of your application files in Azure Storage, or on your nodes.</span></span> <span data-ttu-id="959b0-151">Och du inte behöver tooworry om hur du genererar [SAS-URL: er](../storage/common/storage-dotnet-shared-access-signature-part-1.md) tooprovide åtkomst toohello filer i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="959b0-151">And, you don't need tooworry about generating [SAS URLs](../storage/common/storage-dotnet-shared-access-signature-part-1.md) tooprovide access toohello files in your Storage account.</span></span> <span data-ttu-id="959b0-152">Batch-fungerar på hello bakgrund med Azure Storage toostore programpaket och distribuerar dem toocompute noder.</span><span class="sxs-lookup"><span data-stu-id="959b0-152">Batch works in hello background with Azure Storage toostore application packages and deploy them toocompute nodes.</span></span>

> [!NOTE] 
> <span data-ttu-id="959b0-153">hello total storlek på Startuppgiften måste vara mindre än eller lika med too32768 tecken, inklusive resursfiler och miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="959b0-153">hello total size of a start task must be less than or equal too32768 characters, including resource files and environment variables.</span></span> <span data-ttu-id="959b0-154">Om din startuppgift överskrider denna gräns, är med programpaket ett annat alternativ.</span><span class="sxs-lookup"><span data-stu-id="959b0-154">If your start task exceeds this limit, then using application packages is another option.</span></span> <span data-ttu-id="959b0-155">Du kan också skapa ett komprimerade Arkiv som innehåller din resursfiler, ladda upp den som en blob-tooAzure lagring och packa upp den från hello kommandorad för start-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="959b0-155">You can also create a zipped archive containing your resource files, upload it as a blob tooAzure Storage, and then unzip it from hello command line of your start task.</span></span> 
>
>

## <a name="upload-and-manage-applications"></a><span data-ttu-id="959b0-156">Ladda upp och hantera program</span><span class="sxs-lookup"><span data-stu-id="959b0-156">Upload and manage applications</span></span>
<span data-ttu-id="959b0-157">Du kan använda hello [Azure-portalen] [ portal] eller hello [Batch Management .NET](batch-management-dotnet.md) biblioteket toomanage hello programpaket Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="959b0-157">You can use hello [Azure portal][portal] or hello [Batch Management .NET](batch-management-dotnet.md) library toomanage hello application packages in your Batch account.</span></span> <span data-ttu-id="959b0-158">Nästa Hej i avsnitten, först visar vi hur toolink ett lagringskonto diskutera sedan att lägga till program och paket- och hantera dem med hello portal.</span><span class="sxs-lookup"><span data-stu-id="959b0-158">In hello next few sections, we first show how toolink a Storage account, then discuss adding applications and packages and managing them with hello portal.</span></span>

### <a name="link-a-storage-account"></a><span data-ttu-id="959b0-159">Länka ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="959b0-159">Link a Storage account</span></span>
<span data-ttu-id="959b0-160">toouse programpaket måste du först koppla ett Azure Storage-konto tooyour Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="959b0-160">toouse application packages, you must first link an Azure Storage account tooyour Batch account.</span></span> <span data-ttu-id="959b0-161">Om du ännu inte har konfigurerat ett lagringskonto hello Azure-portalen visas en varning hello första gången du klickar på hello **program** panelen i hello **Batch-kontot** bladet.</span><span class="sxs-lookup"><span data-stu-id="959b0-161">If you have not yet configured a Storage account, hello Azure portal displays a warning hello first time you click hello **Applications** tile in hello **Batch account** blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="959b0-162">Batch för närvarande stöder *endast* hello **allmänna** lagringskontotypen enligt beskrivningen i steg 5 [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account)i [om Azure Storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="959b0-162">Batch currently supports *only* hello **General-purpose** storage account type as described in step 5, [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account), in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="959b0-163">När du länkar ett Azure Storage-konto tooyour Batch-kontot, koppla *endast* en **allmänna** storage-konto.</span><span class="sxs-lookup"><span data-stu-id="959b0-163">When you link an Azure Storage account tooyour Batch account, link *only* a **General-purpose** storage account.</span></span>
> 
> 

<span data-ttu-id="959b0-164">!['Inget lagringskonto har konfigurerats, varning i Azure-portalen][9]</span><span class="sxs-lookup"><span data-stu-id="959b0-164">!['No storage account configured' warning in Azure portal][9]</span></span>

<span data-ttu-id="959b0-165">hello associerade Batch-tjänsten använder hello Storage-konto toostore ditt programpaket.</span><span class="sxs-lookup"><span data-stu-id="959b0-165">hello Batch service uses hello associated Storage account toostore your application packages.</span></span> <span data-ttu-id="959b0-166">När du har länkat hello två konton kan distribuera Batch automatiskt hello-paket som lagras i hello länkade Storage-konto tooyour compute-noder.</span><span class="sxs-lookup"><span data-stu-id="959b0-166">After you've linked hello two accounts, Batch can automatically deploy hello packages stored in hello linked Storage account tooyour compute nodes.</span></span> <span data-ttu-id="959b0-167">toolink en Storage-konto tooyour Batch-kontot, klickar du på **lagringsutrymmet kontoinställningar** på hello **varning** bladet och klicka sedan på **Lagringskonto** på hello **Lagringskonto** bladet.</span><span class="sxs-lookup"><span data-stu-id="959b0-167">toolink a Storage account tooyour Batch account, click **Storage account settings** on hello **Warning** blade, and then click **Storage Account** on hello **Storage Account** blade.</span></span>

<span data-ttu-id="959b0-168">![Välj lagring konto-bladet i Azure-portalen][10]</span><span class="sxs-lookup"><span data-stu-id="959b0-168">![Choose storage account blade in Azure portal][10]</span></span>

<span data-ttu-id="959b0-169">Vi rekommenderar att du skapar ett lagringskonto *specifikt* för användning med Batch-kontot och markera den här.</span><span class="sxs-lookup"><span data-stu-id="959b0-169">We recommend that you create a Storage account *specifically* for use with your Batch account, and select it here.</span></span> <span data-ttu-id="959b0-170">Mer information om hur toocreate ett lagringskonto i avsnittet ”Skapa ett lagringskonto” i [om Azure Storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="959b0-170">For details about how toocreate a storage account, see "Create a Storage account" in [About Azure Storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="959b0-171">När du har skapat ett lagringskonto kan du sedan koppla den tooyour Batch-kontot med hjälp av hello **Lagringskonto** bladet.</span><span class="sxs-lookup"><span data-stu-id="959b0-171">After you've created a Storage account, you can then link it tooyour Batch account by using hello **Storage Account** blade.</span></span>

> [!WARNING]
> <span data-ttu-id="959b0-172">hello Batch-tjänsten använder Azure Storage toostore ditt programpaket som blockblobar.</span><span class="sxs-lookup"><span data-stu-id="959b0-172">hello Batch service uses Azure Storage toostore your application packages as block blobs.</span></span> <span data-ttu-id="959b0-173">Du är [debiteras som vanligt] [ storage_pricing] för hello block blob-data.</span><span class="sxs-lookup"><span data-stu-id="959b0-173">You are [charged as normal][storage_pricing] for hello block blob data.</span></span> <span data-ttu-id="959b0-174">Vara säker på att tooconsider hello storlek och antal programpaket och tar regelbundet bort inaktuella paket toominimize kostnader.</span><span class="sxs-lookup"><span data-stu-id="959b0-174">Be sure tooconsider hello size and number of your application packages, and periodically remove deprecated packages toominimize costs.</span></span>
> 
> 

### <a name="view-current-applications"></a><span data-ttu-id="959b0-175">Visa aktuella program</span><span class="sxs-lookup"><span data-stu-id="959b0-175">View current applications</span></span>
<span data-ttu-id="959b0-176">tooview hello program i Batch-kontot, klickar du på hello **program** menyalternativet i hello vänstra menyn när du visar hello **Batch-kontot** bladet.</span><span class="sxs-lookup"><span data-stu-id="959b0-176">tooview hello applications in your Batch account, click hello **Applications** menu item in hello left menu while viewing hello **Batch account** blade.</span></span>

<span data-ttu-id="959b0-177">![Program sida vid sida][2]</span><span class="sxs-lookup"><span data-stu-id="959b0-177">![Applications tile][2]</span></span>

<span data-ttu-id="959b0-178">Om du markerar alternativet öppnas hello **program** bladet:</span><span class="sxs-lookup"><span data-stu-id="959b0-178">Selecting this menu option opens hello **Applications** blade:</span></span>

<span data-ttu-id="959b0-179">![Lista över program][3]</span><span class="sxs-lookup"><span data-stu-id="959b0-179">![List applications][3]</span></span>

<span data-ttu-id="959b0-180">Hej **program** bladet visar hello ID för varje program i ditt konto och hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="959b0-180">hello **Applications** blade displays hello ID of each application in your account and hello following properties:</span></span>

* <span data-ttu-id="959b0-181">**Paket**: hello antal versioner som är associerade med det här programmet.</span><span class="sxs-lookup"><span data-stu-id="959b0-181">**Packages**: hello number of versions associated with this application.</span></span>
* <span data-ttu-id="959b0-182">**Standardversionen**: hello programversion installeras om du inte anger en version när du anger hello program för en pool.</span><span class="sxs-lookup"><span data-stu-id="959b0-182">**Default version**: hello application version installed if you do not indicate a version when you specify hello application for a pool.</span></span> <span data-ttu-id="959b0-183">Den här inställningen är valfri.</span><span class="sxs-lookup"><span data-stu-id="959b0-183">This setting is optional.</span></span>
* <span data-ttu-id="959b0-184">**Tillåt uppdateringar**: hello-värde som anger om paketet uppdateringar, borttagningar och tillägg är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="959b0-184">**Allow updates**: hello value that specifies whether package updates, deletions, and additions are allowed.</span></span> <span data-ttu-id="959b0-185">Om värdet för**nr**, paketet uppdateringar och borttagningar är inaktiverat för hello program.</span><span class="sxs-lookup"><span data-stu-id="959b0-185">If this is set too**No**, package updates and deletions are disabled for hello application.</span></span> <span data-ttu-id="959b0-186">Endast nya programpaketversioner kan läggas till.</span><span class="sxs-lookup"><span data-stu-id="959b0-186">Only new application package versions can be added.</span></span> <span data-ttu-id="959b0-187">hello standardvärdet är **Ja**.</span><span class="sxs-lookup"><span data-stu-id="959b0-187">hello default is **Yes**.</span></span>

### <a name="view-application-details"></a><span data-ttu-id="959b0-188">Visa programinformation</span><span class="sxs-lookup"><span data-stu-id="959b0-188">View application details</span></span>
<span data-ttu-id="959b0-189">tooopen hello blad som innehåller hello information för ett program, Välj hello-program i hello **program** bladet.</span><span class="sxs-lookup"><span data-stu-id="959b0-189">tooopen hello blade that includes hello details for an application, select hello application in hello **Applications** blade.</span></span>

<span data-ttu-id="959b0-190">![Programinformation][4]</span><span class="sxs-lookup"><span data-stu-id="959b0-190">![Application details][4]</span></span>

<span data-ttu-id="959b0-191">Du kan konfigurera följande inställningar för programmet hello informationsbladet för hello program.</span><span class="sxs-lookup"><span data-stu-id="959b0-191">In hello application details blade, you can configure hello following settings for your application.</span></span>

* <span data-ttu-id="959b0-192">**Tillåt uppdateringar**: Ange om dess programpaket kan uppdateras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="959b0-192">**Allow updates**: Specify whether its application packages can be updated or deleted.</span></span> <span data-ttu-id="959b0-193">Se ”uppdatera eller ta bort ett programpaket” senare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="959b0-193">See "Update or Delete an application package" later in this article.</span></span>
* <span data-ttu-id="959b0-194">**Standardversionen**: Ange en standard programmet paketet toodeploy toocompute noder.</span><span class="sxs-lookup"><span data-stu-id="959b0-194">**Default version**: Specify a default application package toodeploy toocompute nodes.</span></span>
* <span data-ttu-id="959b0-195">**Visningsnamn**: Ange ett eget namn som din lösning kan använda när den visas information om programmet hello, till exempel i hello Användargränssnittet för en tjänst som du anger tooyour kunder via Batch Batch.</span><span class="sxs-lookup"><span data-stu-id="959b0-195">**Display name**: Specify a friendly name that your Batch solution can use when it displays information about hello application, for example, in hello UI of a service that you provide tooyour customers through Batch.</span></span>

### <a name="add-a-new-application"></a><span data-ttu-id="959b0-196">Lägg till ett nytt program</span><span class="sxs-lookup"><span data-stu-id="959b0-196">Add a new application</span></span>
<span data-ttu-id="959b0-197">toocreate nya program, lägga till ett programpaket och ange ett nytt, unikt-ID.</span><span class="sxs-lookup"><span data-stu-id="959b0-197">toocreate a new application, add an application package and specify a new, unique application ID.</span></span> <span data-ttu-id="959b0-198">hello första programpaketet som du lägger till med hello nya program-ID skapas också hello nytt program.</span><span class="sxs-lookup"><span data-stu-id="959b0-198">hello first application package that you add with hello new application ID also creates hello new application.</span></span>

<span data-ttu-id="959b0-199">Klicka på **Lägg till** på hello **program** bladet tooopen hello **nytt program** bladet.</span><span class="sxs-lookup"><span data-stu-id="959b0-199">Click **Add** on hello **Applications** blade tooopen hello **New application** blade.</span></span>

<span data-ttu-id="959b0-200">![Nya program bladet i Azure-portalen][5]</span><span class="sxs-lookup"><span data-stu-id="959b0-200">![New application blade in Azure portal][5]</span></span>

<span data-ttu-id="959b0-201">Hej **nytt program** bladet innehåller hello följande fält toospecify hello inställningarna för ditt nya program och programpaket.</span><span class="sxs-lookup"><span data-stu-id="959b0-201">hello **New application** blade provides hello following fields toospecify hello settings of your new application and application package.</span></span>

<span data-ttu-id="959b0-202">**Program-id**</span><span class="sxs-lookup"><span data-stu-id="959b0-202">**Application id**</span></span>

<span data-ttu-id="959b0-203">Det här fältet anger hello-ID för det nya programmet som är ämne toohello standard Azure Batch-ID valideringsregler.</span><span class="sxs-lookup"><span data-stu-id="959b0-203">This field specifies hello ID of your new application, which is subject toohello standard Azure Batch ID validation rules.</span></span> <span data-ttu-id="959b0-204">hello regler för att tillhandahålla ett program-ID är följande:</span><span class="sxs-lookup"><span data-stu-id="959b0-204">hello rules for providing an application ID are as follows:</span></span>

* <span data-ttu-id="959b0-205">På ett Windows kan hello ID innehålla en kombination av alfanumeriska tecken, bindestreck och understreck.</span><span class="sxs-lookup"><span data-stu-id="959b0-205">On Windows nodes, hello ID can contain any combination of alphanumeric characters, hyphens, and underscores.</span></span> <span data-ttu-id="959b0-206">På Linux-noder tillåts endast alfanumeriska tecken och understreck.</span><span class="sxs-lookup"><span data-stu-id="959b0-206">On Linux nodes, only alphanumeric characters and underscores are permitted.</span></span>
* <span data-ttu-id="959b0-207">Får inte innehålla fler än 64 tecken.</span><span class="sxs-lookup"><span data-stu-id="959b0-207">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="959b0-208">Måste vara unika inom hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="959b0-208">Must be unique within hello Batch account.</span></span>
* <span data-ttu-id="959b0-209">Är bevara och skiftlägesoberoende.</span><span class="sxs-lookup"><span data-stu-id="959b0-209">Is case-preserving and case-insensitive.</span></span>

<span data-ttu-id="959b0-210">**Version**</span><span class="sxs-lookup"><span data-stu-id="959b0-210">**Version**</span></span>

<span data-ttu-id="959b0-211">Det här fältet anger hello-versionen av hello programpaketet som du överför.</span><span class="sxs-lookup"><span data-stu-id="959b0-211">This field specifies hello version of hello application package you are uploading.</span></span> <span data-ttu-id="959b0-212">Version strängar är ämne toohello följande valideringsregler:</span><span class="sxs-lookup"><span data-stu-id="959b0-212">Version strings are subject toohello following validation rules:</span></span>

* <span data-ttu-id="959b0-213">På ett Windows kan hello versionssträng innehålla en kombination av alfanumeriska tecken, bindestreck, understreck och punkter.</span><span class="sxs-lookup"><span data-stu-id="959b0-213">On Windows nodes, hello version string can contain any combination of alphanumeric characters, hyphens, underscores, and periods.</span></span> <span data-ttu-id="959b0-214">För Linux-noder, får hello versionssträng bara innehålla alfanumeriska tecken och understreck.</span><span class="sxs-lookup"><span data-stu-id="959b0-214">On Linux nodes, hello version string can contain only alphanumeric characters and underscores.</span></span>
* <span data-ttu-id="959b0-215">Får inte innehålla fler än 64 tecken.</span><span class="sxs-lookup"><span data-stu-id="959b0-215">Cannot contain more than 64 characters.</span></span>
* <span data-ttu-id="959b0-216">Måste vara unika inom hello program.</span><span class="sxs-lookup"><span data-stu-id="959b0-216">Must be unique within hello application.</span></span>
* <span data-ttu-id="959b0-217">Är bevara och skiftlägesoberoende.</span><span class="sxs-lookup"><span data-stu-id="959b0-217">Are case-preserving and case-insensitive.</span></span>

<span data-ttu-id="959b0-218">**Programpaket**</span><span class="sxs-lookup"><span data-stu-id="959b0-218">**Application package**</span></span>

<span data-ttu-id="959b0-219">Det här fältet anger hello .zip-fil som innehåller hello binärfiler och stödfiler som krävs tooexecute hello program.</span><span class="sxs-lookup"><span data-stu-id="959b0-219">This field specifies hello .zip file that contains hello application binaries and supporting files that are required tooexecute hello application.</span></span> <span data-ttu-id="959b0-220">Klicka på hello **Välj en fil** rutan eller hello mappen ikonen toobrowse tooand väljer en .zip-fil som innehåller filer som ditt program.</span><span class="sxs-lookup"><span data-stu-id="959b0-220">Click hello **Select a file** box or hello folder icon toobrowse tooand select a .zip file that contains your application's files.</span></span>

<span data-ttu-id="959b0-221">När du har valt en fil, klickar du på **OK** toobegin hello överför tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="959b0-221">After you've selected a file, click **OK** toobegin hello upload tooAzure Storage.</span></span> <span data-ttu-id="959b0-222">När hello överföringen är klar, visas ett meddelande hello portal och stänger hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="959b0-222">When hello upload operation is complete, hello portal displays a notification and closes hello blade.</span></span> <span data-ttu-id="959b0-223">Den här åtgärden kan ta lite tid beroende på hello storleken på hello-filen som du är överföring och hello hastigheten för nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="959b0-223">Depending on hello size of hello file that you are uploading and hello speed of your network connection, this operation may take some time.</span></span>

> [!WARNING]
> <span data-ttu-id="959b0-224">Stäng inte hello **nytt program** bladet innan hello överföringen är klar.</span><span class="sxs-lookup"><span data-stu-id="959b0-224">Do not close hello **New application** blade before hello upload operation is complete.</span></span> <span data-ttu-id="959b0-225">Detta stoppar hello överföringen.</span><span class="sxs-lookup"><span data-stu-id="959b0-225">Doing so will stop hello upload process.</span></span>
> 
> 

### <a name="add-a-new-application-package"></a><span data-ttu-id="959b0-226">Lägg till ett nytt programpaket</span><span class="sxs-lookup"><span data-stu-id="959b0-226">Add a new application package</span></span>
<span data-ttu-id="959b0-227">tooadd en ny Paketversion för programmet för ett befintligt program väljer du en programmet hello **program** bladet, klickar du på **paket**, klicka på **Lägg till** tooopen Hej **Lägg till paketet** bladet.</span><span class="sxs-lookup"><span data-stu-id="959b0-227">tooadd a new application package version for an existing application, select an application in hello **Applications** blade, click **Packages**, then click **Add** tooopen hello **Add package** blade.</span></span>

<span data-ttu-id="959b0-228">![Lägg till program paketet bladet i Azure-portalen][8]</span><span class="sxs-lookup"><span data-stu-id="959b0-228">![Add application package blade in Azure portal][8]</span></span>

<span data-ttu-id="959b0-229">Som du ser hello fält matchar de hello **nytt program** bladet, men hello **program-id** kryssrutan är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="959b0-229">As you can see, hello fields match those of hello **New application** blade, but hello **Application id** box is disabled.</span></span> <span data-ttu-id="959b0-230">Som du gjorde för hello nytt program ange hello **Version** för din nya paket, bläddra tooyour **programpaket** ZIP-filen och klicka sedan på **OK** tooupload hello paketet.</span><span class="sxs-lookup"><span data-stu-id="959b0-230">As you did for hello new application, specify hello **Version** for your new package, browse tooyour **Application package** .zip file, then click **OK** tooupload hello package.</span></span>

### <a name="update-or-delete-an-application-package"></a><span data-ttu-id="959b0-231">Uppdatera eller ta bort ett programpaket</span><span class="sxs-lookup"><span data-stu-id="959b0-231">Update or delete an application package</span></span>
<span data-ttu-id="959b0-232">tooupdate eller ta bort ett befintligt programpaket, öppna hello informationsbladet för hello program klickar du på **paket** tooopen hello **paket** bladet, klickar du på hello **tre punkter**i hello raden i hello programpaketet som du vill toomodify och väljer hello åtgärd som du vill tooperform.</span><span class="sxs-lookup"><span data-stu-id="959b0-232">tooupdate or delete an existing application package, open hello details blade for hello application, click **Packages** tooopen hello **Packages** blade, click hello **ellipsis** in hello row of hello application package that you want toomodify, and select hello action that you want tooperform.</span></span>

<span data-ttu-id="959b0-233">![Uppdatera eller ta bort paketet i Azure-portalen][7]</span><span class="sxs-lookup"><span data-stu-id="959b0-233">![Update or delete package in Azure portal][7]</span></span>

<span data-ttu-id="959b0-234">**Uppdatering**</span><span class="sxs-lookup"><span data-stu-id="959b0-234">**Update**</span></span>

<span data-ttu-id="959b0-235">När du klickar på **uppdatering**, hello *uppdateringspaketet* bladet visas.</span><span class="sxs-lookup"><span data-stu-id="959b0-235">When you click **Update**, hello *Update package* blade is displayed.</span></span> <span data-ttu-id="959b0-236">Det här bladet är liknande toohello *nytt programpaket* bladet, men endast hello paketet markeringen fältet är aktiverat, så att du toospecify tooupload för en ny ZIP-filen.</span><span class="sxs-lookup"><span data-stu-id="959b0-236">This blade is similar toohello *New application package* blade, however only hello package selection field is enabled, allowing you toospecify a new ZIP file tooupload.</span></span>

<span data-ttu-id="959b0-237">![Uppdatera paketet bladet i Azure-portalen][11]</span><span class="sxs-lookup"><span data-stu-id="959b0-237">![Update package blade in Azure portal][11]</span></span>

<span data-ttu-id="959b0-238">**Ta bort**</span><span class="sxs-lookup"><span data-stu-id="959b0-238">**Delete**</span></span>

<span data-ttu-id="959b0-239">När du klickar på **ta bort**, du uppmanas tooconfirm hello borttagning av hello Paketversion och Batch tar bort hello-paket från Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="959b0-239">When you click **Delete**, you are asked tooconfirm hello deletion of hello package version, and Batch deletes hello package from Azure Storage.</span></span> <span data-ttu-id="959b0-240">Om du tar bort hello standardversionen av ett program hello **standardversionen** inställningen tas bort för hello program.</span><span class="sxs-lookup"><span data-stu-id="959b0-240">If you delete hello default version of an application, hello **Default version** setting is removed for hello application.</span></span>

<span data-ttu-id="959b0-241">![Ta bort programmet][12]</span><span class="sxs-lookup"><span data-stu-id="959b0-241">![Delete application ][12]</span></span>

## <a name="install-applications-on-compute-nodes"></a><span data-ttu-id="959b0-242">Installera program på datornoderna</span><span class="sxs-lookup"><span data-stu-id="959b0-242">Install applications on compute nodes</span></span>
<span data-ttu-id="959b0-243">Nu när du har lärt dig hur toomanage programmet paket med hello Azure-portalen, diskuterar hur toodeploy dem toocompute noder och köra dem med Batch-uppgifter.</span><span class="sxs-lookup"><span data-stu-id="959b0-243">Now that you've learned how toomanage application packages with hello Azure portal, we can discuss how toodeploy them toocompute nodes and run them with Batch tasks.</span></span>

### <a name="install-pool-application-packages"></a><span data-ttu-id="959b0-244">Installera poolen programpaket</span><span class="sxs-lookup"><span data-stu-id="959b0-244">Install pool application packages</span></span>
<span data-ttu-id="959b0-245">tooinstall ett programpaket på alla compute-noder i en pool, ange en eller flera programpaket *referenser* för hello poolen.</span><span class="sxs-lookup"><span data-stu-id="959b0-245">tooinstall an application package on all compute nodes in a pool, specify one or more application package *references* for hello pool.</span></span> <span data-ttu-id="959b0-246">hello-programpaket som du anger för en pool är installerade på varje beräkningsnod när noden ansluter hello poolen och hello nod startas om eller avbildade.</span><span class="sxs-lookup"><span data-stu-id="959b0-246">hello application packages that you specify for a pool are installed on each compute node when that node joins hello pool, and when hello node is rebooted or reimaged.</span></span>

<span data-ttu-id="959b0-247">Ange en eller flera i Batch .NET [CloudPool][net_cloudpool].[ ApplicationPackageReferences] [ net_cloudpool_pkgref] när du skapar en ny pool eller för en befintlig adresspool.</span><span class="sxs-lookup"><span data-stu-id="959b0-247">In Batch .NET, specify one or more [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref] when you create a new pool, or for an existing pool.</span></span> <span data-ttu-id="959b0-248">Hej [ApplicationPackageReference] [ net_pkgref] klassen anger en program-ID och version tooinstall på en pool compute-noder.</span><span class="sxs-lookup"><span data-stu-id="959b0-248">hello [ApplicationPackageReference][net_pkgref] class specifies an application ID and version tooinstall on a pool's compute nodes.</span></span>

```csharp
// Create hello unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Specify hello application and version tooinstall on hello compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit hello pool so that it's created in hello Batch service. As hello nodes join
// hello pool, hello specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> <span data-ttu-id="959b0-249">Om en distribution av paketet misslyckas av någon anledning hello Batch-tjänstemärken hello nod [oanvändbar][net_nodestate], och inga aktiviteter som är schemalagda för körning på noden.</span><span class="sxs-lookup"><span data-stu-id="959b0-249">If an application package deployment fails for any reason, hello Batch service marks hello node [unusable][net_nodestate], and no tasks are scheduled for execution on that node.</span></span> <span data-ttu-id="959b0-250">I det här fallet bör du **starta om** hello nod tooreinitiate hello paketdistributionen.</span><span class="sxs-lookup"><span data-stu-id="959b0-250">In this case, you should **restart** hello node tooreinitiate hello package deployment.</span></span> <span data-ttu-id="959b0-251">Starta om hello nod kan också schemaläggning igen på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="959b0-251">Restarting hello node also enables task scheduling again on hello node.</span></span>
> 
> 

### <a name="install-task-application-packages"></a><span data-ttu-id="959b0-252">Installera aktivitet programpaket</span><span class="sxs-lookup"><span data-stu-id="959b0-252">Install task application packages</span></span>
<span data-ttu-id="959b0-253">Liknande tooa poolen, ange programpaket *referenser* för en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="959b0-253">Similar tooa pool, you specify application package *references* for a task.</span></span> <span data-ttu-id="959b0-254">När en aktivitet är schemalagd toorun på en nod, hello paketet hämtas och extraheras precis innan hello aktivitetens kommandoraden körs.</span><span class="sxs-lookup"><span data-stu-id="959b0-254">When a task is scheduled toorun on a node, hello package is downloaded and extracted just before hello task's command line is executed.</span></span> <span data-ttu-id="959b0-255">Om ett angivet paket och version redan är installerad på noden hello hello paketet inte har hämtats och hello befintliga paketet används.</span><span class="sxs-lookup"><span data-stu-id="959b0-255">If a specified package and version is already installed on hello node, hello package is not downloaded and hello existing package is used.</span></span>

<span data-ttu-id="959b0-256">tooinstall ett programpaket för aktivitet, konfigurera hello aktivitet [CloudTask][net_cloudtask].[ ApplicationPackageReferences] [ net_cloudtask_pkgref] egenskapen:</span><span class="sxs-lookup"><span data-stu-id="959b0-256">tooinstall a task application package, configure hello task's [CloudTask][net_cloudtask].[ApplicationPackageReferences][net_cloudtask_pkgref] property:</span></span>

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-hello-installed-applications"></a><span data-ttu-id="959b0-257">Köra hello installerade program</span><span class="sxs-lookup"><span data-stu-id="959b0-257">Execute hello installed applications</span></span>
<span data-ttu-id="959b0-258">hello paket som du har angett för en pool eller aktivitet har hämtat och extraherat tooa med namnet katalog i hello `AZ_BATCH_ROOT_DIR` för hello-nod.</span><span class="sxs-lookup"><span data-stu-id="959b0-258">hello packages that you've specified for a pool or task are downloaded and extracted tooa named directory within hello `AZ_BATCH_ROOT_DIR` of hello node.</span></span> <span data-ttu-id="959b0-259">Batch skapar även en miljövariabel som innehåller hello sökvägen toohello heter katalogen.</span><span class="sxs-lookup"><span data-stu-id="959b0-259">Batch also creates an environment variable that contains hello path toohello named directory.</span></span> <span data-ttu-id="959b0-260">Din aktivitet kommandorader använda den här miljövariabeln referera till programmet hello på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="959b0-260">Your task command lines use this environment variable when referencing hello application on hello node.</span></span> 

<span data-ttu-id="959b0-261">På ett Windows är hello variabeln i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="959b0-261">On Windows nodes, hello variable is in hello following format:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

<span data-ttu-id="959b0-262">Hello-formatet är något annorlunda på Linux-noder.</span><span class="sxs-lookup"><span data-stu-id="959b0-262">On Linux nodes, hello format is slightly different.</span></span> <span data-ttu-id="959b0-263">Punkter (.), bindestreck (-) och nummertecken (#) är sammanslagna toounderscores i hello miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="959b0-263">Periods (.), hypens (-) and number signs (#) are flattened toounderscores in hello environment variable.</span></span> <span data-ttu-id="959b0-264">Exempel:</span><span class="sxs-lookup"><span data-stu-id="959b0-264">For example:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_APPLICATIONID_version
```

<span data-ttu-id="959b0-265">`APPLICATIONID`och `version` är värden som motsvarar toohello program och Paketversion du har angett för distribution.</span><span class="sxs-lookup"><span data-stu-id="959b0-265">`APPLICATIONID` and `version` are values that correspond toohello application and package version you've specified for deployment.</span></span> <span data-ttu-id="959b0-266">Om du har angett att version 2.7 av programmet till exempel *blandaren* ska installeras på Windows-noder din aktivitet kommandorader använder den här variabeln tooaccess miljö dess filer:</span><span class="sxs-lookup"><span data-stu-id="959b0-266">For example, if you specified that version 2.7 of application *blender* should be installed on Windows nodes, your task command lines would use this environment variable tooaccess its files:</span></span>

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

<span data-ttu-id="959b0-267">Ange hello-miljövariabeln på Linux-noder i det här formatet:</span><span class="sxs-lookup"><span data-stu-id="959b0-267">On Linux nodes, specify hello environment variable in this format:</span></span>

```
Linux:
AZ_BATCH_APP_PACKAGE_BLENDER_2_7
``` 

<span data-ttu-id="959b0-268">När du överför ett programpaket kan du ange en standard version toodeploy tooyour compute-noder.</span><span class="sxs-lookup"><span data-stu-id="959b0-268">When you upload an application package, you can specify a default version toodeploy tooyour compute nodes.</span></span> <span data-ttu-id="959b0-269">Om du har angett en standardversion för ett program, kan du utelämna hello versionssuffix när du refererar till hello program.</span><span class="sxs-lookup"><span data-stu-id="959b0-269">If you have specified a default version for an application, you can omit hello version suffix when you reference hello application.</span></span> <span data-ttu-id="959b0-270">Du kan ange hello standardversionen för programmet i hello Azure-portalen på hello program bladet som visas i [ladda upp och hantera program](#upload-and-manage-applications).</span><span class="sxs-lookup"><span data-stu-id="959b0-270">You can specify hello default application version in hello Azure portal, on hello Applications blade, as shown in [Upload and manage applications](#upload-and-manage-applications).</span></span>

<span data-ttu-id="959b0-271">Till exempel om du anger ”2.7” som hello standardversionen för programmet *blandaren*, och dina aktiviteter referera hello följande miljövariabeln och sedan Windows-noder som ska köra version 2.7:</span><span class="sxs-lookup"><span data-stu-id="959b0-271">For example, if you set "2.7" as hello default version for application *blender*, and your tasks reference hello following environment variable, then your Windows nodes will execute version 2.7:</span></span>

`AZ_BATCH_APP_PACKAGE_BLENDER`

<span data-ttu-id="959b0-272">hello följande kodavsnitt visar ett exempel uppgiften-kommandoraden som startar hello standardversionen av hello *blandaren* program:</span><span class="sxs-lookup"><span data-stu-id="959b0-272">hello following code snippet shows an example task command line that launches hello default version of hello *blender* application:</span></span>

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> <span data-ttu-id="959b0-273">Se [miljöinställningar för uppgifter](batch-api-basics.md#environment-settings-for-tasks) i hello [Batch funktionsöversikt](batch-api-basics.md) mer information om beräkning nod miljöinställningar.</span><span class="sxs-lookup"><span data-stu-id="959b0-273">See [Environment settings for tasks](batch-api-basics.md#environment-settings-for-tasks) in hello [Batch feature overview](batch-api-basics.md) for more information about compute node environment settings.</span></span>
> 
> 

## <a name="update-a-pools-application-packages"></a><span data-ttu-id="959b0-274">Uppdatera programpaket för en pool</span><span class="sxs-lookup"><span data-stu-id="959b0-274">Update a pool's application packages</span></span>
<span data-ttu-id="959b0-275">Om en befintlig pool har redan konfigurerats med ett programpaket, kan du ange ett nytt paket för hello poolen.</span><span class="sxs-lookup"><span data-stu-id="959b0-275">If an existing pool has already been configured with an application package, you can specify a new package for hello pool.</span></span> <span data-ttu-id="959b0-276">Om du anger en ny paketet referens för en pool gäller hello följande:</span><span class="sxs-lookup"><span data-stu-id="959b0-276">If you specify a new package reference for a pool, hello following apply:</span></span>

* <span data-ttu-id="959b0-277">hello Batch-tjänsten installeras hello nyligen angivet paket på alla nya noder som hello pool för att ansluta till och alla befintliga nod som startats om eller avbildade.</span><span class="sxs-lookup"><span data-stu-id="959b0-277">hello Batch service installs hello newly specified package on all new nodes that join hello pool and on any existing node that is rebooted or reimaged.</span></span>
* <span data-ttu-id="959b0-278">Compute-noder som redan finns i hello pool när du uppdaterar hello paketet refererar till inte installerar automatiskt hello nytt programpaket.</span><span class="sxs-lookup"><span data-stu-id="959b0-278">Compute nodes that are already in hello pool when you update hello package references do not automatically install hello new application package.</span></span> <span data-ttu-id="959b0-279">Dessa compute-noder måste startas om eller avbildade tooreceive hello nytt paket.</span><span class="sxs-lookup"><span data-stu-id="959b0-279">These compute nodes must be rebooted or reimaged tooreceive hello new package.</span></span>
* <span data-ttu-id="959b0-280">När ett nytt paket distribueras avspeglar hello skapade miljövariabler hello nya program paketet refererar till.</span><span class="sxs-lookup"><span data-stu-id="959b0-280">When a new package is deployed, hello created environment variables reflect hello new application package references.</span></span>

<span data-ttu-id="959b0-281">I det här exemplet hello befintlig pool har version 2.7 av hello *blandaren* program som har konfigurerats som en av dess [CloudPool][net_cloudpool].[ ApplicationPackageReferences][net_cloudpool_pkgref].</span><span class="sxs-lookup"><span data-stu-id="959b0-281">In this example, hello existing pool has version 2.7 of hello *blender* application configured as one of its [CloudPool][net_cloudpool].[ApplicationPackageReferences][net_cloudpool_pkgref].</span></span> <span data-ttu-id="959b0-282">tooupdate hello poolen noder med version 2.76b, ange ett nytt [ApplicationPackageReference] [ net_pkgref] med hello ny version och commit hello ändrar.</span><span class="sxs-lookup"><span data-stu-id="959b0-282">tooupdate hello pool's nodes with version 2.76b, specify a new [ApplicationPackageReference][net_pkgref] with hello new version, and commit hello change.</span></span>

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

<span data-ttu-id="959b0-283">Nu när hello nya versionen har konfigurerats, hello Batch-tjänsten installerar version 2.76b tooany *nya* nod som ansluter till hello poolen.</span><span class="sxs-lookup"><span data-stu-id="959b0-283">Now that hello new version has been configured, hello Batch service installs version 2.76b tooany *new* node that joins hello pool.</span></span> <span data-ttu-id="959b0-284">tooinstall 2.76b på hello-noder som är *redan* i hello poolen, starta om eller återavbilda dem.</span><span class="sxs-lookup"><span data-stu-id="959b0-284">tooinstall 2.76b on hello nodes that are *already* in hello pool, reboot or reimage them.</span></span> <span data-ttu-id="959b0-285">Observera att behålla omstartad noder hello filer från tidigare distributioner för paketet.</span><span class="sxs-lookup"><span data-stu-id="959b0-285">Note that rebooted nodes retain hello files from previous package deployments.</span></span>

## <a name="list-hello-applications-in-a-batch-account"></a><span data-ttu-id="959b0-286">Lista hello program i ett Batch-konto</span><span class="sxs-lookup"><span data-stu-id="959b0-286">List hello applications in a Batch account</span></span>
<span data-ttu-id="959b0-287">Du kan ange hello program och deras paket i ett Batch-konto med hjälp av hello [ApplicationOperations][net_appops].[ ListApplicationSummaries] [ net_appops_listappsummaries] metod.</span><span class="sxs-lookup"><span data-stu-id="959b0-287">You can list hello applications and their packages in a Batch account by using hello [ApplicationOperations][net_appops].[ListApplicationSummaries][net_appops_listappsummaries] method.</span></span>

```csharp
// List hello applications and their application packages in hello Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="wrap-up"></a><span data-ttu-id="959b0-288">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="959b0-288">Wrap up</span></span>
<span data-ttu-id="959b0-289">Med programpaket, kan du hjälpa kunderna Välj hello program för sitt arbete och ange hello exakt vilken version toouse vid bearbetning av jobb med aktiverad Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="959b0-289">With application packages, you can help your customers select hello applications for their jobs and specify hello exact version toouse when processing jobs with your Batch-enabled service.</span></span> <span data-ttu-id="959b0-290">Du kan också ange hello möjligheten för kunder-tooupload och spåra sina egna program i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="959b0-290">You might also provide hello ability for your customers tooupload and track their own applications in your service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="959b0-291">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="959b0-291">Next steps</span></span>
* <span data-ttu-id="959b0-292">Hej [Batch REST API] [ api_rest] ger support toowork programpaket.</span><span class="sxs-lookup"><span data-stu-id="959b0-292">hello [Batch REST API][api_rest] also provides support toowork with application packages.</span></span> <span data-ttu-id="959b0-293">Se exempelvis hello [applicationPackageReferences] [ rest_add_pool_with_packages] element i [Lägg till ett poolen tooan] [ rest_add_pool] information om hur toospecify paket tooinstall med hjälp av hello REST API.</span><span class="sxs-lookup"><span data-stu-id="959b0-293">For example, see hello [applicationPackageReferences][rest_add_pool_with_packages] element in [Add a pool tooan account][rest_add_pool] for information about how toospecify packages tooinstall by using hello REST API.</span></span> <span data-ttu-id="959b0-294">Se [program] [ rest_applications] mer information om hur tooobtain programinformation med hjälp av hello Batch REST API.</span><span class="sxs-lookup"><span data-stu-id="959b0-294">See [Applications][rest_applications] for details about how tooobtain application information by using hello Batch REST API.</span></span>
* <span data-ttu-id="959b0-295">Lär dig hur tooprogrammatically [hantera Azure Batch-konton och kvoter med Batch Management .NET](batch-management-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="959b0-295">Learn how tooprogrammatically [manage Azure Batch accounts and quotas with Batch Management .NET](batch-management-dotnet.md).</span></span> <span data-ttu-id="959b0-296">Hej [Batch Management .NET][api_net_mgmt] biblioteket kan aktivera konto skapas eller tas bort för Batch-programmet eller tjänsten.</span><span class="sxs-lookup"><span data-stu-id="959b0-296">hello [Batch Management .NET][api_net_mgmt] library can enable account creation and deletion features for your Batch application or service.</span></span>

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/en-us/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Översiktsdiagram för programmets paket"
[2]: ./media/batch-application-packages/app_pkg_02.png "Program-panelen i Azure-portalen"
[3]: ./media/batch-application-packages/app_pkg_03.png "Program-bladet i Azure-portalen"
[4]: ./media/batch-application-packages/app_pkg_04.png "Programmet informationsbladet i Azure-portalen"
[5]: ./media/batch-application-packages/app_pkg_05.png "Nya program bladet i Azure-portalen"
[7]: ./media/batch-application-packages/app_pkg_07.png "Uppdatera eller ta bort paket listrutan i Azure-portalen"
[8]: ./media/batch-application-packages/app_pkg_08.png "Nya bladet med paketet i Azure-portalen"
[9]: ./media/batch-application-packages/app_pkg_09.png "Ingen varning har länkade Storage-konto"
[10]: ./media/batch-application-packages/app_pkg_10.png "Välj lagring konto-bladet i Azure-portalen"
[11]: ./media/batch-application-packages/app_pkg_11.png "Uppdatera paketet bladet i Azure-portalen"
[12]: ./media/batch-application-packages/app_pkg_12.png "Ta bort paketet bekräftelsedialogruta i Azure-portalen"
