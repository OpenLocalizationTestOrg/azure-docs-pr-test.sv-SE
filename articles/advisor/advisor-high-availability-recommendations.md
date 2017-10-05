---
title: "Azure hög tillgänglighet för Advisor-rekommendationer | Microsoft Docs"
description: "Använda Azure Advisor för att förbättra tillgängligheten för din Azure-distributioner."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 23c159471a6e5a7ad9cb545840e8afd3ac72ecba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="advisor-high-availability-recommendations"></a><span data-ttu-id="d5da5-103">Advisor-rekommendationer för hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="d5da5-103">Advisor High Availability recommendations</span></span>

<span data-ttu-id="d5da5-104">Azure Advisor hjälper dig att kontrollera och förbättra kontinuiteten i dina verksamhetskritiska program.</span><span class="sxs-lookup"><span data-stu-id="d5da5-104">Azure Advisor helps you ensure and improve the continuity of your business-critical applications.</span></span> <span data-ttu-id="d5da5-105">Du kan få rekommendationer för hög tillgänglighet av Advisor från den **hög tillgänglighet** på Advisor-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="d5da5-105">You can get high availability recommendations by Advisor from the **High Availability** tab of the Advisor dashboard.</span></span>

![Hög tillgänglighet-knappen på Advisor-instrumentpanelen](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a><span data-ttu-id="d5da5-107">Se till att virtuella feltolerans</span><span class="sxs-lookup"><span data-stu-id="d5da5-107">Ensure virtual machine fault tolerance</span></span>

<span data-ttu-id="d5da5-108">Advisor identifierar virtuella datorer som inte är en del av en tillgänglighetsuppsättning och rekommenderar flytta dem till en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="d5da5-108">Advisor identifies virtual machines that are not part of an availability set and recommends moving them into an availability set.</span></span> <span data-ttu-id="d5da5-109">För att ge ditt program redundans rekommenderar vi att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="d5da5-109">To provide redundancy to your application, we recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="d5da5-110">Den här konfigurationen garanterar att minst en virtuell dator under antingen en planerad eller oplanerad underhållshändelse, är tillgänglig och uppfyller SLA för Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="d5da5-110">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine is available and meets the Azure virtual machine SLA.</span></span> <span data-ttu-id="d5da5-111">Du kan välja att skapa en tillgänglighetsuppsättning för den virtuella datorn eller lägga till den virtuella datorn i en befintlig tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="d5da5-111">You can choose either to create an availability set for the virtual machine or to add the virtual machine to an existing availability set.</span></span>

> [!NOTE]
> <span data-ttu-id="d5da5-112">Om du väljer att skapa en tillgänglighetsuppsättning måste du lägga till minst en virtuell dator för mer till den.</span><span class="sxs-lookup"><span data-stu-id="d5da5-112">If you choose to create an availability set, you must add at least one more virtual machine into it.</span></span> <span data-ttu-id="d5da5-113">Vi rekommenderar att du grupperar två eller flera virtuella datorer i en tillgänglighetsgrupp anger att se till att minst en dator som är tillgänglig under ett avbrott.</span><span class="sxs-lookup"><span data-stu-id="d5da5-113">We recommend that you group two or more virtual machines in an availability set to ensure that at least one machine is available during an outage.</span></span>

![Advisor-rekommendationer: Använd tillgänglighetsuppsättningar för redundans för virtuell dator,](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a><span data-ttu-id="d5da5-115">Kontrollera tillgänglighet feltolerans</span><span class="sxs-lookup"><span data-stu-id="d5da5-115">Ensure availability set fault tolerance</span></span> 

<span data-ttu-id="d5da5-116">Advisor identifierar tillgänglighetsuppsättningar som innehåller en enda virtuell dator och rekommenderas att lägga till en eller flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d5da5-116">Advisor identifies availability sets that contain a single virtual machine and recommends adding one or more virtual machines to it.</span></span> <span data-ttu-id="d5da5-117">För att ge ditt program redundans rekommenderar vi att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="d5da5-117">To provide redundancy to your application, we recommend that you group two or more virtual machines in an availability set.</span></span> <span data-ttu-id="d5da5-118">Den här konfigurationen garanterar att minst en virtuell dator under antingen en planerad eller oplanerad underhållshändelse, är tillgänglig och uppfyller SLA för Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="d5da5-118">This configuration ensures that during either a planned or unplanned maintenance event, at least one virtual machine is available and meets the Azure virtual machine SLA.</span></span> <span data-ttu-id="d5da5-119">Du kan välja antingen skapa en virtuell dator eller att använda en befintlig virtuell dator och lägga till den i tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="d5da5-119">You can choose either to create a virtual machine or to use an existing virtual machine, and to add it to the availability set.</span></span>  

![Advisor-rekommendationer: Lägg till en eller flera virtuella datorer till den här tillgänglighetsuppsättningen](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a><span data-ttu-id="d5da5-121">Se till att programmet gateway feltolerans</span><span class="sxs-lookup"><span data-stu-id="d5da5-121">Ensure application gateway fault tolerance</span></span>
<span data-ttu-id="d5da5-122">För att garantera kontinuitet för verksamhetskritiska program som tillhandahålls av programgatewayer, Advisor identifierar programmet gateway-instanser som inte är konfigurerade för feltolerans och åtgärder som du kan vidta föreslås.</span><span class="sxs-lookup"><span data-stu-id="d5da5-122">To ensure the business continuity of mission-critical applications that are powered by application gateways, Advisor identifies application gateway instances that are not configured for fault tolerance, and it suggests remediation actions that you can take.</span></span> <span data-ttu-id="d5da5-123">Advisor identifierar medelstora eller stora enkelinstansprogram gateways och rekommenderar att lägga till minst en mer instansen.</span><span class="sxs-lookup"><span data-stu-id="d5da5-123">Advisor identifies medium or large single-instance application gateways, and it recommends adding at least one more instance.</span></span> <span data-ttu-id="d5da5-124">Den identifierar en eller flera instance små programgatewayer och rekommenderar att du migrerar till medelstora eller stora SKU: er.</span><span class="sxs-lookup"><span data-stu-id="d5da5-124">It also identifies single- or multi-instance small application gateways and recommends migrating to medium or large SKUs.</span></span> <span data-ttu-id="d5da5-125">Advisor rekommenderar dessa åtgärder för att se till att din gateway programinstanser konfigureras för att uppfylla de SLA krav som ställs för dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="d5da5-125">Advisor recommends these actions to ensure that your application gateway instances are configured to satisfy the current SLA requirements for these resources.</span></span>

![Advisor-rekommendationer: distribuera minst två medelstora eller stora storlek gateway programinstanser](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a><span data-ttu-id="d5da5-127">Förbättra prestanda och tillförlitlighet för virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="d5da5-127">Improve the performance and reliability of virtual machine disks</span></span>

<span data-ttu-id="d5da5-128">Advisor identifierar virtuella datorer med standarddiskar och rekommenderar att du uppgraderar till premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="d5da5-128">Advisor identifies virtual machines with standard disks and recommends upgrading to premium disks.</span></span>
 
<span data-ttu-id="d5da5-129">Azure Premium Storage ger stöd för virtuella datorer som körs I/O-intensiva arbetsbelastningar diskar med hög prestanda, låg latens.</span><span class="sxs-lookup"><span data-stu-id="d5da5-129">Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines that run I/O-intensive workloads.</span></span> <span data-ttu-id="d5da5-130">Virtuella diskar som använder premiumlagringskonton data som lagras på SSD-enheter (SSD).</span><span class="sxs-lookup"><span data-stu-id="d5da5-130">Virtual machine disks that use premium storage accounts store data on solid-state drives (SSDs).</span></span> <span data-ttu-id="d5da5-131">För bästa prestanda för ditt program rekommenderar vi att du migrerar alla virtuella diskar som kräver hög IOPS till premium-lagring.</span><span class="sxs-lookup"><span data-stu-id="d5da5-131">For the best performance for your application, we recommend that you migrate any virtual machine disks requiring high IOPS to premium storage.</span></span> 

<span data-ttu-id="d5da5-132">Om diskarna inte behöver höga IOPS, kan du begränsa kostnader genom att hålla dem i standardlagring.</span><span class="sxs-lookup"><span data-stu-id="d5da5-132">If your disks do not require high IOPS, you can limit costs by maintaining them in standard storage.</span></span> <span data-ttu-id="d5da5-133">Standardlagring lagrar data för virtuell disk på hårddiskar (HDD) i stället för SSD-enheter.</span><span class="sxs-lookup"><span data-stu-id="d5da5-133">Standard storage stores virtual machine disk data on hard disk drives (HDDs) instead of SSDs.</span></span> <span data-ttu-id="d5da5-134">Du kan välja att migrera dina virtuella datordiskar till premiumdiskar.</span><span class="sxs-lookup"><span data-stu-id="d5da5-134">You can choose to migrate your virtual machine disks to premium disks.</span></span> <span data-ttu-id="d5da5-135">Premiumdiskar stöds i de flesta virtuella SKU: er.</span><span class="sxs-lookup"><span data-stu-id="d5da5-135">Premium disks are supported on most virtual machine SKUs.</span></span> <span data-ttu-id="d5da5-136">Dock i vissa fall kan behöva om du vill använda premiumdiskar kan du uppgradera den virtuella datorn SKU: er samt.</span><span class="sxs-lookup"><span data-stu-id="d5da5-136">However, in some cases, if you want to use premium disks, you might need to upgrade your virtual machine SKUs as well.</span></span>

![Advisor-rekommendationer: förbättra tillförlitligheten för virtuella diskar genom att uppgradera till premiumdiskar](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a><span data-ttu-id="d5da5-138">Skydda dina data för virtuell dator tas bort av misstag</span><span class="sxs-lookup"><span data-stu-id="d5da5-138">Protect your virtual machine data from accidental deletion</span></span>
<span data-ttu-id="d5da5-139">Advisor identifierar virtuella datorer där säkerhetskopiering inte har aktiverats och rekommenderar att aktivera säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="d5da5-139">Advisor identifies virtual machines where backup is not enabled, and it recommends enabling backup.</span></span> <span data-ttu-id="d5da5-140">Konfigurera säkerhetskopiering av virtuella datorer garanterar tillgängligheten för affärskritiska data och ger skydd mot oavsiktlig borttagning eller skadade data.</span><span class="sxs-lookup"><span data-stu-id="d5da5-140">Setting up virtual machine backup ensures the availability of your business-critical data and offers protection against accidental deletion or corruption.</span></span>

![Advisor-rekommendationer: Konfigurera säkerhetskopiering av virtuella datorer för att skydda dina kritiska data](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a><span data-ttu-id="d5da5-142">Rekommendationer för åtkomst med hög tillgänglighet i Advisor</span><span class="sxs-lookup"><span data-stu-id="d5da5-142">Access high availability recommendations in Advisor</span></span>

1. <span data-ttu-id="d5da5-143">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d5da5-143">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="d5da5-144">I den vänstra rutan klickar du på **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="d5da5-144">In the left pane, click **More services**.</span></span>

3. <span data-ttu-id="d5da5-145">I fönstret service menyn under **övervakning och hantering av**, klickar du på **Azure Advisor**.</span><span class="sxs-lookup"><span data-stu-id="d5da5-145">In the service menu pane, under **Monitoring and Management**, click **Azure Advisor**.</span></span>  
 <span data-ttu-id="d5da5-146">Advisor-instrumentpanelen visas.</span><span class="sxs-lookup"><span data-stu-id="d5da5-146">The Advisor dashboard is displayed.</span></span>

4. <span data-ttu-id="d5da5-147">Advisor-instrumentpanelen, klicka på den **hög tillgänglighet** och välj den prenumeration som du vill få rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="d5da5-147">On the Advisor dashboard, click the **High Availability** tab, and then select the subscription for which you want to receive recommendations.</span></span>

> [!NOTE]
> <span data-ttu-id="d5da5-148">Om du vill komma åt Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor.</span><span class="sxs-lookup"><span data-stu-id="d5da5-148">To access Advisor recommendations, you must first *register your subscription* with Advisor.</span></span> <span data-ttu-id="d5da5-149">En prenumeration registreras när en *prenumeration ägare* startar Advisor instrumentpanelen och klickar på den **få rekommendationer** knappen.</span><span class="sxs-lookup"><span data-stu-id="d5da5-149">A subscription is registered when a *subscription Owner* launches the Advisor dashboard and clicks the **Get recommendations** button.</span></span> <span data-ttu-id="d5da5-150">Det här är en *engångsåtgärd*.</span><span class="sxs-lookup"><span data-stu-id="d5da5-150">This is a *one-time operation*.</span></span> <span data-ttu-id="d5da5-151">När prenumerationen har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.</span><span class="sxs-lookup"><span data-stu-id="d5da5-151">After the subscription is registered, you can access Advisor recommendations as *Owner*, *Contributor*, or *Reader* for a subscription, a resource group, or a specific resource.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5da5-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5da5-152">Next steps</span></span>

<span data-ttu-id="d5da5-153">Mer information om Advisor-rekommendationer finns:</span><span class="sxs-lookup"><span data-stu-id="d5da5-153">For more information about Advisor recommendations, see:</span></span>
* [<span data-ttu-id="d5da5-154">Introduktion till Azure Advisor</span><span class="sxs-lookup"><span data-stu-id="d5da5-154">Introduction to Azure Advisor</span></span>](advisor-overview.md)
* [<span data-ttu-id="d5da5-155">Kom igång med Advisor</span><span class="sxs-lookup"><span data-stu-id="d5da5-155">Get started with Advisor</span></span>](advisor-get-started.md)
* [<span data-ttu-id="d5da5-156">Kostnad Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="d5da5-156">Advisor Cost recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="d5da5-157">Advisor-rekommendationer</span><span class="sxs-lookup"><span data-stu-id="d5da5-157">Advisor Performance recommendations</span></span>](advisor-performance-recommendations.md)
* [<span data-ttu-id="d5da5-158">Advisor säkerhetsrekommendationer</span><span class="sxs-lookup"><span data-stu-id="d5da5-158">Advisor Security recommendations</span></span>](advisor-security-recommendations.md)

