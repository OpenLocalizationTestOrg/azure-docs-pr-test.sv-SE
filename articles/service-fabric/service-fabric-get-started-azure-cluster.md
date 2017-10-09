---
title: aaaSet upp ett Azure Service Fabric-kluster | Microsoft Docs
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
ms.openlocfilehash: 13c60e293d19d607bb41ee4859706508c219a833
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a><span data-ttu-id="3b7fa-103">Skapa ditt första Service Fabric-kluster i Azure</span><span class="sxs-lookup"><span data-stu-id="3b7fa-103">Create your first Service Fabric cluster on Azure</span></span>
<span data-ttu-id="3b7fa-104">Ett [Service Fabric-kluster](service-fabric-deploy-anywhere.md) är en nätverksansluten uppsättning virtuella eller fysiska datorer som dina mikrotjänster distribueras till och hanteras från.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-104">A [Service Fabric cluster](service-fabric-deploy-anywhere.md) is a network-connected set of virtual or physical machines into which your microservices are deployed and managed.</span></span> <span data-ttu-id="3b7fa-105">Den här snabbstarten hjälper dig att toocreate ett kluster med fem noder, körs på Windows- eller Linux, via hello [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) eller [Azure-portalen](http://portal.azure.com) i bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-105">This quickstart helps you toocreate a five-node cluster, running on either Windows or Linux, through hello [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) or [Azure portal](http://portal.azure.com) in just a few minutes.</span></span>  

<span data-ttu-id="3b7fa-106">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


## <a name="use-hello-azure-portal"></a><span data-ttu-id="3b7fa-107">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3b7fa-107">Use hello Azure portal</span></span>

<span data-ttu-id="3b7fa-108">Logga in toohello Azure-portalen på [http://portal.azure.com](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3b7fa-108">Log in toohello Azure portal at [http://portal.azure.com](http://portal.azure.com).</span></span>

### <a name="create-hello-cluster"></a><span data-ttu-id="3b7fa-109">Skapa hello-kluster</span><span class="sxs-lookup"><span data-stu-id="3b7fa-109">Create hello cluster</span></span>

1. <span data-ttu-id="3b7fa-110">Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-110">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>
2. <span data-ttu-id="3b7fa-111">Välj **Compute** från hello **ny** bladet och väljer sedan **Service Fabric-kluster** från hello **Compute** bladet.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-111">Select **Compute** from hello **New** blade and then select **Service Fabric Cluster** from hello **Compute** blade.</span></span>
3. <span data-ttu-id="3b7fa-112">Fyll i hello Service Fabric **grunderna** formuläret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-112">Fill out hello Service Fabric **Basics** form.</span></span> <span data-ttu-id="3b7fa-113">För **operativsystemet**väljer hello version av Windows eller Linux som du vill hello toorun för noder av klustret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-113">For **Operating system**, select hello version of Windows or Linux you want hello cluster nodes toorun.</span></span> <span data-ttu-id="3b7fa-114">hello-användarnamn och lösenord som anges här är används toolog i toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-114">hello user name and password entered here is used toolog in toohello virtual machine.</span></span> <span data-ttu-id="3b7fa-115">Skapa en ny för **Resursgrupp**.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-115">For **Resource group**, create a new one.</span></span> <span data-ttu-id="3b7fa-116">En resursgrupp är en logisk behållare där Azure-resurser skapas och hanteras gemensamt.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-116">A resource group is a logical container into which Azure resources are created and collectively managed.</span></span> <span data-ttu-id="3b7fa-117">När du är klar klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-117">When complete, click **OK**.</span></span>

    ![Utdata efter klusterinstallationen][cluster-setup-basics]

4. <span data-ttu-id="3b7fa-119">Fyll i hello **klusterkonfigurationen** formuläret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-119">Fill out hello **Cluster configuration** form.</span></span>  <span data-ttu-id="3b7fa-120">För **Antal nodtyper** anger du "1".</span><span class="sxs-lookup"><span data-stu-id="3b7fa-120">For **Node type count**, enter "1".</span></span>

5. <span data-ttu-id="3b7fa-121">Välj **nodtypen 1 (primär)** och fylla i hello **typen nodkonfiguration** formuläret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-121">Select **Node type 1 (Primary)** and fill out hello **Node type configuration** form.</span></span>  <span data-ttu-id="3b7fa-122">Ange ett typnamn för noden och ange hello [hållbarhetsnivån](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) för ”Brons”.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-122">Enter a node type name and set hello [Durability tier](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) too"Bronze."</span></span>  <span data-ttu-id="3b7fa-123">Välj en VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-123">Select a VM size.</span></span>

    <span data-ttu-id="3b7fa-124">Nodtyper definiera hello VM-storlek, antal virtuella datorer, anpassade slutpunkter och andra inställningar för hello virtuella datorer av den typen.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-124">Node types define hello VM size, number of VMs, custom endpoints, and other settings for hello VMs of that type.</span></span> <span data-ttu-id="3b7fa-125">Varje nodtyp definierats ställs in som en separat virtuell dator skaluppsättning som används toodeploy och hanterade virtuella datorer som en uppsättning.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-125">Each node type defined is set up as a separate virtual machine scale set, which is used toodeploy and managed virtual machines as a set.</span></span> <span data-ttu-id="3b7fa-126">Varje nodtyp kan skalas upp eller ned oberoende av de andra, ha olika portar öppna och ha olika kapacitet.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-126">Each node type can be scaled up or down independently, have different sets of ports open, and can have different capacity metrics.</span></span>  <span data-ttu-id="3b7fa-127">hello första eller primära nodtypen är där Service Fabric systemtjänster finns och måste ha fem eller fler virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-127">hello first, or primary, node type is where Service Fabric system services are hosted and must have five or more VMs.</span></span>

    <span data-ttu-id="3b7fa-128">Vid distribution till en produktionsmiljö är det viktigt med [kapacitetsplanering](service-fabric-cluster-capacity.md).</span><span class="sxs-lookup"><span data-stu-id="3b7fa-128">For any production deployment, [capacity planning](service-fabric-cluster-capacity.md) is an important step.</span></span>  <span data-ttu-id="3b7fa-129">I den här snabbstarten kör du däremot inga program, så välj VM-storleken *DS1_v2 Standard*.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-129">For this quick start, however, you aren't running applications so select a *DS1_v2 Standard* VM size.</span></span>  <span data-ttu-id="3b7fa-130">Välj ”Silver” för hello [tillförlitlighetsnivån](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) och en inledande virtuella skaluppsättning kapacitet på 5.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-130">Select "Silver" for hello [reliability tier](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) and an initial virtual machine scale set capacity of 5.</span></span>  

    <span data-ttu-id="3b7fa-131">Anpassade slutpunkter öppna portar i hello Azure belastningsutjämnare så att du kan ansluta med program som körs på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-131">Custom endpoints open up ports in hello Azure load balancer so that you can connect with applications running on hello cluster.</span></span>  <span data-ttu-id="3b7fa-132">Ange ”80, 8172” tooopen in portarna 80 och 8172.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-132">Enter "80, 8172" tooopen up ports 80 and 8172.</span></span>

    <span data-ttu-id="3b7fa-133">Sök inte hello **konfigurera avancerade inställningar** som används för att anpassa TCP/HTTP-slutpunkter för hantering, programmet portintervall [placeringsbegränsningar](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), och [kapacitet Egenskaper för](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="3b7fa-133">Do not check hello **Configure advanced settings** box, which is used for customizing TCP/HTTP management endpoints, application port ranges, [placement constraints](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), and [capacity properties](service-fabric-cluster-resource-manager-metrics.md).</span></span>    

    <span data-ttu-id="3b7fa-134">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-134">Select **OK**.</span></span>

6. <span data-ttu-id="3b7fa-135">I hello **klusterkonfigurationen** formuläret genom att ange **diagnostik** för**på**.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-135">In hello **Cluster configuration** form, set **Diagnostics** too**On**.</span></span>  <span data-ttu-id="3b7fa-136">För Snabbstart, behöver du inte tooenter alla [fabric inställningen](service-fabric-cluster-fabric-settings.md) egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-136">For this quickstart, you do not need tooenter any [fabric setting](service-fabric-cluster-fabric-settings.md) properties.</span></span>  <span data-ttu-id="3b7fa-137">I **Fabric-versionen**väljer **automatisk** Uppgraderingsläge så att Microsoft uppdaterar automatiskt hello version av hello fabric-kod som körs hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-137">In **Fabric version**, select **Automatic** upgrade mode so that Microsoft automatically updates hello version of hello fabric code running hello cluster.</span></span>  <span data-ttu-id="3b7fa-138">Ange hello-läge för**manuell** om du vill använda för[väljer en version som stöds](service-fabric-cluster-upgrade.md) tooupgrade till.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-138">Set hello mode too**Manual** if you want too[choose a supported version](service-fabric-cluster-upgrade.md) tooupgrade to.</span></span> 

    ![Konfiguration av nodtyp][node-type-config]

    <span data-ttu-id="3b7fa-140">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-140">Select **OK**.</span></span>

7. <span data-ttu-id="3b7fa-141">Fyll i hello **säkerhet** formuläret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-141">Fill out hello **Security** form.</span></span>  <span data-ttu-id="3b7fa-142">I den här snabbstarten kan du välja **Ta bort skydd**.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-142">For this quick start select **Unsecure**.</span></span>  <span data-ttu-id="3b7fa-143">Det är mycket rekommenderas toocreate ett säker kluster för produktionsarbetsbelastningar, men eftersom alla anonymt kan ansluta tooan oskyddade kluster och utföra hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-143">It is highly recommended toocreate a secure cluster for production workloads, however, since anyone can anonymously connect tooan unsecure cluster and perform management operations.</span></span>  

    <span data-ttu-id="3b7fa-144">Certifikat används i Service Fabric tooprovide autentisering och kryptering toosecure olika aspekter av ett kluster och dess program.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-144">Certificates are used in Service Fabric tooprovide authentication and encryption toosecure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="3b7fa-145">Mer information om hur du använder certifikat i Service Fabric finns i [Service Fabric cluster security scenarios](service-fabric-cluster-security.md) (Säkerhet för Service Fabric-kluster).</span><span class="sxs-lookup"><span data-stu-id="3b7fa-145">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md).</span></span>  <span data-ttu-id="3b7fa-146">tooenable användarautentisering med Azure Active Directory eller tooset in certifikat för programsäkerhet, [skapa ett kluster från en Resource Manager-mall](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="3b7fa-146">tooenable user authentication using Azure Active Directory or tooset up certificates for application security, [create a cluster from a Resource Manager template](service-fabric-cluster-creation-via-arm.md).</span></span>

    <span data-ttu-id="3b7fa-147">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-147">Select **OK**.</span></span>

8. <span data-ttu-id="3b7fa-148">Granska hello sammanfattning.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-148">Review hello summary.</span></span>  <span data-ttu-id="3b7fa-149">Om du vill att toodownload en Resource Manager-mall som bygger på hello inställningar som du angett, Välj **ladda ned mall och parametrar**.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-149">If you'd like toodownload a Resource Manager template built from hello settings you entered, select **Download template and parameters**.</span></span>  <span data-ttu-id="3b7fa-150">Välj **skapa** toocreate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-150">Select **Create** toocreate hello cluster.</span></span>

    <span data-ttu-id="3b7fa-151">Du kan se hello förlopp i hello meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-151">You can see hello creation progress in hello notifications.</span></span> <span data-ttu-id="3b7fa-152">(Klicka på hello ”” klockikonen nära hello hello övre högra hörnet på skärmen i statusfältet.) Om du klickade på **PIN-kod tooStartboard** när du skapar hello klustret måste du se **distribuerar Service Fabric-kluster** Fäst toohello **starta** kort.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-152">(Click hello "Bell" icon near hello status bar at hello upper right of your screen.) If you clicked **Pin tooStartboard** while creating hello cluster, you see **Deploying Service Fabric Cluster** pinned toohello **Start** board.</span></span>

### <a name="view-cluster-status"></a><span data-ttu-id="3b7fa-153">Visa klusterstatus</span><span class="sxs-lookup"><span data-stu-id="3b7fa-153">View cluster status</span></span>
<span data-ttu-id="3b7fa-154">När klustret har skapats kan du granska klustret i hello **översikt** bladet i hello portal.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-154">Once your cluster is created, you can inspect your cluster in hello **Overview** blade in hello portal.</span></span> <span data-ttu-id="3b7fa-155">Du kan nu se hello information på klustret i hello instrumentpanel, inklusive hello klustrets offentlig slutpunkt och en länk tooService Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-155">You can now see hello details of your cluster in hello dashboard, including hello cluster's public endpoint and a link tooService Fabric Explorer.</span></span>

![Klusterstatus][cluster-status]

### <a name="visualize-hello-cluster-using-service-fabric-explorer"></a><span data-ttu-id="3b7fa-157">Visualisera hello-kluster med hjälp av Service Fabric explorer</span><span class="sxs-lookup"><span data-stu-id="3b7fa-157">Visualize hello cluster using Service Fabric explorer</span></span>
<span data-ttu-id="3b7fa-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) är ett bra verktyg för att visualisera klustret och hantera program.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-158">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="3b7fa-159">Service Fabric Explorer är en tjänst som körs i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-159">Service Fabric Explorer is a service that runs in hello cluster.</span></span>  <span data-ttu-id="3b7fa-160">Åtkomst till den i en webbläsare genom att klicka på hello **Service Fabric Explorer** länk hello klustret **översikt** sidan hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-160">Access it using a web browser by clicking hello **Service Fabric Explorer** link of hello cluster **Overview** page in hello portal.</span></span>  <span data-ttu-id="3b7fa-161">Du kan också ange hello-adressen direkt i webbläsaren hello: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span><span class="sxs-lookup"><span data-stu-id="3b7fa-161">You can also enter hello address directly into hello browser: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)</span></span>

<span data-ttu-id="3b7fa-162">Hej klusterinstrumentpanel innehåller en översikt över klustret, inklusive en sammanfattning av program och noden hälsa.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-162">hello cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="3b7fa-163">hello nod vyn visar hello fysiska struktur hello klustret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-163">hello node view shows hello physical layout of hello cluster.</span></span> <span data-ttu-id="3b7fa-164">För en viss nod kan du inspektera vilka program som har kod distribuerad på noden.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-164">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

### <a name="connect-toohello-cluster-using-powershell"></a><span data-ttu-id="3b7fa-166">Ansluta toohello kluster med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="3b7fa-166">Connect toohello cluster using PowerShell</span></span>
<span data-ttu-id="3b7fa-167">Kontrollera att hello-kluster körs genom att ansluta med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-167">Verify that hello cluster is running by connecting using PowerShell.</span></span>  <span data-ttu-id="3b7fa-168">Hej ServiceFabric PowerShell-modulen är installerad med hello [Service Fabric SDK](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3b7fa-168">hello ServiceFabric PowerShell module is installed with hello [Service Fabric SDK](service-fabric-get-started.md).</span></span>  <span data-ttu-id="3b7fa-169">Hej [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet upprättar en anslutning toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-169">hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection toohello cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
<span data-ttu-id="3b7fa-170">Se [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md) andra exempel på den anslutande tooa klustret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-170">See [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting tooa cluster.</span></span> <span data-ttu-id="3b7fa-171">När du ansluter toohello kluster, använda hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay en lista över noderna i hello kluster och status för varje nod.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-171">After connecting toohello cluster, use hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay a list of nodes in hello cluster and status information for each node.</span></span> <span data-ttu-id="3b7fa-172">**HealthState** bör vara *OK* för varje nod.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-172">**HealthState** should be *OK* for each node.</span></span>

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

### <a name="remove-hello-cluster"></a><span data-ttu-id="3b7fa-173">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="3b7fa-173">Remove hello cluster</span></span>
<span data-ttu-id="3b7fa-174">Service Fabric-klustret består av andra Azure-resurser förutom toohello klusterresurs sig själv.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-174">A Service Fabric cluster is made up of other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="3b7fa-175">Så toocompletely ta bort Service Fabric-kluster behöver du också toodelete alla hello av resurser.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-175">So toocompletely delete a Service Fabric cluster you also need toodelete all hello resources it is made of.</span></span> <span data-ttu-id="3b7fa-176">hello enklaste sättet toodelete hello kluster och alla hello-resurser som den förbrukar är toodelete hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-176">hello simplest way toodelete hello cluster and all hello resources it consumes is toodelete hello resource group.</span></span> <span data-ttu-id="3b7fa-177">För andra sätt toodelete ett kluster eller toodelete vissa (men inte alla) hello resurser i en resursgrupp, se [tar bort ett kluster](service-fabric-cluster-delete.md)</span><span class="sxs-lookup"><span data-stu-id="3b7fa-177">For other ways toodelete a cluster or toodelete some (but not all) hello resources in a resource group, see [Delete a cluster](service-fabric-cluster-delete.md)</span></span>

<span data-ttu-id="3b7fa-178">Ta bort en resursgrupp i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="3b7fa-178">Delete a resource group in hello Azure portal:</span></span>
1. <span data-ttu-id="3b7fa-179">Navigera toohello Service Fabric-kluster som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-179">Navigate toohello Service Fabric cluster you want toodelete.</span></span>
2. <span data-ttu-id="3b7fa-180">Klicka på hello **resursgruppen** namn på hello klustret essentials sida.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-180">Click hello **Resource Group** name on hello cluster essentials page.</span></span>
3. <span data-ttu-id="3b7fa-181">I hello **resurs grupp Essentials** klickar du på **ta bort resursgruppen** och följer instruktionerna för hello på sidan toocomplete hello borttagningen av hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-181">In hello **Resource Group Essentials** page, click **Delete resource group** and follow hello instructions on that page toocomplete hello deletion of hello resource group.</span></span>
    <span data-ttu-id="3b7fa-182">![Ta bort hello resursgruppen][cluster-delete]</span><span class="sxs-lookup"><span data-stu-id="3b7fa-182">![Delete hello resource group][cluster-delete]</span></span>


## <a name="use-azure-powershell-toodeploy-a-secure-cluster"></a><span data-ttu-id="3b7fa-183">Använda Azure Powershell toodeploy säker kluster</span><span class="sxs-lookup"><span data-stu-id="3b7fa-183">Use Azure Powershell toodeploy a secure cluster</span></span>
1. <span data-ttu-id="3b7fa-184">Hämta hello [Azure Powershell Modulversion 4.0 eller högre](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) på din dator.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-184">Download hello [Azure Powershell module version 4.0 or higher](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) on your machine.</span></span>

2. <span data-ttu-id="3b7fa-185">Öppna Windows PowerShell-fönstret, kör hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-185">Open a Windows PowerShell window, Run hello following command.</span></span> 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    <span data-ttu-id="3b7fa-186">Du bör se en liknande toohello följande i utdata.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-186">You should see an output similar toohello following.</span></span>

    ![ps-list][ps-list]

3. <span data-ttu-id="3b7fa-188">Inloggningen tooAzure och välj hello prenumeration toowhich som du vill toocreate hello kluster</span><span class="sxs-lookup"><span data-stu-id="3b7fa-188">Login tooAzure and Select hello subscription toowhich you want toocreate hello cluster</span></span>

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. <span data-ttu-id="3b7fa-189">Kör hello efter kommandot toonow skapa en säker kluster.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-189">Run hello following command toonow create a secure cluster.</span></span> <span data-ttu-id="3b7fa-190">Glöm inte toocustomize hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-190">Do not forget toocustomize hello parameters.</span></span> 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also hello name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    <span data-ttu-id="3b7fa-191">hello-kommandot kan ta allt från 10 minuter too30 minuter toocomplete, hello slutet av det, du bör få en liknande toohello följande i utdata.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-191">hello command can take anywhere from 10 minutes too30 minutes toocomplete, at hello end of it, you should get an output similar toohello following.</span></span> <span data-ttu-id="3b7fa-192">hello utdata innehåller information om hello certifikat, hello KeyVault där paketet har överförts till, och hello lokala mappen där hello certifikat kopieras.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-192">hello output has information about hello certificate, hello KeyVault where it was uploaded to, and hello local folder where hello certificate is copied.</span></span> 

    ![ps-out][ps-out]

5. <span data-ttu-id="3b7fa-194">Kopiera hela hello-utdata och spara tooa textfil som vi behöver toorefer tooit.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-194">Copy hello entire output and save tooa text file as we need toorefer tooit.</span></span> <span data-ttu-id="3b7fa-195">Anteckna hello följande information från hello utdata.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-195">Make a note of hello following information from hello output.</span></span> 

    - <span data-ttu-id="3b7fa-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span><span class="sxs-lookup"><span data-stu-id="3b7fa-196">**CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx</span></span>
    - <span data-ttu-id="3b7fa-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span><span class="sxs-lookup"><span data-stu-id="3b7fa-197">**CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10</span></span>
    - <span data-ttu-id="3b7fa-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span><span class="sxs-lookup"><span data-stu-id="3b7fa-198">**ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080</span></span>
    - <span data-ttu-id="3b7fa-199">**ClientConnectionEndpointPort** : 19000</span><span class="sxs-lookup"><span data-stu-id="3b7fa-199">**ClientConnectionEndpointPort** : 19000</span></span>

### <a name="install-hello-certificate-on-your-local-machine"></a><span data-ttu-id="3b7fa-200">Installera hello certifikat på den lokala datorn</span><span class="sxs-lookup"><span data-stu-id="3b7fa-200">Install hello certificate on your local machine</span></span>
  
<span data-ttu-id="3b7fa-201">tooconnect toohello klustret, behöver du tooinstall hello certifikat till hello Personal (min) arkivet för hello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-201">tooconnect toohello cluster, you need tooinstall hello certificate into hello Personal (My) store of hello current user.</span></span> 

<span data-ttu-id="3b7fa-202">Kör följande PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="3b7fa-202">Run hello following PowerShell</span></span>

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\hello name of hello cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

<span data-ttu-id="3b7fa-203">Du är nu redo tooconnect tooyour säker klustret.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-203">You are now ready tooconnect tooyour secure cluster.</span></span>

### <a name="connect-tooa-secure-cluster"></a><span data-ttu-id="3b7fa-204">Ansluta tooa säker kluster</span><span class="sxs-lookup"><span data-stu-id="3b7fa-204">Connect tooa secure cluster</span></span> 

<span data-ttu-id="3b7fa-205">Kör följande PowerShell-kommandot tooconnect tooa säker klustret hello.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-205">Run hello following PowerShell command tooconnect tooa secure cluster.</span></span> <span data-ttu-id="3b7fa-206">information om hello certifikat måste matcha ett certifikat som har använt tooset in hello kluster.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-206">hello certificate details must match a certificate that was used tooset up hello cluster.</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


<span data-ttu-id="3b7fa-207">följande exempel visar hello hello slutförts parametrar:</span><span class="sxs-lookup"><span data-stu-id="3b7fa-207">hello following example shows hello completed parameters:</span></span> 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

<span data-ttu-id="3b7fa-208">Kör hello efter kommandot toocheck att du är ansluten och hello klustret är felfri.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-208">Run hello following command toocheck that you are connected and hello cluster is healthy.</span></span>

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="publish-your-apps-tooyour-cluster-from-visual-studio"></a><span data-ttu-id="3b7fa-209">Publicera appar tooyour klustret från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b7fa-209">Publish your apps tooyour cluster from Visual Studio</span></span>

<span data-ttu-id="3b7fa-210">Nu när du har skapat ett Azure-kluster, kan du publicera dina program tooit från Visual Studio genom följande hello [publicera tooan klustret](service-fabric-publish-app-remote-cluster.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-210">Now that you have set up an Azure cluster, you can publish your applications tooit from Visual Studio by following hello [Publish tooan cluster](service-fabric-publish-app-remote-cluster.md) document.</span></span> 

### <a name="remove-hello-cluster"></a><span data-ttu-id="3b7fa-211">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="3b7fa-211">Remove hello cluster</span></span>
<span data-ttu-id="3b7fa-212">Ett kluster består av andra Azure-resurser förutom toohello klusterresurs sig själv.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-212">A cluster is made up of other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="3b7fa-213">hello enklaste sättet toodelete hello kluster och alla hello-resurser som den förbrukar är toodelete hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="3b7fa-213">hello simplest way toodelete hello cluster and all hello resources it consumes is toodelete hello resource group.</span></span> 

```powershell

Remove-AzureRmResourceGroup -Name $RGname -Force

```

## <a name="next-steps"></a><span data-ttu-id="3b7fa-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b7fa-214">Next steps</span></span>
<span data-ttu-id="3b7fa-215">Nu när du har skapat ett kluster för utveckling, försök hello följande:</span><span class="sxs-lookup"><span data-stu-id="3b7fa-215">Now that you have set up a development cluster, try hello following:</span></span>
* [<span data-ttu-id="3b7fa-216">Skapa en säker kluster i hello-portalen</span><span class="sxs-lookup"><span data-stu-id="3b7fa-216">Create a secure cluster in hello portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* <span data-ttu-id="3b7fa-217">[Create a cluster from a template](service-fabric-cluster-creation-via-arm.md) (Skapa ett kluster från en mall)</span><span class="sxs-lookup"><span data-stu-id="3b7fa-217">[Create a cluster from a template](service-fabric-cluster-creation-via-arm.md)</span></span> 
* [<span data-ttu-id="3b7fa-218">Distribuera appar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="3b7fa-218">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
