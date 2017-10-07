---
title: "Skapa och ändra en ExpressRoute-krets: Azure-portalen | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate, etablera, verifiera, uppdatera, ta bort och ta bort etableringen av en ExpressRoute-krets."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/07/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: 200418aed6bdebace43a2f57cf532d1c8d13cb83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a>Skapa och ändra en ExpressRoute-krets
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video - Azure-portalen](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassisk)](expressroute-howto-circuit-classic.md)
>

Den här artikeln beskriver hur toocreate Azure ExpressRoute-kretsen med hjälp av hello Azure-portalen och hello Azure Resource Manager-distributionsmodellen. hello också följande steg visar hur toocheck hello kretsstatusen hello, uppdatera eller ta bort och ta bort etableringen av den.


## <a name="before-you-begin"></a>Innan du börjar
* Granska hello [krav](expressroute-prerequisites.md) och [arbetsflöden](expressroute-workflows.md) innan du börjar konfigurera.
* Kontrollera att du har åtkomst toohello [Azure-portalen](https://portal.azure.com).
* Se till att du har behörigheter toocreate nya nätverksresurser. Kontakta din kontoadministratör om du inte har rätt behörigheter för hello.
* Du kan [visa en video](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) innan du börjar i ordning toobetter förstå hello steg.

## <a name="create-and-provision-an-expressroute-circuit"></a>Skapa och etablera en ExpressRoute-krets
### <a name="1-sign-in-toohello-azure-portal"></a>1. Logga in toohello Azure-portalen
Från en webbläsare, navigerar du toohello [Azure-portalen](http://portal.azure.com) och logga in med ditt Azure-konto.

### <a name="2-create-a-new-expressroute-circuit"></a>2. Skapa en ny ExpressRoute-krets
> [!IMPORTANT]
> ExpressRoute-krets kommer att debiteras från hello tidpunkt då en nyckel har utfärdats. Se till att utföra den här åtgärden när hello anslutningen providern är klar tooprovision hello krets.
> 
> 

1. Du kan skapa en ExpressRoute-krets genom att välja hello alternativet toocreate en ny resurs. Klicka på **ny** > **nätverk** > **ExpressRoute**, enligt följande bild hello:
   
    ![Skapa en ExpressRoute-krets](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. När du klickar på **ExpressRoute**, ser du hello **skapa ExpressRoute-krets** bladet. När du fyller i hello värden på det här bladet kan du se till att du anger hello rätt SKU-nivå och datamätning.
   
   * **Nivån** bestämmer om en ExpressRoute-standard eller premium-tillägg ett ExpressRoute är aktiverat. Du kan ange **Standard** tooget hello standard SKU eller **Premium** för hello premium.
   * **Datamätning** anger hello fakturering typ. Du kan ange **Metered** för en plan för förbrukade data och **obegränsad** för en obegränsad plan. Observera att du kan ändra hello fakturering typen från **Metered** för**obegränsad**, men du kan inte ändra hello typen från **obegränsad** för**Metered**.
     
     ![Konfigurera hello SKU-nivå och datamätning](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> Tänk på att hello Peering plats anger hello [fysisk plats](expressroute-locations.md) där du peering med Microsoft. Detta är **inte** länkade för ”plats”-egenskap som refererar toohello geografi där hello Azure Nätverksresursprovidern finns. När de inte är relaterade är det en bra idé toochoose en Nätverksresursprovidern geografiskt nära toohello Peering-platsen för hello krets. 
> 
> 

### <a name="3-view-hello-circuits-and-properties"></a>3. Visa hello kretsar och egenskaper
**Visa alla hello kretsar**

Du kan visa alla hello kretsar som du skapade genom att välja **alla resurser** på hello vänster-menyn.

![Visa kretsar](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Visa hello egenskaper**

    You can view hello properties of hello circuit by selecting it. On this blade, note hello service key for hello circuit. You must copy hello circuit key for your circuit and pass it down toohello service provider toocomplete hello provisioning process. hello circuit key is specific tooyour circuit.

![Visa egenskaper](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-hello-service-key-tooyour-connectivity-provider-for-provisioning"></a>4. Skicka hello-nyckel tooyour anslutning leverantör för etablering
På det här bladet **Providerstatus** innehåller information om hello aktuell status för etablering på hello-leverantör sida. **Kretsen status** innehåller hello tillstånd om hello Microsoft sida. Mer information om krets etablering tillstånd finns hello [arbetsflöden](expressroute-workflows.md#expressroute-circuit-provisioning-states) artikel.

När du skapar en ny ExpressRoute-krets blir hello krets i hello följande tillstånd:

Providerstatus: inte etablerats<BR>
Kretsen status: aktiverat

![Initiera etableringsprocessen](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

hello krets ändras toohello efter tillstånd när hello anslutningen leverantören är i hello innebär att aktivera den åt dig:

Providerstatus: etablering<BR>
Kretsen status: aktiverat

Om du toobe kan toouse en ExpressRoute-krets, måste den finnas i hello följande tillstånd:

Providerstatus: etablerats<BR>
Kretsen status: aktiverat

### <a name="5-periodically-check-hello-status-and-hello-state-of-hello-circuit-key"></a>5. Kontrollera regelbundet hello status och hello tillståndet för hello krets nyckel
Du kan visa hello egenskaper för hello kretsen som du är intresserad av genom att välja den. Kontrollera hello **Providerstatus** och se till att den har flyttats för**etablerad** innan du fortsätter.

![Kretsen och providern status](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. Skapa din routningskonfiguration
Stegvisa instruktioner finns i toohello [ExpressRoute-krets routningskonfiguration](expressroute-howto-routing-portal-resource-manager.md) artikel toocreate och ändra krets peerkopplingar.

> [!IMPORTANT]
> Dessa anvisningar gäller endast toocircuits som skapas med leverantörer som erbjuder layer 2 tjänster. Om du använder en tjänstprovider som erbjuder Hanterade layer 3-tjänster (vanligtvis en IP VPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.
> 
> 

### <a name="7-link-a-virtual-network-tooan-expressroute-circuit"></a>7. Länka ett virtuellt nätverk tooan ExpressRoute-krets
Därefter länka ett virtuellt nätverk tooyour ExpressRoute-kretsen. Använd hello [länka virtuella nätverk tooExpressRoute kretsar](expressroute-howto-linkvnet-arm.md) artikeln när du arbetar med hello Resource Manager-modellen.

## <a name="getting-hello-status-of-an-expressroute-circuit"></a>Hämta hello status för en ExpressRoute-krets
Du kan visa hello status för en krets genom att välja den. 

![Status för en ExpressRoute-krets](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a>Ändra en ExpressRoute-krets
Du kan ändra vissa egenskaper för en ExpressRoute-krets utan att påverka anslutningen.

Du kan göra hello följande utan avbrott:

* Aktivera eller inaktivera tillägget ExpressRoute premium för ExpressRoute-kretsen.
* Öka hello bandbredden för ExpressRoute-krets såvida det inte finns tillgänglig kapacitet på hello port. Observera att nedgradera hello bandbredden för en krets inte stöds. 
* Ändra hello avläsning plan från förbrukade Data tooUnlimited Data. Observera att ändra hello avläsning plan från obegränsad tooMetered Data inte stöds.
* Du kan aktivera och inaktivera *Tillåt klassiska åtgärder*.

Mer information om gränser och begränsningar finns i toohello [ExpressRoute vanliga frågor och svar](expressroute-faqs.md).

toomodify en ExpressRoute-krets, klicka på hello **Configuration** enligt hello bilden nedan.

![Ändra krets](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

Du kan ändra hello bandbredd, SKU och fakturering modellen och tillåter klassiska åtgärder i hello configuration bladet.

> [!IMPORTANT]
> Du kan ha toorecreate hello ExpressRoute-krets om det finns för lite kapacitet på befintlig hello-port. Du kan inte uppgradera hello krets om det finns inga ytterligare kapacitet på den platsen.
>
> Du kan minska hello bandbredden för en ExpressRoute-krets utan avbrott. Nedgradera bandbredd kräver toodeprovision hello ExpressRoute-krets och etablera en ny ExpressRoute-krets.
> 
> Inaktivera premium-tillägg kan misslyckas om du använder resurser som är större än vad som tillåts för standard hello-kretsen.
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Borttagning och tar bort en ExpressRoute-krets
Du kan ta bort ExpressRoute-krets genom att välja hello **ta bort** ikon. Observera följande hello:

* Du måste ta bort länken alla virtuella nätverk från hello ExpressRoute-kretsen. Om den här åtgärden misslyckas, kontrollera om något virtuellt nätverk är kopplade toohello krets.
* Om hello ExpressRoute-kretsen tjänstleverantör Etableringsstatus **etablering** eller **etablerad** du måste arbeta med service provider toodeprovision hello kretsen på sidan. Kommer att fortsätta tooreserve resurser och debitera dig tills hello-leverantör har slutförts avställningsskript hello-krets och meddela oss.
* Om hello-leverantör har avetableras hello krets (hello-leverantör Etableringsstatus har angetts för**inte etablerats**) du kan sedan ta bort hello krets. Detta förhindrar att faktureringen för hello-krets

## <a name="next-steps"></a>Nästa steg
Kontrollera att du hello följande när du har skapat kretsen:

* [Skapa och ändra routning för ExpressRoute-krets](expressroute-howto-routing-portal-resource-manager.md)
* [Länka ditt virtuella nätverk tooyour ExpressRoute-krets](expressroute-howto-linkvnet-arm.md)

