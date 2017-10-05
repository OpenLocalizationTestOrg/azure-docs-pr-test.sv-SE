---
title: "Snabbstart: Skapa en Azure Database för MySQL-server – Azure CLI | Microsoft Docs"
description: "I den här snabbstarten beskrivs hur du använder Azure CLI till att skapa en Azure Database för MySQL-server i en Azure-resursgrupp."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: hero-article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 04fc441aee7a4c8adc4f02d5e51b2d9e64400f55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="d6617-103">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d6617-103">Create an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="d6617-104">I den här snabbstarten beskrivs hur du använder Azure CLI till att skapa en Azure Database för MySQL-server i en Azure-resursgrupp på ungefär fem minuter.</span><span class="sxs-lookup"><span data-stu-id="d6617-104">This quickstart describes how to use the Azure CLI to create an Azure Database for MySQL server in an Azure resource group in about five minutes.</span></span> <span data-ttu-id="d6617-105">Azure CLI används för att skapa och hantera Azure-resurser från kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="d6617-105">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span>

<span data-ttu-id="d6617-106">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="d6617-106">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d6617-107">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d6617-107">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="d6617-108">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="d6617-108">Run `az --version` to find the version.</span></span> <span data-ttu-id="d6617-109">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d6617-109">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="d6617-110">Om du har flera prenumerationer väljer du en lämplig prenumerationen där resursen ligger eller faktureras.</span><span class="sxs-lookup"><span data-stu-id="d6617-110">If you have multiple subscriptions, choose the appropriate subscription in which the resource exists or is billed for.</span></span> <span data-ttu-id="d6617-111">Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).</span><span class="sxs-lookup"><span data-stu-id="d6617-111">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="d6617-112">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d6617-112">Create a resource group</span></span>
<span data-ttu-id="d6617-113">Skapa en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) med kommandot [az group create](https://docs.microsoft.com/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="d6617-113">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) using the [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="d6617-114">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="d6617-114">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="d6617-115">I följande exempel skapas en resursgrupp med namnet `myresourcegroup` på platsen `westus`.</span><span class="sxs-lookup"><span data-stu-id="d6617-115">The following example creates a resource group named `myresourcegroup` in the `westus` location.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="d6617-116">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="d6617-116">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="d6617-117">Skapa en Azure Database för MySQL-server med kommandot **az sql server create**.</span><span class="sxs-lookup"><span data-stu-id="d6617-117">Create an Azure Database for MySQL server with the **az mysql server create** command.</span></span> <span data-ttu-id="d6617-118">En server kan hantera flera databaser.</span><span class="sxs-lookup"><span data-stu-id="d6617-118">A server can manage multiple databases.</span></span> <span data-ttu-id="d6617-119">Normalt används en separat databas för varje projekt eller för varje användare.</span><span class="sxs-lookup"><span data-stu-id="d6617-119">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="d6617-120">I följande exempel skapas en Azure Database för MySQL-server i `westus` i resursgruppen `myresourcegroup` med namnet `myserver4demo`.</span><span class="sxs-lookup"><span data-stu-id="d6617-120">The following example creates an Azure Database for MySQL server located in `westus` in the resource group `myresourcegroup` with name `myserver4demo`.</span></span> <span data-ttu-id="d6617-121">Servern har en administratörsinloggning med namnet `myadmin` och lösenordet `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="d6617-121">The server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="d6617-122">Servern skapas med prestandanivån **Basic** och **50** beräkningsenheter som delas mellan alla databaser på servern.</span><span class="sxs-lookup"><span data-stu-id="d6617-122">The server is created with **Basic** performance tier and **50** compute units shared between all the databases in the server.</span></span> <span data-ttu-id="d6617-123">Du kan skala beräkning och lagring uppåt eller nedåt beroende på behoven i dina appar.</span><span class="sxs-lookup"><span data-stu-id="d6617-123">You can scale compute and storage up or down depending on the application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="d6617-124">Konfigurera brandväggsregeln</span><span class="sxs-lookup"><span data-stu-id="d6617-124">Configure firewall rule</span></span>
<span data-ttu-id="d6617-125">Skapa en Azure Database för MySQL-brandväggsregel på servernivå med kommandot **az mysql server firewall-rule create**.</span><span class="sxs-lookup"><span data-stu-id="d6617-125">Create an Azure Database for MySQL server-level firewall rule using the **az mysql server firewall-rule create** command.</span></span> <span data-ttu-id="d6617-126">En brandväggsregel på servernivå gör att externa program, som kommandoradsverktyget **mysql.exe** eller MySQL Workbench kan ansluta till servern via Azure MySQL-tjänstens brandvägg.</span><span class="sxs-lookup"><span data-stu-id="d6617-126">A server-level firewall rule allows an external application, such as the **mysql.exe** command-line tool or MySQL Workbench to connect to your server through the Azure MySQL service firewall.</span></span> 

<span data-ttu-id="d6617-127">I följande exempel skapas en brandväggsregel för ett fördefinierat adressintervall, som i det här exemplet är hela det möjliga intervallet med IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="d6617-127">The following example creates a firewall rule for a predefined address range, which in this example is the entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a><span data-ttu-id="d6617-128">Konfigurera SSL-inställningar</span><span class="sxs-lookup"><span data-stu-id="d6617-128">Configure SSL settings</span></span>
<span data-ttu-id="d6617-129">Som standard verkställs SSL-anslutningar mellan servern och dina klientprogram.</span><span class="sxs-lookup"><span data-stu-id="d6617-129">By default, SSL connections between your server and client applications are enforced.</span></span>  <span data-ttu-id="d6617-130">Detta garanterar säkerheten för data ”i rörelse” genom att dataströmmen över internet krypteras.</span><span class="sxs-lookup"><span data-stu-id="d6617-130">This ensures security of "in-motion" data by encrypting the data stream over the internet.</span></span>  <span data-ttu-id="d6617-131">För att förenkla snabbstarten avaktiverar vi SSL-anslutningar för din server.</span><span class="sxs-lookup"><span data-stu-id="d6617-131">To make this quick start easier, we disable SSL connections for your server.</span></span>  <span data-ttu-id="d6617-132">Detta rekommenderas inte för produktionsservrar.</span><span class="sxs-lookup"><span data-stu-id="d6617-132">This is not recommended for production servers.</span></span>  <span data-ttu-id="d6617-133">Se [Konfigurera SSL-anslutning i din app för säker anslutning till Azure Database för MySQL](./howto-configure-ssl.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="d6617-133">For more information, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](./howto-configure-ssl.md).</span></span>

<span data-ttu-id="d6617-134">I följande exempel inaktiveras SSL på MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="d6617-134">The following example disables enforcing SSL on your MySQL server.</span></span>
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-the-connection-information"></a><span data-ttu-id="d6617-135">Hämta anslutningsinformationen</span><span class="sxs-lookup"><span data-stu-id="d6617-135">Get the connection information</span></span>

<span data-ttu-id="d6617-136">För att ansluta till servern måste du ange värddatorinformationen och autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="d6617-136">To connect to your server, you need to provide host information and access credentials.</span></span>

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

<span data-ttu-id="d6617-137">Resultatet är i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="d6617-137">The result is in JSON format.</span></span> <span data-ttu-id="d6617-138">Anteckna **fullyQualifiedDomainName** och **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="d6617-138">Make a note of the **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "myserver4demo.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/myserver4demo",
  "location": "westus",
  "name": "myserver4demo",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-to-the-server-using-the-mysqlexe-command-line-tool"></a><span data-ttu-id="d6617-139">Anslut till servern med kommandoradsverktyget mysql.exe</span><span class="sxs-lookup"><span data-stu-id="d6617-139">Connect to the server using the mysql.exe command-line tool</span></span>
<span data-ttu-id="d6617-140">Anslut till servern med kommandoradverktyget **mysql.exe**.</span><span class="sxs-lookup"><span data-stu-id="d6617-140">Connect to your server using the **mysql.exe** command-line tool.</span></span> <span data-ttu-id="d6617-141">Du kan hämta MySQL [här](https://dev.mysql.com/downloads/) och installera programmet på din dator.</span><span class="sxs-lookup"><span data-stu-id="d6617-141">You can download MySQL from [here](https://dev.mysql.com/downloads/) and install it on your computer.</span></span> <span data-ttu-id="d6617-142">Du kan även klicka på knappen **Prova** på kodexemplen eller på `>_`-knappen i det övre högra verktygsfältet i Azure-portalen och starta **Azure Cloud Shell**.</span><span class="sxs-lookup"><span data-stu-id="d6617-142">Instead you may also click the **Try It** button on code samples, or the  `>_` button from the upper right toolbar in the Azure portal, and launch the **Azure Cloud Shell**.</span></span>

<span data-ttu-id="d6617-143">Skriv nästa kommandon:</span><span class="sxs-lookup"><span data-stu-id="d6617-143">Type the next commands:</span></span> 

1. <span data-ttu-id="d6617-144">Anslut till servern med kommandoradsverktyget **mysql**:</span><span class="sxs-lookup"><span data-stu-id="d6617-144">Connect to the server using **mysql** command-line tool:</span></span>
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. <span data-ttu-id="d6617-145">Visa status för servern:</span><span class="sxs-lookup"><span data-stu-id="d6617-145">View server status:</span></span>
```sql
 mysql> status
```
<span data-ttu-id="d6617-146">Om allt går väl kommer kommandoradverktyget returnera följande text:</span><span class="sxs-lookup"><span data-stu-id="d6617-146">If everything goes well, the command-line tool should output the following text:</span></span>

```dos
C:\Users\>mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
Enter password: ***********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             myserver4demo.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> <span data-ttu-id="d6617-147">För fler kommandon, se [Referensmanual för MySQL 5.7 - kapitel 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span><span class="sxs-lookup"><span data-stu-id="d6617-147">For additional commands, see [MySQL 5.7 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span></span>

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a><span data-ttu-id="d6617-148">Anslut till servern med verktyget MySQL Workbench GUI</span><span class="sxs-lookup"><span data-stu-id="d6617-148">Connect to the server using the MySQL Workbench GUI tool</span></span>
1.  <span data-ttu-id="d6617-149">Starta programmet MySQL Workbench på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="d6617-149">Launch the MySQL Workbench application on your client computer.</span></span> <span data-ttu-id="d6617-150">Du kan ladda ned och installera MySQL Workbench [här](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="d6617-150">You can download and install MySQL Workbench from [here](https://dev.mysql.com/downloads/workbench/).</span></span>

2.  <span data-ttu-id="d6617-151">I dialogrutan **Konfigurera ny anslutning** anger du följande information på fliken **Parametrar**:</span><span class="sxs-lookup"><span data-stu-id="d6617-151">In the **Setup New Connection** dialog box, enter the following information on **Parameters** tab:</span></span>

   ![konfigurera ny anslutning](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| <span data-ttu-id="d6617-153">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="d6617-153">**Setting**</span></span> | <span data-ttu-id="d6617-154">**Föreslaget värde**</span><span class="sxs-lookup"><span data-stu-id="d6617-154">**Suggested Value**</span></span> | <span data-ttu-id="d6617-155">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="d6617-155">**Description**</span></span> |
|---|---|---|
|   <span data-ttu-id="d6617-156">Anslutningsnamn</span><span class="sxs-lookup"><span data-stu-id="d6617-156">Connection Name</span></span> | <span data-ttu-id="d6617-157">Min anslutning</span><span class="sxs-lookup"><span data-stu-id="d6617-157">My Connection</span></span> | <span data-ttu-id="d6617-158">Ange ett namn på anslutningen (det kan vara vad som helst)</span><span class="sxs-lookup"><span data-stu-id="d6617-158">Specify a label for this connection (this can be anything)</span></span> |
| <span data-ttu-id="d6617-159">Anslutningsmetod</span><span class="sxs-lookup"><span data-stu-id="d6617-159">Connection Method</span></span> | <span data-ttu-id="d6617-160">välj Standard (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="d6617-160">choose Standard (TCP/IP)</span></span> | <span data-ttu-id="d6617-161">Använda TCP/IP-protokollet för att ansluta till Azure Database för MySQL ></span><span class="sxs-lookup"><span data-stu-id="d6617-161">Use TCP/IP protocol to connect to Azure Database for MySQL></span></span> |
| <span data-ttu-id="d6617-162">Värdnamn</span><span class="sxs-lookup"><span data-stu-id="d6617-162">Hostname</span></span> | <span data-ttu-id="d6617-163">myserver4demo.mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="d6617-163">myserver4demo.mysql.database.azure.com</span></span> | <span data-ttu-id="d6617-164">Servernamn som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="d6617-164">Server name you previously noted.</span></span> |
| <span data-ttu-id="d6617-165">Port</span><span class="sxs-lookup"><span data-stu-id="d6617-165">Port</span></span> | <span data-ttu-id="d6617-166">3306</span><span class="sxs-lookup"><span data-stu-id="d6617-166">3306</span></span> | <span data-ttu-id="d6617-167">Standardporten för MySQL används.</span><span class="sxs-lookup"><span data-stu-id="d6617-167">The default port for MySQL is used.</span></span> |
| <span data-ttu-id="d6617-168">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="d6617-168">Username</span></span> | myadmin@myserver4demo | <span data-ttu-id="d6617-169">Inloggning för serveradministratör som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="d6617-169">The server admin login you previously noted.</span></span> |
| <span data-ttu-id="d6617-170">Lösenord</span><span class="sxs-lookup"><span data-stu-id="d6617-170">Password</span></span> | **** | <span data-ttu-id="d6617-171">Ange lösenordet som du konfigurerade tidigare.</span><span class="sxs-lookup"><span data-stu-id="d6617-171">Use the admin account password you configured earlier.</span></span> |

<span data-ttu-id="d6617-172">Klicka på **Testanslutning** för att testa om alla parametrar är rätt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="d6617-172">Click **Test Connection** to test if all parameters are correctly configured.</span></span>
<span data-ttu-id="d6617-173">Nu kan du klicka på anslutningen för att ansluta till servern.</span><span class="sxs-lookup"><span data-stu-id="d6617-173">Now, you can click the connection to successfully connect to the server.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="d6617-174">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="d6617-174">Clean up resources</span></span>
<span data-ttu-id="d6617-175">Om du inte behöver de här resurserna för en annan snabbstart/självstudie kan du ta bort dem genom att utföra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d6617-175">If you don't need these resources for another quickstart/tutorial, you can delete them by doing the following command:</span></span> 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a><span data-ttu-id="d6617-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6617-176">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d6617-177">Utforma en MySQL-databas med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d6617-177">Design a MySQL Database with Azure CLI</span></span>](./tutorial-design-database-using-cli.md)
