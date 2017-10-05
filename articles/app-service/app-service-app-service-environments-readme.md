---
title: "Apptjänst-miljö | Microsoft Docs"
description: "Vad är en Azure Apptjänst-miljö? En introduktion till Apptjänst-miljö."
keywords: "Azure apptjänst-miljön, virtuella nätverk, säkra nätverk"
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 1db5c057-3c56-4537-b580-cdd21fe3f3a7
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: stefsch
ms.openlocfilehash: 1de3f2c513f4f5ce6fd2ab2b57514122955a9a98
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="app-service-environment-documentation"></a><span data-ttu-id="cc112-105">Dokumentation för Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="cc112-105">App Service Environment Documentation</span></span>
<span data-ttu-id="cc112-106">En Apptjänst-miljö är en [Premium] [ PremiumTier] service plan alternativet för Azure App Service som tillhandahåller en helt isolerad och dedikerad miljö för Azure App Service-program som körs på ett säkert sätt i hög skala inklusive [Web Apps][WebApps], [Mobilappar][MobileApps], och [API Apps] [ APIApps].</span><span class="sxs-lookup"><span data-stu-id="cc112-106">An App Service Environment is a [Premium][PremiumTier] service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps at high scale, including [Web Apps][WebApps], [Mobile Apps][MobileApps], and [API Apps][APIApps].</span></span>  

<span data-ttu-id="cc112-107">Apptjänstmiljöer är perfekt för programarbetsbelastningar som kräver:</span><span class="sxs-lookup"><span data-stu-id="cc112-107">App Service Environments are ideal for application workloads requiring:</span></span>

* <span data-ttu-id="cc112-108">Mycket hög skala</span><span class="sxs-lookup"><span data-stu-id="cc112-108">Very high scale</span></span>
* <span data-ttu-id="cc112-109">Isolering och säker nätverksåtkomst</span><span class="sxs-lookup"><span data-stu-id="cc112-109">Isolation and secure network access</span></span>

<span data-ttu-id="cc112-110">Kunder kan skapa flera Apptjänstmiljöer inom en enda Azure region, samt över flera Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="cc112-110">Customers can create multiple App Service Environments within a single Azure region, as well as across multiple Azure regions.</span></span>  <span data-ttu-id="cc112-111">Detta gör Apptjänstmiljöer idealiskt för vågrätt skalning statuslösa programnivåerna stöd för hög RPS arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="cc112-111">This makes App Service Environments ideal for horizontally scaling state-less application tiers in support of high RPS workloads.</span></span>

<span data-ttu-id="cc112-112">Apptjänstmiljöer är isolerad för att endast en enskild kund program som körs och alltid har distribuerats till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="cc112-112">App Service Environments are isolated to running only a single customer's applications, and are always deployed into a virtual network.</span></span>  <span data-ttu-id="cc112-113">Kunder har detaljerad kontroll över både inkommande och utgående trafik med [nätverkssäkerhetsgrupper][NetworkSecurityGroups].</span><span class="sxs-lookup"><span data-stu-id="cc112-113">Customers have fine-grained control over both inbound and outbound application network traffic using [network security groups][NetworkSecurityGroups].</span></span>  <span data-ttu-id="cc112-114">Program kan också etablera snabb säkra anslutningar över virtuella nätverk till lokala företagsresurser.</span><span class="sxs-lookup"><span data-stu-id="cc112-114">Applications can also establish high-speed secure connections over virtual networks to on-premises corporate resources.</span></span>

<span data-ttu-id="cc112-115">Appar behöver ofta åtkomst till företagets resurser, till exempel interna databaser och webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="cc112-115">Apps frequently need to access corporate resources such as internal databases and web services.</span></span>  <span data-ttu-id="cc112-116">Appar som körs på Apptjänstmiljöer kan komma åt resurser som nås via [plats-till-plats] [ SiteToSite] VPN och [Azure ExpressRoute] [ ExpressRoute] anslutningar.</span><span class="sxs-lookup"><span data-stu-id="cc112-116">Apps running on App Service Environments can access resources reachable via [Site-to-Site][SiteToSite] VPN and [Azure ExpressRoute][ExpressRoute] connections.</span></span>

* [<span data-ttu-id="cc112-117">Vad är en App Service-miljö (ASE)?</span><span class="sxs-lookup"><span data-stu-id="cc112-117">What is an App Service Environment?</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
* [<span data-ttu-id="cc112-118">Skapa en App Service-miljö</span><span class="sxs-lookup"><span data-stu-id="cc112-118">Creating an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="cc112-119">Skapa appar i en App Service-miljö</span><span class="sxs-lookup"><span data-stu-id="cc112-119">Creating Apps in an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [<span data-ttu-id="cc112-120">Skapa och använda en intern belastningsutjämnare med Apptjänstmiljöer</span><span class="sxs-lookup"><span data-stu-id="cc112-120">Creating and Using an Internal Load Balancer with App Service Environments</span></span>](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [<span data-ttu-id="cc112-121">Konfigurera en App Service-miljö</span><span class="sxs-lookup"><span data-stu-id="cc112-121">Configuring an App Service Environment</span></span>](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [<span data-ttu-id="cc112-122">Skala appar i en App Service-miljö</span><span class="sxs-lookup"><span data-stu-id="cc112-122">Scaling Apps in an App Service Environment</span></span>](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [<span data-ttu-id="cc112-123">Nätverkssäkerhet och arkitektur</span><span class="sxs-lookup"><span data-stu-id="cc112-123">Network Security and Architecture</span></span>](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a><span data-ttu-id="cc112-124">Så här</span><span class="sxs-lookup"><span data-stu-id="cc112-124">How To's</span></span>
[!INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]

## <a name="videos"></a><span data-ttu-id="cc112-125">Videoklipp</span><span class="sxs-lookup"><span data-stu-id="cc112-125">Videos</span></span>
>[!VIDEO https://channel9.msdn.com/Events/Ignite/2016/BRK3205/player]

>[!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON325/player]

>[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3715/player]



<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
