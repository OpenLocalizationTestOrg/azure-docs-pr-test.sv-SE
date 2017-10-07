---
title: "aaaRun Azure Batch arbetsbelastningar på kostnadseffektiv låg prioritet för virtuella datorer (förhandsversion) | Microsoft Docs"
description: "Lär dig hur tooprovision låg prioritet VMs tooreduce hello kostnader för Azure Batch-arbetsbelastningar."
services: batch
author: mscurrell
manager: timlt
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 07/21/2017
ms.author: markscu
ms.openlocfilehash: 91a5e89a819d05583e6b146932d925e217b4be4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-low-priority-vms-with-batch-preview"></a><span data-ttu-id="9f35d-103">Använd låg prioritet virtuella datorer med Batch (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="9f35d-103">Use low-priority VMs with Batch (Preview)</span></span>

<span data-ttu-id="9f35d-104">Azure Batch har låg priorty virtuella maskiner (VMs) tooreduce hello kostnaden för Batch-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="9f35d-104">Azure Batch offers low-priorty virtual machines (VMs) tooreduce hello cost of Batch workloads.</span></span> <span data-ttu-id="9f35d-105">VM med låg prioritet möjliggör nya typer av Batch-arbetsbelastningar genom att tillhandahålla mycket datorkraft som också är ekonomiskt.</span><span class="sxs-lookup"><span data-stu-id="9f35d-105">Low-priority VMs make new types of Batch workloads possible by providing a large amount of compute power that is also economical.</span></span>

<span data-ttu-id="9f35d-106">Låg prioritet VMs utnyttja överflödiga kapacitet i Azure.</span><span class="sxs-lookup"><span data-stu-id="9f35d-106">Low-priority VMs take advantage of surplus capacity in Azure.</span></span> <span data-ttu-id="9f35d-107">När du anger låg prioritet virtuella datorer i din pooler kan Azure Batch automatiskt använda den här överskott när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="9f35d-107">When you specify low-priority VMs in your pools, Azure Batch can automatically use this surplus when available.</span></span>

<span data-ttu-id="9f35d-108">hello förhållandet för med låg prioritet virtuella datorer är dessa virtuella datorer kan avbrytas om någon överskott kapacitet finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="9f35d-108">hello tradeoff for using low-priority VMs is that those VMs may be preempted when no surplus capacity is available in Azure.</span></span> <span data-ttu-id="9f35d-109">Därför är låg prioritet mest lämpliga för vissa typer av arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="9f35d-109">For this reason, low-priority VMs are most suitable for certain types of workloads.</span></span> <span data-ttu-id="9f35d-110">Använd VM med låg prioritet för batch- och asynkron bearbetning arbetsbelastningar där hello tid för slutförande av jobbet är flexibel och hello arbete distribueras till många virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9f35d-110">Use low-priority VMs for batch and asynchronous processing workloads where hello job completion time is flexible and hello work is distributed across many VMs.</span></span>

<span data-ttu-id="9f35d-111">Låg prioritet är betydligt mindre dyrbart än dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9f35d-111">Low-priority VMs are significantly less expensive than dedicated VMs.</span></span> <span data-ttu-id="9f35d-112">Information om priser, se [Batch priser](https://azure.microsoft.com/pricing/details/batch/).</span><span class="sxs-lookup"><span data-stu-id="9f35d-112">For pricing details, see [Batch Pricing](https://azure.microsoft.com/pricing/details/batch/).</span></span>

<span data-ttu-id="9f35d-113">En ytterligare beskrivning av VM med låg prioritet finns hello blogginlägget: [Batch computing på en bråkdel av hello pris](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span><span class="sxs-lookup"><span data-stu-id="9f35d-113">For an additional discussion of low-priority VMs, see hello blog post announcement: [Batch computing at a fraction of hello price](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9f35d-114">Låg prioritet VM finns för närvarande i förhandsvisning och är bara tillgängliga för arbetsbelastningar som körs i en Batch.</span><span class="sxs-lookup"><span data-stu-id="9f35d-114">Low-priority VMs are currently in preview, and are available only for workloads running in Batch.</span></span> 
>
>

## <a name="use-cases-for-low-priority-vms"></a><span data-ttu-id="9f35d-115">Användningsfall för VM med låg prioritet</span><span class="sxs-lookup"><span data-stu-id="9f35d-115">Use cases for low-priority VMs</span></span>

<span data-ttu-id="9f35d-116">Hello egenskaper med låg prioritet virtuella datorer, vilka arbetsbelastningar kan och inte använda dem?</span><span class="sxs-lookup"><span data-stu-id="9f35d-116">Given hello characteristics of low-priority VMs, what workloads can and cannot use them?</span></span> <span data-ttu-id="9f35d-117">I allmänhet är batch bearbetningsbelastningar passa, eftersom jobben är uppdelad i många parallella aktiviteter eller det finns många jobb som skalats ut och distribueras över flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9f35d-117">In general, batch processing workloads are a good fit, as jobs are broken into many parallel tasks or there are many jobs that are scaled out and distributed across many VMs.</span></span>

-   <span data-ttu-id="9f35d-118">toomaximize användning av överskott kapacitet i Azure, lämplig jobb kan skala ut.</span><span class="sxs-lookup"><span data-stu-id="9f35d-118">toomaximize use of surplus capacity in Azure, suitable jobs can scale out.</span></span>

-   <span data-ttu-id="9f35d-119">Ibland virtuella datorer kanske inte tillgänglig eller ska avbrytas, vilket leder till minskad kapacitet för jobb och leda tootask avbrott och repriser.</span><span class="sxs-lookup"><span data-stu-id="9f35d-119">Occasionally VMs may not be available or will be preempted, which will result in reduced capacity for jobs and may lead tootask interruption and reruns.</span></span> <span data-ttu-id="9f35d-120">Jobb måste därför vara flexibel hello tidpunkt som de kan ta toorun.</span><span class="sxs-lookup"><span data-stu-id="9f35d-120">Jobs must therefore be flexible in hello time they can take toorun.</span></span>

-   <span data-ttu-id="9f35d-121">Jobb med längre uppgifter kan påverkas mer om avbryts.</span><span class="sxs-lookup"><span data-stu-id="9f35d-121">Jobs with longer tasks may be impacted more if interrupted.</span></span> <span data-ttu-id="9f35d-122">Om tidskrävande uppgifter implementera kontrollpunkter toosave förlopp som de körs, skulle sedan effekten av avbrott vara betydligt lägre.</span><span class="sxs-lookup"><span data-stu-id="9f35d-122">If long-running tasks implement checkpointing toosave progress as they execute, then the impact of interruption would be far less.</span></span> <span data-ttu-id="9f35d-123">Uppgifter med kortare körningstider tenderar toowork bäst med låg prioritet virtuella datorer som hello effekten av avbrott är mindre.</span><span class="sxs-lookup"><span data-stu-id="9f35d-123">Tasks with shorter execution times tend toowork best with low-priority VMs as hello impact of interruption is far less.</span></span>

-   <span data-ttu-id="9f35d-124">Långvariga MPI-jobb som använder flera virtuella datorer inte är bra toouse kommer låg prioritet virtuella datorer som en frånkopplas VM troligen lead toohello hela jobbet med toobe kör igen.</span><span class="sxs-lookup"><span data-stu-id="9f35d-124">Long-running MPI jobs that utilize multiple VMs are not well suited toouse low-priority VMs as one preempted VM will likely lead toohello whole job having toobe run again.</span></span>

<span data-ttu-id="9f35d-125">Några exempel på batchbearbetning använder fall utmärkt toouse låg prioritet är:</span><span class="sxs-lookup"><span data-stu-id="9f35d-125">Some examples of batch processing use cases well suited toouse low-priority VMs are:</span></span>

-   <span data-ttu-id="9f35d-126">**Utveckling och testning**: I synnerhet om storskaliga lösningar utvecklas betydande besparingar kan genomföras.</span><span class="sxs-lookup"><span data-stu-id="9f35d-126">**Development and testing**: In particular, if large-scale solutions are being developed significant savings can be realized.</span></span> <span data-ttu-id="9f35d-127">Kan dra nytta av alla typer av testning, men stora belastningen testning och regression testning är bra använder.</span><span class="sxs-lookup"><span data-stu-id="9f35d-127">All types of testing can benefit, but large-scale load testing and regression testing are great uses.</span></span>

-   <span data-ttu-id="9f35d-128">**Kompletta kapacitet på begäran**: VM med låg prioritet kan användas för att komplettera regular dedikerade virtuella datorer – när det är tillgängligt, jobb kan skalas och därför slutföra snabbare för lägre kostnad; när inte tillgänglig, hello baslinje för dedikerade virtuella datorer är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="9f35d-128">**Supplementing on-demand capacity**: Low-priority VMs can be used to supplement regular dedicated VMs - when available, jobs can scale and therefore complete quicker for lower cost; when not available, hello baseline of dedicated VMs are available.</span></span>

-   <span data-ttu-id="9f35d-129">**Flexibla jobbet körningstid**: om det finns flexibilitet i hello tid jobb har toocomplete, och sedan potentiella fall kapaciteten som kan tillåtas, men med hello lägga till jobb med låg prioritet virtuella datorer som ofta körs snabbare och för en lägre kostnad.</span><span class="sxs-lookup"><span data-stu-id="9f35d-129">**Flexible job execution time**: If there is flexibility in hello time jobs have toocomplete, then potential drops in capacity can be tolerated; however, with hello addition of low-priority VMs jobs will frequently run faster and for a lower cost.</span></span>

<span data-ttu-id="9f35d-130">Batch-pooler kan vara konfigurerade toouse låg prioritet virtuella datorer på flera sätt, beroende på hello flexibilitet i jobbet körningstid:</span><span class="sxs-lookup"><span data-stu-id="9f35d-130">Batch pools can be configured toouse low-priority VMs in a few ways, depending on hello flexibility in job execution time:</span></span>

-   <span data-ttu-id="9f35d-131">VM med låg prioritet kan endast användas i en pool och Batch ska bara återställa alla preempted kapacitet när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="9f35d-131">Low-priority VMs can solely be used in a pool and Batch will simply recover any preempted capacity when available.</span></span> <span data-ttu-id="9f35d-132">Detta är hello billigaste sätt tooexecute jobb som används för virtuella datorer bara låg prioritet.</span><span class="sxs-lookup"><span data-stu-id="9f35d-132">This is hello cheapest way tooexecute jobs as only low-priority VMs are used.</span></span>

-   <span data-ttu-id="9f35d-133">VM med låg prioritet kan användas tillsammans med en fast baslinje för dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9f35d-133">Low-priority VMs can be used in conjunction with a fixed baseline of dedicated VMs.</span></span> <span data-ttu-id="9f35d-134">hello garanterar fast antal dedikerade virtuella datorer det alltid vissa kapacitet tookeep ett jobb som fortskrider.</span><span class="sxs-lookup"><span data-stu-id="9f35d-134">hello fixed number of dedicated VMs ensures there is always some capacity tookeep a job progressing.</span></span>

-   <span data-ttu-id="9f35d-135">Det kan vara dynamiska blandning av dedikerade och låg prioritet virtuella datorer, så att billigare låg prioritet virtuella datorer används endast när det är tillgängligt, men hello fullständig prisvärda dedikerade virtuella datorer skalas vid behov, tookeep en minimal mängd kapacitet tillgängliga tookeep hello jobb fortskrider.</span><span class="sxs-lookup"><span data-stu-id="9f35d-135">There can be dynamic mix of dedicated and low-priority VMs, so that the cheaper low-priority VMs are solely used when available, but hello full-priced dedicated VMs are scaled up when required, tookeep a minimum amount of capacity available tookeep hello jobs progressing.</span></span>

## <a name="batch-support-for-low-priority-vms"></a><span data-ttu-id="9f35d-136">Batch-stöd för virtuella datorer låg prioritet</span><span class="sxs-lookup"><span data-stu-id="9f35d-136">Batch support for low-priority VMs</span></span>

<span data-ttu-id="9f35d-137">Azure Batch tillhandahåller flera funktioner som gör det enkelt tooconsume och dra nytta av låg prioritet virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="9f35d-137">Azure Batch provides several capabilities that make it easy tooconsume and benefit from low-priority VMs:</span></span>

-   <span data-ttu-id="9f35d-138">Batch-pooler kan innehålla både dedikerade virtuella datorer och låg prioritet virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9f35d-138">Batch pools can contain both dedicated VMs and low-priority VMs.</span></span> <span data-ttu-id="9f35d-139">hello-numret för varje typ av VM kan anges när en programpool skapas eller ändras när som helst för en befintlig pool med hjälp av åtgärden för hello explicit ändra storlek eller Autoskala.</span><span class="sxs-lookup"><span data-stu-id="9f35d-139">hello number of each type of VM can be specified when a pool is created, or changed at any time for an existing pool, using hello explicit resize operation or using auto-scale.</span></span> <span data-ttu-id="9f35d-140">Jobb- och skicka kan ändras och behöver inte bry sig med hello VM typer i hello pool.</span><span class="sxs-lookup"><span data-stu-id="9f35d-140">Job and task submission can remain unchanged and do not need to be concerned with hello VM types in hello pool.</span></span> <span data-ttu-id="9f35d-141">Det är också möjligt toohave poolen helt använder låg prioritet VMs toorun jobb som billigt som möjligt, men rotationsrutan upp dedikerade virtuella datorer om hello kapacitet sjunker under en minsta tröskelvärdet för att hålla jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="9f35d-141">It is also possible toohave a pool completely use low-priority VMs toorun jobs as cheaply as possible, but spin up dedicated VMs if hello capacity drops below a minimum threshold, to keep jobs running.</span></span>

-   <span data-ttu-id="9f35d-142">Batch-pooler söka automatiskt toohello mål antal VM med låg prioritet.</span><span class="sxs-lookup"><span data-stu-id="9f35d-142">Batch pools automatically seek toohello target number of low-priority VMs.</span></span> <span data-ttu-id="9f35d-143">Om virtuella datorer som frånkopplas försöker Batch tooreplace hello förlorade kapacitet och returnera toohello mål.</span><span class="sxs-lookup"><span data-stu-id="9f35d-143">If VMs are preempted, then Batch will attempt tooreplace hello lost capacity and return toohello target.</span></span>

-   <span data-ttu-id="9f35d-144">I hello fallet uppgifter störs Batch identifierar och automatiskt meddelanden uppgifter toobe kör igen.</span><span class="sxs-lookup"><span data-stu-id="9f35d-144">In hello case of tasks being interrupted, Batch will detect and automatically requeue tasks toobe run again.</span></span>

-   <span data-ttu-id="9f35d-145">Låg prioritet virtuella datorer har ett kärnkvot som skiljer sig från dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="9f35d-145">Low-priority VMs have a core quota that differs from that of dedicated VMs.</span></span> 
    <span data-ttu-id="9f35d-146">hello citattecken för virtuella datorer låg prioritet är högre än dedikerade virtuella datorer, eftersom VM med låg prioritet billigare.</span><span class="sxs-lookup"><span data-stu-id="9f35d-146">hello quote for low-priority VMs is higher than that of dedicated VMs, because low-priority VMs cost less.</span></span> <span data-ttu-id="9f35d-147">Se [Batch-tjänsten kvoter och gränser](batch-quota-limit.md#resource-quotas) för mer information.</span><span class="sxs-lookup"><span data-stu-id="9f35d-147">See [Batch service quotas and limits](batch-quota-limit.md#resource-quotas) for more information.</span></span>    

> [!NOTE]
> <span data-ttu-id="9f35d-148">Låg prioritet virtuella datorer stöds inte för närvarande för Batch-konton där hello poolen allokering läge har angetts för[användarens prenumeration](batch-account-create-portal.md#user-subscription-mode).</span><span class="sxs-lookup"><span data-stu-id="9f35d-148">Low-priority VMs are not currently supported for Batch accounts where hello pool allocation mode is set too[User subscription](batch-account-create-portal.md#user-subscription-mode).</span></span>
>
>

## <a name="create-and-update-pools"></a><span data-ttu-id="9f35d-149">Skapa och uppdatera pooler</span><span class="sxs-lookup"><span data-stu-id="9f35d-149">Create and update pools</span></span>

<span data-ttu-id="9f35d-150">En Batch-pool kan innehålla både dedikerade och låg prioritet virtuella datorer (även hänvisade tooas compute-noder).</span><span class="sxs-lookup"><span data-stu-id="9f35d-150">A Batch pool can contain both dedicated and low-priority VMs (also referred tooas compute nodes).</span></span> <span data-ttu-id="9f35d-151">Du kan ange hello mål antalet compute-noder för virtuella datorer både dedikerade och låg prioritet.</span><span class="sxs-lookup"><span data-stu-id="9f35d-151">You can set hello target number of compute nodes for both dedicated and low-priority VMs.</span></span> <span data-ttu-id="9f35d-152">hello mål antalet noder anger hello många virtuella datorer som du vill toohave i hello pool.</span><span class="sxs-lookup"><span data-stu-id="9f35d-152">hello target number of nodes specifies hello number of VMs you want toohave in hello pool.</span></span>

<span data-ttu-id="9f35d-153">Till exempel dedikerade toocreate en pool med hjälp av Azure-molntjänst virtuella datorer med ett mål för 5 virtuella datorer och 20 låg prioritet för virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="9f35d-153">For example, toocreate a pool using Azure cloud service VMs with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

<span data-ttu-id="9f35d-154">toocreate en pool med virtuella Azure-datorer (i det här fallet virtuella Linux-datorer) med ett mål för 5 dedikerade virtuella datorer och 20 låg prioritet för virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="9f35d-154">toocreate a pool using Azure virtual machines (in this case Linux VMs) with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create hello pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

<span data-ttu-id="9f35d-155">Du kan få hello aktuellt antal noder för virtuella datorer både dedikerade och låg prioritet:</span><span class="sxs-lookup"><span data-stu-id="9f35d-155">You can get hello current number of nodes for both dedicated and low-priority VMs:</span></span>

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

<span data-ttu-id="9f35d-156">Poolen noder har en egenskap tooindicate om hello noden är en virtuell dator för dedikerade eller låg prioritet:</span><span class="sxs-lookup"><span data-stu-id="9f35d-156">Pool nodes have a property tooindicate if hello node is a dedicated or low-priority VM:</span></span>

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

<span data-ttu-id="9f35d-157">När en eller flera noder i en pool frånkopplas, en lista över noderna på poolen fortfarande returneras dessa noder, hello aktuellt antal noder med låg prioritet förblir oförändrat, men de noderna har tillståndet anger toothe **tillfälligt**tillstånd.</span><span class="sxs-lookup"><span data-stu-id="9f35d-157">When one or more nodes in a pool are preempted, a list nodes operation on the pool will still return those nodes, hello current number of low-priority nodes will remain unchanged, but those nodes will have their state set toothe **Preempted** state.</span></span> <span data-ttu-id="9f35d-158">Batch försöker toofind ersättning virtuella datorer och, om det lyckas, hello noder går igenom **skapa** och sedan **Start** tillstånd innan den blir tillgänglig för körning av aktiviteten, precis som nya noder.</span><span class="sxs-lookup"><span data-stu-id="9f35d-158">Batch will attempt toofind replacement VMs and, if successful, hello nodes will go through **Creating** and then **Starting** states before becoming available for task execution, just like new nodes.</span></span>

## <a name="scale-a-pool-containing-low-priority-vms"></a><span data-ttu-id="9f35d-159">Skala en pool som innehåller VM med låg prioritet</span><span class="sxs-lookup"><span data-stu-id="9f35d-159">Scale a pool containing low-priority VMs</span></span>

<span data-ttu-id="9f35d-160">Som med pooler som endast består av dedikerade virtuella datorer, är det möjligt tooscale en pool med låg prioritet virtuella datorer genom att anropa metoden för storleksändring av hello eller genom att använda automatisk skalning.</span><span class="sxs-lookup"><span data-stu-id="9f35d-160">As with pools solely consisting of dedicated VMs, it is possible tooscale a pool containing low-priority VMs by calling hello Resize method or by using auto-scale.</span></span>

<span data-ttu-id="9f35d-161">hello åtgärden Ändra storlek för poolen tar en valfri andra parameter som uppdaterar värdet för **targetLowPriorityNodes**:</span><span class="sxs-lookup"><span data-stu-id="9f35d-161">hello pool resize operation takes a second optional parameter that updates the value of **targetLowPriorityNodes**:</span></span>

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

<span data-ttu-id="9f35d-162">hello poolen Autoskala formeln stöder låg prioritet virtuella datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9f35d-162">hello pool auto-scale formula supports low-priority VMs as follows:</span></span>

-   <span data-ttu-id="9f35d-163">Du kan hämta eller ange hello värde för hello tjänst definierade variabel **$TargetLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="9f35d-163">You can get or set hello value of hello service-defined variable **$TargetLowPriorityNodes**.</span></span>

-   <span data-ttu-id="9f35d-164">Du kan hämta hello värdet för hello tjänst definierade variabel **$CurrentLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="9f35d-164">You can get hello value of hello service-defined variable **$CurrentLowPriorityNodes**.</span></span>

-   <span data-ttu-id="9f35d-165">Du kan hämta hello värdet för hello tjänst definierade variabel **$PreemptedNodeCount**.</span><span class="sxs-lookup"><span data-stu-id="9f35d-165">You can get hello value of hello service-defined variable **$PreemptedNodeCount**.</span></span> 
    <span data-ttu-id="9f35d-166">Den här variabeln returnerar hello antalet noder i hello frånkopplas tillstånd och gör att du kan skala upp eller ned hello antal dedikerade noder, beroende på hello antalet frånkopplas noder som inte är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="9f35d-166">This variable returns hello number of nodes in hello preempted state and allows you to scale up or down hello number of dedicated nodes, depending on hello number of preempted nodes that are unavailable.</span></span>

## <a name="jobs-and-tasks"></a><span data-ttu-id="9f35d-167">Jobb och uppgifter</span><span class="sxs-lookup"><span data-stu-id="9f35d-167">Jobs and tasks</span></span>

<span data-ttu-id="9f35d-168">Jobb och uppgifter som kräver mycket lite stöd för låg prioritet noder. hello endast stöder är följande:</span><span class="sxs-lookup"><span data-stu-id="9f35d-168">Jobs and tasks require very little support for low-priority nodes; hello only support is as follows:</span></span>

-   <span data-ttu-id="9f35d-169">Hej JobManagerTask-egenskapen för ett jobb har en ny egenskap **AllowLowPriorityNode**.</span><span class="sxs-lookup"><span data-stu-id="9f35d-169">hello JobManagerTask property of a job has a new property, **AllowLowPriorityNode**.</span></span> 
    <span data-ttu-id="9f35d-170">När den här egenskapen är true kan du schemalägga hello projektaktivitet manager på en dedikerad eller låg prioritet nod.</span><span class="sxs-lookup"><span data-stu-id="9f35d-170">When this property is true, hello job manager task can be scheduled on either a dedicated or low-priority node.</span></span> <span data-ttu-id="9f35d-171">Om den här egenskapen är false kommer hello projektaktivitet manager att schemalagda tooa dedikerade nod.</span><span class="sxs-lookup"><span data-stu-id="9f35d-171">If this property is false, hello job manager task will be scheduled tooa dedicated node only.</span></span>

-   <span data-ttu-id="9f35d-172">En [miljövariabeln](batch-compute-node-environment-variables.md) är tillgängliga tooa aktivitet program så att den kan avgöra om den körs på en nod med låg prioritet eller dedikerade.</span><span class="sxs-lookup"><span data-stu-id="9f35d-172">An [environment variable](batch-compute-node-environment-variables.md) is available tooa task application so that it can determine whether it is running on a low-priority or dedicated node.</span></span> <span data-ttu-id="9f35d-173">hello miljövariabel är AZ_BATCH_NODE_IS_DEDICATED.</span><span class="sxs-lookup"><span data-stu-id="9f35d-173">hello environment variable is AZ_BATCH_NODE_IS_DEDICATED.</span></span>

## <a name="handling-preemption"></a><span data-ttu-id="9f35d-174">Hantera avstängning</span><span class="sxs-lookup"><span data-stu-id="9f35d-174">Handling preemption</span></span>

<span data-ttu-id="9f35d-175">Virtuella datorer kan ibland avbrytas; När detta inträffar hello Batch följande:</span><span class="sxs-lookup"><span data-stu-id="9f35d-175">VMs may occasionally be preempted; when this happens, Batch does hello following:</span></span>

-   <span data-ttu-id="9f35d-176">hello frånkopplas virtuella datorer har det tillståndet uppdateras**tillfälligt**.</span><span class="sxs-lookup"><span data-stu-id="9f35d-176">hello preempted VMs have their state updated too**Preempted**.</span></span>
-   <span data-ttu-id="9f35d-177">Om uppgifter som kördes på frånkopplas hello nod virtuella datorer, och sedan uppgifterna begärdes och kör igen.</span><span class="sxs-lookup"><span data-stu-id="9f35d-177">If tasks were running on hello preempted node VMs, then those tasks are requeued and run again.</span></span>
-   <span data-ttu-id="9f35d-178">hello VM raderas effektivt, inledande tooany data som lagras lokalt på hello VM förlorad.</span><span class="sxs-lookup"><span data-stu-id="9f35d-178">hello VM is effectively deleted, leading tooany data stored locally on hello VM being lost.</span></span>
-   <span data-ttu-id="9f35d-179">hello poolen försöker kontinuerligt tooreach hello mål antalet låg prioritet noder som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="9f35d-179">hello pool continually attempts tooreach hello target number of low-priority nodes available.</span></span> <span data-ttu-id="9f35d-180">När ersättningskapacitet, noderna hålla sina ID: n, men är initieras igen, gå igenom **skapa** och **Start** tillstånd innan de är tillgängliga för schemaläggning.</span><span class="sxs-lookup"><span data-stu-id="9f35d-180">When replacement capacity is found, the nodes keep their ids, but are re-initialized, going through **Creating** and **Starting** states before they are available for task scheduling.</span></span>
-   <span data-ttu-id="9f35d-181">Avstängningen antal är tillgängliga som ett mått i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9f35d-181">Preemption counts are available as a metric in hello Azure portal.</span></span>

## <a name="metrics"></a><span data-ttu-id="9f35d-182">Mått</span><span class="sxs-lookup"><span data-stu-id="9f35d-182">Metrics</span></span>

<span data-ttu-id="9f35d-183">Nya är tillgängliga i hello [Azure-portalen](https://portal.azure.com) för låg prioritet noder.</span><span class="sxs-lookup"><span data-stu-id="9f35d-183">New metrics are available in hello [Azure portal](https://portal.azure.com) for low-priority nodes.</span></span> <span data-ttu-id="9f35d-184">Dessa är:</span><span class="sxs-lookup"><span data-stu-id="9f35d-184">These metrics are:</span></span>

- <span data-ttu-id="9f35d-185">Antalet noder med låg prioritet</span><span class="sxs-lookup"><span data-stu-id="9f35d-185">Low-Priority Node Count</span></span>
- <span data-ttu-id="9f35d-186">Antal kärnor med låg prioritet</span><span class="sxs-lookup"><span data-stu-id="9f35d-186">Low-Priority Core Count</span></span> 
- <span data-ttu-id="9f35d-187">Antalet frånkopplas noder</span><span class="sxs-lookup"><span data-stu-id="9f35d-187">Preempted Node Count</span></span>

<span data-ttu-id="9f35d-188">tooview mått i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="9f35d-188">tooview metrics in hello Azure portal:</span></span>

1. <span data-ttu-id="9f35d-189">Navigera tooyour Batch-kontot i hello-portalen och visa hello inställningar för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="9f35d-189">Navigate tooyour Batch account in hello portal, and view hello settings for your Batch account.</span></span>
2. <span data-ttu-id="9f35d-190">Välj **mått** från hello **övervakning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9f35d-190">Select **Metrics** from hello **Monitoring** section.</span></span>
3. <span data-ttu-id="9f35d-191">Välj hello mått som du önskar hello **tillgängliga mått** lista.</span><span class="sxs-lookup"><span data-stu-id="9f35d-191">Select hello metrics you desire from hello **Available Metrics** list.</span></span>

![Mätvärden för noder med låg prioritet](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a><span data-ttu-id="9f35d-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9f35d-193">Next steps</span></span>

* <span data-ttu-id="9f35d-194">Läs hello [Batch funktionsöversikt för utvecklare](batch-api-basics.md), viktig information för alla förbereder toouse Batch.</span><span class="sxs-lookup"><span data-stu-id="9f35d-194">Read hello [Batch feature overview for developers](batch-api-basics.md), essential information for anyone preparing toouse Batch.</span></span> <span data-ttu-id="9f35d-195">hello artikeln innehåller mer detaljerad information om Batch-tjänsten resurser som pooler, noder, jobb och uppgifter och hello många API-funktioner som du kan använda när du skapar Batch-program.</span><span class="sxs-lookup"><span data-stu-id="9f35d-195">hello article contains more detailed information about Batch service resources like pools, nodes, jobs, and tasks, and hello many API features that you can use while building your Batch application.</span></span>
* <span data-ttu-id="9f35d-196">Lär dig mer om hello [Batch-API: er och verktyg](batch-apis-tools.md) tillgängliga för att skapa Batch-lösningar.</span><span class="sxs-lookup"><span data-stu-id="9f35d-196">Learn about hello [Batch APIs and tools](batch-apis-tools.md) available for building Batch solutions.</span></span>
