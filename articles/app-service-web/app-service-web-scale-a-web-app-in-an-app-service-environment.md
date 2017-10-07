---
title: "aaaHow tooScale en App i en Apptjänst-miljö"
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
ms.openlocfilehash: 08916eac056c46bf8cb6edffbf96285317b32062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-apps-in-an-app-service-environment"></a><span data-ttu-id="f3f24-103">Skala appar i en App Service Environment</span><span class="sxs-lookup"><span data-stu-id="f3f24-103">Scaling apps in an App Service Environment</span></span>
<span data-ttu-id="f3f24-104">Det finns vanligtvis tre saker du kan skala i hello Azure App Service:</span><span class="sxs-lookup"><span data-stu-id="f3f24-104">In hello Azure App Service there are normally three things you can scale:</span></span>

* <span data-ttu-id="f3f24-105">priser för planen</span><span class="sxs-lookup"><span data-stu-id="f3f24-105">pricing plan</span></span>
* <span data-ttu-id="f3f24-106">Worker storlek</span><span class="sxs-lookup"><span data-stu-id="f3f24-106">worker size</span></span> 
* <span data-ttu-id="f3f24-107">antal instanser.</span><span class="sxs-lookup"><span data-stu-id="f3f24-107">number of instances.</span></span>

<span data-ttu-id="f3f24-108">Det finns inga måste tooselect eller ändra hello som priser plan i en ASE.</span><span class="sxs-lookup"><span data-stu-id="f3f24-108">In an ASE there is no need tooselect or change hello pricing plan.</span></span>  <span data-ttu-id="f3f24-109">Vad gäller funktioner har den redan en Premium prissättning kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="f3f24-109">In terms of capabilities it is already at a Premium pricing capability level.</span></span>  

<span data-ttu-id="f3f24-110">Med avseende tooworker storlekar tilldela Hej ASE administratör hello storleken på hello beräkning resurs toobe används för varje worker-pool.</span><span class="sxs-lookup"><span data-stu-id="f3f24-110">With respect tooworker sizes, hello ASE admin can assign hello size of hello compute resource toobe used for each worker pool.</span></span>  <span data-ttu-id="f3f24-111">Det innebär du kan ha Worker Pool 1 med P4 beräkningsresurser och Worker Pool 2 med P1 beräkningsresurser om så önskas.</span><span class="sxs-lookup"><span data-stu-id="f3f24-111">That means you can have Worker Pool 1 with P4 compute resources and Worker Pool 2 with P1 compute resources, if desired.</span></span>  <span data-ttu-id="f3f24-112">De har inte toobe i storlek ordning.</span><span class="sxs-lookup"><span data-stu-id="f3f24-112">They do not have toobe in size order.</span></span>  <span data-ttu-id="f3f24-113">Mer information kring hello storlekar och deras prisnivå finns hello dokumentet här [priser för Azure Apptjänst][AppServicePricing].</span><span class="sxs-lookup"><span data-stu-id="f3f24-113">For details around hello sizes and their pricing see hello document here [Azure App Service Pricing][AppServicePricing].</span></span>  <span data-ttu-id="f3f24-114">Detta lämnar hello skalningsalternativ för webbprogram och Apptjänstplaner i en Apptjänst-miljö toobe:</span><span class="sxs-lookup"><span data-stu-id="f3f24-114">This leaves hello scaling options for web apps and App Service Plans in an App Service Environment toobe:</span></span>

* <span data-ttu-id="f3f24-115">val av Worker-pool</span><span class="sxs-lookup"><span data-stu-id="f3f24-115">worker pool selection</span></span>
* <span data-ttu-id="f3f24-116">antal instanser</span><span class="sxs-lookup"><span data-stu-id="f3f24-116">number of instances</span></span>

<span data-ttu-id="f3f24-117">Ändra antingen objektet görs via hello lämpliga gränssnitt som visas för dina ASE finns App Service-planer.</span><span class="sxs-lookup"><span data-stu-id="f3f24-117">Changing either item is done through hello appropriate UI shown for your ASE hosted App Service Plans.</span></span>  

![][1]

<span data-ttu-id="f3f24-118">Du kan skala upp ASP utöver hello antalet tillgängliga beräkningsresurser i hello arbetspool som ASP finns i.</span><span class="sxs-lookup"><span data-stu-id="f3f24-118">You can't scale up your ASP beyond hello number of available compute resources in hello worker pool that your ASP is in.</span></span>  <span data-ttu-id="f3f24-119">Om du behöver beräkningsresurser i den poolen, worker måste tooget din ASE administratör tooadd dem.</span><span class="sxs-lookup"><span data-stu-id="f3f24-119">If you need compute resources in that worker pool you need tooget your ASE administrator tooadd them.</span></span>  <span data-ttu-id="f3f24-120">För information om hur du konfigurerar din ASE läsa hello information här: [hur tooConfigure en Apptjänst-miljö][HowtoConfigureASE].</span><span class="sxs-lookup"><span data-stu-id="f3f24-120">For information around re-configuring your ASE read hello information here: [How tooConfigure an App Service environment][HowtoConfigureASE].</span></span>  <span data-ttu-id="f3f24-121">Du kan också tootake nytta av hello ASE Autoskala funktioner tooadd kapacitet baserat på schema eller mått.</span><span class="sxs-lookup"><span data-stu-id="f3f24-121">You may also want tootake advantage of hello ASE autoscale features tooadd capacity based on schedule or metrics.</span></span>  <span data-ttu-id="f3f24-122">tooget finns mer information om hur du konfigurerar Autoskala för hello ASE miljö själva [hur tooconfigure Autoskala för en Apptjänstmiljö][ASEAutoscale].</span><span class="sxs-lookup"><span data-stu-id="f3f24-122">tooget more details on configuring autoscale for hello ASE environment itself see [How tooconfigure autoscale for an App Service Environment][ASEAutoscale].</span></span>

<span data-ttu-id="f3f24-123">Du kan skapa flera app service-planer med beräkningsresurser från olika arbetarpooler eller du kan använda hello samma arbetspool.</span><span class="sxs-lookup"><span data-stu-id="f3f24-123">You can create multiple app service plans using compute resources from different worker pools, or you can use hello same worker pool.</span></span>  <span data-ttu-id="f3f24-124">För exempel om du har (10) tillgängliga beräkningsresurser i arbetsprocessen Pool 1, kan du toocreate en apptjänstplan med (6) beräkningsresurser och en andra app service-plan som används (4) beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="f3f24-124">For example if you have (10) available compute resources in Worker Pool 1, you can choose toocreate one app service plan using (6) compute resources, and a second app service plan that uses (4) compute resources.</span></span>

### <a name="scaling-hello-number-of-instances"></a><span data-ttu-id="f3f24-125">Skalning hello antal instanser</span><span class="sxs-lookup"><span data-stu-id="f3f24-125">Scaling hello number of instances</span></span>
<span data-ttu-id="f3f24-126">När du först skapar ditt webbprogram i en Apptjänst-miljö startar med 1-instans.</span><span class="sxs-lookup"><span data-stu-id="f3f24-126">When you first create your web app in an App Service Environment it starts with 1 instance.</span></span>  <span data-ttu-id="f3f24-127">Därefter kan du skala ut tooadditional instanser tooprovide ytterligare beräkningsresurser för din app.</span><span class="sxs-lookup"><span data-stu-id="f3f24-127">You can then scale out tooadditional instances tooprovide additional compute resources for your app.</span></span>   

<span data-ttu-id="f3f24-128">Om din ASE har tillräckligt med kapacitet är detta ganska enkelt.</span><span class="sxs-lookup"><span data-stu-id="f3f24-128">If your ASE has enough capacity then this is pretty simple.</span></span>  <span data-ttu-id="f3f24-129">Du kan gå tooyour App Service-Plan som innehåller hello platser tooscale upp och markera skala.</span><span class="sxs-lookup"><span data-stu-id="f3f24-129">You go tooyour App Service Plan that holds hello sites you want tooscale up and select Scale.</span></span>  <span data-ttu-id="f3f24-130">Då öppnas hello användargränssnitt där du kan manuellt ange hello skala för din ASP eller konfigurera automatiska regler för ASP.</span><span class="sxs-lookup"><span data-stu-id="f3f24-130">This opens hello UI where you can manually set hello scale for your ASP or configure autoscale rules for your ASP.</span></span>  <span data-ttu-id="f3f24-131">toomanually skala ditt program bara ange ***skala genom*** för***instansantal som anges manuellt***.</span><span class="sxs-lookup"><span data-stu-id="f3f24-131">toomanually scale your app simply set ***Scale by*** too***an instance count that I enter manually***.</span></span>  <span data-ttu-id="f3f24-132">Härifrån dra hello skjutreglaget toohello önskad kvantitet eller ange i hello rutan nästa toohello skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="f3f24-132">From here either drag hello slider toohello desired quantity or enter it in hello box next toohello slider.</span></span>  

![][2] 

<span data-ttu-id="f3f24-133">hello automatiska regler för ett ASP i en ASE arbete hello samma som de normalt gör.</span><span class="sxs-lookup"><span data-stu-id="f3f24-133">hello autoscale rules for an ASP in an ASE work hello same as they do normally.</span></span>  <span data-ttu-id="f3f24-134">Du kan välja ***Prosessorprocent*** under ***skala genom*** och skapar automatiska regler för ASP baserat på CPU-procent eller du kan skapa mer komplexa regler med hjälp av ***schema-och prestanda***.</span><span class="sxs-lookup"><span data-stu-id="f3f24-134">You can select ***CPU Percentage*** under ***Scale by*** and create autoscale rules for your ASP based on CPU Percentage or you can create more complex rules using ***schedule and performance rules***.</span></span>  <span data-ttu-id="f3f24-135">toosee slutföra mer information om hur du konfigurerar Autoskala Använd hello guiden här [skala en app i Azure App Service][AppScale].</span><span class="sxs-lookup"><span data-stu-id="f3f24-135">toosee more complete details on configuring autoscale use hello guide here [Scale an app in Azure App Service][AppScale].</span></span> 

### <a name="worker-pool-selection"></a><span data-ttu-id="f3f24-136">Val av Worker-Pool</span><span class="sxs-lookup"><span data-stu-id="f3f24-136">Worker Pool selection</span></span>
<span data-ttu-id="f3f24-137">Som tidigare nämnts åt hello worker poolen markeringen från hello ASP-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f3f24-137">As noted earlier, hello worker pool selection is accessed from hello ASP UI.</span></span>  <span data-ttu-id="f3f24-138">Öppna hello bladet för hello ASP som du vill använda tooscale och välj arbetspool.</span><span class="sxs-lookup"><span data-stu-id="f3f24-138">Open hello blade for hello ASP that you want tooscale and select worker pool.</span></span>  <span data-ttu-id="f3f24-139">Du kommer se alla hello arbetarpooler som du har konfigurerat i din Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="f3f24-139">You will see all of hello worker pools which you have configured in your App Service Environment.</span></span>  <span data-ttu-id="f3f24-140">Om du har bara en arbetspool kommer du bara se hello en pool i listan.</span><span class="sxs-lookup"><span data-stu-id="f3f24-140">If you have only one worker pool then you will only see hello one pool listed.</span></span>  <span data-ttu-id="f3f24-141">toochange vilken worker poolen ASP, du bara välja hello arbetspool som du vill att din App Service-Plan toomove till.</span><span class="sxs-lookup"><span data-stu-id="f3f24-141">toochange what worker pool your ASP is in, you simply select hello worker pool you want your App Service Plan toomove to.</span></span>  

![][3]

<span data-ttu-id="f3f24-142">Innan du flyttar ASP från en arbetare poolen tooanother är det viktigt att du har tillräcklig kapacitet för ASP toomake.</span><span class="sxs-lookup"><span data-stu-id="f3f24-142">Before moving your ASP from one worker pool tooanother it is important toomake sure you will have adequate capacity for your ASP.</span></span>  <span data-ttu-id="f3f24-143">I hello lista över arbetarpooler, inte bara hello worker namn visas men du kan också se hur många personer som är tillgängliga i den poolen, worker.</span><span class="sxs-lookup"><span data-stu-id="f3f24-143">In hello list of worker pools, not only is hello worker pool name listed but you can also see how many workers are available in that worker pool.</span></span>  <span data-ttu-id="f3f24-144">Kontrollera att det inte finns tillräckligt med tillgängliga instanser toocontain din Programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="f3f24-144">Make sure that there are enough instances available toocontain your App Service Plan.</span></span>  <span data-ttu-id="f3f24-145">Om du behöver fler beräkningsresurser i hello arbetspool gärna toomove att hämta dina ASE administratör tooadd dem.</span><span class="sxs-lookup"><span data-stu-id="f3f24-145">If you need more compute resources in hello worker pool you wish toomove to, then get your ASE administrator tooadd them.</span></span>  

> [!NOTE]
> <span data-ttu-id="f3f24-146">Flytta en ASP från en arbetspool gör att kalla startar hello appar i att ASP.</span><span class="sxs-lookup"><span data-stu-id="f3f24-146">Moving an ASP from one worker pool will cause cold starts of hello apps in that ASP.</span></span>  <span data-ttu-id="f3f24-147">Detta kan orsaka begäranden toorun långsamt som din app är kall startats på hello nya beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="f3f24-147">This can cause requests toorun slowly as your app is cold started on hello new compute resources.</span></span>  <span data-ttu-id="f3f24-148">hello kallstart kan undvikas genom att använda hello [programmet varmt in kapaciteten] [ AppWarmup] i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="f3f24-148">hello cold start can be avoided by using hello [application warm up capability][AppWarmup] in Azure App Service.</span></span>  <span data-ttu-id="f3f24-149">hello programinitiering modulen beskrivs i artikeln hello fungerar även för kallstart eftersom hello initieringsprocessen anropas också när appar är kall startats på nya beräkningsresurser.</span><span class="sxs-lookup"><span data-stu-id="f3f24-149">hello Application Initialization module described in hello article also works for cold starts because hello initialization process is also invoked when apps are cold started on new compute resources.</span></span> 
> 
> 

## <a name="getting-started"></a><span data-ttu-id="f3f24-150">Komma igång</span><span class="sxs-lookup"><span data-stu-id="f3f24-150">Getting started</span></span>
<span data-ttu-id="f3f24-151">tooget igång med Apptjänstmiljöer, se [hur tooCreate en Apptjänst-miljö][HowtoCreateASE]</span><span class="sxs-lookup"><span data-stu-id="f3f24-151">tooget started with App Service Environments, see [How tooCreate An App Service Environment][HowtoCreateASE]</span></span>

<span data-ttu-id="f3f24-152">Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="f3f24-152">For more information about hello Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

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
