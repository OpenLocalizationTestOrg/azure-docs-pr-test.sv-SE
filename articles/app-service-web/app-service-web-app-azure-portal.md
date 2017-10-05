---
title: "Referens för navigering Azure-portalen"
description: "Läs olika användarupplevelser för App Service Web mellan management-portalen och Azure Portal"
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
ms.openlocfilehash: d1ef6e87d82df0840e49412154df40cc937b320c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="reference-for-navigating-the-azure-portal"></a><span data-ttu-id="4e137-103">Referens för navigering Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4e137-103">Reference for navigating the Azure portal</span></span>
<span data-ttu-id="4e137-104">Azure Websites kallas [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="4e137-104">Azure Websites are now called [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="4e137-105">Vi uppdaterar alla av vår dokumentation till namnet ändringen och att ge instruktioner åt Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4e137-105">We're updating all of our documentation to reflect this name change and to provide instructions for the Azure Portal.</span></span> <span data-ttu-id="4e137-106">Förrän den här processen är klar, kan du använda det här dokumentet som en vägledning för för att arbeta med Web Apps i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4e137-106">Until that process is done, you can use this document as a guide for working with Web Apps in the Azure portal.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="the-future-of-the-azure-classic-portal"></a><span data-ttu-id="4e137-107">Framtiden för den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4e137-107">The future of the Azure Classic Portal</span></span>
<span data-ttu-id="4e137-108">När du ser företagsanpassning ändringarna på den klassiska Azure-portalen håller som portalanvändare på att ersättas av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4e137-108">While you will notice the branding changes on the Azure Classic Portal, that portal is in the process of being replaced by the Azure Portal.</span></span> <span data-ttu-id="4e137-109">Eftersom den klassiska portalen kommer att överges, rörliga fokus för nyutveckling till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4e137-109">As the classic portal is being phased out, the focus for new development is shifting to the Azure Portal.</span></span> <span data-ttu-id="4e137-110">Alla kommande nya funktioner för Web Apps kommer i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="4e137-110">All upcoming new features for Web Apps will come in the Azure Portal.</span></span> <span data-ttu-id="4e137-111">Börja använda Azure Portal för att dra nytta av de senaste och mest att Web Apps har att erbjuda.</span><span class="sxs-lookup"><span data-stu-id="4e137-111">Start using the Azure Portal to take advantage of the latest and greatest that Web Apps have to offer.</span></span>

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a><span data-ttu-id="4e137-112">Layout skillnader mellan den klassiska Azure-portalen och Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4e137-112">Layout differences between the Azure Classic Portal and Azure Portal</span></span>
<span data-ttu-id="4e137-113">I den klassiska portalen visas alla Azure-tjänster på den vänstra sidan.</span><span class="sxs-lookup"><span data-stu-id="4e137-113">In the classic portal, all the Azure services are listed on the left hand side.</span></span> <span data-ttu-id="4e137-114">Navigering i den klassiska portalen följer trädstrukturer, där du startar från tjänsten och navigera till varje element.</span><span class="sxs-lookup"><span data-stu-id="4e137-114">Navigation in the classic portal follows a tree structure, where you start from the service and navigate into each element.</span></span> <span data-ttu-id="4e137-115">Den här strukturen fungerar bra när du hanterar oberoende komponenter.</span><span class="sxs-lookup"><span data-stu-id="4e137-115">This structure works well when managing independent components.</span></span> <span data-ttu-id="4e137-116">Men program som bygger på Azure är en samling sammankopplade tjänster och den här trädstrukturen är inte idealiskt för att arbeta med samlingar av tjänster.</span><span class="sxs-lookup"><span data-stu-id="4e137-116">However, applications built on Azure are a collection of interconnected services, and this tree structure isn't ideal for working with collections of services.</span></span> 

<span data-ttu-id="4e137-117">Azure portal gör det enkelt att skapa program slutpunkt till slutpunkt med komponenter från flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="4e137-117">The Azure portal makes it easy to build applications end-to-end with components from multiple services.</span></span> <span data-ttu-id="4e137-118">Portalen har ordnats som *resor*.</span><span class="sxs-lookup"><span data-stu-id="4e137-118">The portal is organized as *journeys*.</span></span> <span data-ttu-id="4e137-119">En *resa* är en serie *blad*, som är behållare för de olika komponenterna.</span><span class="sxs-lookup"><span data-stu-id="4e137-119">A *journey* is a series of *blades*, which are containers for the different components.</span></span> <span data-ttu-id="4e137-120">Exempelvis ställa in automatisk skalning för ett webbprogram är en *resa* som tar du flera blad som visas i följande exempel: den **webbplats** bladet (att rubrik inte har ännu har uppdaterats för att använda den nya den här artikeln) den **inställningar** bladet och **skala ut** bladet.</span><span class="sxs-lookup"><span data-stu-id="4e137-120">For example, setting up auto-scaling for a web app is a *journey* which takes you several blades as shown in the following example: the **web-site** blade (that blade title has not yet been updated to use the new terminology), the **Settings** blade, and the **Scale out** blade.</span></span> <span data-ttu-id="4e137-121">I det här exemplet Autoskala ställs in beroende CPU-användning, så det finns också en **CPU-procent** bladet.</span><span class="sxs-lookup"><span data-stu-id="4e137-121">In the example, auto scaling is being set up to depend on CPU usage, so there is also a **CPU Percentage** blade.</span></span> <span data-ttu-id="4e137-122">Komponenterna i den *blad* kallas *delar*, som ser ut som paneler.</span><span class="sxs-lookup"><span data-stu-id="4e137-122">The components within the *blades* are called *parts*, which look like tiles.</span></span> 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a><span data-ttu-id="4e137-123">Navigering exempel: skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="4e137-123">Navigation example: create a web app</span></span>
<span data-ttu-id="4e137-124">Skapa nya webbappar är fortfarande lika enkelt som 1-2-3.</span><span class="sxs-lookup"><span data-stu-id="4e137-124">Creating new web apps is still as easy as 1-2-3.</span></span> <span data-ttu-id="4e137-125">Följande bild visar den klassiska portalen och portal sida vid sida att demonstrera att inte mycket har ändrats i antalet steg som behövs för att komma ett webbprogram och köra.</span><span class="sxs-lookup"><span data-stu-id="4e137-125">The following image shows the classic portal and the portal side-by-side to demonstrate that not much has changed in the number of steps needed to get a web app up and running.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

<span data-ttu-id="4e137-126">Du kan välja från de vanligaste typerna av webbprogram, inklusive Galleri för populära program som WordPress i portalen.</span><span class="sxs-lookup"><span data-stu-id="4e137-126">In the portal you can choose from the most common types of web apps, including popular gallery applications like WordPress.</span></span> <span data-ttu-id="4e137-127">En fullständig lista över tillgängliga program finns i [Azure Marketplace].</span><span class="sxs-lookup"><span data-stu-id="4e137-127">For a full list of available applications, visit the [Azure Marketplace].</span></span>

<span data-ttu-id="4e137-128">När du skapar ett webbprogram kan du ange URL: en App Service-plan och plats på samma sätt som i den klassiska portalen-portalen.</span><span class="sxs-lookup"><span data-stu-id="4e137-128">When you create a web app, you specify URL, App Service plan, and location in the portal just as you do in the classic portal.</span></span> 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

<span data-ttu-id="4e137-129">Dessutom kan portalen du definiera andra vanliga inställningar.</span><span class="sxs-lookup"><span data-stu-id="4e137-129">In addition, the portal lets you define other common settings.</span></span> <span data-ttu-id="4e137-130">Till exempel [resursgrupper](../azure-resource-manager/resource-group-overview.md) gör det enkelt att se och hantera relaterade Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="4e137-130">For example, [resource groups](../azure-resource-manager/resource-group-overview.md) make it simple to see and manage related Azure resources.</span></span> 

## <a name="navigation-example-settings-and-features"></a><span data-ttu-id="4e137-131">Navigering exempel: inställningar och funktioner</span><span class="sxs-lookup"><span data-stu-id="4e137-131">Navigation example: settings and features</span></span>
<span data-ttu-id="4e137-132">Alla inställningar och funktioner är nu logiskt grupperade i ett enda blad som du kan navigera.</span><span class="sxs-lookup"><span data-stu-id="4e137-132">All the settings and features are now logically grouped in a single blade, from which you can navigate.</span></span>

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

<span data-ttu-id="4e137-133">Du kan till exempel skapa anpassade domäner genom att klicka på **anpassade domäner och SSL** i den **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="4e137-133">For example, you can create custom domains by clicking **Custom domains and SSL** in the **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

<span data-ttu-id="4e137-134">Klicka på Konfigurera en övervakningsavisering **begäranden och fel** och sedan **Lägg till avisering**.</span><span class="sxs-lookup"><span data-stu-id="4e137-134">To set up a monitoring alert, click **Requests and errors** and then **Add Alert**.</span></span>

![](./media/app-service-web-app-azure-portal/Monitoring.png)

<span data-ttu-id="4e137-135">Aktivera diagnostik, klicka på **diagnostik loggar** i den **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="4e137-135">To enable diagnostics, click **Diagnostics logs** in the **Settings** blade.</span></span>

![](./media/app-service-web-app-azure-portal/Diagnostics.png)

<span data-ttu-id="4e137-136">Om du vill konfigurera programinställningar, klickar du på **programinställningar** i den **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="4e137-136">To configure application settings, click **Application settings** in the **Settings** blade.</span></span> 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

<span data-ttu-id="4e137-137">Några saker i portalen har bytt namn eller grupperas på olika sätt för att göra det enklare att hitta dem än varumärke.</span><span class="sxs-lookup"><span data-stu-id="4e137-137">Other than the brand name, a few things in the portal have been renamed or grouped differently to make it easier to find them.</span></span> <span data-ttu-id="4e137-138">Till exempel nedan visas en skärmbild av sidan motsvarande för app-inställningar (**konfigurera**) i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="4e137-138">For example, below is a screenshot of the corresponding page for app settings (**Configure**) in the classic portal.</span></span>

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a><span data-ttu-id="4e137-139">Fler resurser</span><span class="sxs-lookup"><span data-stu-id="4e137-139">More Resources</span></span>
[Azure Portal]: https://portal.azure.com
<span data-ttu-id="4e137-140">[Azure Marketplace]: /marketplace/</span><span class="sxs-lookup"><span data-stu-id="4e137-140">[Azure Marketplace]: /marketplace/</span></span>

> [!NOTE]
> <span data-ttu-id="4e137-141">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="4e137-141">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="4e137-142">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="4e137-142">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="4e137-143">Nyheter</span><span class="sxs-lookup"><span data-stu-id="4e137-143">What's changed</span></span>
* <span data-ttu-id="4e137-144">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="4e137-144">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

