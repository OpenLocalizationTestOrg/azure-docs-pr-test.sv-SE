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
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a><span data-ttu-id="580b3-103">Konfigurera en brandvägg för webbaserade program (Brandvägg) för Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="580b3-103">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="580b3-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="580b3-104">Overview</span></span>
<span data-ttu-id="580b3-105">Web application brandväggar som hello [Barracuda Brandvägg för Azure](https://www.barracuda.com/programs/azure) som är tillgänglig på hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) skyddar dina webbprogram genom att kontrollera inkommande web trafik tooblock SQL injektioner, globala webbplatsskript, skadlig kod överföringar & DDoS-program och andra attacker.</span><span class="sxs-lookup"><span data-stu-id="580b3-105">Web application firewalls like hello [Barracuda WAF for Azure](https://www.barracuda.com/programs/azure) that is available on hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) helps secure your web applications by inspecting inbound web traffic tooblock SQL injections, Cross-Site Scripting, malware uploads & application DDoS and other attacks.</span></span> <span data-ttu-id="580b3-106">Den kontrollerar också hello svar från hello backend-webbservrar för Data går förlorade förebyggande (DLP).</span><span class="sxs-lookup"><span data-stu-id="580b3-106">It also inspects hello responses from hello back-end web servers for Data Loss Prevention (DLP).</span></span> <span data-ttu-id="580b3-107">I kombination med hello isolering och ytterligare skalning som tillhandahålls av Apptjänstmiljöer, ger detta en perfekt miljö toohost business kritiska webbprogram som behöver toowithstand skadliga begäranden och hög trafik.</span><span class="sxs-lookup"><span data-stu-id="580b3-107">Combined with hello isolation and additional scaling provided by App Service Environments, this provides an ideal environment toohost business critical web applications that need toowithstand malicious requests and high volume traffic.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a><span data-ttu-id="580b3-108">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="580b3-108">Setup</span></span>
<span data-ttu-id="580b3-109">För det här dokumentet som vi konfigurerar balanserade våra Apptjänstmiljö bakom flera belastningen instanser av Barracuda Brandvägg så att endast trafik från hello Brandvägg kan nå hello Apptjänst-miljö och det inte är tillgänglig från hello DMZ.</span><span class="sxs-lookup"><span data-stu-id="580b3-109">For this document we will configure our App Service Environment behind multiple load balanced instances of Barracuda WAF so that only traffic from hello WAF can reach hello App Service Environment and it will not be accessible from hello DMZ.</span></span> <span data-ttu-id="580b3-110">Vi har även Azure Traffic Manager framför våra Barracuda Brandvägg instanser tooload balans över Azure-datacenter och regioner.</span><span class="sxs-lookup"><span data-stu-id="580b3-110">We will also have Azure Traffic Manager in front of our Barracuda WAF instances tooload balance across Azure data centers and regions.</span></span> <span data-ttu-id="580b3-111">En hög nivå diagram över hello installationen skulle se ut vad som anges nedan.</span><span class="sxs-lookup"><span data-stu-id="580b3-111">A high level diagram of hello setup would look like what is shown below.</span></span>

![Arkitektur][Architecture] 

> <span data-ttu-id="580b3-113">Obs: med hello införandet av [ILB stöd för Apptjänst-miljö](app-service-environment-with-internal-load-balancer.md), kan du konfigurera hello ASE toobe inte nås från hello DMZ och vara tillgängliga toohello privat nätverk.</span><span class="sxs-lookup"><span data-stu-id="580b3-113">Note: With hello introduction of [ILB support for App Service Environment](app-service-environment-with-internal-load-balancer.md), you can configure hello ASE toobe inaccessible from hello DMZ and only be available toohello private network.</span></span> 
> 
> 

## <a name="configuring-your-app-service-environment"></a><span data-ttu-id="580b3-114">Konfigurera din Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="580b3-114">Configuring your App Service Environment</span></span>
<span data-ttu-id="580b3-115">tooconfigure en Apptjänst-miljö finns för[vår dokumentation](app-service-web-how-to-create-an-app-service-environment.md) på hello ämne.</span><span class="sxs-lookup"><span data-stu-id="580b3-115">tooconfigure an App Service Environment refer too[our documentation](app-service-web-how-to-create-an-app-service-environment.md) on hello subject.</span></span> <span data-ttu-id="580b3-116">När du har en Apptjänst-miljö skapas, kan du skapa [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) och [Mobilappar](../app-service-mobile/app-service-mobile-value-prop.md) i den här miljön kommer alla skyddas bakom hello Brandvägg vi Konfigurera i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="580b3-116">Once you have an App Service Environment created, you can create [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) and [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) in this environment that will all be protected behind hello WAF we configure in hello next section.</span></span>

## <a name="configuring-your-barracuda-waf-cloud-service"></a><span data-ttu-id="580b3-117">Konfigurera Barracuda Brandvägg Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="580b3-117">Configuring your Barracuda WAF Cloud Service</span></span>
<span data-ttu-id="580b3-118">Barracuda har en [detaljerad artikel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) om distribution av dess Brandvägg på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="580b3-118">Barracuda has a [detailed article](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) on deploying its WAF on a virtual machine in Azure.</span></span> <span data-ttu-id="580b3-119">Men eftersom vi vill redundans och inte inför en enskild felpunkt toodeploy minst 2 Brandvägg instans virtuella datorer i hello samma molntjänst när följa dessa anvisningar.</span><span class="sxs-lookup"><span data-stu-id="580b3-119">But because we want redundancy and not introduce a single point of failure, you want toodeploy at least 2 WAF instance VMs into hello same Cloud Service when following these instructions.</span></span>

### <a name="adding-endpoints-toocloud-service"></a><span data-ttu-id="580b3-120">Att lägga till slutpunkter tooCloud Service</span><span class="sxs-lookup"><span data-stu-id="580b3-120">Adding Endpoints tooCloud Service</span></span>
<span data-ttu-id="580b3-121">När du har 2 eller mer Brandvägg VM-instanser i din molntjänst kan du använda hello [Azure-portalen](https://portal.azure.com/) tooadd HTTP och HTTPS-slutpunkter som används av ditt program enligt hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="580b3-121">Once you have 2 or more WAF VM instances in your Cloud Service you can use hello [Azure portal](https://portal.azure.com/) tooadd HTTP and HTTPS endpoints that are used by your application as shown in hello image below.</span></span>

![Konfigurera slutpunkt][ConfigureEndpoint]

<span data-ttu-id="580b3-123">Om dina program använder slutpunkter kontrollerar du att tooadd samt de toothis listan.</span><span class="sxs-lookup"><span data-stu-id="580b3-123">If your applications use other endpoints, make sure tooadd those toothis list as well.</span></span> 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a><span data-ttu-id="580b3-124">Konfigurera Barracuda Brandvägg via dess hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="580b3-124">Configuring Barracuda WAF through its Management Portal</span></span>
<span data-ttu-id="580b3-125">Barracuda Brandvägg använder TCP-Port 8000 för konfigurationen med hjälp av dess hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="580b3-125">Barracuda WAF uses TCP Port 8000 for configuration through its management portal.</span></span> <span data-ttu-id="580b3-126">Eftersom vi har flera instanser av virtuella datorer hello-Brandvägg måste toorepeat hello här stegen för varje VM-instans.</span><span class="sxs-lookup"><span data-stu-id="580b3-126">Since we have multiple instances of hello WAF VMs you will need toorepeat hello steps here for each VM instance.</span></span> 

> <span data-ttu-id="580b3-127">Obs: När du är klar med konfigurationen av Brandvägg, ta bort hello TCP/8000 endpoint från din Brandvägg VMs tookeep din Brandvägg säker.</span><span class="sxs-lookup"><span data-stu-id="580b3-127">Note: Once you are done with WAF configuration, remove hello TCP/8000 endpoint from all your WAF VMs tookeep your WAF secure.</span></span>
> 
> 

<span data-ttu-id="580b3-128">Lägg till hello hanteringsslutpunkten enligt hello bild nedan tooconfigure Barracuda-Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="580b3-128">Add hello management endpoint as shown in hello image below tooconfigure your Barracuda WAF.</span></span>

![Lägg till slutpunkt för hantering][AddManagementEndpoint]

<span data-ttu-id="580b3-130">Använd en webbläsare toobrowse toohello management slutpunkt på Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="580b3-130">Use a browser toobrowse toohello management endpoint on your Cloud Service.</span></span> <span data-ttu-id="580b3-131">Om din molntjänst anropas test.cloudapp.net, skulle du åtkomst till den här slutpunkten genom att bläddra toohttp://test.cloudapp.net:8000.</span><span class="sxs-lookup"><span data-stu-id="580b3-131">If your Cloud Service is called test.cloudapp.net, you would access this endpoint by browsing toohttp://test.cloudapp.net:8000.</span></span> <span data-ttu-id="580b3-132">Du bör se en inloggningssida som nedan kan logga in med autentiseringsuppgifterna som du angav i hello Brandvägg VM installationsfasen.</span><span class="sxs-lookup"><span data-stu-id="580b3-132">You should see a login page like below that you can login using credentials you specified in hello WAF VM setup phase.</span></span>

![Hantering av inloggningssidan][ManagementLoginPage]

<span data-ttu-id="580b3-134">När inloggningen bör du se en instrumentpanel som hello i hello bilden nedan som presenterar grundläggande statistik om hello Brandvägg skydd.</span><span class="sxs-lookup"><span data-stu-id="580b3-134">Once you login you should see a dashboard as hello one in hello image below that will present basic statistics about hello WAF protection.</span></span>

![Instrumentpanel för hantering][ManagementDashboard]

<span data-ttu-id="580b3-136">Klicka på fliken för hello-tjänster kan du konfigurera din Brandvägg för tjänster som skyddas.</span><span class="sxs-lookup"><span data-stu-id="580b3-136">Clicking on hello Services tab will let you configure your WAF for services it is protecting.</span></span> <span data-ttu-id="580b3-137">Mer information om hur du konfigurerar din Brandvägg Barracuda finns [deras dokumentation](https://techlib.barracuda.com/waf/getstarted1).</span><span class="sxs-lookup"><span data-stu-id="580b3-137">For more details on configuring your Barracuda WAF you can consult [their documentation](https://techlib.barracuda.com/waf/getstarted1).</span></span> <span data-ttu-id="580b3-138">I exemplet hello under en Azure-Webbapp har trafik på HTTP och HTTPS konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="580b3-138">In hello example below an Azure Web App serving traffic on HTTP and HTTPS has been configured.</span></span>

![Hantering av lägga till tjänster][ManagementAddServices]

> <span data-ttu-id="580b3-140">Obs: Beroende på hur dina program är konfigurerade och vilka funktioner som används i din Apptjänst-miljö, behöver du tooforward trafik för TCP-portar än 80 och 443, t.ex. Om du har IP SSL-inställningar för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="580b3-140">Note: Depending on how your applications are configured and what features are being used in your App Service Environment, you will need tooforward traffic for TCP ports other than 80 and 443, e.g. if you have IP SSL setup for a Web App.</span></span> <span data-ttu-id="580b3-141">En lista över nätverksportar som används i Apptjänstmiljöer finns för[kontroll för inkommande trafik dokumentationen](app-service-app-service-environment-control-inbound-traffic.md) nätverksportar avsnitt.</span><span class="sxs-lookup"><span data-stu-id="580b3-141">For a list of network ports used in App Service Environments, please refer too[Control Inbound Traffic documentation's](app-service-app-service-environment-control-inbound-traffic.md) Network Ports section.</span></span>
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a><span data-ttu-id="580b3-142">Konfigurera Microsoft Azure Traffic Manager (valfritt)</span><span class="sxs-lookup"><span data-stu-id="580b3-142">Configuring Microsoft Azure Traffic Manager (OPTIONAL)</span></span>
<span data-ttu-id="580b3-143">Om ditt program är tillgängligt i flera områden, så att du vill ha tooload saldo dem bakom [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="580b3-143">If your application is available in multiple regions, then you would want tooload balance them behind [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span></span> <span data-ttu-id="580b3-144">toodo så att du kan lägga till en slutpunkt i hello [klassiska Azure-portalen](https://manage.azure.com) använda hello Molntjänsten namn för din Brandvägg i hello Traffic Manager-profilen som visas i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="580b3-144">toodo so you can add an endpoint in hello [Azure classic portal](https://manage.azure.com) using hello Cloud Service name for your WAF in hello Traffic Manager profile as shown in hello image below.</span></span> 

![Traffic Manager-slutpunkt][TrafficManagerEndpoint]

<span data-ttu-id="580b3-146">Om ditt program kräver autentisering, se till att du har en resurs som inte kräver någon autentisering för Traffic Manager tooping hello tillgänglighet för ditt program.</span><span class="sxs-lookup"><span data-stu-id="580b3-146">If your application requires authentication, ensure you have some resource that doesn't require any authentication for Traffic Manager tooping for hello availability of your application.</span></span> <span data-ttu-id="580b3-147">Du kan konfigurera hello URL under hello avsnittet Konfigurera på hello [klassiska Azure-portalen](https://manage.azure.com) enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="580b3-147">You can configure hello URL under hello Configure section on hello [Azure classic portal](https://manage.azure.com) as shown below.</span></span>

![Konfigurera Traffic Manager][ConfigureTrafficManager]

<span data-ttu-id="580b3-149">tooforward hello Traffic Manager-ping från din Brandvägg tooyour program, behöver du toosetup webbplats översättningar till Barracuda Brandvägg tooforward trafik tooyour programmet enligt hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="580b3-149">tooforward hello Traffic Manager pings from your WAF tooyour application, you need toosetup Website Translations on your Barracuda WAF tooforward traffic tooyour application as shown in hello example below.</span></span>

![Webbplatsen översättningar][WebsiteTranslations]

## <a name="securing-traffic-tooapp-service-environment-using-network-security-groups-nsg"></a><span data-ttu-id="580b3-151">Att säkra trafiken tooApp Service miljö med hjälp av Nätverkssäkerhetsgrupp grupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="580b3-151">Securing Traffic tooApp Service Environment Using Network Security Groups (NSG)</span></span>
<span data-ttu-id="580b3-152">Följ hello [kontroll för inkommande trafik dokumentationen](app-service-app-service-environment-control-inbound-traffic.md) för information om hur du begränsar trafiken tooyour Apptjänstmiljö från hello Brandvägg med hello VIP-adressen för din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="580b3-152">Follow hello [Control Inbound Traffic documentation](app-service-app-service-environment-control-inbound-traffic.md) for details on restricting traffic tooyour App Service Environment from hello WAF only by using hello VIP address of your Cloud Service.</span></span> <span data-ttu-id="580b3-153">Här är ett exempel Powershell-kommando för att utföra den här aktiviteten för TCP-port 80.</span><span class="sxs-lookup"><span data-stu-id="580b3-153">Here's a sample Powershell command for performing this task for TCP port 80.</span></span>

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

<span data-ttu-id="580b3-154">Ersätt hello SourceAddressPrefix med hello virtuella IP-adress (VIP) för din Brandvägg Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="580b3-154">Replace hello SourceAddressPrefix with hello Virtual IP Address (VIP) of your WAF's Cloud Service.</span></span>

> <span data-ttu-id="580b3-155">Obs: hello VIP för Molntjänsten ändras när du tar bort och återskapa hello tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="580b3-155">Note: hello VIP of your Cloud Service will change when you delete and re-create hello Cloud Service.</span></span> <span data-ttu-id="580b3-156">Kontrollera att tooupdate hello IP-adressen i hello nätverket resursgrupp när du gör detta.</span><span class="sxs-lookup"><span data-stu-id="580b3-156">Make sure tooupdate hello IP address in hello Network Resource group once you do so.</span></span> 
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
