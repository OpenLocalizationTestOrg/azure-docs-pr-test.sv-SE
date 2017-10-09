---
title: aaaAzure Service Fabric korrigering orchestration program | Microsoft Docs
description: "Programmet tooautomate operativsystem system korrigering på ett Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: 
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/9/2017
ms.author: nachandr
ms.openlocfilehash: fbb89aa2ea418181ee908a01850178c113c462fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="patch-hello-windows-operating-system-in-your-service-fabric-cluster"></a><span data-ttu-id="00922-103">Korrigering av hello Windows-operativsystem i Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="00922-103">Patch hello Windows operating system in your Service Fabric cluster</span></span>

<span data-ttu-id="00922-104">hello korrigering orchestration programmet är ett Azure Service Fabric som automatiserar operativsystemet korrigering på ett Service Fabric-kluster i Azure utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="00922-104">hello patch orchestration application is an Azure Service Fabric application that automates operating system patching on a Service Fabric cluster on Azure without downtime.</span></span>

<span data-ttu-id="00922-105">hello korrigering orchestration app innehåller hello följande:</span><span class="sxs-lookup"><span data-stu-id="00922-105">hello patch orchestration app provides hello following:</span></span>

- <span data-ttu-id="00922-106">**Automatisk uppdatering operativsysteminstallation**.</span><span class="sxs-lookup"><span data-stu-id="00922-106">**Automatic operating system update installation**.</span></span> <span data-ttu-id="00922-107">Uppdateringar av operativsystemet hämtas och installeras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="00922-107">Operating system updates are automatically downloaded and installed.</span></span> <span data-ttu-id="00922-108">Noder i klustret startas om vid behov utan klusterdriftstopp.</span><span class="sxs-lookup"><span data-stu-id="00922-108">Cluster nodes are rebooted as needed without cluster downtime.</span></span>

- <span data-ttu-id="00922-109">**Klustermedveten uppdatering och hälsa integration**.</span><span class="sxs-lookup"><span data-stu-id="00922-109">**Cluster-aware patching and health integration**.</span></span> <span data-ttu-id="00922-110">När uppdateringar, övervakar hello korrigering orchestration app hello hälsotillstånd hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="00922-110">While applying updates, hello patch orchestration app monitors hello health of hello cluster nodes.</span></span> <span data-ttu-id="00922-111">Klusternoder är uppgraderade en nod i taget eller en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="00922-111">Cluster nodes are upgraded one node at a time or one upgrade domain at a time.</span></span> <span data-ttu-id="00922-112">Om hello hälsotillstånd hello klustret stängs av på grund av toohello uppdatering process, är korrigering stoppad tooprevent aggravating hello problem.</span><span class="sxs-lookup"><span data-stu-id="00922-112">If hello health of hello cluster goes down due toohello patching process, patching is stopped tooprevent aggravating hello problem.</span></span>

## <a name="internal-details-of-hello-app"></a><span data-ttu-id="00922-113">Intern information hello-appen</span><span class="sxs-lookup"><span data-stu-id="00922-113">Internal details of hello app</span></span>

<span data-ttu-id="00922-114">hello korrigering orchestration app består av följande delkomponenter hello:</span><span class="sxs-lookup"><span data-stu-id="00922-114">hello patch orchestration app is composed of hello following subcomponents:</span></span>

- <span data-ttu-id="00922-115">**Coordinator-tjänsten**: tillståndskänslig tjänsten ansvarar för att:</span><span class="sxs-lookup"><span data-stu-id="00922-115">**Coordinator Service**: This stateful service is responsible for:</span></span>
    - <span data-ttu-id="00922-116">Samordna hello Windows Update-jobbet på hello hela klustret.</span><span class="sxs-lookup"><span data-stu-id="00922-116">Coordinating hello Windows Update job on hello entire cluster.</span></span>
    - <span data-ttu-id="00922-117">Lagra hello resultatet av slutförda Windows Update-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="00922-117">Storing hello result of completed Windows Update operations.</span></span>
- <span data-ttu-id="00922-118">**Nod-agenttjänsten**: tillståndslösa tjänsten körs på alla klusternoder för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="00922-118">**Node Agent Service**: This stateless service runs on all Service Fabric cluster nodes.</span></span> <span data-ttu-id="00922-119">hello tjänsten ansvarar för:</span><span class="sxs-lookup"><span data-stu-id="00922-119">hello service is responsible for:</span></span>
    - <span data-ttu-id="00922-120">Startprogram för hello nod Agent NTService.</span><span class="sxs-lookup"><span data-stu-id="00922-120">Bootstrapping hello Node Agent NTService.</span></span>
    - <span data-ttu-id="00922-121">Övervakning av hello nod Agent NTService.</span><span class="sxs-lookup"><span data-stu-id="00922-121">Monitoring hello Node Agent NTService.</span></span>
- <span data-ttu-id="00922-122">**Noden Agent NTService**: den här Windows NT-tjänsten körs på en högre nivå behörighet (SYSTEM).</span><span class="sxs-lookup"><span data-stu-id="00922-122">**Node Agent NTService**: This Windows NT service runs at a higher-level privilege (SYSTEM).</span></span> <span data-ttu-id="00922-123">Däremot körs hello nod Agent-tjänsten och hello Coordinator-tjänsten på en lägre nivå behörighet (NETWORK SERVICE).</span><span class="sxs-lookup"><span data-stu-id="00922-123">In contrast, hello Node Agent Service and hello Coordinator Service run at a lower-level privilege (NETWORK SERVICE).</span></span> <span data-ttu-id="00922-124">hello tjänsten ansvarar för att utföra hello följa Windows Update-jobb på alla noder i klustret hello:</span><span class="sxs-lookup"><span data-stu-id="00922-124">hello service is responsible for performing hello following Windows Update jobs on all hello cluster nodes:</span></span>
    - <span data-ttu-id="00922-125">Inaktiverar automatisk Windows Update på hello-nod.</span><span class="sxs-lookup"><span data-stu-id="00922-125">Disabling automatic Windows Update on hello node.</span></span>
    - <span data-ttu-id="00922-126">Hämta och installera Windows Update har enligt toohello princip hello användare angetts.</span><span class="sxs-lookup"><span data-stu-id="00922-126">Downloading and installing Windows Update according toohello policy hello user has provided.</span></span>
    - <span data-ttu-id="00922-127">Starta om hello datorn efter installationen av Windows Update.</span><span class="sxs-lookup"><span data-stu-id="00922-127">Restarting hello machine post Windows Update installation.</span></span>
    - <span data-ttu-id="00922-128">Överför hello resultat av Windows-uppdateringar toohello Coordinator-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="00922-128">Uploading hello results of Windows updates toohello Coordinator Service.</span></span>
    - <span data-ttu-id="00922-129">Reporting systemhälsorapporter om en åtgärd misslyckades efter överbelastar alla nya försök.</span><span class="sxs-lookup"><span data-stu-id="00922-129">Reporting health reports in case an operation has failed after exhausting all retries.</span></span>

> [!NOTE]
> <span data-ttu-id="00922-130">hello korrigering orchestration app använder hello Service Fabric reparera manager system service toodisable eller aktivera hello nod och utföra hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="00922-130">hello patch orchestration app uses hello Service Fabric repair manager system service toodisable or enable hello node and perform health checks.</span></span> <span data-ttu-id="00922-131">hello reparera aktivitet som skapats av hello korrigering orchestration app spårar hello Windows Update status för varje nod.</span><span class="sxs-lookup"><span data-stu-id="00922-131">hello repair task created by hello patch orchestration app tracks hello Windows Update progress for each node.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00922-132">Krav</span><span class="sxs-lookup"><span data-stu-id="00922-132">Prerequisites</span></span>

### <a name="minimum-supported-service-fabric-runtime-version"></a><span data-ttu-id="00922-133">Minimal stödd version av Service Fabric runtime</span><span class="sxs-lookup"><span data-stu-id="00922-133">Minimum supported Service Fabric runtime version</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="00922-134">Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="00922-134">Azure clusters</span></span>
<span data-ttu-id="00922-135">hello korrigering orchestration app måste köras på Azure kluster som har Service Fabric runtime version version 5.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="00922-135">hello patch orchestration app must be run on Azure clusters that have Service Fabric runtime version v5.5 or later.</span></span>

#### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="00922-136">Fristående lokala kluster</span><span class="sxs-lookup"><span data-stu-id="00922-136">Standalone on-premises Clusters</span></span>
<span data-ttu-id="00922-137">hello korrigering orchestration app måste köras på fristående kluster som har Service Fabric runtime version v5.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="00922-137">hello patch orchestration app must be run on Standalone clusters that have Service Fabric runtime version v5.6 or later.</span></span>

### <a name="enable-hello-repair-manager-service-if-its-not-running-already"></a><span data-ttu-id="00922-138">Aktivera hello reparera manager-tjänsten (om den inte redan körs)</span><span class="sxs-lookup"><span data-stu-id="00922-138">Enable hello repair manager service (if it's not running already)</span></span>

<span data-ttu-id="00922-139">hello korrigering orchestration appen kräver hello reparera manager system service toobe aktiverad på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="00922-139">hello patch orchestration app requires hello repair manager system service toobe enabled on hello cluster.</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="00922-140">Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="00922-140">Azure clusters</span></span>

<span data-ttu-id="00922-141">Azure-kluster i hello silver hållbarhetsnivån ha hello reparera manager-tjänsten som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="00922-141">Azure clusters in hello silver durability tier have hello repair manager service enabled by default.</span></span> <span data-ttu-id="00922-142">Azure-kluster i hello guld hållbarhetsnivån kanske eller kanske inte hello manager reparation aktiverad, beroende på när dessa kluster skapades.</span><span class="sxs-lookup"><span data-stu-id="00922-142">Azure clusters in hello gold durability tier might or might not have hello repair manager service enabled, depending on when those clusters were created.</span></span> <span data-ttu-id="00922-143">Azure-kluster i hello Brons hållbarhetsnivån, som standard inte hello reparera manager-tjänsten aktiverad.</span><span class="sxs-lookup"><span data-stu-id="00922-143">Azure clusters in hello bronze durability tier, by default, do not have hello repair manager service enabled.</span></span> <span data-ttu-id="00922-144">Om hello-tjänsten redan är aktiverat, kan du se den i avsnittet hello system services i hello Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="00922-144">If hello service is already enabled, you can see it running in hello system services section in hello Service Fabric Explorer.</span></span>

##### <a name="azure-portal"></a><span data-ttu-id="00922-145">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="00922-145">Azure portal</span></span>
<span data-ttu-id="00922-146">Du kan aktivera reparera manager från Azure-portalen för närvarande hello för att skapa klustret.</span><span class="sxs-lookup"><span data-stu-id="00922-146">You can enable repair manager from Azure portal at hello time of setting up of cluster.</span></span> <span data-ttu-id="00922-147">Välj `Include Repair Manager` alternativ `Add on features` när hello klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="00922-147">Select `Include Repair Manager` option under `Add on features` at hello time of Cluster configuration.</span></span>
<span data-ttu-id="00922-148">![Bild för att aktivera reparera Manager från Azure-portalen](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span><span class="sxs-lookup"><span data-stu-id="00922-148">![Image of Enabling Repair Manager from Azure portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span></span>

##### <a name="azure-resource-manager-template"></a><span data-ttu-id="00922-149">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="00922-149">Azure Resource Manager template</span></span>
<span data-ttu-id="00922-150">Du kan också använda hello [Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) tooenable hello reparera manager-tjänsten på nya och befintliga Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="00922-150">Alternatively you can use hello [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) tooenable hello repair manager service on new and existing Service Fabric clusters.</span></span> <span data-ttu-id="00922-151">Hämta hello mall för hello kluster som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="00922-151">Get hello template for hello cluster that you want toodeploy.</span></span> <span data-ttu-id="00922-152">Du kan använda hello exempelmallarna, eller så kan du skapa en anpassad mall för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="00922-152">You can either use hello sample templates or create a custom Resource Manager template.</span></span> 

<span data-ttu-id="00922-153">tooenable hello reparera manager service med [Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span><span class="sxs-lookup"><span data-stu-id="00922-153">tooenable hello repair manager service using [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span></span>

1. <span data-ttu-id="00922-154">Kontrollera först att hello `apiversion` har angetts för`2017-07-01-preview` för hello `Microsoft.ServiceFabric/clusters` resurs, som visas i följande fragment hello.</span><span class="sxs-lookup"><span data-stu-id="00922-154">First check that hello `apiversion` is set too`2017-07-01-preview` for hello `Microsoft.ServiceFabric/clusters` resource, as shown in hello following snippet.</span></span> <span data-ttu-id="00922-155">Om det skiljer sig, måste du tooupdate hello `apiVersion` toohello värdet `2017-07-01-preview`:</span><span class="sxs-lookup"><span data-stu-id="00922-155">If it is different, then you need tooupdate hello `apiVersion` toohello value `2017-07-01-preview`:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="00922-156">Nu aktivera hello reparera manager-tjänsten genom att lägga till följande hello `addonFeatures` efter hello `fabricSettings` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="00922-156">Now enable hello repair manager service by adding hello following `addonFeatures` section after hello `fabricSettings` section:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="00922-157">När du har uppdaterat mallen för klustret med de här ändringarna, använda dem och låt hello uppgraderingen är klar.</span><span class="sxs-lookup"><span data-stu-id="00922-157">After you have updated your cluster template with these changes, apply them and let hello upgrade finish.</span></span> <span data-ttu-id="00922-158">Du kan nu se hello reparera manager system-tjänsten körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="00922-158">You can now see hello repair manager system service running in your cluster.</span></span> <span data-ttu-id="00922-159">Den kallas `fabric:/System/RepairManagerService` under hello system tjänster i hello Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="00922-159">It is called `fabric:/System/RepairManagerService` in hello system services section in hello Service Fabric Explorer.</span></span> 

### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="00922-160">Fristående lokala kluster</span><span class="sxs-lookup"><span data-stu-id="00922-160">Standalone on-premises clusters</span></span>

<span data-ttu-id="00922-161">Du kan använda hello [konfigurationsinställningar för Windows-kluster för fristående](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) tooenable hello reparera manager-tjänsten på nya och befintliga Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="00922-161">You can use hello [Configuration settings for standalone Windows cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) tooenable hello repair manager service on new and existing Service Fabric cluster.</span></span>

<span data-ttu-id="00922-162">tooenable hello reparera manager-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="00922-162">tooenable hello repair manager service:</span></span>

1. <span data-ttu-id="00922-163">Kontrollera först att hello `apiversion` i [allmänna klusterkonfigurationer](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) har angetts för`04-2017` eller senare:</span><span class="sxs-lookup"><span data-stu-id="00922-163">First check that hello `apiversion` in [General cluster configurations](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) is set too`04-2017` or higher:</span></span>

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. <span data-ttu-id="00922-164">Aktivera nu reparera manager-tjänsten genom att lägga till följande hello `addonFeaturres` efter hello `fabricSettings` enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="00922-164">Now enable repair manager service by adding hello following `addonFeaturres` section after hello `fabricSettings` section as shown below:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="00922-165">Uppdatera din klustermanifestet med dessa ändringar, med hello uppdateras klustermanifestet [skapa ett nytt kluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) eller [uppgradera hello klusterkonfigurationen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span><span class="sxs-lookup"><span data-stu-id="00922-165">Update your cluster manifest with these changes, using hello updated cluster manifest [create a new cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) or [upgrade hello cluster configuration](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span></span> <span data-ttu-id="00922-166">När hello kluster körs med uppdaterade klustermanifestet, du kan nu se hello reparera manager system-tjänsten körs i klustret, som kallas `fabric:/System/RepairManagerService`under system services-avsnittet i hello Service Fabric explorer.</span><span class="sxs-lookup"><span data-stu-id="00922-166">Once hello cluster is running with updated cluster manifest, you can now see hello repair manager system service running in your cluster, which is called `fabric:/System/RepairManagerService`, under system services section in hello Service Fabric explorer.</span></span>

### <a name="disable-automatic-windows-update-on-all-nodes"></a><span data-ttu-id="00922-167">Inaktivera automatisk uppdatering för Windows på alla noder</span><span class="sxs-lookup"><span data-stu-id="00922-167">Disable automatic Windows Update on all nodes</span></span>

<span data-ttu-id="00922-168">Automatiska uppdateringar kan leda tooavailability förlust eftersom flera klusternoder kan du starta om hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="00922-168">Automatic Windows updates might lead tooavailability loss because multiple cluster nodes can restart at hello same time.</span></span> <span data-ttu-id="00922-169">hello korrigering orchestration app som standard försöker toodisable hello automatisk Windows Update på varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="00922-169">hello patch orchestration app, by default, tries toodisable hello automatic Windows Update on each cluster node.</span></span> <span data-ttu-id="00922-170">Vi rekommenderar dock inställningen hello Windows Update-princip för ”avisering före hämtningen” uttryckligen om hello inställningarna hanteras av en administratör eller en Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="00922-170">However, if hello settings are managed by an administrator or group policy, we recommend setting hello Windows Update policy too“Notify before Download” explicitly.</span></span>

### <a name="optional-enable-azure-diagnostics"></a><span data-ttu-id="00922-171">Valfritt: Aktivera Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="00922-171">Optional: Enable Azure Diagnostics</span></span>

<span data-ttu-id="00922-172">Kluster som kör Service Fabric runtime version `5.6.220.9494` och ovan samla in korrigering orchestration app loggarna som en del av Service Fabric-loggarna.</span><span class="sxs-lookup"><span data-stu-id="00922-172">Clusters running Service Fabric runtime version `5.6.220.9494` and above collect patch orchestration app logs as part of Service Fabric logs.</span></span>
<span data-ttu-id="00922-173">Du kan hoppa över det här steget om klustret körs i Service Fabric runtime version `5.6.220.9494` och högre.</span><span class="sxs-lookup"><span data-stu-id="00922-173">You can skip this step if your cluster is running on Service Fabric runtime version `5.6.220.9494` and above.</span></span>

<span data-ttu-id="00922-174">För kluster som kör Service Fabric runtime version mindre än `5.6.220.9494`, loggar för hello korrigering orchestration app samlas lokalt på varje hello klusternoder.</span><span class="sxs-lookup"><span data-stu-id="00922-174">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs for hello patch orchestration app are collected locally on each of hello cluster nodes.</span></span>
<span data-ttu-id="00922-175">Vi rekommenderar att du konfigurerar Azure Diagnostics tooupload loggar från alla noder tooa central plats.</span><span class="sxs-lookup"><span data-stu-id="00922-175">We recommend that you configure Azure Diagnostics tooupload logs from all nodes tooa central location.</span></span>

<span data-ttu-id="00922-176">Information om hur du aktiverar Azure-diagnostik finns [samla in loggar med hjälp av Azure-diagnostik](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span><span class="sxs-lookup"><span data-stu-id="00922-176">For information on enabling Azure Diagnostics, see [Collect logs by using Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span></span>

<span data-ttu-id="00922-177">Loggar för hello korrigering orchestration app genereras på hello följande fasta providern ID: N:</span><span class="sxs-lookup"><span data-stu-id="00922-177">Logs for hello patch orchestration app are generated on hello following fixed provider IDs:</span></span>

- <span data-ttu-id="00922-178">e39b723c-590c-4090-abb0-11e3e6616346</span><span class="sxs-lookup"><span data-stu-id="00922-178">e39b723c-590c-4090-abb0-11e3e6616346</span></span>
- <span data-ttu-id="00922-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span><span class="sxs-lookup"><span data-stu-id="00922-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span></span>
- <span data-ttu-id="00922-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span><span class="sxs-lookup"><span data-stu-id="00922-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span></span>
- <span data-ttu-id="00922-181">05dc046c-60e9-4ef7-965e-91660adffa68</span><span class="sxs-lookup"><span data-stu-id="00922-181">05dc046c-60e9-4ef7-965e-91660adffa68</span></span>

<span data-ttu-id="00922-182">I Resource Manager mallen goto `EtwEventSourceProviderConfiguration` avsnittet `WadCfg` och Lägg till följande poster hello:</span><span class="sxs-lookup"><span data-stu-id="00922-182">In Resource Manager template goto `EtwEventSourceProviderConfiguration` section under `WadCfg` and add hello following entries:</span></span>

```json
  {
    "provider": "e39b723c-590c-4090-abb0-11e3e6616346",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
      "eventDestination": "PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "fc0028ff-bfdc-499f-80dc-ed922c52c5e9",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "24afa313-0d3b-4c7c-b485-1047fd964b60",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "05dc046c-60e9-4ef7-965e-91660adffa68",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  }
```

> [!NOTE]
> <span data-ttu-id="00922-183">Om din Service Fabric-klustret har flera nodtyper och sedan hello föregående avsnitt måste läggas till för alla hello `WadCfg` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="00922-183">If your Service Fabric cluster has multiple node types, then hello previous section must be added for all hello `WadCfg` sections.</span></span>

## <a name="download-hello-app-package"></a><span data-ttu-id="00922-184">Hämta hello app-paket</span><span class="sxs-lookup"><span data-stu-id="00922-184">Download hello app package</span></span>

<span data-ttu-id="00922-185">Hämta hello program från hello [Hämta länk](https://go.microsoft.com/fwlink/P/?linkid=849590).</span><span class="sxs-lookup"><span data-stu-id="00922-185">Download hello application from hello [download link](https://go.microsoft.com/fwlink/P/?linkid=849590).</span></span>

## <a name="configure-hello-app"></a><span data-ttu-id="00922-186">Konfigurera hello app</span><span class="sxs-lookup"><span data-stu-id="00922-186">Configure hello app</span></span>

<span data-ttu-id="00922-187">Hej hello korrigering orchestration appens beteende kan vara konfigurerade toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="00922-187">hello behavior of hello patch orchestration app can be configured toomeet your needs.</span></span> <span data-ttu-id="00922-188">Åsidosätta standardvärden för hello genom att passera i hello programmet parametern under skapa program eller uppdatering.</span><span class="sxs-lookup"><span data-stu-id="00922-188">Override hello default values by passing in hello application parameter during application creation or update.</span></span> <span data-ttu-id="00922-189">Parametrar för program kan anges genom att ange `ApplicationParameter` toohello `Start-ServiceFabricApplicationUpgrade` eller `New-ServiceFabricApplication` cmdlets.</span><span class="sxs-lookup"><span data-stu-id="00922-189">Application parameters can be provided by specifying `ApplicationParameter` toohello `Start-ServiceFabricApplicationUpgrade` or `New-ServiceFabricApplication` cmdlets.</span></span>

|<span data-ttu-id="00922-190">**Parametern**</span><span class="sxs-lookup"><span data-stu-id="00922-190">**Parameter**</span></span>        |<span data-ttu-id="00922-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="00922-191">**Type**</span></span>                          | <span data-ttu-id="00922-192">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="00922-192">**Details**</span></span>|
|:-|-|-|
|<span data-ttu-id="00922-193">MaxResultsToCache</span><span class="sxs-lookup"><span data-stu-id="00922-193">MaxResultsToCache</span></span>    |<span data-ttu-id="00922-194">Lång</span><span class="sxs-lookup"><span data-stu-id="00922-194">Long</span></span>                              | <span data-ttu-id="00922-195">Maximalt antal Windows Update-resultat, som ska cachelagras.</span><span class="sxs-lookup"><span data-stu-id="00922-195">Maximum number of Windows Update results, which should be cached.</span></span> <br><span data-ttu-id="00922-196">Standardvärdet är 3000 under förutsättning att den:</span><span class="sxs-lookup"><span data-stu-id="00922-196">Default value is 3000 assuming the:</span></span> <br> <span data-ttu-id="00922-197">-Antalet noder är 20.</span><span class="sxs-lookup"><span data-stu-id="00922-197">- Number of nodes is 20.</span></span> <br> <span data-ttu-id="00922-198">-Antalet uppdateringar som händer på en nod per månad är fem.</span><span class="sxs-lookup"><span data-stu-id="00922-198">- Number of updates happening on a node per month is five.</span></span> <br> <span data-ttu-id="00922-199">-Antalet resultat per åtgärd kan vara 10.</span><span class="sxs-lookup"><span data-stu-id="00922-199">- Number of results per operation can be 10.</span></span> <br> <span data-ttu-id="00922-200">-Resultat för hello senaste tre månaderna ska lagras.</span><span class="sxs-lookup"><span data-stu-id="00922-200">- Results for hello past three months should be stored.</span></span> |
|<span data-ttu-id="00922-201">TaskApprovalPolicy</span><span class="sxs-lookup"><span data-stu-id="00922-201">TaskApprovalPolicy</span></span>   |<span data-ttu-id="00922-202">Enum</span><span class="sxs-lookup"><span data-stu-id="00922-202">Enum</span></span> <br> <span data-ttu-id="00922-203">{NodeWise, UpgradeDomainWise}</span><span class="sxs-lookup"><span data-stu-id="00922-203">{ NodeWise, UpgradeDomainWise }</span></span>                          |<span data-ttu-id="00922-204">TaskApprovalPolicy anger hello policy som är toobe som används för uppdateringar för Windows hello Coordinator-tjänsten tooinstall hello Service Fabric-noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="00922-204">TaskApprovalPolicy indicates hello policy that is toobe used by hello Coordinator Service tooinstall Windows updates across hello Service Fabric cluster nodes.</span></span><br>                         <span data-ttu-id="00922-205">Tillåtna värden är:</span><span class="sxs-lookup"><span data-stu-id="00922-205">Allowed values are:</span></span> <br>                                                           <span data-ttu-id="00922-206"><b>NodeWise</b>.</span><span class="sxs-lookup"><span data-stu-id="00922-206"><b>NodeWise</b>.</span></span> <span data-ttu-id="00922-207">Windows Update är installerade en nod i taget.</span><span class="sxs-lookup"><span data-stu-id="00922-207">Windows Update is installed one node at a time.</span></span> <br>                                                           <span data-ttu-id="00922-208"><b>UpgradeDomainWise</b>.</span><span class="sxs-lookup"><span data-stu-id="00922-208"><b>UpgradeDomainWise</b>.</span></span> <span data-ttu-id="00922-209">Windows Update är installerade en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="00922-209">Windows Update is installed one upgrade domain at a time.</span></span> <span data-ttu-id="00922-210">(På hello maximala, alla hello noder som tillhör tooan uppgraderingsdomänen kan gå för Windows Update.)</span><span class="sxs-lookup"><span data-stu-id="00922-210">(At hello maximum, all hello nodes belonging tooan upgrade domain can go for Windows Update.)</span></span>
|<span data-ttu-id="00922-211">LogsDiskQuotaInMB</span><span class="sxs-lookup"><span data-stu-id="00922-211">LogsDiskQuotaInMB</span></span>   |<span data-ttu-id="00922-212">Lång</span><span class="sxs-lookup"><span data-stu-id="00922-212">Long</span></span>  <br> <span data-ttu-id="00922-213">(Standard: 1024)</span><span class="sxs-lookup"><span data-stu-id="00922-213">(Default: 1024)</span></span>               |<span data-ttu-id="00922-214">Maximal storlek för korrigering orchestration app loggar i MB, vilket kan sparas lokalt på noder.</span><span class="sxs-lookup"><span data-stu-id="00922-214">Maximum size of patch orchestration app logs in MB, which can be persisted locally on nodes.</span></span>
| <span data-ttu-id="00922-215">WUQuery</span><span class="sxs-lookup"><span data-stu-id="00922-215">WUQuery</span></span>               | <span data-ttu-id="00922-216">Sträng</span><span class="sxs-lookup"><span data-stu-id="00922-216">string</span></span><br><span data-ttu-id="00922-217">(Standard ”: IsInstalled = 0”)</span><span class="sxs-lookup"><span data-stu-id="00922-217">(Default: "IsInstalled=0")</span></span>                | <span data-ttu-id="00922-218">Fråga tooget Windows-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="00922-218">Query tooget Windows updates.</span></span> <span data-ttu-id="00922-219">Mer information finns i [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="00922-219">For more information, see [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span></span>
| <span data-ttu-id="00922-220">InstallWindowsOSOnlyUpdates</span><span class="sxs-lookup"><span data-stu-id="00922-220">InstallWindowsOSOnlyUpdates</span></span> | <span data-ttu-id="00922-221">bool</span><span class="sxs-lookup"><span data-stu-id="00922-221">Bool</span></span> <br> <span data-ttu-id="00922-222">(standard: True)</span><span class="sxs-lookup"><span data-stu-id="00922-222">(default: True)</span></span>                 | <span data-ttu-id="00922-223">Den här flaggan kan Windows-operativsystem system uppdateringar toobe installerad.</span><span class="sxs-lookup"><span data-stu-id="00922-223">This flag allows Windows operating system updates toobe installed.</span></span>            |
| <span data-ttu-id="00922-224">WUOperationTimeOutInMinutes</span><span class="sxs-lookup"><span data-stu-id="00922-224">WUOperationTimeOutInMinutes</span></span> | <span data-ttu-id="00922-225">int</span><span class="sxs-lookup"><span data-stu-id="00922-225">Int</span></span> <br><span data-ttu-id="00922-226">(Standard: 90).</span><span class="sxs-lookup"><span data-stu-id="00922-226">(Default: 90)</span></span>                   | <span data-ttu-id="00922-227">Anger hello timeout för någon Windows Update-åtgärd (Sök eller ladda ned eller installera).</span><span class="sxs-lookup"><span data-stu-id="00922-227">Specifies hello timeout for any Windows Update operation (search or download or install).</span></span> <span data-ttu-id="00922-228">Om hello-åtgärden inte slutfördes inom Hej angiven tidsgräns, den har avbrutits.</span><span class="sxs-lookup"><span data-stu-id="00922-228">If hello operation is not completed within hello specified timeout, it is aborted.</span></span>       |
| <span data-ttu-id="00922-229">WURescheduleCount</span><span class="sxs-lookup"><span data-stu-id="00922-229">WURescheduleCount</span></span>     | <span data-ttu-id="00922-230">int</span><span class="sxs-lookup"><span data-stu-id="00922-230">Int</span></span> <br> <span data-ttu-id="00922-231">(Standard: 5).</span><span class="sxs-lookup"><span data-stu-id="00922-231">(Default: 5)</span></span>                  | <span data-ttu-id="00922-232">hello maximalt antal gånger hello service schemaläggs automatiskt på nytt hello Windows update om en åtgärd misslyckas så.</span><span class="sxs-lookup"><span data-stu-id="00922-232">hello maximum number of times hello service reschedules hello Windows update in case an operation fails persistently.</span></span>          |
| <span data-ttu-id="00922-233">WURescheduleTimeInMinutes</span><span class="sxs-lookup"><span data-stu-id="00922-233">WURescheduleTimeInMinutes</span></span> | <span data-ttu-id="00922-234">int</span><span class="sxs-lookup"><span data-stu-id="00922-234">Int</span></span> <br><span data-ttu-id="00922-235">(Standard: 30).</span><span class="sxs-lookup"><span data-stu-id="00922-235">(Default: 30)</span></span> | <span data-ttu-id="00922-236">hello intervall på vilka hello service schemaläggs automatiskt på nytt hello Windows update om felet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="00922-236">hello interval at which hello service reschedules hello Windows update in case failure persists.</span></span> |
| <span data-ttu-id="00922-237">WUFrequency</span><span class="sxs-lookup"><span data-stu-id="00922-237">WUFrequency</span></span>           | <span data-ttu-id="00922-238">Kommaavgränsad sträng (standard: ”vecka, Onsdag 7:00:00”)</span><span class="sxs-lookup"><span data-stu-id="00922-238">Comma-separated string (Default: "Weekly, Wednesday, 7:00:00")</span></span>     | <span data-ttu-id="00922-239">hello frekvensen för att installera Windows Update.</span><span class="sxs-lookup"><span data-stu-id="00922-239">hello frequency for installing Windows Update.</span></span> <span data-ttu-id="00922-240">hello format och möjliga värden är:</span><span class="sxs-lookup"><span data-stu-id="00922-240">hello format and possible values are:</span></span> <br><span data-ttu-id="00922-241">-: Som mm: ss varje månad, DD, till exempel varje månad, 5, 12: 22:32.</span><span class="sxs-lookup"><span data-stu-id="00922-241">-   Monthly, DD,HH:MM:SS, for example, Monthly, 5,12:22:32.</span></span> <br> <span data-ttu-id="00922-242">-Varje vecka, dag,: mm: ss, för exempelvis varje vecka, tisdag, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="00922-242">-   Weekly, DAY,HH:MM:SS, for example, Weekly, Tuesday, 12:22:32.</span></span>  <br> <span data-ttu-id="00922-243">-Varje dag,: mm: ss, till exempel dagligen, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="00922-243">-   Daily, HH:MM:SS, for example, Daily, 12:22:32.</span></span>  <br> <span data-ttu-id="00922-244">-Ingen anger att Windows Update inte utföras.</span><span class="sxs-lookup"><span data-stu-id="00922-244">-  None indicates that Windows Update shouldn't be done.</span></span>  <br><br> <span data-ttu-id="00922-245">Observera att hello alltid i UTC.</span><span class="sxs-lookup"><span data-stu-id="00922-245">Note that all hello times are in UTC.</span></span>|
| <span data-ttu-id="00922-246">AcceptWindowsUpdateEula</span><span class="sxs-lookup"><span data-stu-id="00922-246">AcceptWindowsUpdateEula</span></span> | <span data-ttu-id="00922-247">bool</span><span class="sxs-lookup"><span data-stu-id="00922-247">Bool</span></span> <br><span data-ttu-id="00922-248">(Standard: true)</span><span class="sxs-lookup"><span data-stu-id="00922-248">(Default: true)</span></span> | <span data-ttu-id="00922-249">Genom att ange den här flaggan accepterar hello programmet hello slutanvändarens License avtal för Windows Update för hello ägare av hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="00922-249">By setting this flag, hello application accepts hello End-User License Agreement for Windows Update on behalf of hello owner of hello machine.</span></span>              |

> [!TIP]
> <span data-ttu-id="00922-250">Om du vill att Windows Update toohappen omedelbart, `WUFrequency` relativa toohello tidpunkten för distribution av programmet.</span><span class="sxs-lookup"><span data-stu-id="00922-250">If you want Windows Update toohappen immediately, set `WUFrequency` relative toohello application deployment time.</span></span> <span data-ttu-id="00922-251">Anta exempelvis att du har en fem-nodtest kluster och planera toodeploy hello app vid cirka 17:00:00 UTC.</span><span class="sxs-lookup"><span data-stu-id="00922-251">For example, suppose that you have a five-node test cluster and plan toodeploy hello app at around 5:00 PM UTC.</span></span> <span data-ttu-id="00922-252">Om du anta att distribution eller uppgradering av programmet hello tar 30 minuter vid hello maximala, Ställ in hello WUFrequency som ”varje dag, 17:30:00”.</span><span class="sxs-lookup"><span data-stu-id="00922-252">If you assume that hello application upgrade or deployment takes 30 minutes at hello maximum, set hello WUFrequency as "Daily, 17:30:00."</span></span>

## <a name="deploy-hello-app"></a><span data-ttu-id="00922-253">Distribuera hello app</span><span class="sxs-lookup"><span data-stu-id="00922-253">Deploy hello app</span></span>

1. <span data-ttu-id="00922-254">Slut alla hello förkraven tooprepare hello klustret.</span><span class="sxs-lookup"><span data-stu-id="00922-254">Finish all hello prerequisite steps tooprepare hello cluster.</span></span>
2. <span data-ttu-id="00922-255">Distribuera hello korrigering orchestration-appen som andra Service Fabric-app.</span><span class="sxs-lookup"><span data-stu-id="00922-255">Deploy hello patch orchestration app like any other Service Fabric app.</span></span> <span data-ttu-id="00922-256">Du kan distribuera hello appen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="00922-256">You can deploy hello app by using PowerShell.</span></span> <span data-ttu-id="00922-257">Gör så hello i [distribuera och ta bort program med hjälp av PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="00922-257">Follow hello steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>
3. <span data-ttu-id="00922-258">tooconfigure hello-programmet vid hello i distributionen, pass hello `ApplicationParamater` toohello `New-ServiceFabricApplication` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="00922-258">tooconfigure hello application at hello time of deployment, pass hello `ApplicationParamater` toohello `New-ServiceFabricApplication` cmdlet.</span></span> <span data-ttu-id="00922-259">Vi har samlat hello skriptet Deploy.ps1 tillsammans med programmet hello för din bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="00922-259">For your convenience, we’ve provided hello script Deploy.ps1 along with hello application.</span></span> <span data-ttu-id="00922-260">toouse hello skript:</span><span class="sxs-lookup"><span data-stu-id="00922-260">toouse hello script:</span></span>

    - <span data-ttu-id="00922-261">Ansluta tooa Service Fabric-kluster med hjälp av `Connect-ServiceFabricCluster`.</span><span class="sxs-lookup"><span data-stu-id="00922-261">Connect tooa Service Fabric cluster by using `Connect-ServiceFabricCluster`.</span></span>
    - <span data-ttu-id="00922-262">Kör hello PowerShell-skriptet Deploy.ps1 med lämpliga hello `ApplicationParameter` värde.</span><span class="sxs-lookup"><span data-stu-id="00922-262">Execute hello PowerShell script Deploy.ps1 with hello appropriate `ApplicationParameter` value.</span></span>

> [!NOTE]
> <span data-ttu-id="00922-263">Tänk hello hello skript och hello programmappen PatchOrchestrationApplication samma katalog.</span><span class="sxs-lookup"><span data-stu-id="00922-263">Keep hello script and hello application folder PatchOrchestrationApplication in hello same directory.</span></span>

## <a name="upgrade-hello-app"></a><span data-ttu-id="00922-264">Uppgradera hello app</span><span class="sxs-lookup"><span data-stu-id="00922-264">Upgrade hello app</span></span>

<span data-ttu-id="00922-265">tooupgrade en befintlig korrigering orchestration-app med hjälp av PowerShell, gör hello i [uppgradering av Service Fabric-programmet med PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span><span class="sxs-lookup"><span data-stu-id="00922-265">tooupgrade an existing patch orchestration app by using PowerShell, follow hello steps in [Service Fabric application upgrade using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span></span>

## <a name="remove-hello-app"></a><span data-ttu-id="00922-266">Ta bort hello appen</span><span class="sxs-lookup"><span data-stu-id="00922-266">Remove hello app</span></span>

<span data-ttu-id="00922-267">tooremove hello program, följ hello stegen i [distribuera och ta bort program med hjälp av PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="00922-267">tooremove hello application, follow hello steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>

<span data-ttu-id="00922-268">Vi har samlat hello skriptet Undeploy.ps1 tillsammans med programmet hello för din bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="00922-268">For your convenience, we've provided hello script Undeploy.ps1 along with hello application.</span></span> <span data-ttu-id="00922-269">toouse hello skript:</span><span class="sxs-lookup"><span data-stu-id="00922-269">toouse hello script:</span></span>

  - <span data-ttu-id="00922-270">Ansluta tooa Service Fabric-kluster med hjälp av ```Connect-ServiceFabricCluster```.</span><span class="sxs-lookup"><span data-stu-id="00922-270">Connect tooa Service Fabric cluster by using ```Connect-ServiceFabricCluster```.</span></span>

  - <span data-ttu-id="00922-271">Köra PowerShell-skript för hello Undeploy.ps1.</span><span class="sxs-lookup"><span data-stu-id="00922-271">Execute hello PowerShell script Undeploy.ps1.</span></span>

> [!NOTE]
> <span data-ttu-id="00922-272">Tänk hello hello skript och hello programmappen PatchOrchestrationApplication samma katalog.</span><span class="sxs-lookup"><span data-stu-id="00922-272">Keep hello script and hello application folder PatchOrchestrationApplication in hello same directory.</span></span>

## <a name="view-hello-windows-update-results"></a><span data-ttu-id="00922-273">Visa hello Windows Update-resultat</span><span class="sxs-lookup"><span data-stu-id="00922-273">View hello Windows Update results</span></span>

<span data-ttu-id="00922-274">hello korrigering orchestration app exponerar REST API: er toodisplay hello historiska resultat toohello användare.</span><span class="sxs-lookup"><span data-stu-id="00922-274">hello patch orchestration app exposes REST APIs toodisplay hello historical results toohello user.</span></span> <span data-ttu-id="00922-275">Ett exempel på hello resultatet JSON:</span><span class="sxs-lookup"><span data-stu-id="00922-275">An example of hello result JSON:</span></span>
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2017-05-21T11:46:52.1953713Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of hello issues that are included in this update, see hello associated Microsoft Knowledge Base article. After you install this update, you may have toorestart your system.",
            "ResultCode": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```
<span data-ttu-id="00922-276">Om ingen uppdatering har schemalagts ännu är hello resultatet JSON tom.</span><span class="sxs-lookup"><span data-stu-id="00922-276">If no update is scheduled yet, hello result JSON is empty.</span></span>

<span data-ttu-id="00922-277">Logga in toohello klustret tooquery Windows Update resultat.</span><span class="sxs-lookup"><span data-stu-id="00922-277">Log in toohello cluster tooquery Windows Update results.</span></span> <span data-ttu-id="00922-278">Sedan reda på hello replik adressen för primär hello av hello Coordinator-tjänsten och tryck hello URL från hello webbläsare: http://&lt;REPLIK-IP-&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="00922-278">Then find out hello replica address for hello primary of hello Coordinator Service, and hit hello URL from hello browser: http://&lt;REPLICA-IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="00922-279">hello REST-slutpunkt för hello Coordinator-tjänsten har en dynamisk port.</span><span class="sxs-lookup"><span data-stu-id="00922-279">hello REST endpoint for hello Coordinator Service has a dynamic port.</span></span> <span data-ttu-id="00922-280">toocheck hello exakta URL finns toohello Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="00922-280">toocheck hello exact URL, refer toohello Service Fabric Explorer.</span></span> <span data-ttu-id="00922-281">Till exempel hello resultat är tillgängliga på `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span><span class="sxs-lookup"><span data-stu-id="00922-281">For example, hello results are available at `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span></span>

![Bild av REST-slutpunkt](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


<span data-ttu-id="00922-283">Om hello omvänd proxy är aktiverat på hello klustret kan du komma åt hello URL: en från utanför hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="00922-283">If hello reverse proxy is enabled on hello cluster, you can access hello URL from outside of hello cluster as well.</span></span>
<span data-ttu-id="00922-284">Hej slutpunkt som behöver toobe träffar är http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="00922-284">hello endpoint that needs toobe hit is http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="00922-285">tooenable hello omvänd proxy på hello klustret gör hello i [omvänd proxy i Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span><span class="sxs-lookup"><span data-stu-id="00922-285">tooenable hello reverse proxy on hello cluster, follow hello steps in [Reverse proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span></span> 

> 
> [!WARNING]
> <span data-ttu-id="00922-286">Alla micro tjänster i hello kluster som Exponerar en HTTP-slutpunkten är adresserbara från utanför hello kluster när hello omvänd proxy har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="00922-286">After hello reverse proxy is configured, all micro services in hello cluster that expose an HTTP endpoint are addressable from outside hello cluster.</span></span>

## <a name="diagnosticshealth-events"></a><span data-ttu-id="00922-287">Diagnostik/hälsotillstånd händelser</span><span class="sxs-lookup"><span data-stu-id="00922-287">Diagnostics/health events</span></span>

### <a name="collect-patch-orchestration-app-logs"></a><span data-ttu-id="00922-288">Samla in korrigering orchestration app loggar</span><span class="sxs-lookup"><span data-stu-id="00922-288">Collect patch orchestration app logs</span></span>

<span data-ttu-id="00922-289">Korrigering orchestration app loggarna har samlats in som en del av Service Fabric-loggar från körningsversion `5.6.220.9494` och högre.</span><span class="sxs-lookup"><span data-stu-id="00922-289">Patch orchestration app logs are collected as part of Service Fabric logs from runtime version `5.6.220.9494` and above.</span></span>
<span data-ttu-id="00922-290">För kluster som kör Service Fabric runtime version mindre än `5.6.220.9494`, loggar samlas in med något av följande metoder hello.</span><span class="sxs-lookup"><span data-stu-id="00922-290">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs can be collected by using one of hello following methods.</span></span>

#### <a name="locally-on-each-node"></a><span data-ttu-id="00922-291">Lokalt på varje nod</span><span class="sxs-lookup"><span data-stu-id="00922-291">Locally on each node</span></span>

<span data-ttu-id="00922-292">Loggarna samlas lokalt på varje klusternod för Service Fabric Service Fabric runtime-versionen om är mindre än `5.6.220.9494`.</span><span class="sxs-lookup"><span data-stu-id="00922-292">Logs are collected locally on each Service Fabric cluster node if Service Fabric runtime version is less than `5.6.220.9494`.</span></span> <span data-ttu-id="00922-293">hello plats tooaccess hello loggar är \[Service Fabric\_Installation\_enhet\]:\\PatchOrchestrationApplication\\loggar.</span><span class="sxs-lookup"><span data-stu-id="00922-293">hello location tooaccess hello logs is \[Service Fabric\_Installation\_Drive\]:\\PatchOrchestrationApplication\\logs.</span></span>

<span data-ttu-id="00922-294">Till exempel om Service Fabric är installerat på enhet D hello sökvägen är D:\\PatchOrchestrationApplication\\loggar.</span><span class="sxs-lookup"><span data-stu-id="00922-294">For example, if Service Fabric is installed on drive D, hello path is D:\\PatchOrchestrationApplication\\logs.</span></span>

#### <a name="central-location"></a><span data-ttu-id="00922-295">Central plats</span><span class="sxs-lookup"><span data-stu-id="00922-295">Central location</span></span>

<span data-ttu-id="00922-296">Om Azure-diagnostik är konfigurerad som en del av nödvändiga steg, är loggar för hello korrigering orchestration app tillgängliga i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="00922-296">If Azure Diagnostics is configured as part of prerequisite steps, logs for hello patch orchestration app are available in Azure Storage.</span></span>

### <a name="health-reports"></a><span data-ttu-id="00922-297">Hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="00922-297">Health reports</span></span>

<span data-ttu-id="00922-298">hello korrigering orchestration app publicerar också hälsorapporter mot hello Coordinator-tjänsten eller hello nod Agent-tjänsten i hello följande fall:</span><span class="sxs-lookup"><span data-stu-id="00922-298">hello patch orchestration app also publishes health reports against hello Coordinator Service or hello Node Agent Service in hello following cases:</span></span>

#### <a name="a-windows-update-operation-failed"></a><span data-ttu-id="00922-299">En Windows Update-åtgärden misslyckades</span><span class="sxs-lookup"><span data-stu-id="00922-299">A Windows Update operation failed</span></span>

<span data-ttu-id="00922-300">Om en Windows Update-åtgärden misslyckas på en nod, skapas en hälsorapport mot hello nod Agent-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="00922-300">If a Windows Update operation fails on a node, a health report is generated against hello Node Agent Service.</span></span> <span data-ttu-id="00922-301">Information om hello hälsorapport innehålla hello problematiska nodnamn.</span><span class="sxs-lookup"><span data-stu-id="00922-301">Details of hello health report contain hello problematic node name.</span></span>

<span data-ttu-id="00922-302">När uppdatering har slutförts på hello problematiska nod, avmarkeras automatiskt hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="00922-302">After patching is successfully completed on hello problematic node, hello report is automatically cleared.</span></span>

#### <a name="hello-node-agent-ntservice-is-down"></a><span data-ttu-id="00922-303">hello nod Agent NTService är nere</span><span class="sxs-lookup"><span data-stu-id="00922-303">hello Node Agent NTService is down</span></span>

<span data-ttu-id="00922-304">Om hello nod Agent NTService är igång på en nod kan genereras en varning nivå hälsorapport mot hello nod Agent-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="00922-304">If hello Node Agent NTService is down on a node, a warning-level health report is generated against hello Node Agent Service.</span></span>

#### <a name="hello-repair-manager-service-is-not-enabled"></a><span data-ttu-id="00922-305">hello reparera manager-tjänsten har inte aktiverats</span><span class="sxs-lookup"><span data-stu-id="00922-305">hello repair manager service is not enabled</span></span>

<span data-ttu-id="00922-306">Om hello reparera manager-tjänsten inte kan hittas på hello klustret, skapas en varningsnivå hälsorapport för hello Coordinator-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="00922-306">If hello repair manager service is not found on hello cluster, a warning-level health report is generated for hello Coordinator Service.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="00922-307">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="00922-307">Frequently asked questions</span></span>

<span data-ttu-id="00922-308">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="00922-308">Q.</span></span> <span data-ttu-id="00922-309">**Varför ser jag min kluster i ett feltillstånd när hello korrigering orchestration app körs?**</span><span class="sxs-lookup"><span data-stu-id="00922-309">**Why do I see my cluster in an error state when hello patch orchestration app is running?**</span></span>

<span data-ttu-id="00922-310">A.</span><span class="sxs-lookup"><span data-stu-id="00922-310">A.</span></span> <span data-ttu-id="00922-311">Under installationen hello hello korrigering orchestration app inaktiverar eller startar om noder som tillfälligt kan resultera i hello hälsotillstånd hello klustret gå.</span><span class="sxs-lookup"><span data-stu-id="00922-311">During hello installation process, hello patch orchestration app disables or restarts nodes, which can temporarily result in hello health of hello cluster going down.</span></span>

<span data-ttu-id="00922-312">Baserat på hello princip för hello program, antingen en nod kan gå under en uppdatering åtgärd *eller* en hel uppgraderingsdomän kan gå samtidigt.</span><span class="sxs-lookup"><span data-stu-id="00922-312">Based on hello policy for hello application, either one node can go down during a patching operation *or* an entire upgrade domain can go down simultaneously.</span></span>

<span data-ttu-id="00922-313">Hello slutet av hello Windows Update-installation, hello noder är reenabled efter omstart.</span><span class="sxs-lookup"><span data-stu-id="00922-313">By hello end of hello Windows Update installation, hello nodes are reenabled post restart.</span></span>

<span data-ttu-id="00922-314">I följande exempel hello, gick hello klustret tooan feltillstånd tillfälligt eftersom två noder har ned och hello MaxPercentageUnhealthNodes princip har överskridits.</span><span class="sxs-lookup"><span data-stu-id="00922-314">In hello following example, hello cluster went tooan error state temporarily because two nodes were down and hello MaxPercentageUnhealthNodes policy was violated.</span></span> <span data-ttu-id="00922-315">hello fel är tillfällig tills hello korrigering åtgärden pågår.</span><span class="sxs-lookup"><span data-stu-id="00922-315">hello error is temporary until hello patching operation is ongoing.</span></span>

![Bild av feltillstånd kluster](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

<span data-ttu-id="00922-317">Om hello problemet kvarstår läser du avsnittet om felsökning toohello.</span><span class="sxs-lookup"><span data-stu-id="00922-317">If hello issue persists, refer toohello Troubleshooting section.</span></span>

<span data-ttu-id="00922-318">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="00922-318">Q.</span></span> <span data-ttu-id="00922-319">**Korrigering av orchestration appar är i varningstillstånd**</span><span class="sxs-lookup"><span data-stu-id="00922-319">**Patch orchestration app is in warning state**</span></span>

<span data-ttu-id="00922-320">A.</span><span class="sxs-lookup"><span data-stu-id="00922-320">A.</span></span> <span data-ttu-id="00922-321">Kontrollera toosee om en hälsorapport bokförd mot hello program är hello grundläggande orsaken.</span><span class="sxs-lookup"><span data-stu-id="00922-321">Check toosee if a health report posted against hello application is hello root cause.</span></span> <span data-ttu-id="00922-322">Vanligtvis innehåller hello varning information om hello problem.</span><span class="sxs-lookup"><span data-stu-id="00922-322">Usually, hello warning contains details of hello problem.</span></span> <span data-ttu-id="00922-323">Om hello problemet är tillfälligt, är hello program förväntade tooauto återskapa från det här läget.</span><span class="sxs-lookup"><span data-stu-id="00922-323">If hello issue is transient, hello application is expected tooauto-recover from this state.</span></span>

<span data-ttu-id="00922-324">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="00922-324">Q.</span></span> <span data-ttu-id="00922-325">**Vad gör jag om min klustret är i feltillstånd och toodo en brådskande operativsystemet uppdatering måste?**</span><span class="sxs-lookup"><span data-stu-id="00922-325">**What can I do if my cluster is unhealthy and I need toodo an urgent operating system update?**</span></span>

<span data-ttu-id="00922-326">A.</span><span class="sxs-lookup"><span data-stu-id="00922-326">A.</span></span> <span data-ttu-id="00922-327">hello korrigering orchestration appen installeras inte uppdateringar medan hello klustret är i feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="00922-327">hello patch orchestration app does not install updates while hello cluster is unhealthy.</span></span> <span data-ttu-id="00922-328">Försök toobring klustret tooa felfritt tillstånd toounblock hello korrigering orchestration app arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="00922-328">Try toobring your cluster tooa healthy state toounblock hello patch orchestration app workflow.</span></span>

<span data-ttu-id="00922-329">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="00922-329">Q.</span></span> <span data-ttu-id="00922-330">**Varför korrigering genom datorkluster tar så lång tid toorun?**</span><span class="sxs-lookup"><span data-stu-id="00922-330">**Why does patching across clusters take so long toorun?**</span></span>

<span data-ttu-id="00922-331">A.</span><span class="sxs-lookup"><span data-stu-id="00922-331">A.</span></span> <span data-ttu-id="00922-332">hello-tid som krävs av hello korrigering orchestration app är främst beroende hello följande faktorer:</span><span class="sxs-lookup"><span data-stu-id="00922-332">hello time needed by hello patch orchestration app is mostly dependent on hello following factors:</span></span>

- <span data-ttu-id="00922-333">hello princip hello Coordinator-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="00922-333">hello policy of hello Coordinator Service.</span></span> 
  - <span data-ttu-id="00922-334">Hej standardprincipen `NodeWise`, resulterar i korrigering endast en nod i taget.</span><span class="sxs-lookup"><span data-stu-id="00922-334">hello default policy, `NodeWise`, results in patching only one node at a time.</span></span> <span data-ttu-id="00922-335">Särskilt i hello fallet med större kluster, rekommenderar vi att du använder hello `UpgradeDomainWise` princip tooachieve snabbare korrigering genom datorkluster.</span><span class="sxs-lookup"><span data-stu-id="00922-335">Especially in hello case of bigger clusters, we recommend that you use hello `UpgradeDomainWise` policy tooachieve faster patching across clusters.</span></span>
- <span data-ttu-id="00922-336">hello antalet uppdateringar som är tillgängliga för hämtning och installation.</span><span class="sxs-lookup"><span data-stu-id="00922-336">hello number of updates available for download and installation.</span></span> 
- <span data-ttu-id="00922-337">Hej toodownload Genomsnittlig tid som krävs och installerar en uppdatering som inte får överstiga några timmar.</span><span class="sxs-lookup"><span data-stu-id="00922-337">hello average time needed toodownload and install an update, which should not exceed a couple of hours.</span></span>
- <span data-ttu-id="00922-338">Hej hello VM prestanda och bandbredd.</span><span class="sxs-lookup"><span data-stu-id="00922-338">hello performance of hello VM and network bandwidth.</span></span>

<span data-ttu-id="00922-339">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="00922-339">Q.</span></span> <span data-ttu-id="00922-340">**Varför ser vissa uppdateringar i Windows Update resultaten via REST-api, men inte under hello Windows Update-historiken på datorn?**</span><span class="sxs-lookup"><span data-stu-id="00922-340">**Why do I see some updates in Windows Update results obtained via REST api's, but not under hello Windows Update history on machine?**</span></span>

<span data-ttu-id="00922-341">A.</span><span class="sxs-lookup"><span data-stu-id="00922-341">A.</span></span> <span data-ttu-id="00922-342">Vissa produktuppdateringar måste toobe checkats in deras respektive uppdatering/korrigering historik.</span><span class="sxs-lookup"><span data-stu-id="00922-342">Some product updates need toobe checked in their respective update/patch history.</span></span> <span data-ttu-id="00922-343">Exempel: Uppdateringar för Windows Defender inte visas i Windows Update-historiken på Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="00922-343">Eg: Windows Defender updates do not show up in Windows Update history on Windows Server 2016.</span></span>

## <a name="disclaimers"></a><span data-ttu-id="00922-344">Ansvarsfriskrivningar</span><span class="sxs-lookup"><span data-stu-id="00922-344">Disclaimers</span></span>

- <span data-ttu-id="00922-345">hello korrigering orchestration app accepterar hello slutanvändarens License avtal för Windows Update för hello användares räkning.</span><span class="sxs-lookup"><span data-stu-id="00922-345">hello patch orchestration app accepts hello End-User License Agreement of Windows Update on behalf of hello user.</span></span> <span data-ttu-id="00922-346">Du kan också kan hello inställningen inaktiveras i hello konfigurationen av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="00922-346">Optionally, hello setting can be turned off in hello configuration of hello application.</span></span>

- <span data-ttu-id="00922-347">hello korrigering orchestration appen samlar in telemetri tootrack användnings- och prestandadata.</span><span class="sxs-lookup"><span data-stu-id="00922-347">hello patch orchestration app collects telemetry tootrack usage and performance.</span></span> <span data-ttu-id="00922-348">Hej programtelemetri följer hello inställningen hello Service Fabric runtime telemetri inställning (som är aktiverad som standard).</span><span class="sxs-lookup"><span data-stu-id="00922-348">hello application’s telemetry follows hello setting of hello Service Fabric runtime’s telemetry setting (which is on by default).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="00922-349">Felsökning</span><span class="sxs-lookup"><span data-stu-id="00922-349">Troubleshooting</span></span>

### <a name="a-node-is-not-coming-back-tooup-state"></a><span data-ttu-id="00922-350">En nod kommer inte tillbaka tooup tillstånd</span><span class="sxs-lookup"><span data-stu-id="00922-350">A node is not coming back tooup state</span></span>

<span data-ttu-id="00922-351">**hello nod kan ha fastnat i tillståndet inaktivera eftersom**:</span><span class="sxs-lookup"><span data-stu-id="00922-351">**hello node might be stuck in a disabling state because**:</span></span>

<span data-ttu-id="00922-352">En säkerhets-kontroll väntar.</span><span class="sxs-lookup"><span data-stu-id="00922-352">A safety check is pending.</span></span> <span data-ttu-id="00922-353">tooremedy den här situationen kan se till att tillräckligt många noder är tillgängliga i felfritt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="00922-353">tooremedy this situation, ensure that enough nodes are available in a healthy state.</span></span>

<span data-ttu-id="00922-354">**hello nod kan ha fastnat i ett inaktiverat tillstånd eftersom**:</span><span class="sxs-lookup"><span data-stu-id="00922-354">**hello node might be stuck in a disabled state because**:</span></span>

- <span data-ttu-id="00922-355">hello-nod har inaktiverats manuellt.</span><span class="sxs-lookup"><span data-stu-id="00922-355">hello node was disabled manually.</span></span>
- <span data-ttu-id="00922-356">hello-nod har inaktiverats på grund av tooan pågående Azure-infrastrukturen jobb.</span><span class="sxs-lookup"><span data-stu-id="00922-356">hello node was disabled due tooan ongoing Azure infrastructure job.</span></span>
- <span data-ttu-id="00922-357">hello-nod har inaktiverats tillfälligt av hello korrigering orchestration app toopatch hello nod.</span><span class="sxs-lookup"><span data-stu-id="00922-357">hello node was disabled temporarily by hello patch orchestration app toopatch hello node.</span></span>

<span data-ttu-id="00922-358">**hello nod kan ha fastnat i ned-läge eftersom**:</span><span class="sxs-lookup"><span data-stu-id="00922-358">**hello node might be stuck in a down state because**:</span></span>

- <span data-ttu-id="00922-359">hello nod placerades i tillståndet nedåt manuellt.</span><span class="sxs-lookup"><span data-stu-id="00922-359">hello node was put in a down state manually.</span></span>
- <span data-ttu-id="00922-360">hello nod genomgår en omstart (som kan utlösas av hello korrigering orchestration app).</span><span class="sxs-lookup"><span data-stu-id="00922-360">hello node is undergoing a restart (which might be triggered by hello patch orchestration app).</span></span>
- <span data-ttu-id="00922-361">hello nod förfaller tooa felaktiga VM eller datorn eller nätverket anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="00922-361">hello node is down due tooa faulty VM or machine or network connectivity issues.</span></span>

### <a name="updates-were-skipped-on-some-nodes"></a><span data-ttu-id="00922-362">Uppdateringar hoppades över på vissa noder</span><span class="sxs-lookup"><span data-stu-id="00922-362">Updates were skipped on some nodes</span></span>

<span data-ttu-id="00922-363">hello korrigering orchestration app försöker tooinstall en Windows update bl.a toohello omplanering princip.</span><span class="sxs-lookup"><span data-stu-id="00922-363">hello patch orchestration app tries tooinstall a Windows update according toohello rescheduling policy.</span></span> <span data-ttu-id="00922-364">hello-tjänsten försöker toorecover hello nod och hoppa över hello update bl.a toohello princip för program.</span><span class="sxs-lookup"><span data-stu-id="00922-364">hello service tries toorecover hello node and skip hello update according toohello application policy.</span></span>

<span data-ttu-id="00922-365">I sådana fall genereras en varning nivå hälsorapport mot hello nod Agent-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="00922-365">In such a case, a warning-level health report is generated against hello Node Agent Service.</span></span> <span data-ttu-id="00922-366">hello resultat för Windows Update innehåller också hello möjlig anledning till hello-fel.</span><span class="sxs-lookup"><span data-stu-id="00922-366">hello result for Windows Update also contains hello possible reason for hello failure.</span></span>

### <a name="hello-health-of-hello-cluster-goes-tooerror-while-hello-update-installs"></a><span data-ttu-id="00922-367">hello hälsotillstånd hello klustret blir tooerror medan hello uppdateringen har installerats</span><span class="sxs-lookup"><span data-stu-id="00922-367">hello health of hello cluster goes tooerror while hello update installs</span></span>

<span data-ttu-id="00922-368">En felaktig Windows update kan skada hello hälsotillståndet för ett program eller ett kluster på en viss nod eller en domän.</span><span class="sxs-lookup"><span data-stu-id="00922-368">A faulty Windows update can bring down hello health of an application or cluster on a particular node or upgrade domain.</span></span> <span data-ttu-id="00922-369">hello korrigering orchestration app upphör alla efterföljande Windows Update-åtgärden tills hello klustret är felfri igen.</span><span class="sxs-lookup"><span data-stu-id="00922-369">hello patch orchestration app discontinues any subsequent Windows Update operation until hello cluster is healthy again.</span></span>

<span data-ttu-id="00922-370">En administratör måste ingripa och ta reda på varför programmet hello eller kluster fick dåligt hälsotillstånd på grund av tooWindows uppdatering.</span><span class="sxs-lookup"><span data-stu-id="00922-370">An administrator must intervene and determine why hello application or cluster became unhealthy due tooWindows Update.</span></span>

## <a name="release-notes-"></a><span data-ttu-id="00922-371">Viktig information:</span><span class="sxs-lookup"><span data-stu-id="00922-371">Release Notes :</span></span>

### <a name="version-110"></a><span data-ttu-id="00922-372">Version 1.1.0</span><span class="sxs-lookup"><span data-stu-id="00922-372">Version 1.1.0</span></span>
- <span data-ttu-id="00922-373">Offentliga utgåvan</span><span class="sxs-lookup"><span data-stu-id="00922-373">Public release</span></span>

### <a name="version-111"></a><span data-ttu-id="00922-374">Version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="00922-374">Version 1.1.1</span></span>
- <span data-ttu-id="00922-375">Fast ett programfel i SetupEntryPoint av NodeAgentService som förhindrade installationen av NodeAgentNTService.</span><span class="sxs-lookup"><span data-stu-id="00922-375">Fixed a bug in SetupEntryPoint of NodeAgentService that prevented installation of NodeAgentNTService.</span></span>

### <a name="version-120-latest"></a><span data-ttu-id="00922-376">Version 1.2.0 (senaste)</span><span class="sxs-lookup"><span data-stu-id="00922-376">Version 1.2.0 (Latest)</span></span>

- <span data-ttu-id="00922-377">Felkorrigeringar runt system starta om arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="00922-377">Bug fixes around system restart workflow.</span></span>
- <span data-ttu-id="00922-378">Buggfix vid skapande av RM uppgifter på grund av toowhich hälsokontroll under förberedelserna reparera uppgifter händer inte som förväntat.</span><span class="sxs-lookup"><span data-stu-id="00922-378">Bug fix in creation of RM tasks due toowhich health check during preparing repair tasks wasn't happening as expected.</span></span>
- <span data-ttu-id="00922-379">Ändrade hello startläge för windows-tjänst POANodeSvc från automatisk toodelayed-automatiskt.</span><span class="sxs-lookup"><span data-stu-id="00922-379">Changed hello startup mode for windows service POANodeSvc from auto toodelayed-auto.</span></span>
