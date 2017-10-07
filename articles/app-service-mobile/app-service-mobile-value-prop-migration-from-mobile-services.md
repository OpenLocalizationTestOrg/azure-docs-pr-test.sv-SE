---
title: "aaaI använder Mobile Services, Apptjänst sätt?"
description: "Lär dig mer om vilken nytta apptjänst tooyour befintliga Mobile Services-projekt."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 26b68a11-8352-4f78-acd2-e4e0ec177781
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 315cc6eedcdca6c3f9f9bb9fd5ec7baf655b7e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="7f03b-103"><a name="getting-started"> </a>Jag använder Mobile Services. Vad bidrar Apptjänst med?</span><span class="sxs-lookup"><span data-stu-id="7f03b-103"><a name="getting-started"> </a>I use Mobile Services, how does App Service help?</span></span>
## <a name="overview"></a><span data-ttu-id="7f03b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7f03b-104">Overview</span></span>
<span data-ttu-id="7f03b-105">Din nuvarande mobiltjänst Mobile Services är säker och kommer att kunna användas även i fortsättningen.</span><span class="sxs-lookup"><span data-stu-id="7f03b-105">Your existing Mobile Service is safe and will remain supported.</span></span> <span data-ttu-id="7f03b-106">Det finns dock antal fördelar hello *Azure App Service* plattformen ger din mobilapp som inte är tillgängliga i Mobile Services:</span><span class="sxs-lookup"><span data-stu-id="7f03b-106">However there are number of advantages hello *Azure App Service* platform provides for your mobile app that are not available today with Mobile Services:</span></span>

* <span data-ttu-id="7f03b-107">enklare och mer kostnadseffektivt utbud av appar för både webb- och mobilklienter</span><span class="sxs-lookup"><span data-stu-id="7f03b-107">Simpler, easier and more cost effective offering for apps that include both web and mobile clients</span></span>
* <span data-ttu-id="7f03b-108">nya värdfunktioner såsom WebJobs, anpassade CName-poster, bättre övervakning</span><span class="sxs-lookup"><span data-stu-id="7f03b-108">New host features including Web Jobs, custom CNames, better monitoring</span></span>
* <span data-ttu-id="7f03b-109">nyckelfärdig integrering med Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="7f03b-109">Turnkey integration with Traffic Manager</span></span>
* <span data-ttu-id="7f03b-110">Anslutningen tooyour lokala resurser och VPN-anslutningar via VNet i tillägget tooHybrid anslutningar</span><span class="sxs-lookup"><span data-stu-id="7f03b-110">Connectivity tooyour on-premises resources and VPNs using VNet in addition tooHybrid Connections</span></span>
* <span data-ttu-id="7f03b-111">övervakning, aviseringar och felsökning av appar med NewRelic eller AppInsights</span><span class="sxs-lookup"><span data-stu-id="7f03b-111">Monitoring, alerting and  troubleshooting for your app using NewRelic or AppInsights</span></span>
* <span data-ttu-id="7f03b-112">Större utbud av hello underliggande beräkningsresurser och prissättning</span><span class="sxs-lookup"><span data-stu-id="7f03b-112">Richer spectrum of hello underlying compute resources and pricing</span></span>
* <span data-ttu-id="7f03b-113">inbyggd automatisk skalning, belastningsutjämning och prestandaövervakning</span><span class="sxs-lookup"><span data-stu-id="7f03b-113">Built-in auto scale, load balancing, and performance monitoring.</span></span>
* <span data-ttu-id="7f03b-114">inbyggd mellanlagring, säkerhetskopiering, återställning och möjlighet till testning i produktionsmiljö</span><span class="sxs-lookup"><span data-stu-id="7f03b-114">Built-in staging, backup, roll-back, and testing-in-production capabilities</span></span>

## <a name="new-hosting-features"></a><span data-ttu-id="7f03b-115">Nya värdfunktioner</span><span class="sxs-lookup"><span data-stu-id="7f03b-115">New hosting features</span></span>
<span data-ttu-id="7f03b-116">I *Azure App Service* hello *Mobilapp* hello i serverdelen koden körs i samma behållare som Web App och API-App.</span><span class="sxs-lookup"><span data-stu-id="7f03b-116">In *Azure App Service* hello *Mobile App* backend code runs in hello same container as Web App and API App.</span></span> <span data-ttu-id="7f03b-117">Därmed kan du dra nytta av alla hello-funktioner i den här behållaren, inklusive några som inte är för närvarande finns i Mobile Services:</span><span class="sxs-lookup"><span data-stu-id="7f03b-117">As such you can take advantage of all hello features in this container, including some of those that are not currently present in Mobile Services:</span></span>

* <span data-ttu-id="7f03b-118">Du kan lägga till serverdelslogik som körs hela tiden via WebJobs.</span><span class="sxs-lookup"><span data-stu-id="7f03b-118">Add continuously running backend logic via Web Jobs</span></span>
* <span data-ttu-id="7f03b-119">Du kan säkerställa att serverdelskoden alltid körs.</span><span class="sxs-lookup"><span data-stu-id="7f03b-119">Ensure your backend code is always running</span></span>
* <span data-ttu-id="7f03b-120">Använd anpassade CNAME-resursposter tooprovide eget och stabil namn tooyour mobila serverdelsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="7f03b-120">Use custom CNames tooprovide friendly and stable names tooyour mobile backend endpoints</span></span>
* <span data-ttu-id="7f03b-121">Du kan geoskala appen med Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="7f03b-121">Geo-scale your app with Traffic Manager</span></span>
* <span data-ttu-id="7f03b-122">Du kan ha med vilka bibliotek och paket du vill.</span><span class="sxs-lookup"><span data-stu-id="7f03b-122">Include any libraries and packages you want.</span></span>
* <span data-ttu-id="7f03b-123">(För .NET) Du kan använda alla funktioner i ASP.NET, även MVC.</span><span class="sxs-lookup"><span data-stu-id="7f03b-123">(For .NET) Leverage any feature of ASP.NET, including MVC</span></span>
* <span data-ttu-id="7f03b-124">(För Node.js) Utnyttja alla bibliotek med endast JavaScript hello Node-ekosystemet, inklusive vanliga MVC-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="7f03b-124">(For Node.js) Leverage any pure JavaScript library of hello Node ecosystem, including common MVC libraries.</span></span>

## <a name="access-on-premises-data-using-vnet"></a><span data-ttu-id="7f03b-125">Möjlighet att nå lokala data via VNet</span><span class="sxs-lookup"><span data-stu-id="7f03b-125">Access on-premises data using VNet</span></span>
<span data-ttu-id="7f03b-126">Med Mobile Services dag som du kan använda Hybridanslutningar tooaccess redan lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="7f03b-126">With Mobile Services today you can already use Hybrid Connections tooaccess on-premises resources.</span></span> <span data-ttu-id="7f03b-127">Det finns dock tillfällen då det är bäst med VPN-lösning.</span><span class="sxs-lookup"><span data-stu-id="7f03b-127">However there are situations where a VPN solution is preferred.</span></span> <span data-ttu-id="7f03b-128">Med *Azure Apptjänst* kan du använda Azure VNet för serverdelskoden till mobilappar.</span><span class="sxs-lookup"><span data-stu-id="7f03b-128">With *Azure App Service* you can use Azure VNet for your Mobile App backend code.</span></span>

## <a name="use-your-favorite-backend-language"></a><span data-ttu-id="7f03b-129">Använd det serverdelsspråk som du gillar bäst</span><span class="sxs-lookup"><span data-stu-id="7f03b-129">Use your favorite backend language</span></span>
<span data-ttu-id="7f03b-130">*Azure Apptjänst* erbjudanden bredare och bättre stöd för ASP.NET och Node.js-plattformarna, inklusive åtkomst toohello senaste körningarna.</span><span class="sxs-lookup"><span data-stu-id="7f03b-130">*Azure App Service* offers broader and richer support for ASP.NET and Node.js platforms, including access toohello latest runtimes.</span></span>

## <a name="set-up-automatic-scale"></a><span data-ttu-id="7f03b-131">Automatisk skalning</span><span class="sxs-lookup"><span data-stu-id="7f03b-131">Set up automatic scale</span></span>
<span data-ttu-id="7f03b-132">Med Mobile Services kördes alla instanser av serverdelskoden på små virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7f03b-132">With Mobile Services, all instances of your backend code were running on Small VMs.</span></span> <span data-ttu-id="7f03b-133">*Azure Apptjänst* kan du tooselect hello storleken på virtuella datorer från en mycket större antal alternativ.</span><span class="sxs-lookup"><span data-stu-id="7f03b-133">*Azure App Service* enables you tooselect hello size of the VMs from a much richer set of options.</span></span> <span data-ttu-id="7f03b-134">Du kan även snabbt skala upp eller ut toohandle alla inkommande kundbelastning utifrån olika prestandamått.</span><span class="sxs-lookup"><span data-stu-id="7f03b-134">You can also  quickly scale up or out toohandle any incoming customer load, based on various performance metrics.</span></span>

## <a name="be-in-hello-know"></a><span data-ttu-id="7f03b-135">Att i hello ”vet”</span><span class="sxs-lookup"><span data-stu-id="7f03b-135">Be in hello “know”</span></span>
<span data-ttu-id="7f03b-136">Reagera tooissues i realtid genom övervakning och aviseringar tooautomatically meddela dig och din grupp.</span><span class="sxs-lookup"><span data-stu-id="7f03b-136">React tooissues in real-time with monitoring and alerts tooautomatically notify you and your team.</span></span> <span data-ttu-id="7f03b-137">Integrera avancerad och övervakningsfunktioner från New Relic och AppInsights ännu bättre tooget-inblick i hur din mobila app utförs.</span><span class="sxs-lookup"><span data-stu-id="7f03b-137">Integrate advanced app analytics and monitoring functionality from New Relic and AppInsights tooget even richer insight into how your Mobile app is performing.</span></span> <span data-ttu-id="7f03b-138">Med *Azure App Service* kan du nu ställa varningar baserat på flera olika prestandamått, antingen genom att programmera eller via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7f03b-138">With *Azure App Service* you can now setup alerts based on variety of performance metrics, either programatically and via hello Azure Portal.</span></span>

## <a name="keep-your-assets-safe"></a><span data-ttu-id="7f03b-139">Dina tillgångar är trygga</span><span class="sxs-lookup"><span data-stu-id="7f03b-139">Keep your assets safe</span></span>
<span data-ttu-id="7f03b-140">Serverdelen och databasen kan säkerhetskopieras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="7f03b-140">Automatically back up your backend and database.</span></span> <span data-ttu-id="7f03b-141">Kod och data är skyddade vid katastrofer och lätt återställas så att du toorun din verksamhet utan problem.</span><span class="sxs-lookup"><span data-stu-id="7f03b-141">Your code and data is secure from disaster and easily restored, allowing you toorun your business with confidence.</span></span>

## <a name="ready-stage-go"></a><span data-ttu-id="7f03b-142">Klara, mellanlagra, kör!</span><span class="sxs-lookup"><span data-stu-id="7f03b-142">Ready, Stage, Go!</span></span>
<span data-ttu-id="7f03b-143">Med *Azure Apptjänst* kan du nu skapa flera olika privata testnings- och mellanlagringsmiljöer för dina mobilappar.</span><span class="sxs-lookup"><span data-stu-id="7f03b-143">With *Azure App Service* you can now create multiple private testing and staging environments for your mobile apps.</span></span> <span data-ttu-id="7f03b-144">Använd dem tooperform testning innan du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="7f03b-144">Use them tooperform testing before you deploy.</span></span> <span data-ttu-id="7f03b-145">Växla tooproduction utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="7f03b-145">Swap tooproduction with no downtime.</span></span> <span data-ttu-id="7f03b-146">Webbappar är förinstallerade, vilket ger hello bästa möjliga kundupplevelse.</span><span class="sxs-lookup"><span data-stu-id="7f03b-146">Web apps are pre-loaded, ensuring hello best customer experience.</span></span>

<span data-ttu-id="7f03b-147">Du kan komma igång och utnyttja alla fördelarna med *Apptjänst* för din befintliga mobiltjänst genom att gå igenom den här [kursen](app-service-mobile-migrating-from-mobile-services.md).</span><span class="sxs-lookup"><span data-stu-id="7f03b-147">You can get start taking advantage of *App Service* for your existing Mobile Service by following this [tutorial](app-service-mobile-migrating-from-mobile-services.md).</span></span>
