---
title: Skapa en Java och MySQL-webbapp i Azure
description: "Lär dig hur du hämtar en Java-app som ansluter till tjänsten Azure MySQL database arbetar i Azure apptjänst."
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
ms.openlocfilehash: eb2d59939c4e4486bb14bb143a4a18f9bc1478e1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a><span data-ttu-id="24a78-103">Skapa en Java och MySQL-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="24a78-103">Build a Java and MySQL web app in Azure</span></span>

<span data-ttu-id="24a78-104">Den här kursen visar hur du skapar en Java-webbapp i Azure och ansluta till en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="24a78-104">This tutorial shows you how to create a Java web app in Azure and connect it to a MySQL database.</span></span> <span data-ttu-id="24a78-105">När du är klar har du en [Vårversionen Start](https://projects.spring.io/spring-boot/) program som lagrar data i [Azure-databas för MySQL](https://docs.microsoft.com/azure/mysql/overview) körs på [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span><span class="sxs-lookup"><span data-stu-id="24a78-105">When you are finished, you will have a [Spring Boot](https://projects.spring.io/spring-boot/) application storing data in [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) running on [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).</span></span>

![Java-app som körs i Azure apptjänst](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="24a78-107">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="24a78-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24a78-108">Skapa en MySQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="24a78-108">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="24a78-109">Ansluta en exempelapp till databasen</span><span class="sxs-lookup"><span data-stu-id="24a78-109">Connect a sample app to the database</span></span>
> * <span data-ttu-id="24a78-110">Distribuera appen till Azure</span><span class="sxs-lookup"><span data-stu-id="24a78-110">Deploy the app to Azure</span></span>
> * <span data-ttu-id="24a78-111">Uppdatera och distribuera om appen</span><span class="sxs-lookup"><span data-stu-id="24a78-111">Update and redeploy the app</span></span>
> * <span data-ttu-id="24a78-112">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="24a78-112">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="24a78-113">Övervaka app på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="24a78-113">Monitor the app in the Azure portal</span></span>


## <a name="prerequisites"></a><span data-ttu-id="24a78-114">Krav</span><span class="sxs-lookup"><span data-stu-id="24a78-114">Prerequisites</span></span>

1. [<span data-ttu-id="24a78-115">Hämta och installera Git</span><span class="sxs-lookup"><span data-stu-id="24a78-115">Download and install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="24a78-116">Hämta och installera JDK för Java-7 eller senare</span><span class="sxs-lookup"><span data-stu-id="24a78-116">Download and install the Java 7 JDK or above</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [<span data-ttu-id="24a78-117">Hämta, installera och starta MySQL</span><span class="sxs-lookup"><span data-stu-id="24a78-117">Download, install, and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="24a78-118">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="24a78-118">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="24a78-119">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="24a78-119">Run `az --version` to find the version.</span></span> <span data-ttu-id="24a78-120">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="24a78-120">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="prepare-local-mysql"></a><span data-ttu-id="24a78-121">Förbereda lokala MySQL</span><span class="sxs-lookup"><span data-stu-id="24a78-121">Prepare local MySQL</span></span> 

<span data-ttu-id="24a78-122">I det här steget skapar du en databas i en lokal MySQL-server för användning i testar appen lokalt på din dator.</span><span class="sxs-lookup"><span data-stu-id="24a78-122">In this step, you create a database in a local MySQL server for use in testing the app locally on your machine.</span></span>

### <a name="connect-to-mysql-server"></a><span data-ttu-id="24a78-123">Ansluta till MySQL-servern</span><span class="sxs-lookup"><span data-stu-id="24a78-123">Connect to MySQL server</span></span>

<span data-ttu-id="24a78-124">Anslut till din lokala MySQL-server i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="24a78-124">In a terminal window, connect to your local MySQL server.</span></span> <span data-ttu-id="24a78-125">Du kan använda den här terminalfönster för att köra alla kommandon i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="24a78-125">You can use this terminal window to run all the commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="24a78-126">Om du uppmanas att ange ett lösenord anger du lösenordet för den `root` konto.</span><span class="sxs-lookup"><span data-stu-id="24a78-126">If you're prompted for a password, enter the password for the `root` account.</span></span> <span data-ttu-id="24a78-127">Om du inte kommer ihåg rotlösenordet, se [MySQL: hur du återställer Rotlösenordet](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="24a78-127">If you don't remember your root account password, see [MySQL: How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="24a78-128">Om kommandot körs utan problem, körs MySQL-servern redan.</span><span class="sxs-lookup"><span data-stu-id="24a78-128">If your command runs successfully, then your MySQL server is already running.</span></span> <span data-ttu-id="24a78-129">Om inte, kontrollera att den lokala MySQL-servern är igång genom att följa den [MySQL efter installationssteg](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="24a78-129">If not, make sure that your local MySQL server is started by following the [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database"></a><span data-ttu-id="24a78-130">Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="24a78-130">Create a database</span></span> 

<span data-ttu-id="24a78-131">I den `mysql` uppmanar, skapa en databas och en tabell för att göra-objekt.</span><span class="sxs-lookup"><span data-stu-id="24a78-131">In the `mysql` prompt, create a database and a table for the to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

<span data-ttu-id="24a78-132">Avsluta anslutningen till servern genom att skriva `quit`.</span><span class="sxs-lookup"><span data-stu-id="24a78-132">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a><span data-ttu-id="24a78-133">Skapa och kör exempelappen</span><span class="sxs-lookup"><span data-stu-id="24a78-133">Create and run the sample app</span></span> 

<span data-ttu-id="24a78-134">I det här steget klona exempelappen källan Start, konfigurera den lokala MySQL-databas och kör det på din dator.</span><span class="sxs-lookup"><span data-stu-id="24a78-134">In this step, you clone sample Spring boot app, configure it to use the local MySQL database, and run it on your computer.</span></span> 

### <a name="clone-the-sample"></a><span data-ttu-id="24a78-135">Klona exemplet</span><span class="sxs-lookup"><span data-stu-id="24a78-135">Clone the sample</span></span>

<span data-ttu-id="24a78-136">Navigera till en arbetskatalog och klona lagringsplatsen exempel i fönstret terminal.</span><span class="sxs-lookup"><span data-stu-id="24a78-136">In the terminal window, navigate to a working directory and clone the sample repository.</span></span> 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a><span data-ttu-id="24a78-137">Konfigurera appen för att använda MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="24a78-137">Configure the app to use the MySQL database</span></span>

<span data-ttu-id="24a78-138">Uppdatering av `spring.datasource.password` och värde i *spring-boot-mysql-todo/src/main/resources/application.properties* med samma rotlösenord som används för att öppna MySQL-fråga:</span><span class="sxs-lookup"><span data-stu-id="24a78-138">Update the `spring.datasource.password` and  value in *spring-boot-mysql-todo/src/main/resources/application.properties* with the same root password used to open the MySQL prompt:</span></span>

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a><span data-ttu-id="24a78-139">Skapa och köra exemplet</span><span class="sxs-lookup"><span data-stu-id="24a78-139">Build and run the sample</span></span>

<span data-ttu-id="24a78-140">Skapa och köra exemplet med Maven-omslutning som ingår i lagringsplatsen:</span><span class="sxs-lookup"><span data-stu-id="24a78-140">Build and run the sample using the Maven wrapper included in the repo:</span></span>

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

<span data-ttu-id="24a78-141">Öppna din webbläsare till http://localhost: 8080 ska se i exemplet i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="24a78-141">Open your browser to http://localhost:8080 to see in the sample in action.</span></span> <span data-ttu-id="24a78-142">När du lägger till aktiviteter i listan använda följande SQL-kommandon MySQL-fråga för att visa data som lagras i MySQL.</span><span class="sxs-lookup"><span data-stu-id="24a78-142">As you add tasks to the list,  use the following SQL commands in the MySQL prompt to view the data stored in MySQL.</span></span>

```SQL
use testdb;
select * from todo_item;
```

<span data-ttu-id="24a78-143">Stoppa programmet genom att träffa `Ctrl` + `C` i terminalen.</span><span class="sxs-lookup"><span data-stu-id="24a78-143">Stop the application by hitting `Ctrl`+`C` in the terminal.</span></span> 

## <a name="create-an-azure-mysql-database"></a><span data-ttu-id="24a78-144">Skapa en Azure MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="24a78-144">Create an Azure MySQL database</span></span>

<span data-ttu-id="24a78-145">I det här steget skapar du en [Azure-databas för MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) instans med det [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="24a78-145">In this step, you create an [Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) instance using the [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="24a78-146">Du kan konfigurera exempelprogrammet att använda den här databasen senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="24a78-146">You configure the sample application to use this database later on in the tutorial.</span></span>

<span data-ttu-id="24a78-147">Använda Azure CLI 2.0 i ett terminalfönster för att skapa de resurser som krävs för att vara värd för Java-program i Azure apptjänst.</span><span class="sxs-lookup"><span data-stu-id="24a78-147">Use the Azure CLI 2.0 in a terminal window to create the resources needed to host your Java application in Azure appservice.</span></span> <span data-ttu-id="24a78-148">Logga in på Azure-prenumerationen med kommandot [az login](/cli/azure/#login) och följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="24a78-148">Log in to your Azure subscription with the [az login](/cli/azure/#login) command and follow the on-screen directions.</span></span> 

```azurecli-interactive 
az login 
```   

### <a name="create-a-resource-group"></a><span data-ttu-id="24a78-149">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="24a78-149">Create a resource group</span></span>

<span data-ttu-id="24a78-150">Skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) med den [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="24a78-150">Create a [resource group](../azure-resource-manager/resource-group-overview.md) with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="24a78-151">En Azure-resursgrupp är en logisk behållare där relaterade resurser som webbappar, databaser och storage-konton distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="24a78-151">An Azure resource group is a logical container where related resources like web apps, databases, and storage accounts are deployed and managed.</span></span> 

<span data-ttu-id="24a78-152">I följande exempel skapas en resursgrupp i Norra Europa region:</span><span class="sxs-lookup"><span data-stu-id="24a78-152">The following example creates a resource group in the North Europe region:</span></span>

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

<span data-ttu-id="24a78-153">Se möjliga värden du kan använda för `--location`, använda den [az apptjänst lista-platser](/cli/azure/appservice#list-locations) kommando.</span><span class="sxs-lookup"><span data-stu-id="24a78-153">To see the possible values you can use for `--location`, use the [az appservice list-locations](/cli/azure/appservice#list-locations) command.</span></span>

### <a name="create-a-mysql-server"></a><span data-ttu-id="24a78-154">Skapa en MySQL-server</span><span class="sxs-lookup"><span data-stu-id="24a78-154">Create a MySQL server</span></span>

<span data-ttu-id="24a78-155">Skapa en server i Azure-databas för MySQL (förhandsversion) med den [az mysql-servern skapa](/cli/azure/mysql/server#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="24a78-155">Create a server in Azure Database for MySQL (Preview) with the [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>    
<span data-ttu-id="24a78-156">Ersätta en egen unik MySQL-servernamn där du ser den `<mysql_server_name>` platshållare.</span><span class="sxs-lookup"><span data-stu-id="24a78-156">Substitute your own unique MySQL server name where you see the `<mysql_server_name>` placeholder.</span></span> <span data-ttu-id="24a78-157">Det här namnet är en del av MySQL-serverns värdnamn, `<mysql_server_name>.mysql.database.azure.com`, så den behöver vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="24a78-157">This name is part of your MySQL server's hostname, `<mysql_server_name>.mysql.database.azure.com`, so it needs to be globally unique.</span></span> <span data-ttu-id="24a78-158">I stället använda `<admin_user>` och `<admin_password>` med egna värden.</span><span class="sxs-lookup"><span data-stu-id="24a78-158">Also substitute `<admin_user>` and `<admin_password>` with your own values.</span></span>

```azurecli-interactive
az mysql server create --name <mysql_server_name> \ 
    --resource-group myResourceGroup \ 
    --location "North Europe" \
    --admin-user <admin_user> \ 
    --admin-password <admin_password>
```

<span data-ttu-id="24a78-159">När MySQL-servern har skapats visas Azure CLI information liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="24a78-159">When the MySQL server is created, the Azure CLI shows information similar to the following example:</span></span>

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

### <a name="configure-server-firewall"></a><span data-ttu-id="24a78-160">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="24a78-160">Configure server firewall</span></span>

<span data-ttu-id="24a78-161">Skapa en brandväggsregel för MySQL-servern att tillåta klientanslutningar med hjälp av den [az mysql-brandväggsregel skapa](/cli/azure/mysql/server/firewall-rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="24a78-161">Create a firewall rule for your MySQL server to allow client connections by using the [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span> 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name>  \ 
    --resource-group myResourceGroup \ 
    --start-ip-address 0.0.0.0 \ 
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="24a78-162">Azure-databas för MySQL (förhandsversion) kan inte för närvarande automatiskt anslutningar från Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="24a78-162">Azure Database for MySQL (Preview) does not currently automatically enable connections from Azure services.</span></span> <span data-ttu-id="24a78-163">IP-adresser i Azure tilldelas dynamiskt, är det bättre att aktivera alla IP-adresser för tillfället.</span><span class="sxs-lookup"><span data-stu-id="24a78-163">As IP addresses in Azure are dynamically assigned, it is better to enable all IP addresses for now.</span></span> <span data-ttu-id="24a78-164">Eftersom tjänsten fortsätter förhandsgranskningen, aktiveras bättre metoder för att skydda databasen.</span><span class="sxs-lookup"><span data-stu-id="24a78-164">As the service continues its preview, better methods for securing your database will be enabled.</span></span>

## <a name="configure-the-azure-mysql-database"></a><span data-ttu-id="24a78-165">Konfigurera Azure MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="24a78-165">Configure the Azure MySQL database</span></span>

<span data-ttu-id="24a78-166">I fönstret terminal på din dator att ansluta till MySQL-server i Azure.</span><span class="sxs-lookup"><span data-stu-id="24a78-166">In the terminal window on your computer, connect to the MySQL server in Azure.</span></span> <span data-ttu-id="24a78-167">Använd värdet du angav tidigare för `<admin_user>` och `<mysql_server_name>`.</span><span class="sxs-lookup"><span data-stu-id="24a78-167">Use the value you specified previously for `<admin_user>` and `<mysql_server_name>`.</span></span>

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a><span data-ttu-id="24a78-168">Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="24a78-168">Create a database</span></span> 

<span data-ttu-id="24a78-169">I den `mysql` uppmanar, skapa en databas och en tabell för att göra-objekt.</span><span class="sxs-lookup"><span data-stu-id="24a78-169">In the `mysql` prompt, create a database and a table for the to-do items.</span></span>

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="24a78-170">Skapa en användare med behörighet</span><span class="sxs-lookup"><span data-stu-id="24a78-170">Create a user with permissions</span></span>

<span data-ttu-id="24a78-171">Skapa en databasanvändare och ge den alla behörigheter i den `tododb` databas.</span><span class="sxs-lookup"><span data-stu-id="24a78-171">Create a database user and give it all privileges in the `tododb` database.</span></span> <span data-ttu-id="24a78-172">Ersätt platshållarna `<Javaapp_user>` och `<Javaapp_password>` med dina egna unika namn.</span><span class="sxs-lookup"><span data-stu-id="24a78-172">Replace the placeholders `<Javaapp_user>` and `<Javaapp_password>` with your own unique app name.</span></span>

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

<span data-ttu-id="24a78-173">Avsluta anslutningen till servern genom att skriva `quit`.</span><span class="sxs-lookup"><span data-stu-id="24a78-173">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a><span data-ttu-id="24a78-174">Distribuera exemplet till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="24a78-174">Deploy the sample to Azure App Service</span></span>

<span data-ttu-id="24a78-175">Skapa en Azure App Service-plan med den **lediga** priser nivå med hjälp av den [az programtjänstplan skapa](/cli/azure/appservice/plan#create) CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="24a78-175">Create an Azure App Service plan with the **FREE** pricing tier using the  [az appservice plan create](/cli/azure/appservice/plan#create) CLI command.</span></span> <span data-ttu-id="24a78-176">Programtjänstplan definierar de fysiska resurserna som används som värd för dina appar.</span><span class="sxs-lookup"><span data-stu-id="24a78-176">The appservice plan defines the physical resources used to host your apps.</span></span> <span data-ttu-id="24a78-177">Alla program som har tilldelats en programtjänstplan dela dessa resurser, så att du kan spara kostnader när värd för flera appar.</span><span class="sxs-lookup"><span data-stu-id="24a78-177">All applications assigned to an appservice plan share these resources, allowing you to save cost when hosting multiple apps.</span></span> 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

<span data-ttu-id="24a78-178">När planen är klar, visas Azure CLI liknande utdata i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="24a78-178">When the plan is ready, the Azure CLI shows similar output to the following example:</span></span>

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

### <a name="create-an-azure-web-app"></a><span data-ttu-id="24a78-179">Skapa en Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="24a78-179">Create an Azure Web app</span></span>

 <span data-ttu-id="24a78-180">Använd den [az webapp skapa](/cli/azure/appservice/web#create) CLI-kommando för att skapa en web app definition i den `myAppServicePlan` App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="24a78-180">Use the [az webapp create](/cli/azure/appservice/web#create) CLI command to create a web app definition in the `myAppServicePlan` App Service plan.</span></span> <span data-ttu-id="24a78-181">Web app definitionen innehåller en URL för att få åtkomst till ditt program med och konfigurerar du flera alternativ för att distribuera din kod till Azure.</span><span class="sxs-lookup"><span data-stu-id="24a78-181">The web app definition provides a URL to access your application with and configures several options to deploy your code to Azure.</span></span> 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

<span data-ttu-id="24a78-182">Ersätt den `<app_name>` med dina egna unika namn.</span><span class="sxs-lookup"><span data-stu-id="24a78-182">Substitute the `<app_name>` placeholder with your own unique app name.</span></span> <span data-ttu-id="24a78-183">Detta unika namn är en del av standarddomännamnet för webbprogram, så att namnet måste vara unikt över alla program i Azure.</span><span class="sxs-lookup"><span data-stu-id="24a78-183">This unique name is part of the default domain name for the web app, so the name needs to be unique across all apps in Azure.</span></span> <span data-ttu-id="24a78-184">Du kan mappa en anpassad domän namnpost till webbprogrammet innan du ansluter till dina användare.</span><span class="sxs-lookup"><span data-stu-id="24a78-184">You can map a custom domain name entry to the web app before you expose it to your users.</span></span>

<span data-ttu-id="24a78-185">När web app definitionen är klar, visas Azure CLI information liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="24a78-185">When the web app definition is ready, the Azure CLI shows information similar to the following example:</span></span> 

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

### <a name="configure-java"></a><span data-ttu-id="24a78-186">Konfigurera Java</span><span class="sxs-lookup"><span data-stu-id="24a78-186">Configure Java</span></span> 

<span data-ttu-id="24a78-187">Ställa in Java runtime-konfiguration som din app behöver med den [uppdatering az apptjänst web-config](/cli/azure/appservice/web/config#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="24a78-187">Set up the Java runtime configuration that your app needs with the  [az appservice web config update](/cli/azure/appservice/web/config#update) command.</span></span>

<span data-ttu-id="24a78-188">Följande kommando konfigurerar webbprogram för att köra på en senaste Java 8 JDK och [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span><span class="sxs-lookup"><span data-stu-id="24a78-188">The following command configures the web app to run on a recent Java 8 JDK and [Apache Tomcat](http://tomcat.apache.org/) 8.0.</span></span>

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a><span data-ttu-id="24a78-189">Konfigurera appen för att använda Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="24a78-189">Configure the app to use the Azure SQL database</span></span>

<span data-ttu-id="24a78-190">Ange inställningar för webbprogram för att använda Azure MySQL-databasen som du skapade i Azure innan du kör exempelappen.</span><span class="sxs-lookup"><span data-stu-id="24a78-190">Before running the sample app, set application settings on the web app to use the Azure MySQL database you created in Azure.</span></span> <span data-ttu-id="24a78-191">Dessa egenskaper som exponeras av webbprogram som miljövariabler och åsidosätter de värden som anges i application.properties inuti paketerade webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="24a78-191">These properties are exposed to the web application as environment variables and override the values set in the application.properties inside the packaged web app.</span></span> 

<span data-ttu-id="24a78-192">Ange inställningar för program med hjälp av [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) i CLI:</span><span class="sxs-lookup"><span data-stu-id="24a78-192">Set application settings using [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) in the CLI:</span></span>

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

### <a name="get-ftp-deployment-credentials"></a><span data-ttu-id="24a78-193">Hämta autentiseringsuppgifter för FTP-distribution</span><span class="sxs-lookup"><span data-stu-id="24a78-193">Get FTP deployment credentials</span></span> 
<span data-ttu-id="24a78-194">Du kan distribuera programmet till Azure apptjänst på olika sätt, inklusive FTP, lokal Git, GitHub, Visual Studio Team Services och BitBucket.</span><span class="sxs-lookup"><span data-stu-id="24a78-194">You can deploy your application to Azure appservice in various ways including FTP, local Git, GitHub, Visual Studio Team Services, and BitBucket.</span></span> <span data-ttu-id="24a78-195">I det här exemplet FTP för att distribuera den. WAR-fil som skapats tidigare på din lokala dator till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="24a78-195">For this example, FTP to deploy the .WAR file built previously on your local machine to Azure App Service.</span></span>

<span data-ttu-id="24a78-196">Använd för att fastställa vilka autentiseringsuppgifter som skickar vidare i ett ftp-kommando till Webbappen [az apptjänst web distributionsåtgärder lista-publicering-profiler](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) kommando:</span><span class="sxs-lookup"><span data-stu-id="24a78-196">To determine what credentials to pass along in an ftp command to the Web App, Use [az appservice web deployment list-publishing-profiles](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) command:</span></span> 

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

### <a name="upload-the-app-using-ftp"></a><span data-ttu-id="24a78-197">Ladda upp appen med hjälp av FTP</span><span class="sxs-lookup"><span data-stu-id="24a78-197">Upload the app using FTP</span></span>

<span data-ttu-id="24a78-198">Använda din favorit FTP-verktyget för att distribuera den. WAR-filen till den */site/wwwroot/webapps* mapp på adressen till servern från den `URL` i det föregående kommandot.</span><span class="sxs-lookup"><span data-stu-id="24a78-198">Use your favorite FTP tool to deploy the .WAR file to the */site/wwwroot/webapps* folder on the server address taken from the `URL` field in the previous command.</span></span> <span data-ttu-id="24a78-199">Ta bort den befintliga programkatalogen (rot) och Ersätt den befintliga ROOT.war med den. WAR-fil i tidigare i kursen.</span><span class="sxs-lookup"><span data-stu-id="24a78-199">Remove the existing default (ROOT) application directory and replace the existing ROOT.war with the .WAR file built in the earlier in the tutorial.</span></span>

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected to waws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-the-web-app"></a><span data-ttu-id="24a78-200">Testa webbappen</span><span class="sxs-lookup"><span data-stu-id="24a78-200">Test the web app</span></span>

<span data-ttu-id="24a78-201">Bläddra till `http://<app_name>.azurewebsites.net/` och lägga till några åtgärder i listan.</span><span class="sxs-lookup"><span data-stu-id="24a78-201">Browse to `http://<app_name>.azurewebsites.net/` and add a few tasks to the list.</span></span> 

![Java-app som körs i Azure apptjänst](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

<span data-ttu-id="24a78-203">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="24a78-203">**Congratulations!**</span></span> <span data-ttu-id="24a78-204">Du kör en datadrivna Java-app i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="24a78-204">You're running a data-driven Java app in Azure App Service.</span></span>

## <a name="update-the-app-and-redeploy"></a><span data-ttu-id="24a78-205">Uppdatera och distribuera om appen</span><span class="sxs-lookup"><span data-stu-id="24a78-205">Update the app and redeploy</span></span>

<span data-ttu-id="24a78-206">Uppdatera programmet så att ytterligare en kolumn i todo-listan för vilken dag objektet skapades.</span><span class="sxs-lookup"><span data-stu-id="24a78-206">Update the application to include an additional column in the todo list for what day the item was created.</span></span> <span data-ttu-id="24a78-207">Källan Start hanterar uppdaterar databasschemat för dig som modellen dataändringar utan att ändra din befintliga databasposter.</span><span class="sxs-lookup"><span data-stu-id="24a78-207">Spring Boot handles updating the database schema for you as the data model changes without altering your existing database records.</span></span>

1. <span data-ttu-id="24a78-208">I det lokala systemet öppna *src/main/java/com/example/fabrikam/TodoItem.java* och Lägg till följande importer i klassen:</span><span class="sxs-lookup"><span data-stu-id="24a78-208">On your local system, open up *src/main/java/com/example/fabrikam/TodoItem.java* and add the following imports to the class:</span></span>   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. <span data-ttu-id="24a78-209">Lägg till en `String` egenskapen `timeCreated` till *src/main/java/com/example/fabrikam/TodoItem.java*, initierar med en tidsstämpel när objektet skapas.</span><span class="sxs-lookup"><span data-stu-id="24a78-209">Add a `String` property `timeCreated` to *src/main/java/com/example/fabrikam/TodoItem.java*, initializing it with a timestamp at object creation.</span></span> <span data-ttu-id="24a78-210">Lägg till Get/Set-metoder för den nya `timeCreated` egenskapen när du redigerar filen.</span><span class="sxs-lookup"><span data-stu-id="24a78-210">Add getters/setters for the new `timeCreated` property while you are editing this file.</span></span>

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

3. <span data-ttu-id="24a78-211">Uppdatera *src/main/java/com/example/fabrikam/TodoDemoController.java* med en rad i den `updateTodo` metod för att ange tidsstämpeln:</span><span class="sxs-lookup"><span data-stu-id="24a78-211">Update *src/main/java/com/example/fabrikam/TodoDemoController.java* with a line in the `updateTodo` method to set the timestamp:</span></span>

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. <span data-ttu-id="24a78-212">Lägga till stöd för det nya fältet i mallen Thymeleaf.</span><span class="sxs-lookup"><span data-stu-id="24a78-212">Add support for the new field in the Thymeleaf template.</span></span> <span data-ttu-id="24a78-213">Uppdatera *src/main/resources/templates/index.html* med ett nytt huvud för tidsstämpel och ett nytt fält att visa värdet för tidsstämpeln i varje datarad i tabellen.</span><span class="sxs-lookup"><span data-stu-id="24a78-213">Update *src/main/resources/templates/index.html* with a new table header for the timestamp, and a new field to display the value of the timestamp in each table data row.</span></span>

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

5. <span data-ttu-id="24a78-214">Återskapa programmet:</span><span class="sxs-lookup"><span data-stu-id="24a78-214">Rebuild the application:</span></span>

    ```bash
    mvnw clean package 
    ```

6. <span data-ttu-id="24a78-215">FTP-den uppdaterade. WAR som tidigare kan ta bort den befintliga *plats/wwwroot/webbappar/ROOT* directory och *ROOT.war*, och sedan ladda upp den uppdaterade. WAR-filen som ROOT.war.</span><span class="sxs-lookup"><span data-stu-id="24a78-215">FTP the updated .WAR as before, removing the existing *site/wwwroot/webapps/ROOT* directory and *ROOT.war*, then uploading the updated .WAR file as ROOT.war.</span></span> 

<span data-ttu-id="24a78-216">När du uppdaterar appen med en **Skapad** kolumnen visas nu.</span><span class="sxs-lookup"><span data-stu-id="24a78-216">When you refresh the app, a **Time Created** column is now visible.</span></span> <span data-ttu-id="24a78-217">När du lägger till en ny uppgift appen kommer att fylla i tidsstämpeln automatiskt.</span><span class="sxs-lookup"><span data-stu-id="24a78-217">When you add a new task, the app will populate the timestamp automatically.</span></span> <span data-ttu-id="24a78-218">Din befintliga aktiviteter förblir oförändrad och arbeta med appen även om den underliggande datamodellen har ändrats.</span><span class="sxs-lookup"><span data-stu-id="24a78-218">Your existing tasks remain unchanged and work with the app even though the underlying data model has changed.</span></span> 

![Java-app som har uppdaterats med en ny kolumn](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a><span data-ttu-id="24a78-220">Dataströmmen diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="24a78-220">Stream diagnostic logs</span></span> 

<span data-ttu-id="24a78-221">Du kan hämta loggarna för konsolen skickas direkt till terminalen när Java-programmet körs i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="24a78-221">While your Java application runs in Azure App Service, you can get the console logs piped directly to your terminal.</span></span> <span data-ttu-id="24a78-222">På så sätt kan du få samma diagnostiska meddelanden för att felsöka programfel.</span><span class="sxs-lookup"><span data-stu-id="24a78-222">That way, you can get the same diagnostic messages to help you debug application errors.</span></span>

<span data-ttu-id="24a78-223">Starta loggen strömning med den [az webapp loggen pilslut](/cli/azure/appservice/web/log#tail) kommando.</span><span class="sxs-lookup"><span data-stu-id="24a78-223">To start log streaming, use the [az webapp log tail](/cli/azure/appservice/web/log#tail) command.</span></span>

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="24a78-224">Hantera Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="24a78-224">Manage your Azure web app</span></span>

<span data-ttu-id="24a78-225">Gå till Azure portal för att se att webbappen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="24a78-225">Go to the Azure portal to see the web app you created.</span></span>

<span data-ttu-id="24a78-226">Logga in på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="24a78-226">To do this, sign in to [https://portal.azure.com](https://portal.azure.com).</span></span>

<span data-ttu-id="24a78-227">Klicka på **App Services** på menyn till vänster och klicka sedan på namnet på din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="24a78-227">From the left menu, click **App Service**, then click the name of your Azure web app.</span></span>

![Navigera till webbappen på Azure Portal](./media/app-service-web-tutorial-java-mysql/access-portal.png)

<span data-ttu-id="24a78-229">Sidan **Översikt** visas som standard på webbappens blad.</span><span class="sxs-lookup"><span data-stu-id="24a78-229">By default, your web app's blade shows the **Overview** page.</span></span> <span data-ttu-id="24a78-230">På den här sidan får du en översikt över hur det går för appen.</span><span class="sxs-lookup"><span data-stu-id="24a78-230">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="24a78-231">Här kan utföra du också hanteringsuppgifter som att stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="24a78-231">Here, you can also perform management tasks like stop, start, restart, and delete.</span></span> <span data-ttu-id="24a78-232">På flikarna till vänster på bladet kan du se olika konfigurationssidor som du kan öppna.</span><span class="sxs-lookup"><span data-stu-id="24a78-232">The tabs on the left side of the blade show the different configuration pages you can open.</span></span>

![App Service-blad på Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

<span data-ttu-id="24a78-234">Flikarna på bladet innehåller många bra funktioner som du kan lägga till i webbappen.</span><span class="sxs-lookup"><span data-stu-id="24a78-234">These tabs in the blade show the many great features you can add to your web app.</span></span> <span data-ttu-id="24a78-235">I listan nedan kan du se några av möjligheterna:</span><span class="sxs-lookup"><span data-stu-id="24a78-235">The following list gives you just a few of the possibilities:</span></span>
* <span data-ttu-id="24a78-236">Mappa ett anpassat DNS-namn</span><span class="sxs-lookup"><span data-stu-id="24a78-236">Map a custom DNS name</span></span>
* <span data-ttu-id="24a78-237">Bind ett anpassat SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="24a78-237">Bind a custom SSL certificate</span></span>
* <span data-ttu-id="24a78-238">Konfigurera kontinuerlig distribution</span><span class="sxs-lookup"><span data-stu-id="24a78-238">Configure continuous deployment</span></span>
* <span data-ttu-id="24a78-239">Skala upp</span><span class="sxs-lookup"><span data-stu-id="24a78-239">Scale up and out</span></span>
* <span data-ttu-id="24a78-240">Lägg till användarautentisering</span><span class="sxs-lookup"><span data-stu-id="24a78-240">Add user authentication</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="24a78-241">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="24a78-241">Clean up resources</span></span>

<span data-ttu-id="24a78-242">Om du inte behöver dessa resurser för en annan självstudiekursen (se [nästa steg](#next)), kan du ta bort dem genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="24a78-242">If you don't need these resources for another tutorial (see [Next steps](#next)), you can delete them by running the following command:</span></span> 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="24a78-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24a78-243">Next steps</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24a78-244">Skapa en MySQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="24a78-244">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="24a78-245">Ansluta en Java-exempelapp till MySQL</span><span class="sxs-lookup"><span data-stu-id="24a78-245">Connect a sample Java app to the MySQL</span></span>
> * <span data-ttu-id="24a78-246">Distribuera appen till Azure</span><span class="sxs-lookup"><span data-stu-id="24a78-246">Deploy the app to Azure</span></span>
> * <span data-ttu-id="24a78-247">Uppdatera och distribuera om appen</span><span class="sxs-lookup"><span data-stu-id="24a78-247">Update and redeploy the app</span></span>
> * <span data-ttu-id="24a78-248">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="24a78-248">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="24a78-249">Hantera appen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="24a78-249">Manage the app in the Azure portal</span></span>

<span data-ttu-id="24a78-250">Gå vidare till nästa kurs information om hur du mappar en anpassad DNS-namn till appen.</span><span class="sxs-lookup"><span data-stu-id="24a78-250">Advance to the next tutorial to learn how to map a custom DNS name to the app.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="24a78-251">Mappa ett befintligt anpassat DNS-namn till Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="24a78-251">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)