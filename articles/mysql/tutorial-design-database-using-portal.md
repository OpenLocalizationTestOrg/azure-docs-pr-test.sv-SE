---
title: "Utforma din första Azure-databas för MySQL - databas i Azure Portal | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur du skapar och hanterar Azure-databas för MySQL-server och databas med hjälp av Azure Portal."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/06/2017
ms.custom: mvc
ms.openlocfilehash: c7b76cacbdc4e483353f64cc4e50c974867bb5b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="f225a-103">Utforma din första Azure-databas för MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="f225a-103">Design your first Azure Database for MySQL database</span></span>
<span data-ttu-id="f225a-104">Azure Database för MySQL är en hanterad tjänst som låter dig köra, hantera och skala högtillgängliga MySQL-databaser i molnet.</span><span class="sxs-lookup"><span data-stu-id="f225a-104">Azure Database for MySQL is a managed service that enables you to run, manage, and scale highly available MySQL databases in the cloud.</span></span> <span data-ttu-id="f225a-105">Med Azure-portalen kan du enkelt hantera servern och utforma en databas.</span><span class="sxs-lookup"><span data-stu-id="f225a-105">Using the Azure portal, you can easily manage your server and design a database.</span></span>

<span data-ttu-id="f225a-106">I kursen får du använder Azure-portalen att lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="f225a-106">In this tutorial, you use the Azure portal to learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f225a-107">Skapa en Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="f225a-107">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="f225a-108">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="f225a-108">Configure the server firewall</span></span>
> * <span data-ttu-id="f225a-109">Kommandoradsverktyget mysql för att skapa en databas</span><span class="sxs-lookup"><span data-stu-id="f225a-109">Use mysql command-line tool to create a database</span></span>
> * <span data-ttu-id="f225a-110">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="f225a-110">Load sample data</span></span>
> * <span data-ttu-id="f225a-111">Frågedata</span><span class="sxs-lookup"><span data-stu-id="f225a-111">Query data</span></span>
> * <span data-ttu-id="f225a-112">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="f225a-112">Update data</span></span>
> * <span data-ttu-id="f225a-113">Återställa data</span><span class="sxs-lookup"><span data-stu-id="f225a-113">Restore data</span></span>

## <a name="sign-in-to-the-azure-portal"></a><span data-ttu-id="f225a-114">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f225a-114">Sign in to the Azure portal</span></span>
<span data-ttu-id="f225a-115">Öppna valfri webbläsare och gå den [Microsoft Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f225a-115">Open your favorite web browser, and visit the [Microsoft Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="f225a-116">Ange dina autentiseringsuppgifter och logga in på portalen.</span><span class="sxs-lookup"><span data-stu-id="f225a-116">Enter your credentials to sign in to the portal.</span></span> <span data-ttu-id="f225a-117">Standardvyn är instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f225a-117">The default view is your service dashboard.</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="f225a-118">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="f225a-118">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="f225a-119">En Azure Database för MySQL-server skapas med en definierad uppsättning [compute- och lagringsresurser](./concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="f225a-119">An Azure Database for MySQL server is created with a defined set of [compute and storage resources](./concepts-compute-unit-and-storage.md).</span></span> <span data-ttu-id="f225a-120">Servern skapas inom en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="f225a-120">The server is created within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

1. <span data-ttu-id="f225a-121">Gå till **databaser** > **Azure MySQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="f225a-121">Navigate to **Databases** > **Azure Database for MySQL**.</span></span> <span data-ttu-id="f225a-122">Om du inte hittar MySQL-servern under **databaser** kategori, klickar du på **se alla** att visa alla tillgängliga databastjänster.</span><span class="sxs-lookup"><span data-stu-id="f225a-122">If you cannot find MySQL Server under **Databases** category, click **See all** to show all available database services.</span></span> <span data-ttu-id="f225a-123">Du kan också skriva **Azure-databas för MySQL** i sökrutan för att snabbt hitta tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f225a-123">You can also type **Azure Database for MySQL** in the search box to quickly find the service.</span></span>
<span data-ttu-id="f225a-124">![2-1 navigerar du till MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span><span class="sxs-lookup"><span data-stu-id="f225a-124">![2-1 Navigate to MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span></span>

2. <span data-ttu-id="f225a-125">Klicka på **Azure-databas för MySQL** panelen och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="f225a-125">Click **Azure Database for MySQL** tile, and then click **Create**.</span></span>

<span data-ttu-id="f225a-126">I vårt exempel, fyller du i Azure-databasen för MySQL formulär med följande information:</span><span class="sxs-lookup"><span data-stu-id="f225a-126">In our example, fill out the Azure Database for MySQL form with the following information:</span></span>

| <span data-ttu-id="f225a-127">**Inställning**</span><span class="sxs-lookup"><span data-stu-id="f225a-127">**Setting**</span></span> | <span data-ttu-id="f225a-128">**Föreslaget värde**</span><span class="sxs-lookup"><span data-stu-id="f225a-128">**Suggested value**</span></span> | <span data-ttu-id="f225a-129">**Fältbeskrivning**</span><span class="sxs-lookup"><span data-stu-id="f225a-129">**Field Description**</span></span> |
|---|---|---|
| <span data-ttu-id="f225a-130">*Servernamn*</span><span class="sxs-lookup"><span data-stu-id="f225a-130">*Server name*</span></span> | <span data-ttu-id="f225a-131">myserver4demo</span><span class="sxs-lookup"><span data-stu-id="f225a-131">myserver4demo</span></span>  | <span data-ttu-id="f225a-132">Servernamnet måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="f225a-132">Server name has to be globally unique.</span></span> |
| <span data-ttu-id="f225a-133">*Prenumeration*</span><span class="sxs-lookup"><span data-stu-id="f225a-133">*Subscription*</span></span> | <span data-ttu-id="f225a-134">mysubscription</span><span class="sxs-lookup"><span data-stu-id="f225a-134">mysubscription</span></span> | <span data-ttu-id="f225a-135">Välj din prenumeration från listrutan.</span><span class="sxs-lookup"><span data-stu-id="f225a-135">Select your subscription from the drop-down.</span></span> |
| <span data-ttu-id="f225a-136">*Resursgrupp*</span><span class="sxs-lookup"><span data-stu-id="f225a-136">*Resource group*</span></span> | <span data-ttu-id="f225a-137">myresourcegroup</span><span class="sxs-lookup"><span data-stu-id="f225a-137">myresourcegroup</span></span> | <span data-ttu-id="f225a-138">Skapa en resursgrupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="f225a-138">Create a resource group or use an existing one.</span></span> |
| <span data-ttu-id="f225a-139">*Inloggning för serveradministratör*</span><span class="sxs-lookup"><span data-stu-id="f225a-139">*Server admin login*</span></span> | <span data-ttu-id="f225a-140">myadmin</span><span class="sxs-lookup"><span data-stu-id="f225a-140">myadmin</span></span> | <span data-ttu-id="f225a-141">Konfigurera admin kontonamn.</span><span class="sxs-lookup"><span data-stu-id="f225a-141">Setup admin account name.</span></span> |
| <span data-ttu-id="f225a-142">*Lösenord*</span><span class="sxs-lookup"><span data-stu-id="f225a-142">*Password*</span></span> |  | <span data-ttu-id="f225a-143">Ange ett starkt administratörslösenord.</span><span class="sxs-lookup"><span data-stu-id="f225a-143">Set a strong admin account password.</span></span> |
| <span data-ttu-id="f225a-144">*Bekräfta lösenord*</span><span class="sxs-lookup"><span data-stu-id="f225a-144">*Confirm password*</span></span> |  | <span data-ttu-id="f225a-145">Bekräfta administratörslösenordet.</span><span class="sxs-lookup"><span data-stu-id="f225a-145">Confirm the admin account password.</span></span> |
| <span data-ttu-id="f225a-146">*Plats*</span><span class="sxs-lookup"><span data-stu-id="f225a-146">*Location*</span></span> |  | <span data-ttu-id="f225a-147">Välj en tillgänglig region.</span><span class="sxs-lookup"><span data-stu-id="f225a-147">Select an available region.</span></span> |
| <span data-ttu-id="f225a-148">*Version*</span><span class="sxs-lookup"><span data-stu-id="f225a-148">*Version*</span></span> | <span data-ttu-id="f225a-149">5.7</span><span class="sxs-lookup"><span data-stu-id="f225a-149">5.7</span></span> | <span data-ttu-id="f225a-150">Välj den senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="f225a-150">Choose the latest version.</span></span> |
| <span data-ttu-id="f225a-151">*Konfigurera prestanda*</span><span class="sxs-lookup"><span data-stu-id="f225a-151">*Configure performance*</span></span> | <span data-ttu-id="f225a-152">Basic, 50 compute enheter, 50 GB</span><span class="sxs-lookup"><span data-stu-id="f225a-152">Basic, 50 compute units, 50 GB</span></span>  | <span data-ttu-id="f225a-153">Välj **Prisnivå**, **Compute-enheter**, **Lagring (GB)** och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f225a-153">Choose **Pricing tier**, **Compute Units**, **Storage (GB)**, and then click **OK**.</span></span> |
| <span data-ttu-id="f225a-154">*Fäst vid instrumentpanelen*</span><span class="sxs-lookup"><span data-stu-id="f225a-154">*Pin to Dashboard*</span></span> | <span data-ttu-id="f225a-155">Markera</span><span class="sxs-lookup"><span data-stu-id="f225a-155">Check</span></span> | <span data-ttu-id="f225a-156">Du bör markera den här kryssrutan så att du enkelt kan hitta servern senare</span><span class="sxs-lookup"><span data-stu-id="f225a-156">Recommended to check this box so you may find the server easily later on</span></span> |
<span data-ttu-id="f225a-157">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f225a-157">Then, click **Create**.</span></span> <span data-ttu-id="f225a-158">Efter några minuter körs en ny Azure Database för MySQL-server i molnet.</span><span class="sxs-lookup"><span data-stu-id="f225a-158">In a minute or two, a new Azure Database for MySQL server is running in the cloud.</span></span> <span data-ttu-id="f225a-159">Du kan klicka på **meddelanden** i verktygsfältet för att övervaka processen för distribution.</span><span class="sxs-lookup"><span data-stu-id="f225a-159">You can click **Notifications** button on the toolbar to monitor the deployment process.</span></span>

## <a name="configure-firewall"></a><span data-ttu-id="f225a-160">Konfigurera brandväggen</span><span class="sxs-lookup"><span data-stu-id="f225a-160">Configure firewall</span></span>
<span data-ttu-id="f225a-161">Azure-databaser för MySQL skyddas av en brandvägg.</span><span class="sxs-lookup"><span data-stu-id="f225a-161">Azure Databases for MySQL are protected by a firewall.</span></span> <span data-ttu-id="f225a-162">Som standard avvisas alla anslutningar till servern och databaser på servern.</span><span class="sxs-lookup"><span data-stu-id="f225a-162">By default, all connections to the server and the databases inside the server are rejected.</span></span> <span data-ttu-id="f225a-163">Innan du ansluter till Azure-databas för MySQL för första gången, kan du konfigurera brandväggen för att lägga till den klientdatorn offentliga IP-adress (eller IP-adressintervall).</span><span class="sxs-lookup"><span data-stu-id="f225a-163">Before connecting to Azure Database for MySQL for the first time, configure the firewall to add the client machine's public network IP address (or IP address range).</span></span>

1. <span data-ttu-id="f225a-164">Klicka på din nya server och klicka sedan på **anslutningssäkerhet**.</span><span class="sxs-lookup"><span data-stu-id="f225a-164">Click your newly created server, and then click **Connection security**.</span></span>
   <span data-ttu-id="f225a-165">![3-1 anslutningssäkerhet](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span><span class="sxs-lookup"><span data-stu-id="f225a-165">![3-1 Connection security](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span></span>
2. <span data-ttu-id="f225a-166">Du kan **Lägg till Min IP**, eller konfigurera brandväggsregler här.</span><span class="sxs-lookup"><span data-stu-id="f225a-166">You can **Add My IP**, or configure firewall rules here.</span></span> <span data-ttu-id="f225a-167">Kom ihåg att klicka på **Spara** när du har skapat reglerna.</span><span class="sxs-lookup"><span data-stu-id="f225a-167">Remember to click **Save** after you have created the rules.</span></span>
<span data-ttu-id="f225a-168">Nu kan du ansluta till servern med mysql-kommandoradsverktyget eller MySQL arbetsstationen GUI-verktyg.</span><span class="sxs-lookup"><span data-stu-id="f225a-168">You can now connect to the server using mysql command-line tool or MySQL Workbench GUI tool.</span></span>

> [!TIP]
> <span data-ttu-id="f225a-169">Azure-databas för MySQL-servern kommunicerar via port 3306.</span><span class="sxs-lookup"><span data-stu-id="f225a-169">Azure Database for MySQL server communicates over port 3306.</span></span> <span data-ttu-id="f225a-170">Om du försöker ansluta inifrån ett företagsnätverk, kan utgående trafik via port 3306 bli nekad av nätverkets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="f225a-170">If you are trying to connect from within a corporate network, outbound traffic over port 3306 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="f225a-171">I så fall, kan inte du ansluta till Azure MySQL-servern om din IT-avdelning öppnar port 3306.</span><span class="sxs-lookup"><span data-stu-id="f225a-171">If so, you cannot connect to Azure MySQL server unless your IT department opens port 3306.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="f225a-172">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="f225a-172">Get connection information</span></span>
<span data-ttu-id="f225a-173">Hämta den fullständiga **servernamn** och **serverinloggningsnamnet för admin** för din Azure-databas för MySQL-servern från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f225a-173">Get the fully qualified **Server name** and **Server admin login name** for your Azure Database for MySQL server from the Azure portal.</span></span> <span data-ttu-id="f225a-174">Du kan använda fullständigt kvalificerade servernamnet för att ansluta till servern med hjälp av kommandoradsverktyget för mysql.</span><span class="sxs-lookup"><span data-stu-id="f225a-174">You use the fully qualified server name to connect to your server using mysql command-line tool.</span></span> 

1. <span data-ttu-id="f225a-175">I [Azure-portalen](https://portal.azure.com/), klickar du på **alla resurser** skriver du namnet i den vänstra menyn och Sök efter din Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="f225a-175">In [Azure portal](https://portal.azure.com/), click **All resources** from the left-hand menu, type the name, and search for your Azure Database for MySQL server.</span></span> <span data-ttu-id="f225a-176">Välj servernamnet för att visa information.</span><span class="sxs-lookup"><span data-stu-id="f225a-176">Select the server name to view the details.</span></span>

2. <span data-ttu-id="f225a-177">Under inställningarna rubrik, klickar du på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="f225a-177">Under the Settings heading, click **Properties**.</span></span> <span data-ttu-id="f225a-178">Notera **servernamn** och **SERVERINLOGGNINGSNAMNET för ADMIN**.</span><span class="sxs-lookup"><span data-stu-id="f225a-178">Note down **SERVER NAME** and **SERVER ADMIN LOGIN NAME**.</span></span> <span data-ttu-id="f225a-179">Du kan klicka på kopieringsknappen bredvid varje fält att kopiera till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="f225a-179">You may click the copy button next to each field to copy to the clipboard.</span></span>
   <span data-ttu-id="f225a-180">![4-2-serveregenskaper](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span><span class="sxs-lookup"><span data-stu-id="f225a-180">![4-2 server properties](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span></span>

<span data-ttu-id="f225a-181">I det här exemplet är servernamnet *myserver4demo.mysql.database.azure.com* och inloggningen för serveradministratören är *myadmin@myserver4demo*.</span><span class="sxs-lookup"><span data-stu-id="f225a-181">In this example, the server name is *myserver4demo.mysql.database.azure.com*, and the server admin login is *myadmin@myserver4demo*.</span></span>

## <a name="connect-to-the-server-using-mysql"></a><span data-ttu-id="f225a-182">Ansluta till servern med mysql</span><span class="sxs-lookup"><span data-stu-id="f225a-182">Connect to the server using mysql</span></span>
<span data-ttu-id="f225a-183">Använd [kommandoradsverktyget mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) till att upprätta en anslutning till din Azure Database för MySQL-server.</span><span class="sxs-lookup"><span data-stu-id="f225a-183">Use [mysql command-line tool](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) to establish a connection to your Azure Database for MySQL server.</span></span> <span data-ttu-id="f225a-184">Du kan köra kommandoradsverktyget mysql från Azure-molnet Shell i webbläsaren eller från din egen dator med hjälp av mysql-verktygen som installeras lokalt.</span><span class="sxs-lookup"><span data-stu-id="f225a-184">You can run the mysql command-line tool from the Azure Cloud Shell in the browser or from your own machine using mysql tools installed locally.</span></span> <span data-ttu-id="f225a-185">Om du vill starta Azure Cloud-gränssnittet klickar du på den `Try It` på ett kodblock i den här artikeln eller besök Azure-portalen och klicka sedan på den `>_` ikonen i verktygsfältet övre högra.</span><span class="sxs-lookup"><span data-stu-id="f225a-185">To launch the Azure Cloud Shell, click the `Try It` button on a code block in this article, or visit the Azure portal and click the `>_` icon in the top right toolbar.</span></span> 

<span data-ttu-id="f225a-186">Skriv anslutningskommandot:</span><span class="sxs-lookup"><span data-stu-id="f225a-186">Type the command to connect:</span></span>
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="f225a-187">Skapa en tom databas</span><span class="sxs-lookup"><span data-stu-id="f225a-187">Create a blank database</span></span>
<span data-ttu-id="f225a-188">När du är ansluten till servern kan du skapa en tom databas att arbeta med.</span><span class="sxs-lookup"><span data-stu-id="f225a-188">Once you’re connected to the server, create a blank database to work with.</span></span>
```sql
CREATE DATABASE mysampledb;
```

<span data-ttu-id="f225a-189">Kör följande kommando för att växla anslutning till den nya databasen i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="f225a-189">At the prompt, run the following command to switch connection to this newly created database:</span></span>
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a><span data-ttu-id="f225a-190">Skapa tabeller i databasen</span><span class="sxs-lookup"><span data-stu-id="f225a-190">Create tables in the database</span></span>
<span data-ttu-id="f225a-191">Nu när du vet hur du ansluter till Azure-databasen för MySQL-databas, gå vi igenom hur du utför några grundläggande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="f225a-191">Now that you know how to connect to the Azure Database for MySQL database, we can go over how to complete some basic tasks.</span></span>

<span data-ttu-id="f225a-192">Vi kan först skapa en tabell och läsa in den med vissa data.</span><span class="sxs-lookup"><span data-stu-id="f225a-192">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="f225a-193">Nu ska vi skapa en tabell som innehåller information om maskinvaruinventering.</span><span class="sxs-lookup"><span data-stu-id="f225a-193">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a><span data-ttu-id="f225a-194">Läser in data i tabeller</span><span class="sxs-lookup"><span data-stu-id="f225a-194">Load data into the tables</span></span>
<span data-ttu-id="f225a-195">Nu när vi har en tabell kan vi infoga vissa data i den.</span><span class="sxs-lookup"><span data-stu-id="f225a-195">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="f225a-196">Kör följande fråga för att infoga vissa rader med data i öppna en kommandotolk-fönster.</span><span class="sxs-lookup"><span data-stu-id="f225a-196">At the open command prompt window, run the following query to insert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="f225a-197">Nu har du två rader med exempeldata i tabellen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f225a-197">Now you have two rows of sample data into the table you created earlier.</span></span>

## <a name="query-and-update-the-data-in-the-tables"></a><span data-ttu-id="f225a-198">Fråga efter och uppdatera data i tabeller</span><span class="sxs-lookup"><span data-stu-id="f225a-198">Query and update the data in the tables</span></span>
<span data-ttu-id="f225a-199">Kör följande fråga för att hämta information från databastabellen.</span><span class="sxs-lookup"><span data-stu-id="f225a-199">Execute the following query to retrieve information from the database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="f225a-200">Du kan också uppdatera data i tabeller.</span><span class="sxs-lookup"><span data-stu-id="f225a-200">You can also update the data in the tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="f225a-201">Raden uppdateras i enlighet med detta när du hämtar data.</span><span class="sxs-lookup"><span data-stu-id="f225a-201">The row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a><span data-ttu-id="f225a-202">Återställa en databas till en tidigare tidpunkt</span><span class="sxs-lookup"><span data-stu-id="f225a-202">Restore a database to a previous point in time</span></span>
<span data-ttu-id="f225a-203">Anta att du har tagits bort av misstag en viktigt-databastabell och kan enkelt återställa data.</span><span class="sxs-lookup"><span data-stu-id="f225a-203">Imagine you have accidentally deleted an important database table, and cannot recover the data easily.</span></span> <span data-ttu-id="f225a-204">Azure MySQL-databas kan du återställa servern till en punkt i tiden, skapar en kopia av databaserna till den nya servern.</span><span class="sxs-lookup"><span data-stu-id="f225a-204">Azure Database for MySQL allows you to restore the server to a point in time, creating a copy of the databases into new server.</span></span> <span data-ttu-id="f225a-205">Du kan använda den här nya servern för att återställa dina data.</span><span class="sxs-lookup"><span data-stu-id="f225a-205">You can use this new server to recover your deleted data.</span></span> <span data-ttu-id="f225a-206">Följande steg återställa exempelserver till en innan tabellen har lagts till.</span><span class="sxs-lookup"><span data-stu-id="f225a-206">The following steps restore the sample server to a point before the table was added.</span></span>

1. <span data-ttu-id="f225a-207">Hitta din Azure-databas för MySQL på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f225a-207">In the Azure portal, locate your Azure Database for MySQL.</span></span> <span data-ttu-id="f225a-208">På den **översikt** klickar du på **återställa** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="f225a-208">On the **Overview** page, click **Restore** on the toolbar.</span></span> <span data-ttu-id="f225a-209">Restore-sidan öppnas.</span><span class="sxs-lookup"><span data-stu-id="f225a-209">The Restore page opens.</span></span>

   ![10-1 Återställ en databas](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. <span data-ttu-id="f225a-211">Fyll i den **återställa** formulär med informationen som krävs.</span><span class="sxs-lookup"><span data-stu-id="f225a-211">Fill out the **Restore** form with the required information.</span></span>
   
   ![10-2-formulär för återställning](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - <span data-ttu-id="f225a-213">**Återställningspunkt**: Välj en i tidpunkt som du vill återställa till, under den tidsperiod som visas.</span><span class="sxs-lookup"><span data-stu-id="f225a-213">**Restore point**: Select a point-in-time that you want to restore to, within the timeframe listed.</span></span> <span data-ttu-id="f225a-214">Se till att konvertera din lokala tidszon till UTC.</span><span class="sxs-lookup"><span data-stu-id="f225a-214">Make sure to convert your local timezone to UTC.</span></span>
   - <span data-ttu-id="f225a-215">**Återställ till en ny server**: Ange ett nytt servernamn som du vill återställa.</span><span class="sxs-lookup"><span data-stu-id="f225a-215">**Restore to new server**: Provide a new server name you want to restore to.</span></span>
   - <span data-ttu-id="f225a-216">**Plats**: regionen är samma som källservern och kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="f225a-216">**Location**: The region is same as the source server, and cannot be changed.</span></span>
   - <span data-ttu-id="f225a-217">**Prisnivån**: prisnivån är samma som källservern och kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="f225a-217">**Pricing tier**: The pricing tier is the same as the source server, and cannot be changed.</span></span>
   
3. <span data-ttu-id="f225a-218">Klicka på **OK** att återställa servern till [återställa till en tidpunkt](./howto-restore-server-portal.md) innan tabellen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="f225a-218">Click **OK** to restore the server to [restore to a point in time](./howto-restore-server-portal.md) before the table was deleted.</span></span> <span data-ttu-id="f225a-219">Återställa en server skapar en ny kopia av servern, från och med tidpunkten du anger.</span><span class="sxs-lookup"><span data-stu-id="f225a-219">Restoring a server creates a new copy of the server, as of the point in time you specify.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f225a-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f225a-220">Next steps</span></span>
<span data-ttu-id="f225a-221">I den här kursen kan du använda Azure portal till inlärda så här:</span><span class="sxs-lookup"><span data-stu-id="f225a-221">In this tutorial, you use the Azure portal to learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f225a-222">Skapa en Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="f225a-222">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="f225a-223">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="f225a-223">Configure the server firewall</span></span>
> * <span data-ttu-id="f225a-224">Kommandoradsverktyget mysql för att skapa en databas</span><span class="sxs-lookup"><span data-stu-id="f225a-224">Use mysql command-line tool to create a database</span></span>
> * <span data-ttu-id="f225a-225">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="f225a-225">Load sample data</span></span>
> * <span data-ttu-id="f225a-226">Frågedata</span><span class="sxs-lookup"><span data-stu-id="f225a-226">Query data</span></span>
> * <span data-ttu-id="f225a-227">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="f225a-227">Update data</span></span>
> * <span data-ttu-id="f225a-228">Återställa data</span><span class="sxs-lookup"><span data-stu-id="f225a-228">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f225a-229">Så här ansluter du program till Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="f225a-229">How to connect applications to Azure Database for MySQL</span></span>](./howto-connection-string.md)
