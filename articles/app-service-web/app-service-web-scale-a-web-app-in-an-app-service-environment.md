---
title: "Så här skalar du en App i en Apptjänst-miljö"
description: "Skala en app i en Apptjänst-miljö"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: jimbe
ms.assetid: 78eb1e49-4fcd-49e7-b3c7-f1906f0f22e3
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: ccompy
ms.openlocfilehash: 240c2486c23b7cd84e2471bf5b2170e08ee1f150
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="scaling-apps-in-an-app-service-environment"></a><span data-ttu-id="62112-103">Skala appar i en App Service Environment</span><span class="sxs-lookup"><span data-stu-id="62112-103">Scaling apps in an App Service Environment</span></span>
<span data-ttu-id="62112-104">Det finns vanligtvis tre saker du kan skala i Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="62112-104">In the Azure App Service there are normally three things you can scale:</span></span>

* <span data-ttu-id="62112-105">priser för planen</span><span class="sxs-lookup"><span data-stu-id="62112-105">pricing plan</span></span>
* <span data-ttu-id="62112-106">Worker storlek</span><span class="sxs-lookup"><span data-stu-id="62112-106">worker size</span></span> 
* <span data-ttu-id="62112-107">antal instanser.</span><span class="sxs-lookup"><span data-stu-id="62112-107">number of instances.</span></span>

<span data-ttu-id="62112-108">Det finns inget behov av att välja eller ändra prisplanen i en ASE.</span><span class="sxs-lookup"><span data-stu-id="62112-108">In an ASE there is no need to select or change the pricing plan.</span></span>  <span data-ttu-id="62112-109">Vad gäller funktioner har den redan en Premium prissättning kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="62112-109">In terms of capabilities it is already at a Premium pricing capability level.</span></span>  

<span data-ttu-id="62112-110">Med avseende på worker storlekar tilldela ASE-administratör storleken på beräkningsresurser som ska användas för varje arbetspool.</span><span class="sxs-lookup"><span data-stu-id="62112-110">With respect to worker sizes, the ASE admin can assign the size of the compute resource to be used for each worker pool.</span></span>  <span data-ttu-id="62112-111">Det innebär du kan ha Worker Pool 1 med P4 beräkningsresurser och Worker Pool 2 med P1 beräkningsresurser om så önskas.</span><span class="sxs-lookup"><span data-stu-id="62112-111">That means you can have Worker Pool 1 with P4 compute resources and Worker Pool 2 with P1 compute resources, if desired.</span></span>  <span data-ttu-id="62112-112">De behöver inte vara i storlek ordning.</span><span class="sxs-lookup"><span data-stu-id="62112-112">They do not have to be in size order.</span></span>  <span data-ttu-id="62112-113">Mer information kring storlekarna och deras prisnivå finns i dokumentet här [priser för Azure Apptjänst][AppServicePricing].</span><span class="sxs-lookup"><span data-stu-id="62112-113">For details around the sizes and their pricing see the document here [Azure App Service Pricing][AppServicePricing].</span></span>  <span data-ttu-id="62112-114">Detta lämnar skalningsalternativ för webbprogram och Apptjänstplaner i en Apptjänst-miljö ska vara:</span><span class="sxs-lookup"><span data-stu-id="62112-114">This leaves the scaling options for web apps and App Service Plans in an App Service Environment to be:</span></span>

* <span data-ttu-id="62112-115">val av Worker-pool</span><span class="sxs-lookup"><span data-stu-id="62112-115">worker pool selection</span></span>
* <span data-ttu-id="62112-116">antal instanser</span><span class="sxs-lookup"><span data-stu-id="62112-116">number of instances</span></span>

<span data-ttu-id="62112-117">Ändra antingen objektet görs via Gränssnittet lämpliga visas för dina ASE finns App Service-planer.</span><span class="sxs-lookup"><span data-stu-id="62112-117">Changing either item is done through the appropriate UI shown for your ASE hosted App Service Plans.</span></span>  

![][1]

<span data-ttu-id="62112-118">Du kan skala upp ASP utöver antalet tillgängliga beräkningsresurser i poolen worker som ASP finns i.</span><span class="sxs-lookup"><span data-stu-id="62112-118">You can't scale up your ASP beyond the number of available compute resources in the worker pool that your ASP is in.</span></span>  <span data-ttu-id="62112-119">Du måste hämta ASE administratören lägga till dem om du behöver beräkningsresurser i den poolen, worker.</span><span class="sxs-lookup"><span data-stu-id="62112-119">If you need compute resources in that worker pool you need to get your ASE administrator to add them.</span></span>  <span data-ttu-id="62112-120">För information om hur du konfigurerar din ASE läsa information här: [så här konfigurerar du en Apptjänst-miljö][HowtoConfigureASE].</span><span class="sxs-lookup"><span data-stu-id="62112-120">For information around re-configuring your ASE read the information here: [How to Configure an App Service environment][HowtoConfigureASE].</span></span>  <span data-ttu-id="62112-121">Du kanske också vill dra nytta av ASE Autoskala funktioner att lägga till kapacitet baserat på schema eller mått.</span><span class="sxs-lookup"><span data-stu-id="62112-121">You may also want to take advantage of the ASE autoscale features to add capacity based on schedule or metrics.</span></span>  <span data-ttu-id="62112-122">Om du vill få mer information om hur du konfigurerar Autoskala för ASE miljön själva finns [hur du konfigurerar Autoskala för en Apptjänstmiljö][ASEAutoscale].</span><span class="sxs-lookup"><span data-stu-id="62112-122">To get more details on configuring autoscale for the ASE environment itself see [How to configure autoscale for an App Service Environment][ASEAutoscale].</span></span>

<span data-ttu-id="62112-123">Du kan skapa flera app service-planer med beräkningsresurser från olika arbetarpooler eller kan du använda samma worker-poolen.</span><span class="sxs-lookup"><span data-stu-id="62112-123">You can create multiple app service plans using compute resources from different worker pools, or you can use the same worker pool.</span></span>  <span data-ttu-id="62112-124">Till exempel om du har (10) tillgängliga beräkningsresurser i Worker Pool 1, kan du skapa en apptjänstplan med (6) beräkningsresurser och en andra app service-plan som använder (4) beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="62112-124">For example if you have (10) available compute resources in Worker Pool 1, you can choose to create one app service plan using (6) compute resources, and a second app service plan that uses (4) compute resources.</span></span>

### <a name="scaling-the-number-of-instances"></a><span data-ttu-id="62112-125">Skala antalet instanser</span><span class="sxs-lookup"><span data-stu-id="62112-125">Scaling the number of instances</span></span>
<span data-ttu-id="62112-126">När du först skapar ditt webbprogram i en Apptjänst-miljö startar med 1-instans.</span><span class="sxs-lookup"><span data-stu-id="62112-126">When you first create your web app in an App Service Environment it starts with 1 instance.</span></span>  <span data-ttu-id="62112-127">Du kan skala ut till ytterligare instanser för att tillhandahålla ytterligare beräkningsresurser för din app.</span><span class="sxs-lookup"><span data-stu-id="62112-127">You can then scale out to additional instances to provide additional compute resources for your app.</span></span>   

<span data-ttu-id="62112-128">Om din ASE har tillräckligt med kapacitet är detta ganska enkelt.</span><span class="sxs-lookup"><span data-stu-id="62112-128">If your ASE has enough capacity then this is pretty simple.</span></span>  <span data-ttu-id="62112-129">Du gå till din App Service-Plan som innehåller platser som du vill skala upp och välj skala.</span><span class="sxs-lookup"><span data-stu-id="62112-129">You go to your App Service Plan that holds the sites you want to scale up and select Scale.</span></span>  <span data-ttu-id="62112-130">Detta öppnar gränssnitt där du kan manuellt ange skalan för ASP eller konfigurera automatiska regler för ASP.</span><span class="sxs-lookup"><span data-stu-id="62112-130">This opens the UI where you can manually set the scale for your ASP or configure autoscale rules for your ASP.</span></span>  <span data-ttu-id="62112-131">Att manuellt skala ditt program bara ange ***skala genom*** till ***instansantal som anges manuellt***.</span><span class="sxs-lookup"><span data-stu-id="62112-131">To manually scale your app simply set ***Scale by*** to ***an instance count that I enter manually***.</span></span>  <span data-ttu-id="62112-132">Härifrån dra reglaget till önskat antal eller ange den i rutan bredvid skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="62112-132">From here either drag the slider to the desired quantity or enter it in the box next to the slider.</span></span>  

![][2] 

<span data-ttu-id="62112-133">Automatiska regler för ett ASP i en ASE fungerar på samma sätt som de normalt gör.</span><span class="sxs-lookup"><span data-stu-id="62112-133">The autoscale rules for an ASP in an ASE work the same as they do normally.</span></span>  <span data-ttu-id="62112-134">Du kan välja ***Prosessorprocent*** under ***skala genom*** och skapar automatiska regler för ASP baserat på CPU-procent eller du kan skapa mer komplexa regler med hjälp av ***schema-och prestanda***.</span><span class="sxs-lookup"><span data-stu-id="62112-134">You can select ***CPU Percentage*** under ***Scale by*** and create autoscale rules for your ASP based on CPU Percentage or you can create more complex rules using ***schedule and performance rules***.</span></span>  <span data-ttu-id="62112-135">Se mer information om hur du konfigurerar Autoskala använda guiden här [skala en app i Azure App Service][AppScale].</span><span class="sxs-lookup"><span data-stu-id="62112-135">To see more complete details on configuring autoscale use the guide here [Scale an app in Azure App Service][AppScale].</span></span> 

### <a name="worker-pool-selection"></a><span data-ttu-id="62112-136">Val av Worker-Pool</span><span class="sxs-lookup"><span data-stu-id="62112-136">Worker Pool selection</span></span>
<span data-ttu-id="62112-137">Som tidigare nämnts används worker poolen markeringen från ASP-Gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="62112-137">As noted earlier, the worker pool selection is accessed from the ASP UI.</span></span>  <span data-ttu-id="62112-138">Öppna bladet för ASP som du vill skala och välj arbetspool.</span><span class="sxs-lookup"><span data-stu-id="62112-138">Open the blade for the ASP that you want to scale and select worker pool.</span></span>  <span data-ttu-id="62112-139">Du kommer se alla worker-pooler som du har konfigurerat i din Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="62112-139">You will see all of the worker pools which you have configured in your App Service Environment.</span></span>  <span data-ttu-id="62112-140">Om du har bara en arbetspool kommer du bara se en pool i listan.</span><span class="sxs-lookup"><span data-stu-id="62112-140">If you have only one worker pool then you will only see the one pool listed.</span></span>  <span data-ttu-id="62112-141">Om du vill ändra vilka arbetspool ASP är i välja du bara arbetspool som du vill att din App Service-Plan att flytta till.</span><span class="sxs-lookup"><span data-stu-id="62112-141">To change what worker pool your ASP is in, you simply select the worker pool you want your App Service Plan to move to.</span></span>  

![][3]

<span data-ttu-id="62112-142">Det är viktigt att se till att du har tillräcklig kapacitet för ASP innan du flyttar ASP från en arbetspool till en annan.</span><span class="sxs-lookup"><span data-stu-id="62112-142">Before moving your ASP from one worker pool to another it is important to make sure you will have adequate capacity for your ASP.</span></span>  <span data-ttu-id="62112-143">I listan över arbetarpooler, inte bara poolnamn worker visas men du kan också se hur många personer som är tillgängliga i den poolen, worker.</span><span class="sxs-lookup"><span data-stu-id="62112-143">In the list of worker pools, not only is the worker pool name listed but you can also see how many workers are available in that worker pool.</span></span>  <span data-ttu-id="62112-144">Kontrollera att det finns tillräckligt med tillgängliga som innehåller din Programtjänstplan instanser.</span><span class="sxs-lookup"><span data-stu-id="62112-144">Make sure that there are enough instances available to contain your App Service Plan.</span></span>  <span data-ttu-id="62112-145">Om du behöver fler beräkningsresurser i arbetspool som du vill flytta till, får du ASE administratören lägga till dem.</span><span class="sxs-lookup"><span data-stu-id="62112-145">If you need more compute resources in the worker pool you wish to move to, then get your ASE administrator to add them.</span></span>  

> [!NOTE]
> <span data-ttu-id="62112-146">Flytta en ASP från en arbetspool gör att kalla startar av appar i att ASP.</span><span class="sxs-lookup"><span data-stu-id="62112-146">Moving an ASP from one worker pool will cause cold starts of the apps in that ASP.</span></span>  <span data-ttu-id="62112-147">Detta kan orsaka begäranden körs långsamt som din app är kall startats på de nya beräkningsresurserna.</span><span class="sxs-lookup"><span data-stu-id="62112-147">This can cause requests to run slowly as your app is cold started on the new compute resources.</span></span>  <span data-ttu-id="62112-148">Kallstart kan undvikas genom att använda den [programmet varmt in kapaciteten] [ AppWarmup] i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="62112-148">The cold start can be avoided by using the [application warm up capability][AppWarmup] in Azure App Service.</span></span>  <span data-ttu-id="62112-149">Modulen programinitiering som beskrivs i artikel fungerar även för kallstart eftersom initieringsprocessen anropas också när appar är kall startats på nya beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="62112-149">The Application Initialization module described in the article also works for cold starts because the initialization process is also invoked when apps are cold started on new compute resources.</span></span> 
> 
> 

## <a name="getting-started"></a><span data-ttu-id="62112-150">Komma igång</span><span class="sxs-lookup"><span data-stu-id="62112-150">Getting started</span></span>
<span data-ttu-id="62112-151">Kom igång med Apptjänstmiljöer finns [hur att skapa en Apptjänst-miljö][HowtoCreateASE]</span><span class="sxs-lookup"><span data-stu-id="62112-151">To get started with App Service Environments, see [How To Create An App Service Environment][HowtoCreateASE]</span></span>

<span data-ttu-id="62112-152">Mer information om plattformen Azure App Service finns [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="62112-152">For more information about the Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
