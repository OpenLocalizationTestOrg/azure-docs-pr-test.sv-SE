---
title: "Länka ett virtuellt nätverk tooan ExpressRoute-krets: PowerShell: klassiska: Azure | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur toolink virtuella nätverk (Vnet) tooExpressRoute kretsar med hjälp av hello klassiska distributionsmodellen och PowerShell."
services: expressroute
documentationcenter: na
author: ganesr
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 9b53fd72-9b6b-4844-80b9-4e1d54fd0c17
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2017
ms.author: ganesr
ms.openlocfilehash: 6b8a01dcd4bbb9618ec3dd438cf0107538fb2a7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-virtual-network-tooan-expressroute-circuit-using-powershell-classic"></a>Ansluta ett virtuellt nätverk tooan ExpressRoute-kretsen med hjälp av PowerShell (klassisk)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Azure CLI](howto-linkvnet-cli.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klassisk)](expressroute-howto-linkvnet-classic.md)
>

Den här artikeln beskriver hur du länka virtuella nätverk (Vnet) tooAzure ExpressRoute-kretsar med hello klassiska distributionsmodellen och PowerShell. Virtuella nätverk kan antingen vara i samma prenumeration hello eller kan vara en del av en annan prenumeration.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Om distributionsmodeller för Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>Förutsättningar för konfiguration
1. Du måste hello senaste versionen av hello Azure PowerShell-moduler. Du kan hämta hello senaste PowerShell-moduler från hello PowerShell avsnitt i hello [Azure hämtar sidan](https://azure.microsoft.com/downloads/). Följ anvisningarna för hello i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) stegvisa instruktioner om hur tooconfigure din dator toouse hello Azure PowerShell-moduler.
2. Du behöver tooreview hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.
3. Du måste ha en aktiv ExpressRoute-krets.
   * Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-classic.md) och har anslutningsleverantören aktivera hello krets.
   * Se till att du har privat Azure-peering konfigurerats för kretsen. Se hello [konfigurera routning](expressroute-howto-routing-classic.md) artikel routning anvisningar.
   * Kontrollera att privat Azure-peering har konfigurerats och hello BGP-peering mellan ditt nätverk och Microsoft är igång så att du kan aktivera anslutning för slutpunkt till slutpunkt.
   * Du måste ha ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad. Följ instruktionerna för hello för[konfigurera ett virtuellt nätverk för ExpressRoute](expressroute-howto-vnet-portal-classic.md).

Du kan länka upp too10 virtuella nätverk tooan ExpressRoute-kretsen. Alla virtuella nätverk måste vara i hello samma geopolitiska region. Du kan länka ett stort antal virtuella nätverk tooyour ExpressRoute-krets eller länka virtuella nätverk som ingår i andra geopolitiska regioner om du har aktiverat hello ExpressRoute premium-tillägg. Kontrollera hello [vanliga frågor och svar](expressroute-faqs.md) för mer information om hello premium-tillägg.

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>Ansluta ett virtuellt nätverk i hello samma prenumeration tooa krets
Du kan länka ett virtuellt nätverk tooan ExpressRoute-kretsen med hjälp av hello följande cmdlet. Kontrollera att den virtuella nätverksgatewayen hello har skapats och är redo för att länka innan du kör cmdlet hello.

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"
    Provisioned

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Ansluta ett virtuellt nätverk i en annan prenumeration tooa-krets
Du kan dela en ExpressRoute-krets över flera prenumerationer. hello följande bild visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.

Varje hello mindre moln inom hello stora moln är används toorepresent prenumerationer som tillhör toodifferent avdelningar inom en organisation. Var och en av hello avdelningar inom hello organisation kan använda sina egna prenumeration för att distribuera sina tjänster, men hello avdelningar kan dela ett ExpressRoute-kretsen tooconnect tillbaka tooyour lokalt nätverk. En avdelning (i det här exemplet: IT) kan äga hello ExpressRoute-kretsen. Andra prenumerationer i hello organisation kan använda hello ExpressRoute-kretsen.

> [!NOTE]
> Anslutningar och bandbredd kostnader för dedikerade hello krets kommer att tillämpas toohello ExpressRoute-kretsen ägare. Alla virtuella nätverk dela hello samma bandbredd.
> 
> 

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Administration
Hej *kretsägaren* är Hej administratör/coadministrator hello prenumeration i vilka hello ExpressRoute-kretsen har skapats. Hej kretsägaren kan tillåta administratörer/coadministrators av andra prenumerationer, enligt tooas *krets användare*, toouse hello dedikerad anslutning som de äger. Kretsen användare som är auktoriserade toouse hello organisation ExpressRoute-kretsen kan länka hello virtuellt nätverk i deras prenumeration toohello ExpressRoute-krets när de har behörighet.

Hej kretsägaren har hello power toomodify samt återkalla tillstånd när som helst. Återkalla ett tillstånd som leder till att alla länkar tas bort från hello prenumeration vars åtkomst har återkallats.

### <a name="circuit-owner-operations"></a>Kretsen ägare åtgärder

**Skapa ett tillstånd**

Hej kretsägaren auktoriserar hello administratörer av andra prenumerationer toouse hello angivna kretsen. I följande exempel hello, aktiverar Hej administratör av hello krets (Contoso IT) Hej administratör av en annan prenumeration (Dev-Test) toolink in tootwo virtuella nätverk toohello krets. hello Contoso IT-administratören gör detta genom att ange hello Dev-Test Microsoft-ID. hello cmdlet Skicka inte e-toohello angivna Microsoft-ID. Hej kretsägaren måste tooexplicitly meddela hello andra prenumerationsägaren som hello auktorisering är klar.

    New-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -Description "Dev-Test Links" -Limit 2 -MicrosoftIds 'devtest@contoso.com'

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : **********************************
    MicrosoftIds        : devtest@contoso.com
    Used                : 0

**Granska tillstånd**

Hej kretsägaren kan granska alla tillstånd som utfärdats för en viss krets genom att köra följande cmdlet hello:

    Get-AzureDedicatedCircuitLinkAuthorization -ServiceKey: "**************************"

    Description         : EngineeringTeam
    Limit               : 3
    LinkAuthorizationId : ####################################
    MicrosoftIds        : engadmin@contoso.com
    Used                : 1

    Description         : MarketingTeam
    Limit               : 1
    LinkAuthorizationId : @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    MicrosoftIds        : marketingadmin@contoso.com
    Used                : 0

    Description         : Dev-Test Links
    Limit               : 2
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : salesadmin@contoso.com
    Used                : 2


**Uppdaterar tillstånd**

Hej kretsägaren kan ändra tillstånd med hjälp av hello följande cmdlet:

    Set-AzureDedicatedCircuitLinkAuthorization -ServiceKey "**************************" -AuthorizationId "&&&&&&&&&&&&&&&&&&&&&&&&&&&&"-Limit 5

    Description         : Dev-Test Links
    Limit               : 5
    LinkAuthorizationId : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    MicrosoftIds        : devtest@contoso.com
    Used                : 0


**Ta bort tillstånd**

Hej kretsägaren kan återkalla/ta bort tillstånd toohello användare genom att köra följande cmdlet hello:

    Remove-AzureDedicatedCircuitLinkAuthorization -ServiceKey "*****************************" -AuthorizationId "###############################"


### <a name="circuit-user-operations"></a>Kretsen användaren åtgärder

**Granska tillstånd**

Hej kretsanvändare kan granska tillstånd med hjälp av hello följande cmdlet:

    Get-AzureAuthorizedDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : ContosoIT
    Location                         : Washington DC
    MaximumAllowedLinks              : 2
    ServiceKey                       : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled
    UsedLinks                        : 0

**Löser länken tillstånd**

hello kretsanvändare kan köra följande cmdlet tooredeem hello ett länk-tillstånd:

    New-AzureDedicatedCircuitLink –servicekey "&&&&&&&&&&&&&&&&&&&&&&&&&&" –VnetName 'SalesVNET1'

    State VnetName
    ----- --------
    Provisioned SalesVNET1

Kör kommandot i hello nyligen länkade prenumerationen för hello virtuellt nätverk:

    New-AzureDedicatedCircuitLink -ServiceKey "*****************************" -VNetName "MyVNet"

## <a name="next-steps"></a>Nästa steg
Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).

