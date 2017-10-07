---
title: "Snabbstart: Skapa en Azure Database för MySQL-server – Azure CLI | Microsoft Docs"
description: "Denna Snabbstart beskriver hur hello toouse Azure CLI toocreate en Azure-databas för MySQL-server i en Azure-resursgrupp."
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
ms.openlocfilehash: 708d0cce12e812cb464adcf7e83e6f85c196bafe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a><span data-ttu-id="e0546-103">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e0546-103">Create an Azure Database for MySQL server using Azure CLI</span></span>
<span data-ttu-id="e0546-104">Denna Snabbstart beskriver hur toouse hello Azure CLI toocreate en Azure-databas för MySQL-server i en Azure-resursgrupp om fem minuter.</span><span class="sxs-lookup"><span data-stu-id="e0546-104">This quickstart describes how toouse hello Azure CLI toocreate an Azure Database for MySQL server in an Azure resource group in about five minutes.</span></span> <span data-ttu-id="e0546-105">hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="e0546-105">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span>

<span data-ttu-id="e0546-106">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="e0546-106">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e0546-107">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e0546-107">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="e0546-108">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="e0546-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="e0546-109">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e0546-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="e0546-110">Om du har flera prenumerationer, Välj hello lämpliga prenumeration där hello resursen finns eller faktureras för.</span><span class="sxs-lookup"><span data-stu-id="e0546-110">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span> <span data-ttu-id="e0546-111">Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).</span><span class="sxs-lookup"><span data-stu-id="e0546-111">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="e0546-112">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e0546-112">Create a resource group</span></span>
<span data-ttu-id="e0546-113">Skapa en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) med hello [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e0546-113">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) using hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="e0546-114">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="e0546-114">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="e0546-115">hello följande exempel skapar en resursgrupp med namnet `myresourcegroup` i hello `westus` plats.</span><span class="sxs-lookup"><span data-stu-id="e0546-115">hello following example creates a resource group named `myresourcegroup` in hello `westus` location.</span></span>

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="e0546-116">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="e0546-116">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="e0546-117">Skapa en Azure-databas för MySQL-server med hello **az mysql-servern skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="e0546-117">Create an Azure Database for MySQL server with hello **az mysql server create** command.</span></span> <span data-ttu-id="e0546-118">En server kan hantera flera databaser.</span><span class="sxs-lookup"><span data-stu-id="e0546-118">A server can manage multiple databases.</span></span> <span data-ttu-id="e0546-119">Normalt används en separat databas för varje projekt eller för varje användare.</span><span class="sxs-lookup"><span data-stu-id="e0546-119">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="e0546-120">hello följande exempel skapas en Azure-databas för MySQL-server finns i `westus` i hello resursgruppen `myresourcegroup` med namnet `myserver4demo`.</span><span class="sxs-lookup"><span data-stu-id="e0546-120">hello following example creates an Azure Database for MySQL server located in `westus` in hello resource group `myresourcegroup` with name `myserver4demo`.</span></span> <span data-ttu-id="e0546-121">hello-servern har en administratörsinloggning i namnet `myadmin` och lösenord `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="e0546-121">hello server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="e0546-122">hello server skapas med **grundläggande** prestandanivån och **50** compute enheter som delas mellan alla hello databaser i hello-server.</span><span class="sxs-lookup"><span data-stu-id="e0546-122">hello server is created with **Basic** performance tier and **50** compute units shared between all hello databases in hello server.</span></span> <span data-ttu-id="e0546-123">Du kan skala beräkning och lagring uppåt eller nedåt beroende på hello programbehov.</span><span class="sxs-lookup"><span data-stu-id="e0546-123">You can scale compute and storage up or down depending on hello application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="e0546-124">Konfigurera brandväggsregeln</span><span class="sxs-lookup"><span data-stu-id="e0546-124">Configure firewall rule</span></span>
<span data-ttu-id="e0546-125">Skapa en Azure-databas för MySQL servernivå brandväggsregel med hello **az mysql-brandväggsregel skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="e0546-125">Create an Azure Database for MySQL server-level firewall rule using hello **az mysql server firewall-rule create** command.</span></span> <span data-ttu-id="e0546-126">En brandväggsregel på servernivå kan ett externt program, till exempel hello **mysql.exe** kommandoradsverktyget eller MySQL arbetsstationen tooconnect tooyour server via hello Azure MySQL service brandväggen.</span><span class="sxs-lookup"><span data-stu-id="e0546-126">A server-level firewall rule allows an external application, such as hello **mysql.exe** command-line tool or MySQL Workbench tooconnect tooyour server through hello Azure MySQL service firewall.</span></span> 

<span data-ttu-id="e0546-127">hello skapas följande exempel en brandväggsregel för en fördefinierad-adressintervall som i det här exemplet är hello hela möjliga IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="e0546-127">hello following example creates a firewall rule for a predefined address range, which in this example is hello entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a><span data-ttu-id="e0546-128">Konfigurera SSL-inställningar</span><span class="sxs-lookup"><span data-stu-id="e0546-128">Configure SSL settings</span></span>
<span data-ttu-id="e0546-129">Som standard verkställs SSL-anslutningar mellan servern och dina klientprogram.</span><span class="sxs-lookup"><span data-stu-id="e0546-129">By default, SSL connections between your server and client applications are enforced.</span></span>  <span data-ttu-id="e0546-130">Detta garanterar säkerheten för ”i rörelse” data genom att kryptera hello dataströmmen över hello internet.</span><span class="sxs-lookup"><span data-stu-id="e0546-130">This ensures security of "in-motion" data by encrypting hello data stream over hello internet.</span></span>  <span data-ttu-id="e0546-131">toomake som den här snabbstartsguide lättare vi att inaktivera SSL-anslutningar för servern.</span><span class="sxs-lookup"><span data-stu-id="e0546-131">toomake this quick start easier, we disable SSL connections for your server.</span></span>  <span data-ttu-id="e0546-132">Detta rekommenderas inte för produktionsservrar.</span><span class="sxs-lookup"><span data-stu-id="e0546-132">This is not recommended for production servers.</span></span>  <span data-ttu-id="e0546-133">Mer information finns i [Konfigurera SSL-anslutning i ditt program toosecurely ansluta tooAzure databas för MySQL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="e0546-133">For more information, see [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](./howto-configure-ssl.md).</span></span>

<span data-ttu-id="e0546-134">hello inaktiverar följande exempel att framtvinga SSL på MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="e0546-134">hello following example disables enforcing SSL on your MySQL server.</span></span>
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-hello-connection-information"></a><span data-ttu-id="e0546-135">Hämta hello anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="e0546-135">Get hello connection information</span></span>

<span data-ttu-id="e0546-136">tooconnect tooyour server behöver du tooprovide värden information och åtkomst-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e0546-136">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

<span data-ttu-id="e0546-137">hello resultatet är i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e0546-137">hello result is in JSON format.</span></span> <span data-ttu-id="e0546-138">Anteckna hello **fullyQualifiedDomainName** och **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="e0546-138">Make a note of hello **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
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

## <a name="connect-toohello-server-using-hello-mysqlexe-command-line-tool"></a><span data-ttu-id="e0546-139">Ansluta toohello servern med hjälp av kommandoradsverktyget för hello mysql.exe</span><span class="sxs-lookup"><span data-stu-id="e0546-139">Connect toohello server using hello mysql.exe command-line tool</span></span>
<span data-ttu-id="e0546-140">Ansluta tooyour servern med hjälp av hello **mysql.exe** kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="e0546-140">Connect tooyour server using hello **mysql.exe** command-line tool.</span></span> <span data-ttu-id="e0546-141">Du kan hämta MySQL [här](https://dev.mysql.com/downloads/) och installera programmet på din dator.</span><span class="sxs-lookup"><span data-stu-id="e0546-141">You can download MySQL from [here](https://dev.mysql.com/downloads/) and install it on your computer.</span></span> <span data-ttu-id="e0546-142">I stället kan du också klicka hello **prova** i kodexempel eller hello `>_` knappen hello övre högra verktygsfältet i hello Azure-portalen och starta hello **Azure Cloud Shell**.</span><span class="sxs-lookup"><span data-stu-id="e0546-142">Instead you may also click hello **Try It** button on code samples, or hello  `>_` button from hello upper right toolbar in hello Azure portal, and launch hello **Azure Cloud Shell**.</span></span>

<span data-ttu-id="e0546-143">Skriv hello nästa kommandon:</span><span class="sxs-lookup"><span data-stu-id="e0546-143">Type hello next commands:</span></span> 

1. <span data-ttu-id="e0546-144">Ansluta toohello server med **mysql** kommandoradsverktyget:</span><span class="sxs-lookup"><span data-stu-id="e0546-144">Connect toohello server using **mysql** command-line tool:</span></span>
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. <span data-ttu-id="e0546-145">Visa status för servern:</span><span class="sxs-lookup"><span data-stu-id="e0546-145">View server status:</span></span>
```sql
 mysql> status
```
<span data-ttu-id="e0546-146">Om allt går bra ska hello kommandoradsverktyget utdata hello följande text:</span><span class="sxs-lookup"><span data-stu-id="e0546-146">If everything goes well, hello command-line tool should output hello following text:</span></span>

```dos
C:\Users\>mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
Enter password: ***********
Welcome toohello MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' tooclear hello current input statement.

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
> <span data-ttu-id="e0546-147">Fler kommandon finns i [referenshandboken för MySQL 5.7 – kapitel 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span><span class="sxs-lookup"><span data-stu-id="e0546-147">For additional commands, see [MySQL 5.7 Reference Manual - Chapter 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).</span></span>

## <a name="connect-toohello-server-using-hello-mysql-workbench-gui-tool"></a><span data-ttu-id="e0546-148">Ansluta toohello servern med hjälp av hello MySQL arbetsstationen GUI-verktyg</span><span class="sxs-lookup"><span data-stu-id="e0546-148">Connect toohello server using hello MySQL Workbench GUI tool</span></span>
1.  <span data-ttu-id="e0546-149">Starta hello MySQL arbetsstationen programmet på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="e0546-149">Launch hello MySQL Workbench application on your client computer.</span></span> <span data-ttu-id="e0546-150">Du kan ladda ned och installera MySQL Workbench [här](https://dev.mysql.com/downloads/workbench/).</span><span class="sxs-lookup"><span data-stu-id="e0546-150">You can download and install MySQL Workbench from [here](https://dev.mysql.com/downloads/workbench/).</span></span>

2.  <span data-ttu-id="e0546-151">I hello **installera ny anslutning** dialogrutan Ange följande information hello **parametrar** fliken:</span><span class="sxs-lookup"><span data-stu-id="e0546-151">In hello **Setup New Connection** dialog box, enter hello following information on **Parameters** tab:</span></span>

   ![konfigurera ny anslutning](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| <span data-ttu-id="e0546-153">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="e0546-153">**Setting**</span></span> | <span data-ttu-id="e0546-154">**Föreslaget värde**</span><span class="sxs-lookup"><span data-stu-id="e0546-154">**Suggested Value**</span></span> | <span data-ttu-id="e0546-155">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="e0546-155">**Description**</span></span> |
|---|---|---|
|   <span data-ttu-id="e0546-156">Anslutningsnamn</span><span class="sxs-lookup"><span data-stu-id="e0546-156">Connection Name</span></span> | <span data-ttu-id="e0546-157">Min anslutning</span><span class="sxs-lookup"><span data-stu-id="e0546-157">My Connection</span></span> | <span data-ttu-id="e0546-158">Ange ett namn på anslutningen (det kan vara vad som helst)</span><span class="sxs-lookup"><span data-stu-id="e0546-158">Specify a label for this connection (this can be anything)</span></span> |
| <span data-ttu-id="e0546-159">Anslutningsmetod</span><span class="sxs-lookup"><span data-stu-id="e0546-159">Connection Method</span></span> | <span data-ttu-id="e0546-160">välj Standard (TCP/IP)</span><span class="sxs-lookup"><span data-stu-id="e0546-160">choose Standard (TCP/IP)</span></span> | <span data-ttu-id="e0546-161">Använda TCP/IP-protokollet tooconnect tooAzure databas för MySQL ></span><span class="sxs-lookup"><span data-stu-id="e0546-161">Use TCP/IP protocol tooconnect tooAzure Database for MySQL></span></span> |
| <span data-ttu-id="e0546-162">Värdnamn</span><span class="sxs-lookup"><span data-stu-id="e0546-162">Hostname</span></span> | <span data-ttu-id="e0546-163">myserver4demo.mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="e0546-163">myserver4demo.mysql.database.azure.com</span></span> | <span data-ttu-id="e0546-164">Servernamn som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e0546-164">Server name you previously noted.</span></span> |
| <span data-ttu-id="e0546-165">Port</span><span class="sxs-lookup"><span data-stu-id="e0546-165">Port</span></span> | <span data-ttu-id="e0546-166">3306</span><span class="sxs-lookup"><span data-stu-id="e0546-166">3306</span></span> | <span data-ttu-id="e0546-167">hello-standardporten för MySQL används.</span><span class="sxs-lookup"><span data-stu-id="e0546-167">hello default port for MySQL is used.</span></span> |
| <span data-ttu-id="e0546-168">Användarnamn</span><span class="sxs-lookup"><span data-stu-id="e0546-168">Username</span></span> | myadmin@myserver4demo | <span data-ttu-id="e0546-169">hello inloggning för serveradministratör du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e0546-169">hello server admin login you previously noted.</span></span> |
| <span data-ttu-id="e0546-170">Lösenord</span><span class="sxs-lookup"><span data-stu-id="e0546-170">Password</span></span> | **** | <span data-ttu-id="e0546-171">Använd hello admin kontolösenord som du tidigare har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="e0546-171">Use hello admin account password you configured earlier.</span></span> |

<span data-ttu-id="e0546-172">Klicka på **Testanslutningen** tootest om alla parametrar är korrekt konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="e0546-172">Click **Test Connection** tootest if all parameters are correctly configured.</span></span>
<span data-ttu-id="e0546-173">Nu kan du klicka på hello anslutning toosuccessfully ansluta toohello server.</span><span class="sxs-lookup"><span data-stu-id="e0546-173">Now, you can click hello connection toosuccessfully connect toohello server.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="e0546-174">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="e0546-174">Clean up resources</span></span>
<span data-ttu-id="e0546-175">Om du inte behöver dessa resurser för en annan Snabbstartsguide, kan du ta bort dem genom att göra hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e0546-175">If you don't need these resources for another quickstart/tutorial, you can delete them by doing hello following command:</span></span> 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a><span data-ttu-id="e0546-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0546-176">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e0546-177">Utforma en MySQL-databas med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e0546-177">Design a MySQL Database with Azure CLI</span></span>](./tutorial-design-database-using-cli.md)
