---
title: "aaaNetwork konfigurationsinformation för att arbeta med Express Route"
description: "Konfigurationsinformation för nätverket för att köra Apptjänstmiljöer i virtuella nätverk anslutet tooan ExpressRoute-kretsen."
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 34b49178-2595-4d32-9b41-110c96dde6bf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2016
ms.author: stefsch
ms.openlocfilehash: 85bbc45cfed619485957ee2a898aa0a7604173cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Information om nätverkskonfiguration för App Service-miljöer med ExpressRoute
## <a name="overview"></a>Översikt
Kunder kan ansluta en [Azure ExpressRoute] [ ExpressRoute] krets tootheir virtuell nätverksinfrastruktur, vilket utöka sina lokala nätverk tooAzure.  En Apptjänst-miljö kan skapas i ett undernät för det här [virtuellt nätverk] [ virtualnetwork] infrastruktur.  Appar som körs på hello Apptjänst-miljö kan upprätta säkra anslutningar tooback slutpunkt resurser tillgängliga om de endast med hello ExpressRoute-anslutning.  

En Apptjänst-miljö kan skapas i **antingen** ett virtuellt nätverk i Azure Resource Manager **eller** klassisk distribution modellen virtuella nätverk.  Med en ändring görs i juni 2016 distribueras ASEs nu också till virtuella nätverk som använder offentliga-adressintervall eller RFC1918 adressutrymmen (d.v.s. privata adresser). 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Nödvändiga nätverksanslutning
Det finns anslutningskrav för Apptjänstmiljöer som inte kanske ursprungligen uppfyllas i ett virtuellt nätverk anslutet tooan ExpressRoute.  Apptjänstmiljöer kräver alla hello följande i ordning toofunction korrekt:

* Utgående anslutning tooAzure lagring nätverksslutpunkter över hela världen på båda portarna 80 och 443.  Detta inkluderar slutpunkter finns i hello samma region som hello Apptjänst-miljö, samt lagring slutpunkter finns i **andra** Azure-regioner.  Azure Storage-slutpunkter lösa under hello följande DNS-domäner: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* och *file.core.windows.net*.  
* Utgående network connectivity toohello filer för Azure-tjänsten på port 445.
* Utgående anslutning tooSql DB nätverksslutpunkter finns i hello samma region som hello Apptjänst-miljö.  SQL DB slutpunkter lösa under hello följande domän: *database.windows.net*.  Detta kräver att öppna åtkomst tooports 1433, 11000 11999 och 14000 14999.  Mer information finns [i den här artikeln på Sql Database V12 portar används](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
* Utgående anslutning toohello Azure management plan nätverksslutpunkter (både ASM och ARM-slutpunkter).  Detta inkluderar utgående anslutning tooboth *management.core.windows.net* och *management.azure.com*. 
* Utgående nätverksanslutningar för*ocsp.msocsp.com*, *mscrl.microsoft.com* och *crl.microsoft.com*.  Detta är nödvändiga toosupport SSL-funktionalitet.
* hello DNS-konfiguration för hello virtuella nätverket måste kunna lösa alla hello slutpunkter och domäner som anges i hello tidigare punkter.  Om dessa slutpunkter inte kan matchas Apptjänstmiljö skapa försök misslyckas och befintliga Apptjänstmiljöer kommer att markeras som ohälsosam.
* Utgående åtkomst på port 53 krävs för kommunikation med DNS-servrar.
* Om en anpassad DNS-server finns på Hej andra änden av en VPN-gateway, hello DNS-server måste kunna nås från hello undernät som innehåller hello Apptjänst-miljö. 
* hello utgående sökvägen kan inte skickas genom interna företagets proxyservrar eller kan den vara force tunneldata tooon lokalt.  Om du gör det ändringar hello effektiva NAT-adress för utgående nätverkstrafik från hello Apptjänst-miljö.  Ändrar hello NAT-adress för en Apptjänstmiljö utgående nätverkstrafik kan anslutningen fel toomany hello slutpunkter som anges ovan.  Detta resulterar i misslyckade försök för skapande av Apptjänst-miljö, samt tidigare felfri Apptjänstmiljöer markeras som ohälsosam.  
* Inkommande nätverksportar åtkomst toorequired för Apptjänstmiljöer får enligt beskrivningen i det här [artikel][requiredports].

hello DNS-krav kan uppfyllas genom att säkerställa att en giltig DNS-infrastruktur konfigurerad och underhålls för hello virtuellt nätverk.  Om DNS-konfiguration ändras när en Apptjänst-miljö har skapats i någon orsak hello, kan utvecklare tvinga en Apptjänstmiljö toopick hello nya DNS-konfiguration.  Utlöser en rullande miljö omstart med hjälp av hello ”Restart” finns hello överst på bladet hantering av hello Apptjänstmiljö i hello [Azure-portalen] [ NewPortal] kommer hello miljö toopick hello nya DNS-konfiguration.

hello inkommande network access kraven kan uppfyllas genom att konfigurera en [nätverkssäkerhetsgruppen] [ NetworkSecurityGroups] på hello Apptjänstmiljös undernät tooallow hello krävs åtkomst som beskrivs i den här [artikel][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Aktivera utgående nätverksanslutning för en Apptjänst-miljö
Som standard annonserar en nyligen skapade ExpressRoute-krets en standardväg som tillåter utgående Internet-anslutning.  Med den här konfigurationen för en Apptjänstmiljö att kan tooconnect tooother Azure-slutpunkter.

Men en gemensam konfiguration av customer är toodefine sina egna standardvägen (0.0.0.0/0) som tvingar utgående Internet tooinstead trafikflöde lokalt.  Den här trafikflöde bryts utan undantag Apptjänstmiljöer eftersom hello utgående trafik är antingen blockerade lokalt eller NAT d tooan okänt uppsättning adresser som inte längre att fungera med olika Azure-slutpunkter.

hello-lösning är toodefine (minst) användardefinierade vägar (udr: er) i hello undernät som innehåller hello Apptjänst-miljö.  En UDR definierar undernät-specifika vägar som ska användas i stället för hello standardväg.

Om möjligt bör toouse hello följande konfiguration:

* Hej ExpressRoute configuration annonserar 0.0.0.0/0 och som standard kraft tunnlar all utgående trafik lokalt.
* Hej UDR tillämpas toohello undernät som innehåller hello Apptjänstmiljö definierar 0.0.0.0/0 med ett nexthop-typ för Internet (ett exempel på detta finns längre ned i den här artikeln).

hello är kombinerade effekten av dessa steg att hello undernätverksnivå UDR företräde framför hello ExpressRoute Tvingad tunneltrafik tillse utgående Internetåtkomst från hello Apptjänst-miljö.

> [!IMPORTANT]
> Hej vägar som definierats i en UDR **måste** vara tillräckligt specifikt för åsidosätter över alla vägar annonseras av hello ExpressRoute konfiguration.  hello exemplet nedan använder hello bred 0.0.0.0/0 adressintervallet och som sådan kan potentiellt av misstag åsidosättas av flödet annonser med hjälp av mer specifika adressintervall.
> 
> Apptjänstmiljöer stöds inte med ExpressRoute-konfigurationer som **annonserar vägar från hello offentlig peering sökväg toohello privat peering sökväg mellan**.  ExpressRoute-konfigurationer som har konfigurerats, offentlig peering får vägannonser från Microsoft för ett stort antal Microsoft Azure IP-adressintervall.  Om dessa adressintervall mellan annonseras på hello privat peering sökväg, är hello slutresultatet att alla utgående paket från hello Apptjänstmiljös undernät kommer vara force tunneldata tooa kundens lokala nätverkets infrastruktur.  Det här flödet i nätverk stöds för närvarande inte med Apptjänstmiljöer.  En lösning toothis problemet är toostop mellan reklam vägar från hello offentlig peering sökväg toohello privat peering sökväg.
> 
> 

Bakgrundsinformation om användardefinierade vägar är tillgänglig i det här [översikt][UDROverview].  

Information om hur du skapar och konfigurerar användardefinierade vägar är tillgänglig i det här [hur tooGuide][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Exempel på UDR konfiguration för en Apptjänst-miljö
**Förutsättningar**

1. Installera Azure Powershell från hello [Azure hämtar sidan] [ AzureDownloads] (funktionsdokumentationen juni 2015 eller senare).  Under ”kommandoradsverktyg” finns det en länk för ”installera” under ”Windows Powershell” som ska installera hello senaste Powershell-cmdlets.
2. Vi rekommenderar att du skapat ett unikt undernät för exklusiv användning av en Apptjänst-miljö.  Detta säkerställer att hello udr: er används toohello undernätet öppnas endast utgående trafik för hello Apptjänst-miljö.
3. **Viktiga**: Distribuera inte hello Apptjänstmiljö tills **när** hello följande konfigurationssteg följs.  Detta säkerställer att utgående nätverksanslutningen är tillgänglig innan du försöker toodeploy en Apptjänst-miljö.

**Steg 1: Skapa en namngiven routningstabell**

hello skapar följande kodavsnitt en routningstabell som kallas ”DirectInternetRouteTable” i hello West US Azure-region:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Steg 2: Skapa en eller flera vägar i routningstabellen för hello**

Du behöver tooadd en eller flera vägar toohello routningstabellen i ordning tooenable utgående Internetåtkomst.  

hello rekommenderat tillvägagångssätt för att konfigurera utgående åtkomst toohello Internet är toodefine en väg för 0.0.0.0/0 som visas nedan.

    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Kom ihåg att 0.0.0.0/0 är en bred adressintervallet och därmed åsidosätts av mer specifika adressintervall annonseras av hello ExpressRoute.  toore-iterera hello tidigare rekommendation, en UDR med en 0.0.0.0/0 väg bör användas tillsammans med en konfiguration för ExressRoute som bara annonserar 0.0.0.0/0 samt. 

Alternativt kan hämta du en omfattande och uppdaterad lista över CIDR-intervallen som används av Azure.  hello Xml-filen som innehåller alla hello Azure IP-adressintervall är tillgängliga från hello [Microsoft Download Center][DownloadCenterAddressRanges].  

Observera dock att dessa intervall ändras med tiden, vilket kräver manuell periodiska uppdateringar toohello användardefinierade vägar tookeep synkroniserade.  Dessutom eftersom det finns en standard som övre gräns för 100 vägar i en enda UDR du behöver för ”sammanfattas” hello Azure IP-adress intervall toofit inom hello 100 vägen måste och Tänk på att UDR användardefinierade vägar toobe mer specifik än hello vägarna som annonseras av din ExpressRoute.  

**Steg 3: Koppla hello flödet tabell toohello undernät som innehåller hello Apptjänstmiljö**

hello sista konfigurationssteget är tooassociate hello flödet tabell toohello undernät där hello Apptjänstmiljö ska distribueras.  hello associerar följande kommando hello ”DirectInternetRouteTable” toohello ”ASESubnet” som slutligen innehåller en Apptjänst-miljö.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Steg 4: Sista stegen**

När hello routningstabellen är bundna toohello undernät, rekommenderas toofirst test och bekräfta hello avsedd effekt.  Till exempel distribuera en virtuell dator i hello undernät och kontrollera att:

* Utgående trafik tooboth Azure och Azure-slutpunkter nämnts tidigare i den här artikeln är **inte** flödar ned hello ExpressRoute-kretsen.  Det är mycket viktigt tooverify problemet eftersom om utgående trafik från hello undernät är fortfarande tvingas tunneldata lokalt, skapa en Apptjänst-miljön kommer alltid att misslyckas. 
* DNS-sökning för hello slutpunkter som nämnts tidigare alla löser korrekt. 

När hello senare steg bekräftas måste toodelete hello virtuell dator eftersom hello undernät toobe ”tomt” på hello tid hello Apptjänst-miljö skapas.

Fortsätt sedan med att skapa en Apptjänst-miljö.

## <a name="getting-started"></a>Komma igång
Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

tooget igång med Apptjänstmiljöer, se [introduktion tooApp-miljö][IntroToAppServiceEnvironment]

Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com


<!-- IMAGES -->
