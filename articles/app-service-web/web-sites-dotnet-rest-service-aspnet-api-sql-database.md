---
title: aaaCreate en REST-API i Azure med ASP.NET och SQL-databas | Microsoft Docs
description: "En självstudiekurs som lär du dig hur toodeploy en app som använder hello ASP.NET Web API tooan Azure-webbapp med hjälp av Visual Studio."
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
ms.openlocfilehash: 1ef45dd1582bfda367e53c39f863164422ad678b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-rest-service-using-aspnet-web-api-and-sql-database-in-azure-app-service"></a>Skapa ett REST-tjänst som använder ASP.NET Web API och SQL-databas i Azure App Service
Den här kursen visar hur toodeploy ett ASP.NET web app tooan [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) med hjälp av guiden för hello Publicera webbplats i Visual Studio 2013 eller Visual Studio 2013 Community Edition. 

Du kan öppna ett Azure-konto gratis och om du inte redan har Visual Studio 2013, hello SDK installerar Visual Studio 2013 automatiskt för Web Express. Så kan du börja utveckla för Azure och helt kostnadsfritt.

Den här kursen förutsätter att du har några tidigare erfarenheter av att använda Azure. På den här kursen har du en enkel webbapp upp och körs i hello moln.

Du får lära dig:

* Hur tooenable datorn för Azure-utveckling genom att installera hello Azure SDK.
* Hur toocreate en Visual Studio ASP.NET MVC 5 projektet och publicera den tooan Azure-app.
* Hur toouse hello ASP.NET Web API tooenable Restful-API-anrop.
* Hur toouse en SQL-databas toostore data i Azure.
* Hur uppdaterar toopublish programmet tooAzure.

Du skapar ett enkelt kontaktlista webbprogram som bygger på ASP.NET MVC 5 och använder hello ADO.NET Entity Framework för åtkomst till databasen. Följande bild visar hello hello slutförts program:

![Skärmbild av webbplats][intro001]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### <a name="create-hello-project"></a>Skapa hello-projekt
1. Starta Visual Studio 2013.
2. Från hello **filen** Klicka **nytt projekt**.
3. I hello **nytt projekt** dialogrutan Expandera **Visual C#** och välj **Web** och välj sedan **ASP.NET-webbprogram**. Namnge programmet hello **ContactManager** och på **OK**.
   
    ![Dialogrutan Nytt projekt](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)
4. I hello **nytt ASP.NET-projekt** dialogrutan, Välj hello **MVC** mall, kontrollera **Web API** och klicka sedan på **ändra autentisering**.
5. I hello **ändra autentisering** dialogrutan klickar du på **ingen autentisering**, och klicka sedan på **OK**.
   
    ![Ingen autentisering](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)
   
    hello exempelprogram som du skapar inte funktioner som kräver användare toolog i. Information om hur tooimplement autentisering och auktorisering funktioner finns hello [nästa steg](#nextsteps) avsnittet hello slutet av den här kursen. 
6. I hello **nytt ASP.NET-projekt** dialogrutan, se till att hello **värd i hello molnet** är markerad och klicka på **OK**.

Om du redan inte har registrerat i tooAzure, kommer du att ange toosign i.

1. hello konfigurationsguiden föreslås ett unikt namn baserat på *ContactManager* (se hello bilden nedan). Välj en region nära dig. Du kan använda [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") toofind hello lägsta svarstid datacenter. 
2. Om du inte har skapat en databasserver innan du väljer **Skapa ny server**, ange ett användarnamn och lösenord.
   
    ![Konfigurera Azure-webbplats](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

Om du har en databasserver, använder du den toocreate en ny databas. Databasservrar är en värdefull resurs och vanligtvis vill toocreate flera databaser på hello samma server för testning och utveckling istället för att skapa en databasserver per databas. Se till att webbplatsen och databasen tillhör hello samma region.

![Konfigurera Azure-webbplats](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### <a name="set-hello-page-header-and-footer"></a>Ange hello sidhuvud och sidfot
1. I **Solution Explorer**, expandera hello *Views\Shared* mapp och öppna hello *_Layout.cshtml* fil.
   
    ![_Layout.cshtml i Solution Explorer][newapp004]
2. Ersätt hello innehållet i hello *Views\Shared_Layout.cshtml* fil med hello följande kod:

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

hello markup ovan ändringar hello appens namn från ”Mina ASP.NET-App” för ”Kontakta Manager” och tar bort hello länkar för**Start**, **om** och **Kontakta**.

### <a name="run-hello-application-locally"></a>Kör hello programmet lokalt
1. Tryck på CTRL + F5 toorun hello program.
   startsidan för hello programmet visas i hello standardwebbläsare.
    ![startsidan för tooDo lista](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)

Det här är allt du behöver toodo för att du ska distribuera tooAzure nu toocreate hello program. Senare ska du lägga till databasfunktioner.

## <a name="deploy-hello-application-tooazure"></a>Distribuera hello programmet tooAzure
1. I Visual Studio högerklickar du på hello-projekt i **Solution Explorer** och välj **publicera** hello snabbmenyn.
   
    ![Publicera i snabbmenyn för projektet][PublishVSSolution]
   
    Hej **Publicera webbplats** öppnas guiden.
2. Klicka på **Publicera**.

![Fliken Inställningar](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

Visual Studio börjar hello processen att kopiera hello filer toohello Azure-servern. Hej **utdata** fönster visar vilka distributionsåtgärder som utförts och rapporterar slutförande av hello-distribution.

1. hello standardwebbläsare öppnas automatiskt toohello URL för hello distribueras webbplats.
   
   hello-program som du skapade körs nu i hello molnet.
   
   ![startsidan för tooDo lista körs i Azure][rxz2]

## <a name="add-a-database-toohello-application"></a>Lägg till ett databasprogram toohello
Därefter måste du uppdatera hello MVC-program tooadd hello möjlighet toodisplay och uppdatera kontakter och lagra hello data i en databas. hello program använder hello Entity Framework toocreate hello databasen och tooread och uppdatera data i hello-databas.

### <a name="add-data-model-classes-for-hello-contacts"></a>Lägg till modellen dataklasser för hello kontakter
Börjar du med att skapa en enkel datamodell i kod.

1. I **Solution Explorer**, högerklicka på mappen för hello modeller, klicka på **Lägg till**, och sedan **klassen**.
   
    ![Lägg till klass i modeller mappen snabbmenyn][adddb001]
2. I hello **Lägg till nytt objekt** dialogrutan, namnet hello nya klassfilen *Contact.cs*, och klicka sedan på **Lägg till**.
   
    ![Lägg till nytt objekt-dialogruta][adddb002]
3. Ersätt hello innehållet i hello Contacts.cs filen med hello följande kod.
   
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

Hej **Kontakta** definierar klassen hello data som ska lagras för varje kontakt, plus en primär nyckel, kontakt ID som krävs av hello-databasen. Du kan hämta mer information om datamodeller i hello [nästa steg](#nextsteps) avsnittet hello slutet av den här kursen.

### <a name="create-web-pages-that-enable-app-users-toowork-with-hello-contacts"></a>Skapa webbsidor som gör app användare toowork med hello kontakter
hello ASP.NET MVC hello scaffold-teknik för funktionen automatiskt generera kod som utför skapa, läsa, uppdatera och ta bort åtgärder (CRUD).

## <a name="add-a-controller-and-a-view-for-hello-data"></a>Lägg till en domänkontrollant och en vy för hello data
1. I **Solution Explorer**, expandera hello domänkontrollanter mappen.
2. Skapa hello projektet **(Ctrl + Skift + B)**. (Du måste skapa hello projektet innan du använder scaffold-teknik mekanism.) 
3. Högerklicka på mappen för hello-styrenheter och klicka på **Lägg till**, och klicka sedan på **domänkontrollant**.
   
    ![Lägga till domänkontrollanter i snabbmenyn för domänkontrollanter mapp][addcode001]
4. I hello **Lägg till Kodskelett** dialogrutan **MVC-enhet med vyer, med hjälp av Entity Framework** och på **Lägg till**.
   
   ![Lägga till en kontrollant](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)
5. Ange hello Kontrollnamn för**HomeController**. Välj **Kontakta** som modellklass. Klicka på hello **nya datakontexten** knappen och acceptera hello standard ”ContactManager.Models.ContactManagerContext” för hello **nya datakontexttyp**. Klicka på **Lägg till**.

    En dialogruta där du uppmanas: ”en fil med namnet hello HomeController redan. Vill du tooreplace den ”?. Klicka på **Ja**. Vi skriver över hello Start domänkontrollant som har skapats med hello nytt projekt. Vi använder hello nya Start styrenhet för våra kontaktlista.

    Visual Studio skapar kontrollantmetoder och vyer för CRUD-åtgärder för databasen för **Kontakta** objekt.

## <a name="enable-migrations-create-hello-database-add-sample-data-and-a-data-initializer"></a>Aktivera migrering, skapa hello databas, lägga till exempeldata och en data-initieraren
hello nästa uppgift är tooenable hello [Code First Migrations](http://curah.microsoft.com/55220) funktion i ordning toocreate hello databasen utifrån hello-datamodell som du skapade.

1. I hello **verktyg** väljer du **Library Package Manager** och sedan **Pakethanterarkonsolen**.
   
    ![Package Manager-konsolen i Verktyg-menyn][addcode008]
2. I hello **Pakethanterarkonsolen** fönstret Ange hello följande kommando:
   
        enable-migrations 
   
    Hej **aktivera migreringar** kommando skapar en *migreringar* mapp och dess placeras i den mappen en *Configuration.cs* fil att du kan redigera tooconfigure migreringar. 
3. I hello **Pakethanterarkonsolen** fönstret Ange hello följande kommando:
   
        add-migration Initial
   
    Hej **lägga till migrering inledande** kommandot genererar en klass med namnet  **&lt;date_stamp&gt;inledande** som skapar hello-databasen. Hej första parametern ( *inledande* ) är valfri och används toocreate hello namnet på hello-filen. Du kan se hello nya klassfiler i **Solution Explorer**.
   
    I hello **inledande** class, hello **in** metoden skapar hello tabellen för kontakter och hello **ned** metod (används när du vill tooreturn toohello läget) släpper den.
4. Öppna hello *Migrations\Configuration.cs* fil. 
5. Lägg till hello följande namnområden. 
   
         using ContactManager.Models;
6. Ersätt hello *Seed* metod med hello följande kod:
   
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
   
    Den här koden ovan ska initiera hello databasen med hello kontaktinformation. Mer information om seeding hello databasen finns [felsökning (EF Entity Framework) DBs](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx).
7. I hello **Pakethanterarkonsolen** ange hello-kommando:
   
        update-database
   
    ![Package Manager-konsolen kommandon][addcode009]
   
    Hej **uppdatering databasen** körs hello första migrering som skapar hello-databas. Som standard skapas hello databasen som en SQL Server Express LocalDB-databas.
8. Tryck på CTRL + F5 toorun hello program. 

hello programmet hello fördefiniera data och ger redigera, information och ta bort länkar.

![MVC-vy av data][rxz3]

## <a name="edit-hello-view"></a>Redigera hello vy
1. Öppna hello *Views\Home\Index.cshtml* fil. I hello nästa steg, kommer vi att ersätta hello genereras markup med kod som använder [jQuery](http://jquery.com/) och [Knockout.js](http://knockoutjs.com/). Den här nya koden hämtar hello lista över kontakter från att använda webb-API och JSON och bindningar hello kontakta data toohello användargränssnitt med knockout.js. Mer information finns i hello [nästa steg](#nextsteps) avsnittet hello slutet av den här kursen. 
2. Ersätt hello innehållet i hello-fil med hello följande kod.
   
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
3. Högerklicka på hello innehållsmappen och på **Lägg till**, och klicka sedan på **nytt objekt...** .
   
    ![Lägg till formatmall innehållsmappen snabbmenyn][addcode005]
4. I hello **Lägg till nytt objekt** dialogrutan Ange **Style** i hello övre högra sökrutan och väljer sedan **formatmall**.
    ![Lägg till nytt objekt-dialogruta][rxStyle]
5. Namnet hello filen *Contacts.css* och på **Lägg till**. Ersätt hello innehållet i hello-fil med hello följande kod.
   
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
   
    Vi använder den här formatmallen för hello layout, färger och format som används i hello kontakta manager app.
6. Öppna hello *App_Start\BundleConfig.cs* fil.
7. Lägg till följande kod tooregister hello hello [blockerade](http://knockoutjs.com/index.html "KO") plugin-programmet.
   
        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    Det här exemplet använder blockerade toosimplify dynamiska JavaScript-kod som hanterar mallar för hello-skärmen.
8. Ändra hello innehållet/css post tooregister hello *contacts.css* formatmall. Ändra hello följande rad:
   
                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
   Så här:
   
        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));
9. Kör hello efter kommandot tooinstall blockering i hello Package Manager-konsolen.
   
        Install-Package knockoutjs

## <a name="add-a-controller-for-hello-web-api-restful-interface"></a>Lägga till en styrenhet för hello Web API Restful-gränssnitt
1. I **Solution Explorer**, högerklicka på domänkontrollanter och klickar på **Lägg till** och sedan **domänkontrollant...** 
2. I hello **Lägg till Kodskelett** dialogrutan Ange **Web API 2-styrenhet med åtgärder, med hjälp av Entity Framework** och klicka sedan på **Lägg till**.
   
    ![Lägga till API-styrenhet](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)
3. I hello **Lägg till styrenhet** dialogrutan Ange ”ContactsController” som domänkontrollantens namn. Välj ”kontakta (ContactManager.Models)” för hello **Modellklass**.  Behåll hello standardvärdet för hello **datakontextklass**. 
4. Klicka på **Lägg till**.

### <a name="run-hello-application-locally"></a>Kör hello programmet lokalt
1. Tryck på CTRL + F5 toorun hello program.
   
    ![Sidan Index][intro001]
2. Ange en kontakt och klicka på **Lägg till**. hello app returnerar toohello startsidan och visar hello-kontakt som du har angett.
   
    ![Sidan index med uppgiften listobjekt][addwebapi004]
3. Lägg till i hello webbläsare, **/api/contacts** toohello URL.
   
    hello resulterande URL: en ska likna http://localhost:1234-api-kontakter. hello RESTful-webb-API som du har lagt till returnerar hello lagras kontakter. Firefox och Chrome visar hello data i XML-format.
   
    ![Sidan index med uppgiften listobjekt][rxFFchrome]

    Internet Explorer ska fråga tooopen eller spara hello kontakter.

    ![Webb-API spara dialogrutan][addwebapi006]


    Du kan öppna hello returneras kontakter i anteckningar eller en webbläsare.

    Den här utdatan kan användas av ett annat program, till exempel mobila webbsida eller ett program.

    ![Webb-API spara dialogrutan][addwebapi007]

    **Säkerhetsvarning**: nu är ditt program är osäker och sårbar tooCSRF attack. Senare i självstudiekursen hello vi ta bort den här säkerhetsrisken. Mer information finns i [hindrar webbplatser begära förfalskning (CSRF) attacker][prevent-csrf-attacks].
## <a name="add-xsrf-protection"></a>Lägga till XSRF skydd
Begäran förfalskning (även kallat XSRF eller CSRF) är en attack mot webbaserat program där en skadlig webbplats kan påverka hello interaktionen mellan en klientens webbläsare och en webbplats som är betrodda av den webbläsaren. Dessa attacker är möjligt eftersom webbläsare skickar autentiseringstoken automatiskt med varje begäran tooa webbplats. hello kanoniska exempel är en autentiseringscookie, till exempel ASP. NETS biljetten för formulär för autentisering. Webbplatser som använder alla beständiga autentiseringsmekanism (till exempel Windows-autentisering, grundläggande och så vidare) kan vara mål av angrepp.

En attack XSRF skiljer sig från en phishing-attacker. Nätfiskeattacker kräver interaktion från hello drabbade. En skadlig webbplats efterliknar hello målwebbplats i en phishing-attacker och hello drabbade lurad till att tillhandahålla känslig information toohello angripare. I en XSRF-attack krävs det ofta utan interaktion från hello drabbade. I stället förlitande hello angripare hello webbläsare som automatiskt skickar alla relevanta cookies toohello mål webbplats.

Mer information finns i hello [säkerhet öppna Webbapprojektet](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_\(CSRF\)).

1. I **Solution Explorer**, höger **ContactManager** projektet och klicka på **Lägg till** och klicka sedan på **klassen**.
2. Namnet hello filen *ValidateHttpAntiForgeryTokenAttribute.cs* och Lägg till följande kod hello:
   
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
3. Lägg till följande hello *med* instruktionen toohello kontrakt domänkontrollant så att du har åtkomst toohello **[ValidateHttpAntiForgeryToken]** attribut.
   
        using ContactManager.Filters;
4. Lägg till hello **[ValidateHttpAntiForgeryToken]** attributet toohello Post-metoderna för hello **ContactsController** tooprotect från XSRF hot. Du lägger till det toohello ”PutContact”, ”PostContact” och **DeleteContact** åtgärdsmetoder.
   
        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {
5. Uppdatera hello *skript* avsnitt i hello *Views\Home\Index.cshtml* filen tooinclude kod tooget hello XSRF token.
   
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

## <a name="publish-hello-application-update-tooazure-and-sql-database"></a>Publicera hello programmet uppdatering tooAzure och SQL-databas
toopublish hello programmet du upprepa hello proceduren du följt tidigare.

1. I **Solution Explorer**, högerklicka på hello projektet och välj **publicera**.
   
    ![Publicera][rxP]
2. Klicka på hello **inställningar** fliken.
3. Under **ContactsManagerContext(ContactsManagerContext)**, klicka på hello **v** ikonen toochange *Remote anslutningssträngen* toohello anslutningssträngen för hello kontakt databas. Klicka på **ContactDB**.
   
    ![Inställningar](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)
4. Kryssrutan för hello **köra Code First Migrations (körs vid programstart)**.
5. Klicka på **nästa** och klicka sedan på **Preview**. Visual Studio visar en lista över hello-filer som lagts till eller uppdaterats.
6. Klicka på **Publicera**.
   När hello distributionen är klar öppnar toohello startsida hello programmet hello webbläsaren.
   
    ![Sidan index med inga kontakter][intro001]
   
    hello Visual Studio Publicera process som automatiskt konfigurerade hello anslutningssträngen i hello distribueras *Web.config* filen toopoint toohello SQL-databas. Det kan också konfigurerad Code First Migrations tooautomatically uppgradera hello toohello senaste databasversionen hello första gången hello program ansluter hello databasen efter distributionen.
   
    På grund av den här konfigurationen Code First skapade hello databas genom att köra hello kod i hello **inledande** klass som du skapade tidigare. Det gjorde hello första gången hello programmet försökte tooaccess hello databasen efter distributionen.
7. Ange en kontakt som du gjorde när du körde hello appen lokalt, tooverify databasen distributionen lyckades.

När du ser att hello-objektet som du anger sparas och visas på hello kontakta manager-sidan, vet du har lagrats i hello-databasen.

![Sidan index med kontakter][addwebapi004]

hello programmet körs nu i hello molnet med hjälp av SQL-databas toostore sina data. Ta bort när du är klar med att testa hello program i Azure. hello programmet är offentlig och har inte en mekanism toolimit åtkomst.

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="next-steps"></a>Nästa steg
Ett annat sätt toostore data i ett Azure-program är toouse Azure-lagring som innehåller icke-relationella datalagring i hello form av blobbar och tabeller. hello följande länkar ger mer information om Web API, ASP.NET MVC och Windows Azure.

* [Komma igång med Entity Framework med MVC][EFCodeFirstMVCTutorial]
* [Introduktion tooASP.NET MVC 5](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [Din första ASP.NET-webb-API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [Felsökning av WAWS](web-sites-dotnet-troubleshoot-visual-studio.md)

Den här kursen och hello exempelprogrammet har skrivits av [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [ @RickAndMSFT ](https://twitter.com/RickAndMSFT)) med hjälp av Tom Dykstra och Barry Dorrans (Twitter [ @blowdart ](https://twitter.com/blowdart)). 

Lämna feedback på vad du tyckte om, eller vad du vill toosee förbättras inte bara om hello kursen själva, utan även om hello produkter som visar den. Din feedback hjälper oss att prioritera förbättringar. Vi kan särskilt intresserad av att ta reda på hur mycket intresse det är i flera automation för hello process för att konfigurera och distribuera hello medlemskap i databasen. 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles toohello Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update hello Membership Database]:#ppd2
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

