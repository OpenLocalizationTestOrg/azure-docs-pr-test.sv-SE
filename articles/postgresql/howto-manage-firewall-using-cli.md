---
title: "aaaCreate och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI-kommandoraden."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 06e34c9e3996c2ec3df63191d794bc34d0ca0cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a><span data-ttu-id="fe62e-103">Skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fe62e-103">Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI</span></span>
<span data-ttu-id="fe62e-104">Brandväggsregler på servernivå aktivera administratörer toomanage åtkomst tooan Azure-databas för PostgreSQL-Server från en specifik IP-adress eller intervall av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="fe62e-104">Server-level firewall rules enable administrators toomanage access tooan Azure Database for PostgreSQL Server from a specific IP address or range of IP addresses.</span></span> <span data-ttu-id="fe62e-105">Med lämplig Azure CLI-kommandona kan du skapa, uppdatera, ta bort, lista och visa brandväggen regler toomanage servern.</span><span class="sxs-lookup"><span data-stu-id="fe62e-105">Using convenient Azure CLI commands, you can create, update, delete, list, and show firewall rules toomanage your server.</span></span> <span data-ttu-id="fe62e-106">En översikt över Azure-databas för PostgreSQL brandväggar, se [Azure-databas för PostgreSQL serverbrandväggsreglerna](concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="fe62e-106">For an overview of Azure Database for PostgreSQL firewalls, see [Azure Database for PostgreSQL Server firewall rules](concepts-firewall-rules.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe62e-107">Krav</span><span class="sxs-lookup"><span data-stu-id="fe62e-107">Prerequisites</span></span>
<span data-ttu-id="fe62e-108">toostep via den här hur tooguide behöver du:</span><span class="sxs-lookup"><span data-stu-id="fe62e-108">toostep through this how-tooguide, you need:</span></span>
- <span data-ttu-id="fe62e-109">En [Azure-databas för PostgreSQL-server och databas](quickstart-create-server-database-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="fe62e-109">An [Azure Database for PostgreSQL server and database](quickstart-create-server-database-azure-cli.md)</span></span>
- <span data-ttu-id="fe62e-110">Installera [Azure CLI 2.0](/cli/azure/install-azure-cli) kommandoraden verktyg eller Använd hello Azure Cloud Shell i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="fe62e-110">Install [Azure CLI 2.0](/cli/azure/install-azure-cli) command line utility or use hello Azure Cloud Shell in hello browser.</span></span>

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a><span data-ttu-id="fe62e-111">Konfigurera brandväggsregler för Azure-databas för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="fe62e-111">Configure firewall rules for Azure Database for PostgreSQL</span></span>
<span data-ttu-id="fe62e-112">Hej [az postgres-brandväggsregel](/cli/azure/postgres/server/firewall-rule) kommandon är används tooconfigure brandväggsregler.</span><span class="sxs-lookup"><span data-stu-id="fe62e-112">hello [az postgres server firewall-rule](/cli/azure/postgres/server/firewall-rule) commands are used tooconfigure firewall rules.</span></span>

## <a name="list-firewall-rules"></a><span data-ttu-id="fe62e-113">Lista brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="fe62e-113">List firewall rules</span></span> 
<span data-ttu-id="fe62e-114">toolist hello befintliga server brandväggsregler på hello-servern kör hello [az postgres brandväggsregel serverlista](/cli/azure/postgres/server/firewall-rule#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="fe62e-114">toolist hello existing server firewall rules on hello server, run hello [az postgres server firewall-rule list](/cli/azure/postgres/server/firewall-rule#list) command.</span></span>
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
<span data-ttu-id="fe62e-115">hello utdata visar hello regler om någon, som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="fe62e-115">hello output lists hello rules if any, by default in JSON format.</span></span> <span data-ttu-id="fe62e-116">Du kan använda hello växel `--output table` för ett mer lättläst tabellformat som hello utdata.</span><span class="sxs-lookup"><span data-stu-id="fe62e-116">You may use hello switch `--output table` for a more readable table format as hello output.</span></span>
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a><span data-ttu-id="fe62e-117">Skapa brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="fe62e-117">Create firewall rule</span></span>
<span data-ttu-id="fe62e-118">toocreate en ny brandväggsregel på hello-servern kör hello [az postgres-brandväggsregel skapa](/cli/azure/postgres/server/firewall-rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="fe62e-118">toocreate a new firewall rule on hello server, run hello [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> 

<span data-ttu-id="fe62e-119">Det här exemplet tillåter en mängd alla IP-adresser tooaccess hello server **mypgserver 20170401.postgres.database.azure.com**</span><span class="sxs-lookup"><span data-stu-id="fe62e-119">This example allows a range of all IP addresses tooaccess hello server **mypgserver-20170401.postgres.database.azure.com**</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
<span data-ttu-id="fe62e-120">Ange tooallow en enda IP-adress tooaccess hello samma adress som hello första IP- och slut-IP, som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="fe62e-120">tooallow a singular IP address tooaccess, provide hello same address as hello Start IP and End IP, as in this example.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "AllowSingleIpAddress" --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
<span data-ttu-id="fe62e-121">Vid lyckades visar hello kommandoutdata hello information om hello brandväggsregel som du har skapat, som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="fe62e-121">Upon success, hello command output lists hello details of hello firewall rule you have created, by default in JSON format.</span></span> <span data-ttu-id="fe62e-122">Om det finns ett fel, utdata hello showserror meddelandetexten i stället.</span><span class="sxs-lookup"><span data-stu-id="fe62e-122">If there is a failure, hello output showserror message text instead.</span></span>

## <a name="update-firewall-rule"></a><span data-ttu-id="fe62e-123">Uppdatera brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="fe62e-123">Update firewall rule</span></span> 
<span data-ttu-id="fe62e-124">Uppdatera en befintlig brandväggsregel på hello-servern med [az postgres server-brandväggsregel uppdateringen](/cli/azure/postgres/server/firewall-rule#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="fe62e-124">Update an existing firewall rule on hello server using [az postgres server firewall-rule update](/cli/azure/postgres/server/firewall-rule#update) command.</span></span> <span data-ttu-id="fe62e-125">Ange hello namnet för hello befintlig brandväggsregel som indata och hello start IP- och IP-attribut tooupdate.</span><span class="sxs-lookup"><span data-stu-id="fe62e-125">Provide hello name of hello existing firewall rule as input, and hello start IP and end IP attributes tooupdate.</span></span>
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
<span data-ttu-id="fe62e-126">Vid lyckades visar hello kommandoutdata hello information om hello brandväggsregel som du har uppdaterat som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="fe62e-126">Upon success, hello command output lists hello details of hello firewall rule you have updated, by default in JSON format.</span></span> <span data-ttu-id="fe62e-127">Om det finns ett fel, utdata hello showserror meddelandetexten i stället.</span><span class="sxs-lookup"><span data-stu-id="fe62e-127">If there is a failure, hello output showserror message text instead.</span></span>
> [!NOTE]
> <span data-ttu-id="fe62e-128">Om hello brandväggsregel inte finns, hämtar den skapats av hello update-kommandot.</span><span class="sxs-lookup"><span data-stu-id="fe62e-128">If hello firewall rule does not exist, it gets created by hello update command.</span></span>

## <a name="show-firewall-rule-details"></a><span data-ttu-id="fe62e-129">Visa brandväggen regeldetaljer</span><span class="sxs-lookup"><span data-stu-id="fe62e-129">Show firewall rule details</span></span>
<span data-ttu-id="fe62e-130">Du kan också visa Regelinformation om för en server för befintliga hello-brandväggen genom att köra [az postgres server-brandväggsregel visa](/cli/azure/postgres/server/firewall-rule#show) kommando.</span><span class="sxs-lookup"><span data-stu-id="fe62e-130">You can also show hello existing firewall rule details for a server by running [az postgres server firewall-rule show](/cli/azure/postgres/server/firewall-rule#show) command.</span></span>
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
<span data-ttu-id="fe62e-131">Vid lyckades visar hello kommandoutdata hello information om hello brandväggsregel som du har angett som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="fe62e-131">Upon success, hello command output lists hello details of hello firewall rule you have specified, by default in JSON format.</span></span> <span data-ttu-id="fe62e-132">Om det finns ett fel, utdata hello showserror meddelandetexten i stället.</span><span class="sxs-lookup"><span data-stu-id="fe62e-132">If there is a failure, hello output showserror message text instead.</span></span>

## <a name="delete-firewall-rule"></a><span data-ttu-id="fe62e-133">Ta bort brandväggsregel</span><span class="sxs-lookup"><span data-stu-id="fe62e-133">Delete firewall rule</span></span>
<span data-ttu-id="fe62e-134">toorevoke åtkomst för ett IP-adressintervall från hello-servern, ta bort en befintlig brandväggsregel genom att köra hello [az postgres-brandväggsregel ta bort](/cli/azure/postgres/server/firewall-rule#delete) kommando.</span><span class="sxs-lookup"><span data-stu-id="fe62e-134">toorevoke access for an IP range from hello server, delete an existing firewall rule by executing hello [az postgres server firewall-rule delete](/cli/azure/postgres/server/firewall-rule#delete) command.</span></span> <span data-ttu-id="fe62e-135">Ange hello namnet för hello befintlig brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="fe62e-135">Provide hello name of hello existing firewall rule.</span></span>
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
<span data-ttu-id="fe62e-136">Det är klart, inga utdata.</span><span class="sxs-lookup"><span data-stu-id="fe62e-136">Upon success, there is no output.</span></span> <span data-ttu-id="fe62e-137">Vid fel returneras hello felmeddelandetext.</span><span class="sxs-lookup"><span data-stu-id="fe62e-137">Upon failure, hello error message text is returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe62e-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe62e-138">Next steps</span></span>
- <span data-ttu-id="fe62e-139">På liknande sätt kan du använda en webbläsare för[skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hello Azure-portalen](howto-manage-firewall-using-portal.md)</span><span class="sxs-lookup"><span data-stu-id="fe62e-139">Similarly, you can use a web browser too[Create and manage Azure Database for PostgreSQL firewall rules using hello Azure portal](howto-manage-firewall-using-portal.md)</span></span>
- <span data-ttu-id="fe62e-140">Mer information om [Azure-databas för PostgreSQL serverbrandväggsreglerna](concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="fe62e-140">Understand more about [Azure Database for PostgreSQL Server firewall rules](concepts-firewall-rules.md)</span></span>
- <span data-ttu-id="fe62e-141">Hjälp med att ansluta tooan Azure-databas för PostgreSQL-servern finns [anslutningsbibliotek för Azure-databas för PostgreSQL](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="fe62e-141">For help in connecting tooan Azure Database for PostgreSQL server, see [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
