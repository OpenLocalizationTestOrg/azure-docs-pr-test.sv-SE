---
title: "aaaDeploy en Sails.js web app tooAzure Apptjänst | Microsoft Docs"
description: "Lär dig hur toodeploy Node.js-programmet Azure App Service. Den här kursen visar hur toodeploy en Sails.js-webbapp."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 8877ddc8-1476-45ae-9e7f-3c75917b4564
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: f5b2518b9c87c040845f7268763862be8c15e83e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-sailsjs-web-app-tooazure-app-service"></a>Distribuera en Sails.js web app tooAzure Apptjänst
De här självstudierna visar hur toodeploy en Sails.js app tooAzure Apptjänst. Hello pågående, kan du glean vissa allmänna kunskaper om hur tooconfigure toorun din Node.js-app i App Service.

Här kan du lära dig användbara kunskaper som:

* Konfigurera en Sails.js-app som körs i Apptjänst.
* Distribuera en app tooApp tjänsten från hello kommandorad.
* Läsa stderr och stdout loggar tootroubleshoot eventuella distributionsproblem.
* Lagra miljövariabler utanför källkontroll.
* Azure miljövariabler för åtkomst från din app.
* Ansluta tooa databasen (MongoDB).

Du bör ha kunskaper om Sails.js. Den här kursen är inte avsedda toohelp du med problem relaterade toorunning Sail.js i allmänhet.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet

Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](app-service-web-nodejs-sails-cli-nodejs.md) – våra CLI för hello klassisk och resurs management distributionsmodeller
- [Azure CLI 2.0](app-service-web-nodejs-sails.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

## <a name="prerequisites"></a>Krav
* [Node.js](https://nodejs.org/)
* [Sails.js](http://sailsjs.org/get-started)
* [Git](http://www.git-scm.com/downloads)
* [Azure CLI 2.0](/cli/azure/install-az-cli2)
* Ett Microsoft Azure-konto. Om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Du kan [Prova App Service](https://azure.microsoft.com/try/app-service/) utan ett Azure-konto. Skapa en startapp och spela upp med den för in tooan timme – inget kreditkort behövs, inga åtaganden.
> 
> 

## <a name="step-1-create-and-configure-a-sailsjs-app-locally"></a>Steg 1: Skapa och konfigurera en Sails.js-appen lokalt
Först snabbt skapa en standard Sails.js-app i din utvecklingsmiljö genom att följa dessa steg:

1. Öppna hello kommandoradsverktyget terminal önskat och `CD` tooa arbetskatalogen.
2. Skapa en Sails.js-app och kör:

        sails new <app_name>
        cd <app_name>
        sails lift

    Kontrollera att du kan navigera toohello standardstartsida på http://localhost:1377.

1. Därefter aktiverar du loggning för Azure. Skapa en fil med namnet i din rotkatalog `iisnode.yml` och Lägg till följande två rader hello:

        loggingEnabled: true
        logDirectory: iisnode

    Loggning har nu aktiverats för hello [iisnode](https://github.com/tjanczuk/iisnode) server att Azure Apptjänst använder toorun Node.js-appar. 
    Mer information om hur detta fungerar finns [hur toodebug en Node.js webbapp i Azure App Service](web-sites-nodejs-debug.md).

2. Konfigurera Azure miljövariabler för hello Sails.js för app toouse. Öppna config/env/production.js tooconfigure produktionsmiljön och ange `port` och `hookTimeout`:

        module.exports = {

            // Use process.env.port toohandle web requests toohello default HTTP port
            port: process.env.port,
            // Increase hooks timout too30 seconds
            // This avoids hello Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Du hittar dokumentationen för dessa konfigurationsinställningar i de [Sails.js dokumentationen](http://sailsjs.org/documentation/reference/configuration/sails-config).

4. Nästa, hårdkoda hello Node.js-version du vill toouse. Lägg till följande hello i package.json, `engines` egenskapen tooset hello Node.js version tooone som vi vill.

        "engines": {
            "node": "6.9.1"
        },

5. Slutligen initiera en Git-lagringsplats och bekräfta dina filer. I hello tillämpningsprogrammets rot (där package.json är), kör hello följande Git-kommandon:

        git init
        git add .
        git commit -m "<your commit message>"

Koden är klar toobe distribueras. 

## <a name="step-2-create-an-azure-app-and-deploy-sailsjs"></a>Steg 2: Skapa en Azure-program och distribuera Sails.js

Därefter skapar hello App Service-resurs i Azure och distribuera din app Sails.js-tooit.

1. logga in tooAzure som detta:

        az login

    Följ hello fråga toocontinue hello inloggning i en webbläsare med ett Microsoft-konto som har din Azure-prenumeration.

3. Ange hello distribution användaren för Apptjänst. Du distribuerar kod med dessa autentiseringsuppgifterna senare.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) med ett namn. För den här självstudien om Node.js behöver du inte verkligen tooknow vad det är.

        az group create --location "<location>" --name my-sailsjs-app-group

    toosee vilka möjliga värden du kan använda för `<location>`, använda hello `az appservice list-locations` CLI-kommando.

3. Skapa en ”gratis” [programtjänstplanen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) med ett namn. För den här självstudien om Node.js, ska du bara vet att du inte kommer att debiteras för webbprogram i den här planen.

        az appservice plan create --name my-sailsjs-appservice-plan --resource-group my-sailsjs-app-group --sku FREE

4. Skapa en ny webbapp med ett unikt namn i `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-sailsjs-app-group --plan my-sailsjs-appservice-plan

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>Steg 3: Konfigurera och distribuera en Sails.js-app

1. Konfigurera lokal Git-distribution för din nya webbapp med hello följande kommando:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-sailsjs-app-group

    Du får en JSON-utdata som liknar detta, vilket innebär att hello fjärransluten Git-lagringsplats har konfigurerats:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Lägg till hello URL i hello JSON som en fjärransluten Git för lokala databasen (kallas `azure` för enkelhetens skull).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Distribuera din exempel kod toohello `azure` fjärransluten Git. När du uppmanas, använda autentiseringsuppgifter för distribution hello du tidigare har konfigurerat.

        git push azure master

7. Bara starta till sist din aktiva Azure-app i hello webbläsare:

        az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

    Du bör nu se hello samma Sails.js-startsidan.

    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>Felsöka distributionen
Om tillämpningsprogrammet Sails.js misslyckas av någon anledning i App Service, hitta hello stderr-loggar toohelp felsöka den.
Mer information finns i [hur toodebug en Node.js webbapp i Azure App Service](web-sites-nodejs-debug.md).
Om hello app har startats hello stdout loggen ska visa bekant hello-meddelande:

                   .-..-.
    
       Sails              <|    .-..-.
       v0.12.11            |\
                          /|.\
                         / || \
                       ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
       __---___--___---___--___---___--___
     ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    toosee your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    tooshut down Sails, press <CTRL> + C at any time.

Du kan styra granularitet hello stdout loggar i hello [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) fil.

## <a name="connect-tooa-database-in-azure"></a>Ansluta tooa databas i Azure
tooconnect tooa databas i Azure måste du skapar hello databasen önskat i Azure, till exempel Azure SQL Database, MySQL, MongoDB, Azure (Redis)-Cache, osv., och använder hello motsvarande [datastore kortet](https://github.com/balderdashy/sails#compatibility) tooconnect tooit. hello steg i det här avsnittet visar hur tooconnect tooMongoDB med hjälp av en [Azure Cosmos DB](../documentdb/documentdb-protocol-mongodb.md) databas som har stöd för MongoDB-klientanslutningar.

1. [Skapa ett Cosmos-DB-konto med Protokollstöd för MongoDB](../documentdb/documentdb-create-mongodb-account.md).
2. [Skapa en Cosmos-DB-samling och databasen](../documentdb/documentdb-create-collection.md). hello namnet på hello samling spelar ingen roll, men du måste hello namnet på hello-databasen när du ansluter från Sails.js.
3. [Hitta hello anslutningsinformation för databasen Cosmos DB](../cosmos-db/connect-mongodb-account.md#GetCustomConnection).
2. Installera hello MongoDB-kort från kommandoraden terminalen:

        npm install sails-mongo --save

3. Öppna config/connections.js och Lägg till hello efter anslutning objektlistan toohello:

        docDbMongo: {
            adapter: 'sails-mongo',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost,
            port: process.env.dbport,
            database: process.env.dbname,
            ssl: true
        },

    > [!NOTE] 
    > Hej `ssl: true` alternativet är viktigt eftersom [Cosmos DB kräver](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 
    >
    >

4. För varje miljövariabel (`process.env.*`), behöver du tooset den i App Service. toodo detta, kör hello följande kommandon från terminalen. Använd hello anslutningsinformationen för Cosmos-DB.

        az appservice web config appsettings update --settings dbuser="<database user>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbpassword="<database password>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbhost="<database hostname>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbport="<database port>" --name <app_name> --resource-group my-sailsjs-app-group
        az appservice web config appsettings update --settings dbname="<database name>" --name <app_name> --resource-group my-sailsjs-app-group

    Om du placerar dina inställningar i Azure app-inställningar håller känsliga data från din källkontrollen (Git). Därefter konfigurerar du din development environment toouse hello samma anslutningsinformationen.
5. Öppna config/local.js och Lägg till följande anslutningar objektet hello:

        connections: {
            docDbMongo: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>",
                database: "<database name>",
                ssl: true
            },
        },

    Den här konfigurationen åsidosätter hello inställningar i filen config/connections.js för hello lokala miljö. Den här filen är exkluderad av hello standard .gitignore i projektet, så att den inte lagras i Git. Du är nu, kan tooconnect tooyour Cosmos DB (MongoDB)-databas från Azure-webbapp och från din lokala utvecklingsmiljö.
6. Öppna config/env/production.js tooconfigure produktionsmiljön och Lägg till följande hello `models` objekt:

        models: {
            connection: 'docDbMongo',
            migrate: 'safe'
        },
7. Öppna config/env/development.js tooconfigure utvecklingsmiljön och Lägg till följande hello `models` objekt:

        models: {
            connection: 'docDbMongo',
            migrate: 'alter'
        },

    `migrate: 'alter'`kan du använda databasen migrering funktioner toocreate och enkelt uppdatera databasen samlingar eller tabeller. Dock `migrate: 'safe'` används för miljön i Azure (produktion) eftersom Sails.js inte tillåter toouse `migrate: 'alter'` i en produktionsmiljö (se [Sails.js dokumentationen](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).
8. Från hello terminal, [generera](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) en Sails.js [modell API](http://sailsjs.org/documentation/concepts/blueprints) som vanligtvis skulle sedan kör du `sails lift` att skapa hello databas med Sails.js Databasmigrering. Exempel:

         sails generate api mywidget
         sails lift

    Hej `mywidget` modell som genererats av det här kommandot är tom, men vi kan använda den tooshow som vi har databasanslutning.
    När du kör `sails lift`, skapar den hello saknas samlingar och tabeller för hello modeller använder din app.
9. Åtkomst hello utkast API du skapade i hello webbläsare. Exempel:

        http://localhost:1337/mywidget/create

    hello API ska returnera hello skapat posten tillbaka tooyou hello i fönster i webbläsaren, vilket innebär att din samling har skapats.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}
10. Nu push tooAzure dina ändringar och bläddra tooyour app toomake att den fortfarande fungerar.

         git add .
         git commit -m "<your commit message>"
         git push azure master
         az appservice web browse --name <app_name> --resource-group my-sailsjs-app-group

11. Åtkomst hello utkast API för din Azure webbapp. Exempel:

         http://<appname>.azurewebsites.net/mywidget/create

     Om en annan ny post returnerar hello API pratar Azure-webbapp tooyour Cosmos DB (MongoDB)-databas.

## <a name="more-resources"></a>Fler resurser
* [Kom igång med Node.js-webbappar i Azure App Service](app-service-web-get-started-nodejs.md)
* [Använda Node.js-moduler med Azure-program](../nodejs-use-node-modules-azure-apps.md)
