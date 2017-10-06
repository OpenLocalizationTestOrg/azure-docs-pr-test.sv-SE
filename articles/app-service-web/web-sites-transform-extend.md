---
title: "aaaAzure App Service webbapp avancerade konfigurationer och tillägg"
description: "Använd XML-dokumentet Transformation(XDT) deklarationer tootransform hello ApplicationHost.config-filen i din Azure App Service web app och tooadd privata tillägg tooenable anpassade administrativa åtgärder."
author: cephalin
writer: cephalin
editor: mollybos
manager: erikre
services: app-service
documentationcenter: 
ms.assetid: b441a286-ef38-4abc-b102-cdb249baf5bc
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2016
ms.author: cephalin
ms.openlocfilehash: 873347ac13113d1ac989cba29128382c81dcfcca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a><span data-ttu-id="93431-103">Azure App Service webbapp avancerade konfigurationer och tillägg</span><span class="sxs-lookup"><span data-stu-id="93431-103">Azure App Service web app advanced config and extensions</span></span>
<span data-ttu-id="93431-104">Med hjälp av [transformering av XML-](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) deklarationer kan du omvandla hello [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) filen i webbappen i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="93431-104">By using [XML Document Transformation](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) declarations, you can transform hello [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) file in your web app in Azure App Service.</span></span> <span data-ttu-id="93431-105">Du kan också använda XDT deklarationer tooadd privata tillägg tooenable anpassade web app administrativa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="93431-105">You can also use XDT declarations tooadd private extensions tooenable custom web app administration actions.</span></span> <span data-ttu-id="93431-106">Den här artikeln innehåller ett exempel PHP Manager webbprogramtillägg som möjliggör hantering av PHP-inställningar via ett webbgränssnitt.</span><span class="sxs-lookup"><span data-stu-id="93431-106">This article includes a sample PHP Manager web app extension that enables management of PHP settings through a web interface.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <span data-ttu-id="93431-107"><a id="transform"></a>Avancerad konfiguration via ApplicationHost.config</span><span class="sxs-lookup"><span data-stu-id="93431-107"><a id="transform"></a>Advanced configuration through ApplicationHost.config</span></span>
<span data-ttu-id="93431-108">Hej Apptjänst-plattformen ger flexibilitet och kontroll för webbappkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="93431-108">hello App Service platform provides flexibility and control for web app configuration.</span></span> <span data-ttu-id="93431-109">Även om hello standard IIS ApplicationHost.config-konfigurationsfilen inte är tillgängliga för direkt redigering i App Service stöder hello plattformen en deklarativ ApplicationHost.config transformeringen modell baserat på XML-dokumentet omvandling (XDT).</span><span class="sxs-lookup"><span data-stu-id="93431-109">Although hello standard IIS ApplicationHost.config configuration file is not available for direct editing in App Service, hello platform supports a declarative ApplicationHost.config transform model based on XML Document Transformation (XDT).</span></span>

<span data-ttu-id="93431-110">tooleverage funktionen transformering som du skapar en ApplicationHost.xdt-fil med XDT innehåll och placeras under hello platsens rot (d:\home\site) i hello [Kudu konsolen](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="93431-110">tooleverage this transform functionality, you create an ApplicationHost.xdt file with XDT content and place under hello site root (d:\home\site) in hello [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span></span> <span data-ttu-id="93431-111">Du kan behöva toorestart hello Web App för ändringar tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="93431-111">You may need toorestart hello Web App for changes tootake effect.</span></span>

<span data-ttu-id="93431-112">hello följande applicationHost.xdt exempel visar hur tooadd en ny anpassad miljö variabeln tooa webbapp som använder PHP 5.4.</span><span class="sxs-lookup"><span data-stu-id="93431-112">hello following applicationHost.xdt sample shows how tooadd a new custom environment variable tooa web app that uses PHP 5.4.</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <fastCgi>
      <application>
        <environmentVariables>
          <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
        </environmentVariables>
      </application>
    </fastCgi>
  </system.webServer>
</configuration>
```

<span data-ttu-id="93431-113">En loggfil med transformeringsstatus och information är tillgänglig från hello FTP roten under LogFiles\Transform.</span><span class="sxs-lookup"><span data-stu-id="93431-113">A log file with transform status and details is available from hello FTP root under LogFiles\Transform.</span></span>

<span data-ttu-id="93431-114">Ytterligare exempel finns [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span><span class="sxs-lookup"><span data-stu-id="93431-114">For additional samples, see [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).</span></span>

<span data-ttu-id="93431-115">**Obs!**</span><span class="sxs-lookup"><span data-stu-id="93431-115">**Note**</span></span><br />
<span data-ttu-id="93431-116">Element från hello listan över moduler under `system.webServer` kan inte tas bort eller ordnas om, men tillägg toohello lista är möjliga.</span><span class="sxs-lookup"><span data-stu-id="93431-116">Elements from hello list of modules under `system.webServer` cannot be removed or reordered, but additions toohello list are possible.</span></span>

## <span data-ttu-id="93431-117"><a id="extend"></a>Utöka ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="93431-117"><a id="extend"></a> Extend your web app</span></span>
### <span data-ttu-id="93431-118"><a id="overview"></a>Översikt över privata webbprogramtillägg</span><span class="sxs-lookup"><span data-stu-id="93431-118"><a id="overview"></a> Overview of private web app extensions</span></span>
<span data-ttu-id="93431-119">Apptjänst stöder webbprogramtillägg som en utökningspunkt för administrativa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="93431-119">App Service supports web app extensions as an extensibility point for administrative actions.</span></span> <span data-ttu-id="93431-120">Vissa funktioner för Apptjänst-plattformen implementeras i själva verket som förinstallerade tillägg.</span><span class="sxs-lookup"><span data-stu-id="93431-120">In fact, some App Service platform features are implemented as pre-installed extensions.</span></span> <span data-ttu-id="93431-121">Medan hello förinstallerade plattformstillägg inte kan ändras kan du skapa och konfigurera privata tillägg för egna webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="93431-121">While hello pre-installed platform extensions cannot be modified, you can create and configure private extensions for your own web app.</span></span> <span data-ttu-id="93431-122">Den här funktionen använder också XDT deklarationer.</span><span class="sxs-lookup"><span data-stu-id="93431-122">This functionality also relies on XDT declarations.</span></span> <span data-ttu-id="93431-123">hello viktiga steg för att skapa en privat webbprogramtillägg är hello följande:</span><span class="sxs-lookup"><span data-stu-id="93431-123">hello key steps for creating a private web app extension are hello following:</span></span>

1. <span data-ttu-id="93431-124">Web app tillägget **innehåll**: skapa alla webbprogram som stöds av Apptjänst</span><span class="sxs-lookup"><span data-stu-id="93431-124">Web app extension **content**: create any web application supported by App Service</span></span>
2. <span data-ttu-id="93431-125">Web app tillägget **deklaration**: skapa en ApplicationHost.xdt-fil</span><span class="sxs-lookup"><span data-stu-id="93431-125">Web app extension **declaration**: create an ApplicationHost.xdt file</span></span>
3. <span data-ttu-id="93431-126">Web app tillägget **distribution**: placera innehållet i hello SiteExtensions mapp under`root`</span><span class="sxs-lookup"><span data-stu-id="93431-126">Web app extension **deployment**: place content in hello SiteExtensions folder under `root`</span></span>

<span data-ttu-id="93431-127">Interna länkar för hello webbprogrammet ska peka tooa sökväg relativa toohello sökväg anges i hello ApplicationHost.xdt-filen.</span><span class="sxs-lookup"><span data-stu-id="93431-127">Internal links for hello web app should point tooa path relative toohello application path specified in hello ApplicationHost.xdt file.</span></span> <span data-ttu-id="93431-128">Ändra toohello ApplicationHost.xdt filer kräver web app återvinning.</span><span class="sxs-lookup"><span data-stu-id="93431-128">Any change toohello ApplicationHost.xdt file requires a web app recycle.</span></span>

<span data-ttu-id="93431-129">**Obs**: Mer information om dessa nyckelelement finns på [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span><span class="sxs-lookup"><span data-stu-id="93431-129">**Note**: Additional information for these key elements is available at [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).</span></span>

<span data-ttu-id="93431-130">Ett detaljerat exempel är inkluderade tooillustrate hello steg för att skapa och aktivera en privat webbprogramtillägg.</span><span class="sxs-lookup"><span data-stu-id="93431-130">A detailed example is included tooillustrate hello steps for creating and enabling a private web app extension.</span></span> <span data-ttu-id="93431-131">Hej källkoden för hello PHP Manager exemplet som följer kan hämtas från [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span><span class="sxs-lookup"><span data-stu-id="93431-131">hello source code for hello PHP Manager example that follows can be downloaded from [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).</span></span>

### <span data-ttu-id="93431-132"><a id="SiteSample"></a>Web app tillägget exempel: PHP Manager</span><span class="sxs-lookup"><span data-stu-id="93431-132"><a id="SiteSample"></a> Web app extension example: PHP Manager</span></span>
<span data-ttu-id="93431-133">PHP Manager är en webbprogramtillägg gör app webbadministratörer tooeasily visa och konfigurera sina PHP-inställningar med hjälp av ett webbgränssnitt i stället för att toomodify PHP ini-filer direkt.</span><span class="sxs-lookup"><span data-stu-id="93431-133">PHP Manager is a web app extension that allows web app administrators tooeasily view and configure their PHP settings using a web interface instead of having toomodify PHP .ini files directly.</span></span> <span data-ttu-id="93431-134">Gemensamma konfigurationsfiler för PHP inkluderar hello php.ini-filen finns under Program- och hello. user.ini fil i hello rotmappen för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="93431-134">Common configuration files for PHP include hello php.ini file located under Program Files and hello .user.ini file located in hello root folder of your web app.</span></span> <span data-ttu-id="93431-135">Eftersom hello php.ini-filen inte är direkt redigeras på hello Apptjänst-plattformen hello tillägget för PHP Manager använder hello. user.ini filen tooapply ändras.</span><span class="sxs-lookup"><span data-stu-id="93431-135">Since hello php.ini file is not directly editable on hello App Service platform, hello PHP Manager extension uses hello .user.ini file tooapply setting changes.</span></span>

#### <span data-ttu-id="93431-136"><a id="PHPwebapp"></a>hello Manager PHP-webbprogram</span><span class="sxs-lookup"><span data-stu-id="93431-136"><a id="PHPwebapp"></a> hello PHP Manager web application</span></span>
<span data-ttu-id="93431-137">hello följer hello startsida hello PHP Manager-distribution:</span><span class="sxs-lookup"><span data-stu-id="93431-137">hello following is hello home page of hello PHP Manager deployment:</span></span>

![TransformSitePHPUI][TransformSitePHPUI]

<span data-ttu-id="93431-139">Som du ser är en webbprogramtillägg precis som ett reguljärt webbprogram, men med ytterligare en ApplicationHost.xdt fil placeras i hello rotmapp hello webbprogram (Mer information om hello ApplicationHost.xdt filen finns i hello nästa avsnitt i det här artikel).</span><span class="sxs-lookup"><span data-stu-id="93431-139">As you can see, a web app extension is just like a regular web application, but with an additional ApplicationHost.xdt file placed in hello root folder of hello web app (more details about hello ApplicationHost.xdt file are available in hello next section of this article).</span></span>

<span data-ttu-id="93431-140">hello PHP Manager extension skapades med hello Visual Studio ASP.NET MVC 4-webbprogram mallen.</span><span class="sxs-lookup"><span data-stu-id="93431-140">hello PHP Manager extension was created using hello Visual Studio ASP.NET MVC 4 Web Application template.</span></span> <span data-ttu-id="93431-141">hello visar följande vy från Solution Explorer hello strukturen för hello Manager PHP-tillägg.</span><span class="sxs-lookup"><span data-stu-id="93431-141">hello following view from Solution Explorer shows hello structure of hello PHP Manager extension.</span></span>

![TransformSiteSolEx][TransformSiteSolEx]

<span data-ttu-id="93431-143">hello är endast särskilda logik som krävs för i/o-fil tooindicate där hello wwwroot directory av hello webbprogram finns.</span><span class="sxs-lookup"><span data-stu-id="93431-143">hello only special logic needed for file I/O is tooindicate where hello wwwroot directory of hello web app is located.</span></span> <span data-ttu-id="93431-144">Eftersom hello koden nedan, hello miljö variabeln ”HOME-visar hello rotsökvägen för webbapp och hello wwwroot sökvägen kan konstrueras genom att lägga till” site\wwwroot ”:</span><span class="sxs-lookup"><span data-stu-id="93431-144">As hello following code example shows, hello environment variable "HOME" indicates hello web app's root path, and hello wwwroot path can be constructed by appending "site\wwwroot":</span></span>

```csharp
/// <summary>
/// Gives hello location of hello .user.ini file, even if one doesn't exist yet
/// </summary>
private static string GetUserSettingsFilePath()
{
  var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
  if (rootPath == null)
  {
    rootPath = System.IO.Path.GetTempPath(); // For testing purposes
  };
  var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
  return userSettingsFile;
}
```


<span data-ttu-id="93431-145">När du har hello katalogsökväg kan du använda reguljära filen tooread för i/o-åtgärder och skriva toofiles.</span><span class="sxs-lookup"><span data-stu-id="93431-145">After you have hello directory path, you can use regular file I/O operations tooread and write toofiles.</span></span>

<span data-ttu-id="93431-146">En punkt i försiktighet med webbprogramtillägg gäller hello hantering av interna länkar.</span><span class="sxs-lookup"><span data-stu-id="93431-146">One point of caution with web app extensions regards hello handling of internal links.</span></span>  <span data-ttu-id="93431-147">Om du har alla länkar i HTML-filer som ger absoluta sökvägar toointernal länkar på ditt webbprogram, måste du kontrollera att dessa länkar inledd med en Tilläggsnamn som din rot.</span><span class="sxs-lookup"><span data-stu-id="93431-147">If you have any links in your HTML files that give absolute paths toointernal links on your web app, you must ensure those links are prepended with your extension name as your root.</span></span> <span data-ttu-id="93431-148">Detta behövs eftersom hello roten för din anknytning nu ”/`[your-extension-name]`/” i stället för att bara ”/”, så alla interna länkar måste uppdateras.</span><span class="sxs-lookup"><span data-stu-id="93431-148">This is needed because hello root for your extension is now "/`[your-extension-name]`/" rather than being just "/", so any internal links must be updated accordingly.</span></span> <span data-ttu-id="93431-149">Anta exempelvis att din kod innehåller en länk toohello följande:</span><span class="sxs-lookup"><span data-stu-id="93431-149">For example, suppose your code includes a link toohello following:</span></span>

`"<a href="/Home/Settings">PHP Settings</a>"`

<span data-ttu-id="93431-150">När hello länk är en del av en webbprogramtillägg kan måste hello länken vara i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="93431-150">When hello link is part of a web app extension, hello link must be in hello following form:</span></span>

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

<span data-ttu-id="93431-151">Du kan undvika det här kravet genom att antingen använda endast relativa sökvägar i ditt webbprogram eller i hello skiftläget för ASP.NET-program med hjälp av hello `@Html.ActionLink` metod som skapar hello lämpliga länkar du.</span><span class="sxs-lookup"><span data-stu-id="93431-151">You can work around this requirement by either using only relative paths within your web application, or in hello case of ASP.NET applications, by using hello `@Html.ActionLink` method which creates hello appropriate links for you.</span></span>

#### <span data-ttu-id="93431-152"><a id="XDT"></a>Hej applicationHost.xdt fil</span><span class="sxs-lookup"><span data-stu-id="93431-152"><a id="XDT"></a> hello applicationHost.xdt file</span></span>
<span data-ttu-id="93431-153">hello-koden för din webbprogramtillägg går under %HOME%\SiteExtensions\[en Tilläggsnamn].</span><span class="sxs-lookup"><span data-stu-id="93431-153">hello code for your web app extension goes under %HOME%\SiteExtensions\[your-extension-name].</span></span> <span data-ttu-id="93431-154">Vi ringer upp dig hello tillägget roten.</span><span class="sxs-lookup"><span data-stu-id="93431-154">We'll call this hello extension root.</span></span>  

<span data-ttu-id="93431-155">tooregister din webbprogramtillägg med hello applicationHost.config-filen måste tooplace en fil med namnet ApplicationHost.xdt i hello tillägget rot.</span><span class="sxs-lookup"><span data-stu-id="93431-155">tooregister your web app extension with hello applicationHost.config file, you need tooplace a file called ApplicationHost.xdt in hello extension root.</span></span> <span data-ttu-id="93431-156">hello innehållet hello ApplicationHost.xdt filen bör vara följande:</span><span class="sxs-lookup"><span data-stu-id="93431-156">hello content of hello ApplicationHost.xdt file should be as follows:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.applicationHost>
    <sites>
      <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
        <!-- NOTE: Add your extension name in hello application paths below -->
        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
          <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
        </application>
      </site>
    </sites>
  </system.applicationHost>
</configuration>
```

<span data-ttu-id="93431-157">hello-namn som du väljer som en Tilläggsnamn ska ha hello samma namn som din rotmapp för tillägget.</span><span class="sxs-lookup"><span data-stu-id="93431-157">hello name you select as your extension name should have hello same name as your extension root folder.</span></span>

<span data-ttu-id="93431-158">Detta påverkar hello för att lägga till ett nytt program sökvägen toohello `system.applicationHost` listan med platser under hello SCM-webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="93431-158">This has hello effect of adding a new application path toohello `system.applicationHost` sites list under hello SCM site.</span></span> <span data-ttu-id="93431-159">hello SCM platsen är en plats administration slutpunkt med särskilda autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="93431-159">hello SCM site is a site administration end point with specific access credentials.</span></span> <span data-ttu-id="93431-160">Det har hello URL `https://[your-site-name].scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="93431-160">It has hello URL `https://[your-site-name].scm.azurewebsites.net`.</span></span>  

```xml
<system.applicationHost>
  ...       
  <sites>
    <site name="~1[your-website]" id="1716402716">
      <bindings>
        <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
        <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
      </bindings>
      <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
      <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
      <logFile logSiteId="false" />
      <application path="/" applicationPool="[your-website]">
        <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
      </application>
      <!-- Note hello custom changes that go here -->
      <application path="/[your-extension-name]" applicationPool="[your-website]">
        <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
      </application>
    </site>
  </sites>
  ... 
</system.applicationHost>
```

### <span data-ttu-id="93431-161"><a id="deploy"></a>Tillägget Web app-distribution</span><span class="sxs-lookup"><span data-stu-id="93431-161"><a id="deploy"></a> Web app extension deployment</span></span>
<span data-ttu-id="93431-162">tooinstall din webbprogramtillägg du kan använda FTP toocopy alla hello-filer på din web application toohello `\SiteExtensions\[your-extension-name]` mappen av hello webbprogram som du vill använda tooinstall hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="93431-162">tooinstall your web app extension, you can use FTP toocopy all hello files of your web application toohello `\SiteExtensions\[your-extension-name]` folder of hello web app on which you want tooinstall hello extension.</span></span>  <span data-ttu-id="93431-163">Vara säker på att toocopy hello ApplicationHost.xdt toothis plats samt.</span><span class="sxs-lookup"><span data-stu-id="93431-163">Be sure toocopy hello ApplicationHost.xdt file toothis location as well.</span></span> <span data-ttu-id="93431-164">Starta om din tooenable hello webbprogramtillägg.</span><span class="sxs-lookup"><span data-stu-id="93431-164">Restart your web app tooenable hello extension.</span></span>

<span data-ttu-id="93431-165">Du bör vara kan toosee din webbprogramtillägg på:</span><span class="sxs-lookup"><span data-stu-id="93431-165">You should be able toosee your web app extension at:</span></span>

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

<span data-ttu-id="93431-166">Observera att hello URL-Adressen ser ut precis som hello URL för ditt webbprogram, förutom att den använder HTTPS och innehåller ”.scm”.</span><span class="sxs-lookup"><span data-stu-id="93431-166">Note that hello URL looks just like hello URL for your web app, except that it uses HTTPS and contains ".scm".</span></span>

<span data-ttu-id="93431-167">Det är möjligt toodisable alla privata (inte förinstallerat)-tillägg för ditt webbprogram under utveckling och utredningar genom att lägga till en app-inställningar med hello nyckel `WEBSITE_PRIVATE_EXTENSIONS` och värdet `0`.</span><span class="sxs-lookup"><span data-stu-id="93431-167">It is possible toodisable all private (not pre-installed) extensions for your web app during development and investigations by adding an app settings with hello key `WEBSITE_PRIVATE_EXTENSIONS` and a value of `0`.</span></span>

> [!NOTE]
> <span data-ttu-id="93431-168">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="93431-168">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="93431-169">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="93431-169">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="93431-170">Nyheter</span><span class="sxs-lookup"><span data-stu-id="93431-170">What's changed</span></span>
* <span data-ttu-id="93431-171">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="93431-171">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

