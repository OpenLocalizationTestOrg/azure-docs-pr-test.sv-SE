---
title: "Konfigurera ett fristående Azure Service Fabric-kluster | Microsoft Docs"
description: "Skapa ett fristående kluster för utveckling med tre noder som körs på samma dator. När du har slutfört den här konfigurationen kan du skapa ett kluster med flera datorer."
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
ms.openlocfilehash: 5c8f4c784eed7b64810a3dd1c36c043d22a66936
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a><span data-ttu-id="32833-104">Skapa ditt första fristående Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="32833-104">Create your first Service Fabric standalone cluster</span></span>
<span data-ttu-id="32833-105">Du kan skapa ett fristående Service Fabric-kluster på valfria virtuella datorer eller datorer som kör Windows Server 2012 R2 eller Windows Server 2016, lokalt eller i molnet.</span><span class="sxs-lookup"><span data-stu-id="32833-105">You can create a Service Fabric standalone cluster on any virtual machines or computers running Windows Server 2012 R2 or Windows Server 2016, on-premises or in the cloud.</span></span> <span data-ttu-id="32833-106">Den här snabbstarten beskriver hur du skapar ett fristående kluster för utveckling på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="32833-106">This quickstart helps you to create a development standalone cluster in just a few minutes.</span></span>  <span data-ttu-id="32833-107">När du är klar har du ett kluster med tre noder som körs på en enskild dator som du kan distribuera appar till.</span><span class="sxs-lookup"><span data-stu-id="32833-107">When you're finished, you have a three-node cluster running on a single computer that you can deploy apps to.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="32833-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="32833-108">Before you begin</span></span>
<span data-ttu-id="32833-109">Fabric Service tillhandahåller ett konfigurationspaket för att skapa fristående Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="32833-109">Service Fabric provides a setup package to create Service Fabric standalone clusters.</span></span>  <span data-ttu-id="32833-110">[Ladda ned konfigurationspaketet](http://go.microsoft.com/fwlink/?LinkId=730690).</span><span class="sxs-lookup"><span data-stu-id="32833-110">[Download the setup package](http://go.microsoft.com/fwlink/?LinkId=730690).</span></span>  <span data-ttu-id="32833-111">Packa upp installationspaketet i en mapp på datorn eller den virtuella datorn där du installerar utvecklingsklustret.</span><span class="sxs-lookup"><span data-stu-id="32833-111">Unzip the setup package to a folder on the computer or virtual machine where you are setting up the development cluster.</span></span>  <span data-ttu-id="32833-112">Innehållet i konfigurationspaketet beskrivs i detalj [här](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="32833-112">The contents of the setup package are described in detail [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="32833-113">Klusteradministratören som distribuerar och konfigurerar klustret måste ha administratörsbehörighet på datorn.</span><span class="sxs-lookup"><span data-stu-id="32833-113">The cluster administrator deploying and configuring the cluster must have administrator privileges on the computer.</span></span> <span data-ttu-id="32833-114">Du kan inte installera Service Fabric på en domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="32833-114">You cannot install Service Fabric on a domain controller.</span></span>

## <a name="validate-the-environment"></a><span data-ttu-id="32833-115">Validera miljön</span><span class="sxs-lookup"><span data-stu-id="32833-115">Validate the environment</span></span>
<span data-ttu-id="32833-116">Skriptet *TestConfiguration.ps1* i det fristående paketet används för att analysera bästa praxis och för att kontrollera om ett kluster kan distribueras i en viss miljö.</span><span class="sxs-lookup"><span data-stu-id="32833-116">The *TestConfiguration.ps1* script in the standalone package is used as a best practices analyzer to validate whether a cluster can be deployed on a given environment.</span></span> <span data-ttu-id="32833-117">[Distributionsförberedelser](service-fabric-cluster-standalone-deployment-preparation.md) visar krav och förutsättningar.</span><span class="sxs-lookup"><span data-stu-id="32833-117">[Deployment preparation](service-fabric-cluster-standalone-deployment-preparation.md) lists the pre-requisites and environment requirements.</span></span> <span data-ttu-id="32833-118">Kör skriptet för att kontrollera om du kan skapa utvecklingsklustret:</span><span class="sxs-lookup"><span data-stu-id="32833-118">Run the script to verify if you can create the development cluster:</span></span>

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-the-cluster"></a><span data-ttu-id="32833-119">Skapa klustret</span><span class="sxs-lookup"><span data-stu-id="32833-119">Create the cluster</span></span>
<span data-ttu-id="32833-120">Flera exempelkonfigurationsfiler för kluster installeras med konfigurationspaketet.</span><span class="sxs-lookup"><span data-stu-id="32833-120">Several sample cluster configuration files are installed with the setup package.</span></span> <span data-ttu-id="32833-121">*ClusterConfig.Unsecure.DevCluster.json* är den enklaste klusterkonfigurationen: ett oskyddat kluster med tre noder som körs på en enskild dator.</span><span class="sxs-lookup"><span data-stu-id="32833-121">*ClusterConfig.Unsecure.DevCluster.json* is the simplest cluster configuration: an unsecure, three-node cluster running on a single computer.</span></span>  <span data-ttu-id="32833-122">Andra konfigurationsfiler beskriver kluster med en eller flera datorer som skyddas med X.509-certifikat eller Windows-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="32833-122">Other config files describe single or multi-machine clusters secured with X.509 certificates or Windows security.</span></span>  <span data-ttu-id="32833-123">Du behöver inte ändra några av standardinställningarna i den här självstudien, men titta igenom konfigurationsfilen så att du bekantar dig med inställningarna.</span><span class="sxs-lookup"><span data-stu-id="32833-123">You don't need to modify any of the default config settings for this tutorial, but look through the config file and get familiar with the settings.</span></span>  <span data-ttu-id="32833-124">I avsnittet **Noder** beskrivs de tre noderna i klustret: namn, IP-adress, [nodtyp, feldomän och uppgraderingsdomän](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="32833-124">The **nodes** section describes the three nodes in the cluster: name, IP address, [node type, fault domain, and upgrade domain](service-fabric-cluster-manifest.md#nodes-on-the-cluster).</span></span>  <span data-ttu-id="32833-125">I avsnittet **Egenskaper** definieras [säkerhet, tillförlitlighetsnivå, diagnostiksamling och nodtyper](service-fabric-cluster-manifest.md#cluster-properties) för klustret.</span><span class="sxs-lookup"><span data-stu-id="32833-125">The **properties** section defines the [security, reliability level, diagnostics collection, and types of nodes](service-fabric-cluster-manifest.md#cluster-properties) for the cluster.</span></span>

<span data-ttu-id="32833-126">Det här klustret är inte säkert.</span><span class="sxs-lookup"><span data-stu-id="32833-126">This cluster is unsecure.</span></span>  <span data-ttu-id="32833-127">Vem som helst kan ansluta anonymt och utföra hanteringsåtgärder, så produktionskluster bör alltid skyddas med X.509-certifikat eller Windows-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="32833-127">Anyone can connect anonymously and perform management operations, so production clusters should always be secured using X.509 certificates or Windows security.</span></span>  <span data-ttu-id="32833-128">Säkerhetsfunktioner konfigureras bara när kluster skapas, och du kan inte aktivera någon säkerhet i efterhand.</span><span class="sxs-lookup"><span data-stu-id="32833-128">Security is only configured at cluster creation time and it is not possible to enable security after the cluster is created.</span></span>  <span data-ttu-id="32833-129">Mer information om klustersäkerheten med Service Fabric finns i avsnittet om hur du [skyddar ett kluster](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="32833-129">Read [Secure a cluster](service-fabric-cluster-security.md) to learn more about Service Fabric cluster security.</span></span>  

<span data-ttu-id="32833-130">Skapa utvecklingsklustret med tre noder genom att köra skriptet *CreateServiceFabricCluster.ps1* från en PowerShell-administratörssession:</span><span class="sxs-lookup"><span data-stu-id="32833-130">To create the three-node development cluster, run the *CreateServiceFabricCluster.ps1* script from an administrator PowerShell session:</span></span>

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="32833-131">Service Fabric-körningspaketet laddas ned automatiskt och installeras när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="32833-131">The Service Fabric runtime package is automatically downloaded and installed at time of cluster creation.</span></span>

## <a name="connect-to-the-cluster"></a><span data-ttu-id="32833-132">Anslut till klustret</span><span class="sxs-lookup"><span data-stu-id="32833-132">Connect to the cluster</span></span>
<span data-ttu-id="32833-133">Utvecklingsklustret med tre noder körs nu.</span><span class="sxs-lookup"><span data-stu-id="32833-133">Your three-node development cluster is now running.</span></span> <span data-ttu-id="32833-134">ServiceFabric PowerShell-modulen installeras med runtime.</span><span class="sxs-lookup"><span data-stu-id="32833-134">The ServiceFabric PowerShell module is installed with the runtime.</span></span>  <span data-ttu-id="32833-135">Du kan kontrollera att klustret körs från samma dator eller från en fjärrdator med Service Fabrics runtime.</span><span class="sxs-lookup"><span data-stu-id="32833-135">You can verify that the cluster is running from the same computer or from a remote computer with the Service Fabric runtime.</span></span>  <span data-ttu-id="32833-136">Cmdleten [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) upprättar en anslutning till klustret.</span><span class="sxs-lookup"><span data-stu-id="32833-136">The [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cmdlet establishes a connection to the cluster.</span></span>   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
<span data-ttu-id="32833-137">Fler exempel på hur du ansluter till ett kluster finns i [Ansluta till ett säkert kluster](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="32833-137">See [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) for other examples of connecting to a cluster.</span></span> <span data-ttu-id="32833-138">När du har anslutit till klustret använder du cmdleten [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) för att visa en lista över noder i klustret och statusinformation för varje nod.</span><span class="sxs-lookup"><span data-stu-id="32833-138">After connecting to the cluster, use the [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) cmdlet to display a list of nodes in the cluster and status information for each node.</span></span> <span data-ttu-id="32833-139">**HealthState** bör vara *OK* för varje nod.</span><span class="sxs-lookup"><span data-stu-id="32833-139">**HealthState** should be *OK* for each node.</span></span>

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-the-cluster-using-service-fabric-explorer"></a><span data-ttu-id="32833-140">Visualisera klustret med hjälp av Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="32833-140">Visualize the cluster using Service Fabric explorer</span></span>
<span data-ttu-id="32833-141">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) är ett bra verktyg för att visualisera klustret och hantera program.</span><span class="sxs-lookup"><span data-stu-id="32833-141">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a good tool for visualizing your cluster and managing applications.</span></span>  <span data-ttu-id="32833-142">Service Fabric Explorer är en tjänst som körs i klustret, som du kommer åt med en webbläsare genom att navigera till [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="32833-142">Service Fabric Explorer is a service that runs in the cluster, which you access using a browser by navigating to [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span> 

<span data-ttu-id="32833-143">Instrumentpanelen för klustret innehåller en översikt över klustret, inklusive en sammanfattning av program- och nodhälsan.</span><span class="sxs-lookup"><span data-stu-id="32833-143">The cluster dashboard provides an overview of your cluster, including a summary of application and node health.</span></span> <span data-ttu-id="32833-144">Nodvyn visar klustrets fysiska layout.</span><span class="sxs-lookup"><span data-stu-id="32833-144">The node view shows the physical layout of the cluster.</span></span> <span data-ttu-id="32833-145">För en viss nod kan du inspektera vilka program som har kod distribuerad på noden.</span><span class="sxs-lookup"><span data-stu-id="32833-145">For a given node, you can inspect which applications have code deployed on that node.</span></span>

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-the-cluster"></a><span data-ttu-id="32833-147">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="32833-147">Remove the cluster</span></span>
<span data-ttu-id="32833-148">Om du vill ta bort ett kluster kör du PowerShell-skriptet *RemoveServiceFabricCluster.ps1* från paketmappen och lägger till sökvägen till JSON-konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="32833-148">To remove a cluster, run the *RemoveServiceFabricCluster.ps1* PowerShell script from the package folder and pass in the path to the JSON configuration file.</span></span> <span data-ttu-id="32833-149">Om du vill kan du ange en plats för loggen över borttagningen.</span><span class="sxs-lookup"><span data-stu-id="32833-149">You can optionally specify a location for the log of the deletion.</span></span>

```powershell
# Removes Service Fabric cluster nodes from each computer in the configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

<span data-ttu-id="32833-150">Om du vill ta bort Service Fabrics runtime från datorn kör du följande PowerShell-skript från paketmappen.</span><span class="sxs-lookup"><span data-stu-id="32833-150">To remove the Service Fabric runtime from the computer, run the following PowerShell script from the package folder.</span></span>

```powershell
# Removes Service Fabric from the current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a><span data-ttu-id="32833-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32833-151">Next steps</span></span>
<span data-ttu-id="32833-152">Nu när du har konfigurerat ett fristående utvecklingskluster kan du läsa följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="32833-152">Now that you have set up a development standalone cluster, try the following articles:</span></span>
* <span data-ttu-id="32833-153">[Konfigurera ett fristående kluster med flera datorer](service-fabric-cluster-creation-for-windows-server.md) och aktivera säkerhet.</span><span class="sxs-lookup"><span data-stu-id="32833-153">[Set up a multi-machine standalone cluster](service-fabric-cluster-creation-for-windows-server.md) and enable security.</span></span>
* [<span data-ttu-id="32833-154">Distribuera appar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="32833-154">Deploy apps using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
