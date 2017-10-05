---
title: "Hur du använder Azure API Management med virtuella nätverk"
description: "Lär dig mer om att skapa en anslutning till ett virtuellt nätverk i Azure API Management och åtkomst till webbtjänster genom den."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 64b58f7b-ca22-47dc-89c0-f6bb0af27a48
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 268cee739188d81feffc36ac07fcdfa18ff95a4d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Hur du använder Azure API Management med virtuella nätverk
Virtuella Azure-nätverk (Vnet) kan du placera någon av dina Azure-resurser i ett routeable-internet-nätverk som du styr åtkomst till. Dessa nätverk kan sedan vara ansluten till ditt lokala nätverk med olika VPN-teknologier. Läs mer om Azure Virtual Networks startar med den här informationen: [Azure översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).

Azure API Management kan distribueras i det virtuella nätverket (VNET), så att den har åtkomst till backend-tjänster i nätverket. Developer-portalen och API-gateway kan konfigureras för att vara tillgänglig från Internet eller i det virtuella nätverket.

> [!NOTE]
> Azure API Management stöder både klassiska och Azure Resource Manager Vnet.
>
>

## <a name="enable-vpn"></a>Aktivera VNET-anslutning
> [!NOTE]
> Virtuella nätverk är tillgängliga i den **Premium** och **Developer** nivåer. Om du vill växla mellan nivåer, öppna API Management-tjänsten i Azure-portalen och sedan öppna den **skala och prissättning** fliken. Under den **prisnivå** väljer Premium eller utvecklare nivån och klicka på Spara.
>

Om du vill aktivera VNET-anslutningen, öppna API Management-tjänsten i Azure portal och öppna den **för virtuella nätverk** sidan.

![Virtuellt nätverk-menyn i API Management][api-management-using-vnet-menu]

Välj typ av åtkomst:

* **Externa**: API Management gateway och developer portal är tillgänglig från internet via en extern belastningsutjämnare. Gatewayen kan komma åt resurser i det virtuella nätverket.

![Offentlig peering][api-management-vnet-public]

* **Internt**: API Management gateway och developer portal är enbart tillgänglig från det virtuella nätverket via en intern belastningsutjämnare. Gatewayen kan komma åt resurser i det virtuella nätverket.

![Privat peering][api-management-vnet-private]

Nu visas en lista över alla regioner där API Management-tjänsten har etablerats. Välj en VNET och undernät för varje region. I listan fylls med både klassiska och Resource Manager virtuella nätverk som är tillgängliga i din Azure-prenumerationer som har konfigurerats i den region som du konfigurerar.

> [!NOTE]
> **Tjänstslutpunkten** i ovanstående diagram innehåller Gateway-proxyn, Publisher Portal, Developer-portalen, GIT och direkt hantering av slutpunkten.
> **Hanteringsslutpunkten** i ovanstående diagram är den slutpunkt som värd för tjänsten för att hantera konfiguration via Azure portal och Powershell.
> Tänk också på, att, även om diagrammet visar IP-adresser för olika, API Management-tjänsten **endast** svarar på dess konfigurerade värdnamn.

> [!IMPORTANT]
> När du distribuerar en Azure API Management-instans till en Resource Manager-VNET, måste tjänsten vara i ett dedikerat undernät som innehåller några resurser förutom Azure API Management-instanser. Om ett försök görs att distribuera en Azure API Management-instans till en Resource Manager VNET-undernät som innehåller andra resurser, distributionen misslyckas.
>
>

![Välj VPN][api-management-setup-vpn-select]

Klicka på **spara** överst på skärmen.

> [!NOTE]
> VIP-adressen för API Management-instansen kommer att ändras varje gång virtuella nätverk är aktiverad eller inaktiverad.  
> VIP-adressen ändras samtidigt när API Management flyttas från **externa** till **internt** eller tvärtom
>


> [!IMPORTANT]
> Om du ta bort API-hantering från ett VNET eller ändrar en distribuerad i kan tidigare VNET förbli låsta för upp till fyra timmar. Under denna tid går det inte att ta bort VNET eller distribuera en ny resurs till den.

## <a name="enable-vnet-powershell"></a>Aktivera VNET-anslutningen med hjälp av PowerShell-cmdlets
Du kan också aktivera VNET-anslutning med hjälp av PowerShell-cmdlets

* **Skapa en API Management-tjänst i ett VNET**: Använd cmdlet [ny AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) att skapa en Azure API Management-tjänst i ett virtuellt nätverk.

* **Distribuera en befintlig API Management-tjänst i ett VNET**: Använd cmdlet [uppdatering AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) att flytta en befintlig Azure API Management-tjänst i ett virtuellt nätverk.

## <a name="connect-vnet"></a>Ansluta till en webbtjänst som finns inom ett virtuellt nätverk
När din API Management-tjänst är ansluten till VNET, är åtkomst till backend-tjänster i den inte annorlunda än att komma åt offentliga tjänster. Skriv i den lokala IP-adressen eller värdnamnet (om en DNS-server är konfigurerad för VNET) för webbtjänsten i den **-webbtjänstens URL** fältet när du skapar ett nytt API eller redigerar en befintlig.

![Lägg till API från VPN][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"></a>Vanliga nätverkskonfigurationen
Nedan följer en lista över vanliga problem som kan uppstå vid distribution av API Management-tjänsten till ett virtuellt nätverk.

* **Anpassade DNS-serverinstallation**: I API Management-tjänsten är beroende av flera Azure-tjänster. När API-hantering finns i ett VNET med en anpassad DNS-server, måste den matcha värdnamn för de Azure-tjänsterna. Följ [detta](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) vägledning om anpassade DNS-inställningarna. Se tabellen portar och andra krav som referens.

> [!IMPORTANT]
> Det rekommenderas att, om du använder en anpassad DNS-servrar för VNET kan du konfigurera som **innan** distribuera en API Management-tjänsten till den. Annars måste du uppdatera API Management-tjänsten varje gång du ändrar DNS-servrar (s) genom att köra den [gäller åtgärden för konfiguration](https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementservice#ApiManagementService_ApplyNetworkConfigurationUpdates)

* **Portar som behövs för API Management**: inkommande och utgående trafik i undernätet som API Management har distribuerats kan kontrolleras med [Nätverkssäkerhetsgruppen][Network Security Group]. Om någon av dessa portar är otillgängliga API Management kanske inte fungerar korrekt och kan inte komma åt. Med en eller flera av de här portarna blockeras är ett annat vanligt felkonfiguration problem när du använder API-hantering med ett VNET.

När en instans för API Management-tjänsten är värd för ett virtuellt nätverk, används portarna i följande tabell.

| Källan / målet portar | Riktning | Transportprotokoll | Syfte | Källan / målet | Åtkomsttyp |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Inkommande |TCP |Klientkommunikation till API-hantering |INTERNET / VIRTUAL_NETWORK |Extern |
| * / 3443 |Inkommande |TCP |Hanteringsslutpunkten för Azure-portalen och Powershell |INTERNET / VIRTUAL_NETWORK |Externa och interna |
| * / 80, 443 |Utgående |TCP |Beroendet av Azure Storage och Azure Service Bus |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / 1433 |Utgående |TCP |Beroende på Azure SQL |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / 11000 - 11999 |Utgående |TCP |V12 Azure SQL-beroendet |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / 14000 - 14999 |Utgående |TCP |V12 Azure SQL-beroendet |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / 5671 |Utgående |AMQP |Beroende för inloggning till Event Hub-principen och övervakningsagent |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| 6381 - 6383 / 6381 - 6383 |Inkommande och utgående |UDP |Beroende på Redis-Cache |VIRTUAL_NETWORK / VIRTUAL_NETWORK |Externa och interna |-
| * / 445 |Utgående |TCP |Beroende på Azure-filresursen för GIT |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / * | Inkommande |TCP |Azure-infrastrukturen belastningsutjämnare | AZURE_LOAD_BALANCER / VIRTUAL_NETWORK |Externa och interna |

* **SSL-funktionen**: att bygga kedja för SSL-certifikat och validering API Management-tjänsten måste utgående nätverksanslutning till ocsp.msocsp.com, mscrl.microsoft.com och crl.microsoft.com. Detta beroende är inte krävs, om något av de certifikat som du överför till API Management innehåller hela kedjan rot-CA: N.

* **DNS-åtkomst**: utgående åtkomst på port 53 krävs för kommunikation med DNS-servrar. Om en anpassad DNS-server finns på den andra änden av en VPN-gateway, måste DNS-servern vara nåbar från det undernät som är värd för API-hantering.

* **Mått och hälsoövervakning**: utgående nätverksanslutning till Azure-övervakning slutpunkter som löser under följande domäner: global.metrics.nsatc.net, shoebox2.metrics.nsatc.net, prod3.metrics.nsatc.net.

* **Expressinstallationen väg**: en gemensam konfiguration av customer är att definiera egna standardvägen (0.0.0.0/0) som tvingar utgående Internet-trafiken flöda lokalt i stället. Den här trafikflöde bryts utan undantag anslutningen till Azure API Management eftersom utgående trafik är antingen blockerade lokalt eller NAT skulle med ett okänt uppsättning adresser som inte längre att fungera med olika Azure-slutpunkter. Lösningen är att definiera en (eller flera) användardefinierade vägar ([udr: er][UDRs]) i undernät som innehåller Azure API Management. En UDR definierar undernät-specifika vägar som ska användas i stället för standardväg.
  Om möjligt bör du använder följande konfiguration:
 * ExpressRoute-konfigurationen annonserar 0.0.0.0/0 och som standard kraft tunnlar all utgående trafik lokalt.
 * UDR tillämpad på undernätet som innehåller Azure API Management definierar 0.0.0.0/0 med ett nexthop-typ för Internet.
 Den kombinerade effekten av dessa steg är att undernätverksnivån UDR företräde framför den ExpressRoute Tvingad tunneltrafik tillse utgående Internetåtkomst från Azure API Management.

>[!WARNING]  
>Azure API Management stöds inte med ExpressRoute-konfigurationer som **felaktigt cross-annonsera vägar från offentlig peering sökvägen att privat peering sökväg**. ExpressRoute-konfigurationer som har konfigurerats, offentlig peering får vägannonser från Microsoft för ett stort antal Microsoft Azure IP-adressintervall. Om dessa adressintervall felaktigt cross-annonseras i privat peering sökväg, är slutresultatet att alla utgående paket från Azure API Management-instans undernät felaktigt kraft tunnlar kundens lokala nätverkets infrastruktur. Det här nätverket flödet bryts Azure API Management. Lösning på problemet är att stoppa mellan reklam vägar från offentlig peering sökvägen att privat peering sökväg.


## <a name="troubleshooting"></a>Felsökning
När du gör ändringar i nätverket referera till [NetworkStatus API](https://docs.microsoft.com/en-us/rest/api/apimanagement/networkstatus), för att verifiera om API Management-tjänsten inte har tillgång till någon av de viktiga resurser som den är beroende av. Statusen ska uppdateras var 15: e minut.

## <a name="limitations"></a>Begränsningar
* Ett undernät som innehåller API Management-instanser får inte innehålla andra Azure-resurs-typer.
* Undernätet och API Management-tjänsten måste vara i samma prenumeration.
* Ett undernät som innehåller API Management-instanser kan inte flyttas mellan prenumerationer.
* När du använder ett internt virtuellt nätverk, endast en intern IP-adress ska vara tillgänglig från intervallet anges i [RFC 1918](https://tools.ietf.org/html/rfc1918), en offentlig IP-adress kan inte anges.
* För distribution av flera regioner API-hantering ansvarar med interna virtuella nätverk som har konfigurerats kan användare för att hantera sina egna belastningsutjämning som de äger DNS.


## <a name="related-content"></a>Relaterat innehåll
* [Ansluta ett virtuellt nätverk till backend med hjälp av Vpn-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Ansluta ett virtuellt nätverk från olika distributionsmodeller](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Hur du använder API-Inspector för att spåra anropar i Azure API Management](api-management-howto-api-inspector.md)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/virtual-networks-nsg.md
