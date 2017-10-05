---
title: Konfigurera webbappar i Azure App Service
description: Hur du konfigurerar en webbapp i Azure App Service
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: cacbcf879555907f81d824dc1069b05579dca010
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-web-apps-in-azure-app-service"></a><span data-ttu-id="897ff-103">Konfigurera webbappar i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="897ff-103">Configure web apps in Azure App Service</span></span>
<span data-ttu-id="897ff-104">Det här avsnittet beskriver hur du konfigurerar en web app med hjälp av den [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="897ff-104">This topic explains how to configure a web app using the [Azure Portal].</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a><span data-ttu-id="897ff-105">Programinställningar</span><span class="sxs-lookup"><span data-stu-id="897ff-105">Application settings</span></span>
1. <span data-ttu-id="897ff-106">I den [Azure Portal], öppna bladet för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="897ff-106">In the [Azure Portal], open the blade for the web app.</span></span>
2. <span data-ttu-id="897ff-107">Klicka på **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="897ff-107">Click **All Settings**.</span></span>
3. <span data-ttu-id="897ff-108">Klicka på **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="897ff-108">Click **Application Settings**.</span></span>

![Programinställningar][configure01]

<span data-ttu-id="897ff-110">Den **programinställningar** bladet har inställningar som är grupperade under flera kategorier.</span><span class="sxs-lookup"><span data-stu-id="897ff-110">The **Application settings** blade has settings grouped under several categories.</span></span>

### <a name="general-settings"></a><span data-ttu-id="897ff-111">Allmänna inställningar</span><span class="sxs-lookup"><span data-stu-id="897ff-111">General settings</span></span>
<span data-ttu-id="897ff-112">**Versioner av Framework**.</span><span class="sxs-lookup"><span data-stu-id="897ff-112">**Framework versions**.</span></span> <span data-ttu-id="897ff-113">Ange dessa alternativ om din app använder alla dessa ramverk:</span><span class="sxs-lookup"><span data-stu-id="897ff-113">Set these options if your app uses any these frameworks:</span></span> 

* <span data-ttu-id="897ff-114">**.NET framework**: Ange .NET framework-version.</span><span class="sxs-lookup"><span data-stu-id="897ff-114">**.NET Framework**: Set the .NET framework version.</span></span> 
* <span data-ttu-id="897ff-115">**PHP**: Ange PHP-version eller **OFF** att inaktivera PHP.</span><span class="sxs-lookup"><span data-stu-id="897ff-115">**PHP**: Set the PHP version, or **OFF** to disable PHP.</span></span> 
* <span data-ttu-id="897ff-116">**Java**: Välj den Java-versionen eller **OFF** att inaktivera Java.</span><span class="sxs-lookup"><span data-stu-id="897ff-116">**Java**: Select the Java version or **OFF** to disable Java.</span></span> <span data-ttu-id="897ff-117">Använd den **Webbehållaren** möjlighet att välja mellan Tomcat och Jetty-versioner.</span><span class="sxs-lookup"><span data-stu-id="897ff-117">Use the **Web Container** option to choose between Tomcat and Jetty versions.</span></span>
* <span data-ttu-id="897ff-118">**Python**: Välj Python-version eller **OFF** att inaktivera Python.</span><span class="sxs-lookup"><span data-stu-id="897ff-118">**Python**: Select the Python version, or **OFF** to disable Python.</span></span>

<span data-ttu-id="897ff-119">Av tekniska skäl inaktiveras när du aktiverar Java för din app alternativen för .NET, PHP och Python.</span><span class="sxs-lookup"><span data-stu-id="897ff-119">For technical reasons, enabling Java for your app disables the .NET, PHP, and Python options.</span></span>

<span data-ttu-id="897ff-120"><a name="platform"></a>
**Plattform**.</span><span class="sxs-lookup"><span data-stu-id="897ff-120"><a name="platform"></a>
**Platform**.</span></span> <span data-ttu-id="897ff-121">Anger om ditt webbprogram kör i en 32-bitars eller 64-bitars miljö.</span><span class="sxs-lookup"><span data-stu-id="897ff-121">Selects whether your web app runs in a 32-bit or 64-bit environment.</span></span> <span data-ttu-id="897ff-122">64-bitars miljö kräver Basic eller Standard-läge.</span><span class="sxs-lookup"><span data-stu-id="897ff-122">The 64-bit environment requires Basic or Standard mode.</span></span> <span data-ttu-id="897ff-123">Frigör och delade lägen köras alltid i en 32-bitars-miljö.</span><span class="sxs-lookup"><span data-stu-id="897ff-123">Free and Shared modes always run in a 32-bit environment.</span></span>

<span data-ttu-id="897ff-124">**Web Sockets**.</span><span class="sxs-lookup"><span data-stu-id="897ff-124">**Web Sockets**.</span></span> <span data-ttu-id="897ff-125">Ange **ON** att WebSocket-protokollet, till exempel om ditt webbprogram använder [ASP.NET SignalR] eller [socket.io].</span><span class="sxs-lookup"><span data-stu-id="897ff-125">Set **ON** to enable the WebSocket protocol; for example, if your web app uses [ASP.NET SignalR] or [socket.io].</span></span>

<span data-ttu-id="897ff-126"><a name="alwayson"></a>
**Always On**.</span><span class="sxs-lookup"><span data-stu-id="897ff-126"><a name="alwayson"></a>
**Always On**.</span></span> <span data-ttu-id="897ff-127">Som standard inaktiveras webbappar om de är inaktiv under en tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="897ff-127">By default, web apps are unloaded if they are idle for some period of time.</span></span> <span data-ttu-id="897ff-128">Detta gör att systemet spara resurser.</span><span class="sxs-lookup"><span data-stu-id="897ff-128">This lets the system conserve resources.</span></span> <span data-ttu-id="897ff-129">I Basic eller Standard-läge kan du aktivera **alltid på** att appen att läsa in hela tiden.</span><span class="sxs-lookup"><span data-stu-id="897ff-129">In Basic or Standard mode, you can enable **Always On** to keep the app loaded all the time.</span></span> <span data-ttu-id="897ff-130">Om din app körs kontinuerliga Webbjobb eller körs WebJobs aktiveras med hjälp av ett CRON-uttryck, bör du aktivera **alltid på**, eller web-jobb körs inte på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="897ff-130">If your app runs continuous WebJobs or runs WebJobs triggered using a CRON expression, you should enable **Always On**, or the web jobs may not run reliably.</span></span>

<span data-ttu-id="897ff-131">**Hanterat Pipeline-Version**.</span><span class="sxs-lookup"><span data-stu-id="897ff-131">**Managed Pipeline Version**.</span></span> <span data-ttu-id="897ff-132">Anger IIS [pipelineläge].</span><span class="sxs-lookup"><span data-stu-id="897ff-132">Sets the IIS [pipeline mode].</span></span> <span data-ttu-id="897ff-133">Låt den här uppsättningen integrerad (standard) om du inte har en äldre app som kräver en äldre version av IIS.</span><span class="sxs-lookup"><span data-stu-id="897ff-133">Leave this set to Integrated (the default) unless you have a legacy app that requires an older version of IIS.</span></span>

<span data-ttu-id="897ff-134">**Automatisk växling**.</span><span class="sxs-lookup"><span data-stu-id="897ff-134">**Auto Swap**.</span></span> <span data-ttu-id="897ff-135">Om du aktiverar automatisk växling för en distributionsplats Apptjänst automatiskt att växla som webbappen till produktionen när du trycker på en uppdatering till platsen.</span><span class="sxs-lookup"><span data-stu-id="897ff-135">If you enable Auto Swap for a deployment slot, App Service will automatically swap the web app into production when you push an update to that slot.</span></span> <span data-ttu-id="897ff-136">Mer information finns i [till mellanlagring fack för web apps i Azure App Service](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="897ff-136">For more information, see [Deploy to staging slots for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span>

### <a name="debugging"></a><span data-ttu-id="897ff-137">Felsökning</span><span class="sxs-lookup"><span data-stu-id="897ff-137">Debugging</span></span>
<span data-ttu-id="897ff-138">**Fjärrfelsökning**.</span><span class="sxs-lookup"><span data-stu-id="897ff-138">**Remote Debugging**.</span></span> <span data-ttu-id="897ff-139">Aktiverar fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="897ff-139">Enables remote debugging.</span></span> <span data-ttu-id="897ff-140">När aktiverat, kan du använda fjärråtkomst felsökaren i Visual Studio för att ansluta direkt till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="897ff-140">When enabled, you can use the remote debugger in Visual Studio to connect directly to your web app.</span></span> <span data-ttu-id="897ff-141">Fjärrfelsökning förblir aktiverat för 48 timmar.</span><span class="sxs-lookup"><span data-stu-id="897ff-141">Remote debugging will remain enabled for 48 hours.</span></span> 

### <a name="app-settings"></a><span data-ttu-id="897ff-142">App-inställningar</span><span class="sxs-lookup"><span data-stu-id="897ff-142">App settings</span></span>
<span data-ttu-id="897ff-143">Det här avsnittet innehåller namn/värde-par som du web app läser in vid start.</span><span class="sxs-lookup"><span data-stu-id="897ff-143">This section contains name/value pairs that you web app will load on start up.</span></span> 

* <span data-ttu-id="897ff-144">För .NET-appar de här inställningarna är injekteras i konfigurationen av .NET `AppSettings` vid körning kan åsidosätta befintliga inställningar.</span><span class="sxs-lookup"><span data-stu-id="897ff-144">For .NET apps, these settings are injected into your .NET configuration `AppSettings` at runtime, overriding existing settings.</span></span> 
* <span data-ttu-id="897ff-145">PHP, Python, Java och noden program kan komma åt de här inställningarna som miljövariabler vid körning.</span><span class="sxs-lookup"><span data-stu-id="897ff-145">PHP, Python, Java and Node applications can access these settings as environment variables at runtime.</span></span> <span data-ttu-id="897ff-146">Två miljövariabler som skapas för varje appinställning. en med namnet som anges i inställningen som appen och en annan med prefixet APPSETTING_.</span><span class="sxs-lookup"><span data-stu-id="897ff-146">For each app setting, two environment variables are created; one with the name specified by the app setting entry, and another with a prefix of APPSETTING_.</span></span> <span data-ttu-id="897ff-147">Båda innehåller samma värde.</span><span class="sxs-lookup"><span data-stu-id="897ff-147">Both contain the same value.</span></span>

### <a name="connection-strings"></a><span data-ttu-id="897ff-148">Anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="897ff-148">Connection strings</span></span>
<span data-ttu-id="897ff-149">Anslutningssträngar för länkade resurser.</span><span class="sxs-lookup"><span data-stu-id="897ff-149">Connection strings for linked resources.</span></span> 

<span data-ttu-id="897ff-150">För .NET-appar har dessa anslutningssträngar injekteras i konfigurationen av .NET `connectionStrings` inställningar vid körning kan åsidosätta befintliga poster där nyckeln är lika med länkade databasnamnet.</span><span class="sxs-lookup"><span data-stu-id="897ff-150">For .NET apps, these connection strings are injected into your .NET configuration `connectionStrings` settings at runtime, overriding existing entries where the key equals the linked database name.</span></span> 

<span data-ttu-id="897ff-151">PHP, Python, Java och noden program, ska inställningarna vara tillgänglig som miljövariabler vid körning, föregås av anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="897ff-151">For PHP, Python, Java and Node applications, these settings will be available as environment variables at runtime, prefixed with the connection type.</span></span> <span data-ttu-id="897ff-152">Variabeln prefix miljö är följande:</span><span class="sxs-lookup"><span data-stu-id="897ff-152">The environment variable prefixes are as follows:</span></span> 

* <span data-ttu-id="897ff-153">SQLServer:`SQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="897ff-153">SQL Server: `SQLCONNSTR_`</span></span>
* <span data-ttu-id="897ff-154">MySQL:`MYSQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="897ff-154">MySQL: `MYSQLCONNSTR_`</span></span>
* <span data-ttu-id="897ff-155">SQL-databas:`SQLAZURECONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="897ff-155">SQL Database: `SQLAZURECONNSTR_`</span></span>
* <span data-ttu-id="897ff-156">Anpassad:`CUSTOMCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="897ff-156">Custom: `CUSTOMCONNSTR_`</span></span>

<span data-ttu-id="897ff-157">Om exempelvis en MySql-anslutningssträng vid namn `connectionstring1`, den kan nås via miljövariabeln `MYSQLCONNSTR_connectionString1`.</span><span class="sxs-lookup"><span data-stu-id="897ff-157">For example, if a MySql connection string were named `connectionstring1`, it would be accessed through the environment variable `MYSQLCONNSTR_connectionString1`.</span></span>

### <a name="default-documents"></a><span data-ttu-id="897ff-158">Standarddokument</span><span class="sxs-lookup"><span data-stu-id="897ff-158">Default documents</span></span>
<span data-ttu-id="897ff-159">Standarddokumentet är webbsidan som visas på rot-URL för en webbplats.</span><span class="sxs-lookup"><span data-stu-id="897ff-159">The default document is the web page that is displayed at the root URL for a website.</span></span>  <span data-ttu-id="897ff-160">Den första matchande filen i listan används.</span><span class="sxs-lookup"><span data-stu-id="897ff-160">The first matching file in the list is used.</span></span> 

<span data-ttu-id="897ff-161">Webbprogram kan använda moduler att vägen baserat på URL: en i stället för betjänar statiskt innehåll, då det finns inga standarddokument som sådana.</span><span class="sxs-lookup"><span data-stu-id="897ff-161">Web apps might use modules that route based on URL, rather than serving static content, in which case there is no default document as such.</span></span>    

### <a name="handler-mappings"></a><span data-ttu-id="897ff-162">Hanterarmappningar</span><span class="sxs-lookup"><span data-stu-id="897ff-162">Handler mappings</span></span>
<span data-ttu-id="897ff-163">Använd det här området för att lägga till processorer för anpassat skript för att hantera begäranden för specifika filtillägg.</span><span class="sxs-lookup"><span data-stu-id="897ff-163">Use this area to add custom script processors to handle requests for specific file extensions.</span></span> 

* <span data-ttu-id="897ff-164">**Tillägget**.</span><span class="sxs-lookup"><span data-stu-id="897ff-164">**Extension**.</span></span> <span data-ttu-id="897ff-165">Filtillägg som ska hanteras, till exempel *.php eller handler.fcgi.</span><span class="sxs-lookup"><span data-stu-id="897ff-165">The file extension to be handled, such as *.php or handler.fcgi.</span></span> 
* <span data-ttu-id="897ff-166">**Script Processor sökvägen**.</span><span class="sxs-lookup"><span data-stu-id="897ff-166">**Script Processor Path**.</span></span> <span data-ttu-id="897ff-167">Den absoluta sökvägen till Skriptprocessor.</span><span class="sxs-lookup"><span data-stu-id="897ff-167">The absolute path of the script processor.</span></span> <span data-ttu-id="897ff-168">Begäranden om filer som matchar filnamnstillägget kommer att behandlas av Skriptprocessor för.</span><span class="sxs-lookup"><span data-stu-id="897ff-168">Requests to files that match the file extension will be processed by the script processor.</span></span> <span data-ttu-id="897ff-169">Använd sökvägen med `D:\home\site\wwwroot` att referera till rotkatalogen för din app.</span><span class="sxs-lookup"><span data-stu-id="897ff-169">Use the path `D:\home\site\wwwroot` to refer to your app's root directory.</span></span>
* <span data-ttu-id="897ff-170">**Ytterligare argument**.</span><span class="sxs-lookup"><span data-stu-id="897ff-170">**Additional Arguments**.</span></span> <span data-ttu-id="897ff-171">Valfria kommandoradsargument för Skriptprocessor</span><span class="sxs-lookup"><span data-stu-id="897ff-171">Optional command-line arguments for the script processor</span></span> 

### <a name="virtual-applications-and-directories"></a><span data-ttu-id="897ff-172">Virtuella program och kataloger</span><span class="sxs-lookup"><span data-stu-id="897ff-172">Virtual applications and directories</span></span>
<span data-ttu-id="897ff-173">Ange varje virtuell katalog och dess motsvarande fysiska sökväg i förhållande till webbplatsroten för att konfigurera virtuella program och kataloger.</span><span class="sxs-lookup"><span data-stu-id="897ff-173">To configure virtual applications and directories, specify each virtual directory and its corresponding physical path relative to the website root.</span></span> <span data-ttu-id="897ff-174">Du kan också markera den **programmet** kryssrutan för att markera en virtuell katalog som ett program.</span><span class="sxs-lookup"><span data-stu-id="897ff-174">Optionally, you can select the **Application** checkbox to mark a virtual directory as an application.</span></span>

## <a name="enabling-diagnostic-logs"></a><span data-ttu-id="897ff-175">Aktivera diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="897ff-175">Enabling diagnostic logs</span></span>
<span data-ttu-id="897ff-176">Aktivera diagnostikloggar:</span><span class="sxs-lookup"><span data-stu-id="897ff-176">To enable diagnostic logs:</span></span>

1. <span data-ttu-id="897ff-177">I bladet för din webbapp klickar du på **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="897ff-177">In the blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="897ff-178">Klicka på **diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="897ff-178">Click **Diagnostic logs**.</span></span> 

<span data-ttu-id="897ff-179">Alternativ för att skriva diagnostiska loggar från webbprogram som har stöd för loggning:</span><span class="sxs-lookup"><span data-stu-id="897ff-179">Options for writing diagnostic logs from a web application that supports logging:</span></span> 

* <span data-ttu-id="897ff-180">**Programloggning**.</span><span class="sxs-lookup"><span data-stu-id="897ff-180">**Application Logging**.</span></span> <span data-ttu-id="897ff-181">Skriver programloggar till filsystemet.</span><span class="sxs-lookup"><span data-stu-id="897ff-181">Writes application logs to the file system.</span></span> <span data-ttu-id="897ff-182">Loggning varar under en period på 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="897ff-182">Logging lasts for a period of 12 hours.</span></span> 

<span data-ttu-id="897ff-183">**Nivå**.</span><span class="sxs-lookup"><span data-stu-id="897ff-183">**Level**.</span></span> <span data-ttu-id="897ff-184">När programmet loggning är aktiverat anger det här alternativet mängden information som ska registreras (fel, varning, Information eller utförlig).</span><span class="sxs-lookup"><span data-stu-id="897ff-184">When application logging is enabled, this option specifies the amount of information that will be recorded (Error, Warning, Information, or Verbose).</span></span>

<span data-ttu-id="897ff-185">**Web server-loggning**.</span><span class="sxs-lookup"><span data-stu-id="897ff-185">**Web server logging**.</span></span> <span data-ttu-id="897ff-186">Loggar sparas i W3C utökat loggfilsformat.</span><span class="sxs-lookup"><span data-stu-id="897ff-186">Logs are saved in the W3C extended log file format.</span></span> 

<span data-ttu-id="897ff-187">**Detaljerade felmeddelanden**.</span><span class="sxs-lookup"><span data-stu-id="897ff-187">**Detailed error messages**.</span></span> <span data-ttu-id="897ff-188">Sparar felmeddelanden detaljerade .htm-filer.</span><span class="sxs-lookup"><span data-stu-id="897ff-188">Saves detailed error messages .htm files.</span></span> <span data-ttu-id="897ff-189">Filerna sparas under /LogFiles/DetailedErrors.</span><span class="sxs-lookup"><span data-stu-id="897ff-189">The files are saved under /LogFiles/DetailedErrors.</span></span> 

<span data-ttu-id="897ff-190">**Spårning av misslyckade begäranden**.</span><span class="sxs-lookup"><span data-stu-id="897ff-190">**Failed request tracing**.</span></span> <span data-ttu-id="897ff-191">Loggar misslyckade förfrågningar till XML-filer.</span><span class="sxs-lookup"><span data-stu-id="897ff-191">Logs failed requests to XML files.</span></span> <span data-ttu-id="897ff-192">Filerna sparas under loggfilerna/W3SVC*xxx*, där xxx är en unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="897ff-192">The files are saved under /LogFiles/W3SVC*xxx*, where xxx is a unique identifier.</span></span> <span data-ttu-id="897ff-193">Den här mappen innehåller en XSL-fil och en eller flera XML-filer.</span><span class="sxs-lookup"><span data-stu-id="897ff-193">This folder contains an XSL file and one or more XML files.</span></span> <span data-ttu-id="897ff-194">Se till att hämta XSL-filen eftersom det ger funktioner för att formatera och filtrera innehållet i XML-filerna.</span><span class="sxs-lookup"><span data-stu-id="897ff-194">Make sure to download the XSL file, because it provides functionality for formatting and filtering the contents of the XML files.</span></span>

<span data-ttu-id="897ff-195">Om du vill visa loggfilerna måste du skapa FTP-autentiseringsuppgifter, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="897ff-195">To view the log files, you must create FTP credentials, as follows:</span></span>

1. <span data-ttu-id="897ff-196">I bladet för din webbapp klickar du på **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="897ff-196">In the blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="897ff-197">Klicka på **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="897ff-197">Click **Deployment credentials**.</span></span>
3. <span data-ttu-id="897ff-198">Ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="897ff-198">Enter a user name and password.</span></span>
4. <span data-ttu-id="897ff-199">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="897ff-199">Click **Save**.</span></span>

![Ange autentiseringsuppgifter för distribution][configure03]

<span data-ttu-id="897ff-201">Det fullständiga namnet för FTP-användare är ”app\username” där *app* är namnet på ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="897ff-201">The full FTP user name is “app\username” where *app* is the name of your web app.</span></span> <span data-ttu-id="897ff-202">Användarnamnet visas i bladet webbapp, under **Essentials**.</span><span class="sxs-lookup"><span data-stu-id="897ff-202">The username is listed in the web app blade, under **Essentials**.</span></span>  

![Autentiseringsuppgifter för FTP-distribution][configure02]

## <a name="other-configuration-tasks"></a><span data-ttu-id="897ff-204">Andra konfigurationsuppgifter</span><span class="sxs-lookup"><span data-stu-id="897ff-204">Other configuration tasks</span></span>
### <a name="ssl"></a><span data-ttu-id="897ff-205">SSL</span><span class="sxs-lookup"><span data-stu-id="897ff-205">SSL</span></span>
<span data-ttu-id="897ff-206">Du kan överföra SSL-certifikat för en anpassad domän i Basic eller Standard-läge.</span><span class="sxs-lookup"><span data-stu-id="897ff-206">In Basic or Standard mode, you can upload SSL certificates for a custom domain.</span></span> <span data-ttu-id="897ff-207">Mer information finns i [aktivera HTTPS för en webbapp].</span><span class="sxs-lookup"><span data-stu-id="897ff-207">For more information, see [Enable HTTPS for a web app].</span></span> 

<span data-ttu-id="897ff-208">Om du vill visa dina överförda certifikat klickar du på **alla inställningar** > **anpassade domäner och SSL**.</span><span class="sxs-lookup"><span data-stu-id="897ff-208">To view your uploaded certificates, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="domain-names"></a><span data-ttu-id="897ff-209">Domännamn</span><span class="sxs-lookup"><span data-stu-id="897ff-209">Domain names</span></span>
<span data-ttu-id="897ff-210">Lägg till anpassade domännamn för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="897ff-210">Add custom domain names for your web app.</span></span> <span data-ttu-id="897ff-211">Mer information finns i [konfigurera ett anpassat domännamn för en webbapp i Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="897ff-211">For more information, see [Configure a custom domain name for a web app in Azure App Service].</span></span>

<span data-ttu-id="897ff-212">Om du vill visa dina domännamn, klickar du på **alla inställningar** > **anpassade domäner och SSL**.</span><span class="sxs-lookup"><span data-stu-id="897ff-212">To view your domain names, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="deployments"></a><span data-ttu-id="897ff-213">Distributioner</span><span class="sxs-lookup"><span data-stu-id="897ff-213">Deployments</span></span>
* <span data-ttu-id="897ff-214">Ställ in kontinuerlig distribution.</span><span class="sxs-lookup"><span data-stu-id="897ff-214">Set up continuous deployment.</span></span> <span data-ttu-id="897ff-215">Se [med Git för att distribuera Webbappar i Azure App Service](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="897ff-215">See [Using Git to deploy Web Apps in Azure App Service](web-sites-deploy.md).</span></span>
* <span data-ttu-id="897ff-216">Distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="897ff-216">Deployment slots.</span></span> <span data-ttu-id="897ff-217">Se [distribuera till Mellanlagringsmiljöer för Web Apps i Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="897ff-217">See [Deploy to Staging Environments for Web Apps in Azure App Service].</span></span>

<span data-ttu-id="897ff-218">Om du vill visa dina distributionsplatser, klickar du på **alla inställningar** > **distributionsfack**.</span><span class="sxs-lookup"><span data-stu-id="897ff-218">To view your deployment slots, click **All Settings** > **Deployment slots**.</span></span>

### <a name="monitoring"></a><span data-ttu-id="897ff-219">Övervakning</span><span class="sxs-lookup"><span data-stu-id="897ff-219">Monitoring</span></span>
<span data-ttu-id="897ff-220">Du kan testa tillgängligheten för HTTP eller HTTPS-slutpunkter från upp till tre fördelade platser i Basic eller Standard-läge.</span><span class="sxs-lookup"><span data-stu-id="897ff-220">In Basic or Standard mode, you can  test the availability of HTTP or HTTPS endpoints, from up to three geo-distributed locations.</span></span> <span data-ttu-id="897ff-221">En övervakning testet misslyckas om HTTP-svarskoden är ett fel (4xx eller 5xx) eller svaret tar mer än 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="897ff-221">A monitoring test fails if the HTTP response code is an error (4xx or 5xx) or the response takes more than 30 seconds.</span></span> <span data-ttu-id="897ff-222">En slutpunkt anses vara tillgänglig om övervakning testerna lyckas från angivna platser.</span><span class="sxs-lookup"><span data-stu-id="897ff-222">An endpoint is considered available if the monitoring tests succeed from all the specified locations.</span></span> 

<span data-ttu-id="897ff-223">Mer information finns i [så här: övervaka status för web endpoint].</span><span class="sxs-lookup"><span data-stu-id="897ff-223">For more information, see [How to: Monitor web endpoint status].</span></span>

> [!NOTE]
> <span data-ttu-id="897ff-224">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst]. Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="897ff-224">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="897ff-225">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="897ff-225">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="897ff-226">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="897ff-226">Next steps</span></span>
* <span data-ttu-id="897ff-227">[Konfigurera ett anpassat domännamn i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="897ff-227">[Configure a custom domain name in Azure App Service]</span></span>
* <span data-ttu-id="897ff-228">[Aktivera HTTPS för en app i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="897ff-228">[Enable HTTPS for an app in Azure App Service]</span></span>
* <span data-ttu-id="897ff-229">[Skala en webbapp i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="897ff-229">[Scale a web app in Azure App Service]</span></span>
* <span data-ttu-id="897ff-230">[Övervakning grunderna för Web Apps i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="897ff-230">[Monitoring basics for Web Apps in Azure App Service]</span></span>

<!-- URL List -->

<span data-ttu-id="897ff-231">[ASP.NET SignalR]: http://www.asp.net/signalr</span><span class="sxs-lookup"><span data-stu-id="897ff-231">[ASP.NET SignalR]: http://www.asp.net/signalr</span></span>
<span data-ttu-id="897ff-232">[Azure Portal]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="897ff-232">[Azure Portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="897ff-233">[Konfigurera ett anpassat domännamn i Azure App Service]: ./app-service-web-tutorial-custom-domain.md</span><span class="sxs-lookup"><span data-stu-id="897ff-233">[Configure a custom domain name in Azure App Service]: ./app-service-web-tutorial-custom-domain.md</span></span>
<span data-ttu-id="897ff-234">[distribuera till Mellanlagringsmiljöer för Web Apps i Azure App Service]: ./web-sites-staged-publishing.md</span><span class="sxs-lookup"><span data-stu-id="897ff-234">[Deploy to Staging Environments for Web Apps in Azure App Service]: ./web-sites-staged-publishing.md</span></span>
<span data-ttu-id="897ff-235">[Aktivera HTTPS för en app i Azure App Service]: ./app-service-web-tutorial-custom-ssl.md</span><span class="sxs-lookup"><span data-stu-id="897ff-235">[Enable HTTPS for an app in Azure App Service]: ./app-service-web-tutorial-custom-ssl.md</span></span>
<span data-ttu-id="897ff-236">[så här: övervaka status för web endpoint]: http://go.microsoft.com/fwLink/?LinkID=279906</span><span class="sxs-lookup"><span data-stu-id="897ff-236">[How to: Monitor web endpoint status]: http://go.microsoft.com/fwLink/?LinkID=279906</span></span>
<span data-ttu-id="897ff-237">[Övervakning grunderna för Web Apps i Azure App Service]: ./web-sites-monitor.md</span><span class="sxs-lookup"><span data-stu-id="897ff-237">[Monitoring basics for Web Apps in Azure App Service]: ./web-sites-monitor.md</span></span>
<span data-ttu-id="897ff-238">[pipelineläge]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application</span><span class="sxs-lookup"><span data-stu-id="897ff-238">[pipeline mode]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application</span></span>
<span data-ttu-id="897ff-239">[Skala en webbapp i Azure App Service]: ./web-sites-scale.md</span><span class="sxs-lookup"><span data-stu-id="897ff-239">[Scale a web app in Azure App Service]: ./web-sites-scale.md</span></span>
<span data-ttu-id="897ff-240">[socket.io]: ./web-sites-nodejs-chat-app-socketio.md</span><span class="sxs-lookup"><span data-stu-id="897ff-240">[socket.io]: ./web-sites-nodejs-chat-app-socketio.md</span></span>
<span data-ttu-id="897ff-241">[Prova Apptjänst]: https://azure.microsoft.com/try/app-service/</span><span class="sxs-lookup"><span data-stu-id="897ff-241">[Try App Service]: https://azure.microsoft.com/try/app-service/</span></span>

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
