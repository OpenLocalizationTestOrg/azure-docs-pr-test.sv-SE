---
title: aaaScale ett Service Fabric-kluster in eller ut | Microsoft Docs
description: "Skala Service Fabric-klustret in eller ut toomatch begäran genom att ange regler för automatisk skalning för varje nod typ/virtuella datorns skaluppsättning. Lägg till eller ta bort noder tooa Service Fabric-kluster"
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: aeb76f63-7303-4753-9c64-46146340b83d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: 37cfeaf80edc016cf6de017d1c2dc6fbcb8acc2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules"></a>Skala Service Fabric-klustret in eller ut använda regler för automatisk skalning
Skaluppsättningar för den virtuella datorn är en Azure compute-resurs som du kan använda toodeploy och hantera en samling med virtuella datorer som en uppsättning. Varje nodtyp som definieras i Service Fabric-klustret har konfigurerats som en separat skaluppsättning för virtuell dator. Varje nodtyp kan sedan skalas i ut oberoende av varandra, har olika uppsättningar av öppna portar och kan ha olika kapacitetsdata. Läs mer om den i hello [nodetypes får Service Fabric](service-fabric-cluster-nodetypes.md) dokumentet. Eftersom hello Service Fabric-nodtyper i klustret har skapats på skalningsuppsättningar i virtuella datorer på hello backend, behöver du tooset Autoskala regler för varje nod typ/virtuella datorns skaluppsättning.

> [!NOTE]
> Din prenumeration måste ha tillräckligt med kärnor tooadd hello nya virtuella datorer som utgör det här klustret. Det finns ingen modellverifiering, så att du får tid distributionsfel, om någon av hello kvotgränserna träffar.
> 
> 

## <a name="choose-hello-node-typevirtual-machine-scale-set-tooscale"></a>Välj hello nod typ/virtuella datorn uppsättning tooscale
För närvarande kan du inte kan toospecify hello Autoskala regler för skalningsuppsättningar i virtuella datorer med hjälp av hello portal, så vi kan använda Azure PowerShell (1.0 +) toolist hello nodtyper och Lägg sedan till Autoskala regler toothem.

tooget hello lista över virtuella skaluppsättning som utgör ditt kluster som kör hello följande cmdlets:

```powershell
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzureRmVmss -ResourceGroupName <RGname> -VMScaleSetName <Virtual Machine scale set name>
```

## <a name="set-auto-scale-rules-for-hello-node-typevirtual-machine-scale-set"></a>Ange regler för automatisk skalning för hello nod typ/virtuella datorns skaluppsättning
Om klustret har flera nodtyper, upprepar du detta för varje nod typer/virtuella datorn anger som du vill tooscale (in eller ut). Ta hänsyn till kontot hello antalet noder som du behöver innan du ställer in automatisk skalning. hello minsta antalet noder som du måste ha för hello primära nodtypen drivs av hello tillförlitlighet nivå som du har valt. Läs mer om [tillförlitlighetsnivåer](service-fabric-cluster-capacity.md).

> [!NOTE]
> Skala ned hello primära noden typen tooless än hello minsta antalet göra hello klustret instabil eller sätta. Detta kan resultera i förlust av data för dina program och hello systemtjänster.
> 
> 

Hello Autoskala funktionen är för närvarande inte styrs av hello belastningar som dina program kan reporting tooService Fabric. Så vid denna tidpunkt hello drivs Autoskala du rent av hello prestandaräknare som orsakat av varje hello Virtual Machine scale set instanser.  

Följ dessa instruktioner [tooset in Autoskala för varje skaluppsättning för virtuell dator för](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md).

> [!NOTE]
> I en skala ned scenario, om inte din nodtyp har en hållbarhet guld eller Silver måste toocall hello [cmdlet Remove-ServiceFabricNodeState](https://msdn.microsoft.com/library/azure/mt125993.aspx) med hello lämpliga nodnamn.
> 
> 

## <a name="manually-add-vms-tooa-node-typevirtual-machine-scale-set"></a>Manuellt lägga till virtuella datorer tooa nod typ/virtuella datorns skaluppsättning
Följ hello exemplet/instruktionerna i hello [Snabbstart mallgalleriet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) toochange hello antalet virtuella datorer i varje Nodetype. 

> [!NOTE]
> Lägga till virtuella datorer tar tid, så förväntar sig inte hello tillägg toobe omedelbar. Så planerar tooadd kapacitet bra i tid, tooallow mer än 10 minuter innan hello VM-kapaciteten är tillgänglig för hello repliker / service instanser tooget placeras.
> 
> 

## <a name="manually-remove-vms-from-hello-primary-node-typevirtual-machine-scale-set"></a>Ta bort virtuella datorer manuellt från hello primära noden typ/virtuella datorns skaluppsättning
> [!NOTE]
> hello service fabric-systemtjänster kör i hello primära nodtypen i klustret. Mindre än vad hello tillförlitlighetsnivån garanterar så aldrig ska stänga av eller skala ned hello antalet instanser i de nodtyperna. Se för[hello information på tillförlitlighet nivåerna här](service-fabric-cluster-capacity.md). 
> 
> 

Du behöver tooexecute hello Följ anvisningarna nedan för en VM-instans i taget. Detta ger hello systemtjänster (och tillståndskänsliga tjänster) toobe avslutas på vanligt sätt på hello VM-instans som du tar bort och nya repliker som skapas på andra noder.

1. Kör [inaktivera ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) med avsiktshantering 'RemoveNode' toodisable hello noden ska tooremove (hello högsta instans i den nodtypen).
2. Kör [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) toomake till noden hello verkligen har övergått toodisabled. Om inte, vänta tills hello nod är inaktiverad. Du kan skynda dig det här steget.
3. Följ hello exemplet/instruktionerna i hello [Snabbstart mallgalleriet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) toochange hello antal virtuella datorer med en i denna Nodetype. hello instansen tas bort är hello högsta VM. 
4. Upprepa steg 1 till 3 efter behov, men aldrig skala ned hello antalet instanser i hello primära nodtyper mindre än vad hello tillförlitlighetsnivån garanterar. Se för[hello information på tillförlitlighet nivåerna här](service-fabric-cluster-capacity.md). 

## <a name="manually-remove-vms-from-hello-non-primary-node-typevirtual-machine-scale-set"></a>Ta bort virtuella datorer manuellt från hello icke-primär nod typ/virtuella datorns skaluppsättning
> [!NOTE]
> För en tillståndskänslig tjänst behöver du ett visst antal noder toobe alltid in toomaintain tillgänglighet och bevara tillståndet för din tjänst. Vid hello mycket minsta behöver du hello antalet noder lika toohello mål replik set antal hello partitionen/service. 
> 
> 

Du behöver hello köra hello följande steg för en VM-instans i taget. Detta ger hello systemtjänster (och tillståndskänsliga tjänster) toobe avslutas på vanligt sätt på hello VM-instans som du tar bort och nya repliker skapas annan var.

1. Kör [inaktivera ServiceFabricNode](https://msdn.microsoft.com/library/mt125852.aspx) med avsiktshantering 'RemoveNode' toodisable hello noden ska tooremove (hello högsta instans i den nodtypen).
2. Kör [Get-ServiceFabricNode](https://msdn.microsoft.com/library/mt125856.aspx) toomake till noden hello verkligen har övergått toodisabled. Om du inte vänta tills hello nod är inaktiverad. Du kan skynda dig det här steget.
3. Följ hello exemplet/instruktionerna i hello [Snabbstart mallgalleriet](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) toochange hello antal virtuella datorer med en i denna Nodetype. Hello högsta VM-instans tas nu bort. 
4. Upprepa steg 1 till 3 efter behov, men aldrig skala ned hello antalet instanser i hello primära nodtyper mindre än vad hello tillförlitlighetsnivån garanterar. Se för[hello information på tillförlitlighet nivåerna här](service-fabric-cluster-capacity.md).

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Beteenden som du kan se i Service Fabric Explorer
När du skalar upp en kluster-hello visar Service Fabric Explorer hello antalet noder (virtuella skaluppsättning instanser) som ingår i hello kluster.  Men när du skalar ett kluster av du ser hello bort noden och VM-instans visas i feltillstånd om du anropar [ta bort ServiceFabricNodeState cmd](https://msdn.microsoft.com/library/mt125993.aspx) med hello lämpliga nodnamn.   

Här är hello förklaring till problemet.

hello noder som anges i Service Fabric Explorer är reflektion vilka hello Service Fabric-systemtjänster (FM specifikt) vet om hello antalet noder hello klustret hade/har. När du skalar hello virtuella skala anges hello VM togs bort men FM systemtjänst fortfarande tror att hello-nod (som var mappade toohello VM som har tagits bort) kommer tillbaka. Så fortsätter Service Fabric Explorer toodisplay noden (även om hälsotillståndet för hello kan vara fel eller okänd).

I ordning toomake till att en nod ska tas bort när en virtuell dator tas bort, har du två alternativ:

1) Välj den hållbarhet guld eller Silver (tillgänglig snart) för hello nodtyper i klustret, som ger dig hello infrastruktur-integration. Som sedan bort hello noder från våra tjänster (FM) systemtillstånd när du skala.
Se för[hello information om hållbarhet nivåer här](service-fabric-cluster-capacity.md)

2) När hello VM-instans har skalats, måste toocall hello [cmdlet Remove-ServiceFabricNodeState](https://msdn.microsoft.com/library/mt125993.aspx).

> [!NOTE]
> Service Fabric-kluster kräver ett visst antal noder toobe in på alla hello tid i ordning toomaintain tillgänglighet och bevara tillstånd - refererad tooas ”för att underhålla kvorum”. Det är den vanligtvis osäkra tooshut ned alla hello-datorer i hello kluster om du först har utfört en [fullständig säkerhetskopiering av din tillstånd](service-fabric-reliable-services-backup-restore.md).
> 
> 

## <a name="next-steps"></a>Nästa steg
Läs hello följande tooalso mer information om planering klustret kapacitet, uppgradera ett kluster och partitionering tjänster:

* [Planera din kapacitet för kluster](service-fabric-cluster-capacity.md)
* [Klusteruppgradering](service-fabric-cluster-upgrade.md)
* [Partitionen tillståndskänsliga tjänster för maximal skala](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png
