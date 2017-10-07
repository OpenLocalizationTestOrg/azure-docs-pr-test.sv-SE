---
title: "Hur tooconfigure routning (peering) för en ExpressRoute-krets: Azure: klassisk | Microsoft Docs"
description: "Den här artikeln vägleder dig genom hello steg för att skapa och etablera hello private, offentlig och Microsoft-peering i ExpressRoute-kretsen. Den här artikeln beskriver också hur toocheck hello status, uppdatera eller ta bort peerkopplingar för kretsen."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: a4bd39d2-373a-467a-8b06-36cfcc1027d2
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: dc5bcc4b86c3bc965a88281b6c2a660f3bc58eb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a>Skapa och ändra peering för en ExpressRoute-krets (klassisk)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Video - privat peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - offentlig peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [Video - Microsoft-peering](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klassisk)](expressroute-howto-routing-classic.md)
> 

Den här artikeln vägleder dig genom hello steg toocreate och hantera routningskonfiguration för en ExpressRoute-krets med PowerShell och hello klassiska distributionsmodellen. hello stegen nedan visas också hur toocheck hello status, uppdatera, eller ta bort och ta bort etableringen av peerkopplingar för en ExpressRoute-krets.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Om distributionsmodeller för Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a>Förutsättningar för konfiguration
* Du behöver hello senaste versionen av hello Azure Service Management (SM) PowerShell-cmdlets. Mer information finns i [komma igång med Azure PowerShell-cmdlets](/powershell/azure/overview).  
* Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md) sidan hello [routningskrav](expressroute-routing.md) sida och hello [arbetsflöden](expressroute-workflows.md) sidan innan du börjar konfigurera.
* Du måste ha en aktiv ExpressRoute-krets. Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-classic.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter. Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd du toobe kan toorun hello-cmdlets som beskrivs nedan.

> [!IMPORTANT]
> Dessa anvisningar gäller endast toocircuits som skapats med leverantörer som erbjuder tjänster nivå 2. Om du använder en Internet-leverantör som erbjuder hanteringstjänster till Layer 3 (vanligtvis en IPVPN, t.ex. MPLS) kommer anslutningsleverantören konfigurera och hantera routning för dig.
> 
> 

Du kan konfigurera en, två eller alla tre peerings (Azure privat, Azure offentlig och Microsoft) för en ExpressRoute-krets. Du kan konfigurera peerings i valfri ordning. Du måste dock se till att du har slutfört hello konfigurationen för varje peering en i taget.


### <a name="log-in-tooyour-azure-account-and-select-a-subscription"></a>Logga in tooyour Azure-konto och välj en prenumeration
1. Öppna PowerShell-konsol med utökade behörigheter och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

        Login-AzureRmAccount

2. Kontrollera hello prenumerationer för hello-kontot.

        Get-AzureRmSubscription

3. Om du har mer än en prenumeration väljer du hello prenumeration som du vill toouse.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. Använd sedan följande cmdlet tooadd hello tooPowerShell din Azure-prenumeration för hello klassiska distributionsmodellen.

        Add-AzureAccount


## <a name="azure-private-peering"></a>Azures privata peering
Det här avsnittet innehåller instruktioner om hur toocreate, hämta, uppdatera och ta bort hello Azure privat peering-konfiguration för en ExpressRoute-krets. 

### <a name="toocreate-azure-private-peering"></a>toocreate privat Azure-peering
1. **Importera hello PowerShell-modulen för ExpressRoute.**
   
    Du måste importera hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen i ordning toostart med hello ExpressRoute-cmdletar. Kör hello följande kommandon tooimport hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen.  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **Skapa en ExpressRoute-krets.**
   
    Följ hello instruktioner toocreate en [ExpressRoute-krets](expressroute-howto-circuit-classic.md) och har etablerats av hello anslutning providern. Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig. Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt. Men om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets Följ hello anvisningarna nedan. 
3. **Kontrollera hello ExpressRoute-kretsen tooensure den har etablerats.**
   
    Du måste först kontrollera toosee om hello ExpressRoute-kretsen är etablerad och aktiverats. Se hello exemplet nedan.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Kontrollera att kretsen hello visas som etablerad och aktiverad. Om det inte fungera med din anslutning providern tooget din krets toohello krävs tillstånd och status.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Konfigurera privat Azure-peering för hello krets.**
   
    Kontrollera att du har följande objekt innan du fortsätter med nästa steg i hello hello:
   
   * Ett/30-undernät för hello primära länken. Detta får inte vara en del av något adressutrymme som reserverats för virtuella nätverk.
   * Ett/30-undernät för hello sekundära länken. Detta får inte vara en del av något adressutrymme som reserverats för virtuella nätverk.
   * Ett giltigt VLAN-ID-tooestablish denna peering på. Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.
   * AS-tal för peering. Du kan använda både 2 byte och 4 byte som AS-tal. Du kan använda ett privat AS-tal för den här peeringen. Se till att du inte använder 65515.
   * En MD5-hash om du väljer toouse en. **Det här är valfritt**.
     
    Du kan köra följande cmdlet tooconfigure privat Azure-peering för kretsen hello.
     
        Nya AzureBGPPeering - AccessType privat - ServiceKey ”***” - PrimaryPeerSubnet ”10.0.0.0/30” - SecondaryPeerSubnet ”10.0.0.4/30” - PeerAsn 1234 - VlanId 100
     
    Du kan använda hello-cmdlet nedan om du väljer toouse en MD5-hash.
     
        Nya AzureBGPPeering - AccessType privat - ServiceKey ”***” - PrimaryPeerSubnet ”10.0.0.0/30” - SecondaryPeerSubnet ”10.0.0.4/30” - PeerAsn 1234 - VlanId 100 - SharedKey ”A1B2C3D4”
     
     > [!IMPORTANT]
     > Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.
     > 
     > 

### <a name="tooview-azure-private-peering-details"></a>tooview Azure privat peering information
Du kan hämta konfigurationsinformation med följande cmdlet hello

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="tooupdate-azure-private-peering-configuration"></a>tooupdate Azure privat peering-konfiguration
Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande cmdlet. I hello exemplet nedan uppdateras hello VLAN-ID för hello krets från 100 too500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="toodelete-azure-private-peering"></a>toodelete privat Azure-peering
Du kan ta bort peering konfigurationen genom att köra följande cmdlet hello.

> [!WARNING]
> Du måste se till att alla virtuella nätverk avlänkas från hello ExpressRoute-krets innan du kör denna cmdlet. 
> 
> 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azures offentliga peering
Det här avsnittet innehåller instruktioner om hur toocreate, hämta, uppdatera och ta bort hello Azure offentlig peering-konfiguration för en ExpressRoute-krets.

### <a name="toocreate-azure-public-peering"></a>toocreate offentlig Azure-peering
1. **Importera hello PowerShell-modulen för ExpressRoute.**
   
    Du måste importera hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen i ordning toostart med hello ExpressRoute-cmdletar. Kör hello följande kommandon tooimport hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen. 
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **Skapa en ExpressRoute-krets**
   
    Följ hello instruktioner toocreate en [ExpressRoute-krets](expressroute-howto-circuit-classic.md) och har etablerats av hello anslutning providern. Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure offentlig peering åt dig. Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt. Men om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets Följ hello anvisningarna nedan.
3. **Kontrollera ExpressRoute-kretsen tooensure den har etablerats**
   
    Du måste först kontrollera toosee om hello ExpressRoute-kretsen är etablerad och aktiverats. Se hello exemplet nedan.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Kontrollera att kretsen hello visas som etablerad och aktiverad. Om det inte fungera med din anslutning providern tooget din krets toohello krävs tillstånd och status.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Konfigurera offentlig Azure-peering för hello-krets**
   
    Se till att du har följande information innan du fortsätter ytterligare hello.
   
   * Ett/30-undernät för hello primära länken. Detta måste vara ett giltigt offentligt IPv4-prefix.
   * Ett/30-undernät för hello sekundära länken. Detta måste vara ett giltigt offentligt IPv4-prefix.
   * Ett giltigt VLAN-ID-tooestablish denna peering på. Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.
   * AS-tal för peering. Du kan använda både 2 byte och 4 byte som AS-tal.
   * En MD5-hash om du väljer toouse en. **Det här är valfritt**.
     
    Du kan köra följande cmdlet tooconfigure offentlig Azure-peering för kretsen hello
     
        Nya AzureBGPPeering - AccessType offentliga - ServiceKey ”***” - PrimaryPeerSubnet ”131.107.0.0/30” - SecondaryPeerSubnet ”131.107.0.4/30” - PeerAsn 1234 - VlanId 200
     
    Du kan använda hello-cmdlet nedan om du väljer toouse en MD5-hash
     
        Nya AzureBGPPeering - AccessType offentliga - ServiceKey ”***” - PrimaryPeerSubnet ”131.107.0.0/30” - SecondaryPeerSubnet ”131.107.0.4/30” - PeerAsn 1234 - VlanId 200 - SharedKey ”A1B2C3D4”
     
     > [!IMPORTANT]
     > Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.
     > 
     > 

### <a name="tooview-azure-public-peering-details"></a>tooview Azure offentlig peering information
Du kan hämta konfigurationsinformation med följande cmdlet hello

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="tooupdate-azure-public-peering-configuration"></a>tooupdate Azure offentlig peering-konfiguration
Du kan uppdatera någon del av hello-konfigurationen med hjälp av följande cmdlet hello

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

hello VLAN-ID för hello krets uppdateras från 200 too600 i hello ovan exempel.

### <a name="toodelete-azure-public-peering"></a>toodelete offentlig Azure-peering
Du kan ta bort peering konfigurationen genom att köra följande cmdlet hello

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft-peering
Det här avsnittet innehåller instruktioner om hur toocreate, hämta, uppdatera och ta bort hello Microsoft peering konfiguration för en ExpressRoute-krets. 

### <a name="toocreate-microsoft-peering"></a>toocreate Microsoft-peering
1. **Importera hello PowerShell-modulen för ExpressRoute.**
   
    Du måste importera hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen i ordning toostart med hello ExpressRoute-cmdletar. Kör hello följande kommandon tooimport hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen.  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. **Skapa en ExpressRoute-krets**
   
    Följ hello instruktioner toocreate en [ExpressRoute-krets](expressroute-howto-circuit-classic.md) och har etablerats av hello anslutning providern. Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig. Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt. Men om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets Följ hello anvisningarna nedan.
3. **Kontrollera ExpressRoute-kretsen tooensure den har etablerats**
   
    Du måste först kontrollera toosee om hello ExpressRoute-kretsen är etablerad och aktiverad.
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    Kontrollera att kretsen hello visas som etablerad och aktiverad. Om det inte fungera med din anslutning providern tooget din krets toohello krävs tillstånd och status.
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. **Konfigurera Microsoft-peering för hello-krets**
   
    Kontrollera att du har hello följande information innan du fortsätter.
   
   * Ett/30-undernät för hello primära länken. Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.
   * Ett/30-undernät för hello sekundära länken. Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.
   * Ett giltigt VLAN-ID-tooestablish denna peering på. Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.
   * AS-tal för peering. Du kan använda både 2 byte och 4 byte som AS-tal.
   * Annonserade prefix: du måste ange en lista över alla prefix du planerar tooadvertise över hello BGP-sessionen. Endast offentliga IP-adressprefix accepteras. Du kan skicka en kommaavgränsad lista om du planerar toosend en uppsättning prefix. Dessa prefix måste vara registrerade tooyou i en RIR / IRR.
   * Kund ASN: Om du annonserar prefix som inte är registrerade toohello peering som du kan ange hello som antalet toowhich som de har registrerats. **Det här är valfritt**.
   * Routning namn i registret: Du kan ange hello RIR / IRR mot vilken hello som antalet och prefix har registrerats.
   * En MD5-hash, om du väljer toouse en. **Det här är valfritt.**
     
    Du kan köra följande cmdlet tooconfigure Microsoft pering för kretsen hello
     
        Nya AzureBGPPeering - AccessType Microsoft - ServiceKey ”***” - PrimaryPeerSubnet ”131.107.0.0/30” - SecondaryPeerSubnet ”131.107.0.4/30” - VlanId 300 - PeerAsn 1234 - CustomerAsn 2245 - AdvertisedPublicPrefixes ”123.0.0.0/30” - RoutingRegistryName ”ARIN” - SharedKey ”A1B2C3D4”

### <a name="tooview-microsoft-peering-details"></a>tooview Microsoft peering information
Du kan hämta konfigurationsinformation med hello följande cmdlet.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="tooupdate-microsoft-peering-configuration"></a>tooupdate Microsoft peering-konfiguration
Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande cmdlet.

    Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="toodelete-microsoft-peering"></a>toodelete Microsoft-peering
Du kan ta bort peering konfigurationen genom att köra följande cmdlet hello.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Nästa steg
Nästa [länka VNet-tooan ExpressRoute-krets](expressroute-howto-linkvnet-classic.md).

* Mer information om arbetsflöden finns [ExpressRoute-arbetsflöden](expressroute-workflows.md).
* Mer information om krets-peering finns i [ExpressRoute-kretsar och routningsdomäner](expressroute-circuit-peerings.md).

