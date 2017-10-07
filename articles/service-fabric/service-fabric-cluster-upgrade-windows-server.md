---
title: "aaaUpgrade som en fristående Azure Service Fabric-kluster i Windows Server | Microsoft Docs"
description: "Uppgradera hello Azure Service Fabric-kod och/eller konfiguration som kör ett fristående Service Fabric-klustret, inklusive inställning hello uppdateringsläge för klustret."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 5132795e544b6f0185accedbf5092dcaafd66df0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a><span data-ttu-id="b4177-103">Uppgradera din fristående Azure Service Fabric på Windows Server-kluster</span><span class="sxs-lookup"><span data-stu-id="b4177-103">Upgrade your standalone Azure Service Fabric on Windows Server cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b4177-104">Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="b4177-104">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="b4177-105">Fristående kluster</span><span class="sxs-lookup"><span data-stu-id="b4177-105">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
>
>

<span data-ttu-id="b4177-106">För alla moderna system är hello möjlighet tooupgrade viktiga toohello långsiktig framgång med produkten.</span><span class="sxs-lookup"><span data-stu-id="b4177-106">For any modern system, hello ability tooupgrade is a key toohello long-term success of your product.</span></span> <span data-ttu-id="b4177-107">Ett Azure Service Fabric-kluster är en resurs som du äger.</span><span class="sxs-lookup"><span data-stu-id="b4177-107">An Azure Service Fabric cluster is a resource that you own.</span></span> <span data-ttu-id="b4177-108">Den här artikeln beskriver hur du kan kontrollera hello klustret körs alltid versioner av Service Fabric-kod och konfigurationer som stöds.</span><span class="sxs-lookup"><span data-stu-id="b4177-108">This article describes how you can make sure that hello cluster always runs supported versions of Service Fabric code and configurations.</span></span>

## <a name="control-hello-service-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="b4177-109">Kontrollen hello Service Fabric-version som körs på klustret</span><span class="sxs-lookup"><span data-stu-id="b4177-109">Control hello Service Fabric version that runs on your cluster</span></span>
<span data-ttu-id="b4177-110">tooset kluster-toodownload uppdateringar av Service Fabric när Microsoft publicerar en ny version, ange hello **fabricClusterAutoupgradeEnabled** kluster configuration tootrue.</span><span class="sxs-lookup"><span data-stu-id="b4177-110">tooset your cluster toodownload updates of Service Fabric when Microsoft releases a new version, set hello **fabricClusterAutoupgradeEnabled** cluster configuration tootrue.</span></span> <span data-ttu-id="b4177-111">tooselect en stödd version av Service Fabric som du vill att ditt kluster toobe på uppsättningen hello **fabricClusterAutoupgradeEnabled** kluster configuration toofalse.</span><span class="sxs-lookup"><span data-stu-id="b4177-111">tooselect a supported version of Service Fabric that you want your cluster toobe on, set hello **fabricClusterAutoupgradeEnabled** cluster configuration toofalse.</span></span>

> [!NOTE]
> <span data-ttu-id="b4177-112">Kontrollera att klustret alltid kör en Service Fabric-version som stöds.</span><span class="sxs-lookup"><span data-stu-id="b4177-112">Make sure that your cluster always runs a supported Service Fabric version.</span></span> <span data-ttu-id="b4177-113">När Microsoft tillkännager hello-versionen av en ny version av Service Fabric, har hello tidigare version markerats för slutet av stödet efter minst 60 dagar från hello hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="b4177-113">When Microsoft announces hello release of a new version of Service Fabric, hello previous version is marked for end of support after a minimum of 60 days from hello date of hello announcement.</span></span> <span data-ttu-id="b4177-114">Nya versioner tillkännages [på hello Service Fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="b4177-114">New releases are announced [on hello Service Fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="b4177-115">hello ny version är tillgänglig toochoose vid den punkten.</span><span class="sxs-lookup"><span data-stu-id="b4177-115">hello new release is available toochoose at that point.</span></span>
>
>

<span data-ttu-id="b4177-116">Du kan uppgradera den nya versionen för kluster-toohello endast om du använder en konfiguration av noden produktions-format, där varje Service Fabric-nod har allokerats på en separat fysisk eller virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b4177-116">You can upgrade your cluster toohello new version only if you are using a production-style node configuration, where each Service Fabric node is allocated on a separate physical or virtual machine.</span></span> <span data-ttu-id="b4177-117">Om du har ett kluster för utveckling, där fler än en Service Fabric-nod är på en enda fysisk eller virtuell dator, måste du återskapa hello kluster med hello ny version.</span><span class="sxs-lookup"><span data-stu-id="b4177-117">If you have a development cluster, where more than one Service Fabric node is on a single physical or virtual machine, you must re-create hello cluster with hello new version.</span></span>

<span data-ttu-id="b4177-118">Två olika arbetsflöden kan uppgradera din klustret toohello senaste version eller en Service Fabric-version som stöds.</span><span class="sxs-lookup"><span data-stu-id="b4177-118">Two distinct workflows can upgrade your cluster toohello latest version or a supported Service Fabric version.</span></span> <span data-ttu-id="b4177-119">Ett arbetsflöde är för kluster som har anslutning toodownload hello senaste versionen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b4177-119">One workflow is for clusters that have connectivity toodownload hello latest version automatically.</span></span> <span data-ttu-id="b4177-120">hello andra arbetsflöde är för kluster som inte har anslutning toodownload hello senaste Service Fabric versionen.</span><span class="sxs-lookup"><span data-stu-id="b4177-120">hello other workflow is for clusters that do not have connectivity toodownload hello latest Service Fabric version.</span></span>

### <a name="upgrade-clusters-that-have-connectivity-toodownload-hello-latest-code-and-configuration"></a><span data-ttu-id="b4177-121">Uppgradera kluster som har anslutning toodownload hello senaste kod och konfiguration</span><span class="sxs-lookup"><span data-stu-id="b4177-121">Upgrade clusters that have connectivity toodownload hello latest code and configuration</span></span>
<span data-ttu-id="b4177-122">Använd dessa steg tooupgrade ditt kluster tooa som stöds av version om klusternoderna är anslutna till Internet för[http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b4177-122">Use these steps tooupgrade your cluster tooa supported version if your cluster nodes have Internet connectivity too[http://download.microsoft.com](http://download.microsoft.com).</span></span>

<span data-ttu-id="b4177-123">För kluster som har anslutning för[http://download.microsoft.com](http://download.microsoft.com), Microsoft söker regelbundet efter hello tillgängligheten för nya Service Fabric-versioner.</span><span class="sxs-lookup"><span data-stu-id="b4177-123">For clusters that have connectivity too[http://download.microsoft.com](http://download.microsoft.com), Microsoft periodically checks for hello availability of new Service Fabric versions.</span></span>

<span data-ttu-id="b4177-124">När en ny Service Fabric-version är tillgänglig hello paketet hämtas lokalt toohello klustret och etableras för uppgradering.</span><span class="sxs-lookup"><span data-stu-id="b4177-124">When a new Service Fabric version is available, hello package is downloaded locally toohello cluster and provisioned for upgrade.</span></span> <span data-ttu-id="b4177-125">Dessutom tooinform hello kunden för den här nya versionen, hello system visar en varning om hälsa explicit kluster som är liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="b4177-125">Additionally, tooinform hello customer of this new version, hello system shows an explicit cluster health warning that's similar toohello following:</span></span>

<span data-ttu-id="b4177-126">”Hej aktuella versionen [version #] klusterstöd slutar [Date]”.</span><span class="sxs-lookup"><span data-stu-id="b4177-126">“hello current cluster version [version#] support ends [Date]."</span></span>

<span data-ttu-id="b4177-127">När hello klustret kör hello senaste versionen, försvinner hello varningen.</span><span class="sxs-lookup"><span data-stu-id="b4177-127">After hello cluster is running hello latest version, hello warning goes away.</span></span>

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="b4177-128">Uppgradera arbetsflödet för kluster</span><span class="sxs-lookup"><span data-stu-id="b4177-128">Cluster upgrade workflow</span></span>
<span data-ttu-id="b4177-129">Efter att du ser hello klustret hälsotillstånd varning hello följande:</span><span class="sxs-lookup"><span data-stu-id="b4177-129">After you see hello cluster health warning, do hello following:</span></span>

1. <span data-ttu-id="b4177-130">Ansluta toohello kluster från en dator som har administratören åtkomst tooall hello datorer som listas som noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="b4177-130">Connect toohello cluster from any machine that has administrator access tooall hello machines that are listed as nodes in hello cluster.</span></span> <span data-ttu-id="b4177-131">hello-dator som det här skriptet körs på har inte toobe tillhör hello kluster.</span><span class="sxs-lookup"><span data-stu-id="b4177-131">hello machine that this script is run on does not have toobe part of hello cluster.</span></span>

    ```powershell

    ###### connect toohello secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. <span data-ttu-id="b4177-132">Hämta hello lista över Service Fabric-versioner som du kan uppgradera till.</span><span class="sxs-lookup"><span data-stu-id="b4177-132">Get hello list of Service Fabric versions that you can upgrade to.</span></span>

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    <span data-ttu-id="b4177-133">Du bör få en liknande toothis utdata:</span><span class="sxs-lookup"><span data-stu-id="b4177-133">You should get an output similar toothis:</span></span>

    ![Hämta fabric versioner][getfabversions]
3. <span data-ttu-id="b4177-135">Starta en klustret uppgradera tooan tillgängliga versionen med hjälp av den [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell cmd.</span><span class="sxs-lookup"><span data-stu-id="b4177-135">Start a cluster upgrade tooan available version by using the [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell cmd.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="b4177-136">toomonitor hello utvecklingen av hello uppgraderingen, som du kan använda Service Fabric-Utforskaren eller kör hello följande Windows PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="b4177-136">toomonitor hello progress of hello upgrade, you can use Service Fabric Explorer or run hello following Windows PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="b4177-137">Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="b4177-137">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="b4177-138">toospecify anpassade hälsoprinciper för hello **Start ServiceFabricClusterUpgrade** finns i dokumentationen för [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4177-138">toospecify custom health policies for hello **Start-ServiceFabricClusterUpgrade** command, see documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="b4177-139">När du har åtgärdat hello problem som resulterade i hello återställning initiera hello uppgraderingen igen med följande hello likadant enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="b4177-139">After you fix hello issues that resulted in hello rollback, initiate hello upgrade again by following hello same steps as previously described.</span></span>

### <a name="upgrade-clusters-that-have-uno-connectivityu-toodownload-hello-latest-code-and-configuration"></a><span data-ttu-id="b4177-140">Uppgradera kluster som har <U>ingen nätverksanslutning</u> toodownload hello senaste kod och konfiguration</span><span class="sxs-lookup"><span data-stu-id="b4177-140">Upgrade clusters that have <U>no connectivity</u> toodownload hello latest code and configuration</span></span>
<span data-ttu-id="b4177-141">Använda dessa steg tooupgrade ditt kluster tooa som stöds av version om klusternoderna inte har Internetanslutning för[http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b4177-141">Use these steps tooupgrade your cluster tooa supported version if your cluster nodes do not have Internet connectivity too[http://download.microsoft.com](http://download.microsoft.com).</span></span>

> [!NOTE]
> <span data-ttu-id="b4177-142">Om du kör ett kluster som inte är anslutna toohello Internet, har du toomonitor hello Service Fabric team-blogg toolearn om en ny version.</span><span class="sxs-lookup"><span data-stu-id="b4177-142">If you are running a cluster that is not connected toohello Internet, you will have toomonitor hello Service Fabric team blog toolearn about a new release.</span></span> <span data-ttu-id="b4177-143">hello system visar inte ett kluster hälsa varning tooalert du en ny version.</span><span class="sxs-lookup"><span data-stu-id="b4177-143">hello system does not show a cluster health warning tooalert you of a new release.</span></span>  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a><span data-ttu-id="b4177-144">Automatisk etablering jämfört med manuella etablering</span><span class="sxs-lookup"><span data-stu-id="b4177-144">Auto Provisioning vs Manual Provisioning</span></span>
<span data-ttu-id="b4177-145">Konfigurera Service Fabric-uppdateringstjänsten tooenable automatisk hämtning och registrering för hello senaste code-versionen.</span><span class="sxs-lookup"><span data-stu-id="b4177-145">tooenable automatic downloading and registration for hello latest code version, set up Service Fabric Update Service.</span></span> <span data-ttu-id="b4177-146">Se toohello Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt inuti hello [fristående paketet](service-fabric-cluster-standalone-package-contents.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="b4177-146">Refer toohello Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt inside hello [Standalone Package](service-fabric-cluster-standalone-package-contents.md) for instructions.</span></span>
<span data-ttu-id="b4177-147">Följ hello anvisningarna nedan för manuell process.</span><span class="sxs-lookup"><span data-stu-id="b4177-147">For manual process, follow hello instructions below.</span></span>

<span data-ttu-id="b4177-148">Ändra ditt kluster configuration tooset hello efter egenskapen toofalse innan du påbörjar en uppgradering av configuration.</span><span class="sxs-lookup"><span data-stu-id="b4177-148">Modify your cluster configuration tooset hello following property toofalse before you start a configuration upgrade.</span></span>

        "fabricClusterAutoupgradeEnabled": false,

<span data-ttu-id="b4177-149">Se för[Start ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) för användningsinformation.</span><span class="sxs-lookup"><span data-stu-id="b4177-149">Refer too[Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) for usage details.</span></span> <span data-ttu-id="b4177-150">Gör att tooupdate 'clusterConfigurationVersion' i din JSON innan du börjar hello configuration uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="b4177-150">Make sure tooupdate 'clusterConfigurationVersion' in your JSON before you start hello configuration upgrade.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="b4177-151">Uppgradera arbetsflödet för kluster</span><span class="sxs-lookup"><span data-stu-id="b4177-151">Cluster upgrade workflow</span></span>

1. <span data-ttu-id="b4177-152">Kör Get-ServiceFabricClusterUpgrade från en hello noder i klustret hello och notera hello TargetCodeVersion.</span><span class="sxs-lookup"><span data-stu-id="b4177-152">Run Get-ServiceFabricClusterUpgrade from one of hello nodes in hello cluster and note hello TargetCodeVersion.</span></span>
2. <span data-ttu-id="b4177-153">Kör hello följande från en internet-anslutna datorn toolist alla uppgradera kompatibla versioner med nuvarande version av hello och hämta hello motsvarande paket från hello associerade länkar.</span><span class="sxs-lookup"><span data-stu-id="b4177-153">Run hello following from an internet connected machine toolist all upgrade compatible versions with hello current version and download hello corresponding package from hello associated download links.</span></span>

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. <span data-ttu-id="b4177-154">Ansluta toohello kluster från en dator som har administratören åtkomst tooall hello datorer som listas som noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="b4177-154">Connect toohello cluster from any machine that has administrator access tooall hello machines that are listed as nodes in hello cluster.</span></span> <span data-ttu-id="b4177-155">hello-dator som det här skriptet körs på inte har toobe tillhör hello kluster</span><span class="sxs-lookup"><span data-stu-id="b4177-155">hello machine that this script is run on does not have toobe part of hello cluster</span></span>

    ```powershell

   ###### Get hello list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file including hello path tooit> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. <span data-ttu-id="b4177-156">Kopiera hello hämtade paketet till hello avbildningsarkivet för klustret.</span><span class="sxs-lookup"><span data-stu-id="b4177-156">Copy hello downloaded package into hello cluster image store.</span></span>

5. <span data-ttu-id="b4177-157">Registrera hello kopierade paketet.</span><span class="sxs-lookup"><span data-stu-id="b4177-157">Register hello copied package.</span></span>

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. <span data-ttu-id="b4177-158">Starta en klustret uppgradera tooan tillgängliga versionen.</span><span class="sxs-lookup"><span data-stu-id="b4177-158">Start a cluster upgrade tooan available version.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="b4177-159">Du kan övervaka hello hello uppgraderingen på Service Fabric-Utforskaren eller köra hello följande PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="b4177-159">You can monitor hello progress of hello upgrade on Service Fabric Explorer, or you can run hello following PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="b4177-160">Hello uppgraderingen återställs om hello klustret hälsoprinciper inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="b4177-160">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="b4177-161">toospecify anpassade hälsoprinciper för hello **Start ServiceFabricClusterUpgrade** finns i dokumentationen hello [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4177-161">toospecify custom health policies for hello **Start-ServiceFabricClusterUpgrade** command, see hello documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="b4177-162">När du har åtgärdat hello problem som resulterade i hello återställning initiera hello uppgraderingen igen med följande hello likadant enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="b4177-162">After you fix hello issues that resulted in hello rollback, initiate hello upgrade again by following hello same steps as previously described.</span></span>


## <a name="upgrade-hello-cluster-configuration"></a><span data-ttu-id="b4177-163">Uppgradera hello klusterkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="b4177-163">Upgrade hello cluster configuration</span></span>
<span data-ttu-id="b4177-164">Innan du startar hello configuration uppgradering kan du testa din nya kluster configuration json genom att köra hello powershell-skript i hello fristående paketet.</span><span class="sxs-lookup"><span data-stu-id="b4177-164">Before you initiate hello configuration upgrade, you can test your new cluster configuration json by running hello powershell script in hello standalone package.</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File>

```
<span data-ttu-id="b4177-165">eller</span><span class="sxs-lookup"><span data-stu-id="b4177-165">or</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File> -FabricRuntimePackagePath <Path toohello .cab file which you want tootest hello configuration against>

```

<span data-ttu-id="b4177-166">Vissa konfigurationer måste uppgraderas, till exempel slutpunkter, klusternamnet, noden IP osv. Detta testar hello nya kluster configuration json mot hello gamla och utlösa fel i hello Powershell-fönstret om det inte finns några problem.</span><span class="sxs-lookup"><span data-stu-id="b4177-166">Some configurations can't be upgraded, such as endpoints, cluster name, node IP, etc. This will test hello new cluster configuration json against hello old one and throw errors in hello Powershell window if there is any issue.</span></span>

<span data-ttu-id="b4177-167">tooupgrade hello konfiguration av klusteruppgradering, kör **Start ServiceFabricClusterConfigurationUpgrade**.</span><span class="sxs-lookup"><span data-stu-id="b4177-167">tooupgrade hello cluster configuration upgrade, run **Start-ServiceFabricClusterConfigurationUpgrade**.</span></span> <span data-ttu-id="b4177-168">hello configuration uppgradering är bearbetade uppgraderingsdomän av uppgraderingsdomänen.</span><span class="sxs-lookup"><span data-stu-id="b4177-168">hello configuration upgrade is processed upgrade domain by upgrade domain.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

### <a name="cluster-certificate-config-upgrade"></a><span data-ttu-id="b4177-169">Klusteruppgradering certifikat config</span><span class="sxs-lookup"><span data-stu-id="b4177-169">Cluster certificate config upgrade</span></span>  
<span data-ttu-id="b4177-170">Klustret certifikat används för autentisering mellan klusternoder, så hello certifikat rulla över ska utföras med extra försiktig eftersom fel blockerar hello kommunikation mellan klusternoder.</span><span class="sxs-lookup"><span data-stu-id="b4177-170">Cluster certificate is used for authentication between cluster nodes, so hello certificate roll over should be performed with extra caution because failure will block hello communication among cluster nodes.</span></span>  
<span data-ttu-id="b4177-171">Tekniskt sett stöds tre alternativ:</span><span class="sxs-lookup"><span data-stu-id="b4177-171">Technically, three options are supported:</span></span>  

1. <span data-ttu-id="b4177-172">Enskilt certifikat uppgraderingen: hello uppgraderingsväg är ' certifikat C (primär)-certifikat (primär) -> certifikatet B (primär) > ->... ”.</span><span class="sxs-lookup"><span data-stu-id="b4177-172">Single certificate upgrade: hello upgrade path is 'Certificate A (Primary) -> Certificate B (Primary) -> Certificate C (Primary) -> ...'.</span></span>   
2. <span data-ttu-id="b4177-173">Dubbla certifikat uppgraderingen: hello uppgraderingsväg är ' B (sekundär) -> certifikatet B (primär) och certifikat (primär) -> certifikat (primär) > certifikatet B (primär) och C (sekundär) -> certifikatet C (primär) ->... ”.</span><span class="sxs-lookup"><span data-stu-id="b4177-173">Double certificate upgrade: hello upgrade path is 'Certificate A (Primary) -> Certificate A (Primary) and B (Secondary) -> Certificate B (Primary) -> Certificate B (Primary) and C (Secondary) -> Certificate C (Primary) -> ...'.</span></span>
3. <span data-ttu-id="b4177-174">Av Typuppgradering av certifikat: tumavtryck-baserade certifikat <>--Vanligtnamn-baserade Certifikatkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="b4177-174">Certificate type upgrade: Thumbprint-based certificate configuration <-> CommonName-based certificate configuration.</span></span> <span data-ttu-id="b4177-175">Till exempel certifikatets tumavtryck (primär) och tumavtrycket B (sekundär) -> certifikatet CommonName C.</span><span class="sxs-lookup"><span data-stu-id="b4177-175">For example, Certificate Thumbprint A (Primary) and Thumbprint B (Secondary) -> Certificate CommonName C.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b4177-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b4177-176">Next steps</span></span>
* <span data-ttu-id="b4177-177">Lär dig hur toocustomize vissa [Service Fabric-klusterinställningar](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="b4177-177">Learn how toocustomize some [Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>
* <span data-ttu-id="b4177-178">Lär dig hur för[skala ditt kluster in och ut](service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="b4177-178">Learn how too[scale your cluster in and out](service-fabric-cluster-scale-up-down.md).</span></span>
* <span data-ttu-id="b4177-179">Lär dig mer om [programuppgraderingar](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="b4177-179">Learn about [application upgrades](service-fabric-application-upgrade.md).</span></span>

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
