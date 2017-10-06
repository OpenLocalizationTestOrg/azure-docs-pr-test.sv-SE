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
# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure App Service webbapp avancerade konfigurationer och tillägg
Med hjälp av [transformering av XML-](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) deklarationer kan du omvandla hello [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) filen i webbappen i Azure App Service. Du kan också använda XDT deklarationer tooadd privata tillägg tooenable anpassade web app administrativa åtgärder. Den här artikeln innehåller ett exempel PHP Manager webbprogramtillägg som möjliggör hantering av PHP-inställningar via ett webbgränssnitt.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a id="transform"></a>Avancerad konfiguration via ApplicationHost.config
Hej Apptjänst-plattformen ger flexibilitet och kontroll för webbappkonfigurationen. Även om hello standard IIS ApplicationHost.config-konfigurationsfilen inte är tillgängliga för direkt redigering i App Service stöder hello plattformen en deklarativ ApplicationHost.config transformeringen modell baserat på XML-dokumentet omvandling (XDT).

tooleverage funktionen transformering som du skapar en ApplicationHost.xdt-fil med XDT innehåll och placeras under hello platsens rot (d:\home\site) i hello [Kudu konsolen](https://github.com/projectkudu/kudu/wiki/Kudu-console). Du kan behöva toorestart hello Web App för ändringar tootake effekt.

hello följande applicationHost.xdt exempel visar hur tooadd en ny anpassad miljö variabeln tooa webbapp som använder PHP 5.4.

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

En loggfil med transformeringsstatus och information är tillgänglig från hello FTP roten under LogFiles\Transform.

Ytterligare exempel finns [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples).

**Obs!**<br />
Element från hello listan över moduler under `system.webServer` kan inte tas bort eller ordnas om, men tillägg toohello lista är möjliga.

## <a id="extend"></a>Utöka ditt webbprogram
### <a id="overview"></a>Översikt över privata webbprogramtillägg
Apptjänst stöder webbprogramtillägg som en utökningspunkt för administrativa åtgärder. Vissa funktioner för Apptjänst-plattformen implementeras i själva verket som förinstallerade tillägg. Medan hello förinstallerade plattformstillägg inte kan ändras kan du skapa och konfigurera privata tillägg för egna webbprogrammet. Den här funktionen använder också XDT deklarationer. hello viktiga steg för att skapa en privat webbprogramtillägg är hello följande:

1. Web app tillägget **innehåll**: skapa alla webbprogram som stöds av Apptjänst
2. Web app tillägget **deklaration**: skapa en ApplicationHost.xdt-fil
3. Web app tillägget **distribution**: placera innehållet i hello SiteExtensions mapp under`root`

Interna länkar för hello webbprogrammet ska peka tooa sökväg relativa toohello sökväg anges i hello ApplicationHost.xdt-filen. Ändra toohello ApplicationHost.xdt filer kräver web app återvinning.

**Obs**: Mer information om dessa nyckelelement finns på [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Ett detaljerat exempel är inkluderade tooillustrate hello steg för att skapa och aktivera en privat webbprogramtillägg. Hej källkoden för hello PHP Manager exemplet som följer kan hämtas från [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

### <a id="SiteSample"></a>Web app tillägget exempel: PHP Manager
PHP Manager är en webbprogramtillägg gör app webbadministratörer tooeasily visa och konfigurera sina PHP-inställningar med hjälp av ett webbgränssnitt i stället för att toomodify PHP ini-filer direkt. Gemensamma konfigurationsfiler för PHP inkluderar hello php.ini-filen finns under Program- och hello. user.ini fil i hello rotmappen för ditt webbprogram. Eftersom hello php.ini-filen inte är direkt redigeras på hello Apptjänst-plattformen hello tillägget för PHP Manager använder hello. user.ini filen tooapply ändras.

#### <a id="PHPwebapp"></a>hello Manager PHP-webbprogram
hello följer hello startsida hello PHP Manager-distribution:

![TransformSitePHPUI][TransformSitePHPUI]

Som du ser är en webbprogramtillägg precis som ett reguljärt webbprogram, men med ytterligare en ApplicationHost.xdt fil placeras i hello rotmapp hello webbprogram (Mer information om hello ApplicationHost.xdt filen finns i hello nästa avsnitt i det här artikel).

hello PHP Manager extension skapades med hello Visual Studio ASP.NET MVC 4-webbprogram mallen. hello visar följande vy från Solution Explorer hello strukturen för hello Manager PHP-tillägg.

![TransformSiteSolEx][TransformSiteSolEx]

hello är endast särskilda logik som krävs för i/o-fil tooindicate där hello wwwroot directory av hello webbprogram finns. Eftersom hello koden nedan, hello miljö variabeln ”HOME-visar hello rotsökvägen för webbapp och hello wwwroot sökvägen kan konstrueras genom att lägga till” site\wwwroot ”:

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


När du har hello katalogsökväg kan du använda reguljära filen tooread för i/o-åtgärder och skriva toofiles.

En punkt i försiktighet med webbprogramtillägg gäller hello hantering av interna länkar.  Om du har alla länkar i HTML-filer som ger absoluta sökvägar toointernal länkar på ditt webbprogram, måste du kontrollera att dessa länkar inledd med en Tilläggsnamn som din rot. Detta behövs eftersom hello roten för din anknytning nu ”/`[your-extension-name]`/” i stället för att bara ”/”, så alla interna länkar måste uppdateras. Anta exempelvis att din kod innehåller en länk toohello följande:

`"<a href="/Home/Settings">PHP Settings</a>"`

När hello länk är en del av en webbprogramtillägg kan måste hello länken vara i hello följande format:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

Du kan undvika det här kravet genom att antingen använda endast relativa sökvägar i ditt webbprogram eller i hello skiftläget för ASP.NET-program med hjälp av hello `@Html.ActionLink` metod som skapar hello lämpliga länkar du.

#### <a id="XDT"></a>Hej applicationHost.xdt fil
hello-koden för din webbprogramtillägg går under %HOME%\SiteExtensions\[en Tilläggsnamn]. Vi ringer upp dig hello tillägget roten.  

tooregister din webbprogramtillägg med hello applicationHost.config-filen måste tooplace en fil med namnet ApplicationHost.xdt i hello tillägget rot. hello innehållet hello ApplicationHost.xdt filen bör vara följande:

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

hello-namn som du väljer som en Tilläggsnamn ska ha hello samma namn som din rotmapp för tillägget.

Detta påverkar hello för att lägga till ett nytt program sökvägen toohello `system.applicationHost` listan med platser under hello SCM-webbplatsen. hello SCM platsen är en plats administration slutpunkt med särskilda autentiseringsuppgifter. Det har hello URL `https://[your-site-name].scm.azurewebsites.net`.  

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

### <a id="deploy"></a>Tillägget Web app-distribution
tooinstall din webbprogramtillägg du kan använda FTP toocopy alla hello-filer på din web application toohello `\SiteExtensions\[your-extension-name]` mappen av hello webbprogram som du vill använda tooinstall hello tillägg.  Vara säker på att toocopy hello ApplicationHost.xdt toothis plats samt. Starta om din tooenable hello webbprogramtillägg.

Du bör vara kan toosee din webbprogramtillägg på:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Observera att hello URL-Adressen ser ut precis som hello URL för ditt webbprogram, förutom att den använder HTTPS och innehåller ”.scm”.

Det är möjligt toodisable alla privata (inte förinstallerat)-tillägg för ditt webbprogram under utveckling och utredningar genom att lägga till en app-inställningar med hello nyckel `WEBSITE_PRIVATE_EXTENSIONS` och värdet `0`.

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png

