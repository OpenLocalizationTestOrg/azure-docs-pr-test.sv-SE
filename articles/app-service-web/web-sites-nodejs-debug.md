---
title: "Felsöka en Node.js-webbapp i Azure Apptjänst"
description: "Lär dig att felsöka en Node.js-webbapp i Azure App Service."
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
ms.openlocfilehash: 5e302a4c58a171d40e43a22c34c724e868019ec8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a><span data-ttu-id="0eca5-103">Felsöka en Node.js-webbapp i Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="0eca5-103">How to debug a Node.js web app in Azure App Service</span></span>
<span data-ttu-id="0eca5-104">Azure tillhandahåller inbyggda diagnostik för att underlätta felsökning Node.js-program finns i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span><span class="sxs-lookup"><span data-stu-id="0eca5-104">Azure provides built-in diagnostics to assist with debugging Node.js applications hosted in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps.</span></span> <span data-ttu-id="0eca5-105">I den här artikeln får du lära dig hur visas information om fel i webbläsaren om du vill aktivera loggning av stdout och stderr och hur du hämtar och visa loggfiler.</span><span class="sxs-lookup"><span data-stu-id="0eca5-105">In this article, you will learn how to enable logging of stdout and stderr, display error information in the browser, and how to download and view log files.</span></span>

<span data-ttu-id="0eca5-106">Diagnostik för Node.js-program i Azure tillhandahålls av [IISNode].</span><span class="sxs-lookup"><span data-stu-id="0eca5-106">Diagnostics for Node.js applications hosted on Azure is provided by [IISNode].</span></span> <span data-ttu-id="0eca5-107">Den här artikeln beskrivs de vanligaste inställningarna för att samla in diagnostikinformation, innehåller det inte en fullständig referens för att arbeta med IISNode.</span><span class="sxs-lookup"><span data-stu-id="0eca5-107">While this article discusses the most common settings for gathering diagnostics information, it does not provide a complete reference for working with IISNode.</span></span> <span data-ttu-id="0eca5-108">Mer information om hur du arbetar med IISNode finns i [IISNode Readme] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="0eca5-108">For more information on working with IISNode, see the [IISNode Readme] on GitHub.</span></span>

<a id="enablelogging"></a>

## <a name="enable-logging"></a><span data-ttu-id="0eca5-109">Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="0eca5-109">Enable logging</span></span>
<span data-ttu-id="0eca5-110">Som standard sparas endast en App Service webbapp diagnostisk information om distributioner, till exempel när du distribuerar ett webbprogram med Git.</span><span class="sxs-lookup"><span data-stu-id="0eca5-110">By default, an App Service web app only captures diagnostic information about deployments, such as when you deploy a web app using Git.</span></span> <span data-ttu-id="0eca5-111">Den här informationen är användbar om du har problem under distributionen, till exempel ett fel när du installerar en modul som refereras i **package.json**, eller om du använder en anpassad distribution med skriptet.</span><span class="sxs-lookup"><span data-stu-id="0eca5-111">This information is useful if you are having problems during deployment, such as a failure when installing a module referenced in **package.json**, or if you are using a custom deployment script.</span></span>

<span data-ttu-id="0eca5-112">Om du vill aktivera loggning av stdout och stderr-strömmar, måste du skapa en **IISNode.yml** filen i roten på Node.js-programmet och Lägg till följande:</span><span class="sxs-lookup"><span data-stu-id="0eca5-112">To enable the logging of stdout and stderr streams, you must create an **IISNode.yml** file at the root of your Node.js application and add the following:</span></span>

    loggingEnabled: true

<span data-ttu-id="0eca5-113">Detta gör att loggning av stderr och stdout från Node.js-programmet.</span><span class="sxs-lookup"><span data-stu-id="0eca5-113">This enables the logging of stderr and stdout from your Node.js application.</span></span>

<span data-ttu-id="0eca5-114">Den **IISNode.yml** filen kan också användas för kontrollen om egna felmeddelanden eller utvecklare fel skickas till webbläsaren när ett fel uppstår.</span><span class="sxs-lookup"><span data-stu-id="0eca5-114">The **IISNode.yml** file can also be used to control whether friendly errors or developer errors are returned to the browser when a failure occurs.</span></span> <span data-ttu-id="0eca5-115">Om du vill aktivera developer fel, lägger du till följande rad i den **IISNode.yml** fil:</span><span class="sxs-lookup"><span data-stu-id="0eca5-115">To enable developer errors, add the following line to the **IISNode.yml** file:</span></span>

    devErrorsEnabled: true

<span data-ttu-id="0eca5-116">När detta alternativ är aktiverat returnerar IISNode informationen som skickas till stderr i stället för ett eget fel som ”ett internt serverfel uppstod” senaste 64K.</span><span class="sxs-lookup"><span data-stu-id="0eca5-116">Once this option is enabled, IISNode will return the last 64K of information sent to stderr instead of a friendly error such as "an internal server error occurred".</span></span>

> [!NOTE]
> <span data-ttu-id="0eca5-117">DevErrorsEnabled är användbart när diagnostisera problem under utveckling, kan aktivera den i en produktionsmiljö leda till utveckling fel skickas till slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="0eca5-117">While devErrorsEnabled is useful when diagnosing problems during development, enabling it in a production environment may result in development errors being sent to end users.</span></span>
> 
> 

<span data-ttu-id="0eca5-118">Om den **IISNode.yml** filen redan finns inte i ditt program, måste du starta om ditt webbprogram när du har publicerat programmet uppdaterade.</span><span class="sxs-lookup"><span data-stu-id="0eca5-118">If the **IISNode.yml** file did not already exist within your application, you must restart your web app after publishing the updated application.</span></span> <span data-ttu-id="0eca5-119">Om du bara ändrar inställningarna i en befintlig **IISNode.yml** -fil som tidigare har publicerats, ingen omstart krävs.</span><span class="sxs-lookup"><span data-stu-id="0eca5-119">If you are simply changing settings in an existing **IISNode.yml** file that has previously been published, no restart is required.</span></span>

> [!NOTE]
> <span data-ttu-id="0eca5-120">Om ditt webbprogram har skapats med hjälp av Azure-kommandoradsverktyg eller Azure PowerShell-Cmdlets, standard **IISNode.yml** fil skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="0eca5-120">If your web app was created using the Azure Command-Line Tools or Azure PowerShell Cmdlets, a default **IISNode.yml** file is automatically created.</span></span>
> 
> 

<span data-ttu-id="0eca5-121">Om du vill starta om webbprogrammet, Välj webbappen i den [Azure Portal](https://portal.azure.com), och klicka sedan på **starta om** knappen:</span><span class="sxs-lookup"><span data-stu-id="0eca5-121">To restart the web app, select the web app in the [Azure Portal](https://portal.azure.com), and then click **RESTART** button:</span></span>

![Starta om knappen][restart-button]

<span data-ttu-id="0eca5-123">Om Azure-kommandoradsverktyg är installerade i din utvecklingsmiljö använda du följande kommando för att starta om webbprogrammet:</span><span class="sxs-lookup"><span data-stu-id="0eca5-123">If the Azure Command-Line Tools are installed in your development environment, you can use the following command to restart the web app:</span></span>

    azure site restart [sitename]

> [!NOTE]
> <span data-ttu-id="0eca5-124">LoggingEnabled och devErrorsEnabled är de vanligaste IISNode.yml konfigurationsalternativ för att samla in diagnostikinformation, användas IISNode.yml för att konfigurera en mängd olika alternativ för din värdmiljö.</span><span class="sxs-lookup"><span data-stu-id="0eca5-124">While loggingEnabled and devErrorsEnabled are the most commonly used IISNode.yml configuration options for capturing diagnostic information, IISNode.yml can be used to configure a variety of options for your hosting environment.</span></span> <span data-ttu-id="0eca5-125">En fullständig lista över konfigurationsalternativ finns i [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) fil.</span><span class="sxs-lookup"><span data-stu-id="0eca5-125">For a full list of the configuration options, see the [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) file.</span></span>
> 
> 

<a id="viewlogs"></a>

## <a name="accessing-logs"></a><span data-ttu-id="0eca5-126">Åtkomst till loggar</span><span class="sxs-lookup"><span data-stu-id="0eca5-126">Accessing logs</span></span>
<span data-ttu-id="0eca5-127">Diagnostikloggar kan nås på tre sätt; Med den protokollet FTP (File Transfer), hämtar ett Zip-arkiv eller som en live uppdateras dataströmmen loggens (även kallat pilslut).</span><span class="sxs-lookup"><span data-stu-id="0eca5-127">Diagnostic logs can be accessed in three ways; Using the File Transfer Protocol (FTP), downloading a Zip archive, or as a live updated stream of the log (also known as a tail).</span></span> <span data-ttu-id="0eca5-128">Hämta Zip-arkivet loggfiler eller visa den direktsända dataströmmen kräver Azure-kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="0eca5-128">Downloading the Zip archive of the log files or viewing the live stream require the Azure Command-Line Tools.</span></span> <span data-ttu-id="0eca5-129">Dessa kan installeras med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0eca5-129">These can be installed by using the following command:</span></span>

    npm install azure-cli -g

<span data-ttu-id="0eca5-130">När du har installerat kan verktygen nås med hjälp av kommandot ”azure”.</span><span class="sxs-lookup"><span data-stu-id="0eca5-130">Once installed, the tools can be accessed using the 'azure' command.</span></span> <span data-ttu-id="0eca5-131">Kommandoradsverktygen måste först konfigureras för att använda din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0eca5-131">The command-line tools must first be configured to use your Azure subscription.</span></span> <span data-ttu-id="0eca5-132">Information om hur du gör detta finns i **hur du laddar ned och importera Publiceringsinställningar** avsnitt i den [så används den Azure-kommandoradsverktyg](../xplat-cli-connect.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0eca5-132">For information on how to accomplish this task, see the **How to download and import publish settings** section of the [How to Use The Azure Command-Line Tools](../xplat-cli-connect.md) article.</span></span>

### <a name="ftp"></a><span data-ttu-id="0eca5-133">FTP</span><span class="sxs-lookup"><span data-stu-id="0eca5-133">FTP</span></span>
<span data-ttu-id="0eca5-134">Om du vill komma åt diagnostisk information via FTP finns i [Azure-portalen](https://portal.azure.com), Välj ditt webbprogram och välj sedan den **INSTRUMENTPANELEN**.</span><span class="sxs-lookup"><span data-stu-id="0eca5-134">To access the diagnostic information through FTP, visit the [Azure Portal](https://portal.azure.com), select your web app, and then select the **DASHBOARD**.</span></span> <span data-ttu-id="0eca5-135">I den **snabblänkar** avsnittet den **FTP DIAGNOSTIKLOGGAR** och **FTPS DIAGNOSTIKLOGGAR** länkar ger åtkomst till loggar med hjälp av FTP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="0eca5-135">In the **quick links** section, the **FTP DIAGNOSTIC LOGS** and **FTPS DIAGNOSTIC LOGS** links provide access to the logs using the FTP protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="0eca5-136">Om du inte tidigare har konfigurerat användarnamn och lösenord för FTP- eller distributionen, kan du göra det från den **Quickstart** sidan för hantering genom att välja **konfigurerat distributionsuppgifter**.</span><span class="sxs-lookup"><span data-stu-id="0eca5-136">If you have not previously configured user name and password for FTP or deployment, you can do so from the **Quickstart** management page by selecting **Set up deployment credentials**.</span></span>
> 
> 

<span data-ttu-id="0eca5-137">FTP-URL som returneras i instrumentpanelen avser den **loggfiler** katalog som innehåller följande underordnade kataloger:</span><span class="sxs-lookup"><span data-stu-id="0eca5-137">The FTP URL returned in the dashboard is for the **LogFiles** directory, which will contain the following sub-directories:</span></span>

* <span data-ttu-id="0eca5-138">[Distributionsmetoden](web-sites-deploy.md) -om du använder en distributionsmetod som till exempel Git en katalog med samma namn skapas och innehåller information relaterad till distributioner.</span><span class="sxs-lookup"><span data-stu-id="0eca5-138">[Deployment Method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of the same name will be created and will contain information related to deployments.</span></span>
* <span data-ttu-id="0eca5-139">nodejs - Stdout och stderr information som hämtats från alla instanser av programmet (när loggingEnabled är true.)</span><span class="sxs-lookup"><span data-stu-id="0eca5-139">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="zip-archive"></a><span data-ttu-id="0eca5-140">ZIP-Arkiv</span><span class="sxs-lookup"><span data-stu-id="0eca5-140">Zip archive</span></span>
<span data-ttu-id="0eca5-141">Använd följande kommando från Azure-kommandoradsverktyg för att hämta ett Zip-arkiv diagnostiska loggar:</span><span class="sxs-lookup"><span data-stu-id="0eca5-141">To download a Zip archive of the diagnostic logs, use the following command from the Azure Command-Line Tools:</span></span>

    azure site log download [sitename]

<span data-ttu-id="0eca5-142">Detta kommer att ladda ned en **diagnostics.zip** i den aktuella katalogen.</span><span class="sxs-lookup"><span data-stu-id="0eca5-142">This will download a **diagnostics.zip** in the current directory.</span></span> <span data-ttu-id="0eca5-143">Det här arkivet innehåller följande katalogstruktur:</span><span class="sxs-lookup"><span data-stu-id="0eca5-143">This archive contains the following directory structure:</span></span>

* <span data-ttu-id="0eca5-144">distributioner – en logg över information om distribution av programmet</span><span class="sxs-lookup"><span data-stu-id="0eca5-144">deployments - A log of information about deployments of your application</span></span>
* <span data-ttu-id="0eca5-145">Loggfiler</span><span class="sxs-lookup"><span data-stu-id="0eca5-145">LogFiles</span></span>
  
  * <span data-ttu-id="0eca5-146">[Distributionsmetoden](web-sites-deploy.md) -om du använder en distributionsmetod som till exempel Git en katalog med samma namn skapas och innehåller information relaterad till distributioner.</span><span class="sxs-lookup"><span data-stu-id="0eca5-146">[Deployment method](web-sites-deploy.md) - If you use a deployment method such as Git, a directory of the same name will be created and will contain information related to deployments.</span></span>
  * <span data-ttu-id="0eca5-147">nodejs - Stdout och stderr information som hämtats från alla instanser av programmet (när loggingEnabled är true.)</span><span class="sxs-lookup"><span data-stu-id="0eca5-147">nodejs - Stdout and stderr information captured from all instances of your application (when loggingEnabled is true.)</span></span>

### <a name="live-stream-tail"></a><span data-ttu-id="0eca5-148">Direktsänd dataström (pilslut)</span><span class="sxs-lookup"><span data-stu-id="0eca5-148">Live stream (tail)</span></span>
<span data-ttu-id="0eca5-149">Använd följande kommando från Azure-kommandoradsverktyg för att visa en direktsänd dataström med diagnostisk logginformation:</span><span class="sxs-lookup"><span data-stu-id="0eca5-149">To view a live stream of diagnostic log information, use the following command from the Azure Command-Line Tools:</span></span>

    azure site log tail [sitename]

<span data-ttu-id="0eca5-150">Detta returnerar en dataström med logghändelser som uppdateras när de inträffar på servern.</span><span class="sxs-lookup"><span data-stu-id="0eca5-150">This will return a stream of log events that are updated as they occur on the server.</span></span> <span data-ttu-id="0eca5-151">Den här dataströmmen returnerar information om programdistribution samt stdout och stderr (när loggingEnabled är true.)</span><span class="sxs-lookup"><span data-stu-id="0eca5-151">This stream will return deployment information as well as stdout and stderr information (when loggingEnabled is true.)</span></span>

<a id="nextsteps"></a>

## <a name="next-steps"></a><span data-ttu-id="0eca5-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0eca5-152">Next Steps</span></span>
<span data-ttu-id="0eca5-153">I den här artikeln beskrivs hur du aktiverar och komma åt diagnostikinformationen för Azure.</span><span class="sxs-lookup"><span data-stu-id="0eca5-153">In this article you learned how to enable and access diagnostics information for Azure.</span></span> <span data-ttu-id="0eca5-154">Den här informationen är användbar i Förstå problem med ditt program, kan den peka på ett problem med en modul som du använder eller som versionen av Node.js som används av App Service Web Apps är annorlunda än den som används i din distributionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0eca5-154">While this information is useful in understanding problems that occur with your application, it may point to a problem with a module you are using or that the version of Node.js used by App Service Web Apps is different than the one used in your deployment environment.</span></span>

<span data-ttu-id="0eca5-155">Information arbetar tillsammans med moduler i Azure finns [med Node.js-moduler med Azure-program](../nodejs-use-node-modules-azure-apps.md).</span><span class="sxs-lookup"><span data-stu-id="0eca5-155">For information in working with modules on Azure, see [Using Node.js Modules with Azure Applications](../nodejs-use-node-modules-azure-apps.md).</span></span>

<span data-ttu-id="0eca5-156">Information om hur du anger en Node.js-version för ditt program finns i [ange en Node.js-version i ett Azure-program].</span><span class="sxs-lookup"><span data-stu-id="0eca5-156">For information on specifying a Node.js version for your application, see [Specifying a Node.js version in an Azure application].</span></span>

<span data-ttu-id="0eca5-157">Mer information finns också i [Node.js Developer Center](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="0eca5-157">For more information, see also the [Node.js Developer Center](/develop/nodejs/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="0eca5-158">Nyheter</span><span class="sxs-lookup"><span data-stu-id="0eca5-158">What's changed</span></span>
* <span data-ttu-id="0eca5-159">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="0eca5-159">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

> [!NOTE]
> <span data-ttu-id="0eca5-160">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="0eca5-160">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="0eca5-161">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="0eca5-161">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="0eca5-162">[IISNode]: https://github.com/tjanczuk/iisnode</span><span class="sxs-lookup"><span data-stu-id="0eca5-162">[IISNode]: https://github.com/tjanczuk/iisnode</span></span>
<span data-ttu-id="0eca5-163">[IISNode Readme]: https://github.com/tjanczuk/iisnode#readme</span><span class="sxs-lookup"><span data-stu-id="0eca5-163">[IISNode Readme]: https://github.com/tjanczuk/iisnode#readme</span></span>
[How to Use The Azure Command-Line Interface]:../cli-install-nodejs.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
<span data-ttu-id="0eca5-164">[ange en Node.js-version i ett Azure-program]: ../nodejs-specify-node-version-azure-apps.md</span><span class="sxs-lookup"><span data-stu-id="0eca5-164">[Specifying a Node.js version in an Azure application]: ../nodejs-specify-node-version-azure-apps.md</span></span>

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png

