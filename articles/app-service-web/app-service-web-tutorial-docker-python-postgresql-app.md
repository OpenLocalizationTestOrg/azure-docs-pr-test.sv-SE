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
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a><span data-ttu-id="c30b3-103">Skapa en Docker Python och PostgreSQL-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="c30b3-103">Build a Docker Python and PostgreSQL web app in Azure</span></span>

<span data-ttu-id="c30b3-104">Azure Web Apps ger en mycket skalbar, automatisk uppdatering värdtjänst.</span><span class="sxs-lookup"><span data-stu-id="c30b3-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="c30b3-105">Den här kursen visar hur toocreate en grundläggande Docker Python webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="c30b3-105">This tutorial shows how toocreate a basic Docker Python web app in Azure.</span></span> <span data-ttu-id="c30b3-106">Du måste ansluta den här appen tooa PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c30b3-106">You'll connect this app tooa PostgreSQL database.</span></span> <span data-ttu-id="c30b3-107">När du är klar har du en Python Flask-program som körs i en dockerbehållare på [Azure App Service Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c30b3-107">When you're done, you'll have a Python Flask application running within a Docker container on [Azure App Service Web Apps](app-service-web-overview.md).</span></span>

![Docker Python Flask-app i Azure Apptjänst](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

<span data-ttu-id="c30b3-109">Du kan följa macOS hello stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="c30b3-109">You can follow hello steps below on macOS.</span></span> <span data-ttu-id="c30b3-110">Instruktioner för Linux och Windows är hello samma i de flesta fall, men hello skillnaderna beskrivs inte i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c30b3-110">Linux and Windows instructions are hello same in most cases, but hello differences are not detailed in this tutorial.</span></span>
 
## <a name="prerequisites"></a><span data-ttu-id="c30b3-111">Krav</span><span class="sxs-lookup"><span data-stu-id="c30b3-111">Prerequisites</span></span>

<span data-ttu-id="c30b3-112">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="c30b3-112">toocomplete this tutorial:</span></span>

1. [<span data-ttu-id="c30b3-113">Installera Git</span><span class="sxs-lookup"><span data-stu-id="c30b3-113">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="c30b3-114">Installera Python</span><span class="sxs-lookup"><span data-stu-id="c30b3-114">Install Python</span></span>](https://www.python.org/downloads/)
1. [<span data-ttu-id="c30b3-115">Installera och köra PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="c30b3-115">Install and run PostgreSQL</span></span>](https://www.postgresql.org/download/)
1. [<span data-ttu-id="c30b3-116">Installera Docker Community Edition</span><span class="sxs-lookup"><span data-stu-id="c30b3-116">Install Docker Community Edition</span></span>](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c30b3-117">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c30b3-117">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c30b3-118">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="c30b3-118">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="c30b3-119">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c30b3-119">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="test-local-postgresql-installation-and-create-a-database"></a><span data-ttu-id="c30b3-120">Testa installationen av lokala PostgreSQL och skapa en databas</span><span class="sxs-lookup"><span data-stu-id="c30b3-120">Test local PostgreSQL installation and create a database</span></span>

<span data-ttu-id="c30b3-121">Öppna hello terminalfönster och kör `psql postgres` tooconnect tooyour lokala PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="c30b3-121">Open hello terminal window and run `psql postgres` tooconnect tooyour local PostgreSQL server.</span></span>

```bash
psql postgres
```

<span data-ttu-id="c30b3-122">Om anslutningen lyckas körs PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c30b3-122">If your connection is successful, your PostgreSQL database is running.</span></span> <span data-ttu-id="c30b3-123">Om inte, kontrollera att den lokala PostgresQL-databasen har startats genom att följa stegen hello på [hämtas - PostgreSQL Core Distribution](https://www.postgresql.org/download/).</span><span class="sxs-lookup"><span data-stu-id="c30b3-123">If not, make sure that your local PostgresQL database is started by following hello steps at [Downloads - PostgreSQL Core Distribution](https://www.postgresql.org/download/).</span></span>

<span data-ttu-id="c30b3-124">Skapa en databas som heter *eventregistration* och ställa in en separat databasanvändare med namnet *manager* med lösenord *supersecretpass*.</span><span class="sxs-lookup"><span data-stu-id="c30b3-124">Create a database called *eventregistration* and set up a separate database user named *manager* with password *supersecretpass*.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```
<span data-ttu-id="c30b3-125">Typen *\q* tooexit hello PostgreSQL-klienten.</span><span class="sxs-lookup"><span data-stu-id="c30b3-125">Type *\q* tooexit hello PostgreSQL client.</span></span> 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a><span data-ttu-id="c30b3-126">Skapa lokala Python Flask-program</span><span class="sxs-lookup"><span data-stu-id="c30b3-126">Create local Python Flask application</span></span>

<span data-ttu-id="c30b3-127">I det här steget kan du ställa in hello lokalt Python Flask-projekt.</span><span class="sxs-lookup"><span data-stu-id="c30b3-127">In this step, you set up hello local Python Flask project.</span></span>

### <a name="clone-hello-sample-application"></a><span data-ttu-id="c30b3-128">Klona hello exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="c30b3-128">Clone hello sample application</span></span>

<span data-ttu-id="c30b3-129">Öppna hello terminalfönster, och `CD` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="c30b3-129">Open hello terminal window, and `CD` tooa working directory.</span></span>  

<span data-ttu-id="c30b3-130">Kör hello följande kommandon tooclone hello exempel databasen och gå toohello *0.1 initialapp* versionen.</span><span class="sxs-lookup"><span data-stu-id="c30b3-130">Run hello following commands tooclone hello sample repository and go toohello *0.1-initialapp* release.</span></span>

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

<span data-ttu-id="c30b3-131">Det här exemplet databasen innehåller en [Flask](http://flask.pocoo.org/) program.</span><span class="sxs-lookup"><span data-stu-id="c30b3-131">This sample repository contains a [Flask](http://flask.pocoo.org/) application.</span></span> 

### <a name="run-hello-application"></a><span data-ttu-id="c30b3-132">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="c30b3-132">Run hello application</span></span>

> [!NOTE] 
> <span data-ttu-id="c30b3-133">I ett senare steg förenklar processen genom att skapa en Docker-behållare toouse med hello produktionsdatabas.</span><span class="sxs-lookup"><span data-stu-id="c30b3-133">In a later step you simplify this process by building a Docker container toouse with hello production database.</span></span>

<span data-ttu-id="c30b3-134">Installera hello krävs paket och starta programmet hello.</span><span class="sxs-lookup"><span data-stu-id="c30b3-134">Install hello required packages and start hello application.</span></span>

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="c30b3-135">När hello app har lästs in helt, se något liknande toohello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="c30b3-135">When hello app is fully loaded, you see something similar toohello following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

<span data-ttu-id="c30b3-136">Navigera toohttp://127.0.0.1:5000 i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c30b3-136">Navigate toohttp://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="c30b3-137">Klicka på **registrera!**</span><span class="sxs-lookup"><span data-stu-id="c30b3-137">Click **Register!**</span></span> <span data-ttu-id="c30b3-138">och skapa en testanvändare.</span><span class="sxs-lookup"><span data-stu-id="c30b3-138">and create a test user.</span></span>

![Python Flask-program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

<span data-ttu-id="c30b3-140">Hej Flask exempelprogrammet lagrar användardata i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="c30b3-140">hello Flask sample application stores user data in hello database.</span></span> <span data-ttu-id="c30b3-141">Om du lyckas vid registrering av en användare kan skriver din app data toohello lokala PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c30b3-141">If you are successful at registering a user, your app is writing data toohello local PostgreSQL database.</span></span>

<span data-ttu-id="c30b3-142">toostop hello Flask server när som helst, skriva Ctrl + C i hello terminal.</span><span class="sxs-lookup"><span data-stu-id="c30b3-142">toostop hello Flask server at anytime, type Ctrl+C in hello terminal.</span></span> 

## <a name="create-a-production-postgresql-database"></a><span data-ttu-id="c30b3-143">Skapa en PostgreSQL-databas för produktion</span><span class="sxs-lookup"><span data-stu-id="c30b3-143">Create a production PostgreSQL database</span></span>

<span data-ttu-id="c30b3-144">I det här steget skapar du en PostgreSQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="c30b3-144">In this step, you create a PostgreSQL database in Azure.</span></span> <span data-ttu-id="c30b3-145">När din app är distribuerade tooAzure, använder den här databasen i molnet.</span><span class="sxs-lookup"><span data-stu-id="c30b3-145">When your app is deployed tooAzure, it will use this cloud database.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="c30b3-146">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="c30b3-146">Log in tooAzure</span></span>

<span data-ttu-id="c30b3-147">Du kan nu gå toouse hello Azure CLI 2.0 toocreate hello resurser som krävs toohost Python programmet i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c30b3-147">You are now going toouse hello Azure CLI 2.0 toocreate hello resources needed toohost your Python application in Azure App Service.</span></span>  <span data-ttu-id="c30b3-148">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="c30b3-148">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> 

```azurecli
az login 
``` 
   
### <a name="create-a-resource-group"></a><span data-ttu-id="c30b3-149">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c30b3-149">Create a resource group</span></span>

<span data-ttu-id="c30b3-150">Skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="c30b3-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with hello [az group create](/cli/azure/group#create).</span></span> 

[!INCLUDE [Resource group intro](../../includes/resource-group.md)]

<span data-ttu-id="c30b3-151">hello följande exempel skapas en resursgrupp i hello västra USA region:</span><span class="sxs-lookup"><span data-stu-id="c30b3-151">hello following example creates a resource group in hello West US region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

<span data-ttu-id="c30b3-152">Använd hello [az apptjänst lista-platser](/cli/azure/appservice#list-locations) Azure CLI kommandot toolist tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="c30b3-152">Use hello [az appservice list-locations](/cli/azure/appservice#list-locations) Azure CLI command toolist available locations.</span></span>

### <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="c30b3-153">Skapa en Azure Database för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="c30b3-153">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="c30b3-154">Skapa en PostgreSQL-server med hello [az postgres servern skapa](/cli/azure/documentdb#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="c30b3-154">Create a PostgreSQL server with hello [az postgres server create](/cli/azure/documentdb#create) command.</span></span>

<span data-ttu-id="c30b3-155">I hello följande kommando, ersätta ett unikt servernamn för hello  *\<postgresql_name >* platshållare och ett användarnamn för hello  *\<admin_username >* platshållare .</span><span class="sxs-lookup"><span data-stu-id="c30b3-155">In hello following command, substitute a unique server name for hello *\<postgresql_name>* placeholder and a user name for hello *\<admin_username>* placeholder.</span></span> <span data-ttu-id="c30b3-156">hello servernamnet används som en del av din PostgreSQL-slutpunkt (`https://<postgresql_name>.postgres.database.azure.com`), så hello namn måste toobe unikt över alla servrar i Azure.</span><span class="sxs-lookup"><span data-stu-id="c30b3-156">hello server name is used as part of your PostgreSQL endpoint (`https://<postgresql_name>.postgres.database.azure.com`), so hello name needs toobe unique across all servers in Azure.</span></span> <span data-ttu-id="c30b3-157">hello användarnamnet är för hello inledande databasen admin-användarkonto.</span><span class="sxs-lookup"><span data-stu-id="c30b3-157">hello user name is for hello initial database admin user account.</span></span> <span data-ttu-id="c30b3-158">Du kan ange toopick ett lösenord för den här användaren.</span><span class="sxs-lookup"><span data-stu-id="c30b3-158">You are prompted toopick a password for this user.</span></span>

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

<span data-ttu-id="c30b3-159">När hello Azure-databas för PostgreSQL server skapas visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c30b3-159">When hello Azure Database for PostgreSQL server is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="create-a-firewall-rule-for-hello-azure-database-for-postgresql-server"></a><span data-ttu-id="c30b3-160">Skapa en brandväggsregel för hello Azure-databas för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="c30b3-160">Create a firewall rule for hello Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="c30b3-161">Kör hello följande Azure CLI kommandot tooallow access toohello-databas från alla IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="c30b3-161">Run hello following Azure CLI command tooallow access toohello database from all IP addresses.</span></span>

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

<span data-ttu-id="c30b3-162">hello Azure CLI bekräftar skapa hello brandväggsregel med utdata liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c30b3-162">hello Azure CLI confirms hello firewall rule creation with output similar toohello following example:</span></span>

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

## <a name="connect-your-python-flask-application-toohello-database"></a><span data-ttu-id="c30b3-163">Ansluta toohello för Python Flask programdatabasen</span><span class="sxs-lookup"><span data-stu-id="c30b3-163">Connect your Python Flask application toohello database</span></span>

<span data-ttu-id="c30b3-164">Anslut din Python Flask exempel programmet toohello Azure-databas för PostgreSQL-server som du skapade i det här steget.</span><span class="sxs-lookup"><span data-stu-id="c30b3-164">In this step, you connect your Python Flask sample application toohello Azure Database for PostgreSQL server you created.</span></span>

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a><span data-ttu-id="c30b3-165">Skapa en tom databas och skapa en ny databasanvändare för programmet</span><span class="sxs-lookup"><span data-stu-id="c30b3-165">Create an empty database and set up a new database application user</span></span>

<span data-ttu-id="c30b3-166">Skapa en databasanvändare med tooa enda endast access-databas.</span><span class="sxs-lookup"><span data-stu-id="c30b3-166">Create a database user with access tooa single database only.</span></span> <span data-ttu-id="c30b3-167">Du använder dessa autentiseringsuppgifter tooavoid ger hello programserver fullständig åtkomst toohello.</span><span class="sxs-lookup"><span data-stu-id="c30b3-167">You'll use these credentials tooavoid giving hello application full access toohello server.</span></span>

<span data-ttu-id="c30b3-168">Ansluta toohello databasen (uppmanas du admin-lösenordet).</span><span class="sxs-lookup"><span data-stu-id="c30b3-168">Connect toohello database (you're prompted for your admin password).</span></span>

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

<span data-ttu-id="c30b3-169">Skapa hello databasen och användaren från hello PostgreSQL CLI.</span><span class="sxs-lookup"><span data-stu-id="c30b3-169">Create hello database and user from hello PostgreSQL CLI.</span></span>

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration toomanager;
```

<span data-ttu-id="c30b3-170">Typen *\q* tooexit hello PostgreSQL-klienten.</span><span class="sxs-lookup"><span data-stu-id="c30b3-170">Type *\q* tooexit hello PostgreSQL client.</span></span>

### <a name="test-hello-application-locally-against-hello-azure-postgresql-database"></a><span data-ttu-id="c30b3-171">Testa hello program lokalt mot hello Azure PostgreSQL-databas</span><span class="sxs-lookup"><span data-stu-id="c30b3-171">Test hello application locally against hello Azure PostgreSQL database</span></span> 

<span data-ttu-id="c30b3-172">Gå tillbaka nu toohello *app* för hello klonas Github-lagringsplatsen, kan du köra hello Python Flask program genom att uppdatera hello miljövariabler för databasen.</span><span class="sxs-lookup"><span data-stu-id="c30b3-172">Going back now toohello *app* folder of hello cloned Github repository, you can run hello Python Flask application by updating hello database environment variables.</span></span>

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="c30b3-173">När hello app har lästs in helt, se något liknande toohello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="c30b3-173">When hello app is fully loaded, you see something similar toohello following message:</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C tooquit)
```

<span data-ttu-id="c30b3-174">Navigera toohttp://127.0.0.1:5000 i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c30b3-174">Navigate toohttp://127.0.0.1:5000 in a browser.</span></span> <span data-ttu-id="c30b3-175">Klicka på **registrera!**</span><span class="sxs-lookup"><span data-stu-id="c30b3-175">Click **Register!**</span></span> <span data-ttu-id="c30b3-176">och skapa en test-registrering.</span><span class="sxs-lookup"><span data-stu-id="c30b3-176">and create a test registration.</span></span> <span data-ttu-id="c30b3-177">Du nu skriver data toohello databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="c30b3-177">You are now writing data toohello database in Azure.</span></span>

![Python Flask-program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-hello-application-from-a-docker-container"></a><span data-ttu-id="c30b3-179">Hello-program som körs från en Docker-behållare</span><span class="sxs-lookup"><span data-stu-id="c30b3-179">Running hello application from a Docker Container</span></span>

<span data-ttu-id="c30b3-180">Skapa hello Docker behållaren image.</span><span class="sxs-lookup"><span data-stu-id="c30b3-180">Build hello Docker container image.</span></span>

```bash
cd ..
docker build -t flask-postgresql-sample .
```

<span data-ttu-id="c30b3-181">Docker visas en bekräftelse hello behållaren it har skapats.</span><span class="sxs-lookup"><span data-stu-id="c30b3-181">Docker displays a confirmation that it successfully created hello container.</span></span>

```bash
Successfully built 7548f983a36b
```

<span data-ttu-id="c30b3-182">Lägg till miljön variabler tooan miljö variabeln databasfilen *db.env*.</span><span class="sxs-lookup"><span data-stu-id="c30b3-182">Add database environment variables tooan environment variable file *db.env*.</span></span> <span data-ttu-id="c30b3-183">hello app ansluter toohello PostgreSQL-produktionsdatabasen i Azure.</span><span class="sxs-lookup"><span data-stu-id="c30b3-183">hello app will connect toohello PostgreSQL production database in Azure.</span></span>

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

<span data-ttu-id="c30b3-184">Kör hello-app från hello dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="c30b3-184">Run hello app from within hello Docker container.</span></span> <span data-ttu-id="c30b3-185">hello följande kommando anger hello miljö av filer och mappar hello Flask port 5000 toolocal standardporten 5000.</span><span class="sxs-lookup"><span data-stu-id="c30b3-185">hello following command specifies hello environment variable file and maps hello default Flask port 5000 toolocal port 5000.</span></span>

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

<span data-ttu-id="c30b3-186">hello-utdata är liknande toowhat som du såg tidigare.</span><span class="sxs-lookup"><span data-stu-id="c30b3-186">hello output is similar toowhat you saw earlier.</span></span> <span data-ttu-id="c30b3-187">Hello inledande Databasmigrering inte längre behöver toobe utförs och ignoreras därför.</span><span class="sxs-lookup"><span data-stu-id="c30b3-187">However, hello initial database migration no longer needs toobe performed and therefore is skipped.</span></span>

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C tooquit)
```

<span data-ttu-id="c30b3-188">hello-databasen innehåller redan hello-registrering som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c30b3-188">hello database already contains hello registration you created previously.</span></span>

![Docker behållaren-baserade Python Flask program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-hello-docker-container-tooa-container-registry"></a><span data-ttu-id="c30b3-190">Överför hello Docker behållare tooa behållare registret</span><span class="sxs-lookup"><span data-stu-id="c30b3-190">Upload hello Docker container tooa container registry</span></span>

<span data-ttu-id="c30b3-191">I det här steget kan överföra du hello Docker behållare tooa behållare registret.</span><span class="sxs-lookup"><span data-stu-id="c30b3-191">In this step, you upload hello Docker container tooa container registry.</span></span> <span data-ttu-id="c30b3-192">Du använder Azure-behållare registret, men du kan också använda andra populära dem som Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="c30b3-192">You'll use Azure Container Registry, but you could also use other popular ones such as Docker Hub.</span></span>

### <a name="create-an-azure-container-registry"></a><span data-ttu-id="c30b3-193">Skapa ett Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="c30b3-193">Create an Azure Container Registry</span></span>

<span data-ttu-id="c30b3-194">Ersätt i hello efter kommandot toocreate en behållare registret  *\<registry_name >* med en unik Azure-behållaren registret valfritt namn.</span><span class="sxs-lookup"><span data-stu-id="c30b3-194">In hello following command toocreate a container registry replace *\<registry_name>* with a unique Azure container registry name of your choice.</span></span>

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

<span data-ttu-id="c30b3-195">Resultat</span><span class="sxs-lookup"><span data-stu-id="c30b3-195">Output</span></span>
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

### <a name="retrieve-hello-registry-credentials-for-pushing-and-pulling-docker-images"></a><span data-ttu-id="c30b3-196">Hämta hello registret autentiseringsuppgifter för push-installation och använda Docker-avbildningar</span><span class="sxs-lookup"><span data-stu-id="c30b3-196">Retrieve hello registry credentials for pushing and pulling Docker images</span></span>

<span data-ttu-id="c30b3-197">tooshow registret autentiseringsuppgifter, aktivera adminläge först.</span><span class="sxs-lookup"><span data-stu-id="c30b3-197">tooshow registry credentials, enable admin mode first.</span></span>

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

<span data-ttu-id="c30b3-198">Du ser två lösenord.</span><span class="sxs-lookup"><span data-stu-id="c30b3-198">You see two passwords.</span></span> <span data-ttu-id="c30b3-199">Anteckna hello namn och hello första lösenord.</span><span class="sxs-lookup"><span data-stu-id="c30b3-199">Make note of hello user name and hello first password.</span></span>

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

### <a name="upload-your-docker-container-tooazure-container-registry"></a><span data-ttu-id="c30b3-200">Ladda upp din Docker behållare tooAzure behållare registret</span><span class="sxs-lookup"><span data-stu-id="c30b3-200">Upload your Docker container tooAzure Container Registry</span></span>

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-hello-docker-python-flask-application-tooazure"></a><span data-ttu-id="c30b3-201">Distribuera hello Docker Python Flask programmet tooAzure</span><span class="sxs-lookup"><span data-stu-id="c30b3-201">Deploy hello Docker Python Flask application tooAzure</span></span>

<span data-ttu-id="c30b3-202">I det här steget kan distribuera du din Docker behållaren-baserade Python Flask programmet tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="c30b3-202">In this step, you deploy your Docker container-based Python Flask application tooAzure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="c30b3-203">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="c30b3-203">Create an App Service plan</span></span>

<span data-ttu-id="c30b3-204">Skapa en apptjänstplan med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="c30b3-204">Create an App Service plan with hello [az appservice plan create](/cli/azure/appservice/plan#create) command.</span></span> 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="c30b3-205">hello följande exempel skapas en Linux-baserade programtjänstplan med namnet *myAppServicePlan* med hello S1 prisnivån:</span><span class="sxs-lookup"><span data-stu-id="c30b3-205">hello following example creates a Linux-based App Service plan named *myAppServicePlan* using hello S1 pricing tier:</span></span>

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

<span data-ttu-id="c30b3-206">När hello programtjänstplanen har skapats, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c30b3-206">When hello App Service plan is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="create-a-web-app"></a><span data-ttu-id="c30b3-207">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="c30b3-207">Create a web app</span></span>

<span data-ttu-id="c30b3-208">Skapa en webbapp i hello *myAppServicePlan* App Service-plan med hello [az webapp skapa](/cli/azure/webapp#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="c30b3-208">Create a web app in hello *myAppServicePlan* App Service plan with hello [az webapp create](/cli/azure/webapp#create) command.</span></span> 

<span data-ttu-id="c30b3-209">hello web app ger du en värd utrymme toodeploy koden och ger en URL för du tooview hello distribuerat program.</span><span class="sxs-lookup"><span data-stu-id="c30b3-209">hello web app gives you a hosting space toodeploy your code and provides a URL for you tooview hello deployed application.</span></span> <span data-ttu-id="c30b3-210">Använda toocreate hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="c30b3-210">Use  toocreate hello web app.</span></span> 

<span data-ttu-id="c30b3-211">Följande kommando, Ersätt hello i hello  *\<appnamn >* med ett unikt appnamn.</span><span class="sxs-lookup"><span data-stu-id="c30b3-211">In hello following command, replace hello *\<app_name>* placeholder with a unique app name.</span></span> <span data-ttu-id="c30b3-212">Det här namnet är en del av hello standard-URL för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c30b3-212">This name is part of hello default URL for hello web app, so hello name needs toobe unique across all apps in Azure App Service.</span></span> 

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

<span data-ttu-id="c30b3-213">När hello webbprogrammet har skapats, visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c30b3-213">When hello web app has been created, hello Azure CLI shows information similar toohello following example:</span></span> 

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

### <a name="configure-hello-database-environment-variables"></a><span data-ttu-id="c30b3-214">Konfigurera hello miljövariabler för databasen</span><span class="sxs-lookup"><span data-stu-id="c30b3-214">Configure hello database environment variables</span></span>

<span data-ttu-id="c30b3-215">Tidigare i självstudierna hello definierat du miljö variabler tooconnect tooyour PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c30b3-215">Earlier in hello tutorial, you defined environment variables tooconnect tooyour PostgreSQL database.</span></span>

<span data-ttu-id="c30b3-216">I App Service som du anger miljövariabler som _appinställningar_ med hjälp av hello [az webapp appsettings konfigurationsuppsättning](/cli/azure/webapp/config#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="c30b3-216">In App Service, you set environment variables as _app settings_ by using hello [az webapp config appsettings set](/cli/azure/webapp/config#set) command.</span></span> 

<span data-ttu-id="c30b3-217">hello anger följande exempel hello anslutningsinformation för databasen som app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="c30b3-217">hello following example specifies hello database connection details as app settings.</span></span> <span data-ttu-id="c30b3-218">Använder också hello *PORT* variabeln toomap PORT 5000 från Dockerbehållare tooreceive HTTP-trafik på PORT 80.</span><span class="sxs-lookup"><span data-stu-id="c30b3-218">It also uses hello *PORT* variable toomap PORT 5000 from your Docker Container tooreceive HTTP traffic on PORT 80.</span></span>

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a><span data-ttu-id="c30b3-219">Konfigurera Docker behållare distribution</span><span class="sxs-lookup"><span data-stu-id="c30b3-219">Configure Docker container deployment</span></span> 

<span data-ttu-id="c30b3-220">Apptjänst kan automatiskt hämta och kör en dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="c30b3-220">AppService can automatically download and run a Docker container.</span></span>

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

<span data-ttu-id="c30b3-221">Starta om hello app när du uppdaterar hello dockerbehållare eller ändra hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="c30b3-221">Whenever you update hello Docker container or change hello settings, restart hello app.</span></span> <span data-ttu-id="c30b3-222">Starta om säkerställer att alla inställningar gäller och hello senaste behållaren hämtas från hello registret.</span><span class="sxs-lookup"><span data-stu-id="c30b3-222">Restarting ensures that all settings are applied and hello latest container is pulled from hello registry.</span></span>

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="c30b3-223">Bläddra toohello Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="c30b3-223">Browse toohello Azure web app</span></span> 

<span data-ttu-id="c30b3-224">Bläddra toohello distribuera webbprogram med hjälp av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c30b3-224">Browse toohello deployed web app using your web browser.</span></span> 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> <span data-ttu-id="c30b3-225">hello webbprogrammet tar längre tooload eftersom hello behållaren har toobe laddats ned och startats när konfigurationen för hello behållaren har ändrats.</span><span class="sxs-lookup"><span data-stu-id="c30b3-225">hello web app takes longer tooload because hello container has toobe downloaded and started after hello container configuration is changed.</span></span>

<span data-ttu-id="c30b3-226">Du kan se tidigare registrerad gäster som sparats toohello Azure produktionsdatabasen i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="c30b3-226">You see previously registered guests that were saved toohello Azure production database in hello previous step.</span></span>

![Docker behållaren-baserade Python Flask program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-app-deployed.png)

<span data-ttu-id="c30b3-228">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="c30b3-228">**Congratulations!**</span></span> <span data-ttu-id="c30b3-229">Du kör en Docker behållare-baserade Python Flask-app i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c30b3-229">You're running a Docker container-based Python Flask app in Azure App Service.</span></span>

## <a name="update-data-model-and-redeploy"></a><span data-ttu-id="c30b3-230">Uppdatera datamodellen och distribuera om</span><span class="sxs-lookup"><span data-stu-id="c30b3-230">Update data model and redeploy</span></span>

<span data-ttu-id="c30b3-231">I det här steget kan du lägga till hello antalet deltagare tooeach Händelseregistrering genom att uppdatera hello gäst modellen.</span><span class="sxs-lookup"><span data-stu-id="c30b3-231">In this step, you add hello number of attendees tooeach event registration by updating hello Guest model.</span></span>

<span data-ttu-id="c30b3-232">Kolla in hello *0,2 migrering* version med hello följande git-kommando:</span><span class="sxs-lookup"><span data-stu-id="c30b3-232">Check out hello *0.2-migration* release with hello following git command:</span></span>

```bash
git checkout tags/0.2-migration
```

<span data-ttu-id="c30b3-233">Den här versionen har redan gjorts hello nödvändiga ändringar tooviews, domänkontrollanter och modell.</span><span class="sxs-lookup"><span data-stu-id="c30b3-233">This release already made hello necessary changes tooviews, controllers, and model.</span></span> <span data-ttu-id="c30b3-234">Den innehåller också en Databasmigrering som skapats *alembic* (`flask db migrate`).</span><span class="sxs-lookup"><span data-stu-id="c30b3-234">It also includes a database migration generated via *alembic* (`flask db migrate`).</span></span> <span data-ttu-id="c30b3-235">Du kan se alla ändringar som gjorts via hello följande git-kommando:</span><span class="sxs-lookup"><span data-stu-id="c30b3-235">You can see all changes made via hello following git command:</span></span>

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a><span data-ttu-id="c30b3-236">Testa ändringarna lokalt</span><span class="sxs-lookup"><span data-stu-id="c30b3-236">Test your changes locally</span></span>

<span data-ttu-id="c30b3-237">Kör följande kommandon tootest hello ändringarna lokalt genom kör hello flask-server.</span><span class="sxs-lookup"><span data-stu-id="c30b3-237">Run hello following commands tootest your changes locally by running hello flask server.</span></span>

<span data-ttu-id="c30b3-238">Mac / Linux:</span><span class="sxs-lookup"><span data-stu-id="c30b3-238">Mac / Linux:</span></span>
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

<span data-ttu-id="c30b3-239">Navigera toohttp://127.0.0.1:5000 i din webbläsare tooview hello ändras.</span><span class="sxs-lookup"><span data-stu-id="c30b3-239">Navigate toohttp://127.0.0.1:5000 in your browser tooview hello changes.</span></span> <span data-ttu-id="c30b3-240">Skapa en test-registrering.</span><span class="sxs-lookup"><span data-stu-id="c30b3-240">Create a test registration.</span></span>

![Docker behållaren-baserade Python Flask program som körs lokalt](./media/app-service-web-tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-tooazure"></a><span data-ttu-id="c30b3-242">Publicera ändringar tooAzure</span><span class="sxs-lookup"><span data-stu-id="c30b3-242">Publish changes tooAzure</span></span>

<span data-ttu-id="c30b3-243">Skapa hello ny docker-avbildning, push-installera den toohello behållare registret och starta om hello app.</span><span class="sxs-lookup"><span data-stu-id="c30b3-243">Build hello new docker image, push it toohello container registry, and restart hello app.</span></span>

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

<span data-ttu-id="c30b3-244">Navigera tooyour Azure webbapp och prova att använda nya funktioner för hello igen.</span><span class="sxs-lookup"><span data-stu-id="c30b3-244">Navigate tooyour Azure web app and try out hello new functionality again.</span></span> <span data-ttu-id="c30b3-245">Skapa en annan Händelseregistrering.</span><span class="sxs-lookup"><span data-stu-id="c30b3-245">Create another event registration.</span></span>

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask-app i Azure Apptjänst](./media/app-service-web-tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="c30b3-247">Hantera Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="c30b3-247">Manage your Azure web app</span></span>

<span data-ttu-id="c30b3-248">Gå toohello [Azure-portalen](https://portal.azure.com) toosee hello webbprogram som du skapade.</span><span class="sxs-lookup"><span data-stu-id="c30b3-248">Go toohello [Azure portal](https://portal.azure.com) toosee hello web app you created.</span></span>

<span data-ttu-id="c30b3-249">Hello vänstra menyn klickar du på **Apptjänster**, klicka på hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="c30b3-249">From hello left menu, click **App Services**, then click hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-docker-python-postgresql-app/app-resource.png)

<span data-ttu-id="c30b3-251">Som standard visar hello portal ditt webbprogram **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="c30b3-251">By default, hello portal shows your web app's **Overview** page.</span></span> <span data-ttu-id="c30b3-252">På den här sidan får du en översikt över hur det går för appen.</span><span class="sxs-lookup"><span data-stu-id="c30b3-252">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="c30b3-253">Här kan du också utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="c30b3-253">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="c30b3-254">hello flikar på hello vänster hello sidan visar hello annan konfigurationssidor som du kan öppna.</span><span class="sxs-lookup"><span data-stu-id="c30b3-254">hello tabs on hello left side of hello page show hello different configuration pages you can open.</span></span>

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a><span data-ttu-id="c30b3-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c30b3-256">Next steps</span></span>

<span data-ttu-id="c30b3-257">I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c30b3-257">Advance toohello next tutorial toolearn how toomap a custom DNS name tooyour web app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="c30b3-258">Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps</span><span class="sxs-lookup"><span data-stu-id="c30b3-258">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
