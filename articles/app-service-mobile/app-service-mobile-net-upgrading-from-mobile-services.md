---
title: "aaaUpgrade från Mobile Services tooAzure Apptjänst"
description: "Lär dig hur tooeasily uppgradera din Mobile Services programmet tooan Apptjänst Mobile App"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 9c0ac353-afb6-462b-ab94-d91b8247322f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2b75a1b824e8ef0d36c9053f2f19af051479f928
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-existing-net-azure-mobile-service-tooapp-service"></a>Uppgradera din befintliga Mobiltjänst med Azure .NET tooApp Service
Apptjänst Mobile är ett nytt sätt toobuild mobila program med Microsoft Azure. Det finns fler toolearn [vad är Mobilappar?].

Det här avsnittet beskrivs hur tooupgrade ett befintligt .NET-serverdel program från Azure Mobile Services tooa nya Mobile Apps i Apptjänst. Befintliga Mobile Services programmet kan fortsätta toooperate medan du utför uppgraderingen.   Om du behöver tooupgrade ett Node.js-serverdel program, se för[uppgraderar din Mobile Services för Node.js](app-service-mobile-node-backend-upgrading-from-mobile-services.md).

När en mobilserverdel är uppgraderade tooAzure Apptjänst, har den åtkomst tooall App Service-funktionerna och är debiteras enligt för[priser för Apptjänst], inte Mobile Services priser.

## <a name="migrate-vs-upgrade"></a>Migrera kontra uppgradering
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Vi rekommenderar att du [utför en migrering](app-service-mobile-migrating-from-mobile-services.md) innan du fortsätter med uppgraderingen. På så sätt kan du placera båda versionerna av programmet hello samma App Service-Plan och innebära utan extra kostnad.
>
>

### <a name="improvements-in-mobile-apps-net-server-sdk"></a>Förbättringar i Mobile Apps .NET server SDK
Uppgradera toohello nya [Mobile Apps-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) ger hello följande fördelar:

* Större flexibilitet på NuGet-beroenden. hello värdmiljön inte längre innehåller sina egna versioner av NuGet-paket, så du kan använda alternativa kompatibla versioner. Om det finns nya kritiska bugfixes eller säkerhet uppdateringar toohello Mobile Server SDK eller beroenden, måste du uppdatera tjänsten manuellt.
* Större flexibilitet hello mobila SDK. Du explicit kan styra vilka funktioner och vägar har ställts in, till exempel autentisering, tabell API: er, och hello push registrering slutpunkt. Det finns fler toolearn [hur toouse hello server-.NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).
* Stöd för andra typer av ASP.NET-projekt och flöden. Du kan nu värd MVC och Web API domänkontrollanter i samma projekt hello som projektet mobila serverdel.
* Stöd för nya App Service autentiseringsfunktioner, som gör att du toouse en vanlig Autentiseringskonfiguration mellan webb- och mobilappar.

## <a name="overview"></a>Uppgradera översikt
I många fall blir uppgraderar enkelt växla toohello nya Mobile Apps server-SDK och publicera din kod till en ny instans för Mobilapp. Det finns dock några scenarier som kräver ytterligare konfigurering, till exempel scenarier med avancerad autentisering och arbeta med schemalagda jobb. Var och en av dessa beskrivs i hello senare avsnitt.

> [!TIP]
> Det rekommenderas att du läser och förstå hello resten av det här avsnittet helt innan du påbörjar en uppgradering. Anteckna alla funktioner som du använder som kallas nedan.
>
>

hello Mobile Services-klienten SDK: er **inte** kompatibel med hello ny Mobile Apps server SDK. I ordning tooprovide tjänstens stabilitet för din app, bör du inte publicera ändringar tooa plats som betjänar publicerade klienter. I stället bör du skapa en ny mobil app som fungerar som en dubblett. Du kan publicera programmet hello samma Apptjänst planerar tooavoid medför ytterligare kostnader.

Du måste sedan två versioner av programmet hello: en som förblir hello samma och hanterar publicerade appar i hello vilda och hello andra som du kan sedan uppgradera och mål med en ny klientversion. Du kan flytta och testa din kod i din takt, men du bör se till att alla felkorrigeringar du komma tillämpade tooboth. När du anser att ett önskat antal klientprogram i hello vilda har uppdaterat toohello senaste versionen, ta bort hello ursprungliga migrerade appen om du vill ha.

hello fullständig disposition för hello uppgraderingsprocessen är följande:

1. Skapa en ny Mobile App
2. Uppdatera hello projektet toouse hello nya Server-SDK
3. Använda en ny version av klientprogrammet
4. (Valfritt) Ta bort din ursprungliga migrerade instans

## <a name="mobile-app-version"></a>Skapa en andra instans av programmet
hello är första steget när du uppgraderar toocreate hello Mobilapp resurs som ska vara värd för hello ny version av programmet. Om du redan har migrerat en befintlig mobila tjänst du vill toocreate den här versionen på hello samma plan som värd. Öppna hello [Azure-portalen] och navigera tooyour migrerat program. Anteckna hello Apptjänstplan den körs på.

Skapa sedan programmet hello instans av följande hello [.NET-serverdel skapa instruktioner](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Ange tooselect väljer du App Service-Plan eller ”värd plan” Hej när planen för migrerade programmet.

Vill du förmodligen toouse hello samma databas och Meddelandehubben som du gjorde i Mobile Services. Du kan kopiera värdena genom att öppna [Azure-portalen] och sedan navigera toohello ursprungliga programmet på **inställningar** > **programinställningar**. Under **anslutningssträngar**, kopiera `MS_NotificationHubConnectionString` och `MS_TableConnectionString`. Navigera tooyour nya uppgradera platsen och klistra in dem i att skriva över befintliga värden. Upprepa processen för alla andra programinställningar app-behov. Om du inte använder en migrerad tjänst, kan du läsa anslutningssträngar och app-inställningar från hello **konfigurera** för hello avsnitt för Mobile Services i hello [klassiska Azure-portalen].

Skapa en kopia av hello ASP.NET-projekt för ditt program och publicera den nya platsen för tooyour. Med en kopia av klientprogrammet uppdateras med hello ny URL, verifiera att allt fungerar som förväntat.

## <a name="updating-hello-server-project"></a>Uppdatera hello serverprojekt
Mobilappar erbjuder en ny [Mobile App-serverns SDK] som ger mycket hello samma funktioner som hello Mobile Services runtime. Först bör du ta bort alla referenser toohello Mobile Services paket. I hello NuGet-Pakethanteraren, söker du efter `WindowsAzure.MobileServices.Backend`. De flesta appar kommer att se flera paket här, inklusive `WindowsAzure.MobileServices.Backend.Tables` och `WindowsAzure.MobileServices.Backend.Entity`. I sådana fall börjar med hello lägsta paketet i hello resursberoende träd, t ex `Entity`, och ta bort den. När du uppmanas, ta inte bort alla beroende paket. Upprepa processen tills du har tagit bort `WindowsAzure.MobileServices.Backend` sig själv.

Nu har du ett projekt som inte längre refererar till Mobile Services SDK.

Nästa lägger du till referenser hello Mobile Apps-SDK. För den här uppgraderingen kommer de flesta utvecklare vill toodownload och installera hello `Microsoft.Azure.Mobile.Server.Quickstart` paketet, eftersom detta hämtar i hello hela krävs uppsättningen.

Det blir ett ganska stort antal kompileringsfel som uppstår till följd av skillnaderna mellan hello SDK: er, men dessa är enkelt tooaddress och behandlas i hello resten av det här avsnittet.

### <a name="base-configuration"></a>Grundläggande konfiguration
Sedan, i WebApiConfig.cs, du kan ersätta:

        // Use this class tooset configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class tooset WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

med

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

> [!NOTE]
> Om du vill toolearn mer om hello nya .NET server SDK och hur tooadd/ta bort funktioner från din app kan du se hello [hur toouse hello .NET server SDK] avsnittet.
>
>

Om din app använder hello autentiseringsfunktioner, måste du också tooregister en OWIN-mellanprogram. I det här fallet bör du flytta hello ovan konfigurationskod till en ny OWIN-startklass.

1. Lägg till NuGet-paketet för hello `Microsoft.Owin.Host.SystemWeb` om det inte redan ingår i projektet.
2. I Visual Studio högerklickar du på projektet och välj **Lägg till** -> **nytt objekt**. Välj **Web** -> **allmänna** -> **OWIN-startklass**.
3. Flytta hello ovan kod för MobileAppConfiguration från `WebApiConfig.Register()` toohello `Configuration()` metod för den nya Start-klassen.

Se till att hello `Configuration()` metoden slutar med:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Det finns ytterligare ändringar relaterade tooauthentication som beskrivs i hello fullständig autentisering nedan.

### <a name="working-with-data"></a>Arbeta med Data
I Mobile Services hello mobilappen namn hanteras som hello schemat för standardnamnet i hello Entity Framework-installationsprogrammet.

tooensure som du har hello samma schema refereras som tidigare Använd hello följande tooset hello schemat i hello DbContext för programmet:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Kontrollera att du har MS_MobileServiceName om du hello ovan. Du kan också ange en annan schemanamnet om tillämpningsprogrammet anpassat detta tidigare.

### <a name="system-properties"></a>Egenskaper för system
#### <a name="naming"></a>Namngivning
I hello Azure Mobile Services-servern SDK Systemegenskaper alltid innehålla ett dubbelt understreck (`__`)-prefixet för hello egenskaper:

* __createdAt
* __updatedAt
* __deleted
* __version

hello Mobile Services-klienten SDK: er har särskilda logik för parsning av Systemegenskaper i det här formatet.

I Azure Mobile Apps har Systemegenskaper inte längre en särskild formatera och har hello följande namn:

* CreatedAt
* updatedAt
* ta bort
* Version

hello Mobile Apps-SDK: er används hello nya system egenskaper klientnamn, så att inga ändringar krävs tooclient kod. Om du är direkt gör REST anropar tooyour-tjänsten och du bör ändra dina frågor i enlighet med detta.

#### <a name="local-store"></a>Lokalt Arkiv
hello ändringar toohello namnen på Systemegenskaper innebär att en lokal databas offlinesynkronisering för Mobile Services inte är kompatibel med Mobile Apps. Om möjligt bör du undvika att uppgradera klientappar från Mobile Services tooMobile appar förrän efter väntande ändringar har skickats toohello server. Sedan ska hello uppgraderade app använda ett nytt databasnamn.

Om ett klientprogram uppgraderas från Mobile Services tooMobile appar när det finns väntande ändringar offline i hello åtgärden kö, måste hello-databas vara uppdaterade toouse hello nya kolumnnamn. På iOS, kan detta uppnås med hjälp av lightweight migreringar toochange hello kolumnnamn. På Android- och hello .NET hanterad klient, bör du skriva anpassade SQL toorename hello kolumner för din datatabeller för objektet.

På iOS, bör du ändra schemat grundläggande information för dina data entiteter toomatch hello följande. Observera att hello egenskaper `createdAt`, `updatedAt` och `version` har inte längre ett `ms_` prefix:

| Attribut | Typ | Obs! |
| --- | --- | --- |
| id |Strängen som markerats krävs |primärnyckeln i fjärranslutna store |
| CreatedAt |Date |(valfritt) maps toocreatedAt Systemegenskapen |
| updatedAt |Date |(valfritt) maps tooupdatedAt Systemegenskapen |
| Version |Sträng |(valfritt) använda toodetect konflikter, maps tooversion |

#### <a name="querying-system-properties"></a>Frågar Systemegenskaper
I Azure Mobile Services Systemegenskaper skickas inte som standard, men endast när de begär med hello frågesträngen `__systemProperties`. Däremot i Azure Mobile Apps system egenskaper är **alltid valt** eftersom de ingår i objektmodellen för hello server SDK.

Den här ändringen påverkar huvudsakligen anpassade implementeringar av domän chefer, till exempel tillägg av `MappedEntityDomainManager`. I Mobile Services, om en klient begär aldrig alla Systemegenskaper är möjliga toouse en `MappedEntityDomainManager` som faktiskt mappar inte alla egenskaper. Men i Azure Mobile Apps genereras egenskaperna omappade ett fel i GET-frågor.

Hej enklaste sättet tooresolve hello problemet är toomodify din DTOs så att de ärver från `ITableData` i stället för `EntityData`. Lägg sedan till hello `[NotMapped]` attributet toohello fält som ska uteslutas.

Till exempel hello följande definierar `TodoItem` utan system-egenskaper:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Obs: om det uppstår fel `NotMapped`, lägga till en referenssammansättning toohello `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS
Mobile Services med vissa stöd för CORS genom att omsluta hello ASP.NET CORS-lösning. Radbrytning lagret har tagits bort toogive hello utvecklare mer kontroll så att du kan utnyttja direkt [stöd för ASP.NET-CORS](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

hello huvudsakliga problemområden om du använder CORS är att hello `eTag` och `Location` huvuden måste tillåtas för hello klient-SDK: er toowork korrekt.

### <a name="push-notifications"></a>Push-meddelanden
Huvudobjekt Hej som kan vara saknas från hello-serverns SDK är push, hello PushRegistrationHandler klass. Registreringar hanteras lite annorlunda i Mobile Apps och tagless registreringar är aktiverade som standard. Hantera taggar kan åstadkommas med hjälp av anpassade API: er. Se hello [registrering för taggar](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) instruktioner för mer information.

### <a name="scheduled-jobs"></a>Schemalagda jobb
Schemalagda jobb är inte inbyggda i Mobilappar, så måste alla befintliga jobb som du har i .NET-serverdel toobe uppgraderas individuellt. Ett alternativ är en schemalagd toocreate [Web jobbet] på hello Mobilapp kod platsen. Du kan också konfigurera en domänkontrollant som innehåller jobbkoden och konfigurera hello [Azure Scheduler] toohit denna slutpunkt på hello förväntades schema.

### <a name="miscellaneous-changes"></a>Funktionsförändringar
Alla ApiControllers som används av en mobil klient nu måste ha hello `[MobileAppController]` attribut. Detta är inte längre ingår som standard så att andra ApiControllers toogo påverkas inte av hello mobila-formaterare.

Hej `ApiServices` objektet är inte längre tillhör hello SDK. Du kan använda hello följande tooaccess Mobile App-inställningar:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

På samma sätt, utförs nu loggning med hello standard ASP.NET trace skrivning:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

## <a name="authentication"></a>Överväganden för autentisering
hello har autentisering i Mobile Services nu flyttats till hello autentisering/auktorisering i Apptjänst-funktionen. Du kan lära dig om hur du aktiverar detta för webbplatsen genom att läsa hello [Lägg till autentisering tooyour mobilappen](app-service-mobile-ios-get-started-users.md) avsnittet.

För vissa leverantörer, till exempel AAD, Facebook och Google, bör du kunna tooleverage hello befintlig registrering från tillämpningsprogrammet kopia. Du bara behöver toonavigate toohello identitetsleverantörens portal och lägga till en ny omdirigerings-URL: en toohello registrering. Konfigurera autentisering/auktorisering i Apptjänst med hello klient-ID och hemlighet.

### <a name="controller-action-authorization"></a>Domänkontrollanten åtgärd auktorisering
Alla instanser av hello `[AuthorizeLevel(AuthorizationLevel.User)]` attributet måste nu vara ändrade toouse hello standard ASP.NET `[Authorize]` attribut. Dessutom är domänkontrollanter nu anonym som standard, som i andra ASP.NET-program.
Om du använder en av hello märke andra AuthorizeLevel alternativ, till exempel Admin eller ett program, Lägg till att de är borta. Du kan i stället konfigurera AuthorizationFilters för delade hemligheter eller konfigurera ett AAD tjänstens huvudnamn tooenable tjänst-till-tjänst-anrop på ett säkert sätt.

### <a name="getting-additional-user-information"></a>Hämtar användarinformation om ytterligare
Du kan få ytterligare information, inklusive åtkomsttoken via hello `GetAppServiceIdentityAsync()` metoden:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Dessutom, om ditt program tar beroenden på användar-ID, till exempel lagra dem i en databas, är det viktigt toonote som hello användar-ID mellan Mobile Services och Apptjänst Mobilappar är olika. Du kan fortfarande få hello Mobile Services användar-ID, även om. Alla hello ProviderCredentials underklasser har en användar-ID-egenskap. Så fortsätter från hello exempel innan:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Om din app tar några beroenden på användar-ID, är det viktigt att du utnyttja hello samma registrering med en identitetsprovider om möjligt. Användar-ID är vanligtvis begränsade toohello appregistrering som användes, så introducerar en ny registrering kan skapa problem med matchande användare tootheir data.

### <a name="custom-authentication"></a>Anpassad autentisering
Om din app använder en lösning för anpassad autentisering, vill du att den uppgraderade hello-platsen har åtkomst toohello system toomake. Följ hello nya instruktioner för anpassad autentisering i hello [översikt över SDK för .NET server] toointegrate din lösning. Observera att hello anpassad autentiseringskomponenter är fortfarande under förhandsgranskning.

## <a name="updating-clients"></a>Uppdatera klienter
När du har en operativa mobilappsserverdel kan arbeta du med en ny version av ditt klientprogram som använder den. Mobile Apps innehåller också en ny version av hello klienten SDK: er och liknande toohello serveruppgraderingen ovan, behöver du tooremove alla referenser toohello Mobile Services SDK innan du installerar hello Mobile Apps versioner.

En av hello huvudsakliga ändringar mellan hello versioner är att hello konstruktorer inte längre behöver en tangent. Du nu skickar hello URL för din Mobilapp. Till exempel på hello .NET-klienter, hello `MobileServiceClient` konstruktorn är nu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of hello Mobile App
        );

Du kan läsa om hur du installerar hello nya SDK: er och använder hello ny struktur via hello länkarna nedan:

* [iOS version 3.0.0 eller senare](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) version 2.0.0 eller senare](app-service-mobile-dotnet-how-to-use-client-library.md)

Om ditt program gör användning av push-meddelanden, anteckna hello registreringen instruktioner för varje plattform, har eftersom det ändringar det också.

När du har hello nya klientversionen redo prova mot projektet uppgraderade servern. Du kan släppa en ny version av programmet-toocustomers efter verifierar att den fungerar. Slutligen, när dina kunder har fått en chans tooreceive dessa uppdateringar, du kan ta bort hello Mobile Services version av appen. Nu är du har uppgraderat tooan App Service-Mobilapp med hjälp av hello senaste Mobile Apps server-SDK.

<!-- URLs. -->

[Azure-portalen]: https://portal.azure.com/
[klassiska Azure-portalen]: https://manage.windowsazure.com/
[vad är Mobilappar?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App-serverns SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication tooyour mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web jobbet]: ../app-service-web/websites-webjobs-resources.md
[hur toouse hello .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services tooan App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service tooApp Service]: app-service-mobile-migrating-from-mobile-services.md
[priser för Apptjänst]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[översikt över SDK för .NET server]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
