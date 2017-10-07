---
title: aaaMigrate SQL Server-databas tooAzure SQL-databas | Microsoft Docs
description: "Lär dig toomigrate din SQL Server-databasen tooAzure SQL-databas."
services: sql-database
documentationcenter: 
author: janeng
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,load & move data
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/27/2017
ms.author: janeng
ms.openlocfilehash: d10ad1d26576194f1dd6858bae5c3e7c1ec4fb91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-sql-server-database-tooazure-sql-database"></a><span data-ttu-id="5f1b8-103">Migrera din SQL Server-databasen tooAzure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="5f1b8-103">Migrate your SQL Server database tooAzure SQL Database</span></span>

<span data-ttu-id="5f1b8-104">Flytta SQL Server database tooAzure SQL-databas är en process med tre delar - du först förbereda och sedan exportera och importera hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-104">Moving your SQL Server database tooAzure SQL Database is a three part process - you first prepare, then export, and then import hello database.</span></span> <span data-ttu-id="5f1b8-105">I kursen får du lära dig att:</span><span class="sxs-lookup"><span data-stu-id="5f1b8-105">In this tutorial, you learn to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5f1b8-106">Förbered en databas i en SQL Server för migrering tooAzure SQL-databas med hjälp av hello [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA)</span><span class="sxs-lookup"><span data-stu-id="5f1b8-106">Prepare a database in a SQL Server for migration tooAzure SQL Database using hello [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA)</span></span>
> * <span data-ttu-id="5f1b8-107">Exportera hello databasfilen tooa BACPAC</span><span class="sxs-lookup"><span data-stu-id="5f1b8-107">Export hello database tooa BACPAC file</span></span>
> * <span data-ttu-id="5f1b8-108">Importera hello BACPAC fil till en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5f1b8-108">Import hello BACPAC file into an Azure SQL Database</span></span>

<span data-ttu-id="5f1b8-109">Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-109">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f1b8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5f1b8-110">Prerequisites</span></span>

<span data-ttu-id="5f1b8-111">den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:</span><span class="sxs-lookup"><span data-stu-id="5f1b8-111">toocomplete this tutorial, make sure hello following prerequisites are completed:</span></span>

- <span data-ttu-id="5f1b8-112">Installerade hello senaste versionen av [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-112">Installed hello newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span></span> <span data-ttu-id="5f1b8-113">Installera SSMS installeras också hello senaste versionen av SQLPackage, ett kommandoradsverktyg som kan använda tooautomate ett antal uppgifter för utveckling av databasen.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-113">Installing SSMS also installs hello newest version of SQLPackage, a command-line utility that can be used tooautomate a range of database development tasks.</span></span> 
- <span data-ttu-id="5f1b8-114">Installerade hello [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-114">Installed hello [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).</span></span>
- <span data-ttu-id="5f1b8-115">Du har identifierat och har åtkomst tooa databasen toomigrate.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-115">You have identified and have access tooa database toomigrate.</span></span> <span data-ttu-id="5f1b8-116">Den här kursen använder hello [SQL Server 2008R2 AdventureWorks OLTP-databasen](https://msftdbprodsamples.codeplex.com/releases/view/59211) på en instans av SQL Server 2008R2 eller senare, men du kan använda alla databaser som du väljer.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-116">This tutorial uses hello [SQL Server 2008R2 AdventureWorks OLTP database](https://msftdbprodsamples.codeplex.com/releases/view/59211) on an instance of SQL Server 2008R2 or newer, but you can use any database of your choice.</span></span> <span data-ttu-id="5f1b8-117">Använd toofix kompatibilitetsproblem [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)</span><span class="sxs-lookup"><span data-stu-id="5f1b8-117">toofix compatibility issues, use [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)</span></span>

## <a name="prepare-for-migration"></a><span data-ttu-id="5f1b8-118">Förbered för migrering</span><span class="sxs-lookup"><span data-stu-id="5f1b8-118">Prepare for migration</span></span>

<span data-ttu-id="5f1b8-119">Är du redo tooprepare för migrering.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-119">You are ready tooprepare for migration.</span></span> <span data-ttu-id="5f1b8-120">Följ dessa steg toouse hello  **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)**  tooassess hello beredskap för din databas för migrering tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-120">Follow these steps toouse hello **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)** tooassess hello readiness of your database for migration tooAzure SQL Database.</span></span>

1. <span data-ttu-id="5f1b8-121">Öppna hello **Data Migration Assistant**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-121">Open hello **Data Migration Assistant**.</span></span> <span data-ttu-id="5f1b8-122">Du kan köra DMA på valfri dator med anslutning toohello SQL Server-instans som innehåller hello-databas som du planerar toomigrate, behöver du inte tooinstall på hello datorn där hello SQLServer-instansen.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-122">You can run DMA on any computer with connectivity toohello SQL Server instance containing hello database that you plan toomigrate, you do not need tooinstall it on hello computer hosting hello SQL Server instance.</span></span>

     ![Öppna assistenten för migrering av data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. <span data-ttu-id="5f1b8-124">Hello vänstra menyn klickar du på **+ ny** toocreate en **Assessment** projekt.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-124">In hello left-hand menu, click **+ New** toocreate an **Assessment** project.</span></span> <span data-ttu-id="5f1b8-125">Fyll i hello formulär med en **projektnamn** (alla andra värden ska vara kvar standardvärdena) och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-125">Fill in hello form with a **Project name** (all other values should be left at their default values) and click **Create**.</span></span> <span data-ttu-id="5f1b8-126">Hej **alternativ** öppnas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-126">hello **Options** page opens.</span></span>

     ![nya data migrering assistenten projektet](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3. <span data-ttu-id="5f1b8-128">På hello **alternativ** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-128">On hello **Options** page, click **Next**.</span></span> <span data-ttu-id="5f1b8-129">Hej **Välj källor** öppnas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-129">hello **Select sources** page opens.</span></span>

     ![nya alternativ för migrering av data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-options.png) 

4. <span data-ttu-id="5f1b8-131">På hello **Välj källor** anger hello namnet på SQL Server-instans som innehåller hello-server som du planerar toomigrate.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-131">On hello **Select sources** page, enter hello name of SQL Server instance containing hello server you plan toomigrate.</span></span> <span data-ttu-id="5f1b8-132">Ändra hello andra värden på den här sidan om det behövs.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-132">Change hello other values on this page if necessary.</span></span> <span data-ttu-id="5f1b8-133">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-133">Click **Connect**.</span></span>

     ![nya migreringen Välj datakällor](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources.png)

5. <span data-ttu-id="5f1b8-135">I hello **lägger till källor** del av hello **Välj källor** väljer hello kryssrutorna för hello databaser toobe testas för kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-135">In hello **Add sources** portion of hello **Select sources** page, select hello checkboxes for hello databases toobe tested for compatibility.</span></span> <span data-ttu-id="5f1b8-136">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-136">Click **Add**.</span></span>

     ![nya migreringen Välj datakällor](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources-add.png)

6. <span data-ttu-id="5f1b8-138">Klicka på **starta Assessment**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-138">Click **Start Assessment**.</span></span>

     ![Starta utvärdering av nya data migreringen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-start-assessment.png)

7. <span data-ttu-id="5f1b8-140">När hello utvärderingen har slutförts först kontrollerar toosee om hello-databasen är tillräckligt kompatibel toomigrate.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-140">When hello assessment completes, first look toosee if hello database is sufficiently compatible toomigrate.</span></span> <span data-ttu-id="5f1b8-141">Leta efter hello bock i en grön cirkel.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-141">Look for hello checkmark in a green circle.</span></span>

     ![nya data migrering utvärderingsresultat kompatibel](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-compatible.png)

8. <span data-ttu-id="5f1b8-143">Granska resultatet från hello.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-143">Review hello results.</span></span> <span data-ttu-id="5f1b8-144">Hej **SQL Server-funktionsparitet** resultat som visas är hello standard resultat tooreview.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-144">hello **SQL Server feature parity** results shown are hello default results tooreview.</span></span> <span data-ttu-id="5f1b8-145">Granska specifikt hello information om funktioner som inte stöds och delvis stöds och hello tillhandahålls information om rekommenderade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-145">Specifically review hello information about unsupported and partially supported features, and hello provided information about recommended actions.</span></span> 

     ![nya data migrering assessment paritet](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. <span data-ttu-id="5f1b8-147">Granska hello **kompatibilitetsproblem** genom att klicka på alternativet i hello övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-147">Review hello **Compatibility issues** by clicking that option in hello upper left.</span></span> <span data-ttu-id="5f1b8-148">Mer specifikt granska hello information om migrering blockeringar, funktionsändringar, och inaktuella funktioner för varje kompatibilitetsnivå.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-148">Specifically review hello information about migration blockers, behavior changes, and deprecated features for each compatibility level.</span></span> <span data-ttu-id="5f1b8-149">Granska hello ändringar för hello AdventureWorks2008R2 databasen tooFull textsökning eftersom SQL Server 2008 och hello ändringar tooSERVERPROPERTY('LCID') sedan SQL Server 2000.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-149">For hello AdventureWorks2008R2 database, review hello changes tooFull-Text Search since SQL Server 2008 and hello changes tooSERVERPROPERTY('LCID') since SQL Server 2000.</span></span> <span data-ttu-id="5f1b8-150">Mer information om dessa ändringar finns länkar för mer information.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-150">For details on these changes, links for more information is provided.</span></span> <span data-ttu-id="5f1b8-151">Många sökalternativ och inställningar för textsökning har ändrats</span><span class="sxs-lookup"><span data-stu-id="5f1b8-151">Many search options and settings for Full-Text Search have changed</span></span> 

   > [!IMPORTANT] 
   > <span data-ttu-id="5f1b8-152">När du migrerar din databas tooAzure SQL-databas, väljer du toooperate hello databas på dess aktuella kompatibilitetsnivån (nivå 100 för hello AdventureWorks2008R2 databasen) eller på en högre nivå.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-152">After you migrate your database tooAzure SQL Database, you can choose toooperate hello database at its current compatibility level (level 100 for hello AdventureWorks2008R2 database) or at a higher level.</span></span> <span data-ttu-id="5f1b8-153">Läs mer om hello effekter och alternativ för att driva en databas på en specifik kompatibilitetsnivå [ändra DATABASENS kompatibilitetsnivån](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-153">For more information on hello implications and options for operating a database at a specific compatibility level, see [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span></span> <span data-ttu-id="5f1b8-154">Se även [ALTER OMFÅNG DATABASKONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) information om ytterligare databasnivå inställningar relaterade toocompatibility nivåer.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-154">See also [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) for information about additional database-level settings related toocompatibility levels.</span></span>
   >

10. <span data-ttu-id="5f1b8-155">Du kan också klicka på **exportera rapporten** toosave hello rapporten som en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-155">Optionally, click **Export report** toosave hello report as a JSON file.</span></span>
11. <span data-ttu-id="5f1b8-156">Stäng hello Data Migration Assistant.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-156">Close hello Data Migration Assistant.</span></span>

## <a name="export-toobacpac-file"></a><span data-ttu-id="5f1b8-157">Exportera tooBACPAC fil</span><span class="sxs-lookup"><span data-stu-id="5f1b8-157">Export tooBACPAC file</span></span> 

<span data-ttu-id="5f1b8-158">En BACPAC-fil är en ZIP-fil med filnamnstillägget BACPAC som innehåller hello metadata och data från en SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-158">A BACPAC file is a ZIP file with an extension of BACPAC containing hello metadata and data from a SQL Server database.</span></span> <span data-ttu-id="5f1b8-159">En BACPAC-fil kan lagras i Azure blob storage eller lokal lagring för arkivering eller för att migrera - exempel från SQL Server-tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-159">A BACPAC file can be stored in Azure blob storage or in local storage for archiving or for migration - such as from SQL Server tooAzure SQL Database.</span></span> <span data-ttu-id="5f1b8-160">För en export-toobe transaktionellt konsekvent, måste du kontrollera antingen att inga skrivåtgärder aktivitet sker under hello exporten.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-160">For an export toobe transactionally consistent, you must ensure either that no write activity is occurring during hello export.</span></span>

<span data-ttu-id="5f1b8-161">Följ dessa steg toouse hello SQLPackage kommandoradsverktyget tooexport hello AdventureWorks2008R2 toolocal databaslagring.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-161">Follow these steps toouse hello SQLPackage command-line utility tooexport hello AdventureWorks2008R2 database toolocal storage.</span></span>

1. <span data-ttu-id="5f1b8-162">Öppna en kommandotolk och ändra katalogen tooa mapp där du har hello **130** version av SQLPackage - som **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-162">Open a Windows command prompt and change your directory tooa folder in which you have hello **130** version of SQLPackage - such as **C:\Program Files (x86)\Microsoft SQL Server\130\DAC\bin**.</span></span>

2. <span data-ttu-id="5f1b8-163">Kör följande SQLPackage kommando på hello kommandotolk tooexport hello hello **AdventureWorks2008R2** databasen från **localhost** för**AdventureWorks2008R2.bacpac**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-163">Execute hello following SQLPackage command at hello command prompt tooexport hello **AdventureWorks2008R2** database from **localhost** too**AdventureWorks2008R2.bacpac**.</span></span> <span data-ttu-id="5f1b8-164">Ändra dessa värden som lämpliga tooyour miljö.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-164">Change any of these values as appropriate tooyour environment.</span></span>

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![sqlpackage export](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

<span data-ttu-id="5f1b8-166">När hello körningen har slutförts lagras hello genereras BACPAC i hello katalogen där hello sqlpackage körbara finns.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-166">Once hello execution is complete hello generated BACPAC file is stored in hello directory where hello sqlpackage executable is located.</span></span> <span data-ttu-id="5f1b8-167">I det här exemplet C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-167">In this example C:\Program Files (x86)\Microsoft SQL Server\130\DAC\bin.</span></span> 

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="5f1b8-168">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5f1b8-168">Log in toohello Azure portal</span></span>

<span data-ttu-id="5f1b8-169">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-169">Log in toohello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="5f1b8-170">Inloggning från hello dator från vilken du kör hello SQLPackage kommandoradsverktyget underlättar hello skapandet av hello brandväggsregel i steg 5.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-170">Logging on from hello computer from which you are running hello SQLPackage command-line utility eases hello creation of hello firewall rule in step 5.</span></span>

## <a name="create-a-sql-server-logical-server"></a><span data-ttu-id="5f1b8-171">Skapa en logisk SQLServer</span><span class="sxs-lookup"><span data-stu-id="5f1b8-171">Create a SQL server logical server</span></span>

<span data-ttu-id="5f1b8-172">En [SQL server logiska servern](sql-database-features.md) fungerar som en central administrativ plats för flera databaser.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-172">A [SQL server logical server](sql-database-features.md) acts as a central administrative point for multiple databases.</span></span> <span data-ttu-id="5f1b8-173">Följ dessa steg toocreate en SQL server logisk server toocontain hello migreras Adventure Works OLTP SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-173">Follow these steps toocreate a SQL server logical server toocontain hello migrated Adventure Works OLTP SQL Server database.</span></span> 

1. <span data-ttu-id="5f1b8-174">Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-174">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="5f1b8-175">Typen **SQLServer** hello Sök i i fönstret hello **ny** och väljer **SQL-databas (logisk server)** från hello filtrerad lista.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-175">Type **sql server** in hello search window on hello **New** page, and select **SQL database (logical server)** from hello filtered list.</span></span>

    ![välj logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. <span data-ttu-id="5f1b8-177">På hello **allt** klickar du på **SQLServer (logisk server)** och klicka sedan på **skapa** på hello **SQL Server (logisk server)** sidan .</span><span class="sxs-lookup"><span data-stu-id="5f1b8-177">On hello **Everything** page, click **SQL server (logical server)** and then click **Create** on hello **SQL Server (logical server)** page.</span></span>

    ![Skapa logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. <span data-ttu-id="5f1b8-179">Fyll i formuläret om hello SQL server (logisk server) med hello följande information som visas i föregående bild hello:</span><span class="sxs-lookup"><span data-stu-id="5f1b8-179">Fill out hello SQL server (logical server) form with hello following information, as shown on hello preceding image:</span></span>     

   | <span data-ttu-id="5f1b8-180">Inställning</span><span class="sxs-lookup"><span data-stu-id="5f1b8-180">Setting</span></span>       | <span data-ttu-id="5f1b8-181">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="5f1b8-181">Suggested value</span></span> | <span data-ttu-id="5f1b8-182">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5f1b8-182">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="5f1b8-183">**Servernamn**</span><span class="sxs-lookup"><span data-stu-id="5f1b8-183">**Server name**</span></span> | <span data-ttu-id="5f1b8-184">Ange ett globalt unikt namn</span><span class="sxs-lookup"><span data-stu-id="5f1b8-184">Enter any globally unique name</span></span> | <span data-ttu-id="5f1b8-185">Giltiga servernamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-185">For valid server names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> | 
   | <span data-ttu-id="5f1b8-186">**Inloggning för serveradministratör**</span><span class="sxs-lookup"><span data-stu-id="5f1b8-186">**Server admin login**</span></span> | <span data-ttu-id="5f1b8-187">Ange ett giltigt namn</span><span class="sxs-lookup"><span data-stu-id="5f1b8-187">Enter any valid name</span></span> | <span data-ttu-id="5f1b8-188">För giltiga inloggningsnamn, se [Databasidentifierare](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-188">For valid login names, see [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span></span> |
   | <span data-ttu-id="5f1b8-189">**Lösenord**</span><span class="sxs-lookup"><span data-stu-id="5f1b8-189">**Password**</span></span> | <span data-ttu-id="5f1b8-190">Ange en giltig lösenord</span><span class="sxs-lookup"><span data-stu-id="5f1b8-190">Enter any valid password</span></span> | <span data-ttu-id="5f1b8-191">Lösenordet måste innehålla minst 8 tecken och måste innehålla tecken från tre av hello följande kategorier: versaler, gemener, siffror och specialtecken.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-191">Your password must have at least 8 characters and must contain characters from three of hello following categories: upper case characters, lower case characters, numbers, and non-alphanumeric characters.</span></span> |
   | <span data-ttu-id="5f1b8-192">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="5f1b8-192">**Subscription**</span></span> | <span data-ttu-id="5f1b8-193">Välj en prenumeration</span><span class="sxs-lookup"><span data-stu-id="5f1b8-193">Select a subscription</span></span> | <span data-ttu-id="5f1b8-194">Mer information om dina prenumerationer finns i [Prenumerationer](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-194">For details about your subscriptions, see [Subscriptions](https://account.windowsazure.com/Subscriptions).</span></span> |
   | <span data-ttu-id="5f1b8-195">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="5f1b8-195">**Resource group**</span></span> | <span data-ttu-id="5f1b8-196">Välj en befintlig resursgrupp eller skapa en ny grupp som **myResourceGroup**</span><span class="sxs-lookup"><span data-stu-id="5f1b8-196">Choose an existing resource group or create a new group, such as **myResourceGroup**</span></span> |  <span data-ttu-id="5f1b8-197">Giltiga resursgruppnamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-197">For valid resource group names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> |
   | <span data-ttu-id="5f1b8-198">**Plats**</span><span class="sxs-lookup"><span data-stu-id="5f1b8-198">**Location**</span></span> | <span data-ttu-id="5f1b8-199">Ange en giltig plats för nya hello-server</span><span class="sxs-lookup"><span data-stu-id="5f1b8-199">Enter any valid location for hello new server</span></span> | <span data-ttu-id="5f1b8-200">För information om regioner, se [Azure-regioner](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-200">For information about regions, see [Azure Regions](https://azure.microsoft.com/regions/).</span></span> |

   ![Skapa logisk server slutförts formulär](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. <span data-ttu-id="5f1b8-202">Klicka på **skapa** tooprovision hello logisk server.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-202">Click **Create** tooprovision hello logical server.</span></span> <span data-ttu-id="5f1b8-203">Etableringen tar några minuter.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-203">Provisioning takes a few minutes.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="5f1b8-204">Kom ihåg din servernamn, server admin-inloggningsnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-204">Remember your server name, server admin login name, and password.</span></span> <span data-ttu-id="5f1b8-205">Du måste dessa värden senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-205">You need these values later in this tutorial.</span></span>
>

## <a name="create-a-server-level-firewall-rule"></a><span data-ttu-id="5f1b8-206">Skapa en brandväggsregel på servernivå</span><span class="sxs-lookup"><span data-stu-id="5f1b8-206">Create a server-level firewall rule</span></span>

<span data-ttu-id="5f1b8-207">hello SQL Database-tjänsten skapar en [brandvägg på servernivå för hello](sql-database-firewall-configure.md) som förhindrar att externa program och verktyg ansluter toohello server eller en databas på servern hello såvida inte en brandväggsregel skapas tooopen hello Brandvägg för specifika IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-207">hello SQL Database service creates a [firewall at hello server-level](sql-database-firewall-configure.md) that prevents external applications and tools from connecting toohello server or any databases on hello server unless a firewall rule is created tooopen hello firewall for specific IP addresses.</span></span> <span data-ttu-id="5f1b8-208">Följ dessa steg toocreate en servernivå brandväggsregel SQL-databas för hello IP-adress hello dator från vilken du kör hello SQLPackage kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-208">Follow these steps toocreate a SQL Database server-level firewall rule for hello IP address of hello computer from which you are running hello SQLPackage command-line utility.</span></span> <span data-ttu-id="5f1b8-209">Detta gör att SQLPackage tooconnect toohello serverDatabase logisk SQL-server via hello Azure SQL Database-brandvägg.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-209">This enables SQLPackage tooconnect toohello SQL serverDatabase logical server through hello Azure SQL Database firewall.</span></span> 

1. <span data-ttu-id="5f1b8-210">Klicka på **alla resurser** hello vänstra menyn och klicka på den nya servern på hello **alla resurser** sidan.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-210">Click **All resources** from hello left-hand menu and click your new server on hello **All resources** page.</span></span> <span data-ttu-id="5f1b8-211">hello översiktssidan för servern öppnas och visar alternativ för ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-211">hello overview page for your server opens and provides options for further configuration.</span></span>

     ![Översikt över logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. <span data-ttu-id="5f1b8-213">Klicka på **brandväggen** hello vänstra menyn under **inställningar** på översiktssidan för hello.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-213">Click **Firewall** in hello left-hand menu under **Settings** on hello overview page.</span></span> <span data-ttu-id="5f1b8-214">Hej **brandväggsinställningar** öppnas sidan för hello SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-214">hello **Firewall settings** page for hello SQL Database server opens.</span></span> 

3. <span data-ttu-id="5f1b8-215">Klicka på **lägga till klientens IP-Adressen** på hello verktygsfältet tooadd hello IP-adressen hello datorn du använder och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-215">Click **Add client IP** on hello toolbar tooadd hello IP address of hello computer you are currently using and then click **Save**.</span></span> <span data-ttu-id="5f1b8-216">En brandväggsregel på servernivå skapas för denna IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-216">A server-level firewall rule is created for this IP address.</span></span>

     ![ange brandväggsregel för server](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. <span data-ttu-id="5f1b8-218">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-218">Click **OK**.</span></span>

<span data-ttu-id="5f1b8-219">Nu kan du ansluta tooall databaser på servern med hjälp av SQL Server Management Studio eller ett annat verktyg som helst från den här IP-adressen med hello server administratörskonto som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-219">You can now connect tooall databases on this server using SQL Server Management Studio or another tool of your choice from this IP address using hello server admin account created previously.</span></span>

> [!NOTE]
> <span data-ttu-id="5f1b8-220">SQL Database kommunicerar via port 1433.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-220">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="5f1b8-221">Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 1433 inte av ditt nätverks brandvägg.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-221">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="5f1b8-222">I så fall, kan du inte ansluta tooyour Azure SQL Database-server om din IT-avdelning öppnar port 1433.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-222">If so, you cannot connect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="import-a-bacpac-file-tooazure-sql-database"></a><span data-ttu-id="5f1b8-223">Importera en BACPAC filen tooAzure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="5f1b8-223">Import a BACPAC file tooAzure SQL Database</span></span> 

<span data-ttu-id="5f1b8-224">hello nyaste versionerna av hello SQLPackage kommandoradsverktyget ger stöd för att skapa en Azure SQL database vid en angiven [tjänsten tjänstnivå och prestandanivå](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-224">hello newest versions of hello SQLPackage command-line utility provide support for creating an Azure SQL database at a specified [service tier and performance level](sql-database-service-tiers.md).</span></span> <span data-ttu-id="5f1b8-225">Välj en hög nivå och prestanda servicenivå för bästa prestanda vid import och sedan skala efter importen om hello tjänstnivå och prestandanivå servicenivå är högre än vad du behöver omedelbart.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-225">For best performance during import, select a high service tier and performance level and then scale down after import if hello service tier and performance level is higher than you need immediately.</span></span>

<span data-ttu-id="5f1b8-226">Följ dessa steg Använd hello SQLPackage kommandoradsverktyget tooimport hello AdventureWorks2008R2 databasen tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-226">Follow these steps use hello SQLPackage command-line utility tooimport hello AdventureWorks2008R2 database tooAzure SQL Database.</span></span> <span data-ttu-id="5f1b8-227">Du kan använda SQL Server Management Studio för den här uppgiften, är SQLPackage hello önskad metod för de flesta produktionsmiljöer för maximal flexibilitet och bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-227">While you can use SQL Server Management Studio for this task, SQLPackage is hello preferred method for most production environments for maximum flexibility and best performance.</span></span> <span data-ttu-id="5f1b8-228">Se [migrera från SQL Server-tooAzure SQL-databas med hjälp av BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-228">See [Migrating from SQL Server tooAzure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

- <span data-ttu-id="5f1b8-229">Kör följande SQLPackage kommando på hello kommandotolk tooimport hello hello **AdventureWorks2008R2** från logiska server för lokal lagring toohello SQL server-databas som du tidigare skapade tooa ny databas, en tjänst nivån av **Premium**, och ett Tjänstmål för **P6**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-229">Execute hello following SQLPackage command at hello command prompt tooimport hello **AdventureWorks2008R2** database from local storage toohello SQL server logical server that you previously created tooa new database, a service tier of **Premium**, and a Service Objective of **P6**.</span></span> <span data-ttu-id="5f1b8-230">Ersätt hello värden i hakparenteser med lämpliga värden för din logiska server för SQL-server och ange ett namn för hello ny databas (även ersätta hello hakparenteser).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-230">Replace hello values in angle brackets with appropriate values for your SQL server logical server and specify a name for hello new database (also replace hello angle brackets).</span></span> <span data-ttu-id="5f1b8-231">Du kan också välja toochange hello värden för databasversionen och tjänsten objectgive som lämpliga tooyour miljö.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-231">You can also choose toochange hello values for database edition and service objectgive as appropriate tooyour environment.</span></span> <span data-ttu-id="5f1b8-232">För hello syftet med den här självstudiekursen, hello migrerade databasen kallas **myMigratedDatabase**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-232">For hello purpose of this tutorial, hello migrated database is called **myMigratedDatabase**.</span></span>

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=<your_server_name>.database.windows.net;Initial Catalog=<your_new_database_name>;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> <span data-ttu-id="5f1b8-234">En logisk SQLServer lyssnar på port 1433.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-234">A SQL server logical server listens on port 1433.</span></span> <span data-ttu-id="5f1b8-235">Om du försöker tooconnect tooa SQL server logiska server från en företagsbrandvägg måste den här porten öppnas i hello företagets brandvägg för du toosuccessfully ansluta.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-235">If you are attempting tooconnect tooa SQL server logical server from within a corporate firewall, this port must be open in hello corporate firewall for you toosuccessfully connect.</span></span>
>

## <a name="connect-using-sql-server-management-studio-ssms"></a><span data-ttu-id="5f1b8-236">Ansluta med SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="5f1b8-236">Connect using SQL Server Management Studio (SSMS)</span></span>

<span data-ttu-id="5f1b8-237">Använda SQL Server Management Studio tooestablish en anslutning tooyour Azure SQL Database-server och migrerade databas som heter **myMigratedDatabase** i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-237">Use SQL Server Management Studio tooestablish a connection tooyour Azure SQL Database server and newly migrated database, called **myMigratedDatabase** in this tutorial.</span></span> <span data-ttu-id="5f1b8-238">Om du kör SQL Server Management Studio på en annan dator som du körde SQLPackage, skapa en brandväggsregel för den här datorn med hjälp av hello steg hello föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-238">If you are running SQL Server Management Studio on a different computer from which you ran SQLPackage, create a firewall rule for this computer using hello steps in hello previous procedure.</span></span>

1. <span data-ttu-id="5f1b8-239">Öppna SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-239">Open SQL Server Management Studio.</span></span>

2. <span data-ttu-id="5f1b8-240">I hello **ansluta tooServer** dialogrutan Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="5f1b8-240">In hello **Connect tooServer** dialog box, enter hello following information:</span></span>
   - <span data-ttu-id="5f1b8-241">**Servertyp**: Ange databasmotor</span><span class="sxs-lookup"><span data-stu-id="5f1b8-241">**Server type**: Specify Database engine</span></span>
   - <span data-ttu-id="5f1b8-242">**Servernamnet**: Ange din fullständiga servernamnet som **mynewserver20170403.database.windows.net**</span><span class="sxs-lookup"><span data-stu-id="5f1b8-242">**Server name**: Enter your fully qualified server name, such as **mynewserver20170403.database.windows.net**</span></span>
   - <span data-ttu-id="5f1b8-243">**Autentisering**: Ange SQL Server-autentisering</span><span class="sxs-lookup"><span data-stu-id="5f1b8-243">**Authentication**: Specify SQL Server Authentication</span></span>
   - <span data-ttu-id="5f1b8-244">**Logga in**: Ange serveradministratörskontot</span><span class="sxs-lookup"><span data-stu-id="5f1b8-244">**Login**: Enter your server admin account</span></span>
   - <span data-ttu-id="5f1b8-245">**Lösenordet**: Ange hello lösenord för administratörskontot server</span><span class="sxs-lookup"><span data-stu-id="5f1b8-245">**Password**: Enter hello password for your server admin account</span></span>
 
    ![ansluta med ssms](./media/sql-database-migrate-your-sql-server-database/connect-ssms.png)

3. <span data-ttu-id="5f1b8-247">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-247">Click **Connect**.</span></span> <span data-ttu-id="5f1b8-248">hello öppnas Object Explorer.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-248">hello Object Explorer window opens.</span></span> 

4. <span data-ttu-id="5f1b8-249">I Object Explorer, expandera **databaser** och expandera sedan **myMigratedDatabase** tooview hello objekt i hello-exempeldatabasen.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-249">In Object Explorer, expand **Databases** and then expand **myMigratedDatabase** tooview hello objects in hello sample database.</span></span>

## <a name="change-database-properties"></a><span data-ttu-id="5f1b8-250">Ändra egenskaper för databas</span><span class="sxs-lookup"><span data-stu-id="5f1b8-250">Change database properties</span></span>

<span data-ttu-id="5f1b8-251">Du kan ändra hello-tjänstnivå och prestandanivå kompatibilitetsnivå med hjälp av SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-251">You can change hello service tier, performance level, and compatibility level using SQL Server Management Studio.</span></span> <span data-ttu-id="5f1b8-252">Under importen hello rekommenderar vi att du databasimport tooa högre prestanda nivån för bästa prestanda, men skala när hello importen är klar toosave pengar tills du är klar tooactively använda hello importerade databas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-252">During hello import phase, we recommend that you import tooa higher performance tier database for best performance, but that you scale down after hello import completes toosave money until you are ready tooactively use hello imported database.</span></span> <span data-ttu-id="5f1b8-253">Ändrar kompatibilitetsnivå hello kan ge bättre prestanda och toohello senaste åtkomstfunktioner för hello Azure SQL Database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-253">Changing hello compatibility level may yield better performance and access toohello newest capabilities of hello Azure SQL Database service.</span></span> <span data-ttu-id="5f1b8-254">När du migrerar en äldre databas bevaras dess Databaskompatibilitetsnivån på hello lägsta stöds nivå som är kompatibel med hello databasen som importeras.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-254">When you migrate an older database, its database compatibility level is maintained at hello lowest supported level that is compatible with hello database being imported.</span></span> <span data-ttu-id="5f1b8-255">Mer information finns i [förbättrad prestanda för frågor med nivå 130 i Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-255">For more information, see [Improved query performance with compatibility Level 130 in Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).</span></span>

1. <span data-ttu-id="5f1b8-256">Högerklicka i Object Explorer **myMigratedDatabase** och på **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-256">In Object Explorer, right-click **myMigratedDatabase** and click **New Query**.</span></span> <span data-ttu-id="5f1b8-257">Ett frågefönster öppnas anslutna tooyour databas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-257">A query window opens connected tooyour database.</span></span>

2. <span data-ttu-id="5f1b8-258">Köra hello efter kommandot tooset hello tjänstnivån för**Standard** och hello prestandanivå för**S1**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-258">Execute hello following command tooset hello service tier too**Standard** and hello performance level too**S1**.</span></span>

    ```
    ALTER DATABASE myMigratedDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

    ![Ändra tjänstnivå](./media/sql-database-migrate-your-sql-server-database/service-tier.png)

3. <span data-ttu-id="5f1b8-260">Köra hello efter kommandot toochange hello databasens kompatibilitetsnivå för**130**.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-260">Execute hello following command toochange hello database compatibility level too**130**.</span></span>

    ```
    ALTER DATABASE myMigratedDatabase  
    SET COMPATIBILITY_LEVEL = 130;
    ```

   ![Ändra kompatibilitetsnivån](./media/sql-database-migrate-your-sql-server-database/compat-level.png)

## <a name="next-steps"></a><span data-ttu-id="5f1b8-262">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f1b8-262">Next steps</span></span> 
<span data-ttu-id="5f1b8-263">I den här självstudiekursen förberedd du, exporteras och importeras din databas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-263">In this tutorial you prepared, exported and imported your database.</span></span> <span data-ttu-id="5f1b8-264">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="5f1b8-264">You learned to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5f1b8-265">Förbered en databas i en SQL Server för migrering tooAzure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="5f1b8-265">Prepare a database in a SQL Server for migration tooAzure SQL Database</span></span>
> * <span data-ttu-id="5f1b8-266">Exportera hello databasfilen tooa BACPAC</span><span class="sxs-lookup"><span data-stu-id="5f1b8-266">Export hello database tooa BACPAC file</span></span>
> * <span data-ttu-id="5f1b8-267">Importera hello BACPAC fil till en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5f1b8-267">Import hello BACPAC file into an Azure SQL Database</span></span>

<span data-ttu-id="5f1b8-268">I förväg toohello nästa självstudiekurs toolearn hur toosecure din databas.</span><span class="sxs-lookup"><span data-stu-id="5f1b8-268">Advance toohello next tutorial toolearn how toosecure your database.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="5f1b8-269">[Skydda din Azure SQL-databas](sql-database-security-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="5f1b8-269">[Secure your Azure SQL Database](sql-database-security-tutorial.md).</span></span>


