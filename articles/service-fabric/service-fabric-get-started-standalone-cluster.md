---
title: "aaaSet in ett fristående Azure Service Fabric-kluster | Microsoft Docs"
description: "Skapa ett fristående kluster för utveckling med tre noder körs på hello samma dator. När du har slutfört den här installationen kommer du att redo toocreate ett kluster för flera datorer."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/06/2017
ms.author: dekapur
ms.openlocfilehash: e4d0ea9fc3b8475160bd8ed19fd3716463791cc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a><span data-ttu-id="ba401-104">Skapa ditt första fristående Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="ba401-104">Create your first Service Fabric standalone cluster</span></span>
<span data-ttu-id="ba401-105">Du kan skapa en fristående Service Fabric-kluster på alla virtuella datorer eller datorer som kör Windows Server 2012 R2 eller Windows Server 2016, lokalt eller i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="ba401-105">You can create a Service Fabric standalone cluster on any virtual machines or computers running Windows Server 2012 R2 or Windows Server 2016, on-premises or in hello cloud.</span></span> <span data-ttu-id="ba401-106">Den här snabbstarten hjälper dig att toocreate ett development fristående kluster på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="ba401-106">This quickstart helps you toocreate a development standalone cluster in just a few minutes.</span></span>  <span data-ttu-id="ba401-107">När du är klar har du ett kluster med tre noder som körs på en enskild dator som du kan distribuera appar till.</span><span class="sxs-lookup"><span data-stu-id="ba401-107">When you're finished, you have a three-node cluster running on a single computer that you can deploy apps to.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ba401-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="ba401-108">Before you begin</span></span>
<span data-ttu-id="ba401-109">Service Fabric innehåller en inställning för paketet toocreate Service Fabric fristående kluster.</span><span class="sxs-lookup"><span data-stu-id="ba401-109">Service Fabric provides a setup package toocreate Service Fabric standalone clusters.</span></span>  <span data-ttu-id="ba401-110">[Hämta installationspaketet för hello](http://go.microsoft.com/fwlink/?LinkId=730690).</span><span class="sxs-lookup"><span data-stu-id="ba401-110">[Download hello setup package](http://go.microsoft.com/fwlink/?LinkId=730690).</span></span>  <span data-ttu-id="ba401-111">Packa upp hello installationsprogrammet tooa paketmapp på hello eller virtuell dator där du konfigurerar hello development klustret.</span><span class="sxs-lookup"><span data-stu-id="ba401-111">Unzip hello setup package tooa folder on hello computer or virtual machine where you are setting up hello development cluster.</span></span>  <span data-ttu-id="ba401-112">hello innehållet i hello installationspaketet beskrivs i detalj [här](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="ba401-112">hello contents of hello setup package are described in detail [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="ba401-113">hello Klusteradministratören distribuera och konfigurera hello klustret måste ha administratörsbehörighet på hello-dator.</span><span class="sxs-lookup"><span data-stu-id="ba401-113">hello cluster administrator deploying and configuring hello cluster must have administrator privileges on hello computer.</span></span> <span data-ttu-id="ba401-114">Du kan inte installera Service Fabric på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="ba401-114">You cannot install Service Fabric on a domain controller.</span></span>

## <a name="validate-hello-environment"></a><span data-ttu-id="ba401-115">Validera hello-miljö</span><span class="sxs-lookup"><span data-stu-id="ba401-115">Validate hello environment</span></span>
<span data-ttu-id="ba401-116">Hej *TestConfiguration.ps1* skript i hello fristående paketet används som en best practices analyzer toovalidate om ett kluster kan distribueras på en given miljö.</span><span class="sxs-lookup"><span data-stu-id="ba401-116">hello *TestConfiguration.ps1* script in hello standalone package is used as a best practices analyzer toovalidate whether a cluster can be deployed on a given environment.</span></span> <span data-ttu-id="ba401-117">[Förberedelse av distribution](service-fabric-cluster-standalone-deployment-preparation.md) visar hello miljökrav och förutsättningar.</span><span class="sxs-lookup"><span data-stu-id="ba401-117">[Deployment preparation](service-fabric-cluster-standalone-deployment-preparation.md) lists hello pre-requisites and environment requirements.</span></span> <span data-ttu-id="ba401-118">Kör hello skriptet tooverify om du kan skapa hello development kluster:</span><span class="sxs-lookup"><span data-stu-id="ba401-118">Run hello script tooverify if you can create hello development cluster:</span></span>

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-hello-cluster"></a><span data-ttu-id="ba401-119">Skapa hello-kluster</span><span class="sxs-lookup"><span data-stu-id="ba401-119">Create hello cluster</span></span>
<span data-ttu-id="ba401-120">Flera exempel klustret konfigurationsfiler installeras med hello installationspaketet.</span><span class="sxs-lookup"><span data-stu-id="ba401-120">Several sample cluster configuration files are installed with hello setup package.</span></span> <span data-ttu-id="ba401-121">*ClusterConfig.Unsecure.DevCluster.json* är hello enklaste klusterkonfigurationen: ett oskyddat, tre noder kluster som körs på en dator.</span><span class="sxs-lookup"><span data-stu-id="ba401-121">*ClusterConfig.Unsecure.DevCluster.json* is hello simplest cluster configuration: an unsecure, three-node cluster running on a single computer.</span></span>  <span data-ttu-id="ba401-122">Andra konfigurationsfiler beskriver kluster med en eller flera datorer som skyddas med X.509-certifikat eller Windows-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="ba401-122">Other config files describe single or multi-machine clusters secured with X.509 certificates or Windows security.</span></span>  <span data-ttu-id="ba401-123">Du inte behöver toomodify hello config standardinställningar för den här självstudiekursen, men titta igenom hello konfigurationsfilen och bekanta dig med hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="ba401-123">You don't need toomodify any of hello default config settings for this tutorial, but look through hello config file and get familiar with hello settings.</span></span>  <span data-ttu-id="ba401-124">Hej **noder** beskrivs hello tre noder i klustret hello: namn, IP-adress, [nodtypen, feldomän och uppgraderingsdomänen](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="ba401-124">hello **nodes** section describes hello three nodes in hello cluster: name, IP address, [node type, fault domain, and upgrade domain](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span></span>  <span data-ttu-id="ba401-125">Hej **egenskaper** avsnittet definierar hello [säkerhet, tillförlitlighet nivå, diagnostik samlingen och typer av noder](service-fabric-cluster-manifest.md#cluster-properties) för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="ba401-125">hello **properties** section defines hello [security, reliability level, diagnostics collection, and types of nodes](service-fabric-cluster-manifest.md#cluster-properties) for hello cluster.</span></span>

<span data-ttu-id="ba401-126">Det här klustret är inte säkert.</span><span class="sxs-lookup"><span data-stu-id="ba401-126">This cluster is unsecure.</span></span>  <span data-ttu-id="ba401-127">Vem som helst kan ansluta anonymt och utföra hanteringsåtgärder, så produktionskluster bör alltid skyddas med X.509-certifikat eller Windows-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="ba401-127">Anyone can connect anonymously and perform management operations, so production clusters should always be secured using X.509 certificates or Windows security.</span></span>  <span data-ttu-id="ba401-128">Säkerheten är bara konfigurerad vid tidpunkten för skapandet av klustret och den är inte möjligt tooenable säkerhet hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="ba401-128">Security is only configured at cluster creation time and it is not possible tooenable security after hello cluster is created.</span></span>  <span data-ttu-id="ba401-129">Läs [skydda ett kluster](service-fabric-cluster-security.md) toolearn mer om säkerhet för Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="ba401-129">Read [Secure a cluster](service-fabric-cluster-security.md) toolearn more about Service Fabric cluster security.</span></span>  

<span data-ttu-id="ba401-130">toocreate hello tre noder development klustret, kör hello *CreateServiceFabricCluster.ps1* skriptet från en PowerShell-session som administratör:</span><span class="sxs-lookup"><span data-stu-id="ba401-130">toocreate hello three-node development cluster, run hello *CreateServiceFabricCluster.ps1* script from an administrator PowerShell session:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="ba401-131">hello Service Fabric runtime-paketet automatiskt hämtas och installeras när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="ba401-131">hello Service Fabric runtime package is automatically downloaded and installed at time of cluster creation.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="ba401-132">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="ba401-132">Connect toohello cluster</span></span>
<span data-ttu-id="ba401-133">Utvecklingsklustret med tre noder körs nu.</span><span class="sxs-lookup"><span data-stu-id="ba401-133">Your three-node development cluster is now running.</span></span> <span data-ttu-id="ba401-134">Hej ServiceFabric PowerShell-modulen är installerad med hello körning.</span><span class="sxs-lookup"><span data-stu-id="ba401-134">hello ServiceFabric PowerShell module is installed with hello runtime.</span></span>  <span data-ttu-id="ba401-135">Du kan verifiera hello klustret körs från hello samma dator eller från en fjärrdator med hello Service Fabric runtime.</span><span class="sxs-lookup"><span data-stu-id="ba401-135">You can verify that hello cluster is running from hello same computer or from a remote computer with hello Service Fabric runtime.</span></span>  <span data-ttu-id="ba401-136">Hej [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet upprättar en anslutning toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="ba401-136">hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection toohello cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
<span data-ttu-id="ba401-137">Se [Anslut tooa säker klustret](service-fabric-connect-to-secure-cluster.md) andra exempel på den anslutande tooa klustret.</span><span class="sxs-lookup"><span data-stu-id="ba401-137">See [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting tooa cluster.</span></span> <span data-ttu-id="ba401-138">När du ansluter toohello kluster, använda hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay en lista över noderna i hello kluster och status för varje nod.</span><span class="sxs-lookup"><span data-stu-id="ba401-138">After connecting toohello cluster, use hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet toodisplay a list of nodes in hello cluster and status information for each node.</span></span> <span data-ttu-id="ba401-139">**HealthState** bör vara *OK* för varje nod.</span><span class="sxs-lookup"><span data-stu-id="ba401-139">**HealthState** should be *OK* for each node.</span></span>

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-hello-cluster-using-service-fabric-explorer"></a><span data-ttu-id="ba401-140">Visualisera hello-kluster med hjälp av Service Fabric explorer</span><span class="sxs-lookup"><span data-stu-id="ba401-140">Visualize hello cluster using Service Fabric explorer</span></span>
<span data-ttu-id="ba401-141">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) är ett bra verktyg för att visualisera klustret och hantera program.</span><span class="sxs-lookup"><span data-stu-id="ba401-141">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="ba401-142">Service Fabric Explorer är en tjänst som körs i hello klustret som du kommer åt med en webbläsare genom att navigera för[http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="ba401-142">Service Fabric Explorer is a service that runs in hello cluster, which you access using a browser by navigating too[http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> 

<span data-ttu-id="ba401-143">Hej klusterinstrumentpanel innehåller en översikt över klustret, inklusive en sammanfattning av program och noden hälsa.</span><span class="sxs-lookup"><span data-stu-id="ba401-143">hello cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="ba401-144">hello nod vyn visar hello fysiska struktur hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ba401-144">hello node view shows hello physical layout of hello cluster.</span></span> <span data-ttu-id="ba401-145">För en viss nod kan du inspektera vilka program som har kod distribuerad på noden.</span><span class="sxs-lookup"><span data-stu-id="ba401-145">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-hello-cluster"></a><span data-ttu-id="ba401-147">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="ba401-147">Remove hello cluster</span></span>
<span data-ttu-id="ba401-148">tooremove ett kluster som kör hello *RemoveServiceFabricCluster.ps1* PowerShell-skript från hello paketmappen och skicka hello sökvägen toohello JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="ba401-148">tooremove a cluster, run hello *RemoveServiceFabricCluster.ps1* PowerShell script from hello package folder and pass in hello path toohello JSON configuration file.</span></span> <span data-ttu-id="ba401-149">Du kan du ange en plats för hello logg hello borttagningen av.</span><span class="sxs-lookup"><span data-stu-id="ba401-149">You can optionally specify a location for hello log of hello deletion.</span></span>

```powershell
# Removes Service Fabric cluster nodes from each computer in hello configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

<span data-ttu-id="ba401-150">tooremove hello Service Fabric runtime från hello-dator, kör följande PowerShell-skript från hello paketmappen hello.</span><span class="sxs-lookup"><span data-stu-id="ba401-150">tooremove hello Service Fabric runtime from hello computer, run hello following PowerShell script from hello package folder.</span></span>

```powershell
# Removes Service Fabric from hello current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a><span data-ttu-id="ba401-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba401-151">Next steps</span></span>
<span data-ttu-id="ba401-152">Nu när du har skapat ett development fristående kluster, försök hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="ba401-152">Now that you have set up a development standalone cluster, try hello following articles:</span></span>
* <span data-ttu-id="ba401-153">[Konfigurera ett fristående kluster med flera datorer](service-fabric-cluster-creation-for-windows-server.md) och aktivera säkerhet.</span><span class="sxs-lookup"><span data-stu-id="ba401-153">[Set up a multi-machine standalone cluster](service-fabric-cluster-creation-for-windows-server.md) and enable security.</span></span>
* [<span data-ttu-id="ba401-154">Distribuera appar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="ba401-154">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
