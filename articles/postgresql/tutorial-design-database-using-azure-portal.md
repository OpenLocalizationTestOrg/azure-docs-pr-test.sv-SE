---
title: "Utforma din första Azure-databas för PostgreSQL med hjälp av Azure portal | Microsoft Docs"
description: "Den här kursen visar hur du utformar din första Azure-databas för PostgreSQL med Azure-portalen."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: tutorial, mvc
ms.topic: tutorial
ms.date: 05/10/2017
ms.openlocfilehash: 2aa9d10749b54537495ad3e09566c43718f67a9e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-the-azure-portal"></a><span data-ttu-id="8d507-103">Utforma din första Azure-databas för PostgreSQL med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8d507-103">Design your first Azure Database for PostgreSQL using the Azure portal</span></span>

<span data-ttu-id="8d507-104">Azure Database för PostgreSQL är en hanterad tjänst som låter dig köra, hantera och skala högtillgängliga PostgreSQL-databaser i molnet.</span><span class="sxs-lookup"><span data-stu-id="8d507-104">Azure Database for PostgreSQL is a managed service that enables you to run, manage, and scale highly available PostgreSQL databases in the cloud.</span></span> <span data-ttu-id="8d507-105">Med Azure-portalen kan du enkelt hantera servern och utforma en databas.</span><span class="sxs-lookup"><span data-stu-id="8d507-105">Using the Azure portal, you can easily manage your server and design a database.</span></span>

<span data-ttu-id="8d507-106">I kursen får du använder Azure-portalen att lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="8d507-106">In this tutorial, you use the Azure portal to learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="8d507-107">Skapa en Azure Database för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="8d507-107">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="8d507-108">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="8d507-108">Configure the server firewall</span></span>
> * <span data-ttu-id="8d507-109">Använd [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) verktyg för att skapa en databas</span><span class="sxs-lookup"><span data-stu-id="8d507-109">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility to create a database</span></span>
> * <span data-ttu-id="8d507-110">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="8d507-110">Load sample data</span></span>
> * <span data-ttu-id="8d507-111">Frågedata</span><span class="sxs-lookup"><span data-stu-id="8d507-111">Query data</span></span>
> * <span data-ttu-id="8d507-112">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="8d507-112">Update data</span></span>
> * <span data-ttu-id="8d507-113">Återställa data</span><span class="sxs-lookup"><span data-stu-id="8d507-113">Restore data</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d507-114">Krav</span><span class="sxs-lookup"><span data-stu-id="8d507-114">Prerequisites</span></span>
<span data-ttu-id="8d507-115">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="8d507-115">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="8d507-116">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8d507-116">Log in to the Azure portal</span></span>
<span data-ttu-id="8d507-117">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8d507-117">Log in to the [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-an-azure-database-for-postgresql"></a><span data-ttu-id="8d507-118">Skapa en Azure Database för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="8d507-118">Create an Azure Database for PostgreSQL</span></span>

<span data-ttu-id="8d507-119">En Azure Database för PostgreSQL-server skapas med en definierad uppsättning [compute- och lagringsresurser](./concepts-compute-unit-and-storage.md).</span><span class="sxs-lookup"><span data-stu-id="8d507-119">An Azure Database for PostgreSQL server is created with a defined set of [compute and storage resources](./concepts-compute-unit-and-storage.md).</span></span> <span data-ttu-id="8d507-120">Servern skapas inom en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8d507-120">The server is created within an [Azure resource group](../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="8d507-121">Följ de här stegen för att skapa en Azure Database för PostgreSQL-server:</span><span class="sxs-lookup"><span data-stu-id="8d507-121">Follow these steps to create an Azure Database for PostgreSQL server:</span></span>
1.  <span data-ttu-id="8d507-122">Klicka på den **+ ny** knapp hittades i det övre vänstra hörnet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8d507-122">Click the **+ New**  button found on the upper left-hand corner of the Azure portal.</span></span>
2.  <span data-ttu-id="8d507-123">Välj **databaser** från sidan **Nytt** och välj **Azure Database för PostgreSQL** från sidan **databaser**.</span><span class="sxs-lookup"><span data-stu-id="8d507-123">Select **Databases** from the **New** page, and select **Azure Database for PostgreSQL** from the **Databases** page.</span></span>
 <span data-ttu-id="8d507-124">![Azure Database för PostgreSQL – Skapa databasen](./media/tutorial-design-database-using-azure-portal/1-create-database.png)</span><span class="sxs-lookup"><span data-stu-id="8d507-124">![Azure Database for PostgreSQL - Create the database](./media/tutorial-design-database-using-azure-portal/1-create-database.png)</span></span>

3.  <span data-ttu-id="8d507-125">Fyll i detaljformuläret för den nya server med följande information, som det visas i föregående bil:</span><span class="sxs-lookup"><span data-stu-id="8d507-125">Fill out the new server details form with the following information, as shown on the preceding image:</span></span>
    - <span data-ttu-id="8d507-126">Servernamn: **mypgserver-20170401** (namnet på en server mappar till DNS-namnet och behöver därför vara globalt unikt)</span><span class="sxs-lookup"><span data-stu-id="8d507-126">Server name: **mypgserver-20170401** (name of a server maps to DNS name and is thus required to be globally unique)</span></span> 
    - <span data-ttu-id="8d507-127">Prenumeration: Om du har flera prenumerationer, väljer du lämplig prenumeration där resursen ska finnas eller debiteras till.</span><span class="sxs-lookup"><span data-stu-id="8d507-127">Subscription: If you have multiple subscriptions, choose the appropriate subscription in which the resource exists or is billed for.</span></span>
    - <span data-ttu-id="8d507-128">Resursgrupp: **myresourcegroup**</span><span class="sxs-lookup"><span data-stu-id="8d507-128">Resource group: **myresourcegroup**</span></span>
    - <span data-ttu-id="8d507-129">Valfritt inloggningsnamn och lösenord för serveradministratören</span><span class="sxs-lookup"><span data-stu-id="8d507-129">Server admin login and password of your choice</span></span>
    - <span data-ttu-id="8d507-130">Plats</span><span class="sxs-lookup"><span data-stu-id="8d507-130">Location</span></span>
    - <span data-ttu-id="8d507-131">PostgreSQL-version</span><span class="sxs-lookup"><span data-stu-id="8d507-131">PostgreSQL Version</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="8d507-132">Det användarnamn och lösenord för serveradministration du anger här krävs för inloggning på servern och databaserna senare i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="8d507-132">The server admin login and password that you specify here are required to log in to the server and its databases later in this quick start.</span></span> <span data-ttu-id="8d507-133">Kom ihåg eller skriv ned den här informationen så att du kan använda den senare.</span><span class="sxs-lookup"><span data-stu-id="8d507-133">Remember or record this information for later use.</span></span>

4.  <span data-ttu-id="8d507-134">Klicka på **Prisnivå** för att ange tjänstenivå och prestandanivå för den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="8d507-134">Click **Pricing tier** to specify the service tier and performance level for your new database.</span></span> <span data-ttu-id="8d507-135">I den här snabbstarten väljer du **Basic**-nivå **50 compute-enheter** och **50 GB** lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="8d507-135">For this quick start, select **Basic** Tier, **50 Compute Units** and **50 GB** of included storage.</span></span>
 <span data-ttu-id="8d507-136">![Azure Database för PostgreSQL – välj tjänstnivå](./media/tutorial-design-database-using-azure-portal/2-service-tier.png)</span><span class="sxs-lookup"><span data-stu-id="8d507-136">![Azure Database for PostgreSQL - pick the service tier](./media/tutorial-design-database-using-azure-portal/2-service-tier.png)</span></span>
5.  <span data-ttu-id="8d507-137">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8d507-137">Click **Ok**.</span></span>
6.  <span data-ttu-id="8d507-138">Klicka på **Skapa** för att etablera servern.</span><span class="sxs-lookup"><span data-stu-id="8d507-138">Click **Create** to provision the server.</span></span> <span data-ttu-id="8d507-139">Etableringen tar några minuter.</span><span class="sxs-lookup"><span data-stu-id="8d507-139">Provisioning takes a few minutes.</span></span>

  > [!TIP]
  > <span data-ttu-id="8d507-140">Markera alternativet **Fäst på instrumentpanelen** för att enkelt kunna spåra dina distributioner.</span><span class="sxs-lookup"><span data-stu-id="8d507-140">Check the **Pin to dashboard** option to allow easy tracking of your deployments.</span></span>

7.  <span data-ttu-id="8d507-141">Klicka på **Aviseringar** i verktygsfältet för att övervaka distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="8d507-141">On the toolbar, click **Notifications** to monitor the deployment process.</span></span>
 <span data-ttu-id="8d507-142">![Azure Database för PostgreSQL – se meddelanden](./media/tutorial-design-database-using-azure-portal/3-notifications.png)</span><span class="sxs-lookup"><span data-stu-id="8d507-142">![Azure Database for PostgreSQL - See notifications](./media/tutorial-design-database-using-azure-portal/3-notifications.png)</span></span>
   
  <span data-ttu-id="8d507-143">Som standard skapas **postgres**-databasen under din server.</span><span class="sxs-lookup"><span data-stu-id="8d507-143">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="8d507-144">[Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html)-databasen är en standarddatabas som är avsedd för användare, verktyg och tredje parts program.</span><span class="sxs-lookup"><span data-stu-id="8d507-144">The [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 

## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="8d507-145">Konfigurera en brandväggsregel på servernivå</span><span class="sxs-lookup"><span data-stu-id="8d507-145">Configure a server-level firewall rule</span></span>

<span data-ttu-id="8d507-146">Azure Database för PostgreSQL-tjänsten skapar en brandvägg på server-nivå.</span><span class="sxs-lookup"><span data-stu-id="8d507-146">The Azure Database for PostgreSQL service creates a firewall at the server-level.</span></span> <span data-ttu-id="8d507-147">Brandväggen förhindrar att externa program och verktyg ansluter till servern eller databaser på servern, om inte en brandväggsregel konfigureras som öppnar brandväggen för specifika IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="8d507-147">This firewall prevents external applications and tools from connecting to the server and any databases on the server unless a firewall rule is created to open the firewall for specific IP addresses.</span></span> 

1.  <span data-ttu-id="8d507-148">När distributionen är klar, klickar du på **alla resurser** från den vänstra menyn och skriver in namnet **mypgserver-20170401** för att söka efter din nyskapade server.</span><span class="sxs-lookup"><span data-stu-id="8d507-148">After the deployment completes, click **All Resources** from the left-hand menu and type in the name **mypgserver-20170401** to search for your newly created server.</span></span> <span data-ttu-id="8d507-149">Klicka på servernamnet som listas i sökresultatet.</span><span class="sxs-lookup"><span data-stu-id="8d507-149">Click the server name listed in the search result.</span></span> <span data-ttu-id="8d507-150">**Översikt**-sidan för din server öppnas och innehåller alternativ ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8d507-150">The **Overview** page for your server opens and provides options for further configuration.</span></span>
 
 ![<span data-ttu-id="8d507-151">Azure Database för PostgreSQL – sök efter server</span><span class="sxs-lookup"><span data-stu-id="8d507-151">Azure Database for PostgreSQL - Search for server</span></span> ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

2.  <span data-ttu-id="8d507-152">I serverbladet, väljer du **anslutningssäkerhet**.</span><span class="sxs-lookup"><span data-stu-id="8d507-152">In the server blade, select **Connection Security**.</span></span> 
3.  <span data-ttu-id="8d507-153">Klicka i textrutan under **regelnamn** och lägg till en ny brandväggsregel som vitlistar IP-adressintervallet för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="8d507-153">Click in the text box under **Rule Name,** and add a new firewall rule to whitelist the IP range for connectivity.</span></span> <span data-ttu-id="8d507-154">För den här självstudiekursen kommer vi att alla IP-adresser genom att skriva in **Regelnamnet = AllowAllIps**, **första IP-= 0.0.0.0** och **sista IP = 255.255.255.255** och klicka sedan på **spara** .</span><span class="sxs-lookup"><span data-stu-id="8d507-154">For this tutorial, let's allow all IPs by typing in **Rule Name = AllowAllIps**, **Start IP = 0.0.0.0** and **End IP = 255.255.255.255** and then click **Save**.</span></span> <span data-ttu-id="8d507-155">Du kan ställa in en brandväggsregel som omfattar ett IP-intervall för att kunna ansluta från ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8d507-155">You can set a firewall rule that covers an IP range to be able to connect from your network.</span></span>
 
 ![Azure Database för PostgreSQL – Skapa brandväggsregel](./media/tutorial-design-database-using-azure-portal/5-firewall-2.png)

4.  <span data-ttu-id="8d507-157">Klicka på **spara** och klicka sedan på **X** för att stänga sidan **anslutningssäkerhet**.</span><span class="sxs-lookup"><span data-stu-id="8d507-157">Click **Save** and then click the **X** to close the **Connections Security** page.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8d507-158">Azure PostgreSQL-servern kommunicerar via port 5432.</span><span class="sxs-lookup"><span data-stu-id="8d507-158">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="8d507-159">Om du försöker ansluta inifrån ett företagsnätverk, kan utgående trafik via port 5432 bli nekad av nätverkets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="8d507-159">If you are trying to connect from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="8d507-160">I så fall kommer du inte att kunna ansluta till din Azure SQL Database-server om inte din IT-avdelning öppnar port 5432.</span><span class="sxs-lookup"><span data-stu-id="8d507-160">If so, you will not be able to connect to your Azure SQL Database server unless your IT department opens port 5432.</span></span>
  >


## <a name="get-the-connection-information"></a><span data-ttu-id="8d507-161">Hämta anslutningsinformationen</span><span class="sxs-lookup"><span data-stu-id="8d507-161">Get the connection information</span></span>

<span data-ttu-id="8d507-162">När vi skapade vår Azure Database för PostgreSQL-server, skapades även standard-**postgres**-databasen.</span><span class="sxs-lookup"><span data-stu-id="8d507-162">When we created our Azure Database for PostgreSQL server, the default **postgres** database also gets created.</span></span> <span data-ttu-id="8d507-163">För att ansluta till din databasserver, måste du ange värddatorinformation och autentiseringsuppgifter för åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8d507-163">To connect to your database server, you need to provide host information and access credentials.</span></span>

1. <span data-ttu-id="8d507-164">I den vänstra menyn i Azure-portalen, klickar du på **Alla resurser** och söker efter den server som du nyss skapade **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="8d507-164">From the left-hand menu in Azure portal, click **All resources** and search for the server you just created **mypgserver-20170401**.</span></span>

  ![<span data-ttu-id="8d507-165">Azure Database för PostgreSQL – sök efter server</span><span class="sxs-lookup"><span data-stu-id="8d507-165">Azure Database for PostgreSQL - Search for server</span></span> ](./media/tutorial-design-database-using-azure-portal/4-locate.png)

3. <span data-ttu-id="8d507-166">Klicka på servernamnet **mypgserver-20170401**.</span><span class="sxs-lookup"><span data-stu-id="8d507-166">Click the server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="8d507-167">Välj serverns **översikt**-sida.</span><span class="sxs-lookup"><span data-stu-id="8d507-167">Select the server's **Overview** page.</span></span> <span data-ttu-id="8d507-168">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="8d507-168">Make a note of the **Server name** and **Server admin login name**.</span></span>

 ![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/tutorial-design-database-using-azure-portal/6-server-name.png)


## <a name="connect-to-postgresql-database-using-psql-in-cloud-shell"></a><span data-ttu-id="8d507-170">Anslut till PostgreSQL-databasen med psql i Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="8d507-170">Connect to PostgreSQL database using psql in Cloud Shell</span></span>

<span data-ttu-id="8d507-171">Nu använder vi psql-kommandoradsverktyget för att ansluta till Azure Database för PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="8d507-171">Let's now use the psql command-line utility to connect to the Azure Database for PostgreSQL server.</span></span> 
1. <span data-ttu-id="8d507-172">Starta Azure Cloud Shell via terminalikonen överst i navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="8d507-172">Launch the Azure Cloud Shell via the terminal icon on the top navigation pane.</span></span>

   ![Azure Database för PostgreSQL – Azure Cloud Shell-terminalikonen](./media/tutorial-design-database-using-azure-portal/7-cloud-shell.png)

2. <span data-ttu-id="8d507-174">Azure Cloud Shell öppnas i din webbläsare så att du kan skriva bash-kommandon.</span><span class="sxs-lookup"><span data-stu-id="8d507-174">The Azure Cloud Shell opens in your browser, enabling you to type bash commands.</span></span>

   ![Azure Database för PostgreSQL – Azure Shell Bash-prompten](./media/tutorial-design-database-using-azure-portal/8-bash.png)

3. <span data-ttu-id="8d507-176">I Cloud Shell-prompten ansluter du till din Azure Database för PostgreSQL-server med psql-kommandona.</span><span class="sxs-lookup"><span data-stu-id="8d507-176">At the Cloud Shell prompt, connect to your Azure Database for PostgreSQL server using the psql commands.</span></span> <span data-ttu-id="8d507-177">Följande format används för att ansluta till en Azure Database för PostgreSQL-server med [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html)-verktyget:</span><span class="sxs-lookup"><span data-stu-id="8d507-177">The following format is used to connect to an Azure Database for PostgreSQL server with the [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility:</span></span>
   ```bash
   psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
   ```

   <span data-ttu-id="8d507-178">Följande kommando till exempel, ansluter till standarddatabasen som heter **postgres** på din PostgreSQL-server **mypgserver-20170401.postgres.database.azure.com** med hjälp av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8d507-178">For example, the following command connects to the default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="8d507-179">Ange ditt lösenord för serveradministratören när du uppmanas till detta.</span><span class="sxs-lookup"><span data-stu-id="8d507-179">Enter your server admin password when prompted.</span></span>

   ```bash
   psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
   ```

## <a name="create-a-new-database"></a><span data-ttu-id="8d507-180">Skapa en ny databas</span><span class="sxs-lookup"><span data-stu-id="8d507-180">Create a New Database</span></span>
<span data-ttu-id="8d507-181">När du är ansluten till servern, skapar du en blank databas i prompten.</span><span class="sxs-lookup"><span data-stu-id="8d507-181">Once you're connected to the server, create a blank database at the prompt.</span></span>
```bash
CREATE DATABASE mypgsqldb;
```

<span data-ttu-id="8d507-182">I prompten kör du följande kommando för att växla anslutning till den nyligen skapade databasen **mypgsqldb**.</span><span class="sxs-lookup"><span data-stu-id="8d507-182">At the prompt, execute the following command to switch connection to the newly created database **mypgsqldb**.</span></span>
```bash
\c mypgsqldb
```
## <a name="create-tables-in-the-database"></a><span data-ttu-id="8d507-183">Skapa tabeller i databasen</span><span class="sxs-lookup"><span data-stu-id="8d507-183">Create tables in the database</span></span>
<span data-ttu-id="8d507-184">Nu när du vet hur du ansluter till Azure-databasen för PostgreSQL gå vi igenom hur du utför några grundläggande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="8d507-184">Now that you know how to connect to the Azure Database for PostgreSQL, we can go over how to complete some basic tasks.</span></span>

<span data-ttu-id="8d507-185">Vi kan först skapa en tabell och läsa in den med vissa data.</span><span class="sxs-lookup"><span data-stu-id="8d507-185">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="8d507-186">Nu ska vi skapa en tabell som spårar inventeringsinformation.</span><span class="sxs-lookup"><span data-stu-id="8d507-186">Let's create a table that tracks inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

<span data-ttu-id="8d507-187">Du kan se den nyligen skapade tabellen i listan över tabvles nu genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="8d507-187">You can see the newly created table in the list of tabvles now by typing:</span></span>
```sql
\dt
```

## <a name="load-data-into-the-tables"></a><span data-ttu-id="8d507-188">Läser in data i tabeller</span><span class="sxs-lookup"><span data-stu-id="8d507-188">Load data into the tables</span></span>
<span data-ttu-id="8d507-189">Nu när vi har en tabell kan vi infoga vissa data i den.</span><span class="sxs-lookup"><span data-stu-id="8d507-189">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="8d507-190">Kör följande fråga för att infoga vissa rader med data i öppna en kommandotolk-fönster</span><span class="sxs-lookup"><span data-stu-id="8d507-190">At the open command prompt window, run the following query to insert some rows of data</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="8d507-191">Du har nu två rader med exempeldata i tabellen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="8d507-191">You have now two rows of sample data into the table you created earlier.</span></span>

## <a name="query-and-update-the-data-in-the-tables"></a><span data-ttu-id="8d507-192">Fråga efter och uppdatera data i tabeller</span><span class="sxs-lookup"><span data-stu-id="8d507-192">Query and update the data in the tables</span></span>
<span data-ttu-id="8d507-193">Kör följande fråga för att hämta information från databastabellen.</span><span class="sxs-lookup"><span data-stu-id="8d507-193">Execute the following query to retrieve information from the database table.</span></span> 
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="8d507-194">Du kan också uppdatera data i tabeller</span><span class="sxs-lookup"><span data-stu-id="8d507-194">You can also update the data in the tables</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="8d507-195">Raden uppdateras i enlighet med detta när du hämtar data.</span><span class="sxs-lookup"><span data-stu-id="8d507-195">The row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-data-to-a-previous-point-in-time"></a><span data-ttu-id="8d507-196">Återställa data till en tidigare tidpunkt</span><span class="sxs-lookup"><span data-stu-id="8d507-196">Restore data to a previous point in time</span></span>
<span data-ttu-id="8d507-197">Anta att du av misstag har tagit bort den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="8d507-197">Imagine you have accidentally deleted this table.</span></span> <span data-ttu-id="8d507-198">Den här situationen är något som du lätt kan återställa från.</span><span class="sxs-lookup"><span data-stu-id="8d507-198">This situation is something you cannot easily recover from.</span></span> <span data-ttu-id="8d507-199">Azure-databas för PostgreSQL kan du gå tillbaka till någon punkt i tid (i det senaste upp till 7 dagar (grundläggande) och 35 dagar (Standard)) och återställa den här i tidpunkt till en ny server.</span><span class="sxs-lookup"><span data-stu-id="8d507-199">Azure Database for PostgreSQL allows you to go back to any point-in-time (in the last up to 7 days (Basic) and 35 days (Standard)) and restore this point-in-time to a new server.</span></span> <span data-ttu-id="8d507-200">Du kan använda den här nya servern för att återställa dina data.</span><span class="sxs-lookup"><span data-stu-id="8d507-200">You can use this new server to recover your deleted data.</span></span> <span data-ttu-id="8d507-201">Följande steg återställa exempelserver till en innan tabellen har lagts till.</span><span class="sxs-lookup"><span data-stu-id="8d507-201">The following steps restore the sample server to a point before the table was added.</span></span>

1.  <span data-ttu-id="8d507-202">Klicka på Azure-databasen för PostgreSQL-sidan för servern **återställa** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="8d507-202">On the Azure Database for PostgreSQL page for your server, click **Restore** on the toolbar.</span></span> <span data-ttu-id="8d507-203">Den **återställa** öppnas.</span><span class="sxs-lookup"><span data-stu-id="8d507-203">The **Restore** page opens.</span></span>
  <span data-ttu-id="8d507-204">![Azure portal – återställningsalternativ för formulär](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)</span><span class="sxs-lookup"><span data-stu-id="8d507-204">![Azure portal - Restore form options](./media/tutorial-design-database-using-azure-portal/9-azure-portal-restore.png)</span></span>
2.  <span data-ttu-id="8d507-205">Fyll i den **återställa** formulär med informationen som krävs:</span><span class="sxs-lookup"><span data-stu-id="8d507-205">Fill out the **Restore** form with the required information:</span></span>

  ![Azure portal – återställningsalternativ för formulär](./media/tutorial-design-database-using-azure-portal/10-azure-portal-restore.png)
  - <span data-ttu-id="8d507-207">**Återställningspunkt**: Välj en i tidpunkt som inträffar innan servern har ändrats</span><span class="sxs-lookup"><span data-stu-id="8d507-207">**Restore point**: Select a point-in-time that occurs before the server was changed</span></span>
  - <span data-ttu-id="8d507-208">**Målservern**: Ange ett nytt servernamn som du vill återställa till</span><span class="sxs-lookup"><span data-stu-id="8d507-208">**Target server**: Provide a new server name you want to restore to</span></span>
  - <span data-ttu-id="8d507-209">**Plats**: du kan inte välja regionen, som standard är det samma som källservern</span><span class="sxs-lookup"><span data-stu-id="8d507-209">**Location**: You cannot select the region, by default it is same as the source server</span></span>
  - <span data-ttu-id="8d507-210">**Prisnivån**: du kan inte ändra det här värdet när du återställer en server.</span><span class="sxs-lookup"><span data-stu-id="8d507-210">**Pricing tier**: You cannot change this value when restoring a server.</span></span> <span data-ttu-id="8d507-211">Det är samma som källservern.</span><span class="sxs-lookup"><span data-stu-id="8d507-211">It is same as the source server.</span></span> 
3.  <span data-ttu-id="8d507-212">Klicka på **OK** att återställa servern till [återställa till point-in-time](./howto-restore-server-portal.md) innan tabellerna har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="8d507-212">Click **OK** to restore the server to [restore to a point-in-time](./howto-restore-server-portal.md) before the tables was deleted.</span></span> <span data-ttu-id="8d507-213">Återställa en server till en annan tidpunkt skapar en dubblett ny server som den ursprungliga servern från och med punkten tidpunkt du anger under förutsättning att det är inom kvarhållningsperioden för din [tjänstnivån](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="8d507-213">Restoring a server to a different point in time creates a duplicate new server as the original server as of the point in time you specify, provided that it is within the retention period for your [service tier](./concepts-service-tiers.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d507-214">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d507-214">Next Steps</span></span>
<span data-ttu-id="8d507-215">I kursen får du har lärt dig hur du använder Azure-portalen och andra verktyg för att:</span><span class="sxs-lookup"><span data-stu-id="8d507-215">In this tutorial, you learned how to use the Azure portal and other utilities to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="8d507-216">Skapa en Azure Database för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="8d507-216">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="8d507-217">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="8d507-217">Configure the server firewall</span></span>
> * <span data-ttu-id="8d507-218">Använd [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) verktyg för att skapa en databas</span><span class="sxs-lookup"><span data-stu-id="8d507-218">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility to create a database</span></span>
> * <span data-ttu-id="8d507-219">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="8d507-219">Load sample data</span></span>
> * <span data-ttu-id="8d507-220">Frågedata</span><span class="sxs-lookup"><span data-stu-id="8d507-220">Query data</span></span>
> * <span data-ttu-id="8d507-221">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="8d507-221">Update data</span></span>
> * <span data-ttu-id="8d507-222">Återställa data</span><span class="sxs-lookup"><span data-stu-id="8d507-222">Restore data</span></span>

<span data-ttu-id="8d507-223">Därefter beskrivs hur du använder Azure CLI gör liknande uppgifter genom att granska den här självstudiekursen: [utforma din första Azure-databas för PostgreSQL med Azure CLI](tutorial-design-database-using-azure-cli.md)</span><span class="sxs-lookup"><span data-stu-id="8d507-223">Next, learn how to use Azure CLI to do similar tasks, review this tutorial: [Design your first Azure Database for PostgreSQL using Azure CLI](tutorial-design-database-using-azure-cli.md)</span></span>
