---
title: aaaIntroduction tooAzure Programgateway | Microsoft Docs
description: "Den här sidan innehåller en översikt över hello Programgateway service layer 7 belastningsutjämning, inklusive gateway storlekar, HTTP läsa in belastningsutjämning, cookie-baserad session tillhörighet och SSL-avlastning."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: b37a2473-4f0e-496b-95e7-c0594e96f83e
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: c40c9dba64ab03d9f6f81b3cb8f26c6562ac26c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-application-gateway"></a>Översikt över Application Gateway

Microsoft Azure Application Gateway är en dedikerad virtuell installation som tillhandahåller ADC (Application Delivery Controller) som en tjänst. Tjänsten erbjuder olika Layer 7-belastningsutjämningsfunktioner för ditt program. Det gör att kunder toooptimize web servergruppen produktivitet genom att avlasta CPU beräkningsintensiva SSL-avslutning toohello Programgateway. Här finns även andra layer 7 funktioner inklusive resursallokering distribution av inkommande trafik, cookie-baserad session tillhörighet, URL-sökväg-baserad Routning och hello möjlighet toohost flera webbplatser bakom en enda Application Gateway. Det finns också en brandvägg för webbaserade program (Brandvägg) som en del av hello Programgateway Brandvägg SKU. Det ger skydd tooweb program från vanliga web säkerhetsrisker och trojaner. Application Gateway kan konfigureras som internetuppkopplad gateway, endast intern gateway eller en kombination av båda. 

![scenario](./media/application-gateway-introduction/scenario.png)

## <a name="features"></a>Funktioner

Programgateway innehåller för närvarande hello följande funktioner:


* **[Brandvägg för webbaserade program](application-gateway-webapplicationfirewall-overview.md)**  -hello Brandvägg för webbaserade program (Brandvägg) i Azure Application Gateway skyddar webbprogram från vanliga webbaserade attacker som SQL injection, webbplatser scripting attacker och sessionen hijacks.
* **HTTP-belastningsutjämning** –Application Gateway erbjuder belastningsutjämning med resursallokering. Belastningsutjämning görs Layer 7 och används enbart för HTTP(S)-trafik.
* **Cookie-baserad session tillhörighet** -hello cookie-baserad session tillhörighet funktionen är praktisk när du vill tookeep en användarsession på hello samma backend. Med hjälp av gateway-hanterade cookies är hello Programgateway kan toodirect efterföljande trafik från en användare session toohello samma backend för bearbetning. Den här funktionen är viktigt i fall där sessionsläget sparas lokalt på hello backend-server för en användarsession.
* **[Secure Sockets Layer (SSL)-avlastning](application-gateway-ssl-arm.md)**  -den här funktionen tar hello kostsamma uppgiften att dekryptera HTTPS-trafik av webbservrar. Av avslutande hello SSL-anslutningen vid hello Programgateway och vidarebefordran hello begäran toohello server okrypterade unburdened hello webbservern av dekryptering.  Programgateway krypterar hello svar innan du skickar den bakre toohello klienten. Den här funktionen är användbart i scenarier där hello backend-finns i hello samma skyddade virtuella nätverk som hello Programgateway i Azure.
* **[Avsluta tooEnd SSL](application-gateway-backend-ssl.md)**  -stöd för Programgateway avslutas tooend kryptering av trafik. Programgateway gör detta genom att avsluta hello SSL-anslutningen vid hello Programgateway. hello gateway gäller hello routningsregler toohello trafik, krypterar hello paket och vidarebefordrar hello paket toohello lämplig backend utifrån hello routning regler. Alla svar från webbservern hello passerar hello samma process tillbaka toohello slutanvändaren.
* **[URL-baserade innehåll routning](application-gateway-url-route-overview.md)**  -den här funktionen ger hello toouse olika backend-servrar för olika trafiktyper. Trafik för en mapp på hello webbservern eller en CDN kan vara routade tooa olika serverdel. Den här funktionen minskar onödig belastning på servrar som inte hanterar specifikt innehåll.
* **[Flera platser routning](application-gateway-multi-site-overview.md)**  -Programgateway tillåter tooconsolidate in too20 webbplatser på en enda Programgateway.
* **[Stöd för Websocket](application-gateway-websocket.md)**  -en annan bra funktion för Programgateway är hello inbyggt stöd för Websocket.
* **[Hälsoövervakning](application-gateway-probe-overview.md)**  -Application gateway ger standard övervakning av serverdelsresurser och anpassade hälsoavsökning toomonitor för mer specifika scenarier.
* **[SSL-Policy och chiffer](application-gateway-ssl-policy-overview.md)**  – den här funktionen ger möjlighet för hello toolimit hello SSL-protokollversioner och hello chiffer paket som stöds och hello ordning de bearbetas.
* **[Begäran om omdirigering](application-gateway-redirect-overview.md)**  -den här funktionen ger hello kapaciteten tooredirect HTTP-begäranden tooan HTTPS-lyssnare.
* **[Stöd för serverdelar för flera klientorganisationer](application-gateway-web-app-overview.md)** – Application Gateway stöder konfiguration av serverdelstjänster för flera klientorganisationer som Azure Web Apps och API Gateway som medlemmar i en serverdelspool. 
* **[Avancerad diagnostik](application-gateway-diagnostics.md)** – Application gateway ger fullständig diagnostik och åtkomst till loggar. Brandväggsloggar är tillgängliga för Application Gateway-resurser som har WAF aktiverat.

## <a name="benefits"></a>Fördelar

Application Gateway är användbar för:

* Program som kräver begäranden från hello samma användare eller-klienten session tooreach hello samma backend-virtuella dator. Exempel på sådana applikationer kan vara shoppingvagnsprogram och e-postwebbservrar.
* Tar bort omkostnader för SSL-avslutning för webbservergrupper.
* Program, till exempel ett nätverk för innehållsleverans som kräver flera HTTP-förfrågningar på hello samma tidskrävande TCP-anslutning toobe dirigerade eller läsa in belastningsutjämnade toodifferent backend-servrar.
* Program som stöder websocket-trafik
* Skydda webbprogrammen från vanliga webbaserade attacker som SQL-injection, cross-site skriptattacker och sessionsövertaganden.
* Logisk distribuering av trafik baserat på olika dirigeringsvillkor som url-sökväg eller domänrubriker.

Application Gateway är helt Azure-hanterat, skalbart och med hög tillgänglighet. För att få en bättre hantering ingår en omfattande uppsättning diagnostik- och loggningsfunktioner. När du skapar en programgateway, kopplas en slutpunkt (offentlig VIP eller intern ILB IP) och används för inkommande nätverkstrafik. Denna VIP eller ILB IP tillhandahålls av Azure belastningsutjämnare fungerar på transportnivå hello (TCP/UDP) och att all inkommande trafik som belastningen belastningsutjämnade toohello Programgateway worker-instanser. Hej Programgateway sedan vägar hello HTTP/HTTPS-trafik baserat på dess konfiguration om det är en virtuell dator, cloud service interna eller externa IP-adressen.

Programgateway belastningsutjämning som en tjänst för Azure-hanterade tillåter hello etablering av en lager 7 belastningsutjämnare bakom hello Azure programbelastningsutjämnaren. Traffic manager kan vara används toocomplete hello scenariot som visas i följande bild, där Traffic Manager tillhandahåller omdirigering och tillgängligheten för trafik toomultiple application gateway-resurser i olika regioner, medan Programgateway innehåller hello mellan region layer 7 belastningsutjämning. Ett exempel på det här scenariot finns på: [med hjälp av tjänster i hello Azure-molnet för belastningsutjämning](../traffic-manager/traffic-manager-load-balancing-azure.md)

![scenario för traffic manager och application gateway](./media/application-gateway-introduction/tm-lb-ag-scenario.png)

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Gateway-storlekar och instanser

Application Gateway finns för närvarande i tre storlekar: **liten**, **medel** och **stor**. Smål instansstorlekar är avsedda för utvecklings- och testningsscenarier.

Du kan skapa upp too50 programgatewayer per prenumeration och varje Programgateway kan ha upp too10 instanser. Varje Application Gateway kan bestå av 20 http-lyssnare. En fullständig lista över gränserna för programgateways finns i avsnittet om [gränser för Application Gateway-tjänsten](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

hello följande tabell visas en genomsnittlig prestanda genomströmning för varje program gateway-instans med SSL-avlastning aktiverad:

| Sidsvar för serverdel | Liten | Medel | Stor |
| --- | --- | --- | --- |
| 6K |7.5 Mbit/s |13 Mbit/s |50 Mbit/s |
| 100K |35 Mbit/s |100 Mbit/s |200 Mbit/s |

> [!NOTE]
> De här värdena är genomsnittliga värden för ett Application Gateway-dataflöde. hello faktiska beror dataflödet på olika miljö information, till exempel genomsnittlig storlek, platsen för backend-instanser och bearbetning tid tooserve en sida. Du bör köra egna test för exakta prestandavärden. Dessa värden är bara för vägledning vid kapacitetsplanering.

## <a name="health-monitoring"></a>Hälsoövervakning

Azure Application Gateway övervakar automatiskt hello hälsotillstånd hello backend-instanser via grundläggande eller anpassade hälsoavsökningar. Med hjälp av hälsoavsökningar, ser till att endast felfri värdar svarar tootraffic. Mer information finns i [Översikt över Application Gateway hälsoövervakning](application-gateway-probe-overview.md).

## <a name="configuring-and-managing"></a>Konfigurera och hantera

Som slutpunkt kan Application Gateway ha en offentlig eller privat IP-Adress, eller bägge när det är konfigurerat. Application Gateway konfigureras inuti ett virtuellt nätverk i sitt eget undernät. hello undernät skapas eller användas för Programgateway får inte innehålla andra typer av resurser, hello endast resurser som är tillåtna i hello undernät är andra programgatewayer. toosecure backend resurser, hello backend-servrar kan finnas i ett annat undernät i hello samma virtuella nätverk som hello Programgateway. Det här undernätet inte krävs för hello backend-program. Så länge hello Programgateway kan nå hello IP-adress, är Programgateway kan tooprovide ADC funktioner för hello backend-servrar. 

Du kan skapa och hantera en Application Gateway med hjälp av REST API:er, PowerShell-cmdletar, Azure CLI eller [Azure Portal](https://portal.azure.com/). Ytterligare frågor om Programgateway finns [programmet Gateway FAQ](application-gateway-faq.md) tooview en lista över vanliga vanliga frågor och svar.

## <a name="pricing"></a>Prissättning

Priserna är baserade på avgifter per gatewayinstans per timme och databearbetningsavgifter. Per timme skiljer gateway priser för hello Brandvägg SKU sig från Standard-SKU avgifter. Du hittar den här prisinformationen i [Priser för Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/). Databearbetning kostnader förblir hello samma.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

Vanliga frågor om Application Gateway finns i [Vanliga frågor och svar om Application Gateway](application-gateway-faq.md).

## <a name="next-steps"></a>Nästa steg

När du lära dig mer om Programgateway, kan du [skapa en Programgateway](application-gateway-create-gateway-portal.md) eller så kan du [skapa en Programgateway SSL-avlastning](application-gateway-ssl-arm.md) tooload saldo HTTPS-anslutningar.

toolearn hur toocreate en Programgateway med hjälp av URL-baserade innehåll routning gå för[skapa en Programgateway med hjälp av URL-baserade routning](application-gateway-create-url-route-arm-ps.md) för mer information.

toolearn om några av Hej andra nyckeln nätverk funktionerna i Azure, se [Azure nätverk](../networking/networking-overview.md).
