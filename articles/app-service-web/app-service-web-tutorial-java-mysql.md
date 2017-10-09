---
title: aaaBuild en Java och MySQL-webbapp i Azure
description: "Lär dig hur tooget en Java-app som ansluter toohello Azure MySQL database-tjänsten fungerar i Azure apptjänst."
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: 0820ee9c2b7bf8fcaa22287c27a7ab848a1c4927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a><span data-ttu-id="08f4a-103">Skapa en Java och MySQL-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="08f4a-103">Build a Java and MySQL web app in Azure</span></span>

<span data-ttu-id="08f4a-104">Den här kursen visar hur toocreate en Java webbapp i Azure och koppla den tooa MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="08f4a-104">This tutorial shows you how toocreate a Java web app in Azure and connect it tooa MySQL database.</span></span> <span data-ttu-id="08f4a-105">När du är klar har du en [Vårversionen Start](https://projects.spring.io/spring-boot/) program som lagrar data i [Azure-databas för MySQL](https://docs.microsoft.com/azure/mysql/overview) körs på [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="08f4a-105">When you are finished, you will have a [Spring Boot](https://projects.spring.io/spring-boot/) application storing data in [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) running on [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span></span>

![Java-app som körs i Azure apptjänst](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="08f4a-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="08f4a-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08f4a-108">Skapa en MySQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="08f4a-108">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="08f4a-109">Ansluta en exempeldatabas app toohello</span><span class="sxs-lookup"><span data-stu-id="08f4a-109">Connect a sample app toohello database</span></span>
> * <span data-ttu-id="08f4a-110">Distribuera hello app tooAzure</span><span class="sxs-lookup"><span data-stu-id="08f4a-110">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="08f4a-111">Uppdatera och distribuera hello app</span><span class="sxs-lookup"><span data-stu-id="08f4a-111">Update and redeploy hello app</span></span>
> * <span data-ttu-id="08f4a-112">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="08f4a-112">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="08f4a-113">Övervaka hello app i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="08f4a-113">Monitor hello app in hello Azure portal</span></span>


## <a name="prerequisites"></a><span data-ttu-id="08f4a-114">Krav</span><span class="sxs-lookup"><span data-stu-id="08f4a-114">Prerequisites</span></span>

1. [<span data-ttu-id="08f4a-115">Hämta och installera Git</span><span class="sxs-lookup"><span data-stu-id="08f4a-115">Download and install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="08f4a-116">Hämta och installera hello JDK för Java-7 eller senare</span><span class="sxs-lookup"><span data-stu-id="08f4a-116">Download and install hello Java 7 JDK or above</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [<span data-ttu-id="08f4a-117">Hämta, installera och starta MySQL</span><span class="sxs-lookup"><span data-stu-id="08f4a-117">Download, install, and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="08f4a-118">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="08f4a-118">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="08f4a-119">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="08f4a-119">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="08f4a-120">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="08f4a-120">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-local-mysql"></a><span data-ttu-id="08f4a-121">Förbereda lokala MySQL</span><span class="sxs-lookup"><span data-stu-id="08f4a-121">Prepare local MySQL</span></span> 

<span data-ttu-id="08f4a-122">I det här steget skapar du en databas i en lokal MySQL-server för användning i tester hello appen lokalt på din dator.</span><span class="sxs-lookup"><span data-stu-id="08f4a-122">In this step, you create a database in a local MySQL server for use in testing hello app locally on your machine.</span></span>

### <a name="connect-toomysql-server"></a><span data-ttu-id="08f4a-123">Anslut tooMySQL server</span><span class="sxs-lookup"><span data-stu-id="08f4a-123">Connect tooMySQL server</span></span>

<span data-ttu-id="08f4a-124">Anslut tooyour lokala MySQL-servern i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="08f4a-124">In a terminal window, connect tooyour local MySQL server.</span></span> <span data-ttu-id="08f4a-125">Du kan använda den här terminalfönster toorun alla hello-kommandon i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="08f4a-125">You can use this terminal window toorun all hello commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="08f4a-126">Om du uppmanas att ange ett lösenord anger hello lösenordet för hello `root` konto.</span><span class="sxs-lookup"><span data-stu-id="08f4a-126">If you're prompted for a password, enter hello password for hello `root` account.</span></span> <span data-ttu-id="08f4a-127">Om du inte kommer ihåg rotlösenordet, se [MySQL: hur tooReset hello Rotlösenordet](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="08f4a-127">If you don't remember your root account password, see [MySQL: How tooReset hello Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="08f4a-128">Om kommandot körs utan problem, körs MySQL-servern redan.</span><span class="sxs-lookup"><span data-stu-id="08f4a-128">If your command runs successfully, then your MySQL server is already running.</span></span> <span data-ttu-id="08f4a-129">Om inte, kontrollera att den lokala MySQL-servern har startats med följande hello [MySQL efter installationssteg](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="08f4a-129">If not, make sure that your local MySQL server is started by following hello [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database"></a><span data-ttu-id="08f4a-130">Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="08f4a-130">Create a database</span></span> 

<span data-ttu-id="08f4a-131">I hello `mysql` uppmanar, skapa en databas och en tabell för hello arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="08f4a-131">In hello `mysql` prompt, create a database and a table for hello to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

<span data-ttu-id="08f4a-132">Avsluta anslutningen till servern genom att skriva `quit`.</span><span class="sxs-lookup"><span data-stu-id="08f4a-132">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="create-and-run-hello-sample-app"></a><span data-ttu-id="08f4a-133">Skapa och köra hello sample-appen</span><span class="sxs-lookup"><span data-stu-id="08f4a-133">Create and run hello sample app</span></span> 

<span data-ttu-id="08f4a-134">I det här steget klona exempelappen källan Start, konfigurera den lokala MySQL-databas i toouse hello och körs på datorn.</span><span class="sxs-lookup"><span data-stu-id="08f4a-134">In this step, you clone sample Spring boot app, configure it toouse hello local MySQL database, and run it on your computer.</span></span> 

### <a name="clone-hello-sample"></a><span data-ttu-id="08f4a-135">Klona hello-exempel</span><span class="sxs-lookup"><span data-stu-id="08f4a-135">Clone hello sample</span></span>

<span data-ttu-id="08f4a-136">Navigera i hello terminalfönster, tooa working directory och klona hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="08f4a-136">In hello terminal window, navigate tooa working directory and clone hello sample repository.</span></span> 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-hello-app-toouse-hello-mysql-database"></a><span data-ttu-id="08f4a-137">Konfigurera hello app toouse hello MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="08f4a-137">Configure hello app toouse hello MySQL database</span></span>

<span data-ttu-id="08f4a-138">Uppdatera hello `spring.datasource.password` och värde i *spring-boot-mysql-todo/src/main/resources/application.properties* med hello används samma rotlösenordet tooopen hello MySQL prompten:</span><span class="sxs-lookup"><span data-stu-id="08f4a-138">Update hello `spring.datasource.password` and  value in *spring-boot-mysql-todo/src/main/resources/application.properties* with hello same root password used tooopen hello MySQL prompt:</span></span>

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-hello-sample"></a><span data-ttu-id="08f4a-139">Skapa och köra hello-exempel</span><span class="sxs-lookup"><span data-stu-id="08f4a-139">Build and run hello sample</span></span>

<span data-ttu-id="08f4a-140">Skapa och köra hello exempel med hjälp av hello Maven wrapper ingår i hello lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="08f4a-140">Build and run hello sample using hello Maven wrapper included in hello repo:</span></span>

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

<span data-ttu-id="08f4a-141">Öppna din webbläsare toohttp://localhost:8080 toosee i hello exemplet i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="08f4a-141">Open your browser toohttp://localhost:8080 toosee in hello sample in action.</span></span> <span data-ttu-id="08f4a-142">När du lägger till toohello uppgiftslistan använda hello följande SQL-kommandon i hello MySQL fråga tooview hello data som lagras i MySQL.</span><span class="sxs-lookup"><span data-stu-id="08f4a-142">As you add tasks toohello list,  use hello following SQL commands in hello MySQL prompt tooview hello data stored in MySQL.</span></span>

```SQL
use testdb;
select * from todo_item;
```

<span data-ttu-id="08f4a-143">Stoppa hello program genom att träffa `Ctrl` + `C` i hello terminal.</span><span class="sxs-lookup"><span data-stu-id="08f4a-143">Stop hello application by hitting `Ctrl`+`C` in hello terminal.</span></span> 

## <a name="create-an-azure-mysql-database"></a><span data-ttu-id="08f4a-144">Skapa en Azure MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="08f4a-144">Create an Azure MySQL database</span></span>

<span data-ttu-id="08f4a-145">I det här steget skapar du en [Azure-databas för MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) -instans med hello [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="08f4a-145">In this step, you create an [Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) instance using hello [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="08f4a-146">Du konfigurerar hello exempel programmet toouse den här databasen senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="08f4a-146">You configure hello sample application toouse this database later on in hello tutorial.</span></span>

<span data-ttu-id="08f4a-147">Använd hello Azure CLI 2.0 i ett terminalfönster toocreate hello resurser behövs toohost Java-program i Azure apptjänst.</span><span class="sxs-lookup"><span data-stu-id="08f4a-147">Use hello Azure CLI 2.0 in a terminal window toocreate hello resources needed toohost your Java application in Azure appservice.</span></span> <span data-ttu-id="08f4a-148">Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="08f4a-148">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span> 

```azurecli-interactive 
az login 
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="08f4a-149">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="08f4a-149">Create a resource group</span></span>

<span data-ttu-id="08f4a-150">Skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="08f4a-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="08f4a-151">En Azure-resursgrupp är en logisk behållare där relaterade resurser som webbappar, databaser och storage-konton distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="08f4a-151">An Azure resource group is a logical container where related resources like web apps, databases, and storage accounts are deployed and managed.</span></span> 

<span data-ttu-id="08f4a-152">hello följande exempel skapas en resursgrupp i hello Nordeuropa region:</span><span class="sxs-lookup"><span data-stu-id="08f4a-152">hello following example creates a resource group in hello North Europe region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

<span data-ttu-id="08f4a-153">toosee hello möjliga värden du kan använda för `--location`, använda hello [az apptjänst lista-platser](/cli/azure/appservice#list-locations) kommando.</span><span class="sxs-lookup"><span data-stu-id="08f4a-153">toosee hello possible values you can use for `--location`, use hello [az appservice list-locations](/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-a-mysql-server"></a><span data-ttu-id="08f4a-154">Skapa en MySQL-server</span><span class="sxs-lookup"><span data-stu-id="08f4a-154">Create a MySQL server</span></span>

<span data-ttu-id="08f4a-155">Skapa en server i Azure-databas för MySQL (förhandsversion) med hello [az mysql-servern skapa](/cli/azure/mysql/server#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="08f4a-155">Create a server in Azure Database for MySQL (Preview) with hello [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>    
<span data-ttu-id="08f4a-156">Ersätt egna unika MySQL namnet på servern där du ser hello `<mysql_server_name>` platshållare.</span><span class="sxs-lookup"><span data-stu-id="08f4a-156">Substitute your own unique MySQL server name where you see hello `<mysql_server_name>` placeholder.</span></span> <span data-ttu-id="08f4a-157">Det här namnet är en del av MySQL-serverns värdnamn, `<mysql_server_name>.mysql.database.azure.com`, så den behöver toobe globalt unika.</span><span class="sxs-lookup"><span data-stu-id="08f4a-157">This name is part of your MySQL server's hostname, `<mysql_server_name>.mysql.database.azure.com`, so it needs toobe globally unique.</span></span> <span data-ttu-id="08f4a-158">I stället använda `<admin_user>` och `<admin_password>` med egna värden.</span><span class="sxs-lookup"><span data-stu-id="08f4a-158">Also substitute `<admin_user>` and `<admin_password>` with your own values.</span></span>

```azurecli-interactive
az mysql server create --name <mysql_server_name> \ 
    --resource-group myResourceGroup \ 
    --location "North Europe" \
    --admin-user <admin_user> \ 
    --admin-password <admin_password>
```

<span data-ttu-id="08f4a-159">När hello MySQL server skapas visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="08f4a-159">When hello MySQL server is created, hello Azure CLI shows information similar toohello following example:</span></span>

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a><span data-ttu-id="08f4a-160">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="08f4a-160">Configure server firewall</span></span>

<span data-ttu-id="08f4a-161">Skapa en brandväggsregel för MySQL server tooallow klienten anslutningar med hello [az mysql-brandväggsregel skapa](/cli/azure/mysql/server/firewall-rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="08f4a-161">Create a firewall rule for your MySQL server tooallow client connections by using hello [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span> 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name>  \ 
    --resource-group myResourceGroup \ 
    --start-ip-address 0.0.0.0 \ 
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="08f4a-162">Azure-databas för MySQL (förhandsversion) kan inte för närvarande automatiskt anslutningar från Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="08f4a-162">Azure Database for MySQL (Preview) does not currently automatically enable connections from Azure services.</span></span> <span data-ttu-id="08f4a-163">IP-adresser i Azure tilldelas dynamiskt, är det bättre tooenable alla IP-adresser för nu.</span><span class="sxs-lookup"><span data-stu-id="08f4a-163">As IP addresses in Azure are dynamically assigned, it is better tooenable all IP addresses for now.</span></span> <span data-ttu-id="08f4a-164">Eftersom hello tjänsten fortsätter förhandsgranskningen, aktiveras bättre metoder för att skydda databasen.</span><span class="sxs-lookup"><span data-stu-id="08f4a-164">As hello service continues its preview, better methods for securing your database will be enabled.</span></span>

## <a name="configure-hello-azure-mysql-database"></a><span data-ttu-id="08f4a-165">Konfigurera hello Azure MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="08f4a-165">Configure hello Azure MySQL database</span></span>

<span data-ttu-id="08f4a-166">Anslut toohello MySQL-server i Azure i hello terminalfönster på datorn.</span><span class="sxs-lookup"><span data-stu-id="08f4a-166">In hello terminal window on your computer, connect toohello MySQL server in Azure.</span></span> <span data-ttu-id="08f4a-167">Använd hello-värde som du angav tidigare för `<admin_user>` och `<mysql_server_name>`.</span><span class="sxs-lookup"><span data-stu-id="08f4a-167">Use hello value you specified previously for `<admin_user>` and `<mysql_server_name>`.</span></span>

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a><span data-ttu-id="08f4a-168">Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="08f4a-168">Create a database</span></span> 

<span data-ttu-id="08f4a-169">I hello `mysql` uppmanar, skapa en databas och en tabell för hello arbetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="08f4a-169">In hello `mysql` prompt, create a database and a table for hello to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="08f4a-170">Skapa en användare med behörighet</span><span class="sxs-lookup"><span data-stu-id="08f4a-170">Create a user with permissions</span></span>

<span data-ttu-id="08f4a-171">Skapa en databasanvändare och ge den alla behörigheter i hello `tododb` databas.</span><span class="sxs-lookup"><span data-stu-id="08f4a-171">Create a database user and give it all privileges in hello `tododb` database.</span></span> <span data-ttu-id="08f4a-172">Ersätt platshållarna hello `<Javaapp_user>` och `<Javaapp_password>` med dina egna unika namn.</span><span class="sxs-lookup"><span data-stu-id="08f4a-172">Replace hello placeholders `<Javaapp_user>` and `<Javaapp_password>` with your own unique app name.</span></span>

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* too'<Javaapp_user>';
```

<span data-ttu-id="08f4a-173">Avsluta anslutningen till servern genom att skriva `quit`.</span><span class="sxs-lookup"><span data-stu-id="08f4a-173">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="deploy-hello-sample-tooazure-app-service"></a><span data-ttu-id="08f4a-174">Distribuera hello exempel tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="08f4a-174">Deploy hello sample tooAzure App Service</span></span>

<span data-ttu-id="08f4a-175">Skapa en Azure App Service-plan med hello **lediga** prisnivån med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="08f4a-175">Create an Azure App Service plan with hello **FREE** pricing tier using hello  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="08f4a-176">Hej programtjänstplan definierar hello fysiska resurser som används toohost dina appar.</span><span class="sxs-lookup"><span data-stu-id="08f4a-176">hello appservice plan defines hello physical resources used toohost your apps.</span></span> <span data-ttu-id="08f4a-177">Alla program som tilldelats tooan programtjänstplan dela dessa resurser, så att du toosave kostnad när värd för flera appar.</span><span class="sxs-lookup"><span data-stu-id="08f4a-177">All applications assigned tooan appservice plan share these resources, allowing you toosave cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="08f4a-178">När hello plan är klar, utdata hello Azure CLI visar liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="08f4a-178">When hello plan is ready, hello Azure CLI shows similar output toohello following example:</span></span>

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a><span data-ttu-id="08f4a-179">Skapa en Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="08f4a-179">Create an Azure Web app</span></span>

 <span data-ttu-id="08f4a-180">Använd hello [az webapp skapa](/cli/azure/appservice/web#create) CLI kommandot toocreate en web app definition i hello `myAppServicePlan` App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="08f4a-180">Use hello [az webapp create](/cli/azure/appservice/web#create) CLI command toocreate a web app definition in hello `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="08f4a-181">hello web app definition innehåller en URL-tooaccess ditt program med och konfigurerar flera alternativ toodeploy tooAzure din kod.</span><span class="sxs-lookup"><span data-stu-id="08f4a-181">hello web app definition provides a URL tooaccess your application with and configures several options toodeploy your code tooAzure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="08f4a-182">Ersätt hello `<app_name>` med dina egna unika namn.</span><span class="sxs-lookup"><span data-stu-id="08f4a-182">Substitute hello `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="08f4a-183">Detta unika namn är en del av hello standarddomännamnet för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure.</span><span class="sxs-lookup"><span data-stu-id="08f4a-183">This unique name is part of hello default domain name for hello web app, so hello name needs toobe unique across all apps in Azure.</span></span> <span data-ttu-id="08f4a-184">Du kan mappa ett webbprogram för domänen namnet post toohello innan exponera tooyour användare.</span><span class="sxs-lookup"><span data-stu-id="08f4a-184">You can map a custom domain name entry toohello web app before you expose it tooyour users.</span></span>

<span data-ttu-id="08f4a-185">När hello web app definition är klar visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="08f4a-185">When hello web app definition is ready, hello Azure CLI shows information similar toohello following example:</span></span> 

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

### <a name="configure-java"></a><span data-ttu-id="08f4a-186">Konfigurera Java</span><span class="sxs-lookup"><span data-stu-id="08f4a-186">Configure Java</span></span> 

<span data-ttu-id="08f4a-187">Konfigurera hello Java runtime konfiguration som din app behöver med hello [uppdatering az apptjänst web-config](/cli/azure/appservice/web/config#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="08f4a-187">Set up hello Java runtime configuration that your app needs with hello  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="08f4a-188">hello följande kommando konfigurerar hello web app toorun på en senaste Java 8 JDK och [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="08f4a-188">hello following command configures hello web app toorun on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

### <a name="configure-hello-app-toouse-hello-azure-sql-database"></a><span data-ttu-id="08f4a-189">Konfigurera hello app toouse hello Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="08f4a-189">Configure hello app toouse hello Azure SQL database</span></span>

<span data-ttu-id="08f4a-190">Ange inställningar för program innan du kör hello sample-appen på hello web app toouse hello Azure MySQL-databas du skapade i Azure.</span><span class="sxs-lookup"><span data-stu-id="08f4a-190">Before running hello sample app, set application settings on hello web app toouse hello Azure MySQL database you created in Azure.</span></span> <span data-ttu-id="08f4a-191">De här egenskaperna är exponerade toohello webbprogram som miljövariabler och åsidosätta hello värdena i hello application.properties inuti hello paketerade webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="08f4a-191">These properties are exposed toohello web application as environment variables and override hello values set in hello application.properties inside hello packaged web app.</span></span> 

<span data-ttu-id="08f4a-192">Ange inställningar för program med hjälp av [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) i hello CLI:</span><span class="sxs-lookup"><span data-stu-id="08f4a-192">Set application settings using [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) in hello CLI:</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" \
    --resource-group myResourceGroup \
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name  \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL=Javaapp_password \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a><span data-ttu-id="08f4a-193">Hämta autentiseringsuppgifter för FTP-distribution</span><span class="sxs-lookup"><span data-stu-id="08f4a-193">Get FTP deployment credentials</span></span> 
<span data-ttu-id="08f4a-194">Du kan distribuera ditt program tooAzure apptjänst på olika sätt, inklusive FTP, lokal Git, GitHub, Visual Studio Team Services och BitBucket.</span><span class="sxs-lookup"><span data-stu-id="08f4a-194">You can deploy your application tooAzure appservice in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="08f4a-195">FTP-toodeploy hello i det här exemplet. WAR-filen tidigare bygger på din lokala dator tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="08f4a-195">For this example, FTP toodeploy hello .WAR file built previously on your local machine tooAzure App Service.</span></span>

<span data-ttu-id="08f4a-196">toodetermine vad autentiseringsuppgifter toopass längs i en FTP-kommandot toohello Web App används [az apptjänst web distributionsåtgärder lista-publicering-profiler](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) kommando:</span><span class="sxs-lookup"><span data-stu-id="08f4a-196">toodetermine what credentials toopass along in an ftp command toohello Web App, Use [az appservice web deployment list-publishing-profiles](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) command:</span></span> 

```azurecli-interactive
az webapp deployment list-publishing-profiles \ 
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" \ 
    --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-hello-app-using-ftp"></a><span data-ttu-id="08f4a-197">Överför hello-app med FTP</span><span class="sxs-lookup"><span data-stu-id="08f4a-197">Upload hello app using FTP</span></span>

<span data-ttu-id="08f4a-198">Använd din favorit toodeploy hello för FTP-verktyget. WAR-filen toohello */site/wwwroot/webapps* mapp på hello serveradress från hello `URL` i hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="08f4a-198">Use your favorite FTP tool toodeploy hello .WAR file toohello */site/wwwroot/webapps* folder on hello server address taken from hello `URL` field in hello previous command.</span></span> <span data-ttu-id="08f4a-199">Ta bort hello befintliga (rot) programkatalogen och Ersätt hello befintliga ROOT.war med hello. Inbyggda hello tidigare i kursen hello WAR-filen.</span><span class="sxs-lookup"><span data-stu-id="08f4a-199">Remove hello existing default (ROOT) application directory and replace hello existing ROOT.war with hello .WAR file built in hello earlier in hello tutorial.</span></span>

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected toowaws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-hello-web-app"></a><span data-ttu-id="08f4a-200">Testa hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="08f4a-200">Test hello web app</span></span>

<span data-ttu-id="08f4a-201">Bläddra för`http://<app_name>.azurewebsites.net/` och lägga till några uppgifter toohello lista.</span><span class="sxs-lookup"><span data-stu-id="08f4a-201">Browse too`http://<app_name>.azurewebsites.net/` and add a few tasks toohello list.</span></span> 

![Java-app som körs i Azure apptjänst](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="08f4a-203">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="08f4a-203">**Congratulations!**</span></span> <span data-ttu-id="08f4a-204">Du kör en datadrivna Java-app i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="08f4a-204">You're running a data-driven Java app in Azure App Service.</span></span>

## <a name="update-hello-app-and-redeploy"></a><span data-ttu-id="08f4a-205">Uppdatera hello app och distribuera om</span><span class="sxs-lookup"><span data-stu-id="08f4a-205">Update hello app and redeploy</span></span>

<span data-ttu-id="08f4a-206">Uppdatera hello programmet tooinclude ytterligare en kolumn i hello todo-listan för vilken dag hello objektet skapades.</span><span class="sxs-lookup"><span data-stu-id="08f4a-206">Update hello application tooinclude an additional column in hello todo list for what day hello item was created.</span></span> <span data-ttu-id="08f4a-207">Källan Start hanterar uppdatering hello-databasschemat för dig som hello datamodelländringar utan att ändra din befintliga databasposter.</span><span class="sxs-lookup"><span data-stu-id="08f4a-207">Spring Boot handles updating hello database schema for you as hello data model changes without altering your existing database records.</span></span>

1. <span data-ttu-id="08f4a-208">I det lokala systemet öppna *src/main/java/com/example/fabrikam/TodoItem.java* och Lägg till följande hello importerar toohello klass:</span><span class="sxs-lookup"><span data-stu-id="08f4a-208">On your local system, open up *src/main/java/com/example/fabrikam/TodoItem.java* and add hello following imports toohello class:</span></span>   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. <span data-ttu-id="08f4a-209">Lägg till en `String` egenskapen `timeCreated` för*src/main/java/com/example/fabrikam/TodoItem.java*, initierar med en tidsstämpel när objektet skapas.</span><span class="sxs-lookup"><span data-stu-id="08f4a-209">Add a `String` property `timeCreated` too*src/main/java/com/example/fabrikam/TodoItem.java*, initializing it with a timestamp at object creation.</span></span> <span data-ttu-id="08f4a-210">Lägg till Get/Set-metoder för hello nya `timeCreated` egenskapen när du redigerar filen.</span><span class="sxs-lookup"><span data-stu-id="08f4a-210">Add getters/setters for hello new `timeCreated` property while you are editing this file.</span></span>

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. <span data-ttu-id="08f4a-211">Uppdatera *src/main/java/com/example/fabrikam/TodoDemoController.java* med en rad i hello `updateTodo` metoden tooset hello tidsstämpel:</span><span class="sxs-lookup"><span data-stu-id="08f4a-211">Update *src/main/java/com/example/fabrikam/TodoDemoController.java* with a line in hello `updateTodo` method tooset hello timestamp:</span></span>

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. <span data-ttu-id="08f4a-212">Lägga till stöd för nya hello-fältet i hello Thymeleaf mall.</span><span class="sxs-lookup"><span data-stu-id="08f4a-212">Add support for hello new field in hello Thymeleaf template.</span></span> <span data-ttu-id="08f4a-213">Uppdatera *src/main/resources/templates/index.html* med ett nytt huvud för hello tidsstämpel och ett nytt fält toodisplay hello-värde för hello tidsstämpeln i varje datarad i tabellen.</span><span class="sxs-lookup"><span data-stu-id="08f4a-213">Update *src/main/resources/templates/index.html* with a new table header for hello timestamp, and a new field toodisplay hello value of hello timestamp in each table data row.</span></span>

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. <span data-ttu-id="08f4a-214">Återskapa hello program:</span><span class="sxs-lookup"><span data-stu-id="08f4a-214">Rebuild hello application:</span></span>

    ```bash
    mvnw clean package 
    ```

6. <span data-ttu-id="08f4a-215">FTP-hello uppdateras. WAR som tidigare kan ta bort befintliga hello *plats/wwwroot/webbappar/ROOT* directory och *ROOT.war*, och sedan ladda upp hello uppdateras. WAR-filen som ROOT.war.</span><span class="sxs-lookup"><span data-stu-id="08f4a-215">FTP hello updated .WAR as before, removing hello existing *site/wwwroot/webapps/ROOT* directory and *ROOT.war*, then uploading hello updated .WAR file as ROOT.war.</span></span> 

<span data-ttu-id="08f4a-216">När du uppdaterar hello appen, en **Skapad** kolumnen visas nu.</span><span class="sxs-lookup"><span data-stu-id="08f4a-216">When you refresh hello app, a **Time Created** column is now visible.</span></span> <span data-ttu-id="08f4a-217">När du lägger till en ny uppgift fyller hello app hello tidsstämpel automatiskt.</span><span class="sxs-lookup"><span data-stu-id="08f4a-217">When you add a new task, hello app will populate hello timestamp automatically.</span></span> <span data-ttu-id="08f4a-218">Din befintliga aktiviteter förblir oförändrad och arbeta med hello appen även om hello underliggande datamodellen har ändrats.</span><span class="sxs-lookup"><span data-stu-id="08f4a-218">Your existing tasks remain unchanged and work with hello app even though hello underlying data model has changed.</span></span> 

![Java-app som har uppdaterats med en ny kolumn](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a><span data-ttu-id="08f4a-220">Dataströmmen diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="08f4a-220">Stream diagnostic logs</span></span> 

<span data-ttu-id="08f4a-221">När Java-programmet körs i Azure App Service kan du få hello konsolen loggar skickas direkt tooyour terminal.</span><span class="sxs-lookup"><span data-stu-id="08f4a-221">While your Java application runs in Azure App Service, you can get hello console logs piped directly tooyour terminal.</span></span> <span data-ttu-id="08f4a-222">På så sätt kan du hello diagnostiska meddelanden för samma toohelp du felsöka programfel.</span><span class="sxs-lookup"><span data-stu-id="08f4a-222">That way, you can get hello same diagnostic messages toohelp you debug application errors.</span></span>

<span data-ttu-id="08f4a-223">toostart loggen direktuppspelning, Använd hello [az webapp loggen pilslut](/cli/azure/appservice/web/log#tail) kommando.</span><span class="sxs-lookup"><span data-stu-id="08f4a-223">toostart log streaming, use hello [az webapp log tail](/cli/azure/appservice/web/log#tail) command.</span></span>

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="08f4a-224">Hantera Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="08f4a-224">Manage your Azure web app</span></span>

<span data-ttu-id="08f4a-225">Gå toohello Azure portal toosee hello webbappen du skapade.</span><span class="sxs-lookup"><span data-stu-id="08f4a-225">Go toohello Azure portal toosee hello web app you created.</span></span>

<span data-ttu-id="08f4a-226">toodo, logga in för[https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="08f4a-226">toodo this, sign in too[https://portal.azure.com](https://portal.azure.com).</span></span>

<span data-ttu-id="08f4a-227">Hello vänstra menyn klickar du på **Apptjänst**, klicka på hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="08f4a-227">From hello left menu, click **App Service**, then click hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-java-mysql/access-portal.png)

<span data-ttu-id="08f4a-229">Som standard visas din webbapps blad hello **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="08f4a-229">By default, your web app's blade shows hello **Overview** page.</span></span> <span data-ttu-id="08f4a-230">På den här sidan får du en översikt över hur det går för appen.</span><span class="sxs-lookup"><span data-stu-id="08f4a-230">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="08f4a-231">Här kan utföra du också hanteringsuppgifter som att stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="08f4a-231">Here, you can also perform management tasks like stop, start, restart, and delete.</span></span> <span data-ttu-id="08f4a-232">hello flikar hello vänster på hello bladet visar hello annan konfigurationssidor som du kan öppna.</span><span class="sxs-lookup"><span data-stu-id="08f4a-232">hello tabs on hello left side of hello blade show hello different configuration pages you can open.</span></span>

![App Service-blad på Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

<span data-ttu-id="08f4a-234">Flikarna i hello bladet innehåller hello många bra funktioner som du kan lägga till tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="08f4a-234">These tabs in hello blade show hello many great features you can add tooyour web app.</span></span> <span data-ttu-id="08f4a-235">hello följande lista innehåller några av hello möjligheter:</span><span class="sxs-lookup"><span data-stu-id="08f4a-235">hello following list gives you just a few of hello possibilities:</span></span>
* <span data-ttu-id="08f4a-236">Mappa ett anpassat DNS-namn</span><span class="sxs-lookup"><span data-stu-id="08f4a-236">Map a custom DNS name</span></span>
* <span data-ttu-id="08f4a-237">Bind ett anpassat SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="08f4a-237">Bind a custom SSL certificate</span></span>
* <span data-ttu-id="08f4a-238">Konfigurera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="08f4a-238">Configure continuous deployment</span></span>
* <span data-ttu-id="08f4a-239">Skala upp</span><span class="sxs-lookup"><span data-stu-id="08f4a-239">Scale up and out</span></span>
* <span data-ttu-id="08f4a-240">Lägg till användarautentisering</span><span class="sxs-lookup"><span data-stu-id="08f4a-240">Add user authentication</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="08f4a-241">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="08f4a-241">Clean up resources</span></span>

<span data-ttu-id="08f4a-242">Om du inte behöver dessa resurser för en annan självstudiekursen (se [nästa steg](#next)), kan du ta bort dem genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="08f4a-242">If you don't need these resources for another tutorial (see [Next steps](#next)), you can delete them by running hello following command:</span></span> 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="08f4a-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="08f4a-243">Next steps</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08f4a-244">Skapa en MySQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="08f4a-244">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="08f4a-245">Ansluta en exempel Java-app toohello MySQL</span><span class="sxs-lookup"><span data-stu-id="08f4a-245">Connect a sample Java app toohello MySQL</span></span>
> * <span data-ttu-id="08f4a-246">Distribuera hello app tooAzure</span><span class="sxs-lookup"><span data-stu-id="08f4a-246">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="08f4a-247">Uppdatera och distribuera hello app</span><span class="sxs-lookup"><span data-stu-id="08f4a-247">Update and redeploy hello app</span></span>
> * <span data-ttu-id="08f4a-248">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="08f4a-248">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="08f4a-249">Hantera hello appen i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="08f4a-249">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="08f4a-250">I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn toohello app.</span><span class="sxs-lookup"><span data-stu-id="08f4a-250">Advance toohello next tutorial toolearn how toomap a custom DNS name toohello app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="08f4a-251">Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps</span><span class="sxs-lookup"><span data-stu-id="08f4a-251">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
