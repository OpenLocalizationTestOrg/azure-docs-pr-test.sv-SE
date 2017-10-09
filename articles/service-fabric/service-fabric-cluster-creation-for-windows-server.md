---
title: "aaaCreate ett fristående Azure Service Fabric-kluster | Microsoft Docs"
description: "Skapa ett Azure Service Fabric-kluster på en dator (fysiska eller virtuella) med Windows Server, om den är lokalt eller i något moln."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik;dekapur
ms.openlocfilehash: 444970816290a0448d88a8b2082c75eb7a64cb44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a><span data-ttu-id="3e7e5-103">Skapa ett fristående kluster som körs på Windows Server</span><span class="sxs-lookup"><span data-stu-id="3e7e5-103">Create a standalone cluster running on Windows Server</span></span>
<span data-ttu-id="3e7e5-104">Du kan använda Azure Service Fabric toocreate Service Fabric-kluster på alla virtuella datorer eller datorer som kör Windows Server.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-104">You can use Azure Service Fabric toocreate Service Fabric clusters on any virtual machines or computers running Windows Server.</span></span> <span data-ttu-id="3e7e5-105">Det innebär att distribuera och köra Service Fabric-program i en miljö som innehåller en uppsättning sammankopplade Windows Server-datorer, att den lokala eller med en molntjänstleverantör för.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-105">This means you can deploy and run Service Fabric applications in any environment that contains a set of interconnected Windows Server computers, be it on premises or with any cloud provider.</span></span> <span data-ttu-id="3e7e5-106">Service Fabric ger en installationsprogrammet paketet toocreate Service Fabric-kluster kallas hello fristående installationspaketet för Windows Server.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-106">Service Fabric provides a setup package toocreate Service Fabric clusters called hello standalone Windows Server package.</span></span>

<span data-ttu-id="3e7e5-107">Den här artikeln vägleder dig genom hello steg för att skapa en fristående Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-107">This article walks you through hello steps for creating a Service Fabric standalone cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="3e7e5-108">Det här fristående Windows Server-paketet är kommersiellt tillgängliga och kan användas för Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-108">This standalone Windows Server package is commercially available and may be used for production deployments.</span></span> <span data-ttu-id="3e7e5-109">Det här paketet kan innehålla nya Service Fabric-funktioner som finns i ”förhandsversion”.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-109">This package may contain new Service Fabric features that are in "Preview".</span></span> <span data-ttu-id="3e7e5-110">Rulla nedåt för ”[Förhandsgranska funktioner som ingår i det här paketet](#previewfeatures_anchor)”.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-110">Scroll down too"[Preview features included in this package](#previewfeatures_anchor)."</span></span> <span data-ttu-id="3e7e5-111">avsnittet för hello lista av hello förhandsgranskningsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-111">section for hello list of hello preview features.</span></span> <span data-ttu-id="3e7e5-112">Du kan [hämta en kopia av hello EULA](http://go.microsoft.com/fwlink/?LinkID=733084) nu.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-112">You can [download a copy of hello EULA](http://go.microsoft.com/fwlink/?LinkID=733084) now.</span></span>
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-hello-service-fabric-for-windows-server-package"></a><span data-ttu-id="3e7e5-113">Få support för hello Service Fabric för Windows Server-paket</span><span class="sxs-lookup"><span data-stu-id="3e7e5-113">Get support for hello Service Fabric for Windows Server package</span></span>
* <span data-ttu-id="3e7e5-114">Be hello community om hello Service Fabric fristående paketet för Windows Server i hello [Azure Service Fabric-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-114">Ask hello community about hello Service Fabric standalone package for Windows Server in hello [Azure Service Fabric forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span></span>
* <span data-ttu-id="3e7e5-115">Skapa ett ärende för [Professional stöd för Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-115">Open a ticket for [Professional Support for Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span></span>  <span data-ttu-id="3e7e5-116">Mer information om Professional Support från Microsoft [här](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-116">Learn more about Professional Support from Microsoft [here](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span></span>
* <span data-ttu-id="3e7e5-117">Du kan också få stöd för det här paketet som en del av [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-117">You can also get support for this package as a part of [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).</span></span>
* <span data-ttu-id="3e7e5-118">Mer information, se [Azure Service Fabric supportalternativ](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-118">For more details, please see [Azure Service Fabric support options](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span></span>
* <span data-ttu-id="3e7e5-119">toocollect loggar för support, kör hello [logginsamlaren för Service Fabric fristående](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-119">toocollect logs for support purposes, run hello [Service Fabric Standalone Log collector](service-fabric-cluster-standalone-package-contents.md).</span></span>

<a id="downloadpackage"></a>

## <a name="download-hello-service-fabric-for-windows-server-package"></a><span data-ttu-id="3e7e5-120">Hämta hello Service Fabric för Windows Server</span><span class="sxs-lookup"><span data-stu-id="3e7e5-120">Download hello Service Fabric for Windows Server package</span></span>
<span data-ttu-id="3e7e5-121">toocreate hello kluster, Använd hello Service Fabric för Windows Server-paket (Windows Server 2012 R2 och senare) finns här:</span><span class="sxs-lookup"><span data-stu-id="3e7e5-121">toocreate hello cluster, use hello Service Fabric for Windows Server package (Windows Server 2012 R2 and newer) found here:</span></span> <br><span data-ttu-id="3e7e5-122">
[Hämta länk - Service Fabric fristående paket - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)</span><span class="sxs-lookup"><span data-stu-id="3e7e5-122">
[Download Link - Service Fabric Standalone Package - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)</span></span>

<span data-ttu-id="3e7e5-123">Hitta information på innehållet i paketet hello [här](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-123">Find details on contents of hello package [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="3e7e5-124">hello Service Fabric runtime-paketet hämtas automatiskt när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-124">hello Service Fabric runtime package is automatically downloaded at time of cluster creation.</span></span> <span data-ttu-id="3e7e5-125">Om hur du distribuerar från en dator inte ansluten toohello internet, ladda ned hello runtime-paketet out of band härifrån:</span><span class="sxs-lookup"><span data-stu-id="3e7e5-125">If deploying from a machine not connected toohello internet, please download hello runtime package out of band from here:</span></span> <br><span data-ttu-id="3e7e5-126">
[Hämta länk - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)</span><span class="sxs-lookup"><span data-stu-id="3e7e5-126">
[Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)</span></span>

<span data-ttu-id="3e7e5-127">Hitta fristående klusterkonfigurationen exempel på:</span><span class="sxs-lookup"><span data-stu-id="3e7e5-127">Find Standalone Cluster Configuration samples at:</span></span> <br><span data-ttu-id="3e7e5-128">
[Exempel på fristående klusterkonfigurationen](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span><span class="sxs-lookup"><span data-stu-id="3e7e5-128">
[Standalone Cluster Configuration Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span></span>

<a id="createcluster"></a>

## <a name="create-hello-cluster"></a><span data-ttu-id="3e7e5-129">Skapa hello-kluster</span><span class="sxs-lookup"><span data-stu-id="3e7e5-129">Create hello cluster</span></span>
<span data-ttu-id="3e7e5-130">Service Fabric kan vara distribuerade tooa utveckling av en dator med hjälp av hello *ClusterConfig.Unsecure.DevCluster.json* -fil som ingår i [exempel](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-130">Service Fabric can be deployed tooa one-machine development cluster by using hello *ClusterConfig.Unsecure.DevCluster.json* file included in [Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span></span>

<span data-ttu-id="3e7e5-131">Packa upp hello fristående paketet tooyour datorn, kopiera hello exempel config-fil toohello lokala datorn och sedan kör hello *CreateServiceFabricCluster.ps1* skript via en administratör PowerShell-session från hello fristående paketmappen:</span><span class="sxs-lookup"><span data-stu-id="3e7e5-131">Unpack hello standalone package tooyour machine, copy hello sample config file toohello local machine, then run hello *CreateServiceFabricCluster.ps1* script through an administrator PowerShell session, from hello standalone package folder:</span></span>
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a><span data-ttu-id="3e7e5-132">Steg 1A: skapa ett oskyddat lokal utveckling-kluster</span><span class="sxs-lookup"><span data-stu-id="3e7e5-132">Step 1A: Create an unsecured local development cluster</span></span>
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="3e7e5-133">Se hello Miljökonfiguration avsnitt i [planera och förbereda distributionen av klustret](service-fabric-cluster-standalone-deployment-preparation.md) för felsökningsinformation.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-133">See hello Environment Setup section at [Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md) for troubleshooting details.</span></span>

<span data-ttu-id="3e7e5-134">Om du är klar utvecklingsscenarier som körs, kan du ta bort hello Service Fabric-klustret hello datorn genom att referera toosteps i avsnittet ”[tar bort ett kluster](#removecluster_anchor)”.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-134">If you're finished running development scenarios, you can remove hello Service Fabric cluster from hello machine by referring toosteps in section "[Remove a cluster](#removecluster_anchor)".</span></span> 

### <a name="step-1b-create-a-multi-machine-cluster"></a><span data-ttu-id="3e7e5-135">Steg 1B: skapa ett kluster för flera datorer</span><span class="sxs-lookup"><span data-stu-id="3e7e5-135">Step 1B: Create a multi-machine cluster</span></span>
<span data-ttu-id="3e7e5-136">När du har gått igenom hello planering och förberedelser detaljerad på hello nedanstående länk, är du redo toocreate produktion klustret med klustret konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-136">After you have gone through hello planning and preparation steps detailed at hello below link, you are ready toocreate your production cluster using your cluster configuration file.</span></span> <br><span data-ttu-id="3e7e5-137">
[Planera och förbereda distributionen av kluster](service-fabric-cluster-standalone-deployment-preparation.md)</span><span class="sxs-lookup"><span data-stu-id="3e7e5-137">
[Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md)</span></span>

1. <span data-ttu-id="3e7e5-138">Verifiera hello-konfigurationsfilen som du har skrivit genom att köra hello *TestConfiguration.ps1* skriptet från hello fristående paketmappen:</span><span class="sxs-lookup"><span data-stu-id="3e7e5-138">Validate hello configuration file you have written by running hello *TestConfiguration.ps1* script from hello standalone package folder:</span></span>  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    <span data-ttu-id="3e7e5-139">Du bör se utdata som nedan.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-139">You should see output like below.</span></span> <span data-ttu-id="3e7e5-140">Om hello nedre fältet ”godkänd” returneras som ”True”, hälsokontroller har gått och hello klustret verkar toobe distribuerbara baserat på inkommande hello-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-140">If hello bottom field "Passed" is returned as "True", sanity checks have passed and hello cluster looks toobe deployable based on hello input configuration.</span></span>

    ```
    Trace folder already exists. Traces will be written tooexisting trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
    Running Best Practices Analyzer...
    Best Practices Analyzer completed successfully.
    
    LocalAdminPrivilege        : True
    IsJsonValid                : True
    IsCabValid                 : True
    RequiredPortsOpen          : True
    RemoteRegistryAvailable    : True
    FirewallAvailable          : True
    RpcCheckPassed             : True
    NoConflictingInstallations : True
    FabricInstallable          : True
    Passed                     : True
    ```

2. <span data-ttu-id="3e7e5-141">Skapa hello kluster: kör hello *CreateServiceFabricCluster.ps1* skriptet toodeploy hello Service Fabric-kluster på varje dator i hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-141">Create hello cluster:  Run hello *CreateServiceFabricCluster.ps1* script toodeploy hello Service Fabric cluster across each machine in hello configuration.</span></span> 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> <span data-ttu-id="3e7e5-142">Distribution spårningar skrivs toohello VM/datorn där du körde hello CreateServiceFabricCluster.ps1 PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-142">Deployment traces are written toohello VM/machine on which you ran hello CreateServiceFabricCluster.ps1 PowerShell script.</span></span> <span data-ttu-id="3e7e5-143">Dessa återfinns i hello undermapp DeploymentTraces, i hello directory från vilka hello skriptet kördes.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-143">These can be found in hello subfolder DeploymentTraces, based in hello directory from which hello script was run.</span></span> <span data-ttu-id="3e7e5-144">toosee om Service Fabric har distribuerats korrekt tooa datorn, hitta hello installerat filer i hello FabricDataRoot directory, enligt anvisningarna i konfigurationsfilen för hello klustret FabricSettings avsnitt (som standard c:\ProgramData\SF).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-144">toosee if Service Fabric was deployed correctly tooa machine, find hello installed files in hello FabricDataRoot directory, as detailed in hello cluster configuration file FabricSettings section (by default c:\ProgramData\SF).</span></span> <span data-ttu-id="3e7e5-145">FabricHost.exe och Fabric.exe processer kan också ses körs i Aktivitetshanteraren.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-145">As well, FabricHost.exe and Fabric.exe processes can be seen running in Task Manager.</span></span>
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a><span data-ttu-id="3e7e5-146">Steg 1C: skapa ett kluster som offline (internet frånkopplad)</span><span class="sxs-lookup"><span data-stu-id="3e7e5-146">Step 1C: Create an offline (internet-disconnected) cluster</span></span>
<span data-ttu-id="3e7e5-147">hello Service Fabric runtime-paketet hämtas automatiskt när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-147">hello Service Fabric runtime package is automatically downloaded at cluster creation.</span></span> <span data-ttu-id="3e7e5-148">Distribuera ett kluster toomachines inte när anslutna toohello internet, behöver du toodownload hello Service Fabric runtime paketet separat och ange hello sökvägen tooit när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-148">When deploying a cluster toomachines not connected toohello internet, you will need toodownload hello Service Fabric runtime package separately, and provide hello path tooit at cluster creation.</span></span>
<span data-ttu-id="3e7e5-149">hello runtime-paketet kan laddas ned separat, från en annan dator ansluten toohello internet, vid [Hämta länk - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-149">hello runtime package can be downloaded separately, from another machine connected toohello internet, at [Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span></span> <span data-ttu-id="3e7e5-150">Kopiera hello runtime paketet toowhere du distribuerar hello offline kluster från och skapa hello kluster genom att köra `CreateServiceFabricCluster.ps1` med hello `-FabricRuntimePackagePath` parametern ingår, enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="3e7e5-150">Copy hello runtime package toowhere you are deploying hello offline cluster from, and create hello cluster by running `CreateServiceFabricCluster.ps1` with hello `-FabricRuntimePackagePath` parameter included, as shown below:</span></span> 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
<span data-ttu-id="3e7e5-151">där `.\ClusterConfig.json` och `.\MicrosoftAzureServiceFabric.cab` är hello sökvägar toohello klusterkonfigurationen respektive hello runtime CAB-fil.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-151">where `.\ClusterConfig.json` and `.\MicrosoftAzureServiceFabric.cab` are hello paths toohello cluster configuration and hello runtime .cab file respectively.</span></span>


### <a name="step-2-connect-toohello-cluster"></a><span data-ttu-id="3e7e5-152">Steg 2: Anslut toohello kluster</span><span class="sxs-lookup"><span data-stu-id="3e7e5-152">Step 2: Connect toohello cluster</span></span>
<span data-ttu-id="3e7e5-153">tooconnect tooa säker kluster, se [Service fabric ansluta toosecure klustret](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-153">tooconnect tooa secure cluster, see [Service fabric connect toosecure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

<span data-ttu-id="3e7e5-154">tooconnect tooan oskyddade kluster, kör hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="3e7e5-154">tooconnect tooan unsecure cluster, run hello following PowerShell command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
<span data-ttu-id="3e7e5-155">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3e7e5-155">Example:</span></span>
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a><span data-ttu-id="3e7e5-156">Steg 3: Ta fram Service Fabric Explorer</span><span class="sxs-lookup"><span data-stu-id="3e7e5-156">Step 3: Bring up Service Fabric Explorer</span></span>
<span data-ttu-id="3e7e5-157">Nu kan du ansluta toohello klustret med Service Fabric Explorer antingen direkt från en av hello datorerna http://localhost:19080/Explorer/index.html eller via fjärranslutning med http://<*IPAddressofaMachine*>: 19080 / Explorer/index.HTML.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-157">Now you can connect toohello cluster with Service Fabric Explorer either directly from one of hello machines with http://localhost:19080/Explorer/index.html or remotely with http://<*IPAddressofaMachine*>:19080/Explorer/index.html.</span></span>

## <a name="add-and-remove-nodes"></a><span data-ttu-id="3e7e5-158">Lägga till och ta bort noder</span><span class="sxs-lookup"><span data-stu-id="3e7e5-158">Add and remove nodes</span></span>
<span data-ttu-id="3e7e5-159">Du kan lägga till eller ta bort noder tooyour fristående Service Fabric-klustret när ditt företag behöver ändras.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-159">You can add or remove nodes tooyour standalone Service Fabric cluster as your business needs change.</span></span> <span data-ttu-id="3e7e5-160">Se [lägga till eller ta bort noder fristående Service Fabric-klustret tooa](service-fabric-cluster-windows-server-add-remove-nodes.md) detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-160">See [Add or Remove nodes tooa Service Fabric standalone cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) for detailed steps.</span></span>

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a><span data-ttu-id="3e7e5-161">Ta bort ett kluster</span><span class="sxs-lookup"><span data-stu-id="3e7e5-161">Remove a cluster</span></span>
<span data-ttu-id="3e7e5-162">tooremove ett kluster som kör hello *RemoveServiceFabricCluster.ps1* PowerShell-skript från hello paketmappen och skicka hello sökvägen toohello JSON-konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-162">tooremove a cluster, run hello *RemoveServiceFabricCluster.ps1* PowerShell script from hello package folder and pass in hello path toohello JSON configuration file.</span></span> <span data-ttu-id="3e7e5-163">Du kan du ange en plats för hello logg hello borttagningen av.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-163">You can optionally specify a location for hello log of hello deletion.</span></span>

<span data-ttu-id="3e7e5-164">Det här skriptet kan köras på en dator som har administratören åtkomst tooall hello datorer som listas som noder i konfigurationsfilen för hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-164">This script can be run on any machine that has administrator access tooall hello machines that are listed as nodes in hello cluster configuration file.</span></span> <span data-ttu-id="3e7e5-165">hello-dator som det här skriptet körs på har inte toobe tillhör hello kluster.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-165">hello machine that this script is run on does not have toobe part of hello cluster.</span></span>

```
# Removes Service Fabric from each machine in hello configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from hello current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-tooopt-out-of-it"></a><span data-ttu-id="3e7e5-166">Telemetridata som samlas in och hur tooopt ur den</span><span class="sxs-lookup"><span data-stu-id="3e7e5-166">Telemetry data collected and how tooopt out of it</span></span>
<span data-ttu-id="3e7e5-167">Som standard hello produkt samlar in telemetri om hello Service Fabric användning tooimprove hello produkt.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-167">As a default, hello product collects telemetry on hello Service Fabric usage tooimprove hello product.</span></span> <span data-ttu-id="3e7e5-168">hello Best Practice Analyzer som körs som en del av installationen hello kontrollerar anslutningen för[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span><span class="sxs-lookup"><span data-stu-id="3e7e5-168">hello Best Practice Analyzer that runs as a part of hello setup checks for connectivity too[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span></span> <span data-ttu-id="3e7e5-169">Om det inte går att nå misslyckas hello installationen om du avanmäler dig från telemetri.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-169">If it is not reachable, hello setup fails unless you opt out of telemetry.</span></span>

1. <span data-ttu-id="3e7e5-170">hello telemetri pipeline försöker tooupload hello följande data för[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) varje gång om dagen.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-170">hello telemetry pipeline tries tooupload hello following data too[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) once every day.</span></span> <span data-ttu-id="3e7e5-171">Det är en bästa överföring och har ingen inverkan på hello klusterfunktionaliteten.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-171">It is a best-effort upload and has no impact on hello cluster functionality.</span></span> <span data-ttu-id="3e7e5-172">hello telemetri skickas endast från hello nod som kör hello failover manager primär.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-172">hello telemetry is only sent from hello node that runs hello failover manager primary.</span></span> <span data-ttu-id="3e7e5-173">Inga andra noder skicka ut telemetri.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-173">No other nodes send out telemetry.</span></span>
2. <span data-ttu-id="3e7e5-174">hello telemetri består av hello följande:</span><span class="sxs-lookup"><span data-stu-id="3e7e5-174">hello telemetry consists of hello following:</span></span>

* <span data-ttu-id="3e7e5-175">Antal tjänster</span><span class="sxs-lookup"><span data-stu-id="3e7e5-175">Number of services</span></span>
* <span data-ttu-id="3e7e5-176">Antal ServiceTypes</span><span class="sxs-lookup"><span data-stu-id="3e7e5-176">Number of ServiceTypes</span></span>
* <span data-ttu-id="3e7e5-177">Antal program</span><span class="sxs-lookup"><span data-stu-id="3e7e5-177">Number of Applications</span></span>
* <span data-ttu-id="3e7e5-178">Antal ApplicationUpgrades</span><span class="sxs-lookup"><span data-stu-id="3e7e5-178">Number of ApplicationUpgrades</span></span>
* <span data-ttu-id="3e7e5-179">Antal FailoverUnits</span><span class="sxs-lookup"><span data-stu-id="3e7e5-179">Number of FailoverUnits</span></span>
* <span data-ttu-id="3e7e5-180">Antal InBuildFailoverUnits</span><span class="sxs-lookup"><span data-stu-id="3e7e5-180">Number of InBuildFailoverUnits</span></span>
* <span data-ttu-id="3e7e5-181">Antal UnhealthyFailoverUnits</span><span class="sxs-lookup"><span data-stu-id="3e7e5-181">Number of UnhealthyFailoverUnits</span></span>
* <span data-ttu-id="3e7e5-182">Antal repliker</span><span class="sxs-lookup"><span data-stu-id="3e7e5-182">Number of Replicas</span></span>
* <span data-ttu-id="3e7e5-183">Antal InBuildReplicas</span><span class="sxs-lookup"><span data-stu-id="3e7e5-183">Number of InBuildReplicas</span></span>
* <span data-ttu-id="3e7e5-184">Antal StandByReplicas</span><span class="sxs-lookup"><span data-stu-id="3e7e5-184">Number of StandByReplicas</span></span>
* <span data-ttu-id="3e7e5-185">Antal OfflineReplicas</span><span class="sxs-lookup"><span data-stu-id="3e7e5-185">Number of OfflineReplicas</span></span>
* <span data-ttu-id="3e7e5-186">CommonQueueLength</span><span class="sxs-lookup"><span data-stu-id="3e7e5-186">CommonQueueLength</span></span>
* <span data-ttu-id="3e7e5-187">QueryQueueLength</span><span class="sxs-lookup"><span data-stu-id="3e7e5-187">QueryQueueLength</span></span>
* <span data-ttu-id="3e7e5-188">FailoverUnitQueueLength</span><span class="sxs-lookup"><span data-stu-id="3e7e5-188">FailoverUnitQueueLength</span></span>
* <span data-ttu-id="3e7e5-189">CommitQueueLength</span><span class="sxs-lookup"><span data-stu-id="3e7e5-189">CommitQueueLength</span></span>
* <span data-ttu-id="3e7e5-190">Antalet noder</span><span class="sxs-lookup"><span data-stu-id="3e7e5-190">Number of Nodes</span></span>
* <span data-ttu-id="3e7e5-191">IsContextComplete: True/False</span><span class="sxs-lookup"><span data-stu-id="3e7e5-191">IsContextComplete: True/False</span></span>
* <span data-ttu-id="3e7e5-192">ClusterId: Detta är ett GUID som genereras slumpmässigt för varje kluster</span><span class="sxs-lookup"><span data-stu-id="3e7e5-192">ClusterId: This is a GUID randomly generated for each cluster</span></span>
* <span data-ttu-id="3e7e5-193">ServiceFabricVersion</span><span class="sxs-lookup"><span data-stu-id="3e7e5-193">ServiceFabricVersion</span></span>
* <span data-ttu-id="3e7e5-194">Hello virtuella datorn eller datorn från vilken hello telemetri överförs IP-adress</span><span class="sxs-lookup"><span data-stu-id="3e7e5-194">IP address of hello virtual machine or machine from which hello telemetry is uploaded</span></span>

<span data-ttu-id="3e7e5-195">toodisable telemetri, Lägg till följande hello för*egenskaper* i klustret config: *enableTelemetry: false*.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-195">toodisable telemetry, add hello following too*properties* in your cluster config: *enableTelemetry: false*.</span></span>

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a><span data-ttu-id="3e7e5-196">Förhandsgranskningsfunktioner som ingår i det här paketet</span><span class="sxs-lookup"><span data-stu-id="3e7e5-196">Preview features included in this package</span></span>
<span data-ttu-id="3e7e5-197">Ingen.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-197">None.</span></span>


> [!NOTE]
> <span data-ttu-id="3e7e5-198">Från och med hello nya [GA-versionen av hello fristående kluster för Windows Server (version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), du kan uppgradera ditt kluster toofuture versioner, manuellt eller automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-198">Starting with hello new [GA version of hello standalone cluster for Windows Server (version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), you can upgrade your cluster toofuture releases, manually or automatically.</span></span> <span data-ttu-id="3e7e5-199">Se för[uppgradera en fristående Service Fabric-kluster version](service-fabric-cluster-upgrade-windows-server.md) dokument för mer information.</span><span class="sxs-lookup"><span data-stu-id="3e7e5-199">Refer too[Upgrade a standalone Service Fabric cluster version](service-fabric-cluster-upgrade-windows-server.md) document for details.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="3e7e5-200">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e7e5-200">Next steps</span></span>
* [<span data-ttu-id="3e7e5-201">Distribuera och ta bort program med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="3e7e5-201">Deploy and remove applications using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="3e7e5-202">Konfigurationsinställningar för fristående Windows-kluster</span><span class="sxs-lookup"><span data-stu-id="3e7e5-202">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="3e7e5-203">Lägg till eller ta bort noder tooa fristående Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="3e7e5-203">Add or remove nodes tooa standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="3e7e5-204">Uppgradera en fristående Service Fabric-kluster-version</span><span class="sxs-lookup"><span data-stu-id="3e7e5-204">Upgrade a standalone Service Fabric cluster version</span></span>](service-fabric-cluster-upgrade-windows-server.md)
* [<span data-ttu-id="3e7e5-205">Skapa ett fristående Service Fabric-kluster med Azure virtuella datorer som kör Windows</span><span class="sxs-lookup"><span data-stu-id="3e7e5-205">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [<span data-ttu-id="3e7e5-206">Skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet</span><span class="sxs-lookup"><span data-stu-id="3e7e5-206">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="3e7e5-207">Skydda ett fristående kluster på Windows med X509 certifikat</span><span class="sxs-lookup"><span data-stu-id="3e7e5-207">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
