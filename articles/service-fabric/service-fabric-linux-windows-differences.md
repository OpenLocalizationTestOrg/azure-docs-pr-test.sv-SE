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
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a>Skillnader mellan Service Fabric i Linux (förhandsversion) och Windows (allmänt tillgänglig)

Eftersom Service Fabric i Linux är en förhandsversion är det vissa funktioner som bara finns i Windows, inte ännu i Linux. Slutligen hello funktionsuppsättningar kommer att vara paritet när Service Fabric på Linux blir allmänt tillgänglig. Den här funktionsskillnaden kommer att krympa i kommande versioner. hello följande skillnader hello senaste tillgängliga versioner (det vill säga mellan version 5.6 på Windows och på Linux-version 5.5): 

* Tillförlitliga samlingar (och tillförlitliga tillståndskänsliga tjänster) 
* ReverseProxy 
* Fristående installationsprogram 
* XML-schemavalidering för manifestfiler 
* Omdirigering av konsol 
* hello fel Analysis Service (FAS)
* Docker-skrivning, volym och loggdrivrutiner för behållare 
* Resursstyrning för behållare och tjänster 
* DNS-tjänst
* Stöd för Azure Active Directory
* CLI-kommandon som motsvarar vissa Powershell-kommandon 
* Endast en del av Powershell-kommandon kan köras mot en Linux-kluster (som visas i nästa avsnitt om hello).

>[!NOTE]
>Omdirigering av konsol stöds inte i produktionskluster (gäller även Windows).

Utvecklingsverktygen skiljer sig också åt mellan Windows och Linux. VisualStudio, Powershell, VSTS och ETW används för Windows medan Yeoman, Eclipse, Jenkins och LTTng används med Linux.

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>PowerShell-cmdletar som inte fungerar mot ett Linux Service Fabric-kluster

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Cmd
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Nästa steg
* [Förbereda utvecklingsmiljön i Linux](service-fabric-get-started-linux.md)
* [Förbereda utvecklingsmiljön i OSX](service-fabric-get-started-mac.md)
* [Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse](service-fabric-get-started-eclipse.md)
* [Skapa ditt första CSharp-program i Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Använd hello Service Fabric CLI toomanage dina program](service-fabric-application-lifecycle-sfctl.md)
