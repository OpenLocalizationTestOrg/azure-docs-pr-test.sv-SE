---
title: "aaaConfiguring en Web programmet Firewall (Brandvägg) för Apptjänst-miljö"
description: "Lär dig hur tooconfigure ett webbprogram i brandväggen framför din Apptjänst-miljö."
services: app-service\web
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: naziml
ms.openlocfilehash: 0fcf62aea871751c9d4f294d2d24df2186fc0e7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurera en brandvägg för webbaserade program (Brandvägg) för Apptjänst-miljö
## <a name="overview"></a>Översikt
Web application brandväggar som hello [Barracuda Brandvägg för Azure](https://www.barracuda.com/programs/azure) som är tillgänglig på hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) skyddar dina webbprogram genom att kontrollera inkommande web trafik tooblock SQL injektioner, globala webbplatsskript, skadlig kod överföringar & DDoS-program och andra attacker. Den kontrollerar också hello svar från hello backend-webbservrar för Data går förlorade förebyggande (DLP). I kombination med hello isolering och ytterligare skalning som tillhandahålls av Apptjänstmiljöer, ger detta en perfekt miljö toohost business kritiska webbprogram som behöver toowithstand skadliga begäranden och hög trafik.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Konfiguration
För det här dokumentet som vi konfigurerar balanserade våra Apptjänstmiljö bakom flera belastningen instanser av Barracuda Brandvägg så att endast trafik från hello Brandvägg kan nå hello Apptjänst-miljö och det inte är tillgänglig från hello DMZ. Vi har även Azure Traffic Manager framför våra Barracuda Brandvägg instanser tooload balans över Azure-datacenter och regioner. En hög nivå diagram över hello installationen skulle se ut vad som anges nedan.

![Arkitektur][Architecture] 

> Obs: med hello införandet av [ILB stöd för Apptjänst-miljö](app-service-environment-with-internal-load-balancer.md), kan du konfigurera hello ASE toobe inte nås från hello DMZ och vara tillgängliga toohello privat nätverk. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Konfigurera din Apptjänst-miljö
tooconfigure en Apptjänst-miljö finns för[vår dokumentation](app-service-web-how-to-create-an-app-service-environment.md) på hello ämne. När du har en Apptjänst-miljö skapas, kan du skapa [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) och [Mobilappar](../app-service-mobile/app-service-mobile-value-prop.md) i den här miljön kommer alla skyddas bakom hello Brandvägg vi Konfigurera i hello nästa avsnitt.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfigurera Barracuda Brandvägg Molntjänsten
Barracuda har en [detaljerad artikel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) om distribution av dess Brandvägg på en virtuell dator i Azure. Men eftersom vi vill redundans och inte inför en enskild felpunkt toodeploy minst 2 Brandvägg instans virtuella datorer i hello samma molntjänst när följa dessa anvisningar.

### <a name="adding-endpoints-toocloud-service"></a>Att lägga till slutpunkter tooCloud Service
När du har 2 eller mer Brandvägg VM-instanser i din molntjänst kan du använda hello [Azure-portalen](https://portal.azure.com/) tooadd HTTP och HTTPS-slutpunkter som används av ditt program enligt hello bilden nedan.

![Konfigurera slutpunkt][ConfigureEndpoint]

Om dina program använder slutpunkter kontrollerar du att tooadd samt de toothis listan. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurera Barracuda Brandvägg via dess hanteringsportalen
Barracuda Brandvägg använder TCP-Port 8000 för konfigurationen med hjälp av dess hanteringsportalen. Eftersom vi har flera instanser av virtuella datorer hello-Brandvägg måste toorepeat hello här stegen för varje VM-instans. 

> Obs: När du är klar med konfigurationen av Brandvägg, ta bort hello TCP/8000 endpoint från din Brandvägg VMs tookeep din Brandvägg säker.
> 
> 

Lägg till hello hanteringsslutpunkten enligt hello bild nedan tooconfigure Barracuda-Brandvägg.

![Lägg till slutpunkt för hantering][AddManagementEndpoint]

Använd en webbläsare toobrowse toohello management slutpunkt på Molntjänsten. Om din molntjänst anropas test.cloudapp.net, skulle du åtkomst till den här slutpunkten genom att bläddra toohttp://test.cloudapp.net:8000. Du bör se en inloggningssida som nedan kan logga in med autentiseringsuppgifterna som du angav i hello Brandvägg VM installationsfasen.

![Hantering av inloggningssidan][ManagementLoginPage]

När inloggningen bör du se en instrumentpanel som hello i hello bilden nedan som presenterar grundläggande statistik om hello Brandvägg skydd.

![Instrumentpanel för hantering][ManagementDashboard]

Klicka på fliken för hello-tjänster kan du konfigurera din Brandvägg för tjänster som skyddas. Mer information om hur du konfigurerar din Brandvägg Barracuda finns [deras dokumentation](https://techlib.barracuda.com/waf/getstarted1). I exemplet hello under en Azure-Webbapp har trafik på HTTP och HTTPS konfigurerats.

![Hantering av lägga till tjänster][ManagementAddServices]

> Obs: Beroende på hur dina program är konfigurerade och vilka funktioner som används i din Apptjänst-miljö, behöver du tooforward trafik för TCP-portar än 80 och 443, t.ex. Om du har IP SSL-inställningar för ett webbprogram. En lista över nätverksportar som används i Apptjänstmiljöer finns för[kontroll för inkommande trafik dokumentationen](app-service-app-service-environment-control-inbound-traffic.md) nätverksportar avsnitt.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurera Microsoft Azure Traffic Manager (valfritt)
Om ditt program är tillgängligt i flera områden, så att du vill ha tooload saldo dem bakom [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). toodo så att du kan lägga till en slutpunkt i hello [klassiska Azure-portalen](https://manage.azure.com) använda hello Molntjänsten namn för din Brandvägg i hello Traffic Manager-profilen som visas i hello bilden nedan. 

![Traffic Manager-slutpunkt][TrafficManagerEndpoint]

Om ditt program kräver autentisering, se till att du har en resurs som inte kräver någon autentisering för Traffic Manager tooping hello tillgänglighet för ditt program. Du kan konfigurera hello URL under hello avsnittet Konfigurera på hello [klassiska Azure-portalen](https://manage.azure.com) enligt nedan.

![Konfigurera Traffic Manager][ConfigureTrafficManager]

tooforward hello Traffic Manager-ping från din Brandvägg tooyour program, behöver du toosetup webbplats översättningar till Barracuda Brandvägg tooforward trafik tooyour programmet enligt hello exemplet nedan.

![Webbplatsen översättningar][WebsiteTranslations]

## <a name="securing-traffic-tooapp-service-environment-using-network-security-groups-nsg"></a>Att säkra trafiken tooApp Service miljö med hjälp av Nätverkssäkerhetsgrupp grupper (NSG)
Följ hello [kontroll för inkommande trafik dokumentationen](app-service-app-service-environment-control-inbound-traffic.md) för information om hur du begränsar trafiken tooyour Apptjänstmiljö från hello Brandvägg med hello VIP-adressen för din tjänst i molnet. Här är ett exempel Powershell-kommando för att utföra den här aktiviteten för TCP-port 80.

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Ersätt hello SourceAddressPrefix med hello virtuella IP-adress (VIP) för din Brandvägg Molntjänsten.

> Obs: hello VIP för Molntjänsten ändras när du tar bort och återskapa hello tjänst i molnet. Kontrollera att tooupdate hello IP-adressen i hello nätverket resursgrupp när du gör detta. 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
