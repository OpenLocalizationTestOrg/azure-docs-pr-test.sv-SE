---
title: "Länka ett virtuellt nätverk tooan ExpressRoute-krets: PowerShell: Azure | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur toolink virtuella nätverk (Vnet) tooExpressRoute kretsar med hjälp av hello Resource Manager-distributionsmodellen och PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: daacb6e5-705a-456f-9a03-c4fc3f8c1f7e
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: ganesr
ms.openlocfilehash: e75a9f6b42fa8e1a579e4f19882ec99b277b545f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit"></a>Ansluta ett virtuellt nätverk tooan ExpressRoute-krets
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassisk)](expressroute-howto-linkvnet-classic.md)
>

Den här artikeln hjälper dig att länka virtuella nätverk (Vnet) tooAzure ExpressRoute-kretsar med hello Resource Manager-distributionsmodellen och PowerShell. Virtuella nätverk kan antingen vara i hello samma prenumeration eller en del av en annan prenumeration. Den här artikeln visar också hur tooupdate ett virtuellt nätverk länkar. 

## <a name="before-you-begin"></a>Innan du börjar
* Installera hello senaste versionen av hello Azure PowerShell-moduler. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
* Granska hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.
* Du måste ha en aktiv ExpressRoute-krets. 
  * Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och ha hello krets aktiveras med anslutningsleverantören. 
  * Se till att du har privat Azure-peering konfigurerats för kretsen. Se hello [konfigurera routning](expressroute-howto-routing-arm.md) artikel routning anvisningar. 
  * Kontrollera att privat Azure-peering har konfigurerats och hello BGP-peering mellan ditt nätverk och Microsoft är igång så att du kan aktivera anslutning för slutpunkt till slutpunkt.
  * Se till att du har ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad. Följ instruktionerna för hello för[skapa en virtuell nätverksgateway för ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). En virtuell nätverksgateway expressroute använder hello GatewayType ExpressRoute, inte VPN.

* Du kan länka upp too10-virtuella nätverk tooa standard ExpressRoute-kretsen. Alla virtuella nätverk måste vara i hello samma geopolitiska region när du använder en standard ExpressRoute-krets. 

* Du kan länka en virtuella nätverk utanför hello geopolitiska regionens hello ExpressRoute-krets eller ansluta ett stort antal virtuella nätverk tooyour ExpressRoute-krets om du har aktiverat hello ExpressRoute premium-tillägg. Kontrollera hello [vanliga frågor och svar](expressroute-faqs.md) för mer information om hello premium-tillägg.


## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>Ansluta ett virtuellt nätverk i hello samma prenumeration tooa krets
Du kan ansluta ett virtuellt nätverk gateway tooan ExpressRoute-kretsen med hjälp av hello följande cmdlet. Kontrollera att hello virtuella nätverkets gateway har skapats och är redo för att länka innan du kör cmdlet hello:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Ansluta ett virtuellt nätverk i en annan prenumeration tooa-krets
Du kan dela en ExpressRoute-krets över flera prenumerationer. hello följande bild visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.

Varje hello mindre moln inom hello stora moln är används toorepresent prenumerationer som tillhör toodifferent avdelningar inom en organisation. Var och en av hello avdelningar inom hello organisation kan använda sina egna prenumeration för att distribuera sina tjänster, men de kan dela ett ExpressRoute-kretsen tooconnect tillbaka tooyour lokalt nätverk. En avdelning (i det här exemplet: IT) kan äga hello ExpressRoute-kretsen. Andra prenumerationer i hello organisation kan använda hello ExpressRoute-kretsen.

> [!NOTE]
> Anslutningar och bandbredd avgifter för hello ExpressRoute-krets kommer att tillämpas toohello prenumerationsägaren. Alla virtuella nätverk dela hello samma bandbredd.
> 
> 

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)


### <a name="administration---circuit-owners-and-circuit-users"></a>Administration - krets ägare och krets användare

hello 'kretsägaren' är en behörig Privilegierade användare i hello ExpressRoute-kretsen resurs. Hej kretsägaren kan skapa tillstånd som kan lösas av krets-användare. Kretsen användare är ägare till virtuella nätverk gateways som inte är inom hello samma prenumeration som hello ExpressRoute-kretsen. Kretsen användare kan lösa tillstånd (en auktorisering per virtuellt nätverk).

Hej kretsägaren har hello power toomodify samt återkalla tillstånd när som helst. Återkalla ett tillstånd som resulterar i alla anslutningar tas bort från hello prenumeration vars åtkomst har återkallats.

### <a name="circuit-owner-operations"></a>Kretsen ägare åtgärder

**toocreate tillstånd**

Hej kretsägaren skapar tillstånd. Detta resulterar i hello skapandet av auktoriseringsnyckel som kan användas av en krets användaren tooconnect sina virtuella nätverk gateways toohello ExpressRoute-kretsen. Ett tillstånd som är giltig för endast en anslutning.

följande cmdlet fragment visas hur hello toocreate tillstånd:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
```


hello svar toothis innehåller hello auktoriseringsnyckeln och status:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded



**tooreview tillstånd**

Hej kretsägaren kan granska alla tillstånd som utfärdats för en viss krets genom att köra följande cmdlet hello:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**tooadd tillstånd**

Hej kretsägaren kan lägga till tillstånd med hjälp av hello följande cmdlet:

```powershell
$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

$circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
$authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
```

**toodelete tillstånd**

Hej kretsägaren kan återkalla/ta bort tillstånd toohello användare genom att köra följande cmdlet hello:

```powershell
Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
```    

### <a name="circuit-user-operations"></a>Kretsen användaren åtgärder

Hej kretsanvändare måste hello peer-ID och auktoriseringsnyckel från hello kretsägaren. Hej auktoriseringsnyckeln är ett GUID.

Peer-ID kan kontrolleras från hello följande kommando:

```powershell
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**tooredeem en anslutningsverifiering**

hello kretsanvändare kan köra följande cmdlet tooredeem hello ett länk-tillstånd:

```powershell
$id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"    
$gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
$connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**toorelease en anslutningsverifiering**

Du kan släppa tillstånd genom att ta bort hello-anslutning som länkar hello ExpressRoute-kretsen toohello virtuellt nätverk.

## <a name="modify-a-virtual-network-connection"></a>Ändra en virtuell nätverksanslutning
Du kan uppdatera vissa egenskaper för virtuella nätverk. 

**tooupdate hello anslutning vikt**

Det virtuella nätverket kan vara anslutna toomultiple ExpressRoute-kretsar. Hello samma prefix får du från mer än en ExpressRoute-kretsen. toochoose vilken anslutning toosend trafik avsedda för det här prefixet, du kan ändra *RoutingWeight* för en anslutning. Trafik skickas på hello anslutning med hello högsta *RoutingWeight*.

```powershell
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name "MyVirtualNetworkConnection" -ResourceGroupName "MyRG"
$connection.RoutingWeight = 100
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection
```

Hej mängd *RoutingWeight* är 0 too32000. hello standardvärdet är 0.

## <a name="next-steps"></a>Nästa steg
Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).
