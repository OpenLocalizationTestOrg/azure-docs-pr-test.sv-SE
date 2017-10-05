---
title: Skapa ett REST-API i Azure med ASP.NET och SQL DB | Microsoft Docs
description: "En självstudiekurs som lär du dig hur du distribuerar en app som använder ASP.NET Web API till en Azure-webbapp med hjälp av Visual Studio."
services: app-service\web
documentationcenter: .net
author: Rick-Anderson
writer: Rick-Anderson
manager: erikre
editor: 
ms.assetid: f4916fc0-ea08-41f7-846b-73e41bc88149
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/29/2016
ms.author: riande
ms.openlocfilehash: 64c18f2cfabbb7af6ffd89b4c2a9095fca1cf799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a>Skapa ett REST-tjänst som använder ASP.NET Web API och SQL-databas i Azure App Service
Den här kursen visar hur du distribuerar ett ASP.NET-webbprogram till en [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) med hjälp av guiden Publicera webbplats i Visual Studio 2013 eller Visual Studio 2013 Community Edition. 

Du kan öppna ett Azure-konto gratis och om du inte redan har Visual Studio 2013, SDK installerar Visual Studio 2013 automatiskt för Web Express. Så kan du börja utveckla för Azure och helt kostnadsfritt.

Den här kursen förutsätter att du har några tidigare erfarenheter av att använda Azure. På den här kursen har du en enkel webbapp upp och körs i molnet.

Du får lära dig:

* Hur du aktiverar datorn för Azure-utveckling genom att installera Azure SDK.
* Hur du skapar ett projekt i Visual Studio ASP.NET MVC 5 och publicera det till en Azure-app.
* Hur du använder ASP.NET Web API för att aktivera Restful-API-anrop.
* Hur du använder en SQL-databas för att lagra data i Azure.
* Hur du publicerar program uppdateras till Azure.

Du måste skapa ett enkelt kontaktlista webbprogram som bygger på ASP.NET MVC 5 och använder ADO.NET Entity Framework för åtkomst till databasen. Följande bild visar det färdiga programmet:

![Skärmbild av webbplats][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-the-project"></a>Skapa projektet
1. Starta Visual Studio 2013.
2. Från den **filen** Klicka **nytt projekt**.
3. I den **nytt projekt** dialogrutan Expandera **Visual C#** och välj **Web** och välj sedan **ASP.NET-webbprogram**. Ge programmet namnet **ContactManager** och på **OK**.
   
    ![Dialogrutan Nytt projekt](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. I den **nytt ASP.NET-projekt** dialogrutan markerar du den **MVC** mall, kontrollera **Web API** och klicka sedan på **ändra autentisering**.
5. I dialogrutan **Ändra autentisering** klickar du på **Ingen autentisering** och sedan på **OK**.
   
    ![Ingen autentisering](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    Det exempelprogram som du skapar inte funktioner som kräver att användare kan logga in. Information om hur du implementerar autentisering och auktorisering funktioner finns i [nästa steg](#nextsteps) avsnittet i slutet av den här kursen. 
6. I den **nytt ASP.NET-projekt** dialogrutan Kontrollera den **värd i molnet** är markerad och klicka på **OK**.

Om du redan inte har registrerat till Azure, uppmanas du att logga in.

1. Guiden för konfiguration av föreslås ett unikt namn baserat på *ContactManager* (se bilden nedan). Välj en region nära dig. Du kan använda [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") att hitta den lägsta svarstid datacentralen. 
2. Om du inte har skapat en databasserver innan du väljer **Skapa ny server**, ange ett användarnamn och lösenord.
   
    ![Konfigurera Azure-webbplats](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

Om du har en databasserver, använder du den och skapa en ny databas. Databasservrar är en värdefull resurs och du normalt vill skapa flera databaser på samma server för testning och utveckling istället för att skapa en databasserver per databas. Kontrollera att webbplatsen och databasen är i samma region.

![Konfigurera Azure-webbplats](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-the-page-header-and-footer"></a>Ange sidhuvud och sidfot
1. I **Solution Explorer**, expandera den *Views\Shared* mapp och öppna den *_Layout.cshtml* fil.
   
    ![_Layout.cshtml i Solution Explorer][newapp004]
2. Ersätt innehållet i den *Views\Shared_Layout.cshtml* med följande kod:

        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="utf-8" />
            <title>@ViewBag.Title - Contact Manager</title>
            <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
            <meta name="viewport" content="width=device-width" />
            @Styles.Render("~/Content/css")
            @Scripts.Render("~/bundles/modernizr")
        </head>
        <body>
            <header>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p class="site-title">@Html.ActionLink("Contact Manager", "Index", "Home")</p>
                    </div>
                </div>
            </header>
            <div id="body">
                @RenderSection("featured", required: false)
                <section class="content-wrapper main-content clear-fix">
                    @RenderBody()
                </section>
            </div>
            <footer>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p>&copy; @DateTime.Now.Year - Contact Manager</p>
                    </div>
                </div>
            </footer>
            @Scripts.Render("~/bundles/jquery")
            @RenderSection("scripts", required: false)
        </body>
        </html>

Koden ovan ändrar namnet på appen från ”mitt ASP.NET-program” till ”Kontakta Manager” och det tar bort länkar till **Start**, **om** och **Kontakta**.

### <a name="run-the-application-locally"></a>Kör programmet lokalt
1. Tryck på CTRL+F5 för att köra programmet.
   Med programmets startsida visas i standardwebbläsaren.
    ![Att göra-lista startsidan](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)

Det här är allt du behöver göra nu att skapa program som du ska distribuera till Azure. Senare ska du lägga till databasfunktioner.

## <a name="deploy-the-application-to-azure"></a>Distribuera programmet till Azure
1. I Visual Studio högerklickar du på projektet i **Solution Explorer** och välj **publicera** på snabbmenyn.
   
    ![Publicera i snabbmenyn för projektet][PublishVSSolution]
   
    Den **Publicera webbplats** öppnas guiden.
2. Klicka på **Publicera**.

![Fliken Inställningar](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

Visual Studio börjar filerna kopieras till Azure-servern. Den **utdata** fönster visar vilka distributionsåtgärder som utförts och rapporterar slutförande av distributionen.

1. Standardwebbläsare öppnas automatiskt URL för den distribuerade webbplatsen.
   
   Programmet som du skapade körs nu i molnet.
   
   ![Att göra-lista startsidan körs i Azure][rxz2]

## <a name="add-a-database-to-the-application"></a>Lägga till en databas för programmet
Därefter ska du uppdatera MVC-program för att lägga till möjligheten att visa och uppdatera kontakter och lagra data i en databas. Programmet använder Entity Framework för att skapa databasen och för att läsa och uppdatera data i databasen.

### <a name="add-data-model-classes-for-the-contacts"></a>Lägg till modellen dataklasser för kontakter
Börjar du med att skapa en enkel datamodell i kod.

1. I **Solution Explorer**, högerklicka på mappen modeller, klicka på **Lägg till**, och sedan **klassen**.
   
    ![Lägg till klass i modeller mappen snabbmenyn][adddb001]
2. I den **Lägg till nytt objekt** dialogrutan namn på den nya klassfilen *Contact.cs*, och klicka sedan på **Lägg till**.
   
    ![Lägg till nytt objekt-dialogruta][adddb002]
3. Ersätt innehållet i filen Contacts.cs med följande kod.
   
        using System.Globalization;
        namespace ContactManager.Models
        {
            public class Contact
               {
                public int ContactId { get; set; }
                public string Name { get; set; }
                public string Address { get; set; }
                public string City { get; set; }
                public string State { get; set; }
                public string Zip { get; set; }
                public string Email { get; set; }
                public string Twitter { get; set; }
                public string Self
                {
                    get { return string.Format(CultureInfo.CurrentCulture,
                         "api/contacts/{0}", this.ContactId); }
                    set { }
                }
            }
        }

Den **Kontakta** klassen definierar de data som ska lagras för varje kontakt, plus en primär nyckel, kontakt ID som krävs av databasen. Du kan få mer information om datamodeller i den [nästa steg](#nextsteps) avsnittet i slutet av den här kursen.

### <a name="create-web-pages-that-enable-app-users-to-work-with-the-contacts"></a>Skapa webbsidor som gör appanvändare kan arbeta med kontakter
ASP.NET MVC funktionen scaffold-teknik kan automatiskt generera kod som utför skapa, läsa, uppdatera och ta bort åtgärder (CRUD).

## <a name="add-a-controller-and-a-view-for-the-data"></a>Lägg till en domänkontrollant och en vy för data
1. I **Solution Explorer**, expandera mappen domänkontrollanter.
2. Skapa projektet **(Ctrl + Skift + B)**. (Du måste skapa projektet innan du använder scaffold-teknik mekanism.) 
3. Högerklicka på mappen domänkontrollanter och klicka på **Lägg till**, och klicka sedan på **domänkontrollant**.
   
    ![Lägga till domänkontrollanter i snabbmenyn för domänkontrollanter mapp][addcode001]
4. I den **Lägg till Kodskelett** dialogrutan **MVC-enhet med vyer, med hjälp av Entity Framework** och på **Lägg till**.
   
   ![Lägga till en kontrollant](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. Ange namnet på domänkontrollanten **HomeController**. Välj **Kontakta** som modellklass. Klicka på den **nya datakontexten** knappen och acceptera standardvärdet ”ContactManager.Models.ContactManagerContext” för den **nya datakontexttyp**. Klicka på **Lägg till**.

    En dialogruta där du uppmanas: ”en fil med namnet HomeController redan. Vill du ersätta den ”?. Klicka på **Ja**. Vi skriver över Start styrenheten som skapades med det nya projektet. Vi använder den nya Start-styrenheten för våra kontaktlista.

    Visual Studio skapar kontrollantmetoder och vyer för CRUD-åtgärder för databasen för **Kontakta** objekt.

## <a name="enable-migrations-create-the-database-add-sample-data-and-a-data-initializer"></a>Aktivera migrering, skapa databasen, lägga till exempeldata och en data-initieraren
Nästa uppgift är att aktivera den [Code First Migrations](http://curah.microsoft.com/55220) funktion för att kunna skapa databasen baserat på den datamodell som du skapade.

1. I den **verktyg** väljer du **Library Package Manager** och sedan **Pakethanterarkonsolen**.
   
    ![Package Manager-konsolen i Verktyg-menyn][addcode008]
2. I den **Pakethanterarkonsolen** fönstret, anger du följande kommando:
   
        enable-migrations 
   
    Den **aktivera migreringar** kommando skapar en *migreringar* mapp och dess placeras i den mappen en *Configuration.cs* fil som du kan redigera för att konfigurera migreringar. 
3. I den **Pakethanterarkonsolen** fönstret, anger du följande kommando:
   
        add-migration Initial
   
    Den **lägga till migrering inledande** kommandot genererar en klass som heter  **&lt;date_stamp&gt;inledande** som skapar databasen. Den första parametern ( *inledande* ) är valfri och används för att skapa namn på filen. Du kan se de nya klassen filerna i **Solution Explorer**.
   
    I den **inledande** klassen, de **in** metoden skapar tabellen kontakter och **ned** metod (används när du vill gå tillbaka till föregående tillstånd) släpper den.
4. Öppna den *Migrations\Configuration.cs* fil. 
5. Lägg till följande namnområden. 
   
         using ContactManager.Models;
6. Ersätt den *Seed* metoden med följande kod:
   
        protected override void Seed(ContactManager.Models.ContactManagerContext context)
        {
            context.Contacts.AddOrUpdate(p => p.Name,
               new Contact
               {
                   Name = "Debra Garcia",
                   Address = "1234 Main St",
                   City = "Redmond",
                   State = "WA",
                   Zip = "10999",
                   Email = "debra@example.com",
                   Twitter = "debra_example"
               },
                new Contact
                {
                    Name = "Thorsten Weinrich",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "thorsten@example.com",
                    Twitter = "thorsten_example"
                },
                new Contact
                {
                    Name = "Yuhong Li",
                    Address = "9012 State st",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "yuhong@example.com",
                    Twitter = "yuhong_example"
                },
                new Contact
                {
                    Name = "Jon Orton",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "jon@example.com",
                    Twitter = "jon_example"
                },
                new Contact
                {
                    Name = "Diliana Alexieva-Bosseva",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "diliana@example.com",
                    Twitter = "diliana_example"
                }
                );
        }
   
    Den här koden ovan ska initiera databasen med kontaktinformation. Mer information om seeding databasen finns [felsökning (EF Entity Framework) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).
7. I den **Pakethanterarkonsolen** anger du kommandot:
   
        update-database
   
    ![Package Manager-konsolen kommandon][addcode009]
   
    Den **uppdatering databasen** körs den första migrering som skapar databasen. Som standard skapas databasen som en SQL Server Express LocalDB-databas.
8. Tryck på CTRL+F5 för att köra programmet. 

Programmet visar seed-data och redigera, information och ta bort länkar.

![MVC-vy av data][rxz3]

## <a name="edit-the-view"></a>Redigera vy
1. Öppna den *Views\Home\Index.cshtml* fil. I nästa steg, kommer vi att ersätta den genererade koden med kod som använder [jQuery](http://jquery.com/) och [Knockout.js](http://knockoutjs.com/). Den här nya koden hämtar listan över kontakter från att använda webb-API och JSON och kopplar sedan kontaktdata till Användargränssnittet med knockout.js. Mer information finns i [nästa steg](#nextsteps) avsnittet i slutet av den här kursen. 
2. Ersätt innehållet i filen med följande kod.
   
        @model IEnumerable<ContactManager.Models.Contact>
        @{
            ViewBag.Title = "Home";
        }
        @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                function ContactsViewModel() {
                    var self = this;
                    self.contacts = ko.observableArray([]);
                    self.addContact = function () {
                        $.post("api/contacts",
                            $("#addContact").serialize(),
                            function (value) {
                                self.contacts.push(value);
                            },
                            "json");
                    }
                    self.removeContact = function (contact) {
                        $.ajax({
                            type: "DELETE",
                            url: contact.Self,
                            success: function () {
                                self.contacts.remove(contact);
                            }
                        });
                    }
   
                    $.getJSON("api/contacts", function (data) {
                        self.contacts(data);
                    });
                }
                ko.applyBindings(new ContactsViewModel());    
        </script>
        }
        <ul id="contacts" data-bind="foreach: contacts">
            <li class="ui-widget-content ui-corner-all">
                <h1 data-bind="text: Name" class="ui-widget-header"></h1>
                <div><span data-bind="text: $data.Address || 'Address?'"></span></div>
                <div>
                    <span data-bind="text: $data.City || 'City?'"></span>,
                    <span data-bind="text: $data.State || 'State?'"></span>
                    <span data-bind="text: $data.Zip || 'Zip?'"></span>
                </div>
                <div data-bind="if: $data.Email"><a data-bind="attr: { href: 'mailto:' + Email }, text: Email"></a></div>
                <div data-bind="ifnot: $data.Email"><span>Email?</span></div>
                <div data-bind="if: $data.Twitter"><a data-bind="attr: { href: 'http://twitter.com/' + Twitter }, text: '@@' + Twitter"></a></div>
                <div data-bind="ifnot: $data.Twitter"><span>Twitter?</span></div>
                <p><a data-bind="attr: { href: Self }, click: $root.removeContact" class="removeContact ui-state-default ui-corner-all">Remove</a></p>
            </li>
        </ul>
        <form id="addContact" data-bind="submit: addContact">
            <fieldset>
                <legend>Add New Contact</legend>
                <ol>
                    <li>
                        <label for="Name">Name</label>
                        <input type="text" name="Name" />
                    </li>
                    <li>
                        <label for="Address">Address</label>
                        <input type="text" name="Address" >
                    </li>
                    <li>
                        <label for="City">City</label>
                        <input type="text" name="City" />
                    </li>
                    <li>
                        <label for="State">State</label>
                        <input type="text" name="State" />
                    </li>
                    <li>
                        <label for="Zip">Zip</label>
                        <input type="text" name="Zip" />
                    </li>
                    <li>
                        <label for="Email">E-mail</label>
                        <input type="text" name="Email" />
                    </li>
                    <li>
                        <label for="Twitter">Twitter</label>
                        <input type="text" name="Twitter" />
                    </li>
                </ol>
                <input type="submit" value="Add" />
            </fieldset>
        </form>
3. Högerklicka på mappen innehåll och klicka på **Lägg till**, och klicka sedan på **nytt objekt...** .
   
    ![Lägg till formatmall innehållsmappen snabbmenyn][addcode005]
4. I den **Lägg till nytt objekt** dialogrutan Ange **Style** i övre högra sökrutan och väljer sedan **formatmall**.
    ![Lägg till nytt objekt-dialogruta][rxStyle]
5. Namn på filen *Contacts.css* och på **Lägg till**. Ersätt innehållet i filen med följande kod.
   
        .column {
            float: left;
            width: 50%;
            padding: 0;
            margin: 5px 0;
        }
        form ol {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }
        form li {
            padding: 1px;
            margin: 3px;
        }
        form input[type="text"] {
            width: 100%;
        }
        #addContact {
            width: 300px;
            float: left;
            width:30%;
        }
        #contacts {
            list-style-type: none;
            margin: 0;
            padding: 0;
            float:left;
            width: 70%;
        }
        #contacts li {
            margin: 3px 3px 3px 0;
            padding: 1px;
            float: left;
            width: 300px;
            text-align: center;
            background-image: none;
            background-color: #F5F5F5;
        }
        #contacts li h1
        {
            padding: 0;
            margin: 0;
            background-image: none;
            background-color: Orange;
            color: White;
            font-family: Trebuchet MS, Tahoma, Verdana, Arial, sans-serif;
        }
        .removeContact, .viewImage
        {
            padding: 3px;
            text-decoration: none;
        }
   
    Vi använder den här formatmallen för layout, färger och format som används i appen kontakta manager.
6. Öppna den *App_Start\BundleConfig.cs* fil.
7. Lägg till följande kod för att registrera den [blockerade](http://knockoutjs.com/index.html "KO") plugin-programmet.
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    Det här exemplet använder blockering för att förenkla dynamiska JavaScript-kod som hanterar skärmen mallar.
8. Ändra innehållet/css-post för att registrera den *contacts.css* formatmall. Ändra följande rad:
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   Så här:
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. Kör följande kommando för att installera blockering i Package Manager-konsolen.
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-the-web-api-restful-interface"></a>Lägga till en styrenhet för Web API Restful-gränssnittet
1. I **Solution Explorer**, högerklicka på domänkontrollanter och klickar på **Lägg till** och sedan **domänkontrollant...** 
2. I den **Lägg till Kodskelett** dialogrutan Ange **Web API 2-styrenhet med åtgärder, med hjälp av Entity Framework** och klicka sedan på **Lägg till**.
   
    ![Lägga till API-styrenhet](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. I den **Lägg till styrenhet** dialogrutan Ange ”ContactsController” som domänkontrollantens namn. Välj ”kontakta (ContactManager.Models)” för den **Modellklass**.  Behåll standardvärdet för den **datakontextklass**. 
4. Klicka på **Lägg till**.

### <a name="run-the-application-locally"></a>Kör programmet lokalt
1. Tryck på CTRL+F5 för att köra programmet.
   
    ![Sidan Index][intro001]
2. Ange en kontakt och klicka på **Lägg till**. Appen skickar tillbaka till startsidan och visar den kontakt som du har angett.
   
    ![Sidan index med uppgiften listobjekt][addwebapi004]
3. Lägg till i webbläsaren, **/api/contacts** i URL: en.
   
    Den resulterande URL: en ska likna http://localhost:1234-api-kontakter. RESTful-webb-API som du har lagt till returnerar lagrade kontakter. Firefox och Chrome visas data i XML-format.
   
    ![Sidan index med uppgiften listobjekt][rxFFchrome]

    IE uppmanas du att öppna eller spara kontakter.

    ![Webb-API spara dialogrutan][addwebapi006]


    Du kan öppna returnerade kontakter i anteckningar eller en webbläsare.

    Den här utdatan kan användas av ett annat program, till exempel mobila webbsida eller ett program.

    ![Webb-API spara dialogrutan][addwebapi007]

    **Säkerhetsvarning**: nu är ditt program är osäker och sårbar för angrepp CSRF. Vi tar bort den här säkerhetsrisken senare under kursen. Mer information finns i [hindrar webbplatser begära förfalskning (CSRF) attacker][prevent-csrf-attacks].
## <a name="add-xsrf-protection"></a>Lägga till XSRF skydd
Begäran förfalskning (även kallat XSRF eller CSRF) är en attack mot webbaserat program där en skadlig webbplats kan påverka interaktionen mellan en klientens webbläsare och en webbplats som är betrodda av den webbläsaren. Dessa attacker är möjligt eftersom webbläsare skickar autentiseringstoken automatiskt med varje begäran till en webbplats. Kanoniskt exempel är en autentiseringscookie, till exempel ASP. NETS biljetten för formulär för autentisering. Webbplatser som använder alla beständiga autentiseringsmekanism (till exempel Windows-autentisering, grundläggande och så vidare) kan vara mål av angrepp.

En attack XSRF skiljer sig från en phishing-attacker. Nätfiskeattacker kräver interaktion från drabbade. En skadlig webbplats efterliknar målwebbplatsen i en phishing-attacker och offer lurad till att angriparen känslig information. I en XSRF-attack krävs det ofta utan interaktion från drabbade. I stället förlitande angriparen i webbläsare som automatiskt skickar alla relevanta cookies till mål-webbplatsen.

Mer information finns i [säkerhet öppna Webbapprojektet](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).

1. I **Solution Explorer**, höger **ContactManager** projektet och klicka på **Lägg till** och klicka sedan på **klassen**.
2. Namn på filen *ValidateHttpAntiForgeryTokenAttribute.cs* och Lägg till följande kod:
   
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Helpers;
        using System.Web.Http.Controllers;
        using System.Web.Http.Filters;
        using System.Web.Mvc;
        namespace ContactManager.Filters
        {
            public class ValidateHttpAntiForgeryTokenAttribute : AuthorizationFilterAttribute
            {
                public override void OnAuthorization(HttpActionContext actionContext)
                {
                    HttpRequestMessage request = actionContext.ControllerContext.Request;
                    try
                    {
                        if (IsAjaxRequest(request))
                        {
                            ValidateRequestHeader(request);
                        }
                        else
                        {
                            AntiForgery.Validate();
                        }
                    }
                    catch (HttpAntiForgeryException e)
                    {
                        actionContext.Response = request.CreateErrorResponse(HttpStatusCode.Forbidden, e);
                    }
                }
                private bool IsAjaxRequest(HttpRequestMessage request)
                {
                    IEnumerable<string> xRequestedWithHeaders;
                    if (request.Headers.TryGetValues("X-Requested-With", out xRequestedWithHeaders))
                    {
                        string headerValue = xRequestedWithHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(headerValue))
                        {
                            return String.Equals(headerValue, "XMLHttpRequest", StringComparison.OrdinalIgnoreCase);
                        }
                    }
                    return false;
                }
                private void ValidateRequestHeader(HttpRequestMessage request)
                {
                    string cookieToken = String.Empty;
                    string formToken = String.Empty;
                    IEnumerable<string> tokenHeaders;
                    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
                    {
                        string tokenValue = tokenHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(tokenValue))
                        {
                            string[] tokens = tokenValue.Split(':');
                            if (tokens.Length == 2)
                            {
                                cookieToken = tokens[0].Trim();
                                formToken = tokens[1].Trim();
                            }
                        }
                    }
                    AntiForgery.Validate(cookieToken, formToken);
                }
            }
        }
3. Lägg till följande *med* instruktionen till kontrakt styrenheten så att du har åtkomst till den **[ValidateHttpAntiForgeryToken]** attribut.
   
        using ContactManager.Filters;
4. Lägg till den **[ValidateHttpAntiForgeryToken]** attributet Post-metoderna för den **ContactsController** att skydda mot XSRF hot. Du kan lägga till den ”PutContact”, ”PostContact” och **DeleteContact** åtgärdsmetoder.
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. Uppdatering av *skript* avsnitt i den *Views\Home\Index.cshtml* filen för att inkludera kod för att hämta XSRF-token.
   
         @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                @functions{
                   public string TokenHeaderValue()
                   {
                      string cookieToken, formToken;
                      AntiForgery.GetTokens(null, out cookieToken, out formToken);
                      return cookieToken + ":" + formToken;                
                   }
                }
   
               function ContactsViewModel() {
                  var self = this;
                  self.contacts = ko.observableArray([]);
                  self.addContact = function () {
   
                     $.ajax({
                        type: "post",
                        url: "api/contacts",
                        data: $("#addContact").serialize(),
                        dataType: "json",
                        success: function (value) {
                           self.contacts.push(value);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
                     });
   
                  }
                  self.removeContact = function (contact) {
                     $.ajax({
                        type: "DELETE",
                        url: contact.Self,
                        success: function () {
                           self.contacts.remove(contact);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
   
                     });
                  }
   
                  $.getJSON("api/contacts", function (data) {
                     self.contacts(data);
                  });
               }
               ko.applyBindings(new ContactsViewModel());
            </script>
         }

## <a name="publish-the-application-update-to-azure-and-sql-database"></a>Publicera programuppdateringen till Azure och SQL-databas
Om du vill publicera programmet måste upprepa du proceduren du följt tidigare.

1. I **Solution Explorer**, högerklicka på projektet och välj **publicera**.
   
    ![Publicera][rxP]
2. Klicka på fliken **Settings** (Inställningar).
3. Under **ContactsManagerContext(ContactsManagerContext)**, klicka på den **v** ikon för att ändra *Remote anslutningssträngen* i anslutningssträngen för databasen kontakta. Klicka på **ContactDB**.
   
    ![Inställningar](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. Markera kryssrutan för **köra Code First Migrations (körs vid programstart)**.
5. Klicka på **nästa** och klicka sedan på **Preview**. Visual Studio visar en lista över de filer som lagts till eller uppdaterats.
6. Klicka på **Publicera**.
   När distributionen är klar öppnas i webbläsaren startsidan för programmet.
   
    ![Sidan index med inga kontakter][intro001]
   
    Visual Studio publiceringsprocess automatiskt konfigurerade anslutningssträngen i den distribuerade *Web.config* fil att peka till SQL-databasen. Den kan också konfigureras Code First Migrations för att automatiskt uppgradera databasen till den senaste versionen första gången programmet ansluter till databasen efter distributionen.
   
    På grund av den här konfigurationen Code First skapade databasen genom att köra koden i den **inledande** klass som du skapade tidigare. Det gjorde detta första gången programmet försökte få åtkomst till databasen efter distributionen.
7. Ange en kontakt som du gjorde när du körde appen lokalt, kontrollera att databasen distributionen lyckades.

När du ser att objektet som du anger sparas och visas på sidan Kontakta manager vet du att den har lagrats i databasen.

![Sidan index med kontakter][addwebapi004]

Programmet körs nu i molnet, använder SQL-databas för att lagra data. Ta bort när du är klar med att testa programmet i Azure. Programmet är offentlig och har inte en mekanism för att begränsa åtkomst.

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="next-steps"></a>Nästa steg
Ett annat sätt att lagra data i ett Azure-program är att använda Azure storage som ger lagring av icke-relationella data i form av blobbar och tabeller. Följande länkar ger mer information om Web API, ASP.NET MVC och Windows Azure.

* [Komma igång med Entity Framework med MVC][EFCodeFirstMVCTutorial]
* [Introduktion till ASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [Din första ASP.NET-webb-API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [Felsökning av WAWS](web-sites-dotnet-troubleshoot-visual-studio.md)

Den här kursen och exempelprogrammet har skrivits av [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) med hjälp av Tom Dykstra och Barry Dorrans (Twitter [ @blowdart ](https://twitter.com/blowdart)). 

Kontrollera bättre lämna feedback om vad du tyckte om, eller vad du skulle vilja se, inte bara om kursen sig själv, men även om de produkter som visar den. Din feedback hjälper oss att prioritera förbättringar. Vi kan särskilt intresserad av att ta reda på hur mycket intresse finns i flera automation för processen för att konfigurera och distribuera medlemskapsdatabasen. 

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles to the Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update the Membership Database]:#ppd2
[setupdbenv]: #bkmk_setupdevenv
[setupwindowsazureenv]: #bkmk_setupwindowsazure
[createapplication]: #bkmk_createmvc4app
[deployapp1]: #bkmk_deploytowindowsazure1
[adddb]: #bkmk_addadatabase
[addcontroller]: #bkmk_addcontroller
[addwebapi]: #bkmk_addwebapi
[deploy2]: #bkmk_deploydatabaseupdate

<!-- links -->
[EFCodeFirstMVCTutorial]: http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
[dbcontext-link]: http://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx


<!-- images-->
[rxE]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxE.png
[rxP]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxP.png
[rx22]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/
[rxb2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxb2.png
[rxz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz.png
[rxzz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxzz.png
[rxz2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz2.png
[rxz3]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz3.png
[rxStyle]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxStyle.png
[rxz4]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz4.png
[rxz44]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz44.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxPrevDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPrevDB.png
[rxOverwrite]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxOverwrite.png
[rxPWS]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPWS.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxAddApiController]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxAddApiController.png
[rxFFchrome]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxFFchrome.png
[intro001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobil-intro-finished-web-app.png
[rxCreateWSwithDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxCreateWSwithDB.png
[setup007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-setup-azure-site-004.png
[setup009]: ../Media/dntutmobile-setup-azure-site-006.png
[newapp002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-002.png
[newapp004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-004.png
[firsdeploy007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-005.png
[firsdeploy009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-007.png
[adddb001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-001.png
[adddb002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-002.png
[addcode001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-context-menu.png
[addcode002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-controller-dialog.png
[addcode004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-index-context.png
[addcode005]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-contents-context-menu.png
[addcode007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-bundleconfig-context.png
[addcode008]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-menu.png
[addcode009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-console.png
[addwebapi004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-added-contact.png
[addwebapi006]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-save-returned-contacts.png
[addwebapi007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-contacts-in-notepad.png
[Add XSRF Protection]: #xsrf
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[Add XSRF Protection]: #xsrf
[ImportPublishSettings]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishSettings.png
[ImportPublishProfile]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishProfile.png
[PublishVSSolution]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/PublishVSSolution.png
[ValidateConnection]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ValidateConnection.png
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[prevent-csrf-attacks]: http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-(csrf)-attacks

