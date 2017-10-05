---
title: "Konfigurera en brandvägg för webbaserade program (Brandvägg) för Apptjänst-miljö"
description: "Lär dig hur du konfigurerar en brandvägg för webbaserade program framför din Apptjänst-miljö."
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
ms.openlocfilehash: 3e9e9fa4ddab60a467e8aa793ec0ca269b0bc4e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a><span data-ttu-id="bfa22-103">Konfigurera en brandvägg för webbaserade program (Brandvägg) för Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="bfa22-103">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="bfa22-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="bfa22-104">Overview</span></span>
<span data-ttu-id="bfa22-105">Web application brandväggar som den [Barracuda Brandvägg för Azure](https://www.barracuda.com/programs/azure) som är tillgänglig på den [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) skyddar dina webbprogram genom att kontrollera inkommande webbtrafik att blockera SQL injektionerna globala webbplatsskript, skadlig kod överföringar & DDoS-program och andra attacker.</span><span class="sxs-lookup"><span data-stu-id="bfa22-105">Web application firewalls like the [Barracuda WAF for Azure](https://www.barracuda.com/programs/azure) that is available on the [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) helps secure your web applications by inspecting inbound web traffic to block SQL injections, Cross-Site Scripting, malware uploads & application DDoS and other attacks.</span></span> <span data-ttu-id="bfa22-106">Den kontrollerar också svar från backend-webbservrar för Data går förlorade förebyggande (DLP).</span><span class="sxs-lookup"><span data-stu-id="bfa22-106">It also inspects the responses from the back-end web servers for Data Loss Prevention (DLP).</span></span> <span data-ttu-id="bfa22-107">I kombination med isolering och ytterligare skalning som tillhandahålls av Apptjänstmiljöer, ger detta en perfekt miljö till värden kritiska web affärsprogram som måste klara skadliga begäranden och hög trafik.</span><span class="sxs-lookup"><span data-stu-id="bfa22-107">Combined with the isolation and additional scaling provided by App Service Environments, this provides an ideal environment to host business critical web applications that need to withstand malicious requests and high volume traffic.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a><span data-ttu-id="bfa22-108">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bfa22-108">Setup</span></span>
<span data-ttu-id="bfa22-109">För det här dokumentet som vi konfigurerar balanserade våra Apptjänstmiljö bakom flera belastningen instanser av Barracuda Brandvägg så att endast trafik från Brandvägg kan nå Apptjänst-miljön och det inte är tillgänglig från Perimeternätverket.</span><span class="sxs-lookup"><span data-stu-id="bfa22-109">For this document we will configure our App Service Environment behind multiple load balanced instances of Barracuda WAF so that only traffic from the WAF can reach the App Service Environment and it will not be accessible from the DMZ.</span></span> <span data-ttu-id="bfa22-110">Vi har även Azure Traffic Manager framför våra Barracuda Brandvägg instanser kan belastningsutjämna mellan Azure-datacenter och regioner.</span><span class="sxs-lookup"><span data-stu-id="bfa22-110">We will also have Azure Traffic Manager in front of our Barracuda WAF instances to load balance across Azure data centers and regions.</span></span> <span data-ttu-id="bfa22-111">En hög nivå diagram över installationen skulle se ut vad som anges nedan.</span><span class="sxs-lookup"><span data-stu-id="bfa22-111">A high level diagram of the setup would look like what is shown below.</span></span>

![Arkitektur][Architecture] 

> <span data-ttu-id="bfa22-113">Obs: med introduktionen av [ILB stöd för Apptjänst-miljö](app-service-environment-with-internal-load-balancer.md), kan du konfigurera ASE för att vara tillgänglig från Perimeternätverket och bara är tillgänglig för det privata nätverket.</span><span class="sxs-lookup"><span data-stu-id="bfa22-113">Note: With the introduction of [ILB support for App Service Environment](app-service-environment-with-internal-load-balancer.md), you can configure the ASE to be inaccessible from the DMZ and only be available to the private network.</span></span> 
> 
> 

## <a name="configuring-your-app-service-environment"></a><span data-ttu-id="bfa22-114">Konfigurera din Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="bfa22-114">Configuring your App Service Environment</span></span>
<span data-ttu-id="bfa22-115">Så här konfigurerar du en Apptjänst-miljö finns i [vår dokumentation](app-service-web-how-to-create-an-app-service-environment.md) om ämnet.</span><span class="sxs-lookup"><span data-stu-id="bfa22-115">To configure an App Service Environment refer to [our documentation](app-service-web-how-to-create-an-app-service-environment.md) on the subject.</span></span> <span data-ttu-id="bfa22-116">När du har en Apptjänst-miljö skapas, kan du skapa [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) och [Mobilappar](../app-service-mobile/app-service-mobile-value-prop.md) i den här miljön kommer alla skyddas bakom en Brandvägg som konfigureras i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bfa22-116">Once you have an App Service Environment created, you can create [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) and [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) in this environment that will all be protected behind the WAF we configure in the next section.</span></span>

## <a name="configuring-your-barracuda-waf-cloud-service"></a><span data-ttu-id="bfa22-117">Konfigurera Barracuda Brandvägg Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="bfa22-117">Configuring your Barracuda WAF Cloud Service</span></span>
<span data-ttu-id="bfa22-118">Barracuda har en [detaljerad artikel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) om distribution av dess Brandvägg på en virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="bfa22-118">Barracuda has a [detailed article](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) on deploying its WAF on a virtual machine in Azure.</span></span> <span data-ttu-id="bfa22-119">Men eftersom vi vill redundans och inte inför en enskild felpunkt som du vill distribuera minst 2 Brandvägg instans virtuella datorer i samma molntjänst när följa dessa anvisningar.</span><span class="sxs-lookup"><span data-stu-id="bfa22-119">But because we want redundancy and not introduce a single point of failure, you want to deploy at least 2 WAF instance VMs into the same Cloud Service when following these instructions.</span></span>

### <a name="adding-endpoints-to-cloud-service"></a><span data-ttu-id="bfa22-120">Att lägga till slutpunkter molntjänst</span><span class="sxs-lookup"><span data-stu-id="bfa22-120">Adding Endpoints to Cloud Service</span></span>
<span data-ttu-id="bfa22-121">När du har 2 eller mer Brandvägg VM-instanser i din molntjänst kan du använda den [Azure-portalen](https://portal.azure.com/) att lägga till HTTP och HTTPS-slutpunkter som används av programmet som visas i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="bfa22-121">Once you have 2 or more WAF VM instances in your Cloud Service you can use the [Azure portal](https://portal.azure.com/) to add HTTP and HTTPS endpoints that are used by your application as shown in the image below.</span></span>

![Konfigurera slutpunkt][ConfigureEndpoint]

<span data-ttu-id="bfa22-123">Om dina program använder slutpunkter, se till att lägga till dem i listan samt.</span><span class="sxs-lookup"><span data-stu-id="bfa22-123">If your applications use other endpoints, make sure to add those to this list as well.</span></span> 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a><span data-ttu-id="bfa22-124">Konfigurera Barracuda Brandvägg via dess hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="bfa22-124">Configuring Barracuda WAF through its Management Portal</span></span>
<span data-ttu-id="bfa22-125">Barracuda Brandvägg använder TCP-Port 8000 för konfigurationen med hjälp av dess hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="bfa22-125">Barracuda WAF uses TCP Port 8000 for configuration through its management portal.</span></span> <span data-ttu-id="bfa22-126">Eftersom vi har flera instanser av de virtuella datorerna Brandvägg behöver du upprepa de här stegen för varje VM-instans.</span><span class="sxs-lookup"><span data-stu-id="bfa22-126">Since we have multiple instances of the WAF VMs you will need to repeat the steps here for each VM instance.</span></span> 

> <span data-ttu-id="bfa22-127">Obs: När du är klar med konfigurationen av Brandvägg, ta bort slutpunkten TCP/8000 från alla din Brandvägg virtuella datorer för att skydda din Brandvägg.</span><span class="sxs-lookup"><span data-stu-id="bfa22-127">Note: Once you are done with WAF configuration, remove the TCP/8000 endpoint from all your WAF VMs to keep your WAF secure.</span></span>
> 
> 

<span data-ttu-id="bfa22-128">Lägg till management-slutpunkt som visas i bilden nedan för att konfigurera din Brandvägg Barracuda.</span><span class="sxs-lookup"><span data-stu-id="bfa22-128">Add the management endpoint as shown in the image below to configure your Barracuda WAF.</span></span>

![Lägg till slutpunkt för hantering][AddManagementEndpoint]

<span data-ttu-id="bfa22-130">Använda en webbläsare för att bläddra till management-slutpunkten på Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="bfa22-130">Use a browser to browse to the management endpoint on your Cloud Service.</span></span> <span data-ttu-id="bfa22-131">Om din molntjänst anropas test.cloudapp.net, skulle du åtkomst till den här slutpunkten genom att bläddra till http://test.cloudapp.net:8000.</span><span class="sxs-lookup"><span data-stu-id="bfa22-131">If your Cloud Service is called test.cloudapp.net, you would access this endpoint by browsing to http://test.cloudapp.net:8000.</span></span> <span data-ttu-id="bfa22-132">Du bör se en inloggningssida som nedan kan logga in med autentiseringsuppgifterna som du angav i installationsfasen Brandvägg VM.</span><span class="sxs-lookup"><span data-stu-id="bfa22-132">You should see a login page like below that you can login using credentials you specified in the WAF VM setup phase.</span></span>

![Hantering av inloggningssidan][ManagementLoginPage]

<span data-ttu-id="bfa22-134">När du loggar in bör du se en instrumentpanel som det i bilden nedan visas grundläggande statistik om Brandvägg skyddet.</span><span class="sxs-lookup"><span data-stu-id="bfa22-134">Once you login you should see a dashboard as the one in the image below that will present basic statistics about the WAF protection.</span></span>

![Instrumentpanel för hantering][ManagementDashboard]

<span data-ttu-id="bfa22-136">Klicka på fliken tjänster kan du konfigurera din Brandvägg för tjänster som skyddas.</span><span class="sxs-lookup"><span data-stu-id="bfa22-136">Clicking on the Services tab will let you configure your WAF for services it is protecting.</span></span> <span data-ttu-id="bfa22-137">Mer information om hur du konfigurerar din Brandvägg Barracuda finns [deras dokumentation](https://techlib.barracuda.com/waf/getstarted1).</span><span class="sxs-lookup"><span data-stu-id="bfa22-137">For more details on configuring your Barracuda WAF you can consult [their documentation](https://techlib.barracuda.com/waf/getstarted1).</span></span> <span data-ttu-id="bfa22-138">I exemplet nedan en Azure-Webbapp har som betjänar trafik via HTTP och HTTPS konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="bfa22-138">In the example below an Azure Web App serving traffic on HTTP and HTTPS has been configured.</span></span>

![Hantering av lägga till tjänster][ManagementAddServices]

> <span data-ttu-id="bfa22-140">Obs: Beroende på hur dina program är konfigurerade och vilka funktioner som används i din Apptjänst-miljö, behöver du vidarebefordrar trafik för TCP andra portar än 80 och 443, t.ex. Om du har IP SSL-inställningar för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="bfa22-140">Note: Depending on how your applications are configured and what features are being used in your App Service Environment, you will need to forward traffic for TCP ports other than 80 and 443, e.g. if you have IP SSL setup for a Web App.</span></span> <span data-ttu-id="bfa22-141">En lista över nätverksportar som används i Apptjänstmiljöer, referera till [kontroll för inkommande trafik dokumentationen](app-service-app-service-environment-control-inbound-traffic.md) nätverksportar avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bfa22-141">For a list of network ports used in App Service Environments, please refer to [Control Inbound Traffic documentation's](app-service-app-service-environment-control-inbound-traffic.md) Network Ports section.</span></span>
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a><span data-ttu-id="bfa22-142">Konfigurera Microsoft Azure Traffic Manager (valfritt)</span><span class="sxs-lookup"><span data-stu-id="bfa22-142">Configuring Microsoft Azure Traffic Manager (OPTIONAL)</span></span>
<span data-ttu-id="bfa22-143">Om ditt program är tillgängligt i flera områden, och du vill läsa in balansera dem bakom [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bfa22-143">If your application is available in multiple regions, then you would want to load balance them behind [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span></span> <span data-ttu-id="bfa22-144">Att göra så att du kan lägga till en slutpunkt i den [klassiska Azure-portalen](https://manage.azure.com) med Molntjänsten namn för din Brandvägg i Traffic Manager-profilen som visas i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="bfa22-144">To do so you can add an endpoint in the [Azure classic portal](https://manage.azure.com) using the Cloud Service name for your WAF in the Traffic Manager profile as shown in the image below.</span></span> 

![Traffic Manager-slutpunkt][TrafficManagerEndpoint]

<span data-ttu-id="bfa22-146">Om ditt program kräver autentisering, se till att du har en resurs som inte kräver någon autentisering för Traffic Manager att pinga tillgängligheten för ditt program.</span><span class="sxs-lookup"><span data-stu-id="bfa22-146">If your application requires authentication, ensure you have some resource that doesn't require any authentication for Traffic Manager to ping for the availability of your application.</span></span> <span data-ttu-id="bfa22-147">Du kan konfigurera URL: en i avsnittet Konfigurera på den [klassiska Azure-portalen](https://manage.azure.com) enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="bfa22-147">You can configure the URL under the Configure section on the [Azure classic portal](https://manage.azure.com) as shown below.</span></span>

![Konfigurera Traffic Manager][ConfigureTrafficManager]

<span data-ttu-id="bfa22-149">För att vidarebefordra Traffic Manager-ping från din Brandvägg till ditt program, måste du installationen webbplats översättningar på Barracuda-Brandvägg att vidarebefordra trafik till tillämpningsprogrammet som visas i exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="bfa22-149">To forward the Traffic Manager pings from your WAF to your application, you need to setup Website Translations on your Barracuda WAF to forward traffic to your application as shown in the example below.</span></span>

![Webbplatsen översättningar][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a><span data-ttu-id="bfa22-151">Skydda trafik till Apptjänst-miljö med Nätverkssäkerhetsgrupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="bfa22-151">Securing Traffic to App Service Environment Using Network Security Groups (NSG)</span></span>
<span data-ttu-id="bfa22-152">Följ den [kontroll för inkommande trafik dokumentationen](app-service-app-service-environment-control-inbound-traffic.md) information om begränsar trafiken till din Apptjänst-miljö från Brandvägg med VIP-adressen för din tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="bfa22-152">Follow the [Control Inbound Traffic documentation](app-service-app-service-environment-control-inbound-traffic.md) for details on restricting traffic to your App Service Environment from the WAF only by using the VIP address of your Cloud Service.</span></span> <span data-ttu-id="bfa22-153">Här är ett exempel Powershell-kommando för att utföra den här aktiviteten för TCP-port 80.</span><span class="sxs-lookup"><span data-stu-id="bfa22-153">Here's a sample Powershell command for performing this task for TCP port 80.</span></span>

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

<span data-ttu-id="bfa22-154">Ersätt SourceAddressPrefix med virtuella IP-adress (VIP) för din Brandvägg Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="bfa22-154">Replace the SourceAddressPrefix with the Virtual IP Address (VIP) of your WAF's Cloud Service.</span></span>

> <span data-ttu-id="bfa22-155">Obs: VIP för Molntjänsten ändras när du tar bort och återskapa Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="bfa22-155">Note: The VIP of your Cloud Service will change when you delete and re-create the Cloud Service.</span></span> <span data-ttu-id="bfa22-156">Se till att uppdatera IP-adressen i nätverket resursgruppens namn när du gör.</span><span class="sxs-lookup"><span data-stu-id="bfa22-156">Make sure to update the IP address in the Network Resource group once you do so.</span></span> 
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
