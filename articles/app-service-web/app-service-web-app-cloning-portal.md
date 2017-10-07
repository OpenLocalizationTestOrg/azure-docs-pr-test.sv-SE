---
title: "aaaWeb App kloning med hjälp av Azure Portal"
description: "Lär dig hur tooclone din Web Apps toonew webbprogram med hjälp av Azure Portal."
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
ms.openlocfilehash: 605c4879f34d568e9981c34109f9496731c9ed18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-app-cloning-using-azure-portal"></a><span data-ttu-id="1e756-103">Azure Apptjänst-App kloning med hjälp av Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1e756-103">Azure App Service App Cloning Using Azure Portal</span></span>
<span data-ttu-id="1e756-104">hello funktionen i kloning [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) kan du enkelt klona befintliga appar tooa nyskapad webbprogrammet i en annan region eller hello samma region.</span><span class="sxs-lookup"><span data-stu-id="1e756-104">hello cloning feature in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) lets you easily clone existing web apps tooa newly created app in a different region or in hello same region.</span></span> <span data-ttu-id="1e756-105">Detta gör att kunder toodeploy ett antal appar över olika regioner, snabbt och enkelt.</span><span class="sxs-lookup"><span data-stu-id="1e756-105">This will enable customers toodeploy a number of apps across different regions quickly and easily.</span></span>

<span data-ttu-id="1e756-106">Kloning av appen är för närvarande stöds endast för apptjänstplaner för premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="1e756-106">App cloning is currently only supported for premium tier app service plans.</span></span> <span data-ttu-id="1e756-107">hello nya funktion använder hello samma begränsningar som Web Apps Backup-funktionen finns [säkerhetskopiera en webbapp i Azure App Service](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="1e756-107">hello new feature uses hello same limitations as Web Apps Backup feature, see [Back up a web app in Azure App Service](web-sites-backup.md).</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a><span data-ttu-id="1e756-108">Kloning av en befintlig App</span><span class="sxs-lookup"><span data-stu-id="1e756-108">Cloning an existing App</span></span>
<span data-ttu-id="1e756-109">hello webbprogram måste köras i hello **Premium** läge för toocreate en klon för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1e756-109">hello web app must be running in hello **Premium** mode in order for you toocreate a clone for hello web app.</span></span>

1. <span data-ttu-id="1e756-110">I hello [Azure Portal](https://portal.azure.com/), öppna ditt webbprogram bladet.</span><span class="sxs-lookup"><span data-stu-id="1e756-110">In hello [Azure Portal](https://portal.azure.com/), open your web app's blade.</span></span>
2. <span data-ttu-id="1e756-111">Klicka på **verktyg**.</span><span class="sxs-lookup"><span data-stu-id="1e756-111">Click **Tools**.</span></span> <span data-ttu-id="1e756-112">Sedan hello **verktyg** bladet, klickar du på **klona App**.</span><span class="sxs-lookup"><span data-stu-id="1e756-112">Then, in hello **Tools** blade, click **Clone App**.</span></span>
   
    ![][1]
   
   > [!NOTE]
   > <span data-ttu-id="1e756-113">Om hello webbapp inte är redan i hello **Premium** läge, visas ett meddelande som anger hello stöds lägen för kloning av app.</span><span class="sxs-lookup"><span data-stu-id="1e756-113">If hello web app is not already in hello **Premium** mode, you will receive a message indicating hello supported modes for app cloning.</span></span> <span data-ttu-id="1e756-114">Nu har du hello alternativet tooselect **uppgradera**.</span><span class="sxs-lookup"><span data-stu-id="1e756-114">At this point, you have hello option tooselect **Upgrade**.</span></span>
   > 
   > 
3. <span data-ttu-id="1e756-115">I hello **klona App** bladet anger du ett namn av hello nytt webbprogram, resursgruppen och App Service-Plan.</span><span class="sxs-lookup"><span data-stu-id="1e756-115">In hello **Clone App** blade provide a name of hello new web app, Resource Group, and App Service Plan.</span></span> <span data-ttu-id="1e756-116">Även hello användaren kommer att kunna toochoose om tooclone ett antal inställningar för datakälla web app eller inte.</span><span class="sxs-lookup"><span data-stu-id="1e756-116">Also hello user will be able toochoose whether tooclone a number of source web app settings or not.</span></span>
   
    ![][2]
4. <span data-ttu-id="1e756-117">När du klickar på **skapa** hello plattform kommer att börja arbeta om hur du skapar en klon av hello käll-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1e756-117">After clicking **create** hello platform will start working on creating a clone of hello source web app.</span></span>

## <a name="cloning-an-existing-app-tooan-app-service-environment"></a><span data-ttu-id="1e756-118">Kloning av en befintlig App tooan Apptjänstmiljö</span><span class="sxs-lookup"><span data-stu-id="1e756-118">Cloning an existing App tooan App Service Environment</span></span>
<span data-ttu-id="1e756-119">I hello **klona App** bladet hello kunden måste hello alternativet toochoose en programpool i en befintlig Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="1e756-119">In hello **Clone App** blade hello customer will have hello option toochoose an app pool in an existing App Service Environment.</span></span>

## <a name="current-restrictions"></a><span data-ttu-id="1e756-120">Aktuella begränsningar</span><span class="sxs-lookup"><span data-stu-id="1e756-120">Current Restrictions</span></span>
<span data-ttu-id="1e756-121">Den här funktionen är för närvarande under förhandsgranskning, vi arbetar tooadd nya funktioner över tid, hello följande lista är hello hello stöder kända begränsningar av app kloning i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="1e756-121">This feature is currently in preview, we are working tooadd new capabilities over time, hello following list are hello known restrictions on hello current support of app cloning in Azure Portal:</span></span>

* <span data-ttu-id="1e756-122">Klonas inte Azure Traffic Manager-inställningar</span><span class="sxs-lookup"><span data-stu-id="1e756-122">Azure Traffic Manager settings are not cloned</span></span>
* <span data-ttu-id="1e756-123">Inställningar för automatisk skalning klonas inte</span><span class="sxs-lookup"><span data-stu-id="1e756-123">Auto scale settings are not cloned</span></span>
* <span data-ttu-id="1e756-124">Schemat för säkerhetskopiering inställningar klonas inte</span><span class="sxs-lookup"><span data-stu-id="1e756-124">Backup schedule settings are not cloned</span></span>
* <span data-ttu-id="1e756-125">VNET-inställningarna klonas inte</span><span class="sxs-lookup"><span data-stu-id="1e756-125">VNET settings are not cloned</span></span>
* <span data-ttu-id="1e756-126">App Insights konfigureras inte automatiskt på hello mål för webbprogram</span><span class="sxs-lookup"><span data-stu-id="1e756-126">App Insights are not automatically set up on hello destination web app</span></span>
* <span data-ttu-id="1e756-127">Enkelt autentiseringsinställningarna klonas inte</span><span class="sxs-lookup"><span data-stu-id="1e756-127">Easy Auth settings are not cloned</span></span>
* <span data-ttu-id="1e756-128">Kudu-tillägget klonas inte</span><span class="sxs-lookup"><span data-stu-id="1e756-128">Kudu Extension are not cloned</span></span>
* <span data-ttu-id="1e756-129">Tips regler klonas inte</span><span class="sxs-lookup"><span data-stu-id="1e756-129">TiP rules are not cloned</span></span>
* <span data-ttu-id="1e756-130">Databasen innehåll klonas inte</span><span class="sxs-lookup"><span data-stu-id="1e756-130">Database content are not cloned</span></span>

### <a name="references"></a><span data-ttu-id="1e756-131">Referenser</span><span class="sxs-lookup"><span data-stu-id="1e756-131">References</span></span>
* [<span data-ttu-id="1e756-132">Web App kloning med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="1e756-132">Web App Cloning using PowerShell</span></span>](app-service-web-app-cloning.md)
* [<span data-ttu-id="1e756-133">Säkerhetskopiera en webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="1e756-133">Back up a web app in Azure App Service</span></span>](web-sites-backup.md)
* [<span data-ttu-id="1e756-134">Hur tooCreate en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="1e756-134">How tooCreate an App Service Environment</span></span>](app-service-web-how-to-create-an-app-service-environment.md)
* [<span data-ttu-id="1e756-135">Skapa en webbapp i en App Service Environment</span><span class="sxs-lookup"><span data-stu-id="1e756-135">Create a web app in an App Service Environment</span></span>](app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [<span data-ttu-id="1e756-136">Introduktion tooApp-miljö</span><span class="sxs-lookup"><span data-stu-id="1e756-136">Introduction tooApp Service Environment</span></span>](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png
