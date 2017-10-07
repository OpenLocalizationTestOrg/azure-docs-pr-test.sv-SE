---
title: "Säkerhet för Service Fabric-kluster: klienten roller | Microsoft Docs"
description: "Den här artikeln beskriver hello två klienten roller och hello behörigheter anges toohello roller."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: coreysa
editor: 
ms.assetid: 7bc808d9-3609-46a1-ac12-b4f53bff98dd
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 4a4a9f93e91ea816005b730bebbcb317f8bab255
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-for-service-fabric-clients"></a><span data-ttu-id="3d6d3-103">Rollbaserad åtkomstkontroll för Service Fabric-klienter</span><span class="sxs-lookup"><span data-stu-id="3d6d3-103">Role-based access control for Service Fabric clients</span></span>
<span data-ttu-id="3d6d3-104">Azure Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna tooa Service Fabric-kluster: administratörs- och.</span><span class="sxs-lookup"><span data-stu-id="3d6d3-104">Azure Service Fabric supports two different access control types for clients that are connected tooa Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="3d6d3-105">Åtkomstkontroll kan Hej administratör toolimit åtkomst toocertain klustret klusteråtgärder för olika grupper av användare, göra hello klustret säkrare.</span><span class="sxs-lookup"><span data-stu-id="3d6d3-105">Access control allows hello cluster administrator toolimit access toocertain cluster operations for different groups of users, making hello cluster more secure.</span></span>  

<span data-ttu-id="3d6d3-106">**Administratörer** har fullständig åtkomst toomanagement funktioner (inklusive funktioner för läsning och skrivning).</span><span class="sxs-lookup"><span data-stu-id="3d6d3-106">**Administrators** have full access toomanagement capabilities (including read/write capabilities).</span></span> <span data-ttu-id="3d6d3-107">Som standard **användare** bara har läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.</span><span class="sxs-lookup"><span data-stu-id="3d6d3-107">By default, **users** only have read access toomanagement capabilities (for example, query capabilities), and hello ability tooresolve applications and services.</span></span>

<span data-ttu-id="3d6d3-108">Du kan ange hello två klienten roller (administratör och klient) när hello klustret har skapats genom att tillhandahålla olika certifikat för varje.</span><span class="sxs-lookup"><span data-stu-id="3d6d3-108">You specify hello two client roles (administrator and client) at hello time of cluster creation by providing separate certificates for each.</span></span> <span data-ttu-id="3d6d3-109">Se [Service Fabric-Klustersäkerhet](service-fabric-cluster-security.md) mer information om hur du konfigurerar en säker Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="3d6d3-109">See [Service Fabric cluster security](service-fabric-cluster-security.md) for details on setting up a secure Service Fabric cluster.</span></span>

## <a name="default-access-control-settings"></a><span data-ttu-id="3d6d3-110">Inställningar för åtkomstkontroll som standard</span><span class="sxs-lookup"><span data-stu-id="3d6d3-110">Default access control settings</span></span>
<span data-ttu-id="3d6d3-111">Hej administratör åtkomst kontrolltypen har fullständig åtkomst tooall hello FabricClient APIs.</span><span class="sxs-lookup"><span data-stu-id="3d6d3-111">hello administrator access control type has full access tooall hello FabricClient APIs.</span></span> <span data-ttu-id="3d6d3-112">Den kan utföra alla läsning och skrivning på hello Service Fabric-klustret, inklusive hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="3d6d3-112">It can perform any reads and writes on hello Service Fabric cluster, including hello following operations:</span></span>

### <a name="application-and-service-operations"></a><span data-ttu-id="3d6d3-113">Program- och tjänståtgärder</span><span class="sxs-lookup"><span data-stu-id="3d6d3-113">Application and service operations</span></span>
* <span data-ttu-id="3d6d3-114">**CreateService**: skapar en tjänst</span><span class="sxs-lookup"><span data-stu-id="3d6d3-114">**CreateService**: service creation</span></span>                             
* <span data-ttu-id="3d6d3-115">**CreateServiceFromTemplate**: tjänsten skapa från mall</span><span class="sxs-lookup"><span data-stu-id="3d6d3-115">**CreateServiceFromTemplate**: service creation from template</span></span>                             
* <span data-ttu-id="3d6d3-116">**UpdateService**: hantera uppdateringar</span><span class="sxs-lookup"><span data-stu-id="3d6d3-116">**UpdateService**: service updates</span></span>                             
* <span data-ttu-id="3d6d3-117">**DeleteService**: Tjänsten tas bort</span><span class="sxs-lookup"><span data-stu-id="3d6d3-117">**DeleteService**: service deletion</span></span>                             
* <span data-ttu-id="3d6d3-118">**ProvisionApplicationType**: programmet typen etablering</span><span class="sxs-lookup"><span data-stu-id="3d6d3-118">**ProvisionApplicationType**: application type provisioning</span></span>                             
* <span data-ttu-id="3d6d3-119">**CreateApplication**: skapa program</span><span class="sxs-lookup"><span data-stu-id="3d6d3-119">**CreateApplication**: application creation</span></span>                               
* <span data-ttu-id="3d6d3-120">**DeleteApplication**: ta bort program</span><span class="sxs-lookup"><span data-stu-id="3d6d3-120">**DeleteApplication**: application deletion</span></span>                             
* <span data-ttu-id="3d6d3-121">**UpgradeApplication**: startar eller störa programuppgraderingar</span><span class="sxs-lookup"><span data-stu-id="3d6d3-121">**UpgradeApplication**: starting or interrupting application upgrades</span></span>                             
* <span data-ttu-id="3d6d3-122">**UnprovisionApplicationType**: programmet typen avetablering</span><span class="sxs-lookup"><span data-stu-id="3d6d3-122">**UnprovisionApplicationType**: application type unprovisioning</span></span>                             
* <span data-ttu-id="3d6d3-123">**MoveNextUpgradeDomain**: återupptar programuppgraderingar med en explicit uppdateringsdomän</span><span class="sxs-lookup"><span data-stu-id="3d6d3-123">**MoveNextUpgradeDomain**: resuming application upgrades with an explicit update domain</span></span>                             
* <span data-ttu-id="3d6d3-124">**ReportUpgradeHealth**: återupptar programuppgraderingar med hello aktuella Uppgraderingsförlopp</span><span class="sxs-lookup"><span data-stu-id="3d6d3-124">**ReportUpgradeHealth**: resuming application upgrades with hello current upgrade progress</span></span>                             
* <span data-ttu-id="3d6d3-125">**ReportHealth**: reporting hälsa</span><span class="sxs-lookup"><span data-stu-id="3d6d3-125">**ReportHealth**: reporting health</span></span>                             
* <span data-ttu-id="3d6d3-126">**PredeployPackageToNode**: hjälp av noggrann API</span><span class="sxs-lookup"><span data-stu-id="3d6d3-126">**PredeployPackageToNode**: predeployment API</span></span>                            
* <span data-ttu-id="3d6d3-127">**CodePackageControl**: starta om koden paket</span><span class="sxs-lookup"><span data-stu-id="3d6d3-127">**CodePackageControl**: restarting code packages</span></span>                             
* <span data-ttu-id="3d6d3-128">**RecoverPartition**: återställning av en partition</span><span class="sxs-lookup"><span data-stu-id="3d6d3-128">**RecoverPartition**: recovering a partition</span></span>                             
* <span data-ttu-id="3d6d3-129">**RecoverPartitions**: återställning av partitioner</span><span class="sxs-lookup"><span data-stu-id="3d6d3-129">**RecoverPartitions**: recovering partitions</span></span>                             
* <span data-ttu-id="3d6d3-130">**RecoverServicePartitions**: återställa tjänsten partitioner</span><span class="sxs-lookup"><span data-stu-id="3d6d3-130">**RecoverServicePartitions**: recovering service partitions</span></span>                             
* <span data-ttu-id="3d6d3-131">**RecoverSystemPartitions**: återställa tjänsten systempartitionerna</span><span class="sxs-lookup"><span data-stu-id="3d6d3-131">**RecoverSystemPartitions**: recovering system service partitions</span></span>                             

### <a name="cluster-operations"></a><span data-ttu-id="3d6d3-132">Klusteråtgärder</span><span class="sxs-lookup"><span data-stu-id="3d6d3-132">Cluster operations</span></span>
* <span data-ttu-id="3d6d3-133">**ProvisionFabric**: MSI och/eller klusternamnresursen manifest etablering</span><span class="sxs-lookup"><span data-stu-id="3d6d3-133">**ProvisionFabric**: MSI and/or cluster manifest provisioning</span></span>                             
* <span data-ttu-id="3d6d3-134">**UpgradeFabric**: starta klusteruppgradering</span><span class="sxs-lookup"><span data-stu-id="3d6d3-134">**UpgradeFabric**: starting cluster upgrades</span></span>                             
* <span data-ttu-id="3d6d3-135">**UnprovisionFabric**: MSI och/eller klusternamnresursen manifest avetablering</span><span class="sxs-lookup"><span data-stu-id="3d6d3-135">**UnprovisionFabric**: MSI and/or cluster manifest unprovisioning</span></span>                         
* <span data-ttu-id="3d6d3-136">**MoveNextFabricUpgradeDomain**: återupptar klusteruppgradering med en explicit uppdateringsdomän</span><span class="sxs-lookup"><span data-stu-id="3d6d3-136">**MoveNextFabricUpgradeDomain**: resuming cluster upgrades with an explicit update domain</span></span>                             
* <span data-ttu-id="3d6d3-137">**ReportFabricUpgradeHealth**: återupptar klusteruppgradering med hello aktuella Uppgraderingsförlopp</span><span class="sxs-lookup"><span data-stu-id="3d6d3-137">**ReportFabricUpgradeHealth**: resuming cluster upgrades with hello current upgrade progress</span></span>                             
* <span data-ttu-id="3d6d3-138">**StartInfrastructureTask**: från infrastrukturen</span><span class="sxs-lookup"><span data-stu-id="3d6d3-138">**StartInfrastructureTask**: starting infrastructure tasks</span></span>                             
* <span data-ttu-id="3d6d3-139">**FinishInfrastructureTask**: Slutför infrastrukturen</span><span class="sxs-lookup"><span data-stu-id="3d6d3-139">**FinishInfrastructureTask**: finishing infrastructure tasks</span></span>                             
* <span data-ttu-id="3d6d3-140">**InvokeInfrastructureCommand**: kommandon för hantering av infrastruktur-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3d6d3-140">**InvokeInfrastructureCommand**: infrastructure task management commands</span></span>                              
* <span data-ttu-id="3d6d3-141">**ActivateNode**: aktivera en nod</span><span class="sxs-lookup"><span data-stu-id="3d6d3-141">**ActivateNode**: activating a node</span></span>                             
* <span data-ttu-id="3d6d3-142">**DeactivateNode**: inaktivera en nod</span><span class="sxs-lookup"><span data-stu-id="3d6d3-142">**DeactivateNode**: deactivating a node</span></span>                             
* <span data-ttu-id="3d6d3-143">**DeactivateNodesBatch**: inaktivera flera noder</span><span class="sxs-lookup"><span data-stu-id="3d6d3-143">**DeactivateNodesBatch**: deactivating multiple nodes</span></span>                             
* <span data-ttu-id="3d6d3-144">**RemoveNodeDeactivations**: Återför inaktiveringen på flera noder</span><span class="sxs-lookup"><span data-stu-id="3d6d3-144">**RemoveNodeDeactivations**: reverting deactivation on multiple nodes</span></span>                             
* <span data-ttu-id="3d6d3-145">**GetNodeDeactivationStatus**: Kontrollera status för inaktivering</span><span class="sxs-lookup"><span data-stu-id="3d6d3-145">**GetNodeDeactivationStatus**: checking deactivation status</span></span>                             
* <span data-ttu-id="3d6d3-146">**NodeStateRemoved**: reporting nodens tillstånd tas bort</span><span class="sxs-lookup"><span data-stu-id="3d6d3-146">**NodeStateRemoved**: reporting node state removed</span></span>                             
* <span data-ttu-id="3d6d3-147">**ReportFault**: fault-rapportering</span><span class="sxs-lookup"><span data-stu-id="3d6d3-147">**ReportFault**: reporting fault</span></span>                             
* <span data-ttu-id="3d6d3-148">**FileContent**: image store-klienten filöverföring (extern toocluster)</span><span class="sxs-lookup"><span data-stu-id="3d6d3-148">**FileContent**: image store client file transfer (external toocluster)</span></span>                             
* <span data-ttu-id="3d6d3-149">**FileDownload**: image store-klienten filen download inledande (extern toocluster)</span><span class="sxs-lookup"><span data-stu-id="3d6d3-149">**FileDownload**: image store client file download initiation (external toocluster)</span></span>                             
* <span data-ttu-id="3d6d3-150">**InternalList**: image store-klienten listan filåtgärd (internt)</span><span class="sxs-lookup"><span data-stu-id="3d6d3-150">**InternalList**: image store client file list operation (internal)</span></span>                             
* <span data-ttu-id="3d6d3-151">**Ta bort**: image store ta bort klientåtgärden</span><span class="sxs-lookup"><span data-stu-id="3d6d3-151">**Delete**: image store client delete operation</span></span>                              
* <span data-ttu-id="3d6d3-152">**Överför**: image store Överföringsåtgärden för klienten</span><span class="sxs-lookup"><span data-stu-id="3d6d3-152">**Upload**: image store client upload operation</span></span>                             
* <span data-ttu-id="3d6d3-153">**NodeControl**: starta, stoppa och starta om noder</span><span class="sxs-lookup"><span data-stu-id="3d6d3-153">**NodeControl**: starting, stopping, and restarting nodes</span></span>                             
* <span data-ttu-id="3d6d3-154">**MoveReplicaControl**: flytta repliker från en nod tooanother</span><span class="sxs-lookup"><span data-stu-id="3d6d3-154">**MoveReplicaControl**: moving replicas from one node tooanother</span></span>                             

### <a name="miscellaneous-operations"></a><span data-ttu-id="3d6d3-155">Diverse åtgärder</span><span class="sxs-lookup"><span data-stu-id="3d6d3-155">Miscellaneous operations</span></span>
* <span data-ttu-id="3d6d3-156">**Ping**: klienten ping</span><span class="sxs-lookup"><span data-stu-id="3d6d3-156">**Ping**: client pings</span></span>                             
* <span data-ttu-id="3d6d3-157">**Frågan**: alla frågor som tillåts</span><span class="sxs-lookup"><span data-stu-id="3d6d3-157">**Query**: all queries allowed</span></span>
* <span data-ttu-id="3d6d3-158">**NameExists**: namnge URI finns kontroller</span><span class="sxs-lookup"><span data-stu-id="3d6d3-158">**NameExists**: naming URI existence checks</span></span>                             

<span data-ttu-id="3d6d3-159">hello användaren åtkomst kontrolltypen är som standard begränsad toohello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="3d6d3-159">hello user access control type is, by default, limited toohello following operations:</span></span> 

* <span data-ttu-id="3d6d3-160">**EnumerateSubnames**: namnge URI uppräkning</span><span class="sxs-lookup"><span data-stu-id="3d6d3-160">**EnumerateSubnames**: naming URI enumeration</span></span>                             
* <span data-ttu-id="3d6d3-161">**EnumerateProperties**: namnge egenskapen uppräkning</span><span class="sxs-lookup"><span data-stu-id="3d6d3-161">**EnumerateProperties**: naming property enumeration</span></span>                             
* <span data-ttu-id="3d6d3-162">**PropertyReadBatch**: namnge egenskapen läsåtgärder</span><span class="sxs-lookup"><span data-stu-id="3d6d3-162">**PropertyReadBatch**: naming property read operations</span></span>                             
* <span data-ttu-id="3d6d3-163">**GetServiceDescription**: long avsökning tjänstmeddelanden och läsa tjänsten beskrivningar</span><span class="sxs-lookup"><span data-stu-id="3d6d3-163">**GetServiceDescription**: long-poll service notifications and reading service descriptions</span></span>                             
* <span data-ttu-id="3d6d3-164">**ResolveService**: klagomål-baserad tjänst-lösning</span><span class="sxs-lookup"><span data-stu-id="3d6d3-164">**ResolveService**: complaint-based service resolution</span></span>                             
* <span data-ttu-id="3d6d3-165">**ResolveNameOwner**: lösa namngivning URI-ägare</span><span class="sxs-lookup"><span data-stu-id="3d6d3-165">**ResolveNameOwner**: resolving naming URI owner</span></span>                             
* <span data-ttu-id="3d6d3-166">**ResolvePartition**: lösa systemtjänster</span><span class="sxs-lookup"><span data-stu-id="3d6d3-166">**ResolvePartition**: resolving system services</span></span>                             
* <span data-ttu-id="3d6d3-167">**ServiceNotifications**: händelsebaserat tjänstmeddelanden</span><span class="sxs-lookup"><span data-stu-id="3d6d3-167">**ServiceNotifications**: event-based service notifications</span></span>                             
* <span data-ttu-id="3d6d3-168">**GetUpgradeStatus**: avsökning uppgraderingsstatus för programmet</span><span class="sxs-lookup"><span data-stu-id="3d6d3-168">**GetUpgradeStatus**: polling application upgrade status</span></span>                             
* <span data-ttu-id="3d6d3-169">**GetFabricUpgradeStatus**: avsökning uppgraderingsstatus för kluster</span><span class="sxs-lookup"><span data-stu-id="3d6d3-169">**GetFabricUpgradeStatus**: polling cluster upgrade status</span></span>                             
* <span data-ttu-id="3d6d3-170">**InvokeInfrastructureQuery**: frågar infrastrukturen</span><span class="sxs-lookup"><span data-stu-id="3d6d3-170">**InvokeInfrastructureQuery**: querying infrastructure tasks</span></span>                             
* <span data-ttu-id="3d6d3-171">**Listan**: image store klientåtgärden filen lista</span><span class="sxs-lookup"><span data-stu-id="3d6d3-171">**List**: image store client file list operation</span></span>                             
* <span data-ttu-id="3d6d3-172">**ResetPartitionLoad**: återställa belastningen för en enhet för växling vid fel</span><span class="sxs-lookup"><span data-stu-id="3d6d3-172">**ResetPartitionLoad**: resetting load for a failover unit</span></span>                             
* <span data-ttu-id="3d6d3-173">**ToggleVerboseServicePlacementHealthReporting**: växla utförlig service placering hälsa reporting</span><span class="sxs-lookup"><span data-stu-id="3d6d3-173">**ToggleVerboseServicePlacementHealthReporting**: toggling verbose service placement health reporting</span></span>                             

<span data-ttu-id="3d6d3-174">Hej administratör åtkomstkontroll har också åtkomst toohello föregående åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3d6d3-174">hello admin access control also has access toohello preceding operations.</span></span>

## <a name="changing-default-settings-for-client-roles"></a><span data-ttu-id="3d6d3-175">Ändra standardinställningarna för klienten roller</span><span class="sxs-lookup"><span data-stu-id="3d6d3-175">Changing default settings for client roles</span></span>
<span data-ttu-id="3d6d3-176">I hello manifestfilen för klustret, kan du ange admin funktioner toohello klienten om det behövs.</span><span class="sxs-lookup"><span data-stu-id="3d6d3-176">In hello cluster manifest file, you can provide admin capabilities toohello client if needed.</span></span> <span data-ttu-id="3d6d3-177">Du kan ändra standardinställningarna för hello genom att gå toohello **Infrastrukturinställningarna** alternativ under [Skapa kluster](service-fabric-cluster-creation-via-portal.md), och ge hello före inställningar i hello **namn**, **admin**, **användare**, och **värdet** fält.</span><span class="sxs-lookup"><span data-stu-id="3d6d3-177">You can change hello defaults by going toohello **Fabric Settings** option during [cluster creation](service-fabric-cluster-creation-via-portal.md), and providing hello preceding settings in hello **name**, **admin**, **user**, and **value** fields.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d6d3-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d6d3-178">Next steps</span></span>
[<span data-ttu-id="3d6d3-179">Säkerhet för Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="3d6d3-179">Service Fabric cluster security</span></span>](service-fabric-cluster-security.md)

[<span data-ttu-id="3d6d3-180">Service Fabric-kluster</span><span class="sxs-lookup"><span data-stu-id="3d6d3-180">Service Fabric cluster creation</span></span>](service-fabric-cluster-creation-via-portal.md)

