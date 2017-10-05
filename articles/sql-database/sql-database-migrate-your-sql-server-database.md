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
# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>Migrera din SQL Server-databas till Azure SQL Database

Flytta SQL Server-databas till Azure SQL Database är en process med tre delar - förbereder du kan sedan exportera och importera sedan databasen. I kursen får du lära dig att:

> [!div class="checklist"]
> * Förbereda en databas i en SQL Server för migrering till Azure SQL Database med hjälp av den [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA)
> * Exportera databasen till en BACPAC-fil
> * Importera filen BACPAC till en Azure SQL Database

Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Krav

Följande krav måste uppfyllas för att kunna köra den här självstudiekursen:

- Installerat den senaste versionen av [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). SSMS också installeras den senaste versionen av SQLPackage, ett kommandoradsverktyg som kan användas för att automatisera en uppsättning uppgifter för utveckling av databasen. 
- Installerat den [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
- Du har identifierat och har åtkomst till en databas för att migrera. Den här kursen använder den [SQL Server 2008R2 AdventureWorks OLTP-databasen](https://msftdbprodsamples.codeplex.com/releases/view/59211) på en instans av SQL Server 2008R2 eller senare, men du kan använda alla databaser som du väljer. Åtgärda kompatibilitetsproblem genom att använda [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)

## <a name="prepare-for-migration"></a>Förbered för migrering

Är du redo att förbereda för migrering. Följ dessa steg för att använda den  **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)**  att kontrollera om databasen för migrering till Azure SQL Database.

1. Öppna den **Data Migration Assistant**. Du kan köra DMA på valfri dator med anslutningen till SQL Server-instans som innehåller den databas som du planerar att migrera, du behöver inte installera den på den dator där SQL Server-instansen.

     ![Öppna assistenten för migrering av data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. I den vänstra menyn klickar du på **+ ny** att skapa en **Assessment** projekt. Fyll i formuläret med en **projektnamn** (alla andra värden ska vara kvar standardvärdena) och klicka på **skapa**. Den **alternativ** öppnas.

     ![nya data migrering assistenten projektet](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3. På den **alternativ** klickar du på **nästa**. Den **Välj källor** öppnas.

     ![nya alternativ för migrering av data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-options.png) 

4. På den **Välj källor** anger du namnet på SQL Server-instans som innehåller den server som du planerar att migrera. Vid behov kan du ändra värdena på den här sidan. Klicka på **Anslut**.

     ![nya migreringen Välj datakällor](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources.png)

5. I den **lägger till källor** del av den **Välj källor** markerar du kryssrutorna för databaserna som ska testas för kompatibilitet. Klicka på **Lägg till**.

     ![nya migreringen Välj datakällor](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources-add.png)

6. Klicka på **starta Assessment**.

     ![Starta utvärdering av nya data migreringen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-start-assessment.png)

7. När utvärderingen har slutförts först kontrollerar om databasen är tillräckligt kompatibla för att migrera. Leta efter bock i en grön cirkel.

     ![nya data migrering utvärderingsresultat kompatibel](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-compatible.png)

8. Granska resultaten. Den **SQL Server-funktionsparitet** resultat som visas är standard-resultat för att granska. Särskilt granska information om funktioner som inte stöds och delvis stöds och informationen om rekommenderade åtgärder. 

     ![nya data migrering assessment paritet](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. Granska de **kompatibilitetsproblem** genom att klicka på alternativet i det övre vänstra hörnet. Särskilt granska information om migrering blockeringar, funktionsändringar och inaktuella funktioner för varje kompatibilitetsnivå. Granska ändringarna fulltextsökning för AdventureWorks2008R2-databasen eftersom SQL Server 2008 och ändringar av SERVERPROPERTY('LCID') eftersom SQL Server 2000. Mer information om dessa ändringar finns länkar för mer information. Många sökalternativ och inställningar för textsökning har ändrats 

   > [!IMPORTANT] 
   > När du migrerar din databas till Azure SQL Database måste välja du att databasen på den aktuella kompatibilitetsnivån (nivå 100 för AdventureWorks2008R2 databasen) eller på en högre nivå. Mer information om effekterna och alternativ för att driva en databas på en specifik kompatibilitetsnivå finns [ändra DATABASENS kompatibilitetsnivån](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Se även [ALTER OMFÅNG DATABASKONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) information om ytterligare databasnivå inställningar som rör kompatibilitetsnivåer.
   >

10. Du kan också klicka på **exportera rapporten** att spara rapporten som en JSON-fil.
11. Stäng Data Migration Assistant.

## <a name="export-to-bacpac-file"></a>Exportera till BACPAC fil 

En BACPAC-fil är en ZIP-fil med filnamnstillägget BACPAC som innehåller metadata och data från en SQL Server-databas. En BACPAC-fil kan lagras i Azure blob-lagring eller i lokal lagring för arkivering eller för att migrera - såsom från SQL Server till Azure SQL Database. För en export transaktionellt konsekvent, måste du kontrollera antingen att inga skrivåtgärder aktivitet sker under exporten.

Följ dessa steg om du vill exportera AdventureWorks2008R2 databasen till lokal lagring med hjälp av kommandoradsverktyget SQLPackage.

1. Öppna en kommandotolk och ändra katalogen till en mapp där du har den **130** version av SQLPackage - som **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.

2. Kör följande SQLPackage-kommando i Kommandotolken att exportera den **AdventureWorks2008R2** databasen från **localhost** till **AdventureWorks2008R2.bacpac**. Ändra dessa värden som är lämplig i din miljö.

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![sqlpackage export](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

När körningen är klar lagras den genererade BACPAC-filen i katalogen där sqlpackage körbara finns. I det här exemplet C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin. 

## <a name="log-in-to-the-azure-portal"></a>Logga in på Azure Portal

Logga in på [Azure-portalen](https://portal.azure.com/). Loggar in från den dator som du kör kommandoradsverktyget SQLPackage underlättar skapandet av brandväggsregeln i steg 5.

## <a name="create-a-sql-server-logical-server"></a>Skapa en logisk SQLServer

En [SQL server logiska servern](sql-database-features.md) fungerar som en central administrativ plats för flera databaser. Följ dessa steg om du vill skapa en SQL server logisk server innehåller migrerade Adventure Works OLTP SQL Server-databasen. 

1. Klicka på knappen **New** (Nytt) i det övre vänstra hörnet i Azure Portal.

2. Typen **SQLServer** i fönstret på den **ny** och väljer **SQL-databas (logisk server)** i den filtrerade listan.

    ![välj logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. På den **allt** klickar du på **SQLServer (logisk server)** och klicka sedan på **skapa** på den **SQL Server (logisk server)** sidan.

    ![Skapa logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. Fyll i följande information i SQL-serverformuläret (logisk server) med följande information, som visas på föregående bild:     

   | Inställning       | Föreslaget värde | Beskrivning | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servernamn** | Ange ett globalt unikt namn | Giltiga servernamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Inloggning för serveradministratör** | Ange ett giltigt namn | För giltiga inloggningsnamn, se [Databasidentifierare](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). |
   | **Lösenord** | Ange en giltig lösenord | Lösenordet måste innehålla minst 8 tecken och måste innehålla tecken från tre av följande kategorier: versaler, gemener, siffror och specialtecken. |
   | **Prenumeration** | Välj en prenumeration | Mer information om dina prenumerationer finns i [Prenumerationer](https://account.windowsazure.com/Subscriptions). |
   | **Resursgrupp** | Välj en befintlig resursgrupp eller skapa en ny grupp som **myResourceGroup** |  Giltiga resursgruppnamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Plats** | Ange en giltig plats för den nya servern | För information om regioner, se [Azure-regioner](https://azure.microsoft.com/regions/). |

   ![Skapa logisk server slutförts formulär](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. Klicka på **skapa** att etablera den logiska servern. Etableringen tar några minuter. 

> [!IMPORTANT]
> Kom ihåg din servernamn, server admin-inloggningsnamn och lösenord. Du måste dessa värden senare i den här kursen.
>

## <a name="create-a-server-level-firewall-rule"></a>Skapa en brandväggsregel på servernivå

SQL Database-tjänsten skapar en [brandvägg på servernivå-](sql-database-firewall-configure.md) som förhindrar externa program och verktyg från att ansluta till servern eller en databas på servern såvida inte en brandväggsregel skapas för att öppna brandväggen för Ange IP-adresser. Följ dessa steg om du vill skapa en servernivå brandväggsregel SQL-databasen för den dator som du kör kommandoradsverktyget SQLPackage IP-adress. Detta gör att SQLPackage att ansluta till SQL serverDatabase logiska servern via Azure SQL Database-brandvägg. 

1. Klicka på **alla resurser** i den vänstra menyn och klicka på den nya servern på den **alla resurser** sidan. På översiktssidan för servern öppnas och visar alternativ för ytterligare konfiguration.

     ![Översikt över logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Klicka på **brandväggen** i den vänstra menyn under **inställningar** på översiktssidan. Sidan **Brandväggsinställningar** för SQL Database-servern öppnas. 

3. Klicka på **lägga till klientens IP-Adressen** i verktygsfältet för att lägga till IP-adressen för datorn du använder och klicka sedan på **spara**. En brandväggsregel på servernivå skapas för denna IP-adress.

     ![ange brandväggsregel för server](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. Klicka på **OK**.

Nu kan du ansluta till alla databaser på servern med hjälp av SQL Server Management Studio eller ett annat verktyg som helst från den här IP-adressen med hjälp av server-administratörskonto som skapats tidigare.

> [!NOTE]
> SQL Database kommunicerar via port 1433. Om du försöker ansluta inifrån ett företagsnätverk, kan utgående trafik via port 1433 nekas av nätverkets brandvägg. I så fall kommer du inte att kunna ansluta till din Azure SQL Database-server om inte din IT-avdelning öppnar port 1433.
>

## <a name="import-a-bacpac-file-to-azure-sql-database"></a>Importera en BACPAC-fil till Azure SQL Database 

De senaste versionerna av kommandoradsverktyget SQLPackage ger stöd för att skapa en Azure SQL database vid en angiven [tjänsten tjänstnivå och prestandanivå](sql-database-service-tiers.md). Välj en hög nivå och prestanda servicenivå för bästa prestanda vid import och sedan skala efter importen om servicenivå tjänstnivå och prestandanivå är högre än vad du behöver omedelbart.

Följ dessa steg Använd kommandoradsverktyget SQLPackage för att importera databasen AdventureWorks2008R2 till Azure SQL Database. Du kan använda SQL Server Management Studio för den här uppgiften, är SQLPackage den bästa metoden för de flesta produktionsmiljöer för maximal flexibilitet och bästa prestanda. Se [migrera från SQL Server till Azure SQL Database med BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

- Kör följande SQLPackage-kommando i Kommandotolken att importera den **AdventureWorks2008R2** databasen från lokal lagring till SQL server logisk server som du skapade tidigare till en ny databas, en tjänstnivå för  **Premium**, och ett Tjänstmål för **P6**. Ersätt värdena i hakparenteser med lämpliga värden för din logiska server för SQL-server och ange ett namn för den nya databasen (även ersätta hakparenteser). Du kan också välja att ändra värdena för databasversionen och tjänsten objectgive som passar din miljö. I den här kursen migrerade databasen kallas **myMigratedDatabase**.

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=<your_server_name>.database.windows.net;Initial Catalog=<your_new_database_name>;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> En logisk SQLServer lyssnar på port 1433. Om du försöker ansluta till en SQL server logisk server från inom en företagsbrandvägg måste den här porten öppnas i företagets brandvägg att ansluta.
>

## <a name="connect-using-sql-server-management-studio-ssms"></a>Ansluta med SQL Server Management Studio (SSMS)

Använda SQL Server Management Studio för att upprätta en anslutning till din Azure SQL Database-server och nyligen migrerade databas som heter **myMigratedDatabase** i den här självstudiekursen. Om du kör SQL Server Management Studio på en annan dator som du körde SQLPackage, skapa en brandväggsregel för den här datorn med hjälp av stegen i föregående procedur.

1. Öppna SQL Server Management Studio.

2. I dialogrutan **Anslut till server** anger du följande information:
   - **Servertyp**: Ange databasmotor
   - **Servernamnet**: Ange din fullständiga servernamnet som **mynewserver20170403.database.windows.net**
   - **Autentisering**: Ange SQL Server-autentisering
   - **Logga in**: Ange serveradministratörskontot
   - **Lösenord**: Ange lösenordet för serveradministratörskontot
 
    ![ansluta med ssms](./media/sql-database-migrate-your-sql-server-database/connect-ssms.png)

3. Klicka på **Anslut**. Fönstret Object Explorer öppnas. 

4. I Object Explorer, expandera **databaser** och expandera sedan **myMigratedDatabase** att visa objekten i-exempeldatabasen.

## <a name="change-database-properties"></a>Ändra egenskaper för databas

Du kan ändra den tjänstnivå och prestandanivå kompatibilitetsnivå med hjälp av SQL Server Management Studio. Under importen rekommenderar vi att du importerar till en högre nivå-databas för prestanda för bästa prestanda, men skala när importen är klar för att spara pengar tills du är redo att använda den importerade databasen aktivt. Ändra kompatibilitetsnivån kan ge bättre prestanda och åtkomst till de senaste funktionerna för Azure SQL Database-tjänsten. När du migrerar en äldre databas bevaras dess databasens kompatibilitetsnivå vid den lägsta stödda nivån som är kompatibel med databasen som importeras. Mer information finns i [förbättrad prestanda för frågor med nivå 130 i Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).

1. Högerklicka i Object Explorer **myMigratedDatabase** och på **ny fråga**. Ett frågefönster öppnas ansluten till din databas.

2. Kör följande kommando för att ange tjänstnivån till **Standard** och prestandanivån uppdateras till **S1**.

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

3. Kör följande kommando för att ändra databasens kompatibilitetsnivån till **130**.

    ```
    ALTER DATABASE myMigratedDatabase  
    SET COMPATIBILITY_LEVEL = 130;
    ```

   ![Ändra kompatibilitetsnivån](./media/sql-database-migrate-your-sql-server-database/compat-level.png)

## <a name="next-steps"></a>Nästa steg 
I den här självstudiekursen förberedd du, exporteras och importeras din databas. Du har lärt dig att:

> [!div class="checklist"]
> * Förbereda en databas i en SQL Server för migrering till Azure SQL Database
> * Exportera databasen till en BACPAC-fil
> * Importera filen BACPAC till en Azure SQL Database

Gå vidare till nästa kurs att lära dig att skydda databasen.

> [!div class="nextstepaction"]
> [Skydda din Azure SQL-databas](sql-database-security-tutorial.md).


