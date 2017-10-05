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
# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Hur man återställer en enskild tabell från en Azure SQL Database-säkerhetskopia
Du kan stöta på en situation där du av misstag ändra vissa data i en SQL-databas och nu vill du återställa den enda berörda tabellen. Den här artikeln beskrivs hur du återställer en enda tabell i en databas från en SQL-databasen [automatiska säkerhetskopieringar](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Förberedelser: Byt namn på tabellen och återställa en kopia av databasen
1. Identifiera tabellen i Azure SQL-databasen som du vill ersätta med den återställda kopian. Använd Microsoft SQL Management Studio för att byta namn på tabellen. Till exempel ändra namn på tabellen som &lt;tabellnamn&gt;_old.
   
   > [!NOTE]
   > Kontrollera att det inte finns någon aktivitet som körs på den tabell som du byter namn på för att undvika blockeras. Om du får problem, kontrollera som utför den här proceduren under en underhållsperiod.
   >

2. Återställa en säkerhetskopia av databasen till en tidpunkt som du vill återställa till med hjälp av den [punkt-In_Time återställa](sql-database-recovery-using-backups.md#point-in-time-restore) steg.
   
   > [!NOTE]
   > Namnet på den återställda databasen ska vara i formatet DBName + tidsstämpel; till exempel **Adventureworks2012_2016-01-01T22-12Z**. Det här steget inte skriva över befintliga databasens namn på servern. Detta är en säkerhetsåtgärd och avsikten så att du kan verifiera den återställda databasen innan de släpper den aktuella databasen och Byt namn på den återställda databasen för produktion.
   
## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Kopiera tabellen från den återställda databasen med hjälp av Migreringsverktyget för SQL-databas

1. Hämta och installera den [Migreringsguiden för SQL-databasen](https://sqlazuremw.codeplex.com).
2. Öppna Migreringsguiden för SQL-databasen på den **Välj Process** väljer **databasen under analysera/migrera**, och klicka sedan på **nästa**.

   ![Guiden Migrera SQL-databas – Välj Process](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. I den **Anslut till Server** dialogrutan rutan, tillämpa följande inställningar:

   * Servernamn: **din SQL server**
   * Autentisering: **SQL Server-autentisering**
   * Inloggning: **din inloggning**
   * Lösenord: **ditt lösenord**
   * Databas: **Master DB (lista över alla databaser)**
   
   > [!NOTE]
   > Som standard sparar guiden inloggningsinformationen. Om du inte vill att markera **glömmer inloggningsinformation**.
   >
   
     ![Migrera för SQL-databasen guiden - Välj källa för - steg 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. I den **Välj källa** dialogrutan Välj återställda databasens namn från den **förberedelsesteg** avsnittet som käll- och klicka sedan på **nästa**.
   
    ![Migrera för SQL-databasen guiden - Välj källa för - steg 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. I den **Välj objekt** dialogrutan markerar du den **välja specifika databasobjekt** alternativet och välj sedan table(or tables) som du vill migrera till målservern.
   ![Guiden Migrera SQL-databas – Välj objekt](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)
6. På den **skript sammanfattning i Återställningsguiden** klickar du på **Ja** när du uppmanas om om du är redo att skapa ett SQL-skript. Du har också möjlighet att spara TSQL-skript för senare användning.
   ![SQL-databas migreringsguiden - skriptet guiden sammanfattning](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)
7. På den **resultatsammanfattning** klickar du på **nästa**.
   ![SQL-databas migreringsguiden - sammanfattning av testresultat](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)
8. På den **installationsprogrammet mål serveranslutning** klickar du på **Anslut till Server**, och sedan anger du informationen på följande sätt:
   
   * **Servernamnet**: Target server-instans
   * **Autentisering**: **SQL Server-autentisering**. Ange dina inloggningsuppgifter.
   * **Databasen**: **Master DB (lista över alla databaser)**. Det här alternativet visas alla databaser på målservern.
     
     ![SQL-databas migreringsguiden - installationsprogrammet Target Server-anslutning](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. Klicka på **Anslut**, Välj måldatabasen som du vill flytta en tabell till och klicka sedan på **nästa**. Detta ska slutföras tidigare genererade skriptet kördes och du bör se tabellen nyligen flyttade kopieras till måldatabasen.

## <a name="verification-step"></a>Verifieringssteg

- Fråga efter och testa den kopierade tabellen för att se till att data är intakt. Du kan släppa formuläret har bytt namn till tabellen vid bekräftelse **förberedelsesteg** avsnitt. (till exempel &lt;tabellnamn&gt;_old).

## <a name="next-steps"></a>Nästa steg
[Automatisk säkerhetskopiering i SQL-databas](sql-database-automated-backups.md)

