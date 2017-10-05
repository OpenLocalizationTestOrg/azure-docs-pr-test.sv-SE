---
title: "Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur du skapar och hanterar Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI-kommandoraden."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 9a03722e9f71be307bdbf0b846a4cbf7b34cd7ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a><span data-ttu-id="e4c56-103">Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e4c56-103">Create and manage Azure Database for MySQL firewall rules using Azure CLI</span></span>
<span data-ttu-id="e4c56-104">Brandväggsregler på servernivå kan administratörer hantera åtkomst till en Azure-databas för MySQL-Server från en specifik IP-adress eller intervall av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="e4c56-104">Server-level firewall rules enable administrators to manage access to an Azure Database for MySQL Server from a specific IP address or range of IP addresses.</span></span> <span data-ttu-id="e4c56-105">Med hjälp av lämplig Azure CLI-kommandona, kan du skapa, uppdatera, ta bort, lista, och visa brandväggsregler för att hantera servern.</span><span class="sxs-lookup"><span data-stu-id="e4c56-105">Using convenient Azure CLI commands, you can create, update, delete, list, and show firewall rules to manage your server.</span></span> <span data-ttu-id="e4c56-106">En översikt över Azure-databas för MySQL brandväggar, se [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="e4c56-106">For an overview of Azure Database for MySQL firewalls, see [Azure Database for MySQL server firewall rules](./concepts-firewall-rules.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4c56-107">Krav</span><span class="sxs-lookup"><span data-stu-id="e4c56-107">Prerequisites</span></span>
* [<span data-ttu-id="e4c56-108">Installera Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e4c56-108">Install Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/install-azure-cli)
* <span data-ttu-id="e4c56-109">Installera Azure Python SDK för PostgreSQL och MySQL-tjänster</span><span class="sxs-lookup"><span data-stu-id="e4c56-109">Install Azure Python SDK for PostgreSQL and MySQL Services</span></span>
* <span data-ttu-id="e4c56-110">Installera Azure CLI-komponenten för PostgreSQL och MySQL-tjänster</span><span class="sxs-lookup"><span data-stu-id="e4c56-110">Install the Azure CLI component for PostgreSQL and MySQL services</span></span>
* <span data-ttu-id="e4c56-111">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="e4c56-111">Create an Azure Database for MySQL server</span></span>

## <a name="firewall-rule-commands"></a><span data-ttu-id="e4c56-112">Brandväggsregel kommandon:</span><span class="sxs-lookup"><span data-stu-id="e4c56-112">Firewall-Rule Commands:</span></span>
<span data-ttu-id="e4c56-113">Den **az mysql-brandväggsregel** kommandot används från Azure CLI för att skapa, ta bort, lista, visa och uppdatera brandväggsreglerna.</span><span class="sxs-lookup"><span data-stu-id="e4c56-113">The **az mysql server firewall-rule** command is used from Azure CLI to create, delete, list, show, and update firewall rules.</span></span>

<span data-ttu-id="e4c56-114">Kommandon:</span><span class="sxs-lookup"><span data-stu-id="e4c56-114">Commands:</span></span>
- <span data-ttu-id="e4c56-115">**Skapa**: skapa en brandväggsregel för Azure MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="e4c56-115">**create**: Create an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="e4c56-116">**ta bort**: ta bort en brandväggsregel för Azure MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="e4c56-116">**delete**: Delete an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="e4c56-117">**listan** : visa en lista med brandväggsregler för Azure MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="e4c56-117">**list** : List the Azure MySQL server firewall rules.</span></span>
- <span data-ttu-id="e4c56-118">**Visa** : Visa detaljer för en server i Azure MySQL brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="e4c56-118">**show** : Show the details of an Azure MySQL server firewall rule.</span></span>
- <span data-ttu-id="e4c56-119">**Uppdatera**: uppdatera en Azure MySQL brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="e4c56-119">**update**: Update an Azure MySQL server firewall rule.</span></span>

## <a name="login-to-azure-and-list-your-azure-database-for-mysql-servers"></a><span data-ttu-id="e4c56-120">Logga in på Azure listan och Azure-databas för MySQL-servrar</span><span class="sxs-lookup"><span data-stu-id="e4c56-120">Login to Azure and List your Azure Database for MySQL Servers</span></span>
<span data-ttu-id="e4c56-121">På ett säkert sätt ansluta Azure CLI med Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e4c56-121">Securely connect Azure CLI with your Azure account.</span></span> <span data-ttu-id="e4c56-122">Använd den **az inloggningen** kommando för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="e4c56-122">Use the **az login** command to do this.</span></span>

1. <span data-ttu-id="e4c56-123">Kör följande kommando från kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="e4c56-123">Run the following command from the command line.</span></span>
```azurecli
az login
```
<span data-ttu-id="e4c56-124">Det här kommandot genererar en kod som ska användas i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="e4c56-124">This command will output a code to use in the next step.</span></span>

2. <span data-ttu-id="e4c56-125">Använd en webbläsare för att öppna sidan [https://aka.ms/devicelogin](https://aka.ms/devicelogin) och ange koden.</span><span class="sxs-lookup"><span data-stu-id="e4c56-125">Use a web browser to open the page [https://aka.ms/devicelogin](https://aka.ms/devicelogin) and enter the code.</span></span>

3. <span data-ttu-id="e4c56-126">Logga in med dina autentiseringsuppgifter för Azure när du ombeds göra det.</span><span class="sxs-lookup"><span data-stu-id="e4c56-126">At the prompt, log in using your Azure credentials.</span></span>

4. <span data-ttu-id="e4c56-127">När inloggningen har behörighet, skrivs en lista över prenumerationer i konsolen.</span><span class="sxs-lookup"><span data-stu-id="e4c56-127">Once your login is authorized, a list of subscriptions will be printed in the console.</span></span> <span data-ttu-id="e4c56-128">Kopiera id för att ange den aktuella prenumerationen för att flytta framåt önskade prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="e4c56-128">Copy the id of the desired subscription to set the current subscription to be used moving forward.</span></span>
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. <span data-ttu-id="e4c56-129">Visa en lista med Azure-databaser för MySQL-servrar för din prenumeration och resurs-grupp om du är osäker på namnen.</span><span class="sxs-lookup"><span data-stu-id="e4c56-129">List the Azure Databases for MySQL servers for your subscription and resource group if you are unsure of the names.</span></span>

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   <span data-ttu-id="e4c56-130">Observera namnattributet i listan som ska användas för att ange vilken MySQL-server för att arbeta med.</span><span class="sxs-lookup"><span data-stu-id="e4c56-130">Note the name attribute in the listing, which will be used to specify which MySQL server to work on.</span></span> <span data-ttu-id="e4c56-131">Bekräfta informationen för servern för att använda attributet name och bekräfta att namnet är rätt om det behövs:</span><span class="sxs-lookup"><span data-stu-id="e4c56-131">If needed, confirm the details for that server to using the name attribute to confirm name is correct:</span></span>

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a><span data-ttu-id="e4c56-132">Lista brandväggsregler på Azure-databas för MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="e4c56-132">List Firewall Rules on Azure Database for MySQL Server</span></span> 
<span data-ttu-id="e4c56-133">Med hjälp av namnet på servern och resursgruppens namn, lista över befintliga serverbrandväggsreglerna på servern.</span><span class="sxs-lookup"><span data-stu-id="e4c56-133">Using the server name and the resource group name, list the existing server firewall rules on the server.</span></span> <span data-ttu-id="e4c56-134">Observera att server name-attribut har angetts i den **--server** växla och inte den **--namnet** växla.</span><span class="sxs-lookup"><span data-stu-id="e4c56-134">Notice that the server name attribute is specified in the **--server** switch and not the **--name** switch.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
<span data-ttu-id="e4c56-135">Utdata visar en lista över regler om någon, som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e4c56-135">The output will list the rules if any, by default in JSON format.</span></span> <span data-ttu-id="e4c56-136">Du kan använda växeln **--utdatatabell** för ett mer lättläst tabellformat som utdata.</span><span class="sxs-lookup"><span data-stu-id="e4c56-136">You may use the switch **--output table** for a more readable table format as the output.</span></span>
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="e4c56-137">Skapa brandväggsregeln på Azure-databas för MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="e4c56-137">Create Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="e4c56-138">Skapa en ny brandväggsregel på servern med namnet på Azure MySQL-servern och resursgruppens namn.</span><span class="sxs-lookup"><span data-stu-id="e4c56-138">Using the Azure MySQL server name and the resource group name, create a new firewall rule on the server.</span></span> <span data-ttu-id="e4c56-139">Ange ett namn för regeln, start-IP och sista IP-regeln för att täcka ett intervall med IP-adresser att tillåta åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e4c56-139">Provide a name for the rule, the start IP, and end IP for the rule to cover a range of IP addresses to allow access.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
<span data-ttu-id="e4c56-140">Ange samma adress som den första IP- och slut-IP som i det här exemplet för en enda IP-adress ska tillåtas åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e4c56-140">For a singular IP address to be allowed access, provide the same address as the Start IP and End IP, as in this example.</span></span>
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
<span data-ttu-id="e4c56-141">Kommandoutdata när lyckades, visar en lista över information om brandväggsregeln som du har skapat, som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e4c56-141">Upon success, the command output will list the details of the firewall rule you have created, by default in JSON format.</span></span> <span data-ttu-id="e4c56-142">Om det finns ett fel, visar utdata felmeddelandetext i stället.</span><span class="sxs-lookup"><span data-stu-id="e4c56-142">If there is a failure, the output will show error message text instead.</span></span>

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="e4c56-143">Uppdatera brandväggsregel på Azure-databas för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="e4c56-143">Update Firewall Rule on Azure Database for MySQL server</span></span> 
<span data-ttu-id="e4c56-144">Med Azure MySQL-servernamnet och resursgruppens namn kan uppdatera en befintlig brandväggsregel på servern.</span><span class="sxs-lookup"><span data-stu-id="e4c56-144">Using the Azure MySQL server name and the resource group name, update an existing firewall rule on the server.</span></span> <span data-ttu-id="e4c56-145">Ange namnet på befintlig brandväggsregel som indata och starta IP- och IP-attribut som ska uppdateras.</span><span class="sxs-lookup"><span data-stu-id="e4c56-145">Provide the name of the existing firewall rule as input, and the start IP and end IP attributes to update.</span></span>
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
<span data-ttu-id="e4c56-146">Kommandoutdata när lyckades, visar en lista över information om brandväggsregeln som du har uppdaterat som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e4c56-146">Upon success, the command output will list the details of the firewall rule you have updated, by default in JSON format.</span></span> <span data-ttu-id="e4c56-147">Om det finns ett fel, visar utdata felmeddelandetext i stället.</span><span class="sxs-lookup"><span data-stu-id="e4c56-147">If there is a failure, the output will show error message text instead.</span></span>

> [!NOTE]
> <span data-ttu-id="e4c56-148">Om brandväggsregeln inte finns, kommer att skapas med kommandot Uppdatera.</span><span class="sxs-lookup"><span data-stu-id="e4c56-148">If the firewall rule does not exist, it will be created by the update command.</span></span>

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a><span data-ttu-id="e4c56-149">Visa regeldetaljer brandvägg på Azure-databas för MySQL-servern</span><span class="sxs-lookup"><span data-stu-id="e4c56-149">Show Firewall Rule Details on Azure Database for MySQL Server</span></span>
<span data-ttu-id="e4c56-150">Använda Azure MySQL-servernamnet och resursgruppens namn, visa befintliga brandväggen Regelinformation från servern.</span><span class="sxs-lookup"><span data-stu-id="e4c56-150">Using the Azure MySQL server name and the resource group name, show the existing firewall rule details from the server.</span></span> <span data-ttu-id="e4c56-151">Ange namnet på befintlig brandväggsregel som indata.</span><span class="sxs-lookup"><span data-stu-id="e4c56-151">Provide the name of the existing firewall rule as input.</span></span>
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="e4c56-152">Kommandoutdata när lyckades, visar en lista över information om brandväggsregeln som du har angett som standard i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e4c56-152">Upon success, the command output will list the details of the firewall rule you have specified, by default in JSON format.</span></span> <span data-ttu-id="e4c56-153">Om det finns ett fel, visar utdata felmeddelandetext i stället.</span><span class="sxs-lookup"><span data-stu-id="e4c56-153">If there is a failure, the output will show error message text instead.</span></span>

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a><span data-ttu-id="e4c56-154">Tar bort brandväggsregeln på Azure-databas för MySQL-Server</span><span class="sxs-lookup"><span data-stu-id="e4c56-154">Delete Firewall Rule on Azure Database for MySQL Server</span></span>
<span data-ttu-id="e4c56-155">Använda Azure MySQL-servernamnet och resursgruppens namn, ta bort en befintlig brandväggsregel från servern.</span><span class="sxs-lookup"><span data-stu-id="e4c56-155">Using the Azure MySQL server name and the resource group name, remove an existing firewall rule from the server.</span></span> <span data-ttu-id="e4c56-156">Ange namnet på befintlig brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="e4c56-156">Provide the name of the existing firewall rule.</span></span>
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
<span data-ttu-id="e4c56-157">Det är klart, inga utdata.</span><span class="sxs-lookup"><span data-stu-id="e4c56-157">Upon success, there is no output.</span></span> <span data-ttu-id="e4c56-158">Felmeddelandetext vid fel, kommer att returneras.</span><span class="sxs-lookup"><span data-stu-id="e4c56-158">Upon failure, the error message text will be returned.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4c56-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4c56-159">Next steps</span></span>
- <span data-ttu-id="e4c56-160">Mer information om [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md)</span><span class="sxs-lookup"><span data-stu-id="e4c56-160">Understand more about [Azure Database for MySQL Server firewall rules](./concepts-firewall-rules.md)</span></span>
- [<span data-ttu-id="e4c56-161">Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="e4c56-161">Create and manage Azure Database for MySQL firewall rules using the Azure portal</span></span>](./howto-manage-firewall-using-portal.md)
