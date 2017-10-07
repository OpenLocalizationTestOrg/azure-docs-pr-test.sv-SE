---
title: aaaFlighting distribution (betatestning) i Azure App Service
description: "Lär dig hur tooflight nya funktioner i din app eller beta testa dina uppdateringar i den här självstudiekursen för slutpunkt till slutpunkt. Den samlar Apptjänst funktioner som kontinuerlig publicering, platser, routning av nätverkstrafik och Application Insights-integration."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 17953c51-38f8-442d-bb0b-f69c1542f0e9
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cephalin
ms.openlocfilehash: e83477b1fe46be09e5baa7bc2bd239b840b05cf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Förhandsversionstestning distribution (betatestning) i Azure App Service
De här självstudierna visar hur toodo *förhandsversionstestning distributioner* hello genom att integrera olika funktioner i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) och [Azure Application Insights](/services/application-insights/).

*Förhandsversionstestning* är en Distributionsprocess som validerar en ny funktion eller ändra med ett begränsat antal verkliga kunder och är en större testning i form av produktionsscenario. Det är akin toobeta testning och kallas ibland ”kontrollerade test svarta”. Många stora företag med webben använda den här metoden för att få tidig validering på deras app-uppdateringar i sina praxis [flexibel utveckling](https://en.wikipedia.org/wiki/Agile_software_development). Azure Apptjänst kan du toointegrate test i produktion med kontinuerlig publicering och Application Insights tooimplement hello samma DevOps-scenario. Den här metoden fördelar:

* **Få verkliga feedback *innan* uppdateringar som är utgivna tooproduction** -hello endast sak bättre än får feedback så fort du släpper har blivit feedback innan du släpper. Du kan testa uppdateringar med verkliga användaraktiviteten och beteenden tidigt du önskar i hello produktens livscykel.
* **Förbättra [kontinuerlig test-driven utveckling (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** – genom att integrera test i produktion med kontinuerlig integrering och instrumentation med Application Insights validering av användare händer tidigt och automatiskt i din produktens livscykel. Detta minskar tiden investeringar i manuell testkörning.
* **Optimera testa arbetsflödet** -genom att automatisera test i produktion med kontinuerlig övervakning instrumentation, du kan potentiellt göra hello målen för olika typer av testerna i en enda process som [integrering](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [användbarhet](https://en.wikipedia.org/wiki/Usability_testing), tillgänglighet, lokalisering, [prestanda](https://en.wikipedia.org/wiki/Software_performance_testing), [säkerhet](https://en.wikipedia.org/wiki/Security_testing), och [ godkännande](https://en.wikipedia.org/wiki/Acceptance_testing).

En flighting distribution inte bara om routning live trafik. Typen av distribution vill du toogain insight så snabbt som möjligt, vare sig det är ett oväntat fel, prestandaförsämring, user experience problem. Kom ihåg att du arbetar med verkliga kunder. Så toodo den höger, måste du se till att du har konfigurerat din flighting distribution toogather alla hello-data som du behöver i ordning toomake ett välgrundat beslut för din nästa steg. De här självstudierna visar hur toocollect data med Application Insights, men du kan använda New Relic eller andra tekniker som passar ditt scenario.

## <a name="what-you-will-do"></a>Vad du ska göra
I den här kursen får du lära dig hur toobring hello följande scenarier tillsammans tootest Apptjänst-app i produktion:

* [Vidarebefordra trafik för produktion](app-service-web-test-in-production-get-start.md) tooyour beta-app
* [Instrumentera appen](../application-insights/app-insights-web-track-usage.md) tooobtain användbara mätvärden
* Distribuera appen beta och spåra aktiva app mått kontinuerligt
* Jämför mått mellan hello produktionsprogrammet och hello beta app toosee hur koden ändras översätta tooresults

## <a name="what-you-will-need"></a>Vad du behöver
* Ett Azure-konto
* En [GitHub](https://github.com/) konto
* Visual Studio 2015 – du kan ladda ned hello [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
* Git Shell (installeras med [GitHub för Windows](https://windows.github.com/))-på så sätt kan du toorun båda hello Git och PowerShell-kommandon i hello samma session
* Senaste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits
* Grundläggande förståelse av hello följande:
  * [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) malldistribution (se [distribuerar ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> Du behöver ett Azure-konto toocomplete den här kursen:
>
> * Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/) – du får kredit du kan använda tootry ut betald Azure-tjänster och även när de används du kan behålla hello kontot och använda kostnadsfria Azure-tjänster, till exempel Web Apps.
> * Du kan [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -din Visual Studio-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.
>
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
>
>

## <a name="set-up-your-production-web-app"></a>Konfigurera ditt webbprogram för produktion
> [!NOTE]
> hello-skriptet som används i den här självstudiekursen konfigurerar automatiskt kontinuerlig publicering från GitHub-lagringsplatsen. Detta kräver att dina GitHub-autentiseringsuppgifter lagras redan i Azure, annars hello skripta distribution misslyckas vid försök tooconfigure inställningar för källkontrollen för hello web apps.
>
> toostore din GitHub autentiseringsuppgifter i Azure, skapa en webbapp i hello [Azure Portal](https://portal.azure.com/) och [konfigurera GitHub distribution](app-service-continuous-deployment.md). Du behöver bara toodo detta en gång.
>
>

Du har ett program som körs live i Azure i ett typiskt scenario för DevOps, och du vill toomake ändringar tooit genom kontinuerlig publicering. I det här scenariot distribuerar du tooproduction en mall som du har utvecklats och testats.

1. Skapa din egen förgrening av hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) databasen. Information om hur du skapar din förgrening finns [duplicera en Repo](https://help.github.com/articles/fork-a-repo/). När din förgrening har skapats kan se du den i webbläsaren.

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Öppna en session för Git-gränssnittet. Om du inte har Git Shell ännu, installera [GitHub för Windows](https://windows.github.com/) nu.
3. Skapa en lokal kloning av din förgrening genom att köra följande kommando hello:

     Git-klon https://github.com/<your_fork>/ToDoApp.git
4. När du har din lokala klona navigera för*&lt;repository_root >*\ARMTemplates och kör hello deploy.ps1 skriptet med ett unikt suffix som visas nedan:

     .\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix >
5. När du uppmanas önskat ange hello användarnamn och lösenord för åtkomst till databasen. Spara autentiseringsuppgifter för databasen eftersom du behöver toospecify dem igen när uppdatering hello resursgruppen.

   Du bör se hello etablering förloppet för olika Azure-resurser. När distributionen är klar kommer hello skriptet Starta hello program i hello webbläsare och ger dig ett eget signal.
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. Tillbaka i sessionen Git Shell, kör du:

     . \swap – namn ToDoApp < your_suffix >

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. När hello skriptet är klar går du tillbaka toobrowse toohello frontend-adress (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) toosee hello program som körs i produktion.
8. Logga in på hello [Azure Portal](https://portal.azure.com/) och ta en titt på vad som har skapats.

   Du bör vara kan toosee två web apps i hello samma resursgrupp, en med hello `Api` suffix i hello namn. Om du tittar på hello resursgruppvy visas också hello SQL-databas och server, hello App Service-plan och hello fristående fack för hello web apps. Bläddra igenom hello olika resurser och jämför dem med * &lt;repository_root >*\ARMTemplates\ProdAndStage.json toosee hur de är konfigurerade i hello mallen.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Du har lagt upp hello produktionsprogrammet.  Nu kan vi anta att du får feedback att användbarhet är dålig för hello app. Så att du bestämmer tooinvestigate. Du kommer tooinstrument app-toogive du feedback.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Undersök: Instrumentera klientappen för övervakning/mått
1. Öppna * &lt;repository_root >*\src\MultiChannelToDo.sln i Visual Studio.
2. Återställa alla Nuget-paket genom att högerklicka på lösningen > **hantera NuGet-paket för lösningen** > **återställa**.
3. Högerklicka på **MultiChannelToDo.Web** > **Lägg till Application Insights Telemetry** > **konfigurera inställningarna för** > ändra resursgrupp tooToDoApp*&lt;your_suffix >* > **Lägg till Application Insights tooProject**.
4. Öppna hello bladet för hello i hello Azure-portalen, **MultiChannelToDo.Web** Application Insights-resurs. I hello **programhälsan** del, klickar du på **Lär dig hur toocollect webbläsare sidinläsning data** > Kopiera kod.
5. Lägg till hello kopieras JS instrumentation kod för*&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml precis innan du stänger hello `<heading>` tagg. Den ska innehålla hello unika instrumentation nyckeln för Application Insights-resurs.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. Skicka anpassade händelser tooApplication Insights för musklickningar genom att lägga till hello följande kod toohello längst ned i brödtext:

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   JavaScript-kodfragment skickar en anpassad händelse tooApplication insikter varje gång en användare klickar någonstans i hello webbapp.
7. Git Shell, genomför och skicka din ändringar tooyour förgrening i GitHub. Vänta sedan klienter toorefresh webbläsare.

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. Växlingen hello distribuerad app ändringar tooproduction:

     . \swap – namn ToDoApp < your_suffix >
9. Bläddra toohello Application Insights-resurs som du har konfigurerat. Klicka på anpassade händelser.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   Om du inte ser mätvärden för anpassade händelser, Vänta några minuter och klicka på **uppdatera**.

Anta att du ser ett diagram som nedan:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

Och hello händelse rutnätet nedan:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

Enligt tooyour ToDoApp programkod hello **knappen** händelse motsvarar toohello Skicka-knapp och hello **indata** händelse motsvarar toohello textruta. Hittills vara saker meningsfullt. Men det verkar som om det är mycket musklickningar och mycket få klick på hello-arbetsuppgifter (hello **LI** händelser).

Baserat på, du formuläret den hypotesen några användare kan blandas ihop vilken del av hello Användargränssnittet är klickbara och eftersom hello markören är formaterad för textmarkering när den är placerad på hello objekt och deras ikoner.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Det här är ett exempel på contrived. Dock du är en förbättring tooyour app för pågående toomake och utför sedan en flighting distribution tooget användbarhet feedback från live-kunder.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Instrumentera din server-app för övervakning/mått
Detta är en tangens eftersom hello-scenariot som visas i den här självstudiekursen bara behandlar hello-klientappen. För fullständighetens skull kommer du dock ställa in hello serversidan app.

1. Högerklicka på **MultiChannelToDo** > **Lägg till Application Insights Telemetry** > **konfigurera inställningarna för** > ändra resursgrupp tooToDoApp*&lt;your_suffix >* > **Lägg till Application Insights tooProject**.
2. Git Shell, genomför och skicka din ändringar tooyour förgrening i GitHub. Vänta sedan klienter toorefresh webbläsare.

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. Växlingen hello distribuerad app ändringar tooproduction:

     . \swap – namn ToDoApp < your_suffix >

Klart!

## <a name="investigate-add-slot-specific-tags-tooyour-client-app-metrics"></a>Undersök: Lägg till plats-specifika taggar tooyour klienten app mått
I det här avsnittet konfigurerar du hello annan distribution fack toosend fack-specifika telemetri toohello samma Application Insights-resurs. På så sätt kan du jämföra telemetri data mellan trafik från olika platser (distributionsmiljöer) tooeasily Se hello din app ändringarna. AT hello samma tid och du kan åtskilja hello produktion trafik från hello rest så att du kan fortsätta toomonitor produktion appen efter behov.

Eftersom du samla in data på klientbeteende, kommer du att [lägga till en telemetri initieraren tooyour JavaScript-kod](../application-insights/app-insights-api-filtering-sampling.md) i index.cshtml. Om du vill tootest serversidan prestanda, t.ex, du kan också göra på samma sätt i serverkoden (se [Application Insights API för anpassade händelser och mått](../application-insights/app-insights-api-custom-events-metrics.md).

1. Lägg först till hello kod mellan hello två `//` kommentarer nedan i hello JavaScript-block som du lagt till toohello `<heading>` tagga tidigare.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Initieraren koden gör hello `appInsights` objekt tooadd hello en anpassad egenskap som kallas `Environment` tooevery typ av telemetri som skickas.
2. Lägg till den anpassade egenskapen som en [fack inställningen](web-sites-staged-publishing.md#AboutConfiguration) för webbappen i Azure. toodo detta, kör hello följande kommandon i sessionen Git-gränssnittet.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    hello Web.config i projektet redan definierar hello `environment` appinställningen. Med den här inställningen när du testar hello appen lokalt, dina märks med `VS Debugger`. Men när du överför dina ändringar tooAzure Azure ska hitta och använda hello `environment` app inställningen i konfigurationen för hello webbapp i stället och dina märkta med `Production`.
3. Genomför push din kod ändringar tooyour förgrening på GitHub och väntar sedan på användare toouse hello nya appen (behovet toorefresh hello webbläsare). Det tar ungefär 15 minuter för hello nya egenskapen tooshow i Application Insights `MultiChannelToDo.Web` resurs.

        git add -A :/
        git commit -m "add environment property tooAI events for client app"
        git push origin master
4. Gå toohello **anpassad händelser** bladet igen och filtrera hello mått på `Environment=Production`. Nu bör du kunna toosee alla hello nya anpassade händelser i hello produktion fack med det här filtret.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. Klicka på hello **Favoriter** knappen toosave hello aktuella Metrics Explorer-inställningar toosomething som **anpassad händelser: produktion**. Du kan enkelt växla mellan den här vyn och vyn distribution fack senare.

   > [!TIP]
   > För ännu mer kraftfulla analytics, Överväg [integrera Application Insights-resursen med Power BI](../application-insights/app-insights-export-power-bi.md).
   >
   >

### <a name="add-slot-specific-tags-tooyour-server-app-metrics"></a>Lägg till plats-specifika taggar tooyour serverstatistik app
Igen, ska du ställa in hello serversidan app för fullständighetens skull. Till skillnad från hello-klientappen som instrumenterats i JavaScript, instrumenterats fack-specifika taggar för hello server app med .NET-kod.

1. Öppna * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Lägg till hello kodblock nedan, precis före hello avslutande klammerparentes för namnområdet.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }
2. Korrigera hello name resolution fel genom att lägga till hello `using` instruktionerna nedan toohello början av hello-fil:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. Lägg till hello kod nedan toohello början av hello `Application_Start()` metoden:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. Genomför push din kod ändringar tooyour förgrening på GitHub och väntar sedan på användare toouse hello nya appen (behovet toorefresh hello webbläsare). Det tar ungefär 15 minuter för hello nya egenskapen tooshow i Application Insights `MultiChannelToDo` resurs.

        git add -A :/
        git commit -m "add environment property tooAI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Uppdatering: Ställ in din gren beta
1. Öppna * &lt;repository_root >*\ARMTemplates\ProdAndStagetest.json och hitta hello `appsettings` resurser (söka efter `"name": "appsettings"`). Det finns 4 i dem, ett för varje plats.
2. För varje `appsettings` resurs, lägga till en `"environment": "[parameters('slotName')]"` app inställningen toohello slutet av hello `properties` matris. Glöm inte tooend hello föregående rad med kommatecken.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    Du har lagt till hello `environment` app inställningen tooall hello fack i hello mallen.
3. I Hej samma fil, hitta hello `slotconfignames` resurser (söka efter `"name": "slotconfignames"`). Det finns 2 av dem, ett för varje app.
4. För varje `slotconfignames` resurs, lägga till `"environment"` toohello slutet av hello `appSettingNames` matris. Glöm inte tooend hello föregående rad med kommatecken.

    Du har skapat hello `environment` app inställningen minne tooits respektive distributionsplatsen för både appar.  
5. I sessionen Git Shell hello kör följande kommandon med hello samma resurs grupp-suffix som du använde tidigare.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. När du uppmanas ange hello autentiseringsuppgifterna för samma SQL-databas som tidigare. Sedan, när du tillfrågas tooupdate hello resursgrupp, typen `Y`, sedan `ENTER`.

    När hello skriptet har körts, alla resurser i hello ursprungliga resursgruppen finns kvar, men en ny plats med namnet ”betaversionen” skapas i den med hello samma konfiguration som hello ”mellanlagring” fack som skapades i hello början.

   > [!NOTE]
   > Den här metoden för att skapa av olika distributionsmiljöer skiljer sig från hello-metoden i [flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md). Här kan skapa du distribution av med distributionsplatser där som det du skapar distributionsmiljöer med resursgrupper. Hantera distribution av med resursgrupper kan du tookeep hello produktion miljö off-limits toodevelopers, men det är inte enkelt toodo test i produktion, vilket du kan göra enkelt med platser.
   >
   >

Om du vill kan skapa du en alfanumeriska app genom att köra

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

För den här självstudiekursen kommer du bara fortsätta använda appen beta.

## <a name="update-push-your-updates-toohello-beta-app"></a>Uppdatering: Push appen uppdateringar toohello beta
Den bakre tooyour app som du vill tooimprove.

1. Kontrollera att du är nu i grenen beta

        git checkout beta
2. I * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, hitta hello `<li>` tagga och Lägg till hello `style="cursor:pointer"` attributet som visas nedan.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. genomför och skicka tooAzure.
4. Kontrollera att hello nu ändringen i hello beta plats genom att gå toohttp://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Om du inte ser hello ändra ännu, uppdatera din webbläsare tooget hello nya javascript-kod.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Nu när du har din ändring som körs i hello beta fack är klar tooperform flighting distribution.

## <a name="validate-route-traffic-toohello-beta-app"></a>Verifiera: Väg trafik toohello beta app
I det här avsnittet dirigerar trafik toohello beta app. For sake of bli tydligare i demonstrationssyfte ska tooroute en betydande del av hello användaren trafik tooit. I själva verket hello mängden trafik som du vill tooroute beror på dina behov. Till exempel om platsen är i hello skala Microsoft.com, kan måste mindre än en procent av den totala trafiken i ordning toogain användbara data.

1. I Git Shell sessionen och kör följande kommandon tooroute hello hälften av hello trafik toohello beta produktionsplatsen:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   Hej `ReroutePercentage=50` egenskapen anger 50% av hello produktion trafik kommer att dirigeras toohello beta app-URL (anges av hello `ActionHostName` egenskap).
2. Navigera toohttp://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50% av hello trafik bör nu vara omdirigerade toohello beta fack.
3. Filtrera hello mått i Application Insights-resurs-miljön = ”betaversionen”.

   > [!NOTE]
   > Om du sparar den filtrerade vyn som en annan favorit, kan du enkelt växla hello mått explorer vyer mellan produktion och beta vyer.
   >
   >

Anta att i Application Insights du se något liknande toohello följande:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Inte bara det visar att det finns många fler klick på hello `<li>` taggar, men det verkar toobe en ökning i klick på `<li>` taggar. Du kan sedan avslutar att personer som har identifierats hello nya `<li>` taggar är klickbara och nu Rensa alla sina tidigare slutförts aktiviteter i hello app.

Baserat på hello data flighting distributionen av du bestämmer dig för att ditt nya Användargränssnittet är klar för produktion.

## <a name="go-live-move-your-new-code-into-production"></a>Publicera: Flytta din nya kod till produktion
Du är nu redo toomove update-tooproduction. Vad är bra är att nu du vet att uppdateringen har redan verifierats *innan* pushas tooproduction. Nu kan du vill distribuera den. Eftersom du har gjort en uppdatering toohello AngularJS-klientappen verifieras endast hello klientkod. Om du toomake ändringar toohello backend-webb-API-app, kan du validera dina ändringar på samma sätt och enkelt.

1. Git Shell, tar du bort hello trafik routningsregel genom att köra följande kommando hello:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. Kör hello Git-kommandon:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. Vänta i några minuter för hello nya code toobe distribueras toohello mellanlagringsplatsen och sedan starta http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net tooverify som hello ny uppdatering varmkörts i hello mellanlagring fack. Kom ihåg att hello din förgrening mastergrenen är länkad toohello mellanlagring plats för din app.
4. Nu kan växla hello mellanlagringsplatsen till produktion

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Sammanfattning
Azure App Service gör det enkelt för toomedium-små företag tootest sina kund-riktade appar i produktion, något som traditionellt har gjorts i stora företag. Förhoppningsvis den här kursen har du fått hello kunskap du behöver toobring tillsammans App Service och Application Insights toomake möjlig förhandsversionstestning distribution och även andra test i produktion-scenarier i DevOps-världen.

## <a name="more-resources"></a>Fler resurser
* [Flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md)
* [Skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md)
* [Distribuera ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md)
* [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - hello JSON-verifieraren](http://jsonlint.com/)
* [Git förgrening – grundläggande förgrening och sammanfogning](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Azure PowerShell](/powershell/azure/overview)
* [Projektet Kudu-Wiki](https://github.com/projectkudu/kudu/wiki)
