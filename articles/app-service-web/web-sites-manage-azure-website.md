---
title: aaaManage en webbapp i Azure App Service
description: "Länkar tooresources för att hantera en webbapp i Azure App Service."
services: app-service\web
documentationcenter: 
author: erikre
manager: erikre
editor: 
ms.assetid: d5e2887a-84f9-4747-a573-867635cb8b39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: rachelap
ms.openlocfilehash: daf69245e66068b0e97e3ae1c3fb5fce45605b91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a><span data-ttu-id="bbf9b-103">Hantera en webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bbf9b-103">Manage a web app in Azure App Service</span></span>
<span data-ttu-id="bbf9b-104">Det här avsnittet innehåller länkar tooresources för att hantera en webbapp i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-104">This topic contains links tooresources for managing a web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="bbf9b-105">Hantering innehåller alla hello-åtgärder som håller din webbapp körs utan problem.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-105">Management includes all of hello tasks that keep your web app running smoothly.</span></span> 

<span data-ttu-id="bbf9b-106">Över hello livslängden för en webbapp, ska du utföra olika hanteringsuppgifter, när du flyttar från första distributionen toonormal åtgärden, underhåll och uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-106">Over hello lifetime of a web app, you will perform different management tasks, as you move from initial deployment toonormal operation, maintenance, and updates.</span></span>

<span data-ttu-id="bbf9b-107">Många aktiviteter för webb-app kan utföras i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-107">Many web app management tasks can be performed in hello Azure Portal.</span></span>

## <a name="before-you-deploy-your-web-app-tooproduction"></a><span data-ttu-id="bbf9b-108">Innan du distribuerar din web app tooproduction</span><span class="sxs-lookup"><span data-stu-id="bbf9b-108">Before you deploy your web app tooproduction</span></span>
### <a name="choose-a-tier"></a><span data-ttu-id="bbf9b-109">Välj en nivå</span><span class="sxs-lookup"><span data-stu-id="bbf9b-109">Choose a tier</span></span>
<span data-ttu-id="bbf9b-110">Azure Apptjänst finns med i fem nivåer: Frigör delade, Basic, Standard och Premium.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-110">Azure App Service is offered in five tiers: Free, Shared, Basic, Standard, and Premium.</span></span> <span data-ttu-id="bbf9b-111">Information om hello funktioner och priser för varje nivå finns [prisinformationen](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-111">For information about hello features and pricing for each tier, see [Pricing details](https://azure.microsoft.com/pricing/details/app-service/).</span></span> 

* <span data-ttu-id="bbf9b-112">[Apptjänstplaner](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) kan du gruppera flera webbprogram under hello samma nivå.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-112">[App Service plans](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) let you group multiple web apps under hello same tier.</span></span>
* <span data-ttu-id="bbf9b-113">Du kan alltid [växla nivåer](web-sites-scale.md) när du har skapat ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-113">You can always [switch tiers](web-sites-scale.md) after you create your web app.</span></span>

### <a name="configuration"></a><span data-ttu-id="bbf9b-114">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bbf9b-114">Configuration</span></span>
<span data-ttu-id="bbf9b-115">Använd hello [Azure Portal](https://portal.azure.com/) tooset olika konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-115">Use hello [Azure Portal](https://portal.azure.com/) tooset various configuration options.</span></span> <span data-ttu-id="bbf9b-116">Mer information finns i [konfigurera webbappar i Azure App Service](web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-116">For details, see [Configure web apps in Azure App Service](web-sites-configure.md).</span></span> <span data-ttu-id="bbf9b-117">Här är en snabb Checklista:</span><span class="sxs-lookup"><span data-stu-id="bbf9b-117">Here is a quick checklist:</span></span>

* <span data-ttu-id="bbf9b-118">Välj **runtime versioner** för .NET, PHP, Java och Python, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-118">Select **runtime versions** for .NET, PHP, Java, or Python, if needed.</span></span>
* <span data-ttu-id="bbf9b-119">Aktivera **WebSockets** om ditt webbprogram använder hello WebSocket-protokollet.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-119">Enable **WebSockets** if your web app uses hello WebSocket protocol.</span></span> <span data-ttu-id="bbf9b-120">(Detta omfattar appar som använder [ASP.NET SignalR](http://www.asp.net/signalr) eller [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span><span class="sxs-lookup"><span data-stu-id="bbf9b-120">(This includes apps that use [ASP.NET SignalR](http://www.asp.net/signalr) or [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span></span>
* <span data-ttu-id="bbf9b-121">Kör du webbjobb?</span><span class="sxs-lookup"><span data-stu-id="bbf9b-121">Are you running continuous web jobs?</span></span> <span data-ttu-id="bbf9b-122">I så fall, aktivera **alltid på**.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-122">If so, enable **Always On**.</span></span>
* <span data-ttu-id="bbf9b-123">Ange hello **standarddokument**, till exempel index.html.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-123">Set hello **default document**, such as index.html.</span></span>

<span data-ttu-id="bbf9b-124">I tillägg toothese grundläggande konfigurationsinställningar, kanske du vill tooconfigure hello följande:</span><span class="sxs-lookup"><span data-stu-id="bbf9b-124">In addition toothese basic configuration settings, you may want tooconfigure hello following:</span></span>

* <span data-ttu-id="bbf9b-125">**Secure Socket Layer (SSL)** kryptering.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-125">**Secure Socket Layer (SSL)** encryption.</span></span> <span data-ttu-id="bbf9b-126">toouse SSL med ett anpassat domännamn måste du få ett SSL-certifikat och konfigurera din web app toouse den.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-126">toouse SSL with a custom domain name, you must get an SSL certificate and configure your web app toouse it.</span></span> <span data-ttu-id="bbf9b-127">Se [aktivera HTTPS för en webbapp i Azure App Service](app-service-web-tutorial-custom-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-127">See [Enable HTTPS for a web app in Azure App Service](app-service-web-tutorial-custom-ssl.md).</span></span>
* <span data-ttu-id="bbf9b-128">**Anpassade domännamn.**</span><span class="sxs-lookup"><span data-stu-id="bbf9b-128">**Custom domain name.**</span></span> <span data-ttu-id="bbf9b-129">Ditt webbprogram har automatiskt en underdomän under azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-129">Your web app automatically has a subdomain under azurewebsites.net.</span></span> <span data-ttu-id="bbf9b-130">Du kan associera ett anpassat domännamn, t.ex contoso.com. Se [konfigurera ett anpassat domännamn i Azure App Service](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-130">You can associate a custom domain name, such as contoso.com. See [Configure a custom domain name in Azure App Service](app-service-web-tutorial-custom-domain.md).</span></span>

<span data-ttu-id="bbf9b-131">Språkspecifika konfiguration:</span><span class="sxs-lookup"><span data-stu-id="bbf9b-131">Language-specific configuration:</span></span>

* <span data-ttu-id="bbf9b-132">**PHP**: [konfigurerar du PHP i Azure App Service Web Apps](web-sites-php-configure.md).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-132">**PHP**: [Configure PHP in Azure App Service Web Apps](web-sites-php-configure.md).</span></span>
* <span data-ttu-id="bbf9b-133">**Python**: [konfigurera Python med Azure App Service Web Apps](web-sites-python-configure.md)</span><span class="sxs-lookup"><span data-stu-id="bbf9b-133">**Python**: [Configuring Python with Azure App Service Web Apps](web-sites-python-configure.md)</span></span>

## <a name="while-your-web-app-is-running"></a><span data-ttu-id="bbf9b-134">När ditt webbprogram kör</span><span class="sxs-lookup"><span data-stu-id="bbf9b-134">While your web app is running</span></span>
<span data-ttu-id="bbf9b-135">När din webbapp körs, vill du att den är tillgänglig och att det skalas toomeet användaraktiviteten toomake.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-135">While your web app is running, you want toomake sure it is available, and that it scales toomeet user traffic.</span></span> <span data-ttu-id="bbf9b-136">Du kanske också måste tootroubleshoot fel.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-136">You may also need tootroubleshoot errors.</span></span>

### <a name="monitoring"></a><span data-ttu-id="bbf9b-137">Övervakning</span><span class="sxs-lookup"><span data-stu-id="bbf9b-137">Monitoring</span></span>
* <span data-ttu-id="bbf9b-138">Via hello Azure-portalen, kan du [lägga till prestandamått](web-sites-monitor.md) , till exempel CPU-användning och antalet klientbegäranden.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-138">Through hello Azure Portal, you can [add performance metrics](web-sites-monitor.md) such as CPU usage and number of client requests.</span></span>
* <span data-ttu-id="bbf9b-139">[Skala ditt webbprogram](web-sites-scale.md) i svaret tootraffic.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-139">[Scale your web app](web-sites-scale.md) in response tootraffic.</span></span> <span data-ttu-id="bbf9b-140">Beroende på din nivå kan du skala hello antal virtuella datorer och/eller hello storleken på hello VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-140">Depending on your tier, you can scale hello number of VMs and/or hello size of hello VM instances.</span></span> <span data-ttu-id="bbf9b-141">I hello Standard och Premium-nivåer, kan du också ställa in autoskalning, så att ditt webbprogram skalar automatiskt enligt ett fast schema eller i svaret tooload.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-141">In hello Standard and Premium tiers, you can also set up autoscaling, so your web app scales automatically, either on a fixed schedule, or in response tooload.</span></span>  

### <a name="backups"></a><span data-ttu-id="bbf9b-142">Säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="bbf9b-142">Backups</span></span>
* <span data-ttu-id="bbf9b-143">Ange [automatiska säkerhetskopieringar](web-sites-backup.md) av ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-143">Set [automatic backups](web-sites-backup.md) of your web app.</span></span> <span data-ttu-id="bbf9b-144">Mer information om säkerhetskopior i [den här videon](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-144">Learn more about backups in [this video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span></span>
* <span data-ttu-id="bbf9b-145">Lär dig mer om hello alternativ för [återställning av databas](../sql-database/sql-database-business-continuity.md) i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-145">Learn about hello options for [database recovery](../sql-database/sql-database-business-continuity.md) in Azure SQL Database.</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="bbf9b-146">Felsökning</span><span class="sxs-lookup"><span data-stu-id="bbf9b-146">Troubleshooting</span></span>
* <span data-ttu-id="bbf9b-147">Om något går fel kan du [felsökning i Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), med hjälp av diagnostiska loggar och live-felsökning i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-147">If something goes wrong, you can [troubleshoot in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), using diagnostic logs and live debugging in hello cloud.</span></span> 
* <span data-ttu-id="bbf9b-148">Det finns olika sätt toocollect diagnostikloggar utanför Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-148">Outside of Visual Studio, there are various ways toocollect diagnostic logs.</span></span> <span data-ttu-id="bbf9b-149">Se [aktivera diagnostikloggning för web apps i Azure App Service](web-sites-enable-diagnostic-log.md).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-149">See [Enable diagnostics logging for web apps in Azure App Service](web-sites-enable-diagnostic-log.md).</span></span>
* <span data-ttu-id="bbf9b-150">Node.js-program finns i [hur toodebug en Node.js webbapp i Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-150">For Node.js applications, see [How toodebug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>

### <a name="restoring-data"></a><span data-ttu-id="bbf9b-151">Återställa Data</span><span class="sxs-lookup"><span data-stu-id="bbf9b-151">Restoring Data</span></span>
* <span data-ttu-id="bbf9b-152">[Återställa](web-sites-restore.md) ett webbprogram som tidigare har säkerhetskopierats.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-152">[Restore](web-sites-restore.md) a web app that was previously backed up.</span></span>

## <a name="when-you-update-your-web-app"></a><span data-ttu-id="bbf9b-153">När du uppdaterar ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="bbf9b-153">When you update your web app</span></span>
<span data-ttu-id="bbf9b-154">Om du inte har aktiverat automatiska säkerhetskopieringar kan du skapa en [manuell säkerhetskopiering](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-154">If you have not enabled automatic backups, you can create a [manual backup](web-sites-backup.md).</span></span>

<span data-ttu-id="bbf9b-155">Överväg att använda [stegvis distribution](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="bbf9b-155">Consider using [staged deployment](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="bbf9b-156">Det här alternativet kan du publicera uppdateringar tooa mellanlagring av distribution som körs sida-vid-sida med din Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="bbf9b-156">This option lets you publish updates tooa staging deployment that runs side-by-side with your production deployment.</span></span> 


<!-- Anchors. -->

[Before you deploy your site tooproduction]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website


