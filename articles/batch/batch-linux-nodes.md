---
title: "aaaRun Linux på den virtuella datorn datornoder - Azure Batch | Microsoft Docs"
description: "Lär dig hur tooprocess din parallell compute arbetsbelastningar på pooler för Linux-datorer i Azure Batch."
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3daabd5c577baaafd0544f9f7913cb7b116d74d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a><span data-ttu-id="32080-103">Etablera Linux compute-noder i Batch-pooler</span><span class="sxs-lookup"><span data-stu-id="32080-103">Provision Linux compute nodes in Batch pools</span></span>

<span data-ttu-id="32080-104">Du kan använda Azure Batch toorun parallella beräkning av arbetsbelastningar på Linux- och Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="32080-104">You can use Azure Batch toorun parallel compute workloads on both Linux and Windows virtual machines.</span></span> <span data-ttu-id="32080-105">Den här artikeln beskrivs hur toocreate pooler för Linux compute-noder i hello Batch-tjänsten med hjälp av både hello [Batch Python] [ py_batch_package] och [Batch .NET] [ api_net] klientbibliotek.</span><span class="sxs-lookup"><span data-stu-id="32080-105">This article details how toocreate pools of Linux compute nodes in hello Batch service by using both hello [Batch Python][py_batch_package] and [Batch .NET][api_net] client libraries.</span></span>

> [!NOTE]
> <span data-ttu-id="32080-106">Programpaket kan användas för alla Batch-pooler som skapats efter 5 juli 2017.</span><span class="sxs-lookup"><span data-stu-id="32080-106">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="32080-107">De stöds i Batch-adresspooler skapa mellan 10 mars 2016 och 5 juli 2017 om hello poolen har skapats med en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="32080-107">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if hello pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="32080-108">Batch-pooler som skapats tidigare too10 mars 2016 stöder inte programpaket.</span><span class="sxs-lookup"><span data-stu-id="32080-108">Batch pools created prior too10 March 2016 do not support application packages.</span></span> <span data-ttu-id="32080-109">Mer information om hur du använder programmet paket toodeploy program tooyour Batch noderna, finns [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="32080-109">For more information about using application packages toodeploy your applications tooyour Batch nodes, see [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md).</span></span>
>
>

## <a name="virtual-machine-configuration"></a><span data-ttu-id="32080-110">Konfiguration av virtuell dator</span><span class="sxs-lookup"><span data-stu-id="32080-110">Virtual machine configuration</span></span>
<span data-ttu-id="32080-111">När du skapar en pool med compute-noder i Batch har du två alternativ från vilka tooselect hello nod storleken och operativsystemet: moln-tjänsternas konfiguration och konfiguration av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="32080-111">When you create a pool of compute nodes in Batch, you have two options from which tooselect hello node size and operating system: Cloud Services Configuration and Virtual Machine Configuration.</span></span>

<span data-ttu-id="32080-112">En **Cloud Services-konfiguration** tillhandahåller *endast*Windows-beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="32080-112">**Cloud Services Configuration** provides Windows compute nodes *only*.</span></span> <span data-ttu-id="32080-113">Tillgängliga compute-nod storlekar listas i [storlekar för molntjänster](../cloud-services/cloud-services-sizes-specs.md), och tillgängliga operativsystem visas i hello [Azure gäst-OS-versioner och SDK-kompatibilitetsmatris](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="32080-113">Available compute node sizes are listed in [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md), and available operating systems are listed in hello [Azure Guest OS releases and SDK compatibility matrix](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span> <span data-ttu-id="32080-114">När du skapar en pool som innehåller Azure Cloud Services noder kan du ange hello nodstorlek och hello OS-familj som beskrivs i hello tidigare nämnts artiklar.</span><span class="sxs-lookup"><span data-stu-id="32080-114">When you create a pool that contains Azure Cloud Services nodes, you specify hello node size and hello OS family, which are described in hello previously mentioned articles.</span></span> <span data-ttu-id="32080-115">Molntjänster används oftast för pooler för Windows-beräkningsnoder.</span><span class="sxs-lookup"><span data-stu-id="32080-115">For pools of Windows compute nodes, Cloud Services is most commonly used.</span></span>

<span data-ttu-id="32080-116">**Konfiguration av virtuell dator** innehåller både Linux och Windows-avbildningar för compute-noder.</span><span class="sxs-lookup"><span data-stu-id="32080-116">**Virtual Machine Configuration** provides both Linux and Windows images for compute nodes.</span></span> <span data-ttu-id="32080-117">Tillgängliga compute-nod storlekar listas i [storlekar för virtuella datorer i Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) och [storlekar för virtuella datorer i Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span><span class="sxs-lookup"><span data-stu-id="32080-117">Available compute node sizes are listed in [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) and [Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span></span> <span data-ttu-id="32080-118">När du skapar en pool som innehåller konfiguration av virtuell dator noder, måste du ange hello storleken på hello noder och hello virtuella bildreferens hello Batch nod agent SKU toobe har installerats på hello noder.</span><span class="sxs-lookup"><span data-stu-id="32080-118">When you create a pool that contains Virtual Machine Configuration nodes, you must specify hello size of hello nodes, hello virtual machine image reference, and hello Batch node agent SKU toobe installed on hello nodes.</span></span>

### <a name="virtual-machine-image-reference"></a><span data-ttu-id="32080-119">Bildreferens för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="32080-119">Virtual machine image reference</span></span>
<span data-ttu-id="32080-120">Hej Batch-tjänsten används [skalningsuppsättningar i virtuella](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tooprovide Linux compute-noder.</span><span class="sxs-lookup"><span data-stu-id="32080-120">hello Batch service uses [virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tooprovide Linux compute nodes.</span></span> <span data-ttu-id="32080-121">Du kan ange en bild från hello [Azure Marketplace][vm_marketplace], eller ange en anpassad avbildning som du har förberett.</span><span class="sxs-lookup"><span data-stu-id="32080-121">You can specify an image from hello [Azure Marketplace][vm_marketplace], or provide a custom image that you have prepared.</span></span> <span data-ttu-id="32080-122">Mer information om anpassade avbildningar finns i [Utveckla storskaliga parallella beräkningslösningar med Batch](batch-api-basics.md#pool).</span><span class="sxs-lookup"><span data-stu-id="32080-122">For more details about custom images, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#pool).</span></span>

<span data-ttu-id="32080-123">När du konfigurerar en virtuell dator en bildreferens kan ange du hello egenskaper för hello avbildning av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="32080-123">When you configure a virtual machine image reference, you specify hello properties of hello virtual machine image.</span></span> <span data-ttu-id="32080-124">När du skapar en virtuell dator bildreferens krävs hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="32080-124">hello following properties are required when you create a virtual machine image reference:</span></span>

| <span data-ttu-id="32080-125">**Referens för Bildegenskaper**</span><span class="sxs-lookup"><span data-stu-id="32080-125">**Image reference properties**</span></span> | <span data-ttu-id="32080-126">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="32080-126">**Example**</span></span> |
| --- | --- |
| <span data-ttu-id="32080-127">Utgivare</span><span class="sxs-lookup"><span data-stu-id="32080-127">Publisher</span></span> |<span data-ttu-id="32080-128">Canonical</span><span class="sxs-lookup"><span data-stu-id="32080-128">Canonical</span></span> |
| <span data-ttu-id="32080-129">Erbjudande</span><span class="sxs-lookup"><span data-stu-id="32080-129">Offer</span></span> |<span data-ttu-id="32080-130">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="32080-130">UbuntuServer</span></span> |
| <span data-ttu-id="32080-131">SKU</span><span class="sxs-lookup"><span data-stu-id="32080-131">SKU</span></span> |<span data-ttu-id="32080-132">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="32080-132">14.04.4-LTS</span></span> |
| <span data-ttu-id="32080-133">Version</span><span class="sxs-lookup"><span data-stu-id="32080-133">Version</span></span> |<span data-ttu-id="32080-134">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-134">latest</span></span> |

> [!TIP]
> <span data-ttu-id="32080-135">Du kan lära dig mer om de här egenskaperna och hur toolist Marketplace bilder i [navigera och välj Linux virtuella avbildningar i Azure med CLI eller PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="32080-135">You can learn more about these properties and how toolist Marketplace images in [Navigate and select Linux virtual machine images in Azure with CLI or PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="32080-136">Observera att inte alla Marketplace-bilder är kompatibelt med Batch.</span><span class="sxs-lookup"><span data-stu-id="32080-136">Note that not all Marketplace images are currently compatible with Batch.</span></span> <span data-ttu-id="32080-137">Mer information finns i [nod agent SKU](#node-agent-sku).</span><span class="sxs-lookup"><span data-stu-id="32080-137">For more information, see [Node agent SKU](#node-agent-sku).</span></span>
>
>

### <a name="node-agent-sku"></a><span data-ttu-id="32080-138">Noden agent SKU</span><span class="sxs-lookup"><span data-stu-id="32080-138">Node agent SKU</span></span>
<span data-ttu-id="32080-139">hello Batch nod agent är ett program som körs på varje nod i hello pool och tillhandahåller hello och kommandokontroll gränssnitt mellan hello nod och hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="32080-139">hello Batch node agent is a program that runs on each node in hello pool and provides hello command-and-control interface between hello node and hello Batch service.</span></span> <span data-ttu-id="32080-140">Det finns olika implementeringar av hello nod agent kallas SKU: er, för olika operativsystem.</span><span class="sxs-lookup"><span data-stu-id="32080-140">There are different implementations of hello node agent, known as SKUs, for different operating systems.</span></span> <span data-ttu-id="32080-141">I princip när du skapar en konfiguration av virtuell dator du första ange hello bildreferens för virtuell dator och du sedan ange hello nod agent tooinstall på hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="32080-141">Essentially, when you create a Virtual Machine Configuration, you first specify hello virtual machine image reference, and then you specify hello node agent tooinstall on hello image.</span></span> <span data-ttu-id="32080-142">Normalt varje nod agent SKU är kompatibel med flera virtuella avbildningar.</span><span class="sxs-lookup"><span data-stu-id="32080-142">Typically, each node agent SKU is compatible with multiple virtual machine images.</span></span> <span data-ttu-id="32080-143">Här följer några exempel på noden agent SKU: er:</span><span class="sxs-lookup"><span data-stu-id="32080-143">Here are a few examples of node agent SKUs:</span></span>

* <span data-ttu-id="32080-144">batch.node.Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="32080-144">batch.node.ubuntu 14.04</span></span>
* <span data-ttu-id="32080-145">batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="32080-145">batch.node.centos 7</span></span>
* <span data-ttu-id="32080-146">batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="32080-146">batch.node.windows amd64</span></span>

> [!IMPORTANT]
> <span data-ttu-id="32080-147">Inte alla virtuella avbildningar som är tillgängliga i hello Marketplace är kompatibla med hello tillgängliga Batch nod agenter.</span><span class="sxs-lookup"><span data-stu-id="32080-147">Not all virtual machine images that are available in hello Marketplace are compatible with hello currently available Batch node agents.</span></span> <span data-ttu-id="32080-148">Använd hello Batch SDK toolist hello tillgängliga noden agent SKU: er och hello avbildningar av virtuella datorer som de är kompatibla.</span><span class="sxs-lookup"><span data-stu-id="32080-148">Use hello Batch SDKs toolist hello available node agent SKUs and hello virtual machine images with which they are compatible.</span></span> <span data-ttu-id="32080-149">Se hello [lista virtuella avbildningar](#list-of-virtual-machine-images) senare i den här artikeln för mer information och exempel på hur tooretrieve en lista över giltiga bilder vid körning.</span><span class="sxs-lookup"><span data-stu-id="32080-149">See hello [List of Virtual Machine images](#list-of-virtual-machine-images) later in this article for more information and examples of how tooretrieve a list of valid images at runtime.</span></span>
>
>

## <a name="create-a-linux-pool-batch-python"></a><span data-ttu-id="32080-150">Skapa en pool med Linux: Batch Python</span><span class="sxs-lookup"><span data-stu-id="32080-150">Create a Linux pool: Batch Python</span></span>
<span data-ttu-id="32080-151">hello följande kodavsnitt visar ett exempel på hur toouse hello [Microsoft Azure Batch-klientbibliotek för Python] [ py_batch_package] toocreate en pool med Ubuntu Server compute-noder.</span><span class="sxs-lookup"><span data-stu-id="32080-151">hello following code snippet shows an example of how toouse hello [Microsoft Azure Batch Client Library for Python][py_batch_package] toocreate a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="32080-152">Referera till dokumentationen för hello Batch Python-modulen finns på [azure.batch paketet] [ py_batch_docs] på Läs hello dokumenten.</span><span class="sxs-lookup"><span data-stu-id="32080-152">Reference documentation for hello Batch Python module can be found at [azure.batch package][py_batch_docs] on Read hello Docs.</span></span>

<span data-ttu-id="32080-153">Den här kodutdrag skapar en [ImageReference] [ py_imagereference] explicit och anger var och en av dess egenskaper (utgivare, erbjudande, SKU och version).</span><span class="sxs-lookup"><span data-stu-id="32080-153">This snippet creates an [ImageReference][py_imagereference] explicitly and specifies each of its properties (publisher, offer, SKU, version).</span></span> <span data-ttu-id="32080-154">I produktionskod, men vi rekommenderar att du använder hello [list_node_agent_skus] [ py_list_skus] metoden toodetermine och välj från hello tillgängliga avbildningen och noden agent SKU kombinationer vid körning.</span><span class="sxs-lookup"><span data-stu-id="32080-154">In production code, however, we recommend that you use hello [list_node_agent_skus][py_list_skus] method toodetermine and select from hello available image and node agent SKU combinations at runtime.</span></span>

```python
# Import hello required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize hello Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create hello unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure hello start task for hello pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies hello Marketplace
# virtual machine image tooinstall on hello nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create hello VirtualMachineConfiguration, specifying
# hello VM image reference and hello Batch node agent to
# be installed on hello node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign hello virtual machine configuration toohello pool
new_pool.virtual_machine_configuration = vmc

# Create pool in hello Batch service
client.pool.add(new_pool)
```

<span data-ttu-id="32080-155">Som tidigare nämnts, rekommenderar vi att i stället för att skapa hello [ImageReference] [ py_imagereference] uttryckligen, du kan använda hello [list_node_agent_skus] [ py_list_skus] metoden toodynamically väljer från hello stöds för närvarande nod agent/Marketplace avbildningen kombinationer.</span><span class="sxs-lookup"><span data-stu-id="32080-155">As mentioned previously, we recommend that instead of creating hello [ImageReference][py_imagereference] explicitly, you use hello [list_node_agent_skus][py_list_skus] method toodynamically select from hello currently supported node agent/Marketplace image combinations.</span></span> <span data-ttu-id="32080-156">Hej följande Python fragment visas hur toouse den här metoden.</span><span class="sxs-lookup"><span data-stu-id="32080-156">hello following Python snippet shows how toouse this method.</span></span>

```python
# Get hello list of node agents from hello Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain hello desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick hello first image reference from hello list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create hello VirtualMachineConfiguration, specifying hello VM image
# reference and hello Batch node agent toobe installed on hello node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a><span data-ttu-id="32080-157">Skapa en pool med Linux: Batch .NET</span><span class="sxs-lookup"><span data-stu-id="32080-157">Create a Linux pool: Batch .NET</span></span>
<span data-ttu-id="32080-158">hello följande kodavsnitt visar ett exempel på hur toouse hello [Batch .NET] [ nuget_batch_net] klienten biblioteket toocreate en pool med Ubuntu Server compute-noder.</span><span class="sxs-lookup"><span data-stu-id="32080-158">hello following code snippet shows an example of how toouse hello [Batch .NET][nuget_batch_net] client library toocreate a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="32080-159">Du kan hitta hello [Batch .NET referensdokumentationen] [ api_net] på MSDN.</span><span class="sxs-lookup"><span data-stu-id="32080-159">You can find hello [Batch .NET reference documentation][api_net] on MSDN.</span></span>

<span data-ttu-id="32080-160">hello följande kodavsnitt använder hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metoden tooselect hello listan över för närvarande Marketplace-avbildning och noden agent SKU kombinationer som stöds.</span><span class="sxs-lookup"><span data-stu-id="32080-160">hello following code snippet uses hello [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method tooselect from hello list of currently supported Marketplace image and node agent SKU combinations.</span></span> <span data-ttu-id="32080-161">Den här tekniken är önskvärt eftersom hello lista över kombinationer som stöds kan ändras från tid tootime.</span><span class="sxs-lookup"><span data-stu-id="32080-161">This technique is desirable because hello list of supported combinations may change from time tootime.</span></span> <span data-ttu-id="32080-162">Oftast läggs kombinationer som stöds.</span><span class="sxs-lookup"><span data-stu-id="32080-162">Most commonly, supported combinations are added.</span></span>

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us tooselect from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of hello VM image
// that we wish toouse.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain hello first node agent SKU in hello collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create hello VirtualMachineConfiguration for use when actually
// creating hello pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

// Create hello unbound pool object using hello VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit hello pool toohello Batch service
await pool.CommitAsync();
```

<span data-ttu-id="32080-163">Även om hello utdraget använder hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metoden toodynamically lista och välj från stöds avbildningen och noden agent SKU kombinationer (rekommenderas), du kan också konfigurera en [ImageReference] [ net_imagereference] explicit:</span><span class="sxs-lookup"><span data-stu-id="32080-163">Although hello previous snippet uses hello [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method toodynamically list and select from supported image and node agent SKU combinations (recommended), you can also configure an [ImageReference][net_imagereference] explicitly:</span></span>

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a><span data-ttu-id="32080-164">Lista över avbildningar av virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="32080-164">List of virtual machine images</span></span>
<span data-ttu-id="32080-165">hello visar följande tabell hello Marketplace avbildningar av virtuella datorer som är kompatibla med hello tillgängliga Batch nod agenter när den här artikeln senast uppdaterades.</span><span class="sxs-lookup"><span data-stu-id="32080-165">hello following table lists hello Marketplace virtual machine images that are compatible with hello available Batch node agents when this article was last updated.</span></span> <span data-ttu-id="32080-166">Det är viktigt toonote att den här listan inte är slutgiltig eftersom bilder och nod-agenter kan läggas till eller tas bort när som helst.</span><span class="sxs-lookup"><span data-stu-id="32080-166">It is important toonote that this list is not definitive because images and node agents may be added or removed at any time.</span></span> <span data-ttu-id="32080-167">Vi rekommenderar att dina Batch-program och tjänster alltid använder [list_node_agent_skus] [ py_list_skus] (Python) och [ListNodeAgentSkus] [ net_list_skus] Toodetermine (batch .NET) och välj bland hello tillgängliga SKU: er.</span><span class="sxs-lookup"><span data-stu-id="32080-167">We recommend that your Batch applications and services always use [list_node_agent_skus][py_list_skus] (Python) and [ListNodeAgentSkus][net_list_skus] (Batch .NET) toodetermine and select from hello currently available SKUs.</span></span>

> [!WARNING]
> <span data-ttu-id="32080-168">hello efter listan kan ändras när som helst.</span><span class="sxs-lookup"><span data-stu-id="32080-168">hello following list may change at any time.</span></span> <span data-ttu-id="32080-169">Använd alltid hello **lista nod agent SKU** metoder som är tillgängliga i hello Batch-API: er toolist hello kompatibla virtuella datorn och noden agent SKU: er när du kör Batch-jobb.</span><span class="sxs-lookup"><span data-stu-id="32080-169">Always use hello **list node agent SKU** methods available in hello Batch APIs toolist hello compatible virtual machine and node agent SKUs when you run your Batch jobs.</span></span>
>
>

| <span data-ttu-id="32080-170">**Publisher**</span><span class="sxs-lookup"><span data-stu-id="32080-170">**Publisher**</span></span> | <span data-ttu-id="32080-171">**Erbjudande**</span><span class="sxs-lookup"><span data-stu-id="32080-171">**Offer**</span></span> | <span data-ttu-id="32080-172">**Bild SKU**</span><span class="sxs-lookup"><span data-stu-id="32080-172">**Image SKU**</span></span> | <span data-ttu-id="32080-173">**Version**</span><span class="sxs-lookup"><span data-stu-id="32080-173">**Version**</span></span> | <span data-ttu-id="32080-174">**Noden agent SKU-ID**</span><span class="sxs-lookup"><span data-stu-id="32080-174">**Node agent SKU ID**</span></span> |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| <span data-ttu-id="32080-175">Canonical</span><span class="sxs-lookup"><span data-stu-id="32080-175">Canonical</span></span> | <span data-ttu-id="32080-176">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="32080-176">UbuntuServer</span></span> | <span data-ttu-id="32080-177">14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="32080-177">14.04.5-LTS</span></span> | <span data-ttu-id="32080-178">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-178">latest</span></span> | <span data-ttu-id="32080-179">batch.node.Ubuntu 14.04</span><span class="sxs-lookup"><span data-stu-id="32080-179">batch.node.ubuntu 14.04</span></span> |
| <span data-ttu-id="32080-180">Canonical</span><span class="sxs-lookup"><span data-stu-id="32080-180">Canonical</span></span> | <span data-ttu-id="32080-181">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="32080-181">UbuntuServer</span></span> | <span data-ttu-id="32080-182">16.04.0-LTS</span><span class="sxs-lookup"><span data-stu-id="32080-182">16.04.0-LTS</span></span> | <span data-ttu-id="32080-183">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-183">latest</span></span> | <span data-ttu-id="32080-184">batch.node.Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="32080-184">batch.node.ubuntu 16.04</span></span> |
| <span data-ttu-id="32080-185">credativ</span><span class="sxs-lookup"><span data-stu-id="32080-185">Credativ</span></span> | <span data-ttu-id="32080-186">Debian</span><span class="sxs-lookup"><span data-stu-id="32080-186">Debian</span></span> | <span data-ttu-id="32080-187">8</span><span class="sxs-lookup"><span data-stu-id="32080-187">8</span></span> | <span data-ttu-id="32080-188">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-188">latest</span></span> | <span data-ttu-id="32080-189">batch.node.debian 8</span><span class="sxs-lookup"><span data-stu-id="32080-189">batch.node.debian 8</span></span> |
| <span data-ttu-id="32080-190">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="32080-190">OpenLogic</span></span> | <span data-ttu-id="32080-191">CentOS</span><span class="sxs-lookup"><span data-stu-id="32080-191">CentOS</span></span> | <span data-ttu-id="32080-192">7.0</span><span class="sxs-lookup"><span data-stu-id="32080-192">7.0</span></span> | <span data-ttu-id="32080-193">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-193">latest</span></span> | <span data-ttu-id="32080-194">batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="32080-194">batch.node.centos 7</span></span> |
| <span data-ttu-id="32080-195">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="32080-195">OpenLogic</span></span> | <span data-ttu-id="32080-196">CentOS</span><span class="sxs-lookup"><span data-stu-id="32080-196">CentOS</span></span> | <span data-ttu-id="32080-197">7.1</span><span class="sxs-lookup"><span data-stu-id="32080-197">7.1</span></span> | <span data-ttu-id="32080-198">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-198">latest</span></span> | <span data-ttu-id="32080-199">batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="32080-199">batch.node.centos 7</span></span> |
| <span data-ttu-id="32080-200">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="32080-200">OpenLogic</span></span> | <span data-ttu-id="32080-201">CentOS HPC</span><span class="sxs-lookup"><span data-stu-id="32080-201">CentOS-HPC</span></span> | <span data-ttu-id="32080-202">7.1</span><span class="sxs-lookup"><span data-stu-id="32080-202">7.1</span></span> | <span data-ttu-id="32080-203">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-203">latest</span></span> | <span data-ttu-id="32080-204">batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="32080-204">batch.node.centos 7</span></span> |
| <span data-ttu-id="32080-205">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="32080-205">OpenLogic</span></span> | <span data-ttu-id="32080-206">CentOS</span><span class="sxs-lookup"><span data-stu-id="32080-206">CentOS</span></span> | <span data-ttu-id="32080-207">7.2</span><span class="sxs-lookup"><span data-stu-id="32080-207">7.2</span></span> | <span data-ttu-id="32080-208">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-208">latest</span></span> | <span data-ttu-id="32080-209">batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="32080-209">batch.node.centos 7</span></span> |
| <span data-ttu-id="32080-210">Oracle</span><span class="sxs-lookup"><span data-stu-id="32080-210">Oracle</span></span> | <span data-ttu-id="32080-211">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="32080-211">Oracle-Linux</span></span> | <span data-ttu-id="32080-212">7.0</span><span class="sxs-lookup"><span data-stu-id="32080-212">7.0</span></span> | <span data-ttu-id="32080-213">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-213">latest</span></span> | <span data-ttu-id="32080-214">batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="32080-214">batch.node.centos 7</span></span> |
| <span data-ttu-id="32080-215">Oracle</span><span class="sxs-lookup"><span data-stu-id="32080-215">Oracle</span></span> | <span data-ttu-id="32080-216">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="32080-216">Oracle-Linux</span></span> | <span data-ttu-id="32080-217">7.2</span><span class="sxs-lookup"><span data-stu-id="32080-217">7.2</span></span> | <span data-ttu-id="32080-218">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-218">latest</span></span> | <span data-ttu-id="32080-219">batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="32080-219">batch.node.centos 7</span></span> |
| <span data-ttu-id="32080-220">SUSE</span><span class="sxs-lookup"><span data-stu-id="32080-220">SUSE</span></span> | <span data-ttu-id="32080-221">openSUSE</span><span class="sxs-lookup"><span data-stu-id="32080-221">openSUSE</span></span> | <span data-ttu-id="32080-222">13.2</span><span class="sxs-lookup"><span data-stu-id="32080-222">13.2</span></span> | <span data-ttu-id="32080-223">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-223">latest</span></span> | <span data-ttu-id="32080-224">batch.node.OpenSuSE 13.2</span><span class="sxs-lookup"><span data-stu-id="32080-224">batch.node.opensuse 13.2</span></span> |
| <span data-ttu-id="32080-225">SUSE</span><span class="sxs-lookup"><span data-stu-id="32080-225">SUSE</span></span> | <span data-ttu-id="32080-226">openSUSE Leap</span><span class="sxs-lookup"><span data-stu-id="32080-226">openSUSE-Leap</span></span> | <span data-ttu-id="32080-227">42.1</span><span class="sxs-lookup"><span data-stu-id="32080-227">42.1</span></span> | <span data-ttu-id="32080-228">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-228">latest</span></span> | <span data-ttu-id="32080-229">batch.node.OpenSuSE 42.1</span><span class="sxs-lookup"><span data-stu-id="32080-229">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="32080-230">SUSE</span><span class="sxs-lookup"><span data-stu-id="32080-230">SUSE</span></span> | <span data-ttu-id="32080-231">SLES</span><span class="sxs-lookup"><span data-stu-id="32080-231">SLES</span></span> | <span data-ttu-id="32080-232">12-SP1</span><span class="sxs-lookup"><span data-stu-id="32080-232">12-SP1</span></span> | <span data-ttu-id="32080-233">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-233">latest</span></span> | <span data-ttu-id="32080-234">batch.node.OpenSuSE 42.1</span><span class="sxs-lookup"><span data-stu-id="32080-234">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="32080-235">SUSE</span><span class="sxs-lookup"><span data-stu-id="32080-235">SUSE</span></span> | <span data-ttu-id="32080-236">SLES HPC</span><span class="sxs-lookup"><span data-stu-id="32080-236">SLES-HPC</span></span> | <span data-ttu-id="32080-237">12-SP1</span><span class="sxs-lookup"><span data-stu-id="32080-237">12-SP1</span></span> | <span data-ttu-id="32080-238">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-238">latest</span></span> | <span data-ttu-id="32080-239">batch.node.OpenSuSE 42.1</span><span class="sxs-lookup"><span data-stu-id="32080-239">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="32080-240">Microsoft-annonser</span><span class="sxs-lookup"><span data-stu-id="32080-240">microsoft-ads</span></span> | <span data-ttu-id="32080-241">Linux-data-vetenskap-vm</span><span class="sxs-lookup"><span data-stu-id="32080-241">linux-data-science-vm</span></span> | <span data-ttu-id="32080-242">linuxdsvm</span><span class="sxs-lookup"><span data-stu-id="32080-242">linuxdsvm</span></span> | <span data-ttu-id="32080-243">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-243">latest</span></span> | <span data-ttu-id="32080-244">batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="32080-244">batch.node.centos 7</span></span> |
| <span data-ttu-id="32080-245">Microsoft-annonser</span><span class="sxs-lookup"><span data-stu-id="32080-245">microsoft-ads</span></span> | <span data-ttu-id="32080-246">standard-data-vetenskap-vm</span><span class="sxs-lookup"><span data-stu-id="32080-246">standard-data-science-vm</span></span> | <span data-ttu-id="32080-247">standard-data-vetenskap-vm</span><span class="sxs-lookup"><span data-stu-id="32080-247">standard-data-science-vm</span></span> | <span data-ttu-id="32080-248">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-248">latest</span></span> | <span data-ttu-id="32080-249">batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="32080-249">batch.node.windows amd64</span></span> |
| <span data-ttu-id="32080-250">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-250">MicrosoftWindowsServer</span></span> | <span data-ttu-id="32080-251">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-251">WindowsServer</span></span> | <span data-ttu-id="32080-252">2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="32080-252">2008-R2-SP1</span></span> | <span data-ttu-id="32080-253">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-253">latest</span></span> | <span data-ttu-id="32080-254">batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="32080-254">batch.node.windows amd64</span></span> |
| <span data-ttu-id="32080-255">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-255">MicrosoftWindowsServer</span></span> | <span data-ttu-id="32080-256">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-256">WindowsServer</span></span> | <span data-ttu-id="32080-257">2012-Datacenter</span><span class="sxs-lookup"><span data-stu-id="32080-257">2012-Datacenter</span></span> | <span data-ttu-id="32080-258">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-258">latest</span></span> | <span data-ttu-id="32080-259">batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="32080-259">batch.node.windows amd64</span></span> |
| <span data-ttu-id="32080-260">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-260">MicrosoftWindowsServer</span></span> | <span data-ttu-id="32080-261">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-261">WindowsServer</span></span> | <span data-ttu-id="32080-262">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="32080-262">2012-R2-Datacenter</span></span> | <span data-ttu-id="32080-263">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-263">latest</span></span> | <span data-ttu-id="32080-264">batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="32080-264">batch.node.windows amd64</span></span> |
| <span data-ttu-id="32080-265">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-265">MicrosoftWindowsServer</span></span> | <span data-ttu-id="32080-266">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-266">WindowsServer</span></span> | <span data-ttu-id="32080-267">2016 Datacenter</span><span class="sxs-lookup"><span data-stu-id="32080-267">2016-Datacenter</span></span> | <span data-ttu-id="32080-268">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-268">latest</span></span> | <span data-ttu-id="32080-269">batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="32080-269">batch.node.windows amd64</span></span> |
| <span data-ttu-id="32080-270">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-270">MicrosoftWindowsServer</span></span> | <span data-ttu-id="32080-271">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="32080-271">WindowsServer</span></span> | <span data-ttu-id="32080-272">2016 Datacenter med behållare</span><span class="sxs-lookup"><span data-stu-id="32080-272">2016-Datacenter-with-Containers</span></span> | <span data-ttu-id="32080-273">senaste</span><span class="sxs-lookup"><span data-stu-id="32080-273">latest</span></span> | <span data-ttu-id="32080-274">batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="32080-274">batch.node.windows amd64</span></span> |

## <a name="connect-toolinux-nodes-using-ssh"></a><span data-ttu-id="32080-275">Ansluta tooLinux noder via SSH</span><span class="sxs-lookup"><span data-stu-id="32080-275">Connect tooLinux nodes using SSH</span></span>
<span data-ttu-id="32080-276">Under utveckling eller vid felsökning kan det vara nödvändigt toosign i toohello noder i din pool.</span><span class="sxs-lookup"><span data-stu-id="32080-276">During development or while troubleshooting, you may find it necessary toosign in toohello nodes in your pool.</span></span> <span data-ttu-id="32080-277">Till skillnad från Windows compute-noder kan du inte använda Remote Desktop Protocol (RDP) tooconnect tooLinux noder.</span><span class="sxs-lookup"><span data-stu-id="32080-277">Unlike Windows compute nodes, you cannot use Remote Desktop Protocol (RDP) tooconnect tooLinux nodes.</span></span> <span data-ttu-id="32080-278">Hello Batch-tjänsten kan i stället SSH-åtkomst på varje nod för fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="32080-278">Instead, hello Batch service enables SSH access on each node for remote connection.</span></span>

<span data-ttu-id="32080-279">hello skapar följande kodavsnitt i Python en användare på varje nod i en programpool, vilket krävs för anslutning.</span><span class="sxs-lookup"><span data-stu-id="32080-279">hello following Python code snippet creates a user on each node in a pool, which is required for remote connection.</span></span> <span data-ttu-id="32080-280">Hello secure shell (SSH) anslutningsinformationen för varje nod skriver sedan ut.</span><span class="sxs-lookup"><span data-stu-id="32080-280">It then prints hello secure shell (SSH) connection information for each node.</span></span>

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify hello ID of an existing pool containing Linux nodes
# currently in hello 'idle' state
pool_id = ''

# Specify hello username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create hello user that will be added tooeach node in hello pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get hello list of nodes in hello pool
nodes = batch_client.compute_node.list(pool_id)

# Add hello user tooeach node in hello pool and print
# hello connection information for hello node
for node in nodes:
    # Add hello user toohello node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for hello node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print hello connection info for hello node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

<span data-ttu-id="32080-281">Här är exempel på utdata hello föregående kod för en pool som innehåller fyra Linux noder:</span><span class="sxs-lookup"><span data-stu-id="32080-281">Here is sample output for hello previous code for a pool that contains four Linux nodes:</span></span>

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

<span data-ttu-id="32080-282">Du kan ange en offentlig SSH-nyckel när du skapar en användare på en nod i stället för ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="32080-282">Instead of a password, you can specify an SSH public key when you create a user on a node.</span></span> <span data-ttu-id="32080-283">Använd hello i hello Python SDK, **ssh_public_key** parameter på [ComputeNodeUser][py_computenodeuser].</span><span class="sxs-lookup"><span data-stu-id="32080-283">In hello Python SDK, use hello **ssh_public_key** parameter on [ComputeNodeUser][py_computenodeuser].</span></span> <span data-ttu-id="32080-284">Använd hello i .NET, [ComputeNodeUser][net_computenodeuser].[ SshPublicKey] [ net_ssh_key] egenskapen.</span><span class="sxs-lookup"><span data-stu-id="32080-284">In .NET, use hello [ComputeNodeUser][net_computenodeuser].[SshPublicKey][net_ssh_key] property.</span></span>

## <a name="pricing"></a><span data-ttu-id="32080-285">Prissättning</span><span class="sxs-lookup"><span data-stu-id="32080-285">Pricing</span></span>
<span data-ttu-id="32080-286">Azure Batch bygger på Azure-molntjänster och virtuella datorer i Azure-teknik.</span><span class="sxs-lookup"><span data-stu-id="32080-286">Azure Batch is built on Azure Cloud Services and Azure Virtual Machines technology.</span></span> <span data-ttu-id="32080-287">hello själva Batch-tjänsten erbjuds utan kostnad, vilket betyder att debiteras du bara för hello beräkningsresurser som Batch-lösningar använder.</span><span class="sxs-lookup"><span data-stu-id="32080-287">hello Batch service itself is offered at no cost, which means you are charged only for hello compute resources that your Batch solutions consume.</span></span> <span data-ttu-id="32080-288">När du väljer **Services Molnkonfigurationen**, debiteras du baserat på hello [molntjänster priser] [ cloud_services_pricing] struktur.</span><span class="sxs-lookup"><span data-stu-id="32080-288">When you choose **Cloud Services Configuration**, you are charged based on hello [Cloud Services pricing][cloud_services_pricing] structure.</span></span> <span data-ttu-id="32080-289">När du väljer **konfiguration av virtuell dator**, debiteras du baserat på hello [virtuella datorer priser] [ vm_pricing] struktur.</span><span class="sxs-lookup"><span data-stu-id="32080-289">When you choose **Virtual Machine Configuration**, you are charged based on hello [Virtual Machines pricing][vm_pricing] structure.</span></span> 

<span data-ttu-id="32080-290">Om du distribuerar program tooyour Batch-noder som använder [programpaket](batch-application-packages.md), även debiteras du för hello Azure Storage-resurser att använda din programpaket.</span><span class="sxs-lookup"><span data-stu-id="32080-290">If you deploy applications tooyour Batch nodes using [application packages](batch-application-packages.md), you are also charged for hello Azure Storage resources that your application packages consume.</span></span> <span data-ttu-id="32080-291">I allmänhet är hello Azure lagringskostnader minimal.</span><span class="sxs-lookup"><span data-stu-id="32080-291">In general, hello Azure Storage costs are minimal.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="32080-292">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32080-292">Next steps</span></span>
### <a name="batch-python-tutorial"></a><span data-ttu-id="32080-293">Batch Python-självstudier</span><span class="sxs-lookup"><span data-stu-id="32080-293">Batch Python tutorial</span></span>
<span data-ttu-id="32080-294">För en mer detaljerad vägledning om hur toowork med Batch med hjälp av Python, checka ut [Kom igång med hello Azure Batch Python klienten](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="32080-294">For a more in-depth tutorial about how toowork with Batch by using Python, check out [Get started with hello Azure Batch Python client](batch-python-tutorial.md).</span></span> <span data-ttu-id="32080-295">Dess tillhörande [kodexemplet] [ github_samples_pyclient] innehåller en hjälpfunktion `get_vm_config_for_distro`, som visar ett annat tekniken tooobtain konfiguration av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="32080-295">Its companion [code sample][github_samples_pyclient] includes a helper function, `get_vm_config_for_distro`, that shows another technique tooobtain a virtual machine configuration.</span></span>

### <a name="batch-python-code-samples"></a><span data-ttu-id="32080-296">Batch Python-kodexempel</span><span class="sxs-lookup"><span data-stu-id="32080-296">Batch Python code samples</span></span>
<span data-ttu-id="32080-297">Hej [Python kodexempel] [ github_samples_py] i hello [azure-batch-samples] [ github_samples] databasen på GitHub innehåller skript som visar hur tooperform vanliga Batch-åtgärder, till exempel pool, jobb och skapa aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="32080-297">hello [Python code samples][github_samples_py] in hello [azure-batch-samples][github_samples] repository on GitHub contain scripts that show you how tooperform common Batch operations, such as pool, job, and task creation.</span></span> <span data-ttu-id="32080-298">Hej [viktigt] [ github_py_readme] som medföljer hello Python-exempel innehåller information om hur tooinstall hello krävs paket.</span><span class="sxs-lookup"><span data-stu-id="32080-298">hello [README][github_py_readme] that accompanies hello Python samples has details about how tooinstall hello required packages.</span></span>

### <a name="batch-forum"></a><span data-ttu-id="32080-299">Batch-forum</span><span class="sxs-lookup"><span data-stu-id="32080-299">Batch forum</span></span>
<span data-ttu-id="32080-300">Hej [Azure Batch-Forum] [ forum] är utmärkt placera toodiscuss Batch och ställa frågor om hello-tjänsten på MSDN.</span><span class="sxs-lookup"><span data-stu-id="32080-300">hello [Azure Batch Forum][forum] on MSDN is a great place toodiscuss Batch and ask questions about hello service.</span></span> <span data-ttu-id="32080-301">Läs användbara ”fasta” anslår och publicera dina frågor när de uppstår när du skapar Batch-lösningar.</span><span class="sxs-lookup"><span data-stu-id="32080-301">Read helpful "pinned" posts, and post your questions as they arise while you build your Batch solutions.</span></span>

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
