---
title: "aaaNetwork arkitektur översikt av Apptjänstmiljöer"
description: "Översikt över arkitekturen i nätverket topologi ofApp miljöer."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 13d03a37-1fe2-4e3e-9d57-46dfb330ba52
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 3cbc86883f5687a9ada35a3ab2f577a450a3fa0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="network-architecture-overview-of-app-service-environments"></a>Översikt över nätverksarkitekturen i App Service-miljöer
## <a name="introduction"></a>Introduktion
Apptjänstmiljöer skapas alltid i ett undernät för en [virtuellt nätverk] [ virtualnetwork] -appar som körs i en Apptjänst-miljö kan kommunicera med privata slutpunkter som ligger inom hello samma virtuella nätverkets topologi.  Eftersom kunder kan låsa delar av deras virtuella nätverksinfrastruktur, är det viktigt toounderstand hello typer av nätverksflöden för kommunikation som sker med en Apptjänst-miljö.

## <a name="general-network-flow"></a>Allmänna flödet för nätverk
När en App Service miljö (ASE) använder en offentlig virtuella IP-adress (VIP) för appar, kommer all inkommande trafik på den offentliga VIP.  Det inkluderar HTTP och HTTPS-trafik för appar, samt annan trafik för FTP, felsökning fjärrfunktioner och Azure hanteringsåtgärder.  En fullständig lista över hello specifika portar (obligatoriska och valfria) som är tillgängliga på hello offentliga VIP finns hello artikel på [styra inkommande trafik] [ controllinginboundtraffic] tooan Apptjänst-miljö. 

Apptjänstmiljöer stöder också körs appar som är bundna endast tooa virtuella datornätverket interna adressen kallas tooas ILB (intern belastningsutjämnare)-adressen.  På en ILB inkommer aktiverad ASE, HTTP och HTTPS-trafik för appar samt felsökning fjärranrop till hello ILB-adress.  För de vanligaste ILB ASE konfigurationer kommer också FTP-/ FTPS-trafik på hello ILB-adress.  Däremot aktiverad Azure hanteringsåtgärder flödar fortfarande tooports 454/455 på hello offentliga VIP för en ILB ASE.

hello diagrammet nedan visar en översikt över hello olika inkommande och utgående nätverksflöden för en Apptjänst-miljö där hello appar som är bundna tooa offentliga virtuella IP-adressen:

![Allmänna nätverksflöden][GeneralNetworkFlows]

En Apptjänst-miljö kan kommunicera med en mängd olika privata kunden slutpunkter.  Till exempel hello appar som körs i hello Apptjänst-miljö kan ansluta toodatabase servrar som körs på IaaS-virtuella datorer i samma virtuella nätverkstopologin.

> [!IMPORTANT]
> Titta på hello nätverksdiagram hello ”andra beräkna resurser” distribueras i ett annat undernät från hello Apptjänst-miljö. Distribuera resurser i hello blockerar samma undernät med hello ASE anslutningen från ASE toothose resurser (förutom för specifika intra-ASE routning). Distribuera tooa olika undernät i stället (i hello samma virtuella nätverk). Hej Apptjänstmiljö kommer sedan att kunna tooconnect. Det krävs ingen ytterligare konfiguration.
> 
> 

Apptjänstmiljöer också kommunicera med Sql-databas och Azure Storage resurser som krävs för att hantera och driva en Apptjänst-miljö.  Hello Sql- och resurser som en Apptjänst-miljö kommunicerar med finns i hello samma region som hello Apptjänst-miljön, medan andra finns i Azure-regioner.  Därför utgående anslutning toohello Internet krävs alltid för en Apptjänstmiljö toofunction korrekt. 

Eftersom en Apptjänst-miljö har distribuerats i ett undernät, nätverkssäkerhetsgrupper vara används toocontrol toohello undernätet för inkommande trafik.  Mer information om hur toocontrol inkommande trafik tooan Apptjänst-miljö finns hello följande [artikel][controllinginboundtraffic].

Mer information om hur tooallow utgående Internetanslutning från en Apptjänst-miljö, se följande artikel om hur du arbetar med hello [Expressroute][ExpressRoute].  hello samma metod som beskrivs i hello artikeln gäller när du arbetar med plats-till-plats-anslutning och använder Tvingad tunneltrafik.

## <a name="outbound-network-addresses"></a>Utgående nätverksadresser
När en Apptjänst-miljö gör utgående samtal, är alltid en IP-adress kopplad till hello utgående samtal.  hello specifik IP-adress som används beror på om hello-slutpunkt anropas finns i hello virtuella nätverkstopologin eller utanför hello virtuella nätverkstopologin.

Om hello-slutpunkt anropas är **utanför** hello virtuella nätverkets topologi och sedan hello utgående adress (aka hello utgående NAT-adress) som används är hello offentliga VIP för hello Apptjänst-miljö.  Den här adressen finns i hello portalens användargränssnitt för hello Apptjänstmiljö i egenskapsbladet.

![Utgående IP-adress][OutboundIPAddress]

Den här adressen kan även fastställas för ASEs som bara har en offentlig VIP genom att skapa en app i hello Apptjänst-miljö och utför sedan en *nslookup* hello app-adressen. hello gällande IP-adressen är både hello offentliga VIP samt hello Apptjänstmiljös utgående NAT-adress.

Om hello-slutpunkt anropas är **i** för hello virtuella nätverkets topologi hello utgående adress hello anropa app kommer att vara hello interna IP-adress hello enskilda beräkningsresurser kör hello app.  Men det är inte en beständig mappning av virtuellt nätverk interna IP-adresser tooapps.  Appar kan flytta över olika beräkningsresurser och hello-pool med tillgängliga beräkningsresurser i en Apptjänst-miljö kan ändra på grund av tooscaling åtgärder.

Eftersom en Apptjänst-miljö finns alltid i ett undernät kan garanteras du dock att hello interna IP-adressen för en beräkningsresurser som kör en app alltid ska ligga inom hello CIDR-intervall för hello undernät.  Därför när detaljerade ACL: er eller nätverkssäkerhetsgrupper används toosecure tooother slutpunkter inom hello virtuellt nätverk kan hello undernätsintervall som innehåller hello Apptjänstmiljö behov toobe beviljas åtkomst.

hello följande diagram visar dessa koncept i detalj:

![Utgående nätverksadresser][OutboundNetworkAddresses]

I hello ovan diagram:

* Eftersom hello offentliga VIP för hello Apptjänstmiljö 192.23.1.2, som är hello utgående IP-adress som används när du anropar för ”Internet” slutpunkter.
* hello CIDR-intervall för hello som innehåller undernät för hello Apptjänst-miljö är 10.0.1.0/26.  Slutpunkter inom hello samma infrastruktur för virtuellt nätverk ser anrop från appar som härrör från någonstans i den här adressintervallet.

## <a name="calls-between-app-service-environments"></a>Anrop mellan Apptjänstmiljöer
En mer komplex situationen kan uppstå om du distribuerar flera Apptjänstmiljöer i hello samma virtuella nätverk och utgående anrop från en Apptjänst-miljö tooanother Apptjänst-miljö.  Dessa typer av mellan Apptjänstmiljö anrop kommer också att behandlas som ”Internet” anrop.

hello följande diagram visar ett exempel på en skiktad arkitektur med appar på en Apptjänst-miljö (t.ex.) ”Dörren” web apps) anropa appar på en andra Apptjänst-miljö (t.ex. interna backend-API apps inte avsedda toobe som är tillgänglig från hello Internet). 

![Anrop mellan Apptjänstmiljöer][CallsBetweenAppServiceEnvironments] 

I hello har 192.23.1.2 utgående IP-adressen i exemplet ovan hello Apptjänstmiljö ”ASE en”.  Om en app som körs på den här Apptjänst-miljön gör en utgående samtal tooan app som körs på en andra Apptjänstmiljö (”ASE två”) finns i samma virtuella nätverk, hello hello utgående behandlas anropet som ett anrop för ”Internet”.  Därmed visas hello nätverkstrafik som inkommer på hello andra Apptjänstmiljö som härrör från 192.23.1.2 (d.v.s. inte hello undernät adressintervall hello första Apptjänst-miljö).

Även om anrop mellan olika Apptjänstmiljöer behandlas som ”Internet” anrop, när båda Apptjänstmiljöer befinner sig i hello samma Azure-region hello-nätverkstrafik finns kvar på hello regionala Azure-nätverk och flödar inte fysiskt över hello offentliga Internet.  Därför kan du använda en nätverkssäkerhetsgrupp på hello undernätet för hello hello andra Apptjänstmiljö tooonly tillåta inkommande anrop från första Apptjänst-miljö (vars utgående IP-adress är 192.23.1.2), därför att säkerställa säker kommunikation mellan hello Apptjänst-miljöer.

## <a name="additional-links-and-information"></a>Information och ytterligare länkar
Alla artiklar och hur-att datorns för Apptjänstmiljöer finns tillgängliga i hello [viktigt för Programtjänstmiljöer](../app-service/app-service-app-service-environments-readme.md).

Information om inkommande portar som används av Apptjänstmiljöer och använda network security grupper toocontrol inkommande trafik finns [här][controllinginboundtraffic].

Information om hur du använder användardefinierade vägar toogrant utgående Internet access tooApp miljöer finns i det här [artikel][ExpressRoute]. 

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

