---
title: "Web App kloning med hjälp av Azure Portal"
description: "Lär dig mer om att klona ditt webbprogram till nya webbprogram med hjälp av Azure Portal."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: 20b0ae4e-67e8-4bae-9d74-8a24dc445cce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2016
ms.author: aelnably
ms.openlocfilehash: 9ebfa91c7972ab3c264032ead8376c23c1197a4b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a><span data-ttu-id="793d9-103">Azure Apptjänst-App kloning med hjälp av Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="793d9-103">Azure App Service App Cloning Using Azure Portal</span></span>
<span data-ttu-id="793d9-104">Funktionen kloning i [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) kan du enkelt klona befintliga webbappar till en nyligen skapade appen i en annan region eller i samma region.</span><span class="sxs-lookup"><span data-stu-id="793d9-104">The cloning feature in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) lets you easily clone existing web apps to a newly created app in a different region or in the same region.</span></span> <span data-ttu-id="793d9-105">Detta gör att kunder att distribuera flera appar över olika regioner, snabbt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="793d9-105">This will enable customers to deploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="793d9-106">Kloning av appen är för närvarande stöds endast för apptjänstplaner för premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="793d9-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="793d9-107">Ny funktion använder samma begränsningar som Web Apps Backup-funktionen, se [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="793d9-107">The new feature uses the same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a><span data-ttu-id="793d9-108">Kloning av en befintlig App</span><span class="sxs-lookup"><span data-stu-id="793d9-108">Cloning an existing App</span></span>
<span data-ttu-id="793d9-109">Webbprogrammet måste köras den **Premium** läge för att du kan skapa en klon för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="793d9-109">The web app must be running in the **Premium** mode in order for you to create a clone for the web app.</span></span>

1. <span data-ttu-id="793d9-110">I den [Azure Portal](https://portal.azure.com/), öppna ditt webbprogram bladet.</span><span class="sxs-lookup"><span data-stu-id="793d9-110">In the [Azure Portal](https://portal.azure.com/), open your web app's blade.</span></span>
2. <span data-ttu-id="793d9-111">Klicka på **verktyg**.</span><span class="sxs-lookup"><span data-stu-id="793d9-111">Click **Tools**.</span></span> <span data-ttu-id="793d9-112">I den **verktyg** bladet, klickar du på **klona App**.</span><span class="sxs-lookup"><span data-stu-id="793d9-112">Then, in the **Tools** blade, click **Clone App**.</span></span>
   
    ![][1]
   
   > [!NOTE]
   > <span data-ttu-id="793d9-113">Om webbappen inte är redan i den **Premium** läge, visas ett meddelande som anger stöds lägen för kloning av app.</span><span class="sxs-lookup"><span data-stu-id="793d9-113">If the web app is not already in the **Premium** mode, you will receive a message indicating the supported modes for app cloning.</span></span> <span data-ttu-id="793d9-114">Nu har du möjlighet att välja **uppgradera**.</span><span class="sxs-lookup"><span data-stu-id="793d9-114">At this point, you have the option to select **Upgrade**.</span></span>
   > 
   > 
3. <span data-ttu-id="793d9-115">I den **klona App** bladet ange ett namn på ny webbapp, resursgruppen och App Service-Plan.</span><span class="sxs-lookup"><span data-stu-id="793d9-115">In the **Clone App** blade provide a name of the new web app, Resource Group, and App Service Plan.</span></span> <span data-ttu-id="793d9-116">Användaren kommer även att kunna välja om du vill klona ett antal inställningar för datakälla web app eller inte.</span><span class="sxs-lookup"><span data-stu-id="793d9-116">Also the user will be able to choose whether to clone a number of source web app settings or not.</span></span>
   
    ![][2]
4. <span data-ttu-id="793d9-117">När du klickar på **skapa** plattformen börjar skapa en klon av webbprogram källa.</span><span class="sxs-lookup"><span data-stu-id="793d9-117">After clicking **create** the platform will start working on creating a clone of the source web app.</span></span>

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a><span data-ttu-id="793d9-118">Kloning av en befintlig App som en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="793d9-118">Cloning an existing App to an App Service Environment</span></span>
<span data-ttu-id="793d9-119">I den **klona App** bladet kunden har möjlighet att välja en programpool i en befintlig Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="793d9-119">In the **Clone App** blade the customer will have the option to choose an app pool in an existing App Service Environment.</span></span>

## <a name="current-restrictions"></a><span data-ttu-id="793d9-120">Aktuella begränsningar</span><span class="sxs-lookup"><span data-stu-id="793d9-120">Current Restrictions</span></span>
<span data-ttu-id="793d9-121">Den här funktionen är för närvarande under förhandsgranskning, vi arbetar för att lägga till nya funktioner över tiden, i följande lista finns kända begränsningar för det aktuella stödet för kloning av app i Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="793d9-121">This feature is currently in preview, we are working to add new capabilities over time, the following list are the known restrictions on the current support of app cloning in Azure Portal:</span></span>

* <span data-ttu-id="793d9-122">Klonas inte Azure Traffic Manager-inställningar</span><span class="sxs-lookup"><span data-stu-id="793d9-122">Azure Traffic Manager settings are not cloned</span></span>
* <span data-ttu-id="793d9-123">Inställningar för automatisk skalning klonas inte</span><span class="sxs-lookup"><span data-stu-id="793d9-123">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="793d9-124">Schemat för säkerhetskopiering inställningar klonas inte</span><span class="sxs-lookup"><span data-stu-id="793d9-124">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="793d9-125">VNET-inställningarna klonas inte</span><span class="sxs-lookup"><span data-stu-id="793d9-125">VNET settings are not cloned</span></span>
* <span data-ttu-id="793d9-126">App Insights konfigureras inte automatiskt på mål-webbprogram</span><span class="sxs-lookup"><span data-stu-id="793d9-126">App Insights are not automatically set up on the destination web app</span></span>
* <span data-ttu-id="793d9-127">Enkelt autentiseringsinställningarna klonas inte</span><span class="sxs-lookup"><span data-stu-id="793d9-127">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="793d9-128">Kudu-tillägget klonas inte</span><span class="sxs-lookup"><span data-stu-id="793d9-128">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="793d9-129">Tips regler klonas inte</span><span class="sxs-lookup"><span data-stu-id="793d9-129">TiP rules are not cloned</span></span>
* <span data-ttu-id="793d9-130">Databasen innehåll klonas inte</span><span class="sxs-lookup"><span data-stu-id="793d9-130">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="793d9-131">Referenser</span><span class="sxs-lookup"><span data-stu-id="793d9-131">References</span></span>
* [<span data-ttu-id="793d9-132">Web App kloning med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="793d9-132">Web App Cloning using PowerShell</span></span>](app-service-web-app-cloning.md)
* [<span data-ttu-id="793d9-133">Säkerhetskopiera en webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="793d9-133">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="793d9-134">Skapa en App Service Environment</span><span class="sxs-lookup"><span data-stu-id="793d9-134">How to Create an App Service Environment</span></span>](app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="793d9-135">Skapa en webbapp i en App Service Environment</span><span class="sxs-lookup"><span data-stu-id="793d9-135">Create a web app in an App Service Environment</span></span>](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [<span data-ttu-id="793d9-136">Introduktion till App Service-miljöer</span><span class="sxs-lookup"><span data-stu-id="793d9-136">Introduction to App Service Environment</span></span>](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
