---
title: Migrera SQLServer-databas till Azure SQL Database | Microsoft Docs
description: "Lär dig att migrera din SQL Server-databas till Azure SQL Database."
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
ms.openlocfilehash: 375d3ea0230e7d3fd0fc02ca7e0b8a7a76c24a27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a><span data-ttu-id="22c78-103">Migrera din SQL Server-databas till Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="22c78-103">Migrate your SQL Server database to Azure SQL Database</span></span>

<span data-ttu-id="22c78-104">Flytta SQL Server-databas till Azure SQL Database är en process med tre delar - förbereder du kan sedan exportera och importera sedan databasen.</span><span class="sxs-lookup"><span data-stu-id="22c78-104">Moving your SQL Server database to Azure SQL Database is a three part process - you first prepare, then export, and then import the database.</span></span> <span data-ttu-id="22c78-105">I kursen får du lära dig att:</span><span class="sxs-lookup"><span data-stu-id="22c78-105">In this tutorial, you learn to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="22c78-106">Förbereda en databas i en SQL Server för migrering till Azure SQL Database med hjälp av den [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA)</span><span class="sxs-lookup"><span data-stu-id="22c78-106">Prepare a database in a SQL Server for migration to Azure SQL Database using the [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA)</span></span>
> * <span data-ttu-id="22c78-107">Exportera databasen till en BACPAC-fil</span><span class="sxs-lookup"><span data-stu-id="22c78-107">Export the database to a BACPAC file</span></span>
> * <span data-ttu-id="22c78-108">Importera filen BACPAC till en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="22c78-108">Import the BACPAC file into an Azure SQL Database</span></span>

<span data-ttu-id="22c78-109">Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="22c78-109">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22c78-110">Krav</span><span class="sxs-lookup"><span data-stu-id="22c78-110">Prerequisites</span></span>

<span data-ttu-id="22c78-111">Följande krav måste uppfyllas för att kunna köra den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="22c78-111">To complete this tutorial, make sure the following prerequisites are completed:</span></span>

- <span data-ttu-id="22c78-112">Installerat den senaste versionen av [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="22c78-112">Installed the newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span></span> <span data-ttu-id="22c78-113">SSMS också installeras den senaste versionen av SQLPackage, ett kommandoradsverktyg som kan användas för att automatisera en uppsättning uppgifter för utveckling av databasen.</span><span class="sxs-lookup"><span data-stu-id="22c78-113">Installing SSMS also installs the newest version of SQLPackage, a command-line utility that can be used to automate a range of database development tasks.</span></span> 
- <span data-ttu-id="22c78-114">Installerat den [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).</span><span class="sxs-lookup"><span data-stu-id="22c78-114">Installed the [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).</span></span>
- <span data-ttu-id="22c78-115">Du har identifierat och har åtkomst till en databas för att migrera.</span><span class="sxs-lookup"><span data-stu-id="22c78-115">You have identified and have access to a database to migrate.</span></span> <span data-ttu-id="22c78-116">Den här kursen använder den [SQL Server 2008R2 AdventureWorks OLTP-databasen](https://msftdbprodsamples.codeplex.com/releases/view/59211) på en instans av SQL Server 2008R2 eller senare, men du kan använda alla databaser som du väljer.</span><span class="sxs-lookup"><span data-stu-id="22c78-116">This tutorial uses the [SQL Server 2008R2 AdventureWorks OLTP database](https://msftdbprodsamples.codeplex.com/releases/view/59211) on an instance of SQL Server 2008R2 or newer, but you can use any database of your choice.</span></span> <span data-ttu-id="22c78-117">Åtgärda kompatibilitetsproblem genom att använda [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)</span><span class="sxs-lookup"><span data-stu-id="22c78-117">To fix compatibility issues, use [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)</span></span>

## <a name="prepare-for-migration"></a><span data-ttu-id="22c78-118">Förbered för migrering</span><span class="sxs-lookup"><span data-stu-id="22c78-118">Prepare for migration</span></span>

<span data-ttu-id="22c78-119">Är du redo att förbereda för migrering.</span><span class="sxs-lookup"><span data-stu-id="22c78-119">You are ready to prepare for migration.</span></span> <span data-ttu-id="22c78-120">Följ dessa steg för att använda den  **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)**  att kontrollera om databasen för migrering till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="22c78-120">Follow these steps to use the **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)** to assess the readiness of your database for migration to Azure SQL Database.</span></span>

1. <span data-ttu-id="22c78-121">Öppna den **Data Migration Assistant**.</span><span class="sxs-lookup"><span data-stu-id="22c78-121">Open the **Data Migration Assistant**.</span></span> <span data-ttu-id="22c78-122">Du kan köra DMA på valfri dator med anslutningen till SQL Server-instans som innehåller den databas som du planerar att migrera, du behöver inte installera den på den dator där SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="22c78-122">You can run DMA on any computer with connectivity to the SQL Server instance containing the database that you plan to migrate, you do not need to install it on the computer hosting the SQL Server instance.</span></span>

     ![Öppna assistenten för migrering av data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. <span data-ttu-id="22c78-124">I den vänstra menyn klickar du på **+ ny** att skapa en **Assessment** projekt.</span><span class="sxs-lookup"><span data-stu-id="22c78-124">In the left-hand menu, click **+ New** to create an **Assessment** project.</span></span> <span data-ttu-id="22c78-125">Fyll i formuläret med en **projektnamn** (alla andra värden ska vara kvar standardvärdena) och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="22c78-125">Fill in the form with a **Project name** (all other values should be left at their default values) and click **Create**.</span></span> <span data-ttu-id="22c78-126">Den **alternativ** öppnas.</span><span class="sxs-lookup"><span data-stu-id="22c78-126">The **Options** page opens.</span></span>

     ![nya data migrering assistenten projektet](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3. <span data-ttu-id="22c78-128">På den **alternativ** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="22c78-128">On the **Options** page, click **Next**.</span></span> <span data-ttu-id="22c78-129">Den **Välj källor** öppnas.</span><span class="sxs-lookup"><span data-stu-id="22c78-129">The **Select sources** page opens.</span></span>

     ![nya alternativ för migrering av data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-options.png) 

4. <span data-ttu-id="22c78-131">På den **Välj källor** anger du namnet på SQL Server-instans som innehåller den server som du planerar att migrera.</span><span class="sxs-lookup"><span data-stu-id="22c78-131">On the **Select sources** page, enter the name of SQL Server instance containing the server you plan to migrate.</span></span> <span data-ttu-id="22c78-132">Vid behov kan du ändra värdena på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="22c78-132">Change the other values on this page if necessary.</span></span> <span data-ttu-id="22c78-133">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="22c78-133">Click **Connect**.</span></span>

     ![nya migreringen Välj datakällor](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources.png)

5. <span data-ttu-id="22c78-135">I den **lägger till källor** del av den **Välj källor** markerar du kryssrutorna för databaserna som ska testas för kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="22c78-135">In the **Add sources** portion of the **Select sources** page, select the checkboxes for the databases to be tested for compatibility.</span></span> <span data-ttu-id="22c78-136">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="22c78-136">Click **Add**.</span></span>

     ![nya migreringen Välj datakällor](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources-add.png)

6. <span data-ttu-id="22c78-138">Klicka på **starta Assessment**.</span><span class="sxs-lookup"><span data-stu-id="22c78-138">Click **Start Assessment**.</span></span>

     ![Starta utvärdering av nya data migreringen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-start-assessment.png)

7. <span data-ttu-id="22c78-140">När utvärderingen har slutförts först kontrollerar om databasen är tillräckligt kompatibla för att migrera.</span><span class="sxs-lookup"><span data-stu-id="22c78-140">When the assessment completes, first look to see if the database is sufficiently compatible to migrate.</span></span> <span data-ttu-id="22c78-141">Leta efter bock i en grön cirkel.</span><span class="sxs-lookup"><span data-stu-id="22c78-141">Look for the checkmark in a green circle.</span></span>

     ![nya data migrering utvärderingsresultat kompatibel](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-compatible.png)

8. <span data-ttu-id="22c78-143">Granska resultaten.</span><span class="sxs-lookup"><span data-stu-id="22c78-143">Review the results.</span></span> <span data-ttu-id="22c78-144">Den **SQL Server-funktionsparitet** resultat som visas är standard-resultat för att granska.</span><span class="sxs-lookup"><span data-stu-id="22c78-144">The **SQL Server feature parity** results shown are the default results to review.</span></span> <span data-ttu-id="22c78-145">Särskilt granska information om funktioner som inte stöds och delvis stöds och informationen om rekommenderade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="22c78-145">Specifically review the information about unsupported and partially supported features, and the provided information about recommended actions.</span></span> 

     ![nya data migrering assessment paritet](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. <span data-ttu-id="22c78-147">Granska de **kompatibilitetsproblem** genom att klicka på alternativet i det övre vänstra hörnet.</span><span class="sxs-lookup"><span data-stu-id="22c78-147">Review the **Compatibility issues** by clicking that option in the upper left.</span></span> <span data-ttu-id="22c78-148">Särskilt granska information om migrering blockeringar, funktionsändringar och inaktuella funktioner för varje kompatibilitetsnivå.</span><span class="sxs-lookup"><span data-stu-id="22c78-148">Specifically review the information about migration blockers, behavior changes, and deprecated features for each compatibility level.</span></span> <span data-ttu-id="22c78-149">Granska ändringarna fulltextsökning för AdventureWorks2008R2-databasen eftersom SQL Server 2008 och ändringar av SERVERPROPERTY('LCID') eftersom SQL Server 2000.</span><span class="sxs-lookup"><span data-stu-id="22c78-149">For the AdventureWorks2008R2 database, review the changes to Full-Text Search since SQL Server 2008 and the changes to SERVERPROPERTY('LCID') since SQL Server 2000.</span></span> <span data-ttu-id="22c78-150">Mer information om dessa ändringar finns länkar för mer information.</span><span class="sxs-lookup"><span data-stu-id="22c78-150">For details on these changes, links for more information is provided.</span></span> <span data-ttu-id="22c78-151">Många sökalternativ och inställningar för textsökning har ändrats</span><span class="sxs-lookup"><span data-stu-id="22c78-151">Many search options and settings for Full-Text Search have changed</span></span> 

   > [!IMPORTANT] 
   > <span data-ttu-id="22c78-152">När du migrerar din databas till Azure SQL Database måste välja du att databasen på den aktuella kompatibilitetsnivån (nivå 100 för AdventureWorks2008R2 databasen) eller på en högre nivå.</span><span class="sxs-lookup"><span data-stu-id="22c78-152">After you migrate your database to Azure SQL Database, you can choose to operate the database at its current compatibility level (level 100 for the AdventureWorks2008R2 database) or at a higher level.</span></span> <span data-ttu-id="22c78-153">Mer information om effekterna och alternativ för att driva en databas på en specifik kompatibilitetsnivå finns [ändra DATABASENS kompatibilitetsnivån](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span><span class="sxs-lookup"><span data-stu-id="22c78-153">For more information on the implications and options for operating a database at a specific compatibility level, see [ALTER DATABASE Compatibility Level](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).</span></span> <span data-ttu-id="22c78-154">Se även [ALTER OMFÅNG DATABASKONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) information om ytterligare databasnivå inställningar som rör kompatibilitetsnivåer.</span><span class="sxs-lookup"><span data-stu-id="22c78-154">See also [ALTER DATABASE SCOPED CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) for information about additional database-level settings related to compatibility levels.</span></span>
   >

10. <span data-ttu-id="22c78-155">Du kan också klicka på **exportera rapporten** att spara rapporten som en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="22c78-155">Optionally, click **Export report** to save the report as a JSON file.</span></span>
11. <span data-ttu-id="22c78-156">Stäng Data Migration Assistant.</span><span class="sxs-lookup"><span data-stu-id="22c78-156">Close the Data Migration Assistant.</span></span>

## <a name="export-to-bacpac-file"></a><span data-ttu-id="22c78-157">Exportera till BACPAC fil</span><span class="sxs-lookup"><span data-stu-id="22c78-157">Export to BACPAC file</span></span> 

<span data-ttu-id="22c78-158">En BACPAC-fil är en ZIP-fil med filnamnstillägget BACPAC som innehåller metadata och data från en SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="22c78-158">A BACPAC file is a ZIP file with an extension of BACPAC containing the metadata and data from a SQL Server database.</span></span> <span data-ttu-id="22c78-159">En BACPAC-fil kan lagras i Azure blob-lagring eller i lokal lagring för arkivering eller för att migrera - såsom från SQL Server till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="22c78-159">A BACPAC file can be stored in Azure blob storage or in local storage for archiving or for migration - such as from SQL Server to Azure SQL Database.</span></span> <span data-ttu-id="22c78-160">För en export transaktionellt konsekvent, måste du kontrollera antingen att inga skrivåtgärder aktivitet sker under exporten.</span><span class="sxs-lookup"><span data-stu-id="22c78-160">For an export to be transactionally consistent, you must ensure either that no write activity is occurring during the export.</span></span>

<span data-ttu-id="22c78-161">Följ dessa steg om du vill exportera AdventureWorks2008R2 databasen till lokal lagring med hjälp av kommandoradsverktyget SQLPackage.</span><span class="sxs-lookup"><span data-stu-id="22c78-161">Follow these steps to use the SQLPackage command-line utility to export the AdventureWorks2008R2 database to local storage.</span></span>

1. <span data-ttu-id="22c78-162">Öppna en kommandotolk och ändra katalogen till en mapp där du har den **130** version av SQLPackage - som **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.</span><span class="sxs-lookup"><span data-stu-id="22c78-162">Open a Windows command prompt and change your directory to a folder in which you have the **130** version of SQLPackage - such as **C:\Program Files (x86)\Microsoft SQL Server\130\DAC\bin**.</span></span>

2. <span data-ttu-id="22c78-163">Kör följande SQLPackage-kommando i Kommandotolken att exportera den **AdventureWorks2008R2** databasen från **localhost** till **AdventureWorks2008R2.bacpac**.</span><span class="sxs-lookup"><span data-stu-id="22c78-163">Execute the following SQLPackage command at the command prompt to export the **AdventureWorks2008R2** database from **localhost** to **AdventureWorks2008R2.bacpac**.</span></span> <span data-ttu-id="22c78-164">Ändra dessa värden som är lämplig i din miljö.</span><span class="sxs-lookup"><span data-stu-id="22c78-164">Change any of these values as appropriate to your environment.</span></span>

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![sqlpackage export](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

<span data-ttu-id="22c78-166">När körningen är klar lagras den genererade BACPAC-filen i katalogen där sqlpackage körbara finns.</span><span class="sxs-lookup"><span data-stu-id="22c78-166">Once the execution is complete the generated BACPAC file is stored in the directory where the sqlpackage executable is located.</span></span> <span data-ttu-id="22c78-167">I det här exemplet C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin.</span><span class="sxs-lookup"><span data-stu-id="22c78-167">In this example C:\Program Files (x86)\Microsoft SQL Server\130\DAC\bin.</span></span> 

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="22c78-168">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="22c78-168">Log in to the Azure portal</span></span>

<span data-ttu-id="22c78-169">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="22c78-169">Log in to the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="22c78-170">Loggar in från den dator som du kör kommandoradsverktyget SQLPackage underlättar skapandet av brandväggsregeln i steg 5.</span><span class="sxs-lookup"><span data-stu-id="22c78-170">Logging on from the computer from which you are running the SQLPackage command-line utility eases the creation of the firewall rule in step 5.</span></span>

## <a name="create-a-sql-server-logical-server"></a><span data-ttu-id="22c78-171">Skapa en logisk SQLServer</span><span class="sxs-lookup"><span data-stu-id="22c78-171">Create a SQL server logical server</span></span>

<span data-ttu-id="22c78-172">En [SQL server logiska servern](sql-database-features.md) fungerar som en central administrativ plats för flera databaser.</span><span class="sxs-lookup"><span data-stu-id="22c78-172">A [SQL server logical server](sql-database-features.md) acts as a central administrative point for multiple databases.</span></span> <span data-ttu-id="22c78-173">Följ dessa steg om du vill skapa en SQL server logisk server innehåller migrerade Adventure Works OLTP SQL Server-databasen.</span><span class="sxs-lookup"><span data-stu-id="22c78-173">Follow these steps to create a SQL server logical server to contain the migrated Adventure Works OLTP SQL Server database.</span></span> 

1. <span data-ttu-id="22c78-174">Klicka på knappen **New** (Nytt) i det övre vänstra hörnet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="22c78-174">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="22c78-175">Typen **SQLServer** i fönstret på den **ny** och väljer **SQL-databas (logisk server)** i den filtrerade listan.</span><span class="sxs-lookup"><span data-stu-id="22c78-175">Type **sql server** in the search window on the **New** page, and select **SQL database (logical server)** from the filtered list.</span></span>

    ![välj logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. <span data-ttu-id="22c78-177">På den **allt** klickar du på **SQLServer (logisk server)** och klicka sedan på **skapa** på den **SQL Server (logisk server)** sidan.</span><span class="sxs-lookup"><span data-stu-id="22c78-177">On the **Everything** page, click **SQL server (logical server)** and then click **Create** on the **SQL Server (logical server)** page.</span></span>

    ![Skapa logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. <span data-ttu-id="22c78-179">Fyll i följande information i SQL-serverformuläret (logisk server) med följande information, som visas på föregående bild:</span><span class="sxs-lookup"><span data-stu-id="22c78-179">Fill out the SQL server (logical server) form with the following information, as shown on the preceding image:</span></span>     

   | <span data-ttu-id="22c78-180">Inställning</span><span class="sxs-lookup"><span data-stu-id="22c78-180">Setting</span></span>       | <span data-ttu-id="22c78-181">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="22c78-181">Suggested value</span></span> | <span data-ttu-id="22c78-182">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="22c78-182">Description</span></span> | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | <span data-ttu-id="22c78-183">**Servernamn**</span><span class="sxs-lookup"><span data-stu-id="22c78-183">**Server name**</span></span> | <span data-ttu-id="22c78-184">Ange ett globalt unikt namn</span><span class="sxs-lookup"><span data-stu-id="22c78-184">Enter any globally unique name</span></span> | <span data-ttu-id="22c78-185">Giltiga servernamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="22c78-185">For valid server names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> | 
   | <span data-ttu-id="22c78-186">**Inloggning för serveradministratör**</span><span class="sxs-lookup"><span data-stu-id="22c78-186">**Server admin login**</span></span> | <span data-ttu-id="22c78-187">Ange ett giltigt namn</span><span class="sxs-lookup"><span data-stu-id="22c78-187">Enter any valid name</span></span> | <span data-ttu-id="22c78-188">För giltiga inloggningsnamn, se [Databasidentifierare](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span><span class="sxs-lookup"><span data-stu-id="22c78-188">For valid login names, see [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).</span></span> |
   | <span data-ttu-id="22c78-189">**Lösenord**</span><span class="sxs-lookup"><span data-stu-id="22c78-189">**Password**</span></span> | <span data-ttu-id="22c78-190">Ange en giltig lösenord</span><span class="sxs-lookup"><span data-stu-id="22c78-190">Enter any valid password</span></span> | <span data-ttu-id="22c78-191">Lösenordet måste innehålla minst 8 tecken och måste innehålla tecken från tre av följande kategorier: versaler, gemener, siffror och specialtecken.</span><span class="sxs-lookup"><span data-stu-id="22c78-191">Your password must have at least 8 characters and must contain characters from three of the following categories: upper case characters, lower case characters, numbers, and non-alphanumeric characters.</span></span> |
   | <span data-ttu-id="22c78-192">**Prenumeration**</span><span class="sxs-lookup"><span data-stu-id="22c78-192">**Subscription**</span></span> | <span data-ttu-id="22c78-193">Välj en prenumeration</span><span class="sxs-lookup"><span data-stu-id="22c78-193">Select a subscription</span></span> | <span data-ttu-id="22c78-194">Mer information om dina prenumerationer finns i [Prenumerationer](https://account.windowsazure.com/Subscriptions).</span><span class="sxs-lookup"><span data-stu-id="22c78-194">For details about your subscriptions, see [Subscriptions](https://account.windowsazure.com/Subscriptions).</span></span> |
   | <span data-ttu-id="22c78-195">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="22c78-195">**Resource group**</span></span> | <span data-ttu-id="22c78-196">Välj en befintlig resursgrupp eller skapa en ny grupp som **myResourceGroup**</span><span class="sxs-lookup"><span data-stu-id="22c78-196">Choose an existing resource group or create a new group, such as **myResourceGroup**</span></span> |  <span data-ttu-id="22c78-197">Giltiga resursgruppnamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span><span class="sxs-lookup"><span data-stu-id="22c78-197">For valid resource group names, see [Naming rules and restrictions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> |
   | <span data-ttu-id="22c78-198">**Plats**</span><span class="sxs-lookup"><span data-stu-id="22c78-198">**Location**</span></span> | <span data-ttu-id="22c78-199">Ange en giltig plats för den nya servern</span><span class="sxs-lookup"><span data-stu-id="22c78-199">Enter any valid location for the new server</span></span> | <span data-ttu-id="22c78-200">För information om regioner, se [Azure-regioner](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="22c78-200">For information about regions, see [Azure Regions](https://azure.microsoft.com/regions/).</span></span> |

   ![Skapa logisk server slutförts formulär](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. <span data-ttu-id="22c78-202">Klicka på **skapa** att etablera den logiska servern.</span><span class="sxs-lookup"><span data-stu-id="22c78-202">Click **Create** to provision the logical server.</span></span> <span data-ttu-id="22c78-203">Etableringen tar några minuter.</span><span class="sxs-lookup"><span data-stu-id="22c78-203">Provisioning takes a few minutes.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="22c78-204">Kom ihåg din servernamn, server admin-inloggningsnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="22c78-204">Remember your server name, server admin login name, and password.</span></span> <span data-ttu-id="22c78-205">Du måste dessa värden senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="22c78-205">You need these values later in this tutorial.</span></span>
>

## <a name="create-a-server-level-firewall-rule"></a><span data-ttu-id="22c78-206">Skapa en brandväggsregel på servernivå</span><span class="sxs-lookup"><span data-stu-id="22c78-206">Create a server-level firewall rule</span></span>

<span data-ttu-id="22c78-207">SQL Database-tjänsten skapar en [brandvägg på servernivå-](sql-database-firewall-configure.md) som förhindrar externa program och verktyg från att ansluta till servern eller en databas på servern såvida inte en brandväggsregel skapas för att öppna brandväggen för Ange IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="22c78-207">The SQL Database service creates a [firewall at the server-level](sql-database-firewall-configure.md) that prevents external applications and tools from connecting to the server or any databases on the server unless a firewall rule is created to open the firewall for specific IP addresses.</span></span> <span data-ttu-id="22c78-208">Följ dessa steg om du vill skapa en servernivå brandväggsregel SQL-databasen för den dator som du kör kommandoradsverktyget SQLPackage IP-adress.</span><span class="sxs-lookup"><span data-stu-id="22c78-208">Follow these steps to create a SQL Database server-level firewall rule for the IP address of the computer from which you are running the SQLPackage command-line utility.</span></span> <span data-ttu-id="22c78-209">Detta gör att SQLPackage att ansluta till SQL serverDatabase logiska servern via Azure SQL Database-brandvägg.</span><span class="sxs-lookup"><span data-stu-id="22c78-209">This enables SQLPackage to connect to the SQL serverDatabase logical server through the Azure SQL Database firewall.</span></span> 

1. <span data-ttu-id="22c78-210">Klicka på **alla resurser** i den vänstra menyn och klicka på den nya servern på den **alla resurser** sidan.</span><span class="sxs-lookup"><span data-stu-id="22c78-210">Click **All resources** from the left-hand menu and click your new server on the **All resources** page.</span></span> <span data-ttu-id="22c78-211">På översiktssidan för servern öppnas och visar alternativ för ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="22c78-211">The overview page for your server opens and provides options for further configuration.</span></span>

     ![Översikt över logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. <span data-ttu-id="22c78-213">Klicka på **brandväggen** i den vänstra menyn under **inställningar** på översiktssidan.</span><span class="sxs-lookup"><span data-stu-id="22c78-213">Click **Firewall** in the left-hand menu under **Settings** on the overview page.</span></span> <span data-ttu-id="22c78-214">Sidan **Brandväggsinställningar** för SQL Database-servern öppnas.</span><span class="sxs-lookup"><span data-stu-id="22c78-214">The **Firewall settings** page for the SQL Database server opens.</span></span> 

3. <span data-ttu-id="22c78-215">Klicka på **lägga till klientens IP-Adressen** i verktygsfältet för att lägga till IP-adressen för datorn du använder och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="22c78-215">Click **Add client IP** on the toolbar to add the IP address of the computer you are currently using and then click **Save**.</span></span> <span data-ttu-id="22c78-216">En brandväggsregel på servernivå skapas för denna IP-adress.</span><span class="sxs-lookup"><span data-stu-id="22c78-216">A server-level firewall rule is created for this IP address.</span></span>

     ![ange brandväggsregel för server](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. <span data-ttu-id="22c78-218">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="22c78-218">Click **OK**.</span></span>

<span data-ttu-id="22c78-219">Nu kan du ansluta till alla databaser på servern med hjälp av SQL Server Management Studio eller ett annat verktyg som helst från den här IP-adressen med hjälp av server-administratörskonto som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="22c78-219">You can now connect to all databases on this server using SQL Server Management Studio or another tool of your choice from this IP address using the server admin account created previously.</span></span>

> [!NOTE]
> <span data-ttu-id="22c78-220">SQL Database kommunicerar via port 1433.</span><span class="sxs-lookup"><span data-stu-id="22c78-220">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="22c78-221">Om du försöker ansluta inifrån ett företagsnätverk, kan utgående trafik via port 1433 nekas av nätverkets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="22c78-221">If you are trying to connect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="22c78-222">I så fall kommer du inte att kunna ansluta till din Azure SQL Database-server om inte din IT-avdelning öppnar port 1433.</span><span class="sxs-lookup"><span data-stu-id="22c78-222">If so, you cannot connect to your Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="import-a-bacpac-file-to-azure-sql-database"></a><span data-ttu-id="22c78-223">Importera en BACPAC-fil till Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="22c78-223">Import a BACPAC file to Azure SQL Database</span></span> 

<span data-ttu-id="22c78-224">De senaste versionerna av kommandoradsverktyget SQLPackage ger stöd för att skapa en Azure SQL database vid en angiven [tjänsten tjänstnivå och prestandanivå](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="22c78-224">The newest versions of the SQLPackage command-line utility provide support for creating an Azure SQL database at a specified [service tier and performance level](sql-database-service-tiers.md).</span></span> <span data-ttu-id="22c78-225">Välj en hög nivå och prestanda servicenivå för bästa prestanda vid import och sedan skala efter importen om servicenivå tjänstnivå och prestandanivå är högre än vad du behöver omedelbart.</span><span class="sxs-lookup"><span data-stu-id="22c78-225">For best performance during import, select a high service tier and performance level and then scale down after import if the service tier and performance level is higher than you need immediately.</span></span>

<span data-ttu-id="22c78-226">Följ dessa steg Använd kommandoradsverktyget SQLPackage för att importera databasen AdventureWorks2008R2 till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="22c78-226">Follow these steps use the SQLPackage command-line utility to import the AdventureWorks2008R2 database to Azure SQL Database.</span></span> <span data-ttu-id="22c78-227">Du kan använda SQL Server Management Studio för den här uppgiften, är SQLPackage den bästa metoden för de flesta produktionsmiljöer för maximal flexibilitet och bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="22c78-227">While you can use SQL Server Management Studio for this task, SQLPackage is the preferred method for most production environments for maximum flexibility and best performance.</span></span> <span data-ttu-id="22c78-228">Se [migrera från SQL Server till Azure SQL Database med BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span><span class="sxs-lookup"><span data-stu-id="22c78-228">See [Migrating from SQL Server to Azure SQL Database using BACPAC Files](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).</span></span>

- <span data-ttu-id="22c78-229">Kör följande SQLPackage-kommando i Kommandotolken att importera den **AdventureWorks2008R2** databasen från lokal lagring till SQL server logisk server som du skapade tidigare till en ny databas, en tjänstnivå för  **Premium**, och ett Tjänstmål för **P6**.</span><span class="sxs-lookup"><span data-stu-id="22c78-229">Execute the following SQLPackage command at the command prompt to import the **AdventureWorks2008R2** database from local storage to the SQL server logical server that you previously created to a new database, a service tier of **Premium**, and a Service Objective of **P6**.</span></span> <span data-ttu-id="22c78-230">Ersätt värdena i hakparenteser med lämpliga värden för din logiska server för SQL-server och ange ett namn för den nya databasen (även ersätta hakparenteser).</span><span class="sxs-lookup"><span data-stu-id="22c78-230">Replace the values in angle brackets with appropriate values for your SQL server logical server and specify a name for the new database (also replace the angle brackets).</span></span> <span data-ttu-id="22c78-231">Du kan också välja att ändra värdena för databasversionen och tjänsten objectgive som passar din miljö.</span><span class="sxs-lookup"><span data-stu-id="22c78-231">You can also choose to change the values for database edition and service objectgive as appropriate to your environment.</span></span> <span data-ttu-id="22c78-232">I den här kursen migrerade databasen kallas **myMigratedDatabase**.</span><span class="sxs-lookup"><span data-stu-id="22c78-232">For the purpose of this tutorial, the migrated database is called **myMigratedDatabase**.</span></span>

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=<your_server_name>.database.windows.net;Initial Catalog=<your_new_database_name>;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> <span data-ttu-id="22c78-234">En logisk SQLServer lyssnar på port 1433.</span><span class="sxs-lookup"><span data-stu-id="22c78-234">A SQL server logical server listens on port 1433.</span></span> <span data-ttu-id="22c78-235">Om du försöker ansluta till en SQL server logisk server från inom en företagsbrandvägg måste den här porten öppnas i företagets brandvägg att ansluta.</span><span class="sxs-lookup"><span data-stu-id="22c78-235">If you are attempting to connect to a SQL server logical server from within a corporate firewall, this port must be open in the corporate firewall for you to successfully connect.</span></span>
>

## <a name="connect-using-sql-server-management-studio-ssms"></a><span data-ttu-id="22c78-236">Ansluta med SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="22c78-236">Connect using SQL Server Management Studio (SSMS)</span></span>

<span data-ttu-id="22c78-237">Använda SQL Server Management Studio för att upprätta en anslutning till din Azure SQL Database-server och nyligen migrerade databas som heter **myMigratedDatabase** i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="22c78-237">Use SQL Server Management Studio to establish a connection to your Azure SQL Database server and newly migrated database, called **myMigratedDatabase** in this tutorial.</span></span> <span data-ttu-id="22c78-238">Om du kör SQL Server Management Studio på en annan dator som du körde SQLPackage, skapa en brandväggsregel för den här datorn med hjälp av stegen i föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="22c78-238">If you are running SQL Server Management Studio on a different computer from which you ran SQLPackage, create a firewall rule for this computer using the steps in the previous procedure.</span></span>

1. <span data-ttu-id="22c78-239">Öppna SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="22c78-239">Open SQL Server Management Studio.</span></span>

2. <span data-ttu-id="22c78-240">I dialogrutan **Anslut till server** anger du följande information:</span><span class="sxs-lookup"><span data-stu-id="22c78-240">In the **Connect to Server** dialog box, enter the following information:</span></span>
   - <span data-ttu-id="22c78-241">**Servertyp**: Ange databasmotor</span><span class="sxs-lookup"><span data-stu-id="22c78-241">**Server type**: Specify Database engine</span></span>
   - <span data-ttu-id="22c78-242">**Servernamnet**: Ange din fullständiga servernamnet som **mynewserver20170403.database.windows.net**</span><span class="sxs-lookup"><span data-stu-id="22c78-242">**Server name**: Enter your fully qualified server name, such as **mynewserver20170403.database.windows.net**</span></span>
   - <span data-ttu-id="22c78-243">**Autentisering**: Ange SQL Server-autentisering</span><span class="sxs-lookup"><span data-stu-id="22c78-243">**Authentication**: Specify SQL Server Authentication</span></span>
   - <span data-ttu-id="22c78-244">**Logga in**: Ange serveradministratörskontot</span><span class="sxs-lookup"><span data-stu-id="22c78-244">**Login**: Enter your server admin account</span></span>
   - <span data-ttu-id="22c78-245">**Lösenord**: Ange lösenordet för serveradministratörskontot</span><span class="sxs-lookup"><span data-stu-id="22c78-245">**Password**: Enter the password for your server admin account</span></span>
 
    ![ansluta med ssms](./media/sql-database-migrate-your-sql-server-database/connect-ssms.png)

3. <span data-ttu-id="22c78-247">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="22c78-247">Click **Connect**.</span></span> <span data-ttu-id="22c78-248">Fönstret Object Explorer öppnas.</span><span class="sxs-lookup"><span data-stu-id="22c78-248">The Object Explorer window opens.</span></span> 

4. <span data-ttu-id="22c78-249">I Object Explorer, expandera **databaser** och expandera sedan **myMigratedDatabase** att visa objekten i-exempeldatabasen.</span><span class="sxs-lookup"><span data-stu-id="22c78-249">In Object Explorer, expand **Databases** and then expand **myMigratedDatabase** to view the objects in the sample database.</span></span>

## <a name="change-database-properties"></a><span data-ttu-id="22c78-250">Ändra egenskaper för databas</span><span class="sxs-lookup"><span data-stu-id="22c78-250">Change database properties</span></span>

<span data-ttu-id="22c78-251">Du kan ändra den tjänstnivå och prestandanivå kompatibilitetsnivå med hjälp av SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="22c78-251">You can change the service tier, performance level, and compatibility level using SQL Server Management Studio.</span></span> <span data-ttu-id="22c78-252">Under importen rekommenderar vi att du importerar till en högre nivå-databas för prestanda för bästa prestanda, men skala när importen är klar för att spara pengar tills du är redo att använda den importerade databasen aktivt.</span><span class="sxs-lookup"><span data-stu-id="22c78-252">During the import phase, we recommend that you import to a higher performance tier database for best performance, but that you scale down after the import completes to save money until you are ready to actively use the imported database.</span></span> <span data-ttu-id="22c78-253">Ändra kompatibilitetsnivån kan ge bättre prestanda och åtkomst till de senaste funktionerna för Azure SQL Database-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="22c78-253">Changing the compatibility level may yield better performance and access to the newest capabilities of the Azure SQL Database service.</span></span> <span data-ttu-id="22c78-254">När du migrerar en äldre databas bevaras dess databasens kompatibilitetsnivå vid den lägsta stödda nivån som är kompatibel med databasen som importeras.</span><span class="sxs-lookup"><span data-stu-id="22c78-254">When you migrate an older database, its database compatibility level is maintained at the lowest supported level that is compatible with the database being imported.</span></span> <span data-ttu-id="22c78-255">Mer information finns i [förbättrad prestanda för frågor med nivå 130 i Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).</span><span class="sxs-lookup"><span data-stu-id="22c78-255">For more information, see [Improved query performance with compatibility Level 130 in Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).</span></span>

1. <span data-ttu-id="22c78-256">Högerklicka i Object Explorer **myMigratedDatabase** och på **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="22c78-256">In Object Explorer, right-click **myMigratedDatabase** and click **New Query**.</span></span> <span data-ttu-id="22c78-257">Ett frågefönster öppnas ansluten till din databas.</span><span class="sxs-lookup"><span data-stu-id="22c78-257">A query window opens connected to your database.</span></span>

2. <span data-ttu-id="22c78-258">Kör följande kommando för att ange tjänstnivån till **Standard** och prestandanivån uppdateras till **S1**.</span><span class="sxs-lookup"><span data-stu-id="22c78-258">Execute the following command to set the service tier to **Standard** and the performance level to **S1**.</span></span>

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

3. <span data-ttu-id="22c78-260">Kör följande kommando för att ändra databasens kompatibilitetsnivån till **130**.</span><span class="sxs-lookup"><span data-stu-id="22c78-260">Execute the following command to change the database compatibility level to **130**.</span></span>

    ```
    ALTER DATABASE myMigratedDatabase  
    SET COMPATIBILITY_LEVEL = 130;
    ```

   ![Ändra kompatibilitetsnivån](./media/sql-database-migrate-your-sql-server-database/compat-level.png)

## <a name="next-steps"></a><span data-ttu-id="22c78-262">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="22c78-262">Next steps</span></span> 
<span data-ttu-id="22c78-263">I den här självstudiekursen förberedd du, exporteras och importeras din databas.</span><span class="sxs-lookup"><span data-stu-id="22c78-263">In this tutorial you prepared, exported and imported your database.</span></span> <span data-ttu-id="22c78-264">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="22c78-264">You learned to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="22c78-265">Förbereda en databas i en SQL Server för migrering till Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="22c78-265">Prepare a database in a SQL Server for migration to Azure SQL Database</span></span>
> * <span data-ttu-id="22c78-266">Exportera databasen till en BACPAC-fil</span><span class="sxs-lookup"><span data-stu-id="22c78-266">Export the database to a BACPAC file</span></span>
> * <span data-ttu-id="22c78-267">Importera filen BACPAC till en Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="22c78-267">Import the BACPAC file into an Azure SQL Database</span></span>

<span data-ttu-id="22c78-268">Gå vidare till nästa kurs att lära dig att skydda databasen.</span><span class="sxs-lookup"><span data-stu-id="22c78-268">Advance to the next tutorial to learn how to secure your database.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="22c78-269">[Skydda din Azure SQL-databas](sql-database-security-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="22c78-269">[Secure your Azure SQL Database](sql-database-security-tutorial.md).</span></span>


