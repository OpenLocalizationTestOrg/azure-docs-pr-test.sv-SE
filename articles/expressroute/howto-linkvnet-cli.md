---
title: "Länka ett virtuellt nätverk tooan ExpressRoute-krets: CLI: Azure | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur toolink virtuella nätverk (Vnet) tooExpressRoute kretsar med hjälp av hello Resource Manager-modellen och CLI."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlit
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: anzaman,cherylmc
ms.openlocfilehash: 1251f016d9b94d3fee81de1df164cb085cbe9d78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-cli"></a>Ansluta ett virtuellt nätverk tooan ExpressRoute-kretsen med hjälp av CLI

Den här artikeln hjälper dig att länka virtuella nätverk (Vnet) tooAzure ExpressRoute-kretsar med hjälp av CLI. toolink med hjälp av Azure CLI hello virtuella nätverk måste skapas med hello Resource Manager-modellen. De kan antingen vara i hello samma prenumeration, eller en del av en annan prenumeration. Om du vill toouse en annan metod tooconnect ditt VNet tooan ExpressRoute-krets, kan du välja en artikel hello följande lista:

> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassisk)](expressroute-howto-linkvnet-classic.md)
> 

## <a name="configuration-prerequisites"></a>Förutsättningar för konfiguration

* Du måste hello senaste versionen av hello-kommandoradsgränssnittet (CLI). Mer information finns i [installera Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).
* Du behöver tooreview hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.
* Du måste ha en aktiv ExpressRoute-krets. 
  * Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](howto-circuit-cli.md) och ha hello krets aktiveras med anslutningsleverantören. 
  * Se till att du har privat Azure-peering konfigurerats för kretsen. Se hello [konfigurera routning](howto-routing-cli.md) artikel routning anvisningar. 
  * Kontrollera att Azure privat peering är konfigurerad. hello BGP-peering mellan ditt nätverk och Microsoft måste vara in så att du kan aktivera anslutning för slutpunkt till slutpunkt.
  * Se till att du har ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad. Följ instruktionerna för hello för[konfigurera en virtuell nätverksgateway för ExpressRoute](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli). Vara säker på att toouse `--gateway-type ExpressRoute`.

* Du kan länka upp too10-virtuella nätverk tooa standard ExpressRoute-kretsen. Alla virtuella nätverk måste vara i hello samma geopolitiska region när du använder en standard ExpressRoute-krets. 

* Om du aktiverar hello ExpressRoute premium-tillägg kan du länka ett virtuellt nätverk utanför hello geopolitiska regionens hello ExpressRoute-krets eller ansluta ett stort antal virtuella nätverk tooyour ExpressRoute-kretsen. Mer information om hello premium-tillägg finns hello [vanliga frågor och svar](expressroute-faqs.md).

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>Ansluta ett virtuellt nätverk i hello samma prenumeration tooa krets

Du kan ansluta ett virtuellt nätverk gateway tooan ExpressRoute-kretsen med hjälp av hello exempel. Kontrollera att den virtuella nätverksgatewayen hello har skapats och är redo för att länka innan du kör kommandot hello.

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit
```

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Ansluta ett virtuellt nätverk i en annan prenumeration tooa-krets

Du kan dela en ExpressRoute-krets över flera prenumerationer. hello bilden nedan visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.

Varje hello mindre moln inom hello stora moln är används toorepresent prenumerationer som tillhör toodifferent avdelningar inom en organisation. Var och en av hello avdelningar inom hello organisation kan använda sina egna prenumeration för att distribuera sina tjänster, men de kan dela ett ExpressRoute-kretsen tooconnect tillbaka tooyour lokalt nätverk. En avdelning (i det här exemplet: IT) kan äga hello ExpressRoute-kretsen. Andra prenumerationer i hello organisation kan använda hello ExpressRoute-kretsen.

> [!NOTE]
> Anslutningar och bandbredd kostnader för dedikerade hello krets kommer att tillämpas toohello ExpressRoute-kretsen ägare. Alla virtuella nätverk dela hello samma bandbredd.
> 
> 

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration---circuit-owners-and-circuit-users"></a>Administration - krets ägare och krets användare

hello 'Kretsägaren' är en behörig Privilegierade användare i hello ExpressRoute-kretsen resurs. Hej Kretsägaren kan skapa tillstånd som kan lösas av krets-användare. Kretsen användare är ägare till virtuella nätverk gateways som inte är inom hello samma prenumeration som hello ExpressRoute-kretsen. Kretsen användare kan lösa tillstånd (en auktorisering per virtuellt nätverk).

Hej Kretsägaren har hello power toomodify samt återkalla tillstånd när som helst. När ett tillstånd återkallas tas alla anslutningar bort från hello prenumeration vars åtkomst har återkallats.

### <a name="circuit-owner-operations"></a>Kretsen ägare åtgärder

**toocreate tillstånd**

Hej Kretsägaren skapar tillstånd, vilket skapar auktoriseringsnyckel som kan användas av en Kretsanvändare tooconnect sina virtuella nätverk gateways toohello ExpressRoute-kretsen. Ett tillstånd som är giltig för endast en anslutning.

följande exempel visar hur hello toocreate tillstånd:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization
```

hello svaret innehåller hello auktoriseringsnyckeln och status:

```azurecli
"authorizationKey": "0a7f3020-541f-4b4b-844a-5fb43472e3d7",
"authorizationUseStatus": "Available",
"etag": "W/\"010353d4-8955-4984-807a-585c21a22ae0\"",
"id": "/subscriptions/81ab786c-56eb-4a4d-bb5f-f60329772466/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/MyCircuit/authorizations/MyAuthorization1",
"name": "MyAuthorization1",
"provisioningState": "Succeeded",
"resourceGroup": "ExpressRouteResourceGroup"
```

**tooreview tillstånd**

Hej Kretsägaren kan granska alla tillstånd som utfärdats för en viss krets genom att köra hello följande exempel:

```azurecli
az network express-route auth list --circuit-name MyCircuit -g ExpressRouteResourceGroup
```

**tooadd tillstånd**

Hej Kretsägaren kan lägga till tillstånd med hjälp av hello följande exempel:

```azurecli
az network express-route auth create --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

**toodelete tillstånd**

Hej Kretsägaren kan återkalla/ta bort tillstånd toohello användare genom att köra hello följande exempel:

```azurecli
az network express-route auth delete --circuit-name MyCircuit -g ExpressRouteResourceGroup -n MyAuthorization1
```

### <a name="circuit-user-operations"></a>Kretsen användare

Hej Kretsanvändare måste hello peer-ID och auktoriseringsnyckel från hello Kretsägaren. Hej auktoriseringsnyckeln är ett GUID.

```azurecli
Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
```

**tooredeem en anslutningsverifiering**

Hej Kretsanvändare kan köra hello följande exempel tooredeem ett länk-tillstånd:

```azurecli
az network vpn-connection create --name ERConnection --resource-group ExpressRouteResourceGroup --vnet-gateway1 VNet1GW --express-route-circuit2 MyCircuit --authorization-key "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"
```

**toorelease en anslutningsverifiering**

Du kan släppa tillstånd genom att ta bort hello-anslutning som länkar hello ExpressRoute-kretsen toohello virtuellt nätverk.

## <a name="next-steps"></a>Nästa steg

Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).
