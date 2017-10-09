---
title: aaaBuild en Node.js och MongoDB-webbapp i Azure | Microsoft Docs
description: "Lär dig hur tooget en Node.js-app som arbetar i Azure med anslutning tooa Cosmos DB-databas med en anslutningssträng för MongoDB."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 532251c51ed6f8513e6e366393e889b67a85e5b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a>Skapa en Node.js och MongoDB-webbapp i Azure

Azure Web Apps ger en mycket skalbar, automatisk uppdatering värdtjänst. Den här kursen visar hur toocreate en Node.js webbapp i Azure och koppla den tooa MongoDB-databas. När du är klar har du en medelvärde program (MongoDB, snabb, AngularJS och Node.js) som körs i [Azure App Service](app-service-web-overview.md). För enkelhetens skull använder hello exempelprogrammet hello [MEAN.js webbramverk](http://meanjs.org/).

![MEAN.js-app som körs i Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Vad du lära dig:

> [!div class="checklist"]
> * Skapa en MongoDB-databas i Azure
> * Ansluta tooMongoDB en Node.js-app
> * Distribuera hello app tooAzure
> * Uppdatera hello-datamodell och omdistribuera hello app
> * Dataströmmen diagnostiska loggar från Azure
> * Hantera hello appen i hello Azure-portalen

## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

1. [Installera Git](https://git-scm.com/)
1. [Installera Node.js och NPM](https://nodejs.org/)
1. [Installera Gulp.js](http://gulpjs.com/) (krävs av [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))
1. [Installera och köra MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="test-local-mongodb"></a>Testa lokala MongoDB

Öppna hello terminalfönster och `cd` toohello `bin` katalogen MongoDB-installationen. Du kan använda den här terminalfönster toorun alla hello-kommandon i den här självstudiekursen.

Kör `mongo` i hello-terminal tooconnect tooyour lokala MongoDB-servern.

```bash
mongo
```

Om anslutningen lyckas körs redan MongoDB-databas. Om inte, kontrollera att den lokala MongoDB-databasen har startats genom att följa stegen hello på [installera MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/). Ofta MongoDB installeras, men du behöver fortfarande toostart den genom att köra `mongod`. 

När du är klar testning MongoDB-databas, Skriv `Ctrl+C` i hello terminal. 

## <a name="create-local-nodejs-app"></a>Skapa lokal Node.js-app

I det här steget kan du ställa in hello lokalt Node.js-projekt.

### <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

I hello terminalfönster, `cd` tooa arbetskatalogen.  

Hello kör följande kommando tooclone hello exempel lagringsplatsen. 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

Det här exemplet databasen innehåller en kopia av hello [MEAN.js databasen](https://github.com/meanjs/mean). Är det ändrade toorun på Apptjänst (Mer information finns i databasen för hello MEAN.js [filen Viktigt](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).

### <a name="run-hello-application"></a>Kör programmet hello

Kör följande kommandon tooinstall hello krävs paket hello och starta programmet hello.

```bash
cd meanjs
npm install
npm start
```

När hello app har lästs in helt, se något liknande toohello följande meddelande:

```
--
MEAN.JS - Development Environment

Environment:     development
Server:          http://0.0.0.0:3000
Database:        mongodb://localhost/mean-dev
App version:     0.5.0
MEAN.JS version: 0.5.0
--
```

Navigera toohttp://localhost:3000 i en webbläsare. Klicka på **registrera dig** i hello översta menyn och skapa en testanvändare. 

Hej MEAN.js exempelprogrammet lagrar användardata i hello-databasen. Om du lyckas skapa en användare och logga in är appen skriver data toohello lokala MongoDB-databas.

![MEAN.js ansluter har tooMongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

Välj **Admin > Hantera artiklar** tooadd vissa artiklar.

Tryck på toostop Node.js när som helst `Ctrl+C` i hello terminal. 

## <a name="create-production-mongodb"></a>Skapa produktion MongoDB

I det här steget skapar du en MongoDB-databas i Azure. När din app är distribuerade tooAzure, använder den här databasen i molnet.

Den här kursen används för MongoDB, [Azure Cosmos DB](/azure/documentdb/). Cosmos DB stöder MongoDB-klientanslutningar.

### <a name="log-in-tooazure"></a>Logga in tooAzure

Du ska använda hello Azure CLI 2.0 toocreate hello resurser som krävs för toohost din app i Azure. Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med hello [az gruppen skapa](/cli/azure/group#create) kommando.

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

hello följande exempel skapas en resursgrupp i hello Västeuropa region.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Använd hello [az apptjänst lista-platser](/cli/azure/appservice#list-locations) Azure CLI kommandot toolist tillgängliga platser. 

### <a name="create-a-cosmos-db-account"></a>Skapa ett Cosmos-DB-konto

Skapa ett Cosmos-DB-konto med hello [az cosmosdb skapa](/cli/azure/cosmosdb#create) kommando.

I hello följande kommando, ersätta ett unikt Cosmos-databasnamn för hello  *\<cosmosdb_name >* platshållare. Det här namnet används som en del hello hello Cosmos DB slutpunkt `https://<cosmosdb_name>.documents.azure.com/`, så hello namn måste toobe unikt över alla Cosmos-DB-konton i Azure. hello namn får innehålla endast små bokstäver, siffror och hello bindestreck (-), och måste vara mellan 3 och 50 tecken.

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

Hej *--kind MongoDB* parametern aktiverar MongoDB-klientanslutningar.

När hello Cosmos-DB-konto har skapats, visar hello Azure CLI information liknande toohello följande exempel:

```json
{
  "consistencyPolicy":
  {
    "defaultConsistencyLevel": "Session",
    "maxIntervalInSeconds": 5,
    "maxStalenessPrefix": 100
  },
  "databaseAccountOfferType": "Standard",
  "documentEndpoint": "https://<cosmosdb_name>.documents.azure.com:443/",
  "failoverPolicies": 
  ...
  < Output truncated for readability >
}
```

## <a name="connect-app-tooproduction-mongodb"></a>Ansluta appen tooproduction MongoDB

I det här steget kan ansluta du din MEAN.js programmet toohello Cosmos DB exempeldatabasen du precis skapade, med hjälp av en anslutningssträng för MongoDB. 

### <a name="retrieve-hello-database-key"></a>Hämta hello-databas

tooconnect toohello Cosmos DB databasen måste hello databasens nyckel. Använd hello [az cosmosdb lista nycklar](/cli/azure/cosmosdb#list-keys) kommandot tooretrieve hello primärnyckel.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

hello Azure CLI visar information liknande toohello följande exempel:

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Kopiera hello värdet för `primaryMasterKey`. Du behöver den här informationen i hello nästa steg.

<a name="devconfig"></a>
### <a name="configure-hello-connection-string-in-your-nodejs-application"></a>Konfigurera hello anslutningssträngen i Node.js-programmet

Öppna din databas MEAN.js _config/env/production.js_.

I hello `db` objekt, uppdatera hello värdet för `uri`:

* Ersätt hello två  *\<cosmosdb_name >* platshållare med databasnamnet Cosmos DB.
* Ersätt hello  *\<primary_master_key >* platshållaren med hello-nyckel som du kopierade i föregående steg i hello.

hello följande kod visar hello `db` objekt:

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

Hej `ssl=true` alternativet krävs eftersom [Cosmos DB kräver SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 

Spara ändringarna.

### <a name="test-hello-application-in-production-mode"></a>Testa programmet hello i Produktionsläge 

Kör hello följande toominify och paket kommandoskript för hello-produktionsmiljö. Den här processen genererar hello-filer som behövs i hello-produktionsmiljö.

```bash
gulp prod
```

Kör hello efter kommandot toouse hello-anslutningssträng som du konfigurerade i _config/env/production.js_.

```bash
NODE_ENV=production node server.js
```

`NODE_ENV=production`Anger hello miljövariabel som anger Node.js toorun i hello-produktionsmiljö.  `node server.js`startar hello Node.js server med `server.js` i Lagringsplatsens rot. Detta är hur Node.js-programmet läses in i Azure. 

När hello app har lästs in, kontrollera att den körs i produktionsmiljö hello toomake:

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

Navigera toohttp://localhost:8443 i en webbläsare. Klicka på **registrera dig** i hello översta menyn och skapa en testanvändare. Om du lyckas skapa en användare och logga in, sedan skriver din app data toohello Cosmos-DB-databas i Azure. 

Stoppa Node.js genom att skriva i hello terminal, `Ctrl+C`. 

## <a name="deploy-app-tooazure"></a>Distribuera appen tooAzure

I det här steget kan distribuera du din MongoDB-anslutna Node.js-programmet tooAzure Apptjänst.

### <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

Skapa en apptjänstplan med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) kommando. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

hello följande exempel skapas en apptjänstplan med namnet _myAppServicePlan_ med hello **lediga** prisnivån:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

När hello programtjänstplanen har skapats, visar hello Azure CLI information liknande toohello följande exempel:

```json 
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
```

### <a name="create-a-web-app"></a>Skapa en webbapp

Skapa en webbapp i hello `myAppServicePlan` App Service-plan med hello [az webapp skapa](/cli/azure/webapp#create) kommando. 

hello web app ger du en värd utrymme toodeploy koden och ger en URL för du tooview hello distribuerat program. Använda toocreate hello webbapp. 

Följande kommando, Ersätt hello i hello  *\<appnamn >* med ett unikt appnamn. Det här namnet används under hello hello standard-URL för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure App Service. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

När hello webbprogrammet har skapats, visar hello Azure CLI information liknande toohello följande exempel: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-an-environment-variable"></a>Konfigurera en miljövariabel

Tidigare i hello självstudier, du hårdkodad hello databasanslutningssträng i _config/env/production.js_. Inom ramen för säkerhets skull bör du tookeep känsliga data utanför Git-lagringsplatsen. För din app som körs i Azure, ska du använda en miljövariabel i stället.

I App Service som du anger miljövariabler som _appinställningar_ med hjälp av hello [az webapp config appsettings uppdatera](/cli/azure/webapp/config/appsettings#update) kommando. 

hello följande exempel konfigureras en `MONGODB_URI` appinställningen i ditt Azure webbapp. Ersätt hello  *\<appnamn >*,  *\<cosmosdb_name >*, och  *\<primary_master_key >* platshållare.

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

I Node.js-kod du åtkomst till den här appinställning med `process.env.MONGODB_URI`, precis som du skulle använda en miljövariabel. 

Nu kan ångra ändringar-too_config/env/production.js_ med hello följande kommando:

```bash
git checkout -- .
```

Öppna _config/env/production.js_ igen. Observera att MEAN.js standardappen hello är redan konfigurerade toouse hello `MONGODB_URI` miljövariabel som du skapade.

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a>Konfigurera lokal git-distribution 

Använd hello [az webapp distributionsanvändare har angetts](/cli/azure/webapp/deployment/user#set) kommandot toocreate autentiseringsuppgifter för distribution.

Du kan distribuera ditt program tooAzure Apptjänst på olika sätt, inklusive FTP, lokal Git, GitHub, Visual Studio Team Services och BitBucket. FTP- och lokala Git, är det nödvändigt toohave en distribution av användare som har konfigurerats på hello server tooauthenticate din distribution. Den här distributionen användaren är kontonivå och skiljer sig från ditt konto i Azure-prenumeration. Du behöver bara tooconfigure distribution användaren en gång.

Följande kommando, Ersätt i hello  *\<användarnamn >* och  *\<lösenord >* med ett nytt användarnamn och lösenord. hello användarnamnet måste vara unika. hello lösenord måste innehålla minst åtta tecken, med två av följande tre element hello: bokstäver, siffror, symboler. Om du får en ` 'Conflict'. Details: 409` fel, ändra hello användarnamn. Om du ser felet ` 'Bad Request'. Details: 400` ska du använda ett starkare lösenord.

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

Posten hello användarnamn och lösenord för användning i senare steg när du distribuerar hello app.

Använd hello [az webapp distribution källa config-lokal-git](/cli/azure/webapp/deployment/source#config-local-git) kommandot tooconfigure lokal Git åtkomst toohello Azure webbapp. 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

När hello distribution användaren är konfigurerad, visar hello Azure CLI hello distributionens URL för din Azure-webbapp i hello följande format:

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

Kopiera hello utdata från hello terminal som kommer att användas i hello nästa steg. 

### <a name="push-tooazure-from-git"></a>Push-tooAzure från Git

Lägg till en Azure remote tooyour lokal Git-lagringsplats. 

```bash
git remote add azure <paste_copied_url_here> 
```

Skicka toohello Azure remote toodeploy Node.js-programmet. Du uppmanas att hello lösenordet du angav tidigare som en del av hello skapandet av hello distribution av användaren. 

```bash
git push azure master
```

Under distributionen kommunicerar förloppet med Git i Azure App Service.

```bash
Counting objects: 5, done.
Delta compression using up too4 threads.
Compressing objects: 100% (5/5), done.
Writing objects: 100% (5/5), 489 bytes | 0 bytes/s, done.
Total 5 (delta 3), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6c7c716eee'.
remote: Running custom deployment command...
remote: Running deployment command...
remote: Handling node.js deployment.
.
.
.
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

Det kan hända att hello distributionsprocessen körs [Gulp](http://gulpjs.com/) när `npm install`. App Service körs inte Gulp eller Grunt uppgifter under distributionen, så att den här lagringsplatsen exempel har två ytterligare filer i dess rot directory tooenable som: 

- _.Deployment_ -den här filen talar om Apptjänst toorun `bash deploy.sh` som hello distribution av anpassade skript.
- _Deploy.SH_ -hello anpassat distributionsskriptet. Om du har läst hello-fil, ser du att den körs `gulp prod` när `npm install` och `bower install`. 

Du kan använda den här metoden tooadd alla steg tooyour Git-baserad distribution. Om du startar om din Azure-webbapp när som helst kör inte App Service dessa automation-aktiviteter.

### <a name="browse-toohello-azure-web-app"></a>Bläddra toohello Azure-webbapp 

Bläddra toohello distribuera webbprogram med hjälp av webbläsaren. 

```bash 
http://<app_name>.azurewebsites.net 
``` 

Klicka på **registrera dig** i hello översta menyn och skapa en dummy användare. 

Om du har lyckats och hello appen automatiskt loggar in har toohello skapade användaren sedan MEAN.js-app i Azure anslutningen toohello MongoDB (Cosmos-DB) databas. 

![MEAN.js-app som körs i Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Välj **Admin > Hantera artiklar** tooadd vissa artiklar. 

**Grattis!** Du kör en datadrivna Node.js-app i Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Uppdatera datamodellen och distribuera om

I det här steget kan du ändra hello `article` data modell och publicera din ändring tooAzure.

### <a name="update-hello-data-model"></a>Uppdatera hello-datamodell

Öppna _modules/articles/server/models/article.server.model.js_.

I `ArticleSchema`, lägga till en `String` typ som kallas `comment`. När du är klar schemat koden se ut så här:

```javascript
var ArticleSchema = new Schema({
  ...,
  user: {
    type: Schema.ObjectId,
    ref: 'User'
  },
  comment: {
    type: String,
    default: '',
    trim: true
  }
});
```

### <a name="update-hello-articles-code"></a>Uppdatera hello artiklar kod

Uppdatera hello resten av dina `articles` code toouse `comment`.

Det finns fem filer som du behöver toomodify: hello server domänkontrollant och hello fyra klienten vyer. 

Öppna _modules/articles/server/controllers/articles.server.controller.js_.

I hello `update` fungera, lägga till en tilldelning för `article.comment`. hello följande kod visar hello slutförts `update` funktionen:

```javascript
exports.update = function (req, res) {
  var article = req.article;

  article.title = req.body.title;
  article.content = req.body.content;
  article.comment = req.body.comment;

  ...
};
```

Öppna _modules/articles/client/views/view-article.client.view.html_.

Ovanför hello avslutande `</section>` tagg, Lägg till följande rad toodisplay hello `comment` tillsammans med hello resten av hello artikeldata:

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

Öppna _modules/articles/client/views/list-articles.client.view.html_.

Ovanför hello avslutande `</a>` tagg, Lägg till följande rad toodisplay hello `comment` tillsammans med hello resten av hello artikeldata:

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

Öppna _modules/articles/client/views/admin/list-articles.client.view.html_.

I hello `<div class="list-group">` element och ovanför hello avslutande `</a>` tagg, Lägg till följande rad toodisplay hello `comment` tillsammans med hello resten av hello artikeldata:

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

Öppna _modules/articles/client/views/admin/form-article.client.view.html_.

Hitta hello `<div class="form-group">` element som innehåller hello knappen Skicka som ser ut så här:

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

Lägga till en annan ovanför den här taggen `<div class="form-group">` element som används för att redigera hello `comment` fältet. Din nya element ska se ut så här:

```HTML
<div class="form-group">
  <label class="control-label" for="comment">Comment</label>
  <textarea name="comment" data-ng-model="vm.article.comment" id="comment" class="form-control" cols="30" rows="10" placeholder="Comment"></textarea>
</div>
```

### <a name="test-your-changes-locally"></a>Testa ändringarna lokalt

Spara alla ändringar.

Testa dina ändringar i Produktionsläge igen.

```bash
gulp prod
NODE_ENV=production node server.js
```

> [!NOTE]
> Kom ihåg att din _config/env/production.js_ har återställts och hello `MONGODB_URI` miljövariabeln anges endast i ditt Azure webbapp och inte på den lokala datorn. Om du tittar på hello config-fil som du hittar den hello produktion configuration standardvärden toouse en lokal MongoDB-databas. Detta säkerställer att du inte rör produktionsdata när du testar din kodändringar lokalt.

Navigera för`http://localhost:8443` i en webbläsare och se till att du har loggat in.

Välj **Admin > Hantera artiklar**, lägga till en artikel genom att välja hello  **+**  knappen.

Du ser hello nya `Comment` textruta nu.

![Tillagda kommentar fältet tooArticles](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

Stoppa Node.js genom att skriva i hello terminal, `Ctrl+C`. 

### <a name="publish-changes-tooazure"></a>Publicera ändringar tooAzure

Genomför ändringarna i Git och sedan push hello kod ändringar tooAzure.

```bash
git commit -am "added article comment"
git push azure master
```

En gång hello `git push` är klar, navigera tooyour Azure webbapp och testa hello nya funktioner.

![Ändringar i modellen och databasen publicerade tooAzure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

Om du lagt till alla artiklar tidigare kan kan du fortfarande se dem. Befintliga data i Cosmos-databasen är inte förlorade. Dessutom uppdateringar toohello dataschemat och lämnar företaget dina befintliga data.

## <a name="stream-diagnostic-logs"></a>Dataströmmen diagnostikloggar 

När din Node.js-program körs i Azure App Service, kan du få hello konsolen loggar via rörledningar tooyour terminal. På så sätt kan du hello diagnostiska meddelanden för samma toohelp du felsöka programfel.

toostart loggen direktuppspelning, Använd hello [az webapp loggen pilslut](/cli/azure/webapp/log#tail) kommando.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

Uppdatera din Azure-webbapp i hello webbläsare tooget vissa webbtrafik när loggen streaming har startats. Du kan nu se loggarna för konsolen skickas tooyour terminal.

Stopploggen strömning när som helst genom att skriva `Ctrl+C`. 

## <a name="manage-your-azure-web-app"></a>Hantera Azure-webbapp

Gå toohello [Azure-portalen](https://portal.azure.com) toosee hello webbprogram som du skapade.

Hello vänstra menyn klickar du på **Apptjänster**, klicka på hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

Som standard visar hello portal ditt webbprogram **översikt** sidan. På den här sidan får du en översikt över hur det går för appen. Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. hello flikar på hello vänster hello sidan visar hello annan konfigurationssidor som du kan öppna.

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Nästa steg

Vad du lärt dig:

> [!div class="checklist"]
> * Skapa en MongoDB-databas i Azure
> * Ansluta tooMongoDB en Node.js-app
> * Distribuera hello app tooAzure
> * Uppdatera hello-datamodell och omdistribuera hello app
> * Dataströmmen loggas från Azure tooyour terminal
> * Hantera hello appen i hello Azure-portalen

I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn tooyour webbprogram.

> [!div class="nextstepaction"] 
> [Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](app-service-web-tutorial-custom-domain.md)
