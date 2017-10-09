---
title: "aaaMoving ExpressRoute-kretsar från klassiska tooResource Manager | Microsoft Docs"
description: "Den här sidan innehåller en översikt över vad du behöver tooknow om bryggning hello klassiska och hello Resource Manager distributionsmodellerna."
documentationcenter: na
services: expressroute
author: ganesr
manager: carmonm
editor: 
ms.assetid: bdf01217-1a98-4ec0-a08e-d84fd37f78af
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: ganesr
ms.openlocfilehash: c901d2cda01aec409b528d29fc937ac6afaea511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="moving-expressroute-circuits-from-hello-classic-toohello-resource-manager-deployment-model"></a>Flytta ExpressRoute-kretsar från hello klassiska toohello Resource Manager-distributionsmodellen
Den här artikeln innehåller en översikt över vad det innebär att toomove en Azure ExpressRoute-krets från hello klassiska toohello Azure Resource Manager-distributionsmodellen.

Du kan använda en enda toovirtual nätverk för ExpressRoute-kretsen tooconnect som distribueras både i hello klassiska och hello Resource Manager distributionsmodellerna. En ExpressRoute-krets, oavsett hur den har skapats, kan nu koppla toovirtual nätverk över båda distributionsmodellerna.

![En ExpressRoute-krets som länkar toovirtual nätverk över båda distributionsmodellerna](./media/expressroute-move/expressroute-move-1.png)

## <a name="expressroute-circuits-that-are-created-in-hello-classic-deployment-model"></a>ExpressRoute-kretsar som skapas i hello klassiska distributionsmodellen
ExpressRoute-kretsar som skapas i hello klassiska distributionsmodellen måste toobe flyttas toohello Resource Manager distribution modellen första tooenable anslutning tooboth hello klassisk och hello Resource Manager distributionsmodellerna. Ingen anslutningsförlust eller avbrott sker när en anslutning flyttas. Alla krets-till-virtuell nätverkslänkar i hello klassiska distributionsmodellen (hello inom samma prenumeration och över prenumerationer) bevaras.

När hello flyttningen är klar hello ExpressRoute-kretsen ser ut, utför och känns precis som en ExpressRoute-krets som skapades i hello Resource Manager-distributionsmodellen. Du kan nu skapa anslutningar toovirtual nätverk i hello Resource Manager-distributionsmodellen.

När en ExpressRoute-krets har flyttats toohello Resource Manager-distributionsmodellen kan hantera du hello livscykel hello ExpressRoute-kretsen med hello Resource Manager-modellen. Det innebär att du kan utföra åtgärder som att lägga till/Uppdatera/ta bort peerkopplingar uppdaterar krets egenskaper (till exempel bandbredd, SKU och fakturering typen) och ta bort kretsar endast i hello Resource Manager-distributionsmodellen. Mer information finns i kretsar som skapas i hello Resource Manager-modellen för mer information om hur du kan hantera åtkomst tooboth distributionsmodeller toohello avsnittet nedan.

Du har inte tooinvolve din anslutning providern tooperform hello flytta.

## <a name="expressroute-circuits-that-are-created-in-hello-resource-manager-deployment-model"></a>ExpressRoute-kretsar som skapas i hello Resource Manager-distributionsmodellen
Du kan aktivera ExpressRoute-kretsar som skapas i hello Resource Manager distribution modellen toobe tillgänglig från båda distributionsmodellerna. Alla ExpressRoute-krets i din prenumeration kan vara aktiverad toobe nås från båda distributionsmodellerna.

* ExpressRoute-kretsar som har skapats i hello Resource Manager-modellen har inte åtkomst toohello klassiska distributionsmodellen som standard.
* ExpressRoute-kretsar som har flyttats från hello klassisk distribution modellen toohello Resource manager-distributionsmodellen är tillgänglig från båda distributionsmodellerna som standard.
* En ExpressRoute-krets har alltid åtkomst toohello Resource Manager-distributionsmodellen, oavsett om den har skapats i hello Resource Manager eller klassiska distributionsmodellen. Detta innebär att du kan skapa anslutningar toovirtual nätverk som har skapats i hello Resource Manager-modellen genom att följa instruktionerna på [hur toolink virtuella nätverk](expressroute-howto-linkvnet-arm.md).
* Åtkomst toohello klassiska distributionsmodellen styrs av hello **allowClassicOperations** parameter i hello ExpressRoute-kretsen.

> [!IMPORTANT]
> Alla kvoter som finns dokumenterade i hello [gränser](../azure-subscription-service-limits.md) sidan. Exempelvis kan en standard krets ha högst 10 virtuella länkar/nätverksanslutningar i både hello klassiska och hello Resource Manager distributionsmodellerna.
> 
> 

## <a name="controlling-access-toohello-classic-deployment-model"></a>Kontrollera åtkomst toohello klassiska distributionsmodellen
Du kan aktivera en enda ExpressRoute-kretsen toolink toovirtual nätverk i båda distributionsmodellerna genom att ange hello **allowClassicOperations** parametern för hello ExpressRoute-kretsen.

Ange **allowClassicOperations** tooTRUE kan du toolink virtuella nätverk från både distribution modeller toohello ExpressRoute-kretsen. Du kan koppla toovirtual nätverk i hello klassiska distributionsmodellen med följande riktlinjer på [hur toolink virtuella nätverk i hello klassiska distributionsmodellen](expressroute-howto-linkvnet-classic.md). Du kan länka toovirtual nätverk i hello Resource Manager-modellen genom följande riktlinjer på [hur toolink virtuella nätverk i hello Resource Manager-distributionsmodellen](expressroute-howto-linkvnet-arm.md).

Ange **allowClassicOperations** tooFALSE blockerar åtkomsten toohello krets från hello klassiska distributionsmodellen. Men bevaras alla virtuella nätverk länkar i hello klassiska distributionsmodellen. I det här fallet visas inte hello ExpressRoute-krets i hello klassiska distributionsmodellen.

## <a name="supported-operations-in-hello-classic-deployment-model"></a>Åtgärder som stöds i hello klassiska distributionsmodellen
följande klassiska operations hello stöds på en ExpressRoute circuit när **allowClassicOperations** anges tooTRUE:

* Hämta information om ExpressRoute-kretsen
* Skapa/uppdatera/get/ta bort virtuellt nätverk länkar tooclassic virtuella nätverk
* Skapa/uppdatera/hämta/ta bort auktoriseringar för virtuella nätverkslänkar till anslutningen mellan prenumerationer

Du kan inte utföra hello följande klassiska åtgärder när **allowClassicOperations** anges tooTRUE:

* Skapa/uppdatera/hämta/ta bort BGP-peerings (Border Gateway Protocol) för Azures privata, Azures offentliga och Microsoft-peerings
* Ta bort ExpressRoute-kretsar

## <a name="communication-between-hello-classic-and-hello-resource-manager-deployment-models"></a>Kommunikation mellan hello klassiska och hello Resource Manager distributionsmodellerna
Hej ExpressRoute-krets fungerar som en brygga mellan hello klassiska och hello Resource Manager distributionsmodellerna. Trafik mellan virtuella datorer i virtuella nätverk i hello klassiska distributionsmodellen och de i virtuella nätverk i hello Resource Manager distribution modellen flödar genom ExpressRoute om båda virtuella nätverken är länkade toohello samma ExpressRoute-kretsen.

Sammanställda genomflöde begränsas av hello genomflödeskapaciteten i hello virtuell nätverksgateway. Trafiken anger inte hello anslutningen providern nätverk eller ditt nätverk i sådana fall. Trafikflödet mellan virtuella nätverk för hello finns fullständigt i hello Microsoft-nätverk.

## <a name="access-tooazure-public-and-microsoft-peering-resources"></a>Åtkomst tooAzure offentliga och resurser för Microsoft-peering
Du kan fortsätta tooaccess resurser som är normalt tillgängliga via offentlig Azure-peering och Microsoft-peering utan störningar.  

## <a name="whats-supported"></a>Vad som stöds
I det här avsnittet beskrivs vad som stöds för ExpressRoute-kretsar:

* Du kan använda en enda ExpressRoute-kretsen tooaccess virtuella nätverk som har distribuerats i hello klassiska och hello Resource Manager distributionsmodellerna.
* Du kan flytta en ExpressRoute-krets från hello klassiska toohello Resource Manager-distributionsmodellen. När den flyttas hello ExpressRoute-krets verkar känns och utför som andra ExpressRoute-krets som skapas i hello Resource Manager-distributionsmodellen.
* Du kan flytta enbart hello ExpressRoute-kretsen. Kretslänkar, virtuella nätverk och VPN-gatewayer kan inte flyttas med den här åtgärden.
* När en ExpressRoute-krets har flyttats toohello Resource Manager-distributionsmodellen kan hantera du hello livscykel hello ExpressRoute-kretsen med hello Resource Manager-modellen. Det innebär att du kan utföra åtgärder som att lägga till/Uppdatera/ta bort peerkopplingar uppdaterar krets egenskaper (till exempel bandbredd, SKU och fakturering typen) och ta bort kretsar endast i hello Resource Manager-distributionsmodellen.
* Hej ExpressRoute-krets fungerar som en brygga mellan hello klassiska och hello Resource Manager distributionsmodellerna. Trafik mellan virtuella datorer i virtuella nätverk i hello klassiska distributionsmodellen och de i virtuella nätverk i hello Resource Manager distribution modellen flödar genom ExpressRoute om båda virtuella nätverken är länkade toohello samma ExpressRoute-kretsen.
* Anslutning över prenumerationer stöds både hello klassiska och hello Resource Manager distributionsmodellerna.
* När du flyttar en ExpressRoute-krets från hello klassiska modellen toohello Azure Resource Manager-modellen, kan du [migrera hello virtuella nätverk länkade toohello ExpressRoute-krets](expressroute-migration-classic-resource-manager.md).

## <a name="whats-not-supported"></a>Vad som inte stöds
I det här avsnittet beskrivs vad som inte stöds för ExpressRoute-kretsar:

* Hantera hello livscykeln för en ExpressRoute-krets hello klassiska distributionsmodellen.
* Rollbaserad åtkomstkontroll (RBAC) stöd för hello klassiska distributionsmodellen. Du kan inte utföra RBAC kontroller tooa krets i hello klassiska distributionsmodellen. En administratör/coadministrator hello prenumeration kan länka till eller Avlänka virtuella nätverk toohello krets.

## <a name="configuration"></a>Konfiguration
Följ instruktionerna för hello som beskrivs i [flytta en ExpressRoute-krets från hello klassiska toohello Resource Manager-distributionsmodellen](expressroute-howto-move-arm.md).

## <a name="next-steps"></a>Nästa steg
* [Migrera hello virtuella nätverk länkade toohello ExpressRoute-krets från hello klassiska modellen toohello Azure Resource Manager-modellen](expressroute-migration-classic-resource-manager.md)
* Arbetsflödesinformation finns i [ExpressRoute-kretsens etablering av arbetsflöden och kretsstatus](expressroute-workflows.md).
* tooconfigure ExpressRoute-anslutning:
  
  * [Skapa en ExpressRoute-krets](expressroute-howto-circuit-arm.md)
  * [Konfigurera routning](expressroute-howto-routing-arm.md)
  * [Länka ett virtuellt nätverk tooan ExpressRoute-krets](expressroute-howto-linkvnet-arm.md)

