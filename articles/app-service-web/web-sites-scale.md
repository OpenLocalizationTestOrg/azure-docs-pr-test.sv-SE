---
title: aaaScale in en app i Azure | Microsoft Docs
description: "Lär dig hur tooscale upp en app i Azure App Service tooadd kapacitet och funktioner."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.openlocfilehash: 97f46f77aeef95aec90d38e8396a023820e3caeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-up-an-app-in-azure"></a><span data-ttu-id="4d91e-103">Skala upp en app i Azure</span><span class="sxs-lookup"><span data-stu-id="4d91e-103">Scale up an app in Azure</span></span>
<span data-ttu-id="4d91e-104">Den här artikeln beskrivs hur du tooscale din app i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="4d91e-104">This article shows you how tooscale your app in Azure App Service.</span></span> <span data-ttu-id="4d91e-105">Det finns två arbetsflöden för skalning, skala upp och skala ut och den här artikeln förklarar hello skala in arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="4d91e-105">There are two workflows for scaling, scale up and scale out, and this article explains hello scale up workflow.</span></span>

* <span data-ttu-id="4d91e-106">[Skala upp](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): hämta mer CPU, minne, diskutrymme och extra funktioner såsom dedikerade virtuella datorer (VM), anpassade domäner och certifikat, mellanlagringsplatser kortplatser, autoskalning med mera.</span><span class="sxs-lookup"><span data-stu-id="4d91e-106">[Scale up](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Get more CPU, memory, disk space, and extra features like dedicated virtual machines (VMs), custom domains and certificates, staging slots, autoscaling, and more.</span></span> <span data-ttu-id="4d91e-107">Du skalar upp genom att ändra hello prisnivån för App Service-plan som tillhör din app.</span><span class="sxs-lookup"><span data-stu-id="4d91e-107">You scale up by changing hello pricing tier of the App Service plan that your app belongs to.</span></span>
* <span data-ttu-id="4d91e-108">[Skala ut](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): öka hello antalet VM-instanser som appen körs.</span><span class="sxs-lookup"><span data-stu-id="4d91e-108">[Scale out](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Increase hello number of VM instances that run your app.</span></span>
  <span data-ttu-id="4d91e-109">Du kan skala ut tooas många som 20 instanser, beroende på din prisnivå.</span><span class="sxs-lookup"><span data-stu-id="4d91e-109">You can scale out tooas many as 20 instances, depending on your pricing tier.</span></span> <span data-ttu-id="4d91e-110">[Apptjänstmiljöer](../app-service/app-service-app-service-environments-readme.md) i **Premium** öka nivån ytterligare dina skalbar antal too50-instanser.</span><span class="sxs-lookup"><span data-stu-id="4d91e-110">[App Service Environments](../app-service/app-service-app-service-environments-readme.md) in **Premium** tier will further increase your scale-out count too50 instances.</span></span> <span data-ttu-id="4d91e-111">Mer information om att skala ut finns [skala instansantalet manuellt eller automatiskt](../monitoring-and-diagnostics/insights-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="4d91e-111">For more information about scaling out, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md).</span></span> <span data-ttu-id="4d91e-112">Det finns out hur toouse autoskalning, vilket är tooscale instansantalet automatiskt baserat på fördefinierade regler och scheman.</span><span class="sxs-lookup"><span data-stu-id="4d91e-112">There you will find out how toouse autoscaling, which is tooscale instance count automatically based on predefined rules and schedules.</span></span>

<span data-ttu-id="4d91e-113">Hej skala inställningar ta endast sekunder tooapply och påverkar alla program i din [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4d91e-113">hello scale settings take only seconds tooapply and affect all apps in your [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
<span data-ttu-id="4d91e-114">De behöver du toochange koden eller inte omdistribuera ditt program.</span><span class="sxs-lookup"><span data-stu-id="4d91e-114">They do not require you toochange your code or redeploy your application.</span></span>

<span data-ttu-id="4d91e-115">Information om hello priser och funktioner i enskilda App Service-planer finns [App Service-prisinformation](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="4d91e-115">For information about hello pricing and features of individual App Service plans, see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>  

> [!NOTE]
> <span data-ttu-id="4d91e-116">Innan du växlar en apptjänstplan från hello **lediga** nivån, måste du först ta bort hello [utgiftsgränser](https://azure.microsoft.com/pricing/spending-limits/) för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4d91e-116">Before you switch an App Service plan from hello **Free** tier, you must first remove hello [spending limits](https://azure.microsoft.com/pricing/spending-limits/) in place for your Azure subscription.</span></span> <span data-ttu-id="4d91e-117">tooview eller ändra alternativen för prenumerationen Microsoft Azure App Service finns [Microsoft Azure-prenumerationer][azuresubscriptions].</span><span class="sxs-lookup"><span data-stu-id="4d91e-117">tooview or change options for your Microsoft Azure App Service subscription, see [Microsoft Azure Subscriptions][azuresubscriptions].</span></span>
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a><span data-ttu-id="4d91e-118">Skala upp din prisnivå</span><span class="sxs-lookup"><span data-stu-id="4d91e-118">Scale up your pricing tier</span></span>
1. <span data-ttu-id="4d91e-119">Öppna i din webbläsare hello [Azure-portalen][portal].</span><span class="sxs-lookup"><span data-stu-id="4d91e-119">In your browser, open hello [Azure portal][portal].</span></span>
2. <span data-ttu-id="4d91e-120">I bladet för din app, klickar du på **alla inställningar**, och klicka sedan på **skala upp**.</span><span class="sxs-lookup"><span data-stu-id="4d91e-120">In your app's blade, click **All settings**, and then click **Scale Up**.</span></span>
   
    ![Navigera tooscale in din Azure-app.][ChooseWHP]
3. <span data-ttu-id="4d91e-122">Välj din nivå och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="4d91e-122">Choose your tier, and then click **Select**.</span></span>
   
    <span data-ttu-id="4d91e-123">Hej **meddelanden** fliken blinkar en grön **lyckade** när hello åtgärd har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4d91e-123">hello **Notifications** tab will flash a green **SUCCESS** after hello operation is complete.</span></span>

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a><span data-ttu-id="4d91e-124">Skala relaterade resurser</span><span class="sxs-lookup"><span data-stu-id="4d91e-124">Scale related resources</span></span>
<span data-ttu-id="4d91e-125">Om din app är beroende av andra tjänster, till exempel Azure SQL Database eller Azure Storage, kan du även skala upp dessa resurser utifrån dina behov.</span><span class="sxs-lookup"><span data-stu-id="4d91e-125">If your app depends on other services, such as Azure SQL Database or Azure Storage, you can also scale up those resources based on your needs.</span></span> <span data-ttu-id="4d91e-126">Dessa resurser skalas inte med hello App Service-plan och måste skalas separat.</span><span class="sxs-lookup"><span data-stu-id="4d91e-126">These resources are not scaled with hello App Service plan and must be scaled separately.</span></span>

1. <span data-ttu-id="4d91e-127">I **Essentials**, klicka på hello **resursgruppen** länk.</span><span class="sxs-lookup"><span data-stu-id="4d91e-127">In **Essentials**, click hello **Resource group** link.</span></span>
   
    ![Skala upp din Azure-app relaterade resurser](./media/web-sites-scale/RGEssentialsLink.png)
2. <span data-ttu-id="4d91e-129">I hello **sammanfattning** tillhör hello **resursgruppen** blad, klicka på en resurs som du vill tooscale.</span><span class="sxs-lookup"><span data-stu-id="4d91e-129">In hello **Summary** part of hello **Resource group** blade, click a resource that you want tooscale.</span></span> <span data-ttu-id="4d91e-130">hello följande skärmbild visar en SQL-databas resurs och ett Azure Storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="4d91e-130">hello following screenshot shows a SQL Database resource and an Azure Storage resource.</span></span>
   
    ![Navigera tooresource grupp bladet tooscale in din Azure-app](./media/web-sites-scale/ResourceGroup.png)
3. <span data-ttu-id="4d91e-132">För en resurs för SQL-databasen, klickar du på **inställningar** > **prisnivå** tooscale hello prisnivån.</span><span class="sxs-lookup"><span data-stu-id="4d91e-132">For a SQL Database resource, click **Settings** > **Pricing tier** tooscale hello pricing tier.</span></span>
   
    ![Skala upp hello SQL Database-serverdelen för din Azure-app](./media/web-sites-scale/ScaleDatabase.png)
   
    <span data-ttu-id="4d91e-134">Du kan också aktivera [georeplikering](../sql-database/sql-database-geo-replication-overview.md) för din instans av SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="4d91e-134">You can also turn on [geo-replication](../sql-database/sql-database-geo-replication-overview.md) for your SQL Database instance.</span></span>
   
    <span data-ttu-id="4d91e-135">För ett Azure Storage-resurs, klickar du på **inställningar** > **Configuration** tooscale konfigurerar din lagringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="4d91e-135">For an Azure Storage resource, click **Settings** > **Configuration** tooscale up your storage options.</span></span>
   
    ![Skala upp hello Azure Storage-konto som används av din Azure-app](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a><span data-ttu-id="4d91e-137">Läs mer om funktionerna för utvecklare</span><span class="sxs-lookup"><span data-stu-id="4d91e-137">Learn about developer features</span></span>
<span data-ttu-id="4d91e-138">Beroende på hello prisnivån, hello följande utvecklarorienterade funktioner är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="4d91e-138">Depending on hello pricing tier, hello following developer-oriented features are available:</span></span>

### <a name="bitness"></a><span data-ttu-id="4d91e-139">Bitness</span><span class="sxs-lookup"><span data-stu-id="4d91e-139">Bitness</span></span>
* <span data-ttu-id="4d91e-140">Hej **grundläggande**, **Standard**, och **Premium** nivåer stöd för 64-bitars och 32-bitars program.</span><span class="sxs-lookup"><span data-stu-id="4d91e-140">hello **Basic**, **Standard**, and **Premium** tiers support 64-bit and 32-bit applications.</span></span>
* <span data-ttu-id="4d91e-141">Hej **lediga** och **delade** plan nivåer stöder endast 32-bitarsprogram.</span><span class="sxs-lookup"><span data-stu-id="4d91e-141">hello **Free** and **Shared** plan tiers support 32-bit applications only.</span></span>

### <a name="debugger-support"></a><span data-ttu-id="4d91e-142">Stöd för felsökning</span><span class="sxs-lookup"><span data-stu-id="4d91e-142">Debugger support</span></span>
* <span data-ttu-id="4d91e-143">Felsökare support är tillgänglig för hello **lediga**, **delade**, och **grundläggande** lägen på en anslutning per App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="4d91e-143">Debugger support is available for hello **Free**, **Shared**, and **Basic** modes at one connection per App Service plan.</span></span>
* <span data-ttu-id="4d91e-144">Felsökare support är tillgänglig för hello **Standard** och **Premium** lägen på fem anslutningar per App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="4d91e-144">Debugger support is available for hello **Standard** and **Premium** modes at five concurrent connections per App Service plan.</span></span>

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a><span data-ttu-id="4d91e-145">Lär dig mer om andra funktioner</span><span class="sxs-lookup"><span data-stu-id="4d91e-145">Learn about other features</span></span>
* <span data-ttu-id="4d91e-146">Detaljerad information om alla återstående funktioner i hello Apptjänst hello planer, inklusive priser och funktioner intresse tooall användare (inklusive utvecklare), finns [App Service-prisinformation](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="4d91e-146">For detailed information about all of hello remaining features in hello App Service plans, including pricing and features of interest tooall users (including developers), see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>

> [!NOTE]
> <span data-ttu-id="4d91e-147">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/) där du direkt kan skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="4d91e-147">If you want tooget started with Azure App Service before you sign up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/) where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="4d91e-148">Inget kreditkort krävs och gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="4d91e-148">No credit cards are required and there are no commitments.</span></span>
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a><span data-ttu-id="4d91e-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d91e-149">Next steps</span></span>
* <span data-ttu-id="4d91e-150">tooget igång med Azure, se [kostnadsfri utvärderingsversion av Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4d91e-150">tooget started with Azure, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4d91e-151">Besök hello följande länkar för information om priser, support och SERVICENIVÅAVTAL.</span><span class="sxs-lookup"><span data-stu-id="4d91e-151">For information about pricing, support, and SLA, visit hello following links.</span></span>
  
    [<span data-ttu-id="4d91e-152">Prisinformation för dataöverföringar</span><span class="sxs-lookup"><span data-stu-id="4d91e-152">Data Transfers Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [<span data-ttu-id="4d91e-153">Microsoft Azure-supportplaner</span><span class="sxs-lookup"><span data-stu-id="4d91e-153">Microsoft Azure Support Plans</span></span>](https://azure.microsoft.com/support/plans/)
  
    [<span data-ttu-id="4d91e-154">serviceavtal</span><span class="sxs-lookup"><span data-stu-id="4d91e-154">Service Level Agreements</span></span>](https://azure.microsoft.com/support/legal/sla/)
  
    [<span data-ttu-id="4d91e-155">Prisinformation för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="4d91e-155">SQL Database Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/sql-database/)
  
    <span data-ttu-id="4d91e-156">[Virtuell dator och Molntjänststorlekar för Microsoft Azure][vmsizes]</span><span class="sxs-lookup"><span data-stu-id="4d91e-156">[Virtual Machine and Cloud Service Sizes for Microsoft Azure][vmsizes]</span></span>
  
    [<span data-ttu-id="4d91e-157">Prisinformation för Apptjänst</span><span class="sxs-lookup"><span data-stu-id="4d91e-157">App Service Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/app-service/)
  
    [<span data-ttu-id="4d91e-158">Apptjänst prisinformationen - SSL-anslutningar</span><span class="sxs-lookup"><span data-stu-id="4d91e-158">App Service Pricing Details - SSL Connections</span></span>](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* <span data-ttu-id="4d91e-159">Information om Azure App Service finns bästa praxis, inklusive att skapa en skalbar och flexibel arkitektur [metodtips: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d91e-159">For information about Azure App Service best practices, including building a scalable and resilient architecture, see [Best Practices: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span></span>
* <span data-ttu-id="4d91e-160">Videor om att skala Apptjänst-appar finns i hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="4d91e-160">For videos about scaling App Service apps, see hello following resources:</span></span>
  
  * [<span data-ttu-id="4d91e-161">När tooScale Azure Websites – med Stefan Schackow</span><span class="sxs-lookup"><span data-stu-id="4d91e-161">When tooScale Azure Websites - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [<span data-ttu-id="4d91e-162">Automatisk skalning Azure Websites, CPU eller schemalagt - med Stefan Schackow</span><span class="sxs-lookup"><span data-stu-id="4d91e-162">Auto Scaling Azure Websites, CPU or Scheduled - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [<span data-ttu-id="4d91e-163">Hur Azure webbplatser skala - med Stefan Schackow</span><span class="sxs-lookup"><span data-stu-id="4d91e-163">How Azure Websites Scale - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:/pricing/details/app-service/
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
