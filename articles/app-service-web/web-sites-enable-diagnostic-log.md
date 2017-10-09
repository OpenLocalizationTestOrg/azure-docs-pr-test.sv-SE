---
title: "aaaEnable diagnostikloggning för web apps i Azure App Service"
description: "Lär dig hur tooenable diagnostikloggning och lägga till instrumentation tooyour program, samt hur tooaccess hello information som loggas av Azure."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.openlocfilehash: 4b2903ff31cc93180552cf51196c33505ffbaf07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Aktivera diagnostikloggning för web apps i Azure App Service
## <a name="overview"></a>Översikt
Azure tillhandahåller inbyggd diagnostik tooassist med felsökning av ett [Apptjänst webbapp](http://go.microsoft.com/fwlink/?LinkId=529714). I den här artikeln lär du dig hur tooenable diagnostikloggning och lägga till instrumentation tooyour program, samt hur tooaccess hello information som loggas av Azure.

Den här artikeln använder hello [Azure Portal](https://portal.azure.com), Azure PowerShell och hello Azure-kommandoradsgränssnittet (Azure CLI) toowork med diagnostikloggar. Information om hur du arbetar med diagnostikloggar med Visual Studio finns i [felsöka Azure i Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Web serverdiagnostik- och programdiagnostik
App Service web apps ange diagnostikfunktion för att logga information från både hello webbservern och hello webbprogram. Dessa logiskt är indelade i **web serverdiagnostik** och **programdiagnostik**.

### <a name="web-server-diagnostics"></a>Web serverdiagnostik
Du kan aktivera eller inaktivera hello följande typer av loggar:

* **Detaljerad felloggning** -information om felet för HTTP-statuskoder som indikerar att en (statuskod 400 eller högre). Detta kan innehålla information som kan hjälpa dig att avgöra varför hello servern returnerade felkoden hello.
* **Kunde inte begäran spårning** -detaljerad information om misslyckade förfrågningar, inklusive en spårning av hello IIS-komponenter som används tooprocess hello begäran och hello tid i varje komponent. Detta kan vara användbart om du försöker tooincrease platsprestanda eller isolera vad som orsakar en specifika HTTP-fel toobe returneras.
* **Web Server-loggning** -Information om HTTP-transaktioner med hello [W3C utökat loggfilsformat](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Detta är användbart när du fastställer övergripande plats mätvärden, till exempel hello antal begäranden som hanteras eller hur många förfrågningar som kommer från en specifik IP-adress.

### <a name="application-diagnostics"></a>Programdiagnostik
Programdiagnostik kan du toocapture information som produceras av ett webbprogram. ASP.NET-program kan använda hello [System.Diagnostics.Trace](http://msdn.microsoft.com/library/36hhw2t6.aspx) programdiagnostik för klassen toolog information toohello loggar. Exempel:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Du kan hämta dessa loggar toohelp med felsökning vid körning. Mer information finns i [felsöka Azure web apps i Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

App Service web apps även logga information om distribution när du publicerar innehåll tooa webbapp. Detta sker automatiskt och det finns inga konfigurationsinställningar för loggning för distribution. Loggning för distribution kan du toodetermine varför en distribution misslyckades. Om du använder ett anpassat distributionsskriptet, kan du använda deployment loggning toodetermine varför hello skript inte.

## <a name="enablediag"></a>Hur tooenable diagnostik
tooenable diagnostik i hello [Azure Portal](https://portal.azure.com)går toohello bladet för din webbapp och klicka på **Inställningar > diagnostik loggar**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![En del loggar](./media/web-sites-enable-diagnostic-log/logspart.png)

När du aktiverar **programdiagnostik** du också välja hello **nivå**. Den här inställningen kan toofilter hello information avbildas för**informationsmeddelande**, **varning** eller **fel** information. Inställningar för**utförlig** ska logga alla information som genereras av programmet hello.

> [!NOTE]
> Till skillnad från ändra hello web.config Återanvänd fil aktiverar programdiagnostik eller ändrar diagnostiska loggningsnivåer inte hello tillämpningsdomän som hello programmet körs inom.
>
>

I hello [klassiska portalen](https://manage.windowsazure.com) webbapp **konfigurera** fliken kan du välja **lagring** eller **filsystem** för **webbserver loggning av**. Att välja **lagring** gör att du tooselect ett lagringskonto och en blobbbehållare som hello loggar kommer att skrivas till. Alla loggar för **plats diagnostik** skrivs toohello-filsystemet.

Hej [klassiska portalen](https://manage.windowsazure.com) webbapp **konfigurera** fliken finns också ytterligare inställningar för application diagnostics:

* **Filsystem** -butiker hello application diagnostics information toohello web app filsystem. Dessa filer kan nås av FTP eller hämtas som en Zip-arkiv med hjälp av hello Azure PowerShell eller Azure-kommandoradsgränssnittet (Azure CLI).
* **Table storage** -butiker hello diagnostik programinformationen i hello Azure Storage-konto och tabellen namnet.
* **BLOB storage** -butiker hello programinformation diagnostik i hello angivna Azure Storage-konto och blob-behållaren.
* **Kvarhållningsperioden** -som standard loggar tas inte bort automatiskt från **blob storage**. Välj **ange kvarhållning** och ange hello antalet dagar som tookeep loggarna om du inte vill tooautomatically ta bort loggar.

> [!NOTE]
> Om du [återskapa åtkomstnycklar för ditt lagringskonto](../storage/common/storage-create-storage-account.md), måste du återställa hello respektive loggning configuration toouse hello uppdateras nycklar. toodo detta:
>
> 1. I hello **konfigurera** ange hello respektive loggningsfunktionen för**av**. Spara dina inställningar.
> 2. Aktivera loggning toohello lagringsblob konto eller tabell igen. Spara dina inställningar.
>
>

En kombination av filsystemet, tabellagring eller blob-lagring kan aktiveras på hello samma tid och har enskild logg nivån konfigurationer. Exempelvis kanske vill toolog fel och varningar tooblob lagring som en långsiktig loggning lösning, samtidigt som filen systemloggning med utförlig nivå.

När alla tre lagringsplatser ange hello samma grundläggande information om händelser, **tabell lagring** och **blob storage** logga ytterligare information, till exempel hello instans-ID, tråd-ID och ett mer detaljerade tidsstämpel (skalstreck format) än loggning för**filsystem**.

> [!NOTE]
> Information som lagras i **tabell lagring** eller **blob storage** kan bara användas med en storage-klient eller ett program som kan arbeta direkt med dessa lagringssystem. Till exempel Visual Studio 2013 innehåller en lagringsutforskare som kan använda tooexplore tabell eller blob storage och HDInsight kan komma åt data som lagras i blob storage. Du kan också skriva ett program som har åtkomst till Azure Storage genom att använda en hello [Azure SDK](/downloads/#).
>
> [!NOTE]
> Diagnostiken kan även aktiveras från Azure PowerShell med hjälp av hello **Set AzureWebsite** cmdlet. Om du inte har installerat Azure PowerShell eller har inte konfigurerat den toouse din Azure-prenumeration finns [hur tooUse Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

## <a name="download"></a>Så här: hämta loggar
Diagnostisk information lagras toohello web app filsystem kan nås med hjälp av FTP. Det kan också hämtas som en Zip-arkiv med hjälp av Azure PowerShell eller hello Azure-kommandoradsgränssnittet.

hello katalogstruktur som hello loggfilerna lagras i är följande:

* **Programloggarna** -/LogFiles/program /. Den här mappen innehåller en eller flera textfiler som innehåller information som genereras av programloggning.
* **Det gick inte begäranden** -/ loggfiler/W3SVC ### /. Den här mappen innehåller en XSL-fil och en eller flera XML-filer. Se till att du hämtar hello XSL-fil i samma katalog som XML-filerna eftersom hello XSL-filen innehåller funktioner för att formatera och filtrera hello innehållet i hello XML hello filerna när de visas i Internet Explorer hello.
* **Detaljerade felloggar** -/LogFiles/DetailedErrors /. Den här mappen innehåller en eller flera .htm-filer som innehåller omfattande information för HTTP-fel som har inträffat.
* **Web Server-loggar** -/LogFiles/http/RawLogs. Den här mappen innehåller en eller flera textfiler som formaterats med hello [W3C utökat loggfilsformat](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Distributionsloggar** -loggfilerna/Git. Den här mappen innehåller loggar som genereras av interna hello distributionsprocesser som används av Azure-webbappar samt loggar för Git-distributioner.

### <a name="ftp"></a>FTP
tooaccess diagnostikinformation med FTP, besök hello **instrumentpanelen** av ditt webbprogram i hello [klassiska portalen](https://manage.windowsazure.com). I hello **snabböversikten** Använd hello **FTP diagnostikloggar** länka tooaccess hello loggfiler med hjälp av FTP. Hej **distribution/FTP-användare** post visar hello användarnamn som ska använda tooaccess hello FTP-plats.

> [!NOTE]
> Om hello **distribution/FTP-användare** transaktionen inte har angetts eller du har glömt hello lösenord för den här användaren kan du skapa en ny användare och lösenord med hjälp av hello **återställa distributionsbehörigheterna** länken i hello **snabböversikten** avsnitt i hello **instrumentpanelen**.
>
>

### <a name="download-with-azure-powershell"></a>Hämta med Azure PowerShell
toodownload hello loggfiler, starta en ny instans av Azure PowerShell och använda hello följande kommando:

    Save-AzureWebSiteLog -Name webappname

Detta sparar hello loggar för hello webbprogrammet som anges av hello **-namn** parametern tooa fil med namnet **logs.zip** i hello aktuella katalogen.

> [!NOTE]
> Om du inte har installerat Azure PowerShell eller har inte konfigurerat den toouse din Azure-prenumeration finns [hur tooUse Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="download-with-azure-command-line-interface"></a>Hämta med Azure-kommandoradsgränssnittet
toodownload hello loggfiler med hello Azure-kommandoradsgränssnittet, öppnar en ny kommandotolk, PowerShell, Bash eller terminalsession och skriver hello följande kommando:

    azure site log download webappname

Detta sparar hello loggar för hello webbprogrammet med namnet 'webappname' tooa fil med namnet **diagnostics.zip** i hello aktuella katalogen.

> [!NOTE]
> Om du inte har installerat hello Azure-kommandoradsgränssnittet (Azure CLI) eller inte har konfigurerat den toouse din Azure-prenumeration finns [hur tooUse Azure CLI](../cli-install-nodejs.md).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Så här: visa loggar i Application Insights
Programinsikter för Visual Studio innehåller verktyg för att filtrera och söka i loggar och för korrelation mellan hello loggar med begäranden och andra händelser.

1. Lägg till hello Application Insights SDK tooyour projektet i Visual Studio.
   * Högerklicka på projektet i Solution Explorer och välja Lägg till Application Insights. Du kommer att få vägledning genom steg som omfattar att skapa en Application Insights-resurs. [Läs mer](../application-insights/app-insights-asp-net.md)
2. Lägg till hello Trace Listener paketet tooyour projektet.
   * Högerklicka på projektet och välj Hantera NuGet-paket. Välj `Microsoft.ApplicationInsights.TraceListener` [Läs mer](../application-insights/app-insights-asp-net-trace-logs.md)
3. Överför ditt projekt och kör det toogenerate loggdata.
4. I hello [Azure Portal](https://portal.azure.com/), bläddra tooyour ny Application Insights-resurs och öppna **Sök**. Du ser ditt loggdata tillsammans med begäran, användning och andra telemetri. Vissa telemetri kan ta några minuter tooarrive: Klicka på Uppdatera. [Läs mer](../application-insights/app-insights-diagnostic-search.md)

[Mer information om prestanda spårning med Application Insights](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a>Så här: strömma loggar
När du utvecklar ett program, är det ofta användbara toosee loggningsinformation i nära realtid. Detta kan åstadkommas med streaming loggning information tooyour utvecklingsmiljö med hjälp av Azure PowerShell eller hello Azure-kommandoradsgränssnittet.

> [!NOTE]
> Vissa typer av loggning buffert skriva toohello loggfil, vilket kan leda till oordnade händelser i hello dataströmmen. Exempelvis kan en loggpost för program som uppstår när en användare besöker en sida visas i hello dataströmmen innan hello motsvarande http-loggpost för hello begäran.
>
> [!NOTE]
> Loggen strömning direktuppspelas också information som skrivs tooany textfil som lagras i hello **D:\\hem\\loggfiler\\**  mapp.
>
>

### <a name="streaming-with-azure-powershell"></a>Strömning med Azure PowerShell
toostream loggningsinformation, starta en ny instans av Azure PowerShell och använda hello följande kommando:

    Get-AzureWebSiteLog -Name webappname -Tail

Detta ansluter toohello webbprogram som anges av hello **-namn** parametern och börja strömning information toohello PowerShell-fönstret när händelser inträffar på hello webbprogrammet. All information som skrivs toofiles som slutar på .txt, .log eller .htm som lagras i hello /LogFiles directory (d:/home/loggfilerna) kommer att strömmas toohello lokala konsolen.

toofilter specifika händelser, till exempel fel, Använd hello **-meddelandet** parameter. Exempel:

    Get-AzureWebSiteLog -Name webappname -Tail -Message Error

toofilter viss loggning typer, till exempel HTTP, använda hello **-sökvägen** parameter. Exempel:

    Get-AzureWebSiteLog -Name webappname -Tail -Path http

toosee en lista över tillgängliga sökvägar, Använd hello ListPath - parametern.

> [!NOTE]
> Om du inte har installerat Azure PowerShell eller har inte konfigurerat den toouse din Azure-prenumeration finns [hur tooUse Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="streaming-with-azure-command-line-interface"></a>Strömning med Azure-kommandoradsgränssnittet
toostream loggningsinformation öppnar en ny kommandotolk, PowerShell, Bash eller terminalsession och skriver hello följande kommando:

    azure site log tail webappname

Detta ansluta toohello webbprogrammet med namnet 'webappname' och påbörja strömning toohello informationsfönster när händelser inträffar på hello webbprogrammet. All information som skrivs toofiles som slutar på .txt, .log eller .htm som lagras i hello /LogFiles directory (d:/home/loggfilerna) kommer att strömmas toohello lokala konsolen.

toofilter specifika händelser, till exempel fel, Använd hello **--Filter** parameter. Exempel:

    azure site log tail webappname --filter Error

toofilter viss loggning typer, till exempel HTTP, använda hello **--sökväg** parameter. Exempel:

    azure site log tail webappname --path http

> [!NOTE]
> Om du inte har installerat hello Azure-kommandoradsgränssnittet eller har inte konfigurerat den toouse din Azure-prenumeration finns [hur tooUse Azure-kommandoradsgränssnittet](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a>Så här: Förstå diagnostik loggar
### <a name="application-diagnostics-logs"></a>Programloggarna för diagnostik
Programdiagnostik lagrar information i ett specifikt format för .NET-program, beroende på om du lagrar loggar toohello filsystem, tabellagring, eller blob storage. hello basuppsättning med data som lagras är hello samma i alla tre lagringstyper av - hello datum och tid hello händelsen inträffade, hello process-ID som producerade hello händelse hello händelsetyp (information, varning, fel) och hello händelsemeddelandet.

**Filsystem**

Varje rad loggade toohello filsystem eller tas emot med streaming blir i hello följande format:

    {Date}  PID[{process id}] {event type/level} {message}

En felhändelse visas exempelvis liknande toohello följande:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on hello page!

Loggning toohello filsystemet innehåller hello mest grundläggande information av hello tre tillgängliga metoder, tillhandahåller endast hello tid, process-id, Händelsenivå och meddelandet.

**Table Storage**

När du loggar tootable lagring är ytterligare egenskaper används toofacilitate söker hello data som lagras i hello tabellen, samt mer detaljerad information om hello-händelse. hello används följande egenskaper (kolumner) för varje entitet (rad) lagras i hello-tabellen.

| Egenskapsnamn | Value-format |
| --- | --- |
| PartitionKey |Datum/tid för hello händelsen i yyyyMMddHH format |
| RowKey |Ett GUID-värde som unikt identifierar den här entiteten |
| tidsstämpel |hello datum och tid då hello händelsen inträffade |
| EventTickCount |hello datum och tid då hello händelsen inträffade i skalstreck format (större precision) |
| ApplicationName |hello webbprogrammets namn |
| Nivå |Händelsenivå (t.ex. fel, varning, information) |
| Händelse-ID |hello händelse-ID för den här händelsen<p><p>Standard too0 om inget har angetts |
| InstanceId |Instans av hello webbprogram som hello även inträffade på |
| Process-ID |Process-ID |
| tid |hello tråd-ID för hello tråd som producerade hello-händelse |
| Meddelande |Detaljerat meddelande |

**Blob Storage**

När du loggar tooblob lagring, lagras data i kommaavgränsade värden (CSV)-format. Liknande tootable lagring, extra fälten är loggade tooprovide mer detaljerad information om hello-händelse. hello används följande egenskaper för varje rad i hello CSV:

| Egenskapsnamn | Value-format |
| --- | --- |
| Date |hello datum och tid då hello händelsen inträffade |
| Nivå |Händelsenivå (t.ex. fel, varning, information) |
| ApplicationName |hello webbprogrammets namn |
| InstanceId |Instans av hello webbprogram som hello händelsen inträffade på |
| EventTickCount |hello datum och tid då hello händelsen inträffade i skalstreck format (större precision) |
| Händelse-ID |hello händelse-ID för den här händelsen<p><p>Standard too0 om inget har angetts |
| Process-ID |Process-ID |
| tid |hello tråd-ID för hello tråd som producerade hello-händelse |
| Meddelande |Detaljerat meddelande |

hello-data som lagras i en blob som ser liknande toohello följande:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> hello första raden i hello loggen innehåller kolumnrubriker hello som representeras i det här exemplet.
>
>

### <a name="failed-request-traces"></a>Det gick inte begäranden
Misslyckade begäranden lagras i XML-filer med namnet **fr ### .xml**. toomake den enklare tooview hello loggas information, en XSL-formatmallar med namnet **freb.xsl** tillhandahålls i hello samma katalog som hello XML-filer. Öppna en hello XML-filer i Internet Explorer använder hello XSL stylesheet tooprovide formaterade visningen av hello spårningsinformation. Detta visas liknande toohello följande:

![misslyckade begäranden som visas i hello webbläsare](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Detaljerade felloggar
Detaljerade felloggar är HTML-dokument som innehåller mer detaljerad information om HTTP-fel som har inträffat. Eftersom de bara HTML-dokument, kan de visas i en webbläsare.

### <a name="web-server-logs"></a>Webbserverloggarna
Hej webbserverloggarna är formaterad med hello [W3C utökat loggfilsformat](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Den här informationen kan läsas med hjälp av en textredigerare eller parsas med verktyg, till exempel [Loggparser](http://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> hello loggar som genereras av Azure-webbappar stöder inte hello **s-computername**, **s-IP-**, eller **cs-version** fält.
>
>

## <a name="nextsteps"></a>Nästa steg
* [Hur tooMonitor Web Apps](http://docs.microsoft.com/en-us/azure/app-service-web/web-sites-monitor)
* [Felsöka Azure web apps i Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analysera Webbprogramloggar i HDInsight](http://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
>
>

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)
* En guide toohello ändring av hello gamla portalen toohello nya portalen finns: [referens för navigering hello Azure-portalen](http://go.microsoft.com/fwlink/?LinkId=529715)
