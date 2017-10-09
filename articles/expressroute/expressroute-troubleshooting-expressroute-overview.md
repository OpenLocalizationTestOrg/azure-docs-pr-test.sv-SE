---
title: "Verifiera anslutningsbarhet: Azure ExpressRoute felsökningsguide för | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för felsökning och verifiera slutet tooend anslutningen för en ExpressRoute-krets."
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 713c39c7eafd77a4380b2a91902a9686f2ce1d85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="verifying-expressroute-connectivity"></a>Verifiera en ExpressRoute-anslutning
ExpressRoute som utökar ett lokalt nätverk till hello Microsoft cloud över en privat anslutning som möjliggörs med hjälp av en provider för anslutningen, omfattar följande tre separata nätverkszoner hello:

-   Kundens nätverk
-   Providern nätverk
-   Microsoft-Datacenter

hello syftet med det här dokumentet är toohelp användaren tooidentify där (eller även om) ett anslutningsproblem finns och i vilken zon vilket tooseek hjälp från rätt team tooresolve hello problemet. Öppna ett supportärende med om Microsoft-supporten nödvändiga tooresolve problem [Microsoft-supporten][Support].

> [!IMPORTANT]
> Det här dokumentet är avsett toohelp diagnostisera och åtgärda problem med enkel. Det är inte avsedda toobe en ersättning för Microsoft-supporten. Öppna ett supportärende med [Microsoft-supporten] [ Support] om du toosolve hello problemet med hjälp av hello riktlinjer som tillhandahålls.
>
>

## <a name="overview"></a>Översikt
hello visar följande diagram hello logiska anslutningen för en kund nätverk tooMicrosoft nätverk med hjälp av ExpressRoute.
[![1]][1]

I föregående diagram hello, visar hello nummer viktiga nätverket punkter. hello nätverket punkter refereras ofta via den här artikeln av deras associerade nummer.

Beroende på hello ExpressRoute-anslutningen modellen (moln Exchange samordning, Point-to-Point Ethernet-anslutning eller alla-till-alla (IPVPN)) hello nätverket punkter 3 och 4 kan vara växlar (nivå 2-enheter). hello viktiga nätverket punkter visas är följande:

1.  Kunden beräkning enheten (exempelvis en server eller dator)
2.  CEs: Klientens gränsroutrar 
3.  Sämsta (CE inriktad): providern edge routrar/växlar som är riktade mot klientens gränsroutrar. Kallas tooas PE CEs i det här dokumentet.
4.  Sämsta (MSEE inriktad): providern edge routrar/växlar som är riktade mot MSEEs. Kallas tooas PE MSEEs i det här dokumentet.
5.  MSEEs: Microsoft Enterprise kant (MSEE) ExpressRoute routrar
6.  Gateway för virtuellt nätverk (VNet)
7.  Compute-enheten på hello Azure VNet

Om hello molnet Exchange samplacering eller Point-to-Point Ethernet-anslutning anslutningen modeller används skulle hello klientens gränsrouter (2) upprätta BGP-peering med MSEEs (5). Nätverket punkterna 3 och 4 skulle fortfarande finns men är något transparent som Lager2-enheter.

Om hello alla-till-alla (IPVPN) anslutningen mall används hello PEs (MSEE inriktad) (4) skulle upprätta BGP-peering med MSEEs (5). Vägar skulle sedan sprider tillbaka toohello kundens nätverk via hello IPVPN service provider nätverk.

>[!NOTE]
>Microsoft kräver ett redundant BGP-sessioner mellan MSEEs (5) och PE-MSEEs (4)-par för ExpressRoute med hög tillgänglighet. Ett redundant par nätverkssökvägar rekommenderas också mellan kundens nätverk och PE CEs. I modellen för alla-till-alla (IPVPN)-anslutning kanske en enskild CE-enhet (2) men anslutna tooone eller mer PEs (3).
>
>

toovalidate en ExpressRoute-krets hello följande steg beskrivs (med hello nätverkspunkt som anges av hello associerade nummer):
1. [Validera kretsetablering och tillstånd (5)](#validate-circuit-provisioning-and-state)
2. [Verifiera minst en ExpressRoute-peering är konfigurerad (5)](#validate-peering-configuration)
3. [Validera ARP mellan Microsoft och hello-leverantör (länk mellan 4 och 5)](#validate-arp-between-microsoft-and-the-service-provider)
4. [Validera BGP och vägar på hello MSEE (BGP mellan too5 4 och 5 too6 om ett virtuellt nätverk är ansluten)](#validate-bgp-and-routes-on-the-msee)
5. [Kontrollera hello trafik statistik (trafik som passerar genom 5)](#check-the-traffic-statistics)

Mer verifieringar och kontroller ska läggas till i hello framtida Kom tillbaka varje månad!

##<a name="validate-circuit-provisioning-and-state"></a>Validera kretsetablering och tillstånd
Oavsett hello anslutningen modellen har en ExpressRoute-krets toobe skapas och därmed en tjänst nyckeln genereras för kretsetablering. Etablera en ExpressRoute-krets upprättar en redundant nivå 2-anslutningar mellan PE-MSEEs (4) och MSEEs (5). Mer information om hur toocreate, ändra, konfigurera och verifiera en ExpressRoute-krets finns hello artikel [skapa och ändra en ExpressRoute-krets][CreateCircuit].

>[!TIP]
>Nyckeln för tjänsten identifierar en ExpressRoute-krets. Den här nyckeln är obligatorisk för de flesta hello powershell-kommandon som nämns i det här dokumentet. Dessutom bör du behöver hjälp från Microsoft eller från en ExpressRoute partner tootroubleshoot ExpressRoute problemet betjäna hello viktiga tooreadily identifiera hello krets.
>
>

###<a name="verification-via-hello-azure-portal"></a>Verifiering via hello Azure-portalen
I hello Azure-portalen, hello status för en ExpressRoute-krets kan kontrolleras genom att välja ![2][2] hello hello vänster sidorutan-menyn och sedan välja ExpressRoute-kretsen. Om du markerar en ExpressRoute öppnas krets som anges under ”alla resurser” hello ExpressRoute-kretsen bladet. I hello ![3][3] avsnitt i hello bladet hello ExpressRoute essentials listas som visas i följande skärmbild som visar hello:

![4][4]    

I hello ExpressRoute Essentials *krets status* anger hello kretsstatusen hello på hello Microsoft sida. *Providerstatus* visar om hello kretsen har *etablerad ej etablerad* på hello-leverantör sida. 

En ExpressRoute-kretsen toobe operativa, hello *krets status* måste vara *aktiverad* och hello *Providerstatus* måste vara *etablerad*.

>[!NOTE]
>Om hello *krets status* är inte aktiverad, kontakta [Microsoft-supporten][Support]. Om hello *Providerstatus* har inte etablerats kontakta tjänstleverantören.
>
>

###<a name="verification-via-powershell"></a>Verifiering via PowerShell
toolist alla hello ExpressRoute-kretsar i en resursgrupp, använder hello följande kommando:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
>Du kan hämta din resursgruppens namn via hello Azure-portalen. Se hello föregående avsnitt i det här dokumentet och Observera att hello resursgruppens namn anges i hello exempel skärmbild.
>
>

tooselect en viss ExpressRoute-krets i en resursgrupp, Använd hello följande kommando:

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

En exempelsvar är:

    Name                             : Test-ER-Ckt
    ResourceGroupName                : Test-ER-RG
    Location                         : westus2
    Id                               : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                        "Name": "Standard_UnlimitedData",
                                        "Tier": "Standard",
                                        "Family": "UnlimitedData"
                                        }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             : 
    ServiceProviderProperties        : {
                                        "ServiceProviderName": "****",
                                        "PeeringLocation": "******",
                                        "BandwidthInMbps": 100
                                        }
    ServiceKey                       : **************************************
    Peerings                         : []
    Authorizations                   : []

tooconfirm om en ExpressRoute-krets fungerar betala närmare toohello följande fält:

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

>[!NOTE]
>Om hello *CircuitProvisioningState* är inte aktiverad, kontakta [Microsoft-supporten][Support]. Om hello *ServiceProviderProvisioningState* har inte etablerats kontakta tjänstleverantören.
>
>

###<a name="verification-via-powershell-classic"></a>Verifiering via PowerShell (klassisk)
toolist alla hello ExpressRoute-kretsar under en prenumeration, använder hello följande kommando:

    Get-AzureDedicatedCircuit

tooselect en viss ExpressRoute-kretsen använder hello följande kommando:

    Get-AzureDedicatedCircuit -ServiceKey **************************************

En exempelsvar är:

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

tooconfirm om en ExpressRoute-krets fungerar betala närmare toohello följande fält: ServiceProviderProvisioningState: etablerats Status: aktiverat

>[!NOTE]
>Om hello *Status* är inte aktiverad, kontakta [Microsoft-supporten][Support]. Om hello *ServiceProviderProvisioningState* har inte etablerats kontakta tjänstleverantören.
>
>

##<a name="validate-peering-configuration"></a>Verifiera Peering konfiguration
När hello-leverantör har slutförts hello etablering hello ExpressRoute-krets, kan du skapa en routningskonfiguration över hello ExpressRoute-krets mellan MSEE fso (4) och MSEEs (5). Varje ExpressRoute-kretsen kan ha en, två eller tre routning kontexter aktiverad: privat Azure-peering (trafik tooprivate virtuella nätverk i Azure), offentlig Azure-peering (trafik toopublic IP-adresser i Azure) och Microsoft-peering (trafik tooOffice 365 och Dynamics 365). Mer information om hur toocreate och ändra konfigurationen för routning, se hello artikel [skapa och ändra routning för en ExpressRoute-krets][CreatePeering].

###<a name="verification-via-hello-azure-portal"></a>Verifiering via hello Azure-portalen
>[!IMPORTANT]
>Det finns ett känt fel i hello Azure-portalen där ExpressRoute peerkopplingar är *inte* visas i hello portal om hello-leverantör har konfigurerats. Lägga till ExpressRoute peerkopplingar via hello-portalen eller PowerShell *skriver över hello providern tjänstinställningar*. Den här åtgärden bryter hello routning på hello ExpressRoute-krets och kräver hello stöd av hello providern toorestore hello tjänstinställningar och återupprätta normal routning. Ändra bara hello ExpressRoute peerkopplingar om det är säkert att hello-leverantören tillhandahåller nivå 2-tjänster!
>
>

<p/>
>[!NOTE]
>Om nivå 3 har angetts av hello service provider och hello peerkopplingar är tomma i hello portal, vara PowerShell används toosee hello providern konfigurerade inställningar.
>
>

I hello Azure-portalen, status för en ExpressRoute-krets kan kontrolleras genom att välja ![2][2] hello hello vänster sidorutan-menyn och sedan välja ExpressRoute-kretsen. Att välja en ExpressRoute öppnas krets som anges under ”alla resurser” hello ExpressRoute-kretsen bladet. I hello ![3][3] avsnitt i hello bladet hello ExpressRoute essentials anges som visas i följande skärmbild som visar hello:

![5][5]

I föregående exempel hello, som noterats Azure är privat peering routning kontext aktiverad, medan Azure offentliga och Microsoft peering routning kontexter inte är aktiverade. En kontext som har aktiverats peering har också hello primära och sekundära point-to-point (krävs för BGP)-undernät i listan. Hej /30 undernät används för hello IP-gränssnitt hello MSEEs och PE MSEEs. 

>[!NOTE]
>Om en peering inte är aktiverat, kontrollera om hello primära och sekundära undernät som tilldelats matcha hello konfigurationen på PE MSEEs. Om inte, toochange hello konfiguration på MSEE routrar, se för[skapa och ändra routning för en ExpressRoute-krets][CreatePeering]
>
>

###<a name="verification-via-powershell"></a>Verifiering via PowerShell
tooget hello Azure privat peering konfigurationsinformation, Använd hello följande kommandon:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt

En exempelsvar för en har konfigurerats privat peering, är:

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : ####
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 300
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 En kontext som har aktiverats peering skulle ha hello primära och sekundära adressprefix i listan. Hej /30 undernät används för hello IP-gränssnitt hello MSEEs och PE MSEEs.

tooget hello Azure offentlig peering konfigurationsinformation, Använd hello följande kommandon:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt

tooget hello Microsoft peering konfigurationsinformation, Använd hello följande kommandon:

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

Om en peering inte har konfigurerats, skulle det finnas ett felmeddelande. Exempelsvar när hello anges peering (offentlig Azure-peering i det här exemplet) har inte konfigurerats i hello krets:

    Get-AzureRmExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzureRmExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
>Om en peering inte är aktiverad, kan du kontrollera om hello primära och sekundära undernät som tilldelats matchar hello konfigurationen på hello länkat PE MSEE. Även om hello korrigera *VlanId*, *AzureASN*, och *PeerASN* används på MSEEs och om du värdena mappar toohello som används på hello länkad PE MSEE. Om du väljer MD5-hash ska hello delad nyckel vara samma på MSEE och PE MSEE. toochange hello konfigurationen på hello MSEE routrar finns för [skapa och ändra routning för en ExpressRoute-krets] [CreatePeering].  
>
>

### <a name="verification-via-powershell-classic"></a>Verifiering via PowerShell (klassisk)
tooget hello Azure privat peering konfigurationsinformation, Använd hello följande kommando:

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

En exempelsvar för en har konfigurerats privat peering är:

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : ####
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100

A, aktiverats peering kontexten skulle ha hello primära och sekundära peer-undernät i listan. Hej /30 undernät används för hello IP-gränssnitt hello MSEEs och PE MSEEs.

tooget hello Azure offentlig peering konfigurationsinformation, Använd hello följande kommandon:

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

tooget hello Microsoft peering konfigurationsinformation, Använd hello följande kommandon:

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
>Om nivå 3 peerkopplingar ställts in av hello-leverantör, ange hello ExpressRoute peerkopplingar via hello-portalen eller PowerShell skriver över hello tjänstinställningar för providern. Återställer hello sida peering providerinställningar kräver hello stöd av hello-leverantör. Ändra bara hello ExpressRoute peerkopplingar om det är säkert att hello-leverantören tillhandahåller nivå 2-tjänster!
>
>

<p/>
>[!NOTE]
>Om en peering inte är aktiverad, kan du kontrollera om hello primära och sekundära peer-undernät som tilldelats matchar hello konfigurationen på hello länkat PE MSEE. Även om hello korrigera *VlanId*, *AzureAsn*, och *PeerAsn* används på MSEEs och om du värdena mappar toohello som används på hello länkad PE MSEE. toochange hello konfigurationen på hello MSEE routrar finns för [skapa och ändra routning för en ExpressRoute-krets] [CreatePeering].
>
>

## <a name="validate-arp-between-microsoft-and-hello-service-provider"></a>Validera ARP mellan Microsoft och hello-leverantör
Det här avsnittet använder PowerShell (klassisk)-kommandon. Om du har använt Azure Resource Manager för PowerShell-kommandon, kontrollera att du har åtkomst till admin-/ medadministratör toohello prenumeration via [klassiska Azure-portalen][OldPortal]. Felsöka med Azure Resource Manager kommandon finns toohello [komma ARP-tabeller i hello Resource Manager-distributionsmodellen] [ ARP] dokumentet.

>[!NOTE]
>tooget ARP, både hello Azure-portalen och Azure Resource Manager PowerShell-kommandon kan användas. Om det uppstår fel med hello Azure Resource Manager PowerShell-kommandon, bör klassiska PowerShell-kommandon fungera som klassiska PowerShell kommandon också fungera med Azure Resource Manager ExpressRoute-kretsar.
>
>

tooget hello ARP-tabell från hello primära MSEE router för privata hello-peering använder hello följande kommando:

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Ett exempelsvar för hello-kommando i hello lyckade scenariot:

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

På liknande sätt kan du kontrollera hello ARP-tabell från hello MSEE i hello *primära*/*sekundära* sökvägen för *privata* /  *Offentliga*/*Microsoft* peerkopplingar.

hello finns följande exempel visar hello svar hello-kommando för en peering inte.

    ARP Info:
       
>[!NOTE]
>Om hello ARP-tabell saknar mappas IP-adresser för hello gränssnitt tooMAC adresser, granska hello följande information:
>1. Om hello första IP-adressen för hello /30 undernät tilldelas för hello länken mellan hello MSEE PR och MSEE används hello-gränssnittet på MSEE PR. Azure använder alltid hello andra IP-adressen för MSEEs.
>2. Kontrollera om hello kund (C-tagg) och service (S-tagg) VLAN-taggarna matchar både på MSEE PR och MSEE.
>
>

## <a name="validate-bgp-and-routes-on-hello-msee"></a>Validera BGP och vägar på hello MSEE
Det här avsnittet använder PowerShell (klassisk)-kommandon. Om du har använt Azure Resource Manager för PowerShell-kommandon, kontrollera att du har åtkomst till admin-/ medadministratör toohello prenumeration via [klassiska Azure-portalen][OldPortal]

>[!NOTE]
>tooget BGP information, både hello Azure-portalen och Azure Resource Manager PowerShell-kommandon kan användas. Om det uppstår fel med hello Azure Resource Manager PowerShell-kommandon, bör klassiska PowerShell-kommandon fungera som klassiska PowerShell kommandon också fungera med Azure Resource Manager ExpressRoute-kretsar.
>
>

tooget hello routningstabellen (BGP-Grannens) sammanfattning för en viss routning kontext, använder hello följande kommando:

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

Ett exempelsvar är:

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

I föregående exempel hello visas hello-kommandot är användbara toodetermine för hur länge hello routning kontexten har upprättats. Den anger också antalet väg prefix annonseras av hello peering router.

>[!NOTE]
>Kontrollera om hello primära och sekundära peer-undernät som tilldelats matchar hello konfigurationen på hello länkade PE MSEE om hello tillstånd är i aktivt eller inaktivt. Även om hello korrigera *VlanId*, *AzureAsn*, och *PeerAsn* används på MSEEs och om du värdena mappar toohello som används på hello länkad PE MSEE. Om du väljer MD5-hash ska hello delad nyckel vara samma på MSEE och PE MSEE. toochange hello konfigurationen på hello MSEE routrar finns för[skapa och ändra routning för en ExpressRoute-krets][CreatePeering].
>
>

<p/>
>[!NOTE]
>Om vissa mål inte kan nås via en viss peering, kontrollera hello routningstabellen för hello MSEEs tillhörande toohello särskilda peering sammanhang. Om ett matchande prefix (kan vara NATed IP) finns i routningstabellen för hello, kontrollerar du om det finns ACL-brandväggar/NSG på hello sökväg och om de tillåter hello trafik.
>
>

tooget hello fullständig routningstabellen från MSEE på hello *primära* sökvägen för hello viss *privata* routning kontext, Använd hello följande kommando:

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

Exempel lyckade resultat för hello-kommandot är:

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

På liknande sätt kan du kontrollera hello routningstabellen från hello MSEE i hello *primära*/*sekundära* sökvägen för *privata* / *Offentliga*/*Microsoft* en peering-kontext.

hello finns följande exempel visar hello svar hello-kommando för en peering inte:

    Route Table Info:

##<a name="check-hello-traffic-statistics"></a>Kontrollera hello trafik statistik
tooget hello kombineras primära och sekundära sökväg trafik statistik--byte in och ut – för en peering kontext, Använd hello följande kommando:

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

Ett exempel på utdata för hello-kommandot är:

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

Ett exempel på utdata hello-kommando för en obefintlig peering är:

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a>Nästa steg
Mer information eller hjälp, ta en titt hello följande länkar:

- [Microsoft Support][Support]
- [Skapa och ändra en ExpressRoute-krets][CreateCircuit]
- [Skapa och ändra routning för en ExpressRoute-krets][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "Logisk Expressroute-anslutning"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Ikon för alla resurser"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Översikt över ikonen"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials exempel skärmbild"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials exempel skärmbild"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[OldPortal]: https://manage.windowsazure.com
[ARP]: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






