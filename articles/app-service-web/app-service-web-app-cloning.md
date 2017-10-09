---
title: "aaaWeb App kloning med hjälp av PowerShell"
description: "Lär dig hur tooclone dina Web Apps toonew webbprogram med hjälp av PowerShell."
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
ms.openlocfilehash: b8882370d6db6939f8e4473ccc1414091bdcb8f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a><span data-ttu-id="d0287-103">Azure Apptjänst-App kloning med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d0287-103">Azure App Service App Cloning Using PowerShell</span></span>
<span data-ttu-id="d0287-104">Hello-versionen av Microsoft Azure PowerShell version 1.1.0 ett nytt alternativ har lagts till i tooNew AzureRMWebApp som skulle ge hello användaren hello möjlighet tooclone en befintlig Webbapp tooa nyskapad app i en annan region eller hello samma region.</span><span class="sxs-lookup"><span data-stu-id="d0287-104">With hello release of Microsoft Azure PowerShell version 1.1.0 a new option has been added tooNew-AzureRMWebApp that would give hello user hello ability tooclone an existing Web App tooa newly created app in a different region or in hello same region.</span></span> <span data-ttu-id="d0287-105">Detta gör att kunder toodeploy ett antal appar över olika regioner, snabbt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="d0287-105">This will enable customers toodeploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="d0287-106">Kloning av appen är för närvarande stöds endast för apptjänstplaner för premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="d0287-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="d0287-107">hello nya funktion använder hello samma begränsningar som Web Apps Backup-funktionen finns [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="d0287-107">hello new feature uses hello same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<span data-ttu-id="d0287-108">toolearn om hur du använder Azure Resource Manager baserat Azure PowerShell-cmdlets toomanage Web Apps-Kontrollera [Azure Resource Manager baserat PowerShell-kommandon för Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="d0287-108">toolearn about using Azure Resource Manager based Azure PowerShell cmdlets toomanage your Web Apps check [Azure Resource Manager based PowerShell commands for Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)</span></span>

## <a name="cloning-an-existing-app"></a><span data-ttu-id="d0287-109">Kloning av en befintlig App</span><span class="sxs-lookup"><span data-stu-id="d0287-109">Cloning an existing App</span></span>
<span data-ttu-id="d0287-110">Scenario: En befintlig webbapp i södra centrala USA region hello användaren vill tooclone hello innehållet tooa ny webbapp i norra centrala USA region.</span><span class="sxs-lookup"><span data-stu-id="d0287-110">Scenario: An existing web app in South Central US region, hello user would like tooclone hello contents tooa new web app in North Central US region.</span></span> <span data-ttu-id="d0287-111">Detta kan åstadkommas med hjälp av hello Azure Resource Manager version av hello PowerShell cmdlet toocreate ett nytt webbprogram hello - SourceWebApp alternativet.</span><span class="sxs-lookup"><span data-stu-id="d0287-111">This can be accomplished by using hello Azure Resource Manager version of hello PowerShell cmdlet toocreate a new web app with hello -SourceWebApp option.</span></span>

<span data-ttu-id="d0287-112">Att veta hello resurs gruppnamn som innehåller hello källa webbapp, vi använder hello följande PowerShell-kommandot tooget hello källa webbprogram information (i det här fallet med namnet källa webapp):</span><span class="sxs-lookup"><span data-stu-id="d0287-112">Knowing hello resource group name that contains hello source web app, we can use hello following PowerShell command tooget hello source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="d0287-113">toocreate som en ny App Service-Plan som vi kan använda kommandot New AzureRmAppServicePlan som i följande exempel hello</span><span class="sxs-lookup"><span data-stu-id="d0287-113">toocreate a new App Service Plan, we can use New-AzureRmAppServicePlan command as in hello following example</span></span>

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

<span data-ttu-id="d0287-114">Hello ny AzureRmWebApp kommandot vi kan skapa nytt webbprogram för hello i hello norra centrala USA region och koppla den tooan befintliga premiumnivån App Service-Plan kan dessutom vi använda hello samma resurs gruppen som hello källa webbapp eller definiera en ny resursgrupp , hello följande visar att:</span><span class="sxs-lookup"><span data-stu-id="d0287-114">Using hello New-AzureRmWebApp command, we can create hello new web app in hello North Central US region, and tie it tooan existing premium tier App Service Plan, moreover we can use hello same resource group as hello source web app, or define a new resource group, hello following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

<span data-ttu-id="d0287-115">en befintlig webbapp inklusive alla associerade distributionsplatser hello användaren måste toouse tooclone Hej IncludeSourceWebAppSlots parameter, hello följande PowerShell-kommando visar hello använder parametern med hello ny AzureRmWebApp kommando:</span><span class="sxs-lookup"><span data-stu-id="d0287-115">tooclone an existing web app including all associated deployment slots, hello user will need toouse hello IncludeSourceWebAppSlots parameter, hello following PowerShell command demonstrates hello use of that parameter with hello New-AzureRmWebApp command:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

<span data-ttu-id="d0287-116">tooclone en befintlig webbapp inom hello samma region hello användaren måste en ny resursgrupp och en ny apptjänst planerar i hello toocreate samma region och sedan använda hello följande PowerShell-kommandot tooclone hello webbprogram</span><span class="sxs-lookup"><span data-stu-id="d0287-116">tooclone an existing web app within hello same region, hello user will need toocreate a new resource group and a new app service plan in hello same region, and then using hello following PowerShell command tooclone hello web app</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a><span data-ttu-id="d0287-117">Kloning av en befintlig App tooan Apptjänstmiljö</span><span class="sxs-lookup"><span data-stu-id="d0287-117">Cloning an existing App tooan App Service Environment</span></span>
<span data-ttu-id="d0287-118">Scenario: En befintlig webbapp i södra centrala USA region hello användaren vill tooclone hello innehållet tooa nya web app tooan befintliga App Service miljö (ASE).</span><span class="sxs-lookup"><span data-stu-id="d0287-118">Scenario: An existing web app in South Central US region, hello user would like tooclone hello contents tooa new web app tooan existing App Service Environment (ASE).</span></span>

<span data-ttu-id="d0287-119">Att veta hello resurs gruppnamn som innehåller hello källa webbapp, vi använder hello följande PowerShell-kommandot tooget hello källa webbprogram information (i det här fallet med namnet källa webapp):</span><span class="sxs-lookup"><span data-stu-id="d0287-119">Knowing hello resource group name that contains hello source web app, we can use hello following PowerShell command tooget hello source web app's information (in this case named source-webapp):</span></span>

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

<span data-ttu-id="d0287-120">Att veta hello ASE namn och hello resursgruppens namn som hello ASE tillhör kan hello användaren använda hello ny AzureRmWebApp kommandot toocreate hello ny webbapp i hello befintliga ASE, hello följande visar att:</span><span class="sxs-lookup"><span data-stu-id="d0287-120">Knowing hello ASE's name, and hello resource group name that hello ASE belongs to, hello user can use hello New-AzureRmWebApp command toocreate hello new web app in hello existing ASE, hello following demonstrates that:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

<span data-ttu-id="d0287-121">Hej platsparametern krävs på grund av toolegacy orsak, men hello gäller att skapa en app i en ASE kommer att ignoreras.</span><span class="sxs-lookup"><span data-stu-id="d0287-121">hello Location parameter is required due toolegacy reason, but in hello case of creating an app in an ASE it will be ignored.</span></span> 

## <a name="cloning-an-existing-app-slot"></a><span data-ttu-id="d0287-122">Kloning av en befintlig App-plats</span><span class="sxs-lookup"><span data-stu-id="d0287-122">Cloning an existing App Slot</span></span>
<span data-ttu-id="d0287-123">Scenario: hello användaren vill tooclone en befintlig Webbprogramplats tooeither ett nytt webbprogram eller en ny plats för webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d0287-123">Scenario: hello user would like tooclone an existing Web App Slot tooeither a new Web App or a new Web App slot.</span></span> <span data-ttu-id="d0287-124">hello nytt webbprogram kan vara i hello samma region som hello ursprungliga webbprogrammet plats eller i en annan region.</span><span class="sxs-lookup"><span data-stu-id="d0287-124">hello new Web App can be in hello same region as hello original Web App slot or in a different region.</span></span>

<span data-ttu-id="d0287-125">Att veta hello resurs gruppnamn som innehåller hello källa webbapp, vi använder följande PowerShell-kommando tooget hello källa webbprogramplatss information (i det här fallet med namnet källa webappslot) knutna tooWeb App käll-webapp hello:</span><span class="sxs-lookup"><span data-stu-id="d0287-125">Knowing hello resource group name that contains hello source web app, we can use hello following PowerShell command tooget hello source web app slot's information (in this case named source-webappslot) tied tooWeb App source-webapp:</span></span>

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

<span data-ttu-id="d0287-126">hello följande visar hur du skapar en klon av hello källa web app tooa nytt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="d0287-126">hello following demonstrates creating a clone of hello source web app tooa new web app:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a><span data-ttu-id="d0287-127">Konfigurera Traffic Manager under kloning av en App</span><span class="sxs-lookup"><span data-stu-id="d0287-127">Configuring Traffic Manager while cloning a App</span></span>
<span data-ttu-id="d0287-128">Skapa flera regioner webbprogram och konfigurera Azure Traffic Manager tooroute trafik tooall dessa webbprogram, är en n viktiga scenariot tooinsure som kunders appar är hög tillgänglighet när du klonar en befintlig webbapp som du har hello alternativet tooconnect både webbserver appar tooeither en ny traffic manager-profil eller en befintlig - medveten om att endast Azure Resource Manager-version för Traffic Manager stöds.</span><span class="sxs-lookup"><span data-stu-id="d0287-128">Creating multi-region web apps and configuring Azure Traffic Manager tooroute traffic tooall these web apps, is a n important scenario tooinsure that customers' apps are highly available, when cloning an existing web app you have hello option tooconnect both web apps tooeither a new traffic manager profile or an existing one - note that only Azure Resource Manager version of Traffic Manager is supported.</span></span>

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a><span data-ttu-id="d0287-129">Skapa en ny Traffic Manager-profil under kloning av en App</span><span class="sxs-lookup"><span data-stu-id="d0287-129">Creating a new Traffic Manager profile while cloning a App</span></span>
<span data-ttu-id="d0287-130">Scenario: hello användaren vill tooclone en web app tooanother region, när du konfigurerar en Azure Resource Manager traffic manager-profil som innehåller både webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d0287-130">Scenario: hello user would like tooclone an web app tooanother region, while configuring an Azure Resource Manager traffic manager profile that include both web apps.</span></span> <span data-ttu-id="d0287-131">hello följande visar hur du skapar en klon av hello källa web app tooa nytt webbprogram när du konfigurerar en ny Traffic Manager-profil:</span><span class="sxs-lookup"><span data-stu-id="d0287-131">hello following demonstrates creating a clone of hello source web app tooa new web app while configuring a new Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-tooan-existing-traffic-manager-profile"></a><span data-ttu-id="d0287-132">Lägga till nya klonade webbprogrammet tooan befintlig trafikhanterarprofil</span><span class="sxs-lookup"><span data-stu-id="d0287-132">Adding new cloned Web App tooan existing Traffic Manager profile</span></span>
<span data-ttu-id="d0287-133">Scenario: hello användaren redan har en Azure Resource Manager trafikhanterarprofilen att han vill tooadd både webbappar som slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="d0287-133">Scenario: hello user already have an Azure Resource Manager traffic manager profile that he would like tooadd both web apps as endpoints.</span></span> <span data-ttu-id="d0287-134">toodo så vi måste först tooassemble hello befintlig traffic manager profil-id, måste vi hello prenumerations-id, resursgruppens namn och hello befintliga traffic manager profilnamnet.</span><span class="sxs-lookup"><span data-stu-id="d0287-134">toodo so, we first need tooassemble hello existing traffic manager profile id, we will need hello subscription id, resource group name and hello existing traffic manager profile name.</span></span>

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

<span data-ttu-id="d0287-135">Efter att ha hello traffic Manager-id Visar hello följande att skapa en klon av hello källa web app tooa nytt webbprogram när du lägger till dem tooan befintlig Traffic Manager-profilen:</span><span class="sxs-lookup"><span data-stu-id="d0287-135">After having hello traffic manger id, hello following demonstrates creating a clone of hello source web app tooa new web app while adding them tooan existing Traffic Manager profile:</span></span>

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a><span data-ttu-id="d0287-136">Aktuella begränsningar</span><span class="sxs-lookup"><span data-stu-id="d0287-136">Current Restrictions</span></span>
<span data-ttu-id="d0287-137">Den här funktionen är för närvarande under förhandsgranskning, vi arbetar tooadd nya funktioner över tid, hello följande lista är hello kända begränsningar för hello nuvarande version av appen kloning:</span><span class="sxs-lookup"><span data-stu-id="d0287-137">This feature is currently in preview, we are working tooadd new capabilities over time, hello following list are hello known restrictions on hello current version of app cloning:</span></span>

* <span data-ttu-id="d0287-138">Inställningar för automatisk skalning klonas inte</span><span class="sxs-lookup"><span data-stu-id="d0287-138">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="d0287-139">Schemat för säkerhetskopiering inställningar klonas inte</span><span class="sxs-lookup"><span data-stu-id="d0287-139">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="d0287-140">VNET-inställningarna klonas inte</span><span class="sxs-lookup"><span data-stu-id="d0287-140">VNET settings are not cloned</span></span>
* <span data-ttu-id="d0287-141">App Insights konfigureras inte automatiskt på hello mål för webbprogram</span><span class="sxs-lookup"><span data-stu-id="d0287-141">App Insights are not automatically set up on hello destination web app</span></span>
* <span data-ttu-id="d0287-142">Enkelt autentiseringsinställningarna klonas inte</span><span class="sxs-lookup"><span data-stu-id="d0287-142">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="d0287-143">Kudu-tillägget klonas inte</span><span class="sxs-lookup"><span data-stu-id="d0287-143">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="d0287-144">Tips regler klonas inte</span><span class="sxs-lookup"><span data-stu-id="d0287-144">TiP rules are not cloned</span></span>
* <span data-ttu-id="d0287-145">Databasen innehåll klonas inte</span><span class="sxs-lookup"><span data-stu-id="d0287-145">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="d0287-146">Referenser</span><span class="sxs-lookup"><span data-stu-id="d0287-146">References</span></span>
* [<span data-ttu-id="d0287-147">Azure Resource Manager baserat PowerShell-kommandon för Azure Web App</span><span class="sxs-lookup"><span data-stu-id="d0287-147">Azure Resource Manager based PowerShell commands for Azure Web App</span></span>](app-service-web-app-azure-resource-manager-powershell.md)
* [<span data-ttu-id="d0287-148">Web App kloning med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d0287-148">Web App Cloning using Azure Portal</span></span>](app-service-web-app-cloning-portal.md)
* [<span data-ttu-id="d0287-149">Säkerhetskopiera en webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d0287-149">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="d0287-150">Azure Resource Manager-stöd för Azure Traffic Manager-Preview</span><span class="sxs-lookup"><span data-stu-id="d0287-150">Azure Resource Manager support for Azure Traffic Manager Preview</span></span>](../traffic-manager/traffic-manager-powershell-arm.md)
* [<span data-ttu-id="d0287-151">Introduktion tooApp-miljö</span><span class="sxs-lookup"><span data-stu-id="d0287-151">Introduction tooApp Service Environment</span></span>](app-service-app-service-environment-intro.md)
* [<span data-ttu-id="d0287-152">Använda Azure PowerShell med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d0287-152">Using Azure PowerShell with Azure Resource Manager</span></span>](../powershell-azure-resource-manager.md)

