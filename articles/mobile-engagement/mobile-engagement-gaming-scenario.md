---
title: "aaaAzure Mobile Engagement-implementering för Spelappen"
description: Spel app scenariot tooimplement Azure Mobile Engagement
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
ms.openlocfilehash: b82b4a868a33f42e5b759e43e66103556c097f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a><span data-ttu-id="59c1b-103">Implementerar Mobile Engagement med Spelappen</span><span class="sxs-lookup"><span data-stu-id="59c1b-103">Implement Mobile Engagement with Gaming App</span></span>
## <a name="overview"></a><span data-ttu-id="59c1b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="59c1b-104">Overview</span></span>
<span data-ttu-id="59c1b-105">En spel Startup har startas en ny fiske baserat role play/strategi spel app.</span><span class="sxs-lookup"><span data-stu-id="59c1b-105">A gaming start-up has launched a new fishing based role-play/strategy game app.</span></span> <span data-ttu-id="59c1b-106">hello spelet har varit igång i sex månader.</span><span class="sxs-lookup"><span data-stu-id="59c1b-106">hello game has been up and running for 6 months.</span></span> <span data-ttu-id="59c1b-107">Spelet är mycket stora lyckas, och den har miljontals hämtningar och hello är mycket hög jämfört med tooother uppstart spel appar.</span><span class="sxs-lookup"><span data-stu-id="59c1b-107">This game is a huge success, and it has millions of downloads and hello retention is very high compared tooother start-up game apps.</span></span> <span data-ttu-id="59c1b-108">Vid hello kvartalsvis granskningsmöte accepterar intressenter de behöver tooincrease genomsnittliga intäkter per användare (ARPU).</span><span class="sxs-lookup"><span data-stu-id="59c1b-108">At hello quarterly review meeting, stakeholders agree they need tooincrease average revenue per user (ARPU).</span></span> <span data-ttu-id="59c1b-109">Premium i spelet paket är tillgängliga som specialerbjudanden.</span><span class="sxs-lookup"><span data-stu-id="59c1b-109">Premium in-game packages are available as special offers.</span></span> <span data-ttu-id="59c1b-110">Dessa spel Pack Tillåt användare tooupgrade hello utseende och prestanda på deras fiske rader och bete eller tackles i hello spel.</span><span class="sxs-lookup"><span data-stu-id="59c1b-110">These game packs allow users tooupgrade hello appearance and performance of their fishing lines and lures or tackles in hello game.</span></span> <span data-ttu-id="59c1b-111">Paketet försäljning är dock mycket låg.</span><span class="sxs-lookup"><span data-stu-id="59c1b-111">However, package sales are very low.</span></span> <span data-ttu-id="59c1b-112">Så att de bestämma med ett webbanalysverktyg för till första tooanalyze hello customer experience och sedan toodevelop en engagement program tooincrease försäljning med avancerade segmentering.</span><span class="sxs-lookup"><span data-stu-id="59c1b-112">So they decide first tooanalyze hello customer experience with an analytics tool, and then toodevelop an engagement program tooincrease sales using advanced segmentation.</span></span>

<span data-ttu-id="59c1b-113">Baserat på hello [Azure Mobile Engagement – komma igång med bästa praxis](mobile-engagement-getting-started-best-practices.md) de skapar en strategi.</span><span class="sxs-lookup"><span data-stu-id="59c1b-113">Based on hello [Azure Mobile Engagement - Getting Started Guide with Best practices](mobile-engagement-getting-started-best-practices.md) they build an engagement strategy.</span></span>

## <a name="objectives-and-kpis"></a><span data-ttu-id="59c1b-114">Mål och KPI: er</span><span class="sxs-lookup"><span data-stu-id="59c1b-114">Objectives and KPIs</span></span>
<span data-ttu-id="59c1b-115">Viktiga intressenter för hello spel uppfylla.</span><span class="sxs-lookup"><span data-stu-id="59c1b-115">Key stakeholders for hello game meet.</span></span> <span data-ttu-id="59c1b-116">Komma överens om en Huvudsyftet - tooincrease premium paketet försäljningar efter 15%.</span><span class="sxs-lookup"><span data-stu-id="59c1b-116">All agree on one main objective - tooincrease premium package sales by 15%.</span></span> <span data-ttu-id="59c1b-117">De skapar toomeasure (Business Key Performance Indicator) och enheten detta mål</span><span class="sxs-lookup"><span data-stu-id="59c1b-117">They create Business Key Performance Indicators (KPIs) toomeasure and drive this objective</span></span>

* <span data-ttu-id="59c1b-118">På vilken nivå av hello spelet köps paketen?</span><span class="sxs-lookup"><span data-stu-id="59c1b-118">On which level of hello game are these packages purchased?</span></span>
* <span data-ttu-id="59c1b-119">Vad är hello intäkter per användare, sessioner, per vecka och per månad?</span><span class="sxs-lookup"><span data-stu-id="59c1b-119">What is hello revenue per user, per session, per week, and per month?</span></span>
* <span data-ttu-id="59c1b-120">Vad är hello favorit köp typer?</span><span class="sxs-lookup"><span data-stu-id="59c1b-120">What are hello favorite purchase types?</span></span>

<span data-ttu-id="59c1b-121">Del 1 av hello [komma igång](mobile-engagement-getting-started-best-practices.md) förklarar hur toodefine hello mål och KPI: er.</span><span class="sxs-lookup"><span data-stu-id="59c1b-121">Part 1 of hello [Getting Started Guide](mobile-engagement-getting-started-best-practices.md) explains how toodefine hello objectives and KPIs.</span></span> 

<span data-ttu-id="59c1b-122">Med hello nu definierat KPI: er, skapar hello Mobile produkten Manager Engagement KPI: er toodetermine nya användare trender och lagring.</span><span class="sxs-lookup"><span data-stu-id="59c1b-122">With hello Business KPIs now defined, hello Mobile Product Manager creates Engagement KPIs toodetermine new user trends and retention.</span></span>

* <span data-ttu-id="59c1b-123">Övervaka kvarhållning och använda på hello följande intervall: varje dag, varje 2 dagar, varje vecka, månad och var tredje månad</span><span class="sxs-lookup"><span data-stu-id="59c1b-123">Monitor retention and use across hello following intervals: daily, every 2 days, weekly, monthly and every 3 months</span></span>
* <span data-ttu-id="59c1b-124">Aktiva användare</span><span class="sxs-lookup"><span data-stu-id="59c1b-124">Active user counts</span></span>
* <span data-ttu-id="59c1b-125">Hej appklassificering i hello lagra</span><span class="sxs-lookup"><span data-stu-id="59c1b-125">hello app rating in hello store</span></span>

<span data-ttu-id="59c1b-126">Baserat på rekommendationer från hello IT-teamet hello följande tekniska KPI: er har lagts till tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="59c1b-126">Based on recommendations from hello IT team, hello following technical KPIs were added tooanswer hello following questions:</span></span>

* <span data-ttu-id="59c1b-127">Vad är Mina användare sökväg (vilken sida besökta, hur mycket tid användare spendera på den)</span><span class="sxs-lookup"><span data-stu-id="59c1b-127">What is my user path (which page is visited, how much time users spend on it)</span></span>
* <span data-ttu-id="59c1b-128">Antal krascher eller buggar påträffade per session</span><span class="sxs-lookup"><span data-stu-id="59c1b-128">Number of crashes or bugs encountered per session</span></span>
* <span data-ttu-id="59c1b-129">Vilka operativsystemversioner körs Mina användare?</span><span class="sxs-lookup"><span data-stu-id="59c1b-129">What OS versions are my users running?</span></span>
* <span data-ttu-id="59c1b-130">Vad är hello genomsnittlig storlek på skärmen för Mina användare?</span><span class="sxs-lookup"><span data-stu-id="59c1b-130">What is hello average size of screen for my users?</span></span>
* <span data-ttu-id="59c1b-131">Vilken typ av internet-anslutning har Mina användare?</span><span class="sxs-lookup"><span data-stu-id="59c1b-131">What kind of internet connectivity do my users have?</span></span>

<span data-ttu-id="59c1b-132">För varje KPI anger hello Mobile produkten Manager hello data hon och var den finns i sitt playbook.</span><span class="sxs-lookup"><span data-stu-id="59c1b-132">For each KPI hello Mobile Product Manager specifies hello data she needs and where it is located in her playbook.</span></span>

## <a name="engagement-program-and-integration"></a><span data-ttu-id="59c1b-133">Program för användarinteraktion och integrering</span><span class="sxs-lookup"><span data-stu-id="59c1b-133">Engagement program and integration</span></span>
<span data-ttu-id="59c1b-134">Innan du skapar en avancerad engagement-programmet ska hello Mobile projektet Director ansvarig hello projektet ha en djupgående förståelse för hur och när produkter förbrukas av hello användare.</span><span class="sxs-lookup"><span data-stu-id="59c1b-134">Before building an advanced engagement program, hello Mobile Project Director in charge of hello project should have a deep understanding of how and when products are consumed by hello users.</span></span>

<span data-ttu-id="59c1b-135">Efter tre månader hello Mobile projektet Director har samlat in tillräckligt med data tooenhance sin app push notification försäljning.</span><span class="sxs-lookup"><span data-stu-id="59c1b-135">After 3 months, hello Mobile Project Director has collected enough data tooenhance his in-app push notification sales.</span></span> <span data-ttu-id="59c1b-136">Han Lär som:</span><span class="sxs-lookup"><span data-stu-id="59c1b-136">He learns that:</span></span>

* <span data-ttu-id="59c1b-137">hello första inköpet sker vanligtvis på hello nivå 14.</span><span class="sxs-lookup"><span data-stu-id="59c1b-137">hello first purchase generally happens at hello level 14.</span></span> <span data-ttu-id="59c1b-138">Hello inköp är 90% av de fall nya legendariska vapen för $3.</span><span class="sxs-lookup"><span data-stu-id="59c1b-138">For 90% of those cases, hello purchase is new legendary weapons for $3.</span></span>
* <span data-ttu-id="59c1b-139">Användare som har gjort ett inköp fortsätta med hello produkt och göra mer inköp i 80% av de fallen.</span><span class="sxs-lookup"><span data-stu-id="59c1b-139">In 80 % of those cases, users who have made a purchase, continue with hello product and make more purchases.</span></span>
* <span data-ttu-id="59c1b-140">Användare som har klarat hello nivå 20, starta toospend mer än 10 $ i veckan.</span><span class="sxs-lookup"><span data-stu-id="59c1b-140">Users who have passed hello level 20, start toospend more than $10/week.</span></span>
* <span data-ttu-id="59c1b-141">Användare tenderar toobuy premium-paket på nivå 16, 24 och 32.</span><span class="sxs-lookup"><span data-stu-id="59c1b-141">Users tend toobuy premium packages at level 16, 24 and 32.</span></span>

<span data-ttu-id="59c1b-142">Tack toothis analys hello Mobile projektet Director beslutar toocreate vissa push notification-sekvenser tooincrease i appen försäljning.</span><span class="sxs-lookup"><span data-stu-id="59c1b-142">Thanks toothis analysis hello Mobile Project Director decides toocreate specific push notification sequences tooincrease in app sales.</span></span> <span data-ttu-id="59c1b-143">Han skapar tre push-sekvenser som han kallar: Välkommen program, försäljning Program och inaktiva Program.</span><span class="sxs-lookup"><span data-stu-id="59c1b-143">He creates three push sequences which he calls: Welcome program, Sales Program, and Inactive Program.</span></span> <span data-ttu-id="59c1b-144">Mer information finns i toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]</span><span class="sxs-lookup"><span data-stu-id="59c1b-144">For more information refer toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks) ![][1]</span></span>

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
