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
# <a name="migrate-your-sql-server-database-tooazure-sql-database"></a>Migrera din SQL Server-databasen tooAzure SQL-databas

Flytta SQL Server database tooAzure SQL-databas är en process med tre delar - du först förbereda och sedan exportera och importera hello-databasen. I kursen får du lära dig att:

> [!div class="checklist"]
> * Förbered en databas i en SQL Server för migrering tooAzure SQL-databas med hjälp av hello [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA)
> * Exportera hello databasfilen tooa BACPAC
> * Importera hello BACPAC fil till en Azure SQL Database

Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Krav

den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:

- Installerade hello senaste versionen av [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Installera SSMS installeras också hello senaste versionen av SQLPackage, ett kommandoradsverktyg som kan använda tooautomate ett antal uppgifter för utveckling av databasen. 
- Installerade hello [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
- Du har identifierat och har åtkomst tooa databasen toomigrate. Den här kursen använder hello [SQL Server 2008R2 AdventureWorks OLTP-databasen](https://msftdbprodsamples.codeplex.com/releases/view/59211) på en instans av SQL Server 2008R2 eller senare, men du kan använda alla databaser som du väljer. Använd toofix kompatibilitetsproblem [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)

## <a name="prepare-for-migration"></a>Förbered för migrering

Är du redo tooprepare för migrering. Följ dessa steg toouse hello  **[Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595)**  tooassess hello beredskap för din databas för migrering tooAzure SQL-databas.

1. Öppna hello **Data Migration Assistant**. Du kan köra DMA på valfri dator med anslutning toohello SQL Server-instans som innehåller hello-databas som du planerar toomigrate, behöver du inte tooinstall på hello datorn där hello SQLServer-instansen.

     ![Öppna assistenten för migrering av data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. Hello vänstra menyn klickar du på **+ ny** toocreate en **Assessment** projekt. Fyll i hello formulär med en **projektnamn** (alla andra värden ska vara kvar standardvärdena) och klicka på **skapa**. Hej **alternativ** öppnas.

     ![nya data migrering assistenten projektet](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3. På hello **alternativ** klickar du på **nästa**. Hej **Välj källor** öppnas.

     ![nya alternativ för migrering av data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-options.png) 

4. På hello **Välj källor** anger hello namnet på SQL Server-instans som innehåller hello-server som du planerar toomigrate. Ändra hello andra värden på den här sidan om det behövs. Klicka på **Anslut**.

     ![nya migreringen Välj datakällor](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources.png)

5. I hello **lägger till källor** del av hello **Välj källor** väljer hello kryssrutorna för hello databaser toobe testas för kompatibilitet. Klicka på **Lägg till**.

     ![nya migreringen Välj datakällor](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources-add.png)

6. Klicka på **starta Assessment**.

     ![Starta utvärdering av nya data migreringen](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-start-assessment.png)

7. När hello utvärderingen har slutförts först kontrollerar toosee om hello-databasen är tillräckligt kompatibel toomigrate. Leta efter hello bock i en grön cirkel.

     ![nya data migrering utvärderingsresultat kompatibel](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-compatible.png)

8. Granska resultatet från hello. Hej **SQL Server-funktionsparitet** resultat som visas är hello standard resultat tooreview. Granska specifikt hello information om funktioner som inte stöds och delvis stöds och hello tillhandahålls information om rekommenderade åtgärder. 

     ![nya data migrering assessment paritet](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. Granska hello **kompatibilitetsproblem** genom att klicka på alternativet i hello övre vänstra hörnet. Mer specifikt granska hello information om migrering blockeringar, funktionsändringar, och inaktuella funktioner för varje kompatibilitetsnivå. Granska hello ändringar för hello AdventureWorks2008R2 databasen tooFull textsökning eftersom SQL Server 2008 och hello ändringar tooSERVERPROPERTY('LCID') sedan SQL Server 2000. Mer information om dessa ändringar finns länkar för mer information. Många sökalternativ och inställningar för textsökning har ändrats 

   > [!IMPORTANT] 
   > När du migrerar din databas tooAzure SQL-databas, väljer du toooperate hello databas på dess aktuella kompatibilitetsnivån (nivå 100 för hello AdventureWorks2008R2 databasen) eller på en högre nivå. Läs mer om hello effekter och alternativ för att driva en databas på en specifik kompatibilitetsnivå [ändra DATABASENS kompatibilitetsnivån](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Se även [ALTER OMFÅNG DATABASKONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) information om ytterligare databasnivå inställningar relaterade toocompatibility nivåer.
   >

10. Du kan också klicka på **exportera rapporten** toosave hello rapporten som en JSON-fil.
11. Stäng hello Data Migration Assistant.

## <a name="export-toobacpac-file"></a>Exportera tooBACPAC fil 

En BACPAC-fil är en ZIP-fil med filnamnstillägget BACPAC som innehåller hello metadata och data från en SQL Server-databas. En BACPAC-fil kan lagras i Azure blob storage eller lokal lagring för arkivering eller för att migrera - exempel från SQL Server-tooAzure SQL-databas. För en export-toobe transaktionellt konsekvent, måste du kontrollera antingen att inga skrivåtgärder aktivitet sker under hello exporten.

Följ dessa steg toouse hello SQLPackage kommandoradsverktyget tooexport hello AdventureWorks2008R2 toolocal databaslagring.

1. Öppna en kommandotolk och ändra katalogen tooa mapp där du har hello **130** version av SQLPackage - som **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.

2. Kör följande SQLPackage kommando på hello kommandotolk tooexport hello hello **AdventureWorks2008R2** databasen från **localhost** för**AdventureWorks2008R2.bacpac**. Ändra dessa värden som lämpliga tooyour miljö.

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![sqlpackage export](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

När hello körningen har slutförts lagras hello genereras BACPAC i hello katalogen där hello sqlpackage körbara finns. I det här exemplet C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin. 

## <a name="log-in-toohello-azure-portal"></a>Logga in toohello Azure-portalen

Logga in toohello [Azure-portalen](https://portal.azure.com/). Inloggning från hello dator från vilken du kör hello SQLPackage kommandoradsverktyget underlättar hello skapandet av hello brandväggsregel i steg 5.

## <a name="create-a-sql-server-logical-server"></a>Skapa en logisk SQLServer

En [SQL server logiska servern](sql-database-features.md) fungerar som en central administrativ plats för flera databaser. Följ dessa steg toocreate en SQL server logisk server toocontain hello migreras Adventure Works OLTP SQL Server-databas. 

1. Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.

2. Typen **SQLServer** hello Sök i i fönstret hello **ny** och väljer **SQL-databas (logisk server)** från hello filtrerad lista.

    ![välj logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. På hello **allt** klickar du på **SQLServer (logisk server)** och klicka sedan på **skapa** på hello **SQL Server (logisk server)** sidan .

    ![Skapa logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. Fyll i formuläret om hello SQL server (logisk server) med hello följande information som visas i föregående bild hello:     

   | Inställning       | Föreslaget värde | Beskrivning | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servernamn** | Ange ett globalt unikt namn | Giltiga servernamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Inloggning för serveradministratör** | Ange ett giltigt namn | För giltiga inloggningsnamn, se [Databasidentifierare](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). |
   | **Lösenord** | Ange en giltig lösenord | Lösenordet måste innehålla minst 8 tecken och måste innehålla tecken från tre av hello följande kategorier: versaler, gemener, siffror och specialtecken. |
   | **Prenumeration** | Välj en prenumeration | Mer information om dina prenumerationer finns i [Prenumerationer](https://account.windowsazure.com/Subscriptions). |
   | **Resursgrupp** | Välj en befintlig resursgrupp eller skapa en ny grupp som **myResourceGroup** |  Giltiga resursgruppnamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Plats** | Ange en giltig plats för nya hello-server | För information om regioner, se [Azure-regioner](https://azure.microsoft.com/regions/). |

   ![Skapa logisk server slutförts formulär](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. Klicka på **skapa** tooprovision hello logisk server. Etableringen tar några minuter. 

> [!IMPORTANT]
> Kom ihåg din servernamn, server admin-inloggningsnamn och lösenord. Du måste dessa värden senare i den här kursen.
>

## <a name="create-a-server-level-firewall-rule"></a>Skapa en brandväggsregel på servernivå

hello SQL Database-tjänsten skapar en [brandvägg på servernivå för hello](sql-database-firewall-configure.md) som förhindrar att externa program och verktyg ansluter toohello server eller en databas på servern hello såvida inte en brandväggsregel skapas tooopen hello Brandvägg för specifika IP-adresser. Följ dessa steg toocreate en servernivå brandväggsregel SQL-databas för hello IP-adress hello dator från vilken du kör hello SQLPackage kommandoradsverktyg. Detta gör att SQLPackage tooconnect toohello serverDatabase logisk SQL-server via hello Azure SQL Database-brandvägg. 

1. Klicka på **alla resurser** hello vänstra menyn och klicka på den nya servern på hello **alla resurser** sidan. hello översiktssidan för servern öppnas och visar alternativ för ytterligare konfiguration.

     ![Översikt över logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Klicka på **brandväggen** hello vänstra menyn under **inställningar** på översiktssidan för hello. Hej **brandväggsinställningar** öppnas sidan för hello SQL Database-server. 

3. Klicka på **lägga till klientens IP-Adressen** på hello verktygsfältet tooadd hello IP-adressen hello datorn du använder och klicka sedan på **spara**. En brandväggsregel på servernivå skapas för denna IP-adress.

     ![ange brandväggsregel för server](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. Klicka på **OK**.

Nu kan du ansluta tooall databaser på servern med hjälp av SQL Server Management Studio eller ett annat verktyg som helst från den här IP-adressen med hello server administratörskonto som skapats tidigare.

> [!NOTE]
> SQL Database kommunicerar via port 1433. Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 1433 inte av ditt nätverks brandvägg. I så fall, kan du inte ansluta tooyour Azure SQL Database-server om din IT-avdelning öppnar port 1433.
>

## <a name="import-a-bacpac-file-tooazure-sql-database"></a>Importera en BACPAC filen tooAzure SQL-databas 

hello nyaste versionerna av hello SQLPackage kommandoradsverktyget ger stöd för att skapa en Azure SQL database vid en angiven [tjänsten tjänstnivå och prestandanivå](sql-database-service-tiers.md). Välj en hög nivå och prestanda servicenivå för bästa prestanda vid import och sedan skala efter importen om hello tjänstnivå och prestandanivå servicenivå är högre än vad du behöver omedelbart.

Följ dessa steg Använd hello SQLPackage kommandoradsverktyget tooimport hello AdventureWorks2008R2 databasen tooAzure SQL-databas. Du kan använda SQL Server Management Studio för den här uppgiften, är SQLPackage hello önskad metod för de flesta produktionsmiljöer för maximal flexibilitet och bästa prestanda. Se [migrera från SQL Server-tooAzure SQL-databas med hjälp av BACPAC filer](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

- Kör följande SQLPackage kommando på hello kommandotolk tooimport hello hello **AdventureWorks2008R2** från logiska server för lokal lagring toohello SQL server-databas som du tidigare skapade tooa ny databas, en tjänst nivån av **Premium**, och ett Tjänstmål för **P6**. Ersätt hello värden i hakparenteser med lämpliga värden för din logiska server för SQL-server och ange ett namn för hello ny databas (även ersätta hello hakparenteser). Du kan också välja toochange hello värden för databasversionen och tjänsten objectgive som lämpliga tooyour miljö. För hello syftet med den här självstudiekursen, hello migrerade databasen kallas **myMigratedDatabase**.

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=<your_server_name>.database.windows.net;Initial Catalog=<your_new_database_name>;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> En logisk SQLServer lyssnar på port 1433. Om du försöker tooconnect tooa SQL server logiska server från en företagsbrandvägg måste den här porten öppnas i hello företagets brandvägg för du toosuccessfully ansluta.
>

## <a name="connect-using-sql-server-management-studio-ssms"></a>Ansluta med SQL Server Management Studio (SSMS)

Använda SQL Server Management Studio tooestablish en anslutning tooyour Azure SQL Database-server och migrerade databas som heter **myMigratedDatabase** i den här självstudiekursen. Om du kör SQL Server Management Studio på en annan dator som du körde SQLPackage, skapa en brandväggsregel för den här datorn med hjälp av hello steg hello föregående procedur.

1. Öppna SQL Server Management Studio.

2. I hello **ansluta tooServer** dialogrutan Ange hello följande information:
   - **Servertyp**: Ange databasmotor
   - **Servernamnet**: Ange din fullständiga servernamnet som **mynewserver20170403.database.windows.net**
   - **Autentisering**: Ange SQL Server-autentisering
   - **Logga in**: Ange serveradministratörskontot
   - **Lösenordet**: Ange hello lösenord för administratörskontot server
 
    ![ansluta med ssms](./media/sql-database-migrate-your-sql-server-database/connect-ssms.png)

3. Klicka på **Anslut**. hello öppnas Object Explorer. 

4. I Object Explorer, expandera **databaser** och expandera sedan **myMigratedDatabase** tooview hello objekt i hello-exempeldatabasen.

## <a name="change-database-properties"></a>Ändra egenskaper för databas

Du kan ändra hello-tjänstnivå och prestandanivå kompatibilitetsnivå med hjälp av SQL Server Management Studio. Under importen hello rekommenderar vi att du databasimport tooa högre prestanda nivån för bästa prestanda, men skala när hello importen är klar toosave pengar tills du är klar tooactively använda hello importerade databas. Ändrar kompatibilitetsnivå hello kan ge bättre prestanda och toohello senaste åtkomstfunktioner för hello Azure SQL Database-tjänsten. När du migrerar en äldre databas bevaras dess Databaskompatibilitetsnivån på hello lägsta stöds nivå som är kompatibel med hello databasen som importeras. Mer information finns i [förbättrad prestanda för frågor med nivå 130 i Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).

1. Högerklicka i Object Explorer **myMigratedDatabase** och på **ny fråga**. Ett frågefönster öppnas anslutna tooyour databas.

2. Köra hello efter kommandot tooset hello tjänstnivån för**Standard** och hello prestandanivå för**S1**.

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

3. Köra hello efter kommandot toochange hello databasens kompatibilitetsnivå för**130**.

    ```
    ALTER DATABASE myMigratedDatabase  
    SET COMPATIBILITY_LEVEL = 130;
    ```

   ![Ändra kompatibilitetsnivån](./media/sql-database-migrate-your-sql-server-database/compat-level.png)

## <a name="next-steps"></a>Nästa steg 
I den här självstudiekursen förberedd du, exporteras och importeras din databas. Du har lärt dig att:

> [!div class="checklist"]
> * Förbered en databas i en SQL Server för migrering tooAzure SQL-databas
> * Exportera hello databasfilen tooa BACPAC
> * Importera hello BACPAC fil till en Azure SQL Database

I förväg toohello nästa självstudiekurs toolearn hur toosecure din databas.

> [!div class="nextstepaction"]
> [Skydda din Azure SQL-databas](sql-database-security-tutorial.md).


