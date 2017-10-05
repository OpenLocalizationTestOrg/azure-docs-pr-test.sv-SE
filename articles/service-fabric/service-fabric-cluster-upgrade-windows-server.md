---
title: "Uppgradera en fristående Azure Service Fabric-kluster i Windows Server | Microsoft Docs"
description: "Uppgradera Azure Service Fabric-kod och/eller konfiguration som kör ett fristående Service Fabric-klustret, inklusive inställning uppdateringsläget klustret."
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
ms.openlocfilehash: ac40775ca62362a32184207857a0b965a798e135
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a><span data-ttu-id="089cd-103">Uppgradera din fristående Azure Service Fabric på Windows Server-kluster</span><span class="sxs-lookup"><span data-stu-id="089cd-103">Upgrade your standalone Azure Service Fabric on Windows Server cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="089cd-104">Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="089cd-104">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="089cd-105">Fristående kluster</span><span class="sxs-lookup"><span data-stu-id="089cd-105">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
>
>

<span data-ttu-id="089cd-106">För alla moderna system är möjligheten att uppgradera en nyckel till långsiktig framgång för en produkt.</span><span class="sxs-lookup"><span data-stu-id="089cd-106">For any modern system, the ability to upgrade is a key to the long-term success of your product.</span></span> <span data-ttu-id="089cd-107">Ett Azure Service Fabric-kluster är en resurs som du äger.</span><span class="sxs-lookup"><span data-stu-id="089cd-107">An Azure Service Fabric cluster is a resource that you own.</span></span> <span data-ttu-id="089cd-108">Den här artikeln beskriver hur du kan kontrollera att klustret alltid körs versioner av Service Fabric-kod och konfigurationer som stöds.</span><span class="sxs-lookup"><span data-stu-id="089cd-108">This article describes how you can make sure that the cluster always runs supported versions of Service Fabric code and configurations.</span></span>

## <a name="control-the-service-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="089cd-109">Kontrollera Service Fabric-version som körs på klustret</span><span class="sxs-lookup"><span data-stu-id="089cd-109">Control the Service Fabric version that runs on your cluster</span></span>
<span data-ttu-id="089cd-110">Att ställa in klustret för att hämta uppdateringar av Service Fabric när Microsoft publicerar en ny version av **fabricClusterAutoupgradeEnabled** klusterkonfigurationen till true.</span><span class="sxs-lookup"><span data-stu-id="089cd-110">To set your cluster to download updates of Service Fabric when Microsoft releases a new version, set the **fabricClusterAutoupgradeEnabled** cluster configuration to true.</span></span> <span data-ttu-id="089cd-111">En version av Service Fabric som du vill att klustret ska finnas på stöds kan ange den **fabricClusterAutoupgradeEnabled** klusterkonfigurationen till false.</span><span class="sxs-lookup"><span data-stu-id="089cd-111">To select a supported version of Service Fabric that you want your cluster to be on, set the **fabricClusterAutoupgradeEnabled** cluster configuration to false.</span></span>

> [!NOTE]
> <span data-ttu-id="089cd-112">Kontrollera att klustret alltid kör en Service Fabric-version som stöds.</span><span class="sxs-lookup"><span data-stu-id="089cd-112">Make sure that your cluster always runs a supported Service Fabric version.</span></span> <span data-ttu-id="089cd-113">När Microsoft Beskriver utgivningen av en ny version av Service Fabric, markeras den tidigare versionen för slutet av stödet efter minst 60 dagar från datumet då meddelandet.</span><span class="sxs-lookup"><span data-stu-id="089cd-113">When Microsoft announces the release of a new version of Service Fabric, the previous version is marked for end of support after a minimum of 60 days from the date of the announcement.</span></span> <span data-ttu-id="089cd-114">Nya versioner tillkännages [på Service Fabric-teamets blogg](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="089cd-114">New releases are announced [on the Service Fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="089cd-115">Den nya versionen finns att välja vid den punkten.</span><span class="sxs-lookup"><span data-stu-id="089cd-115">The new release is available to choose at that point.</span></span>
>
>

<span data-ttu-id="089cd-116">Du kan uppgradera klustret till den nya versionen endast om du använder en konfiguration av noden produktions-format, där varje Service Fabric-nod har allokerats på en separat fysisk eller virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="089cd-116">You can upgrade your cluster to the new version only if you are using a production-style node configuration, where each Service Fabric node is allocated on a separate physical or virtual machine.</span></span> <span data-ttu-id="089cd-117">Om du har ett kluster för utveckling, där fler än en Service Fabric-nod är på en enda fysisk eller virtuell dator, måste du återskapa klustret med den nya versionen.</span><span class="sxs-lookup"><span data-stu-id="089cd-117">If you have a development cluster, where more than one Service Fabric node is on a single physical or virtual machine, you must re-create the cluster with the new version.</span></span>

<span data-ttu-id="089cd-118">Två olika arbetsflöden kan uppgradera klustret till den senaste versionen eller en Service Fabric-version som stöds.</span><span class="sxs-lookup"><span data-stu-id="089cd-118">Two distinct workflows can upgrade your cluster to the latest version or a supported Service Fabric version.</span></span> <span data-ttu-id="089cd-119">Ett arbetsflöde är för kluster som har anslutning till den senaste versionen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="089cd-119">One workflow is for clusters that have connectivity to download the latest version automatically.</span></span> <span data-ttu-id="089cd-120">Andra arbetsflödet är för kluster som inte har anslutning till den senaste versionen Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="089cd-120">The other workflow is for clusters that do not have connectivity to download the latest Service Fabric version.</span></span>

### <a name="upgrade-clusters-that-have-connectivity-to-download-the-latest-code-and-configuration"></a><span data-ttu-id="089cd-121">Uppgradera kluster som har anslutning att hämta de senaste kod och konfiguration</span><span class="sxs-lookup"><span data-stu-id="089cd-121">Upgrade clusters that have connectivity to download the latest code and configuration</span></span>
<span data-ttu-id="089cd-122">Följ dessa steg för att uppgradera ditt kluster till en version som stöds om klusternoderna är anslutna till Internet att [http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="089cd-122">Use these steps to upgrade your cluster to a supported version if your cluster nodes have Internet connectivity to [http://download.microsoft.com](http://download.microsoft.com).</span></span>

<span data-ttu-id="089cd-123">För kluster som har anslutning till [http://download.microsoft.com](http://download.microsoft.com), Microsoft regelbundet kontrollerar tillgängligheten för nya Service Fabric-versioner.</span><span class="sxs-lookup"><span data-stu-id="089cd-123">For clusters that have connectivity to [http://download.microsoft.com](http://download.microsoft.com), Microsoft periodically checks for the availability of new Service Fabric versions.</span></span>

<span data-ttu-id="089cd-124">När en ny Service Fabric-version är tillgänglig, hämtas lokalt till klustret paketet och etableras för uppgradering.</span><span class="sxs-lookup"><span data-stu-id="089cd-124">When a new Service Fabric version is available, the package is downloaded locally to the cluster and provisioned for upgrade.</span></span> <span data-ttu-id="089cd-125">För att informera kunden om den här nya versionen visas dessutom systemet en explicit klustret hälsotillstånd varning som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="089cd-125">Additionally, to inform the customer of this new version, the system shows an explicit cluster health warning that's similar to the following:</span></span>

<span data-ttu-id="089cd-126">”Den aktuella versionen [version #] klusterstöd slutar [Date]”.</span><span class="sxs-lookup"><span data-stu-id="089cd-126">“The current cluster version [version#] support ends [Date]."</span></span>

<span data-ttu-id="089cd-127">När klustret kör den senaste versionen, försvinner varningen.</span><span class="sxs-lookup"><span data-stu-id="089cd-127">After the cluster is running the latest version, the warning goes away.</span></span>

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="089cd-128">Uppgradera arbetsflödet för kluster</span><span class="sxs-lookup"><span data-stu-id="089cd-128">Cluster upgrade workflow</span></span>
<span data-ttu-id="089cd-129">När du ser varningen för klustret hälsa gör du följande:</span><span class="sxs-lookup"><span data-stu-id="089cd-129">After you see the cluster health warning, do the following:</span></span>

1. <span data-ttu-id="089cd-130">Ansluta till klustret från en dator som har administratörsåtkomst till alla datorer som listas som noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="089cd-130">Connect to the cluster from any machine that has administrator access to all the machines that are listed as nodes in the cluster.</span></span> <span data-ttu-id="089cd-131">Den dator som det här skriptet körs på behöver inte vara en del av klustret.</span><span class="sxs-lookup"><span data-stu-id="089cd-131">The machine that this script is run on does not have to be part of the cluster.</span></span>

    ```powershell

    ###### connect to the secure cluster using certs
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

2. <span data-ttu-id="089cd-132">Hämta en lista över Service Fabric-versioner som du kan uppgradera till.</span><span class="sxs-lookup"><span data-stu-id="089cd-132">Get the list of Service Fabric versions that you can upgrade to.</span></span>

    ```powershell

    ###### Get the list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    <span data-ttu-id="089cd-133">Du bör få ett utgående ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="089cd-133">You should get an output similar to this:</span></span>

    ![Hämta fabric versioner][getfabversions]
3. <span data-ttu-id="089cd-135">Starta en klusteruppgradering till en version som är tillgängliga med hjälp av den [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell cmd.</span><span class="sxs-lookup"><span data-stu-id="089cd-135">Start a cluster upgrade to an available version by using the [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell cmd.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="089cd-136">Du kan använda Service Fabric-Utforskaren eller kör följande Windows PowerShell-kommando för att övervaka förloppet för uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="089cd-136">To monitor the progress of the upgrade, you can use Service Fabric Explorer or run the following Windows PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="089cd-137">Uppgraderingen återställs om hälsoprinciper klustret inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="089cd-137">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="089cd-138">Ange anpassade hälsoprinciper för den **Start ServiceFabricClusterUpgrade** finns i dokumentationen för [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="089cd-138">To specify custom health policies for the **Start-ServiceFabricClusterUpgrade** command, see documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="089cd-139">När du har åtgärdat problemen som resulterade i återställningen kan du initiera uppgraderingen igen genom att följa samma steg som beskrivits tidigare.</span><span class="sxs-lookup"><span data-stu-id="089cd-139">After you fix the issues that resulted in the rollback, initiate the upgrade again by following the same steps as previously described.</span></span>

### <a name="upgrade-clusters-that-have-uno-connectivityu-to-download-the-latest-code-and-configuration"></a><span data-ttu-id="089cd-140">Uppgradera kluster som har <U>ingen nätverksanslutning</u> att ladda ned de senaste kod och konfiguration</span><span class="sxs-lookup"><span data-stu-id="089cd-140">Upgrade clusters that have <U>no connectivity</u> to download the latest code and configuration</span></span>
<span data-ttu-id="089cd-141">Följ dessa steg för att uppgradera ditt kluster till en version som stöds om klusternoderna inte har Internetanslutning till [http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="089cd-141">Use these steps to upgrade your cluster to a supported version if your cluster nodes do not have Internet connectivity to [http://download.microsoft.com](http://download.microsoft.com).</span></span>

> [!NOTE]
> <span data-ttu-id="089cd-142">Om du använder ett kluster som inte är ansluten till Internet, behöver du övervaka Service Fabric-teamets blogg att lära dig om en ny version.</span><span class="sxs-lookup"><span data-stu-id="089cd-142">If you are running a cluster that is not connected to the Internet, you will have to monitor the Service Fabric team blog to learn about a new release.</span></span> <span data-ttu-id="089cd-143">Systemet visar inte ett kluster hälsotillstånd varning att varna dig för en ny version.</span><span class="sxs-lookup"><span data-stu-id="089cd-143">The system does not show a cluster health warning to alert you of a new release.</span></span>  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a><span data-ttu-id="089cd-144">Automatisk etablering jämfört med manuella etablering</span><span class="sxs-lookup"><span data-stu-id="089cd-144">Auto Provisioning vs Manual Provisioning</span></span>
<span data-ttu-id="089cd-145">Om du vill aktivera automatisk överföring och registrering för den senaste versionen av koden, ställa in Service Fabric-uppdateringstjänsten.</span><span class="sxs-lookup"><span data-stu-id="089cd-145">To enable automatic downloading and registration for the latest code version, set up Service Fabric Update Service.</span></span> <span data-ttu-id="089cd-146">Referera till Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt inuti den [fristående paketet](service-fabric-cluster-standalone-package-contents.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="089cd-146">Refer to the Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt inside the [Standalone Package](service-fabric-cluster-standalone-package-contents.md) for instructions.</span></span>
<span data-ttu-id="089cd-147">Följ anvisningarna nedan för manuell process.</span><span class="sxs-lookup"><span data-stu-id="089cd-147">For manual process, follow the instructions below.</span></span>

<span data-ttu-id="089cd-148">Ändra klusterkonfigurationen för att ange följande egenskap till false innan du påbörjar en uppgradering av konfiguration.</span><span class="sxs-lookup"><span data-stu-id="089cd-148">Modify your cluster configuration to set the following property to false before you start a configuration upgrade.</span></span>

        "fabricClusterAutoupgradeEnabled": false,

<span data-ttu-id="089cd-149">Referera till [Start ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) för användningsinformation.</span><span class="sxs-lookup"><span data-stu-id="089cd-149">Refer to [Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) for usage details.</span></span> <span data-ttu-id="089cd-150">Se till att uppdatera clusterConfigurationVersion om du i din JSON innan du börjar uppgradera konfiguration.</span><span class="sxs-lookup"><span data-stu-id="089cd-150">Make sure to update 'clusterConfigurationVersion' in your JSON before you start the configuration upgrade.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="089cd-151">Uppgradera arbetsflödet för kluster</span><span class="sxs-lookup"><span data-stu-id="089cd-151">Cluster upgrade workflow</span></span>

1. <span data-ttu-id="089cd-152">Kör Get-ServiceFabricClusterUpgrade från en av noderna i klustret och notera TargetCodeVersion.</span><span class="sxs-lookup"><span data-stu-id="089cd-152">Run Get-ServiceFabricClusterUpgrade from one of the nodes in the cluster and note the TargetCodeVersion.</span></span>
2. <span data-ttu-id="089cd-153">Kör följande från en internet-ansluten dator att visa en lista med alla uppgradera kompatibla versioner med den aktuella versionen och hämta motsvarande paketet från de associera länkarna.</span><span class="sxs-lookup"><span data-stu-id="089cd-153">Run the following from an internet connected machine to list all upgrade compatible versions with the current version and download the corresponding package from the associated download links.</span></span>

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. <span data-ttu-id="089cd-154">Ansluta till klustret från en dator som har administratörsåtkomst till alla datorer som listas som noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="089cd-154">Connect to the cluster from any machine that has administrator access to all the machines that are listed as nodes in the cluster.</span></span> <span data-ttu-id="089cd-155">Den dator som det här skriptet körs på behöver inte vara en del av klustret</span><span class="sxs-lookup"><span data-stu-id="089cd-155">The machine that this script is run on does not have to be part of the cluster</span></span>

    ```powershell

   ###### Get the list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. <span data-ttu-id="089cd-156">Kopiera det Hämta paketet till klustret image store.</span><span class="sxs-lookup"><span data-stu-id="089cd-156">Copy the downloaded package into the cluster image store.</span></span>

5. <span data-ttu-id="089cd-157">Registrera kopierade paketet.</span><span class="sxs-lookup"><span data-stu-id="089cd-157">Register the copied package.</span></span>

    ```powershell

    ###### Get the list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. <span data-ttu-id="089cd-158">Starta en klusteruppgradering till en version som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="089cd-158">Start a cluster upgrade to an available version.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="089cd-159">Du kan övervaka förloppet för uppgraderingen på Service Fabric-Utforskaren eller genom att köra följande PowerShell-kommando.</span><span class="sxs-lookup"><span data-stu-id="089cd-159">You can monitor the progress of the upgrade on Service Fabric Explorer, or you can run the following PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="089cd-160">Uppgraderingen återställs om hälsoprinciper klustret inte är uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="089cd-160">If the cluster health policies are not met, the upgrade is rolled back.</span></span> <span data-ttu-id="089cd-161">Ange anpassade hälsoprinciper för den **Start ServiceFabricClusterUpgrade** finns i dokumentationen för [Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="089cd-161">To specify custom health policies for the **Start-ServiceFabricClusterUpgrade** command, see the documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="089cd-162">När du har åtgärdat problemen som resulterade i återställningen kan du initiera uppgraderingen igen genom att följa samma steg som beskrivits tidigare.</span><span class="sxs-lookup"><span data-stu-id="089cd-162">After you fix the issues that resulted in the rollback, initiate the upgrade again by following the same steps as previously described.</span></span>


## <a name="upgrade-the-cluster-configuration"></a><span data-ttu-id="089cd-163">Uppgradera klusterkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="089cd-163">Upgrade the cluster configuration</span></span>
<span data-ttu-id="089cd-164">Innan du startar uppgraderingen konfiguration kan du testa din nya kluster configuration json genom att köra PowerShell.skript i fristående paketet.</span><span class="sxs-lookup"><span data-stu-id="089cd-164">Before you initiate the configuration upgrade, you can test your new cluster configuration json by running the powershell script in the standalone package.</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>

```
<span data-ttu-id="089cd-165">eller</span><span class="sxs-lookup"><span data-stu-id="089cd-165">or</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>

```

<span data-ttu-id="089cd-166">Vissa konfigurationer måste uppgraderas, till exempel slutpunkter, klusternamnet, noden IP osv. Detta kommer att testa nya kluster configuration json mot den gamla servern och utlösa fel i Powershell-fönstret om det inte finns några problem.</span><span class="sxs-lookup"><span data-stu-id="089cd-166">Some configurations can't be upgraded, such as endpoints, cluster name, node IP, etc. This will test the new cluster configuration json against the old one and throw errors in the Powershell window if there is any issue.</span></span>

<span data-ttu-id="089cd-167">Om du vill uppgradera configuration klusteruppgradering kör **Start ServiceFabricClusterConfigurationUpgrade**.</span><span class="sxs-lookup"><span data-stu-id="089cd-167">To upgrade the cluster configuration upgrade, run **Start-ServiceFabricClusterConfigurationUpgrade**.</span></span> <span data-ttu-id="089cd-168">Konfiguration av uppgraderingen är bearbetade uppgraderingsdomän av uppgraderingsdomänen.</span><span class="sxs-lookup"><span data-stu-id="089cd-168">The configuration upgrade is processed upgrade domain by upgrade domain.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>

```

### <a name="cluster-certificate-config-upgrade"></a><span data-ttu-id="089cd-169">Klusteruppgradering certifikat config</span><span class="sxs-lookup"><span data-stu-id="089cd-169">Cluster certificate config upgrade</span></span>  
<span data-ttu-id="089cd-170">Klustret certifikatet används för autentisering mellan klusternoder, så certifikatet sammanslagning via ska utföras med extra försiktig eftersom fel blockerar kommunikation mellan klusternoder.</span><span class="sxs-lookup"><span data-stu-id="089cd-170">Cluster certificate is used for authentication between cluster nodes, so the certificate roll over should be performed with extra caution because failure will block the communication among cluster nodes.</span></span>  
<span data-ttu-id="089cd-171">Tekniskt sett stöds tre alternativ:</span><span class="sxs-lookup"><span data-stu-id="089cd-171">Technically, three options are supported:</span></span>  

1. <span data-ttu-id="089cd-172">Enskilt certifikat uppgraderingen: sökvägen för uppgraderingen är ' certifikat C (primär)-certifikat (primär) -> certifikatet B (primär) > ->... ”.</span><span class="sxs-lookup"><span data-stu-id="089cd-172">Single certificate upgrade: The upgrade path is 'Certificate A (Primary) -> Certificate B (Primary) -> Certificate C (Primary) -> ...'.</span></span>   
2. <span data-ttu-id="089cd-173">Dubbla certifikat uppgraderingen: sökvägen för uppgraderingen är ' B (sekundär) -> certifikatet B (primär) och certifikat (primär) -> certifikat (primär) > certifikatet B (primär) och C (sekundär) -> certifikatet C (primär) ->... ”.</span><span class="sxs-lookup"><span data-stu-id="089cd-173">Double certificate upgrade: The upgrade path is 'Certificate A (Primary) -> Certificate A (Primary) and B (Secondary) -> Certificate B (Primary) -> Certificate B (Primary) and C (Secondary) -> Certificate C (Primary) -> ...'.</span></span>
3. <span data-ttu-id="089cd-174">Av Typuppgradering av certifikat: tumavtryck-baserade certifikat <>--Vanligtnamn-baserade Certifikatkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="089cd-174">Certificate type upgrade: Thumbprint-based certificate configuration <-> CommonName-based certificate configuration.</span></span> <span data-ttu-id="089cd-175">Till exempel certifikatets tumavtryck (primär) och tumavtrycket B (sekundär) -> certifikatet CommonName C.</span><span class="sxs-lookup"><span data-stu-id="089cd-175">For example, Certificate Thumbprint A (Primary) and Thumbprint B (Secondary) -> Certificate CommonName C.</span></span>


## <a name="next-steps"></a><span data-ttu-id="089cd-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="089cd-176">Next steps</span></span>
* <span data-ttu-id="089cd-177">Lär dig hur du anpassar vissa [Service Fabric-klusterinställningar](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="089cd-177">Learn how to customize some [Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>
* <span data-ttu-id="089cd-178">Lär dig hur du [skala ditt kluster in och ut](service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="089cd-178">Learn how to [scale your cluster in and out](service-fabric-cluster-scale-up-down.md).</span></span>
* <span data-ttu-id="089cd-179">Lär dig mer om [programuppgraderingar](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="089cd-179">Learn about [application upgrades](service-fabric-application-upgrade.md).</span></span>

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
