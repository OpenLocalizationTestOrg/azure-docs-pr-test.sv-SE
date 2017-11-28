---
title: aaaConfigure web apps i Azure App Service
description: Hur tooconfigure en webbapp i Azure App Service
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
ms.openlocfilehash: 8697ab6f21cfeb470e11f0d82c68692d43142fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-apps-in-azure-app-service"></a><span data-ttu-id="d4d33-103">Konfigurera webbappar i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d4d33-103">Configure web apps in Azure App Service</span></span>
<span data-ttu-id="d4d33-104">Det här avsnittet beskrivs hur en web app med hjälp av tooconfigure hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="d4d33-104">This topic explains how tooconfigure a web app using hello [Azure Portal].</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a><span data-ttu-id="d4d33-105">Programinställningar</span><span class="sxs-lookup"><span data-stu-id="d4d33-105">Application settings</span></span>
1. <span data-ttu-id="d4d33-106">I hello [Azure Portal]öppnar hello bladet för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d4d33-106">In hello [Azure Portal], open hello blade for hello web app.</span></span>
2. <span data-ttu-id="d4d33-107">Klicka på **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-107">Click **All Settings**.</span></span>
3. <span data-ttu-id="d4d33-108">Klicka på **programinställningar**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-108">Click **Application Settings**.</span></span>

![Programinställningar][configure01]

<span data-ttu-id="d4d33-110">Hej **programinställningar** bladet har inställningar som är grupperade under flera kategorier.</span><span class="sxs-lookup"><span data-stu-id="d4d33-110">hello **Application settings** blade has settings grouped under several categories.</span></span>

### <a name="general-settings"></a><span data-ttu-id="d4d33-111">Allmänna inställningar</span><span class="sxs-lookup"><span data-stu-id="d4d33-111">General settings</span></span>
<span data-ttu-id="d4d33-112">**Versioner av Framework**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-112">**Framework versions**.</span></span> <span data-ttu-id="d4d33-113">Ange dessa alternativ om din app använder alla dessa ramverk:</span><span class="sxs-lookup"><span data-stu-id="d4d33-113">Set these options if your app uses any these frameworks:</span></span> 

* <span data-ttu-id="d4d33-114">**.NET framework**: Ange hello .NET framework version.</span><span class="sxs-lookup"><span data-stu-id="d4d33-114">**.NET Framework**: Set hello .NET framework version.</span></span> 
* <span data-ttu-id="d4d33-115">**PHP**: Ange hello PHP version eller **OFF** toodisable PHP.</span><span class="sxs-lookup"><span data-stu-id="d4d33-115">**PHP**: Set hello PHP version, or **OFF** toodisable PHP.</span></span> 
* <span data-ttu-id="d4d33-116">**Java**: Välj hello Java-version eller **OFF** toodisable Java.</span><span class="sxs-lookup"><span data-stu-id="d4d33-116">**Java**: Select hello Java version or **OFF** toodisable Java.</span></span> <span data-ttu-id="d4d33-117">Använd hello **Webbehållaren** alternativet toochoose mellan Tomcat- och Jetty-versioner.</span><span class="sxs-lookup"><span data-stu-id="d4d33-117">Use hello **Web Container** option toochoose between Tomcat and Jetty versions.</span></span>
* <span data-ttu-id="d4d33-118">**Python**: Välj hello Python-versionen eller **OFF** toodisable Python.</span><span class="sxs-lookup"><span data-stu-id="d4d33-118">**Python**: Select hello Python version, or **OFF** toodisable Python.</span></span>

<span data-ttu-id="d4d33-119">Av tekniska skäl inaktiveras när du aktiverar Java för din app hello alternativ för .NET, PHP och Python.</span><span class="sxs-lookup"><span data-stu-id="d4d33-119">For technical reasons, enabling Java for your app disables hello .NET, PHP, and Python options.</span></span>

<span data-ttu-id="d4d33-120"><a name="platform"></a>
**Plattform**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-120"><a name="platform"></a>
**Platform**.</span></span> <span data-ttu-id="d4d33-121">Anger om ditt webbprogram kör i en 32-bitars eller 64-bitars miljö.</span><span class="sxs-lookup"><span data-stu-id="d4d33-121">Selects whether your web app runs in a 32-bit or 64-bit environment.</span></span> <span data-ttu-id="d4d33-122">hello 64-bitars miljö kräver Basic eller Standard-läge.</span><span class="sxs-lookup"><span data-stu-id="d4d33-122">hello 64-bit environment requires Basic or Standard mode.</span></span> <span data-ttu-id="d4d33-123">Frigör och delade lägen köras alltid i en 32-bitars-miljö.</span><span class="sxs-lookup"><span data-stu-id="d4d33-123">Free and Shared modes always run in a 32-bit environment.</span></span>

<span data-ttu-id="d4d33-124">**Web Sockets**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-124">**Web Sockets**.</span></span> <span data-ttu-id="d4d33-125">Ange **ON** tooenable hello WebSocket-protokollet, till exempel, om ditt webbprogram använder [ASP.NET SignalR] eller [socket.io].</span><span class="sxs-lookup"><span data-stu-id="d4d33-125">Set **ON** tooenable hello WebSocket protocol; for example, if your web app uses [ASP.NET SignalR] or [socket.io].</span></span>

<span data-ttu-id="d4d33-126"><a name="alwayson"></a>
**Always On**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-126"><a name="alwayson"></a>
**Always On**.</span></span> <span data-ttu-id="d4d33-127">Som standard inaktiveras webbappar om de är inaktiv under en tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="d4d33-127">By default, web apps are unloaded if they are idle for some period of time.</span></span> <span data-ttu-id="d4d33-128">På så sätt kan hello system spara resurser.</span><span class="sxs-lookup"><span data-stu-id="d4d33-128">This lets hello system conserve resources.</span></span> <span data-ttu-id="d4d33-129">I Basic eller Standard-läge kan du aktivera **alltid på** tookeep hello app laddas alla hello tid.</span><span class="sxs-lookup"><span data-stu-id="d4d33-129">In Basic or Standard mode, you can enable **Always On** tookeep hello app loaded all hello time.</span></span> <span data-ttu-id="d4d33-130">Om din app körs kontinuerliga Webbjobb eller körs WebJobs aktiveras med hjälp av ett CRON-uttryck, bör du aktivera **alltid på**, eller hello web jobb körs inte på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="d4d33-130">If your app runs continuous WebJobs or runs WebJobs triggered using a CRON expression, you should enable **Always On**, or hello web jobs may not run reliably.</span></span>

<span data-ttu-id="d4d33-131">**Hanterat Pipeline-Version**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-131">**Managed Pipeline Version**.</span></span> <span data-ttu-id="d4d33-132">Anger hello IIS [pipelineläge].</span><span class="sxs-lookup"><span data-stu-id="d4d33-132">Sets hello IIS [pipeline mode].</span></span> <span data-ttu-id="d4d33-133">Lämna detta anges tooIntegrated (hello standard) om du har en äldre app som kräver en äldre version av IIS.</span><span class="sxs-lookup"><span data-stu-id="d4d33-133">Leave this set tooIntegrated (hello default) unless you have a legacy app that requires an older version of IIS.</span></span>

<span data-ttu-id="d4d33-134">**Automatisk växling**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-134">**Auto Swap**.</span></span> <span data-ttu-id="d4d33-135">Om du aktiverar automatisk växling för en distributionsplats ska Apptjänst automatiskt växla hello webbprogram till produktionen när du trycker på en uppdatering toothat plats.</span><span class="sxs-lookup"><span data-stu-id="d4d33-135">If you enable Auto Swap for a deployment slot, App Service will automatically swap hello web app into production when you push an update toothat slot.</span></span> <span data-ttu-id="d4d33-136">Mer information finns i [distribuera toostaging fack för web apps i Azure App Service](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d4d33-136">For more information, see [Deploy toostaging slots for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span>

### <a name="debugging"></a><span data-ttu-id="d4d33-137">Felsökning</span><span class="sxs-lookup"><span data-stu-id="d4d33-137">Debugging</span></span>
<span data-ttu-id="d4d33-138">**Fjärrfelsökning**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-138">**Remote Debugging**.</span></span> <span data-ttu-id="d4d33-139">Aktiverar fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="d4d33-139">Enables remote debugging.</span></span> <span data-ttu-id="d4d33-140">När aktiverat, kan du använda hello remote felsökning i Visual Studio tooconnect direkt tooyour web app.</span><span class="sxs-lookup"><span data-stu-id="d4d33-140">When enabled, you can use hello remote debugger in Visual Studio tooconnect directly tooyour web app.</span></span> <span data-ttu-id="d4d33-141">Fjärrfelsökning förblir aktiverat för 48 timmar.</span><span class="sxs-lookup"><span data-stu-id="d4d33-141">Remote debugging will remain enabled for 48 hours.</span></span> 

### <a name="app-settings"></a><span data-ttu-id="d4d33-142">App-inställningar</span><span class="sxs-lookup"><span data-stu-id="d4d33-142">App settings</span></span>
<span data-ttu-id="d4d33-143">Det här avsnittet innehåller namn/värde-par som du web app läser in vid start.</span><span class="sxs-lookup"><span data-stu-id="d4d33-143">This section contains name/value pairs that you web app will load on start up.</span></span> 

* <span data-ttu-id="d4d33-144">För .NET-appar de här inställningarna är injekteras i konfigurationen av .NET `AppSettings` vid körning kan åsidosätta befintliga inställningar.</span><span class="sxs-lookup"><span data-stu-id="d4d33-144">For .NET apps, these settings are injected into your .NET configuration `AppSettings` at runtime, overriding existing settings.</span></span> 
* <span data-ttu-id="d4d33-145">PHP, Python, Java och noden program kan komma åt de här inställningarna som miljövariabler vid körning.</span><span class="sxs-lookup"><span data-stu-id="d4d33-145">PHP, Python, Java and Node applications can access these settings as environment variables at runtime.</span></span> <span data-ttu-id="d4d33-146">Två miljövariabler som skapas för varje appinställning. en med hello namn anges av hello app inställningen som och en annan med prefixet APPSETTING_.</span><span class="sxs-lookup"><span data-stu-id="d4d33-146">For each app setting, two environment variables are created; one with hello name specified by hello app setting entry, and another with a prefix of APPSETTING_.</span></span> <span data-ttu-id="d4d33-147">Innehåller båda hello samma värde.</span><span class="sxs-lookup"><span data-stu-id="d4d33-147">Both contain hello same value.</span></span>

### <a name="connection-strings"></a><span data-ttu-id="d4d33-148">Anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="d4d33-148">Connection strings</span></span>
<span data-ttu-id="d4d33-149">Anslutningssträngar för länkade resurser.</span><span class="sxs-lookup"><span data-stu-id="d4d33-149">Connection strings for linked resources.</span></span> 

<span data-ttu-id="d4d33-150">För .NET-appar har dessa anslutningssträngar injekteras i konfigurationen av .NET `connectionStrings` inställningar vid körning kan åsidosätta befintliga poster där hello nyckel är lika med hello länkade databasnamnet.</span><span class="sxs-lookup"><span data-stu-id="d4d33-150">For .NET apps, these connection strings are injected into your .NET configuration `connectionStrings` settings at runtime, overriding existing entries where hello key equals hello linked database name.</span></span> 

<span data-ttu-id="d4d33-151">PHP, Python, Java och noden program, ska inställningarna vara tillgänglig som miljövariabler vid körning, prefixet hello anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="d4d33-151">For PHP, Python, Java and Node applications, these settings will be available as environment variables at runtime, prefixed with hello connection type.</span></span> <span data-ttu-id="d4d33-152">hello miljö variabeln prefix är följande:</span><span class="sxs-lookup"><span data-stu-id="d4d33-152">hello environment variable prefixes are as follows:</span></span> 

* <span data-ttu-id="d4d33-153">SQLServer:`SQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="d4d33-153">SQL Server: `SQLCONNSTR_`</span></span>
* <span data-ttu-id="d4d33-154">MySQL:`MYSQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="d4d33-154">MySQL: `MYSQLCONNSTR_`</span></span>
* <span data-ttu-id="d4d33-155">SQL-databas:`SQLAZURECONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="d4d33-155">SQL Database: `SQLAZURECONNSTR_`</span></span>
* <span data-ttu-id="d4d33-156">Anpassad:`CUSTOMCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="d4d33-156">Custom: `CUSTOMCONNSTR_`</span></span>

<span data-ttu-id="d4d33-157">Om exempelvis en MySql-anslutningssträng vid namn `connectionstring1`, den kan nås via hello miljövariabeln `MYSQLCONNSTR_connectionString1`.</span><span class="sxs-lookup"><span data-stu-id="d4d33-157">For example, if a MySql connection string were named `connectionstring1`, it would be accessed through hello environment variable `MYSQLCONNSTR_connectionString1`.</span></span>

### <a name="default-documents"></a><span data-ttu-id="d4d33-158">Standarddokument</span><span class="sxs-lookup"><span data-stu-id="d4d33-158">Default documents</span></span>
<span data-ttu-id="d4d33-159">hello standarddokument är hello webbsida som visas på hello rot-URL för en webbplats.</span><span class="sxs-lookup"><span data-stu-id="d4d33-159">hello default document is hello web page that is displayed at hello root URL for a website.</span></span>  <span data-ttu-id="d4d33-160">hello första matchande fil i listan hello används.</span><span class="sxs-lookup"><span data-stu-id="d4d33-160">hello first matching file in hello list is used.</span></span> 

<span data-ttu-id="d4d33-161">Webbprogram kan använda moduler att vägen baserat på URL: en i stället för betjänar statiskt innehåll, då det finns inga standarddokument som sådana.</span><span class="sxs-lookup"><span data-stu-id="d4d33-161">Web apps might use modules that route based on URL, rather than serving static content, in which case there is no default document as such.</span></span>    

### <a name="handler-mappings"></a><span data-ttu-id="d4d33-162">Hanterarmappningar</span><span class="sxs-lookup"><span data-stu-id="d4d33-162">Handler mappings</span></span>
<span data-ttu-id="d4d33-163">Använda det här området tooadd anpassat skript processorer toohandle begäranden för specifika filtillägg.</span><span class="sxs-lookup"><span data-stu-id="d4d33-163">Use this area tooadd custom script processors toohandle requests for specific file extensions.</span></span> 

* <span data-ttu-id="d4d33-164">**Tillägget**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-164">**Extension**.</span></span> <span data-ttu-id="d4d33-165">hello filen tillägget toobe hanteras som *.php eller handler.fcgi.</span><span class="sxs-lookup"><span data-stu-id="d4d33-165">hello file extension toobe handled, such as *.php or handler.fcgi.</span></span> 
* <span data-ttu-id="d4d33-166">**Script Processor sökvägen**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-166">**Script Processor Path**.</span></span> <span data-ttu-id="d4d33-167">hello absolut sökväg hello Skriptprocessor.</span><span class="sxs-lookup"><span data-stu-id="d4d33-167">hello absolute path of hello script processor.</span></span> <span data-ttu-id="d4d33-168">Begäranden toofiles som matchar filnamnstillägget hello bearbetas av hello Skriptprocessor.</span><span class="sxs-lookup"><span data-stu-id="d4d33-168">Requests toofiles that match hello file extension will be processed by hello script processor.</span></span> <span data-ttu-id="d4d33-169">Använd hello sökväg `D:\home\site\wwwroot` toorefer tooyour appens rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="d4d33-169">Use hello path `D:\home\site\wwwroot` toorefer tooyour app's root directory.</span></span>
* <span data-ttu-id="d4d33-170">**Ytterligare argument**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-170">**Additional Arguments**.</span></span> <span data-ttu-id="d4d33-171">Valfria kommandoradsargument för hello Skriptprocessor</span><span class="sxs-lookup"><span data-stu-id="d4d33-171">Optional command-line arguments for hello script processor</span></span> 

### <a name="virtual-applications-and-directories"></a><span data-ttu-id="d4d33-172">Virtuella program och kataloger</span><span class="sxs-lookup"><span data-stu-id="d4d33-172">Virtual applications and directories</span></span>
<span data-ttu-id="d4d33-173">Ange varje virtuell katalog och dess motsvarande fysisk sökväg relativa toohello webbplatsroten tooconfigure virtuella program och kataloger.</span><span class="sxs-lookup"><span data-stu-id="d4d33-173">tooconfigure virtual applications and directories, specify each virtual directory and its corresponding physical path relative toohello website root.</span></span> <span data-ttu-id="d4d33-174">Du kan också markera hello **programmet** kryssrutan toomark en virtuell katalog som ett program.</span><span class="sxs-lookup"><span data-stu-id="d4d33-174">Optionally, you can select hello **Application** checkbox toomark a virtual directory as an application.</span></span>

## <a name="enabling-diagnostic-logs"></a><span data-ttu-id="d4d33-175">Aktivera diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="d4d33-175">Enabling diagnostic logs</span></span>
<span data-ttu-id="d4d33-176">tooenable diagnostikloggar:</span><span class="sxs-lookup"><span data-stu-id="d4d33-176">tooenable diagnostic logs:</span></span>

1. <span data-ttu-id="d4d33-177">Klicka i hello bladet för din webbapp **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-177">In hello blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="d4d33-178">Klicka på **diagnostikloggar**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-178">Click **Diagnostic logs**.</span></span> 

<span data-ttu-id="d4d33-179">Alternativ för att skriva diagnostiska loggar från webbprogram som har stöd för loggning:</span><span class="sxs-lookup"><span data-stu-id="d4d33-179">Options for writing diagnostic logs from a web application that supports logging:</span></span> 

* <span data-ttu-id="d4d33-180">**Programloggning**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-180">**Application Logging**.</span></span> <span data-ttu-id="d4d33-181">Skriver programloggarna toohello filsystem.</span><span class="sxs-lookup"><span data-stu-id="d4d33-181">Writes application logs toohello file system.</span></span> <span data-ttu-id="d4d33-182">Loggning varar under en period på 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="d4d33-182">Logging lasts for a period of 12 hours.</span></span> 

<span data-ttu-id="d4d33-183">**Nivå**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-183">**Level**.</span></span> <span data-ttu-id="d4d33-184">När programmet loggning är aktiverat anger det här alternativet hello mängden information som är registrerade (fel, varning, Information eller utförlig).</span><span class="sxs-lookup"><span data-stu-id="d4d33-184">When application logging is enabled, this option specifies hello amount of information that will be recorded (Error, Warning, Information, or Verbose).</span></span>

<span data-ttu-id="d4d33-185">**Web server-loggning**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-185">**Web server logging**.</span></span> <span data-ttu-id="d4d33-186">Loggar sparas i hello W3C utökad loggning.</span><span class="sxs-lookup"><span data-stu-id="d4d33-186">Logs are saved in hello W3C extended log file format.</span></span> 

<span data-ttu-id="d4d33-187">**Detaljerade felmeddelanden**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-187">**Detailed error messages**.</span></span> <span data-ttu-id="d4d33-188">Sparar felmeddelanden detaljerade .htm-filer.</span><span class="sxs-lookup"><span data-stu-id="d4d33-188">Saves detailed error messages .htm files.</span></span> <span data-ttu-id="d4d33-189">hello filerna sparas under /LogFiles/DetailedErrors.</span><span class="sxs-lookup"><span data-stu-id="d4d33-189">hello files are saved under /LogFiles/DetailedErrors.</span></span> 

<span data-ttu-id="d4d33-190">**Spårning av misslyckade begäranden**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-190">**Failed request tracing**.</span></span> <span data-ttu-id="d4d33-191">Det gick inte att begäranden tooXML filer loggar.</span><span class="sxs-lookup"><span data-stu-id="d4d33-191">Logs failed requests tooXML files.</span></span> <span data-ttu-id="d4d33-192">hello filerna sparas under loggfilerna/W3SVC*xxx*, där xxx är en unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="d4d33-192">hello files are saved under /LogFiles/W3SVC*xxx*, where xxx is a unique identifier.</span></span> <span data-ttu-id="d4d33-193">Den här mappen innehåller en XSL-fil och en eller flera XML-filer.</span><span class="sxs-lookup"><span data-stu-id="d4d33-193">This folder contains an XSL file and one or more XML files.</span></span> <span data-ttu-id="d4d33-194">Se till att toodownload hello XSL-filen eftersom det ger funktioner för att formatera och filtrera hello innehållet i hello XML-filer.</span><span class="sxs-lookup"><span data-stu-id="d4d33-194">Make sure toodownload hello XSL file, because it provides functionality for formatting and filtering hello contents of hello XML files.</span></span>

<span data-ttu-id="d4d33-195">tooview hello loggfiler, måste du skapa FTP-autentiseringsuppgifter, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="d4d33-195">tooview hello log files, you must create FTP credentials, as follows:</span></span>

1. <span data-ttu-id="d4d33-196">Klicka i hello bladet för din webbapp **alla inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-196">In hello blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="d4d33-197">Klicka på **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-197">Click **Deployment credentials**.</span></span>
3. <span data-ttu-id="d4d33-198">Ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="d4d33-198">Enter a user name and password.</span></span>
4. <span data-ttu-id="d4d33-199">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-199">Click **Save**.</span></span>

![Ange autentiseringsuppgifter för distribution][configure03]

<span data-ttu-id="d4d33-201">hello fullständig FTP-användarnamnet är ”app\username” där *app* är hello namnet på ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d4d33-201">hello full FTP user name is “app\username” where *app* is hello name of your web app.</span></span> <span data-ttu-id="d4d33-202">hello användarnamnet visas i hello web app bladet under **Essentials**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-202">hello username is listed in hello web app blade, under **Essentials**.</span></span>  

![Autentiseringsuppgifter för FTP-distribution][configure02]

## <a name="other-configuration-tasks"></a><span data-ttu-id="d4d33-204">Andra konfigurationsuppgifter</span><span class="sxs-lookup"><span data-stu-id="d4d33-204">Other configuration tasks</span></span>
### <a name="ssl"></a><span data-ttu-id="d4d33-205">SSL</span><span class="sxs-lookup"><span data-stu-id="d4d33-205">SSL</span></span>
<span data-ttu-id="d4d33-206">Du kan överföra SSL-certifikat för en anpassad domän i Basic eller Standard-läge.</span><span class="sxs-lookup"><span data-stu-id="d4d33-206">In Basic or Standard mode, you can upload SSL certificates for a custom domain.</span></span> <span data-ttu-id="d4d33-207">Mer information finns i [aktivera HTTPS för en webbapp].</span><span class="sxs-lookup"><span data-stu-id="d4d33-207">For more information, see [Enable HTTPS for a web app].</span></span> 

<span data-ttu-id="d4d33-208">tooview överförda certifikat, klicka på **alla inställningar** > **anpassade domäner och SSL**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-208">tooview your uploaded certificates, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="domain-names"></a><span data-ttu-id="d4d33-209">Domännamn</span><span class="sxs-lookup"><span data-stu-id="d4d33-209">Domain names</span></span>
<span data-ttu-id="d4d33-210">Lägg till anpassade domännamn för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="d4d33-210">Add custom domain names for your web app.</span></span> <span data-ttu-id="d4d33-211">Mer information finns i [konfigurera ett anpassat domännamn för en webbapp i Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="d4d33-211">For more information, see [Configure a custom domain name for a web app in Azure App Service].</span></span>

<span data-ttu-id="d4d33-212">tooview ditt domännamn, klicka på **alla inställningar** > **anpassade domäner och SSL**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-212">tooview your domain names, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="deployments"></a><span data-ttu-id="d4d33-213">Distributioner</span><span class="sxs-lookup"><span data-stu-id="d4d33-213">Deployments</span></span>
* <span data-ttu-id="d4d33-214">Ställ in kontinuerlig distribution.</span><span class="sxs-lookup"><span data-stu-id="d4d33-214">Set up continuous deployment.</span></span> <span data-ttu-id="d4d33-215">Se [med Git toodeploy Web Apps i Azure App Service](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="d4d33-215">See [Using Git toodeploy Web Apps in Azure App Service](web-sites-deploy.md).</span></span>
* <span data-ttu-id="d4d33-216">Distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="d4d33-216">Deployment slots.</span></span> <span data-ttu-id="d4d33-217">Se [distribuera tooStaging miljöer för Web Apps i Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="d4d33-217">See [Deploy tooStaging Environments for Web Apps in Azure App Service].</span></span>

<span data-ttu-id="d4d33-218">tooview dina distributionsplatser, klicka på **alla inställningar** > **distributionsfack**.</span><span class="sxs-lookup"><span data-stu-id="d4d33-218">tooview your deployment slots, click **All Settings** > **Deployment slots**.</span></span>

### <a name="monitoring"></a><span data-ttu-id="d4d33-219">Övervakning</span><span class="sxs-lookup"><span data-stu-id="d4d33-219">Monitoring</span></span>
<span data-ttu-id="d4d33-220">I Basic eller Standard-läge kan du testa hello tillgängligheten för HTTP eller HTTPS-slutpunkter från in toothree fördelade platser.</span><span class="sxs-lookup"><span data-stu-id="d4d33-220">In Basic or Standard mode, you can  test hello availability of HTTP or HTTPS endpoints, from up toothree geo-distributed locations.</span></span> <span data-ttu-id="d4d33-221">En övervakning testet misslyckas om hello HTTP-svarskoden är ett fel (4xx eller 5xx) eller hello svar tar mer än 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="d4d33-221">A monitoring test fails if hello HTTP response code is an error (4xx or 5xx) or hello response takes more than 30 seconds.</span></span> <span data-ttu-id="d4d33-222">En slutpunkt anses vara tillgänglig om hello övervakningstesten lyckas från alla hello angivna platser.</span><span class="sxs-lookup"><span data-stu-id="d4d33-222">An endpoint is considered available if hello monitoring tests succeed from all hello specified locations.</span></span> 

<span data-ttu-id="d4d33-223">Mer information finns i [så här: övervaka status för web endpoint].</span><span class="sxs-lookup"><span data-stu-id="d4d33-223">For more information, see [How to: Monitor web endpoint status].</span></span>

> [!NOTE]
> <span data-ttu-id="d4d33-224">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service], där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="d4d33-224">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="d4d33-225">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="d4d33-225">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d4d33-226">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4d33-226">Next steps</span></span>
* <span data-ttu-id="d4d33-227">[Konfigurera ett anpassat domännamn i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="d4d33-227">[Configure a custom domain name in Azure App Service]</span></span>
* <span data-ttu-id="d4d33-228">[Aktivera HTTPS för en app i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="d4d33-228">[Enable HTTPS for an app in Azure App Service]</span></span>
* <span data-ttu-id="d4d33-229">[Skala en webbapp i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="d4d33-229">[Scale a web app in Azure App Service]</span></span>
* <span data-ttu-id="d4d33-230">[Övervakning grunderna för Web Apps i Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="d4d33-230">[Monitoring basics for Web Apps in Azure App Service]</span></span>

<!-- URL List -->

[ASP.NET-SignalR]: http://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Konfigurera ett anpassat domännamn i Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[Distribuera tooStaging miljöer för Web Apps i Azure App Service]: ./web-sites-staged-publishing.md
[Aktivera HTTPS för en app i Azure App Service]: ./app-service-web-tutorial-custom-ssl.md
[Så här: övervaka status på slutpunkt]: http://go.microsoft.com/fwLink/?LinkID=279906
[Övervakning grunderna för Web Apps i Azure App Service]: ./web-sites-monitor.md
[Pipeline-läge]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Skala en webbapp i Azure App Service]: ./web-sites-scale.md
[socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Prova App Service]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
