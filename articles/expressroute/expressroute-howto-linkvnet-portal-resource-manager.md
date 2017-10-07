---
title: "Länka ett virtuellt nätverk tooan ExpressRoute-krets: Azure-portalen | Microsoft Docs"
description: "Det här dokumentet innehåller en översikt över hur toolink virtuella nätverk (Vnet) tooExpressRoute kretsar."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8bedcb11df7e30281fd439afdfb76cc67626a8f5
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

Den här artikeln hjälper dig att länka virtuella nätverk (Vnet) tooAzure ExpressRoute-kretsar med hjälp av hello Resource Manager-modellen och hello Azure-portalen. Virtuella nätverk kan antingen vara i hello samma prenumeration och de kan vara en del av en annan prenumeration.

## <a name="before-you-begin"></a>Innan du börjar
* Granska hello [krav](expressroute-prerequisites.md), [routningskrav](expressroute-routing.md), och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.
* Du måste ha en aktiv ExpressRoute-krets.
  
  * Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och ha hello krets aktiveras med anslutningsleverantören.
  * Se till att du har privat Azure-peering konfigurerats för kretsen. Se hello [konfigurera routning](expressroute-howto-routing-portal-resource-manager.md) artikel routning anvisningar.
  * Kontrollera att privat Azure-peering har konfigurerats och hello BGP-peering mellan ditt nätverk och Microsoft är igång så att du kan aktivera anslutning för slutpunkt till slutpunkt.
  * Se till att du har ett virtuellt nätverk och en virtuell nätverksgateway skapas och helt etablerad. Följ instruktionerna för hello för[skapa en virtuell nätverksgateway för ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). En virtuell nätverksgateway expressroute använder hello GatewayType ExpressRoute, inte VPN.

* Du kan länka upp too10-virtuella nätverk tooa standard ExpressRoute-kretsen. Alla virtuella nätverk måste vara i hello samma geopolitiska region när du använder en standard ExpressRoute-krets. 
* Du kan länka ett virtuellt nätverk utanför hello geopolitiska regionens hello ExpressRoute-krets eller ansluta ett stort antal virtuella nätverk tooyour ExpressRoute-krets om du har aktiverat hello ExpressRoute premium-tillägg. Kontrollera hello [vanliga frågor och svar](expressroute-faqs.md) för mer information om hello premium-tillägg.
* Du kan [visa en video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) innan början toobetter förstå hello steg.

## <a name="connect-a-virtual-network-in-hello-same-subscription-tooa-circuit"></a>Ansluta ett virtuellt nätverk i hello samma prenumeration tooa krets

### <a name="toocreate-a-connection"></a>toocreate en anslutning

> [!NOTE]
> BGP-konfigurationsinformationen kommer inte att visas om hello nivå 3-providern konfigurerad din peerkopplingar. Om kretsen är i ett etablerat tillstånd, bör du kunna toocreate anslutningar.
>

1. Se till att dina ExpressRoute-krets och privat Azure-peering har konfigurerats. Följ anvisningarna för hello i [skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och [konfigurera routning](expressroute-howto-routing-arm.md). ExpressRoute-krets bör se ut som hello följande bild:

    ![Skärmbild för ExpressRoute-krets](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. Du kan nu börja etablera en anslutning toolink ditt virtuella nätverk på en gateway-tooyour ExpressRoute-kretsen. Klicka på **anslutning** > **Lägg till** tooopen hello **Lägg till anslutning** bladet och sedan konfigurera hello värden.

    ![Lägg till anslutning skärmbild](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. När anslutningen har konfigurerats, visas connection-objektet hello information för hello-anslutning.

     ![Skärmbild av anslutningen objekt](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="toodelete-a-connection"></a>toodelete en anslutning
Du kan ta bort en anslutning genom att välja hello **ta bort** ikon på hello bladet för din anslutning.

## <a name="connect-a-virtual-network-in-a-different-subscription-tooa-circuit"></a>Ansluta ett virtuellt nätverk i en annan prenumeration tooa-krets
Du kan dela en ExpressRoute-krets över flera prenumerationer. hello bilden nedan visar ett enkelt schema i så här fungerar för ExpressRoute-kretsar över flera prenumerationer.

![Anslutning över prenumerationer](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Varje hello mindre moln inom hello stora moln är används toorepresent prenumerationer som tillhör toodifferent avdelningar inom en organisation.
- Var och en av hello avdelningar inom hello organisation kan använda sina egna prenumeration för att distribuera sina tjänster, men de kan dela ett ExpressRoute-kretsen tooconnect tillbaka tooyour lokalt nätverk.
- En avdelning (i det här exemplet: IT) kan äga hello ExpressRoute-kretsen. Andra prenumerationer i hello organisation kan använda hello ExpressRoute-kretsen.

    > [!NOTE]
    > Anslutningar och bandbredd kostnader för dedikerade hello krets kommer att tillämpas toohello ExpressRoute-kretsen ägare. Alla virtuella nätverk dela hello samma bandbredd.
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a>Administration - krets ägare och krets användare

hello 'kretsägaren' är en behörig Privilegierade användare i hello ExpressRoute-kretsen resurs. Hej kretsägaren kan skapa tillstånd som kan lösas av krets-användare. Kretsen användare är ägare till virtuella nätverk gateways som inte är inom hello samma prenumeration som hello ExpressRoute-kretsen. Kretsen användare kan lösa tillstånd (en auktorisering per virtuellt nätverk).

Hej kretsägaren har hello power toomodify samt återkalla tillstånd när som helst. Återkalla ett tillstånd som resulterar i alla anslutningar tas bort från hello prenumeration vars åtkomst har återkallats.

### <a name="circuit-owner-operations"></a>Kretsen ägare åtgärder

**toocreate en anslutningsverifiering**

Hej kretsägaren skapar tillstånd. Detta resulterar i hello skapandet av auktoriseringsnyckel som kan användas av en krets användaren tooconnect sina virtuella nätverk gateways toohello ExpressRoute-kretsen. Ett tillstånd som är giltig för endast en anslutning.

1. Hej ExpressRoute-bladet, klickar du på **tillstånd** och skriv sedan ett **namn** för hello auktorisering och klickar på **spara**.

    ![Tillstånd](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. Kopiera hello när hello konfiguration sparas **resurs-ID** och hello **Auktoriseringsnyckeln**.

    ![Auktoriseringsnyckeln](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**toodelete en anslutningsverifiering**

Du kan ta bort en anslutning genom att välja hello **ta bort** ikon på hello bladet för din anslutning.

### <a name="circuit-user-operations"></a>Kretsen användaren åtgärder

Hej kretsanvändare behöver hello resurs-ID och auktoriseringsnyckel från hello kretsägaren. 

**tooredeem en anslutningsverifiering**

1.  Klicka på hello **+ ny** knappen.

    ![Klicka på ny](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  Sök efter **”anslutning”** i hello Marketplace, markerar du den och klickar på **skapa**.

    ![Sök efter anslutning](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  Se till att hello **anslutningstypen** anges för ”ExpressRoute”.


4.  Fyll i hello information och klicka sedan på **OK** hello grunderna-bladet.

    ![Bladet Grundläggande inställningar](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  I hello **inställningar** bladet, Välj hello **virtuell nätverksgateway** och kontrollera hello **lösa auktorisering** kryssrutan.

6.  Ange hello **auktoriseringsnyckeln** och hello **Peer-krets URI** och namnge hello-anslutning. Klicka på **OK**.

    ![Bladet Inställningar](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. Studera hello hello **sammanfattning** bladet och klicka på **OK**.


**toorelease en anslutningsverifiering**

Du kan släppa tillstånd genom att ta bort hello-anslutning som länkar hello ExpressRoute-kretsen toohello virtuellt nätverk.

## <a name="next-steps"></a>Nästa steg
Mer information om ExpressRoute finns hello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).
