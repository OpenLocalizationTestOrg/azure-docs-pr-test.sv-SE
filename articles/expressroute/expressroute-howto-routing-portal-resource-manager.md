---
title: "Hur tooconfigure routning (peering) för en ExpressRoute-krets: hanteraren för filserverresurser: Azure | Microsoft Docs"
description: "Den här artikeln vägleder dig genom hello steg för att skapa och etablera hello private, offentlig och Microsoft-peering i ExpressRoute-kretsen. Den här artikeln beskriver också hur toocheck hello status, uppdatera eller ta bort peerkopplingar för kretsen."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: a8ca25f488dde728cb3b06cd2c91873f3069feaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Skapa och ändra peering för en ExpressRoute-krets

Den här artikeln hjälper dig att skapa och hantera routningskonfiguration för en ExpressRoute-krets i hello Resource Manager-distributionsmodellen med hjälp av hello Azure-portalen. Du kan också kontrollera hello status, uppdatera eller ta bort och ta bort etableringen av peerkopplingar för en ExpressRoute-krets. Om du vill toouse en annan metod toowork med kretsen, väljer du en artikel från hello följande lista:


## <a name="configuration-prerequisites"></a>Förutsättningar för konfiguration

* Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md) sidan hello [routningskrav](expressroute-routing.md) sida och hello [arbetsflöden](expressroute-workflows.md) sidan innan du börjar konfigurera.
* Du måste ha en aktiv ExpressRoute-krets. Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-portal-resource-manager.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter. Hej ExpressRoute-krets måste vara i ett etablerad och aktiverade tillstånd du toobe kan toorun hello-cmdlets i hello nästa avsnitt.
* Om du planerar toouse en delad nyckel/MD5-hash, vara säker på att toouse detta på båda sidor av hello tunnel och gränsen hello antal tecken tooa högst 25.

Dessa anvisningar gäller endast toocircuits som skapats med leverantörer som erbjuder tjänster nivå 2. Om du använder en tjänstprovider som erbjuder hanterade Layer 3-tjänster (vanligtvis en IPVPN, t.ex. MPLS), anslutningsleverantören konfigurerar och hanterar routning för dig. 

> [!IMPORTANT]
> Vi annonseras för närvarande inte peerkopplingar som konfigurerats av leverantörer via hello service management portal. Vi arbetar på att kunna aktivera den här funktionen snart. Kontrollera med leverantören innan du konfigurerar BGP peerkopplingar.
> 
> 

Du kan konfigurera en, två eller alla tre peerings (Azure privat, Azure offentlig och Microsoft) för en ExpressRoute-krets. Du kan konfigurera peerings i valfri ordning. Du måste dock se till att du har slutfört hello konfigurationen för varje peering en i taget.

## <a name="azure-private-peering"></a>Azures privata peering

Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure privat peering-konfiguration för en ExpressRoute-krets.

### <a name="toocreate-azure-private-peering"></a>toocreate privat Azure-peering

1. Konfigurera hello ExpressRoute-kretsen. Se till att hello kretsen är helt etablerad av hello anslutningen providern innan du fortsätter.

  ![lista](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Konfigurera privat Azure-peering för hello krets. Kontrollera att du har följande objekt innan du fortsätter med nästa steg i hello hello:

  * Ett/30-undernät för hello primära länken. hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.
  * Ett/30-undernät för hello sekundära länken. hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.
  * Ett giltigt VLAN-ID-tooestablish denna peering på. Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.
  * AS-tal för peering. Du kan använda både 2 byte och 4 byte som AS-tal. Du kan använda ett privat AS-tal för den här peeringen. Se till att du inte använder 65515.
  * **Valfritt -** en MD5-hash om du väljer toouse en.
3. Välj hello Azure privat peering-raden som visas i följande exempel hello:

  ![privata](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Konfigurera privat peering. hello följande bild visar ett Konfigurationsexempel på:

  ![Konfigurera privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Spara hello konfigurationen när du har angett alla parametrar. När hello konfiguration har tagits emot, kan du se något liknande toohello följande exempel:

  ![Spara privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooview-azure-private-peering-details"></a>tooview Azure privat peering information

Du kan visa hello egenskaper för privat Azure-peering genom att välja hello peering.

![Visa privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate Azure privat peering-konfiguration

Du kan välja hello raden för peering och ändra hello peering egenskaper.

![Uppdatera privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="toodelete-azure-private-peering"></a>toodelete privat Azure-peering

Du kan ta bort peering konfigurationen genom att välja ikonen Ta bort hello enligt följande bild hello:

![ta bort privat peering](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a>Azures offentliga peering

Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure offentlig peering-konfiguration för en ExpressRoute-krets.

### <a name="toocreate-azure-public-peering"></a>toocreate offentlig Azure-peering

1. Konfigurera ExpressRoute-kretsen. Se till att hello kretsen är helt etablerad av hello anslutningen providern innan du fortsätter ytterligare.

  ![Visa en lista med offentlig peering](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Konfigurera offentlig Azure-peering för hello krets. Kontrollera att du har följande objekt innan du fortsätter med nästa steg i hello hello:

  * Ett/30-undernät för hello primära länken. Detta måste vara ett giltigt offentligt IPv4-prefix.
  * Ett/30-undernät för hello sekundära länken. Detta måste vara ett giltigt offentligt IPv4-prefix.
  * Ett giltigt VLAN-ID-tooestablish denna peering på. Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.
  * AS-tal för peering. Du kan använda både 2 byte och 4 byte som AS-tal.
  * **Valfritt -** en MD5-hash om du väljer toouse en.
3. Välj hello Azure offentlig peering-raden som visas i följande bild hello:

  ![Välj offentlig peering rad](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Konfigurera offentlig peering. hello följande bild visar ett Konfigurationsexempel på:

  ![Konfigurera offentlig peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Spara hello konfigurationen när du har angett alla parametrar. När hello konfiguration har tagits emot, kan du se något liknande toohello följande exempel:

  ![Spara offentlig peering konfiguration](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooview-azure-public-peering-details"></a>tooview Azure offentlig peering information

Du kan visa hello egenskaper för offentlig Azure-peering genom att välja hello peering.

![Visa offentlig peering egenskaper](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="tooupdate-azure-public-peering-configuration"></a>tooupdate Azure offentlig peering-konfiguration

Du kan välja hello raden för peering och ändra hello peering egenskaper.

![Välj offentlig peering rad](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="toodelete-azure-public-peering"></a>toodelete offentlig Azure-peering

Du kan ta bort peering konfigurationen genom att välja ikonen Ta bort hello som visas i följande exempel hello:

![ta bort offentlig peering](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a>Microsoft-peering

Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Microsoft peering konfiguration för en ExpressRoute-krets.

> [!IMPORTANT]
> Microsoft-peering i ExpressRoute-kretsar som har konfigurerats tidigare tooAugust 1, 2017 kommer ha alla service prefix annonserade via Microsoft hello-peering, även om filter routning inte har definierats. Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonserade tills ett flöde filter ansluts toohello krets. Mer information finns i [konfigurera route filter för Microsoft-peering](how-to-routefilter-powershell.md).
> 
> 

### <a name="toocreate-microsoft-peering"></a>toocreate Microsoft-peering

1. Konfigurera ExpressRoute-kretsen. Se till att hello kretsen är helt etablerad av hello anslutningen providern innan du fortsätter ytterligare.

  ![Visa en lista över Microsoft-peering](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Konfigurera Microsoft-peering för hello krets. Kontrollera att du har hello följande information innan du fortsätter.

  * Ett/30-undernät för hello primära länken. Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.
  * Ett/30-undernät för hello sekundära länken. Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.
  * Ett giltigt VLAN-ID-tooestablish denna peering på. Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.
  * AS-tal för peering. Du kan använda både 2 byte och 4 byte som AS-tal.
  * Annonserade prefix: du måste ange en lista över alla prefix du planerar tooadvertise över hello BGP-sessionen. Endast offentliga IP-adressprefix accepteras. Om du planerar toosend en uppsättning prefix, kan du skicka en kommaavgränsad lista. Dessa prefix måste vara registrerade tooyou i en RIR / IRR.
  * **Valfritt -** kunden ASN: Om du är reklam prefix som inte är registrerade toohello peering som du kan ange hello som antalet toowhich som de har registrerats.
  * Routning namn i registret: Du kan ange hello RIR / IRR mot vilken hello som antalet och prefix har registrerats.
  * **Valfritt -** en MD5-hash om du väljer toouse en.
3. Du kan välja hello peering gärna tooconfigure, enligt hello följande exempel. Välj hello Microsoft peering rad.

  ![Välj Microsoft peering rad](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Konfigurera Microsoft-peering. hello följande bild visar ett Konfigurationsexempel på:

  ![Konfigurera Microsoft-peering](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Spara hello konfigurationen när du har angett alla parametrar.

  Om kretsen hämtar tooa 'Validation behövs-tillstånd (enligt hello bild), måste du öppna en stöd biljett tooshow bevis av äganderätten till hello prefix tooour supportteamet.

  ![Spara konfigurationen för Microsoft peering](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  Du kan öppna ett supportärende direkt från hello-portalen, enligt följande exempel hello:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. När hello konfiguration har tagits emot, kan du se något liknande toohello följande bild:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="tooview-microsoft-peering-details"></a>tooview Microsoft peering information

Du kan visa hello egenskaper för offentlig Azure-peering genom att välja hello peering.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="tooupdate-microsoft-peering-configuration"></a>tooupdate Microsoft peering-konfiguration

Du kan välja hello raden för peering och ändra hello peering egenskaper.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="toodelete-microsoft-peering"></a>toodelete Microsoft-peering

Du kan ta bort peering konfigurationen genom att välja ikonen Ta bort hello enligt följande bild hello:

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a>Nästa steg

Nästa steg [länka VNet-tooan ExpressRoute-krets](expressroute-howto-linkvnet-portal-resource-manager.md)
* Mer information om ExpressRoute-arbetsflöden finns i [ExpressRoute-arbetsflöden](expressroute-workflows.md).
* Mer information om krets-peering finns i [ExpressRoute-kretsar och routningsdomäner](expressroute-circuit-peerings.md).
* Mer information om hur du arbetar med virtuella nätverk finns i [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).
