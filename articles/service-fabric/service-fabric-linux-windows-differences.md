---
title: aaaAzure Service Fabric skillnaderna mellan Linux och Windows | Microsoft Docs
description: "Skillnader mellan hello Azure Service Fabric Preview på Linux- och Azure Service Fabric i Windows."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 7a16a440dfc8d9006e274f46951be1562e6f10d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a><span data-ttu-id="ad103-103">Skillnader mellan Service Fabric i Linux (förhandsversion) och Windows (allmänt tillgänglig)</span><span class="sxs-lookup"><span data-stu-id="ad103-103">Differences between Service Fabric on Linux (preview) and Windows (generally available)</span></span>

<span data-ttu-id="ad103-104">Eftersom Service Fabric i Linux är en förhandsversion är det vissa funktioner som bara finns i Windows, inte ännu i Linux.</span><span class="sxs-lookup"><span data-stu-id="ad103-104">Since Service Fabric on Linux is a preview, there are some features that are supported on Windows, but not yet on Linux.</span></span> <span data-ttu-id="ad103-105">Slutligen hello funktionsuppsättningar kommer att vara paritet när Service Fabric på Linux blir allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="ad103-105">Eventually, hello feature sets will be at parity when Service Fabric on Linux becomes generally available.</span></span> <span data-ttu-id="ad103-106">Den här funktionsskillnaden kommer att krympa i kommande versioner.</span><span class="sxs-lookup"><span data-stu-id="ad103-106">With upcoming releases, this feature gap will shrink.</span></span> <span data-ttu-id="ad103-107">hello följande skillnader hello senaste tillgängliga versioner (det vill säga mellan version 5.6 på Windows och på Linux-version 5.5):</span><span class="sxs-lookup"><span data-stu-id="ad103-107">hello following differences exist between hello latest available releases (that is, between version 5.6 on Windows and version 5.5 on Linux):</span></span> 

* <span data-ttu-id="ad103-108">Tillförlitliga samlingar (och tillförlitliga tillståndskänsliga tjänster)</span><span class="sxs-lookup"><span data-stu-id="ad103-108">Reliable Collections (and Reliable Stateful Services)</span></span> 
* <span data-ttu-id="ad103-109">ReverseProxy</span><span class="sxs-lookup"><span data-stu-id="ad103-109">ReverseProxy</span></span> 
* <span data-ttu-id="ad103-110">Fristående installationsprogram</span><span class="sxs-lookup"><span data-stu-id="ad103-110">Standalone installer</span></span> 
* <span data-ttu-id="ad103-111">XML-schemavalidering för manifestfiler</span><span class="sxs-lookup"><span data-stu-id="ad103-111">XML schema validation for manifest files</span></span> 
* <span data-ttu-id="ad103-112">Omdirigering av konsol</span><span class="sxs-lookup"><span data-stu-id="ad103-112">Console redirection</span></span> 
* <span data-ttu-id="ad103-113">hello fel Analysis Service (FAS)</span><span class="sxs-lookup"><span data-stu-id="ad103-113">hello Fault Analysis Service (FAS)</span></span>
* <span data-ttu-id="ad103-114">Docker-skrivning, volym och loggdrivrutiner för behållare</span><span class="sxs-lookup"><span data-stu-id="ad103-114">Docker compose and volume and logging drivers for containers</span></span> 
* <span data-ttu-id="ad103-115">Resursstyrning för behållare och tjänster</span><span class="sxs-lookup"><span data-stu-id="ad103-115">Resource governance for containers and services</span></span> 
* <span data-ttu-id="ad103-116">DNS-tjänst</span><span class="sxs-lookup"><span data-stu-id="ad103-116">DNS service</span></span>
* <span data-ttu-id="ad103-117">Stöd för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ad103-117">Azure Active Directory support</span></span>
* <span data-ttu-id="ad103-118">CLI-kommandon som motsvarar vissa Powershell-kommandon</span><span class="sxs-lookup"><span data-stu-id="ad103-118">CLI command equivalents of certain Powershell commands</span></span> 
* <span data-ttu-id="ad103-119">Endast en del av Powershell-kommandon kan köras mot en Linux-kluster (som visas i nästa avsnitt om hello).</span><span class="sxs-lookup"><span data-stu-id="ad103-119">Only a subset of Powershell commands can be run against a Linux cluster (as expanded in hello next section).</span></span>

>[!NOTE]
><span data-ttu-id="ad103-120">Omdirigering av konsol stöds inte i produktionskluster (gäller även Windows).</span><span class="sxs-lookup"><span data-stu-id="ad103-120">Console redirection is not supported in production clusters, even on Windows.</span></span>

<span data-ttu-id="ad103-121">Utvecklingsverktygen skiljer sig också åt mellan Windows och Linux.</span><span class="sxs-lookup"><span data-stu-id="ad103-121">Development tooling is also different between Windows and Linux.</span></span> <span data-ttu-id="ad103-122">VisualStudio, Powershell, VSTS och ETW används för Windows medan Yeoman, Eclipse, Jenkins och LTTng används med Linux.</span><span class="sxs-lookup"><span data-stu-id="ad103-122">VisualStudio, Powershell, VSTS, and ETW are used on Windows while Yeoman, Eclipse, Jenkins, and LTTng are used on Linux.</span></span>

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a><span data-ttu-id="ad103-123">PowerShell-cmdletar som inte fungerar mot ett Linux Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="ad103-123">Powershell cmdlets that do not work against a Linux Service Fabric cluster</span></span>

* <span data-ttu-id="ad103-124">Invoke-ServiceFabricChaosTestScenario</span><span class="sxs-lookup"><span data-stu-id="ad103-124">Invoke-ServiceFabricChaosTestScenario</span></span>
* <span data-ttu-id="ad103-125">Invoke-ServiceFabricFailoverTestScenario</span><span class="sxs-lookup"><span data-stu-id="ad103-125">Invoke-ServiceFabricFailoverTestScenario</span></span>
* <span data-ttu-id="ad103-126">Invoke-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="ad103-126">Invoke-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="ad103-127">Invoke-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="ad103-127">Invoke-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="ad103-128">Restart-ServiceFabricPartition</span><span class="sxs-lookup"><span data-stu-id="ad103-128">Restart-ServiceFabricPartition</span></span>
* <span data-ttu-id="ad103-129">Start-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="ad103-129">Start-ServiceFabricNode</span></span>
* <span data-ttu-id="ad103-130">Stop-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="ad103-130">Stop-ServiceFabricNode</span></span>
* <span data-ttu-id="ad103-131">Get-ServiceFabricImageStoreContent</span><span class="sxs-lookup"><span data-stu-id="ad103-131">Get-ServiceFabricImageStoreContent</span></span>
* <span data-ttu-id="ad103-132">Get-ServiceFabricChaosReport</span><span class="sxs-lookup"><span data-stu-id="ad103-132">Get-ServiceFabricChaosReport</span></span>
* <span data-ttu-id="ad103-133">Get-ServiceFabricNodeTransitionProgress</span><span class="sxs-lookup"><span data-stu-id="ad103-133">Get-ServiceFabricNodeTransitionProgress</span></span>
* <span data-ttu-id="ad103-134">Get-ServiceFabricPartitionDataLossProgress</span><span class="sxs-lookup"><span data-stu-id="ad103-134">Get-ServiceFabricPartitionDataLossProgress</span></span>
* <span data-ttu-id="ad103-135">Get-ServiceFabricPartitionQuorumLossProgress</span><span class="sxs-lookup"><span data-stu-id="ad103-135">Get-ServiceFabricPartitionQuorumLossProgress</span></span>
* <span data-ttu-id="ad103-136">Get-ServiceFabricPartitionRestartProgress</span><span class="sxs-lookup"><span data-stu-id="ad103-136">Get-ServiceFabricPartitionRestartProgress</span></span>
* <span data-ttu-id="ad103-137">Get-ServiceFabricTestCommandStatusList</span><span class="sxs-lookup"><span data-stu-id="ad103-137">Get-ServiceFabricTestCommandStatusList</span></span>
* <span data-ttu-id="ad103-138">Remove-ServiceFabricTestState</span><span class="sxs-lookup"><span data-stu-id="ad103-138">Remove-ServiceFabricTestState</span></span>
* <span data-ttu-id="ad103-139">Start-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="ad103-139">Start-ServiceFabricChaos</span></span>
* <span data-ttu-id="ad103-140">Start-ServiceFabricNodeTransition</span><span class="sxs-lookup"><span data-stu-id="ad103-140">Start-ServiceFabricNodeTransition</span></span>
* <span data-ttu-id="ad103-141">Start-ServiceFabricPartitionDataLoss</span><span class="sxs-lookup"><span data-stu-id="ad103-141">Start-ServiceFabricPartitionDataLoss</span></span>
* <span data-ttu-id="ad103-142">Start-ServiceFabricPartitionQuorumLoss</span><span class="sxs-lookup"><span data-stu-id="ad103-142">Start-ServiceFabricPartitionQuorumLoss</span></span>
* <span data-ttu-id="ad103-143">Start-ServiceFabricPartitionRestart</span><span class="sxs-lookup"><span data-stu-id="ad103-143">Start-ServiceFabricPartitionRestart</span></span>
* <span data-ttu-id="ad103-144">Stop-ServiceFabricChaos</span><span class="sxs-lookup"><span data-stu-id="ad103-144">Stop-ServiceFabricChaos</span></span>
* <span data-ttu-id="ad103-145">Stop-ServiceFabricTestCommand</span><span class="sxs-lookup"><span data-stu-id="ad103-145">Stop-ServiceFabricTestCommand</span></span>
* <span data-ttu-id="ad103-146">Cmd</span><span class="sxs-lookup"><span data-stu-id="ad103-146">Cmd</span></span>
* <span data-ttu-id="ad103-147">Get-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="ad103-147">Get-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="ad103-148">Get-ServiceFabricClusterConfiguration</span><span class="sxs-lookup"><span data-stu-id="ad103-148">Get-ServiceFabricClusterConfiguration</span></span>
* <span data-ttu-id="ad103-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span><span class="sxs-lookup"><span data-stu-id="ad103-149">Get-ServiceFabricClusterConfigurationUpgradeStatus</span></span>
* <span data-ttu-id="ad103-150">Get-ServiceFabricPackageDebugParameters</span><span class="sxs-lookup"><span data-stu-id="ad103-150">Get-ServiceFabricPackageDebugParameters</span></span>
* <span data-ttu-id="ad103-151">New-ServiceFabricPackageDebugParameter</span><span class="sxs-lookup"><span data-stu-id="ad103-151">New-ServiceFabricPackageDebugParameter</span></span>
* <span data-ttu-id="ad103-152">New-ServiceFabricPackageSharingPolicy</span><span class="sxs-lookup"><span data-stu-id="ad103-152">New-ServiceFabricPackageSharingPolicy</span></span>
* <span data-ttu-id="ad103-153">Add-ServiceFabricNode</span><span class="sxs-lookup"><span data-stu-id="ad103-153">Add-ServiceFabricNode</span></span>
* <span data-ttu-id="ad103-154">Copy-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="ad103-154">Copy-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="ad103-155">Get-ServiceFabricRuntimeSupportedVersion</span><span class="sxs-lookup"><span data-stu-id="ad103-155">Get-ServiceFabricRuntimeSupportedVersion</span></span>
* <span data-ttu-id="ad103-156">Get-ServiceFabricRuntimeUpgradeVersion</span><span class="sxs-lookup"><span data-stu-id="ad103-156">Get-ServiceFabricRuntimeUpgradeVersion</span></span>
* <span data-ttu-id="ad103-157">New-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="ad103-157">New-ServiceFabricCluster</span></span>
* <span data-ttu-id="ad103-158">New-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="ad103-158">New-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="ad103-159">Remove-ServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="ad103-159">Remove-ServiceFabricCluster</span></span>
* <span data-ttu-id="ad103-160">Remove-ServiceFabricClusterPackage</span><span class="sxs-lookup"><span data-stu-id="ad103-160">Remove-ServiceFabricClusterPackage</span></span>
* <span data-ttu-id="ad103-161">Remove-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="ad103-161">Remove-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="ad103-162">Test-ServiceFabricClusterManifest</span><span class="sxs-lookup"><span data-stu-id="ad103-162">Test-ServiceFabricClusterManifest</span></span>
* <span data-ttu-id="ad103-163">Test-ServiceFabricConfiguration</span><span class="sxs-lookup"><span data-stu-id="ad103-163">Test-ServiceFabricConfiguration</span></span>
* <span data-ttu-id="ad103-164">Update-ServiceFabricNodeConfiguration</span><span class="sxs-lookup"><span data-stu-id="ad103-164">Update-ServiceFabricNodeConfiguration</span></span>
* <span data-ttu-id="ad103-165">Approve-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="ad103-165">Approve-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="ad103-166">Complete-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="ad103-166">Complete-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="ad103-167">Get-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="ad103-167">Get-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="ad103-168">Invoke-ServiceFabricDecryptText</span><span class="sxs-lookup"><span data-stu-id="ad103-168">Invoke-ServiceFabricDecryptText</span></span>
* <span data-ttu-id="ad103-169">Invoke-ServiceFabricEncryptSecret</span><span class="sxs-lookup"><span data-stu-id="ad103-169">Invoke-ServiceFabricEncryptSecret</span></span>
* <span data-ttu-id="ad103-170">Invoke-ServiceFabricEncryptText</span><span class="sxs-lookup"><span data-stu-id="ad103-170">Invoke-ServiceFabricEncryptText</span></span>
* <span data-ttu-id="ad103-171">Invoke-ServiceFabricInfrastructureCommand</span><span class="sxs-lookup"><span data-stu-id="ad103-171">Invoke-ServiceFabricInfrastructureCommand</span></span>
* <span data-ttu-id="ad103-172">Invoke-ServiceFabricInfrastructureQuery</span><span class="sxs-lookup"><span data-stu-id="ad103-172">Invoke-ServiceFabricInfrastructureQuery</span></span>
* <span data-ttu-id="ad103-173">Remove-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="ad103-173">Remove-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="ad103-174">Start-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="ad103-174">Start-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="ad103-175">Stop-ServiceFabricRepairTask</span><span class="sxs-lookup"><span data-stu-id="ad103-175">Stop-ServiceFabricRepairTask</span></span>
* <span data-ttu-id="ad103-176">Update-ServiceFabricRepairTaskHealthPolicy</span><span class="sxs-lookup"><span data-stu-id="ad103-176">Update-ServiceFabricRepairTaskHealthPolicy</span></span>



## <a name="next-steps"></a><span data-ttu-id="ad103-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ad103-177">Next steps</span></span>
* [<span data-ttu-id="ad103-178">Förbereda utvecklingsmiljön i Linux</span><span class="sxs-lookup"><span data-stu-id="ad103-178">Prepare your development environment on Linux</span></span>](service-fabric-get-started-linux.md)
* [<span data-ttu-id="ad103-179">Förbereda utvecklingsmiljön i OSX</span><span class="sxs-lookup"><span data-stu-id="ad103-179">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="ad103-180">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman</span><span class="sxs-lookup"><span data-stu-id="ad103-180">Create and deploy your first Service Fabric Java application on Linux using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="ad103-181">Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse</span><span class="sxs-lookup"><span data-stu-id="ad103-181">Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="ad103-182">Skapa ditt första CSharp-program i Linux</span><span class="sxs-lookup"><span data-stu-id="ad103-182">Create your first CSharp application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
* [<span data-ttu-id="ad103-183">Använd hello Service Fabric CLI toomanage dina program</span><span class="sxs-lookup"><span data-stu-id="ad103-183">Use hello Service Fabric CLI toomanage your applications</span></span>](service-fabric-application-lifecycle-sfctl.md)
