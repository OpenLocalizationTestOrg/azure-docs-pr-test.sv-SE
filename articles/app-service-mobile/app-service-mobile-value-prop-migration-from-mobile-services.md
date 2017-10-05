---
title: "Jag använder Mobile Services. Vad bidrar Apptjänst med?"
description: "Läs om vilken nytta du har av Apptjänst i dina befintliga Mobile Services-projekt."
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
ms.openlocfilehash: 22397b6b448b418d5b54a457c3bafaf5c68ecc7b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <span data-ttu-id="3955f-103"><a name="getting-started"> </a>Jag använder Mobile Services. Vad bidrar Apptjänst med?</span><span class="sxs-lookup"><span data-stu-id="3955f-103"><a name="getting-started"> </a>I use Mobile Services, how does App Service help?</span></span>
## <a name="overview"></a><span data-ttu-id="3955f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="3955f-104">Overview</span></span>
<span data-ttu-id="3955f-105">Din nuvarande mobiltjänst Mobile Services är säker och kommer att kunna användas även i fortsättningen.</span><span class="sxs-lookup"><span data-stu-id="3955f-105">Your existing Mobile Service is safe and will remain supported.</span></span> <span data-ttu-id="3955f-106">Det finns emellertid ett antal fördelar som *Azure Apptjänst*-plattformen ger din mobilapp som i dagsläget inte är tillgängliga i Mobile Services:</span><span class="sxs-lookup"><span data-stu-id="3955f-106">However there are number of advantages the *Azure App Service* platform provides for your mobile app that are not available today with Mobile Services:</span></span>

* <span data-ttu-id="3955f-107">enklare och mer kostnadseffektivt utbud av appar för både webb- och mobilklienter</span><span class="sxs-lookup"><span data-stu-id="3955f-107">Simpler, easier and more cost effective offering for apps that include both web and mobile clients</span></span>
* <span data-ttu-id="3955f-108">nya värdfunktioner såsom WebJobs, anpassade CName-poster, bättre övervakning</span><span class="sxs-lookup"><span data-stu-id="3955f-108">New host features including Web Jobs, custom CNames, better monitoring</span></span>
* <span data-ttu-id="3955f-109">nyckelfärdig integrering med Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="3955f-109">Turnkey integration with Traffic Manager</span></span>
* <span data-ttu-id="3955f-110">anslutning till lokala resurser och VPN-nätverk via VNet som komplement till hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="3955f-110">Connectivity to your on-premises resources and VPNs using VNet in addition to Hybrid Connections</span></span>
* <span data-ttu-id="3955f-111">övervakning, aviseringar och felsökning av appar med NewRelic eller AppInsights</span><span class="sxs-lookup"><span data-stu-id="3955f-111">Monitoring, alerting and  troubleshooting for your app using NewRelic or AppInsights</span></span>
* <span data-ttu-id="3955f-112">större utbud av underliggande beräkningsresurser och prissättning</span><span class="sxs-lookup"><span data-stu-id="3955f-112">Richer spectrum of the underlying compute resources and pricing</span></span>
* <span data-ttu-id="3955f-113">inbyggd automatisk skalning, belastningsutjämning och prestandaövervakning</span><span class="sxs-lookup"><span data-stu-id="3955f-113">Built-in auto scale, load balancing, and performance monitoring.</span></span>
* <span data-ttu-id="3955f-114">inbyggd mellanlagring, säkerhetskopiering, återställning och möjlighet till testning i produktionsmiljö</span><span class="sxs-lookup"><span data-stu-id="3955f-114">Built-in staging, backup, roll-back, and testing-in-production capabilities</span></span>

## <a name="new-hosting-features"></a><span data-ttu-id="3955f-115">Nya värdfunktioner</span><span class="sxs-lookup"><span data-stu-id="3955f-115">New hosting features</span></span>
<span data-ttu-id="3955f-116">I *Azure Apptjänst* körs serverdelskoden för *mobilappar* i samma behållare som för webbappen och API-appen.</span><span class="sxs-lookup"><span data-stu-id="3955f-116">In *Azure App Service* the *Mobile App* backend code runs in the same container as Web App and API App.</span></span> <span data-ttu-id="3955f-117">Det innebär att du kan använda alla funktionerna i den här behållaren, inklusive några som för närvarande inte finns i Mobile Services:</span><span class="sxs-lookup"><span data-stu-id="3955f-117">As such you can take advantage of all the features in this container, including some of those that are not currently present in Mobile Services:</span></span>

* <span data-ttu-id="3955f-118">Du kan lägga till serverdelslogik som körs hela tiden via WebJobs.</span><span class="sxs-lookup"><span data-stu-id="3955f-118">Add continuously running backend logic via Web Jobs</span></span>
* <span data-ttu-id="3955f-119">Du kan säkerställa att serverdelskoden alltid körs.</span><span class="sxs-lookup"><span data-stu-id="3955f-119">Ensure your backend code is always running</span></span>
* <span data-ttu-id="3955f-120">Du kan använda anpassade CNames-poster och sätta egna stabila namn på dina mobila serverdelsslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="3955f-120">Use custom CNames to provide friendly and stable names to your mobile backend endpoints</span></span>
* <span data-ttu-id="3955f-121">Du kan geoskala appen med Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="3955f-121">Geo-scale your app with Traffic Manager</span></span>
* <span data-ttu-id="3955f-122">Du kan ha med vilka bibliotek och paket du vill.</span><span class="sxs-lookup"><span data-stu-id="3955f-122">Include any libraries and packages you want.</span></span>
* <span data-ttu-id="3955f-123">(För .NET) Du kan använda alla funktioner i ASP.NET, även MVC.</span><span class="sxs-lookup"><span data-stu-id="3955f-123">(For .NET) Leverage any feature of ASP.NET, including MVC</span></span>
* <span data-ttu-id="3955f-124">(För Node.js) Du kan använda alla bibliotek med endast JavaScript i Node-ekosystemet, även vanliga MVC-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="3955f-124">(For Node.js) Leverage any pure JavaScript library of the Node ecosystem, including common MVC libraries.</span></span>

## <a name="access-on-premises-data-using-vnet"></a><span data-ttu-id="3955f-125">Möjlighet att nå lokala data via VNet</span><span class="sxs-lookup"><span data-stu-id="3955f-125">Access on-premises data using VNet</span></span>
<span data-ttu-id="3955f-126">Med Mobile Services kan du redan idag använda hybridanslutningar för att komma åt lokala resurser.</span><span class="sxs-lookup"><span data-stu-id="3955f-126">With Mobile Services today you can already use Hybrid Connections to access on-premises resources.</span></span> <span data-ttu-id="3955f-127">Det finns dock tillfällen då det är bäst med VPN-lösning.</span><span class="sxs-lookup"><span data-stu-id="3955f-127">However there are situations where a VPN solution is preferred.</span></span> <span data-ttu-id="3955f-128">Med *Azure Apptjänst* kan du använda Azure VNet för serverdelskoden till mobilappar.</span><span class="sxs-lookup"><span data-stu-id="3955f-128">With *Azure App Service* you can use Azure VNet for your Mobile App backend code.</span></span>

## <a name="use-your-favorite-backend-language"></a><span data-ttu-id="3955f-129">Använd det serverdelsspråk som du gillar bäst</span><span class="sxs-lookup"><span data-stu-id="3955f-129">Use your favorite backend language</span></span>
<span data-ttu-id="3955f-130">*Azure Apptjänst* har bredare och bättre stöd för ASP.NET- och Node.js-plattformarna, inklusive tillgång till de senaste körningarna.</span><span class="sxs-lookup"><span data-stu-id="3955f-130">*Azure App Service* offers broader and richer support for ASP.NET and Node.js platforms, including access to the latest runtimes.</span></span>

## <a name="set-up-automatic-scale"></a><span data-ttu-id="3955f-131">Automatisk skalning</span><span class="sxs-lookup"><span data-stu-id="3955f-131">Set up automatic scale</span></span>
<span data-ttu-id="3955f-132">Med Mobile Services kördes alla instanser av serverdelskoden på små virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3955f-132">With Mobile Services, all instances of your backend code were running on Small VMs.</span></span> <span data-ttu-id="3955f-133">Med *Azure Apptjänst* kan du välja storlek på de virtuella datorerna bland ett mycket större antal alternativ.</span><span class="sxs-lookup"><span data-stu-id="3955f-133">*Azure App Service* enables you to select the size of the VMs from a much richer set of options.</span></span> <span data-ttu-id="3955f-134">Du kan även snabbt skala upp eller ut för att hantera inkommande kundbelastning utifrån olika prestandamått.</span><span class="sxs-lookup"><span data-stu-id="3955f-134">You can also  quickly scale up or out to handle any incoming customer load, based on various performance metrics.</span></span>

## <a name="be-in-the-know"></a><span data-ttu-id="3955f-135">Håll koll</span><span class="sxs-lookup"><span data-stu-id="3955f-135">Be in the “know”</span></span>
<span data-ttu-id="3955f-136">Du kan åtgärda problem i realtid genom övervakning och aviseringar så att du och dina kolleger alltid har koll på vad som händer.</span><span class="sxs-lookup"><span data-stu-id="3955f-136">React to issues in real-time with monitoring and alerts to automatically notify you and your team.</span></span> <span data-ttu-id="3955f-137">Du kan integrera avancerad appanalys och övervakningsfunktioner från New Relic och AppInsights och få ännu bättre inblick i vad som händer med din mobilapp.</span><span class="sxs-lookup"><span data-stu-id="3955f-137">Integrate advanced app analytics and monitoring functionality from New Relic and AppInsights to get even richer insight into how your Mobile app is performing.</span></span> <span data-ttu-id="3955f-138">Med *Azure Apptjänst* kan du nu ställa in aviseringar på basis av flera olika prestandamått, antingen genom att programmera eller via Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="3955f-138">With *Azure App Service* you can now setup alerts based on variety of performance metrics, either programatically and via the Azure Portal.</span></span>

## <a name="keep-your-assets-safe"></a><span data-ttu-id="3955f-139">Dina tillgångar är trygga</span><span class="sxs-lookup"><span data-stu-id="3955f-139">Keep your assets safe</span></span>
<span data-ttu-id="3955f-140">Serverdelen och databasen kan säkerhetskopieras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3955f-140">Automatically back up your backend and database.</span></span> <span data-ttu-id="3955f-141">Kod och data är skyddade vid katastrofer och kan lätt återställas så att du kan bedriva din verksamhet utan att behöva oroa dig.</span><span class="sxs-lookup"><span data-stu-id="3955f-141">Your code and data is secure from disaster and easily restored, allowing you to run your business with confidence.</span></span>

## <a name="ready-stage-go"></a><span data-ttu-id="3955f-142">Klara, mellanlagra, kör!</span><span class="sxs-lookup"><span data-stu-id="3955f-142">Ready, Stage, Go!</span></span>
<span data-ttu-id="3955f-143">Med *Azure Apptjänst* kan du nu skapa flera olika privata testnings- och mellanlagringsmiljöer för dina mobilappar.</span><span class="sxs-lookup"><span data-stu-id="3955f-143">With *Azure App Service* you can now create multiple private testing and staging environments for your mobile apps.</span></span> <span data-ttu-id="3955f-144">I de här miljöerna kan du testa apparna innan du distribuerar ut dem.</span><span class="sxs-lookup"><span data-stu-id="3955f-144">Use them to perform testing before you deploy.</span></span> <span data-ttu-id="3955f-145">Du kan växla till produktionsmiljö utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="3955f-145">Swap to production with no downtime.</span></span> <span data-ttu-id="3955f-146">Webbappar är förinstallerade, vilket ger bästa möjliga kundupplevelse.</span><span class="sxs-lookup"><span data-stu-id="3955f-146">Web apps are pre-loaded, ensuring the best customer experience.</span></span>

<span data-ttu-id="3955f-147">Du kan komma igång och utnyttja alla fördelarna med *Apptjänst* för din befintliga mobiltjänst genom att gå igenom den här [kursen](app-service-mobile-migrating-from-mobile-services.md).</span><span class="sxs-lookup"><span data-stu-id="3955f-147">You can get start taking advantage of *App Service* for your existing Mobile Service by following this [tutorial](app-service-mobile-migrating-from-mobile-services.md).</span></span>
