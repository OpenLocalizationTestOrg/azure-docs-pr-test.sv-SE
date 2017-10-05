---
title: "Web App kloning med hjälp av PowerShell"
description: "Lär dig mer om att klona ditt webbprogram till nya webbprogram med hjälp av PowerShell."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: d47d5a2f7d2462525bf37718a234e222b4f64e6d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a><span data-ttu-id="dda02-103">Azure Apptjänst-App kloning med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="dda02-103">Azure App Service App Cloning Using PowerShell</span></span>
<span data-ttu-id="dda02-104">Med versionen av Microsoft Azure PowerShell version 1.1.0 har ett nytt alternativ lagts till ny AzureRMWebApp som skulle ge användaren möjlighet att klona en befintlig Webbapp till en nyligen skapade appen i en annan region eller i samma region.</span><span class="sxs-lookup"><span data-stu-id="dda02-104">With the release of Microsoft Azure PowerShell version 1.1.0 a new option has been added to New-AzureRMWebApp that would give the user the ability to clone an existing Web App to a newly created app in a different region or in the same region.</span></span> <span data-ttu-id="dda02-105">Detta gör att kunder att distribuera flera appar över olika regioner, snabbt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="dda02-105">This will enable customers to deploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="dda02-106">Kloning av appen är för närvarande stöds endast för apptjänstplaner för premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="dda02-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="dda02-107">Ny funktion använder samma begränsningar som Web Apps Backup-funktionen, se [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="dda02-107">The new feature uses the same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="dda02-108">Läs om hur du använder Azure Resource Manager baserade Azure PowerShell-cmdlets för att hantera ditt webbprogram Kontrollera [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="dda02-108">To learn about using Azure Resource Manager based Azure PowerShell cmdlets to manage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="cloning-an-existing-app"></a><span data-ttu-id="dda02-109">Kloning av en befintlig App</span><span class="sxs-lookup"><span data-stu-id="dda02-109">Cloning an existing App</span></span>
<span data-ttu-id="dda02-110">Scenario: En befintlig webbapp i södra centrala USA region användaren vill klona innehållet till en ny webbapp i norra centrala USA region.</span><span class="sxs-lookup"><span data-stu-id="dda02-110">Scenario: An existing web app in South Central US region, the user would like to clone the contents to a new web app in North Central US region.</span></span> <span data-ttu-id="dda02-111">Detta kan åstadkommas med hjälp av Azure Resource Manager-version av PowerShell-cmdleten för att skapa ett nytt webbprogram med alternativet - SourceWebApp.</span><span class="sxs-lookup"><span data-stu-id="dda02-111">This can be accomplished by using the Azure Resource Manager version of the PowerShell cmdlet to create a new web app with the -SourceWebApp option.</span></span>

<span data-ttu-id="dda02-112">Vi veta resursgruppens namn som innehåller webbappen källa, kan du använda följande PowerShell-kommando för att hämta information om käll-webbappen (i det här fallet med namnet källa webapp):</span><span class="sxs-lookup"><span data-stu-id="dda02-112">Knowing the resource group name that contains the source web app, we can use the following PowerShell command to get the source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="dda02-113">Vi kan använda New AzureRmAppServicePlan kommandot som i följande exempel för att skapa en ny App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="dda02-113">To create a new App Service Plan, we can use New-AzureRmAppServicePlan command as in the following example</span></span>

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

<span data-ttu-id="dda02-114">Med kommandot New AzureRmWebApp kan vi skapa den nya webbappen i området norra centrala USA och koppla den till en befintlig premiumnivån App Service-Plan, vi kan dessutom använda samma resursgrupp som webbappen källa eller definiera en ny resursgrupp , följande visar att:</span><span class="sxs-lookup"><span data-stu-id="dda02-114">Using the New-AzureRmWebApp command, we can create the new web app in the North Central US region, and tie it to an existing premium tier App Service Plan, moreover we can use the same resource group as the source web app, or define a new resource group, the following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

<span data-ttu-id="dda02-115">Att klona en befintlig webbapp, inklusive alla associerade distributionen fack användaren måste använda parametern IncludeSourceWebAppSlots, följande PowerShell-kommando visar hur du använder av denna parameter med kommandot Nytt AzureRmWebApp:</span><span class="sxs-lookup"><span data-stu-id="dda02-115">To clone an existing web app including all associated deployment slots, the user will need to use the IncludeSourceWebAppSlots parameter, the following PowerShell command demonstrates the use of that parameter with the New-AzureRmWebApp command:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

<span data-ttu-id="dda02-116">Om du vill klona en befintlig webbapp inom samma region användaren kommer att behöva skapa en ny resursgrupp och en ny apptjänst planera i samma region och sedan använda följande PowerShell-kommando för att klona webbappen</span><span class="sxs-lookup"><span data-stu-id="dda02-116">To clone an existing web app within the same region, the user will need to create a new resource group and a new app service plan in the same region, and then using the following PowerShell command to clone the web app</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a><span data-ttu-id="dda02-117">Kloning av en befintlig App som en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="dda02-117">Cloning an existing App to an App Service Environment</span></span>
<span data-ttu-id="dda02-118">Scenario: En befintlig webbapp i södra centrala USA region användaren vill klona innehållet till en ny webbapp till en befintlig App Service miljö (ASE).</span><span class="sxs-lookup"><span data-stu-id="dda02-118">Scenario: An existing web app in South Central US region, the user would like to clone the contents to a new web app to an existing App Service Environment (ASE).</span></span>

<span data-ttu-id="dda02-119">Vi veta resursgruppens namn som innehåller webbappen källa, kan du använda följande PowerShell-kommando för att hämta information om käll-webbappen (i det här fallet med namnet källa webapp):</span><span class="sxs-lookup"><span data-stu-id="dda02-119">Knowing the resource group name that contains the source web app, we can use the following PowerShell command to get the source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="dda02-120">Känna till de ASE namnet och resursgruppens namn som ASE tillhör, användaren kan använda kommandot New AzureRmWebApp för att skapa den nya webbappen i befintliga ASE, följande visar att:</span><span class="sxs-lookup"><span data-stu-id="dda02-120">Knowing the ASE's name, and the resource group name that the ASE belongs to, the user can use the New-AzureRmWebApp command to create the new web app in the existing ASE, the following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

<span data-ttu-id="dda02-121">Parametern plats krävs av äldre skäl, men när det gäller att skapa en app i en ASE kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="dda02-121">The Location parameter is required due to legacy reason, but in the case of creating an app in an ASE it will be ignored.</span></span> 

## <a name="cloning-an-existing-app-slot"></a><span data-ttu-id="dda02-122">Kloning av en befintlig App-plats</span><span class="sxs-lookup"><span data-stu-id="dda02-122">Cloning an existing App Slot</span></span>
<span data-ttu-id="dda02-123">Scenario: Användaren vill klona en befintlig Webbprogramplats till antingen ett nytt webbprogram eller en ny plats för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="dda02-123">Scenario: The user would like to clone an existing Web App Slot to either a new Web App or a new Web App slot.</span></span> <span data-ttu-id="dda02-124">Den nya Webbappen kan vara i samma region som den ursprungliga platsen Web App eller i en annan region.</span><span class="sxs-lookup"><span data-stu-id="dda02-124">The new Web App can be in the same region as the original Web App slot or in a different region.</span></span>

<span data-ttu-id="dda02-125">Veta resursgruppens namn som innehåller webbappen källa använda vi följande PowerShell-kommando för att få källa web app platsens information (i det här fallet med namnet källa webappslot) knutna till webbprogrammet käll-webapp:</span><span class="sxs-lookup"><span data-stu-id="dda02-125">Knowing the resource group name that contains the source web app, we can use the following PowerShell command to get the source web app slot's information (in this case named source-webappslot) tied to Web App source-webapp:</span></span>

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

<span data-ttu-id="dda02-126">Följande visar hur du skapar en klon av webbprogram för källan till ett nytt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="dda02-126">The following demonstrates creating a clone of the source web app to a new web app:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a><span data-ttu-id="dda02-127">Konfigurera Traffic Manager under kloning av en App</span><span class="sxs-lookup"><span data-stu-id="dda02-127">Configuring Traffic Manager while cloning a App</span></span>
<span data-ttu-id="dda02-128">Skapa flera regioner webbprogram och konfigurera Azure Traffic Manager för att dirigera trafik till dessa webbprogram, är ett n viktiga scenario till att säkerställa att kunders appar är hög tillgänglighet, när du klonar en befintlig webbapp har du möjlighet att ansluta båda webbprogram till en ny traffic manager-profil eller en befintlig - medveten om att endast Azure Resource Manager-version för Traffic Manager stöds.</span><span class="sxs-lookup"><span data-stu-id="dda02-128">Creating multi-region web apps and configuring Azure Traffic Manager to route traffic to all these web apps, is a n important scenario to insure that customers' apps are highly available, when cloning an existing web app you have the option to connect both web apps to either a new traffic manager profile or an existing one - note that only Azure Resource Manager version of Traffic Manager is supported.</span></span>

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a><span data-ttu-id="dda02-129">Skapa en ny Traffic Manager-profil under kloning av en App</span><span class="sxs-lookup"><span data-stu-id="dda02-129">Creating a new Traffic Manager profile while cloning a App</span></span>
<span data-ttu-id="dda02-130">Scenario: Användaren vill klona ett webbprogram till en annan region, när du konfigurerar en Azure Resource Manager traffic manager-profil som innehåller både webbprogram.</span><span class="sxs-lookup"><span data-stu-id="dda02-130">Scenario: The user would like to clone an web app to another region, while configuring an Azure Resource Manager traffic manager profile that include both web apps.</span></span> <span data-ttu-id="dda02-131">Följande visar hur du skapar en klon av webbprogram för källan till en ny webbapp när du konfigurerar en ny Traffic Manager-profil:</span><span class="sxs-lookup"><span data-stu-id="dda02-131">The following demonstrates creating a clone of the source web app to a new web app while configuring a new Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a><span data-ttu-id="dda02-132">Att lägga till nya klonas webbprogram i en befintlig Traffic Manager-profil</span><span class="sxs-lookup"><span data-stu-id="dda02-132">Adding new cloned Web App to an existing Traffic Manager profile</span></span>
<span data-ttu-id="dda02-133">Scenario: Användaren redan har en Azure Resource Manager traffic manager-profil som han vill lägga till både webbprogram som slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="dda02-133">Scenario: The user already have an Azure Resource Manager traffic manager profile that he would like to add both web apps as endpoints.</span></span> <span data-ttu-id="dda02-134">Om du vill göra det måste vi måste först att montera den befintliga traffic manager profil-id, behöver vi prenumerations-id, resursgruppens namn och det befintliga profilnamnet för traffic manager.</span><span class="sxs-lookup"><span data-stu-id="dda02-134">To do so, we first need to assemble the existing traffic manager profile id, we will need the subscription id, resource group name and the existing traffic manager profile name.</span></span>

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

<span data-ttu-id="dda02-135">När du har med traffic Manager-id, visar följande skapa en klon av webbprogram för källan till en ny webbapp när du lägger till dem i en befintlig Traffic Manager-profil:</span><span class="sxs-lookup"><span data-stu-id="dda02-135">After having the traffic manger id, the following demonstrates creating a clone of the source web app to a new web app while adding them to an existing Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a><span data-ttu-id="dda02-136">Aktuella begränsningar</span><span class="sxs-lookup"><span data-stu-id="dda02-136">Current Restrictions</span></span>
<span data-ttu-id="dda02-137">Den här funktionen är för närvarande under förhandsgranskning, vi arbetar för att lägga till nya funktioner över tiden, i följande lista finns kända begränsningar för den aktuella versionen av appen kloning:</span><span class="sxs-lookup"><span data-stu-id="dda02-137">This feature is currently in preview, we are working to add new capabilities over time, the following list are the known restrictions on the current version of app cloning:</span></span>

* <span data-ttu-id="dda02-138">Inställningar för automatisk skalning klonas inte</span><span class="sxs-lookup"><span data-stu-id="dda02-138">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="dda02-139">Schemat för säkerhetskopiering inställningar klonas inte</span><span class="sxs-lookup"><span data-stu-id="dda02-139">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="dda02-140">VNET-inställningarna klonas inte</span><span class="sxs-lookup"><span data-stu-id="dda02-140">VNET settings are not cloned</span></span>
* <span data-ttu-id="dda02-141">App Insights konfigureras inte automatiskt på mål-webbprogram</span><span class="sxs-lookup"><span data-stu-id="dda02-141">App Insights are not automatically set up on the destination web app</span></span>
* <span data-ttu-id="dda02-142">Enkelt autentiseringsinställningarna klonas inte</span><span class="sxs-lookup"><span data-stu-id="dda02-142">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="dda02-143">Kudu-tillägget klonas inte</span><span class="sxs-lookup"><span data-stu-id="dda02-143">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="dda02-144">Tips regler klonas inte</span><span class="sxs-lookup"><span data-stu-id="dda02-144">TiP rules are not cloned</span></span>
* <span data-ttu-id="dda02-145">Databasen innehåll klonas inte</span><span class="sxs-lookup"><span data-stu-id="dda02-145">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="dda02-146">Referenser</span><span class="sxs-lookup"><span data-stu-id="dda02-146">References</span></span>
* [<span data-ttu-id="dda02-147">Azure Resource Manager baserat PowerShell-kommandon för Azure Web App</span><span class="sxs-lookup"><span data-stu-id="dda02-147">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="dda02-148">Web App kloning med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dda02-148">Web App Cloning using Azure Portal</span></span>](app-service-web-app-cloning-portal.md)
* [<span data-ttu-id="dda02-149">Säkerhetskopiera en webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="dda02-149">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="dda02-150">Azure Resource Manager-stöd för Azure Traffic Manager-Preview</span><span class="sxs-lookup"><span data-stu-id="dda02-150">Azure Resource Manager support for Azure Traffic Manager Preview</span></span>](../traffic-manager/traffic-manager-powershell-arm.md)
* [<span data-ttu-id="dda02-151">Introduktion till App Service-miljöer</span><span class="sxs-lookup"><span data-stu-id="dda02-151">Introduction to App Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="dda02-152">Använda Azure PowerShell med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="dda02-152">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

