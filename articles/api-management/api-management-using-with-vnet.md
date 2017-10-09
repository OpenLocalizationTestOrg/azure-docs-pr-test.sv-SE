---
title: "aaaHow toouse Azure API Management med virtuella nätverk"
description: "Lär dig hur toosetup anslutning tooa virtuella nätverk i Azure API Management och åtkomst web services genom den."
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
ms.openlocfilehash: 426b3974e4fed7daffdb0c3f02381edbc326dc28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-api-management-with-virtual-networks"></a>Hur toouse Azure API Management med virtuella nätverk
Virtuella Azure-nätverk (Vnet) kan du tooplace någon av dina Azure-resurser i ett routeable-internet-nätverk som du styr åtkomst till. Dessa nätverk kan sedan vara anslutna tooyour lokala nätverk med olika VPN-teknologier. toolearn mer om Azure Virtual Networks börja med hello information här: [Azure översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).

Azure API Management kan distribueras i hello virtuella nätverk (VNET), så att den har åtkomst till backend-tjänster inom hello nätverk. hello kan developer-portalen och API-gateway vara konfigurerade toobe som är tillgänglig från hello Internet eller endast inom hello virtuellt nätverk.

> [!NOTE]
> Azure API Management stöder både klassiska och Azure Resource Manager Vnet.
>
>

## <a name="enable-vpn"></a>Aktivera VNET-anslutning
> [!NOTE]
> Virtuella nätverk är tillgängliga i hello **Premium** och **Developer** nivåer. tooswitch mellan hello nivåer, öppna API Management-tjänsten i hello Azure-portalen och sedan hello **skala och prissättning** fliken. Under hello **prisnivå** väljer hello Premium eller utvecklare nivån och klicka på Spara.
>

tooenable VNET-anslutningar, öppna API Management-tjänsten i hello Azure-portalen och hello **för virtuella nätverk** sidan.

![Virtuellt nätverk-menyn i API Management][api-management-using-vnet-menu]

Välj önskad hello åtkomsttyp:

* **Externa**: hello API Management gateway- och developer-portalen är tillgängliga från hello offentliga internet via en extern belastningsutjämnare. hello gateway kan komma åt resurser i hello virtuellt nätverk.

![Offentlig peering][api-management-vnet-public]

* **Internt**: hello API Management gateway- och developer-portalen är enbart tillgänglig från hello virtuellt nätverk via en intern belastningsutjämnare. hello gateway kan komma åt resurser i hello virtuellt nätverk.

![Privat peering][api-management-vnet-private]

Nu visas en lista över alla regioner där API Management-tjänsten har etablerats. Välj en VNET och undernät för varje region. hello listan är fylld med både klassiska och Resource Manager virtuella nätverk som är tillgängliga i din Azure-prenumerationer som har konfigurerats i hello region som du konfigurerar.

> [!NOTE]
> **Tjänstslutpunkten** i hello ovanför diagrammet innehåller Gateway-proxyn, Publisher Portal, Developer-portalen, GIT och hello direkt hantering av slutpunkten.
> **Hanteringsslutpunkten** i hello ovan diagram är hello-slutpunkt som finns på hello toomanage tjänstkonfiguration via Azure portal och Powershell.
> Tänk också på, att, även om hello diagram visar IP-adresser för olika, API Management-tjänsten **endast** svarar på dess konfigurerade värdnamn.

> [!IMPORTANT]
> När du distribuerar ett Azure API Management instans tooa Resource Manager VNET måste hello tjänsten vara i ett dedikerat undernät som innehåller några resurser förutom Azure API Management-instanser. Om ett försök görs toodeploy misslyckas en Azure API Management instans tooa Resource Manager VNET-undernät som innehåller andra resurser, hello-distribution.
>
>

![Välj VPN][api-management-setup-vpn-select]

Klicka på **spara** hello överst i hello-skärmen.

> [!NOTE]
> hello VIP-adressen för hello API Management-instansen kommer att ändras varje gång virtuella nätverk är aktiverad eller inaktiverad.  
> hello VIP-adressen ändras samtidigt när API Management flyttas från **externa** för**internt** eller tvärtom
>


> [!IMPORTANT]
> Om du tar bort API-hantering från ett VNET eller ändrar hello ett den har distribuerats i låst hello har använts tidigare VNET kan vara upp too4 timmar. Under denna tid vara möjligt toodelete hello VNET eller inte distribuera en ny resurs tooit.

## <a name="enable-vnet-powershell"></a>Aktivera VNET-anslutningen med hjälp av PowerShell-cmdlets
Du kan också aktivera VNET-anslutning med hello PowerShell-cmdlets

* **Skapa en API Management-tjänst i ett VNET**: Använd cmdlet hello [ny AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate Azure API Management-tjänsten i ett virtuellt nätverk.

* **Distribuera en befintlig API Management-tjänst i ett VNET**: Använd cmdlet hello [uppdatering AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove en befintlig Azure API Management-tjänst i ett virtuellt nätverk.

## <a name="connect-vnet"></a>Ansluta tooa webbtjänst inom ett virtuellt nätverk
När din API Management-tjänst är ansluten toohello VNET, är åtkomst till backend-tjänster i den inte annorlunda än att komma åt offentliga tjänster. Skriv i hello lokal IP-adress eller hello värdnamnet (om en DNS-server är konfigurerad för hello VNET) för webbtjänsten i hello **-webbtjänstens URL** fältet när du skapar ett nytt API eller redigerar en befintlig.

![Lägg till API från VPN][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"></a>Vanliga nätverkskonfigurationen
Nedan följer en lista över vanliga problem som kan uppstå vid distribution av API Management-tjänsten till ett virtuellt nätverk.

* **Anpassade DNS-serverinstallation**: hello API Management-tjänsten är beroende av flera Azure-tjänster. När API-hantering finns i ett VNET med en anpassad DNS-server, måste tooresolve hello värdnamn för de Azure-tjänsterna. Följ [detta](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) vägledning om anpassade DNS-inställningarna. Se hello portar tabellen nedan och andra krav som referens.

> [!IMPORTANT]
> Det rekommenderas att, om du använder en anpassad DNS-servrar för hello VNET kan du konfigurera som **innan** distribuera en API Management-tjänsten till den. Annat du behöver för uppdatera hello API Management-tjänsten varje gång du ändrar hello DNS-servrar (s) genom att köra hello [gäller åtgärden för konfiguration](https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementservice#ApiManagementService_ApplyNetworkConfigurationUpdates)

* **Portar som behövs för API Management**: inkommande och utgående trafik till hello undernät som API Management har distribuerats kan kontrolleras med [Nätverkssäkerhetsgruppen][Network Security Group]. Om någon av dessa portar är otillgängliga API Management kanske inte fungerar korrekt och kan inte komma åt. Med en eller flera av de här portarna blockeras är ett annat vanligt felkonfiguration problem när du använder API-hantering med ett VNET.

När en instans för API Management-tjänsten finns i ett VNET, används hello portarna i följande tabell hello.

| Källan / målet portar | Riktning | Transportprotokoll | Syfte | Källan / målet | Åtkomsttyp |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Inkommande |TCP |Klienten kommunikation tooAPI Management |INTERNET / VIRTUAL_NETWORK |Extern |
| * / 3443 |Inkommande |TCP |Hanteringsslutpunkten för Azure-portalen och Powershell |INTERNET / VIRTUAL_NETWORK |Externa och interna |
| * / 80, 443 |Utgående |TCP |Beroendet av Azure Storage och Azure Service Bus |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / 1433 |Utgående |TCP |Beroende på Azure SQL |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / 11000 - 11999 |Utgående |TCP |V12 Azure SQL-beroendet |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / 14000 - 14999 |Utgående |TCP |V12 Azure SQL-beroendet |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / 5671 |Utgående |AMQP |Beroende av loggen tooEvent hubb principen och övervakningsagent |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| 6381 - 6383 / 6381 - 6383 |Inkommande och utgående |UDP |Beroende på Redis-Cache |VIRTUAL_NETWORK / VIRTUAL_NETWORK |Externa och interna |-
| * / 445 |Utgående |TCP |Beroende på Azure-filresursen för GIT |VIRTUAL_NETWORK / INTERNET |Externa och interna |
| * / * | Inkommande |TCP |Azure-infrastrukturen belastningsutjämnare | AZURE_LOAD_BALANCER / VIRTUAL_NETWORK |Externa och interna |

* **SSL-funktionen**: kedjan byggnad och validering hello API Management-tjänsten måste utgående network connectivity tooocsp.msocsp.com, mscrl.microsoft.com och crl.microsoft.com tooenable SSL-certifikat. Detta beroende är inte krävs, om alla certifikat som du överför tooAPI Management innehåller hello fullständig kedja toohello CA rot.

* **DNS-åtkomst**: utgående åtkomst på port 53 krävs för kommunikation med DNS-servrar. Om en anpassad DNS-server finns på Hej andra änden av en VPN-gateway, hello DNS-server måste kunna nås från hello undernät som är värd för API-hantering.

* **Mått och hälsoövervakning**: utgående anslutning tooAzure övervakning nätverksslutpunkter, vilket löser under hello följande domäner: global.metrics.nsatc.net, shoebox2.metrics.nsatc.net, prod3.metrics.nsatc.net.

* **Expressinstallationen väg**: en gemensam konfiguration av customer är toodefine sina egna standardvägen (0.0.0.0/0) som tvingar utgående Internet-trafik tooinstead flöde lokalt. Den här trafikflöde bryts utan undantag anslutningen till Azure API Management eftersom hello utgående trafik är antingen blockerade lokalt eller NAT d tooan okänt uppsättning adresser som inte längre att fungera med olika Azure-slutpunkter. hello lösningen är toodefine (minst) användardefinierade vägar ([udr: er][UDRs]) i hello undernät som innehåller hello Azure API Management. En UDR definierar undernät-specifika vägar som ska användas i stället för hello standardväg.
  Om möjligt bör toouse hello följande konfiguration:
 * Hej ExpressRoute configuration annonserar 0.0.0.0/0 och som standard kraft tunnlar all utgående trafik lokalt.
 * Hej UDR tillämpas toohello undernät som innehåller hello Azure API Management definierar 0.0.0.0/0 med ett nexthop-typ för Internet.
 hello är kombinerade effekten av dessa steg att hello undernätverksnivå UDR företräde framför hello ExpressRoute Tvingad tunneltrafik tillse utgående Internetåtkomst från hello Azure API Management.

>[!WARNING]  
>Azure API Management stöds inte med ExpressRoute-konfigurationer som **felaktigt cross-annonsera vägar från hello offentlig peering sökväg toohello privat peering sökväg**. ExpressRoute-konfigurationer som har konfigurerats, offentlig peering får vägannonser från Microsoft för ett stort antal Microsoft Azure IP-adressintervall. Om dessa adressintervall felaktigt cross-annonserade på hello privat peering sökväg, är hello slutresultatet att alla utgående paket från hello Azure API Management instans undernät är felaktigt kraft tunneldata tooa kundens lokala nätverk infrastruktur. Det här nätverket flödet bryts Azure API Management. hello lösning toothis problemet är toostop mellan reklam vägar från hello offentlig peering sökväg toohello privat peering sökväg.


## <a name="troubleshooting"></a>Felsökning
När du gör ändringar tooyour nätverket referera för[NetworkStatus API](https://docs.microsoft.com/en-us/rest/api/apimanagement/networkstatus), toovalidate om hello API Management-tjänsten inte har förlorat åtkomst tooany av hello viktiga resurser som den är beroende av. hello anslutningsstatus ska uppdateras var 15: e minut.

## <a name="limitations"></a>Begränsningar
* Ett undernät som innehåller API Management-instanser får inte innehålla andra Azure-resurs-typer.
* hello-undernät och hello API Management-tjänsten måste vara i hello samma prenumeration.
* Ett undernät som innehåller API Management-instanser kan inte flyttas mellan prenumerationer.
* När du använder ett internt virtuellt nätverk endast en intern IP-adress ska vara tillgänglig från hello intervallet anges i [RFC 1918](https://tools.ietf.org/html/rfc1918), en offentlig IP-adress kan inte anges.
* För distribution av flera regioner API Management ansvarar med interna virtuella nätverk som har konfigurerats kan användare för att hantera sina egna belastningsutjämning som de äger hello DNS.


## <a name="related-content"></a>Relaterat innehåll
* [Ansluta ett virtuellt nätverk toobackend med hjälp av Vpn-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Ansluta ett virtuellt nätverk från olika distributionsmodeller](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Hur toouse hello API Inspector tootrace anropar i Azure API Management](api-management-howto-api-inspector.md)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Enable VPN connections]: #enable-vpn
[Connect tooa web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/virtual-networks-nsg.md
