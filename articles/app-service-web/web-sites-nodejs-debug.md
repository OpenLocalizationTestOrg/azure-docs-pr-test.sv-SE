---
title: aaaHow toodebug Node.js-webbapp i Azure App Service
description: "Lär dig hur toodebug en Node.js webbapp i Azure App Service."
tags: azure-portal
services: app-service\web
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: a48f906c-1a3e-43bc-ae84-7d2dde175b15
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 888ec5c3f92cfc3aeea4ea86005b9b6a0d1306ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodebug-a-nodejs-web-app-in-azure-app-service"></a><span data-ttu-id="6adef-103">Hur toodebug en Node.js webbapp i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6adef-103">How toodebug a Node.js web app in Azure App Service</span></span>
<span data-ttu-id="6adef-104">Azure tillhandahåller inbyggd diagnostik tooassist med felsökning Node.js-program finns i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span><span class="sxs-lookup"><span data-stu-id="6adef-104">Azure provides built-in diagnostics tooassist with debugging Node.js applications hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="6adef-105">I den här artikeln får du lära dig hur tooenable loggning av stdout och stderr, visa felinformation i hello webbläsare och hur toodownload och visa loggfiler.</span><span class="sxs-lookup"><span data-stu-id="6adef-105">In this article, you will learn how tooenable logging of stdout and stderr, display error information in hello browser, and how toodownload and view log files.</span></span>

<span data-ttu-id="6adef-106">Diagnostik för Node.js-program i Azure tillhandahålls av [IISNode].</span><span class="sxs-lookup"><span data-stu-id="6adef-106">Diagnostics for Node.js applications hosted on Azure is provided by [IISNode].</span></span> <span data-ttu-id="6adef-107">Den här artikeln beskrivs hello de vanligaste inställningarna för att samla in diagnostikinformation, innehåller det inte en fullständig referens för att arbeta med IISNode.</span><span class="sxs-lookup"><span data-stu-id="6adef-107">While this article discusses hello most common settings for gathering diagnostics information, it does not provide a complete reference for working with IISNode.</span></span> <span data-ttu-id="6adef-108">Mer information om hur du arbetar med IISNode finns hello [IISNode Readme] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="6adef-108">For more information on working with IISNode, see hello [IISNode Readme] on GitHub.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="6adef-109">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="6adef-109">Enable logging</span></span>
<span data-ttu-id="6adef-110">Som standard sparas endast en App Service webbapp diagnostisk information om distributioner, till exempel när du distribuerar ett webbprogram med Git.</span><span class="sxs-lookup"><span data-stu-id="6adef-110">By default, an App Service web app only captures diagnostic information about deployments, such as when you deploy a web app using Git.</span></span> <span data-ttu-id="6adef-111">Den här informationen är användbar om du har problem under distributionen, till exempel ett fel när du installerar en modul som refereras i **package.json**, eller om du använder en anpassad distribution med skriptet.</span><span class="sxs-lookup"><span data-stu-id="6adef-111">This information is useful if you are having problems during deployment, such as a failure when installing a module referenced in **package.json**, or if you are using a custom deployment script.</span></span>

<span data-ttu-id="6adef-112">tooenable hello loggning av stdout och stderr-strömmar, måste du skapa en **IISNode.yml** hello roten i Node.js-programmet och Lägg till hello följande:</span><span class="sxs-lookup"><span data-stu-id="6adef-112">tooenable hello logging of stdout and stderr streams, you must create an **IISNode.yml** file at hello root of your Node.js application and add hello following:</span></span>

    loggingEnabled: true

<span data-ttu-id="6adef-113">Detta gör att hello loggning av stderr och stdout från Node.js-programmet.</span><span class="sxs-lookup"><span data-stu-id="6adef-113">This enables hello logging of stderr and stdout from your Node.js application.</span></span>

<span data-ttu-id="6adef-114">Hej **IISNode.yml** filen kan även vara används toocontrol om egna felmeddelanden eller utvecklare fel returneras toohello webbläsare när ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="6adef-114">hello **IISNode.yml** file can also be used toocontrol whether friendly errors or developer errors are returned toohello browser when a failure occurs.</span></span> <span data-ttu-id="6adef-115">tooenable developer fel, Lägg till följande rad toohello hello **IISNode.yml** fil:</span><span class="sxs-lookup"><span data-stu-id="6adef-115">tooenable developer errors, add hello following line toohello **IISNode.yml** file:</span></span>

    devErrorsEnabled: true

<span data-ttu-id="6adef-116">När detta alternativ är aktiverat returnerar IISNode hello senaste 64 kB i informationen som skickas toostderr i stället för ett eget fel som ”ett internt serverfel uppstod”.</span><span class="sxs-lookup"><span data-stu-id="6adef-116">Once this option is enabled, IISNode will return hello last 64K of information sent toostderr instead of a friendly error such as "an internal server error occurred".</span></span>

> [!NOTE]
> <span data-ttu-id="6adef-117">DevErrorsEnabled är användbart när diagnostisera problem under utveckling, kan aktiverar du den i en produktionsmiljö leda till utveckling fel skickas tooend användare.</span><span class="sxs-lookup"><span data-stu-id="6adef-117">While devErrorsEnabled is useful when diagnosing problems during development, enabling it in a production environment may result in development errors being sent tooend users.</span></span>
> 
> 

<span data-ttu-id="6adef-118">Om hello **IISNode.yml** filen redan finns inte i ditt program, måste du starta om ditt webbprogram när du har publicerat programmet hello uppdateras.</span><span class="sxs-lookup"><span data-stu-id="6adef-118">If hello **IISNode.yml** file did not already exist within your application, you must restart your web app after publishing hello updated application.</span></span> <span data-ttu-id="6adef-119">Om du bara ändrar inställningarna i en befintlig **IISNode.yml** -fil som tidigare har publicerats, ingen omstart krävs.</span><span class="sxs-lookup"><span data-stu-id="6adef-119">If you are simply changing settings in an existing **IISNode.yml** file that has previously been published, no restart is required.</span></span>

> [!NOTE]
> <span data-ttu-id="6adef-120">Om ditt webbprogram har skapats med hello Azure-kommandoradsverktyg eller Azure PowerShell-cmdletar, standard **IISNode.yml** fil skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6adef-120">If your web app was created using hello Azure Command-Line Tools or Azure PowerShell Cmdlets, a default **IISNode.yml** file is automatically created.</span></span>
> 
> 

<span data-ttu-id="6adef-121">toorestart hello webbapp väljer hello webbprogram i hello [Azure Portal](https://portal.azure.com), och klicka sedan på **starta om** knappen:</span><span class="sxs-lookup"><span data-stu-id="6adef-121">toorestart hello web app, select hello web app in hello [Azure Portal](https://portal.azure.com), and then click **RESTART** button:</span></span>

![Starta om knappen][restart-button]

<span data-ttu-id="6adef-123">Kommandoradsverktyg för hello Azure är installerade i din utvecklingsmiljö, kan du använda hello efter kommandot toorestart hello-webbprogram:</span><span class="sxs-lookup"><span data-stu-id="6adef-123">If hello Azure Command-Line Tools are installed in your development environment, you can use hello following command toorestart hello web app:</span></span>

    azure site restart [sitename]

> [!NOTE]
> <span data-ttu-id="6adef-124">LoggingEnabled och devErrorsEnabled är de vanligaste hello IISNode.yml konfigurationsalternativ för att samla in diagnostikinformation, vara IISNode.yml används tooconfigure en mängd olika alternativ för din värdmiljö.</span><span class="sxs-lookup"><span data-stu-id="6adef-124">While loggingEnabled and devErrorsEnabled are hello most commonly used IISNode.yml configuration options for capturing diagnostic information, IISNode.yml can be used tooconfigure a variety of options for your hosting environment.</span></span> <span data-ttu-id="6adef-125">En fullständig lista över konfigurationsalternativ för hello finns hello [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) fil.</span><span class="sxs-lookup"><span data-stu-id="6adef-125">For a full list of hello configuration options, see hello [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) file.</span></span>
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a><span data-ttu-id="6adef-126">Åtkomst till loggar</span><span class="sxs-lookup"><span data-stu-id="6adef-126">Accessing logs</span></span>
<span data-ttu-id="6adef-127">Diagnostikloggar kan nås på tre sätt; Med hjälp av hello protokollet FTP (File Transfer), hämtar ett Zip-arkiv eller som en live uppdateras dataströmmen hello-loggen (även kallat pilslut).</span><span class="sxs-lookup"><span data-stu-id="6adef-127">Diagnostic logs can be accessed in three ways; Using hello File Transfer Protocol (FTP), downloading a Zip archive, or as a live updated stream of hello log (also known as a tail).</span></span> <span data-ttu-id="6adef-128">Hämta hello Zip-arkiv hello loggfiler eller visa hello direktsänd dataström kräver hello Azure-kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="6adef-128">Downloading hello Zip archive of hello log files or viewing hello live stream require hello Azure Command-Line Tools.</span></span> <span data-ttu-id="6adef-129">Dessa kan installeras med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6adef-129">These can be installed by using hello following command:</span></span>

    npm install azure-cli -g

<span data-ttu-id="6adef-130">När du har installerat kan hello verktyg kommas åt kommandot hello ”azure”.</span><span class="sxs-lookup"><span data-stu-id="6adef-130">Once installed, hello tools can be accessed using hello 'azure' command.</span></span> <span data-ttu-id="6adef-131">Hej kommandoradsverktyg måste först vara konfigurerade toouse din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6adef-131">hello command-line tools must first be configured toouse your Azure subscription.</span></span> <span data-ttu-id="6adef-132">Information om hur tooaccomplish detta uppgift finns hello **hur toodownload och importera Publiceringsinställningar** avsnitt i hello [hur tooUse hello Azure kommandoradsverktyg](../xplat-cli-connect.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6adef-132">For information on how tooaccomplish this task, see hello **How toodownload and import publish settings** section of hello [How tooUse hello Azure Command-Line Tools](../xplat-cli-connect.md) article.</span></span>

### <a name="ftp"></a><span data-ttu-id="6adef-133">FTP</span><span class="sxs-lookup"><span data-stu-id="6adef-133">FTP</span></span>
<span data-ttu-id="6adef-134">tooaccess hello diagnostisk information via FTP, besök hello [Azure Portal](https://portal.azure.com), Välj ditt webbprogram och väljer sedan hello **INSTRUMENTPANELEN**.</span><span class="sxs-lookup"><span data-stu-id="6adef-134">tooaccess hello diagnostic information through FTP, visit hello [Azure Portal](https://portal.azure.com), select your web app, and then select hello **DASHBOARD**.</span></span> <span data-ttu-id="6adef-135">I hello **snabblänkar** avsnittet hello **FTP DIAGNOSTIKLOGGAR** och **FTPS DIAGNOSTIKLOGGAR** länkar ger toohello åtkomstloggar med hello FTP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="6adef-135">In hello **quick links** section, hello **FTP DIAGNOSTIC LOGS** and **FTPS DIAGNOSTIC LOGS** links provide access toohello logs using hello FTP protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="6adef-136">Om du inte tidigare har konfigurerat användarnamn och lösenord för FTP- eller distributionen, kan du göra det från hello **Quickstart** sidan för hantering genom att välja **konfigurerat distributionsuppgifter**.</span><span class="sxs-lookup"><span data-stu-id="6adef-136">If you have not previously configured user name and password for FTP or deployment, you can do so from hello **Quickstart** management page by selecting **Set up deployment credentials**.</span></span>
> 
> 

<span data-ttu-id="6adef-137">hello FTP-URL som returneras i hello instrumentpanelen är för hello **loggfiler** katalog som ska innehålla hello efter underkataloger:</span><span class="sxs-lookup"><span data-stu-id="6adef-137">hello FTP URL returned in hello dashboard is for hello **LogFiles** directory, which will contain hello following sub-directories:</span></span>

* <span data-ttu-id="6adef-138">[Distributionsmetoden](web-sites-deploy.md) -om du använder en distributionsmetod som till exempel Git en katalog med hello samma namn skapas och innehåller information relaterad toodeployments.</span><span class="sxs-lookup"><span data-stu-id="6adef-138">[Deployment Method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of hello same name will be created and will contain information related toodeployments.</span></span>
* <span data-ttu-id="6adef-139">nodejs - Stdout och stderr information som hämtats från alla instanser av programmet (när loggingEnabled är true.)</span><span class="sxs-lookup"><span data-stu-id="6adef-139">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="zip-archive"></a><span data-ttu-id="6adef-140">ZIP-Arkiv</span><span class="sxs-lookup"><span data-stu-id="6adef-140">Zip archive</span></span>
<span data-ttu-id="6adef-141">toodownload ett Zip-arkiv hello diagnostiska loggar, Använd hello följande kommando från hello Azure kommandoradsverktyg:</span><span class="sxs-lookup"><span data-stu-id="6adef-141">toodownload a Zip archive of hello diagnostic logs, use hello following command from hello Azure Command-Line Tools:</span></span>

    azure site log download [sitename]

<span data-ttu-id="6adef-142">Detta kommer att ladda ned en **diagnostics.zip** i hello aktuella katalogen.</span><span class="sxs-lookup"><span data-stu-id="6adef-142">This will download a **diagnostics.zip** in hello current directory.</span></span> <span data-ttu-id="6adef-143">Det här arkivet innehåller hello följande katalogstruktur:</span><span class="sxs-lookup"><span data-stu-id="6adef-143">This archive contains hello following directory structure:</span></span>

* <span data-ttu-id="6adef-144">distributioner – en logg över information om distribution av programmet</span><span class="sxs-lookup"><span data-stu-id="6adef-144">deployments - A log of information about deployments of your application</span></span>
* <span data-ttu-id="6adef-145">Loggfiler</span><span class="sxs-lookup"><span data-stu-id="6adef-145">LogFiles</span></span>
  
  * <span data-ttu-id="6adef-146">[Distributionsmetoden](web-sites-deploy.md) -om du använder en distributionsmetod som till exempel Git en katalog med hello samma namn skapas och innehåller information relaterad toodeployments.</span><span class="sxs-lookup"><span data-stu-id="6adef-146">[Deployment method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of hello same name will be created and will contain information related toodeployments.</span></span>
  * <span data-ttu-id="6adef-147">nodejs - Stdout och stderr information som hämtats från alla instanser av programmet (när loggingEnabled är true.)</span><span class="sxs-lookup"><span data-stu-id="6adef-147">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="live-stream-tail"></a><span data-ttu-id="6adef-148">Direktsänd dataström (pilslut)</span><span class="sxs-lookup"><span data-stu-id="6adef-148">Live stream (tail)</span></span>
<span data-ttu-id="6adef-149">tooview en direktsänd dataström av diagnostiska logginformation, Använd hello följande kommando från hello Azure kommandoradsverktyg:</span><span class="sxs-lookup"><span data-stu-id="6adef-149">tooview a live stream of diagnostic log information, use hello following command from hello Azure Command-Line Tools:</span></span>

    azure site log tail [sitename]

<span data-ttu-id="6adef-150">Detta returnerar en dataström med logghändelser som uppdateras allteftersom de sker på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="6adef-150">This will return a stream of log events that are updated as they occur on hello server.</span></span> <span data-ttu-id="6adef-151">Den här dataströmmen returnerar information om programdistribution samt stdout och stderr (när loggingEnabled är true.)</span><span class="sxs-lookup"><span data-stu-id="6adef-151">This stream will return deployment information as well as stdout and stderr information (when loggingEnabled is true.)</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="6adef-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6adef-152">Next Steps</span></span>
<span data-ttu-id="6adef-153">I denna artikel du lärt dig hur tooenable och åtkomst diagnostikinformationen för Azure.</span><span class="sxs-lookup"><span data-stu-id="6adef-153">In this article you learned how tooenable and access diagnostics information for Azure.</span></span> <span data-ttu-id="6adef-154">När den här informationen är användbar Förstå problem med ditt program, kan den peka tooa problem med en modul som du använder eller som hello-versionen av Node.js som används av App Service Web Apps skiljer sig från hello som används i distributionen miljö.</span><span class="sxs-lookup"><span data-stu-id="6adef-154">While this information is useful in understanding problems that occur with your application, it may point tooa problem with a module you are using or that hello version of Node.js used by App Service Web Apps is different than hello one used in your deployment environment.</span></span>

<span data-ttu-id="6adef-155">Information arbetar tillsammans med moduler i Azure finns [med Node.js-moduler med Azure-program](../nodejs-use-node-modules-azure-apps.md).</span><span class="sxs-lookup"><span data-stu-id="6adef-155">For information in working with modules on Azure, see [Using Node.js Modules with Azure Applications](../nodejs-use-node-modules-azure-apps.md).</span></span>

<span data-ttu-id="6adef-156">Information om hur du anger en Node.js-version för ditt program finns i [ange en Node.js-version i ett Azure-program].</span><span class="sxs-lookup"><span data-stu-id="6adef-156">For information on specifying a Node.js version for your application, see [Specifying a Node.js version in an Azure application].</span></span>

<span data-ttu-id="6adef-157">Mer information finns också hello [Node.js Developer Center](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="6adef-157">For more information, see also hello [Node.js Developer Center](/develop/nodejs/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="6adef-158">Nyheter</span><span class="sxs-lookup"><span data-stu-id="6adef-158">What's changed</span></span>
* <span data-ttu-id="6adef-159">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="6adef-159">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="6adef-160">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="6adef-160">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="6adef-161">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="6adef-161">No credit cards required; no commitments.</span></span>
> 
> 

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode Readme]: https://github.com/tjanczuk/iisnode#readme
[How tooUse hello Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[ange en Node.js-version i ett Azure-program]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

