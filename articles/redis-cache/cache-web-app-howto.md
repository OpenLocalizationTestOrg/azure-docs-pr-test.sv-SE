---
title: aaaHow toocreate en Webbapp med Redis-Cache | Microsoft Docs
description: "Lär dig hur toocreate ett webbprogram med Redis-Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 454e23d7-a99b-4e6e-8dd7-156451d2da7c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: hero-article
ms.date: 05/09/2017
ms.author: sdanie
ms.openlocfilehash: d3e6df97b06fdf9032570dc360944be4bd7715de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-web-app-with-redis-cache"></a>Hur toocreate ett webbprogram med Redis-Cache
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Den här kursen visar hur toocreate och distribuera ett ASP.NET web application tooa webbapp i Azure App Service med Visual Studio 2017. hello exempelprogrammet visar en lista över team statistik från en databas och visar olika sätt toouse Azure Redis-Cache toostore och hämta data från hello cache. När du slutför hello kursen har du en webbapp som körs som läser och skriver tooa databasen optimerats med Azure Redis-Cache och värdbaserad i Azure.

Du får lära dig:

* Hur toocreate en ASP.NET MVC 5 webbprogrammet i Visual Studio.
* Hur tooaccess data från en databas med hjälp av Entity Framework.
* Hur tooimprove datagenomströmning och minska databasbelastningen genom att lagra och hämta data med hjälp av Azure Redis-Cache.
* Hur toouse en Redis sorterade set tooretrieve hello övre 5 team.
* Hur tooprovision hello Azure-resurser för hello program med hjälp av en Resource Manager-mall.
* Hur toopublish hello programmet tooAzure med Visual Studio.

## <a name="prerequisites"></a>Krav
Du måste ha hello följande förutsättningar toocomplete hello kursen.

* [Azure-konto](#azure-account)
* [Visual Studio 2017 med hello Azure SDK för .NET](#visual-studio-2017-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Azure-konto
Du behöver en Azure-konto toocomplete hello vägledning. Du kan:

* [Öppna ett Azure-konto kostnadsfritt](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Du får då krediter som kan vara används tootry ut betald Azure-tjänster. Även efter hello krediten är slut, kan du hålla hello-konto och använda gratis Azure-tjänster och funktioner.
* [Aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Din MSDN-prenumeration ger dig krediter varje månad som kan användas för Azure-betaltjänster.

### <a name="visual-studio-2017-with-hello-azure-sdk-for-net"></a>Visual Studio 2017 med hello Azure SDK för .NET
hello kursen gäller för Visual Studio 2017 med hello [Azure SDK för .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools). hello Azure SDK 2.9.5 ingår i installationsprogrammet för hello Visual Studio.

Om du har Visual Studio 2015, du kan följa hello självstudier med hello [Azure SDK för .NET](../dotnet-sdk.md) 2.8.2 eller senare. [Hämta hello senaste Azure SDK för Visual Studio 2015 här](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio installeras automatiskt med hello SDK om du inte redan har det. Vissa av skärmarna kan skilja sig från hello illustrationer visas i den här kursen.

Om du har Visual Studio 2013, kan du [download hello senaste Azure SDK för Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Vissa av skärmarna kan skilja sig från hello illustrationer visas i den här kursen.

## <a name="create-hello-visual-studio-project"></a>Skapa hello Visual Studio-projekt
1. Öppna Visual Studio och klicka på **Arkiv**, **Nytt**, **Projekt**.
2. Expandera hello **Visual C#** nod i hello **mallar** väljer **moln**, och klicka på **ASP.NET-webbprogram**. Kontrollera att **.NET Framework 4.5.2** eller senare har valts.  Typen **ContosoTeamStats** till hello **namn** textrutan och klicka på **OK**.
   
    ![Skapa projekt][cache-create-project]
3. Välj **MVC** som hello projekttypen. 

    Se till att **ingen autentisering** har angetts för hello **autentisering** inställningar. Hello standard kan endast ange toosomething annan beroende på din version av Visual Studio. toochange, klickar du på **ändra autentisering** och välj **ingen autentisering**.

    Om du följer tillsammans med Visual Studio 2015, avmarkera hello **värd i molnet hello** kryssrutan. Du kommer [etablera hello Azure-resurser](#provision-the-azure-resources) och [publicera hello programmet tooAzure](#publish-the-application-to-azure) i efterföljande steg i självstudiekursen hello. Ett exempel på etablering av en Apptjänst-webbapp från Visual Studio genom att låta **värd i molnet hello** markerad finns [Kom igång med Webbappar i Azure App Service med hjälp av ASP.NET och Visual Studio](../app-service-web/app-service-web-get-started-dotnet.md).
   
    ![Välj projektmall][cache-select-template]
4. Klicka på **OK** toocreate hello projektet.

## <a name="create-hello-aspnet-mvc-application"></a>Skapa hello ASP.NET MVC-program
I det här avsnittet av kursen hello skapar du grundläggande hello-program som läser och visar team statistik från en-databas.

* [Lägg till hello Entity Framework NuGet-paketet](#add-the-entity-framework-nuget-package)
* [Lägga till hello modell](#add-the-model)
* [Lägga till hello-styrenhet](#add-the-controller)
* [Konfigurera hello vyer](#configure-the-views)

### <a name="add-hello-entity-framework-nuget-package"></a>Lägg till hello Entity Framework NuGet-paketet

1. Klicka på **NuGet Package Manager**, **Pakethanterarkonsolen** från hello **verktyg** menyn.
2. Kör hello följande kommando från hello **Pakethanterarkonsolen** fönster.
    
    ```
    Install-Package EntityFramework
    ```

Mer information om det här paketet finns hello [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet-sidan.

### <a name="add-hello-model"></a>Lägga till hello modell
1. Högerklicka på **Modeller** i **Solution Explorer** och välj **Lägg till**, **Klass**. 
   
    ![Lägg till modell][cache-model-add-class]
2. Ange `Team` för hello klassnamn och klickar på **Lägg till**.
   
    ![Lägg till modellklass][cache-model-add-class-dialog]
3. Ersätt hello `using` instruktioner överst hello i hello `Team.cs` fil med hello följande `using` instruktioner.

    ```c#
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.SqlServer;
    ```


1. Ersätt hello definitionen av hello `Team` klassen med följande kodavsnitt som innehåller en uppdaterad hello `Team` klassen definition samt vissa andra hjälpprogramklasser för Entity Framework. Mer information om hello kod första metoden tooEntity Framework som används i den här kursen finns [kod första tooa ny databas](https://msdn.microsoft.com/data/jj193542).

    ```c#
    public class Team
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
        public int Ties { get; set; }
    
        static public void PlayGames(IEnumerable<Team> teams)
        {
            // Simple random generation of statistics.
            Random r = new Random();
    
            foreach (var t in teams)
            {
                t.Wins = r.Next(33);
                t.Losses = r.Next(33);
                t.Ties = r.Next(0, 5);
            }
        }
    }
    
    public class TeamContext : DbContext
    {
        public TeamContext()
            : base("TeamContext")
        {
        }
    
        public DbSet<Team> Teams { get; set; }
    }
    
    public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
    {
        protected override void Seed(TeamContext context)
        {
            var teams = new List<Team>
            {
                new Team{Name="Adventure Works Cycles"},
                new Team{Name="Alpine Ski House"},
                new Team{Name="Blue Yonder Airlines"},
                new Team{Name="Coho Vineyard"},
                new Team{Name="Contoso, Ltd."},
                new Team{Name="Fabrikam, Inc."},
                new Team{Name="Lucerne Publishing"},
                new Team{Name="Northwind Traders"},
                new Team{Name="Consolidated Messenger"},
                new Team{Name="Fourth Coffee"},
                new Team{Name="Graphic Design Institute"},
                new Team{Name="Nod Publishers"}
            };
    
            Team.PlayGames(teams);
    
            teams.ForEach(t => context.Teams.Add(t));
            context.SaveChanges();
        }
    }
    
    public class TeamConfiguration : DbConfiguration
    {
        public TeamConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
        }
    }
    ```


1. I **Solution Explorer**, dubbelklicka på **web.config** tooopen den.
   
    ![Web.config][cache-web-config]
2. Lägg till följande hello `connectionStrings` avsnitt. hello namnet på hello anslutningssträng måste matcha hello namnet på hello Entity Framework databasen kontexten klassen som är `TeamContext`.

    ```xml
    <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    Du kan lägga till nya hello `connectionStrings` avsnittet så att den följer `configSections`som visas i följande exempel hello.

    ```xml
    <configuration>
      <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
      </configSections>
      <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
      </connectionStrings>
      ...
      ```

    > [!NOTE]
    > Anslutningssträngen kan vara olika beroende på hello version av Visual Studio och SQL Server Express edition används toocomplete hello kursen. hello web.config mallen ska vara konfigurerade toomatch installationen och kan innehålla `Data Source` poster som `(LocalDB)\v11.0` (från SQL Server Express 2012) eller `Data Source=(LocalDB)\MSSQLLocalDB` (från SQL Server Express 2014 och nyare). Mer information om anslutningssträngar och SQL Express-versioner finns i [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) .

### <a name="add-hello-controller"></a>Lägga till hello-styrenhet
1. Tryck på **F6** toobuild hello projektet. 
2. I **Solution Explorer**, högerklicka på hello **domänkontrollanter** mapp och välj **Lägg till**, **domänkontrollant**.
   
    ![Lägga till en kontrollant][cache-add-controller]
3. Välj **MVC 5-kontrollant med vyer, med hjälp av Entity Framework** och klicka på **Lägg till**. Om du får ett felmeddelande när du klickar på **Lägg till**, se till att du har skapat hello projekt först.
   
    ![Lägga till en kontrollantklass][cache-add-controller-class]
4. Välj **Team (ContosoTeamStats.Models)** från hello **Modellklass** listrutan. Välj **TeamContext (ContosoTeamStats.Models)** från hello **datakontextklass** listrutan. Typen `TeamsController` i hello **domänkontrollant** textrutan (om det inte fylls i automatiskt). Klicka på **Lägg till** toocreate hello controller-klassen och Lägg till hello standardvyer.
   
    ![Konfigurera kontrollanten][cache-configure-controller]
5. I **Solution Explorer**, expandera **Global.asax** och dubbelklicka på **Global.asax.cs** tooopen den.
   
    ![Global.asax.cs][cache-global-asax]
6. Lägg till följande två hello `using` instruktioner överst hello i hello-filen under hello andra `using` instruktioner.

    ```c#
    using System.Data.Entity;
    using ContosoTeamStats.Models;
    ```


1. Lägg till följande kodrad i slutet av hello av hello hello `Application_Start` metod.

    ```c#
    Database.SetInitializer<TeamContext>(new TeamInitializer());
    ```


1. I **Solution Explorer** expanderar du `App_Start` och dubbelklickar på `RouteConfig.cs`.
   
    ![RouteConfig.cs][cache-RouteConfig-cs]
2. Ersätt `controller = "Home"` i följande kod i hello hello `RegisterRoutes` metod med `controller = "Teams"` som visas i följande exempel hello.

    ```c#
    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
    );
    ```


### <a name="configure-hello-views"></a>Konfigurera hello vyer
1. I **Solution Explorer**, expandera hello **vyer** mapp och sedan hello **delade** mappen och dubbelklickar på **_Layout.cshtml**. 
   
    ![_Layout.cshtml][cache-layout-cshtml]
2. Ändra hello innehåll hello `title` element och Ersätt `My ASP.NET Application` med `Contoso Team Stats` som visas i följande exempel hello.

    ```html
    <title>@ViewBag.Title - Contoso Team Stats</title>
    ```


1. I hello `body` och uppdatera hello först `Html.ActionLink` -instruktionen och Ersätt `Application name` med `Contoso Team Stats` och Ersätt `Home` med `Teams`.
   
   * Innan: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
   * Efter: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`
     
     ![Kodändringar][cache-layout-cshtml-code]
2. Tryck på **Ctrl + F5** toobuild och kör hello-programmet. Den här versionen av programmet hello läser hello resultat direkt från hello-databasen. Obs hello **Skapa nytt**, **redigera**, **information**, och **ta bort** åtgärder som automatiskt har lagts till toohello program av hello **MVC 5 styrenhet med vyer, med hjälp av Entity Framework** kodskelett. I nästa avsnitt om hello hello kursen ska du lägga till Redis-Cache toooptimize hello data komma åt och erbjuder ytterligare funktioner toohello program.

![Startprogram][cache-starter-application]

## <a name="configure-hello-application-toouse-redis-cache"></a>Konfigurera hello programmet toouse Redis-Cache
I det här avsnittet av kursen hello du konfigurera hello exempel programmet toostore och hämta Contoso teamet statistik från Azure Redis-Cache-instans med hjälp av hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) cacheklienten.

* [Konfigurera hello programmet toouse StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
* [Uppdatera hello TeamsController klassen tooreturn resultat från hello cache eller hello-databas](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [Uppdatera hello skapa, redigera och ta bort metoder toowork med hello-cache](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [Uppdatera hello team Index visa toowork med hello-cache](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-hello-application-toouse-stackexchangeredis"></a>Konfigurera hello programmet toouse StackExchange.Redis
1. tooconfigure ett klientprogram i Visual Studio med hello StackExchange.Redis NuGet-paketet, klickar du på **NuGet Package Manager**, **Pakethanterarkonsolen** från hello **verktyg** menyn.
2. Kör hello följande kommando från hello `Package Manager Console` fönster.
    
    ```
    Install-Package StackExchange.Redis
    ```
   
    Hej NuGet paket hämtar och lägger till hello krävs sammansättningsreferenser för din klient programmet tooaccess Azure Redis-Cache med hello StackExchange.Redis cache-klienten. Om du föredrar toouse ett starkt krypterat namn version av hello `StackExchange.Redis` klientbiblioteket, installera hello `StackExchange.Redis.StrongName` paketet.
3. I **Solution Explorer**, expandera hello **domänkontrollanter** mappen och dubbelklickar på **TeamsController.cs** tooopen den.
   
    ![Teamkontrollant][cache-teamscontroller]
4. Lägg till följande två hello `using` instruktioner för**TeamsController.cs**.

    ```c#   
    using System.Configuration;
    using StackExchange.Redis;
    ```

5. Lägg till följande två egenskaper toohello hello `TeamsController` klass.

    ```c#   
    // Redis Connection string info
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

6. Skapa en fil på datorn med namnet `WebAppPlusCacheAppSecrets.config` och placera den på en plats som inte kontrolleras in med hello källkoden för exempelprogrammet, bör du bestämma toocheck den i någon plats. I det här exemplet hello `AppSettingsSecrets.config` filen finns `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.
   
    Redigera hello `WebAppPlusCacheAppSecrets.config` och Lägg till hello efter innehållet. Om du kör programmet hello lokalt är den här informationen används tooconnect tooyour Azure Redis-Cache-instans. Senare i självstudiekursen hello du etablera en instans av Azure Redis-Cache och uppdatera hello cache namn och lösenord. Om du inte planerar toorun hello exempelprogrammet lokalt kan du hoppa över hello skapandet av den här filen och hello efterföljande steg som refererar till hello filen eftersom när du distribuerar tooAzure hello program hämtar hello cache anslutningsinformation från hello app inställningen för hello Web App och inte från den här filen. Eftersom hello `WebAppPlusCacheAppSecrets.config` distribueras inte tooAzure med ditt program behöver du inte den om du ska toorun hello programmet lokalt.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. I **Solution Explorer**, dubbelklicka på **web.config** tooopen den.
   
    ![Web.config][cache-web-config]
2. Lägg till följande hello `file` attributet toohello `appSettings` element. Om du använde ett annat namn eller en plats i stället dessa värden för hello som visas i hello exempel.
   
   * Innan: `<appSettings>`
   * Efter: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`
     
   hello ASP.NET-körningsmiljön sammanfogar hello innehållet i hello externa filen med hello markeringar i hello `<appSettings>` element. hello runtime ignorerar hello Filattributet om hello angivna filen inte kan hittas. Din hemliga (hello anslutning sträng tooyour cache) ingår inte som en del av hello källkoden för hello program. När du distribuerar din web app tooAzure hello `WebAppPlusCacheAppSecrests.config` filen inte distribueras (som du vill). Det finns flera sätt toospecify dessa hemligheter för i Azure och i den här självstudiekursen de konfigureras automatiskt för dig när du [etablera hello Azure-resurser](#provision-the-azure-resources) i ett efterföljande steg. Mer information om hur du arbetar med hemligheter i Azure finns [bästa praxis för att distribuera lösenord och andra känsliga data tooASP.NET och Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

### <a name="update-hello-teamscontroller-class-tooreturn-results-from-hello-cache-or-hello-database"></a>Uppdatera hello TeamsController klassen tooreturn resultat från hello cache eller hello-databas
I det här exemplet kan team statistik hämtas från databasen hello eller hello cache. Team statistik lagras i cacheminnet för hello som en serialiserad `List<Team>`, och även som en sorterad uppsättning med Redis-datatyper. När du hämtar objekt från en sorterad uppsättning hämtar du vissa, alla eller frågar efter vissa objekt. I det här exemplet ska du fråga hello sorteras set för hello övre 5 team rangordnas av antal wins.

> [!NOTE]
> Det är inte obligatoriska toostore hello team statistik i olika format i hello-cachen i ordning toouse Azure Redis-Cache. Den här kursen använder flera format toodemonstrate vissa hello olika sätt och olika datatyper du kan använda toocache data.
> 
> 

1. Lägg till följande hello `using` instruktioner toohello `TeamsController.cs` filen överst hello med hello andra `using` instruktioner.

    ```c#   
    using System.Diagnostics;
    using Newtonsoft.Json;
    ```

2. Ersätta hello aktuella `public ActionResult Index()` metodimplementering med hello efter implementering.

    ```c#
    // GET: Teams
    public ActionResult Index(string actionType, string resultType)
    {
        List<Team> teams = null;

        switch(actionType)
        {
            case "playGames": // Play a new season of games.
                PlayGames();
                break;

            case "clearCache": // Clear hello results from hello cache.
                ClearCachedTeams();
                break;

            case "rebuildDB": // Rebuild hello database with sample data.
                RebuildDB();
                break;
        }

        // Measure hello time it takes tooretrieve hello results.
        Stopwatch sw = Stopwatch.StartNew();

        switch(resultType)
        {
            case "teamsSortedSet": // Retrieve teams from sorted set.
                teams = GetFromSortedSet();
                break;

            case "teamsSortedSetTop5": // Retrieve hello top 5 teams from hello sorted set.
                teams = GetFromSortedSetTop5();
                break;

            case "teamsList": // Retrieve teams from hello cached List<Team>.
                teams = GetFromList();
                break;

            case "fromDB": // Retrieve results from hello database.
            default:
                teams = GetFromDB();
                break;
        }

        sw.Stop();
        double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

        // Add hello elapsed time of hello operation toohello ViewBag.msg.
        ViewBag.msg += " MS: " + ms.ToString();

        return View(teams);
    }
    ```


1. Lägg till följande tre metoder toohello hello `TeamsController` klassen tooimplement hello `playGames`, `clearCache`, och `rebuildDB` åtgärdstyper från hello växla instruktionen som har lagts till i hello föregående kodfragment.
   
    Hej `PlayGames` metoden programuppdateringarna hello team statistik genom att simulera en säsongen spel, sparar hello resultat toohello databasen och raderar hello nu inaktuella data från hello cache.

    ```c#
    void PlayGames()
    {
        ViewBag.msg += "Updating team statistics. ";
        // Play a "season" of games.
        var teams = from t in db.Teams
                    select t;

        Team.PlayGames(teams);

        db.SaveChanges();

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Hej `RebuildDB` metoden initierar hello databasen med hello standarduppsättning team genererar statistik för dem och raderar hello nu inaktuella data från hello cache.

    ```c#
    void RebuildDB()
    {
        ViewBag.msg += "Rebuilding DB. ";
        // Delete and re-initialize hello database with sample data.
        db.Database.Delete();
        db.Database.Initialize(true);

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Hej `ClearCachedTeams` metod tar du bort cachelagrade team statistik från hello cache.

    ```c#
    void ClearCachedTeams()
    {
        IDatabase cache = Connection.GetDatabase();
        cache.KeyDelete("teamsList");
        cache.KeyDelete("teamsSortedSet");
        ViewBag.msg += "Team data removed from cache. ";
    } 
    ```


1. Lägg till följande fyra metoderna toohello hello `TeamsController` klassen tooimplement hello hämtas hello team statistik från hello cache och hello databasen på olika sätt. Var och en av dessa metoder returnerar en `List<Team>` som visas av hello vyn.
   
    Hej `GetFromDB` metoden läser hello team statistik från hello-databasen.
   
    ```c#
    List<Team> GetFromDB()
    {
        ViewBag.msg += "Results read from DB. ";
        var results = from t in db.Teams
            orderby t.Wins descending
            select t; 

        return results.ToList<Team>();
    }
    ```

    Hej `GetFromList` metod läser hello team statistik från cachen som en serialiserad `List<Team>`. Om det finns en cache-miss, läsa från hello databasen hello team statistik och lagras sedan i hello cache för nästa gång. I det här exemplet använder vi JSON.NET serialisering tooserialize hello .NET objekt tooand från hello cache. Mer information finns i [hur toowork med .NET objekt i Azure Redis-Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

    ```c#
    List<Team> GetFromList()
    {
        List<Team> teams = null;

        IDatabase cache = Connection.GetDatabase();
        string serializedTeams = cache.StringGet("teamsList");
        if (!String.IsNullOrEmpty(serializedTeams))
        {
            teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

            ViewBag.msg += "List read from cache. ";
        }
        else
        {
            ViewBag.msg += "Teams list cache miss. ";
            // Get from database and store in cache
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
        }
        return teams;
    }
    ```

    Hej `GetFromSortedSet` metoden läser hello team statistik från en cachelagrad sorterade. Om det finns en cache-miss, läsa från hello databasen och hello cachelagras som en sorterad uppsättning hello team statistik.

    ```c#
    List<Team> GetFromSortedSet()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();
        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
        if (teamsSortedSet.Count() > 0)
        {
            ViewBag.msg += "Reading sorted set from cache. ";
            teams = new List<Team>();
            foreach (var t in teamsSortedSet)
            {
                Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                teams.Add(tt);
            }
        }
        else
        {
            ViewBag.msg += "Teams sorted set cache miss. ";

            // Read from DB
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            foreach (var t in teams)
            {
                Console.WriteLine("Adding toosorted set: {0} - {1}", t.Name, t.Wins);
                cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
            }
        }
        return teams;
    }
    ```

    Hej `GetFromSortedSetTop5` metoden läser hello övre 5 team från hello cachelagras sorteras uppsättningen. Startar det genom att kontrollerar hello cache hello befintliga hello `teamsSortedSet` nyckel. Om den här nyckeln inte finns hello `GetFromSortedSet` metoden anropas tooread hello team statistik och lagrar dem i hello cache. Därefter efterfrågas hello cachelagrade sorterade set hello övre 5 team som returneras.

    ```c#
    List<Team> GetFromSortedSetTop5()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();

        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        if(teamsSortedSet.Count() == 0)
        {
            // Load hello entire sorted set into hello cache.
            GetFromSortedSet();

            // Retrieve hello top 5 teams.
            teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        }

        ViewBag.msg += "Retrieving top 5 teams from cache. ";
        // Get hello top 5 teams from hello sorted set
        teams = new List<Team>();
        foreach (var team in teamsSortedSet)
        {
            teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
        }
        return teams;
    }
    ```

### <a name="update-hello-create-edit-and-delete-methods-toowork-with-hello-cache"></a>Uppdatera hello skapa, redigera och ta bort metoder toowork med hello-cache
hello scaffold-teknik för kod som har skapats som en del av det här exemplet innehåller metoder tooadd, redigera och ta bort team. Varje gång som ett team läggs till, redigera eller ta bort blir hello data i hello cache inaktuella. I det här avsnittet ska du ändra cachelagras dessa tre metoder tooclear hello team så att hello cachen inte synkroniserad med hello-databasen.

1. Bläddra toohello `Create(Team team)` metod i hello `TeamsController` klass. Lägg till ett anrop toohello `ClearCachedTeams` -metoden, som visas i följande exempel hello.

    ```c#
    // POST: Teams/Create
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Teams.Add(team);
            db.SaveChanges();
            // When a team is added, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }

        return View(team);
    }
    ```


1. Bläddra toohello `Edit(Team team)` metod i hello `TeamsController` klass. Lägg till ett anrop toohello `ClearCachedTeams` -metoden, som visas i följande exempel hello.

    ```c#
    // POST: Teams/Edit/5
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Entry(team).State = EntityState.Modified;
            db.SaveChanges();
            // When a team is edited, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }
        return View(team);
    }
    ```


1. Bläddra toohello `DeleteConfirmed(int id)` metod i hello `TeamsController` klass. Lägg till ett anrop toohello `ClearCachedTeams` -metoden, som visas i följande exempel hello.

    ```c#
    // POST: Teams/Delete/5
    [HttpPost, ActionName("Delete")]
    [ValidateAntiForgeryToken]
    public ActionResult DeleteConfirmed(int id)
    {
        Team team = db.Teams.Find(id);
        db.Teams.Remove(team);
        db.SaveChanges();
        // When a team is deleted, hello cache is out of date.
        // Clear hello cached teams.
        ClearCachedTeams();
        return RedirectToAction("Index");
    }
    ```


### <a name="update-hello-teams-index-view-toowork-with-hello-cache"></a>Uppdatera hello team Index visa toowork med hello-cache
1. I **Solution Explorer**, expandera hello **vyer** mapp och sedan hello **team** mappen och dubbelklickar på **Index.cshtml**.
   
    ![Index.cshtml][cache-views-teams-index-cshtml]
2. Leta efter hello följande punkt elementet hello övre delen av hello-filen.
   
    ![Åtgärdstabell][cache-teams-index-table]
   
    Detta är hello länken toocreate ett nytt team. Ersätt hello punkt element med hello i den följande tabellen. Den här tabellen har åtgärdslänkar för att skapa ett nytt team spela upp en ny säsongen för spel, hello cachen rensas, hämtar hello team från hello-cache i olika format, hämta hello team från hello databas och återskapa hello databas med ny exempeldata.

    ```html
    <table class="table">
        <tr>
            <td>
                @Html.ActionLink("Create New", "Create")
            </td>
            <td>
                @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
            </td>
            <td>
                @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
            </td>
            <td>
                @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
            </td>
            <td>
                @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
            </td>
            <td>
                @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
            </td>
            <td>
                @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
            </td>
            <td>
                @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
            </td>
        </tr>    
    </table>
    ```


1. Rulla toohello längst ned på hello **Index.cshtml** och Lägg till följande hello `tr` elementet så att den är hello sista raden i hello senast tabell i hello-filen.
   
    ```html
    <tr><td colspan="5">@ViewBag.Msg</td></tr>
    ```
   
    Den här raden visar hello värdet för `ViewBag.Msg` som innehåller en statusrapport om hello den aktuella åtgärden. Hej `ViewBag.Msg` anges när du klickar på någon av hello åtgärdslänkar hello föregående steg.   
   
    ![Statusmeddelande][cache-status-message]
2. Tryck på **F6** toobuild hello projektet.

## <a name="provision-hello-azure-resources"></a>Etablera hello Azure-resurser
toohost ditt program i Azure måste du först etablera hello Azure-tjänster som krävs för ditt program. hello exempelprogrammet i den här kursen använder hello följande Azure-tjänster.

* Azure Redis Cache
* App Service-webbapp
* SQL Database

toodeploy dessa tjänster tooa ny eller befintlig resursgrupp för ditt val klickar du på följande hello **distribuera tooAzure** knappen.

[! [Distribuera tooAzure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Detta **distribuera tooAzure** knappen använder hello [skapa en Webbapp plus Redis-Cache plus SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) mallen tooprovision dessa tjänster och ange hello anslutningssträngen för hello SQL-databasen och hello inställningen för hello anslutningssträngen för Azure Redis-Cache.

> [!NOTE]
> Om du inte har något Azure-konto kan du [skapa ett kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) på bara några minuter.
> 
> 

Klicka på hello **distribuera tooAzure** tar dig toohello Azure-portalen och initierar hello skapar hello resurser som beskrivs av hello mallen.

![Distribuera tooAzure][cache-deploy-to-azure-step-1]

1. I hello **grunderna** avsnittet Välj hello Azure-prenumeration toouse och välj en befintlig resursgrupp eller skapa ett nytt och ange hello resursgruppens plats.
2. I hello **inställningar** , anger ett **administratörsinloggning** (Använd inte **admin**), **inloggningen administratörslösenord**, och  **Databasnamn**. hello har andra parametrar konfigurerats för en kostnadsfri Apptjänst värd planerings- och lägre kostnad alternativ för hello SQL Database och Azure Redis-Cache som inte levereras med en kostnadsfri nivå.

    ![Distribuera tooAzure][cache-deploy-to-azure-step-2]

3. Rulla toohello slutet av hello-sidan, skrivskyddade hello villkor när du har konfigurerat hello önskade inställningar och kontrollera hello **acceptera toohello villkoren ovan** kryssrutan.
4. toobegin etablering hello resurser, klickar du på **inköp**.

tooview hello förloppet för din distribution, klicka på meddelandeikonen hello och på **distributionen har startat**.

![Distributionen har påbörjats][cache-deployment-started]

Du kan visa hello statusen för distributionen på hello **Microsoft.Template** bladet.

![Distribuera tooAzure][cache-deploy-to-azure-step-3]

När etableringen är klar, kan du publicera dina program tooAzure från Visual Studio.

> [!NOTE]
> Eventuella fel som kan uppstå under etableringsprocessen hello visas på hello **Microsoft.Template** bladet. Vanliga fel är för många SQL-servrar eller för många värdplaner för den kostnadsfria apptjänsten per prenumeration. Lös eventuella fel och starta om hello processen genom att klicka på **omdistribuera** på hello **Microsoft.Template** bladet eller hello **distribuera tooAzure** knappen i den här självstudiekursen.
> 
> 

## <a name="publish-hello-application-tooazure"></a>Publicera hello programmet tooAzure
I det här steget i självstudiekursen hello du publicera hello programmet tooAzure och kör det i hello molnet.

1. Högerklicka på hello **ContosoTeamStats** projektet i Visual Studio och välj **publicera**.
   
    ![Publicera][cache-publish-app]
2. Klicka på **Microsoft Azure App Service**, välj **Välj befintlig** och klicka på **Publicera**.
   
    ![Publicera][cache-publish-to-app-service]
3. Välj hello-prenumeration som används när du skapar hello Azure-resurser, expandera hello resursgruppen som innehåller hello resurser och välj hello önskad Web App. Om du använde hello **distribuera tooAzure** knappen Web App-namnet börjar med **webbplats** följt av vissa ytterligare tecken.
   
    ![Välj webbapp][cache-select-web-app]
4. Klicka på **OK** toobegin hello publiceringsprocessen. Efter en liten stund hello publiceringsprocessen har slutförts och en webbläsare startas med hello kör exempelprogrammet. Om du får ett DNS-fel vid validering eller publicera och hello etableringsprocessen för hello Azure-resurser för programmet hello har precis slutfört, vänta en stund och försök igen.
   
    ![Cachen har lagts till][cache-added-to-application]

hello beskrivs följande tabell varje åtgärdslänken från hello exempelprogram.

| Åtgärd | Beskrivning |
| --- | --- |
| Skapa ny |Skapa ett nytt team. |
| Spela säsong |Spela upp ett säsongen för spel, uppdatering hello team statistik och avmarkera något inaktuell team data från hello cache. |
| Rensa cacheminne |Rensa hello team statistik från hello cache. |
| Lista från cache |Hämta hello team statistik från hello cache. Om det finns en cache-miss, läsa in hello statistik från hello-databasen och spara toohello cache för nästa gång. |
| Sorterad uppsättning från cachen |Hämta hello team statistik från hello cache med hjälp av en sortering. Om det finns en cache-miss, läsa in hello statistik från hello-databasen och spara toohello cache med hjälp av en sortering. |
| Översta 5 teamen från cachen |Hämta hello övre 5 team från hello cache med hjälp av en sortering. Om det finns en cache-miss, läsa in hello statistik från hello-databasen och spara toohello cache med hjälp av en sortering. |
| Läsa in från databas |Hämta hello team statistik från hello-databas. |
| Återskapa databas |Återskapa hello databasen och öppna den igen med exempeldata i teamet. |
| Redigera/Information/Ta bort |Redigera ett team, visa information om ett team, ta bort ett team. |

Klicka på några av hello åtgärder och experimentera med att hämta hello data från olika källor för hello. Inte hello skillnader i hello tid det tar toocomplete hello hello data hämtades från hello databasen och hello cache på olika sätt.

## <a name="delete-hello-resources-when-you-are-finished-with-hello-application"></a>Ta bort hello resurser när du är klar med hello program
När du är klar med hello självstudiekursen exempelprogram hello Azure kan ta bort resurser som används i ordning tooconserve kostnad och resurser. Om du använder hello **distribuera tooAzure** knapp i hello [etablera hello Azure-resurser](#provision-the-azure-resources) avsnittet och alla dina resurser finns i hello samma resursgrupp, du kan ta bort dem tillsammans i en åtgärden genom att ta bort hello resursgruppen.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) och på **resursgrupper**.
2. Ange hello namnet på resursgruppen i hello **Filtrera objekt...**  textruta.
3. Klicka på **...**  toohello höger i resursgruppen.
4. Klicka på **Ta bort**.
   
    ![Ta bort][cache-delete-resource-group]
5. Hello-typnamn för din resursgrupp och klicka på **ta bort**.
   
    ![Bekräfta borttagning][cache-delete-confirm]

Efter några ögonblick hello resurs tas gruppen och alla dess befintliga resurser bort.

> [!IMPORTANT]
> Observera att om du tar bort en resursgrupp är irreversibel och att hello resursgruppen och alla hello resurser i den tas bort permanent. Kontrollera att du inte oavsiktligt bort hello fel resursgrupp eller resurser. Om du har skapat hello resurser som värd för det här exemplet i en befintlig resursgrupp kan du ta bort varje resurs separat från deras respektive blad.
> 
> 

## <a name="run-hello-sample-application-on-your-local-machine"></a>Kör exempelprogrammet hello på din lokala dator
toorun hello program lokalt på datorn, du behöver ett Azure Redis-Cache-instansen i vilka toocache dina data. 

* Om du har publicerat program-tooAzure enligt beskrivningen i föregående avsnitt i hello kan använda du hello Azure Redis-Cache-instans som har etablerats under steget.
* Om du har en annan befintlig Azure Redis-Cache-instans kan använda du den toorun det här exemplet lokalt.
* Om du behöver toocreate en Azure Redis-Cache-instans, kan du följa hello stegen i [skapa ett cacheminne](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

När du har valt eller skapat hello cache toouse, bläddra toohello cache i hello Azure-portalen och hämta hello [värdnamn](cache-configure.md#properties) och [åtkomstnycklar](cache-configure.md#access-keys) för ditt cacheminne. Instruktioner finns i [Konfigurera Redis-cacheinställningarna](cache-configure.md#configure-redis-cache-settings).

1. Öppna hello `WebAppPlusCacheAppSecrets.config` -fil som du skapade under hello [konfigurera hello programmet toouse Redis-Cache](#configure-the-application-to-use-redis-cache) steg i den här kursen använder hello redigeringsprogram.
2. Redigera hello `value` attributet och Ersätt `MyCache.redis.cache.windows.net` med hello [värdnamn](cache-configure.md#properties) för ditt cacheminne, och ange antingen hello [primära och sekundära nycklarna](cache-configure.md#access-keys) för ditt cacheminne som hello lösenord.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. Tryck på **Ctrl + F5** toorun hello program.

> [!NOTE]
> Observera att eftersom hello-programmet, inklusive hello databas körs lokalt och hello Redis-Cache finns i Azure, hello cache kan visas toounder-utföra hello-databasen. För bästa prestanda hello klientprogrammet och Azure Redis-Cache-instansen måste vara i hello samma plats. 
> 
> 

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [komma igång med ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) på hello [ASP.NET](http://asp.net/) plats.
* Fler exempel för att skapa en ASP.NET-Webbapp i App Service finns [skapa och distribuera en ASP.NET-webbapp i Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) från hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Anslut [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
  * Läs mer Snabbstart från hello HealthClinic.biz demo [Azure Developer Tools Snabbstart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
* Mer information om hello [kod första tooa ny databas](https://msdn.microsoft.com/data/jj193542) hanterar tooEntity Framework som används i den här kursen.
* Läs mer om [webbappar i Azure App Service](../app-service-web/app-service-web-overview.md).
* Lär dig hur för[övervakaren](cache-how-to-monitor.md) ditt cacheminne i hello Azure-portalen.
* Utforska Azure Redis Caches premiumfunktioner
  
  * [Hur tooconfigure persistence för Premium Azure Redis-Cache](cache-how-to-premium-persistence.md)
  * [Hur tooconfigure klustring för Premium Azure Redis-Cache](cache-how-to-premium-clustering.md)
  * [Hur tooconfigure virtuella nätverket har stöd för Premium Azure Redis-Cache](cache-how-to-premium-vnet.md)
  * Se hello [Azure Redis-Cache vanliga frågor och svar](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) för mer information om storlek, genomflöde och bandbredd med premium cacheminnen.

<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

