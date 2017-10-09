---
title: "aaaHow Azure Apptjänst fungerar"
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
ms.openlocfilehash: b20733ec8844773d063e2b6918605c4a48db1f5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-app-service-works"></a><span data-ttu-id="fc9ab-104">Så här fungerar App Service</span><span class="sxs-lookup"><span data-stu-id="fc9ab-104">How App Service works</span></span>
<span data-ttu-id="fc9ab-105">Azure Apptjänst är en molnbaserad tjänst som är utformad toosolve hello praktiska problem som tekniker möter idag.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-105">Azure App Service is a cloud service that's designed toosolve hello practical problems that engineers face today.</span></span>
<span data-ttu-id="fc9ab-106">Apptjänst fokuserar på att erbjuda överlägsen utvecklarproduktivitet utan att kompromissa med hello måste toodeliver program i molnskala.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-106">App Service focuses on providing superior developer productivity without compromising on hello need toodeliver applications at cloud scale.</span></span> 

<span data-ttu-id="fc9ab-107">Apptjänst innehåller också hello funktioner och ramverk som behövs för att skapa enterprise line-of-business-program.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-107">App Service also provides hello features and frameworks that are necessary for creating enterprise line-of-business applications.</span></span> <span data-ttu-id="fc9ab-108">Med Apptjänst kan du utveckla appar i de flesta populära utvecklingsspråken, inklusive Java, PHP, Node.js, Python och hello Microsoft .NET-språk.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-108">App Service lets you develop apps in most popular development languages, including Java, PHP, Node.js, Python, and hello Microsoft .NET languages.</span></span> <span data-ttu-id="fc9ab-109">Med App Service kan du:</span><span class="sxs-lookup"><span data-stu-id="fc9ab-109">With App Service, you can:</span></span>

* <span data-ttu-id="fc9ab-110">Skapa i hög grad skalbara Web Apps.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-110">Build highly scalable web apps.</span></span>
* <span data-ttu-id="fc9ab-111">Snabbt bygga Mobile App-servrar med en uppsättning lättanvända mobila funktioner, till exempel dataservrar, användarautentisering och push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-111">Quickly build Mobile Apps back ends with a set of easy-to-use mobile capabilities such as data back ends, user authentication, and push notifications.</span></span>
* <span data-ttu-id="fc9ab-112">Implementera, distribuera och publicera API:er med API Apps.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-112">Implement, deploy, and publish APIs with API Apps.</span></span>
* <span data-ttu-id="fc9ab-113">Koppla samman affärsappar i arbetsflöden och transformera data med Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-113">Tie business applications together into workflows and transform data with Logic Apps.</span></span>

> [!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]
> 
> 

<span data-ttu-id="fc9ab-114">Alla apptyper baseras på hello skalbara och flexibla Web Apps plattform, vilket gör att utvecklare toohave en optimerad hela livscykeln upplevelse som påminner om appen design tooapp underhåll.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-114">All app types rely on hello scalable and flexible Web Apps platform, which enables developers toohave an optimized full lifecycle experience from app design tooapp maintenance.</span></span> <span data-ttu-id="fc9ab-115">Hej livscykelfunktionerna aktivera hello följande:</span><span class="sxs-lookup"><span data-stu-id="fc9ab-115">hello lifecycle capabilities enable hello following:</span></span>

* <span data-ttu-id="fc9ab-116">**Snabbt appskapande**.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-116">**Quick app creation**.</span></span> <span data-ttu-id="fc9ab-117">Börja från början eller välj ett operational stödsystem (OSS)-paket från hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-117">Start from scratch or pick an operational support system (OSS) package from hello Azure Marketplace.</span></span>
* <span data-ttu-id="fc9ab-118">**Kontinuerlig distribution**.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-118">**Continuous deployment**.</span></span> <span data-ttu-id="fc9ab-119">Distribuera automatiskt ny kod från populära lösningar för källkontroll, till exempel TFS, GitHub och Bitbucket, och synkronisera innehåll från webblagringstjänster, till exempel OneDrive och Dropbox.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-119">Automatically deploy new code from popular source control solutions such as TFS, GitHub, and Bitbucket, and sync content from online storage services such as OneDrive and Dropbox.</span></span>
* <span data-ttu-id="fc9ab-120">**Testa i produktion**.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-120">**Test in production**.</span></span> <span data-ttu-id="fc9ab-121">Smidigt att skapa förproduktionsmiljöer och hantera hello mängden trafik som går toothem.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-121">Smoothly create pre-production environments and manage hello amount of traffic that's going toothem.</span></span> <span data-ttu-id="fc9ab-122">Felsöka i hello molnet vid behov och rulla tillbaka när problem identifieras.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-122">Debug in hello cloud when needed, and roll back when issues are found.</span></span>
* <span data-ttu-id="fc9ab-123">**Kör asynkrona åtgärder och batchjobb**.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-123">**Running asynchronous tasks and batch jobs**.</span></span> <span data-ttu-id="fc9ab-124">Kör kod i bakgrunden eller aktivera din kod baserat på händelser (till exempel meddelanden som hamnar i en kö för Azure-lagring) och schemalagda tider (CRON).</span><span class="sxs-lookup"><span data-stu-id="fc9ab-124">Run code in a background process or activate your code based on events (such as messages landing in an Azure Storage queue) and scheduled times (CRON).</span></span>
* <span data-ttu-id="fc9ab-125">**Skalning hello app**.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-125">**Scaling hello app**.</span></span> <span data-ttu-id="fc9ab-126">Använd en av många alternativ tooautomatically skala din tjänst vågrätt och lodrätt baserat på trafik och Resursanvändning.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-126">Use one of many options tooautomatically scale your service horizontally and vertically based on traffic and resource utilization.</span></span> <span data-ttu-id="fc9ab-127">Konfigurera privata miljöer som är dedikerad tooyour appar.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-127">Configure private environments that are dedicated tooyour apps.</span></span>   
* <span data-ttu-id="fc9ab-128">**Underhålla hello app**.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-128">**Maintaining hello app**.</span></span> <span data-ttu-id="fc9ab-129">Använda många av hello felsökning och diagnostik funktioner toostay i problem och tooefficiently lösa dem antingen i realtid (med funktioner som automatisk återställning och live-felsökning) eller efter hello fakta genom att analysera loggar och Dumpar.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-129">Use many of hello debugging and diagnostics features toostay ahead of problems and tooefficiently resolve them either in real time (with features such as auto-healing and live debugging) or after hello fact by analyzing logs and memory dumps.</span></span>

<span data-ttu-id="fc9ab-130">Hela Apptjänst-funktionerna för att aktivera utvecklare toofocus på koden och nå snabbt ett stabilt och mycket skalbart produktionstillstånd.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-130">As a whole, App Service capabilities enable developers toofocus on their code and quickly reach a stable and highly scalable production state.</span></span> <span data-ttu-id="fc9ab-131">Med hello API Apps och funktioner i Logic Apps kan utvecklare skapa verkliga företagsprogram som överbrygga barriärer mellan affärslösningar och lokal toocloud integrering.</span><span class="sxs-lookup"><span data-stu-id="fc9ab-131">With hello API Apps and Logic Apps features, developers can build real-world enterprise applications that bridge barriers between business solutions and on-premises toocloud integration.</span></span> 

## <a name="videos"></a><span data-ttu-id="fc9ab-132">Videoklipp</span><span class="sxs-lookup"><span data-stu-id="fc9ab-132">Videos</span></span>
* [<span data-ttu-id="fc9ab-133">Azure App Service-arkitektur</span><span class="sxs-lookup"><span data-stu-id="fc9ab-133">Azure App Service Architecture</span></span>](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/)

## <a name="next-steps"></a><span data-ttu-id="fc9ab-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc9ab-134">Next steps</span></span>

<span data-ttu-id="fc9ab-135">Läs mer om Apptjänst på något av följande avsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="fc9ab-135">Learn more about App Service in one of hello following topics:</span></span>

* [<span data-ttu-id="fc9ab-136">Vad är Azure App Service?</span><span class="sxs-lookup"><span data-stu-id="fc9ab-136">What is Azure App Service?</span></span>](app-service-value-prop-what-is.md)
  * [<span data-ttu-id="fc9ab-137">Webbapp</span><span class="sxs-lookup"><span data-stu-id="fc9ab-137">Web App</span></span>](../app-service-web/app-service-web-overview.md)
  * [<span data-ttu-id="fc9ab-138">Mobilapp</span><span class="sxs-lookup"><span data-stu-id="fc9ab-138">Mobile App</span></span>](../app-service-mobile/app-service-mobile-value-prop.md)
  * [<span data-ttu-id="fc9ab-139">API-app</span><span class="sxs-lookup"><span data-stu-id="fc9ab-139">API App</span></span>](../app-service-api/app-service-api-apps-why-best-platform.md)
* [<span data-ttu-id="fc9ab-140">Azure App Service-arkitektur (presentation)</span><span class="sxs-lookup"><span data-stu-id="fc9ab-140">Azure App Service Architecture (presentation)</span></span>](http://www.slideshare.net/maartenba/windows-azure-web-sites-things-they-dont-teach-kids-in-school-comunity-day-2013)
* [<span data-ttu-id="fc9ab-141">Jämförelse mellan Azure App Service, Azure Cloud Services och Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="fc9ab-141">Azure App Service, Cloud Services, and Virtual Machines comparison</span></span>](../app-service-web/choose-web-site-cloud-service-vm.md)
* [<span data-ttu-id="fc9ab-142">Så här fungerar App Service-planer</span><span class="sxs-lookup"><span data-stu-id="fc9ab-142">Understanding App Service Plans</span></span>](azure-web-sites-web-hosting-plans-in-depth-overview.md)
* [<span data-ttu-id="fc9ab-143">Introduktion tooApp-miljö</span><span class="sxs-lookup"><span data-stu-id="fc9ab-143">Introduction tooApp Service Environment</span></span>](../app-service-web/app-service-app-service-environment-intro.md)
  * [<span data-ttu-id="fc9ab-144">Övning: Skapa en App Service-miljö</span><span class="sxs-lookup"><span data-stu-id="fc9ab-144">Exercise: Create an App Service Environment</span></span>](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="fc9ab-145">Azure App Service-stöd för utvecklingsstackar</span><span class="sxs-lookup"><span data-stu-id="fc9ab-145">Azure App Service Development Stacks Support</span></span>](https://azure.microsoft.com/blog/windows-azure-websites-development-stacks-support/)



