---
title: aaaBuild en Docker Python och PostgreSQL-webbapp i Azure | Microsoft Docs
description: "Lär dig hur tooget en Docker Python app arbetar i Azure med anslutning tooa PostgreSQL databasen."
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
editor: 
ms.assetid: 2bada123-ef18-44e5-be71-e16323b20466
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: e594ef9ec8c04ef2bf725e5f998691f3fb8cf815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Skapa en Docker Python och PostgreSQL-webbapp i Azure

Azure Web Apps ger en mycket skalbar, automatisk uppdatering värdtjänst. Den här kursen visar hur toocreate en grundläggande Docker Python webbapp i Azure. Du måste ansluta den här appen tooa PostgreSQL-databas. När du är klar har du en Python Flask-program som körs i en dockerbehållare på [Azure App Service Web Apps](app-service-web-overview.md).

![Docker Python Flask-app i Azure Apptjänst](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

Du kan följa macOS hello stegen nedan. Instruktioner för Linux och Windows är hello samma i de flesta fall, men hello skillnaderna beskrivs inte i den här självstudiekursen.
 
## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

1. [Installera Git](https://git-scm.com/)
1. [Installera Python](https://www.python.org/downloads/)
1. [Installera och köra PostgreSQL](https://www.postgresql.org/download/)
1. [Installera Docker Community Edition](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testa installationen av lokala PostgreSQL och skapa en databas

Öppna hello terminalfönster och kör `psql postgres` tooconnect tooyour lokala PostgreSQL-servern.

```bash
psql postgres
```

Om anslutningen lyckas körs PostgreSQL-databas. Om inte, kontrollera att den lokala PostgresQL-databasen har startats genom att följa stegen hello på [hämtas - PostgreSQL Core Distribution](https://www.postgresql.org/download/).

Skapa en databas som heter *eventregistration* och ställa in en separat databasanvändare med namnet *manager* med lösenord *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```
Typen *\q* tooexit hello PostgreSQL-klienten. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Skapa lokala Python Flask-program

I det här steget kan du ställa in hello lokalt Python Flask-projekt.

### <a name="clone-hello-sample-application"></a>Klona hello exempelprogrammet

Öppna hello terminalfönster, och `CD` tooa arbetskatalogen.  

Kör hello följande kommandon tooclone hello exempel databasen och gå toohello *0.1 initialapp* versionen.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Det här exemplet databasen innehåller en [Flask](http://flask.pocoo.org/) program. 

### <a name="run-hello-application"></a>Kör programmet hello

> [!NOTE] 
> I ett senare steg förenklar processen genom att skapa en Docker-behållare toouse med hello produktionsdatabas.

Installera hello krävs paket och starta programmet hello.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

När hello app har lästs in helt, se något liknande toohello följande meddelande:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

Navigera toohttp://127.0.0.1:5000 i en webbläsare. Klicka på **registrera!** och skapa en testanvändare.

![Python Flask-program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

Hej Flask exempelprogrammet lagrar användardata i hello-databasen. Om du lyckas vid registrering av en användare kan skriver din app data toohello lokala PostgreSQL-databas.

toostop hello Flask server när som helst, skriva Ctrl + C i hello terminal. 

## <a name="create-a-production-postgresql-database"></a>Skapa en PostgreSQL-databas för produktion

I det här steget skapar du en PostgreSQL-databas i Azure. När din app är distribuerade tooAzure, använder den här databasen i molnet.

### <a name="log-in-tooazure"></a>Logga in tooAzure

Du kan nu gå toouse hello Azure CLI 2.0 toocreate hello resurser som krävs toohost Python programmet i Azure App Service.  Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar. 

```azurecli
az login 
``` 
   
### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create). 

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

hello följande exempel skapas en resursgrupp i hello västra USA region:

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

Använd hello [az apptjänst lista-platser](/cli/azure/appservice#list-locations) Azure CLI kommandot toolist tillgängliga platser.

### <a name="create-an-azure-database-for-postgresql-server"></a>Skapa en Azure Database för PostgreSQL-server

Skapa en PostgreSQL-server med hello [az postgres servern skapa](/cli/azure/documentdb#create) kommando.

I hello följande kommando, ersätta ett unikt servernamn för hello  *\<postgresql_name >* platshållare och ett användarnamn för hello  *\<admin_username >* platshållare . hello servernamnet används som en del av din PostgreSQL-slutpunkt (`https://<postgresql_name>.postgres.database.azure.com`), så hello namn måste toobe unikt över alla servrar i Azure. hello användarnamnet är för hello inledande databasen admin-användarkonto. Du kan ange toopick ett lösenord för den här användaren.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

När hello Azure-databas för PostgreSQL server skapas visar hello Azure CLI information liknande toohello följande exempel:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-hello-azure-database-for-postgresql-server"></a>Skapa en brandväggsregel för hello Azure-databas för PostgreSQL-server

Kör hello följande Azure CLI kommandot tooallow access toohello-databas från alla IP-adresser.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

hello Azure CLI bekräftar skapa hello brandväggsregel med utdata liknande toohello följande exempel:

```json
{
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

## <a name="connect-your-python-flask-application-toohello-database"></a>Ansluta toohello för Python Flask programdatabasen

Anslut din Python Flask exempel programmet toohello Azure-databas för PostgreSQL-server som du skapade i det här steget.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Skapa en tom databas och skapa en ny databasanvändare för programmet

Skapa en databasanvändare med tooa enda endast access-databas. Du använder dessa autentiseringsuppgifter tooavoid ger hello programserver fullständig åtkomst toohello.

Ansluta toohello databasen (uppmanas du admin-lösenordet).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Skapa hello databasen och användaren från hello PostgreSQL CLI.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```

Typen *\q* tooexit hello PostgreSQL-klienten.

### <a name="test-hello-application-locally-against-hello-azure-postgresql-database"></a>Testa hello program lokalt mot hello Azure PostgreSQL-databas 

Gå tillbaka nu toohello *app* för hello klonas Github-lagringsplatsen, kan du köra hello Python Flask program genom att uppdatera hello miljövariabler för databasen.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

När hello app har lästs in helt, se något liknande toohello följande meddelande:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

Navigera toohttp://127.0.0.1:5000 i en webbläsare. Klicka på **registrera!** och skapa en test-registrering. Du nu skriver data toohello databas i Azure.

![Python Flask-program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-hello-application-from-a-docker-container"></a>Hello-program som körs från en Docker-behållare

Skapa hello Docker behållaren image.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker visas en bekräftelse hello behållaren it har skapats.

```bash
Successfully built 7548f983a36b
```

Lägg till miljön variabler tooan miljö variabeln databasfilen *db.env*. hello app ansluter toohello PostgreSQL-produktionsdatabasen i Azure.

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

Kör hello-app från hello dockerbehållare. hello följande kommando anger hello miljö av filer och mappar hello Flask port 5000 toolocal standardporten 5000.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

hello-utdata är liknande toowhat som du såg tidigare. Hello inledande Databasmigrering inte längre behöver toobe utförs och ignoreras därför.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C tooquit)
```

hello-databasen innehåller redan hello-registrering som du skapade tidigare.

![Docker behållaren-baserade Python Flask program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-hello-docker-container-tooa-container-registry"></a>Överför hello Docker behållare tooa behållare registret

I det här steget kan överföra du hello Docker behållare tooa behållare registret. Du använder Azure-behållare registret, men du kan också använda andra populära dem som Docker Hub.

### <a name="create-an-azure-container-registry"></a>Skapa ett Azure Container Registry

Ersätt i hello efter kommandot toocreate en behållare registret  *\<registry_name >* med en unik Azure-behållaren registret valfritt namn.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Resultat
```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-hello-registry-credentials-for-pushing-and-pulling-docker-images"></a>Hämta hello registret autentiseringsuppgifter för push-installation och använda Docker-avbildningar

tooshow registret autentiseringsuppgifter, aktivera adminläge först.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Du ser två lösenord. Anteckna hello namn och hello första lösenord.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-tooazure-container-registry"></a>Ladda upp din Docker behållare tooAzure behållare registret

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-hello-docker-python-flask-application-tooazure"></a>Distribuera hello Docker Python Flask programmet tooAzure

I det här steget kan distribuera du din Docker behållaren-baserade Python Flask programmet tooAzure Apptjänst.

### <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

Skapa en apptjänstplan med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) kommando. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

hello följande exempel skapas en Linux-baserade programtjänstplan med namnet *myAppServicePlan* med hello S1 prisnivån:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

När hello programtjänstplanen har skapats, visar hello Azure CLI information liknande toohello följande exempel:

```json 
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "linux",
  "location": "West US",
  "maximumNumberOfWorkers": 10,
  "name": "myAppServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "S",
    "locations": null,
    "name": "S1",
    "size": "S1",
    "skuCapacity": null,
    "tier": "Standard"
  },
  "status": "Ready",
  "subscription": "00000000-0000-0000-0000-000000000000",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
``` 

### <a name="create-a-web-app"></a>Skapa en webbapp

Skapa en webbapp i hello *myAppServicePlan* App Service-plan med hello [az webapp skapa](/cli/azure/webapp#create) kommando. 

hello web app ger du en värd utrymme toodeploy koden och ger en URL för du tooview hello distribuerat program. Använda toocreate hello webbapp. 

Följande kommando, Ersätt hello i hello  *\<appnamn >* med ett unikt appnamn. Det här namnet är en del av hello standard-URL för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure App Service. 

```azurecli
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

### <a name="configure-hello-database-environment-variables"></a>Konfigurera hello miljövariabler för databasen

Tidigare i självstudierna hello definierat du miljö variabler tooconnect tooyour PostgreSQL-databas.

I App Service som du anger miljövariabler som _appinställningar_ med hjälp av hello [az webapp appsettings konfigurationsuppsättning](/cli/azure/webapp/config#set) kommando. 

hello anger följande exempel hello anslutningsinformation för databasen som app-inställningar. Använder också hello *PORT* variabeln toomap PORT 5000 från Dockerbehållare tooreceive HTTP-trafik på PORT 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Konfigurera Docker behållare distribution 

Apptjänst kan automatiskt hämta och kör en dockerbehållare.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Starta om hello app när du uppdaterar hello dockerbehållare eller ändra hello inställningar. Starta om säkerställer att alla inställningar gäller och hello senaste behållaren hämtas från hello registret.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-toohello-azure-web-app"></a>Bläddra toohello Azure-webbapp 

Bläddra toohello distribuera webbprogram med hjälp av webbläsaren. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> hello webbprogrammet tar längre tooload eftersom hello behållaren har toobe laddats ned och startats när konfigurationen för hello behållaren har ändrats.

Du kan se tidigare registrerad gäster som sparats toohello Azure produktionsdatabasen i hello föregående steg.

![Docker behållaren-baserade Python Flask program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Grattis!** Du kör en Docker behållare-baserade Python Flask-app i Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Uppdatera datamodellen och distribuera om

I det här steget kan du lägga till hello antalet deltagare tooeach Händelseregistrering genom att uppdatera hello gäst modellen.

Kolla in hello *0,2 migrering* version med hello följande git-kommando:

```bash
git checkout tags/0.2-migration
```

Den här versionen har redan gjorts hello nödvändiga ändringar tooviews, domänkontrollanter och modell. Den innehåller också en Databasmigrering som skapats *alembic* (`flask db migrate`). Du kan se alla ändringar som gjorts via hello följande git-kommando:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>Testa ändringarna lokalt

Kör följande kommandon tootest hello ändringarna lokalt genom kör hello flask-server.

Mac / Linux:
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Navigera toohttp://127.0.0.1:5000 i din webbläsare tooview hello ändras. Skapa en test-registrering.

![Docker behållaren-baserade Python Flask program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-tooazure"></a>Publicera ändringar tooAzure

Skapa hello ny docker-avbildning, push-installera den toohello behållare registret och starta om hello app.

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Navigera tooyour Azure webbapp och prova att använda nya funktioner för hello igen. Skapa en annan Händelseregistrering.

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask-app i Azure Apptjänst](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Hantera Azure-webbapp

Gå toohello [Azure-portalen](https://portal.azure.com) toosee hello webbprogram som du skapade.

Hello vänstra menyn klickar du på **Apptjänster**, klicka på hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-docker-python-postgresql-app/app-resource.png)

Som standard visar hello portal ditt webbprogram **översikt** sidan. På den här sidan får du en översikt över hur det går för appen. Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort. hello flikar på hello vänster hello sidan visar hello annan konfigurationssidor som du kan öppna.

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Nästa steg

I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn tooyour webbprogram.

> [!div class="nextstepaction"] 
> [Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](app-service-web-tutorial-custom-domain.md)
