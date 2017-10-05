---
title: "Återställa en enskild tabell från Azure SQL Database-säkerhetskopia | Microsoft Docs"
description: "Lär dig mer om att återställa en enskild tabell från Azure SQL Database-säkerhetskopia."
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
ms.openlocfilehash: 8c750c503d10ea63b9665958b96db2dfea3d9a3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a><span data-ttu-id="81605-103">Hur man återställer en enskild tabell från en Azure SQL Database-säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="81605-103">How to restore a single table from an Azure SQL Database backup</span></span>
<span data-ttu-id="81605-104">Du kan stöta på en situation där du av misstag ändra vissa data i en SQL-databas och nu vill du återställa den enda berörda tabellen.</span><span class="sxs-lookup"><span data-stu-id="81605-104">You may encounter a situation in which you accidentally modified some data in a SQL database and now you want to recover the single affected table.</span></span> <span data-ttu-id="81605-105">Den här artikeln beskrivs hur du återställer en enda tabell i en databas från en SQL-databasen [automatiska säkerhetskopieringar](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="81605-105">This article describes how to restore a single table in a database from one of the SQL Database [automatic backups](sql-database-automated-backups.md).</span></span>

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a><span data-ttu-id="81605-106">Förberedelser: Byt namn på tabellen och återställa en kopia av databasen</span><span class="sxs-lookup"><span data-stu-id="81605-106">Preparation steps: Rename the table and restore a copy of the database</span></span>
1. <span data-ttu-id="81605-107">Identifiera tabellen i Azure SQL-databasen som du vill ersätta med den återställda kopian.</span><span class="sxs-lookup"><span data-stu-id="81605-107">Identify the table in your Azure SQL database that you want to replace with the restored copy.</span></span> <span data-ttu-id="81605-108">Använd Microsoft SQL Management Studio för att byta namn på tabellen.</span><span class="sxs-lookup"><span data-stu-id="81605-108">Use Microsoft SQL Management Studio to rename the table.</span></span> <span data-ttu-id="81605-109">Till exempel ändra namn på tabellen som &lt;tabellnamn&gt;_old.</span><span class="sxs-lookup"><span data-stu-id="81605-109">For example, rename the table as &lt;table name&gt;_old.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="81605-110">Kontrollera att det inte finns någon aktivitet som körs på den tabell som du byter namn på för att undvika blockeras.</span><span class="sxs-lookup"><span data-stu-id="81605-110">To avoid being blocked, make sure that there's no activity running on the table that you are renaming.</span></span> <span data-ttu-id="81605-111">Om du får problem, kontrollera som utför den här proceduren under en underhållsperiod.</span><span class="sxs-lookup"><span data-stu-id="81605-111">If you encounter issues, make sure that perform this procedure during a maintenance window.</span></span>
   >

2. <span data-ttu-id="81605-112">Återställa en säkerhetskopia av databasen till en tidpunkt som du vill återställa till med hjälp av den [punkt-In_Time återställa](sql-database-recovery-using-backups.md#point-in-time-restore) steg.</span><span class="sxs-lookup"><span data-stu-id="81605-112">Restore a backup of your database to a point in time that you want to recover to using the [Point-In_Time Restore](sql-database-recovery-using-backups.md#point-in-time-restore) steps.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="81605-113">Namnet på den återställda databasen ska vara i formatet DBName + tidsstämpel; till exempel **Adventureworks2012_2016-01-01T22-12Z**.</span><span class="sxs-lookup"><span data-stu-id="81605-113">The name of the restored database will be in the DBName+TimeStamp format; for example, **Adventureworks2012_2016-01-01T22-12Z**.</span></span> <span data-ttu-id="81605-114">Det här steget inte skriva över befintliga databasens namn på servern.</span><span class="sxs-lookup"><span data-stu-id="81605-114">This step doesn't overwrite the existing database name on the server.</span></span> <span data-ttu-id="81605-115">Detta är en säkerhetsåtgärd och avsikten så att du kan verifiera den återställda databasen innan de släpper den aktuella databasen och Byt namn på den återställda databasen för produktion.</span><span class="sxs-lookup"><span data-stu-id="81605-115">This is a safety measure, and it's intended to allow you to verify the restored database before they drop their current database and rename the restored database for production use.</span></span>
   
## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a><span data-ttu-id="81605-116">Kopiera tabellen från den återställda databasen med hjälp av Migreringsverktyget för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="81605-116">Copying the table from the restored database by using the SQL Database Migration tool</span></span>

1. <span data-ttu-id="81605-117">Hämta och installera den [Migreringsguiden för SQL-databasen](https://sqlazuremw.codeplex.com).</span><span class="sxs-lookup"><span data-stu-id="81605-117">Download and install the [SQL Database Migration Wizard](https://sqlazuremw.codeplex.com).</span></span>
2. <span data-ttu-id="81605-118">Öppna Migreringsguiden för SQL-databasen på den **Välj Process** väljer **databasen under analysera/migrera**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="81605-118">Open the SQL Database Migration Wizard, on the **Select Process** page, select **Database under Analyze/Migrate**, and then click **Next**.</span></span>

   ![Guiden Migrera SQL-databas – Välj Process](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. <span data-ttu-id="81605-120">I den **Anslut till Server** dialogrutan rutan, tillämpa följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="81605-120">In the **Connect to Server** dialog box, apply the following settings:</span></span>

   * <span data-ttu-id="81605-121">Servernamn: **din SQL server**</span><span class="sxs-lookup"><span data-stu-id="81605-121">Server name: **Your SQL server**</span></span>
   * <span data-ttu-id="81605-122">Autentisering: **SQL Server-autentisering**</span><span class="sxs-lookup"><span data-stu-id="81605-122">Authentication: **SQL Server Authentication**</span></span>
   * <span data-ttu-id="81605-123">Inloggning: **din inloggning**</span><span class="sxs-lookup"><span data-stu-id="81605-123">Login: **Your login**</span></span>
   * <span data-ttu-id="81605-124">Lösenord: **ditt lösenord**</span><span class="sxs-lookup"><span data-stu-id="81605-124">Password: **Your password**</span></span>
   * <span data-ttu-id="81605-125">Databas: **Master DB (lista över alla databaser)**</span><span class="sxs-lookup"><span data-stu-id="81605-125">Database: **Master DB (List all databases)**</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="81605-126">Som standard sparar guiden inloggningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="81605-126">By default the wizard saves your login information.</span></span> <span data-ttu-id="81605-127">Om du inte vill att markera **glömmer inloggningsinformation**.</span><span class="sxs-lookup"><span data-stu-id="81605-127">If you don't want it to, select **Forget Login Information**.</span></span>
   >
   
     ![Migrera för SQL-databasen guiden - Välj källa för - steg 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. <span data-ttu-id="81605-129">I den **Välj källa** dialogrutan Välj återställda databasens namn från den **förberedelsesteg** avsnittet som käll- och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="81605-129">In the **Select Source** dialog box, select the restored database name from the **Preparation steps** section as your source, and then click **Next**.</span></span>
   
    ![Migrera för SQL-databasen guiden - Välj källa för - steg 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. <span data-ttu-id="81605-131">I den **Välj objekt** dialogrutan markerar du den **välja specifika databasobjekt** alternativet och välj sedan table(or tables) som du vill migrera till målservern.</span><span class="sxs-lookup"><span data-stu-id="81605-131">In the **Choose Objects** dialog box, select the **Select specific database objects** option, and then select the table(or tables) that you want to migrate to the target server.</span></span>
   <span data-ttu-id="81605-132">![Guiden Migrera SQL-databas – Välj objekt](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span><span class="sxs-lookup"><span data-stu-id="81605-132">![SQL Database Migration wizard - Choose Objects](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)</span></span>
6. <span data-ttu-id="81605-133">På den **skript sammanfattning i Återställningsguiden** klickar du på **Ja** när du uppmanas om om du är redo att skapa ett SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="81605-133">On the **Script Wizard Summary** page, click **Yes** when you’re prompted about whether you’re ready to generate a SQL script.</span></span> <span data-ttu-id="81605-134">Du har också möjlighet att spara TSQL-skript för senare användning.</span><span class="sxs-lookup"><span data-stu-id="81605-134">You also have the option to save the TSQL Script for later use.</span></span>
   <span data-ttu-id="81605-135">![SQL-databas migreringsguiden - skriptet guiden sammanfattning](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span><span class="sxs-lookup"><span data-stu-id="81605-135">![SQL Database Migration wizard - Script Wizard Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)</span></span>
7. <span data-ttu-id="81605-136">På den **resultatsammanfattning** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="81605-136">On the **Results Summary** page, click **Next**.</span></span>
   <span data-ttu-id="81605-137">![SQL-databas migreringsguiden - sammanfattning av testresultat](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span><span class="sxs-lookup"><span data-stu-id="81605-137">![SQL Database Migration wizard - Results Summary](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)</span></span>
8. <span data-ttu-id="81605-138">På den **installationsprogrammet mål serveranslutning** klickar du på **Anslut till Server**, och sedan anger du informationen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="81605-138">On the **Setup Target Server Connection** page, click **Connect to Server**, and then enter the details as follows:</span></span>
   
   * <span data-ttu-id="81605-139">**Servernamnet**: Target server-instans</span><span class="sxs-lookup"><span data-stu-id="81605-139">**Server Name**: Target server instance</span></span>
   * <span data-ttu-id="81605-140">**Autentisering**: **SQL Server-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="81605-140">**Authentication**: **SQL Server authentication**.</span></span> <span data-ttu-id="81605-141">Ange dina inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="81605-141">Enter your login credentials.</span></span>
   * <span data-ttu-id="81605-142">**Databasen**: **Master DB (lista över alla databaser)**.</span><span class="sxs-lookup"><span data-stu-id="81605-142">**Database**: **Master DB (List all databases)**.</span></span> <span data-ttu-id="81605-143">Det här alternativet visas alla databaser på målservern.</span><span class="sxs-lookup"><span data-stu-id="81605-143">This option lists all the databases on the target server.</span></span>
     
     ![SQL-databas migreringsguiden - installationsprogrammet Target Server-anslutning](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. <span data-ttu-id="81605-145">Klicka på **Anslut**, Välj måldatabasen som du vill flytta en tabell till och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="81605-145">Click **Connect**, select the target database that you want to move the table to, and then click **Next**.</span></span> <span data-ttu-id="81605-146">Detta ska slutföras tidigare genererade skriptet kördes och du bör se tabellen nyligen flyttade kopieras till måldatabasen.</span><span class="sxs-lookup"><span data-stu-id="81605-146">This should finish running the previously generated script, and you should see the newly moved table copied to the target database.</span></span>

## <a name="verification-step"></a><span data-ttu-id="81605-147">Verifieringssteg</span><span class="sxs-lookup"><span data-stu-id="81605-147">Verification step</span></span>

- <span data-ttu-id="81605-148">Fråga efter och testa den kopierade tabellen för att se till att data är intakt.</span><span class="sxs-lookup"><span data-stu-id="81605-148">Query and test the newly copied table to make sure that the data is intact.</span></span> <span data-ttu-id="81605-149">Du kan släppa formuläret har bytt namn till tabellen vid bekräftelse **förberedelsesteg** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="81605-149">Upon confirmation, you can drop the renamed table form **Preparation steps** section.</span></span> <span data-ttu-id="81605-150">(till exempel &lt;tabellnamn&gt;_old).</span><span class="sxs-lookup"><span data-stu-id="81605-150">(for example, &lt;table name&gt;_old).</span></span>

## <a name="next-steps"></a><span data-ttu-id="81605-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81605-151">Next steps</span></span>
[<span data-ttu-id="81605-152">Automatisk säkerhetskopiering i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="81605-152">SQL Database automatic backups</span></span>](sql-database-automated-backups.md)

