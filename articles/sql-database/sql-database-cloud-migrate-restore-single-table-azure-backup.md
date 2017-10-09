---
title: "aaaRestore en enskild tabell från Azure SQL Database-säkerhetskopia | Microsoft Docs"
description: "Lär dig hur toorestore en enskild tabell från Azure SQL Database-säkerhetskopia."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: 340b41bd-9df8-47fb-adfc-03216de38a5e
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: 696d2ac87a70bccdf063bfecb8255723404aa5bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorestore-a-single-table-from-an-azure-sql-database-backup"></a><span data-ttu-id="05390-103">Hur toorestore en enskild tabell från en Azure SQL Database-säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="05390-103">How toorestore a single table from an Azure SQL Database backup</span></span>
<span data-ttu-id="05390-104">Du kan stöta på en situation där du av misstag ändra vissa data i en SQL-databas och nu vill toorecover hello berörda tabell.</span><span class="sxs-lookup"><span data-stu-id="05390-104">You may encounter a situation in which you accidentally modified some data in a SQL database and now you want toorecover hello single affected table.</span></span> <span data-ttu-id="05390-105">Den här artikeln beskriver hur toorestore en enda tabell i en databas från en hello SQL Database [automatiska säkerhetskopieringar](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="05390-105">This article describes how toorestore a single table in a database from one of hello SQL Database [automatic backups](sql-database-automated-backups.md).</span></span>

## <a name="preparation-steps-rename-hello-table-and-restore-a-copy-of-hello-database"></a><span data-ttu-id="05390-106">Förberedelser: Byt namn på tabellen hello och återställa en kopia av hello databas</span><span class="sxs-lookup"><span data-stu-id="05390-106">Preparation steps: Rename hello table and restore a copy of hello database</span></span>
1. <span data-ttu-id="05390-107">Identifiera hello tabell i Azure SQL-databasen som du vill tooreplace med hello återställts kopia.</span><span class="sxs-lookup"><span data-stu-id="05390-107">Identify hello table in your Azure SQL database that you want tooreplace with hello restored copy.</span></span> <span data-ttu-id="05390-108">Använd Microsoft SQL Management Studio toorename hello tabell.</span><span class="sxs-lookup"><span data-stu-id="05390-108">Use Microsoft SQL Management Studio toorename hello table.</span></span> <span data-ttu-id="05390-109">Till exempel ändra namn på hello tabell som &lt;tabellnamn&gt;_old.</span><span class="sxs-lookup"><span data-stu-id="05390-109">For example, rename hello table as &lt;table name&gt;_old.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="05390-110">tooavoid blockeras, se till att det inte finns någon aktivitet som körs på hello-tabell som du byter namn på.</span><span class="sxs-lookup"><span data-stu-id="05390-110">tooavoid being blocked, make sure that there's no activity running on hello table that you are renaming.</span></span> <span data-ttu-id="05390-111">Om du får problem, kontrollera som utför den här proceduren under en underhållsperiod.</span><span class="sxs-lookup"><span data-stu-id="05390-111">If you encounter issues, make sure that perform this procedure during a maintenance window.</span></span>
   >

2. <span data-ttu-id="05390-112">Återställa en säkerhetskopia av din databas tooa tidpunkt som du vill toorecover toousing hello [punkt-In_Time återställa](sql-database-recovery-using-backups.md#point-in-time-restore) steg.</span><span class="sxs-lookup"><span data-stu-id="05390-112">Restore a backup of your database tooa point in time that you want toorecover toousing hello [Point-In_Time Restore](sql-database-recovery-using-backups.md#point-in-time-restore) steps.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="05390-113">hello namnet på hello återställs databasen blir i hello DBName + tidstämpelformatet; till exempel **Adventureworks2012_2016-01-01T22-12Z**.</span><span class="sxs-lookup"><span data-stu-id="05390-113">hello name of hello restored database will be in hello DBName+TimeStamp format; for example, **Adventureworks2012_2016-01-01T22-12Z**.</span></span> <span data-ttu-id="05390-114">Det här steget över inte hello befintligt databasnamn på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="05390-114">This step doesn't overwrite hello existing database name on hello server.</span></span> <span data-ttu-id="05390-115">Detta är en säkerhetsåtgärd, och den är avsedd tooallow tooverify hello återställa databasen innan de släpper den aktuella databasen och Byt namn på hello återställs databasen för produktion.</span><span class="sxs-lookup"><span data-stu-id="05390-115">This is a safety measure, and it's intended tooallow you tooverify hello restored database before they drop their current database and rename hello restored database for production use.</span></span>
   
## <a name="copying-hello-table-from-hello-restored-database-by-using-hello-sql-database-migration-tool"></a><span data-ttu-id="05390-116">Kopiera hello tabell från hello återställs databasen med hjälp av hello SQL-databas Migreringsverktyget</span><span class="sxs-lookup"><span data-stu-id="05390-116">Copying hello table from hello restored database by using hello SQL Database Migration tool</span></span>

1. <span data-ttu-id="05390-117">Hämta och installera hello [Migreringsguiden för SQL-databasen](https://sqlazuremw.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="05390-117">Download and install hello [SQL Database Migration Wizard](https://sqlazuremw.codeplex.com).</span></span>
2. <span data-ttu-id="05390-118">Öppna hello Migreringsguiden för SQL-databas på hello **Välj Process** väljer **databasen under analysera/migrera**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="05390-118">Open hello SQL Database Migration Wizard, on hello **Select Process** page, select **Database under Analyze/Migrate**, and then click **Next**.</span></span>

   ![Guiden Migrera SQL-databas – Välj Process](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. <span data-ttu-id="05390-120">I hello **ansluta tooServer** dialogrutan rutan gäller hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="05390-120">In hello **Connect tooServer** dialog box, apply hello following settings:</span></span>

   * <span data-ttu-id="05390-121">Servernamn: **din SQL server**</span><span class="sxs-lookup"><span data-stu-id="05390-121">Server name: **Your SQL server**</span></span>
   * <span data-ttu-id="05390-122">Autentisering: **SQL Server-autentisering**</span><span class="sxs-lookup"><span data-stu-id="05390-122">Authentication: **SQL Server Authentication**</span></span>
   * <span data-ttu-id="05390-123">Inloggning: **din inloggning**</span><span class="sxs-lookup"><span data-stu-id="05390-123">Login: **Your login**</span></span>
   * <span data-ttu-id="05390-124">Lösenord: **ditt lösenord**</span><span class="sxs-lookup"><span data-stu-id="05390-124">Password: **Your password**</span></span>
   * <span data-ttu-id="05390-125">Databas: **Master DB (lista över alla databaser)**</span><span class="sxs-lookup"><span data-stu-id="05390-125">Database: **Master DB (List all databases)**</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="05390-126">Som standard sparas hello guiden inloggningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="05390-126">By default hello wizard saves your login information.</span></span> <span data-ttu-id="05390-127">Om du inte vill att markera **glömmer inloggningsinformation**.</span><span class="sxs-lookup"><span data-stu-id="05390-127">If you don't want it to, select **Forget Login Information**.</span></span>
   >
   
     ![Migrera för SQL-databasen guiden - Välj källa för - steg 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. <span data-ttu-id="05390-129">I hello **Välj källa** dialogrutan, Välj hello återställts databasnamn från hello **förberedelsesteg** avsnittet som käll- och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="05390-129">In hello **Select Source** dialog box, select hello restored database name from hello **Preparation steps** section as your source, and then click **Next**.</span></span>
   
    ![Migrera för SQL-databasen guiden - Välj källa för - steg 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. <span data-ttu-id="05390-131">I hello **Välj objekt** dialogrutan, Välj hello **välja specifika databasobjekt** alternativet och välj sedan hello table(or tables) som du vill toomigrate toohello målservern.</span><span class="sxs-lookup"><span data-stu-id="05390-131">In hello **Choose Objects** dialog box, select hello **Select specific database objects** option, and then select hello table(or tables) that you want toomigrate toohello target server.</span></span>
   <span data-ttu-id="05390-132">![Guiden Migrera SQL-databas – Välj objekt](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span><span class="sxs-lookup"><span data-stu-id="05390-132">![SQL Database Migration wizard - Choose Objects](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span></span>
6. <span data-ttu-id="05390-133">På hello **skript sammanfattning i Återställningsguiden** klickar du på **Ja** när du uppmanas om oavsett om du är klar toogenerate ett SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="05390-133">On hello **Script Wizard Summary** page, click **Yes** when you’re prompted about whether you’re ready toogenerate a SQL script.</span></span> <span data-ttu-id="05390-134">Du kan också ha hello alternativet toosave hello TSQL-skript för senare användning.</span><span class="sxs-lookup"><span data-stu-id="05390-134">You also have hello option toosave hello TSQL Script for later use.</span></span>
   <span data-ttu-id="05390-135">![SQL-databas migreringsguiden - skriptet guiden sammanfattning](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span><span class="sxs-lookup"><span data-stu-id="05390-135">![SQL Database Migration wizard - Script Wizard Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span></span>
7. <span data-ttu-id="05390-136">På hello **resultatsammanfattning** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="05390-136">On hello **Results Summary** page, click **Next**.</span></span>
   <span data-ttu-id="05390-137">![SQL-databas migreringsguiden - sammanfattning av testresultat](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span><span class="sxs-lookup"><span data-stu-id="05390-137">![SQL Database Migration wizard - Results Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span></span>
8. <span data-ttu-id="05390-138">På hello **installationsprogrammet mål serveranslutning** klickar du på **ansluta tooServer**, och ange sedan hello information på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="05390-138">On hello **Setup Target Server Connection** page, click **Connect tooServer**, and then enter hello details as follows:</span></span>
   
   * <span data-ttu-id="05390-139">**Servernamnet**: Target server-instans</span><span class="sxs-lookup"><span data-stu-id="05390-139">**Server Name**: Target server instance</span></span>
   * <span data-ttu-id="05390-140">**Autentisering**: **SQL Server-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="05390-140">**Authentication**: **SQL Server authentication**.</span></span> <span data-ttu-id="05390-141">Ange dina inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="05390-141">Enter your login credentials.</span></span>
   * <span data-ttu-id="05390-142">**Databasen**: **Master DB (lista över alla databaser)**.</span><span class="sxs-lookup"><span data-stu-id="05390-142">**Database**: **Master DB (List all databases)**.</span></span> <span data-ttu-id="05390-143">Det här alternativet visas alla hello-databaser på hello-målservern.</span><span class="sxs-lookup"><span data-stu-id="05390-143">This option lists all hello databases on hello target server.</span></span>
     
     ![SQL-databas migreringsguiden - installationsprogrammet Target Server-anslutning](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. <span data-ttu-id="05390-145">Klicka på **Anslut**väljer hello måldatabasen som du vill toomove hello tabellen och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="05390-145">Click **Connect**, select hello target database that you want toomove hello table to, and then click **Next**.</span></span> <span data-ttu-id="05390-146">Detta ska slutföras kör hello tidigare genererade skriptet och du bör se hello nyligen flyttas tabellen kopierade toohello måldatabasen.</span><span class="sxs-lookup"><span data-stu-id="05390-146">This should finish running hello previously generated script, and you should see hello newly moved table copied toohello target database.</span></span>

## <a name="verification-step"></a><span data-ttu-id="05390-147">Verifieringssteg</span><span class="sxs-lookup"><span data-stu-id="05390-147">Verification step</span></span>

- <span data-ttu-id="05390-148">Fråga och testa hello nyligen kopierade tabellen toomake att hello data är intakt.</span><span class="sxs-lookup"><span data-stu-id="05390-148">Query and test hello newly copied table toomake sure that hello data is intact.</span></span> <span data-ttu-id="05390-149">Vid bekräftelse, du kan släppa hello byta namn på tabellen formuläret **förberedelsesteg** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="05390-149">Upon confirmation, you can drop hello renamed table form **Preparation steps** section.</span></span> <span data-ttu-id="05390-150">(till exempel &lt;tabellnamn&gt;_old).</span><span class="sxs-lookup"><span data-stu-id="05390-150">(for example, &lt;table name&gt;_old).</span></span>

## <a name="next-steps"></a><span data-ttu-id="05390-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="05390-151">Next steps</span></span>
[<span data-ttu-id="05390-152">Automatisk säkerhetskopiering i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="05390-152">SQL Database automatic backups</span></span>](sql-database-automated-backups.md)

