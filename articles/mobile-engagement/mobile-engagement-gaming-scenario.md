---
title: "Azure Mobile Engagement-implementering för Spelappen"
description: Spel app scenario om du vill implementera Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2cafc044-4902-4058-8037-49399bf6bf7f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0ca35a3d634db8eb5c63afacba046a35b8a3e7ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a><span data-ttu-id="2c61d-103">Implementerar Mobile Engagement med Spelappen</span><span class="sxs-lookup"><span data-stu-id="2c61d-103">Implement Mobile Engagement with Gaming App</span></span>
## <a name="overview"></a><span data-ttu-id="2c61d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2c61d-104">Overview</span></span>
<span data-ttu-id="2c61d-105">En spel Startup har startas en ny fiske baserat role play/strategi spel app.</span><span class="sxs-lookup"><span data-stu-id="2c61d-105">A gaming start-up has launched a new fishing based role-play/strategy game app.</span></span> <span data-ttu-id="2c61d-106">Spelet har varit igång i sex månader.</span><span class="sxs-lookup"><span data-stu-id="2c61d-106">The game has been up and running for 6 months.</span></span> <span data-ttu-id="2c61d-107">Spelet är mycket stora lyckas, och den har miljontals hämtningar och kvarhållning är mycket högt jämfört med andra uppstart spel appar.</span><span class="sxs-lookup"><span data-stu-id="2c61d-107">This game is a huge success, and it has millions of downloads and the retention is very high compared to other start-up game apps.</span></span> <span data-ttu-id="2c61d-108">Möte kvartalsvis granska accepterar intressenter de behöver för att öka genomsnittliga intäkter per användare (ARPU).</span><span class="sxs-lookup"><span data-stu-id="2c61d-108">At the quarterly review meeting, stakeholders agree they need to increase average revenue per user (ARPU).</span></span> <span data-ttu-id="2c61d-109">Premium i spelet paket är tillgängliga som specialerbjudanden.</span><span class="sxs-lookup"><span data-stu-id="2c61d-109">Premium in-game packages are available as special offers.</span></span> <span data-ttu-id="2c61d-110">Dessa spel Pack kan du uppgradera utseende och prestanda på deras fiske rader och bete eller tackles i spelet.</span><span class="sxs-lookup"><span data-stu-id="2c61d-110">These game packs allow users to upgrade the appearance and performance of their fishing lines and lures or tackles in the game.</span></span> <span data-ttu-id="2c61d-111">Paketet försäljning är dock mycket låg.</span><span class="sxs-lookup"><span data-stu-id="2c61d-111">However, package sales are very low.</span></span> <span data-ttu-id="2c61d-112">Därför vill först analysera kundupplevelsen med ett webbanalysverktyg för och sedan för att utveckla ett uppdrag att öka försäljning med avancerade segmentering.</span><span class="sxs-lookup"><span data-stu-id="2c61d-112">So they decide first to analyze the customer experience with an analytics tool, and then to develop an engagement program to increase sales using advanced segmentation.</span></span>

<span data-ttu-id="2c61d-113">Baserat på de [Azure Mobile Engagement – komma igång med bästa praxis](mobile-engagement-getting-started-best-practices.md) de skapar en strategi.</span><span class="sxs-lookup"><span data-stu-id="2c61d-113">Based on the [Azure Mobile Engagement - Getting Started Guide with Best practices](mobile-engagement-getting-started-best-practices.md) they build an engagement strategy.</span></span>

## <a name="objectives-and-kpis"></a><span data-ttu-id="2c61d-114">Mål och KPI: er</span><span class="sxs-lookup"><span data-stu-id="2c61d-114">Objectives and KPIs</span></span>
<span data-ttu-id="2c61d-115">Viktiga intressenter för spelet uppfylla.</span><span class="sxs-lookup"><span data-stu-id="2c61d-115">Key stakeholders for the game meet.</span></span> <span data-ttu-id="2c61d-116">Komma överens om en Huvudsyftet - att öka försäljningen för premium-paket med 15%.</span><span class="sxs-lookup"><span data-stu-id="2c61d-116">All agree on one main objective - to increase premium package sales by 15%.</span></span> <span data-ttu-id="2c61d-117">De skapar (Business Key Performance Indicator) som mäter och enhet för detta mål</span><span class="sxs-lookup"><span data-stu-id="2c61d-117">They create Business Key Performance Indicators (KPIs) to measure and drive this objective</span></span>

* <span data-ttu-id="2c61d-118">På vilken nivå av spelet köps paketen?</span><span class="sxs-lookup"><span data-stu-id="2c61d-118">On which level of the game are these packages purchased?</span></span>
* <span data-ttu-id="2c61d-119">Vad är intäkter per användare, sessioner, per vecka och per månad?</span><span class="sxs-lookup"><span data-stu-id="2c61d-119">What is the revenue per user, per session, per week, and per month?</span></span>
* <span data-ttu-id="2c61d-120">Vilka är typerna favorit inköp?</span><span class="sxs-lookup"><span data-stu-id="2c61d-120">What are the favorite purchase types?</span></span>

<span data-ttu-id="2c61d-121">Del 1 av den [komma igång](mobile-engagement-getting-started-best-practices.md) förklarar hur du definierar mål och KPI: er.</span><span class="sxs-lookup"><span data-stu-id="2c61d-121">Part 1 of the [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) explains how to define the objectives and KPIs.</span></span> 

<span data-ttu-id="2c61d-122">Med affärsverksamhet nu definierat, skapar produkten-hanteraren Mobile Engagement KPI: er för att fastställa den nya användaren trender och lagring.</span><span class="sxs-lookup"><span data-stu-id="2c61d-122">With the Business KPIs now defined, the Mobile Product Manager creates Engagement KPIs to determine new user trends and retention.</span></span>

* <span data-ttu-id="2c61d-123">Övervaka kvarhållning och använda på följande intervall: varje dag, varje 2 dagar, varje vecka, månad och var tredje månad</span><span class="sxs-lookup"><span data-stu-id="2c61d-123">Monitor retention and use across the following intervals: daily, every 2 days, weekly, monthly and every 3 months</span></span>
* <span data-ttu-id="2c61d-124">Aktiva användare</span><span class="sxs-lookup"><span data-stu-id="2c61d-124">Active user counts</span></span>
* <span data-ttu-id="2c61d-125">Appklassificering i arkivet</span><span class="sxs-lookup"><span data-stu-id="2c61d-125">The app rating in the store</span></span>

<span data-ttu-id="2c61d-126">Baserat på rekommendationer från IT-teamet har följande tekniska KPI: er lagts till i besvara följande frågor:</span><span class="sxs-lookup"><span data-stu-id="2c61d-126">Based on recommendations from the IT team, the following technical KPIs were added to answer the following questions:</span></span>

* <span data-ttu-id="2c61d-127">Vad är Mina användare sökväg (vilken sida besökta, hur mycket tid användare spendera på den)</span><span class="sxs-lookup"><span data-stu-id="2c61d-127">What is my user path (which page is visited, how much time users spend on it)</span></span>
* <span data-ttu-id="2c61d-128">Antal krascher eller buggar påträffade per session</span><span class="sxs-lookup"><span data-stu-id="2c61d-128">Number of crashes or bugs encountered per session</span></span>
* <span data-ttu-id="2c61d-129">Vilka operativsystemversioner körs Mina användare?</span><span class="sxs-lookup"><span data-stu-id="2c61d-129">What OS versions are my users running?</span></span>
* <span data-ttu-id="2c61d-130">Vad är den genomsnittliga storleken på skärmen för Mina användare?</span><span class="sxs-lookup"><span data-stu-id="2c61d-130">What is the average size of screen for my users?</span></span>
* <span data-ttu-id="2c61d-131">Vilken typ av internet-anslutning har Mina användare?</span><span class="sxs-lookup"><span data-stu-id="2c61d-131">What kind of internet connectivity do my users have?</span></span>

<span data-ttu-id="2c61d-132">För varje KPI anger Mobile produkten Manager data hon och var den finns i sitt playbook.</span><span class="sxs-lookup"><span data-stu-id="2c61d-132">For each KPI the Mobile Product Manager specifies the data she needs and where it is located in her playbook.</span></span>

## <a name="engagement-program-and-integration"></a><span data-ttu-id="2c61d-133">Program för användarinteraktion och integrering</span><span class="sxs-lookup"><span data-stu-id="2c61d-133">Engagement program and integration</span></span>
<span data-ttu-id="2c61d-134">Innan du skapar ett program för avancerade användarinteraktion bör Mobile projektet Director ansvarig för projektet ha en djupgående förståelse av hur och när produkter förbrukas av användare.</span><span class="sxs-lookup"><span data-stu-id="2c61d-134">Before building an advanced engagement program, the Mobile Project Director in charge of the project should have a deep understanding of how and when products are consumed by the users.</span></span>

<span data-ttu-id="2c61d-135">Mobila projekt Director har samlat in tillräckligt med data för att förbättra sin app push notification försäljning efter tre månader.</span><span class="sxs-lookup"><span data-stu-id="2c61d-135">After 3 months, the Mobile Project Director has collected enough data to enhance his in-app push notification sales.</span></span> <span data-ttu-id="2c61d-136">Han Lär som:</span><span class="sxs-lookup"><span data-stu-id="2c61d-136">He learns that:</span></span>

* <span data-ttu-id="2c61d-137">Det första inköpet sker vanligtvis på nivån 14.</span><span class="sxs-lookup"><span data-stu-id="2c61d-137">The first purchase generally happens at the level 14.</span></span> <span data-ttu-id="2c61d-138">Köpet är 90% av de fall nya legendariska vapen för $3.</span><span class="sxs-lookup"><span data-stu-id="2c61d-138">For 90% of those cases, the purchase is new legendary weapons for $3.</span></span>
* <span data-ttu-id="2c61d-139">Användare som har gjort ett inköp fortsätta med produkten och göra mer inköp i 80% av de fallen.</span><span class="sxs-lookup"><span data-stu-id="2c61d-139">In 80 % of those cases, users who have made a purchase, continue with the product and make more purchases.</span></span>
* <span data-ttu-id="2c61d-140">Användare som har klarat nivån 20, starta ägna mer än 10 $/ vecka.</span><span class="sxs-lookup"><span data-stu-id="2c61d-140">Users who have passed the level 20, start to spend more than $10/week.</span></span>
* <span data-ttu-id="2c61d-141">Tenderar användare att köpa premium-paket på nivå 16, 24 och 32.</span><span class="sxs-lookup"><span data-stu-id="2c61d-141">Users tend to buy premium packages at level 16, 24 and 32.</span></span>

<span data-ttu-id="2c61d-142">Tack vare den här analysen Mobile projektet Director bestämmer sig för att skapa vissa push notification-sekvenser ökar i appen försäljning.</span><span class="sxs-lookup"><span data-stu-id="2c61d-142">Thanks to this analysis the Mobile Project Director decides to create specific push notification sequences to increase in app sales.</span></span> <span data-ttu-id="2c61d-143">Han skapar tre push-sekvenser som han kallar: Välkommen program, försäljning Program och inaktiva Program.</span><span class="sxs-lookup"><span data-stu-id="2c61d-143">He creates three push sequences which he calls: Welcome program, Sales Program, and Inactive Program.</span></span> <span data-ttu-id="2c61d-144">Mer information finns i den [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]</span><span class="sxs-lookup"><span data-stu-id="2c61d-144">For more information refer to the [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks) ![][1]</span></span>

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
