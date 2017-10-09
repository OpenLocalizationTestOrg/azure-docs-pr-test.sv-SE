---
title: "aaaAzure App Service-planer djupgående översikt | Microsoft Docs"
description: "Lär dig hur App Service-planer för Azure App Service och hur de får din hanteringsupplevelse."
keywords: app service, azure app service, scale, scalable, app service plan, app service cost
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: byvinyal
ms.openlocfilehash: b384790d9e69b234ca69ac591164c48a4b6ed210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a><span data-ttu-id="5f9b4-104">Detaljerad översikt över Azure App Service-avtal</span><span class="sxs-lookup"><span data-stu-id="5f9b4-104">Azure App Service plans in-depth overview</span></span>

<span data-ttu-id="5f9b4-105">Programtjänstplaner representerar hello mängd fysiska resurser som används toohost dina appar.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-105">App Service plans represent hello collection of physical resources used toohost your apps.</span></span>

<span data-ttu-id="5f9b4-106">App Service-planer definierar följande:</span><span class="sxs-lookup"><span data-stu-id="5f9b4-106">App Service plans define:</span></span>

- <span data-ttu-id="5f9b4-107">Region (västra USA, östra USA osv.)</span><span class="sxs-lookup"><span data-stu-id="5f9b4-107">Region (West US, East US, etc.)</span></span>
- <span data-ttu-id="5f9b4-108">Skalningsantalet (en, två, tre instanser, etc.)</span><span class="sxs-lookup"><span data-stu-id="5f9b4-108">Scale count (one, two, three instances, etc.)</span></span>
- <span data-ttu-id="5f9b4-109">Storlek (Small, Medium, Large)</span><span class="sxs-lookup"><span data-stu-id="5f9b4-109">Instance size (Small, Medium, Large)</span></span>
- <span data-ttu-id="5f9b4-110">SKU (Kostnadsfri, Delad, Basic, Standard, Premium)</span><span class="sxs-lookup"><span data-stu-id="5f9b4-110">SKU (Free, Shared, Basic, Standard, Premium)</span></span>

<span data-ttu-id="5f9b4-111">Web Apps, Mobilappar, API Apps, funktionen appar (eller funktioner) i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) alla kör i en apptjänstplan.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-111">Web Apps, Mobile Apps, API Apps, Function Apps (or Functions), in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) all run in an App Service plan.</span></span>  <span data-ttu-id="5f9b4-112">Appar i hello samma prenumeration och region kan dela en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-112">Apps in hello same subscription, and region can share an App Service plan.</span></span> 

<span data-ttu-id="5f9b4-113">Alla program som tilldelats tooan **programtjänstplanen** dela hello resurser som definieras av den.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-113">All applications assigned tooan **App Service plan** share hello resources defined by it.</span></span> <span data-ttu-id="5f9b4-114">Den här delning sparar pengar när värd för flera appar i en enda App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-114">This sharing saves money when hosting multiple apps in a single App Service plan.</span></span>

<span data-ttu-id="5f9b4-115">Din **programtjänstplanen** kan skala från **lediga** och **delade** SKU: er för**grundläggande**, **Standard**, och **Premium** SKU: er som ger dig åtkomst till toomore resurser och funktioner längs hello sätt.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-115">Your **App Service plan** can scale from **Free** and **Shared** SKUs too**Basic**, **Standard**, and **Premium** SKUs giving you access toomore resources and features along hello way.</span></span>

<span data-ttu-id="5f9b4-116">Om din App Service-plan anges för**grundläggande** SKU eller senare, och du kan styra hello **storlek** och skala antalet hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-116">If your App Service plan is set too**Basic** SKU or higher, then you can control hello **size** and scale count of hello VMs.</span></span>

<span data-ttu-id="5f9b4-117">Om din plan är konfigurerade toouse två ”liten” instanser i hello standard-tjänstnivå, till exempel köra alla appar som är kopplade med planen på båda instanser.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-117">For example, if your plan is configured toouse two "small" instances in hello standard service tier, all apps that are associated with that plan run on both instances.</span></span> <span data-ttu-id="5f9b4-118">Appar har också åtkomst toohello standard service tier funktioner.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-118">Apps also have access toohello standard service tier features.</span></span> <span data-ttu-id="5f9b4-119">Instanser av programtjänstplanen som kör appar är helt hanterad och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-119">Plan instances on which apps are running are fully managed and highly available.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5f9b4-120">Hej **SKU** och **skala** av hello App Service plan avgör hello kostnad och inte hello många appar i den.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-120">hello **SKU** and **Scale** of hello App Service plan determines hello cost and not hello number of apps hosted in it.</span></span>

<span data-ttu-id="5f9b4-121">Den här artikeln innehåller hello viktiga egenskaper som till exempel nivå och skalan för en apptjänstplan och hur de komma till användning vid hantering av dina appar.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-121">This article explores hello key characteristics, such as tier and scale, of an App Service plan and how they come into play while managing your apps.</span></span>

## <a name="apps-and-app-service-plans"></a><span data-ttu-id="5f9b4-122">Appar och App Service-planer</span><span class="sxs-lookup"><span data-stu-id="5f9b4-122">Apps and App Service plans</span></span>

<span data-ttu-id="5f9b4-123">En app i App Service kan associeras med endast en App Service-plan vid en given tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-123">An app in App Service can be associated with only one App Service plan at any given time.</span></span>

<span data-ttu-id="5f9b4-124">Både appar och planer finns i en **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-124">Both apps and plans are contained in a **resource group**.</span></span> <span data-ttu-id="5f9b4-125">En resursgrupp fungerar som hello livscykel gräns för varje resurs som ligger inom det.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-125">A resource group serves as hello lifecycle boundary for every resource that's within it.</span></span> <span data-ttu-id="5f9b4-126">Du kan använda resursen grupper toomanage alla hello delar i ett program tillsammans.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-126">You can use resource groups toomanage all hello pieces of an application together.</span></span>

<span data-ttu-id="5f9b4-127">Eftersom en enskild resursgrupp kan ha flera programtjänstplaner, kan du allokera olika appar toodifferent fysiska resurser.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-127">Because a single resource group can have multiple App Service plans, you can allocate different apps toodifferent physical resources.</span></span>

<span data-ttu-id="5f9b4-128">Du kan till exempel separata resurser mellan dev, test- och miljöer.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-128">For example, you can separate resources among dev, test, and production environments.</span></span> <span data-ttu-id="5f9b4-129">Med separata miljöer för produktion och utveckling och testning kan du isolera resurser.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-129">Having separate environments for production and dev/test lets you isolate resources.</span></span> <span data-ttu-id="5f9b4-130">På så sätt kan läsa in testar mot en ny version av dina appar inte konkurrerar för hello samma resurser som dina appar för produktion som hanterar verkliga kunder.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-130">In this way, load testing against a new version of your apps does not compete for hello same resources as your production apps, which are serving real customers.</span></span>

<span data-ttu-id="5f9b4-131">När du har flera planer i en enskild resursgrupp kan du också definiera ett program som omfattar geografiska regioner.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-131">When you have multiple plans in a single resource group, you can also define an application that spans geographical regions.</span></span>

<span data-ttu-id="5f9b4-132">En hög tillgänglighet app som körs i två områden innehåller till exempel minst två planer, en för varje region och en app som är associerade med varje plan.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-132">For example, a highly available app running in two regions includes at least two plans, one for each region, and one app associated with each plan.</span></span> <span data-ttu-id="5f9b4-133">I en sådan situation finns sedan alla hello kopior av hello app i en enskild resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-133">In such a situation, all hello copies of hello app are then contained in a single resource group.</span></span> <span data-ttu-id="5f9b4-134">En resursgrupp med flera planer och flera appar blir det enkelt toomanage-, kontroll- och visa hello hälsotillståndet för programmet hello.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-134">Having a resource group with multiple plans and multiple apps makes it easy toomanage, control, and view hello health of hello application.</span></span>

## <a name="create-an-app-service-plan-or-use-existing-one"></a><span data-ttu-id="5f9b4-135">Skapa en apptjänstplan eller använda befintlig</span><span class="sxs-lookup"><span data-stu-id="5f9b4-135">Create an App Service plan or use existing one</span></span>

<span data-ttu-id="5f9b4-136">När du skapar en app, bör du överväga att skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-136">When you create an app, you should consider creating a resource group.</span></span> <span data-ttu-id="5f9b4-137">Hej däremot på om den här appen är en komponent för ett större program, skapa inom hello resursgrupp som har allokerats för större programmet.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-137">On hello other hand, if this app is a component for a larger application, create it within hello resource group that's allocated for that larger application.</span></span>

<span data-ttu-id="5f9b4-138">Om hello appen är ett helt nytt program eller en del av en större, kan du välja toouse en befintlig plan toohost den eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-138">Whether hello app is an altogether new application or part of a larger one, you can choose toouse an existing plan toohost it or create a new one.</span></span> <span data-ttu-id="5f9b4-139">Det här beslutet är mer en fråga av kapacitet och belastning.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-139">This decision is more a question of capacity and expected load.</span></span>

<span data-ttu-id="5f9b4-140">Vi rekommenderar att isolera din app till en ny Apptjänst planerar:</span><span class="sxs-lookup"><span data-stu-id="5f9b4-140">We recommend isolating your app into a new App Service plan when:</span></span>

- <span data-ttu-id="5f9b4-141">Appen är resurskrävande.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-141">App is resource-intensive.</span></span>
- <span data-ttu-id="5f9b4-142">Programmet har olika skalning faktorer från hello andra appar som finns i ett befintligt schema.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-142">App has different scaling factors from hello other apps hosted in an existing plan.</span></span>
- <span data-ttu-id="5f9b4-143">Appen måste resurs i en annan geografisk region.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-143">App needs resource in a different geographical region.</span></span>

<span data-ttu-id="5f9b4-144">Det här sättet du tilldela en ny uppsättning resurser för din app och få större kontroll över dina appar.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-144">This way you can allocate a new set of resources for your app and gain greater control of your apps.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="5f9b4-145">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="5f9b4-145">Create an App Service plan</span></span>

> [!TIP]
> <span data-ttu-id="5f9b4-146">Om du har en Apptjänst-miljö kan du granska hello dokumentationen specifika tooApp miljöer här: [skapa en apptjänstplan i en Apptjänst-miljö](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span><span class="sxs-lookup"><span data-stu-id="5f9b4-146">If you have an App Service Environment, you can review hello documentation specific tooApp Service Environments here: [Create an App Service plan in an App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span></span>

<span data-ttu-id="5f9b4-147">Du kan skapa en tom apptjänstplan från hello navigeringsmiljö för App Service-plan eller som en del av skapa en app.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-147">You can create an empty App Service plan from hello App Service plan browse experience or as part of app creation.</span></span>

<span data-ttu-id="5f9b4-148">I hello [Azure-portalen](https://portal.azure.com), klickar du på **ny** > **webb + mobilt**, och välj sedan **Web App** eller en annan typ för Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-148">In hello [Azure portal](https://portal.azure.com), click **New** > **Web + mobile**, and then select **Web App** or other App Service app kind.</span></span>

![Skapa en app i hello Azure-portalen.][createWebApp]

<span data-ttu-id="5f9b4-150">Du kan sedan välja eller skapa hello App Service-plan för hello nya app.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-150">You can then select or create hello App Service plan for hello new app.</span></span>

 ![Skapa en apptjänstplan.][createASP]

<span data-ttu-id="5f9b4-152">toocreate en apptjänstplan klickar du på **[+] Skapa nya**, typen hello **programtjänstplanen** namn och välj sedan ett lämpligt **plats**.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-152">toocreate an App Service plan, click **[+] Create New**, type hello **App Service plan** name, and then select an appropriate **Location**.</span></span> <span data-ttu-id="5f9b4-153">Klicka på **prisnivå**, och välj sedan ett lämpligt prisnivån för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-153">Click **Pricing tier**, and then select an appropriate pricing tier for hello service.</span></span> <span data-ttu-id="5f9b4-154">Välj **visa alla** tooview priser fler alternativ, exempelvis **lediga** och **delade**.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-154">Select **View all** tooview more pricing options, such as **Free** and **Shared**.</span></span> <span data-ttu-id="5f9b4-155">När du har valt hello prisnivå, klickar du på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-155">After you have selected hello pricing tier, click hello **Select** button.</span></span>

## <a name="move-an-app-tooa-different-app-service-plan"></a><span data-ttu-id="5f9b4-156">Flytta en app tooa annan programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="5f9b4-156">Move an app tooa different App Service plan</span></span>

<span data-ttu-id="5f9b4-157">Du kan flytta en app tooa olika apptjänstplan i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5f9b4-157">You can move an app tooa different App Service plan in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="5f9b4-158">Du kan flytta appar mellan planer så länge hello-planer finns i hello samma resursgrupp och geografiska region.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-158">You can move apps between plans as long as hello plans are in hello same resource group and geographical region.</span></span>

<span data-ttu-id="5f9b4-159">toomove en app tooanother plan:</span><span class="sxs-lookup"><span data-stu-id="5f9b4-159">toomove an app tooanother plan:</span></span>

- <span data-ttu-id="5f9b4-160">Navigera toohello app som du vill toomove.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-160">Navigate toohello app that you want toomove.</span></span>
- <span data-ttu-id="5f9b4-161">I hello **menyn**, leta efter hello **Apptjänstplan** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-161">In hello **Menu**, look for hello **App Service Plan** section.</span></span>
- <span data-ttu-id="5f9b4-162">Välj **ändra programtjänstplanen** toostart hello processen.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-162">Select **Change App Service plan** toostart hello process.</span></span>

<span data-ttu-id="5f9b4-163">**Ändra programtjänstplanen** öppnas hello **programtjänstplanen** väljare.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-163">**Change App Service plan** opens hello **App Service plan** selector.</span></span> <span data-ttu-id="5f9b4-164">Nu kan du välja en befintlig plan toomove den här appen till.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-164">At this point, you can pick an existing plan toomove this app into.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5f9b4-165">hello väljer programtjänstplanen UI filtreras efter hello följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="5f9b4-165">hello select App Service plan UI is filtered by hello following criteria:</span></span>
> - <span data-ttu-id="5f9b4-166">Finns i hello samma resursgrupp</span><span class="sxs-lookup"><span data-stu-id="5f9b4-166">Exists within hello same Resource Group</span></span>
> - <span data-ttu-id="5f9b4-167">Det finns i hello samma geografiska Region</span><span class="sxs-lookup"><span data-stu-id="5f9b4-167">Exists in hello same Geographical Region</span></span>
> - <span data-ttu-id="5f9b4-168">Finns i hello samma Webbutrymmet</span><span class="sxs-lookup"><span data-stu-id="5f9b4-168">Exists within hello same Webspace</span></span>
>
> <span data-ttu-id="5f9b4-169">En Webbutrymmet är en logisk konstruktion i App Service som definierar en gruppering av serverresurser.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-169">A Webspace is a logical construct within App Service which defines a grouping of server resources.</span></span> <span data-ttu-id="5f9b4-170">En geografisk region (till exempel USA, västra) innehåller många Webspaces i ordning tooallocate kunder använder App Service.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-170">A Geographical region (such as West US) contains many Webspaces in order tooallocate customers using App Service.</span></span> <span data-ttu-id="5f9b4-171">Apptjänst resurser är för närvarande inte kan toobe flyttas mellan Webspaces.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-171">Currently, App Service resources aren’t able toobe moved between Webspaces.</span></span>
>

![App Service-plan väljare.][change]

<span data-ttu-id="5f9b4-173">Varje plan har sin egen prisnivån.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-173">Each plan has its own pricing tier.</span></span> <span data-ttu-id="5f9b4-174">Flytta en plats från en kostnadsfri nivå tooa standardnivån kan till exempel alla appar som har tilldelats tooit toouse hello funktioner och resurser av hello standardnivån.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-174">For example, moving a site from a Free tier tooa Standard tier, enables all apps assigned tooit toouse hello features and resources of hello Standard tier.</span></span>

## <a name="clone-an-app-tooa-different-app-service-plan"></a><span data-ttu-id="5f9b4-175">Klona en app tooa annan programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="5f9b4-175">Clone an app tooa different App Service plan</span></span>

<span data-ttu-id="5f9b4-176">Om du vill toomove hello app tooa annan region är ett alternativ app kloning.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-176">If you want toomove hello app tooa different region, one alternative is app cloning.</span></span> <span data-ttu-id="5f9b4-177">Kloningen gör en kopia av din app i en ny eller befintlig programtjänstplan i en region.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-177">Cloning makes a copy of your app in a new or existing App Service plan in any region.</span></span>

<span data-ttu-id="5f9b4-178">Du kan hitta **klona App** i hello **utvecklingsverktyg** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-178">You can find **Clone App** in hello **Development Tools** section of hello menu.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5f9b4-179">Kloningen har vissa begränsningar som du kan läsa om vid [Azure Apptjänst-App kloning med hjälp av Azure portal](../app-service-web/app-service-web-app-cloning-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5f9b4-179">Cloning has some limitations that you can read about at [Azure App Service App cloning using Azure portal](../app-service-web/app-service-web-app-cloning-portal.md).</span></span>

## <a name="scale-an-app-service-plan"></a><span data-ttu-id="5f9b4-180">Skala en programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="5f9b4-180">Scale an App Service plan</span></span>

<span data-ttu-id="5f9b4-181">Det finns tre sätt tooscale en plan:</span><span class="sxs-lookup"><span data-stu-id="5f9b4-181">There are three ways tooscale a plan:</span></span>

- <span data-ttu-id="5f9b4-182">**Ändra hello plan prisnivån**.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-182">**Change hello plan’s pricing tier**.</span></span> <span data-ttu-id="5f9b4-183">En plan i hello grundläggande nivån kan vara konverterade tooStandard och alla appar tilldelad tooit toouse hello funktioner i hello standardnivån.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-183">A plan in hello Basic tier can be converted tooStandard, and all apps assigned tooit toouse hello features of hello Standard tier.</span></span>
- <span data-ttu-id="5f9b4-184">**Ändra storlek på hello plan**.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-184">**Change hello plan’s instance size**.</span></span> <span data-ttu-id="5f9b4-185">Exempelvis kan en plan i hello grundläggande nivån som använder små instanser vara ändrade toouse stora instanser.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-185">As an example, a plan in hello Basic tier that uses small instances can be changed toouse large instances.</span></span> <span data-ttu-id="5f9b4-186">Alla appar som är kopplade med planen kan använda hello ytterligare minne och processorresurser som hello större instans storlek erbjudanden.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-186">All apps that are associated with that plan now can use hello additional memory and CPU resources that hello larger instance size offers.</span></span>
- <span data-ttu-id="5f9b4-187">**Ändra hello plan instansantalet**.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-187">**Change hello plan’s instance count**.</span></span> <span data-ttu-id="5f9b4-188">En Standard-plan som utskalad toothree instanser kan till exempel vara skalade too10 instanser.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-188">For example, a Standard plan that's scaled out toothree instances can be scaled too10 instances.</span></span> <span data-ttu-id="5f9b4-189">En premiumplan kan skaländras ut too20 instanser (ämne tooavailability).</span><span class="sxs-lookup"><span data-stu-id="5f9b4-189">A Premium plan can be scaled out too20 instances (subject tooavailability).</span></span> <span data-ttu-id="5f9b4-190">Alla appar som är kopplade med planen kan använda hello ytterligare minne och processorresurser som hello större instans antal erbjudanden.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-190">All apps that are associated with that plan now can use hello additional memory and CPU resources that hello larger instance count offers.</span></span>

<span data-ttu-id="5f9b4-191">Du kan ändra hello priser nivå och instans storlek genom att klicka på hello **skala upp** objekt under inställningar för hello app eller hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-191">You can change hello pricing tier and instance size by clicking hello **Scale Up** item under settings for either hello app or hello App Service plan.</span></span> <span data-ttu-id="5f9b4-192">Ändringarna tillämpas toohello App Service-plan och påverkar alla appar som den är värd för.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-192">Changes apply toohello App Service plan and affect all apps that it hosts.</span></span>

 ![Ange värden tooscale upp en app.][pricingtier]

## <a name="app-service-plan-cleanup"></a><span data-ttu-id="5f9b4-194">Rensning av App Service-plan</span><span class="sxs-lookup"><span data-stu-id="5f9b4-194">App Service plan cleanup</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5f9b4-195">**Apptjänstplaner** som har inga appar som är associerade toothem fortfarande avgifter eftersom de fortsätta tooreserve hello beräkningskapacitet.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-195">**App Service plans** that have no apps associated toothem still incur charges since they continue tooreserve hello compute capacity.</span></span>

<span data-ttu-id="5f9b4-196">tooavoid oväntat kostnader, när du tar bort hello senaste app finns i en apptjänstplan hello resulterande tom App Service-plan tas också bort.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-196">tooavoid unexpected charges, when hello last app hosted in an App Service plan is deleted, hello resulting empty App Service plan is also deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="5f9b4-197">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="5f9b4-197">Summary</span></span>

<span data-ttu-id="5f9b4-198">Programtjänstplaner representerar en uppsättning funktioner och kapaciteter som du kan dela över dina appar.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-198">App Service plans represent a set of features and capacity that you can share across your apps.</span></span> <span data-ttu-id="5f9b4-199">Apptjänstplaner ger du hello flexibilitet tooallocate specifika appar tooa uppsättning resurser och ytterligare optimera din Azure resursutnyttjande.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-199">App Service plans give you hello flexibility tooallocate specific apps tooa set of resources and further optimize your Azure resource utilization.</span></span> <span data-ttu-id="5f9b4-200">På så sätt kan om du vill toosave pengar i din testmiljö kan du dela en plan över flera appar.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-200">This way, if you want toosave money on your testing environment, you can share a plan across multiple apps.</span></span> <span data-ttu-id="5f9b4-201">Du kan också maximera dataflödet för produktionsmiljön genom att skala över flera regioner och planer.</span><span class="sxs-lookup"><span data-stu-id="5f9b4-201">You can also maximize throughput for your production environment by scaling it across multiple regions and plans.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="5f9b4-202">Nyheter</span><span class="sxs-lookup"><span data-stu-id="5f9b4-202">What's changed</span></span>

- <span data-ttu-id="5f9b4-203">En guide toohello ändring från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="5f9b4-203">For a guide toohello change from Websites tooApp Service, see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
