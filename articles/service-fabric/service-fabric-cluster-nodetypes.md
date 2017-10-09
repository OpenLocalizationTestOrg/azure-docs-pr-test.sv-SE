---
title: "aaaService Fabric nodtyper och Skalningsuppsättningar | Microsoft Docs"
description: "Beskriver hur Service Fabric-nodtyper relaterade tooVM Skaluppsättningar och hur tooremote ansluter tooa VM Scale Set-instans eller en nod i klustret."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: chackdan
ms.openlocfilehash: 830ea2816f5864de146a77483c85de26f91c2425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>hello förhållandet mellan Service Fabric-nodtyper och Skalningsuppsättningar i virtuella datorer
Skaluppsättningar för den virtuella datorn är en Azure Compute resurs kan du använda toodeploy och hantera en samling med virtuella datorer som en uppsättning. Varje nodtyp som definieras i Service Fabric-klustret har konfigurerats som en separat Skaluppsättning. Varje nodtyp kan sedan skalas upp eller ned separat, har olika uppsättningar av öppna portar och kan ha olika kapacitetsdata.

hello följande skärmbild visar ett kluster med två nodtyper: FrontEnd och BackEnd.  Varje nodtyp har fem noder.

![Skärmbild av ett kluster med två nodtyper][NodeTypes]

## <a name="mapping-vm-scale-set-instances-toonodes"></a>Mappning av Skaluppsättning instanser toonodes
Som du ser ovan hello VM Scale Set instanser börja från instans 0 och går sedan upp. hello numrering återspeglas i hello namn. BackEnd_0 är till exempel hello serverdelens VM Scale Set-instans 0. Den här viss VM Scale Set har fem instanser, med namnet BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 och BackEnd_4.

När du skalar upp en VM Scale Set skapas en ny instans. hello nya VM Scale Set instansnamn kommer normalt att hello VM Scale Set namn + hello nästa instansnummer. I vårt exempel är BackEnd_5.

## <a name="mapping-vm-scale-set-load-balancers-tooeach-node-typevm-scale-set"></a>Mappning av VM skaluppsättning för att läsa in belastningsutjämning typ och VM-nod tooeach Skaluppsättning
Om du har distribuerat ditt kluster från hello-portalen eller har hello exempel Resource Manager-mall som vi angav, sedan när du får en lista över alla resurser i en resursgrupp ser du hello belastningsutjämning för varje typ av VM Scale Set eller nod.

hello namn skulle något som liknar: **LB -&lt;NodeType namnet&gt;**. Till exempel LB-sfcluster4doc-0, som visas i den här skärmbilden:

![Resurser][Resources]

## <a name="remote-connect-tooa-vm-scale-set-instance-or-a-cluster-node"></a>Fjärranslutning tooa VM Scale Set-instans eller en klusternod
Varje nodtyp som definieras i ett kluster har konfigurerats som en separat Skaluppsättning.  Att innebär hello nodtyper kan skalas upp eller ned separat och kan göras av olika VM SKU: er. Till skillnad från en enda instans VMs får hello VM Scale Set instanser inte en virtuell IP-adress för sina egna. Så kan det ta en stund ansluta utmaning när du söker efter en IP-adress och port som du kan använda tooremote tooa specifika instansen.

Här är hello-åtgärder som du kan följa toodiscover dem.

### <a name="step-1-find-out-hello-virtual-ip-address-for-hello-node-type-and-then-inbound-nat-rules-for-rdp"></a>Steg 1: Ta reda på hello virtuella IP-adressen för hello nodtypen och inkommande NAT-regler för RDP
I ordning tooget att du behöver tooget hello inkommande NAT-regler värden som definierats som en del av hello resursdefinitionen för **Microsoft.Network/loadBalancers**.

Navigera i hello portal toohello Load balancer bladet och sedan **inställningar**.

![LBBlade][LBBlade]

I **inställningar**, klicka på **inkommande NAT-regler**. Det här nu ger du hello IP-adress och port som du kan använda tooremote ansluta toohello första VM Scale Set-instansen. I hello skärmbilden nedan, är det **104.42.106.156** och **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-hello-port-that-you-can-use-tooremote-connect-toohello-specific-vm-scale-set-instancenode"></a>Steg 2: Ta reda på hello-port som du kan använda tooremote Anslut toohello specifik VM Scale Set instans/nod
Tidigare i det här dokumentet I talade vi om hur hello VM Scale Set instanser mappa toohello noder. Vi använder den toofigure ut hello exakt porten.

hello portar tilldelas i stigande ordning hello VM Scale Set-instans. i vårt exempel för hello klientdel nodtyp finns så hello portar för varje hello fem instanser hello följande. du nu måste toodo hello samma mappning för din VM Scale Set-instans.

| **VM Scale Set-instans** | **Port** |
| --- | --- |
| FrontEnd_0 |3389 |
| FrontEnd_1 |3390 |
| FrontEnd_2 |3391 |
| FrontEnd_3 |3392 |
| FrontEnd_4 |3393 |
| FrontEnd_5 |3394 |

### <a name="step-3-remote-connect-toohello-specific-vm-scale-set-instance"></a>Steg 3: Fjärranslutning toohello specifika VM Scale Set-instans
Jag använder anslutning till fjärrskrivbord tooconnect toohello FrontEnd_1 i hello skärmbilden nedan:

![RDP][RDP]

## <a name="how-toochange-hello-rdp-port-range-values"></a>Hur toochange hello RDP-porten vara värden
### <a name="before-cluster-deployment"></a>Innan du Klusterdistribution
När du ställer in hello-kluster med hjälp av en Resource Manager-mall kan du ange hello intervallet i hello **inboundNatPools**.

Gå toohello resursdefinitionen **Microsoft.Network/loadBalancers**. Under som du hittar hello beskrivning för **inboundNatPools**.  Ersätt hello *frontendPortRangeStart* och *frontendPortRangeEnd* värden.

![InboundNatPools][InboundNatPools]

### <a name="after-cluster-deployment"></a>Efter distribution
Det här är lite mer invecklad och kan resultera i hello VMs återanvänds. Du har nu tooset nya värden med hjälp av Azure PowerShell. Kontrollera att Azure PowerShell 1.0 eller senare är installerat på datorn. Om du inte har gjort det innan jag starkt föreslår att du följer stegen i hello [hur tooinstall och konfigurera Azure PowerShell.](/powershell/azure/overview)

Logga in tooyour Azure-konto. Om det här PowerShell-kommandot misslyckas av någon anledning, bör du kontrollera om du har Azure PowerShell har installerats korrekt.

```
Login-AzureRmAccount
```

Kör hello följande tooget information på din belastningsutjämnare och du ser hello värden för hello beskrivning för **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Ange *frontendPortRangeEnd* och *frontendPortRangeStart* toohello värden som du vill.

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use hello API version that get returned> -Force
```


## <a name="next-steps"></a>Nästa steg
* [Översikt över hello ”var som helst distribuera” funktionen och en jämförelse med Azure-hanterade kluster](service-fabric-deploy-anywhere.md)
* [Klustersäkerhet](service-fabric-cluster-security.md)
* [Fabric-SDK och komma igång](service-fabric-get-started.md)

<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
