---
title: "aaaWorkflows för att konfigurera en ExpressRoute-krets | Microsoft Docs"
description: "Den här sidan vägleder dig genom hello arbetsflöden för att konfigurera ExpressRoute-krets och peerkopplingar"
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 55e0418c-e0bf-44a7-9aa1-720076df9297
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: cherylmc
ms.openlocfilehash: 8e1dfc137401e0d6d53608ae6c8de0085e182eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>Arbetsflöden i ExpressRoute för kretsetablering och kretstillstånd
Den här sidan vägleder dig genom hello service etablering och routning configuration arbetsflöden på en hög nivå.

![](./media/expressroute-workflows/expressroute-circuit-workflow.png)

hello följande bild och motsvarande steg visar hello uppgifter i ordning toohave måste du följa en ExpressRoute-kretsen etablerade slutpunkt till slutpunkt. 

1. Använd PowerShell tooconfigure en ExpressRoute-krets. Följ anvisningarna för hello i hello [skapa ExpressRoute-kretsar](expressroute-howto-circuit-classic.md) mer information.
2. Ordna anslutningen från hello-leverantören. Den här processen varierar. Kontakta anslutningsleverantören för mer information om hur tooorder anslutning.
3. Se till att hello kretsen har har etablerats genom att verifiera hello ExpressRoute-krets Etableringsstatus via PowerShell. 
4. Konfigurera routning domäner. Om anslutningsleverantören hanterar Layer 3 åt dig, kommer de att konfigurera routning för kretsen. Om anslutningsleverantören erbjuder endast nivå 2-tjänster, måste du konfigurera routning per riktlinjer som beskrivs i hello [routningskrav](expressroute-routing.md) och [routningskonfiguration](expressroute-howto-routing-classic.md) sidor.
   
   * Aktivera privat Azure-peering - du måste aktivera den här peering tooconnect tooVMs / molntjänster distribueras inom virtuella nätverk.
   * Aktivera Azure offentlig peering - du måste aktivera offentlig Azure-peering om du inte vill tooconnect tooAzure services på den offentliga IP-adresser. Detta är ett krav tooaccess Azure-resurser om du har valt tooenable standardroutning för privat Azure-peering.
   * Aktivera Microsoft-peering - du måste aktivera den här tooaccess Office 365 och Dynamics 365. 
     
     > [!IMPORTANT]
     > Du måste se till att du använder en separat proxyserver / edge tooconnect tooMicrosoft än hello som du använder för hello Internet. Med hjälp av hello samma kant för ExpressRoute- och hello Internet innebär orsaka asymmetriska Routning och anslutningen avbrott för ditt nätverk.
     > 
     > 
     
     ![](./media/expressroute-workflows/routing-workflow.png)
5. Länka virtuella nätverk tooExpressRoute kretsar – du kan länka virtuella nätverk tooyour ExpressRoute-kretsen. Följ anvisningarna [toolink Vnet](expressroute-howto-linkvnet-arm.md) tooyour krets. Dessa Vnet kan antingen vara i samma Azure-prenumeration som hello ExpressRoute-krets hello eller kan vara i en annan prenumeration.

## <a name="expressroute-circuit-provisioning-states"></a>ExpressRoute-krets etablering tillstånd
Varje ExpressRoute-kretsen har två lägen:

* Etableringsstatusen för service provider
* Status

Status representerar Microsofts etablering. Den här egenskapen anges tooEnabled när du skapar en Expressroute-krets

anslutningen hello providern etableringsstatusen representerar hello på hello anslutningen providern sida. Det kan antingen vara *NotProvisioned*, *etablering*, eller *etablerad*. Hej ExpressRoute-krets måste vara i etablerad tillstånd för du toobe kan toouse den.

### <a name="possible-states-of-an-expressroute-circuit"></a>Möjliga tillstånd för en ExpressRoute-krets
Det här avsnittet innehåller ut hello möjliga tillstånd för en ExpressRoute-krets.

**Vid skapandet**

Hej ExpressRoute-krets i hello följande tillstånd så snart som du kör hello PowerShell cmdlet toocreate hello ExpressRoute-krets visas.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**När anslutningen leverantören är i hello processen för etablering hello-krets**

Du ser hello ExpressRoute-krets i hello följande tillstånd så snart som du skickar hello tjänstleverantör viktiga toohello anslutning och de har startat hello etableringsprocessen.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**När anslutningen providern har slutförts hello etableringsprocessen**

Du ser hello ExpressRoute-krets i hello följande tillstånd när hello anslutningen providern har slutförts hello etableringsprocessen.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled

Etablerad och aktiverat är hello tillstånd hello kretsen kan vara i av du toobe kan toouse den. Om du använder en nivå 2-provider kan du konfigurera routning för kretsen endast när det är i det här tillståndet.

**När anslutningen provider avetablering hello-krets**

Om du har begärt hello service provider toodeprovision hello ExpressRoute-krets visas hello krets ange toohello följande tillstånd när hello-leverantör har slutförts hello avetablering process.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Du kan välja toore – aktivera det om behövs eller som kör PowerShell-cmdlets toodelete hello krets.  

> [!IMPORTANT]
> Om du kör misslyckas hello PowerShell cmdlet toodelete hello krets när hello ServiceProviderProvisioningState är etablering eller etablerad hello åtgärden. Se tillsammans med din anslutning providern toodeprovision hello ExpressRoute-krets först och ta sedan bort hello kretsen. Microsoft kommer att fortsätta toobill hello krets förrän du kör hello PowerShell cmdlet toodelete hello krets.
> 
> 

## <a name="routing-session-configuration-state"></a>Routning sessionstillstånd konfiguration
hello BGP etableringsstatusen får du reda på om hello BGP-sessionen har aktiverats på hello Microsoft edge. hello tillstånd måste vara aktiverat för du toobe kan toouse hello peering.

Det är viktigt toocheck hello BGP sessionstillstånd särskilt för Microsoft-peering. Tillägg toohello BGP etableringsstatusen, det finns ett annat tillstånd som kallas *annonserade offentliga prefix tillstånd*. hello annonserade offentliga prefix tillstånd måste vara i *konfigurerats* tillstånd, både för hello BGP-sessionen toobe dig och dina routning toowork slutpunkt till slutpunkt. 

Om hello annonserade offentliga prefix status är inställd tooa *validering krävs* tillstånd, hello BGP-sessionen inte är aktiverad, som hello annonserade prefix inte matchade hello som i någon av hello routning register. 

> [!IMPORTANT]
> Om hello annonserade offentliga prefix tillstånd *manuell verifiering* tillstånd, måste du öppna ett supportärende med [Microsoft-supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) och bevisa att du äger hello IP-adresser annonserade längs hello associeras autonomt systemnummer.
> 
> 

## <a name="next-steps"></a>Nästa steg
* Konfigurera ExpressRoute-anslutningen.
  
  * [Skapa en ExpressRoute-krets](expressroute-howto-circuit-arm.md)
  * [Konfigurera routning](expressroute-howto-routing-arm.md)
  * [Länka ett VNet tooan ExpressRoute-krets](expressroute-howto-linkvnet-arm.md)

