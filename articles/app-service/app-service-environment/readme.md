---
title: "Azure Apptjänst-miljö viktigt"
description: "Visar en lista över dokumentationen som beskriver Azure Apptjänst-miljö"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 77452413-5193-4762-8b3d-5fa8e4edf1ca
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 5b1362854dbc3b0098718bd2ea3cffb06366000c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="app-service-environment-documentation"></a><span data-ttu-id="4e963-103">Dokumentation för Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="4e963-103">App Service environment documentation</span></span>
 <span data-ttu-id="4e963-104">Azure Apptjänst-miljö är en funktion i Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för säker körning av Apptjänst-appar i hög skala.</span><span class="sxs-lookup"><span data-stu-id="4e963-104">Azure App Service Environment is an Azure App Service feature that provides a fully isolated and dedicated environment for securely running App Service apps at high scale.</span></span> <span data-ttu-id="4e963-105">Den här funktionen kan vara värd för din [webbappar][webapps], [mobilappar][mobileapps], [API apps][APIApps], och [funktioner][Functions].</span><span class="sxs-lookup"><span data-stu-id="4e963-105">This capability can host your [web apps][webapps], [mobile apps][mobileapps], [API apps][APIApps], and [functions][Functions].</span></span>

<span data-ttu-id="4e963-106">Apptjänstmiljöer (ASEs) är perfekt för programarbetsbelastningar som kräver:</span><span class="sxs-lookup"><span data-stu-id="4e963-106">App Service environments (ASEs) are ideal for application workloads that require:</span></span>

* <span data-ttu-id="4e963-107">Mycket hög skala.</span><span class="sxs-lookup"><span data-stu-id="4e963-107">Very high scale.</span></span>
* <span data-ttu-id="4e963-108">Isolering och säker nätverksåtkomst.</span><span class="sxs-lookup"><span data-stu-id="4e963-108">Isolation and secure network access.</span></span>

<span data-ttu-id="4e963-109">Kunder kan skapa flera ASEs inom ett enda Azure-region och över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="4e963-109">Customers can create multiple ASEs within a single Azure region and across multiple Azure regions.</span></span> <span data-ttu-id="4e963-110">Den här flexibilitet gör ASEs idealiskt för vågrätt skalning tillståndslös programnivåerna stöd för hög RPS arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="4e963-110">This versatility makes ASEs ideal for horizontally scaling stateless application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="4e963-111">ASEs är isolerad för att endast en enskild kund program som körs och alltid har distribuerats till Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="4e963-111">ASEs are isolated to running only a single customer's applications and are always deployed into an Azure virtual network.</span></span> <span data-ttu-id="4e963-112">Kunder har detaljerad kontroll över inkommande och utgående nätverkstrafik genom att använda [Nätverkssäkerhetsgrupper][NSGs].</span><span class="sxs-lookup"><span data-stu-id="4e963-112">Customers have fine-grained control over inbound and outbound application network traffic by using [Network Security Groups][NSGs].</span></span> <span data-ttu-id="4e963-113">Program kan också etablera snabb säkra anslutningar över virtuella nätverk till lokala företagsresurser.</span><span class="sxs-lookup"><span data-stu-id="4e963-113">Applications can also establish high-speed secure connections over virtual networks to on-premises corporate resources.</span></span>

<span data-ttu-id="4e963-114">Appar behöver ofta åtkomst till företagets resurser, till exempel interna databaser och webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="4e963-114">Apps frequently need to access corporate resources, such as internal databases and web services.</span></span> <span data-ttu-id="4e963-115">Appar som körs på ASEs kan komma åt resurser via [plats-till-plats] [ SiteToSite] VPN och [Azure ExpressRoute] [ ExpressRoute] anslutningar.</span><span class="sxs-lookup"><span data-stu-id="4e963-115">Apps that run on ASEs can access resources via [site-to-site][SiteToSite] VPN and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

* <span data-ttu-id="4e963-116">[Vad är en Apptjänst-miljö?][Intro]</span><span class="sxs-lookup"><span data-stu-id="4e963-116">[What is an App Service environment?][Intro]</span></span>
* <span data-ttu-id="4e963-117">[Skapa en Apptjänst-miljö][MakeExternalASE]</span><span class="sxs-lookup"><span data-stu-id="4e963-117">[Create an App Service environment][MakeExternalASE]</span></span>
* <span data-ttu-id="4e963-118">[Skapa en intern belastningsutjämnare Apptjänst-miljö][MakeILBASE]</span><span class="sxs-lookup"><span data-stu-id="4e963-118">[Create an internal load balancer App Service environment][MakeILBASE]</span></span>
* <span data-ttu-id="4e963-119">[Använd en Apptjänst-miljö][UsingASE]</span><span class="sxs-lookup"><span data-stu-id="4e963-119">[Use an App Service environment][UsingASE]</span></span>
* <span data-ttu-id="4e963-120">[Överväganden för nätverk och Apptjänst-miljö][ASENetwork]</span><span class="sxs-lookup"><span data-stu-id="4e963-120">[Networking considerations and the App Service environment][ASENetwork]</span></span>
* <span data-ttu-id="4e963-121">[Skapa en Apptjänst-miljö från en mall][MakeASEfromTemplate]</span><span class="sxs-lookup"><span data-stu-id="4e963-121">[Create an App Service environment from a template][MakeASEfromTemplate]</span></span>


## <a name="videos"></a><span data-ttu-id="4e963-122">Videoklipp</span><span class="sxs-lookup"><span data-stu-id="4e963-122">Videos</span></span>
<span data-ttu-id="4e963-123">Hantera modern PaaS för företaget med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4e963-123">Master Modern PaaS for the Enterprise with Azure App Service</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

<span data-ttu-id="4e963-124">Distribuera säkra appar med hög skalbarhet</span><span class="sxs-lookup"><span data-stu-id="4e963-124">Deploying Highly Scalable and Secure Apps</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

<span data-ttu-id="4e963-125">Köra företagswebbappar och mobilappar på Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4e963-125">Running Enterprise Web and Mobile Apps on Azure App Service</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]

## <a name="app-service-environment-v1"></a><span data-ttu-id="4e963-126">App Service Environment v1</span><span class="sxs-lookup"><span data-stu-id="4e963-126">App Service Environment v1</span></span> ##
<span data-ttu-id="4e963-127">Det finns två versioner av Apptjänstmiljö: ASEv1 och ASEv2.</span><span class="sxs-lookup"><span data-stu-id="4e963-127">There are two versions of App Service Environment: ASEv1 and ASEv2.</span></span> <span data-ttu-id="4e963-128">Information om ASEv1 finns [Apptjänstmiljö v1 dokumentationen][ASEv1README].</span><span class="sxs-lookup"><span data-stu-id="4e963-128">For information on ASEv1, see [App Service Environment v1 documentation][ASEv1README].</span></span>


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
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[ASEv1README]: ../app-service-app-service-environments-readme.md
[SiteToSite]: ../../vpn-gateway/vpn-gateway-site-to-site-create.md
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
