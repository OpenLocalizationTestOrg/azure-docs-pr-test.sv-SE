---
title: Skapa en Node.js och MongoDB-webbapp i Azure | Microsoft Docs
description: "Lär dig hur du hämtar en Node.js-app som arbetar i Azure, med anslutning till en Cosmos-DB-databas med en anslutningssträng för MongoDB."
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
ms.openlocfilehash: 3b309382be8cdf8d48b396207fd482a5dc5ed934
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="build-a-nodejs-and-mongodb-web-app-in-azure"></a>Skapa en Node.js och MongoDB-webbapp i Azure

Azure Web Apps ger en mycket skalbar, automatisk uppdatering värdtjänst. Den här kursen visar hur du skapar en Node.js-webbapp i Azure och ansluta den till en MongoDB-databas. När du är klar har du en medelvärde program (MongoDB, snabb, AngularJS och Node.js) som körs i [Azure App Service](app-service-web-overview.md). För enkelhetens skull exempelprogrammet använder den [MEAN.js webbramverk](http://meanjs.org/).

![MEAN.js-app som körs i Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Vad du lära dig:

> [!div class="checklist"]
> * Skapa en MongoDB-databas i Azure
> * Ansluta en Node.js-app till MongoDB
> * Distribuera appen till Azure
> * Uppdatera datamodellen och distribuera appen
> * Dataströmmen diagnostiska loggar från Azure
> * Hantera appen i Azure-portalen

## <a name="prerequisites"></a>Krav

För att slutföra den här kursen behöver du:

1. [Installera Git](https://git-scm.com/)
1. [Installera Node.js och NPM](https://nodejs.org/)
1. [Installera Gulp.js](http://gulpjs.com/) (krävs av [MEAN.js](http://meanjs.org/docs/0.5.x/#getting-started))
1. [Installera och köra MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="test-local-mongodb"></a>Testa lokala MongoDB

Öppna fönstret terminal och `cd` till den `bin` katalogen MongoDB-installationen. Du kan använda den här terminalfönster för att köra alla kommandon i den här självstudiekursen.

Kör `mongo` i terminalen för att ansluta till din lokala MongoDB-servern.

```bash
mongo
```

Om anslutningen lyckas körs redan MongoDB-databas. Om inte, kontrollera att den lokala MongoDB-databasen har startats genom att följa stegen i [installera MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/). Ofta MongoDB installeras, men du måste fortfarande starta genom att köra `mongod`. 

När du är klar testning MongoDB-databas, Skriv `Ctrl+C` i terminalen. 

## <a name="create-local-nodejs-app"></a>Skapa lokal Node.js-app

I det här steget kan du ställa in det lokala Node.js-projektet.

### <a name="clone-the-sample-application"></a>Klona exempelprogrammet

I fönstret terminal `cd` till en arbetskatalog.  

Klona exempellagringsplatsen med följande kommando. 

```bash
git clone https://github.com/Azure-Samples/meanjs.git
```

Det här exemplet databasen innehåller en kopia av den [MEAN.js databasen](https://github.com/meanjs/mean). Den ändras för att köras på Apptjänst (Mer information finns i databasen MEAN.js [filen Viktigt](https://github.com/Azure-Samples/meanjs/blob/master/README.md)).

### <a name="run-the-application"></a>Köra programmet

Kör följande kommandon för att installera de nödvändiga paketen och starta programmet.

```bash
cd meanjs
npm install
npm start
```

När appen har lästs in helt, se något som liknar följande meddelande:

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

Gå till http://localhost: 3000 i en webbläsare. Klicka på **registrera dig** i den översta menyn och skapa en testanvändare. 

MEAN.js-exempelprogrammet lagrar användardata i databasen. Om du lyckas skapa en användare och logga in sedan skriver din app data till den lokala MongoDB-databasen.

![MEAN.js ansluter till MongoDB](./media/app-service-web-tutorial-nodejs-mongodb-app/mongodb-connect-success.png)

Välj **Admin > Hantera artiklar** att lägga till vissa artiklar.

Stoppa Node.js när som helst genom att trycka på `Ctrl+C` i terminalen. 

## <a name="create-production-mongodb"></a>Skapa produktion MongoDB

I det här steget skapar du en MongoDB-databas i Azure. När appen har distribuerats till Azure, använder den här databasen i molnet.

Den här kursen används för MongoDB, [Azure Cosmos DB](/azure/documentdb/). Cosmos DB stöder MongoDB-klientanslutningar.

### <a name="log-in-to-azure"></a>Logga in på Azure

Du använder Azure CLI 2.0 till att skapa de resurser som behövs när Azure ska vara värd för din app. Logga in på Azure-prenumerationen med kommandot [az login](/cli/azure/#login) och följ anvisningarna på skärmen.

```azurecli-interactive
az login
```   

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en resursgrupp med kommandot [az group create](/cli/azure/group#create).

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

Följande exempel skapar en resursgrupp i regionen västeuropa.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Använd den [az apptjänst lista-platser](/cli/azure/appservice#list-locations) Azure CLI-kommando för att lista över tillgängliga platser. 

### <a name="create-a-cosmos-db-account"></a>Skapa ett Cosmos-DB-konto

Skapa ett Cosmos-DB-konto med den [az cosmosdb skapa](/cli/azure/cosmosdb#create) kommando.

I följande kommando i stället använda ett unikt Cosmos-databasnamn för den  *\<cosmosdb_name >* platshållare. Det här namnet används som en del av Cosmos-DB-slutpunkten `https://<cosmosdb_name>.documents.azure.com/`, så namnet måste vara unikt för alla Cosmos-DB-konton i Azure. Namnet måste innehålla endast små bokstäver, siffror och bindestreck (-) och måste vara mellan 3 och 50 tecken.

```azurecli-interactive
az cosmosdb create \
    --name <cosmosdb_name> \
    --resource-group myResourceGroup \
    --kind MongoDB
```

Den *--kind MongoDB* parametern aktiverar MongoDB-klientanslutningar.

När Cosmos-DB-kontot har skapats visas Azure CLI information liknar följande exempel:

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

## <a name="connect-app-to-production-mongodb"></a>Anslut appen till produktion MongoDB

I det här steget kan ansluta du MEAN.js exempelprogrammet till Cosmos-DB-databasen som du precis skapade, med hjälp av en anslutningssträng för MongoDB. 

### <a name="retrieve-the-database-key"></a>Hämta databasens nyckel

För att ansluta till databasen Cosmos DB, måste databasnyckeln för. Använd kommandot [az documentdb list-keys](/cli/azure/cosmosdb#list-keys) för att hämta den primära nyckeln.

```azurecli-interactive
az cosmosdb list-keys --name <cosmosdb_name> --resource-group myResourceGroup
```

Azure CLI visar information som liknar följande exempel:

```json
{
  "primaryMasterKey": "RS4CmUwzGRASJPMoc0kiEvdnKmxyRILC9BWisAYh3Hq4zBYKr0XQiSE4pqx3UchBeO4QRCzUt1i7w0rOkitoJw==",
  "primaryReadonlyMasterKey": "HvitsjIYz8TwRmIuPEUAALRwqgKOzJUjW22wPL2U8zoMVhGvregBkBk9LdMTxqBgDETSq7obbwZtdeFY7hElTg==",
  "secondaryMasterKey": "Lu9aeZTiXU4PjuuyGBbvS1N9IRG3oegIrIh95U6VOstf9bJiiIpw3IfwSUgQWSEYM3VeEyrhHJ4rn3Ci0vuFqA==",
  "secondaryReadonlyMasterKey": "LpsCicpVZqHRy7qbMgrzbRKjbYCwCKPQRl0QpgReAOxMcggTvxJFA94fTi0oQ7xtxpftTJcXkjTirQ0pT7QFrQ=="
}
```

Kopiera värdet för `primaryMasterKey`. Du behöver den här informationen i nästa steg.

<a name="devconfig"></a>
### <a name="configure-the-connection-string-in-your-nodejs-application"></a>Konfigurera anslutningssträngen i ditt Node.js-program

Öppna din databas MEAN.js _config/env/production.js_.

I den `db` objekt, uppdatera värdet i `uri`:

* Ersätt två  *\<cosmosdb_name >* platshållare med databasnamnet Cosmos DB.
* Ersätt den  *\<primary_master_key >* med den nyckel som du kopierade i föregående steg.

Följande kod visar den `db` objekt:

```javascript
db: {
  uri: 'mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false',
  ...
},
```

Den `ssl=true` alternativet krävs eftersom [Cosmos DB kräver SSL](../cosmos-db/connect-mongodb-account.md#connection-string-requirements). 

Spara ändringarna.

### <a name="test-the-application-in-production-mode"></a>Testa programmet i Produktionsläge 

Kör följande kommando för att minify och sedan paketera skript för produktionsmiljön. Den här processen genererar filer som behövs i produktionsmiljön.

```bash
gulp prod
```

Kör följande kommando för att använda den anslutningssträng som du konfigurerade i _config/env/production.js_.

```bash
NODE_ENV=production node server.js
```

`NODE_ENV=production`anger miljövariabeln som talar om Node.js körs i produktionsmiljön.  `node server.js`startar Node.js-server med `server.js` i Lagringsplatsens rot. Detta är hur Node.js-programmet läses in i Azure. 

När appen har lästs in, kontrollera att den körs i produktionsmiljön:

```
--
MEAN.JS

Environment:     production
Server:          http://0.0.0.0:8443
Database:        mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true&sslverifycertificate=false
App version:     0.5.0
MEAN.JS version: 0.5.0
```

Navigera till http://localhost:8443 i en webbläsare. Klicka på **registrera dig** i den översta menyn och skapa en testanvändare. Om du lyckas skapa en användare och logga in, sedan skriver din app data till Cosmos-DB-databas i Azure. 

Stoppa Node.js genom att skriva till terminalen och `Ctrl+C`. 

## <a name="deploy-app-to-azure"></a>Distribuera appen till Azure

I det här steget kan distribuera du MongoDB-anslutna Node.js-programmet till Azure App Service.

### <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

Skapa en App Service-plan med kommandot [az appservice plan create](/cli/azure/appservice/plan#create). 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

I följande exempel skapas en apptjänstplan med namnet _myAppServicePlan_ med hjälp av den **lediga** prisnivån:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

När programtjänstplanen har skapats visas Azure CLI information liknar följande exempel:

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

Skapa en webbapp i den `myAppServicePlan` App Service-plan med de [az webapp skapa](/cli/azure/webapp#create) kommando. 

Webbappen ger dig en värd med utrymme för att distribuera din kod och ger en URL som du kan visa det distribuerade programmet. Använd för att skapa webbprogrammet. 

I följande kommando och ersätter den  *\<appnamn >* med ett unikt appnamn. Det här namnet används som en del av standard-URL för webbapp så att namnet måste vara unikt över alla program i Azure App Service. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

När webbappen har skapats visar Azure CLI information liknande den i följande exempel: 

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

Tidigare i självstudierna hårdkodad databasanslutningen sträng i _config/env/production.js_. Inom ramen för säkerhets skull bör vill du behålla dessa känsliga data utanför Git-lagringsplatsen. För din app som körs i Azure, ska du använda en miljövariabel i stället.

I App Service som du anger miljövariabler som _appinställningar_ med hjälp av den [az webapp config appsettings uppdatera](/cli/azure/webapp/config/appsettings#update) kommando. 

I följande exempel konfigureras en `MONGODB_URI` appinställningen i ditt Azure webbapp. Ersätt den  *\<appnamn >*,  *\<cosmosdb_name >*, och  *\<primary_master_key >* platshållare.

```azurecli-interactive
az webapp config appsettings update \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings MONGODB_URI="mongodb://<cosmosdb_name>:<primary_master_key>@<cosmosdb_name>.documents.azure.com:10250/mean?ssl=true"
```

I Node.js-kod du åtkomst till den här appinställning med `process.env.MONGODB_URI`, precis som du skulle använda en miljövariabel. 

Nu kan ångra ändringarna till _config/env/production.js_ med följande kommando:

```bash
git checkout -- .
```

Öppna _config/env/production.js_ igen. Observera att MEAN.js standardappen har redan konfigurerats för att använda den `MONGODB_URI` miljövariabel som du skapade.

```javascript
db: {
  uri: ... || process.env.MONGODB_URI || ...,
  ...
},
```

### <a name="configure-local-git-deployment"></a>Konfigurera lokal git-distribution 

Använd den [az webapp distributionsanvändare har angetts](/cli/azure/webapp/deployment/user#set) kommando för att skapa autentiseringsuppgifter för distribution.

Du kan distribuera programmet till Azure App Service på olika sätt, inklusive FTP, lokal Git, GitHub, Visual Studio Team Services och BitBucket. För FTP- och lokala Git är det nödvändigt att ha en distribution konfigurerade användaren på servern att autentisera din distribution. Den här distributionen användaren är kontonivå och skiljer sig från ditt konto i Azure-prenumeration. Du behöver bara konfigurera den här distributionen användaren en gång.

I följande kommando ersätter du  *\<användarnamn >* och  *\<lösenord >* med ett nytt användarnamn och lösenord. Användarnamnet måste vara unikt. Lösenordet måste innehålla minst åtta tecken, med två av följande tre element: bokstäver, siffror, symboler. Om du ser felet ` 'Conflict'. Details: 409` ska du byta användarnamn. Om du ser felet ` 'Bad Request'. Details: 400` ska du använda ett starkare lösenord.

```azurecli-interactive
az appservice web deployment user set --user-name <username> --password <password>
```

Registrera användarnamn och lösenord som ska användas i senare steg när du distribuerar appen.

Använd den [az webapp distribution källa config-lokal-git](/cli/azure/webapp/deployment/source#config-local-git) kommando för att konfigurera lokal Git-åtkomst till Azure webbapp. 

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup
```

När användaren distribution är konfigurerad, visar Azure CLI distribution URL-Adressen för din Azure-webbapp i följande format:

```bash 
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git 
``` 

Kopiera utdata från terminalen och som kommer att användas i nästa steg. 

### <a name="push-to-azure-from-git"></a>Skicka till Azure från Git

Lägg till en Azure-fjärrdatabas till din lokala Git-databas. 

```bash
git remote add azure <paste_copied_url_here> 
```

Skicka till Azure remote distribuerar Node.js-program. Du uppmanas att ange lösenordet du angav tidigare. Det behövs för att skapa distributionsanvändaren. 

```bash
git push azure master
```

Under distributionen kommunicerar förloppet med Git i Azure App Service.

```bash
Counting objects: 5, done.
Delta compression using up to 4 threads.
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
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
``` 

Det kan hända att distributionen körs [Gulp](http://gulpjs.com/) när `npm install`. App Service körs inte Gulp eller Grunt uppgifter under distributionen, så att den här lagringsplatsen exempel har två ytterligare filer i rotkatalogen för att den: 

- _.Deployment_ -den här filen talar om App Service för att köra `bash deploy.sh` som anpassade distributions-skriptet.
- _Deploy.SH_ -skriptet anpassad distribution. Om du granska filen ser du att den körs `gulp prod` när `npm install` och `bower install`. 

Du kan använda den här metoden för att lägga till något steg i distributionen Git-baserade. Om du startar om din Azure-webbapp när som helst kör inte App Service dessa automation-aktiviteter.

### <a name="browse-to-the-azure-web-app"></a>Bläddra till Azure-webbappen 

Bläddra till den distribuerade webbappens med hjälp av webbläsaren. 

```bash 
http://<app_name>.azurewebsites.net 
``` 

Klicka på **registrera dig** i den översta menyn och skapa en dummy användare. 

Om du har lyckats och appen automatiskt loggar in på den skapade användaren, har MEAN.js-app i Azure anslutningen till databasen för MongoDB (Cosmos-DB). 

![MEAN.js-app som körs i Azure App Service](./media/app-service-web-tutorial-nodejs-mongodb-app/meanjs-in-azure.png)

Välj **Admin > Hantera artiklar** att lägga till vissa artiklar. 

**Grattis!** Du kör en datadrivna Node.js-app i Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Uppdatera datamodellen och distribuera om

I det här steget kan du ändra den `article` data modell och publicera ändringarna till Azure.

### <a name="update-the-data-model"></a>Uppdatera datamodellen

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

### <a name="update-the-articles-code"></a>Uppdatera artiklar kod

Uppdatera resten av dina `articles` kod för att använda `comment`.

Det finns fem filer som du behöver ändra: server-domänkontrollant och fyra klienten vyer. 

Öppna _modules/articles/server/controllers/articles.server.controller.js_.

I den `update` fungera, lägga till en tilldelning för `article.comment`. Följande kod visar den färdiga `update` funktionen:

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

Ovanför avslutande `</section>` tagg, Lägg till följande rad att visa `comment` tillsammans med resten av artikeldata:

```HTML
<p class="lead" ng-bind="vm.article.comment"></p>
```

Öppna _modules/articles/client/views/list-articles.client.view.html_.

Ovanför avslutande `</a>` tagg, Lägg till följande rad att visa `comment` tillsammans med resten av artikeldata:

```HTML
<p class="list-group-item-text" ng-bind="article.comment"></p>
```

Öppna _modules/articles/client/views/admin/list-articles.client.view.html_.

I den `<div class="list-group">` element och ovanför avslutande `</a>` tagg, Lägg till följande rad att visa `comment` tillsammans med resten av artikeldata:

```HTML
<p class="list-group-item-text" data-ng-bind="article.comment"></p>
```

Öppna _modules/articles/client/views/admin/form-article.client.view.html_.

Hitta de `<div class="form-group">` element som innehåller skickaknappen som ser ut så här:

```HTML
<div class="form-group">
  <button type="submit" class="btn btn-default">{{vm.article._id ? 'Update' : 'Create'}}</button>
</div>
```

Lägga till en annan ovanför den här taggen `<div class="form-group">` element som används för att redigera den `comment` fältet. Din nya element ska se ut så här:

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
> Kom ihåg att din _config/env/production.js_ har återställts och `MONGODB_URI` miljövariabeln anges endast i ditt Azure webbapp och inte på den lokala datorn. Om du tittar på konfigurationsfilen upptäcker du att produktion konfigurationen standardvärden om du vill använda en lokal MongoDB-databas. Detta säkerställer att du inte rör produktionsdata när du testar din kodändringar lokalt.

Navigera till `http://localhost:8443` i en webbläsare och se till att du har loggat in.

Välj **Admin > Hantera artiklar**, lägga till en artikel genom att välja den  **+**  knappen.

Du ser den nya `Comment` textruta nu.

![Tillagda kommentar fältet till artiklar](./media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field.png)

Stoppa Node.js genom att skriva till terminalen och `Ctrl+C`. 

### <a name="publish-changes-to-azure"></a>Publicera ändringar i Azure

Genomför ändringarna i Git och skicka koden ändringar till Azure.

```bash
git commit -am "added article comment"
git push azure master
```

En gång i `git push` är klar, gå till Azure webbapp och prova att använda de nya funktionerna.

![Ändringar i modellen och databasen publiceras på Azure](media/app-service-web-tutorial-nodejs-mongodb-app/added-comment-field-published.png)

Om du lagt till alla artiklar tidigare kan kan du fortfarande se dem. Befintliga data i Cosmos-databasen är inte förlorade. Dessutom dataschemat uppdateringarna och lämnar företaget dina befintliga data.

## <a name="stream-diagnostic-logs"></a>Dataströmmen diagnostikloggar 

Du kan hämta loggarna för konsolen skickas till terminalen när Node.js-programmet körs i Azure App Service. På så sätt kan du få samma diagnostiska meddelanden för att felsöka programfel.

Starta loggen strömning med den [az webapp loggen pilslut](/cli/azure/webapp/log#tail) kommando.

```azurecli-interactive
az webapp log tail --name <app_name> --resource-group myResourceGroup
``` 

Uppdatera din Azure-webbapp i webbläsaren att hämta vissa webbtrafik när loggen streaming har startats. Nu kan du se loggarna för konsolen skickas till terminalen.

Stopploggen strömning när som helst genom att skriva `Ctrl+C`. 

## <a name="manage-your-azure-web-app"></a>Hantera Azure-webbapp

Gå till den [Azure-portalen](https://portal.azure.com) att se att webbappen som du skapade.

Klicka på **Apptjänster** på menyn till vänster och klicka sedan på namnet på din Azure-webbapp.

![Navigera till webbappen på Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/access-portal.png)

Som standard visas på portalen ditt webbprogram **översikt** sidan. På den här sidan får du en översikt över hur det går för appen. Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. Flikar till vänster på sidan Visa sidorna annan konfiguration som du kan öppna.

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-nodejs-mongodb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Nästa steg

Vad du lärt dig:

> [!div class="checklist"]
> * Skapa en MongoDB-databas i Azure
> * Ansluta en Node.js-app till MongoDB
> * Distribuera appen till Azure
> * Uppdatera datamodellen och distribuera appen
> * Dataströmmen loggas från Azure till terminalen
> * Hantera appen i Azure-portalen

Gå vidare till nästa kurs att lära dig hur du mappar en anpassad DNS-namn till ditt webbprogram.

> [!div class="nextstepaction"] 
> [Mappa ett befintligt anpassat DNS-namn till Azure Web Apps](app-service-web-tutorial-custom-domain.md)
