---
title: "aaaHow toowork med serverdelen för hello .NET SDK för Mobile Apps | Microsoft Docs"
description: "Lär dig hur toowork med hello serverdelen för .NET SDK för Azure Apptjänst Mobilappar."
keywords: "App service, azure app service, mobilapp, mobiltjänsten, skala, skalbara och app-distribution, azure app-distribution"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2946c5ba4424565ac764e2ce5597bf42362fcedf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-hello-net-backend-server-sdk-for-azure-mobile-apps"></a>Arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Det här avsnittet visar hur toouse hello serverdelen för .NET SDK i Azure Apptjänst Mobilappar scenarier. hello Azure Mobile Apps-SDK hjälper dig att arbeta med mobila klienter från ASP.NET-programmet.

> [!TIP]
> Hej [server .NET SDK för Azure Mobile Apps] [ 2] är öppen källkod på GitHub. hello-databasen innehåller all källkod inklusive hello hela servern SDK enhet test suite och vissa exempelprojekt.
>
>

## <a name="reference-documentation"></a>Referensdokumentation
hello-referensdokumentationen för hello server SDK finns här: [.NET-referens för Azure Mobile Apps][1].

## <a name="create-app"></a>Så här: skapa en .NET-mobilappsserverdel
Om du startar ett nytt projekt, kan du skapa en Apptjänst-program med hjälp av antingen hello [Azure-portalen] eller Visual Studio. Du kan köra hello Apptjänst program lokalt eller publicera hello projektet tooyour molnbaserade Apptjänst mobilappar.

Om du lägger till mobila funktioner tooan befintligt projekt, se hello [ladda ned och initiera hello SDK](#install-sdk) avsnitt.

### <a name="create-a-net-backend-using-hello-azure-portal"></a>Skapa en .NET-serverdel med hello Azure-portalen
toocreate en Apptjänst mobilserverdel antingen följa hello [Snabbstartsguide] [ 3] eller följa dessa steg:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Tillbaka i hello *Kom igång* bladet under **skapa en tabell API**, Välj **C#** som din **språk för serverdel**. Klicka på **hämta**extrahera den komprimerade filen filer tooyour lokala datorn och öppna hello lösningen i Visual Studio.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Skapa en .NET-serverdel med hjälp av Visual Studio 2013 och Visual Studio 2015
Installera hello [Azure SDK för .NET] [ 4] (version 2.9.0 eller senare) toocreate ett Azure Mobile Apps-projekt i Visual Studio. När du har installerat hello SDK, skapa ett ASP.NET-program med hjälp av hello följande steg:

1. Öppna hello **nytt projekt** dialogrutan (från *filen* > **ny** > **projekt...** ).
2. Expandera **mallar** > **Visual C#**, och välj **Web**.
3. Välj **ASP.NET-webbprogram**.
4. Fyll i hello projektnamn. Klicka sedan på **OK**.
5. Under *ASP.NET 4.5.2-mallen mallar*väljer **Azure Mobile App**. Kontrollera **värd i molnet hello** toocreate mobilserverdel i hello molnet toowhich som du kan publicera det här projektet.
6. Klicka på **OK**.

## <a name="install-sdk"></a>Så här: hämta och initiera hello SDK
hello SDK är tillgängligt på [NuGet.org]. Det här paketet innehåller hello grundläggande funktion som krävs för tooget igång med hello SDK. tooinitialize hello SDK måste du tooperform åtgärder på hello **HttpConfiguration** objekt.

### <a name="install-hello-sdk"></a>Installera hello SDK
tooinstall hello SDK, högerklicka på hello server-projekt i Visual Studio, markera **hantera NuGet-paket**, Sök efter hello [Microsoft.Azure.Mobile.Server] paketet och klicka sedan på  **Installera**.

### <a name="server-project-setup"></a>Initiera hello serverprojekt
Ett .NET-serverdel serverprojekt är initierad liknande tooother ASP.NET-projekt, genom att lägga till en OWIN-startklass. Se till att du har refererade hello NuGet-paketet `Microsoft.Owin.Host.SystemWeb`. tooadd den här klassen i Visual Studio högerklickar du på serverprojektet och välj **Lägg till** >
**nytt objekt**, sedan **Web**  >  ** Allmän** > **OWIN-startklass**.  En klass skapas med hello följande attribut:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

I hello `Configuration()` metod för din OWIN-startklass, Använd en **HttpConfiguration** objekt tooconfigure hello Azure Mobile Apps miljö.
följande exempel hello initierar hello serverprojekt med inga extra funktioner:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

tooenable enskilda funktioner, måste du anropa tilläggsmetoder på hello **MobileAppConfiguration** objektet innan du anropar **gällafoderråvaran**. Till exempel hello följande kod lägger till hello standard dirigerar tooall API-domänkontrollanter som har hello attribut `[MobileAppController]` under initieringen:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

hello server quickstart från hello Azure portal anrop **UseDefaultConfiguration()**. Den här motsvarande toohello efter installationen:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from hello Home package
            .MapApiControllers()
            .AddTables(                               // from hello Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from hello Entity package
                )
            .AddPushNotifications()                   // from hello Notifications package
            .MapLegacyCrossDomainController()         // from hello CrossDomain package
            .ApplyTo(config);

hello tillägget metoderna är:

* `AddMobileAppHomeController()`ger hello standardstartsida för Azure Mobile Apps.
* `MapApiControllers()`ger anpassad API-funktioner för WebAPI domänkontrollanter dekorerad med hello `[MobileAppController]` attribut.
* `AddTables()`innehåller en mappning av hello `/tables` slutpunkter tootable domänkontrollanter.
* `AddTablesWithEntityFramework()`är en kort hand för mappning hello `/tables` slutpunkter som använder Entity Framework-baserade domänkontrollanter.
* `AddPushNotifications()`ger en enkel metod för att registrera enheter för Notification Hubs.
* `MapLegacyCrossDomainController()`innehåller standard CORS-huvuden för lokal utveckling.

### <a name="sdk-extensions"></a>SDK-tillägg
följande NuGet-baserade tilläggspaket hello har olika mobila funktioner som kan användas av ditt program. Du aktiverar tillägg under initiering med hjälp av hello **MobileAppConfiguration** objekt.

* [Microsoft.Azure.Mobile.Server.Quickstart] stöder hello grundläggande inställningar för Mobilappar. Tillagda toohello konfiguration genom att anropa hello **UseDefaultConfiguration** tilläggsmetoden under initieringen. Det här tillägget ingår följande tillägg: meddelanden, autentisering, enhet, tabeller, domäner och Home paket. Det här paketet används av hello Mobile Apps Quickstart på hello Azure-portalen.
* [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) implementerar hello standard *mobila appen är igång sidan* för hello webbplatsroten. Lägg till toohello konfiguration genom att anropa den **AddMobileAppHomeController** metod.
* [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) innehåller klasser för att arbeta med data och anger upp hello data pipeline. Lägg till toohello konfiguration genom att anropa hello **AddTables** metod.
* [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) aktiverar hello Entity Framework tooaccess data i hello SQL-databas. Lägg till toohello konfiguration genom att anropa hello **AddTablesWithEntityFramework** metod.
* [Microsoft.Azure.Mobile.Server.Authentication] aktiverar autentisering och anger upp hello OWIN mellanprogram används toovalidate token. Lägg till toohello konfiguration genom att anropa hello **AddAppServiceAuthentication** och **IAppBuilder**. **UseAppServiceAuthentication** tilläggsmetoder.
* [Microsoft.Azure.Mobile.Server.Notifications] aktiverar push-meddelanden och definierar en slutpunkt för push-registrering. Lägg till toohello konfiguration genom att anropa hello **AddPushNotifications** metod.
* [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) skapar en domänkontrollant som fungerar data toolegacy webbläsare från din Mobilapp. Lägg till toohello konfiguration genom att anropa den **MapLegacyCrossDomainController** metod.
* [Microsoft.Azure.Mobile.Server.Login] ger hello AppServiceLoginHandler.CreateToken() metod som är en statisk metod som används under scenarier för anpassad autentisering.

## <a name="publish-server-project"></a>Så här: publicera hello serverprojekt
Det här avsnittet visar hur toopublish .NET-serverdel projektet i Visual Studio. Du kan också distribuera serverdelsprojektet med Git eller någon av hello andra metoder som beskrivs i hello [dokumentationen för Azure App Service](../app-service-web/web-sites-deploy.md).

1. Återskapa hello projektet toorestore NuGet-paket i Visual Studio.
2. I Solution Explorer, högerklicka på hello projektet klickar du på **publicera**. hello måste första gången du publicerar toodefine en publiceringsprofilen. När du redan har en profil som har definierats, markerar du den och klicka på **publicera**.
3. Om du blir tillfrågad tooselect ett publiceringsmål, klickar du på **Microsoft Azure App Service** > **nästa**, och sedan (vid behov) logga in med dina autentiseringsuppgifter för Azure.
   Visual Studio hämtar och lagrar på ett säkert sätt dina publiceringsinställningar direkt från Azure.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. Välj din **prenumeration**väljer **resurstypen** från **visa**, expandera **Mobilapp**, och serverdelen för Mobilappen klickar **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. Kontrollera hello publicera profilinformation och klickar på **publicera**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    När din mobilappsserverdel har publicerats visas en landningssida som anger lyckades.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <a name="define-table-controller"></a>Så här: definiera en tabell-styrenhet
Definiera en tabell Controller tooexpose en SQL-tabell toomobile klienter.  Konfigurera en tabell Controller kräver tre steg:

1. Skapa en klass för Data Transfer objekt (DTO).
2. Konfigurera en tabellreferens i hello Mobile DbContext-klassen.
3. Skapa en tabell-styrenhet.

En Data Transfer objekt (DTO) är ett vanligt C#-objekt som ärver från `EntityData`.  Exempel:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

Hej DTO är används toodefine hello tabell i hello SQL-databas.  toocreate hello databasen post, lägga till en `DbSet<>` egenskapen hello DbContext som du använder.  I hello projektet standardmallen för Azure Mobile Apps hello DbContext kallas `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Om du har hello Azure SDK har installerats kan skapa du nu en mall för tabellen domänkontrollant på följande sätt:

1. Högerklicka på hello domänkontrollanter mapp och välj **Lägg till** > **domänkontrollant...** .
2. Välj hello **Azure Mobile Apps tabell Controller** alternativ och klicka sedan på **Lägg till**.
3. I hello **Lägg till styrenhet** dialogrutan:
   * I hello **Modellklass** listrutan, Välj din nya DTO.
   * I hello **DbContext** listrutan, Välj hello mobila tjänsten DbContext-klassen.
   * hello Kontrollnamn skapas åt dig.
4. Klicka på **Lägg till**.

hello server snabbstartsprojektet innehåller ett exempel på en enkel **TodoItemController**.

### <a name="adjust-pagesize"></a>Så här: justera hello tabell växlingsfilens storlek
Som standard returnerar Azure Mobile Apps 50 poster per begäran.  Sidindelning säkerställer att hello-klienten inte binder det sina UI-tråden eller hello Server under för lång tid att säkerställa en bra användarupplevelse. toochange hello tabellens växlingsfilens storlek, öka hello serversidan ”tillåtna storleken för frågan” och hello klientsidan sidan storlek hello serversidan ”tillåtna storleken för frågan” justeras med hello `EnableQuery` attribut:

    [EnableQuery(PageSize = 500)]

Kontrollera hello PageSize hello samma eller större än hello storlek som har begärts av hello-klienten.  Finns toohello specifika klient ta dokumentation för information om hur du ändrar hello sidstorleken för klienten.

## <a name="how-to-define-a-custom-api-controller"></a>Så här: definiera en anpassad API-styrenhet
hello anpassade API-kontrollanten ger hello mest grundläggande funktioner tooyour mobilappsserverdel genom att exponera en slutpunkt. Du kan registrera en mobil-specifika API-kontrollanten med hello [MobileAppController]-attributet. Hej `MobileAppController` attributet registrerar hello flödet, ställer in hello Mobile Apps JSON-serialisering och aktiverar [klienten versionskontroll](app-service-mobile-client-and-server-versioning.md).

1. I Visual Studio, högerklicka på hello domänkontrollanter mappen och klicka på **Lägg till** > **domänkontrollant**väljer **Web API 2-styrenhet&mdash;tom** och Klicka på **Lägg till**.
2. Ange en **Kontrollnamn**, som `CustomController`, och klicka på **Lägg till**.
3. Hello nya klassen för styrenheten, lägga till hello följande med instruktionen:

        using Microsoft.Azure.Mobile.Server.Config;
4. Tillämpa hello **[MobileAppController]** attributet toohello API klassdefinitionen styrenhet, som i följande exempel hello:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. App_Start/Startup.MobileApp.cs filen, lägga till ett anrop toohello **MapApiControllers** metod, som i följande exempel hello:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Du kan också använda hello `UseDefaultConfiguration()` metod i stället för `MapApiControllers()`. Alla domänkontrollanter som inte har **MobileAppControllerAttribute** tillämpas kan fortfarande användas av klienter, men det kanske inte korrekt förbrukas av klienter som använder en Mobilapp klient-SDK.

## <a name="how-to-work-with-authentication"></a>Så här: arbeta med autentisering
Azure Mobile Apps använder App Service-autentisering / auktorisering toosecure din mobila serverdel.  Det här avsnittet beskrivs hur du tooperform hello efter autentisering-relaterade uppgifter i projektet .NET backend-servern:

* [Så här: Lägg till autentisering tooa serverprojekt](#add-auth)
* [Så här: Använd anpassad autentisering för ditt program](#custom-auth)
* [Så här: hämta autentiserad användarinformation](#user-info)
* [Så här: begränsa åtkomst till data för behöriga användare](#authorize)

### <a name="add-auth"></a>Så här: Lägg till autentisering tooa serverprojekt
Du kan lägga till autentisering tooyour serverprojekt genom att utöka hello **MobileAppConfiguration** objekt och konfigurera OWIN mellanprogram. När du installerar hello [Microsoft.Azure.Mobile.Server.Quickstart] paket- och anrop hello **UseDefaultConfiguration** metod, kan du hoppa över toostep 3.

1. I Visual Studio, installera hello [Microsoft.Azure.Mobile.Server.Authentication] paketet.
2. Hej Startup.cs projektfilen, lägga till följande kodrad hello början av hello hello **Configuration** metoden:

        app.UseAppServiceAuthentication(config);

    Den här OWIN mellanprogram komponenten verifierar token som utfärdats av hello associerade Apptjänst gateway.
3. Lägg till hello `[Authorize]` attributet tooany domänkontrollant eller metoden som kräver autentisering.

toolearn om hur tooauthenticate klienter tooyour Mobile Apps-serverdel finns [Lägg till autentisering tooyour app](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Så här: Använd anpassad autentisering för ditt program
Du kan implementera inloggningen systemet om du inte vill att toouse en av leverantörerna för hello autentisering/auktorisering i Apptjänst. Installera hello [Microsoft.Azure.Mobile.Server.Login] paketet tooassist med autentisering-token generering.  Ange egen kod vid verifiering av autentiseringsuppgifter. Du kan exempelvis kontrollera mot saltat och hashformaterats lösenord i en databas. I hello exemplet nedan hello `isValidAssertion()` metod (definierat någon annanstans) ansvarar för dessa kontroller.

hello anpassad autentisering exponeras genom att skapa en ApiController och exponera `register` och `login` åtgärder. hello klienten bör använda en anpassad UI toocollect hello-information från hello användare.  hello information är skickade toohello API med en standard HTTP POST anropa. När hello server verifierar hello assertion, ett token utfärdas med hello `AppServiceLoginHandler.CreateToken()` metod.  Hej ApiController **bör inte** använda hello `[MobileAppController]` attribut.

Ett exempel `login` åtgärd:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

Hello föregående exempel, är LoginResult och LoginResultUser serialiserbara objekt med synliga nödvändiga egenskaper. hello klienten förväntar sig inloggningen svar toobe returneras som JSON-objekt på hello formulär:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

Hej `AppServiceLoginHandler.CreateToken()` metoden innehåller en *målgruppen* och en *utfärdaren* parameter. Båda parametrarna anges toohello URL-Adressen för din programrot med hello HTTPS-schema. På samma sätt bör du ange *secretKey* signeringsnyckel för toobe hello värdet för ditt program. Distribuera inte hello signeringsnyckel i en klient som kan använda toomint nycklar och Personifiera användare. Du kan hämta hello signeringsnyckel medan finns i App Service genom att referera hello *webbplats\_AUTH\_signering\_NYCKELN* miljövariabeln. Om det behövs i en kontext som lokala felsökning, följer du anvisningarna för hello i hello [lokala felsökning med autentisering](#local-debug) avsnittet tooretrieve hello nyckeln och spara den som en programinställning.

hello utfärdade token kan också omfatta andra anspråk och upphör att gälla.  Åtminstone hello utfärdade token måste innehålla ett ämne (**sub**) anspråk.

Du kan använda hello standard klienten `loginAsync()` metoden av överbelastning hello autentisering vägen.  Om hello klienten anropar `client.loginAsync('custom');` toolog i rutten måste vara `/.auth/login/custom`.  Du kan ange hello väg för hello anpassad autentisering domänkontrollanten med `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> Med hjälp av hello `loginAsync()` metoden gör att hello autentiseringstoken är ansluten tooevery efterföljande anrop toohello service.
>
>

### <a name="user-info"></a>Så här: hämta autentiserad användarinformation
När en användare autentiseras av App Service kan du komma åt hello tilldelade användar-ID och annan information i serverdelskoden .NET. hello användarinformation kan användas för att göra auktoriseringsbeslut i hello backend. hello hämtar följande kod hello användar-ID som är associerade med en begäran:

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

hello SID är härledd från hello provider-specifik användar-ID och är statisk för en viss användare och inloggningen provider.  hello SID är null för ogiltig autentiseringstoken.

Apptjänst kan du begära specifika anspråk från leverantören inloggningen. Varje identitetsleverantör kan ge mer information med hjälp av identitetsleverantör SDK.  Du kan till exempel använda hello Facebook Graph API vänner information.  Du kan ange anspråk som begärs i hello providern bladet i hello Azure-portalen. Vissa anspråk kräva ytterligare konfiguration med hello identitetsprovider.

hello följande kod anrop hello **GetAppServiceIdentityAsync** tillägget metoden tooget hello inloggningsuppgifterna, bland annat hello token nödvändiga toomake förfrågningar mot hello Facebook Graph API:

    // Get hello credentials for hello logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with hello Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request hello current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with hello Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Lägga till ett using-uttryck för `System.Security.Principal` tooprovide hello **GetAppServiceIdentityAsync** metod.

### <a name="authorize"></a>Så här: begränsa åtkomst till data för behöriga användare
Under hello tidigare visades hur tooretrieve hello användar-ID för en autentiserad användare. Du kan begränsa åtkomst toodata och andra resurser baserat på det här värdet. Till exempel är att lägga till en kolumn tootables för användar-ID och filtrering hello frågeresultat av hello användar-ID ett enkelt sätt toolimit returnerade data endast tooauthorized användare. hello returnerar följande kod rader med data endast när hello SID matchar värdet i kolumnen för hello användar-ID för hello TodoItem-tabellen:

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong toohello current user.
    return Query().Where(t => t.UserId == sid);

Hej `Query()` metoden returnerar en `IQueryable` som kan ändras av LINQ toohandle filtrering.

## <a name="how-to-add-push-notifications-tooa-server-project"></a>Så här: Lägg till push-meddelanden tooa server-projekt
Lägg till push-meddelanden tooyour serverprojektet genom att utöka hello **MobileAppConfiguration** objektet och skapar en klient med Notification Hubs.

1. Högerklicka på hello serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**, söka efter `Microsoft.Azure.Mobile.Server.Notifications`, klicka på **installera**.
2. Upprepa det här steget tooinstall hello `Microsoft.Azure.NotificationHubs` paket som innehåller klientbiblioteket för hello Notification Hubs.
3. I App_Start/Startup.MobileApp.cs, och Lägg till ett anrop toohello **AddPushNotifications()** tilläggsmetoden under initieringen:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. Lägg till hello följande kod som skapar en Meddelandehubbar klient:

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Du kan nu använda hello Meddelandehubbar toosend push-meddelanden tooregistered klientenheter. Mer information finns i [Lägg till push-meddelanden tooyour app](app-service-mobile-ios-get-started-push.md). toolearn mer information om Notification Hubs finns [översikt över Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

## <a name="tags"></a>Så här: aktivera riktade push med hjälp av taggar
Notification Hubs kan du skicka riktade meddelanden toospecific registreringar med hjälp av taggar. Flera etiketter skapas automatiskt:

* hello installations-ID identifierar en specifik enhet.
* hello användar-Id utifrån hello autentiserad SID identifierar en specifik användare.

Hej installationen ID kan nås från hello **installationId** egenskapen hello **MobileServiceClient**.  hello visar följande exempel hur du använder ett installations-ID tooadd en tagg tooa installation i Notification Hubs:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Alla taggar som tillhandahålls av hello klienten under registreringen av push-meddelanden ignoreras av hello backend när du skapar hello-installationen. tooenable en klient tooadd taggar toohello installationen måste du skapa en anpassad API som lägger till taggar i hello föregående mönster.

Se [klienten tillagda push notification taggar] [ 5] i hello Apptjänst Mobilappar slutförda quickstart prov ett exempel.

## <a name="push-user"></a>Så här: skicka push-meddelanden tooan autentiserade användare
När en autentiserad användare registrerar för push-meddelanden, läggs en tagg för användar-ID automatiskt toohello registrering. Du kan skicka meddelanden tooall enheter som registrerats som personen push med hjälp av den här taggen. hello följande kod hämtar hello SID för användare som skickar begäran och skickar en mall push notification tooevery registrering av enheten för den personen:

    // Get hello current user SID and create a tag for hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for hello template with hello item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification toohello user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

När du registrerar dig för push-meddelanden från en autentiserad klient, se till att autentiseringen har slutförts innan du försöker utföra registreringen. Mer information finns i [Push toousers] [ 6] i hello Apptjänst Mobilappar slutförda quickstart prov för .NET-serverdel.

## <a name="how-to-debug-and-troubleshoot-hello-net-server-sdk"></a>Så här: felsöka och felsöka hello SDK för .NET-Server
Azure App Service tillhandahåller flera felsökning och felsökningsmetoder för ASP.NET-program:

* [Övervaka en Azure Apptjänst](../app-service-web/web-sites-monitor.md)
* [Aktivera diagnostikloggning i Azure App Service](../app-service-web/web-sites-enable-diagnostic-log.md)
* [Felsöka en Azure Apptjänst i Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Loggning
Du kan skriva tooApp diagnostikloggarna för tjänsten med hjälp av hello standard ASP.NET trace skrivning. Innan du kan skriva toohello loggar, måste du aktivera diagnostik i din mobilappsserverdel.

tooenable diagnostik- och skrivåtgärder toohello loggar:

1. Gör så hello i [hur tooenable diagnostik](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).
2. Lägg till hello följande med instruktionen i kodfilen:

        using System.Web.Http.Tracing;
3. Skapa en spårning writer toowrite från hello .NET-serverdel toohello diagnostikloggar, enligt följande:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. Publicera serverprojektet och komma åt hello Mobilapp backend tooexecute hello kodsökvägen med hello loggning.
5. Hämta och utvärdera hello loggar, enligt beskrivningen i [så här: hämta loggar](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Lokala felsökning med autentisering
Du kan köra programmet lokalt tootest ändras innan du publicerar dem. toohello moln. Tryck på för de flesta Azure Mobile Apps serverdelar *F5* medan i Visual Studio. Det finns dock några ytterligare överväganden när du använder autentisering.

Du måste ha en molnbaserad mobilapp med autentisering/auktorisering i Apptjänst konfigurerats och klienten måste ha hello molnet slutpunkten som specificeras som hello alternativt inloggnings-värd. Hello dokumentationen för din klientplattform för hello särskilda åtgärder krävs.

Kontrollera att din mobila serverdel har [Microsoft.Azure.Mobile.Server.Authentication] installerad. Sedan, i ditt program OWIN-startklass, Lägg till hello följande efter `MobileAppConfiguration` har tillämpade tooyour `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

I föregående exempel hello, bör du konfigurera hello *authAudience* och *authIssuer* programinställningar i din Web.config-filen tooeach att URL-Adressen för din programrot med hello HTTPS schemat. På samma sätt bör du ange *authSigningKey* signeringsnyckel för toobe hello värdet för ditt program.
tooobtain hello signeringsnyckeln:

1. Navigera tooyour app i hello [Azure-portalen]
2. Klicka på **verktyg**, **Kudu**, **Gå**.
3. I hello Kudu hanteringswebbplats, klickar du på **miljö**.
4. Hitta hello värdet för *webbplats\_AUTH\_signering\_NYCKELN*.

Använd hello signeringsnyckel för hello *authSigningKey* parameter i din lokala program config.  Din mobila serverdel är nu utrustad toovalidate token när du kör lokalt, vilka hello-klienten hämtar hello-token från hello molnbaserade slutpunkt.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure-portalen]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
