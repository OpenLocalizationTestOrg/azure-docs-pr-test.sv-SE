---
title: "aaaIntroduction tooAzure apptjänstmiljöer"
description: "Kort översikt över Azure App Service-miljöer"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 9261041333cf59374974a039edf89c4983c45cdd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapp-service-environments"></a>Introduktion tooApp miljöer #
 
## <a name="overview"></a>Översikt ##

Azure Apptjänst-miljö är en funktion i Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för säker körning av Apptjänst-appar i hög skala. Den här funktionen kan vara värd för din [webbappar][webapps], [mobilappar][mobileapps], [API apps][APIapps], och [funktioner][Functions].

Apptjänstmiljöer (ASEs) som är lämpliga för arbetsbelastningar för program som kräver:

- Mycket hög skala.
- Isolering och säker nätverksåtkomst.
- Hög minnesanvändning.

Kunder kan skapa flera ASEs inom ett enda Azure-region eller över flera Azure-regioner. Den här flexibiliteten gör ASEs idealiskt för vågrätt skalning tillståndslös programnivåerna stöd för hög RPS arbetsbelastningar.

ASEs är isolerad toorunning de program som en enskild kund och alltid har distribuerats till ett virtuellt nätverk. Kunder har detaljerad kontroll över inkommande och utgående nätverkstrafik. Program kan upprätta säkra höghastighetsanslutning via VPN tooon lokala företagets resurser.

Alla artiklar och hur tooinstructions om ASEs är tillgängliga i hello [viktigt för apptjänstmiljöer][ASEReadme]:

* ASEs Aktivera hög skala appen värd med säker nätverksåtkomst. Mer information finns i hello [AzureCon ingående](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) på ASEs.
* Flera ASEs kan vara används tooscale vågrätt. Mer information finns i [hur tooset fördelade app utrymme](https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/).
* ASEs kan vara används tooconfigure säkerhetsarkitekturen enligt hello AzureCon ingående. toosee hur hello-säkerhetsarkitekturen som visas i hello AzureCon ingående har konfigurerats, se hello [artikel om hur tooimplement en skiktad säkerhetsarkitekturen](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-layered-security) med Apptjänst-miljöer.
* Appar som körs på ASEs kan behörighet gated av överordnad enheter, till exempel web application brandväggar (WAFs). Mer information finns i [konfigurera en Brandvägg för apptjänstmiljöer](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-app-service-environment-web-application-firewall).

## <a name="dedicated-environment"></a>Dedikerade miljön ##

En ASE är dedikerad enbart tooa enda prenumeration och kan vara värd för 100 instanser. hello-intervall kan sträcka sig över 100 instanser i en enda App Service-plan too100 single instance App Service-planer och allt i mellan.

En ASE består av frontwebbservrarna och personer. Frontwebbservrarna ansvarar för HTTP/HTTPS-avslutning och automatisk belastningsutjämning för app-förfrågningar inom en ASE. Frontwebbservrarna läggs automatiskt till som hello programtjänstplaner i hello ASE skalas ut.

Anställda är roller som värd för kund-appar. Anställda finns i tre fast storlek:

* En core/3.5 GB RAM-minne
* Två kärnor/7 GB RAM-minne
* Fyra kärnor/14 GB RAM-minne

Kunder behöver inte toomanage frontwebbservrarna och personer. Alla infrastruktur läggs automatiskt till som kunder skala ut sin App Service-planer. Som App Service planer skapas eller skalas i en ASE krävs hello infrastruktur läggs till eller tas bort efter behov.

Det finns en platt månatliga för en ASE som betalar för hello infrastruktur och ändras inte med hello storlek på hello ASE. Dessutom finns en kostnad per App Service-plan kärna. Alla appar som finns i en ASE finns i hello isolerade priser SKU. Information om priser för en ASE finns hello [priser för Apptjänst] [ Pricing] sidan och granska hello alternativen för ASEs.

## <a name="virtual-network-support"></a>Stöd för virtuella nätverk ##

En ASE kan bara skapas i ett virtuellt nätverk med Azure Resource Manager. toolearn mer om Azure-nätverk finns hello [Azure virtual networks vanliga frågor och svar](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). En ASE finns alltid i ett virtuellt nätverk och mer exakt i ett undernät i ett virtuellt nätverk. Du kan använda hello säkerhetsfunktionerna i virtuella nätverk toocontrol inkommande och utgående nätverk kommunikation för dina appar.

En ASE kan vara mot internet med en offentlig IP-adress eller internt riktade med endast en Azure internt (ILB)-adressen för belastningsutjämnaren.

[Nätverkssäkerhetsgrupper] [ NSGs] begränsa inkommande kommunikation toohello undernät där en ASE finns. Du kan använda NSG: er toorun appar bakom överordnade enheter och tjänster som WAFs och SaaS-leverantörer för nätverk.

Apparna måste också ofta tooaccess företagets resurser, till exempel interna databaser och webbtjänster. Om du distribuerar hello ASE i ett virtuellt nätverk som har en VPN-anslutning toohello lokalt nätverk hello appar i hello ASE kan komma åt hello lokala resurser. Den här funktionen gäller oavsett om hello VPN är en [plats-till-plats](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/) eller [Azure ExpressRoute](http://azure.microsoft.com/services/expressroute/) VPN.

Mer information om hur ASEs fungerar med virtuella nätverk och lokala nätverk finns [Apptjänstmiljö Nätverksöverväganden][ASENetwork].

## <a name="app-service-environment-v1"></a>App Service Environment v1 ##

Apptjänst-miljö har två versioner: ASEv1 och ASEv2. hello föregående information har baserat på ASEv2. Det här avsnittet visar hello av skillnaderna mellan ASEv1 och ASEv2. 

I ASEv1, behöver du toomanage alla hello resurser manuellt. Som innehåller hello frontwebbservrarna, personal och IP-adresser som används för IP-baserade SSL. Innan du kan skala ut din programtjänstplan måste toofirst skala ut hello arbetspool där du vill att toohost den.

ASEv1 använder en annan prisnivå modell från ASEv2. ASEv1 betalar du för varje kärna allokerade. Som innehåller kärnor som används för frontwebbservrarna eller personer som inte är värd för alla arbetsbelastningar. I ASEv1 är hello maximal skala standardstorlek en ASE 55 Totalt antal värdar. Som innehåller arbetare och frontwebbservrarna. En fördel tooASEv1 är kan distribueras i ett klassiskt virtuellt nätverk och ett virtuellt nätverk för hanteraren för filserverresurser. toolearn mer om ASEv1, se [Apptjänstmiljö v1 introduktion][ASEv1Intro].

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
