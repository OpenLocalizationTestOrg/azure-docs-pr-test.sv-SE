---
title: Azure Service Fabric korrigering orchestration-program | Microsoft Docs
description: "Program för att automatisera operativsystemet korrigering på ett Service Fabric-kluster."
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
ms.openlocfilehash: 2c5842822e347113e388d570f6ae603a313944d6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="patch-the-windows-operating-system-in-your-service-fabric-cluster"></a><span data-ttu-id="59a7b-103">Korrigering av Windows-operativsystemet i Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="59a7b-103">Patch the Windows operating system in your Service Fabric cluster</span></span>

<span data-ttu-id="59a7b-104">Korrigering av orchestration-programmet är ett Azure Service Fabric som automatiserar operativsystemet korrigering på ett Service Fabric-kluster i Azure utan driftavbrott.</span><span class="sxs-lookup"><span data-stu-id="59a7b-104">The patch orchestration application is an Azure Service Fabric application that automates operating system patching on a Service Fabric cluster on Azure without downtime.</span></span>

<span data-ttu-id="59a7b-105">Korrigering orchestration appen innehåller följande:</span><span class="sxs-lookup"><span data-stu-id="59a7b-105">The patch orchestration app provides the following:</span></span>

- <span data-ttu-id="59a7b-106">**Automatisk uppdatering operativsysteminstallation**.</span><span class="sxs-lookup"><span data-stu-id="59a7b-106">**Automatic operating system update installation**.</span></span> <span data-ttu-id="59a7b-107">Uppdateringar av operativsystemet hämtas och installeras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="59a7b-107">Operating system updates are automatically downloaded and installed.</span></span> <span data-ttu-id="59a7b-108">Noder i klustret startas om vid behov utan klusterdriftstopp.</span><span class="sxs-lookup"><span data-stu-id="59a7b-108">Cluster nodes are rebooted as needed without cluster downtime.</span></span>

- <span data-ttu-id="59a7b-109">**Klustermedveten uppdatering och hälsa integration**.</span><span class="sxs-lookup"><span data-stu-id="59a7b-109">**Cluster-aware patching and health integration**.</span></span> <span data-ttu-id="59a7b-110">När du tillämpar uppdateringar övervakar korrigering orchestration appen hälsan för klusternoderna.</span><span class="sxs-lookup"><span data-stu-id="59a7b-110">While applying updates, the patch orchestration app monitors the health of the cluster nodes.</span></span> <span data-ttu-id="59a7b-111">Klusternoder är uppgraderade en nod i taget eller en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="59a7b-111">Cluster nodes are upgraded one node at a time or one upgrade domain at a time.</span></span> <span data-ttu-id="59a7b-112">Om hälsotillståndet för klustret slutar att fungera på grund av uppdatering processen stoppas korrigering för att förhindra aggravating problemet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-112">If the health of the cluster goes down due to the patching process, patching is stopped to prevent aggravating the problem.</span></span>

## <a name="internal-details-of-the-app"></a><span data-ttu-id="59a7b-113">Intern information om appen</span><span class="sxs-lookup"><span data-stu-id="59a7b-113">Internal details of the app</span></span>

<span data-ttu-id="59a7b-114">Korrigering orchestration appen består av följande delkomponenter:</span><span class="sxs-lookup"><span data-stu-id="59a7b-114">The patch orchestration app is composed of the following subcomponents:</span></span>

- <span data-ttu-id="59a7b-115">**Coordinator-tjänsten**: tillståndskänslig tjänsten ansvarar för att:</span><span class="sxs-lookup"><span data-stu-id="59a7b-115">**Coordinator Service**: This stateful service is responsible for:</span></span>
    - <span data-ttu-id="59a7b-116">Samordning av Windows Update-jobbet på hela klustret.</span><span class="sxs-lookup"><span data-stu-id="59a7b-116">Coordinating the Windows Update job on the entire cluster.</span></span>
    - <span data-ttu-id="59a7b-117">Lagra resultatet av slutförda Windows Update-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="59a7b-117">Storing the result of completed Windows Update operations.</span></span>
- <span data-ttu-id="59a7b-118">**Nod-agenttjänsten**: tillståndslösa tjänsten körs på alla klusternoder för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="59a7b-118">**Node Agent Service**: This stateless service runs on all Service Fabric cluster nodes.</span></span> <span data-ttu-id="59a7b-119">Tjänsten ansvarar för:</span><span class="sxs-lookup"><span data-stu-id="59a7b-119">The service is responsible for:</span></span>
    - <span data-ttu-id="59a7b-120">Startprogram för noden Agent NTService.</span><span class="sxs-lookup"><span data-stu-id="59a7b-120">Bootstrapping the Node Agent NTService.</span></span>
    - <span data-ttu-id="59a7b-121">Övervakning av noden Agent NTService.</span><span class="sxs-lookup"><span data-stu-id="59a7b-121">Monitoring the Node Agent NTService.</span></span>
- <span data-ttu-id="59a7b-122">**Noden Agent NTService**: den här Windows NT-tjänsten körs på en högre nivå behörighet (SYSTEM).</span><span class="sxs-lookup"><span data-stu-id="59a7b-122">**Node Agent NTService**: This Windows NT service runs at a higher-level privilege (SYSTEM).</span></span> <span data-ttu-id="59a7b-123">Däremot nod Agent-tjänsten och Coordinator-tjänsten som körs på en lägre nivå behörighet (NETWORK SERVICE).</span><span class="sxs-lookup"><span data-stu-id="59a7b-123">In contrast, the Node Agent Service and the Coordinator Service run at a lower-level privilege (NETWORK SERVICE).</span></span> <span data-ttu-id="59a7b-124">Tjänsten ansvarar för att utföra följande Windows Update-jobb på alla noder i klustret:</span><span class="sxs-lookup"><span data-stu-id="59a7b-124">The service is responsible for performing the following Windows Update jobs on all the cluster nodes:</span></span>
    - <span data-ttu-id="59a7b-125">Inaktiverar automatisk Windows Update på noden.</span><span class="sxs-lookup"><span data-stu-id="59a7b-125">Disabling automatic Windows Update on the node.</span></span>
    - <span data-ttu-id="59a7b-126">Hämta och installera Windows Update enligt principen har användaren angett.</span><span class="sxs-lookup"><span data-stu-id="59a7b-126">Downloading and installing Windows Update according to the policy the user has provided.</span></span>
    - <span data-ttu-id="59a7b-127">Starta om datorn efter installationen av Windows Update.</span><span class="sxs-lookup"><span data-stu-id="59a7b-127">Restarting the machine post Windows Update installation.</span></span>
    - <span data-ttu-id="59a7b-128">Överför resultat av Windows-uppdateringar till Coordinator-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59a7b-128">Uploading the results of Windows updates to the Coordinator Service.</span></span>
    - <span data-ttu-id="59a7b-129">Reporting systemhälsorapporter om en åtgärd misslyckades efter överbelastar alla nya försök.</span><span class="sxs-lookup"><span data-stu-id="59a7b-129">Reporting health reports in case an operation has failed after exhausting all retries.</span></span>

> [!NOTE]
> <span data-ttu-id="59a7b-130">Korrigering orchestration appen använder tjänsten Service Fabric reparera manager system för att inaktivera eller aktivera noden och utföra hälsokontroller.</span><span class="sxs-lookup"><span data-stu-id="59a7b-130">The patch orchestration app uses the Service Fabric repair manager system service to disable or enable the node and perform health checks.</span></span> <span data-ttu-id="59a7b-131">Reparationsuppgiften som skapats av korrigering orchestration appen spårar förloppet för Windows Update för varje nod.</span><span class="sxs-lookup"><span data-stu-id="59a7b-131">The repair task created by the patch orchestration app tracks the Windows Update progress for each node.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59a7b-132">Krav</span><span class="sxs-lookup"><span data-stu-id="59a7b-132">Prerequisites</span></span>

### <a name="minimum-supported-service-fabric-runtime-version"></a><span data-ttu-id="59a7b-133">Minimal stödd version av Service Fabric runtime</span><span class="sxs-lookup"><span data-stu-id="59a7b-133">Minimum supported Service Fabric runtime version</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="59a7b-134">Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="59a7b-134">Azure clusters</span></span>
<span data-ttu-id="59a7b-135">Korrigering orchestration-app måste köras på Azure kluster som har Service Fabric runtime version version 5.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="59a7b-135">The patch orchestration app must be run on Azure clusters that have Service Fabric runtime version v5.5 or later.</span></span>

#### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="59a7b-136">Fristående lokala kluster</span><span class="sxs-lookup"><span data-stu-id="59a7b-136">Standalone on-premises Clusters</span></span>
<span data-ttu-id="59a7b-137">Korrigering av orchestration-app måste köras på fristående kluster som har Service Fabric runtime version v5.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="59a7b-137">The patch orchestration app must be run on Standalone clusters that have Service Fabric runtime version v5.6 or later.</span></span>

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a><span data-ttu-id="59a7b-138">Aktivera tjänsten reparera manager (om det inte redan körs)</span><span class="sxs-lookup"><span data-stu-id="59a7b-138">Enable the repair manager service (if it's not running already)</span></span>

<span data-ttu-id="59a7b-139">Korrigering orchestration appen kräver tjänsten reparera manager system måste vara aktiverade på klustret.</span><span class="sxs-lookup"><span data-stu-id="59a7b-139">The patch orchestration app requires the repair manager system service to be enabled on the cluster.</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="59a7b-140">Azure-kluster</span><span class="sxs-lookup"><span data-stu-id="59a7b-140">Azure clusters</span></span>

<span data-ttu-id="59a7b-141">Azure-kluster i silver hållbarhetsnivån har tjänsten reparera manager aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="59a7b-141">Azure clusters in the silver durability tier have the repair manager service enabled by default.</span></span> <span data-ttu-id="59a7b-142">Azure-kluster i guld hållbarhetsnivån kanske eller kanske inte reparera manager-tjänsten aktiverad, beroende på när dessa kluster skapades.</span><span class="sxs-lookup"><span data-stu-id="59a7b-142">Azure clusters in the gold durability tier might or might not have the repair manager service enabled, depending on when those clusters were created.</span></span> <span data-ttu-id="59a7b-143">Azure-kluster i Brons hållbarhetsnivån som standard har inte reparera manager-tjänsten aktiverad.</span><span class="sxs-lookup"><span data-stu-id="59a7b-143">Azure clusters in the bronze durability tier, by default, do not have the repair manager service enabled.</span></span> <span data-ttu-id="59a7b-144">Om den redan är aktiverad, visas den i avsnittet system tjänster i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="59a7b-144">If the service is already enabled, you can see it running in the system services section in the Service Fabric Explorer.</span></span>

##### <a name="azure-portal"></a><span data-ttu-id="59a7b-145">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="59a7b-145">Azure portal</span></span>
<span data-ttu-id="59a7b-146">Du kan aktivera reparera manager från Azure-portalen när du konfigurerar i klustret.</span><span class="sxs-lookup"><span data-stu-id="59a7b-146">You can enable repair manager from Azure portal at the time of setting up of cluster.</span></span> <span data-ttu-id="59a7b-147">Välj `Include Repair Manager` alternativ `Add on features` vid tidpunkten för klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="59a7b-147">Select `Include Repair Manager` option under `Add on features` at the time of Cluster configuration.</span></span>
<span data-ttu-id="59a7b-148">![Bild för att aktivera reparera Manager från Azure-portalen](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span><span class="sxs-lookup"><span data-stu-id="59a7b-148">![Image of Enabling Repair Manager from Azure portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span></span>

##### <a name="azure-resource-manager-template"></a><span data-ttu-id="59a7b-149">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="59a7b-149">Azure Resource Manager template</span></span>
<span data-ttu-id="59a7b-150">Du kan också använda den [Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) att reparera manager-tjänsten på nya och befintliga Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="59a7b-150">Alternatively you can use the [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) to enable the repair manager service on new and existing Service Fabric clusters.</span></span> <span data-ttu-id="59a7b-151">Hämta mallen för det kluster som du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="59a7b-151">Get the template for the cluster that you want to deploy.</span></span> <span data-ttu-id="59a7b-152">Du kan använda exempelmallarna, eller så kan du skapa en anpassad mall för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="59a7b-152">You can either use the sample templates or create a custom Resource Manager template.</span></span> 

<span data-ttu-id="59a7b-153">Så här aktiverar du reparera manager service med [Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span><span class="sxs-lookup"><span data-stu-id="59a7b-153">To enable the repair manager service using [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span></span>

1. <span data-ttu-id="59a7b-154">Kontrollera först att den `apiversion` är inställd på `2017-07-01-preview` för den `Microsoft.ServiceFabric/clusters` resurs, enligt följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="59a7b-154">First check that the `apiversion` is set to `2017-07-01-preview` for the `Microsoft.ServiceFabric/clusters` resource, as shown in the following snippet.</span></span> <span data-ttu-id="59a7b-155">Om det är olika, så måste du uppdatera den `apiVersion` till värdet `2017-07-01-preview`:</span><span class="sxs-lookup"><span data-stu-id="59a7b-155">If it is different, then you need to update the `apiVersion` to the value `2017-07-01-preview`:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="59a7b-156">Aktivera nu reparera manager-tjänsten genom att lägga till följande `addonFeatures` avsnittet efter den `fabricSettings` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="59a7b-156">Now enable the repair manager service by adding the following `addonFeatures` section after the `fabricSettings` section:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="59a7b-157">När du har uppdaterat mallen för klustret med de här ändringarna, använda dem och låt uppgraderingen är klar.</span><span class="sxs-lookup"><span data-stu-id="59a7b-157">After you have updated your cluster template with these changes, apply them and let the upgrade finish.</span></span> <span data-ttu-id="59a7b-158">Du kan nu se reparera manager system-tjänsten körs i klustret.</span><span class="sxs-lookup"><span data-stu-id="59a7b-158">You can now see the repair manager system service running in your cluster.</span></span> <span data-ttu-id="59a7b-159">Den kallas `fabric:/System/RepairManagerService` i avsnittet system tjänster i Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="59a7b-159">It is called `fabric:/System/RepairManagerService` in the system services section in the Service Fabric Explorer.</span></span> 

### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="59a7b-160">Fristående lokala kluster</span><span class="sxs-lookup"><span data-stu-id="59a7b-160">Standalone on-premises clusters</span></span>

<span data-ttu-id="59a7b-161">Du kan använda den [konfigurationsinställningar för Windows-kluster för fristående](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) att reparera manager-tjänsten på nya och befintliga Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="59a7b-161">You can use the [Configuration settings for standalone Windows cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) to enable the repair manager service on new and existing Service Fabric cluster.</span></span>

<span data-ttu-id="59a7b-162">Aktivera tjänsten reparera manager:</span><span class="sxs-lookup"><span data-stu-id="59a7b-162">To enable the repair manager service:</span></span>

1. <span data-ttu-id="59a7b-163">Kontrollera först att den `apiversion` i [allmänna klusterkonfigurationer](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) är inställd på `04-2017` eller senare:</span><span class="sxs-lookup"><span data-stu-id="59a7b-163">First check that the `apiversion` in [General cluster configurations](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) is set to `04-2017` or higher:</span></span>

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. <span data-ttu-id="59a7b-164">Aktivera nu reparera manager-tjänsten genom att lägga till följande `addonFeaturres` avsnittet efter den `fabricSettings` enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="59a7b-164">Now enable repair manager service by adding the following `addonFeaturres` section after the `fabricSettings` section as shown below:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="59a7b-165">Uppdatera din klustermanifestet med ändringarna med hjälp av det uppdaterade klustermanifestet [skapa ett nytt kluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) eller [uppgradera klusterkonfigurationen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span><span class="sxs-lookup"><span data-stu-id="59a7b-165">Update your cluster manifest with these changes, using the updated cluster manifest [create a new cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) or [upgrade the cluster configuration](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span></span> <span data-ttu-id="59a7b-166">När klustret körs med uppdaterade klustermanifestet kan du nu se reparera manager system-tjänsten körs i klustret, som kallas `fabric:/System/RepairManagerService`under system services-avsnittet i Service Fabric explorer.</span><span class="sxs-lookup"><span data-stu-id="59a7b-166">Once the cluster is running with updated cluster manifest, you can now see the repair manager system service running in your cluster, which is called `fabric:/System/RepairManagerService`, under system services section in the Service Fabric explorer.</span></span>

### <a name="disable-automatic-windows-update-on-all-nodes"></a><span data-ttu-id="59a7b-167">Inaktivera automatisk uppdatering för Windows på alla noder</span><span class="sxs-lookup"><span data-stu-id="59a7b-167">Disable automatic Windows Update on all nodes</span></span>

<span data-ttu-id="59a7b-168">Automatiska uppdateringar kan det leda till förlust av tillgänglighet eftersom flera klusternoder kan starta om på samma gång.</span><span class="sxs-lookup"><span data-stu-id="59a7b-168">Automatic Windows updates might lead to availability loss because multiple cluster nodes can restart at the same time.</span></span> <span data-ttu-id="59a7b-169">Appen korrigering orchestration försöker som standard inaktivera automatisk uppdatering för Windows på varje nod i klustret.</span><span class="sxs-lookup"><span data-stu-id="59a7b-169">The patch orchestration app, by default, tries to disable the automatic Windows Update on each cluster node.</span></span> <span data-ttu-id="59a7b-170">Om inställningarna hanteras av en administratör eller en Grupprincip, rekommenderar vi dock ställa in Windows Update-princip att ”Avisera innan hämta” explicit.</span><span class="sxs-lookup"><span data-stu-id="59a7b-170">However, if the settings are managed by an administrator or group policy, we recommend setting the Windows Update policy to “Notify before Download” explicitly.</span></span>

### <a name="optional-enable-azure-diagnostics"></a><span data-ttu-id="59a7b-171">Valfritt: Aktivera Azure-diagnostik</span><span class="sxs-lookup"><span data-stu-id="59a7b-171">Optional: Enable Azure Diagnostics</span></span>

<span data-ttu-id="59a7b-172">Kluster som kör Service Fabric runtime version `5.6.220.9494` och ovan samla in korrigering orchestration app loggarna som en del av Service Fabric-loggarna.</span><span class="sxs-lookup"><span data-stu-id="59a7b-172">Clusters running Service Fabric runtime version `5.6.220.9494` and above collect patch orchestration app logs as part of Service Fabric logs.</span></span>
<span data-ttu-id="59a7b-173">Du kan hoppa över det här steget om klustret körs i Service Fabric runtime version `5.6.220.9494` och högre.</span><span class="sxs-lookup"><span data-stu-id="59a7b-173">You can skip this step if your cluster is running on Service Fabric runtime version `5.6.220.9494` and above.</span></span>

<span data-ttu-id="59a7b-174">För kluster som kör Service Fabric runtime version mindre än `5.6.220.9494`, loggar för korrigering orchestration appen samlas lokalt på alla klusternoder.</span><span class="sxs-lookup"><span data-stu-id="59a7b-174">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs for the patch orchestration app are collected locally on each of the cluster nodes.</span></span>
<span data-ttu-id="59a7b-175">Vi rekommenderar att du konfigurerar Azure-diagnostik för att ladda upp loggar från alla noder till en central plats.</span><span class="sxs-lookup"><span data-stu-id="59a7b-175">We recommend that you configure Azure Diagnostics to upload logs from all nodes to a central location.</span></span>

<span data-ttu-id="59a7b-176">Information om hur du aktiverar Azure-diagnostik finns [samla in loggar med hjälp av Azure-diagnostik](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span><span class="sxs-lookup"><span data-stu-id="59a7b-176">For information on enabling Azure Diagnostics, see [Collect logs by using Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span></span>

<span data-ttu-id="59a7b-177">Loggfiler för appen korrigering orchestration genereras på följande fasta provider ID: N:</span><span class="sxs-lookup"><span data-stu-id="59a7b-177">Logs for the patch orchestration app are generated on the following fixed provider IDs:</span></span>

- <span data-ttu-id="59a7b-178">e39b723c-590c-4090-abb0-11e3e6616346</span><span class="sxs-lookup"><span data-stu-id="59a7b-178">e39b723c-590c-4090-abb0-11e3e6616346</span></span>
- <span data-ttu-id="59a7b-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span><span class="sxs-lookup"><span data-stu-id="59a7b-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span></span>
- <span data-ttu-id="59a7b-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span><span class="sxs-lookup"><span data-stu-id="59a7b-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span></span>
- <span data-ttu-id="59a7b-181">05dc046c-60e9-4ef7-965e-91660adffa68</span><span class="sxs-lookup"><span data-stu-id="59a7b-181">05dc046c-60e9-4ef7-965e-91660adffa68</span></span>

<span data-ttu-id="59a7b-182">I Resource Manager mallen goto `EtwEventSourceProviderConfiguration` avsnittet `WadCfg` och Lägg till följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="59a7b-182">In Resource Manager template goto `EtwEventSourceProviderConfiguration` section under `WadCfg` and add the following entries:</span></span>

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
> <span data-ttu-id="59a7b-183">Om din Service Fabric-klustret har flera nodtyper och sedan det föregående avsnittet måste läggas till för alla de `WadCfg` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="59a7b-183">If your Service Fabric cluster has multiple node types, then the previous section must be added for all the `WadCfg` sections.</span></span>

## <a name="download-the-app-package"></a><span data-ttu-id="59a7b-184">Hämta app-paket</span><span class="sxs-lookup"><span data-stu-id="59a7b-184">Download the app package</span></span>

<span data-ttu-id="59a7b-185">Hämta programmet från den [Hämta länk](https://go.microsoft.com/fwlink/P/?linkid=849590).</span><span class="sxs-lookup"><span data-stu-id="59a7b-185">Download the application from the [download link](https://go.microsoft.com/fwlink/P/?linkid=849590).</span></span>

## <a name="configure-the-app"></a><span data-ttu-id="59a7b-186">Konfigurera appen</span><span class="sxs-lookup"><span data-stu-id="59a7b-186">Configure the app</span></span>

<span data-ttu-id="59a7b-187">Korrigering av orchestration appens beteende kan konfigureras för att uppfylla dina behov.</span><span class="sxs-lookup"><span data-stu-id="59a7b-187">The behavior of the patch orchestration app can be configured to meet your needs.</span></span> <span data-ttu-id="59a7b-188">Åsidosätta standardvärdena genom att passera i parametern program under skapa program eller uppdatering.</span><span class="sxs-lookup"><span data-stu-id="59a7b-188">Override the default values by passing in the application parameter during application creation or update.</span></span> <span data-ttu-id="59a7b-189">Parametrar för program kan anges genom att ange `ApplicationParameter` till den `Start-ServiceFabricApplicationUpgrade` eller `New-ServiceFabricApplication` cmdlets.</span><span class="sxs-lookup"><span data-stu-id="59a7b-189">Application parameters can be provided by specifying `ApplicationParameter` to the `Start-ServiceFabricApplicationUpgrade` or `New-ServiceFabricApplication` cmdlets.</span></span>

|<span data-ttu-id="59a7b-190">**Parametern**</span><span class="sxs-lookup"><span data-stu-id="59a7b-190">**Parameter**</span></span>        |<span data-ttu-id="59a7b-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="59a7b-191">**Type**</span></span>                          | <span data-ttu-id="59a7b-192">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="59a7b-192">**Details**</span></span>|
|:-|-|-|
|<span data-ttu-id="59a7b-193">MaxResultsToCache</span><span class="sxs-lookup"><span data-stu-id="59a7b-193">MaxResultsToCache</span></span>    |<span data-ttu-id="59a7b-194">Lång</span><span class="sxs-lookup"><span data-stu-id="59a7b-194">Long</span></span>                              | <span data-ttu-id="59a7b-195">Maximalt antal Windows Update-resultat, som ska cachelagras.</span><span class="sxs-lookup"><span data-stu-id="59a7b-195">Maximum number of Windows Update results, which should be cached.</span></span> <br><span data-ttu-id="59a7b-196">Standardvärdet är 3000 under förutsättning att den:</span><span class="sxs-lookup"><span data-stu-id="59a7b-196">Default value is 3000 assuming the:</span></span> <br> <span data-ttu-id="59a7b-197">-Antalet noder är 20.</span><span class="sxs-lookup"><span data-stu-id="59a7b-197">- Number of nodes is 20.</span></span> <br> <span data-ttu-id="59a7b-198">-Antalet uppdateringar som händer på en nod per månad är fem.</span><span class="sxs-lookup"><span data-stu-id="59a7b-198">- Number of updates happening on a node per month is five.</span></span> <br> <span data-ttu-id="59a7b-199">-Antalet resultat per åtgärd kan vara 10.</span><span class="sxs-lookup"><span data-stu-id="59a7b-199">- Number of results per operation can be 10.</span></span> <br> <span data-ttu-id="59a7b-200">-Resultat för de senaste tre månaderna ska lagras.</span><span class="sxs-lookup"><span data-stu-id="59a7b-200">- Results for the past three months should be stored.</span></span> |
|<span data-ttu-id="59a7b-201">TaskApprovalPolicy</span><span class="sxs-lookup"><span data-stu-id="59a7b-201">TaskApprovalPolicy</span></span>   |<span data-ttu-id="59a7b-202">Enum</span><span class="sxs-lookup"><span data-stu-id="59a7b-202">Enum</span></span> <br> <span data-ttu-id="59a7b-203">{NodeWise, UpgradeDomainWise}</span><span class="sxs-lookup"><span data-stu-id="59a7b-203">{ NodeWise, UpgradeDomainWise }</span></span>                          |<span data-ttu-id="59a7b-204">TaskApprovalPolicy anger den princip som ska användas av Coordinator-tjänsten för att installera Windows-uppdateringar för Service Fabric-klusternoder.</span><span class="sxs-lookup"><span data-stu-id="59a7b-204">TaskApprovalPolicy indicates the policy that is to be used by the Coordinator Service to install Windows updates across the Service Fabric cluster nodes.</span></span><br>                         <span data-ttu-id="59a7b-205">Tillåtna värden är:</span><span class="sxs-lookup"><span data-stu-id="59a7b-205">Allowed values are:</span></span> <br>                                                           <span data-ttu-id="59a7b-206"><b>NodeWise</b>.</span><span class="sxs-lookup"><span data-stu-id="59a7b-206"><b>NodeWise</b>.</span></span> <span data-ttu-id="59a7b-207">Windows Update är installerade en nod i taget.</span><span class="sxs-lookup"><span data-stu-id="59a7b-207">Windows Update is installed one node at a time.</span></span> <br>                                                           <span data-ttu-id="59a7b-208"><b>UpgradeDomainWise</b>.</span><span class="sxs-lookup"><span data-stu-id="59a7b-208"><b>UpgradeDomainWise</b>.</span></span> <span data-ttu-id="59a7b-209">Windows Update är installerade en domän i taget.</span><span class="sxs-lookup"><span data-stu-id="59a7b-209">Windows Update is installed one upgrade domain at a time.</span></span> <span data-ttu-id="59a7b-210">(Max, alla noder som tillhör en domän kan gå för Windows Update.)</span><span class="sxs-lookup"><span data-stu-id="59a7b-210">(At the maximum, all the nodes belonging to an upgrade domain can go for Windows Update.)</span></span>
|<span data-ttu-id="59a7b-211">LogsDiskQuotaInMB</span><span class="sxs-lookup"><span data-stu-id="59a7b-211">LogsDiskQuotaInMB</span></span>   |<span data-ttu-id="59a7b-212">Lång</span><span class="sxs-lookup"><span data-stu-id="59a7b-212">Long</span></span>  <br> <span data-ttu-id="59a7b-213">(Standard: 1024)</span><span class="sxs-lookup"><span data-stu-id="59a7b-213">(Default: 1024)</span></span>               |<span data-ttu-id="59a7b-214">Maximal storlek för korrigering orchestration app loggar i MB, vilket kan sparas lokalt på noder.</span><span class="sxs-lookup"><span data-stu-id="59a7b-214">Maximum size of patch orchestration app logs in MB, which can be persisted locally on nodes.</span></span>
| <span data-ttu-id="59a7b-215">WUQuery</span><span class="sxs-lookup"><span data-stu-id="59a7b-215">WUQuery</span></span>               | <span data-ttu-id="59a7b-216">Sträng</span><span class="sxs-lookup"><span data-stu-id="59a7b-216">string</span></span><br><span data-ttu-id="59a7b-217">(Standard ”: IsInstalled = 0”)</span><span class="sxs-lookup"><span data-stu-id="59a7b-217">(Default: "IsInstalled=0")</span></span>                | <span data-ttu-id="59a7b-218">Frågan att hämta Windows-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="59a7b-218">Query to get Windows updates.</span></span> <span data-ttu-id="59a7b-219">Mer information finns i [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="59a7b-219">For more information, see [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span></span>
| <span data-ttu-id="59a7b-220">InstallWindowsOSOnlyUpdates</span><span class="sxs-lookup"><span data-stu-id="59a7b-220">InstallWindowsOSOnlyUpdates</span></span> | <span data-ttu-id="59a7b-221">bool</span><span class="sxs-lookup"><span data-stu-id="59a7b-221">Bool</span></span> <br> <span data-ttu-id="59a7b-222">(standard: True)</span><span class="sxs-lookup"><span data-stu-id="59a7b-222">(default: True)</span></span>                 | <span data-ttu-id="59a7b-223">Den här flaggan kan uppdateringar av Windows-operativsystemet installeras.</span><span class="sxs-lookup"><span data-stu-id="59a7b-223">This flag allows Windows operating system updates to be installed.</span></span>            |
| <span data-ttu-id="59a7b-224">WUOperationTimeOutInMinutes</span><span class="sxs-lookup"><span data-stu-id="59a7b-224">WUOperationTimeOutInMinutes</span></span> | <span data-ttu-id="59a7b-225">int</span><span class="sxs-lookup"><span data-stu-id="59a7b-225">Int</span></span> <br><span data-ttu-id="59a7b-226">(Standard: 90).</span><span class="sxs-lookup"><span data-stu-id="59a7b-226">(Default: 90)</span></span>                   | <span data-ttu-id="59a7b-227">Anger timeout för någon Windows Update-åtgärd (Sök eller ladda ned eller installera).</span><span class="sxs-lookup"><span data-stu-id="59a7b-227">Specifies the timeout for any Windows Update operation (search or download or install).</span></span> <span data-ttu-id="59a7b-228">Om åtgärden inte slutförs inom den angivna tidsgränsen, avbryts.</span><span class="sxs-lookup"><span data-stu-id="59a7b-228">If the operation is not completed within the specified timeout, it is aborted.</span></span>       |
| <span data-ttu-id="59a7b-229">WURescheduleCount</span><span class="sxs-lookup"><span data-stu-id="59a7b-229">WURescheduleCount</span></span>     | <span data-ttu-id="59a7b-230">int</span><span class="sxs-lookup"><span data-stu-id="59a7b-230">Int</span></span> <br> <span data-ttu-id="59a7b-231">(Standard: 5).</span><span class="sxs-lookup"><span data-stu-id="59a7b-231">(Default: 5)</span></span>                  | <span data-ttu-id="59a7b-232">Om en åtgärd misslyckas så att uppdatera det maximala antalet gånger som tjänsten schemaläggs automatiskt på nytt i Windows.</span><span class="sxs-lookup"><span data-stu-id="59a7b-232">The maximum number of times the service reschedules the Windows update in case an operation fails persistently.</span></span>          |
| <span data-ttu-id="59a7b-233">WURescheduleTimeInMinutes</span><span class="sxs-lookup"><span data-stu-id="59a7b-233">WURescheduleTimeInMinutes</span></span> | <span data-ttu-id="59a7b-234">int</span><span class="sxs-lookup"><span data-stu-id="59a7b-234">Int</span></span> <br><span data-ttu-id="59a7b-235">(Standard: 30).</span><span class="sxs-lookup"><span data-stu-id="59a7b-235">(Default: 30)</span></span> | <span data-ttu-id="59a7b-236">Intervallet då tjänsten schemaläggs automatiskt på nytt Windows update om felet kvarstår.</span><span class="sxs-lookup"><span data-stu-id="59a7b-236">The interval at which the service reschedules the Windows update in case failure persists.</span></span> |
| <span data-ttu-id="59a7b-237">WUFrequency</span><span class="sxs-lookup"><span data-stu-id="59a7b-237">WUFrequency</span></span>           | <span data-ttu-id="59a7b-238">Kommaavgränsad sträng (standard: ”vecka, Onsdag 7:00:00”)</span><span class="sxs-lookup"><span data-stu-id="59a7b-238">Comma-separated string (Default: "Weekly, Wednesday, 7:00:00")</span></span>     | <span data-ttu-id="59a7b-239">Frekvensen för att installera Windows Update.</span><span class="sxs-lookup"><span data-stu-id="59a7b-239">The frequency for installing Windows Update.</span></span> <span data-ttu-id="59a7b-240">Format och möjliga värden är:</span><span class="sxs-lookup"><span data-stu-id="59a7b-240">The format and possible values are:</span></span> <br><span data-ttu-id="59a7b-241">-: Som mm: ss varje månad, DD, till exempel varje månad, 5, 12: 22:32.</span><span class="sxs-lookup"><span data-stu-id="59a7b-241">-   Monthly, DD,HH:MM:SS, for example, Monthly, 5,12:22:32.</span></span> <br> <span data-ttu-id="59a7b-242">-Varje vecka, dag,: mm: ss, för exempelvis varje vecka, tisdag, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="59a7b-242">-   Weekly, DAY,HH:MM:SS, for example, Weekly, Tuesday, 12:22:32.</span></span>  <br> <span data-ttu-id="59a7b-243">-Varje dag,: mm: ss, till exempel dagligen, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="59a7b-243">-   Daily, HH:MM:SS, for example, Daily, 12:22:32.</span></span>  <br> <span data-ttu-id="59a7b-244">-Ingen anger att Windows Update inte utföras.</span><span class="sxs-lookup"><span data-stu-id="59a7b-244">-  None indicates that Windows Update shouldn't be done.</span></span>  <br><br> <span data-ttu-id="59a7b-245">Observera att hela tiden i UTC.</span><span class="sxs-lookup"><span data-stu-id="59a7b-245">Note that all the times are in UTC.</span></span>|
| <span data-ttu-id="59a7b-246">AcceptWindowsUpdateEula</span><span class="sxs-lookup"><span data-stu-id="59a7b-246">AcceptWindowsUpdateEula</span></span> | <span data-ttu-id="59a7b-247">bool</span><span class="sxs-lookup"><span data-stu-id="59a7b-247">Bool</span></span> <br><span data-ttu-id="59a7b-248">(Standard: true)</span><span class="sxs-lookup"><span data-stu-id="59a7b-248">(Default: true)</span></span> | <span data-ttu-id="59a7b-249">Genom att ange den här flaggan accepterar programmet slutanvändarens licensavtal för Windows Update för ägare för datorn.</span><span class="sxs-lookup"><span data-stu-id="59a7b-249">By setting this flag, the application accepts the End-User License Agreement for Windows Update on behalf of the owner of the machine.</span></span>              |

> [!TIP]
> <span data-ttu-id="59a7b-250">Om du vill att Windows Update sker omedelbart anger `WUFrequency` i förhållande till tidpunkten för distribution av programmet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-250">If you want Windows Update to happen immediately, set `WUFrequency` relative to the application deployment time.</span></span> <span data-ttu-id="59a7b-251">Anta att du har ett testkluster med fem noder och planerar att distribuera appen vid cirka 17:00:00 UTC.</span><span class="sxs-lookup"><span data-stu-id="59a7b-251">For example, suppose that you have a five-node test cluster and plan to deploy the app at around 5:00 PM UTC.</span></span> <span data-ttu-id="59a7b-252">Om du anta att programmet uppgraderingen eller distributionen tar 30 minuter Max, anger du WUFrequency som ”varje dag, 17:30:00”.</span><span class="sxs-lookup"><span data-stu-id="59a7b-252">If you assume that the application upgrade or deployment takes 30 minutes at the maximum, set the WUFrequency as "Daily, 17:30:00."</span></span>

## <a name="deploy-the-app"></a><span data-ttu-id="59a7b-253">Distribuera appen</span><span class="sxs-lookup"><span data-stu-id="59a7b-253">Deploy the app</span></span>

1. <span data-ttu-id="59a7b-254">Slut alla nödvändiga steg för att förbereda klustret.</span><span class="sxs-lookup"><span data-stu-id="59a7b-254">Finish all the prerequisite steps to prepare the cluster.</span></span>
2. <span data-ttu-id="59a7b-255">Distribuera appen korrigering orchestration som andra Service Fabric-app.</span><span class="sxs-lookup"><span data-stu-id="59a7b-255">Deploy the patch orchestration app like any other Service Fabric app.</span></span> <span data-ttu-id="59a7b-256">Du kan distribuera appen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="59a7b-256">You can deploy the app by using PowerShell.</span></span> <span data-ttu-id="59a7b-257">Följ stegen i [distribuera och ta bort program med hjälp av PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="59a7b-257">Follow the steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>
3. <span data-ttu-id="59a7b-258">För att konfigurera programmet vid tidpunkten för distribution, skickar den `ApplicationParamater` till den `New-ServiceFabricApplication` cmdlet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-258">To configure the application at the time of deployment, pass the `ApplicationParamater` to the `New-ServiceFabricApplication` cmdlet.</span></span> <span data-ttu-id="59a7b-259">Vi har samlat skriptet Deploy.ps1 tillsammans med programmet för din bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-259">For your convenience, we’ve provided the script Deploy.ps1 along with the application.</span></span> <span data-ttu-id="59a7b-260">Du använder skriptet:</span><span class="sxs-lookup"><span data-stu-id="59a7b-260">To use the script:</span></span>

    - <span data-ttu-id="59a7b-261">Ansluta till ett Service Fabric-kluster med hjälp av `Connect-ServiceFabricCluster`.</span><span class="sxs-lookup"><span data-stu-id="59a7b-261">Connect to a Service Fabric cluster by using `Connect-ServiceFabricCluster`.</span></span>
    - <span data-ttu-id="59a7b-262">Köra PowerShell-skriptet Deploy.ps1 med lämplig `ApplicationParameter` värde.</span><span class="sxs-lookup"><span data-stu-id="59a7b-262">Execute the PowerShell script Deploy.ps1 with the appropriate `ApplicationParameter` value.</span></span>

> [!NOTE]
> <span data-ttu-id="59a7b-263">Spara skriptet och programmappen PatchOrchestrationApplication i samma katalog.</span><span class="sxs-lookup"><span data-stu-id="59a7b-263">Keep the script and the application folder PatchOrchestrationApplication in the same directory.</span></span>

## <a name="upgrade-the-app"></a><span data-ttu-id="59a7b-264">Uppgradera appen</span><span class="sxs-lookup"><span data-stu-id="59a7b-264">Upgrade the app</span></span>

<span data-ttu-id="59a7b-265">Om du vill uppgradera en befintlig korrigering orchestration app med hjälp av PowerShell, följer du stegen i [uppgradering av Service Fabric-programmet med PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span><span class="sxs-lookup"><span data-stu-id="59a7b-265">To upgrade an existing patch orchestration app by using PowerShell, follow the steps in [Service Fabric application upgrade using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span></span>

## <a name="remove-the-app"></a><span data-ttu-id="59a7b-266">Ta bort appen</span><span class="sxs-lookup"><span data-stu-id="59a7b-266">Remove the app</span></span>

<span data-ttu-id="59a7b-267">Om du vill ta bort programmet, följer du stegen i [distribuera och ta bort program med hjälp av PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="59a7b-267">To remove the application, follow the steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>

<span data-ttu-id="59a7b-268">Vi har samlat skriptet Undeploy.ps1 tillsammans med programmet för din bekvämlighet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-268">For your convenience, we've provided the script Undeploy.ps1 along with the application.</span></span> <span data-ttu-id="59a7b-269">Du använder skriptet:</span><span class="sxs-lookup"><span data-stu-id="59a7b-269">To use the script:</span></span>

  - <span data-ttu-id="59a7b-270">Ansluta till ett Service Fabric-kluster med hjälp av ```Connect-ServiceFabricCluster```.</span><span class="sxs-lookup"><span data-stu-id="59a7b-270">Connect to a Service Fabric cluster by using ```Connect-ServiceFabricCluster```.</span></span>

  - <span data-ttu-id="59a7b-271">Köra PowerShell-skriptet Undeploy.ps1.</span><span class="sxs-lookup"><span data-stu-id="59a7b-271">Execute the PowerShell script Undeploy.ps1.</span></span>

> [!NOTE]
> <span data-ttu-id="59a7b-272">Spara skriptet och programmappen PatchOrchestrationApplication i samma katalog.</span><span class="sxs-lookup"><span data-stu-id="59a7b-272">Keep the script and the application folder PatchOrchestrationApplication in the same directory.</span></span>

## <a name="view-the-windows-update-results"></a><span data-ttu-id="59a7b-273">Visa resultaten för Windows Update</span><span class="sxs-lookup"><span data-stu-id="59a7b-273">View the Windows Update results</span></span>

<span data-ttu-id="59a7b-274">Korrigering orchestration appen exponerar REST API: er för att visa historiska resultat för användaren.</span><span class="sxs-lookup"><span data-stu-id="59a7b-274">The patch orchestration app exposes REST APIs to display the historical results to the user.</span></span> <span data-ttu-id="59a7b-275">Ett exempel på resultat JSON:</span><span class="sxs-lookup"><span data-stu-id="59a7b-275">An example of the result JSON:</span></span>
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
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
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
<span data-ttu-id="59a7b-276">Om ingen uppdatering har schemalagts ännu är resultatet JSON tom.</span><span class="sxs-lookup"><span data-stu-id="59a7b-276">If no update is scheduled yet, the result JSON is empty.</span></span>

<span data-ttu-id="59a7b-277">Logga in till klustret för att fråga Windows Update resultat.</span><span class="sxs-lookup"><span data-stu-id="59a7b-277">Log in to the cluster to query Windows Update results.</span></span> <span data-ttu-id="59a7b-278">Sedan reda på replik-adressen för primär av Coordinator-tjänsten och tryck URL i webbläsaren: http://&lt;REPLIK-IP-&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="59a7b-278">Then find out the replica address for the primary of the Coordinator Service, and hit the URL from the browser: http://&lt;REPLICA-IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="59a7b-279">REST-slutpunkt för Coordinator-tjänsten har en dynamisk port.</span><span class="sxs-lookup"><span data-stu-id="59a7b-279">The REST endpoint for the Coordinator Service has a dynamic port.</span></span> <span data-ttu-id="59a7b-280">Referera till Service Fabric Explorer för att kontrollera en exakt Webbadress.</span><span class="sxs-lookup"><span data-stu-id="59a7b-280">To check the exact URL, refer to the Service Fabric Explorer.</span></span> <span data-ttu-id="59a7b-281">Till exempel resultaten finns på `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span><span class="sxs-lookup"><span data-stu-id="59a7b-281">For example, the results are available at `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span></span>

![Bild av REST-slutpunkt](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


<span data-ttu-id="59a7b-283">Du kan komma åt URL: en från utanför klustret samt om omvänd proxy är aktiverat på klustret.</span><span class="sxs-lookup"><span data-stu-id="59a7b-283">If the reverse proxy is enabled on the cluster, you can access the URL from outside of the cluster as well.</span></span>
<span data-ttu-id="59a7b-284">Den slutpunkt som måste vara träffar är http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="59a7b-284">The endpoint that needs to be hit is http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="59a7b-285">För att aktivera omvänd proxy i klustret, följer du stegen i [omvänd proxy i Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span><span class="sxs-lookup"><span data-stu-id="59a7b-285">To enable the reverse proxy on the cluster, follow the steps in [Reverse proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span></span> 

> 
> [!WARNING]
> <span data-ttu-id="59a7b-286">När omvänd proxy har konfigurerats, är alla micro tjänster i klustret som Exponerar en HTTP-slutpunkt adresserbara från utanför klustret.</span><span class="sxs-lookup"><span data-stu-id="59a7b-286">After the reverse proxy is configured, all micro services in the cluster that expose an HTTP endpoint are addressable from outside the cluster.</span></span>

## <a name="diagnosticshealth-events"></a><span data-ttu-id="59a7b-287">Diagnostik/hälsotillstånd händelser</span><span class="sxs-lookup"><span data-stu-id="59a7b-287">Diagnostics/health events</span></span>

### <a name="collect-patch-orchestration-app-logs"></a><span data-ttu-id="59a7b-288">Samla in korrigering orchestration app loggar</span><span class="sxs-lookup"><span data-stu-id="59a7b-288">Collect patch orchestration app logs</span></span>

<span data-ttu-id="59a7b-289">Korrigering orchestration app loggarna har samlats in som en del av Service Fabric-loggar från körningsversion `5.6.220.9494` och högre.</span><span class="sxs-lookup"><span data-stu-id="59a7b-289">Patch orchestration app logs are collected as part of Service Fabric logs from runtime version `5.6.220.9494` and above.</span></span>
<span data-ttu-id="59a7b-290">För kluster som kör Service Fabric runtime version mindre än `5.6.220.9494`, loggar samlas in genom att använda någon av följande metoder.</span><span class="sxs-lookup"><span data-stu-id="59a7b-290">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs can be collected by using one of the following methods.</span></span>

#### <a name="locally-on-each-node"></a><span data-ttu-id="59a7b-291">Lokalt på varje nod</span><span class="sxs-lookup"><span data-stu-id="59a7b-291">Locally on each node</span></span>

<span data-ttu-id="59a7b-292">Loggarna samlas lokalt på varje klusternod för Service Fabric Service Fabric runtime-versionen om är mindre än `5.6.220.9494`.</span><span class="sxs-lookup"><span data-stu-id="59a7b-292">Logs are collected locally on each Service Fabric cluster node if Service Fabric runtime version is less than `5.6.220.9494`.</span></span> <span data-ttu-id="59a7b-293">Platsen för att komma åt loggarna är \[Service Fabric\_Installation\_enhet\]:\\PatchOrchestrationApplication\\loggar.</span><span class="sxs-lookup"><span data-stu-id="59a7b-293">The location to access the logs is \[Service Fabric\_Installation\_Drive\]:\\PatchOrchestrationApplication\\logs.</span></span>

<span data-ttu-id="59a7b-294">Till exempel om Service Fabric är installerat på enhet D, är sökvägen D:\\PatchOrchestrationApplication\\loggar.</span><span class="sxs-lookup"><span data-stu-id="59a7b-294">For example, if Service Fabric is installed on drive D, the path is D:\\PatchOrchestrationApplication\\logs.</span></span>

#### <a name="central-location"></a><span data-ttu-id="59a7b-295">Central plats</span><span class="sxs-lookup"><span data-stu-id="59a7b-295">Central location</span></span>

<span data-ttu-id="59a7b-296">Om Azure-diagnostik är konfigurerad som en del av nödvändiga steg, är loggar för korrigering orchestration appen tillgängliga i Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="59a7b-296">If Azure Diagnostics is configured as part of prerequisite steps, logs for the patch orchestration app are available in Azure Storage.</span></span>

### <a name="health-reports"></a><span data-ttu-id="59a7b-297">Hälsorapporter</span><span class="sxs-lookup"><span data-stu-id="59a7b-297">Health reports</span></span>

<span data-ttu-id="59a7b-298">Korrigering orchestration appen publiceras även hälsorapporter mot Coordinator-tjänsten eller nod Agent-tjänsten i följande fall:</span><span class="sxs-lookup"><span data-stu-id="59a7b-298">The patch orchestration app also publishes health reports against the Coordinator Service or the Node Agent Service in the following cases:</span></span>

#### <a name="a-windows-update-operation-failed"></a><span data-ttu-id="59a7b-299">En Windows Update-åtgärden misslyckades</span><span class="sxs-lookup"><span data-stu-id="59a7b-299">A Windows Update operation failed</span></span>

<span data-ttu-id="59a7b-300">Om en Windows Update-åtgärden misslyckas på en nod, skapas en hälsorapport mot nod Agent-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59a7b-300">If a Windows Update operation fails on a node, a health report is generated against the Node Agent Service.</span></span> <span data-ttu-id="59a7b-301">Information om hälsorapporten innehålla problematiska nodnamn.</span><span class="sxs-lookup"><span data-stu-id="59a7b-301">Details of the health report contain the problematic node name.</span></span>

<span data-ttu-id="59a7b-302">När uppdatering har slutförts på noden problematiskt, avmarkeras automatiskt i rapporten.</span><span class="sxs-lookup"><span data-stu-id="59a7b-302">After patching is successfully completed on the problematic node, the report is automatically cleared.</span></span>

#### <a name="the-node-agent-ntservice-is-down"></a><span data-ttu-id="59a7b-303">Noden Agent NTService är nere</span><span class="sxs-lookup"><span data-stu-id="59a7b-303">The Node Agent NTService is down</span></span>

<span data-ttu-id="59a7b-304">Om noden Agent NTService är igång på en nod kan genereras en varning nivå hälsorapport mot nod Agent-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59a7b-304">If the Node Agent NTService is down on a node, a warning-level health report is generated against the Node Agent Service.</span></span>

#### <a name="the-repair-manager-service-is-not-enabled"></a><span data-ttu-id="59a7b-305">Reparera manager-tjänsten är inte aktiverad</span><span class="sxs-lookup"><span data-stu-id="59a7b-305">The repair manager service is not enabled</span></span>

<span data-ttu-id="59a7b-306">Om reparationen manager-tjänsten inte kan hittas på klustret, skapas en varningsnivå hälsorapport för Coordinator-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59a7b-306">If the repair manager service is not found on the cluster, a warning-level health report is generated for the Coordinator Service.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="59a7b-307">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="59a7b-307">Frequently asked questions</span></span>

<span data-ttu-id="59a7b-308">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="59a7b-308">Q.</span></span> <span data-ttu-id="59a7b-309">**Varför ser jag min kluster i ett feltillstånd när korrigering orchestration appen körs?**</span><span class="sxs-lookup"><span data-stu-id="59a7b-309">**Why do I see my cluster in an error state when the patch orchestration app is running?**</span></span>

<span data-ttu-id="59a7b-310">A.</span><span class="sxs-lookup"><span data-stu-id="59a7b-310">A.</span></span> <span data-ttu-id="59a7b-311">Under installationen av appen korrigering orchestration inaktiverar eller startar om noder som tillfälligt kan resultera i hälsotillståndet för klustret gå.</span><span class="sxs-lookup"><span data-stu-id="59a7b-311">During the installation process, the patch orchestration app disables or restarts nodes, which can temporarily result in the health of the cluster going down.</span></span>

<span data-ttu-id="59a7b-312">Baserat på principen för programmet, antingen en nod kan gå under en uppdatering åtgärd *eller* en hel uppgraderingsdomän kan gå samtidigt.</span><span class="sxs-lookup"><span data-stu-id="59a7b-312">Based on the policy for the application, either one node can go down during a patching operation *or* an entire upgrade domain can go down simultaneously.</span></span>

<span data-ttu-id="59a7b-313">I slutet av installationen av uppdateringen för Windows noderna reenabled efter omstart.</span><span class="sxs-lookup"><span data-stu-id="59a7b-313">By the end of the Windows Update installation, the nodes are reenabled post restart.</span></span>

<span data-ttu-id="59a7b-314">I följande exempel klustret ledde till ett feltillstånd tillfälligt eftersom två noder har ned och MaxPercentageUnhealthNodes principen har överskridits.</span><span class="sxs-lookup"><span data-stu-id="59a7b-314">In the following example, the cluster went to an error state temporarily because two nodes were down and the MaxPercentageUnhealthNodes policy was violated.</span></span> <span data-ttu-id="59a7b-315">Felet är tillfällig tills åtgärden uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="59a7b-315">The error is temporary until the patching operation is ongoing.</span></span>

![Bild av feltillstånd kluster](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

<span data-ttu-id="59a7b-317">Om problemet kvarstår finns i avsnittet Felsökning.</span><span class="sxs-lookup"><span data-stu-id="59a7b-317">If the issue persists, refer to the Troubleshooting section.</span></span>

<span data-ttu-id="59a7b-318">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="59a7b-318">Q.</span></span> <span data-ttu-id="59a7b-319">**Korrigering av orchestration appar är i varningstillstånd**</span><span class="sxs-lookup"><span data-stu-id="59a7b-319">**Patch orchestration app is in warning state**</span></span>

<span data-ttu-id="59a7b-320">A.</span><span class="sxs-lookup"><span data-stu-id="59a7b-320">A.</span></span> <span data-ttu-id="59a7b-321">Kontrollera om en hälsorapport bokförd i programmet är den grundläggande orsaken.</span><span class="sxs-lookup"><span data-stu-id="59a7b-321">Check to see if a health report posted against the application is the root cause.</span></span> <span data-ttu-id="59a7b-322">Varningen innehåller vanligtvis information om problemet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-322">Usually, the warning contains details of the problem.</span></span> <span data-ttu-id="59a7b-323">Om problemet är tillfälligt, förväntas programmet återskapa automatiskt från det här läget.</span><span class="sxs-lookup"><span data-stu-id="59a7b-323">If the issue is transient, the application is expected to auto-recover from this state.</span></span>

<span data-ttu-id="59a7b-324">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="59a7b-324">Q.</span></span> <span data-ttu-id="59a7b-325">**Vad gör jag om min klustret är i feltillstånd och måste du göra en uppdatering för brådskande operativsystem?**</span><span class="sxs-lookup"><span data-stu-id="59a7b-325">**What can I do if my cluster is unhealthy and I need to do an urgent operating system update?**</span></span>

<span data-ttu-id="59a7b-326">A.</span><span class="sxs-lookup"><span data-stu-id="59a7b-326">A.</span></span> <span data-ttu-id="59a7b-327">Korrigering orchestration appen installeras inte uppdateringar när klustret är i feltillstånd.</span><span class="sxs-lookup"><span data-stu-id="59a7b-327">The patch orchestration app does not install updates while the cluster is unhealthy.</span></span> <span data-ttu-id="59a7b-328">Försök att ta med ditt kluster till ett felfritt tillstånd att avblockera korrigering orchestration app arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-328">Try to bring your cluster to a healthy state to unblock the patch orchestration app workflow.</span></span>

<span data-ttu-id="59a7b-329">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="59a7b-329">Q.</span></span> <span data-ttu-id="59a7b-330">**Varför korrigering genom datorkluster tar så lång tid för att köra?**</span><span class="sxs-lookup"><span data-stu-id="59a7b-330">**Why does patching across clusters take so long to run?**</span></span>

<span data-ttu-id="59a7b-331">A.</span><span class="sxs-lookup"><span data-stu-id="59a7b-331">A.</span></span> <span data-ttu-id="59a7b-332">Den tid som krävs av korrigering orchestration appen beror oftast på följande faktorer:</span><span class="sxs-lookup"><span data-stu-id="59a7b-332">The time needed by the patch orchestration app is mostly dependent on the following factors:</span></span>

- <span data-ttu-id="59a7b-333">Principen för Coordinator-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59a7b-333">The policy of the Coordinator Service.</span></span> 
  - <span data-ttu-id="59a7b-334">Standardprincipen, `NodeWise`, resulterar i korrigering endast en nod i taget.</span><span class="sxs-lookup"><span data-stu-id="59a7b-334">The default policy, `NodeWise`, results in patching only one node at a time.</span></span> <span data-ttu-id="59a7b-335">Särskilt när större kluster rekommenderar vi att du använder den `UpgradeDomainWise` att uppnå snabbare korrigering genom datorkluster.</span><span class="sxs-lookup"><span data-stu-id="59a7b-335">Especially in the case of bigger clusters, we recommend that you use the `UpgradeDomainWise` policy to achieve faster patching across clusters.</span></span>
- <span data-ttu-id="59a7b-336">Antalet uppdateringar som är tillgängliga för hämtning och installation.</span><span class="sxs-lookup"><span data-stu-id="59a7b-336">The number of updates available for download and installation.</span></span> 
- <span data-ttu-id="59a7b-337">Genomsnittlig tid det behövs för att ladda ned och installera en uppdatering som får inte överstiga några timmar.</span><span class="sxs-lookup"><span data-stu-id="59a7b-337">The average time needed to download and install an update, which should not exceed a couple of hours.</span></span>
- <span data-ttu-id="59a7b-338">Prestanda för den virtuella dator och nätverket bandbredden.</span><span class="sxs-lookup"><span data-stu-id="59a7b-338">The performance of the VM and network bandwidth.</span></span>

<span data-ttu-id="59a7b-339">FRÅGOR.</span><span class="sxs-lookup"><span data-stu-id="59a7b-339">Q.</span></span> <span data-ttu-id="59a7b-340">**Varför ser vissa uppdateringar i Windows Update resultaten via REST-api, men inte under Windows Update-historiken på datorn?**</span><span class="sxs-lookup"><span data-stu-id="59a7b-340">**Why do I see some updates in Windows Update results obtained via REST api's, but not under the Windows Update history on machine?**</span></span>

<span data-ttu-id="59a7b-341">A.</span><span class="sxs-lookup"><span data-stu-id="59a7b-341">A.</span></span> <span data-ttu-id="59a7b-342">Vissa produktuppdateringar måste checkas in deras respektive uppdatering/korrigering historik.</span><span class="sxs-lookup"><span data-stu-id="59a7b-342">Some product updates need to be checked in their respective update/patch history.</span></span> <span data-ttu-id="59a7b-343">Exempel: Uppdateringar för Windows Defender inte visas i Windows Update-historiken på Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="59a7b-343">Eg: Windows Defender updates do not show up in Windows Update history on Windows Server 2016.</span></span>

## <a name="disclaimers"></a><span data-ttu-id="59a7b-344">Ansvarsfriskrivningar</span><span class="sxs-lookup"><span data-stu-id="59a7b-344">Disclaimers</span></span>

- <span data-ttu-id="59a7b-345">Korrigering orchestration appen accepterar slutanvändarens licensavtal för Windows Update för användarens räkning.</span><span class="sxs-lookup"><span data-stu-id="59a7b-345">The patch orchestration app accepts the End-User License Agreement of Windows Update on behalf of the user.</span></span> <span data-ttu-id="59a7b-346">Alternativt kan kan inställningen inaktiveras i konfigurationen för programmet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-346">Optionally, the setting can be turned off in the configuration of the application.</span></span>

- <span data-ttu-id="59a7b-347">Korrigering orchestration appen samlar in telemetri om du vill spåra användning och prestanda.</span><span class="sxs-lookup"><span data-stu-id="59a7b-347">The patch orchestration app collects telemetry to track usage and performance.</span></span> <span data-ttu-id="59a7b-348">Programmets telemetri följer inställningen för Service Fabric-körningsmiljön telemetri inställningen (som är aktiverad som standard).</span><span class="sxs-lookup"><span data-stu-id="59a7b-348">The application’s telemetry follows the setting of the Service Fabric runtime’s telemetry setting (which is on by default).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="59a7b-349">Felsökning</span><span class="sxs-lookup"><span data-stu-id="59a7b-349">Troubleshooting</span></span>

### <a name="a-node-is-not-coming-back-to-up-state"></a><span data-ttu-id="59a7b-350">En nod som inte kommer tillbaka på upp tillstånd</span><span class="sxs-lookup"><span data-stu-id="59a7b-350">A node is not coming back to up state</span></span>

<span data-ttu-id="59a7b-351">**Noden kan ha fastnat i tillståndet inaktivera eftersom**:</span><span class="sxs-lookup"><span data-stu-id="59a7b-351">**The node might be stuck in a disabling state because**:</span></span>

<span data-ttu-id="59a7b-352">En säkerhets-kontroll väntar.</span><span class="sxs-lookup"><span data-stu-id="59a7b-352">A safety check is pending.</span></span> <span data-ttu-id="59a7b-353">Du kan åtgärda detta genom att se till att tillräckligt många noder är tillgängliga i felfritt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="59a7b-353">To remedy this situation, ensure that enough nodes are available in a healthy state.</span></span>

<span data-ttu-id="59a7b-354">**Noden kan ha fastnat i ett inaktiverat tillstånd eftersom**:</span><span class="sxs-lookup"><span data-stu-id="59a7b-354">**The node might be stuck in a disabled state because**:</span></span>

- <span data-ttu-id="59a7b-355">Noden har inaktiverats manuellt.</span><span class="sxs-lookup"><span data-stu-id="59a7b-355">The node was disabled manually.</span></span>
- <span data-ttu-id="59a7b-356">Noden har inaktiverats på grund av ett pågående Azure-infrastrukturen jobb.</span><span class="sxs-lookup"><span data-stu-id="59a7b-356">The node was disabled due to an ongoing Azure infrastructure job.</span></span>
- <span data-ttu-id="59a7b-357">Noden har inaktiverats tillfälligt av korrigering orchestration appen att uppdatera noden.</span><span class="sxs-lookup"><span data-stu-id="59a7b-357">The node was disabled temporarily by the patch orchestration app to patch the node.</span></span>

<span data-ttu-id="59a7b-358">**Noden kan ha fastnat i ned-läge eftersom**:</span><span class="sxs-lookup"><span data-stu-id="59a7b-358">**The node might be stuck in a down state because**:</span></span>

- <span data-ttu-id="59a7b-359">Noden placerades i tillståndet nedåt manuellt.</span><span class="sxs-lookup"><span data-stu-id="59a7b-359">The node was put in a down state manually.</span></span>
- <span data-ttu-id="59a7b-360">Noden genomgår en omstart (som kan utlösas av korrigering orchestration-app).</span><span class="sxs-lookup"><span data-stu-id="59a7b-360">The node is undergoing a restart (which might be triggered by the patch orchestration app).</span></span>
- <span data-ttu-id="59a7b-361">Noden inte är på grund av en felaktig virtuell dator eller datorn eller nätverket anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="59a7b-361">The node is down due to a faulty VM or machine or network connectivity issues.</span></span>

### <a name="updates-were-skipped-on-some-nodes"></a><span data-ttu-id="59a7b-362">Uppdateringar hoppades över på vissa noder</span><span class="sxs-lookup"><span data-stu-id="59a7b-362">Updates were skipped on some nodes</span></span>

<span data-ttu-id="59a7b-363">Korrigering orchestration-appen försöker installera en Windows update enligt omplanering principen.</span><span class="sxs-lookup"><span data-stu-id="59a7b-363">The patch orchestration app tries to install a Windows update according to the rescheduling policy.</span></span> <span data-ttu-id="59a7b-364">Tjänsten försöker återställa noden och hoppa över uppdateringen enligt principen för program.</span><span class="sxs-lookup"><span data-stu-id="59a7b-364">The service tries to recover the node and skip the update according to the application policy.</span></span>

<span data-ttu-id="59a7b-365">I sådana fall genereras en varning nivå hälsorapport mot nod Agent-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59a7b-365">In such a case, a warning-level health report is generated against the Node Agent Service.</span></span> <span data-ttu-id="59a7b-366">Resultat för Windows Update innehåller också möjliga orsaker till felet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-366">The result for Windows Update also contains the possible reason for the failure.</span></span>

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a><span data-ttu-id="59a7b-367">Hälsotillståndet för klustret går till när uppdateringen har installerats</span><span class="sxs-lookup"><span data-stu-id="59a7b-367">The health of the cluster goes to error while the update installs</span></span>

<span data-ttu-id="59a7b-368">En felaktig Windows update kan skada hälsotillståndet för ett program eller ett kluster på en viss nod eller en domän.</span><span class="sxs-lookup"><span data-stu-id="59a7b-368">A faulty Windows update can bring down the health of an application or cluster on a particular node or upgrade domain.</span></span> <span data-ttu-id="59a7b-369">Korrigering orchestration app upphör alla efterföljande Windows Update-åtgärden tills klustret är felfri igen.</span><span class="sxs-lookup"><span data-stu-id="59a7b-369">The patch orchestration app discontinues any subsequent Windows Update operation until the cluster is healthy again.</span></span>

<span data-ttu-id="59a7b-370">En administratör måste ingripa och fastställa varför programmet eller kluster blev inte hälsosamt eftersom Windows Update.</span><span class="sxs-lookup"><span data-stu-id="59a7b-370">An administrator must intervene and determine why the application or cluster became unhealthy due to Windows Update.</span></span>

## <a name="release-notes-"></a><span data-ttu-id="59a7b-371">Viktig information:</span><span class="sxs-lookup"><span data-stu-id="59a7b-371">Release Notes :</span></span>

### <a name="version-110"></a><span data-ttu-id="59a7b-372">Version 1.1.0</span><span class="sxs-lookup"><span data-stu-id="59a7b-372">Version 1.1.0</span></span>
- <span data-ttu-id="59a7b-373">Offentliga utgåvan</span><span class="sxs-lookup"><span data-stu-id="59a7b-373">Public release</span></span>

### <a name="version-111"></a><span data-ttu-id="59a7b-374">Version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="59a7b-374">Version 1.1.1</span></span>
- <span data-ttu-id="59a7b-375">Fast ett programfel i SetupEntryPoint av NodeAgentService som förhindrade installationen av NodeAgentNTService.</span><span class="sxs-lookup"><span data-stu-id="59a7b-375">Fixed a bug in SetupEntryPoint of NodeAgentService that prevented installation of NodeAgentNTService.</span></span>

### <a name="version-120-latest"></a><span data-ttu-id="59a7b-376">Version 1.2.0 (senaste)</span><span class="sxs-lookup"><span data-stu-id="59a7b-376">Version 1.2.0 (Latest)</span></span>

- <span data-ttu-id="59a7b-377">Felkorrigeringar runt system starta om arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="59a7b-377">Bug fixes around system restart workflow.</span></span>
- <span data-ttu-id="59a7b-378">Buggfix för att skapa RM uppgifter på grund av vilka hälsotillstånd Kontrollera under förberedelserna reparera uppgifter inte händer som förväntat.</span><span class="sxs-lookup"><span data-stu-id="59a7b-378">Bug fix in creation of RM tasks due to which health check during preparing repair tasks wasn't happening as expected.</span></span>
- <span data-ttu-id="59a7b-379">Ändra startmetoden för windows-tjänst POANodeSvc från automatisk till fördröjd auto.</span><span class="sxs-lookup"><span data-stu-id="59a7b-379">Changed the startup mode for windows service POANodeSvc from auto to delayed-auto.</span></span>
