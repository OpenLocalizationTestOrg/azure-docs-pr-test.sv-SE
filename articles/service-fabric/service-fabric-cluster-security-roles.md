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
# <a name="role-based-access-control-for-service-fabric-clients"></a>Rollbaserad åtkomstkontroll för Service Fabric-klienter
Azure Service Fabric stöder två typer av olika åtkomstkontroll för klienter som är anslutna tooa Service Fabric-kluster: administratörs- och. Åtkomstkontroll kan Hej administratör toolimit åtkomst toocertain klustret klusteråtgärder för olika grupper av användare, göra hello klustret säkrare.  

**Administratörer** har fullständig åtkomst toomanagement funktioner (inklusive funktioner för läsning och skrivning). Som standard **användare** bara har läsbehörighet toomanagement funktioner (till exempel frågefunktioner) och hello möjlighet tooresolve program och tjänster.

Du kan ange hello två klienten roller (administratör och klient) när hello klustret har skapats genom att tillhandahålla olika certifikat för varje. Se [Service Fabric-Klustersäkerhet](service-fabric-cluster-security.md) mer information om hur du konfigurerar en säker Service Fabric-klustret.

## <a name="default-access-control-settings"></a>Inställningar för åtkomstkontroll som standard
Hej administratör åtkomst kontrolltypen har fullständig åtkomst tooall hello FabricClient APIs. Den kan utföra alla läsning och skrivning på hello Service Fabric-klustret, inklusive hello följande åtgärder:

### <a name="application-and-service-operations"></a>Program- och tjänståtgärder
* **CreateService**: skapar en tjänst                             
* **CreateServiceFromTemplate**: tjänsten skapa från mall                             
* **UpdateService**: hantera uppdateringar                             
* **DeleteService**: Tjänsten tas bort                             
* **ProvisionApplicationType**: programmet typen etablering                             
* **CreateApplication**: skapa program                               
* **DeleteApplication**: ta bort program                             
* **UpgradeApplication**: startar eller störa programuppgraderingar                             
* **UnprovisionApplicationType**: programmet typen avetablering                             
* **MoveNextUpgradeDomain**: återupptar programuppgraderingar med en explicit uppdateringsdomän                             
* **ReportUpgradeHealth**: återupptar programuppgraderingar med hello aktuella Uppgraderingsförlopp                             
* **ReportHealth**: reporting hälsa                             
* **PredeployPackageToNode**: hjälp av noggrann API                            
* **CodePackageControl**: starta om koden paket                             
* **RecoverPartition**: återställning av en partition                             
* **RecoverPartitions**: återställning av partitioner                             
* **RecoverServicePartitions**: återställa tjänsten partitioner                             
* **RecoverSystemPartitions**: återställa tjänsten systempartitionerna                             

### <a name="cluster-operations"></a>Klusteråtgärder
* **ProvisionFabric**: MSI och/eller klusternamnresursen manifest etablering                             
* **UpgradeFabric**: starta klusteruppgradering                             
* **UnprovisionFabric**: MSI och/eller klusternamnresursen manifest avetablering                         
* **MoveNextFabricUpgradeDomain**: återupptar klusteruppgradering med en explicit uppdateringsdomän                             
* **ReportFabricUpgradeHealth**: återupptar klusteruppgradering med hello aktuella Uppgraderingsförlopp                             
* **StartInfrastructureTask**: från infrastrukturen                             
* **FinishInfrastructureTask**: Slutför infrastrukturen                             
* **InvokeInfrastructureCommand**: kommandon för hantering av infrastruktur-aktivitet                              
* **ActivateNode**: aktivera en nod                             
* **DeactivateNode**: inaktivera en nod                             
* **DeactivateNodesBatch**: inaktivera flera noder                             
* **RemoveNodeDeactivations**: Återför inaktiveringen på flera noder                             
* **GetNodeDeactivationStatus**: Kontrollera status för inaktivering                             
* **NodeStateRemoved**: reporting nodens tillstånd tas bort                             
* **ReportFault**: fault-rapportering                             
* **FileContent**: image store-klienten filöverföring (extern toocluster)                             
* **FileDownload**: image store-klienten filen download inledande (extern toocluster)                             
* **InternalList**: image store-klienten listan filåtgärd (internt)                             
* **Ta bort**: image store ta bort klientåtgärden                              
* **Överför**: image store Överföringsåtgärden för klienten                             
* **NodeControl**: starta, stoppa och starta om noder                             
* **MoveReplicaControl**: flytta repliker från en nod tooanother                             

### <a name="miscellaneous-operations"></a>Diverse åtgärder
* **Ping**: klienten ping                             
* **Frågan**: alla frågor som tillåts
* **NameExists**: namnge URI finns kontroller                             

hello användaren åtkomst kontrolltypen är som standard begränsad toohello följande åtgärder: 

* **EnumerateSubnames**: namnge URI uppräkning                             
* **EnumerateProperties**: namnge egenskapen uppräkning                             
* **PropertyReadBatch**: namnge egenskapen läsåtgärder                             
* **GetServiceDescription**: long avsökning tjänstmeddelanden och läsa tjänsten beskrivningar                             
* **ResolveService**: klagomål-baserad tjänst-lösning                             
* **ResolveNameOwner**: lösa namngivning URI-ägare                             
* **ResolvePartition**: lösa systemtjänster                             
* **ServiceNotifications**: händelsebaserat tjänstmeddelanden                             
* **GetUpgradeStatus**: avsökning uppgraderingsstatus för programmet                             
* **GetFabricUpgradeStatus**: avsökning uppgraderingsstatus för kluster                             
* **InvokeInfrastructureQuery**: frågar infrastrukturen                             
* **Listan**: image store klientåtgärden filen lista                             
* **ResetPartitionLoad**: återställa belastningen för en enhet för växling vid fel                             
* **ToggleVerboseServicePlacementHealthReporting**: växla utförlig service placering hälsa reporting                             

Hej administratör åtkomstkontroll har också åtkomst toohello föregående åtgärder.

## <a name="changing-default-settings-for-client-roles"></a>Ändra standardinställningarna för klienten roller
I hello manifestfilen för klustret, kan du ange admin funktioner toohello klienten om det behövs. Du kan ändra standardinställningarna för hello genom att gå toohello **Infrastrukturinställningarna** alternativ under [Skapa kluster](service-fabric-cluster-creation-via-portal.md), och ge hello före inställningar i hello **namn**, **admin**, **användare**, och **värdet** fält.

## <a name="next-steps"></a>Nästa steg
[Säkerhet för Service Fabric-kluster](service-fabric-cluster-security.md)

[Service Fabric-kluster](service-fabric-cluster-creation-via-portal.md)

