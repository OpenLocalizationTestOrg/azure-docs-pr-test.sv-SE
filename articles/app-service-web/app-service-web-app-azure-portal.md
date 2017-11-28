---
title: "aaaReference för navigering hello Azure-portalen"
description: "Läs hello olika användarupplevelser för App Service Web mellan hello-hanteringsportalen och hello Azure-portalen"
services: app-service
documentationcenter: 
author: jaime-espinosa
manager: erikre
editor: jimbe
ms.assetid: 0cc6a3cc-bd89-4a96-9177-d25f6fb737bb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: jaime-espinosa
ms.openlocfilehash: dcf7c1fc17f9a0c31005ad0f2fd53723d2966058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reference-for-navigating-hello-azure-portal"></a><span data-ttu-id="93119-103">Referens för navigering hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="93119-103">Reference for navigating hello Azure portal</span></span>
<span data-ttu-id="93119-104">Azure Websites kallas [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="93119-104">Azure Websites are now called [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="93119-105">Vi uppdaterar alla vår dokumentation tooreflect det här namnet för ändrings- och tooprovide instruktioner för hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93119-105">We're updating all of our documentation tooreflect this name change and tooprovide instructions for hello Azure Portal.</span></span> <span data-ttu-id="93119-106">Förrän den här processen är klar kan du använda det här dokumentet som en vägledning för att arbeta med Webbappar i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93119-106">Until that process is done, you can use this document as a guide for working with Web Apps in hello Azure portal.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="hello-future-of-hello-azure-classic-portal"></a><span data-ttu-id="93119-107">hello framtida hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="93119-107">hello future of hello Azure Classic Portal</span></span>
<span data-ttu-id="93119-108">När du ser hello anpassningsändringar på hello klassiska Azure-portalen är den portalen i hello processen att ersättas med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93119-108">While you will notice hello branding changes on hello Azure Classic Portal, that portal is in hello process of being replaced by hello Azure Portal.</span></span> <span data-ttu-id="93119-109">Eftersom hello klassiska portalen kommer att överges, hello fokus för nyutveckling rörliga toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93119-109">As hello classic portal is being phased out, hello focus for new development is shifting toohello Azure Portal.</span></span> <span data-ttu-id="93119-110">Alla kommande nya funktioner för Web Apps kommer i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93119-110">All upcoming new features for Web Apps will come in hello Azure Portal.</span></span> <span data-ttu-id="93119-111">Börja använda hello Azure Portal tootake nytta av hello senaste och bästa att Web Apps har toooffer.</span><span class="sxs-lookup"><span data-stu-id="93119-111">Start using hello Azure Portal tootake advantage of hello latest and greatest that Web Apps have toooffer.</span></span>

## <a name="layout-differences-between-hello-azure-classic-portal-and-azure-portal"></a><span data-ttu-id="93119-112">Layout skillnader mellan hello klassiska Azure-portalen och Azure Portal</span><span class="sxs-lookup"><span data-stu-id="93119-112">Layout differences between hello Azure Classic Portal and Azure Portal</span></span>
<span data-ttu-id="93119-113">I klassiska portalen hello alla hello Azure tjänsterna hello vänster.</span><span class="sxs-lookup"><span data-stu-id="93119-113">In hello classic portal, all hello Azure services are listed on hello left hand side.</span></span> <span data-ttu-id="93119-114">Navigering i hello klassiska portalen följer trädstrukturer, där du startar från hello-tjänsten och navigera till varje element.</span><span class="sxs-lookup"><span data-stu-id="93119-114">Navigation in hello classic portal follows a tree structure, where you start from hello service and navigate into each element.</span></span> <span data-ttu-id="93119-115">Den här strukturen fungerar bra när du hanterar oberoende komponenter.</span><span class="sxs-lookup"><span data-stu-id="93119-115">This structure works well when managing independent components.</span></span> <span data-ttu-id="93119-116">Men program som bygger på Azure är en samling sammankopplade tjänster och den här trädstrukturen är inte idealiskt för att arbeta med samlingar av tjänster.</span><span class="sxs-lookup"><span data-stu-id="93119-116">However, applications built on Azure are a collection of interconnected services, and this tree structure isn't ideal for working with collections of services.</span></span> 

<span data-ttu-id="93119-117">hello Azure-portalen gör det enkelt toobuild program slutpunkt till slutpunkt med komponenter från flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="93119-117">hello Azure portal makes it easy toobuild applications end-to-end with components from multiple services.</span></span> <span data-ttu-id="93119-118">hello-portalen har ordnats som *resor*.</span><span class="sxs-lookup"><span data-stu-id="93119-118">hello portal is organized as *journeys*.</span></span> <span data-ttu-id="93119-119">En *resa* är en serie *blad*, som är behållare för hello olika komponenter.</span><span class="sxs-lookup"><span data-stu-id="93119-119">A *journey* is a series of *blades*, which are containers for hello different components.</span></span> <span data-ttu-id="93119-120">Exempelvis ställa in automatisk skalning för ett webbprogram är en *resa* som tar du flera blad som visas i följande exempel hello: hello **webbplats** bladet (att rubrik ännu inte har uppdaterats toouse hello nya terminologi), hello **inställningar** bladet och hello **skala ut** bladet.</span><span class="sxs-lookup"><span data-stu-id="93119-120">For example, setting up auto-scaling for a web app is a *journey* which takes you several blades as shown in hello following example: hello **web-site** blade (that blade title has not yet been updated toouse hello new terminology), hello **Settings** blade, and hello **Scale out** blade.</span></span> <span data-ttu-id="93119-121">I exemplet hello Autoskala ställs in toodepend på CPU-användning, så det finns också en **CPU-procent** bladet.</span><span class="sxs-lookup"><span data-stu-id="93119-121">In hello example, auto scaling is being set up toodepend on CPU usage, so there is also a **CPU Percentage** blade.</span></span> <span data-ttu-id="93119-122">Hej komponenter i hello *blad* kallas *delar*, som ser ut som paneler.</span><span class="sxs-lookup"><span data-stu-id="93119-122">hello components within hello *blades* are called *parts*, which look like tiles.</span></span> 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a><span data-ttu-id="93119-123">Navigering exempel: skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="93119-123">Navigation example: create a web app</span></span>
<span data-ttu-id="93119-124">Skapa nya webbappar är fortfarande lika enkelt som 1-2-3.</span><span class="sxs-lookup"><span data-stu-id="93119-124">Creating new web apps is still as easy as 1-2-3.</span></span> <span data-ttu-id="93119-125">hello följande bild visar hello klassiska portalen och hello portal sida-vid-sida toodemonstrate som inte mycket har ändrats i hello antalet steg som behövs tooget en webbapp in och körs.</span><span class="sxs-lookup"><span data-stu-id="93119-125">hello following image shows hello classic portal and hello portal side-by-side toodemonstrate that not much has changed in hello number of steps needed tooget a web app up and running.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

<span data-ttu-id="93119-126">Du kan välja från hello de vanligaste typerna av webbprogram, inklusive Galleri för populära program som WordPress hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="93119-126">In hello portal you can choose from hello most common types of web apps, including popular gallery applications like WordPress.</span></span> <span data-ttu-id="93119-127">En fullständig lista över tillgängliga program finns hello [Azure Marketplace].</span><span class="sxs-lookup"><span data-stu-id="93119-127">For a full list of available applications, visit hello [Azure Marketplace].</span></span>

<span data-ttu-id="93119-128">När du skapar ett webbprogram kan du ange URL: en App Service-plan och plats på samma sätt som i hello klassiska portalen hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="93119-128">When you create a web app, you specify URL, App Service plan, and location in hello portal just as you do in hello classic portal.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

<span data-ttu-id="93119-129">Dessutom kan hello-portalen du definiera andra vanliga inställningar.</span><span class="sxs-lookup"><span data-stu-id="93119-129">In addition, hello portal lets you define other common settings.</span></span> <span data-ttu-id="93119-130">Till exempel [resursgrupper](../azure-resource-manager/resource-group-overview.md) gör det enkelt toosee och hantera relaterade Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="93119-130">For example, [resource groups](../azure-resource-manager/resource-group-overview.md) make it simple toosee and manage related Azure resources.</span></span> 

## <a name="navigation-example-settings-and-features"></a><span data-ttu-id="93119-131">Navigering exempel: inställningar och funktioner</span><span class="sxs-lookup"><span data-stu-id="93119-131">Navigation example: settings and features</span></span>
<span data-ttu-id="93119-132">Alla hello inställningar och funktioner nu är logiskt grupperade i ett enda blad som du kan navigera.</span><span class="sxs-lookup"><span data-stu-id="93119-132">All hello settings and features are now logically grouped in a single blade, from which you can navigate.</span></span>

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

<span data-ttu-id="93119-133">Du kan till exempel skapa anpassade domäner genom att klicka på **anpassade domäner och SSL** i hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="93119-133">For example, you can create custom domains by clicking **Custom domains and SSL** in hello **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

<span data-ttu-id="93119-134">tooset in en övervakningsavisering klickar du på **begäranden och fel** och sedan **Lägg till avisering**.</span><span class="sxs-lookup"><span data-stu-id="93119-134">tooset up a monitoring alert, click **Requests and errors** and then **Add Alert**.</span></span>

![](./media/app-service-web-app-azure-portal/Monitoring.png)

<span data-ttu-id="93119-135">tooenable diagnostics, klickar du på **diagnostik loggar** i hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="93119-135">tooenable diagnostics, click **Diagnostics logs** in hello **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

<span data-ttu-id="93119-136">tooconfigure programinställningar, klickar du på **programinställningar** i hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="93119-136">tooconfigure application settings, click **Application settings** in hello **Settings** blade.</span></span> 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

<span data-ttu-id="93119-137">Än hello varumärke, några saker i hello portal ha ändrats eller grupperade annorlunda toomake den enklare toofind dem.</span><span class="sxs-lookup"><span data-stu-id="93119-137">Other than hello brand name, a few things in hello portal have been renamed or grouped differently toomake it easier toofind them.</span></span> <span data-ttu-id="93119-138">Till exempel nedan visas en skärmbild av sidan hello motsvarande app-inställningar (**konfigurera**) i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="93119-138">For example, below is a screenshot of hello corresponding page for app settings (**Configure**) in hello classic portal.</span></span>

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a><span data-ttu-id="93119-139">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="93119-139">More Resources</span></span>
[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

> [!NOTE]
> <span data-ttu-id="93119-141">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="93119-141">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="93119-142">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="93119-142">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="93119-143">Nyheter</span><span class="sxs-lookup"><span data-stu-id="93119-143">What's changed</span></span>
* <span data-ttu-id="93119-144">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="93119-144">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

