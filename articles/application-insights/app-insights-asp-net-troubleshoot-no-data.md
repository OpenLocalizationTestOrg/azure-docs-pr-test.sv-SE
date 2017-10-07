---
title: "aaaTroubleshooting inga data - Programinsikter för .NET"
description: "Inte visa data i Azure Application Insights? Försök här."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: e231569f-1b38-48f8-a744-6329f41d91d3
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 261c25c89884c46e41bbc305474ac854360221ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-no-data---application-insights-for-net"></a>Felsökning utan data, Application Insights för .NET
## <a name="some-of-my-telemetry-is-missing"></a>Vissa av min telemetri saknas
*I Application Insights visas bara en del av hello händelser som genereras av min app.*

* Om du alltid ser hello samma bråk, förmodligen på grund av tooadaptive [provtagning](app-insights-sampling.md). tooconfirm, Öppna sökning (från hello översikt bladet) och titta på en instans av en begäran eller händelse. Klicka på ”...” tooget fullständig egenskapen information längst ned hello hello avsnittet för egenskaper. Om begär antal > 1 sedan provtagning är i drift. 
* I annat fall är det möjligt att du träffa en [data hastighetsbegränsning](app-insights-pricing.md#limits-summary) för din prisavtal. Dessa begränsningar tillämpas per minut.

## <a name="no-data-from-my-server"></a>Inga data från servern
*Jag har installerat mitt program på webbservern och nu visas inte alla telemetri från den. Det fungerade OK på datorn dev.*

* Förmodligen en brandvägg problemet. [Ange brandväggsundantag för Application Insights toosend data](app-insights-ip-addresses.md).

*Jag [installerat statusövervakaren](app-insights-monitor-performance-live-website-now.md) i Min server toomonitor befintliga webbprogram. Det visas inte några resultat.*

* Se [felsökning statusövervakaren](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights). 

## <a name="q01"></a>Alternativet ”Lägg till Application Insights' i Visual Studio
*När jag högerklickar på ett befintligt projekt i Solution Explorer, visas inte alla alternativ som Application Insights.*

* Inte alla typer av .NET-projekt som stöds av hello verktyg. Webb-och WCF stöds. För andra projekttyper, till exempel skrivbord eller tjänsten program kan du fortfarande [manuellt lägga till ett projekt för Application Insights SDK tooyour](app-insights-windows-desktop.md).
* Kontrollera att du har [Visual Studio 2013 Update 3 eller senare](http://go.microsoft.com/fwlink/?LinkId=397827). Det finns förinstallerat med utvecklare Analytics verktyg som ger hello Application Insights SDK.
* Välj **verktyg**, **tillägg och uppdateringar** och kontrollera att **Analytics utvecklingsverktyg** är installerat och aktiverat. I så fall, klickar du på **uppdateringar** toosee om det finns en uppdatering.
* Öppna dialogrutan för hello nytt projekt och välj ASP.NET-webbprogram. Om du ser hello Application Insights alternativ och sedan hello-verktygen är installerade. Om inte, försök att avinstallera och sedan installera hello Application Insights Tools.

## <a name="q02"></a>Det gick inte att lägga till Application Insights
*När jag försöker tooadd Application Insights tooan befintligt projekt, visas ett felmeddelande.*

Troliga orsaker:

* Kommunikation med hello Application Insights-portalen misslyckades. eller
* Det finns problem med ditt Azure-konto;
* Du har bara [läsbehörighet toohello prenumeration eller grupp där du försökte toocreate hello ny resurs](app-insights-resources-roles-access-control.md).

Åtgärda:

* Kontrollera att du har angett inloggningsuppgifter för hello höger Azure-konto. 
* I webbläsaren och kontrollera att du har åtkomst till toohello [Azure-portalen](https://portal.azure.com). Öppna inställningar och se om det finns några begränsningar.
* [Lägg till Application Insights tooyour befintligt projekt](app-insights-asp-net.md): I Solution Explorer högerklickar du på projektet och välj ”Lägg till Application Insights”.
* Om det fortfarande inte fungerar kan du följa hello [manuell proceduren](app-insights-windows-services.md) tooadd en resurs i hello portal och Lägg sedan till tooyour hello SDK-projektet. 

## <a name="emptykey"></a>Ett felmeddelande ”Instrumentation nyckeln kan inte vara tomt”
Något verkar ha gått fel när du installerar Application Insights eller kanske loggning nätverkskort.

Högerklicka på projektet i Solution Explorer och välj **Programinsikter > Konfigurera Application Insights**. Du får en dialogruta där du toosign i tooAzure och skapa en Application Insights-resurs eller återanvända en befintlig.

## <a name="NuGetBuild"></a>”NuGet-paket saknas” på min build-server
*Allt bygger OK när jag felsökningsläget datorn utveckling, men får ett NuGet-fel på hello build-servern.*

Se [NuGet-Paketåterställning](http://docs.nuget.org/Consume/Package-Restore) och [automatisk Paketåterställning](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore).

## <a name="missing-menu-command-tooopen-application-insights-from-visual-studio"></a>Saknade menyn kommandot tooopen Application Insights från Visual Studio
*När jag högerklickar på projektet Solution Explorer, visas inte några Application Insights-kommandon eller en öppen Application Insights-kommandot finns inte.*

Troliga orsaker:

* Om du har skapat hello Application Insights-resursen manuellt, eller om hello projektet är av en typ som inte stöds av hello Application Insights verktyg.
* hello Developer Analytics verktyg är inaktiverade i din Visual Studio. 
* Din Visual Studio är äldre än 2013 Update 3.

Åtgärda:

* Kontrollera din version av Visual Studio 2013 update 3 eller senare.
* Välj **verktyg**, **tillägg och uppdateringar** och kontrollera att **Developer Analytics verktyg** är installerat och aktiverat. I så fall, klickar du på **uppdateringar** toosee om det finns en uppdatering.
* Högerklicka på projektet i Solution Explorer. Om du ser hello kommandot **Application Insights > Konfigurera Application Insights**, använda den tooconnect ditt projekt toohello resurs i hello Application Insights-tjänsten.

I annat fall stöds din projekttypen direkt inte av hello Application Insights verktyg. toosee telemetri inloggning toohello [Azure-portalen](https://portal.azure.com), Välj Application Insights hello vänstra navigeringsfältet och välj ditt program.

## <a name="access-denied-on-opening-application-insights-from-visual-studio"></a>”Åtkomst nekad” på Öppna Application Insights från Visual Studio
*hello öppna Programinsikter menykommandot tar mig toohello Azure-portalen, men jag får felmeddelandet ”åtkomst nekad”.*

![](./media/app-insights-asp-net-troubleshoot-no-data/access-denied.png)

hello Microsoft inloggning som senast användes på din standardwebbläsare har inte åtkomst för[hello resurs som skapades när Application Insights lades till toothis app](app-insights-asp-net.md). Det finns två troliga orsaker: 

* Du har mer än ett Microsoft-konto – kanske en arbete och personligt Microsoft-konto? hello-inloggning som senast användes på din standardwebbläsare var för ett annat konto än hello som har åtkomst för[lägga till Application Insights toohello projekt](app-insights-asp-net.md). 
  
  * Korrigera: Klicka på ditt namn i övre högra av hello webbläsarfönster och logga ut. Logga sedan in med hello-konto som har åtkomst. Klicka på Application Insights på hello vänstra navigeringsfältet och välj din app.
* Någon annan lagt till Application Insights toohello projekt och glömt toogive du [åtkomst toohello resursgruppen](app-insights-resources-roles-access-control.md) i som det skapades. 
  
  * Lösning: Om de används ett organisationskonto de kan lägga till dig toohello team; eller kan de beviljar du enskilda åtkomst toohello resursgruppen.

## <a name="asset-not-found-on-opening-application-insights-from-visual-studio"></a>'Tillgången inte att hitta' på Öppna Application Insights från Visual Studio
*hello öppna Programinsikter menykommandot tar mig toohello Azure-portalen, men ett felmeddelande ”det gick inte att hitta tillgången'.*

Troliga orsaker:

* hello Application Insights-resurs för tillämpningsprogrammet har tagits bort; eller
* hello instrumentation nyckel angavs eller ändrades i ApplicationInsights.config genom att redigera den direkt, utan att uppdatera hello projektfilen. 

hello instrumentation nyckeln i ApplicationInsights.config kontroller där hello telemetri skickas. En rad i projektfilen hello styr vilken resurs som öppnas när kommandot hello i Visual Studio. 

Åtgärda:

* Högerklicka på hello-projektet i Solution Explorer och välj Application Insights, konfigurera Application Insights. I dialogrutan för hello, kan du antingen välja toosend telemetri tooan befintlig resurs eller skapa en ny. Eller:
* Öppna hello resurs direkt. Logga in för[hello Azure-portalen](https://portal.azure.com), klicka på Application Insights hello vänstra navigeringsfältet och välj sedan din app.

## <a name="where-do-i-find-my-telemetry"></a>Var hittar jag telemetri?
*Jag har loggat in toohello [Microsoft Azure-portalen](https://portal.azure.com), och jag tittar på hello Azure startinstrumentpanel. Så var hittar jag mina Application Insights-data?*

* Klicka på Application Insights, sedan din app name hello vänstra navigeringsfältet. Om du inte har några projekt det, måste du för[lägga till eller konfigurera Application Insights i webbprojektet](app-insights-asp-net.md).
  
    Det ser du vissa sammanfattning diagram. Du kan klicka på igenom dem toosee mer detaljerat.
* I Visual Studio när du felsöker appen Klicka hello Application Insights.

## <a name="q03"></a>Ingen server-data (eller inga data alls)
*Jag har kört min app och sedan öppnas hello Application Insights-tjänsten i Microsoft Azure, men alla hello diagram visar ' Lär dig hur toocollect... ”eller” inte konfigurerad ”.* Eller, *endast vyn sida och användardata, men inga serverdata.*

* Kör ditt program i felsökningsläge i Visual Studio (F5). Använd programmet hello så som toogenerate vissa telemetri. Kontrollera att du kan se händelserna som loggas i utdatafönstret för hello Visual Studio. 
  
    ![](./media/app-insights-asp-net-troubleshoot-no-data/output-window.png)
* Öppna i hello Application Insights-portalen, [diagnostiska Sök](app-insights-diagnostic-search.md). Data visas vanligtvis här först.
* Klicka på uppdateringsknappen hello. hello bladet uppdaterar sig själv med jämna mellanrum, men du kan också göra det manuellt. intervall för uppdatering av hello är längre för större tidsintervall.
* Kontrollera hello instrumentation nycklar matchar. På hello huvudblad för din app i hello Application Insights-portalen i hello **Essentials** listrutan, titta på **Instrumentation nyckeln**. I ditt projekt i Visual Studio, öppna ApplicationInsights.config och hitta hello `<instrumentationkey>`. Kontrollera att hello två nycklar är lika. Om inte:
  
  * Hello-portalen klickar du på Application Insights och leta efter hello appresursen med rätt nyckel för hello; eller
  * Högerklicka på hello projekt i Visual Studio Solution Explorer och välj Application Insights, konfigurera. Återställa hello app toosend telemetri toohello rätt resurs.
  * Om du inte hittar hello matchar nycklar, kontrollera att du använder hello samma inloggningsuppgifter i Visual Studio som toohello portal.
    
    ![](./media/app-insights-asp-net-troubleshoot-no-data/ikey-check.png)
* I hello [Microsoft Azure startinstrumentpanel](https://portal.azure.com), titta på hello tjänstens hälsa för kartan. Om det finns vissa uppgifter som avisering, vänta tills de returnerade tooOK och Stäng och öppna bladet din Application Insights-programmet igen.
* Kontrollera också [vår status blogg](http://blogs.msdn.com/b/applicationinsights-status/).
* Har du skriva kod för hello [serversidan SDK](app-insights-api-custom-events-metrics.md) som kan ändra hello instrumentation nyckeln i `TelemetryClient` instanser eller i `TelemetryContext`? Eller har du skriver en [filter eller provtagning](app-insights-api-filtering-sampling.md) som kanske filtreras för mycket?
* Om du redigerade ApplicationInsights.config noggrant kontrollera hello konfigurationen för [TelemetryInitializers och TelemetryProcessors](app-insights-api-filtering-sampling.md). En typ av fel namn eller parameter som kan orsaka hello SDK toosend inga data.

## <a name="q04"></a>Inga data om sidvisningar, webbläsare, användning
*Data i serversvarstid och serverbegäranden diagram, men inga data i tid för sidhämtning vyn, eller visas i hello webbläsare eller användning blad.*

hello data kommer från skript i hello webbsidor. 

* Om du har lagt till Application Insights tooan befintligt webbprojekt, [är tooadd hello skript manuellt](app-insights-javascript.md).
* Se till att Internet Explorer inte visar webbplatsen i kompatibilitetsläge.
* Funktionen hello webbläsarens debug (F12 på vissa webbläsare, väljer nätverket) tooverify som data skickas för`dc.services.visualstudio.com`.

## <a name="no-dependency-or-exception-data"></a>Inga beroenden eller undantag data
Se [beroendetelemetri](app-insights-asp-net-dependencies.md) och [undantagstelemetri](app-insights-asp-net-exceptions.md).

## <a name="no-performance-data"></a>Inga prestandadata
Prestandadata (CPU, IO-frekvens och så vidare) är tillgängligt för [Java-webbtjänster](app-insights-java-collectd.md), [Windows-skrivbordsappar](app-insights-windows-desktop.md), [av IIS-program och tjänster om du installerar statusövervakaren](app-insights-monitor-performance-live-website-now.md), och [Azure-molntjänster](app-insights-azure.md). Du hittar den under inställningar för servrar.

Det är inte tillgänglig för Azure websites.

## <a name="no-server-data-since-i-published-hello-app-toomy-server"></a>Inga (server) data eftersom publicerat hello toomy applikationsserver
* Kontrollera att du verkligen kopieras alla hello Microsoft. ApplicationInsights DLL: er toohello server, tillsammans med Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll
* I brandväggen, du kanske för[öppna vissa TCP-portarna](app-insights-ip-addresses.md#data-access-api).
* Om du toouse en proxy-toosend utanför företagets nätverk, ange [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) i Web.config
* Windows Server 2008: Kontrollera att du har installerat hello följande uppdateringar: [KB2468871](https://support.microsoft.com/kb/2468871), [KB2533523](https://support.microsoft.com/kb/2533523), [KB2600217](https://support.microsoft.com/kb/2600217).

## <a name="i-used-toosee-data-but-it-has-stopped"></a>Jag använde toosee data, men den har stoppats
* Kontrollera hello [status blogg](http://blogs.msdn.com/b/applicationinsights-status/).
* Har du uppnått den månatliga kvoten för datapunkter? Öppna hello inställningar/kvoten och priser toofind ut. I så fall, kan du uppgradera din plan eller betala för extra kapacitet. Se hello [priser schemat](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="i-dont-see-all-hello-data-im-expecting"></a>Alla hello data jag förväntade visas inte
Om ditt program skickar stora mängder data och du använder hello Application Insights SDK för ASP.NET version 2.0.0-beta3 eller senare, hello [anpassningsbar provtagning](app-insights-sampling.md) funktion kan fungera och skicka endast en del av din telemetri. 

Du kan inaktivera det, men detta rekommenderas inte. Beräkningarna fungerar så att relaterade telemetri korrekt överförs, för att ställa diagnoser. 

## <a name="wrong-geographical-data-in-user-telemetry"></a>Felaktiga geografiska data i användaren telemetri
hello ort, region och land dimensioner härleds från IP-adresser och alltid är inte korrekt.

## <a name="exception-method-not-found-on-running-in-azure-cloud-services"></a>Undantaget ”metoden hittades inte” vid körning i Azure Cloud Services
Utvecklade du för .NET 4.6? 4.6 stöds inte automatiskt i Azure Cloud Services-roller. [Installera 4.6 för varje roll](../cloud-services/cloud-services-dotnet-install-dotnet.md) innan du kör din app.

## <a name="still-not-working"></a>Fortfarande fungerar inte...
* [Application Insights-forum](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

