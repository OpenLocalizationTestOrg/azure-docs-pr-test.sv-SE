---
title: "Komma igång med datasynkronisering för Azure SQL (förhandsversion) | Microsoft Docs"
description: "Den här kursen hjälper dig att komma igång med datasynkronisering för Azure SQL (förhandsversion)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: douglasl
ms.openlocfilehash: 2d0f9d7f32ad79f49d58165d734b9df4af862835
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a><span data-ttu-id="46095-103">Komma igång med datasynkronisering för Azure SQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="46095-103">Getting Started with Azure SQL Data Sync (Preview)</span></span>
<span data-ttu-id="46095-104">Lär dig hur du ställer in Azure SQL Data Sync genom att skapa en hybrid sync-grupp som innehåller både Azure SQL Database och SQL Server-instanser i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="46095-104">In this tutorial, you learn how to set up Azure SQL Data Sync by creating a hybrid sync group that contains both Azure SQL Database and SQL Server instances.</span></span> <span data-ttu-id="46095-105">Den nya gruppen sync helt har konfigurerats och synkroniserar enligt det schema du anger.</span><span class="sxs-lookup"><span data-stu-id="46095-105">The new sync group is fully configured and synchronizes on the schedule you set.</span></span>

<span data-ttu-id="46095-106">Den här kursen förutsätter att du har minst tidigare erfarenhet med SQL Database och SQL Server.</span><span class="sxs-lookup"><span data-stu-id="46095-106">This tutorial assumes that you have at least some prior experience with SQL Database and with SQL Server.</span></span> 

<span data-ttu-id="46095-107">En översikt över SQL datasynkronisering finns [data synkroniseras](sql-database-sync-data.md).</span><span class="sxs-lookup"><span data-stu-id="46095-107">For an overview of SQL Data Sync, see [Sync data](sql-database-sync-data.md).</span></span>

<span data-ttu-id="46095-108">Fullständig PowerShell-exempel som visar hur du konfigurerar SQL datasynkronisering, finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="46095-108">For complete PowerShell examples that show how to configure SQL Data Sync, see the following articles:</span></span>
-   [<span data-ttu-id="46095-109">Använd PowerShell för att synkronisera mellan flera Azure SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="46095-109">Use PowerShell to sync between multiple Azure SQL databases</span></span>](scripts/sql-database-sync-data-between-sql-databases.md)
-   [<span data-ttu-id="46095-110">Använd PowerShell för att synkronisera mellan en Azure SQL Database och en lokal SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="46095-110">Use PowerShell to sync between an Azure SQL Database and a SQL Server on-premises database</span></span>](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!NOTE]
> <span data-ttu-id="46095-111">Den omfattande teknisk dokumentation för Azure SQL datasynkronisering som tidigare fanns på MSDN, är tillgänglig som en. PDF-dokument.</span><span class="sxs-lookup"><span data-stu-id="46095-111">The complete technical documentation set for Azure SQL Data Sync, formerly located on MSDN, is available as a .PDF document.</span></span> <span data-ttu-id="46095-112">Hämta den [här](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span><span class="sxs-lookup"><span data-stu-id="46095-112">Download it [here](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span></span>

## <a name="step-1---create-sync-group"></a><span data-ttu-id="46095-113">Steg 1 – Skapa sync-grupp</span><span class="sxs-lookup"><span data-stu-id="46095-113">Step 1 - Create sync group</span></span>

### <a name="locate-the-data-sync-settings"></a><span data-ttu-id="46095-114">Hitta inställningarna för datasynkronisering</span><span class="sxs-lookup"><span data-stu-id="46095-114">Locate the Data Sync settings</span></span>

1.  <span data-ttu-id="46095-115">Gå till Azure-portalen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="46095-115">In your browser, navigate to the Azure portal.</span></span>

2.  <span data-ttu-id="46095-116">Leta upp din SQL-databaser från instrumentpanelen eller ikonen SQL-databaser i verktygsfältet i portalen.</span><span class="sxs-lookup"><span data-stu-id="46095-116">In the portal, locate your SQL databases from your Dashboard or from the SQL Databases icon on the toolbar.</span></span>

    ![Lista över Azure SQL-databaser](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  <span data-ttu-id="46095-118">På den **SQL-databaser** bladet Välj den befintliga SQL-databasen som du vill använda som databasen hubb för datasynkronisering. Då öppnas bladet SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="46095-118">On the **SQL databases** blade, select the existing SQL database that you want to use as the hub database for Data Sync. The SQL database blade opens.</span></span>

4.  <span data-ttu-id="46095-119">På bladet för SQL-databasen för den valda databasen väljer **synkronisering till andra databaser**.</span><span class="sxs-lookup"><span data-stu-id="46095-119">On the SQL database blade for the selected database, select **Sync to other databases**.</span></span> <span data-ttu-id="46095-120">Datasynkronisering blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="46095-120">The Data Sync blade opens.</span></span>

    ![Synkronisera till andra databaser alternativ](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a><span data-ttu-id="46095-122">Skapa en ny grupp för synkronisering</span><span class="sxs-lookup"><span data-stu-id="46095-122">Create a new Sync Group</span></span>

1.  <span data-ttu-id="46095-123">På bladet datasynkronisering väljer **ny synkronisering grupp**.</span><span class="sxs-lookup"><span data-stu-id="46095-123">On the Data Sync blade, select **New Sync Group**.</span></span> <span data-ttu-id="46095-124">Den **ny synkronisering grupp** blad öppnas med steg 1, **skapa sync grupp**, markerade.</span><span class="sxs-lookup"><span data-stu-id="46095-124">The **New sync group** blade opens with Step 1, **Create sync group**, highlighted.</span></span> <span data-ttu-id="46095-125">Den **skapa Sync datagruppen** också öppnas i blad.</span><span class="sxs-lookup"><span data-stu-id="46095-125">The **Create Data Sync Group** blade also opens.</span></span>

2.  <span data-ttu-id="46095-126">På den **skapa Sync datagruppen** bladet göra följande:</span><span class="sxs-lookup"><span data-stu-id="46095-126">On the **Create Data Sync Group** blade, do the following things:</span></span>

    1.  <span data-ttu-id="46095-127">I den **Sync gruppnamn** , ange ett namn för den nya gruppen för synkronisering.</span><span class="sxs-lookup"><span data-stu-id="46095-127">In the **Sync Group Name** field, enter a name for the new sync group.</span></span>

    2.  <span data-ttu-id="46095-128">I den **synkronisera Metadata-databasen** väljer du om du vill skapa en ny databas (rekommenderas) eller använda en befintlig databas.</span><span class="sxs-lookup"><span data-stu-id="46095-128">In the **Sync Metadata Database** section, choose whether to create a new database (recommended) or to use an existing database.</span></span>

        > [!NOTE]
        > <span data-ttu-id="46095-129">Microsoft rekommenderar att du skapar en ny, tom databas som ska användas som synkronisera Metadata-databasen.</span><span class="sxs-lookup"><span data-stu-id="46095-129">Microsoft recommends that you create a new, empty database to use as the Sync Metadata Database.</span></span> <span data-ttu-id="46095-130">Datasynkronisering skapar tabeller i databasen och kör en frekventa arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="46095-130">Data Sync creates tables in this database and runs a frequent workload.</span></span> <span data-ttu-id="46095-131">Den här databasen delas automatiskt som synkronisera Metadata-databasen för alla dina synkroniseringsgrupper i den valda regionen.</span><span class="sxs-lookup"><span data-stu-id="46095-131">This database is automatically shared as the Sync Metadata Database for all of your Sync groups in the selected region.</span></span> <span data-ttu-id="46095-132">Du kan inte ändra synkronisera Metadata-databasen, namnet eller dess servicenivå utan att släppa den.</span><span class="sxs-lookup"><span data-stu-id="46095-132">You can't change the Sync Metadata Database, its name, or its service level without dropping it.</span></span>

        <span data-ttu-id="46095-133">Om du väljer **ny databas**väljer **Skapa ny databas.**</span><span class="sxs-lookup"><span data-stu-id="46095-133">If you chose **New database**, select **Create new database.**</span></span> <span data-ttu-id="46095-134">Den **SQL-databas** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="46095-134">The **SQL Database** blade opens.</span></span> <span data-ttu-id="46095-135">På den **SQL-databas** bladet namn och konfigurera den nya databasen.</span><span class="sxs-lookup"><span data-stu-id="46095-135">On the **SQL Database** blade, name and configure the new database.</span></span> <span data-ttu-id="46095-136">Välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="46095-136">Then select **OK**.</span></span>

        <span data-ttu-id="46095-137">Om du väljer **Använd befintlig databas**, markera databasen i listan.</span><span class="sxs-lookup"><span data-stu-id="46095-137">If you chose **Use existing database**, select the database from the list.</span></span>

    3.  <span data-ttu-id="46095-138">I den **automatisk synkronisering** väljer du först **på** eller **av**.</span><span class="sxs-lookup"><span data-stu-id="46095-138">In the **Automatic Sync** section, first select **On** or **Off**.</span></span>

        <span data-ttu-id="46095-139">Om du väljer **på**i den **Synkroniseringsfrekvensen** , ange ett tal och välj sekunder, minuter, timmar eller dagar.</span><span class="sxs-lookup"><span data-stu-id="46095-139">If you chose **On**, in the **Sync Frequency** section, enter a number and select Seconds, Minutes, Hours, or Days.</span></span>

        ![Ange frekvens för synkronisering](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  <span data-ttu-id="46095-141">I den **konfliktlösning** väljer du ”hubb wins” eller ”medlem wins”.</span><span class="sxs-lookup"><span data-stu-id="46095-141">In the **Conflict Resolution** section, select "Hub wins" or "Member wins."</span></span>

        ![Ange hur konflikter löses](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  <span data-ttu-id="46095-143">Välj **OK** och vänta tills den nya gruppen synkronisering kan skapas och distribueras.</span><span class="sxs-lookup"><span data-stu-id="46095-143">Select **OK** and wait for the new sync group to be created and deployed.</span></span>

## <a name="step-2---add-sync-members"></a><span data-ttu-id="46095-144">Steg 2 – lägga till sync-medlemmar</span><span class="sxs-lookup"><span data-stu-id="46095-144">Step 2 - Add sync members</span></span>

<span data-ttu-id="46095-145">När den nya gruppen sync skapas och distribueras, steg 2, **lägga till medlemmar i synkronisering**, är markerad i den **ny synkronisering grupp** bladet.</span><span class="sxs-lookup"><span data-stu-id="46095-145">After the new sync group is created and deployed, Step 2, **Add sync members**, is highlighted in the **New sync group** blade.</span></span>

<span data-ttu-id="46095-146">I den **hubb databasen** ange befintliga autentiseringsuppgifter för SQL Database-server där NAV-databasen finns.</span><span class="sxs-lookup"><span data-stu-id="46095-146">In the **Hub Database** section, enter the existing credentials for the SQL Database server on which the hub database is located.</span></span> <span data-ttu-id="46095-147">Ange inte *nya* autentiseringsuppgifter i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="46095-147">Don't enter *new* credentials in this section.</span></span>

![NAV-databasen har lagts till synkronisera grupp](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a><span data-ttu-id="46095-149">Lägg till en Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="46095-149">Add an Azure SQL Database</span></span>

<span data-ttu-id="46095-150">I den **medlem databasen** avsnittet om du vill lägga till en Azure SQL Database i gruppen synkronisering genom att välja **lägga till en Azure-databas**.</span><span class="sxs-lookup"><span data-stu-id="46095-150">In the **Member Database** section, optionally add an Azure SQL Database to the sync group by selecting **Add an Azure Database**.</span></span> <span data-ttu-id="46095-151">Den **konfigurera Azure Database** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="46095-151">The **Configure Azure Database** blade opens.</span></span>

<span data-ttu-id="46095-152">På den **konfigurera Azure Database** bladet göra följande:</span><span class="sxs-lookup"><span data-stu-id="46095-152">On the **Configure Azure Database** blade, do the following things:</span></span>

1.  <span data-ttu-id="46095-153">I den **Sync medlemsnamn** anger ett namn för den nya medlemmen synkronisering.</span><span class="sxs-lookup"><span data-stu-id="46095-153">In the **Sync Member Name** field, provide a name for the new sync member.</span></span> <span data-ttu-id="46095-154">Det här namnet skiljer sig från namnet på själva databasen.</span><span class="sxs-lookup"><span data-stu-id="46095-154">This name is distinct from the name of the database itself.</span></span>

2.  <span data-ttu-id="46095-155">I den **prenumeration** , markera den tillhörande Azure-prenumerationen för fakturering.</span><span class="sxs-lookup"><span data-stu-id="46095-155">In the **Subscription** field, select the associated Azure subscription for billing purposes.</span></span>

3.  <span data-ttu-id="46095-156">I den **Azure SQL Server** väljer du den befintliga SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="46095-156">In the **Azure SQL Server** field, select the existing SQL database server.</span></span>

4.  <span data-ttu-id="46095-157">I den **Azure SQL Database** väljer du den befintliga SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="46095-157">In the **Azure SQL Database** field, select the existing SQL database.</span></span>

5.  <span data-ttu-id="46095-158">I den **Sync anvisningarna** fält, Välj dubbelriktad synkronisering, till hubben eller från hubb.</span><span class="sxs-lookup"><span data-stu-id="46095-158">In the **Sync Directions** field, select Bi-directional Sync, To the Hub, or From the Hub.</span></span>

    ![Lägga till en ny SQL-databas sync medlem](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  <span data-ttu-id="46095-160">I den **användarnamn** och **lösenord** ange befintliga autentiseringsuppgifter för SQL Database-servern som medlem databasen finns.</span><span class="sxs-lookup"><span data-stu-id="46095-160">In the **Username** and **Password** fields, enter the existing credentials for the SQL Database server on which the member database is located.</span></span> <span data-ttu-id="46095-161">Ange inte *nya* autentiseringsuppgifter i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="46095-161">Don't enter *new* credentials in this section.</span></span>

7.  <span data-ttu-id="46095-162">Välj **OK** och vänta tills den nya medlemmen synkronisering kan skapas och distribueras.</span><span class="sxs-lookup"><span data-stu-id="46095-162">Select **OK** and wait for the new sync member to be created and deployed.</span></span>

    ![Ny SQL-databas sync medlem har lagts till](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a><span data-ttu-id="46095-164">Lägg till en lokal SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="46095-164">Add an on-premises SQL Server database</span></span>

<span data-ttu-id="46095-165">I den **medlem databasen** avsnittet om du vill lägga till en lokal SQL Server i gruppen synkronisering genom att välja **lägga till en lokal databas**.</span><span class="sxs-lookup"><span data-stu-id="46095-165">In the **Member Database** section, optionally add an on-premises SQL Server to the sync group by selecting **Add an On-Premises Database**.</span></span> <span data-ttu-id="46095-166">Den **konfigurera lokalt** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="46095-166">The **Configure On-Premises** blade opens.</span></span>

<span data-ttu-id="46095-167">På den **konfigurera lokalt** bladet göra följande:</span><span class="sxs-lookup"><span data-stu-id="46095-167">On the **Configure On-Premises** blade, do the following things:</span></span>

1.  <span data-ttu-id="46095-168">Välj **Välj Sync Agent Gateway**.</span><span class="sxs-lookup"><span data-stu-id="46095-168">Select **Choose the Sync Agent Gateway**.</span></span> <span data-ttu-id="46095-169">Den **Välj Agent för Sync** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="46095-169">The **Select Sync Agent** blade opens.</span></span>

    ![Välj sync agent gateway](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  <span data-ttu-id="46095-171">På den **Välj Sync Agent Gateway** bladet Välj om du vill använda en befintlig agent eller skapa en ny agent.</span><span class="sxs-lookup"><span data-stu-id="46095-171">On the **Choose the Sync Agent Gateway** blade, choose whether to use an existing agent or create a new agent.</span></span>

    <span data-ttu-id="46095-172">Om du väljer **befintliga agenter**, Välj den befintliga agenten i listan.</span><span class="sxs-lookup"><span data-stu-id="46095-172">If you chose **Existing agents**, select the existing agent from the list.</span></span>

    <span data-ttu-id="46095-173">Om du väljer **skapa en ny agent**, göra följande:</span><span class="sxs-lookup"><span data-stu-id="46095-173">If you chose **Create a new agent**, do the following things:</span></span>

    1.  <span data-ttu-id="46095-174">Hämta klientprogrammet för sync-agenten från länken och installera den på den dator där SQL Server.</span><span class="sxs-lookup"><span data-stu-id="46095-174">Download the client sync agent software from the link provided and install it on the computer where the SQL Server is located.</span></span>
 
        > [!IMPORTANT]
        > <span data-ttu-id="46095-175">Du måste öppna utgående TCP-port 1433 i brandväggen för att låta klientagenten kommunicera med servern.</span><span class="sxs-lookup"><span data-stu-id="46095-175">You have to open outbound TCP port 1433 in the firewall to let the client agent communicate with the server.</span></span>


    2.  <span data-ttu-id="46095-176">Ange ett namn för agenten.</span><span class="sxs-lookup"><span data-stu-id="46095-176">Enter a name for the agent.</span></span>

    3.  <span data-ttu-id="46095-177">Välj **skapa och generera nyckel**.</span><span class="sxs-lookup"><span data-stu-id="46095-177">Select **Create and Generate Key**.</span></span>

    4.  <span data-ttu-id="46095-178">Kopiera agentnyckeln till Urklipp.</span><span class="sxs-lookup"><span data-stu-id="46095-178">Copy the agent key to the clipboard.</span></span>
        
        ![Skapa en ny agent för synkronisering](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  <span data-ttu-id="46095-180">Välj **OK** att stänga den **Välj Agent för Sync** bladet.</span><span class="sxs-lookup"><span data-stu-id="46095-180">Select **OK** to close the **Select Sync Agent** blade.</span></span>

    6.  <span data-ttu-id="46095-181">Leta upp på SQL Server-datorn och kör appen Sync-Klientagenten.</span><span class="sxs-lookup"><span data-stu-id="46095-181">On the SQL Server computer, locate and run the Client Sync Agent app.</span></span>

        ![Data synkroniseras agent-klientappen](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  <span data-ttu-id="46095-183">I appen sync agent väljer **skicka Agentnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="46095-183">In the sync agent app, select **Submit Agent Key**.</span></span> <span data-ttu-id="46095-184">Den **synkronisera Metadata databaskonfiguration** öppnas.</span><span class="sxs-lookup"><span data-stu-id="46095-184">The **Sync Metadata Database Configuration** dialog box opens.</span></span>

    8.  <span data-ttu-id="46095-185">I den **synkronisera Metadata databaskonfiguration** dialogrutan Klistra in agentnyckeln kopieras från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="46095-185">In the **Sync Metadata Database Configuration** dialog box, paste in the agent key copied from the Azure portal.</span></span> <span data-ttu-id="46095-186">Dessutom befintliga autentiseringsuppgifter för Azure SQL Database-server som metadata-databasen finns.</span><span class="sxs-lookup"><span data-stu-id="46095-186">Also provide the existing credentials for the Azure SQL Database server on which the metadata database is located.</span></span> <span data-ttu-id="46095-187">(Om du har skapat en ny databas för metadata är den här databasen på samma server som NAV-databasen.) Välj **OK** och vänta på att slutföra konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="46095-187">(If you created a new metadata database, this database is on the same server as the hub database.) Select **OK** and wait for the configuration to finish.</span></span>

        ![Ange autentiseringsuppgifter för agenten nyckel och server](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   <span data-ttu-id="46095-189">Om du får ett Brandväggsfel nu kan måste du skapa en brandväggsregel i Azure för att tillåta inkommande trafik från SQL Server-datorn.</span><span class="sxs-lookup"><span data-stu-id="46095-189">If you get a firewall error at this point, you have to create a firewall rule on Azure to allow incoming traffic from the SQL Server computer.</span></span> <span data-ttu-id="46095-190">Du kan skapa regeln manuellt i portalen, men det kan vara lättare att skapa den i SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="46095-190">You can create the rule manually in the portal, but you may find it easier to create it in SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="46095-191">I SSMS, försök ansluta till NAV-databas på Azure.</span><span class="sxs-lookup"><span data-stu-id="46095-191">In SSMS, try to connect to the hub database on Azure.</span></span> <span data-ttu-id="46095-192">Ange namnet som \<hub_database_name\>. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="46095-192">Enter its name as \<hub_database_name\>.database.windows.net.</span></span> <span data-ttu-id="46095-193">Följ stegen i dialogrutan för att konfigurera Azure brandväggsregeln.</span><span class="sxs-lookup"><span data-stu-id="46095-193">Follow the steps in the dialog box to configure the Azure firewall rule.</span></span> <span data-ttu-id="46095-194">Återgå sedan till appen Sync-Klientagenten.</span><span class="sxs-lookup"><span data-stu-id="46095-194">Then return to the Client Sync Agent app.</span></span>

    9.  <span data-ttu-id="46095-195">Klicka på appen Sync-Klientagenten **registrera** att registrera en SQL Server-databas med agenten.</span><span class="sxs-lookup"><span data-stu-id="46095-195">In the Client Sync Agent app, click **Register** to register a SQL Server database with the agent.</span></span> <span data-ttu-id="46095-196">Den **SQL Server-konfigurationsfilen** öppnas.</span><span class="sxs-lookup"><span data-stu-id="46095-196">The **SQL Server Configuration** dialog box opens.</span></span>

        ![Lägga till och konfigurera en SQL Server-databas](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. <span data-ttu-id="46095-198">I den **SQL Server-konfigurationsfilen** dialogrutan Välj om du vill ansluta med hjälp av SQL Server-autentisering eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="46095-198">In the **SQL Server Configuration** dialog box, choose whether to connect by using SQL Server authentication or Windows authentication.</span></span> <span data-ttu-id="46095-199">Om du väljer SQL Server-autentisering anger du de befintliga autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="46095-199">If you chose SQL Server authentication, enter the existing credentials.</span></span> <span data-ttu-id="46095-200">Ange namnet på SQL Server och namnet på databasen som du vill synkronisera. Välj **Anslutningstestet** testa inställningarna.</span><span class="sxs-lookup"><span data-stu-id="46095-200">Provide the SQL Server name and the name of the database that you want to sync. Select **Test connection** to test your settings.</span></span> <span data-ttu-id="46095-201">Välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="46095-201">Then select **Save**.</span></span> <span data-ttu-id="46095-202">Registrerade databasen visas i listan.</span><span class="sxs-lookup"><span data-stu-id="46095-202">The registered database appears in the list.</span></span>

        ![SQL Server-databasen är nu registrerad](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. <span data-ttu-id="46095-204">Nu kan du stänga appen Sync-Klientagenten.</span><span class="sxs-lookup"><span data-stu-id="46095-204">You can now close the Client Sync Agent app.</span></span>

    12. <span data-ttu-id="46095-205">I portalen på den **konfigurera lokalt** bladet väljer **Markera databasen.**</span><span class="sxs-lookup"><span data-stu-id="46095-205">In the portal, on the **Configure On-Premises** blade, select **Select the Database.**</span></span> <span data-ttu-id="46095-206">Den **Välj databas** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="46095-206">The **Select Database** blade opens.</span></span>

    13. <span data-ttu-id="46095-207">På den **Välj databas** blad i den **Sync medlemsnamn** anger ett namn för den nya medlemmen synkronisering.</span><span class="sxs-lookup"><span data-stu-id="46095-207">On the **Select Database** blade, in the **Sync Member Name** field, provide a name for the new sync member.</span></span> <span data-ttu-id="46095-208">Det här namnet skiljer sig från namnet på själva databasen.</span><span class="sxs-lookup"><span data-stu-id="46095-208">This name is distinct from the name of the database itself.</span></span> <span data-ttu-id="46095-209">Välj databasen i listan.</span><span class="sxs-lookup"><span data-stu-id="46095-209">Select the database from the list.</span></span> <span data-ttu-id="46095-210">I den **Sync anvisningarna** fält, Välj dubbelriktad synkronisering, till hubben eller från hubb.</span><span class="sxs-lookup"><span data-stu-id="46095-210">In the **Sync Directions** field, select Bi-directional Sync, To the Hub, or From the Hub.</span></span>

        ![Välj den på lokal databasen](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. <span data-ttu-id="46095-212">Välj **OK** att stänga den **Välj databas** bladet.</span><span class="sxs-lookup"><span data-stu-id="46095-212">Select **OK** to close the **Select Database** blade.</span></span> <span data-ttu-id="46095-213">Välj sedan **OK** att stänga den **konfigurera lokalt** bladet och vänta tills den nya medlemmen synkronisering kan skapas och distribueras.</span><span class="sxs-lookup"><span data-stu-id="46095-213">Then select **OK** to close the **Configure On-Premises** blade and wait for the new sync member to be created and deployed.</span></span> <span data-ttu-id="46095-214">Klicka slutligen på **OK** att stänga den **Välj sync medlemmar** bladet.</span><span class="sxs-lookup"><span data-stu-id="46095-214">Finally, click **OK** to close the **Select sync members** blade.</span></span>

        ![På lokal databas sync-grupp](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  <span data-ttu-id="46095-216">Om du vill ansluta till SQL-datasynkronisering och lokal agent, Lägg till ditt användarnamn i rollen `DataSync_Executor`.</span><span class="sxs-lookup"><span data-stu-id="46095-216">To connect to SQL Data Sync and the local agent, add your user name to the role `DataSync_Executor`.</span></span> <span data-ttu-id="46095-217">Datasynkronisering skapar den här rollen på SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="46095-217">Data Sync creates this role on the SQL Server instance.</span></span>

## <a name="step-3---configure-sync-group"></a><span data-ttu-id="46095-218">Steg 3 – Konfigurera synkronisering grupp</span><span class="sxs-lookup"><span data-stu-id="46095-218">Step 3 - Configure sync group</span></span>

<span data-ttu-id="46095-219">När de nya sync gruppmedlemmarna skapas och distribueras, steg3 **Konfigurera synkronisering grupp**, är markerat i den **ny synkronisering grupp** bladet.</span><span class="sxs-lookup"><span data-stu-id="46095-219">After the new sync group members are created and deployed, Step 3, **Configure sync group**, is highlighted in the **New sync group** blade.</span></span>

1.  <span data-ttu-id="46095-220">På den **tabeller** bladet Välj en databas i listan över medlemmar och välj sedan **Uppdatera schema**.</span><span class="sxs-lookup"><span data-stu-id="46095-220">On the **Tables** blade, select a database from the list of sync group members, and then select **Refresh schema**.</span></span>

2.  <span data-ttu-id="46095-221">Välj de tabeller som du vill synkronisera från listan över tillgängliga tabeller.</span><span class="sxs-lookup"><span data-stu-id="46095-221">From the list of available tables, select the tables that you want to sync.</span></span>

    ![Välj tabeller ska synkroniseras](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  <span data-ttu-id="46095-223">Alla kolumner i tabellen är markerad som standard.</span><span class="sxs-lookup"><span data-stu-id="46095-223">By default, all columns in the table are selected.</span></span> <span data-ttu-id="46095-224">Om du inte vill synkronisera alla kolumner, inaktivera kryssrutan för de kolumner som du inte vill synkronisera. Se till att lämna primärnyckelkolumnen har valts.</span><span class="sxs-lookup"><span data-stu-id="46095-224">If you don't want to sync all the columns, disable the checkbox for the columns that you don't want to sync. Be sure to leave the primary key column selected.</span></span>

    ![Välj fält som ska synkroniseras](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  <span data-ttu-id="46095-226">Välj slutligen **spara**.</span><span class="sxs-lookup"><span data-stu-id="46095-226">Finally, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46095-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46095-227">Next steps</span></span>
<span data-ttu-id="46095-228">Grattis.</span><span class="sxs-lookup"><span data-stu-id="46095-228">Congratulations.</span></span> <span data-ttu-id="46095-229">Du har skapat en sync-grupp som innehåller både en instans av SQL Database och SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="46095-229">You have created a sync group that includes both a SQL Database instance and a SQL Server database.</span></span>

<span data-ttu-id="46095-230">För mer information om SQL Database och SQL-datasynkronisering, se:</span><span class="sxs-lookup"><span data-stu-id="46095-230">For more info about SQL Database and SQL Data Sync, see:</span></span>

-   [<span data-ttu-id="46095-231">Hämta den fullständiga SQL datasynkronisering teknisk dokumentationen</span><span class="sxs-lookup"><span data-stu-id="46095-231">Download the complete SQL Data Sync technical documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [<span data-ttu-id="46095-232">Hämta SQL Data Sync REST API-dokumentation</span><span class="sxs-lookup"><span data-stu-id="46095-232">Download the SQL Data Sync REST API documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [<span data-ttu-id="46095-233">Översikt över SQL-databas</span><span class="sxs-lookup"><span data-stu-id="46095-233">SQL Database Overview</span></span>](sql-database-technical-overview.md)
-   [<span data-ttu-id="46095-234">Livscykelhantering för databasen</span><span class="sxs-lookup"><span data-stu-id="46095-234">Database Lifecycle Management</span></span>](https://msdn.microsoft.com/library/jj907294.aspx)
