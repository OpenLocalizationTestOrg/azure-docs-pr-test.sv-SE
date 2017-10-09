---
title: "aaaCreate och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI-kommandoraden."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: b2f0b4ccf34ce88e3a5e72a64d3f8c78a5bc2a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a><span data-ttu-id="16078-103">Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="16078-103">Create and manage Azure Database for MySQL firewall rules using Azure CLI</span></span>
<span data-ttu-id="16078-104">Brandväggsregler på servernivå aktivera administratörer toomanage åtkomst tooan Azure-databas för MySQL-Server från en specifik IP-adress eller intervall av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="16078-104">Server-level firewall rules enable administrators toomanage access tooan Azure Database for MySQL Server from a specific IP address or range of IP addresses.</span></span> <span data-ttu-id="16078-105">Med lämplig Azure CLI-kommandona kan du skapa, uppdatera, ta bort, lista och visa brandväggen regler toomanage servern.</span><span class="sxs-lookup"><span data-stu-id="16078-105">Using convenient Azure CLI commands, you can create, update, delete, list, and show firewall rules toomanage your server.</span></span> <span data-ttu-id="16078-106">En översikt över Azure-databas för MySQL brandväggar, se [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="16078-106">For an overview of Azure Database for MySQL firewalls, see [Azure Database for MySQL server firewall rules](./concepts-firewall-rules.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16078-107">Krav</span><span class="sxs-lookup"><span data-stu-id="16078-107">Prerequisites</span></span>
* [<span data-ttu-id="16078-108">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="16078-108">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)
* <span data-ttu-id="16078-109">Installera Azure Python SDK för PostgreSQL och MySQL-tjänster</span><span class="sxs-lookup"><span data-stu-id="16078-109">Install Azure Python SDK for PostgreSQL and MySQL Services</span></span>
* <span data-ttu-id="16078-110">Installera hello Azure CLI-komponenten för PostgreSQL och MySQL-tjänster</span><span class="sxs-lookup"><span data-stu-id="16078-110">Install hello Azure CLI component for PostgreSQL and MySQL services</span></span>
* <span data-ttu-id="16078-111">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="16078-111">Create an Azure Database for MySQL server</span></span>

## <a name="firewall-rule-commands"></a><span data-ttu-id="16078-112">Brandväggsregel kommandon:</span><span class="sxs-lookup"><span data-stu-id="16078-112">Firewall-Rule Commands:</span></span>
<span data-ttu-id="16078-113">Hej **az mysql-brandväggsregel** kommandot används från Azure CLI toocreate, ta bort, lista, visa och uppdatera brandväggsreglerna.</span><span class="sxs-lookup"><span data-stu-id="16078-113">hello **az mysql server firewall-rule** command is used from Azure CLI toocreate, delete, list, show, and update firewall rules.</span></span>

<span data-ttu-id="16078-114">Kommandon:</span><span class="sxs-lookup"><span data-stu-id="16078-114">Commands:</span></span>
- <span data-ttu-id="16078-115">**Skapa**: skapa en brandväggsregel för Azure MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="16078-115">**create**: Create an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="16078-116">**ta bort**: ta bort en brandväggsregel för Azure MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="16078-116">**delete**: Delete an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="16078-117">**listan** : visa en lista med brandväggsregler för hello Azure MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="16078-117">**list** : List hello Azure MySQL server firewall rules.</span></span>
- <span data-ttu-id="16078-118">**Visa** : Visa hello information på en server i Azure MySQL brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="16078-118">**show** : Show hello details of an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="16078-119">**Uppdatera**: uppdatera en Azure MySQL brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="16078-119">**update**: Update an Azure MySQL server firewall rule.</span></span>

## <a name="login-tooazure-and-list-your-azure-database-for-mysql-servers"></a><span data-ttu-id="16078-120">Inloggningen tooAzure listan och din Azure-databas för MySQL-servrar</span><span class="sxs-lookup"><span data-stu-id="16078-120">Login tooAzure and List your Azure Database for MySQL Servers</span></span>
<span data-ttu-id="16078-121">På ett säkert sätt ansluta Azure CLI med Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="16078-121">Securely connect Azure CLI with your Azure account.</span></span> <span data-ttu-id="16078-122">Använd hello **az inloggningen** kommandot toodo detta.</span><span class="sxs-lookup"><span data-stu-id="16078-122">Use hello **az login** command toodo this.</span></span>

1. <span data-ttu-id="16078-123">Kör följande kommando från kommandoraden hello hello.</span><span class="sxs-lookup"><span data-stu-id="16078-123">Run hello following command from hello command line.</span></span>
```azurecli
az login
```
<span data-ttu-id="16078-124">Det här kommandot kommer att bli en kod toouse i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="16078-124">This command will output a code toouse in hello next step.</span></span>

2. <span data-ttu-id="16078-125">Använda en webbläsare tooopen hello webbsida [https://aka.ms/devicelogin](https://aka.ms/devicelogin) och ange hello kod.</span><span class="sxs-lookup"><span data-stu-id="16078-125">Use a web browser tooopen hello page [https://aka.ms/devicelogin](https://aka.ms/devicelogin) and enter hello code.</span></span>

3. <span data-ttu-id="16078-126">Logga in med dina autentiseringsuppgifter för Azure i Kommandotolken hello.</span><span class="sxs-lookup"><span data-stu-id="16078-126">At hello prompt, log in using your Azure credentials.</span></span>

4. <span data-ttu-id="16078-127">När inloggningen har behörighet, skrivs en lista över prenumerationer ut i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="16078-127">Once your login is authorized, a list of subscriptions will be printed in hello console.</span></span> <span data-ttu-id="16078-128">Kopiera hello-id för hello önskade prenumeration tooset hello aktuell prenumeration toobe används går vidare.</span><span class="sxs-lookup"><span data-stu-id="16078-128">Copy hello id of hello desired subscription tooset hello current subscription toobe used moving forward.</span></span>
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. <span data-ttu-id="16078-129">Lista över hello Azure-databaser för MySQL-servrar för din prenumeration och resurs-grupp om du är osäker på hello namn.</span><span class="sxs-lookup"><span data-stu-id="16078-129">List hello Azure Databases for MySQL servers for your subscription and resource group if you are unsure of hello names.</span></span>

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   <span data-ttu-id="16078-130">Observera hello name-attribut i hello lista, som kommer att använda toospecify vilka MySQL server toowork på.</span><span class="sxs-lookup"><span data-stu-id="16078-130">Note hello name attribute in hello listing, which will be used toospecify which MySQL server toowork on.</span></span> <span data-ttu-id="16078-131">Om det behövs, bekräfta hello information för att toousing hello namn attributet tooconfirm servernamnet är korrekt:</span><span class="sxs-lookup"><span data-stu-id="16078-131">If needed, confirm hello details for that server toousing hello name attribute tooconfirm name is correct:</span></span>

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a><span data-ttu-id="16078-132">Lista brandväggsregler på Azure-databas för MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="16078-132">List Firewall Rules on Azure Database for MySQL Server</span></span> 
<span data-ttu-id="16078-133">Med hjälp av hello servernamn och hello resursgruppens namn, lista hello befintliga serverbrandväggsreglerna på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="16078-133">Using hello server name and hello resource group name, list hello existing server firewall rules on hello server.</span></span> <span data-ttu-id="16078-134">Observera att hello server name-attribut har angetts i hello **--server** växlar och inte hello **--namnet** växla.</span><span class="sxs-lookup"><span data-stu-id="16078-134">Notice that hello server name attribute is specified in hello **--server** switch and not hello **--name** switch.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
<span data-ttu-id="16078-135">hello utdata visar en lista över hello regler om någon, som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="16078-135">hello output will list hello rules if any, by default in JSON format.</span></span> <span data-ttu-id="16078-136">Du kan använda hello växel **--utdatatabell** för ett mer lättläst tabellformat som hello utdata.</span><span class="sxs-lookup"><span data-stu-id="16078-136">You may use hello switch **--output table** for a more readable table format as hello output.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="16078-137">Skapa brandväggsregeln på Azure-databas för MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="16078-137">Create Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="16078-138">Skapa en ny brandväggsregel på hello-servern med hello Azure MySQL-servernamnet och hello resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="16078-138">Using hello Azure MySQL server name and hello resource group name, create a new firewall rule on hello server.</span></span> <span data-ttu-id="16078-139">Ange ett namn för hello regeln, hello start-IP och sista IP för hello regeln toocover tooallow åtkomst till ett intervall med IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="16078-139">Provide a name for hello rule, hello start IP, and end IP for hello rule toocover a range of IP addresses tooallow access.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
<span data-ttu-id="16078-140">För en enda IP-adress toobe åtkomst, ange hello samma adress som hello första IP- och slut-IP, som i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="16078-140">For a singular IP address toobe allowed access, provide hello same address as hello Start IP and End IP, as in this example.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
<span data-ttu-id="16078-141">Hello kommandoutdata vid lyckades, visar en lista över hello information om hello brandväggsregel som du har skapat, som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="16078-141">Upon success, hello command output will list hello details of hello firewall rule you have created, by default in JSON format.</span></span> <span data-ttu-id="16078-142">Om det finns ett fel, visar hello utdata text för felmeddelande i stället.</span><span class="sxs-lookup"><span data-stu-id="16078-142">If there is a failure, hello output will show error message text instead.</span></span>

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="16078-143">Uppdatera brandväggsregel på Azure-databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="16078-143">Update Firewall Rule on Azure Database for MySQL server</span></span> 
<span data-ttu-id="16078-144">Med hello Azure MySQL-servernamnet och hello resursgruppens namn kan uppdatera en befintlig brandväggsregel på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="16078-144">Using hello Azure MySQL server name and hello resource group name, update an existing firewall rule on hello server.</span></span> <span data-ttu-id="16078-145">Ange hello namnet för hello befintlig brandväggsregel som indata och hello start IP- och IP-attribut tooupdate.</span><span class="sxs-lookup"><span data-stu-id="16078-145">Provide hello name of hello existing firewall rule as input, and hello start IP and end IP attributes tooupdate.</span></span>
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
<span data-ttu-id="16078-146">Hello kommandoutdata vid lyckades, visar en lista över hello information om hello brandväggsregel som du har uppdaterat som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="16078-146">Upon success, hello command output will list hello details of hello firewall rule you have updated, by default in JSON format.</span></span> <span data-ttu-id="16078-147">Om det finns ett fel, visar hello utdata text för felmeddelande i stället.</span><span class="sxs-lookup"><span data-stu-id="16078-147">If there is a failure, hello output will show error message text instead.</span></span>

> [!NOTE]
> <span data-ttu-id="16078-148">Om hello brandväggsregel inte finns, kommer att skapas av hello update-kommandot.</span><span class="sxs-lookup"><span data-stu-id="16078-148">If hello firewall rule does not exist, it will be created by hello update command.</span></span>

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a><span data-ttu-id="16078-149">Visa regeldetaljer brandvägg på Azure-databas för MySQL-servern</span><span class="sxs-lookup"><span data-stu-id="16078-149">Show Firewall Rule Details on Azure Database for MySQL Server</span></span>
<span data-ttu-id="16078-150">Använda hello Azure MySQL-servernamn och hello resursgruppens namn, visa hello befintliga brandväggen Regelinformation från hello-servern.</span><span class="sxs-lookup"><span data-stu-id="16078-150">Using hello Azure MySQL server name and hello resource group name, show hello existing firewall rule details from hello server.</span></span> <span data-ttu-id="16078-151">Ange hello namnet för hello befintlig brandväggsregel som indata.</span><span class="sxs-lookup"><span data-stu-id="16078-151">Provide hello name of hello existing firewall rule as input.</span></span>
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="16078-152">Hello kommandoutdata vid lyckades, visar en lista över hello information om hello brandväggsregel som du har angett som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="16078-152">Upon success, hello command output will list hello details of hello firewall rule you have specified, by default in JSON format.</span></span> <span data-ttu-id="16078-153">Om det finns ett fel, visar hello utdata text för felmeddelande i stället.</span><span class="sxs-lookup"><span data-stu-id="16078-153">If there is a failure, hello output will show error message text instead.</span></span>

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="16078-154">Tar bort brandväggsregeln på Azure-databas för MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="16078-154">Delete Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="16078-155">Använda hello Azure MySQL-servernamn och hello resursgruppens namn, ta bort en befintlig brandväggsregel från hello-server.</span><span class="sxs-lookup"><span data-stu-id="16078-155">Using hello Azure MySQL server name and hello resource group name, remove an existing firewall rule from hello server.</span></span> <span data-ttu-id="16078-156">Ange hello namnet för hello befintlig brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="16078-156">Provide hello name of hello existing firewall rule.</span></span>
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="16078-157">Det är klart, inga utdata.</span><span class="sxs-lookup"><span data-stu-id="16078-157">Upon success, there is no output.</span></span> <span data-ttu-id="16078-158">Vid fel returneras hello felmeddelandetext.</span><span class="sxs-lookup"><span data-stu-id="16078-158">Upon failure, hello error message text will be returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="16078-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16078-159">Next steps</span></span>
- <span data-ttu-id="16078-160">Mer information om [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="16078-160">Understand more about [Azure Database for MySQL Server firewall rules](./concepts-firewall-rules.md)</span></span>
- [<span data-ttu-id="16078-161">Skapa och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="16078-161">Create and manage Azure Database for MySQL firewall rules using hello Azure portal</span></span>](./howto-manage-firewall-using-portal.md)
