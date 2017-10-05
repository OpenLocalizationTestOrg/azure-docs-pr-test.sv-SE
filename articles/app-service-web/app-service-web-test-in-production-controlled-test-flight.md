---
title: "Förhandsversionstestning distribution (betatestning) i Azure App Service"
description: "Lär dig flight nya funktioner i din app eller beta testa dina uppdateringar i den här självstudiekursen för slutpunkt till slutpunkt. Den samlar Apptjänst funktioner som kontinuerlig publicering, platser, routning av nätverkstrafik och Application Insights-integration."
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
ms.openlocfilehash: 83e3247310461ac148fff3c4ade3aa7216478537
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Förhandsversionstestning distribution (betatestning) i Azure App Service
Den här kursen visar hur du gör *förhandsversionstestning distributioner* genom att integrera olika funktioner i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) och [Azure Application Insights](/services/application-insights/).

*Förhandsversionstestning* är en Distributionsprocess som validerar en ny funktion eller ändra med ett begränsat antal verkliga kunder och är en större testning i form av produktionsscenario. Det är som beta testning och kallas ibland ”kontrollerade test svarta”. Många stora företag med webben använda den här metoden för att få tidig validering på deras app-uppdateringar i sina praxis [flexibel utveckling](https://en.wikipedia.org/wiki/Agile_software_development). Azure Apptjänst kan du integrera test i produktion med kontinuerlig publicering och Application Insights för att genomföra samma DevOps-scenario. Den här metoden fördelar:

* **Få verkliga feedback *innan* uppdateringar som släpps till produktion** -enda som är bättre än får feedback så fort du släpper har blivit feedback innan du släpper. Du kan testa uppdateringar med verkliga användaraktiviteten och beteenden tidigt du önskar i produktens livscykel.
* **Förbättra [kontinuerlig test-driven utveckling (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  – genom att integrera test i produktion med kontinuerlig integrering och instrumentation med Application Insights validering av användare händer tidigt och automatiskt i din produktens livscykel. Detta minskar tiden investeringar i manuell testkörning.
* **Optimera testa arbetsflödet** -genom att automatisera test i produktion med kontinuerlig övervakning instrumentation, du kan potentiellt göra målen för olika typer av testerna i en enda process som [integrering](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [användbarhet](https://en.wikipedia.org/wiki/Usability_testing), tillgänglighet, lokalisering, [prestanda](https://en.wikipedia.org/wiki/Software_performance_testing), [säkerhet](https://en.wikipedia.org/wiki/Security_testing), och [ godkännande](https://en.wikipedia.org/wiki/Acceptance_testing).

En flighting distribution inte bara om routning live trafik. I en sådan distribution du vill få insyn så snabbt som möjligt, om det vara ett oväntat fel, prestandaförsämring, user experience problem. Kom ihåg att du arbetar med verkliga kunder. Så här gör du det höger, måste du se till att du har lagt upp flighting distributionen för att samla in alla data som du behöver för att fatta ett välgrundat beslut för din nästa steg. Den här kursen visar hur du samlar in data med Application Insights, men du kan använda New Relic eller andra tekniker som passar ditt scenario.

## <a name="what-you-will-do"></a>Vad du ska göra
I kursen får du lära dig hur att göra följande scenarier tillsammans för att testa din Apptjänst-app i produktion:

* [Vidarebefordra trafik för produktion](app-service-web-test-in-production-get-start.md) i appen beta
* [Instrumentera appen](../application-insights/app-insights-web-track-usage.md) att hämta användbara mätvärden
* Distribuera appen beta och spåra aktiva app mått kontinuerligt
* Jämför mått mellan produktionsprogrammet och beta-app för att se hur koden konverteringar till resultat

## <a name="what-you-will-need"></a>Vad du behöver
* Ett Azure-konto
* En [GitHub](https://github.com/) konto
* Visual Studio 2015 – du kan hämta den [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
* Git Shell (installeras med [GitHub för Windows](https://windows.github.com/))-det här kan du köra Git- och PowerShell-kommandon i samma session
* Senaste [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits
* Grundläggande förståelse av följande:
  * [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) malldistribution (se [distribuerar ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> Du behöver ett Azure-konto för att kunna slutföra den här guiden:
>
> * Du kan [öppna ett Azure-konto gratis](https://azure.microsoft.com/pricing/free-trial/) – du får kredit du kan använda för att testa Azure-betaltjänster och även när de används du kan behålla kontot och använda kostnadsfria Azure-tjänster, till exempel Web Apps.
> * Du kan [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -din Visual Studio-prenumeration ger dig krediter varje månad som du kan använda för Azure-betaltjänster.
>
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
>
>

## <a name="set-up-your-production-web-app"></a>Konfigurera ditt webbprogram för produktion
> [!NOTE]
> Skriptet som används i den här självstudiekursen konfigurerar automatiskt kontinuerlig publicering från GitHub-lagringsplatsen. Detta kräver att dina GitHub-autentiseringsuppgifter lagras redan i Azure, annars skriptbaserade distributionen misslyckas vid försök att konfigurera inställningar för kontroll av datakälla för web apps.
>
> För att lagra dina GitHub-autentiseringsuppgifter i Azure, skapa en webbapp i den [Azure Portal](https://portal.azure.com/) och [konfigurera GitHub distribution](app-service-continuous-deployment.md). Du behöver bara göra detta en gång.
>
>

Du har ett program som körs live i Azure i ett typiskt scenario för DevOps, och du vill göra ändringar genom kontinuerlig publicering. I detta scenario distribuerar du en mall som du har utvecklats och testats till produktion.

1. Skapa din egen förgrening av den [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) databasen. Information om hur du skapar din förgrening finns [duplicera en Repo](https://help.github.com/articles/fork-a-repo/). När din förgrening har skapats kan se du den i webbläsaren.

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Öppna en session för Git-gränssnittet. Om du inte har Git Shell ännu, installera [GitHub för Windows](https://windows.github.com/) nu.
3. Skapa en lokal kloning av din förgrening genom att köra följande kommando:

     Git-klon https://github.com/<your_fork>/ToDoApp.git
4. När du har din lokala kloning, gå till  *&lt;repository_root >*\ARMTemplates och köra deploy.ps1 skript med ett unikt suffix enligt nedan:

     .\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix >
5. När du uppmanas ange önskat användarnamn och lösenord för åtkomst till databasen. Kom ihåg autentiseringsuppgifterna databasen eftersom du behöver ange dem igen när du uppdaterar resursgruppen.

   Du bör se etablering förloppet för olika Azure-resurser. När distributionen är klar kommer skriptet Starta program i webbläsaren och ger dig ett eget signal.
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. Tillbaka i sessionen Git Shell, kör du:

     . \swap – namn ToDoApp < your_suffix >

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. Gå tillbaka till Bläddra till den frontend-adress när skriptet har slutförts (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) att se det program som körs i produktion.
8. Logga in på den [Azure Portal](https://portal.azure.com/) och ta en titt på vad som har skapats.

   Du ska kunna se två web apps i samma resursgrupp, en med den `Api` suffix i namnet. Om du tittar på vyn för gruppen kan även se SQL-databasen och server App Service-plan och fristående fack för web apps. Bläddra igenom de olika resurserna och jämför dem med  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json att se hur de är konfigurerade i mallen.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Du har lagt upp produktionsprogrammet.  Nu kan vi anta att du får feedback att användbarhet är dålig för appen. Så att du vill undersöka. Ska du instrumentera din app om du vill ge feedback.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Undersök: Instrumentera klientappen för övervakning/mått
1. Öppna  *&lt;repository_root >*\src\MultiChannelToDo.sln i Visual Studio.
2. Återställa alla Nuget-paket genom att högerklicka på lösningen > **hantera NuGet-paket för lösningen** > **återställa**.
3. Högerklicka på **MultiChannelToDo.Web** > **Lägg till Application Insights Telemetry** > **konfigurera inställningarna för** > ändra resursgrupp ToDoApp*&lt;your_suffix >* > **Lägg till Application Insights i projekt**.
4. Öppna bladet för i Azure-portalen på **MultiChannelToDo.Web** Application Insights-resurs. I den **programhälsan** del, klickar du på **Lär dig att samla in sidhämtningsinformation för webbläsare** > Kopiera kod.
5. Lägg till den kopierade JS instrumentation kod  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml precis före avslutande `<heading>` tagg. Den ska innehålla unika instrumentation nyckeln för Application Insights-resurs.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. Skicka anpassade händelser till Application Insights för mus klickar genom att lägga till följande kod längst ned i brödtext:

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   JavaScript-kodfragment skickar en anpassad händelse till Application Insights varje gång en användare klickar någonstans i webbapp.
7. Git Shell, genomför och skicka ändringarna till din förgrening i GitHub. Vänta sedan klienter ska kunna uppdatera webbläsaren.

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. Byt distribuerad app-ändringar till produktion:

     . \swap – namn ToDoApp < your_suffix >
9. Bläddra till Application Insights-resursen som du har konfigurerat. Klicka på anpassade händelser.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   Om du inte ser mätvärden för anpassade händelser, Vänta några minuter och klicka på **uppdatera**.

Anta att du ser ett diagram som nedan:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

Och händelsen rutnätet nedan:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

Enligt programkoden ToDoApp den **knappen** händelse som motsvarar skickaknappen och **indata** händelse som motsvarar textrutan. Hittills vara saker meningsfullt. Men det verkar som om det är mycket musklickningar och mycket få klick på att göra-objekt (det **LI** händelser).

Baserat på det formulär du den hypotesen några användare kan blandas ihop vilken del av Användargränssnittet är klickbara och beror det på markören är formaterad för textmarkering när den är placerad på objekten i listan och deras ikoner.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Det här är ett exempel på contrived. Dock ska du göra en förbättring i appen och utför sedan en flighting distribution för att få användbarhet feedback från live-kunder.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Instrumentera din server-app för övervakning/mått
Detta är en tangent eftersom det scenario som visas i den här självstudiekursen bara behandlar klientappen. För fullständighetens skull kommer du dock ställa in serversidan appen.

1. Högerklicka på **MultiChannelToDo** > **Lägg till Application Insights Telemetry** > **konfigurera inställningarna för** > ändra resursgrupp ToDoApp*&lt;your_suffix >* > **Lägg till Application Insights i projekt**.
2. Git Shell, genomför och skicka ändringarna till din förgrening i GitHub. Vänta sedan klienter ska kunna uppdatera webbläsaren.

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. Byt distribuerad app-ändringar till produktion:

     . \swap – namn ToDoApp < your_suffix >

Klart!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Undersök: Lägga till fack-specifika taggar i din app mått för klienten
I det här avsnittet ska du konfigurera olika distributionsplatser för att skicka telemetri om plats-specifika till samma Application Insights-resurs. På så sätt kan du jämföra telemetridata mellan trafik från olika platser (distributionsmiljöer) för att enkelt se effekten av ändringarna app. På samma gång, kan du separera trafiken produktion från resten så att du kan fortsätta att övervaka din produktionsapp efter behov.

Eftersom du samla in data på klientbeteende, kommer du att [lägga till en telemetri initieraren i JavaScript-kod](../application-insights/app-insights-api-filtering-sampling.md) i index.cshtml. Om du vill testa prestanda för serversidan, t.ex, du kan också göra på samma sätt i serverkoden (se [Application Insights API för anpassade händelser och mått](../application-insights/app-insights-api-custom-events-metrics.md).

1. Lägg först till kod mellan två `//` kommentarer nedan i JavaScript blockera som du har lagts till i `<heading>` tagga tidigare.

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

    Initieraren koden gör den `appInsights` objekt att lägga till den kallas en anpassad egenskap `Environment` till all telemetri som skickas.
2. Lägg till den anpassade egenskapen som en [fack inställningen](web-sites-staged-publishing.md#AboutConfiguration) för webbappen i Azure. Gör detta genom att köra följande kommandon i Git Shell-sessionen.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Filen Web.config i projektet redan definierar den `environment` appinställningen. Med den här inställningen när du testar appen lokalt, dina märks med `VS Debugger`. Men när du pusha ändringarna till Azure, Azure ska hitta och använda den `environment` app inställningen i konfigurationen för webbapp i stället och dina märkta med `Production`.
3. Bekräfta och skicka din kodändringar till din förgrening på GitHub och vänta användarna att använda den nya appen (behöver uppdatera webbläsaren). Det tar ungefär 15 minuter för den nya egenskapen visas i Application Insights `MultiChannelToDo.Web` resurs.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master
4. Gå till den **anpassad händelser** bladet igen och filtrera mätvärdena på `Environment=Production`. Nu bör du kunna se alla nya anpassade händelser på produktionsplatsen med det här filtret.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. Klicka på den **Favoriter** för att spara de aktuella Metrics Explorer-inställningarna till något som liknar **anpassad händelser: produktion**. Du kan enkelt växla mellan den här vyn och vyn distribution fack senare.

   > [!TIP]
   > För ännu mer kraftfulla analytics, Överväg [integrera Application Insights-resursen med Power BI](../application-insights/app-insights-export-power-bi.md).
   >
   >

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Lägg till plats-specifika taggar i din app serverstatistik
Igen, ska du ställa in appen serversidan för fullständighetens skull. Till skillnad från klientappen som instrumenterats i JavaScript instrumenterats fack-specifika taggar för server-app med .NET-kod.

1. Öppna  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Lägg till kodblocket nedan, precis före avslutande klammerparentesen för namnområdet.

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
2. Korrigera namnet upplösning fel genom att lägga till den `using` instruktionerna nedan till början av filen:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. Lägg till koden nedan i början av den `Application_Start()` metoden:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. Bekräfta och skicka din kodändringar till din förgrening på GitHub och vänta användarna att använda den nya appen (behöver uppdatera webbläsaren). Det tar ungefär 15 minuter för den nya egenskapen visas i Application Insights `MultiChannelToDo` resurs.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Uppdatering: Ställ in din gren beta
1. Öppna  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json och söka efter den `appsettings` resurser (söka efter `"name": "appsettings"`). Det finns 4 i dem, ett för varje plats.
2. För varje `appsettings` resurs, lägga till en `"environment": "[parameters('slotName')]"` appinställningen till slutet av den `properties` matris. Glöm inte att avsluta föregående rad med kommatecken.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    Du har lagt till den `environment` appinställningen alla platser i mallen.
3. I samma fil finns på `slotconfignames` resurser (söka efter `"name": "slotconfignames"`). Det finns 2 av dem, ett för varje app.
4. För varje `slotconfignames` resurs, lägga till `"environment"` till slutet av den `appSettingNames` matris. Glöm inte att avsluta föregående rad med kommatecken.

    Du har skapat den `environment` app anger minne till sina respektive distributionsplatsen för både appar.  
5. Kör följande kommandon i sessionen Git-gränssnittet med samma resurs grupp-suffix som du använde tidigare.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. När du uppmanas, anger du samma SQL-databas autentiseringsuppgifterna som innan. När du tillfrågas om du vill uppdatera resursgruppen, Skriv `Y`, sedan `ENTER`.

    När skriptet har slutförts, alla resurser i den ursprungliga resursgruppen finns kvar, men en ny plats med namnet ”betaversionen” har skapats i den med samma konfiguration som ”mellanlagring” facket som skapades i början.

   > [!NOTE]
   > Den här metoden för att skapa av olika distributionsmiljöer skiljer sig från metoden i [flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md). Här kan skapa du distribution av med distributionsplatser där som det du skapar distributionsmiljöer med resursgrupper. Hantera distribution av med resursgrupper kan du behålla produktionsmiljön off-limits för utvecklare, men det är inte lätt att göra test i produktion, vilket du kan göra enkelt med platser.
   >
   >

Om du vill kan skapa du en alfanumeriska app genom att köra

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

För den här självstudiekursen kommer du bara fortsätta använda appen beta.

## <a name="update-push-your-updates-to-the-beta-app"></a>Uppdatering: Överför din uppdateringar till appen beta
Tillbaka till din app som du vill förbättra.

1. Kontrollera att du är nu i grenen beta

        git checkout beta
2. I  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, hitta den `<li>` tagga och Lägg till den `style="cursor:pointer"` attributet som visas nedan.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. genomförande och push i Azure.
4. Kontrollera att ändringen nu visas i beta-plats genom att gå till http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Om du inte se ändringen ännu, uppdatera webbläsaren om du vill hämta den nya javascript-koden.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Nu när du har din ändring som körs i beta-plats är du redo att utföra en flighting distribution.

## <a name="validate-route-traffic-to-the-beta-app"></a>Verifiera: Vidarebefordra trafik till appen beta
I det här avsnittet kommer du dirigerar trafik till appen beta. For sake of förstå demonstration ska du vidarebefordra en betydande del av användartrafiken till den. I verkligheten kan beror mängden trafik som du vill dirigera på dina behov. Exempelvis om webbplatsen är i skala Microsoft.com, kanske du måste mindre än en procent av den totala trafiken för att få användbara data.

1. Kör följande kommandon för att dirigera hälften av produktion trafiken till facket som beta i sessionen Git Shell:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   Den `ReroutePercentage=50` egenskapen anger 50% av produktions-trafik vidarebefordras till beta-appens URL (anges av den `ActionHostName` egenskap).
2. Navigera till http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50% av trafiken ska nu omdirigeras till facket som beta.
3. Filtrera mätvärdena efter miljö i Application Insights-resurs = ”betaversionen”.

   > [!NOTE]
   > Om du sparar den filtrerade vyn som en annan favorit, kan du enkelt växla mått explorer vyer mellan produktion och beta vyer.
   >
   >

Anta att du ser något som liknar följande i Application Insights:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Inte bara det visar att det finns många fler klick på den `<li>` taggar, men det verkar vara en ökning i klick på `<li>` taggar. Du kan sedan avslutar att personer har identifierat nya `<li>` taggar är klickbara och nu Rensa alla sina tidigare slutförts aktiviteter i appen.

Baserat på de flighting distributionen av du bestämmer dig för att ditt nya Användargränssnittet är klar för produktion.

## <a name="go-live-move-your-new-code-into-production"></a>Publicera: Flytta din nya kod till produktion
Nu är du redo att flytta din uppdatering till produktionen. Vad är bra är att nu du vet att uppdateringen har redan verifierats *innan* pushas till produktionen. Nu kan du vill distribuera den. Eftersom du har gjort en uppdatering i klientappen AngularJS verifieras endast koden för klientsidan. Om du vill göra ändringar för backend-webb-API-app, kan du validera dina ändringar på samma sätt och enkelt.

1. Ta bort routning regel för nätverkstrafik genom att köra följande kommando i Git-gränssnittet:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. Kör Git-kommandon:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. Vänta några minuter för den nya koden som ska distribueras till mellanlagringsplatsen och sedan starta http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net för att verifiera att den nya uppdateringen varmkörts i mellanlagringsplatsen. Kom ihåg att den din förgrening mastergrenen är kopplad till mellanlagringsplatsen för din app.
4. Nu byta mellanlagringsplatsen till produktion

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Sammanfattning
Azure App Service gör det enkelt för små till medelstora företag att testa sina kund-riktade appar i produktion, något som traditionellt har utförts i stora företag. Den här självstudiekursen har förhoppningsvis gett dig kunskap du behöver samordnar App Service och Application Insights för att göra möjlig förhandsversionstestning distribution och även andra test i produktion-scenarier i DevOps-världen.

## <a name="more-resources"></a>Fler resurser
* [Flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md)
* [Skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md)
* [Distribuera ett komplexa program förutsägbart i Azure](app-service-deploy-complex-application-predictably.md)
* [Redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - JSON-verifieraren](http://jsonlint.com/)
* [Git förgrening – grundläggande förgrening och sammanfogning](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Azure PowerShell](/powershell/azure/overview)
* [Projektet Kudu-Wiki](https://github.com/projectkudu/kudu/wiki)
