---
title: "Köra Azure Batch arbetsbelastningar på kostnadseffektiv låg prioritet för virtuella datorer (förhandsversion) | Microsoft Docs"
description: "Lär dig hur du etablerar låg prioritet virtuella datorer för att minska kostnaden för Azure Batch-arbetsbelastningar."
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
ms.openlocfilehash: 9bf0ac322020d8a8453011c3207c1930175db6d3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-low-priority-vms-with-batch-preview"></a><span data-ttu-id="96cc0-103">Använd låg prioritet virtuella datorer med Batch (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="96cc0-103">Use low-priority VMs with Batch (Preview)</span></span>

<span data-ttu-id="96cc0-104">Azure Batch har låg priorty virtuella datorer (VM) för att minska kostnaden för Batch-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="96cc0-104">Azure Batch offers low-priorty virtual machines (VMs) to reduce the cost of Batch workloads.</span></span> <span data-ttu-id="96cc0-105">VM med låg prioritet möjliggör nya typer av Batch-arbetsbelastningar genom att tillhandahålla mycket datorkraft som också är ekonomiskt.</span><span class="sxs-lookup"><span data-stu-id="96cc0-105">Low-priority VMs make new types of Batch workloads possible by providing a large amount of compute power that is also economical.</span></span>

<span data-ttu-id="96cc0-106">Låg prioritet VMs utnyttja överflödiga kapacitet i Azure.</span><span class="sxs-lookup"><span data-stu-id="96cc0-106">Low-priority VMs take advantage of surplus capacity in Azure.</span></span> <span data-ttu-id="96cc0-107">När du anger låg prioritet virtuella datorer i din pooler kan Azure Batch automatiskt använda den här överskott när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="96cc0-107">When you specify low-priority VMs in your pools, Azure Batch can automatically use this surplus when available.</span></span>

<span data-ttu-id="96cc0-108">Förhållandet för med låg prioritet virtuella datorer är dessa virtuella datorer kan avbrytas om någon överskott kapacitet finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="96cc0-108">The tradeoff for using low-priority VMs is that those VMs may be preempted when no surplus capacity is available in Azure.</span></span> <span data-ttu-id="96cc0-109">Därför är låg prioritet mest lämpliga för vissa typer av arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="96cc0-109">For this reason, low-priority VMs are most suitable for certain types of workloads.</span></span> <span data-ttu-id="96cc0-110">Använd VM med låg prioritet för batch- och asynkron bearbetning arbetsbelastningar där slutförandetid jobbet är flexibel och vad som ska distribueras till många virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96cc0-110">Use low-priority VMs for batch and asynchronous processing workloads where the job completion time is flexible and the work is distributed across many VMs.</span></span>

<span data-ttu-id="96cc0-111">Låg prioritet är betydligt mindre dyrbart än dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96cc0-111">Low-priority VMs are significantly less expensive than dedicated VMs.</span></span> <span data-ttu-id="96cc0-112">Information om priser, se [Batch priser](https://azure.microsoft.com/pricing/details/batch/).</span><span class="sxs-lookup"><span data-stu-id="96cc0-112">For pricing details, see [Batch Pricing](https://azure.microsoft.com/pricing/details/batch/).</span></span>

<span data-ttu-id="96cc0-113">En ytterligare beskrivning av låg prioritet virtuella datorer finns i blogginlägget: [Batch computing på en bråkdel av priset](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span><span class="sxs-lookup"><span data-stu-id="96cc0-113">For an additional discussion of low-priority VMs, see the blog post announcement: [Batch computing at a fraction of the price](https://azure.microsoft.com/blog/announcing-public-preview-of-azure-batch-low-priority-vms/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96cc0-114">Låg prioritet VM finns för närvarande i förhandsvisning och är bara tillgängliga för arbetsbelastningar som körs i en Batch.</span><span class="sxs-lookup"><span data-stu-id="96cc0-114">Low-priority VMs are currently in preview, and are available only for workloads running in Batch.</span></span> 
>
>

## <a name="use-cases-for-low-priority-vms"></a><span data-ttu-id="96cc0-115">Användningsfall för VM med låg prioritet</span><span class="sxs-lookup"><span data-stu-id="96cc0-115">Use cases for low-priority VMs</span></span>

<span data-ttu-id="96cc0-116">Angivna egenskaper med låg prioritet virtuella datorer, vilka arbetsbelastningar kan och inte kan använda dem?</span><span class="sxs-lookup"><span data-stu-id="96cc0-116">Given the characteristics of low-priority VMs, what workloads can and cannot use them?</span></span> <span data-ttu-id="96cc0-117">I allmänhet är batch bearbetningsbelastningar passa, eftersom jobben är uppdelad i många parallella aktiviteter eller det finns många jobb som skalats ut och distribueras över flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96cc0-117">In general, batch processing workloads are a good fit, as jobs are broken into many parallel tasks or there are many jobs that are scaled out and distributed across many VMs.</span></span>

-   <span data-ttu-id="96cc0-118">Om du vill maximera användningen av överskott kapacitet i Azure kan lämplig jobb skala ut.</span><span class="sxs-lookup"><span data-stu-id="96cc0-118">To maximize use of surplus capacity in Azure, suitable jobs can scale out.</span></span>

-   <span data-ttu-id="96cc0-119">Ibland virtuella datorer kanske inte tillgänglig eller ska avbrytas, vilket leder till minskad kapacitet för jobb och kan leda till avbrott i aktiviteten och repriser.</span><span class="sxs-lookup"><span data-stu-id="96cc0-119">Occasionally VMs may not be available or will be preempted, which will result in reduced capacity for jobs and may lead to task interruption and reruns.</span></span> <span data-ttu-id="96cc0-120">Jobb måste därför vara flexibel tidpunkt som de kan ta för att köra.</span><span class="sxs-lookup"><span data-stu-id="96cc0-120">Jobs must therefore be flexible in the time they can take to run.</span></span>

-   <span data-ttu-id="96cc0-121">Jobb med längre uppgifter kan påverkas mer om avbryts.</span><span class="sxs-lookup"><span data-stu-id="96cc0-121">Jobs with longer tasks may be impacted more if interrupted.</span></span> <span data-ttu-id="96cc0-122">Om tidskrävande uppgifter implementera kontrollpunkter för att spara förloppet som de körs, skulle sedan effekten av avbrott vara betydligt lägre.</span><span class="sxs-lookup"><span data-stu-id="96cc0-122">If long-running tasks implement checkpointing to save progress as they execute, then the impact of interruption would be far less.</span></span> <span data-ttu-id="96cc0-123">Uppgifter med kortare körningstider tenderar fungerar bäst med låg prioritet virtuella datorer som effekten av avbrott är mindre.</span><span class="sxs-lookup"><span data-stu-id="96cc0-123">Tasks with shorter execution times tend to work best with low-priority VMs as the impact of interruption is far less.</span></span>

-   <span data-ttu-id="96cc0-124">Långvariga MPI-jobb som använder flera virtuella datorer inte är bra leder om du vill använda låg prioritet virtuella datorer som en frånkopplas VM sannolikt till hela jobbet att köras igen.</span><span class="sxs-lookup"><span data-stu-id="96cc0-124">Long-running MPI jobs that utilize multiple VMs are not well suited to use low-priority VMs as one preempted VM will likely lead to the whole job having to be run again.</span></span>

<span data-ttu-id="96cc0-125">Det är några exempel på batch bearbetning användningsfall och lämpligt för att använda VM med låg prioritet:</span><span class="sxs-lookup"><span data-stu-id="96cc0-125">Some examples of batch processing use cases well suited to use low-priority VMs are:</span></span>

-   <span data-ttu-id="96cc0-126">**Utveckling och testning**: I synnerhet om storskaliga lösningar utvecklas betydande besparingar kan genomföras.</span><span class="sxs-lookup"><span data-stu-id="96cc0-126">**Development and testing**: In particular, if large-scale solutions are being developed significant savings can be realized.</span></span> <span data-ttu-id="96cc0-127">Kan dra nytta av alla typer av testning, men stora belastningen testning och regression testning är bra använder.</span><span class="sxs-lookup"><span data-stu-id="96cc0-127">All types of testing can benefit, but large-scale load testing and regression testing are great uses.</span></span>

-   <span data-ttu-id="96cc0-128">**Kompletta kapacitet på begäran**: VM med låg prioritet kan användas för att komplettera regular dedikerade virtuella datorer – när det är tillgängligt, jobb kan skalas och därför slutföra snabbare för lägre kostnad; när det är inte tillgänglig, baslinje för dedikerade virtuella datorer är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="96cc0-128">**Supplementing on-demand capacity**: Low-priority VMs can be used to supplement regular dedicated VMs - when available, jobs can scale and therefore complete quicker for lower cost; when not available, the baseline of dedicated VMs are available.</span></span>

-   <span data-ttu-id="96cc0-129">**Flexibla jobbet körningstid**: om det finns flexibilitet i tid jobben har slutförts, och sedan potentiella fall kapaciteten som kan tillåtas, men med låg prioritet VMs jobb kommer ofta köra snabbare och för en lägre kostnad.</span><span class="sxs-lookup"><span data-stu-id="96cc0-129">**Flexible job execution time**: If there is flexibility in the time jobs have to complete, then potential drops in capacity can be tolerated; however, with the addition of low-priority VMs jobs will frequently run faster and for a lower cost.</span></span>

<span data-ttu-id="96cc0-130">Batch-pooler kan konfigureras för att använda låg prioritet virtuella datorer på flera sätt, beroende på flexibilitet vid körningstid för jobb:</span><span class="sxs-lookup"><span data-stu-id="96cc0-130">Batch pools can be configured to use low-priority VMs in a few ways, depending on the flexibility in job execution time:</span></span>

-   <span data-ttu-id="96cc0-131">VM med låg prioritet kan endast användas i en pool och Batch ska bara återställa alla preempted kapacitet när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="96cc0-131">Low-priority VMs can solely be used in a pool and Batch will simply recover any preempted capacity when available.</span></span> <span data-ttu-id="96cc0-132">Detta är det billigaste sättet att köra jobb som användes för virtuella datorer bara låg prioritet.</span><span class="sxs-lookup"><span data-stu-id="96cc0-132">This is the cheapest way to execute jobs as only low-priority VMs are used.</span></span>

-   <span data-ttu-id="96cc0-133">VM med låg prioritet kan användas tillsammans med en fast baslinje för dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96cc0-133">Low-priority VMs can be used in conjunction with a fixed baseline of dedicated VMs.</span></span> <span data-ttu-id="96cc0-134">Fast antal dedikerade virtuella datorer garanterar alltid är vissa kapacitet att behålla en jobbet fortskrider.</span><span class="sxs-lookup"><span data-stu-id="96cc0-134">The fixed number of dedicated VMs ensures there is always some capacity to keep a job progressing.</span></span>

-   <span data-ttu-id="96cc0-135">Det kan vara dynamiska blandning av dedikerade och låg prioritet virtuella datorer, så att de billigare låg prioritet virtuella datorerna används endast när det är tillgängligt, men fullständig prisvärda dedikerade virtuella datorer skalas när krävs för att hålla en minimal mängd kapacitet för att hålla jobb fortskrider.</span><span class="sxs-lookup"><span data-stu-id="96cc0-135">There can be dynamic mix of dedicated and low-priority VMs, so that the cheaper low-priority VMs are solely used when available, but the full-priced dedicated VMs are scaled up when required, to keep a minimum amount of capacity available to keep the jobs progressing.</span></span>

## <a name="batch-support-for-low-priority-vms"></a><span data-ttu-id="96cc0-136">Batch-stöd för virtuella datorer låg prioritet</span><span class="sxs-lookup"><span data-stu-id="96cc0-136">Batch support for low-priority VMs</span></span>

<span data-ttu-id="96cc0-137">Azure Batch innehåller flera funktioner som gör det enkelt att använda och dra nytta av låg prioritet virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="96cc0-137">Azure Batch provides several capabilities that make it easy to consume and benefit from low-priority VMs:</span></span>

-   <span data-ttu-id="96cc0-138">Batch-pooler kan innehålla både dedikerade virtuella datorer och låg prioritet virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96cc0-138">Batch pools can contain both dedicated VMs and low-priority VMs.</span></span> <span data-ttu-id="96cc0-139">Numret på varje typ av virtuell dator kan anges när en programpool skapas eller ändras när som helst för en befintlig pool med hjälp av åtgärden explicit ändra storlek eller Autoskala.</span><span class="sxs-lookup"><span data-stu-id="96cc0-139">The number of each type of VM can be specified when a pool is created, or changed at any time for an existing pool, using the explicit resize operation or using auto-scale.</span></span> <span data-ttu-id="96cc0-140">Jobb- och skicka kan ändras och behöver inte bry sig med VM-typer i poolen.</span><span class="sxs-lookup"><span data-stu-id="96cc0-140">Job and task submission can remain unchanged and do not need to be concerned with the VM types in the pool.</span></span> <span data-ttu-id="96cc0-141">Det är också möjligt att ha en pool som använder helt VM med låg prioritet för att köra jobb som billigt som möjligt, men rotationsrutan upp dedikerade virtuella datorer om kapacitet sjunker under en minsta tröskelvärdet för att hålla jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="96cc0-141">It is also possible to have a pool completely use low-priority VMs to run jobs as cheaply as possible, but spin up dedicated VMs if the capacity drops below a minimum threshold, to keep jobs running.</span></span>

-   <span data-ttu-id="96cc0-142">Batch-pooler försöka automatiskt mål antalet VM med låg prioritet.</span><span class="sxs-lookup"><span data-stu-id="96cc0-142">Batch pools automatically seek to the target number of low-priority VMs.</span></span> <span data-ttu-id="96cc0-143">Om virtuella datorer som frånkopplas försöker Batch att ersätta förlorade kapacitet och återgå till målet.</span><span class="sxs-lookup"><span data-stu-id="96cc0-143">If VMs are preempted, then Batch will attempt to replace the lost capacity and return to the target.</span></span>

-   <span data-ttu-id="96cc0-144">När det gäller uppgifter störs, Batch identifierar och automatiskt meddelanden aktiviteter körs igen.</span><span class="sxs-lookup"><span data-stu-id="96cc0-144">In the case of tasks being interrupted, Batch will detect and automatically requeue tasks to be run again.</span></span>

-   <span data-ttu-id="96cc0-145">Låg prioritet virtuella datorer har ett kärnkvot som skiljer sig från dedikerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="96cc0-145">Low-priority VMs have a core quota that differs from that of dedicated VMs.</span></span> 
    <span data-ttu-id="96cc0-146">Citattecken för VM med låg prioritet är högre än dedikerade virtuella datorer, eftersom VM med låg prioritet billigare.</span><span class="sxs-lookup"><span data-stu-id="96cc0-146">The quote for low-priority VMs is higher than that of dedicated VMs, because low-priority VMs cost less.</span></span> <span data-ttu-id="96cc0-147">Se [Batch-tjänsten kvoter och gränser](batch-quota-limit.md#resource-quotas) för mer information.</span><span class="sxs-lookup"><span data-stu-id="96cc0-147">See [Batch service quotas and limits](batch-quota-limit.md#resource-quotas) for more information.</span></span>    

> [!NOTE]
> <span data-ttu-id="96cc0-148">Låg prioritet virtuella datorer stöds inte för närvarande för Batch-konton där poolen allokering läget är inställt på [användarens prenumeration](batch-account-create-portal.md#user-subscription-mode).</span><span class="sxs-lookup"><span data-stu-id="96cc0-148">Low-priority VMs are not currently supported for Batch accounts where the pool allocation mode is set to [User subscription](batch-account-create-portal.md#user-subscription-mode).</span></span>
>
>

## <a name="create-and-update-pools"></a><span data-ttu-id="96cc0-149">Skapa och uppdatera pooler</span><span class="sxs-lookup"><span data-stu-id="96cc0-149">Create and update pools</span></span>

<span data-ttu-id="96cc0-150">En Batch-pool kan innehålla både dedikerade och låg prioritet virtuella datorer (även kallat compute-noder).</span><span class="sxs-lookup"><span data-stu-id="96cc0-150">A Batch pool can contain both dedicated and low-priority VMs (also referred to as compute nodes).</span></span> <span data-ttu-id="96cc0-151">Du kan ange antalet datornoderna mål för virtuella datorer både dedikerade och låg prioritet.</span><span class="sxs-lookup"><span data-stu-id="96cc0-151">You can set the target number of compute nodes for both dedicated and low-priority VMs.</span></span> <span data-ttu-id="96cc0-152">Mål antalet noder som anger hur många virtuella datorer du vill ha i poolen.</span><span class="sxs-lookup"><span data-stu-id="96cc0-152">The target number of nodes specifies the number of VMs you want to have in the pool.</span></span>

<span data-ttu-id="96cc0-153">Till exempel om du vill skapa en pool dedikerade med hjälp av Azure-molntjänst virtuella datorer med ett mål för 5 virtuella datorer och 20 låg prioritet för virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="96cc0-153">For example, to create a pool using Azure cloud service VMs with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4") // WS 2012 R2
);
```

<span data-ttu-id="96cc0-154">Om du vill skapa en pool dedikerade med virtuella Azure-datorer (i det här fallet virtuella Linux-datorer) med ett mål för 5 virtuella datorer och 20 låg prioritet för virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="96cc0-154">To create a pool using Azure virtual machines (in this case Linux VMs) with a target of 5 dedicated VMs and 20 low-priority VMs:</span></span>

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04.0-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard\_D2\_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

<span data-ttu-id="96cc0-155">Du kan få det aktuella antalet noder för virtuella datorer både dedikerade och låg prioritet:</span><span class="sxs-lookup"><span data-stu-id="96cc0-155">You can get the current number of nodes for both dedicated and low-priority VMs:</span></span>

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

<span data-ttu-id="96cc0-156">Poolen noder har en egenskap som anger om noden är en virtuell dator för dedikerade eller låg prioritet:</span><span class="sxs-lookup"><span data-stu-id="96cc0-156">Pool nodes have a property to indicate if the node is a dedicated or low-priority VM:</span></span>

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

<span data-ttu-id="96cc0-157">När en eller flera noder i en pool frånkopplas, en lista över noderna på poolen fortfarande returneras dessa noder, det aktuella antalet noder med låg prioritet förblir oförändrat, men de noderna har sina statusen inställd på den **tillfälligt** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="96cc0-157">When one or more nodes in a pool are preempted, a list nodes operation on the pool will still return those nodes, the current number of low-priority nodes will remain unchanged, but those nodes will have their state set to the **Preempted** state.</span></span> <span data-ttu-id="96cc0-158">Batch kommer att försöka hitta ersättning av virtuella datorer och, om det lyckas, noderna går igenom **skapa** och sedan **Start** tillstånd innan den blir tillgänglig för körning av aktiviteten, precis som nya noder.</span><span class="sxs-lookup"><span data-stu-id="96cc0-158">Batch will attempt to find replacement VMs and, if successful, the nodes will go through **Creating** and then **Starting** states before becoming available for task execution, just like new nodes.</span></span>

## <a name="scale-a-pool-containing-low-priority-vms"></a><span data-ttu-id="96cc0-159">Skala en pool som innehåller VM med låg prioritet</span><span class="sxs-lookup"><span data-stu-id="96cc0-159">Scale a pool containing low-priority VMs</span></span>

<span data-ttu-id="96cc0-160">Precis som med pooler som endast består av dedikerade virtuella datorer, är det möjligt att skala en pool som innehåller låg prioritet virtuella datorer genom att anropa metoden ändra storlek eller Autoskala.</span><span class="sxs-lookup"><span data-stu-id="96cc0-160">As with pools solely consisting of dedicated VMs, it is possible to scale a pool containing low-priority VMs by calling the Resize method or by using auto-scale.</span></span>

<span data-ttu-id="96cc0-161">Åtgärden Ändra storlek poolen tar en valfri andra parameter som uppdaterar värdet för **targetLowPriorityNodes**:</span><span class="sxs-lookup"><span data-stu-id="96cc0-161">The pool resize operation takes a second optional parameter that updates the value of **targetLowPriorityNodes**:</span></span>

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

<span data-ttu-id="96cc0-162">Poolen Autoskala formeln stöder låg prioritet virtuella datorer på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="96cc0-162">The pool auto-scale formula supports low-priority VMs as follows:</span></span>

-   <span data-ttu-id="96cc0-163">Du kan hämta eller ange värdet på variabeln tjänst definierade **$TargetLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="96cc0-163">You can get or set the value of the service-defined variable **$TargetLowPriorityNodes**.</span></span>

-   <span data-ttu-id="96cc0-164">Du kan hämta värdet för variabeln tjänst definierade **$CurrentLowPriorityNodes**.</span><span class="sxs-lookup"><span data-stu-id="96cc0-164">You can get the value of the service-defined variable **$CurrentLowPriorityNodes**.</span></span>

-   <span data-ttu-id="96cc0-165">Du kan hämta värdet för variabeln tjänst definierade **$PreemptedNodeCount**.</span><span class="sxs-lookup"><span data-stu-id="96cc0-165">You can get the value of the service-defined variable **$PreemptedNodeCount**.</span></span> 
    <span data-ttu-id="96cc0-166">Den här variabeln returnerar antalet noder i preempted tillstånd och gör att du kan skala upp eller ned antalet dedicerade noder, beroende på antalet frånkopplas noder som inte är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="96cc0-166">This variable returns the number of nodes in the preempted state and allows you to scale up or down the number of dedicated nodes, depending on the number of preempted nodes that are unavailable.</span></span>

## <a name="jobs-and-tasks"></a><span data-ttu-id="96cc0-167">Jobb och uppgifter</span><span class="sxs-lookup"><span data-stu-id="96cc0-167">Jobs and tasks</span></span>

<span data-ttu-id="96cc0-168">Jobb och uppgifter som kräver mycket lite stöd för låg prioritet noder. endast stöder är följande:</span><span class="sxs-lookup"><span data-stu-id="96cc0-168">Jobs and tasks require very little support for low-priority nodes; the only support is as follows:</span></span>

-   <span data-ttu-id="96cc0-169">Egenskapen JobManagerTask för ett jobb har en ny egenskap **AllowLowPriorityNode**.</span><span class="sxs-lookup"><span data-stu-id="96cc0-169">The JobManagerTask property of a job has a new property, **AllowLowPriorityNode**.</span></span> 
    <span data-ttu-id="96cc0-170">När den här egenskapen är true kan du schemalägga projektaktiviteten manager på en dedikerad eller låg prioritet nod.</span><span class="sxs-lookup"><span data-stu-id="96cc0-170">When this property is true, the job manager task can be scheduled on either a dedicated or low-priority node.</span></span> <span data-ttu-id="96cc0-171">Om den här egenskapen är false kommer att schemaläggas manager projektaktivitet till en dedikerad nod.</span><span class="sxs-lookup"><span data-stu-id="96cc0-171">If this property is false, the job manager task will be scheduled to a dedicated node only.</span></span>

-   <span data-ttu-id="96cc0-172">En [miljövariabeln](batch-compute-node-environment-variables.md) är tillgänglig för programmet för en aktivitet så att den kan avgöra om den körs på en nod med låg prioritet eller dedikerade.</span><span class="sxs-lookup"><span data-stu-id="96cc0-172">An [environment variable](batch-compute-node-environment-variables.md) is available to a task application so that it can determine whether it is running on a low-priority or dedicated node.</span></span> <span data-ttu-id="96cc0-173">Miljövariabeln är AZ_BATCH_NODE_IS_DEDICATED.</span><span class="sxs-lookup"><span data-stu-id="96cc0-173">The environment variable is AZ_BATCH_NODE_IS_DEDICATED.</span></span>

## <a name="handling-preemption"></a><span data-ttu-id="96cc0-174">Hantera avstängning</span><span class="sxs-lookup"><span data-stu-id="96cc0-174">Handling preemption</span></span>

<span data-ttu-id="96cc0-175">Virtuella datorer kan ibland avbrytas; När detta inträffar gör Batch följande:</span><span class="sxs-lookup"><span data-stu-id="96cc0-175">VMs may occasionally be preempted; when this happens, Batch does the following:</span></span>

-   <span data-ttu-id="96cc0-176">Preempted virtuella datorer har det tillståndet som uppdateras till **tillfälligt**.</span><span class="sxs-lookup"><span data-stu-id="96cc0-176">The preempted VMs have their state updated to **Preempted**.</span></span>
-   <span data-ttu-id="96cc0-177">Om aktiviteter körs på de virtuella datorerna som frånkopplas nod, har sedan aktiviteterna placerats i kö och kör igen.</span><span class="sxs-lookup"><span data-stu-id="96cc0-177">If tasks were running on the preempted node VMs, then those tasks are requeued and run again.</span></span>
-   <span data-ttu-id="96cc0-178">Den virtuella datorn är effektivt bort, vilket leder till alla data som lagras lokalt på den virtuella datorn går förlorad.</span><span class="sxs-lookup"><span data-stu-id="96cc0-178">The VM is effectively deleted, leading to any data stored locally on the VM being lost.</span></span>
-   <span data-ttu-id="96cc0-179">Poolen försöker kontinuerligt nå målet antalet låg prioritet noder som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="96cc0-179">The pool continually attempts to reach the target number of low-priority nodes available.</span></span> <span data-ttu-id="96cc0-180">När ersättningskapacitet, noderna hålla sina ID: n, men är initieras igen, gå igenom **skapa** och **Start** tillstånd innan de är tillgängliga för schemaläggning.</span><span class="sxs-lookup"><span data-stu-id="96cc0-180">When replacement capacity is found, the nodes keep their ids, but are re-initialized, going through **Creating** and **Starting** states before they are available for task scheduling.</span></span>
-   <span data-ttu-id="96cc0-181">Avstängningen antal är tillgängliga som ett mått i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="96cc0-181">Preemption counts are available as a metric in the Azure portal.</span></span>

## <a name="metrics"></a><span data-ttu-id="96cc0-182">Mått</span><span class="sxs-lookup"><span data-stu-id="96cc0-182">Metrics</span></span>

<span data-ttu-id="96cc0-183">Nya är tillgängliga i den [Azure-portalen](https://portal.azure.com) för låg prioritet noder.</span><span class="sxs-lookup"><span data-stu-id="96cc0-183">New metrics are available in the [Azure portal](https://portal.azure.com) for low-priority nodes.</span></span> <span data-ttu-id="96cc0-184">Dessa är:</span><span class="sxs-lookup"><span data-stu-id="96cc0-184">These metrics are:</span></span>

- <span data-ttu-id="96cc0-185">Antalet noder med låg prioritet</span><span class="sxs-lookup"><span data-stu-id="96cc0-185">Low-Priority Node Count</span></span>
- <span data-ttu-id="96cc0-186">Antal kärnor med låg prioritet</span><span class="sxs-lookup"><span data-stu-id="96cc0-186">Low-Priority Core Count</span></span> 
- <span data-ttu-id="96cc0-187">Antalet frånkopplas noder</span><span class="sxs-lookup"><span data-stu-id="96cc0-187">Preempted Node Count</span></span>

<span data-ttu-id="96cc0-188">Visa mått i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="96cc0-188">To view metrics in the Azure portal:</span></span>

1. <span data-ttu-id="96cc0-189">Navigera till Batch-kontot i portalen och visa inställningarna för Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="96cc0-189">Navigate to your Batch account in the portal, and view the settings for your Batch account.</span></span>
2. <span data-ttu-id="96cc0-190">Välj **mått** från den **övervakning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="96cc0-190">Select **Metrics** from the **Monitoring** section.</span></span>
3. <span data-ttu-id="96cc0-191">Välj det mått som du vill ha av den **tillgängliga mått** lista.</span><span class="sxs-lookup"><span data-stu-id="96cc0-191">Select the metrics you desire from the **Available Metrics** list.</span></span>

![Mätvärden för noder med låg prioritet](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a><span data-ttu-id="96cc0-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96cc0-193">Next steps</span></span>

* <span data-ttu-id="96cc0-194">Läs [Översikt över Batch-funktioner för utvecklare](batch-api-basics.md). Här finns viktig information för alla som tänker använda Batch.</span><span class="sxs-lookup"><span data-stu-id="96cc0-194">Read the [Batch feature overview for developers](batch-api-basics.md), essential information for anyone preparing to use Batch.</span></span> <span data-ttu-id="96cc0-195">Artikeln innehåller mer detaljerad information om Batch-tjänstresurser som pooler, noder, jobb och uppgifter, och de många API-funktioner som du kan använda när du skapar ett Batch-program.</span><span class="sxs-lookup"><span data-stu-id="96cc0-195">The article contains more detailed information about Batch service resources like pools, nodes, jobs, and tasks, and the many API features that you can use while building your Batch application.</span></span>
* <span data-ttu-id="96cc0-196">Läs om tillgängliga [Batch-API:er och verktyg](batch-apis-tools.md) för att skapa Batch-lösningar.</span><span class="sxs-lookup"><span data-stu-id="96cc0-196">Learn about the [Batch APIs and tools](batch-apis-tools.md) available for building Batch solutions.</span></span>
