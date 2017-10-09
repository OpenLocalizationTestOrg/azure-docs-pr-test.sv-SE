---
title: aaaDeploy ett ASP.NET MVC 5 mobila webbappen i Azure App Service
description: "En självstudiekurs som lär du dig hur toodeploy en web app tooAzure App Service med mobila funktioner i ASP.NET MVC 5 webbprogram."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 0752c802-8609-4956-a755-686116913645
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 01119c07246c0252fd357562774a2e90b3ef77d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a>Distribuera ett ASP.NET MVC 5 mobila webbappen i Azure App Service
Den här självstudiekursen kommer lära du hello grunderna för hur toobuild en ASP.NET MVC 5 web app som är mobilvänlig och distribuerar den tooAzure Apptjänst. För den här kursen behöver du [Visual Studio Express 2013 för webben] [ Visual Studio Express 2013] eller hello professionell utgåva av Visual Studio om du redan har som. Du kan använda [Visual Studio 2015] men hello skärmdumpar är olika och du måste använda hello ASP.NET 4.x mallar.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a>Vad du ska skapa
Den här kursen ska du lägga till mobila funktioner toohello enkelt konferens-lista över program som har angetts i hello [starter projektet][StarterProject]. hello följande skärmbild visar hello ASP.NET sessioner i hello slutförts program som det visas i hello webbläsare emulatorn i Internet Explorer 11 F12 utvecklingsverktyg.

![][FixedSessionsByTag]

Du kan använda hello Internet Explorer 11 F12 utvecklingsverktyg och hello [Fiddler verktyget] [ Fiddler] toohelp felsöka programmet. 

## <a name="skills-youll-learn"></a>Kunskaper lär du dig
Här är lär du dig:

* Hur toouse Visual Studio 2013 toopublish webbprogrammet direkt tooa webbapp i Azure App Service.
* Hur använder hello ASP.NET MVC 5 mallar hello CSS Bootstrap framework för att förbättra visas på mobila enheter
* Hur toocreate mobile-specifika vyer tootarget specifika mobila webbläsare, till exempel hello iPhone- och Android
* Hur toocreate responsiv vyer (vyer som svarar toodifferent webbläsare mellan enheter)

## <a name="set-up-hello-development-environment"></a>Ställa in hello utvecklingsmiljö
Ställ in din utvecklingsmiljö genom att installera hello Azure SDK för .NET 2.5.1 eller senare. 

1. tooinstall hello Azure SDK för .NET, klicka hello länken nedan. Om du inte har Visual Studio 2013 har installerats ännu, kommer att installeras av hello-länken. Den här kursen kräver Visual Studio 2013. [Azure SDK för Visual Studio 2013][AzureSDKVs2013]
2. Klicka på i hello installationsprogram för webbplattform **installera** och fortsätta med installationen hello.

Du måste också en emulator mobila webbläsare. Hello följande fungerar:

* Webbläsaren emulatorn i [utvecklingsverktyg för Internet Explorer 11 F12] [ EmulatorIE11] (används i alla mobila webbläsare skärmbilder). Den har användaren agent sträng förinställningar för Windows Phone 8, Windows Phone 7 och Apple iPad.
* Webbläsaren emulatorn i [Google Chrome DevTools][EmulatorChrome]. Den innehåller förinställningar för flera Android-enheter samt Apple iPhone, iPad Apple och Amazon Kindle brand. Det kan också emulerar touch-händelser.
* [Opera Mobilemulator][EmulatorOpera]

Visual Studio-projekt med C\# källkod och är tillgängliga tooaccompany det här avsnittet:

* [Ladda ned Starter-projekt][StarterProject]
* [Slutfört projekt hämtning][CompletedProject]

## <a name="bkmk_DeployStarterProject"></a>Distribuera hello starter projektet tooan Azure-webbapp
1. Hämta hello konferens lista program [starter projektet][StarterProject].
2. Högerklicka på hello hämtade ZIP-filen i Utforskaren och välj sedan *egenskaper*.
3. I hello **egenskaper** dialogrutan Välj hello **avblockera** knappen. (Avblockera förhindrar att en säkerhetsvarning som uppstår när du försöker toouse en *.zip* -fil som du har laddat ned från hello webbserver.)
4. Högerklicka på hello ZIP-filen och välj **extrahera alla** att packa hello-filen. 
5. Öppna i Visual Studio hello *C#\Mvc5Mobile.sln* fil.
6. Högerklicka på hello-projektet i Solution Explorer och klicka på **publicera**.
   
   ![][DeployClickPublish]
7. Klicka på Publicera webbplats **Microsoft Azure App Service**.
   
   ![][DeployClickWebSites]
8. Om du inte har redan loggat in på Azure, klickar du på **Lägg till ett konto**.
   
   ![][DeploySignIn]
9. Följ hello prompter toolog i ditt Azure-konto.
10. hello dialogrutan för Apptjänst ska nu visa du som loggat in. Klicka på **Ny**.
    
    ![][DeployNewWebsite]  
11. I hello **Webbprogramnamnet** anger ett unikt app namnprefix. Din fullständiga webbprogrammets namn kommer att  *&lt;prefix >*. azurewebsites.net. Dessutom väljer eller anger ett namn på ny resursgrupp i **resursgruppen**. Klicka på **ny** toocreate en ny programtjänstplan.
    
    ![][DeploySiteSettings]
12. Konfigurera hello nya App Service-plan och klicka på **OK**. 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. Klicka på tillbaka i dialogrutan Skapa App Service hello **skapa**.
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. När hello Azure-resurser skapas, fylls hello Publicera webbplats dialogrutan med hello inställningar för din nya app. Klicka på **Publicera**.
    
    ![][DeployPublishSite]
    
    När Visual Studio har slutförts publishing hello projektet toohello Azure startwebbapp öppnas hello skrivbord webbläsaren toodisplay hello live-webbprogram.
15. Starta din mobila webbläsare emulator, kopiera hello URL för hello konferensprogram (*<prefix>*. azurewebsites.net) till hello-emulatorn och sedan klicka på knappen längst upp till höger och välj **Bläddra efter tagg**. Om du använder Internet Explorer 11 som hello standardwebbläsare, behöver du bara tootype `F12`, sedan `Ctrl+8`, och sedan ändra hello webbläsare profil för**Windows Phone**. Bilden nedan visar hello *AllTags* vyn i stående läge (från att välja **Bläddra efter tagg**).
    
    ![][AllTags]

> [!TIP]
> Medan du kan felsöka MVC 5 programmet från Visual Studio, kan du publicera din web app tooAzure igen tooverify hello live webbprogrammet direkt från din mobila webbläsare eller en webbläsare emulator.
> 
> 

Visa hello är mycket läsbar på en mobil enhet. Du kan också redan se några av hello visual effekter av hello Bootstrap CSS framework.
Klicka på hello **ASP.NET** länk.

![][SessionsByTagASP.NET]

hello ASP.NET taggen vyn är monterade zoomning toohello skärmen som Bootstrap sparas automatiskt. Du kan dock förbättra den här vyn toobetter färg hello mobila webbläsare. Till exempel hello **datum** kolumnen är svåra att läsa. Senare i självstudiekursen hello ska du ändra hello *AllTags* visa toomake den mobilvänlig.

## <a name="bkmk_bootstrap"></a>Bootstrap CSS Framework
Nya är hello MVC 5 inbyggt stöd för Bootstrap på mallen. Du har redan sett hur det omedelbart förbättrar hello olika vyer i ditt program. Hello navigeringsfältet längst upp i hello är till exempel automatiskt döljas när hello webbläsarbredd är mindre. Hello skrivbord webbläsare, försök storleksändring hello webbläsarfönster och se hur hello navigeringsfältet ändras dess utseende. Detta är hello responsiv webbdesign som är inbyggd i starttjänsten.

toosee hur hello webbprogrammet ser utan Bootstrap, öppna *App\_starta\\BundleConfig.cs* och kommentera ut hello rader som innehåller *bootstrap.js* och *bootstrap.css*. hello följande kod visar hello två sista rapporter för hello `RegisterBundles` metoden efter hello ändring:

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

Tryck på `Ctrl+F5` toorun hello program.

Se hello döljas navigeringsfältet är nu bara en vanlig osorterad lista. Klicka på **Bläddra efter tagg** igen och klicka sedan på **ASP.NET**.
I hello mobilemulator vyn visas nu när den inte längre zoomning monterade toohello skärmen och du måste rulla åt sidan i ordning toosee hello höger sida av hello tabell.

![][SessionsByTagASP.NETNoBootstrap]

Ångra ändringarna och uppdatera hello mobila webbläsare tooverify hello mobilvänlig visa har återställts.

Starttjänsten är inte specifik tooASP.NET MVC 5 och du kan dra nytta av funktionerna i alla webbprogram. Men det är nu inbyggda i projektmall ASP.NET MVC 5 så att ditt webbprogram för MVC 5 kan dra nytta av Bootstrap som standard.

Mer information om Bootstrap gå toothe [Bootstrap] [ BootstrapSite] plats.

I hello nästa avsnitt får du se hur tooprovide mobila webbläsare vissa vyer.

## <a name="bkmk_overrideviews"></a>Åsidosätt hello vyer och layouter partiella vyer
Du kan åsidosätta en vy (inklusive partiella vyer och layouter) för mobila webbläsare i allmänhet för en enskild mobila webbläsare eller för specifika webbläsare. tooprovide mobile-specifika visa kan du kopiera en fil i vyn och lägga till *. Mobila* toohello filnamn. Till exempel toocreate en mobil *Index* vy, kan du kopiera *vyer\\Start\\Index.cshtml* till *vyer\\Start\\ Index.Mobile.cshtml*.

I det här avsnittet skapar du en mobile-specifika layout-fil.

toostart, kopiera *vyer\\delade\\\_Layout.cshtml* till *vyer\\delade\\\_Layout.Mobile.cshtml* . Öppna  *\_Layout.Mobile.cshtml* och ändra hello rubrik från **MVC5 programmet** för**MVC5 program (Mobile)**.

I varje `Html.ActionLink` anropa för hello navigeringsfältet, ta bort ”Bläddra efter” i varje länk *ActionLink*. hello följande kod visar hello slutförts `<ul class="nav navbar-nav">` taggen för hello mobila layoutfilen.

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

Kopiera hello *vyer\\Start\\AllTags.cshtml* filen till *vyer\\Start\\AllTags.Mobile.cshtml*. Öppna ny hello-filen och ändra den `<h2>` element från ”taggar” för ”taggar (M)”:

    <h2>Tags (M)</h2>

Bläddra toohello taggar sida med en webbläsare för stationära och mobila webbläsare emulatorn använder. hello mobila webbläsare emulator visar hello två ändringar (hello titel från  *\_Layout.Mobile.cshtml* och hello titel från *AllTags.Mobile.cshtml*).

![][AllTagsMobile_LayoutMobile]

Däremot hello visning av skrivbordet inte har ändrats (med rubriker från  *\_Layout.cshtml* och *AllTags.cshtml*).

![][AllTagsMobile_LayoutMobileDesktop]

## <a name="bkmk_browserviews"></a>Skapa specifika webbläsare vyer
Dessutom toomobile-specifika och desktop-specifika vyer, du kan skapa vyer för en enskild webbläsare. Du kan till exempel skapa vyer som är specifikt för hello iPhone- eller hello Android-webbläsare. I det här avsnittet skapar du en layout för hello iPhone webbläsare och en iPhone-version av hello *AllTags* vyn.

Öppna hello *Global.asax* och Lägg till hello följande kod toohello längst ned i `Application_Start` metod.

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

Den här koden definierar ett nytt visningsläge med namnet ”iPhone” som matchas mot varje inkommande begäran. Om hello inkommande begäran matchar de villkor som du definierade (det vill säga om hello användaragent innehåller hello strängen ”iPhone”), kollar ASP.NET MVC för vyer vars namn innehåller suffixet ”iPhone”.

> [!NOTE]
> När lägger till specifika mobila webbläsare visningslägen, såsom för iPhone- och Android, vara säker på att tooset hello första argumentet för`0` (insert hello överst i listan hello) toomake till hello specifika webbläsare läget företräde framför hello mobila mall (*. Mobile.cshtml). Om hello mobila mall är hello överst i listan hello i stället, väljs den via din avsedda visningsläget (hello första matchar wins och hello mobila mall matchar alla mobila webbläsare). 
> 
> 

I hello kod, högerklickar du på `DefaultDisplayMode`, Välj **lösa**, och välj sedan `using System.Web.WebPages;`. Detta lägger till en referens toothe `System.Web.WebPages` namnområdet, som är var den `DisplayModeProvider` och `DefaultDisplayMode` anges.

![][ResolveDefaultDisplayMode]

Du kan också du kan bara manuellt lägga till följande rad toothe hello `using` i hello-filen.

    using System.Web.WebPages;

Spara hello ändringar. Kopiera den *vyer\\delade\\\_Layout.Mobile.cshtml* filen till *vyer\\delade\\\_Layout.iPhone.cshtml*. Öppna ny hello-fil och ändra sedan hello titel från `MVC5 Application (Mobile)` till `MVC5 Application (iPhone)`.

Kopiera hello *vyer\\Start\\AllTags.Mobile.cshtml* filen till *vyer\\Start\\AllTags.iPhone.cshtml*. Ny hello-fil, ändra hello `<h2>` element från ”taggar (M)” för ”taggar (iPhone)”.

Kör hello program. Kör en mobila webbläsare emulator, se till att dess användaragent anges för ”iPhone”, och bläddra toohello *AllTags* vyn. Om du använder hello emulatorn i Internet Explorer 11 F12 utvecklingsverktyg, konfigurera emulering toohello följande:

* Webbläsaren profil = **Windows Phone**
* Användaragentsträngen = **anpassad**
* Anpassad sträng = **1001.525-Apple-iPhone5C1**

hello följande skärmbild visar hello *AllTags* visa återges i emulatorn i Internet Explorer 11 F12 utvecklingsverktyg med hello anpassade användaragentsträngen (detta är en iPhone 5 C användaragentsträngen).

![][AllTagsIPhone_LayoutIPhone]

I hello mobila webbläsare väljer hello **högtalare** länk. Eftersom det inte är en mobil vy (*AllSpeakers.Mobile.cshtml*), visa hello standard högtalare (*AllSpeakers.cshtml*) återges med hello mobila vyn ( *\_ Layout.Mobile.cshtml*). Enligt nedan, hello rubrik **MVC5 program (Mobile)** har definierats i  *\_Layout.Mobile.cshtml*.

![][AllSpeakers_LayoutMobile]

Du kan inaktivera en standardvy (icke-mobila) från återgivning inuti en mobila layout globalt genom att ange `RequireConsistentDisplayMode` till `true` i hello *vyer\\\_ViewStart.cshtml* filen, så här:

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

När `RequireConsistentDisplayMode` har angetts för`true`, hello mobila layout (*\_Layout.Mobile.cshtml*) används endast för mobila vyer (d.v.s. när filen vyn är i formatet hello  ***vynamn** . Mobile.cshtml*). Du kanske vill tooset `RequireConsistentDisplayMode` för`true` om mobila layouten inte fungerar bra med dina icke-mobila vyer. hello skärmbilden nedan visar hur hello *högtalare* sidan återger när `RequireConsistentDisplayMode` har angetts för`true` (utan hello strängen ”(mobilt)” i hello navigering liggande hello överst).

![][AllSpeakers_LayoutMobileOverridden]

Du kan inaktivera konsekvent visningsläge i en specifik vy genom att ange `RequireConsistentDisplayMode` för`false` i hello visa filen. Följande markering i hello *vyer\\Start\\AllSpeakers.cshtml* filen anger `RequireConsistentDisplayMode` för`false`:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

I det här avsnittet har vi sett hur toocreate mobila layouter och vyer och hur toocreate layouter och vyer för specifika enheter såsom hello iPhone.
Hello största fördelen hello Bootstrap CSS framework är dock dynamisk layout, vilket innebär att en enda formatmall kan tillämpas på skrivbordet, phone och tablet webbläsare toocreate ett konsekvent utseende. I hello nästa avsnitt får du se hur tooleverage Bootstrap toocreate mobilvänlig vyer.

## <a name="bkmk_Improvespeakerslist"></a>Förbättra hello högtalare lista
Som du precis såg hello *högtalare* läget läsbar men hello länkar är mindre och svåra tootap på en mobil enhet. I det här avsnittet ska du göra hello *AllSpeakers* view mobilvänlig, som visar stora, enkelt att tryck länkar och innehåller en Sök-rutan tooquickly hitta högtalare.

Du kan använda hello Bootstrap [länkad lista grupp] [ linked list group] formatmalls att förbättra hello *högtalare* vyn. I *vyer\\Start\\AllSpeakers.cshtml*, Ersätt hello innehållet i hello Razor-filen med hello koden nedan.

     @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker }, new { @class = "list-group-item" })
        }
    </div>

Hej `class="list-group"` attribut i hello `<div>` taggen gäller Bootstrap listan formatmalls och hello `class="input-group-item"` attributet gäller Bootstrap listan artikel formatmalls tooeach länk.

Uppdatera hello mobila webbläsare. hello uppdaterat vyn ser ut så här:

![][AllSpeakersFixed]

hello Bootstrap [länkad lista grupp] [ linked list group] formatmalls gör hello hela rutan för varje länk klickbara, vilket är en mycket bättre användarupplevelse. Växla toothe skrivbordsvy och se hello konsekvent utseende.

![][AllSpeakersFixedDesktop]

Även om hello mobila webbläsare vyn har förbättrats, är det svårt att navigera hello lång lista med högtalare. Starttjänsten ger inte en sökning filter funktioner out-of-the-box, men du kan lägga till den med några få rader med kod. Du kommer först lägga till en sökning rutan toohello vy och sedan koppla samman med hello JavaScript-kod för hello filterfunktionen. I *vyer\\Start\\AllSpeakers.cshtml*, lägga till en \<formuläret\> taggen precis efter hello \<h2\> tagg, enligt nedan:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <form class="input-group">
        <span class="input-group-addon"><span class="glyphicon glyphicon-search"></span></span>
        <input type="text" class="form-control" placeholder="Search speaker">
    </form>
    <br />
    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class = "list-group-item" })
        }
    </div>

Observera att hello `<form>` och `<input>` taggar som båda har hello Bootstrap formatmallar toothem. Hej `<span>` element lägger till en Bootstrap [glyphicon] [ glyphicon] toothe sökrutan.

I hello *skript* mapp, lägga till en JavaScript-fil som heter *filter.js*. Öppna hello-filen och klistra in följande kod till den hello:

    $(function () {

        // reset hello search form when hello page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up hello events toohello <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with hello typed text and hide others
            for (var i = 0; i < items.length; i++) {
                var val = items[i].text.toLowerCase();
                val = val.substring(0, searchtxt.length);
                if (val == searchtxt) {
                    $(items[i]).show();
                }
                else {
                    $(items[i]).hide();
                }
            }
        });
    });

Du måste också tooinclude filter.js i din registrerade paket. Öppna *App\_starta\\BundleConfig.cs* och ändra hello första paket. Ändra först `bundles.Add` instruktionen (för hello **jquery** paket) tooinclude *skript\\filter.js*, enligt följande:

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

Hej **jquery** paket återges redan hello standard  *\_Layout* vyn. Senare kan du använda hello samma JavaScript-kod tooapply filtrera funktioner tooother listvyer.

Uppdatera hello mobila webbläsare och gå toohello *AllSpeakers* vyn. Skriv ”sc” i sökrutan. hello högtalare lista bör nu filtreras enligt tooyour söksträngen.

![][AllSpeakersFixedSearchBySC]

## <a name="bkmk_improvetags"></a>Förbättra hello Tagglista
Som hello *högtalare* visas hello *taggar* läget läsbar men hello länkar är liten och svår tootap på en mobil enhet. Du kan åtgärda hello *taggar* visa hello samma sätt du åtgärda hello *högtalare* visas om du använder hello kodändringar beskrivs tidigare, men med följande hello `Html.ActionLink` metodsyntax i  *Vyer\\Start\\AllTags.cshtml*:

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

hello uppdateras skrivbord webbläsaren ser ut så här:

![][AllTagsFixedDesktop]

Och hello uppdateras mobila webbläsare ser ut så här: 

![][AllTagsFixed]

> [!NOTE]
> Om du märker att hello listformatering finns kvar i Hej mobila webbläsare och undrar vad hände tooyour nice Bootstrap formatmalls, detta är en metod med en tidigare åtgärd toocreate mobila vissa vyer. Nu när du använder hello Bootstrap CSS framework toocreate en responsiv design, gå head och ta bort dessa mobile-specifika vyer och hello mobile-specifika layout vyer. När du har gjort det, kommer hello uppdateras mobila webbläsare visar hello Bootstrap formatmalls.
> 
> 

## <a name="bkmk_improvedates"></a>Förbättra hello datum lista
Du kan förbättra hello *datum* Visa som du förbättrad hello *högtalare* och *taggar* vyer om du använder hello kodändringar beskrivs tidigare, men med följande hello `Html.ActionLink` metodsyntax i *vyer\\Start\\AllDates.cshtml*:

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

Du får en Webbläsarvy uppdateras mobila så här:

![][AllDatesFixed]

Du kan förbättra hello ytterligare *datum* vyn genom att ordna hello datum / tid-värden efter datum. Detta kan göras med hello Bootstrap [paneler] [ panels] formatering. Ersätt hello innehållet i hello *vyer\\Start\\AllDates.cshtml* med följande kod:

    @model IEnumerable<DateTime>

    @{
        ViewBag.Title = "All Dates";
    }

    <h2>Dates</h2>

    @foreach (var dategroup in Model.GroupBy(x=>x.Date))
    {
        <div class="panel panel-primary">
            <div class="panel-heading">
                @dategroup.Key.ToString("ddd, MMM dd")
            </div>
            <div class="panel-body list-group">
                @foreach (var date in dategroup)
                {
                    @Html.ActionLink(date.ToString("h:mm tt"), 
                                     "SessionsByDate", 
                                     new { date }, 
                                     new { @class = "list-group-item" })
                }
            </div>
        </div>
    }

Den här koden skapar en separat `<div class="panel panel-primary">` taggen för varje distinkta datum i hello-lista och använder hello [länkad lista grupp] [ linked list group] för respektive länkar som tidigare. Här är vilken hello mobila webbläsare ser ut som när den här koden körs:

![][AllDatesFixed2]

Växla toohello skrivbord webbläsare. Tänk återigen hello konsekvent.

![][AllDatesFixed2Desktop]

## <a name="bkmk_improvesessionstable"></a>Förbättra hello SessionsTable vy
I det här avsnittet ska du göra hello *SessionsTable* visa mer mobilvänlig. Den här ändringen är mer omfattande hello tidigare ändringar.

Tryck på hello i hello mobila webbläsare **taggen** knappen och klicka sedan på Ange `asp` i sökrutan.

![][AllTagsFixedSearchByASP]

Tryck på hello **ASP.NET** länk.

![][SessionsTableTagASP.NET]

Som du ser formateras hello visas som en tabell som är för närvarande utformad toobe visas i hello skrivbord webbläsare. Det är dock lite svårt tooread mobila webbläsare. toofix detta, öppna *vyer\\Start\\SessionsTable.cshtml* och Ersätt hello innehållet i filen med hello följande kod:

    @model IEnumerable<Mvc5Mobile.Models.Session>

    <h2>@ViewBag.Title</h2>

    <div class="container">
        <div class="row">
            @foreach (var session in Model)
            {
                <div class="col-md-4">
                    <div class="list-group">
                        @Html.ActionLink(session.Title, 
                                         "SessionByCode", 
                                         new { session.Code }, 
                                         new { @class="list-group-item active" })
                        <div class="list-group-item">
                            <div class="list-group-item-text">
                                @Html.Partial("_SpeakersLinks", session)
                            </div>
                            <div class="list-group-item-info">
                                @session.DateText
                            </div>
                            <div class="list-group-item-info small hidden-xs">
                                @Html.Partial("_TagsLinks", session)
                            </div>
                        </div>
                    </div>
                </div>
            }
        </div>
    </div>

hello koden gör 3 saker:

* använder hello Bootstrap [anpassad länkade lista grupp] [ custom linked list group] tooformat hello sessionsinformation lodrätt, så att den här informationen kan läsas på en mobila webbläsare (med klasser som lista-grupp-objekt-text)
* gäller hello [nätet] [ grid system] toothe layout så hello sessionen objekt flödet vågrätt i hello skrivbord webbläsare och lodrätt i hello mobila webbläsare (med hello kolumn md 4 klassen)
* använder hello [dynamisk verktygen] [ responsive utilities] att dölja hello session taggar när visas i hello mobila webbläsare (med hello dolda xs klassen)

Du kan också trycka på en avdelning länken toogo toohello respektive session. hello bilden nedan visar hello kodändringar.

![][FixedSessionsByTag]

hello Bootstrap nätet som du använde automatiskt ordnar sessioner lodrätt i hello mobila webbläsare. Observera också att hello taggar inte visas. Växla toohello skrivbord webbläsare.

![][SessionsTableFixedTagASP.NETDesktop]

Observera att hello taggar visas nu i hello skrivbord webbläsare. Du kan också se att hello Bootstrap nätet du använt ordnar hello session objekt i två kolumner. Om du förstorar webbläsaren visas som hello arrangemang ändringar toothree kolumner.

## <a name="bkmk_improvesessionbycode"></a>Förbättra hello SessionByCode vy
Slutligen hello korrigera *SessionByCode* visa toomake den mobilvänlig.

Tryck på hello i hello mobila webbläsare **taggen** knappen och klicka sedan på Ange `asp` i sökrutan.

![][AllTagsFixedSearchByASP]

Tryck på hello **ASP.NET** länk. Sessioner hello ASP.NET taggar för visas.

![][FixedSessionsByTag]

Välj hello **skapa ett program med ASP.NET och AngularJS sidan** länk.

![][SessionByCode3-644]

hello standardvyn för fjärrskrivbord är bra, men du kan förbättra hello utseende enkelt vissa Bootstrap GUI-komponenter.

Öppna *vyer\\Start\\SessionByCode.cshtml* och Ersätt hello innehållet med hello följande kod:

    @model Mvc5Mobile.Models.Session

    @{
        ViewBag.Title = "Session details";
    }
    <h3>@Model.Title (@Model.Code)</h3>
    <p>
        <strong>@Model.DateText</strong> in <strong>@Model.Room</strong>
    </p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Speakers
        </div>
        @foreach (var speaker in Model.Speakers)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class="panel-body" })
        }
    </div>

    <p>@Model.Abstract</p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Tags
        </div>
        @foreach (var tag in Model.Tags)
        {
            @Html.ActionLink(tag, 
                             "SessionsByTag", 
                             new { tag }, 
                             new { @class = "panel-body" })
        }
    </div>

hello nya markup använder Bootstrap paneler formatering tooimprove hello mobila vyn. 

Uppdatera hello mobila webbläsare. hello visar följande bild hello kodändringar som du skapat:

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a>Sammanfattning och granska
Den här kursen visar du hur toouse ASP.NET MVC 5 toodevelop mobilvänlig webbprogram. Exempel på dessa är:

* Distribuera ett ASP.NET MVC 5 programmet tooan App Service web-appen
* Använd Bootstrap toocreate responsiv Webblayout i MVC 5-program
* Åsidosätt layout, vyer och partiella vyer, både globalt och för en enskild vy
* Kontrollens layout och partiella åsidosätter tvingande med hjälp av den `RequireConsistentDisplayMode` egenskapen
* Skapa vyer som är avsedda för vissa webbläsare, till exempel hello iPhone webbläsare
* Tillämpa Bootstrap formatmalls i Razor-kod

## <a name="see-also"></a>Se även
* [9 grundläggande principerna för dynamisk webbdesign](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* [Starttjänsten][BootstrapSite]
* [Officiell Bootstrap blogg][Official Bootstrap Blog]
* [Twitter Bootstrap kursen från kursen Republiken][Twitter Bootstrap Tutorial from Tutorial Republic]
* [hello Bootstrap Playground][hello Bootstrap Playground]
* [Metodtips för W3C rekommendation mobilen program][W3C Recommendation Mobile Web Application Best Practices]
* [W3C kandidat rekommendation för media frågor][W3C Candidate Recommendation for media queries]

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- Internal Links -->
[Deploy hello starter project tooan Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override hello Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve hello Speakers List]: #bkmk_Improvespeakerslist
[Improve hello Tags List]: #bkmk_improvetags
[Improve hello Dates List]: #bkmk_improvedates
[Improve hello SessionsTable View]: #bkmk_improvesessionstable
[Improve hello SessionByCode View]: #bkmk_improvesessionbycode

<!-- External Links -->
[Visual Studio Express 2013]: http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-web
[Visual Studio 2015]: https://www.visualstudio.com/downloads/download-visual-studio-vs
[AzureSDKVs2013]: http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409
[Fiddler]: http://www.fiddler2.com/fiddler2/
[EmulatorIE11]: http://msdn.microsoft.com/library/ie/dn255001.aspx
[EmulatorChrome]: https://developers.google.com/chrome-developer-tools/docs/mobile-emulation
[EmulatorOpera]: http://www.opera.com/developer/tools/mobile/
[StarterProject]: http://go.microsoft.com/fwlink/?LinkID=398780&clcid=0x409
[CompletedProject]: http://go.microsoft.com/fwlink/?LinkID=398781&clcid=0x409
[BootstrapSite]: http://getbootstrap.com/
[WebPIAzureSdk23NetVS13]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/WebPIAzureSdk23NetVS13.png
[linked list group]: http://getbootstrap.com/components/#list-group-linked
[glyphicon]: http://getbootstrap.com/components/#glyphicons
[panels]: http://getbootstrap.com/components/#panels
[custom linked list group]: http://getbootstrap.com/components/#list-group-custom-content
[grid system]: http://getbootstrap.com/css/#grid
[responsive utilities]: http://getbootstrap.com/css/#responsive-utilities
[Official Bootstrap Blog]: http://blog.getbootstrap.com/
[Twitter Bootstrap Tutorial from Tutorial Republic]: http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/
[hello Bootstrap Playground]: http://www.bootply.com/
[W3C Recommendation Mobile Web Application Best Practices]: http://www.w3.org/TR/mwabp/
[W3C Candidate Recommendation for media queries]: http://www.w3.org/TR/css3-mediaqueries/

<!-- Images -->
[DeployClickPublish]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png
[DeployClickWebSites]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png
[DeploySignIn]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png
[DeployUsername]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png
[DeployPassword]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png
[DeployNewWebsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png
[DeploySiteSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png
[DeployPublishSite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png
[MobileHomePage]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/mobile-home-page.png
[FixedSessionsByTag]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-Fixed.png
[AllTags]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags.png
[SessionsByTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET.png
[SessionsByTagASP.NETNoBootstrap]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-NoBootstrap.png
[AllTagsMobile_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile.png
[AllTagsMobile_LayoutMobileDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile-Desktop.png
[ResolveDefaultDisplayMode]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/Resolve-DefaultDisplayMode.png
[AllTagsIPhone_LayoutIPhone]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsIPhone-_LayoutIPhone.png
[AllSpeakers_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile.png
[AllSpeakers_LayoutMobileOverridden]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile-Overridden.png
[AllSpeakersFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed.png
[AllSpeakersFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-Desktop.png
[AllSpeakersFixedSearchBySC]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-SearchBySC.png
[AllTagsFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-Desktop.png 
[AllTagsFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed.png
[AllDatesFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed.png
[AllDatesFixed2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2.png
[AllDatesFixed2Desktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2-Desktop.png
[AllTagsFixedSearchByASP]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-SearchByASP.png
[SessionsTableTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Tag-ASP.NET.png
[SessionsTableFixedTagASP.NETDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Fixed-Tag-ASP.NET-Desktop.png
[SessionByCode3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-3-644.png
[SessionByCodeFixed3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-Fixed-3-644.png

