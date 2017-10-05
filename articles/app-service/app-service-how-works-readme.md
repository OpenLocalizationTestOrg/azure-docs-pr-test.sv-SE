---
title: "Så här fungerar Azure App Service"
description: "Lär dig hur App Service fungerar"
keywords: app service, azure app service, scale, scalable, app service plan, app service cost
services: app-service
documentationcenter: 
author: yochay
manager: erikre
editor: 
ms.assetid: ae74fc32-969e-4580-8d61-02c922f1f184
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 02/23/2017
ms.author: yochayk
ms.openlocfilehash: 2d830963d3d2adba71a6ca99f79eac0fc8cbfb12
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-app-service-works"></a><span data-ttu-id="18a70-104">Så här fungerar App Service</span><span class="sxs-lookup"><span data-stu-id="18a70-104">How App Service works</span></span>
<span data-ttu-id="18a70-105">Azure App Service är en molntjänst som har utformats för att lösa de praktiska problem som tekniker möter idag.</span><span class="sxs-lookup"><span data-stu-id="18a70-105">Azure App Service is a cloud service that's designed to solve the practical problems that engineers face today.</span></span>
<span data-ttu-id="18a70-106">App Service fokuserar på att erbjuda överlägsen utvecklarproduktivitet utan att ge avkall på behovet att leverera program i molnskala.</span><span class="sxs-lookup"><span data-stu-id="18a70-106">App Service focuses on providing superior developer productivity without compromising on the need to deliver applications at cloud scale.</span></span> 

<span data-ttu-id="18a70-107">App Service innehåller även de funktioner och ramverk som behövs när du ska skapa de företagsprogram du behöver i verksamheten.</span><span class="sxs-lookup"><span data-stu-id="18a70-107">App Service also provides the features and frameworks that are necessary for creating enterprise line-of-business applications.</span></span> <span data-ttu-id="18a70-108">Med App Service kan du utveckla appar i de vanligaste utvecklingsspråken som Java, PHP, Node.js, Python och Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="18a70-108">App Service lets you develop apps in most popular development languages, including Java, PHP, Node.js, Python, and the Microsoft .NET languages.</span></span> <span data-ttu-id="18a70-109">Med App Service kan du:</span><span class="sxs-lookup"><span data-stu-id="18a70-109">With App Service, you can:</span></span>

* <span data-ttu-id="18a70-110">Skapa i hög grad skalbara Web Apps.</span><span class="sxs-lookup"><span data-stu-id="18a70-110">Build highly scalable web apps.</span></span>
* <span data-ttu-id="18a70-111">Snabbt bygga Mobile App-servrar med en uppsättning lättanvända mobila funktioner, till exempel dataservrar, användarautentisering och push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="18a70-111">Quickly build Mobile Apps back ends with a set of easy-to-use mobile capabilities such as data back ends, user authentication, and push notifications.</span></span>
* <span data-ttu-id="18a70-112">Implementera, distribuera och publicera API:er med API Apps.</span><span class="sxs-lookup"><span data-stu-id="18a70-112">Implement, deploy, and publish APIs with API Apps.</span></span>
* <span data-ttu-id="18a70-113">Koppla samman affärsappar i arbetsflöden och transformera data med Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="18a70-113">Tie business applications together into workflows and transform data with Logic Apps.</span></span>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

<span data-ttu-id="18a70-114">Alla apptyper baseras på den skalbara och flexibla webbapp-plattformen som ger utvecklare en optimerad upplevelse av hela livscykeln från appdesign till appunderhåll.</span><span class="sxs-lookup"><span data-stu-id="18a70-114">All app types rely on the scalable and flexible Web Apps platform, which enables developers to have an optimized full lifecycle experience from app design to app maintenance.</span></span> <span data-ttu-id="18a70-115">Livscykelfunktionerna möjliggör följande:</span><span class="sxs-lookup"><span data-stu-id="18a70-115">The lifecycle capabilities enable the following:</span></span>

* <span data-ttu-id="18a70-116">**Snabbt appskapande**.</span><span class="sxs-lookup"><span data-stu-id="18a70-116">**Quick app creation**.</span></span> <span data-ttu-id="18a70-117">Börja från början eller välj ett OSS-paket (operational support system) från Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="18a70-117">Start from scratch or pick an operational support system (OSS) package from the Azure Marketplace.</span></span>
* <span data-ttu-id="18a70-118">**Kontinuerlig distribution**.</span><span class="sxs-lookup"><span data-stu-id="18a70-118">**Continuous deployment**.</span></span> <span data-ttu-id="18a70-119">Distribuera automatiskt ny kod från populära lösningar för källkontroll, till exempel TFS, GitHub och Bitbucket, och synkronisera innehåll från webblagringstjänster, till exempel OneDrive och Dropbox.</span><span class="sxs-lookup"><span data-stu-id="18a70-119">Automatically deploy new code from popular source control solutions such as TFS, GitHub, and Bitbucket, and sync content from online storage services such as OneDrive and Dropbox.</span></span>
* <span data-ttu-id="18a70-120">**Testa i produktion**.</span><span class="sxs-lookup"><span data-stu-id="18a70-120">**Test in production**.</span></span> <span data-ttu-id="18a70-121">Skapa förproduktionsmiljöer smidigt och hantera den mängd trafik som går till dem.</span><span class="sxs-lookup"><span data-stu-id="18a70-121">Smoothly create pre-production environments and manage the amount of traffic that's going to them.</span></span> <span data-ttu-id="18a70-122">Felsök i molnet när det behövs och återställ när du har identifierat problemet.</span><span class="sxs-lookup"><span data-stu-id="18a70-122">Debug in the cloud when needed, and roll back when issues are found.</span></span>
* <span data-ttu-id="18a70-123">**Kör asynkrona åtgärder och batchjobb**.</span><span class="sxs-lookup"><span data-stu-id="18a70-123">**Running asynchronous tasks and batch jobs**.</span></span> <span data-ttu-id="18a70-124">Kör kod i bakgrunden eller aktivera din kod baserat på händelser (till exempel meddelanden som hamnar i en kö för Azure-lagring) och schemalagda tider (CRON).</span><span class="sxs-lookup"><span data-stu-id="18a70-124">Run code in a background process or activate your code based on events (such as messages landing in an Azure Storage queue) and scheduled times (CRON).</span></span>
* <span data-ttu-id="18a70-125">**Skala appen**.</span><span class="sxs-lookup"><span data-stu-id="18a70-125">**Scaling the app**.</span></span> <span data-ttu-id="18a70-126">Använd ett av många alternativ för att automatiskt skala din tjänst vågrätt och lodrätt baserat på trafik och resursanvändning.</span><span class="sxs-lookup"><span data-stu-id="18a70-126">Use one of many options to automatically scale your service horizontally and vertically based on traffic and resource utilization.</span></span> <span data-ttu-id="18a70-127">Konfigurera privata miljöer som är dedikerade till dina appar.</span><span class="sxs-lookup"><span data-stu-id="18a70-127">Configure private environments that are dedicated to your apps.</span></span>   
* <span data-ttu-id="18a70-128">**Underhåll appen**.</span><span class="sxs-lookup"><span data-stu-id="18a70-128">**Maintaining the app**.</span></span> <span data-ttu-id="18a70-129">Utnyttja många av funktionerna för felsökning och diagnostik för att undvika problem och effektivt lösa dem antingen i realtid (med funktioner som automatisk återställning och live-felsökning) eller efteråt genom att analysera loggar och minnesdumpar.</span><span class="sxs-lookup"><span data-stu-id="18a70-129">Use many of the debugging and diagnostics features to stay ahead of problems and to efficiently resolve them either in real time (with features such as auto-healing and live debugging) or after the fact by analyzing logs and memory dumps.</span></span>

<span data-ttu-id="18a70-130">Sammantaget så gör App Service-funktionerna att utvecklare kan fokusera på koden och snabbt nå ett stabilt och mycket skalbart produktionstillstånd.</span><span class="sxs-lookup"><span data-stu-id="18a70-130">As a whole, App Service capabilities enable developers to focus on their code and quickly reach a stable and highly scalable production state.</span></span> <span data-ttu-id="18a70-131">Tack vare funktionerna API Apps och Logic Apps kan utvecklare skapa verkliga företagsprogram som överbygger barriärer mellan affärslösningar och för integrering lokalt till molnet.</span><span class="sxs-lookup"><span data-stu-id="18a70-131">With the API Apps and Logic Apps features, developers can build real-world enterprise applications that bridge barriers between business solutions and on-premises to cloud integration.</span></span> 

## <a name="videos"></a><span data-ttu-id="18a70-132">Videoklipp</span><span class="sxs-lookup"><span data-stu-id="18a70-132">Videos</span></span>
* [<span data-ttu-id="18a70-133">Azure App Service-arkitektur</span><span class="sxs-lookup"><span data-stu-id="18a70-133">Azure App Service Architecture</span></span>](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)

## <a name="next-steps"></a><span data-ttu-id="18a70-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18a70-134">Next steps</span></span>

<span data-ttu-id="18a70-135">Läs mer om App Service i följande ämnen:</span><span class="sxs-lookup"><span data-stu-id="18a70-135">Learn more about App Service in one of the following topics:</span></span>

* [<span data-ttu-id="18a70-136">Vad är Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="18a70-136">What is Azure App Service?</span></span>](app-service-value-prop-what-is.md)
  * [<span data-ttu-id="18a70-137">Webbapp</span><span class="sxs-lookup"><span data-stu-id="18a70-137">Web App</span></span>](../app-service-web/app-service-web-overview.md)
  * [<span data-ttu-id="18a70-138">Mobilapp</span><span class="sxs-lookup"><span data-stu-id="18a70-138">Mobile App</span></span>](../app-service-mobile/app-service-mobile-value-prop.md)
  * [<span data-ttu-id="18a70-139">API-app</span><span class="sxs-lookup"><span data-stu-id="18a70-139">API App</span></span>](../app-service-api/app-service-api-apps-why-best-platform.md)
* [<span data-ttu-id="18a70-140">Azure App Service-arkitektur (presentation)</span><span class="sxs-lookup"><span data-stu-id="18a70-140">Azure App Service Architecture (presentation)</span></span>](http://www.slideshare.net/maartenba/windows-azure-web-sites-things-they-dont-teach-kids-in-school-comunity-day-2013)
* [<span data-ttu-id="18a70-141">Jämförelse mellan Azure App Service, Azure Cloud Services och Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="18a70-141">Azure App Service, Cloud Services, and Virtual Machines comparison</span></span>](../app-service-web/choose-web-site-cloud-service-vm.md)
* [<span data-ttu-id="18a70-142">Så här fungerar App Service-planer</span><span class="sxs-lookup"><span data-stu-id="18a70-142">Understanding App Service Plans</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
* [<span data-ttu-id="18a70-143">Introduktion till App Service-miljöer</span><span class="sxs-lookup"><span data-stu-id="18a70-143">Introduction to App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
  * [<span data-ttu-id="18a70-144">Övning: Skapa en App Service-miljö</span><span class="sxs-lookup"><span data-stu-id="18a70-144">Exercise: Create an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="18a70-145">Azure App Service-stöd för utvecklingsstackar</span><span class="sxs-lookup"><span data-stu-id="18a70-145">Azure App Service Development Stacks Support</span></span>](https://azure.microsoft.com/blog/windows-azure-websites-development-stacks-support/)



