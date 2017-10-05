---
title: Konfigurera ett Azure Service Fabric-kluster | Microsoft Docs
description: "Snabbstart – skapa ett Service Fabric-kluster i Azure för Windows eller Linux."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: ec59450052b377412a28f7eaf55d1f1512b55195
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a><span data-ttu-id="31511-103">Skapa ditt första Service Fabric-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="31511-103">Create your first Service Fabric cluster on Azure</span></span>
<span data-ttu-id="31511-104">Ett [Service Fabric-kluster](service-fabric-deploy-anywhere.md) är en nätverksansluten uppsättning virtuella eller fysiska datorer som dina mikrotjänster distribueras till och hanteras från.</span><span class="sxs-lookup"><span data-stu-id="31511-104">A [Service Fabric cluster](service-fabric-deploy-anywhere.md) is a network-connected set of virtual or physical machines into which your microservices are deployed and managed.</span></span> <span data-ttu-id="31511-105">I den här snabbstarten får du hjälp att skapa ett kluster med fem noder som körs i antingen Windows eller Linux på bara några minuter via [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) eller [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="31511-105">This quickstart helps you to create a five-node cluster, running on either Windows or Linux, through the [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) or [Azure portal](http://portal.azure.com) in just a few minutes.</span></span>  

<span data-ttu-id="31511-106">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="31511-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


## <a name="use-the-azure-portal"></a><span data-ttu-id="31511-107">Använda Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="31511-107">Use the Azure portal</span></span>

<span data-ttu-id="31511-108">Logga in på Azure Portal på [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="31511-108">Log in to the Azure portal at [http://portal.azure.com](http://portal.azure.com).</span></span>

### <a name="create-the-cluster"></a><span data-ttu-id="31511-109">Skapa klustret</span><span class="sxs-lookup"><span data-stu-id="31511-109">Create the cluster</span></span>

1. <span data-ttu-id="31511-110">Klicka på knappen **New** (Nytt) i det övre vänstra hörnet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="31511-110">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>
2. <span data-ttu-id="31511-111">Välj **Compute** från bladet **Nytt** och sedan **Service Fabric-kluster** från bladet **Compute**.</span><span class="sxs-lookup"><span data-stu-id="31511-111">Select **Compute** from the **New** blade and then select **Service Fabric Cluster** from the **Compute** blade.</span></span>
3. <span data-ttu-id="31511-112">Fyll i Service Fabric-formuläret **Grundläggande inställningar**.</span><span class="sxs-lookup"><span data-stu-id="31511-112">Fill out the Service Fabric **Basics** form.</span></span> <span data-ttu-id="31511-113">För **Operativsystem** väljer du den version av Windows eller Linux som du vill köra klusternoderna i.</span><span class="sxs-lookup"><span data-stu-id="31511-113">For **Operating system**, select the version of Windows or Linux you want the cluster nodes to run.</span></span> <span data-ttu-id="31511-114">Användarnamnet och lösenordet som anges här används för att logga in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="31511-114">The user name and password entered here is used to log in to the virtual machine.</span></span> <span data-ttu-id="31511-115">Skapa en ny för **Resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="31511-115">For **Resource group**, create a new one.</span></span> <span data-ttu-id="31511-116">En resursgrupp är en logisk behållare där Azure-resurser skapas och hanteras gemensamt.</span><span class="sxs-lookup"><span data-stu-id="31511-116">A resource group is a logical container into which Azure resources are created and collectively managed.</span></span> <span data-ttu-id="31511-117">När du är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="31511-117">When complete, click **OK**.</span></span>

    ![Utdata efter klusterinstallationen][cluster-setup-basics]

4. <span data-ttu-id="31511-119">Fyll i formuläret **Klusterkonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="31511-119">Fill out the **Cluster configuration** form.</span></span>  <span data-ttu-id="31511-120">För **Antal nodtyper** anger du "1".</span><span class="sxs-lookup"><span data-stu-id="31511-120">For **Node type count**, enter "1".</span></span>

5. <span data-ttu-id="31511-121">Välj **Node type 1 (Primary)** (Nodtyp 1 (Primär)) och fyll i formuläret **Konfiguration av nodtyp**.</span><span class="sxs-lookup"><span data-stu-id="31511-121">Select **Node type 1 (Primary)** and fill out the **Node type configuration** form.</span></span>  <span data-ttu-id="31511-122">Ange ett nodtypnamn och ställ in [Hållbarhetsnivå](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) på "Brons."</span><span class="sxs-lookup"><span data-stu-id="31511-122">Enter a node type name and set the [Durability tier](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) to "Bronze."</span></span>  <span data-ttu-id="31511-123">Välj en VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="31511-123">Select a VM size.</span></span>

    <span data-ttu-id="31511-124">Nodtypen definierar antalet virtuella datorer och deras storlek, anpassade slutpunkter och andra inställningar för virtuella datorer av samma typ.</span><span class="sxs-lookup"><span data-stu-id="31511-124">Node types define the VM size, number of VMs, custom endpoints, and other settings for the VMs of that type.</span></span> <span data-ttu-id="31511-125">Varje definierad nodtyp konfigureras som en separat VM-skalningsuppsättning som används till att distribuera och hantera virtuella datorer som en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="31511-125">Each node type defined is set up as a separate virtual machine scale set, which is used to deploy and managed virtual machines as a set.</span></span> <span data-ttu-id="31511-126">Varje nodtyp kan skalas upp eller ned oberoende av de andra, ha olika portar öppna och ha olika kapacitet.</span><span class="sxs-lookup"><span data-stu-id="31511-126">Each node type can be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>  <span data-ttu-id="31511-127">Den första, eller primära, nodtypen används för systemtjänsterna för Service Fabric, och den måste ha fem eller fler virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="31511-127">The first, or primary, node type is where Service Fabric system services are hosted and must have five or more VMs.</span></span>

    <span data-ttu-id="31511-128">Vid distribution till en produktionsmiljö är det viktigt med [kapacitetsplanering](service-fabric-cluster-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="31511-128">For any production deployment, [capacity planning](service-fabric-cluster-capacity.md) is an important step.</span></span>  <span data-ttu-id="31511-129">I den här snabbstarten kör du däremot inga program, så välj VM-storleken *DS1_v2 Standard*.</span><span class="sxs-lookup"><span data-stu-id="31511-129">For this quick start, however, you aren't running applications so select a *DS1_v2 Standard* VM size.</span></span>  <span data-ttu-id="31511-130">Välj ”Silver” som [tillförlitlighetsnivå](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) och en inledande kapacitet på 5 för VM-skalningsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="31511-130">Select "Silver" for the [reliability tier](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) and an initial virtual machine scale set capacity of 5.</span></span>  

    <span data-ttu-id="31511-131">Anpassade slutpunkter används till att öppna portar i Azure Load Balancer så att du kan ansluta med program som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="31511-131">Custom endpoints open up ports in the Azure load balancer so that you can connect with applications running on the cluster.</span></span>  <span data-ttu-id="31511-132">Ange ”80, 8172” så att du öppnar portarna 80 och 8172.</span><span class="sxs-lookup"><span data-stu-id="31511-132">Enter "80, 8172" to open up ports 80 and 8172.</span></span>

    <span data-ttu-id="31511-133">Markera inte kryssrutan **Konfigurera avancerade inställningar**, som används till att anpassa slutpunkter för TCP/HTTP-hantering, portintervall för program, [placeringsbegränsningar](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) och [kapacitetsegenskaper](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="31511-133">Do not check the **Configure advanced settings** box, which is used for customizing TCP/HTTP management endpoints, application port ranges, [placement constraints](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), and [capacity properties](service-fabric-cluster-resource-manager-metrics.md).</span></span>    

    <span data-ttu-id="31511-134">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="31511-134">Select **OK**.</span></span>

6. <span data-ttu-id="31511-135">I formuläret **Klusterkonfiguration** ställer du in **Diagnostik** på **På**.</span><span class="sxs-lookup"><span data-stu-id="31511-135">In the **Cluster configuration** form, set **Diagnostics** to **On**.</span></span>  <span data-ttu-id="31511-136">I den här snabbstarten behöver du inte ange några anpassade [infrastrukturinställningar](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="31511-136">For this quickstart, you do not need to enter any [fabric setting](service-fabric-cluster-fabric-settings.md) properties.</span></span>  <span data-ttu-id="31511-137">Under **Fabric-version** väljer du uppgraderingsläget **Automatiskt** så att Microsoft automatiskt uppdaterar den Fabric-kod som kör klustret.</span><span class="sxs-lookup"><span data-stu-id="31511-137">In **Fabric version**, select **Automatic** upgrade mode so that Microsoft automatically updates the version of the fabric code running the cluster.</span></span>  <span data-ttu-id="31511-138">Ställ in läget som **Manuellt** om du vill [välja en version som stöds](service-fabric-cluster-upgrade.md) och uppgradera till den.</span><span class="sxs-lookup"><span data-stu-id="31511-138">Set the mode to **Manual** if you want to [choose a supported version](service-fabric-cluster-upgrade.md) to upgrade to.</span></span> 

    ![Konfiguration av nodtyp][node-type-config]

    <span data-ttu-id="31511-140">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="31511-140">Select **OK**.</span></span>

7. <span data-ttu-id="31511-141">Fyll i formuläret **Säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="31511-141">Fill out the **Security** form.</span></span>  <span data-ttu-id="31511-142">I den här snabbstarten kan du välja **Ta bort skydd**.</span><span class="sxs-lookup"><span data-stu-id="31511-142">For this quick start select **Unsecure**.</span></span>  <span data-ttu-id="31511-143">I produktionsmiljöer bör du endast skapa skyddade kluster eftersom alla kan ansluta anonymt till oskyddade kluster och utföra hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="31511-143">It is highly recommended to create a secure cluster for production workloads, however, since anyone can anonymously connect to an unsecure cluster and perform management operations.</span></span>  

    <span data-ttu-id="31511-144">Certifikat används i Service Fabric till att autentisera och kryptera olika delar av ett kluster och de program som körs där.</span><span class="sxs-lookup"><span data-stu-id="31511-144">Certificates are used in Service Fabric to provide authentication and encryption to secure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="31511-145">Mer information om hur du använder certifikat i Service Fabric finns i [Service Fabric cluster security scenarios](service-fabric-cluster-security.md) (Säkerhet för Service Fabric-kluster).</span><span class="sxs-lookup"><span data-stu-id="31511-145">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md).</span></span>  <span data-ttu-id="31511-146">Om du vill aktivera autentisering av användare via Azure Active Directory eller ställa in certifikat för programsäkerhet kan du [skapa ett kluster från en Resource Manager-mall](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="31511-146">To enable user authentication using Azure Active Directory or to set up certificates for application security, [create a cluster from a Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span>

    <span data-ttu-id="31511-147">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="31511-147">Select **OK**.</span></span>

8. <span data-ttu-id="31511-148">Granska sammanfattningen.</span><span class="sxs-lookup"><span data-stu-id="31511-148">Review the summary.</span></span>  <span data-ttu-id="31511-149">Om du vill ladda ned en Resource Manager-mall som bygger på de inställningar du har angett väljer du **Ladda ned mall och parametrar**.</span><span class="sxs-lookup"><span data-stu-id="31511-149">If you'd like to download a Resource Manager template built from the settings you entered, select **Download template and parameters**.</span></span>  <span data-ttu-id="31511-150">Välj **Skapa** så att klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="31511-150">Select **Create** to create the cluster.</span></span>

    <span data-ttu-id="31511-151">Du kan se förloppet bland aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="31511-151">You can see the creation progress in the notifications.</span></span> <span data-ttu-id="31511-152">(Klicka på klockikonen nära statusfältet uppe till höger på skärmen.) Om du klickade på **Fäst på startsidan** när du skapade klustret ser du **Deploying Service Fabric Cluster** (Distribuerar Service Fabric-kluster) fäst på **startsidan**.</span><span class="sxs-lookup"><span data-stu-id="31511-152">(Click the "Bell" icon near the status bar at the upper right of your screen.) If you clicked **Pin to Startboard** while creating the cluster, you see **Deploying Service Fabric Cluster** pinned to the **Start** board.</span></span>

### <a name="view-cluster-status"></a><span data-ttu-id="31511-153">Visa klusterstatus</span><span class="sxs-lookup"><span data-stu-id="31511-153">View cluster status</span></span>
<span data-ttu-id="31511-154">När du har skapat ett kluster kan du granska det på bladet **Översikt** i portalen.</span><span class="sxs-lookup"><span data-stu-id="31511-154">Once your cluster is created, you can inspect your cluster in the **Overview** blade in the portal.</span></span> <span data-ttu-id="31511-155">Där kan du se information om klustret på instrumentpanelen, bland annat klustrets offentliga slutpunkter och en länk till Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="31511-155">You can now see the details of your cluster in the dashboard, including the cluster's public endpoint and a link to Service Fabric Explorer.</span></span>

![Klusterstatus][cluster-status]

### <a name="visualize-the-cluster-using-service-fabric-explorer"></a><span data-ttu-id="31511-157">Visualisera klustret med hjälp av Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="31511-157">Visualize the cluster using Service Fabric explorer</span></span>
<span data-ttu-id="31511-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) är ett bra verktyg för att visualisera klustret och hantera program.</span><span class="sxs-lookup"><span data-stu-id="31511-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="31511-159">Service Fabric Explorer är en tjänst som körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="31511-159">Service Fabric Explorer is a service that runs in the cluster.</span></span>  <span data-ttu-id="31511-160">Du kommer åt den i en webbläsare genom att klicka på länken **Service Fabric Explorer** på sidan **Översikt** för klustret i portalen.</span><span class="sxs-lookup"><span data-stu-id="31511-160">Access it using a web browser by clicking the **Service Fabric Explorer** link of the cluster **Overview** page in the portal.</span></span>  <span data-ttu-id="31511-161">Du kan också ange adressen direkt i webbläsaren: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span><span class="sxs-lookup"><span data-stu-id="31511-161">You can also enter the address directly into the browser: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span></span>

<span data-ttu-id="31511-162">Instrumentpanelen för klustret innehåller en översikt över klustret, inklusive en sammanfattning av program- och nodhälsan.</span><span class="sxs-lookup"><span data-stu-id="31511-162">The cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="31511-163">Nodvyn visar klustrets fysiska layout.</span><span class="sxs-lookup"><span data-stu-id="31511-163">The node view shows the physical layout of the cluster.</span></span> <span data-ttu-id="31511-164">För en viss nod kan du inspektera vilka program som har kod distribuerad på noden.</span><span class="sxs-lookup"><span data-stu-id="31511-164">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

### <a name="connect-to-the-cluster-using-powershell"></a><span data-ttu-id="31511-166">Ansluta till klustret med PowerShell</span><span class="sxs-lookup"><span data-stu-id="31511-166">Connect to the cluster using PowerShell</span></span>
<span data-ttu-id="31511-167">Kontrollera att klustret körs genom att ansluta med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="31511-167">Verify that the cluster is running by connecting using PowerShell.</span></span>  <span data-ttu-id="31511-168">Service Fabric PowerShell-modulen installeras med [Service Fabric SDK](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="31511-168">The ServiceFabric PowerShell module is installed with the [Service Fabric SDK](service-fabric-get-started.md).</span></span>  <span data-ttu-id="31511-169">Cmdleten [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) upprättar en anslutning till klustret.</span><span class="sxs-lookup"><span data-stu-id="31511-169">The [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection to the cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
<span data-ttu-id="31511-170">Fler exempel på hur du ansluter till ett kluster finns i [Ansluta till ett säkert kluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="31511-170">See [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting to a cluster.</span></span> <span data-ttu-id="31511-171">När du har anslutit till klustret använder du cmdleten [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) för att visa en lista över noder i klustret och statusinformation för varje nod.</span><span class="sxs-lookup"><span data-stu-id="31511-171">After connecting to the cluster, use the [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet to display a list of nodes in the cluster and status information for each node.</span></span> <span data-ttu-id="31511-172">**HealthState** bör vara *OK* för varje nod.</span><span class="sxs-lookup"><span data-stu-id="31511-172">**HealthState** should be *OK* for each node.</span></span>

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-the-cluster"></a><span data-ttu-id="31511-173">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="31511-173">Remove the cluster</span></span>
<span data-ttu-id="31511-174">Ett Service Fabric-kluster består av andra Azure-resurser förutom själva klusterresursen.</span><span class="sxs-lookup"><span data-stu-id="31511-174">A Service Fabric cluster is made up of other Azure resources in addition to the cluster resource itself.</span></span> <span data-ttu-id="31511-175">Så om du vill ta bort ett Service Fabric-kluster helt måste du också ta bort alla resurser det består av.</span><span class="sxs-lookup"><span data-stu-id="31511-175">So to completely delete a Service Fabric cluster you also need to delete all the resources it is made of.</span></span> <span data-ttu-id="31511-176">Det enklaste sättet att ta bort klustret och alla de resurser det använder är att ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="31511-176">The simplest way to delete the cluster and all the resources it consumes is to delete the resource group.</span></span> <span data-ttu-id="31511-177">Andra sätt att ta bort ett kluster eller borttagning av vissa (men inte alla) resurser i en resursgrupp beskrivs i [Ta bort ett kluster](service-fabric-cluster-delete.md)</span><span class="sxs-lookup"><span data-stu-id="31511-177">For other ways to delete a cluster or to delete some (but not all) the resources in a resource group, see [Delete a cluster](service-fabric-cluster-delete.md)</span></span>

<span data-ttu-id="31511-178">Ta bort en resursgrupp på Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="31511-178">Delete a resource group in the Azure portal:</span></span>
1. <span data-ttu-id="31511-179">Navigera till det Service Fabric-kluster du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="31511-179">Navigate to the Service Fabric cluster you want to delete.</span></span>
2. <span data-ttu-id="31511-180">Klicka på **Resursgrupp** på sidan med klusterinformation.</span><span class="sxs-lookup"><span data-stu-id="31511-180">Click the **Resource Group** name on the cluster essentials page.</span></span>
3. <span data-ttu-id="31511-181">På sidan **Resource Group Essentials** (Information om resursgrupp) klickar du på **Ta bort resursgrupp** och följer sedan anvisningarna för borttagning av resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="31511-181">In the **Resource Group Essentials** page, click **Delete resource group** and follow the instructions on that page to complete the deletion of the resource group.</span></span>
    <span data-ttu-id="31511-182">![Ta bort resursgruppen][cluster-delete]</span><span class="sxs-lookup"><span data-stu-id="31511-182">![Delete the resource group][cluster-delete]</span></span>


## <a name="use-azure-powershell-to-deploy-a-secure-cluster"></a><span data-ttu-id="31511-183">Använda Azure Powershell för att distribuera ett säkert kluster</span><span class="sxs-lookup"><span data-stu-id="31511-183">Use Azure Powershell to deploy a secure cluster</span></span>
1. <span data-ttu-id="31511-184">Ladda ned [Azure Powershell-modul version 4.0 eller senare](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) på datorn.</span><span class="sxs-lookup"><span data-stu-id="31511-184">Download the [Azure Powershell module version 4.0 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) on your machine.</span></span>

2. <span data-ttu-id="31511-185">Öppna ett Windows PowerShell-fönster och kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="31511-185">Open a Windows PowerShell window, Run the following command.</span></span> 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    <span data-ttu-id="31511-186">Du bör se utdata som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="31511-186">You should see an output similar to the following.</span></span>

    ![ps-list][ps-list]

3. <span data-ttu-id="31511-188">Logga in på Azure och välj den prenumeration som du vill skapa klustret på</span><span class="sxs-lookup"><span data-stu-id="31511-188">Login to Azure and Select the subscription to which you want to create the cluster</span></span>

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. <span data-ttu-id="31511-189">Kör följande kommande för att skapa ett säkert kluster.</span><span class="sxs-lookup"><span data-stu-id="31511-189">Run the following command to now create a secure cluster.</span></span> <span data-ttu-id="31511-190">Glöm inte att anpassa parametrarna.</span><span class="sxs-lookup"><span data-stu-id="31511-190">Do not forget to customize the parameters.</span></span> 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also the name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    <span data-ttu-id="31511-191">Det kan ta mellan 10 och 30 minuter att slutföra kommandot. När det har slutförts bör utdata se ut ungefär som nedan.</span><span class="sxs-lookup"><span data-stu-id="31511-191">The command can take anywhere from 10 minutes to 30 minutes to complete, at the end of it, you should get an output similar to the following.</span></span> <span data-ttu-id="31511-192">Utdata innehåller information om certifikatet, nyckelvalvet som certifikatet överfördes till och den lokala mapp som certifikatet kopieras till.</span><span class="sxs-lookup"><span data-stu-id="31511-192">The output has information about the certificate, the KeyVault where it was uploaded to, and the local folder where the certificate is copied.</span></span> 

    ![ps-out][ps-out]

5. <span data-ttu-id="31511-194">Kopiera alla utdata och spara dem i en textfil eftersom vi behöver hänvisa till dem.</span><span class="sxs-lookup"><span data-stu-id="31511-194">Copy the entire output and save to a text file as we need to refer to it.</span></span> <span data-ttu-id="31511-195">Anteckna följande information från utdata.</span><span class="sxs-lookup"><span data-stu-id="31511-195">Make a note of the following information from the output.</span></span> 

    - <span data-ttu-id="31511-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span><span class="sxs-lookup"><span data-stu-id="31511-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span></span>
    - <span data-ttu-id="31511-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span><span class="sxs-lookup"><span data-stu-id="31511-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span></span>
    - <span data-ttu-id="31511-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span><span class="sxs-lookup"><span data-stu-id="31511-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span></span>
    - <span data-ttu-id="31511-199">**ClientConnectionEndpointPort** : 19000</span><span class="sxs-lookup"><span data-stu-id="31511-199">**ClientConnectionEndpointPort** : 19000</span></span>

### <a name="install-the-certificate-on-your-local-machine"></a><span data-ttu-id="31511-200">Installera certifikatet på din lokala dator</span><span class="sxs-lookup"><span data-stu-id="31511-200">Install the certificate on your local machine</span></span>
  
<span data-ttu-id="31511-201">För att kunna ansluta till klustret måste du installera certifikatet i det personliga arkivet för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="31511-201">To connect to the cluster, you need to install the certificate into the Personal (My) store of the current user.</span></span> 

<span data-ttu-id="31511-202">Kör följande PowerShell-kommando</span><span class="sxs-lookup"><span data-stu-id="31511-202">Run the following PowerShell</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\the name of the cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

<span data-ttu-id="31511-203">Du är nu redo att ansluta till det säkra klustret.</span><span class="sxs-lookup"><span data-stu-id="31511-203">You are now ready to connect to your secure cluster.</span></span>

### <a name="connect-to-a-secure-cluster"></a><span data-ttu-id="31511-204">Ansluta till ett säkert kluster</span><span class="sxs-lookup"><span data-stu-id="31511-204">Connect to a secure cluster</span></span> 

<span data-ttu-id="31511-205">Kör följande PowerShell-kommando för att ansluta till ett säkert kluster.</span><span class="sxs-lookup"><span data-stu-id="31511-205">Run the following PowerShell command to connect to a secure cluster.</span></span> <span data-ttu-id="31511-206">Certifikatinformationen måste matcha certifikatet som användes för att konfigurera klustret.</span><span class="sxs-lookup"><span data-stu-id="31511-206">The certificate details must match a certificate that was used to set up the cluster.</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


<span data-ttu-id="31511-207">I följande exempel visas de ifyllda parametrarna:</span><span class="sxs-lookup"><span data-stu-id="31511-207">The following example shows the completed parameters:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="31511-208">Kör följande kommando för att kontrollera att du är ansluten och att klustret är felfritt.</span><span class="sxs-lookup"><span data-stu-id="31511-208">Run the following command to check that you are connected and the cluster is healthy.</span></span>

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="publish-your-apps-to-your-cluster-from-visual-studio"></a><span data-ttu-id="31511-209">Publicera dina appar till klustret från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="31511-209">Publish your apps to your cluster from Visual Studio</span></span>

<span data-ttu-id="31511-210">Nu när du har konfigurerat ett Azure-kluster kan du publicera dina program från Visual Studio till Azure enligt anvisningarna i dokumentet [Publicera till ett kluster](service-fabric-publish-app-remote-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="31511-210">Now that you have set up an Azure cluster, you can publish your applications to it from Visual Studio by following the [Publish to an cluster](service-fabric-publish-app-remote-cluster.md) document.</span></span> 

### <a name="remove-the-cluster"></a><span data-ttu-id="31511-211">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="31511-211">Remove the cluster</span></span>
<span data-ttu-id="31511-212">Ett kluster består av andra Azure-resurser förutom själva klusterresursen.</span><span class="sxs-lookup"><span data-stu-id="31511-212">A cluster is made up of other Azure resources in addition to the cluster resource itself.</span></span> <span data-ttu-id="31511-213">Det enklaste sättet att ta bort klustret och alla de resurser det använder är att ta bort resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="31511-213">The simplest way to delete the cluster and all the resources it consumes is to delete the resource group.</span></span> 

```powershell

Remove-AzureRmResourceGroup -Name $RGname -Force

```

## <a name="next-steps"></a><span data-ttu-id="31511-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31511-214">Next steps</span></span>
<span data-ttu-id="31511-215">Nu när du har konfigurerat ett utvecklingskluster provar du följande:</span><span class="sxs-lookup"><span data-stu-id="31511-215">Now that you have set up a development cluster, try the following:</span></span>
* <span data-ttu-id="31511-216">[Create a secure cluster in the portal](service-fabric-cluster-creation-via-portal.md) (Skapa ett säkert kluster i portalen)</span><span class="sxs-lookup"><span data-stu-id="31511-216">[Create a secure cluster in the portal](service-fabric-cluster-creation-via-portal.md)</span></span>
* <span data-ttu-id="31511-217">[Create a cluster from a template](service-fabric-cluster-creation-via-arm.md) (Skapa ett kluster från en mall)</span><span class="sxs-lookup"><span data-stu-id="31511-217">[Create a cluster from a template](service-fabric-cluster-creation-via-arm.md)</span></span> 
* [<span data-ttu-id="31511-218">Distribuera appar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="31511-218">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
