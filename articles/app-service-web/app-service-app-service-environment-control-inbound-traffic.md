---
title: "aaaHow tooControl inkommande trafik tooan Apptjänstmiljö"
description: "Läs mer om hur tooconfigure network security regler toocontrol inkommande trafik tooan Apptjänst-miljö."
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: 
ms.assetid: 4cc82439-8791-48a4-9485-de6d8e1d1a08
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: stefsch
ms.openlocfilehash: e7c6e6201db6a1ea77f7a2eee29a3b5445175495
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocontrol-inbound-traffic-tooan-app-service-environment"></a>Hur tooControl inkommande trafik tooan Apptjänstmiljö
## <a name="overview"></a>Översikt
En Apptjänst-miljö kan skapas i **antingen** ett virtuellt nätverk i Azure Resource Manager **eller** klassiska distributionsmodellen [virtuellt nätverk][virtualnetwork].  Ett nytt virtuellt nätverk och ett nytt undernät kan definieras när hello en Apptjänst-miljö skapas.  Alternativt kan en Apptjänst-miljö skapas i ett befintligt virtuellt nätverk och befintlig undernät.  Med en ändring som gjorts i juni 2016 distribueras ASEs också till virtuella nätverk som använder offentliga-adressintervall eller RFC1918 adressutrymmen (d.v.s. privata adresser).  Mer information om hur du skapar en Apptjänst-miljö finns [hur tooCreate en Apptjänstmiljö][HowToCreateAnAppServiceEnvironment].

En Apptjänst-miljö måste alltid skapas i ett undernät eftersom ett undernät ger en nätverksgräns som kan vara används toolock ned inkommande trafik bakom överordnade enheter och tjänster så att HTTP och HTTPS-trafik tillåts endast från specifika överordnad IP-adresser.

Inkommande och utgående nätverkstrafik på ett undernät kontrolleras med en [nätverkssäkerhetsgruppen][NetworkSecurityGroups]. Kontrollera inkommande trafik kräver att skapa Nätverkssäkerhetsregler i en nätverkssäkerhetsgrupp och sedan tilldela hello network security grupp hello undernät som innehåller hello Apptjänst-miljö.

När en nätverkssäkerhetsgrupp har tilldelats tooa undernät, inkommande trafik tooapps i Apptjänst-miljö är tillåtet/blockeras baserat på hello hello tillåta och neka regler som definierats i hello nätverkssäkerhetsgruppen.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="inbound-network-ports-used-in-an-app-service-environment"></a>Inkommande nätverksportar som används i en Apptjänst-miljö
Innan du låsa inkommande nätverkstrafik till en nätverkssäkerhetsgruppen är viktiga tooknow hello obligatoriska och valfria nätverksportar som används av en Apptjänst-miljö.  Stänger av misstag av trafik toosome portar kan resultera i förlust av funktioner i en Apptjänst-miljö.

hello följer en lista över portar som används av en Apptjänst-miljö. Alla portar **TCP**, såvida inte annat anges tydligt:

* 454: **krävs port** används av Azure-infrastrukturen för att hantera och underhålla Apptjänstmiljöer via SSL.  Inte blockera trafik toothis port.  Den här porten är alltid bundna toohello offentliga VIP för en ASE.
* 455: **krävs port** används av Azure-infrastrukturen för att hantera och underhålla Apptjänstmiljöer via SSL.  Inte blockera trafik toothis port.  Den här porten är alltid bundna toohello offentliga VIP för en ASE.
* 80: standardport för inkommande HTTP-trafik tooapps körs i App Service-planer i en Apptjänst-miljö.  Den här porten är på en ILB-aktiverade ASE bundna toohello ILB adressen hello ASE.
* 443: standardport för inkommande SSL-trafik tooapps körs i App Service-planer i en Apptjänst-miljö.  Den här porten är på en ILB-aktiverade ASE bundna toohello ILB adressen hello ASE.
* 21: kontrollkanal för FTP.  Den här porten blockeras på ett säkert sätt om FTP inte används.  Den här porten kan vara bundna toohello ILB-adressen för en ASE på en ILB-aktiverade ASE.
* 990: kontrollkanal för FTPS.  Den här porten blockeras på ett säkert sätt om FTPS inte används.  Den här porten kan vara bundna toohello ILB-adressen för en ASE på en ILB-aktiverade ASE.
* 10001 10020: datakanaler för FTP.  Som med hello kontrollkanal blockeras dessa portar på ett säkert sätt om FTP inte används.  På en ASE ILB-aktiverad, kan den här porten vara bundna toohello ASE'S ILB adress.
* 4016: används för fjärrfelsökning med Visual Studio 2012.  Den här porten blockeras på ett säkert sätt om hello-funktionen inte används.  Den här porten är på en ILB-aktiverade ASE bundna toohello ILB adressen hello ASE.
* 4018: används för fjärrfelsökning med Visual Studio 2013.  Den här porten blockeras på ett säkert sätt om hello-funktionen inte används.  Den här porten är på en ILB-aktiverade ASE bundna toohello ILB adressen hello ASE.
* 4020: används för fjärrfelsökning med Visual Studio 2015.  Den här porten blockeras på ett säkert sätt om hello-funktionen inte används.  Den här porten är på en ILB-aktiverade ASE bundna toohello ILB adressen hello ASE.

## <a name="outbound-connectivity-and-dns-requirements"></a>Utgående anslutning och DNS-krav
För en Apptjänstmiljö toofunction korrekt, kräver det också utgående åtkomst toovarious slutpunkter. En fullständig lista över hello externa slutpunkter som används av en ASE är i hello nätverksanslutning ”krävs” avsnittet av hello [nätverkskonfigurationen för ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikel.

Apptjänstmiljöer kräver en giltig DNS-infrastruktur som konfigurerats för hello virtuellt nätverk.  Om DNS-konfiguration ändras när en Apptjänst-miljö har skapats i någon orsak hello, kan utvecklare tvinga en Apptjänstmiljö toopick hello nya DNS-konfiguration.  Utlöser en rullande miljö omstart med hjälp av hello ”Restart” finns hello överst på bladet hantering av hello Apptjänstmiljö i hello [Azure-portalen] [ NewPortal] kommer hello miljö toopick hello nya DNS-konfiguration.

Vi rekommenderar också att alla anpassade DNS-servrar för hello virtuella nätverk konfigureras i tid tidigare toocreating en Apptjänst-miljö.  Om DNS-konfiguration för ett virtuellt nätverk har ändrats medan en Apptjänst-miljö skapas, vilket leder till att hello Apptjänstmiljö skapa processen misslyckas.  I en liknande vein är andra änden av en VPN-gateway och hello DNS-server kan inte nås eller otillgängliga hello Apptjänstmiljö processen fungerar inte om en anpassad DNS-server finns på hello.

## <a name="creating-a-network-security-group"></a>Skapa en Nätverkssäkerhetsgrupp
Mer information om hur nätverkssäkerhet grupper arbetar finns hello följande [information][NetworkSecurityGroups].  hello Azure Service Management exemplet nedan finputsningen på visar för nätverkssäkerhetsgrupper med fokus på Konfigurera och tillämpa en säkerhet grupp tooa undernät som innehåller en Apptjänst-miljö.

**Obs:** nätverkssäkerhetsgrupper kan konfigureras med grafiskt hello [Azure Portal](https://portal.azure.com) eller via Azure PowerShell.

Nätverkssäkerhetsgrupper skapas först som en fristående enhet som är associerad med en prenumeration. Eftersom nätverkssäkerhetsgrupper skapas i en Azure-region, kontrollera att hello nätverkssäkerhetsgruppen skapas i hello samma region som hello Apptjänst-miljö.

hello nedan visar hur du skapar en nätverkssäkerhetsgrupp:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

När du har skapat en nätverkssäkerhetsgrupp läggs en eller flera Nätverkssäkerhetsregler tooit.  Eftersom hello uppsättning regler som kan ändras med tiden rekommenderas toospace ut hello numrering schema används för regeln prioriteter toomake den ytterligare regler för enkelt tooinsert över tid.

hello exemplet nedan visar en regel som uttryckligen beviljar åtkomst toohello management portar som krävs av hello Azure-infrastrukturen toomanage och underhålla en Apptjänst-miljö.  Observera att alla hantering av trafik som flödar över SSL och skyddas av klientcertifikat, så att även om hello portar är öppna de är tillgängligt som en annan entitet än Azure hanteringsinfrastruktur.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP


När låsa åtkomst tooport 80 och 443 för ”dölja” en Apptjänst-miljö bakom överordnade enheter eller tjänster, behöver du tooknow hello överordnade IP-adress.  Till exempel om du använder en brandvägg för webbaserade program (Brandvägg) hello Brandvägg ska ha sin egen IP-adress (eller adresserna) som används när proxyanslutning trafik tooa underordnade Apptjänstmiljö.  Du behöver toouse denna IP-adress i hello *SourceAddressPrefix* -parametern för en nätverkssäkerhetsregeln.

Inkommande trafik från en specifik överordnade IP-adress är uttryckligen tillåts i hello exemplet nedan.  Hej adress *1.2.3.4* används som platshållare för hello IP-adressen för en överordnad Brandvägg.  Ändra hello värdet toomatch hello adress som används av din överordnade enhet eller tjänst.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Om FTP-stöd önskas kan hello enligt reglerna användas som en mall toogrant toohello FTP-kontrollen åtkomstport och data channel-portar.  Eftersom FTP är ett tillståndskänslig protokoll, kanske inte kan tooroute FTP-trafik via en traditionell HTTP/HTTPS brandvägg eller proxyserver enhet.  I det här fallet behöver du tooset hello *SourceAddressPrefix* tooa annat värde – till exempel hello IP-adressintervall utvecklare eller distribution datorer som kör på vilken FTP-klienter. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Obs:** hello portintervall för datakanal kan ändras under hello förhandsversionen.)

Om fjärrfelsökning med Visual Studio används, visar hello enligt reglerna för hur toogrant åt.  Det finns en separat regel för varje version av Visual Studio som stöds eftersom varje version använder en annan port för fjärrfelsökning.  Precis som med FTP-åtkomst kan remote felsökning inte trafiken korrekt via en traditionell Brandvägg eller proxyserver enhet.  Hej *SourceAddressPrefix* kan i stället anges toohello IP-adressintervall developer-datorer som kör Visual Studio.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-tooa-subnet"></a>Tilldela en Nätverkssäkerhetsgrupp tooa undernät
En nätverkssäkerhetsgrupp har en standardsäkerhetsregel som nekar åtkomst tooall externa trafiken.  Hej resultatet från att kombinera hello Nätverkssäkerhetsregler som beskrivs ovan, och hello standardsäkerhetsregel blockerar inkommande trafik, är endast trafik från käll-adressintervall som är associerade med en *Tillåt* åtgärd ska kunna toosend trafik tooapps körs i en Apptjänst-miljö.

När en nätverkssäkerhetsgrupp fylls med säkerhetsregler måste toobe tilldelade toohello undernät som innehåller hello Apptjänst-miljö.  hello tilldelning kommandot refererar till både hello namn i hello virtuellt nätverk där hello Apptjänst-miljö finns, samt hello namnet på hello undernät där hello Apptjänstmiljö skapades.  

hello exemplet nedan visar en nätverkssäkerhetsgrupp som tilldelas tooa undernät och virtuella nätverk:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

När hello network security grupptilldelning lyckas (hello tilldelning är en långvariga åtgärder och kan ta några minuter toocomplete) bara inkommande trafik matchar *Tillåt* regler ska komma åt appar i hello App -Miljö.

Följande exempel visar hur tooremove och därmed DIS associera hello nätverkssäkerhet gruppen från hello undernät för fullständiga hello:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Speciella överväganden vid Explicit IP-SSL
Om en app har konfigurerats med en explicit SSL-IP-adress (tillämpliga *endast* tooASEs som har en offentlig VIP), istället för att använda hello standard IP-adressen för hello Apptjänstmiljö både HTTP och HTTPS-trafik, flöden i hello undernät via en annan uppsättning andra portar än portarna 80 och 443.

hello enskilda par portar som används av varje SSL-IP-adress kan hittas i hello portalens användargränssnitt från hello Apptjänstmiljös UX informationsbladet.  Välj ”alla inställningar”--> ”IP-adresser”.  Hej ”IP-adresser” bladet innehåller en tabell med alla uttryckligen konfigurerade SSL-IP-adresser för hello Apptjänst-miljön, tillsammans med hello Särskild port-par som används tooroute HTTP och HTTPS-trafik som är associerade med varje SSL-IP-adress.  Det är detta port-par som behöver toobe som används för hello DestinationPortRange parametrar när du konfigurerar regler i en nätverkssäkerhetsgrupp.

När en app på en ASE är konfigurerade toouse IP-SSL, visas inte externa kunder och behöver inte tooworry om hello särskilda par portmappning.  Trafik toohello appar flödar normalt toohello konfigurerade SSL-IP-adress.  hello översättning toohello särskilda port par sker automatiskt internt under hello sista delen av dirigera trafiken till hello undernät som innehåller hello ASE. 

## <a name="getting-started"></a>Komma igång
tooget igång med Apptjänstmiljöer, se [introduktion tooApp-miljö][IntroToAppServiceEnvironment]

Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

Mer information kring appar på ett säkert sätt ansluta toobackend resurs från en Apptjänst-miljö finns [på ett säkert sätt ansluta tooBackend resurser från en Apptjänst-miljö][SecurelyConnecttoBackend]

Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->

