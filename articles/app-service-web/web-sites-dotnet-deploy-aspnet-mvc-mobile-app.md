---
title: Distribuera ett ASP.NET MVC 5 mobila webbappen i Azure App Service
description: "En självstudiekurs som lär du dig hur du distribuerar en webbapp till Azure App Service med mobila funktioner i ASP.NET MVC 5 webbprogram."
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
ms.openlocfilehash: c98e9b485c52a82e5be5c0f6b0b67912d1e890b9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a>Distribuera ett ASP.NET MVC 5 mobila webbappen i Azure App Service
Den här kursen lär du dig grunderna för hur du skapar ett webbprogram för ASP.NET MVC 5 som mobilvänlig och distribuera den till Azure App Service. För den här kursen behöver du [Visual Studio Express 2013 för webben] [ Visual Studio Express 2013] eller professionell utgåva av Visual Studio om du redan har som. Du kan använda [Visual Studio 2015] men skärmdumpar är olika och måste du använda mallar för ASP.NET 4.x.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a>Vad du ska skapa
Den här självstudiekursen ska du lägga till mobila funktioner enkelt konferens lista-program som har angetts i den [starter projektet][StarterProject]. Följande skärmbild visar ASP.NET-sessioner i det färdiga programmet som visas i webbläsaren emulatorn i Internet Explorer 11 F12 utvecklingsverktyg.

![][FixedSessionsByTag]

Du kan använda Internet Explorer 11 F12 utvecklingsverktyg och [Fiddler verktyget] [ Fiddler] för att felsöka programmet. 

## <a name="skills-youll-learn"></a>Kunskaper lär du dig
Här är lär du dig:

* Hur du använder Visual Studio 2013 för att publicera webbprogrammet direkt till en webbapp i Azure App Service.
* Hur ASP.NET MVC 5 mallar använder CSS Bootstrap-ramverket för att förbättra visas på mobila enheter
* Så här skapar du mobile-specifika vyer om du vill rikta specifika mobila webbläsare, till exempel iPhone- och Android
* Så här skapar du responsiv vyer (vyer som svarar på olika webbläsare mellan enheter)

## <a name="set-up-the-development-environment"></a>Konfigurera utvecklingsmiljön
Ställ in din utvecklingsmiljö genom att installera Azure SDK för .NET 2.5.1 eller senare. 

1. Klicka på länken nedan om du vill installera Azure SDK för .NET. Om du inte har Visual Studio 2013 har installerats ännu, kommer att installeras av länken. Den här kursen kräver Visual Studio 2013. [Azure SDK för Visual Studio 2013][AzureSDKVs2013]
2. Klicka på i fönstret Installationsprogram för webbplattform **installera** och fortsätta med installationen.

Du måste också en emulator mobila webbläsare. Något av följande fungerar:

* Webbläsaren emulatorn i [utvecklingsverktyg för Internet Explorer 11 F12] [ EmulatorIE11] (används i alla mobila webbläsare skärmbilder). Den har användaren agent sträng förinställningar för Windows Phone 8, Windows Phone 7 och Apple iPad.
* Webbläsaren emulatorn i [Google Chrome DevTools][EmulatorChrome]. Den innehåller förinställningar för flera Android-enheter samt Apple iPhone, iPad Apple och Amazon Kindle brand. Det kan också emulerar touch-händelser.
* [Opera Mobilemulator][EmulatorOpera]

Visual Studio-projekt med C\# källkoden är tillgängliga som åtföljer det här avsnittet:

* [Ladda ned Starter-projekt][StarterProject]
* [Slutfört projekt hämtning][CompletedProject]

## <a name="bkmk_DeployStarterProject"></a>Distribuera starter-projektet till en Azure-webbapp
1. Hämta programmet konferens lista [starter projektet][StarterProject].
2. Högerklicka på den hämta ZIP-filen i Utforskaren och välj sedan *egenskaper*.
3. I den **egenskaper** dialogrutan väljer du den **avblockera** knappen. (Avblockera förhindrar att en säkerhetsvarning som uppstår vid försök att använda en *.zip* -fil som du har laddat ned från webben.)
4. Högerklicka på ZIP-filen och välj **extrahera alla** att packa upp filen. 
5. Öppna i Visual Studio den *C#\Mvc5Mobile.sln* fil.
6. Högerklicka på projektet i Solution Explorer och klicka på **publicera**.
   
   ![][DeployClickPublish]
7. Klicka på Publicera webbplats **Microsoft Azure App Service**.
   
   ![][DeployClickWebSites]
8. Om du inte har redan loggat in på Azure, klickar du på **Lägg till ett konto**.
   
   ![][DeploySignIn]
9. Följ anvisningarna för att logga in på ditt Azure-konto.
10. Dialogrutan för Apptjänst ska nu visa du som loggat in. Klicka på **Ny**.
    
    ![][DeployNewWebsite]  
11. I den **Webbprogramnamnet** anger ett unikt app namnprefix. Din fullständiga webbprogrammets namn kommer att  *&lt;prefix >*. azurewebsites.net. Dessutom väljer eller anger ett namn på ny resursgrupp i **resursgruppen**. Klicka på **ny** att skapa en ny programtjänstplan.
    
    ![][DeploySiteSettings]
12. Konfigurera den nya programtjänstplanen och klicka på **OK**. 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. Klicka på tillbaka i dialogrutan Skapa Apptjänst **skapa**.
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. När Azure skapas resurser i Publicera webbplats dialogrutan fylls med inställningarna för den nya appen. Klicka på **Publicera**.
    
    ![][DeployPublishSite]
    
    När Visual Studio har slutförts publicering starter-projektet till Azure-webbappen öppnas skrivbord webbläsaren om du vill visa live-webb-app.
15. Starta din mobila webbläsare emulator, Kopiera URL-Adressen för programmets konferens (*<prefix>*. azurewebsites.net) i emulatorn och sedan klicka på knappen längst upp till höger och välj **Bläddra efter tagg**. Om du använder Internet Explorer 11 som standardwebbläsare, du behöver bara ange `F12`, sedan `Ctrl+8`, och sedan ändra profilen som webbläsaren ska **Windows Phone**. Bilden nedan visar den *AllTags* vyn i stående läge (från att välja **Bläddra efter tagg**).
    
    ![][AllTags]

> [!TIP]
> Medan du kan felsöka MVC 5 programmet från Visual Studio, kan du publicera webbappen till Azure igen för att verifiera live webbprogrammet direkt från din mobila webbläsare eller en webbläsare emulator.
> 
> 

Visningen kan mycket läsas på en mobil enhet. Du kan också redan se några visual effekter som används av Bootstrap CSS-ramverket.
Klicka på den **ASP.NET** länk.

![][SessionsByTagASP.NET]

ASP.NET-tagg vyn är zoomning monterade på skärmen som Bootstrap sparas automatiskt. Du kan dock förbättra den här vyn så att de bättre passar din mobila webbläsare. Till exempel den **datum** kolumnen är svåra att läsa. Senare under kursen ska du ändra den *AllTags* vy så att den mobilvänlig.

## <a name="bkmk_bootstrap"></a>Bootstrap CSS Framework
Nya är MVC 5 inbyggt stöd för Bootstrap på mallen. Du har redan sett hur de olika vyerna i tillämpningsprogrammet omedelbart förbättras. Navigeringsfältet längst upp är till exempel automatiskt döljas när webbläsaren är mindre. Försök ändra storlek på webbläsarfönstret skrivbord webbläsare och se hur navigeringsfältet ändras dess utseende. Detta är dynamisk webbdesign som är inbyggd i starttjänsten.

Om du vill se hur webbappen ser ut utan Bootstrap, öppna *App\_starta\\BundleConfig.cs* och kommentera ut rader som innehåller *bootstrap.js* och *bootstrap.css*. Följande kod visar de två sista rapporterna för den `RegisterBundles` metoden när ändringen:

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

Tryck på `Ctrl+F5` att köra programmet.

Observera att döljas navigeringsfältet nu är bara en vanlig osorterad lista. Klicka på **Bläddra efter tagg** igen och klicka sedan på **ASP.NET**.
I vyn mobilemulator ser nu att den inte längre zoomning monterade på skärmen och du måste rulla åt sidan för att visa till höger i tabellen.

![][SessionsByTagASP.NETNoBootstrap]

Ångra ändringarna och uppdatera din mobila webbläsare för att verifiera att mobilvänlig visningen har återställts.

Starttjänsten är inte specifik för ASP.NET MVC 5 och du kan dra nytta av funktionerna i alla webbprogram. Men det är nu inbyggda i projektmall ASP.NET MVC 5 så att ditt webbprogram för MVC 5 kan dra nytta av Bootstrap som standard.

Mer information om Bootstrap går du till den [Bootstrap] [ BootstrapSite] plats.

I nästa avsnitt visas hur du skapar mobila webbläsare vissa vyer.

## <a name="bkmk_overrideviews"></a>Åsidosätt vyer, layout och partiella vyer
Du kan åsidosätta en vy (inklusive partiella vyer och layouter) för mobila webbläsare i allmänhet för en enskild mobila webbläsare eller för specifika webbläsare. För att ge en mobile-specifika vy, kan du kopiera en fil i vyn och lägga till *. Mobila* i filnamnet. Till exempel för att skapa en mobil *Index* vy, kan du kopiera *vyer\\Start\\Index.cshtml* till *vyer\\Start\\Index.Mobile.cshtml*.

I det här avsnittet skapar du en mobile-specifika layout-fil.

Starta genom att kopiera *vyer\\delade\\\_Layout.cshtml* till *vyer\\delade\\\_Layout.Mobile.cshtml*. Öppna  *\_Layout.Mobile.cshtml* och ändra rubriken från **MVC5 programmet** till **MVC5 program (Mobile)**.

I varje `Html.ActionLink` anropa för navigeringsfältet, ta bort ”Bläddra efter” i varje länk *ActionLink*. Följande kod visar den färdiga `<ul class="nav navbar-nav">` taggen för layoutfilen mobila.

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

Kopiera den *vyer\\Start\\AllTags.cshtml* filen till *vyer\\Start\\AllTags.Mobile.cshtml*. Öppna den nya filen och ändra den `<h2>` element från ”taggar” till ”taggar (M)”:

    <h2>Tags (M)</h2>

Gå till sidan taggar med en webbläsare för stationära och använder mobila webbläsare emulatorn. Mobila webbläsare emulatorn visar två ändringar (titel från  *\_Layout.Mobile.cshtml* och titel från *AllTags.Mobile.cshtml*).

![][AllTagsMobile_LayoutMobile]

Däremot stationära visningen inte har ändrats (med rubriker från  *\_Layout.cshtml* och *AllTags.cshtml*).

![][AllTagsMobile_LayoutMobileDesktop]

## <a name="bkmk_browserviews"></a>Skapa specifika webbläsare vyer
Du kan skapa vyer för en enskild webbläsare förutom mobile-specifika och desktop-specifika vyer. Du kan till exempel skapa vyer som är specifikt för iPhone- eller Android-webbläsare. I det här avsnittet skapar du en layout för iPhone webbläsaren och en iPhone-version av den *AllTags* vyn.

Öppna den *Global.asax* och Lägg till följande kod längst ned på den `Application_Start` metoden.

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

Den här koden definierar ett nytt visningsläge med namnet ”iPhone” som matchas mot varje inkommande begäran. Om den inkommande begäranden matchar de villkor som du definierade (det vill säga om användaragenten innehåller strängen ”iPhone”), kollar ASP.NET MVC för vyer vars namn innehåller suffixet ”iPhone”.

> [!NOTE]
> När du lägger till mobila visningslägen specifika webbläsare, till exempel för iPhone- och Android, måste du ange det första argumentet `0` (Infoga överst i listan) se till att specifika webbläsare läge företräde framför mobila mallen (*. Mobile.cshtml). Om mallen mobila är överst i listan i stället, markeras den via din avsedda visningsläget (första matchar wins och mallen för mobila matchar alla mobila webbläsare). 
> 
> 

Högerklicka i koden, `DefaultDisplayMode`, Välj **lösa**, och välj sedan `using System.Web.WebPages;`. Detta lägger till en referens till den `System.Web.WebPages` namnområdet, som är var den `DisplayModeProvider` och `DefaultDisplayMode` anges.

![][ResolveDefaultDisplayMode]

Du kan också du kan bara manuellt lägga till följande rad i den `using` avsnitt i filen.

    using System.Web.WebPages;

Spara ändringarna. Kopiera den *vyer\\delade\\\_Layout.Mobile.cshtml* filen till *vyer\\delade\\\_Layout.iPhone.cshtml*. Öppna den nya filen och ändra rubriken från `MVC5 Application (Mobile)` till `MVC5 Application (iPhone)`.

Kopiera den *vyer\\Start\\AllTags.Mobile.cshtml* filen till *vyer\\Start\\AllTags.iPhone.cshtml*. I den nya filen ändrar den `<h2>` element från ”taggar (M)” till ”-taggar (iPhone)”.

Kör appen. Kör en emulator mobila webbläsare och kontrollera att dess användaragent är inställd på ”iPhone”, bläddra till den *AllTags* vyn. Om du använder emulatorn i Internet Explorer 11 F12 utvecklingsverktyg, konfigurera emulering av följande:

* Webbläsaren profil = **Windows Phone**
* Användaragentsträngen = **anpassad**
* Anpassad sträng = **1001.525-Apple-iPhone5C1**

I följande skärmbild visas den *AllTags* visa återges i emulatorn i Internet Explorer 11 F12 utvecklingsverktyg med anpassade användaragentsträngen (detta är en iPhone 5 C användaragentsträngen).

![][AllTagsIPhone_LayoutIPhone]

I den mobila webbläsaren, Välj den **högtalare** länk. Eftersom det inte är en mobil vy (*AllSpeakers.Mobile.cshtml*), standard högtalarna visa (*AllSpeakers.cshtml*) återges med hjälp av vyn mobila layout (*\_Layout.Mobile.cshtml*). Som visas under rubriken **MVC5 program (Mobile)** har definierats i  *\_Layout.Mobile.cshtml*.

![][AllSpeakers_LayoutMobile]

Du kan inaktivera en standardvy (icke-mobila) från återgivning inuti en mobila layout globalt genom att ange `RequireConsistentDisplayMode` till `true` i den *vyer\\\_ViewStart.cshtml* filen, så här:

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

När `RequireConsistentDisplayMode` är inställd på `true`, mobila layouten (*\_Layout.Mobile.cshtml*) används endast för mobila vyer (d.v.s. när filen vyn är i formatet  ***vynamn**. Mobile.cshtml*). Du kan vilja ange `RequireConsistentDisplayMode` till `true` om mobila layouten inte fungerar bra med dina icke-mobila vyer. Skärmbilden nedan visar hur *högtalare* sidan återger när `RequireConsistentDisplayMode` är inställd på `true` (utan strängen ”(mobilt)” i navigerings-fältet högst upp).

![][AllSpeakers_LayoutMobileOverridden]

Du kan inaktivera konsekvent visningsläge i en specifik vy genom att ange `RequireConsistentDisplayMode` till `false` i filen vyn. Följande markering i den *vyer\\Start\\AllSpeakers.cshtml* filen anger `RequireConsistentDisplayMode` till `false`:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

Vi har sett hur du skapar mobila layouter och vyer och hur du skapar vyer för specifika enheter, till exempel iPhone och layouter i det här avsnittet.
Den största fördelen Bootstrap CSS-ramverket är dock dynamisk layout, vilket innebär att en enda formatmall kan tillämpas på skrivbordet, phone och tablet webbläsare för att skapa ett konsekvent utseende. I nästa avsnitt visas hur man utnyttjar Bootstrap för att skapa mobilvänlig vyer.

## <a name="bkmk_Improvespeakerslist"></a>Förbättra listan högtalare
Som du såg bara kan den *högtalare* läget läsbar men länkarna små och är svåra att trycka på en mobil enhet. I det här avsnittet ska du göra det *AllSpeakers* visa mobilvänlig, som visar stora, enkelt att tryck länkar och innehåller en kryssruta för att snabbt hitta högtalare.

Du kan använda Bootstrap [länkad lista grupp] [ linked list group] formatmalls att förbättra den *högtalare* vyn. I *vyer\\Start\\AllSpeakers.cshtml*, ersätta innehållet i filen Razor med koden nedan.

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

Den `class="list-group"` attribut i den `<div>` taggen gäller Bootstrap lista-stil och `class="input-group-item"` attributet gäller Bootstrap listan objektet formatmalls för varje länk.

Uppdatera din mobila webbläsare. Den uppdaterade vyn ser ut så här:

![][AllSpeakersFixed]

Bootstrap [länkad lista grupp] [ linked list group] formatmalls gör hela rutan för varje länk klickbara, vilket är en mycket bättre användarupplevelse. Växla till vyn skrivbord och se konsekvent utseende.

![][AllSpeakersFixedDesktop]

Även om mobila webbläsare vyn har förbättrats, är det svårt att navigera lång lista med högtalare. Starttjänsten ger inte en sökning filter funktioner out-of-the-box, men du kan lägga till den med några få rader med kod. Du kommer först lägger till en kryssruta i vyn och sedan koppla samman med JavaScript-koden för filterfunktionen. I *vyer\\Start\\AllSpeakers.cshtml*, lägga till en \<formuläret\> tagga precis efter den \<h2\> tagg, enligt nedan:

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

Observera att den `<form>` och `<input>` taggar som båda har Bootstrap stilar används. Den `<span>` element lägger till en Bootstrap [glyphicon] [ glyphicon] till sökrutan.

I den *skript* mapp, lägga till en JavaScript-fil som heter *filter.js*. Öppna filen och klistra in följande kod i den:

    $(function () {

        // reset the search form when the page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up the events to the <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with the typed text and hide others
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

Du måste också innehålla filter.js i din registrerade paket. Öppna *App\_starta\\BundleConfig.cs* och ändra första paket. Ändra först `bundles.Add` instruktionen (för den **jquery** paket) att inkludera *skript\\filter.js*, enligt följande:

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

Den **jquery** paket redan återges av standard  *\_Layout* vyn. Senare kan använda du samma JavaScript-kod om du vill använda filter-funktioner till andra vyer.

Uppdatera mobila webbläsare och gå till den *AllSpeakers* vyn. Skriv ”sc” i sökrutan. Listan över högtalare bör nu filtreras enligt en söksträng.

![][AllSpeakersFixedSearchBySC]

## <a name="bkmk_improvetags"></a>Förbättra listan taggar
Som det *högtalare* vyn den *taggar* läget läsbar men länkarna är små och svåra att trycka på en mobil enhet. Du kan åtgärda den *taggar* visa på samma sätt som du har åtgärdat den *högtalare* visas om du använder kodändringar beskrivs tidigare, men med följande `Html.ActionLink` metodsyntax i *vyer\\Start\\AllTags.cshtml*:

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

Uppdatera skrivbord webbläsaren ser ut som följer:

![][AllTagsFixedDesktop]

Och uppdateras mobila webbläsare ser ut som följer: 

![][AllTagsFixed]

> [!NOTE]
> Om du upptäcker att den ursprungliga listformateringen finns kvar i din mobila webbläsare och undrar vad hände med din nice Bootstrap formatmalls är detta ett resultat av den tidigare åtgärden Skapa mobila vissa vyer. Nu när du använder Bootstrap CSS-ramverket för att skapa en responsiv design, gå head och ta bort dessa mobile-specifika vyer och mobile-specifika layout vyer. När du har gjort det, visas uppdateras mobila webbläsare Bootstrap stil.
> 
> 

## <a name="bkmk_improvedates"></a>Förbättra listan datum
Du kan förbättra den *datum* Visa som du har förbättrats i *högtalare* och *taggar* vyer om du använder kodändringar beskrivs tidigare, men med följande `Html.ActionLink` metodsyntax i *vyer\\Start\\AllDates.cshtml*:

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

Du får en Webbläsarvy uppdateras mobila så här:

![][AllDatesFixed]

Du kan förbättra ytterligare den *datum* vyn genom att ordna datum / tid-värden efter datum. Detta kan göras med Bootstrap [paneler] [ panels] formatering. Ersätt innehållet i den *vyer\\Start\\AllDates.cshtml* med följande kod:

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

Den här koden skapar en separat `<div class="panel panel-primary">` tagg för varje distinkta datum i listan och använder den [länkad lista grupp] [ linked list group] för respektive länkar som tidigare. Här är hur mobila webbläsare ser ut när den här koden körs:

![][AllDatesFixed2]

Växla till skrivbord webbläsaren. Tänk återigen enhetligt utseende.

![][AllDatesFixed2Desktop]

## <a name="bkmk_improvesessionstable"></a>Förbättra SessionsTable vyn
I det här avsnittet ska du göra det *SessionsTable* visa mer mobilvänlig. Den här ändringen är mer omfattande tidigare ändringar.

I den mobila webbläsaren trycker du på den **taggen** knappen och klicka sedan på Ange `asp` i sökrutan.

![][AllTagsFixedSearchByASP]

Tryck på den **ASP.NET** länk.

![][SessionsTableTagASP.NET]

Som du ser formateras visningen som en tabell som för närvarande används för att visas i webbläsaren skrivbord. Det är dock lite svårt att läsa mobila webbläsare. Lös problemet genom att öppna *vyer\\Start\\SessionsTable.cshtml* och Ersätt innehållet i filen med följande kod:

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

Koden gör 3 saker:

* använder Bootstrap [anpassad länkade lista grupp] [ custom linked list group] att formatera sessionsinformation lodrätt, så att den här informationen kan läsas på en mobila webbläsare (med klasser, till exempel listan-grupp-objekt-text)
* gäller de [nätet] [ grid system] till layout, så att sessionen objekt flöda vågrätt i webbläsaren skrivbord och lodrätt i din mobila webbläsare (med klassen kolumn-md-4)
* använder den [dynamisk verktygen] [ responsive utilities] Dölj taggarna sessionen när den visas i din mobila webbläsare (med klassen dolda xs)

Du kan också trycka på en avdelning länken till respektive sessionen. Bilden nedan visar kodändringarna.

![][FixedSessionsByTag]

Bootstrap nätet som du använde automatiskt ordnar sessioner lodrätt i din mobila webbläsare. Observera också att taggar inte visas. Växla till skrivbord webbläsaren.

![][SessionsTableFixedTagASP.NETDesktop]

Observera att taggar visas nu i webbläsaren skrivbord. Du kan också se att du använt Bootstrap nätet ordnar session objekten i två kolumner. Om du förstorar webbläsaren visas som hur ändringar i tre kolumner.

## <a name="bkmk_improvesessionbycode"></a>Förbättra SessionByCode vyn
Slutligen kan du korrigera den *SessionByCode* vy så att den mobilvänlig.

I den mobila webbläsaren trycker du på den **taggen** knappen och klicka sedan på Ange `asp` i sökrutan.

![][AllTagsFixedSearchByASP]

Tryck på den **ASP.NET** länk. Sessioner för ASP.NET-taggen visas.

![][FixedSessionsByTag]

Välj den **skapa ett program med ASP.NET och AngularJS sidan** länk.

![][SessionByCode3-644]

Standardvyn för fjärrskrivbord är bra, men du kan förbättra utseendet enkelt vissa Bootstrap GUI-komponenter.

Öppna *vyer\\Start\\SessionByCode.cshtml* och Ersätt innehållet med följande kod:

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

Nya koden använder Bootstrap paneler formatering för att förbättra mobila vyn. 

Uppdatera din mobila webbläsare. Följande bild visar kodändringar som du skapat:

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a>Sammanfattning och granska
Den här självstudiekursen har visas hur du använder ASP.NET MVC 5 för att utveckla mobilvänlig webbprogram. Exempel på dessa är:

* Distribuera ett ASP.NET MVC 5-program till en Apptjänst-webbapp
* Använd Bootstrap för att skapa responsiv web-layouten i MVC 5-program
* Åsidosätt layout, vyer och partiella vyer, både globalt och för en enskild vy
* Kontrollens layout och partiella åsidosätter tvingande med hjälp av den `RequireConsistentDisplayMode` egenskapen
* Skapa vyer som är avsedda för vissa webbläsare, till exempel iPhone-webbläsare
* Tillämpa Bootstrap formatmalls i Razor-kod

## <a name="see-also"></a>Se även
* [9 grundläggande principerna för dynamisk webbdesign](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* [Starttjänsten][BootstrapSite]
* [Officiell Bootstrap blogg][Official Bootstrap Blog]
* [Twitter Bootstrap kursen från kursen Republiken][Twitter Bootstrap Tutorial from Tutorial Republic]
* [Bootstrap Playground][The Bootstrap Playground]
* [Metodtips för W3C rekommendation mobilen program][W3C Recommendation Mobile Web Application Best Practices]
* [W3C kandidat rekommendation för media frågor][W3C Candidate Recommendation for media queries]

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- Internal Links -->
[Deploy the starter project to an Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override the Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve the Speakers List]: #bkmk_Improvespeakerslist
[Improve the Tags List]: #bkmk_improvetags
[Improve the Dates List]: #bkmk_improvedates
[Improve the SessionsTable View]: #bkmk_improvesessionstable
[Improve the SessionByCode View]: #bkmk_improvesessionbycode

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
[The Bootstrap Playground]: http://www.bootply.com/
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

