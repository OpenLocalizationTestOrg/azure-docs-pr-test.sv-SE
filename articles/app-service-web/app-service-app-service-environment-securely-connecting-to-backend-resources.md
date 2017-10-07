---
title: "aaaSecurely ansluter tooBackEnd resurser från en Apptjänst-miljö"
description: "Läs mer om hur toosecurely ansluta toobackend resurser från en Apptjänst-miljö."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 6311d3fc301512ea3c4ed8f14f268f75755aa415
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securely-connecting-toobackend-resources-from-an-app-service-environment"></a>På ett säkert sätt ansluta tooBackend resurser från en Apptjänst-miljö
## <a name="overview"></a>Översikt
Eftersom en Apptjänst-miljö skapas alltid i **antingen** ett virtuellt nätverk i Azure Resource Manager **eller** klassiska distributionsmodellen [virtuellt nätverk] [ virtualnetwork], utgående anslutningar från en Apptjänst-miljö tooother serverdelsresurser kan flöda uteslutande över hello virtuella nätverk.  Med en ändring görs i juni 2016 distribueras ASEs också till virtuella nätverk som använder offentliga-adressintervall eller RFC1918 adressutrymmen (d.v.s. privata adresser).  

Det kan exempelvis vara en SQL-Server som körs på ett kluster med virtuella datorer med port 1433 låst.  hello-slutpunkten kan vara ACLd tooonly tillåta åtkomst från andra resurser på hello samma virtuella nätverk.  

Ett annat exempel är känsliga slutpunkter kan köra lokalt och kan vara anslutna tooAzure via antingen [plats-till-plats] [ SiteToSite] eller [Azure ExpressRoute] [ ExpressRoute] anslutningar.  Därför kan endast resurser i virtuella nätverk ansluten toohello plats-till-plats eller ExpressRoute tunnlar kommer att kunna tooaccess lokala slutpunkter.

För alla dessa scenarier, att appar som körs på en Apptjänst-miljö kan toosecurely ansluter toohello olika servrar och resurser.  Utgående trafik från appar som körs i en Apptjänst-miljö tooprivate slutpunkter i hello samma virtuella nätverk (eller anslutna toohello samma virtuella nätverk), kommer endast flödet över hello virtuella nätverk.  Utgående trafik tooprivate slutpunkter inte flödar över hello offentliga Internet.

Ett villkor gäller toooutbound trafik från en Apptjänst-miljö tooendpoints inom ett virtuellt nätverk.  Apptjänstmiljöer kan inte nå slutpunkter av virtuella datorer finns i hello **samma** undernät som hello Apptjänst-miljö.  Det får normalt inte vara ett problem så länge Apptjänstmiljöer distribueras i ett undernät som reserverats för exklusiv användning av endast hello Apptjänst-miljö.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a>Utgående anslutning och DNS-krav
För en Apptjänstmiljö toofunction korrekt, kräver utgående åtkomst toovarious slutpunkter. En fullständig lista över hello externa slutpunkter som används av en ASE är i hello nätverksanslutning ”krävs” avsnittet av hello [nätverkskonfigurationen för ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.

Apptjänstmiljöer kräver en giltig DNS-infrastruktur som konfigurerats för hello virtuellt nätverk.  Om DNS-konfiguration ändras när en Apptjänst-miljö har skapats i någon orsak hello, kan utvecklare tvinga en Apptjänstmiljö toopick hello nya DNS-konfiguration.  Utlöser en rullande miljö omstart med ”starta om” hello-ikonen finns hello överst i hello Apptjänstmiljö kommer bladet hantering i hello portal hello miljö toopick hello nya DNS-konfiguration.

Vi rekommenderar också att alla anpassade DNS-servrar för hello virtuella nätverk konfigureras i tid tidigare toocreating en Apptjänst-miljö.  Om DNS-konfiguration för ett virtuellt nätverk har ändrats medan en Apptjänst-miljö skapas, vilket leder till att hello Apptjänstmiljö skapa processen misslyckas.  I en liknande vein är andra änden av en VPN-gateway och hello DNS-server kan inte nås eller otillgängliga hello Apptjänstmiljö processen fungerar inte om en anpassad DNS-server finns på hello.

## <a name="connecting-tooa-sql-server"></a>Ansluta tooa SQL Server
En vanlig SQL Server-konfiguration har en slutpunkt som lyssnade på port 1433:

![Slutpunkten för SQL Server][SqlServerEndpoint]

Det finns två tillvägagångssätt för att begränsa trafik toothis slutpunkt:

* [Network Access Control List] [ NetworkAccessControlLists] (Network ACL: er)
* [Nätverkssäkerhetsgrupper][NetworkSecurityGroups]

## <a name="restricting-access-with-a-network-acl"></a>Begränsa åtkomst till ett nätverk ACL
Port 1433 kan skyddas med hjälp av en lista över åtkomstkontroll.  hello exemplet nedan whitelists klienten adresserar kommer från inuti ett virtuellt nätverk och blockerar åtkomsten tooall andra klienter.

![Exempel på nätverket Access Control][NetworkAccessControlListExample]

Alla program som körs i Apptjänst-miljö i hello samma virtuella nätverk som hello SQL Server kommer att kunna tooconnect toohello SQL Server-instans med hello **VNet interna** IP-adress för hello SQL Server-datorn.  

hello exempel anslutningssträngen nedan referenser hello SQL Server med dess privata IP-adress.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Även om hello virtuella datorn har en offentlig slutpunkt, avvisas anslutningsförsök med hjälp av hello offentliga IP-adressen på grund av hello nätverket ACL. 

## <a name="restricting-access-with-a-network-security-group"></a>Begränsa åtkomst med en Nätverkssäkerhetsgrupp
Det är en alternativ metod för att skydda åtkomsten med en nätverkssäkerhetsgrupp.  Nätverkssäkerhetsgrupper kan vara tillämpade tooindividual virtuella datorer eller tooa undernät som innehåller virtuella datorer.

Först måste en nätverkssäkerhetsgrupp toobe skapas:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Begränsa åtkomst tooonly intern trafik för virtuella nätverk är mycket enkelt med en nätverkssäkerhetsgrupp.  hello standardregler i en nätverkssäkerhetsgrupp Tillåt endast åtkomst från andra klienter i nätverk i hello samma virtuella nätverk.

Därför låsa åtkomst tooSQL är Server så enkelt som att tillämpa en nätverkssäkerhetsgrupp med dess standard regler tooeither hello virtuella datorer som kör SQL Server eller hello undernät som innehåller hello virtuella datorer.

hello exemplet nedan gäller en säkerhet grupp toohello som innehåller nätverket:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

hello slutresultatet är en uppsättning säkerhetsregler som blockerar extern åtkomst samtidigt VNet intern åtkomst:

![Standard Nätverkssäkerhetsregler][DefaultNetworkSecurityRules]

## <a name="getting-started"></a>Komma igång
Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

tooget igång med Apptjänstmiljöer, se [introduktion tooApp-miljö][IntroToAppServiceEnvironment]

Mer information kring styra inkommande trafik tooyour Apptjänst-miljö finns [styra inkommande trafik tooan Apptjänstmiljö][ControlInboundASE]

Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
