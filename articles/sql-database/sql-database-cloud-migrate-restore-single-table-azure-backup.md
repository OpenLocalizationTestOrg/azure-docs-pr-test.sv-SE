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
# <a name="how-toorestore-a-single-table-from-an-azure-sql-database-backup"></a>Hur toorestore en enskild tabell från en Azure SQL Database-säkerhetskopia
Du kan stöta på en situation där du av misstag ändra vissa data i en SQL-databas och nu vill toorecover hello berörda tabell. Den här artikeln beskriver hur toorestore en enda tabell i en databas från en hello SQL Database [automatiska säkerhetskopieringar](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-hello-table-and-restore-a-copy-of-hello-database"></a>Förberedelser: Byt namn på tabellen hello och återställa en kopia av hello databas
1. Identifiera hello tabell i Azure SQL-databasen som du vill tooreplace med hello återställts kopia. Använd Microsoft SQL Management Studio toorename hello tabell. Till exempel ändra namn på hello tabell som &lt;tabellnamn&gt;_old.
   
   > [!NOTE]
   > tooavoid blockeras, se till att det inte finns någon aktivitet som körs på hello-tabell som du byter namn på. Om du får problem, kontrollera som utför den här proceduren under en underhållsperiod.
   >

2. Återställa en säkerhetskopia av din databas tooa tidpunkt som du vill toorecover toousing hello [punkt-In_Time återställa](sql-database-recovery-using-backups.md#point-in-time-restore) steg.
   
   > [!NOTE]
   > hello namnet på hello återställs databasen blir i hello DBName + tidstämpelformatet; till exempel **Adventureworks2012_2016-01-01T22-12Z**. Det här steget över inte hello befintligt databasnamn på hello-servern. Detta är en säkerhetsåtgärd, och den är avsedd tooallow tooverify hello återställa databasen innan de släpper den aktuella databasen och Byt namn på hello återställs databasen för produktion.
   
## <a name="copying-hello-table-from-hello-restored-database-by-using-hello-sql-database-migration-tool"></a>Kopiera hello tabell från hello återställs databasen med hjälp av hello SQL-databas Migreringsverktyget

1. Hämta och installera hello [Migreringsguiden för SQL-databasen](https://sqlazuremw.codeplex.com).
2. Öppna hello Migreringsguiden för SQL-databas på hello **Välj Process** väljer **databasen under analysera/migrera**, och klicka sedan på **nästa**.

   ![Guiden Migrera SQL-databas – Välj Process](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. I hello **ansluta tooServer** dialogrutan rutan gäller hello följande inställningar:

   * Servernamn: **din SQL server**
   * Autentisering: **SQL Server-autentisering**
   * Inloggning: **din inloggning**
   * Lösenord: **ditt lösenord**
   * Databas: **Master DB (lista över alla databaser)**
   
   > [!NOTE]
   > Som standard sparas hello guiden inloggningsinformationen. Om du inte vill att markera **glömmer inloggningsinformation**.
   >
   
     ![Migrera för SQL-databasen guiden - Välj källa för - steg 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. I hello **Välj källa** dialogrutan, Välj hello återställts databasnamn från hello **förberedelsesteg** avsnittet som käll- och klicka sedan på **nästa**.
   
    ![Migrera för SQL-databasen guiden - Välj källa för - steg 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. I hello **Välj objekt** dialogrutan, Välj hello **välja specifika databasobjekt** alternativet och välj sedan hello table(or tables) som du vill toomigrate toohello målservern.
   ![Guiden Migrera SQL-databas – Välj objekt](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)
6. På hello **skript sammanfattning i Återställningsguiden** klickar du på **Ja** när du uppmanas om oavsett om du är klar toogenerate ett SQL-skript. Du kan också ha hello alternativet toosave hello TSQL-skript för senare användning.
   ![SQL-databas migreringsguiden - skriptet guiden sammanfattning](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)
7. På hello **resultatsammanfattning** klickar du på **nästa**.
   ![SQL-databas migreringsguiden - sammanfattning av testresultat](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)
8. På hello **installationsprogrammet mål serveranslutning** klickar du på **ansluta tooServer**, och ange sedan hello information på följande sätt:
   
   * **Servernamnet**: Target server-instans
   * **Autentisering**: **SQL Server-autentisering**. Ange dina inloggningsuppgifter.
   * **Databasen**: **Master DB (lista över alla databaser)**. Det här alternativet visas alla hello-databaser på hello-målservern.
     
     ![SQL-databas migreringsguiden - installationsprogrammet Target Server-anslutning](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. Klicka på **Anslut**väljer hello måldatabasen som du vill toomove hello tabellen och klickar sedan på **nästa**. Detta ska slutföras kör hello tidigare genererade skriptet och du bör se hello nyligen flyttas tabellen kopierade toohello måldatabasen.

## <a name="verification-step"></a>Verifieringssteg

- Fråga och testa hello nyligen kopierade tabellen toomake att hello data är intakt. Vid bekräftelse, du kan släppa hello byta namn på tabellen formuläret **förberedelsesteg** avsnitt. (till exempel &lt;tabellnamn&gt;_old).

## <a name="next-steps"></a>Nästa steg
[Automatisk säkerhetskopiering i SQL-databas](sql-database-automated-backups.md)

