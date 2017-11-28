---
title: Hantera en webbapp i Azure App Service
description: "Länkar till resurser för att hantera en webbapp i Azure App Service."
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
ms.openlocfilehash: 9e19618a1b24bbdf3163ddfc3423c5c932dcd7af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a><span data-ttu-id="ef66c-103">Hantera en webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ef66c-103">Manage a web app in Azure App Service</span></span>
<span data-ttu-id="ef66c-104">Det här avsnittet innehåller länkar till resurser för att hantera en webbapp i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="ef66c-104">This topic contains links to resources for managing a web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="ef66c-105">Hantering innehåller alla åtgärder som håller din webbapp körs utan problem.</span><span class="sxs-lookup"><span data-stu-id="ef66c-105">Management includes all of the tasks that keep your web app running smoothly.</span></span> 

<span data-ttu-id="ef66c-106">Över livslängden för en webbapp ska du utföra olika hanteringsuppgifter, när du flyttar från första distributionen till normal drift, underhåll och uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="ef66c-106">Over the lifetime of a web app, you will perform different management tasks, as you move from initial deployment to normal operation, maintenance, and updates.</span></span>

<span data-ttu-id="ef66c-107">Många aktiviteter för webb-app kan utföras i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="ef66c-107">Many web app management tasks can be performed in the Azure Portal.</span></span>

## <a name="before-you-deploy-your-web-app-to-production"></a><span data-ttu-id="ef66c-108">Innan du distribuerar ditt webbprogram till produktion</span><span class="sxs-lookup"><span data-stu-id="ef66c-108">Before you deploy your web app to production</span></span>
### <a name="choose-a-tier"></a><span data-ttu-id="ef66c-109">Välj en nivå</span><span class="sxs-lookup"><span data-stu-id="ef66c-109">Choose a tier</span></span>
<span data-ttu-id="ef66c-110">Azure Apptjänst finns med i fem nivåer: Frigör delade, Basic, Standard och Premium.</span><span class="sxs-lookup"><span data-stu-id="ef66c-110">Azure App Service is offered in five tiers: Free, Shared, Basic, Standard, and Premium.</span></span> <span data-ttu-id="ef66c-111">Information om funktionerna och priser för varje nivå finns [prisinformationen](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="ef66c-111">For information about the features and pricing for each tier, see [Pricing details](https://azure.microsoft.com/pricing/details/app-service/).</span></span> 

* <span data-ttu-id="ef66c-112">[Apptjänstplaner](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) kan du gruppera flera webbprogram under samma nivå.</span><span class="sxs-lookup"><span data-stu-id="ef66c-112">[App Service plans](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) let you group multiple web apps under the same tier.</span></span>
* <span data-ttu-id="ef66c-113">Du kan alltid [växla nivåer](web-sites-scale.md) när du har skapat ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ef66c-113">You can always [switch tiers](web-sites-scale.md) after you create your web app.</span></span>

### <a name="configuration"></a><span data-ttu-id="ef66c-114">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="ef66c-114">Configuration</span></span>
<span data-ttu-id="ef66c-115">Använd den [Azure Portal](https://portal.azure.com/) att ställa in olika konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="ef66c-115">Use the [Azure Portal](https://portal.azure.com/) to set various configuration options.</span></span> <span data-ttu-id="ef66c-116">Mer information finns i [konfigurera webbappar i Azure App Service](web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="ef66c-116">For details, see [Configure web apps in Azure App Service](web-sites-configure.md).</span></span> <span data-ttu-id="ef66c-117">Här är en snabb Checklista:</span><span class="sxs-lookup"><span data-stu-id="ef66c-117">Here is a quick checklist:</span></span>

* <span data-ttu-id="ef66c-118">Välj **runtime versioner** för .NET, PHP, Java och Python, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ef66c-118">Select **runtime versions** for .NET, PHP, Java, or Python, if needed.</span></span>
* <span data-ttu-id="ef66c-119">Aktivera **WebSockets** om ditt webbprogram använder WebSocket-protokollet.</span><span class="sxs-lookup"><span data-stu-id="ef66c-119">Enable **WebSockets** if your web app uses the WebSocket protocol.</span></span> <span data-ttu-id="ef66c-120">(Detta omfattar appar som använder [ASP.NET SignalR](http://www.asp.net/signalr) eller [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span><span class="sxs-lookup"><span data-stu-id="ef66c-120">(This includes apps that use [ASP.NET SignalR](http://www.asp.net/signalr) or [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span></span>
* <span data-ttu-id="ef66c-121">Kör du webbjobb?</span><span class="sxs-lookup"><span data-stu-id="ef66c-121">Are you running continuous web jobs?</span></span> <span data-ttu-id="ef66c-122">I så fall, aktivera **alltid på**.</span><span class="sxs-lookup"><span data-stu-id="ef66c-122">If so, enable **Always On**.</span></span>
* <span data-ttu-id="ef66c-123">Ange den **standarddokument**, till exempel index.html.</span><span class="sxs-lookup"><span data-stu-id="ef66c-123">Set the **default document**, such as index.html.</span></span>

<span data-ttu-id="ef66c-124">Förutom dessa inställningar för grundläggande konfiguration kan du vill konfigurera följande:</span><span class="sxs-lookup"><span data-stu-id="ef66c-124">In addition to these basic configuration settings, you may want to configure the following:</span></span>

* <span data-ttu-id="ef66c-125">**Secure Socket Layer (SSL)** kryptering.</span><span class="sxs-lookup"><span data-stu-id="ef66c-125">**Secure Socket Layer (SSL)** encryption.</span></span> <span data-ttu-id="ef66c-126">Om du vill använda SSL med ett anpassat domännamn, måste du få ett SSL-certifikat och konfigurerar ditt webbprogram för att använda den.</span><span class="sxs-lookup"><span data-stu-id="ef66c-126">To use SSL with a custom domain name, you must get an SSL certificate and configure your web app to use it.</span></span> <span data-ttu-id="ef66c-127">Se [aktivera HTTPS för en webbapp i Azure App Service](app-service-web-tutorial-custom-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="ef66c-127">See [Enable HTTPS for a web app in Azure App Service](app-service-web-tutorial-custom-ssl.md).</span></span>
* <span data-ttu-id="ef66c-128">**Anpassade domännamn.**</span><span class="sxs-lookup"><span data-stu-id="ef66c-128">**Custom domain name.**</span></span> <span data-ttu-id="ef66c-129">Ditt webbprogram har automatiskt en underdomän under azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="ef66c-129">Your web app automatically has a subdomain under azurewebsites.net.</span></span> <span data-ttu-id="ef66c-130">Du kan associera ett anpassat domännamn, t.ex contoso.com.</span><span class="sxs-lookup"><span data-stu-id="ef66c-130">You can associate a custom domain name, such as contoso.com.</span></span> <span data-ttu-id="ef66c-131">Se [konfigurera ett anpassat domännamn i Azure App Service](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="ef66c-131">See [Configure a custom domain name in Azure App Service](app-service-web-tutorial-custom-domain.md).</span></span>

<span data-ttu-id="ef66c-132">Språkspecifika konfiguration:</span><span class="sxs-lookup"><span data-stu-id="ef66c-132">Language-specific configuration:</span></span>

* <span data-ttu-id="ef66c-133">**PHP**: [konfigurerar du PHP i Azure App Service Web Apps](web-sites-php-configure.md).</span><span class="sxs-lookup"><span data-stu-id="ef66c-133">**PHP**: [Configure PHP in Azure App Service Web Apps](web-sites-php-configure.md).</span></span>
* <span data-ttu-id="ef66c-134">**Python**: [konfigurera Python med Azure App Service Web Apps](web-sites-python-configure.md)</span><span class="sxs-lookup"><span data-stu-id="ef66c-134">**Python**: [Configuring Python with Azure App Service Web Apps](web-sites-python-configure.md)</span></span>

## <a name="while-your-web-app-is-running"></a><span data-ttu-id="ef66c-135">När ditt webbprogram kör</span><span class="sxs-lookup"><span data-stu-id="ef66c-135">While your web app is running</span></span>
<span data-ttu-id="ef66c-136">När din webbapp körs, som du vill kontrollera att den är tillgänglig och att det skalas för att uppfylla användaraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="ef66c-136">While your web app is running, you want to make sure it is available, and that it scales to meet user traffic.</span></span> <span data-ttu-id="ef66c-137">Du kan också behöva felsöka.</span><span class="sxs-lookup"><span data-stu-id="ef66c-137">You may also need to troubleshoot errors.</span></span>

### <a name="monitoring"></a><span data-ttu-id="ef66c-138">Övervakning</span><span class="sxs-lookup"><span data-stu-id="ef66c-138">Monitoring</span></span>
* <span data-ttu-id="ef66c-139">Azure-portalen kan du [lägga till prestandamått](web-sites-monitor.md) , till exempel CPU-användning och antalet klientbegäranden.</span><span class="sxs-lookup"><span data-stu-id="ef66c-139">Through the Azure Portal, you can [add performance metrics](web-sites-monitor.md) such as CPU usage and number of client requests.</span></span>
* <span data-ttu-id="ef66c-140">[Skala ditt webbprogram](web-sites-scale.md) som svar på trafik.</span><span class="sxs-lookup"><span data-stu-id="ef66c-140">[Scale your web app](web-sites-scale.md) in response to traffic.</span></span> <span data-ttu-id="ef66c-141">Du kan skala antalet virtuella datorer och/eller storleken på VM-instanser beroende på din nivå.</span><span class="sxs-lookup"><span data-stu-id="ef66c-141">Depending on your tier, you can scale the number of VMs and/or the size of the VM instances.</span></span> <span data-ttu-id="ef66c-142">I Standard- och Premium-nivåer, kan du också ställa in autoskalning, så att ditt webbprogram automatiskt, skalar på ett fast schema eller på att läsa in.</span><span class="sxs-lookup"><span data-stu-id="ef66c-142">In the Standard and Premium tiers, you can also set up autoscaling, so your web app scales automatically, either on a fixed schedule, or in response to load.</span></span>  

### <a name="backups"></a><span data-ttu-id="ef66c-143">Säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="ef66c-143">Backups</span></span>
* <span data-ttu-id="ef66c-144">Ange [automatiska säkerhetskopieringar](web-sites-backup.md) av ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ef66c-144">Set [automatic backups](web-sites-backup.md) of your web app.</span></span> <span data-ttu-id="ef66c-145">Mer information om säkerhetskopior i [den här videon](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span><span class="sxs-lookup"><span data-stu-id="ef66c-145">Learn more about backups in [this video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span></span>
* <span data-ttu-id="ef66c-146">Lär dig mer om alternativen för [återställning av databas](../sql-database/sql-database-business-continuity.md) i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ef66c-146">Learn about the options for [database recovery](../sql-database/sql-database-business-continuity.md) in Azure SQL Database.</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="ef66c-147">Felsökning</span><span class="sxs-lookup"><span data-stu-id="ef66c-147">Troubleshooting</span></span>
* <span data-ttu-id="ef66c-148">Om något går fel kan du [felsökning i Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), med hjälp av diagnostiska loggar och live-felsökning i molnet.</span><span class="sxs-lookup"><span data-stu-id="ef66c-148">If something goes wrong, you can [troubleshoot in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), using diagnostic logs and live debugging in the cloud.</span></span> 
* <span data-ttu-id="ef66c-149">Det finns olika sätt att samla in diagnostikloggar utanför Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef66c-149">Outside of Visual Studio, there are various ways to collect diagnostic logs.</span></span> <span data-ttu-id="ef66c-150">Se [aktivera diagnostikloggning för web apps i Azure App Service](web-sites-enable-diagnostic-log.md).</span><span class="sxs-lookup"><span data-stu-id="ef66c-150">See [Enable diagnostics logging for web apps in Azure App Service](web-sites-enable-diagnostic-log.md).</span></span>
* <span data-ttu-id="ef66c-151">Node.js-program finns i [felsöka en Node.js-webbapp i Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="ef66c-151">For Node.js applications, see [How to debug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>

### <a name="restoring-data"></a><span data-ttu-id="ef66c-152">Återställa Data</span><span class="sxs-lookup"><span data-stu-id="ef66c-152">Restoring Data</span></span>
* <span data-ttu-id="ef66c-153">[Återställa](web-sites-restore.md) ett webbprogram som tidigare har säkerhetskopierats.</span><span class="sxs-lookup"><span data-stu-id="ef66c-153">[Restore](web-sites-restore.md) a web app that was previously backed up.</span></span>

## <a name="when-you-update-your-web-app"></a><span data-ttu-id="ef66c-154">När du uppdaterar ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="ef66c-154">When you update your web app</span></span>
<span data-ttu-id="ef66c-155">Om du inte har aktiverat automatiska säkerhetskopieringar kan du skapa en [manuell säkerhetskopiering](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="ef66c-155">If you have not enabled automatic backups, you can create a [manual backup](web-sites-backup.md).</span></span>

<span data-ttu-id="ef66c-156">Överväg att använda [stegvis distribution](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="ef66c-156">Consider using [staged deployment](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="ef66c-157">Det här alternativet kan du publicera uppdateringar till en fristående distribution som körs sida-vid-sida med din Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="ef66c-157">This option lets you publish updates to a staging deployment that runs side-by-side with your production deployment.</span></span> 


<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website


