---
title: "Azure App Service-planer djupgående översikt | Microsoft Docs"
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
ms.openlocfilehash: f97be571d104e3cc1c6ee732886fa7133ba0dc83
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-app-service-plans-in-depth-overview"></a><span data-ttu-id="3768a-104">Detaljerad översikt över Azure App Service-avtal</span><span class="sxs-lookup"><span data-stu-id="3768a-104">Azure App Service plans in-depth overview</span></span>

<span data-ttu-id="3768a-105">Programtjänstplaner representerar mängd fysiska resurser som används som värd för dina appar.</span><span class="sxs-lookup"><span data-stu-id="3768a-105">App Service plans represent the collection of physical resources used to host your apps.</span></span>

<span data-ttu-id="3768a-106">App Service-planer definierar följande:</span><span class="sxs-lookup"><span data-stu-id="3768a-106">App Service plans define:</span></span>

- <span data-ttu-id="3768a-107">Region (västra USA, östra USA osv.)</span><span class="sxs-lookup"><span data-stu-id="3768a-107">Region (West US, East US, etc.)</span></span>
- <span data-ttu-id="3768a-108">Skalningsantalet (en, två, tre instanser, etc.)</span><span class="sxs-lookup"><span data-stu-id="3768a-108">Scale count (one, two, three instances, etc.)</span></span>
- <span data-ttu-id="3768a-109">Storlek (Small, Medium, Large)</span><span class="sxs-lookup"><span data-stu-id="3768a-109">Instance size (Small, Medium, Large)</span></span>
- <span data-ttu-id="3768a-110">SKU (Kostnadsfri, Delad, Basic, Standard, Premium)</span><span class="sxs-lookup"><span data-stu-id="3768a-110">SKU (Free, Shared, Basic, Standard, Premium)</span></span>

<span data-ttu-id="3768a-111">Web Apps, Mobilappar, API Apps, funktionen appar (eller funktioner) i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) alla kör i en apptjänstplan.</span><span class="sxs-lookup"><span data-stu-id="3768a-111">Web Apps, Mobile Apps, API Apps, Function Apps (or Functions), in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) all run in an App Service plan.</span></span>  <span data-ttu-id="3768a-112">Appar i samma prenumeration och region kan dela en App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="3768a-112">Apps in the same subscription, and region can share an App Service plan.</span></span> 

<span data-ttu-id="3768a-113">Alla program som har tilldelats en **programtjänstplanen** delar resurser som definieras av den.</span><span class="sxs-lookup"><span data-stu-id="3768a-113">All applications assigned to an **App Service plan** share the resources defined by it.</span></span> <span data-ttu-id="3768a-114">Den här delning sparar pengar när värd för flera appar i en enda App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="3768a-114">This sharing saves money when hosting multiple apps in a single App Service plan.</span></span>

<span data-ttu-id="3768a-115">Din **App Service-plan** kan skalas från SKU:erna **Kostnadsfri** och **Delad** till SKU:erna **Bas**, **Standard** och **Premium** så att du får åtkomst till fler resurser och funktioner.</span><span class="sxs-lookup"><span data-stu-id="3768a-115">Your **App Service plan** can scale from **Free** and **Shared** SKUs to **Basic**, **Standard**, and **Premium** SKUs giving you access to more resources and features along the way.</span></span>

<span data-ttu-id="3768a-116">Om din App Service-plan anges till **grundläggande** SKU eller senare, och du kan styra den **storlek** och skala antalet på de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="3768a-116">If your App Service plan is set to **Basic** SKU or higher, then you can control the **size** and scale count of the VMs.</span></span>

<span data-ttu-id="3768a-117">Om din plan har konfigurerats för att använda två ”liten” instanser i standard-tjänstnivå, till exempel köra alla appar som är kopplade med planen på båda instanser.</span><span class="sxs-lookup"><span data-stu-id="3768a-117">For example, if your plan is configured to use two "small" instances in the standard service tier, all apps that are associated with that plan run on both instances.</span></span> <span data-ttu-id="3768a-118">Appar har också åtkomst till standard-nivån servicefunktioner.</span><span class="sxs-lookup"><span data-stu-id="3768a-118">Apps also have access to the standard service tier features.</span></span> <span data-ttu-id="3768a-119">Instanser av programtjänstplanen som kör appar är helt hanterad och hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="3768a-119">Plan instances on which apps are running are fully managed and highly available.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3768a-120">App Service-planens **SKU** och **Skala** avgör kostnaden och inte antalet appar som finns i den.</span><span class="sxs-lookup"><span data-stu-id="3768a-120">The **SKU** and **Scale** of the App Service plan determines the cost and not the number of apps hosted in it.</span></span>

<span data-ttu-id="3768a-121">Den här artikeln innehåller viktiga egenskaper, till exempel nivå och skala för en apptjänstplan och hur de komma till användning vid hantering av dina appar.</span><span class="sxs-lookup"><span data-stu-id="3768a-121">This article explores the key characteristics, such as tier and scale, of an App Service plan and how they come into play while managing your apps.</span></span>

## <a name="apps-and-app-service-plans"></a><span data-ttu-id="3768a-122">Appar och App Service-planer</span><span class="sxs-lookup"><span data-stu-id="3768a-122">Apps and App Service plans</span></span>

<span data-ttu-id="3768a-123">En app i App Service kan associeras med endast en App Service-plan vid en given tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="3768a-123">An app in App Service can be associated with only one App Service plan at any given time.</span></span>

<span data-ttu-id="3768a-124">Både appar och planer finns i en **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="3768a-124">Both apps and plans are contained in a **resource group**.</span></span> <span data-ttu-id="3768a-125">En resursgrupp fungerar som livscykel gräns för varje resurs som ligger inom det.</span><span class="sxs-lookup"><span data-stu-id="3768a-125">A resource group serves as the lifecycle boundary for every resource that's within it.</span></span> <span data-ttu-id="3768a-126">Du kan använda resursgrupper för att hantera alla delar av ett program tillsammans.</span><span class="sxs-lookup"><span data-stu-id="3768a-126">You can use resource groups to manage all the pieces of an application together.</span></span>

<span data-ttu-id="3768a-127">Eftersom en enskild resursgrupp kan ha flera programtjänstplaner, kan du allokera olika appar till olika fysiska resurser.</span><span class="sxs-lookup"><span data-stu-id="3768a-127">Because a single resource group can have multiple App Service plans, you can allocate different apps to different physical resources.</span></span>

<span data-ttu-id="3768a-128">Du kan till exempel separata resurser mellan dev, test- och miljöer.</span><span class="sxs-lookup"><span data-stu-id="3768a-128">For example, you can separate resources among dev, test, and production environments.</span></span> <span data-ttu-id="3768a-129">Med separata miljöer för produktion och utveckling och testning kan du isolera resurser.</span><span class="sxs-lookup"><span data-stu-id="3768a-129">Having separate environments for production and dev/test lets you isolate resources.</span></span> <span data-ttu-id="3768a-130">På så sätt kan konkurrerar läsa in testar mot en ny version av dina appar inte för samma resurser som dina appar för produktion som hanterar verkliga kunder.</span><span class="sxs-lookup"><span data-stu-id="3768a-130">In this way, load testing against a new version of your apps does not compete for the same resources as your production apps, which are serving real customers.</span></span>

<span data-ttu-id="3768a-131">När du har flera planer i en enskild resursgrupp kan du också definiera ett program som omfattar geografiska regioner.</span><span class="sxs-lookup"><span data-stu-id="3768a-131">When you have multiple plans in a single resource group, you can also define an application that spans geographical regions.</span></span>

<span data-ttu-id="3768a-132">En hög tillgänglighet app som körs i två områden innehåller till exempel minst två planer, en för varje region och en app som är associerade med varje plan.</span><span class="sxs-lookup"><span data-stu-id="3768a-132">For example, a highly available app running in two regions includes at least two plans, one for each region, and one app associated with each plan.</span></span> <span data-ttu-id="3768a-133">I en sådan situation finns sedan alla kopior av appen i en enskild resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3768a-133">In such a situation, all the copies of the app are then contained in a single resource group.</span></span> <span data-ttu-id="3768a-134">Med en resursgrupp med flera planer och flera appar gör det enkelt att hantera, styra och visa hälsotillståndet för programmet.</span><span class="sxs-lookup"><span data-stu-id="3768a-134">Having a resource group with multiple plans and multiple apps makes it easy to manage, control, and view the health of the application.</span></span>

## <a name="create-an-app-service-plan-or-use-existing-one"></a><span data-ttu-id="3768a-135">Skapa en apptjänstplan eller använda befintlig</span><span class="sxs-lookup"><span data-stu-id="3768a-135">Create an App Service plan or use existing one</span></span>

<span data-ttu-id="3768a-136">När du skapar en app, bör du överväga att skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3768a-136">When you create an app, you should consider creating a resource group.</span></span> <span data-ttu-id="3768a-137">Å andra sidan, om den här appen är en komponent för ett större program kan skapa den inom den resursgrupp som har allokerats för större programmet.</span><span class="sxs-lookup"><span data-stu-id="3768a-137">On the other hand, if this app is a component for a larger application, create it within the resource group that's allocated for that larger application.</span></span>

<span data-ttu-id="3768a-138">Om appen är ett helt nytt program eller en del av en större kan välja du att använda en befintlig plan för att vara värd för den eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="3768a-138">Whether the app is an altogether new application or part of a larger one, you can choose to use an existing plan to host it or create a new one.</span></span> <span data-ttu-id="3768a-139">Det här beslutet är mer en fråga av kapacitet och belastning.</span><span class="sxs-lookup"><span data-stu-id="3768a-139">This decision is more a question of capacity and expected load.</span></span>

<span data-ttu-id="3768a-140">Vi rekommenderar att isolera din app till en ny Apptjänst planerar:</span><span class="sxs-lookup"><span data-stu-id="3768a-140">We recommend isolating your app into a new App Service plan when:</span></span>

- <span data-ttu-id="3768a-141">Appen är resurskrävande.</span><span class="sxs-lookup"><span data-stu-id="3768a-141">App is resource-intensive.</span></span>
- <span data-ttu-id="3768a-142">Programmet har olika skalning faktorer från andra appar som finns i ett befintligt schema.</span><span class="sxs-lookup"><span data-stu-id="3768a-142">App has different scaling factors from the other apps hosted in an existing plan.</span></span>
- <span data-ttu-id="3768a-143">Appen måste resurs i en annan geografisk region.</span><span class="sxs-lookup"><span data-stu-id="3768a-143">App needs resource in a different geographical region.</span></span>

<span data-ttu-id="3768a-144">Det här sättet du tilldela en ny uppsättning resurser för din app och få större kontroll över dina appar.</span><span class="sxs-lookup"><span data-stu-id="3768a-144">This way you can allocate a new set of resources for your app and gain greater control of your apps.</span></span>

## <a name="create-an-app-service-plan"></a><span data-ttu-id="3768a-145">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="3768a-145">Create an App Service plan</span></span>

> [!TIP]
> <span data-ttu-id="3768a-146">Om du har en Apptjänst-miljö kan du granska dokumentationen för Apptjänstmiljöer här: [skapa en apptjänstplan i en Apptjänst-miljö](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span><span class="sxs-lookup"><span data-stu-id="3768a-146">If you have an App Service Environment, you can review the documentation specific to App Service Environments here: [Create an App Service plan in an App Service Environment](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)</span></span>

<span data-ttu-id="3768a-147">Du kan skapa en tom App Service-plan från navigeringsmiljö för App Service-plan eller som en del av skapa en app.</span><span class="sxs-lookup"><span data-stu-id="3768a-147">You can create an empty App Service plan from the App Service plan browse experience or as part of app creation.</span></span>

<span data-ttu-id="3768a-148">I den [Azure-portalen](https://portal.azure.com), klickar du på **ny** > **webb + mobilt**, och välj sedan **Web App** eller en annan typ för Apptjänst-app.</span><span class="sxs-lookup"><span data-stu-id="3768a-148">In the [Azure portal](https://portal.azure.com), click **New** > **Web + mobile**, and then select **Web App** or other App Service app kind.</span></span>

![Skapa en app i Azure-portalen.][createWebApp]

<span data-ttu-id="3768a-150">Du kan sedan välja eller skapa en apptjänstplan för den nya appen.</span><span class="sxs-lookup"><span data-stu-id="3768a-150">You can then select or create the App Service plan for the new app.</span></span>

 ![Skapa en apptjänstplan.][createASP]

<span data-ttu-id="3768a-152">Klicka för att skapa en apptjänstplan **[+] Skapa nya**, typen av **programtjänstplanen** namn och välj sedan ett lämpligt **plats**.</span><span class="sxs-lookup"><span data-stu-id="3768a-152">To create an App Service plan, click **[+] Create New**, type the **App Service plan** name, and then select an appropriate **Location**.</span></span> <span data-ttu-id="3768a-153">Klicka på **prisnivå**, och välj sedan ett lämpligt prisnivån för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3768a-153">Click **Pricing tier**, and then select an appropriate pricing tier for the service.</span></span> <span data-ttu-id="3768a-154">Välj **visa alla** att visa priser fler alternativ, exempelvis **lediga** och **delade**.</span><span class="sxs-lookup"><span data-stu-id="3768a-154">Select **View all** to view more pricing options, such as **Free** and **Shared**.</span></span> <span data-ttu-id="3768a-155">När du har valt prisnivå, klickar du på den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="3768a-155">After you have selected the pricing tier, click the **Select** button.</span></span>

## <a name="move-an-app-to-a-different-app-service-plan"></a><span data-ttu-id="3768a-156">Flytta en app till en annan programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="3768a-156">Move an app to a different App Service plan</span></span>

<span data-ttu-id="3768a-157">Du kan flytta en app till en annan programtjänstplan i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3768a-157">You can move an app to a different App Service plan in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3768a-158">Du kan flytta appar mellan planer så länge planerna finns i samma resursgrupp och geografisk region.</span><span class="sxs-lookup"><span data-stu-id="3768a-158">You can move apps between plans as long as the plans are in the same resource group and geographical region.</span></span>

<span data-ttu-id="3768a-159">Flytta en app till en annan plan:</span><span class="sxs-lookup"><span data-stu-id="3768a-159">To move an app to another plan:</span></span>

- <span data-ttu-id="3768a-160">Navigera till den app som du vill flytta.</span><span class="sxs-lookup"><span data-stu-id="3768a-160">Navigate to the app that you want to move.</span></span>
- <span data-ttu-id="3768a-161">I den **menyn**, leta efter den **Apptjänstplan** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3768a-161">In the **Menu**, look for the **App Service Plan** section.</span></span>
- <span data-ttu-id="3768a-162">Välj **ändra programtjänstplanen** att starta processen.</span><span class="sxs-lookup"><span data-stu-id="3768a-162">Select **Change App Service plan** to start the process.</span></span>

<span data-ttu-id="3768a-163">**Ändra programtjänstplanen** öppnar den **programtjänstplanen** väljare.</span><span class="sxs-lookup"><span data-stu-id="3768a-163">**Change App Service plan** opens the **App Service plan** selector.</span></span> <span data-ttu-id="3768a-164">Nu kan du välja en befintlig plan för att flytta den här appen till.</span><span class="sxs-lookup"><span data-stu-id="3768a-164">At this point, you can pick an existing plan to move this app into.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3768a-165">Välj apptjänstplan UI filtreras efter följande kriterier:</span><span class="sxs-lookup"><span data-stu-id="3768a-165">The select App Service plan UI is filtered by the following criteria:</span></span>
> - <span data-ttu-id="3768a-166">Finns i samma resursgrupp</span><span class="sxs-lookup"><span data-stu-id="3768a-166">Exists within the same Resource Group</span></span>
> - <span data-ttu-id="3768a-167">Det finns i samma geografiska Region</span><span class="sxs-lookup"><span data-stu-id="3768a-167">Exists in the same Geographical Region</span></span>
> - <span data-ttu-id="3768a-168">Det finns inom samma Webbutrymmet</span><span class="sxs-lookup"><span data-stu-id="3768a-168">Exists within the same Webspace</span></span>
>
> <span data-ttu-id="3768a-169">En Webbutrymmet är en logisk konstruktion i App Service som definierar en gruppering av serverresurser.</span><span class="sxs-lookup"><span data-stu-id="3768a-169">A Webspace is a logical construct within App Service which defines a grouping of server resources.</span></span> <span data-ttu-id="3768a-170">En geografisk region (till exempel USA, västra) innehåller många Webspaces för att allokera kunder som använder App Service.</span><span class="sxs-lookup"><span data-stu-id="3768a-170">A Geographical region (such as West US) contains many Webspaces in order to allocate customers using App Service.</span></span> <span data-ttu-id="3768a-171">Apptjänst resurser är för närvarande inte kan flyttas mellan Webspaces.</span><span class="sxs-lookup"><span data-stu-id="3768a-171">Currently, App Service resources aren’t able to be moved between Webspaces.</span></span>
>

![App Service-plan väljare.][change]

<span data-ttu-id="3768a-173">Varje plan har sin egen prisnivån.</span><span class="sxs-lookup"><span data-stu-id="3768a-173">Each plan has its own pricing tier.</span></span> <span data-ttu-id="3768a-174">Till exempel flytta en plats från en kostnadsfria nivån till ett skikt som Standard kan alla appar som har tilldelats till den använda de funktioner och resurser i standardnivån.</span><span class="sxs-lookup"><span data-stu-id="3768a-174">For example, moving a site from a Free tier to a Standard tier, enables all apps assigned to it to use the features and resources of the Standard tier.</span></span>

## <a name="clone-an-app-to-a-different-app-service-plan"></a><span data-ttu-id="3768a-175">Klona en app på en annan programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="3768a-175">Clone an app to a different App Service plan</span></span>

<span data-ttu-id="3768a-176">Om du vill flytta appen till en annan region är ett alternativ app kloning.</span><span class="sxs-lookup"><span data-stu-id="3768a-176">If you want to move the app to a different region, one alternative is app cloning.</span></span> <span data-ttu-id="3768a-177">Kloningen gör en kopia av din app i en ny eller befintlig programtjänstplan i en region.</span><span class="sxs-lookup"><span data-stu-id="3768a-177">Cloning makes a copy of your app in a new or existing App Service plan in any region.</span></span>

<span data-ttu-id="3768a-178">Du kan hitta **klona App** i den **utvecklingsverktyg** på menyn.</span><span class="sxs-lookup"><span data-stu-id="3768a-178">You can find **Clone App** in the **Development Tools** section of the menu.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3768a-179">Kloningen har vissa begränsningar som du kan läsa om vid [Azure Apptjänst-App kloning med hjälp av Azure portal](../app-service-web/app-service-web-app-cloning-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3768a-179">Cloning has some limitations that you can read about at [Azure App Service App cloning using Azure portal](../app-service-web/app-service-web-app-cloning-portal.md).</span></span>

## <a name="scale-an-app-service-plan"></a><span data-ttu-id="3768a-180">Skala en programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="3768a-180">Scale an App Service plan</span></span>

<span data-ttu-id="3768a-181">Det finns tre sätt att skala en plan:</span><span class="sxs-lookup"><span data-stu-id="3768a-181">There are three ways to scale a plan:</span></span>

- <span data-ttu-id="3768a-182">**Ändra planen prisnivån**.</span><span class="sxs-lookup"><span data-stu-id="3768a-182">**Change the plan’s pricing tier**.</span></span> <span data-ttu-id="3768a-183">En plan i den grundläggande nivån kan konverteras till Standard- och alla appar som har tilldelats för att använda funktioner i standardnivån.</span><span class="sxs-lookup"><span data-stu-id="3768a-183">A plan in the Basic tier can be converted to Standard, and all apps assigned to it to use the features of the Standard tier.</span></span>
- <span data-ttu-id="3768a-184">**Ändra instansstorleken för planens**.</span><span class="sxs-lookup"><span data-stu-id="3768a-184">**Change the plan’s instance size**.</span></span> <span data-ttu-id="3768a-185">Exempelvis kan en plan i den grundläggande nivån som använder små instanser ändras om du vill använda stora instanser.</span><span class="sxs-lookup"><span data-stu-id="3768a-185">As an example, a plan in the Basic tier that uses small instances can be changed to use large instances.</span></span> <span data-ttu-id="3768a-186">Alla appar som är kopplade med planen kan använda ytterligare minne och processorresurser som större instansstorleken erbjuder.</span><span class="sxs-lookup"><span data-stu-id="3768a-186">All apps that are associated with that plan now can use the additional memory and CPU resources that the larger instance size offers.</span></span>
- <span data-ttu-id="3768a-187">**Ändra planens instansantalet**.</span><span class="sxs-lookup"><span data-stu-id="3768a-187">**Change the plan’s instance count**.</span></span> <span data-ttu-id="3768a-188">Till exempel kan en standardplan skalas ut till tre instanser skalas till 10 instanser.</span><span class="sxs-lookup"><span data-stu-id="3768a-188">For example, a Standard plan that's scaled out to three instances can be scaled to 10 instances.</span></span> <span data-ttu-id="3768a-189">En premiumplan kan skaländras ut till 20 instanser (beroende på tillgänglighet).</span><span class="sxs-lookup"><span data-stu-id="3768a-189">A Premium plan can be scaled out to 20 instances (subject to availability).</span></span> <span data-ttu-id="3768a-190">Alla appar som är kopplade med planen kan använda ytterligare minne och processorresurser som erbjuder större instansantalet.</span><span class="sxs-lookup"><span data-stu-id="3768a-190">All apps that are associated with that plan now can use the additional memory and CPU resources that the larger instance count offers.</span></span>

<span data-ttu-id="3768a-191">Du kan ändra prisnivå nivå och instans storlek genom att klicka på den **skala upp** objekt under inställningar för appen eller App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="3768a-191">You can change the pricing tier and instance size by clicking the **Scale Up** item under settings for either the app or the App Service plan.</span></span> <span data-ttu-id="3768a-192">Ändringarna tillämpas på App Service-plan och påverkar alla appar som den är värd för.</span><span class="sxs-lookup"><span data-stu-id="3768a-192">Changes apply to the App Service plan and affect all apps that it hosts.</span></span>

 ![Ange värden för att skala upp en app.][pricingtier]

## <a name="app-service-plan-cleanup"></a><span data-ttu-id="3768a-194">Rensning av App Service-plan</span><span class="sxs-lookup"><span data-stu-id="3768a-194">App Service plan cleanup</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3768a-195">**Apptjänstplaner** som har inga appar som är kopplade till dem fortfarande avgifter eftersom de fortsätter att reservera beräkningskapacitet.</span><span class="sxs-lookup"><span data-stu-id="3768a-195">**App Service plans** that have no apps associated to them still incur charges since they continue to reserve the compute capacity.</span></span>

<span data-ttu-id="3768a-196">Om du vill undvika oväntade kostnader, senaste appen finns i en apptjänstplan tas bort, raderas även resulterande tom App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="3768a-196">To avoid unexpected charges, when the last app hosted in an App Service plan is deleted, the resulting empty App Service plan is also deleted.</span></span>

## <a name="summary"></a><span data-ttu-id="3768a-197">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3768a-197">Summary</span></span>

<span data-ttu-id="3768a-198">Programtjänstplaner representerar en uppsättning funktioner och kapaciteter som du kan dela över dina appar.</span><span class="sxs-lookup"><span data-stu-id="3768a-198">App Service plans represent a set of features and capacity that you can share across your apps.</span></span> <span data-ttu-id="3768a-199">Apptjänstplaner ger dig flexibiliteten att allokera specifika appar till en uppsättning resurser och ytterligare optimera din Azure resursutnyttjande.</span><span class="sxs-lookup"><span data-stu-id="3768a-199">App Service plans give you the flexibility to allocate specific apps to a set of resources and further optimize your Azure resource utilization.</span></span> <span data-ttu-id="3768a-200">På så sätt kan om du vill spara pengar i din testmiljö kan du dela en plan över flera appar.</span><span class="sxs-lookup"><span data-stu-id="3768a-200">This way, if you want to save money on your testing environment, you can share a plan across multiple apps.</span></span> <span data-ttu-id="3768a-201">Du kan också maximera dataflödet för produktionsmiljön genom att skala över flera regioner och planer.</span><span class="sxs-lookup"><span data-stu-id="3768a-201">You can also maximize throughput for your production environment by scaling it across multiple regions and plans.</span></span>

## <a name="whats-changed"></a><span data-ttu-id="3768a-202">Nyheter</span><span class="sxs-lookup"><span data-stu-id="3768a-202">What's changed</span></span>

- <span data-ttu-id="3768a-203">En guide till övergången från webbplatser till App Service finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="3768a-203">For a guide to the change from Websites to App Service, see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
