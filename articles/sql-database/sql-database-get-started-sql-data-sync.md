---
title: "aaaGetting igång med datasynkronisering för Azure SQL (förhandsversion) | Microsoft Docs"
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
ms.openlocfilehash: 666d09237e42acc23ae3c8c81e60734a413f5949
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a><span data-ttu-id="9d613-103">Komma igång med datasynkronisering för Azure SQL (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="9d613-103">Getting Started with Azure SQL Data Sync (Preview)</span></span>
<span data-ttu-id="9d613-104">I kursen får du lära dig hur tooset upp Azure SQL Data Sync genom att skapa en hybrid sync-grupp som innehåller både Azure SQL Database och SQL Server-instanser.</span><span class="sxs-lookup"><span data-stu-id="9d613-104">In this tutorial, you learn how tooset up Azure SQL Data Sync by creating a hybrid sync group that contains both Azure SQL Database and SQL Server instances.</span></span> <span data-ttu-id="9d613-105">hello ny synkronisering grupp helt har konfigurerats och synkroniserar på hello schema du anger.</span><span class="sxs-lookup"><span data-stu-id="9d613-105">hello new sync group is fully configured and synchronizes on hello schedule you set.</span></span>

<span data-ttu-id="9d613-106">Den här kursen förutsätter att du har minst tidigare erfarenhet med SQL Database och SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9d613-106">This tutorial assumes that you have at least some prior experience with SQL Database and with SQL Server.</span></span> 

<span data-ttu-id="9d613-107">En översikt över SQL datasynkronisering finns [data synkroniseras](sql-database-sync-data.md).</span><span class="sxs-lookup"><span data-stu-id="9d613-107">For an overview of SQL Data Sync, see [Sync data](sql-database-sync-data.md).</span></span>

<span data-ttu-id="9d613-108">Fullständig PowerShell-exempel som visar hur tooconfigure SQL datasynkronisering Se hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="9d613-108">For complete PowerShell examples that show how tooconfigure SQL Data Sync, see hello following articles:</span></span>
-   [<span data-ttu-id="9d613-109">Använd PowerShell toosync mellan flera Azure SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="9d613-109">Use PowerShell toosync between multiple Azure SQL databases</span></span>](scripts/sql-database-sync-data-between-sql-databases.md)
-   [<span data-ttu-id="9d613-110">Använd PowerShell toosync mellan en Azure SQL Database och en lokal SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="9d613-110">Use PowerShell toosync between an Azure SQL Database and a SQL Server on-premises database</span></span>](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!NOTE]
> <span data-ttu-id="9d613-111">hello omfattande teknisk dokumentation för Azure SQL datasynkronisering som tidigare fanns på MSDN, är tillgänglig som en. PDF-dokument.</span><span class="sxs-lookup"><span data-stu-id="9d613-111">hello complete technical documentation set for Azure SQL Data Sync, formerly located on MSDN, is available as a .PDF document.</span></span> <span data-ttu-id="9d613-112">Hämta den [här](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span><span class="sxs-lookup"><span data-stu-id="9d613-112">Download it [here](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).</span></span>

## <a name="step-1---create-sync-group"></a><span data-ttu-id="9d613-113">Steg 1 – Skapa sync-grupp</span><span class="sxs-lookup"><span data-stu-id="9d613-113">Step 1 - Create sync group</span></span>

### <a name="locate-hello-data-sync-settings"></a><span data-ttu-id="9d613-114">Hitta inställningarna för hello datasynkronisering</span><span class="sxs-lookup"><span data-stu-id="9d613-114">Locate hello Data Sync settings</span></span>

1.  <span data-ttu-id="9d613-115">Navigera toohello Azure-portalen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="9d613-115">In your browser, navigate toohello Azure portal.</span></span>

2.  <span data-ttu-id="9d613-116">Leta upp din SQL-databaser från instrumentpanelen eller hello SQL-databaser ikon hello verktygsfältet i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="9d613-116">In hello portal, locate your SQL databases from your Dashboard or from hello SQL Databases icon on hello toolbar.</span></span>

    ![Lista över Azure SQL-databaser](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  <span data-ttu-id="9d613-118">På hello **SQL-databaser** bladet, Välj hello befintliga SQL-databas som du vill toouse som hello hubb databasen för Data Sync. hello SQL-databasbladet öppnas.</span><span class="sxs-lookup"><span data-stu-id="9d613-118">On hello **SQL databases** blade, select hello existing SQL database that you want toouse as hello hub database for Data Sync. hello SQL database blade opens.</span></span>

4.  <span data-ttu-id="9d613-119">Välj på hello SQL-databasbladet för hello valda databasen **synkronisera tooother databaser**.</span><span class="sxs-lookup"><span data-stu-id="9d613-119">On hello SQL database blade for hello selected database, select **Sync tooother databases**.</span></span> <span data-ttu-id="9d613-120">hello datasynkronisering blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="9d613-120">hello Data Sync blade opens.</span></span>

    ![Synkroniseringsalternativet tooother databaser](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a><span data-ttu-id="9d613-122">Skapa en ny grupp för synkronisering</span><span class="sxs-lookup"><span data-stu-id="9d613-122">Create a new Sync Group</span></span>

1.  <span data-ttu-id="9d613-123">Hello datasynkronisering bladet välj **ny synkronisering grupp**.</span><span class="sxs-lookup"><span data-stu-id="9d613-123">On hello Data Sync blade, select **New Sync Group**.</span></span> <span data-ttu-id="9d613-124">Hej **ny synkronisering grupp** blad öppnas med steg 1, **skapa sync grupp**, markerade.</span><span class="sxs-lookup"><span data-stu-id="9d613-124">hello **New sync group** blade opens with Step 1, **Create sync group**, highlighted.</span></span> <span data-ttu-id="9d613-125">Hej **skapa Sync datagruppen** också öppnas i blad.</span><span class="sxs-lookup"><span data-stu-id="9d613-125">hello **Create Data Sync Group** blade also opens.</span></span>

2.  <span data-ttu-id="9d613-126">På hello **skapa Sync datagruppen** bladet hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="9d613-126">On hello **Create Data Sync Group** blade, do hello following things:</span></span>

    1.  <span data-ttu-id="9d613-127">I hello **Sync gruppnamn** , ange ett namn för hello nya sync-gruppen.</span><span class="sxs-lookup"><span data-stu-id="9d613-127">In hello **Sync Group Name** field, enter a name for hello new sync group.</span></span>

    2.  <span data-ttu-id="9d613-128">I hello **synkronisera Metadata-databasen** väljer du om toocreate en ny databas (rekommenderas) eller toouse en befintlig databas.</span><span class="sxs-lookup"><span data-stu-id="9d613-128">In hello **Sync Metadata Database** section, choose whether toocreate a new database (recommended) or toouse an existing database.</span></span>

        > [!NOTE]
        > <span data-ttu-id="9d613-129">Microsoft rekommenderar att du skapar en ny, tom databas toouse som hello synkronisera Metadata-databasen.</span><span class="sxs-lookup"><span data-stu-id="9d613-129">Microsoft recommends that you create a new, empty database toouse as hello Sync Metadata Database.</span></span> <span data-ttu-id="9d613-130">Datasynkronisering skapar tabeller i databasen och kör en frekventa arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="9d613-130">Data Sync creates tables in this database and runs a frequent workload.</span></span> <span data-ttu-id="9d613-131">Den här databasen delas automatiskt som hello synkronisera Metadata-databasen för alla dina synkroniseringsgrupper i hello valda regionen.</span><span class="sxs-lookup"><span data-stu-id="9d613-131">This database is automatically shared as hello Sync Metadata Database for all of your Sync groups in hello selected region.</span></span> <span data-ttu-id="9d613-132">Du kan inte ändra hello synkronisera Metadata-databasen, namnet eller dess servicenivå utan att släppa den.</span><span class="sxs-lookup"><span data-stu-id="9d613-132">You can't change hello Sync Metadata Database, its name, or its service level without dropping it.</span></span>

        <span data-ttu-id="9d613-133">Om du väljer **ny databas**väljer **Skapa ny databas.**</span><span class="sxs-lookup"><span data-stu-id="9d613-133">If you chose **New database**, select **Create new database.**</span></span> <span data-ttu-id="9d613-134">Hej **SQL-databas** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="9d613-134">hello **SQL Database** blade opens.</span></span> <span data-ttu-id="9d613-135">På hello **SQL-databas** bladet namn och konfigurera hello ny databas.</span><span class="sxs-lookup"><span data-stu-id="9d613-135">On hello **SQL Database** blade, name and configure hello new database.</span></span> <span data-ttu-id="9d613-136">Välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d613-136">Then select **OK**.</span></span>

        <span data-ttu-id="9d613-137">Om du väljer **Använd befintlig databas**väljer hello databasen hello-listan.</span><span class="sxs-lookup"><span data-stu-id="9d613-137">If you chose **Use existing database**, select hello database from hello list.</span></span>

    3.  <span data-ttu-id="9d613-138">I hello **automatisk synkronisering** väljer du först **på** eller **av**.</span><span class="sxs-lookup"><span data-stu-id="9d613-138">In hello **Automatic Sync** section, first select **On** or **Off**.</span></span>

        <span data-ttu-id="9d613-139">Om du väljer **på**, i hello **Synkroniseringsfrekvensen** , ange ett tal och välj sekunder, minuter, timmar eller dagar.</span><span class="sxs-lookup"><span data-stu-id="9d613-139">If you chose **On**, in hello **Sync Frequency** section, enter a number and select Seconds, Minutes, Hours, or Days.</span></span>

        ![Ange frekvens för synkronisering](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  <span data-ttu-id="9d613-141">I hello **konfliktlösning** väljer du ”hubb wins” eller ”medlem wins”.</span><span class="sxs-lookup"><span data-stu-id="9d613-141">In hello **Conflict Resolution** section, select "Hub wins" or "Member wins."</span></span>

        ![Ange hur konflikter löses](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  <span data-ttu-id="9d613-143">Välj **OK** och vänta tills hello nya sync grupp toobe skapas och distribueras.</span><span class="sxs-lookup"><span data-stu-id="9d613-143">Select **OK** and wait for hello new sync group toobe created and deployed.</span></span>

## <a name="step-2---add-sync-members"></a><span data-ttu-id="9d613-144">Steg 2 – lägga till sync-medlemmar</span><span class="sxs-lookup"><span data-stu-id="9d613-144">Step 2 - Add sync members</span></span>

<span data-ttu-id="9d613-145">När hello ny synkronisering grupp skapas och distribueras, steg 2, **lägga till medlemmar i synkronisering**, markeras i hello **ny synkronisering grupp** bladet.</span><span class="sxs-lookup"><span data-stu-id="9d613-145">After hello new sync group is created and deployed, Step 2, **Add sync members**, is highlighted in hello **New sync group** blade.</span></span>

<span data-ttu-id="9d613-146">I hello **hubb databasen** ange hello befintliga autentiseringsuppgifter för hello SQL Database-server som hello hubb databasen finns.</span><span class="sxs-lookup"><span data-stu-id="9d613-146">In hello **Hub Database** section, enter hello existing credentials for hello SQL Database server on which hello hub database is located.</span></span> <span data-ttu-id="9d613-147">Ange inte *nya* autentiseringsuppgifter i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="9d613-147">Don't enter *new* credentials in this section.</span></span>

![NAV-databasen har lagts till toosync grupp](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a><span data-ttu-id="9d613-149">Lägg till en Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="9d613-149">Add an Azure SQL Database</span></span>

<span data-ttu-id="9d613-150">I hello **medlem databasen** avsnittet om du vill lägga till en Azure SQL Database toohello sync-grupp genom att välja **lägga till en Azure-databas**.</span><span class="sxs-lookup"><span data-stu-id="9d613-150">In hello **Member Database** section, optionally add an Azure SQL Database toohello sync group by selecting **Add an Azure Database**.</span></span> <span data-ttu-id="9d613-151">Hej **konfigurera Azure Database** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="9d613-151">hello **Configure Azure Database** blade opens.</span></span>

<span data-ttu-id="9d613-152">På hello **konfigurera Azure Database** bladet hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="9d613-152">On hello **Configure Azure Database** blade, do hello following things:</span></span>

1.  <span data-ttu-id="9d613-153">I hello **Sync medlemsnamn** anger ett namn för hello nya sync medlemmen.</span><span class="sxs-lookup"><span data-stu-id="9d613-153">In hello **Sync Member Name** field, provide a name for hello new sync member.</span></span> <span data-ttu-id="9d613-154">Det här namnet skiljer sig från hello namnet på själva hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="9d613-154">This name is distinct from hello name of hello database itself.</span></span>

2.  <span data-ttu-id="9d613-155">I hello **prenumeration** , markera hello som är associerade med Azure-prenumeration för fakturering.</span><span class="sxs-lookup"><span data-stu-id="9d613-155">In hello **Subscription** field, select hello associated Azure subscription for billing purposes.</span></span>

3.  <span data-ttu-id="9d613-156">I hello **Azure SQL Server** , markera hello befintliga SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="9d613-156">In hello **Azure SQL Server** field, select hello existing SQL database server.</span></span>

4.  <span data-ttu-id="9d613-157">I hello **Azure SQL Database** , markera hello befintliga SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="9d613-157">In hello **Azure SQL Database** field, select hello existing SQL database.</span></span>

5.  <span data-ttu-id="9d613-158">I hello **Sync anvisningarna** väljer dubbelriktad synkronisering toohello hubben, eller från hello hubb.</span><span class="sxs-lookup"><span data-stu-id="9d613-158">In hello **Sync Directions** field, select Bi-directional Sync, toohello Hub, or From hello Hub.</span></span>

    ![Lägga till en ny SQL-databas sync medlem](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  <span data-ttu-id="9d613-160">I hello **användarnamn** och **lösenord** ange hello befintliga autentiseringsuppgifter för hello SQL Database-server på vilken hello medlem databasen är placerad.</span><span class="sxs-lookup"><span data-stu-id="9d613-160">In hello **Username** and **Password** fields, enter hello existing credentials for hello SQL Database server on which hello member database is located.</span></span> <span data-ttu-id="9d613-161">Ange inte *nya* autentiseringsuppgifter i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="9d613-161">Don't enter *new* credentials in this section.</span></span>

7.  <span data-ttu-id="9d613-162">Välj **OK** och vänta tills hello nya sync medlem toobe skapas och distribueras.</span><span class="sxs-lookup"><span data-stu-id="9d613-162">Select **OK** and wait for hello new sync member toobe created and deployed.</span></span>

    ![Ny SQL-databas sync medlem har lagts till](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a><span data-ttu-id="9d613-164">Lägg till en lokal SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="9d613-164">Add an on-premises SQL Server database</span></span>

<span data-ttu-id="9d613-165">I hello **medlem databasen** avsnittet om du vill lägga till en lokal SQL Server toohello sync-grupp genom att välja **lägga till en lokal databas**.</span><span class="sxs-lookup"><span data-stu-id="9d613-165">In hello **Member Database** section, optionally add an on-premises SQL Server toohello sync group by selecting **Add an On-Premises Database**.</span></span> <span data-ttu-id="9d613-166">Hej **konfigurera lokalt** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="9d613-166">hello **Configure On-Premises** blade opens.</span></span>

<span data-ttu-id="9d613-167">På hello **konfigurera lokalt** bladet hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="9d613-167">On hello **Configure On-Premises** blade, do hello following things:</span></span>

1.  <span data-ttu-id="9d613-168">Välj **Välj hello Sync Agent Gateway**.</span><span class="sxs-lookup"><span data-stu-id="9d613-168">Select **Choose hello Sync Agent Gateway**.</span></span> <span data-ttu-id="9d613-169">Hej **Välj Agent för Sync** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="9d613-169">hello **Select Sync Agent** blade opens.</span></span>

    ![Välj hello sync agent gateway](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  <span data-ttu-id="9d613-171">På hello **Välj hello Sync Agent Gateway** bladet Välj om toouse en befintlig agent eller skapa en ny agent.</span><span class="sxs-lookup"><span data-stu-id="9d613-171">On hello **Choose hello Sync Agent Gateway** blade, choose whether toouse an existing agent or create a new agent.</span></span>

    <span data-ttu-id="9d613-172">Om du väljer **befintliga agenter**, Välj hello befintliga agenten hello-listan.</span><span class="sxs-lookup"><span data-stu-id="9d613-172">If you chose **Existing agents**, select hello existing agent from hello list.</span></span>

    <span data-ttu-id="9d613-173">Om du väljer **skapa en ny agent**, hello följande saker:</span><span class="sxs-lookup"><span data-stu-id="9d613-173">If you chose **Create a new agent**, do hello following things:</span></span>

    1.  <span data-ttu-id="9d613-174">Hämta hello klientprogrammet sync-agenten från hello länken och installera den på hello datorn där hello SQL Server finns.</span><span class="sxs-lookup"><span data-stu-id="9d613-174">Download hello client sync agent software from hello link provided and install it on hello computer where hello SQL Server is located.</span></span>
 
        > [!IMPORTANT]
        > <span data-ttu-id="9d613-175">Du har tooopen utgående TCP-port 1433 i hello brandväggen toolet hello klientagenten kommunicera med hello-servern.</span><span class="sxs-lookup"><span data-stu-id="9d613-175">You have tooopen outbound TCP port 1433 in hello firewall toolet hello client agent communicate with hello server.</span></span>


    2.  <span data-ttu-id="9d613-176">Ange ett namn för hello agent.</span><span class="sxs-lookup"><span data-stu-id="9d613-176">Enter a name for hello agent.</span></span>

    3.  <span data-ttu-id="9d613-177">Välj **skapa och generera nyckel**.</span><span class="sxs-lookup"><span data-stu-id="9d613-177">Select **Create and Generate Key**.</span></span>

    4.  <span data-ttu-id="9d613-178">Kopiera hello agent viktiga toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="9d613-178">Copy hello agent key toohello clipboard.</span></span>
        
        ![Skapa en ny agent för synkronisering](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  <span data-ttu-id="9d613-180">Välj **OK** tooclose hello **Välj Agent för Sync** bladet.</span><span class="sxs-lookup"><span data-stu-id="9d613-180">Select **OK** tooclose hello **Select Sync Agent** blade.</span></span>

    6.  <span data-ttu-id="9d613-181">Leta reda på hello SQL Server-datorn och kör hello Sync-Klientagenten app.</span><span class="sxs-lookup"><span data-stu-id="9d613-181">On hello SQL Server computer, locate and run hello Client Sync Agent app.</span></span>

        ![hello data synkroniseras agent-klientappen](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  <span data-ttu-id="9d613-183">I hello sync agent app väljer **skicka Agentnyckeln**.</span><span class="sxs-lookup"><span data-stu-id="9d613-183">In hello sync agent app, select **Submit Agent Key**.</span></span> <span data-ttu-id="9d613-184">Hej **synkronisera Metadata databaskonfiguration** öppnas.</span><span class="sxs-lookup"><span data-stu-id="9d613-184">hello **Sync Metadata Database Configuration** dialog box opens.</span></span>

    8.  <span data-ttu-id="9d613-185">I hello **synkronisera Metadata databaskonfiguration** dialogrutan Klistra in hello agentnyckeln kopieras från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9d613-185">In hello **Sync Metadata Database Configuration** dialog box, paste in hello agent key copied from hello Azure portal.</span></span> <span data-ttu-id="9d613-186">Dessutom hello befintliga autentiseringsuppgifter för hello Azure SQL Database-server på vilken hello metadata-databasen finns.</span><span class="sxs-lookup"><span data-stu-id="9d613-186">Also provide hello existing credentials for hello Azure SQL Database server on which hello metadata database is located.</span></span> <span data-ttu-id="9d613-187">(Om du har skapat en ny databas för metadata för databasen finns i hello samma server som hello hubb databasen.) Välj **OK** och vänta tills hello configuration toofinish.</span><span class="sxs-lookup"><span data-stu-id="9d613-187">(If you created a new metadata database, this database is on hello same server as hello hub database.) Select **OK** and wait for hello configuration toofinish.</span></span>

        ![Ange hello och autentiseringsuppgifter för agenten](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   <span data-ttu-id="9d613-189">Om du får ett Brandväggsfel nu har du toocreate en brandväggsregel på Azure tooallow inkommande trafik från hello SQL Server-datorn.</span><span class="sxs-lookup"><span data-stu-id="9d613-189">If you get a firewall error at this point, you have toocreate a firewall rule on Azure tooallow incoming traffic from hello SQL Server computer.</span></span> <span data-ttu-id="9d613-190">Du kan skapa hello regel manuellt i hello-portalen, men kan det vara lättare toocreate den i SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="9d613-190">You can create hello rule manually in hello portal, but you may find it easier toocreate it in SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="9d613-191">Försök i SSMS, tooconnect toohello NAV-databas på Azure.</span><span class="sxs-lookup"><span data-stu-id="9d613-191">In SSMS, try tooconnect toohello hub database on Azure.</span></span> <span data-ttu-id="9d613-192">Ange namnet som \<hub_database_name\>. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="9d613-192">Enter its name as \<hub_database_name\>.database.windows.net.</span></span> <span data-ttu-id="9d613-193">Hello åtgärderna i hello dialogrutan rutan tooconfigure hello Azure brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="9d613-193">Follow hello steps in hello dialog box tooconfigure hello Azure firewall rule.</span></span> <span data-ttu-id="9d613-194">Returnera toohello Sync-Klientagenten app.</span><span class="sxs-lookup"><span data-stu-id="9d613-194">Then return toohello Client Sync Agent app.</span></span>

    9.  <span data-ttu-id="9d613-195">Klicka i hello Sync-Klientagenten app **registrera** tooregister en SQL Server-databas med hello agent.</span><span class="sxs-lookup"><span data-stu-id="9d613-195">In hello Client Sync Agent app, click **Register** tooregister a SQL Server database with hello agent.</span></span> <span data-ttu-id="9d613-196">Hej **SQL Server-konfigurationsfilen** öppnas.</span><span class="sxs-lookup"><span data-stu-id="9d613-196">hello **SQL Server Configuration** dialog box opens.</span></span>

        ![Lägga till och konfigurera en SQL Server-databas](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. <span data-ttu-id="9d613-198">I hello **SQL Server-konfigurationsfilen** dialogrutan Välj om tooconnect med hjälp av SQL Server-autentisering eller Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="9d613-198">In hello **SQL Server Configuration** dialog box, choose whether tooconnect by using SQL Server authentication or Windows authentication.</span></span> <span data-ttu-id="9d613-199">Om du väljer SQL Server-autentisering måste du ange hello befintliga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="9d613-199">If you chose SQL Server authentication, enter hello existing credentials.</span></span> <span data-ttu-id="9d613-200">Ange hello SQL Server-namn och hello namnet på hello-databasen som du vill toosync.</span><span class="sxs-lookup"><span data-stu-id="9d613-200">Provide hello SQL Server name and hello name of hello database that you want toosync.</span></span> <span data-ttu-id="9d613-201">Välj **Testanslutningen** tootest dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="9d613-201">Select **Test connection** tootest your settings.</span></span> <span data-ttu-id="9d613-202">Välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="9d613-202">Then select **Save**.</span></span> <span data-ttu-id="9d613-203">hello registrerade databasen visas i hello.</span><span class="sxs-lookup"><span data-stu-id="9d613-203">hello registered database appears in hello list.</span></span>

        ![SQL Server-databasen är nu registrerad](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. <span data-ttu-id="9d613-205">Nu kan du stänga hello Sync-Klientagenten app.</span><span class="sxs-lookup"><span data-stu-id="9d613-205">You can now close hello Client Sync Agent app.</span></span>

    12. <span data-ttu-id="9d613-206">I hello-portalen på hello **konfigurera lokalt** bladet väljer **Välj hello databasen.**</span><span class="sxs-lookup"><span data-stu-id="9d613-206">In hello portal, on hello **Configure On-Premises** blade, select **Select hello Database.**</span></span> <span data-ttu-id="9d613-207">Hej **Välj databas** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="9d613-207">hello **Select Database** blade opens.</span></span>

    13. <span data-ttu-id="9d613-208">På hello **Välj databas** bladet i hello **Sync medlemsnamn** anger ett namn för hello nya sync medlemmen.</span><span class="sxs-lookup"><span data-stu-id="9d613-208">On hello **Select Database** blade, in hello **Sync Member Name** field, provide a name for hello new sync member.</span></span> <span data-ttu-id="9d613-209">Det här namnet skiljer sig från hello namnet på själva hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="9d613-209">This name is distinct from hello name of hello database itself.</span></span> <span data-ttu-id="9d613-210">Välj hello databasen hello-listan.</span><span class="sxs-lookup"><span data-stu-id="9d613-210">Select hello database from hello list.</span></span> <span data-ttu-id="9d613-211">I hello **Sync anvisningarna** väljer dubbelriktad synkronisering toohello hubben, eller från hello hubb.</span><span class="sxs-lookup"><span data-stu-id="9d613-211">In hello **Sync Directions** field, select Bi-directional Sync, toohello Hub, or From hello Hub.</span></span>

        ![Välj hello på lokal databas](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. <span data-ttu-id="9d613-213">Välj **OK** tooclose hello **Välj databas** bladet.</span><span class="sxs-lookup"><span data-stu-id="9d613-213">Select **OK** tooclose hello **Select Database** blade.</span></span> <span data-ttu-id="9d613-214">Välj sedan **OK** tooclose hello **konfigurera lokalt** bladet och vänta tills hello nya synkroniseras medlem toobe skapas och distribueras.</span><span class="sxs-lookup"><span data-stu-id="9d613-214">Then select **OK** tooclose hello **Configure On-Premises** blade and wait for hello new sync member toobe created and deployed.</span></span> <span data-ttu-id="9d613-215">Klicka slutligen på **OK** tooclose hello **Välj sync medlemmar** bladet.</span><span class="sxs-lookup"><span data-stu-id="9d613-215">Finally, click **OK** tooclose hello **Select sync members** blade.</span></span>

        ![På en lokal databas som lagts till toosync grupp](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  <span data-ttu-id="9d613-217">tooconnect tooSQL datasynkronisering och hello lokal agent, lägger du till din användarroll namn toohello `DataSync_Executor`.</span><span class="sxs-lookup"><span data-stu-id="9d613-217">tooconnect tooSQL Data Sync and hello local agent, add your user name toohello role `DataSync_Executor`.</span></span> <span data-ttu-id="9d613-218">Datasynkronisering skapar den här rollen på hello SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="9d613-218">Data Sync creates this role on hello SQL Server instance.</span></span>

## <a name="step-3---configure-sync-group"></a><span data-ttu-id="9d613-219">Steg 3 – Konfigurera synkronisering grupp</span><span class="sxs-lookup"><span data-stu-id="9d613-219">Step 3 - Configure sync group</span></span>

<span data-ttu-id="9d613-220">När gruppmedlemmar hello nya sync skapas och distribueras, steg3 **Konfigurera synkronisering grupp**, markeras i hello **ny synkronisering grupp** bladet.</span><span class="sxs-lookup"><span data-stu-id="9d613-220">After hello new sync group members are created and deployed, Step 3, **Configure sync group**, is highlighted in hello **New sync group** blade.</span></span>

1.  <span data-ttu-id="9d613-221">På hello **tabeller** bladet Välj en databas från hello lista över medlemmar och välj sedan **Uppdatera schema**.</span><span class="sxs-lookup"><span data-stu-id="9d613-221">On hello **Tables** blade, select a database from hello list of sync group members, and then select **Refresh schema**.</span></span>

2.  <span data-ttu-id="9d613-222">Välj hello tabeller som du vill toosync hello listan över tillgängliga tabeller.</span><span class="sxs-lookup"><span data-stu-id="9d613-222">From hello list of available tables, select hello tables that you want toosync.</span></span>

    ![Välj tabeller toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  <span data-ttu-id="9d613-224">Alla kolumner i tabellen hello är markerade som standard.</span><span class="sxs-lookup"><span data-stu-id="9d613-224">By default, all columns in hello table are selected.</span></span> <span data-ttu-id="9d613-225">Om du inte vill toosync alla hello kolumner, inaktivera hello kryssrutan för hello kolumner som du inte vill toosync.</span><span class="sxs-lookup"><span data-stu-id="9d613-225">If you don't want toosync all hello columns, disable hello checkbox for hello columns that you don't want toosync.</span></span> <span data-ttu-id="9d613-226">Kontrollera att väljas tooleave hello primärnyckelkolumnen.</span><span class="sxs-lookup"><span data-stu-id="9d613-226">Be sure tooleave hello primary key column selected.</span></span>

    ![Välj fält toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  <span data-ttu-id="9d613-228">Välj slutligen **spara**.</span><span class="sxs-lookup"><span data-stu-id="9d613-228">Finally, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d613-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9d613-229">Next steps</span></span>
<span data-ttu-id="9d613-230">Grattis!</span><span class="sxs-lookup"><span data-stu-id="9d613-230">Congratulations.</span></span> <span data-ttu-id="9d613-231">Du har skapat en sync-grupp som innehåller både en instans av SQL Database och SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="9d613-231">You have created a sync group that includes both a SQL Database instance and a SQL Server database.</span></span>

<span data-ttu-id="9d613-232">För mer information om SQL Database och SQL-datasynkronisering, se:</span><span class="sxs-lookup"><span data-stu-id="9d613-232">For more info about SQL Database and SQL Data Sync, see:</span></span>

-   [<span data-ttu-id="9d613-233">Hämta hello Fullständig datasynkronisering SQL teknisk dokumentation</span><span class="sxs-lookup"><span data-stu-id="9d613-233">Download hello complete SQL Data Sync technical documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [<span data-ttu-id="9d613-234">Hämta hello SQL Data Sync REST API-dokumentation</span><span class="sxs-lookup"><span data-stu-id="9d613-234">Download hello SQL Data Sync REST API documentation</span></span>](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [<span data-ttu-id="9d613-235">Översikt över SQL-databas</span><span class="sxs-lookup"><span data-stu-id="9d613-235">SQL Database Overview</span></span>](sql-database-technical-overview.md)
-   [<span data-ttu-id="9d613-236">Livscykelhantering för databasen</span><span class="sxs-lookup"><span data-stu-id="9d613-236">Database Lifecycle Management</span></span>](https://msdn.microsoft.com/library/jj907294.aspx)
