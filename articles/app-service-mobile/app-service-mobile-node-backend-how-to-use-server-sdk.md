---
title: "aaaHow toowork med serverdelen för hello Node.js SDK för Mobile Apps | Microsoft Docs"
description: "Lär dig hur toowork med hello serverdelen för Node.js SDK för Azure Apptjänst Mobilappar."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 2b1ea5fda6f6ca422b92fe29ff8d16bf035018d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-mobile-apps-nodejs-sdk"></a>Hur toouse hello Azure Mobile Apps Node.js SDK
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Den här artikeln innehåller detaljerad information och exempel som visar hur toowork med Node.js-serverdel i Azure Apptjänst Mobilappar.

## <a name="Introduction"></a>Introduktion
Azure Apptjänst Mobilappar tillhandahåller hello kapaciteten tooadd en mobile optimerad dataåtkomst Web API tooa webbprogram.  hello Azure App Service Mobile Apps-SDK har angetts för ASP.NET och Node.js webbprogram.  hello SDK innehåller hello följande åtgärder:

* Tabellåtgärder (Läs-, Insert-, Update-, Delete) för dataåtkomst
* Anpassade API-åtgärder

Både operations erbjuda autentisering över alla identitetsleverantörer som tillåts av Azure App Service, inklusive sociala identitetsleverantörer, till exempel Facebook, Twitter, Google och Microsoft samt Azure Active Directory för enterprise-identitet.

Du kan hitta exempel för varje användningsfall hello [katalogen samples på GitHub].

## <a name="supported-platforms"></a>Plattformar som stöds
hello Azure Mobile Apps nod SDK stöder hello aktuella LTS viktiga för nod och senare.  Från och med skrivning är hello senaste LTS versionen noden v4.5.0.  Andra versioner av noden fungerar men stöds inte.

hello Azure Mobile Apps nod SDK stöder två databasdrivrutiner för - hello nod mssql drivrutinen har stöd för SQL Azure och lokala SQL Server-instanser.  Hej sqlite3 drivrutinen stöder SQLite databaser på en enda instans.

### <a name="howto-cmdline-basicapp"></a>Så här: skapa ett grundläggande Node.js-serverdel med hello kommandoraden
Varje Azure App Service Mobile App Node.js-serverdel startar som ett ExpressJS program.  ExpressJS är hello populäraste web service ramverk för Node.js.  Du kan skapa en grundläggande [Express] program på följande sätt:

1. Skapa en katalog för ditt projekt i ett kommando eller PowerShell-fönstret.

        mkdir basicapp
2. Kör npm init tooinitialize hello paketet struktur.

        cd basicapp
        npm init

    Hej npm init kommandot frågar en uppsättning frågor tooinitialize hello projektet.  Se hello exempel på utdata:

    ![Hej npm init-utdata][0]
3. Installera hello express och azure mobilappar bibliotek från hello npm-databasen.

        npm install --save express azure-mobile-apps
4. Skapa en app.js tooimplement hello grundläggande mobila filserver.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Det här programmet skapar en optimerad för-WebAPI med en enda slutpunkt (`/tables/TodoItem`) som ger obehörig åtkomst tooan underliggande SQL-datalagret med en dynamisk schema.  Det är lämpligt för följande snabbstarter för klient-biblioteket:

* [Snabbstart för Android-klienten]
* [Snabbstart för Apache Cordova-klient]
* [iOS klienten Snabbstart]
* [Snabbstart för Windows Store-klienten]
* [Snabbstart för Xamarin.iOS-klient]
* [Snabbstart för Xamarin.Android-klient]
* [Snabbstart för Xamarin.Forms-klient]

Du hittar hello koden för grundläggande programmet i hello [basicapp exempel på GitHub].

### <a name="howto-vs2015-basicapp"></a>Så här: skapa en nod serverdel med Visual Studio 2015
Visual Studio 2015 kräver ett tillägg toodevelop Node.js-program inom hello IDE.  toostart, installera hello [1.1 för Node.js-verktyg för Visual Studio].  När hello Node.js Tools för Visual Studio har installerats kan du skapa ett Express 4.x program:

1. Öppna hello **nytt projekt** dialogrutan (från **filen** > **ny** > **projekt...** ).
2. Expandera **mallar** > **JavaScript** > **Node.js**.
3. Välj hello **grundläggande Azure Node.js Express 4 program**.
4. Fyll i hello projektnamn.  Klicka på *OK*.

    ![Nytt projekt i Visual Studio 2015][1]
5. Högerklicka på hello **npm** och välj **installera nya npm-paket...** .
6. Du kan behöva toorefresh hello npm katalogen om hur du skapar din första Node.js-program.  Klicka på **uppdatera** om det behövs.
7. Ange *azure mobilappar* i hello sökrutan.  Klicka på hello **azure mobile apps 2.0.0** paketet och klicka sedan på **installera paket**.

    ![Installera nya npm-paket][2]
8. Klicka på **Stäng**.
9. Öppna hello *app.js* tooadd stöd för hello Azure Mobile Apps-SDK.  Längst ned rad 6 at hello hello bibliotek kräver instruktioner, Lägg till följande kod hello:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Lägg till hello följande kod på cirka rad 27 efter hello andra app.use instruktioner:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Spara hello-filen.
10. Kör hello programmet lokalt (hello API levereras på http://localhost: 3000) eller publicera tooAzure.

### <a name="create-node-backend-portal"></a>Så här: skapa en Node.js-serverdel med hello Azure-portalen
Du kan skapa en Mobilapp backend rätt i hello [Azure-portalen]. Du kan antingen följa hello följande steg eller skapa en klient och server tillsammans med följande hello [skapar en mobil app](app-service-mobile-ios-get-started.md) kursen. hello självstudier innehåller en förenklad version av dessa anvisningar och är bäst för bevis på koncept projekt.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Tillbaka i hello *Kom igång* bladet under **skapa en tabell API**, Välj **Node.js** som din **språk för serverdel**.
Kryssrutan för hello ”**bekräftar jag att skrivs alla plats innehållet.**”, klicka på **skapa TodoItem-tabell**.

### <a name="download-quickstart"></a>Så här: hämta hello Node.js-serverdel kod snabbstartsprojekt med Git
När du skapar en Node.js mobilappsserverdel genom att använda hello portal **Snabbstart** bladet ett Node.js-projekt har skapats för dig och distribuerade tooyour plats. Du kan lägga till tabeller och API: er och redigera kodfiler för hello Node.js-serverdel i hello-portalen. Du kan också använda olika verktyg toodownload hello backend projekt för distribution så att du kan lägga till eller ändra tabeller och API: er och sedan publicera hello-projekt. Mer information finns i [Azure App Service Deployment Guide]. hello används följande procedur en Git-lagringsplatsen toodownload hello quickstart Projektkod.

1. Installera Git om du inte redan gjort det.. hello steg krävs tooinstall Git variera mellan olika operativsystem. Se [installerar Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) för operativsystemspecifika distributioner och installationer.
2. Gör så hello i [aktivera hello Apptjänst app databasen](../app-service-web/app-service-deploy-local-git.md#Step3) tooenable hello Git-lagringsplats för backend-platsen, och ett meddelande om hello distribution användarnamn och lösenord.
3. I hello-bladet för din mobilappsserverdel anteckna hello **URL för Git-klon** inställningen.
4. Köra hello `git clone` kommandot med hello Git klon-URL att ange lösenordet när det behövs, som i följande exempel:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Bläddra toolocal directory, som i föregående exempel hello /todolist, och Lägg märke till att projektfiler har hämtats. Leta upp hello `todoitem.json` filen i hello `/tables` directory.  Den här filen definierar behörigheter i tabellen.  Också hitta hello `todoitem.js` filen i hello samma katalog som anger att skript CRUD-åtgärden för hello tabellen.
6. När du har gjort ändringar tooproject filer, kör du följande kommandon tooadd, genomför och sedan överföra ändringar toohello plats hello:

        $ git commit -m "updated hello table script"
        $ git push origin master

    När du lägger till nya filer toohello projekt måste du först tooexecute hello `git add .` kommando.

hello platsen publiceras varje gång en ny uppsättning incheckningar pushas toohello plats.

### <a name="howto-publish-to-azure"></a>Så här: publicera din Node.js-serverdel tooAzure
Microsoft Azure tillhandahåller många metoder för att publicera din Azure App Service Mobile Apps Node.js-serverdel för att hello Azure-tjänsten.  Dessa inkluderar använder distributionsverktyg integrerade i Visual Studio, kommandoradsverktyg och kontinuerlig distributionsalternativ baserat på källkontroll.  Mer information om det här avsnittet finns det [Azure App Service Deployment Guide].

Azure Apptjänst har råd för Node.js-program som du bör läsa innan du distribuerar:

* Hur för[ange hello nod Version]
* Hur för[använda noden moduler]

### <a name="howto-enable-homepage"></a>Så här: aktivera en hemsida för ditt program
Hej ExpressJS framework kan du toocombine två aspekter och många program är en kombination av webb- och mobilappar.  Ibland kan dock gärna tooonly implementera ett mobila gränssnitt.  Det är användbart tooprovide en landning sidan tooensure hello app-tjänsten är igång.  Du kan ange en egen hemsida eller aktivera en tillfällig startsida.  tooenable en tillfällig startsida, Använd följande tooinstantiate Azure Mobile Apps hello:

    var mobile = azureMobileApps({ homePage: true });

Om du vill bara det här alternativet är tillgängligt när du utvecklar lokalt kan du lägga till den här inställningen tooyour `azureMobile.js` fil.

## <a name="TableOperations"></a>Åtgärder för tabeller
hello azure mobilappar Node.js Server SDK innehåller mekanismer tooexpose data tabeller som lagras i Azure SQL Database som en WebAPI.  Fem tillhandahålls.

| Åtgärd | Beskrivning |
| --- | --- |
| GET-/tables/*tablename* |Hämta alla poster i hello tabell |
| GET-/tables/*tablename*/:id |Hämta en viss post i tabellen hello |
| POST /tables/*tablename* |Skapa en post i tabellen hello |
| KORRIGERING /tables/*tablename*/:id |Uppdatera en post i tabellen hello |
| Ta bort /tables/*tablename*/:id |Ta bort en post i tabellen hello |

Har stöd för den här WebAPI [OData] och utökar hello tabellen schema toosupport [datasynkronisering offline].

### <a name="howto-dynamicschema"></a>Så här: definiera tabeller med en dynamisk schema
Innan en tabell kan användas, måste ha definierats.  Tabeller kan definieras med en statisk schema (där hello developer definierar hello kolumner inom hello schemat) eller dynamiskt (där hello SDK styr hello schema baserat på inkommande begäranden). Hello utvecklare kan dessutom styra vissa aspekter av hello WebAPI genom att lägga till Javascript-kod toohello definition.

Som bästa praxis bör du definiera varje tabell i en Javascript-fil i hello tabeller och sedan använda tables.import() metoden tooimport hello tabeller.  Utöka hello basic-app skulle hello app.js filen ställas in:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define hello database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add hello mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definiera hello tabellen. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for hello table goes here

    module.exports = table;

Dynamisk schemat använder som standard.  tooturn av dynamisk schemat anger globalt, hello Appinställningen **MS_DynamicSchema** toofalse inom hello Azure-portalen.

Du hittar ett komplett exempel i hello [todo prov på GitHub].

### <a name="howto-staticschema"></a>Så här: definiera tabeller med en statisk schema
Du kan uttryckligen definiera hello kolumner tooexpose via hello WebAPI.  hello azure mobilappar Node.js SDK lägger automatiskt till alla andra kolumner som krävs för offlinedata toohello Synkroniseringslista som du anger.  Exempelvis klientprogram Snabbstart kräver en tabell med två kolumner: text (en sträng) och slutföra (ett booleskt värde).  
hello tabellen kan definieras i hello tabell definition JavaScript-fil (finns i hello tabeller directory) enligt följande:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Om du definierar tabeller statiskt, måste du också anropa hello tables.initialize() metoden toocreate hello-databasschemat vid start.  Hej tables.initialize() metoden returnerar en [Promise] så att hello webbtjänsten inte hantera begäranden innan hello databasen initieras.

### <a name="howto-sqlexpress-setup"></a>Så här: använda SQL Express som en utveckling datalager på den lokala datorn
hello Azure Mobile Apps hello AzureMobile appar nod SDK innehåller tre alternativ för betjänar data out of box hello: SDK innehåller tre alternativ för betjänar data out of box hello:

* Använd hello **minne** drivrutinsarkiv tooprovide en icke-beständig exempel
* Använd hello **mssql** drivrutinen tooprovide SQL Express datalagret för utveckling
* Använd hello **mssql** drivrutinen tooprovide en Azure SQL Database-datalagret för produktion

hello Azure Mobile Apps Node.js SDK använder hello [mssql Node.js paketet] tooestablish och använder en anslutning tooboth SQL Express och SQL-databas.  Det här paketet kräver att du aktiverar TCP-anslutningar på din SQL Express-instans.

> [!TIP]
> hello minne drivrutinen ger inte en fullständig uppsättning funktioner för testning.  Om du vill tootest serverdelen lokalt kan vi rekommenderar hello användning av ett SQL Express datalager och hello mssql drivrutinen.
>
>

1. Hämta och installera [Microsoft SQL Server 2014 Express].  Se till att du installerar SQL Server 2014 Express hello med verktyg edition.  Om du uttryckligen kräver stöd för 64-bitars förbrukar hello 32-bitarsversionen mindre minne när du kör.
2. Kör hello Konfigurationshanteraren för SQL Server 2014.

   1. Expandera hello **SQL Server-nätverkskonfigurationen** nod i hello vänstra träd-menyn.
   2. Klicka på **protokoll för SQLEXPRESS**.
   3. Högerklicka på **TCP/IP** och välj **aktivera**.  Klicka på **OK** i hello popup-fönstret.
   4. Högerklicka på **TCP/IP** och välj **egenskaper**.
   5. Klicka på hello **IP-adresser** fliken.
   6. Hitta hello **IPAll** nod.  I hello **TCP-Port** anger **1433**.

          ![Configure SQL Express for TCP/IP][3]
   7. Klicka på **OK**.  Klicka på **OK** i hello popup-fönstret.
   8. Klicka på **SQL Server Services** i hello vänstra träd-menyn.
   9. Högerklicka på **SQL Server (SQLEXPRESS)** och välj **starta om**
   10. Stäng hello Konfigurationshanteraren för SQL Server 2014.
3. Kör hello SQL Server 2014 Management Studio och Anslut tooyour lokala SQL Express-instansen

   1. Högerklicka på din instans i hello Object Explorer och välj **egenskaper**
   2. Välj hello **säkerhet** sidan.
   3. Se till att hello **SQL Server och Windows-autentisering läge** har valts
   4. Klicka på **OK**

          ![Configure SQL Express Authentication][4]
   5. Expandera **säkerhet** > **inloggningar** i hello Object Explorer
   6. Högerklicka på **inloggningar** och välj **ny inloggning...**
   7. Ange ett inloggningsnamn.  Välj **SQL Server-autentisering**.  Ange ett lösenord och sedan hello samma lösenord i **Bekräfta lösenord**.  hello lösenord måste uppfylla krav på komplexitet Windows.
   8. Klicka på **OK**

          ![Add a new user tooSQL Express][5]
   9. Högerklicka på en ny inloggning och välj **egenskaper**
   10. Välj hello **serverroller** sidan
   11. Kontrollera hello rutan nästa toohello **dbcreator** serverroll
   12. Klicka på **OK**
   13. Stäng hello SQL Server 2015 Management Studio

Se till att du poster hello användarnamn och lösenord som du har valt.  Du kanske måste tooassign ytterligare serverroller eller behörigheter beroende på dina specifika krav.

Hej Node.js-program läser hello **SQLCONNSTR_MS_TableConnectionString** miljövariabeln för hello anslutningssträngen för den här databasen.  Du kan ange den här variabeln i din miljö.  Du kan till exempel använda PowerShell tooset den här miljövariabeln:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Komma åt hello databasen via en TCP/IP-anslutning och ange ett användarnamn och lösenord för hello-anslutning.

### <a name="howto-config-localdev"></a>Så här: konfigurera ditt projekt för lokal utveckling
Azure Mobile Apps läser en JavaScript-fil som heter *azureMobile.js* från hello lokala filsystem.  Inte använda den här filen tooconfigure hello Azure Mobile Apps-SDK i produktion – använda App-inställningar i hello [Azure-portalen] i stället.  Hej *azureMobile.js* filen ska exportera ett konfigurationsobjekt.  hello de vanligaste inställningarna är:

* Databasinställningar
* Inställningar för diagnostikloggning
* Alternativa CORS-inställningarna

Ett exempel *azureMobile.js* implementera hello föregående databasen inställningarna för följande fil:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Vi rekommenderar att du lägger till *azureMobile.js* tooyour *.gitignore* fil (eller andra källkodskontroll ignorera filen) tooprevent lösenord lagras i hello molnet.  Konfigurera inställningar för produktion alltid i App-inställningar i hello [Azure-portalen].

### <a name="howto-appsettings"></a>Så här: Konfigurera Appinställningar för din mobila App
De flesta inställningar i hello *azureMobile.js* filen har en motsvarande Appinställning i hello [Azure-portalen].  Använd följande lista tooconfigure hello din app i App-inställningar:

| Appinställningen | *azureMobile.js* inställning | Beskrivning | Giltiga värden |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |namn |hello namnet på hello app |Sträng |
| **MS_MobileLoggingLevel** |Logging.level |Lägsta loggningsnivån för meddelanden toolog |fel, varning, information, verbose,-debug, Löjliga |
| **MS_DebugMode** |Felsöka |Aktivera eller inaktivera felsökningsläge |SANT, FALSKT |
| **MS_TableSchema** |data.schema |Schemat för standardnamnet för SQL-tabeller |String (standard: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Aktivera eller inaktivera felsökningsläge |SANT, FALSKT |
| **MS_DisableVersionHeader** |version (set tooundefined) |Inaktiverar hello X-ZUMO-Server-Version-huvud |SANT, FALSKT |
| **MS_SkipVersionCheck** |skipversioncheck |Inaktiverar hello API-version klientkontroll |SANT, FALSKT |

tooset en Appinställning:

1. Logga in toohello [Azure-portalen].
2. Välj **alla resurser** eller **Apptjänster** Klicka hello namnet på din Mobilapp.
3. hello inställningsbladet öppnas som standard. Om den inte, klickar du på **inställningar**.
4. Klicka på **programinställningar** allmänna hello-menyn.
5. Rulla toohello App-inställningar.
6. Om din app inställningen redan finns på hello värdet av hello app tooedit hello värdet.
7. Om din appinställning inte finns, ange hello Appinställningen i hello nyckel och hello värde i rutan för hello-värde.
8. När du är klar klickar du på **spara**.

Ändrar appinställningar för de flesta måste tjänsten startas om.

### <a name="howto-use-sqlazure"></a>Så här: använda SQL-databas som datalager produktion
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Med Azure SQL Database som ett datalager är identisk för alla typer i Azure App Service-programmet. Om du redan inte har gjort det, följer du dessa steg toocreate en mobilappsserverdel.

1. Logga in toohello [Azure-portalen].
2. I hello upp till vänster i hello-fönstret klickar du på hello **+ ny** knappen > **webb + mobilt** > **Mobilapp**, ange ett namn för din mobilappsserverdel.
3. I hello **resursgruppen** ange hello samma namn som din app.
4. hello standard programtjänstplanen har valts.  Om du vill toochange App Service-plan kan du göra det genom att klicka på hello Apptjänstplan > **+ skapa nya**.  Ange ett namn på hello nya App Service-plan och välj en lämplig plats.  Klicka på hello prisnivå och välj en lämplig prisnivån för hello-tjänsten. Välj **visa alla** tooview priser fler alternativ, exempelvis **lediga** och **delade**.  När du har valt prisnivå, klickar du på hello **Välj** knappen.  Tillbaka i hello **programtjänstplanen** bladet, klickar du på **OK**.
5. Klicka på **Skapa**. Etablering av en mobilappsserverdel kan ta några minuter.  När hello mobilappsserverdel etableras hello portalen öppnar hello **inställningar** bladet för hello mobilappsserverdel.

När hello mobilappsserverdel har skapats kan du välja tooeither Anslut en befintlig SQL-databas tooyour mobilappsserverdel eller skapa en ny SQL-databas.  I det här avsnittet skapar vi en SQL-databas.

> [!NOTE]
> Om du redan har en databas i hello samma plats som hello mobilappsserverdel kan du istället välja **Använd en befintlig databas** och välj sedan den databasen. Det rekommenderas inte hello användning av en databas på en annan plats på grund av ökad latens.
>
>

1. I hello ny mobilappsserverdel, klickar du på **inställningar** > **Mobilapp** > **Data** > **+ Lägg till**.
2. I hello **Lägg till dataanslutning** bladet, klickar du på **SQL Database - konfigurera nödvändiga inställningar** > **skapa en ny databas**.  Ange hello namn hello ny databas i hello **namn** fältet.
3. Klicka på **Server**.  I hello **ny server** bladet, ange ett unikt servernamn i hello **servernamn** fältet och ange ett lämpligt **inloggning för serveradministratör** och **lösenord**.  Se till att **Tillåt azure-tjänster tooaccess server** är markerad.  Klicka på **OK**.

    ![Skapa en Azure SQL Database][6]
4. På hello **ny databas** bladet, klickar du på **OK**.
5. Tillbaka på hello **Lägg till dataanslutning** bladet väljer **anslutningssträngen**, ange hello inloggningsnamn och lösenord som du angav när du skapar hello-databasen.  Ange hello inloggningsuppgifterna för databasen om du använder en befintlig databas.  När du har angett, klickar du på **OK**.
6. Tillbaka på hello **Lägg till dataanslutning** bladet Klicka på **OK** toocreate hello-databasen.

<!--- END OF ALTERNATE INCLUDE -->

Hello databasen kan ta några minuter.  Använd hello **meddelanden** område toomonitor hello fortskrider hello-distribution.  Passera inte förrän hello databas har distribuerats korrekt.  När har distribuerats, skapas en anslutningssträng för hello SQL Database-instans i din mobila serverdel App-inställningar.  Du kan se den här appen inställningen i hello **inställningar** > **programinställningar** > **anslutningssträngar**.

### <a name="howto-tables-auth"></a>Så här: kräver autentisering för åtkomst tootables
Om du vill toouse App Service-autentisering med hello tabeller slutpunkt måste du konfigurera App Service-autentisering i hello [Azure-portalen] första.  För mer information om hur du konfigurerar autentisering i Azure App Service, granska hello konfigurationsguiden för hello identitetsleverantör avser toouse:

* [Hur tooconfigure Azure Active Directory-autentisering]
* [Hur tooconfigure Facebook-autentisering]
* [Hur tooconfigure Google-autentisering]
* [Hur tooconfigure Microsoft Authentication]
* [Hur tooconfigure Twitter-autentisering]

Varje tabell har en åtkomst-egenskap som kan använda toocontrol access toohello-tabell.  hello följande exempel visas en statiskt definierade tabell med autentisering krävs.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

hello åtkomst egenskapen kan ha ett av tre värden

* *anonym* anger hello klientprogrammet tillåts tooread data utan autentisering
* *autentiserad* anger att hello-klientprogrammet måste skicka en giltig autentiserings-token med hello begäran
* *inaktiverad* anger att den här tabellen är för närvarande inaktiverat

Om hello åtkomst egenskapen är odefinierad tillåts obehörig åtkomst.

### <a name="howto-tables-getidentity"></a>Så här: använda anspråk för autentisering med tabeller
Du kan ställa in olika anspråk som begärs när autentisering har ställts in.  Dessa anspråk inte är normalt tillgängliga via hello `context.user` objekt.  Men de kan hämtas med hjälp av hello `context.user.getIdentity()` metod.  Hej `getIdentity()` metoden returnerar en Promise som löser tooan objekt.  hello-objekt aktiveras av autentiseringsmetod (facebook, google, twitter, microsoftaccount eller aad).

Om du konfigurerar Account autentisering och begäran hello e-postadresser anspråk du lägga till hello e-post toohello med hello efter tabellen domänkontrollant:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit hello context query toothose records with hello authenticated user email address
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds hello email address from hello claims toohello context item - used for
    * insert operations
    * @param {Context} context hello operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when hello client does a request
    // READ - only return records belonging toohello authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite hello userId based on hello authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong toohello authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong toohello authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

toosee vilka anspråk är tillgängliga, använda en web webbläsare tooview hello `/.auth/me` slutpunkten för din webbplats.

### <a name="howto-tables-disabled"></a>Så här: inaktivera åtkomståtgärder toospecific tabell
I tillägg tooappearing för hello tabellen vara hello åtkomst egenskapen används toocontrol enskilda åtgärder.  Det finns fyra åtgärder:

* *läsa* är hello RESTful GET-åtgärd på hello tabell
* *Infoga* hello RESTful efter åtgärd för hello tabellen
* *Uppdatera* hello RESTful korrigering åtgärd på hello tabell
* *ta bort* hello RESTful ta bort åtgärd för hello tabellen

Till exempel du tooprovide en skrivskyddad oautentiserade tabell:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Så här: justera hello-fråga som ska användas med åtgärder för tabeller
Ett vanligt krav för tabellåtgärder är tooprovide en begränsad vy av hello data.  Du kan till exempel ange en tabell som är märkta med hello autentiserat användar-ID så att du kan bara läsa eller uppdatera dina egna poster.  hello följande tabelldefinitionen innehåller den här funktionen:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for hello table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for hello authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite hello userId with hello authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Åtgärder som normalt köra en fråga har en fråga-egenskap som du kan justera med where satsen. hello fråga egenskapen är en [QueryJS] objekt som används tooconvert toosomething en OData-frågan som hello data backend kan bearbeta.  En mappning kan användas för enkla likheten fall (hello före en). Du kan också lägga till specifika SQL-satser:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Så här: Konfigurera mjuk borttagning för en tabell
Mjuk borttagning faktiskt bort inte poster.  I stället märks de som borttagna i hello-databas genom att ange hello bort kolumnen tootrue.  hello Azure Mobile Apps-SDK tar automatiskt bort ej permanent borttagna poster från resultat om hello Mobile klient-SDK använder IncludeDeleted().  en tabell för mjuk bort tooconfigure ange hello `softDelete` egenskap i definitionsfilen för hello tabell:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

Du bör fastställa en mekanism för att rensa poster – antingen från ett klientprogram, via ett Webbjobb Azure-funktion eller via en anpassad API.

### <a name="howto-tables-seeding"></a>Så här: startvärde för databasen med data
När du skapar ett nytt program kan du tooseed en tabell med data.  Detta kan göras i hello tabell definition JavaScript-fil på följande sätt:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define hello columns within hello table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication tooaccess hello table
    table.access = 'authenticated';

    module.exports = table;

Dirigering av data utförs endast när hello tabell skapas av hello Azure Mobile Apps-SDK.  Om hello tabellen redan finns i hello databas injekteras inga data i hello tabell.  Om dynamiska schemat är aktiverat härleda schemat från hello dirigeras data.

Vi rekommenderar att du explicit anropa hello `tables.initialize()` metoden toocreate hello tabellen när hello-tjänsten startar.

### <a name="Swagger"></a>Så här: aktivera stöd för Swagger
Azure Apptjänst Mobilappar medföljer inbyggda [Swagger] stöd.  tooenable Swagger-support, kan du först installera hello swagger-användargränssnitt som ett beroende:

    npm install --save swagger-ui

När du har installerat kan du aktivera stöd för Swagger i hello Azure Mobile Apps-konstruktorn:

    var mobile = azureMobileApps({ swagger: true });

Du förmodligen bara vill tooenable Swagger-stöd i development utgåvor.  Du kan göra detta genom att använda den `NODE_ENV` appinställningen:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Hej swagger-slutpunkt finns på http://*yoursite*.azurewebsites.net/swagger.  Du kan komma åt hello Swagger-användargränssnitt via hello `/swagger/ui` slutpunkt.  Om du väljer toorequire autentisering över hela programmet genererar Swagger ett fel.  Välj tooallow oautentiserade begäranden via för bästa resultat i hello Azure App Service autentisering / auktoriseringsinställningar därefter kontrollera autentisering med hjälp av hello `table.access` egenskapen.

Du kan också lägga till hello Swagger alternativet tooyour `azureMobile.js` filen om du bara vill Swagger-stöd när du utvecklar lokalt.

## <a name="a-namepushpush-notifications"></a><a name="push">Push-meddelanden
Mobile Apps integreras med Azure Notification Hubs tooenable du toosend riktade push-meddelanden toomillions enheter alla större plattformar. Genom att använda Notification Hubs kan du skicka push meddelanden tooiOS, Android och Windows-enheter. toolearn mer om allt som du kan göra med Notification Hubs finns [översikt över Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Så här: skicka push-meddelanden
hello följande kod visar hur toouse hello push objektet toosend en sändning push notification tooregistered iOS-enheter:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do hello push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

Du kan i stället skicka mallen push meddelandet toodevices på alla plattformar som stöds genom att skapa en mall för push-registrering från hello-klienten. Hej följande kod visar hur toosend ett mall-meddelande:

    // Define hello template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do hello push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }


### <a name="push-user"></a>Så här: skicka push-meddelanden tooan autentiserade användare med hjälp av taggar
När en autentiserad användare registrerar för push-meddelanden, läggs en tagg för användar-ID automatiskt toohello registrering. Du kan skicka meddelanden tooall enheter som har registrerats av en viss användare push med hjälp av den här taggen. hello följande kod hämtar hello SID för användaren att göra hello begäran och skickar en mall registreringen push-meddelanden tooevery enhet för användaren:

    // Only do hello push if configured
    if (context.push) {
        // Send a notification toohello current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log hello error.
            }
        });
    }

När du registrerar dig för push-meddelanden från en autentiserad klient, se till att autentiseringen har slutförts innan du försöker utföra registreringen.

## <a name="CustomAPI"></a>Anpassade API: er
### <a name="howto-customapi-basic"></a>Så här: definiera en anpassad API
Dessutom toohello dataåtkomst API via hello /tables slutpunkt, Azure Mobile Apps kan tillhandahålla anpassade API täckning.  Anpassade API: er som definieras i ett liknande sätt toohello definitioner och kan komma åt alla hello samma anläggningar, inklusive autentisering.

Om du vill toouse App Service-autentisering med en anpassad API måste du konfigurera App Service-autentisering i hello [Azure-portalen] första.  För mer information om hur du konfigurerar autentisering i Azure App Service, granska hello konfigurationsguiden för hello identitetsleverantör avser toouse:

* [Hur tooconfigure Azure Active Directory-autentisering]
* [Hur tooconfigure Facebook-autentisering]
* [Hur tooconfigure Google-autentisering]
* [Hur tooconfigure Microsoft Authentication]
* [Hur tooconfigure Twitter-autentisering]

Anpassade API: er som är definierade i mycket hello samma sätt som hello tabeller API.

1. Skapa en **api** directory
2. Skapa en API-definitionen JavaScript-fil i hello **api** directory.
3. Använd hello importera metoden tooimport hello **api** directory.

Här är hello prototyp api-definition baserat på hello basic-app sampel som vi använde tidigare.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Låt oss ta ett exempel API som returnerar hello server datum med hello *Date.now()* metod.  Här är hello api/date.js fil:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Varje parameter är en av hello RESTful standardverb - GET, POST, uppdatera eller ta bort.  hello-metoden är en standard [ExpressJS Middleware] funktion som skickar utdata hello krävs.

### <a name="howto-customapi-auth"></a>Så här: kräver autentisering för åtkomst tooa anpassade API
Azure Mobile Apps-SDK implementerar autentisering i hello samma sätt för anpassade API: erna och hello tabeller slutpunkt.  Om du vill lägga till autentisering toohello API utvecklas i hello föregående avsnitt, lägga till en **åtkomst** egenskapen:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Du kan också ange autentisering på specifika åtgärder:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // hello GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

hello måste samma token som används för hello tabeller slutpunkt användas för anpassade API: er som kräver autentisering.

### <a name="howto-customapi-auth"></a>Så här: hantera stora filöverföringar
Azure Mobile Apps-SDK använder hello [body-parsern mellanprogram](https://github.com/expressjs/body-parser) tooaccept och avkoda brödtext innehållet i din ansökan.  Du kan förkonfigurera body-parsern tooaccept större filöverföringar:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import hello Custom API
    mobile.api.import('./api');

    // Add hello mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

hello-filen är Base64-kodad före överföringen.  Detta ökar hello storleken på hello faktiska överför (och därför hello storlek som du måste ta hänsyn till).

### <a name="howto-customapi-sql"></a>Så här: köra anpassade SQL-instruktioner
hello Azure Mobile Apps-SDK tillåter åtkomst toohello hela kontexten via hello request-objektet, vilket gör att du tooexecute parametriserade SQL-instruktioner toohello definierade dataprovidern enkelt:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on tooa later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define hello query - anything that can be handled by hello mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute hello query.  hello context for Azure Mobile Apps is available through
            // request.azureMobile - hello data object contains hello configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Felsökning, enkelt tabeller och enkelt API: er
### <a name="howto-diagnostic-logs"></a>Så här: Felsök, diagnostisera och Felsök Azure Mobile apps
hello Azure App Service tillhandahåller flera felsökning och felsökningstekniker för Node.js-program.
Se följande artiklar tooget igång vid felsökning av din Node.js Mobile-serverdel toohello:

* [Övervaka en Azure Apptjänst]
* [Aktivera diagnostikloggning i Azure App Service]
* [Felsöka en Azure Apptjänst i Visual Studio]

Node.js-program har åtkomst tooa mängd diagnostiska loggen verktyg.  Internt hello Azure Mobile Apps Node.js SDK använder [Winston] för diagnostikloggning.  Loggning aktiveras automatiskt genom att aktivera felsökningsläge eller genom att ange hello **MS_DebugMode** app inställningen tootrue i hello [Azure-portalen]. Genererade loggar visas i hello diagnostikloggar på hello [Azure-portalen].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Så här: arbeta med enkelt tabeller i hello Azure-portalen
Enkelt tabeller i hello-portalen kan du skapa och arbeta med tabeller direkt i hello-portalen. Du kan även redigera tabellåtgärder med hjälp av hello App Service Editor.

När du klickar på **enkelt tabeller** i dina backend-inställningar, du kan lägga till, ändra eller ta bort en tabell. Du kan också se data i hello tabell.

![Arbeta med enkelt tabeller](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

hello följande kommandon är tillgängliga i hello kommandofält för en tabell:

* **Ändra behörigheter** - hello ändringsbehörighet för läsning, infoga, uppdatera och ta bort hello tabell.
  Alternativen är tooallow anonym åtkomst, toorequire autentisering eller toodisable komma åt toohello igen.
* **Redigera skriptet** -hello skriptfilen för hello tabellen öppnas i hello App Service Editor.
* **Hantera schemat** - Lägg till eller ta bort kolumner eller ändra hello index för tabellen.
* **Rensa tabellen** -trunkerar en befintlig tabell är om du tar bort alla datarader men lämnar hello schemat oförändrat.
* **Ta bort rader** -ta bort enskilda rader med data.
* **Visa direktuppspelningsloggar** -ansluter toohello strömning Loggtjänsten för webbplatsen.

### <a name="work-easy-apis"></a>Så här: arbeta med enkla API: er i hello Azure-portalen
Enkelt API: er i hello-portalen kan du skapa och arbeta med anpassade API: er direkt i hello-portalen. Du kan redigera API-skript med hjälp av hello App Service Editor.

När du klickar på **enkelt API: er** i dina backend-inställningar, du kan lägga till, ändra eller ta bort en anpassad API-slutpunkt.

![Arbeta med enkla API: er](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

I hello portal du ändra hello åtkomstbehörighet för en viss HTTP-åtgärd, redigera hello API skriptfilen i App Service Editor eller visa hello direktuppspelningsloggar.

### <a name="online-editor"></a>Så här: redigera koden i hello App Service Editor
hello Azure-portalen kan du redigera skriptfilerna Node.js-serverdel i hello App Service Editor utan att behöva hämta hello projektet tooyour lokal dator. tooedit skriptfiler i hello online redigeraren:

1. I din Mobilapp backend-bladet klickar du på **alla inställningar** > antingen **enkelt tabeller** eller **enkelt API: er**, klicka på en tabell eller API: et och sedan på **redigera skriptet**. hello skriptfilen öppnas i hello App Service Editor.

    ![App Service Editor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Se dina ändringar toohello kodfilen i hello online-redigeraren. Ändringarna sparas automatiskt när du skriver.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Snabbstart för Android-klienten]: app-service-mobile-android-get-started.md
[Snabbstart för Apache Cordova-klient]: app-service-mobile-cordova-get-started.md
[iOS klienten Snabbstart]: app-service-mobile-ios-get-started.md
[Snabbstart för Xamarin.iOS-klient]: app-service-mobile-xamarin-ios-get-started.md
[Snabbstart för Xamarin.Android-klient]: app-service-mobile-xamarin-android-get-started.md
[Snabbstart för Xamarin.Forms-klient]: app-service-mobile-xamarin-forms-get-started.md
[Snabbstart för Windows Store-klienten]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[datasynkronisering offline]: app-service-mobile-offline-data-sync.md
[Hur tooconfigure Azure Active Directory-autentisering]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Hur tooconfigure Facebook-autentisering]: app-service-mobile-how-to-configure-facebook-authentication.md
[Hur tooconfigure Google-autentisering]: app-service-mobile-how-to-configure-google-authentication.md
[Hur tooconfigure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Hur tooconfigure Twitter-autentisering]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service Deployment Guide]: ../app-service-web/web-sites-deploy.md
[Övervaka en Azure Apptjänst]: ../app-service-web/web-sites-monitor.md
[Aktivera diagnostikloggning i Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Felsöka en Azure Apptjänst i Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[ange hello nod Version]: ../nodejs-specify-node-version-azure-apps.md
[använda noden moduler]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure-portalen]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp exempel på GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo prov på GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[katalogen samples på GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[1.1 för Node.js-verktyg för Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js paketet]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
