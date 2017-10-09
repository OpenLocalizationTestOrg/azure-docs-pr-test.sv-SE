---
title: aaaSecure Azure SQL database | Microsoft Docs
description: Mer information om tekniker och funktioner toosecure Azure SQL database.
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: 1450d633d6f65faf1b8a2dc0dc7dfe996fb0719d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-azure-sql-database"></a>Skydda din Azure SQL-databas

SQL-databas skyddar dina data genom att begränsa åtkomst tooyour databasen med hjälp av brandväggsregler, autentiseringsmekanismer som kräver användare tooprove deras identitet och auktorisering toodata genom rollbaserad medlemskap och behörigheter, samt via säkerhet på radnivå och dynamisk datamaskning.

Du kan förbättra hello skyddet av databasen mot obehöriga användare eller obehörig åtkomst med bara några få enkla steg. I den här kursen lär du dig: 

> [!div class="checklist"]
> * Konfigurera brandväggsregler på servernivå för servern i hello Azure-portalen
> * Konfigurera brandväggsregler på databasnivå för databasen med hjälp av SSMS
> * Ansluta tooyour databasen med hjälp av en säker anslutningssträng
> * Hantera användarnas åtkomst
> * Skydda dina data med kryptering
> * Aktivera granskning av SQL-databas
> * Aktivera hotidentifiering för SQL-databas

Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Krav

toocomplete den här självstudiekursen, se till att du har hello följande:

- Installerade hello senaste versionen av [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). 
- Installerade Microsoft Excel
- Skapa en Azure SQL-server och databas - finns [skapa en Azure SQL database i hello Azure-portalen](sql-database-get-started-portal.md), [skapar en enda Azure SQL-databas med hello Azure CLI](sql-database-get-started-cli.md), och [skapa en enskild SQL Azure databasen med hjälp av PowerShell](sql-database-get-started-powershell.md). 

## <a name="log-in-toohello-azure-portal"></a>Logga in toohello Azure-portalen

Logga in toohello [Azure-portalen](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Skapa en brandväggsregel på servernivå i hello Azure-portalen

SQL-databaser är skyddade av en brandvägg i Azure. Som standard är alla anslutningar toohello server och hello databaser hello-servern avvisade förutom anslutningar från andra Azure-tjänster. Mer information finns i [Azure SQL Database server- och databasnivå brandväggsregler](sql-database-firewall-configure.md).

hello säkraste konfigurationen är tooset 'Tillåter åtkomst tooAzure services' tooOFF. Om du behöver tooconnect toohello databasen från en virtuell Azure-dator eller ett moln-tjänst kan du skapa en [reserverade IP: N](../virtual-network/virtual-networks-reserved-public-ip.md) och att bara tillåta hello reserverade IP-adress åtkomst via hello-brandväggen. 

Följ dessa steg toocreate en [SQL-databas brandväggsregel på servernivå](sql-database-firewall-configure.md) för server tooallow-anslutningar från en specifik IP-adress. 

> [!NOTE]
> Om du har skapat en exempeldatabas i Azure med hjälp av en av hello tidigare självstudier eller Snabbstart och är utför den här kursen på en dator med hello samma IP-adress som den hade när du har gått igenom dessa självstudier, du kan hoppa över detta steg som du har redan skapat en brandväggsregel på servernivå.
>

1. Klicka på **SQL-databaser** från hello vänstra menyn och klicka på hello databasen du vill tooconfigure hello brandväggsregeln på hello **SQL-databaser** sidan. hello översiktssidan för din databas öppnas som visar du hello fullständigt kvalificerade servernamnet (exempelvis **mynewserver 20170313.database.windows.net**) och innehåller alternativ för ytterligare konfiguration.

      ![brandväggsregler för server](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Klicka på **ange serverbrandvägg** hello verktygsfältet enligt hello föregående bild. Hej **brandväggsinställningar** öppnas sidan för hello SQL Database-server. 

3. Klicka på **lägga till klientens IP-Adressen** på hello verktygsfältet tooadd hello offentliga IP-adress hello datorn anslutna toohello portal med eller ange hello brandväggsregel manuellt och klickar sedan på **spara**.

      ![ange brandväggsregel för server](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. Klicka på **OK** och klicka sedan på hello **X** tooclose hello **brandväggsinställningar** sidan.

Nu kan du ansluta tooany databas i hello server med hello angivna IP-adress eller IP-adressintervall.

> [!NOTE]
> SQL Database kommunicerar via port 1433. Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 1433 inte av ditt nätverks brandvägg. I så fall, blir inte kan tooconnect tooyour Azure SQL Database-server om din IT-avdelning öppnar port 1433.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>Skapa en brandväggsregel på databasnivå med hjälp av SSMS

Brandväggsregler på databasnivå aktivera toocreate olika brandväggsinställningar för olika databaser inom hello samma logiska server och toocreate brandväggsregler som kan användas-vilket innebär att de följer hello databas under en [växling vid fel ](sql-database-geo-replication-overview.md) i stället för att lagras på hello SQLServer. Brandväggsregler på databasnivå kan bara vara konfigurerad med hjälp av Transact-SQL-uttryck och bara när du har konfigurerat hello första servernivå brandväggsregel. Mer information finns i [Azure SQL Database server- och databasnivå brandväggsregler](sql-database-firewall-configure.md).

Följer dessa steg toocreate en databasspecifika brandväggsregel.

1. Ansluta tooyour databasen, till exempel med [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. Högerklicka på hello-databas som du vill tooadd en brandvägg regel för och klicka på i Object Explorer **ny fråga**. Ett tomt frågefönster öppnas som är anslutna tooyour databas.

3. Ändra hello IP-adress tooyour offentliga IP-adressen i hello frågefönstret och kör följande fråga hello:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. På verktygsfältet hello **kör** toocreate hello brandväggsregel.

## <a name="view-how-tooconnect-an-application-tooyour-database-using-a-secure-connection-string"></a>Visa hur tooconnect ett program tooyour databasen med en säker anslutningssträng

tooensure en säker och krypterad anslutning mellan ett klientprogram och en SQL-databas, hello anslutningssträngen har toobe som konfigurerats för att:

- Begär en krypterad anslutning och
- toonot förtroende hello servercertifikat. 

Detta upprättar en anslutning med Transport Layer Security (TLS) och minskar hello risken för man-in-the-middle-attacker. Du kan få korrekt konfigurerade anslutningssträngar för SQL-databas för klient som stöds drivrutiner från hello Azure-portalen som visas för ADO.net i den här skärmbilden.

1. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.

2. På hello **översikt** för din databas, klickar du på **visa databasanslutningssträngar**.

3. Granska hello fullständig **ADO.NET** anslutningssträngen.

    ![ADO.NET-anslutningssträng](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Skapa databasanvändare

Innan du skapar användare, måste du först välja från en av två typer av autentisering som stöds av Azure SQL Database: 

**SQL-autentisering**, som använder användarnamn och lösenord för inloggning och användare som är giltig endast i hello kontexten för en viss databas i en logisk server. 

**Azure Active Directory-autentisering**, som använder identiteter som hanteras av Azure Active Directory. 

Om du vill toouse [Azure Active Directory](./sql-database-aad-authentication.md) tooauthenticate mot SQL-databasen, en ifylld Azure Active Directory måste finnas innan du kan fortsätta.

Följ dessa steg toocreate en användare som använder SQL-autentisering:

1. Ansluta tooyour databasen, till exempel med [SQL Server Management Studio](./sql-database-connect-query-ssms.md) med dina administratörsautentiseringsuppgifter för servern.

2. Högerklicka på hello databasen du vill använda tooadd en ny användare på och klicka på i Object Explorer **ny fråga**. Ett tomt frågefönster öppnas som är anslutna toohello valda databasen.

3. Ange hello följande fråga i frågefönstret hello:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. På verktygsfältet hello **kör** toocreate hello användare.

5. Som standard hello användare kan ansluta toohello databasen men har inte några behörigheter tooread eller skriva data. toogrant dessa behörigheter toohello skapade nyligen användaren, köra hello följande två kommandon i ett nytt frågefönster

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

Det är bästa praxis toocreate dessa icke-administratörskonton på hello databasen nivå tooconnect tooyour databasen om du inte behöver tooexecute administratörsåtgärder som skapar nya användare. Granska hello [Azure Active Directory-kursen](./sql-database-aad-authentication-configure.md) om hur tooauthenticate med Azure Active Directory.


## <a name="protect-your-data-with-encryption"></a>Skydda dina data med kryptering

Azure SQL Database transparent datakryptering (TDE) krypteras automatiskt data i vila, utan några ändringar toohello programmet komma åt hello krypterade databasen. För nya databaser är TDE aktiverad som standard. tooenable TDE för din databas eller tooverify TDE är aktiverad, Följ dessa steg:

1. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan. 

2. Klicka på **Transparent datakryptering** tooopen hello konfigurationssidan för TDE.

    ![Transparent datakryptering](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. Om det behövs, ange **datakryptering** tooON och på **spara**.

hello krypteringsprocessen startar i hello bakgrund. Du kan övervaka hello med anslutande tooSQL databasen [SQL Server Management Studio](./sql-database-connect-query-ssms.md) genom att fråga hello encryption_state kolumn i hello `sys.dm_database_encryption_keys` vyn.

## <a name="enable-sql-database-auditing-if-necessary"></a>Aktivera SQL Database auditing, om det behövs

Azure SQL Database Auditing spårar databashändelser och skriver dem tooan granskningslogg i ditt Azure Storage-konto. Granskning hjälper dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan indikera potentiella säkerhetsöverträdelser. Följ dessa steg toocreate en granskning standardprincip för SQL-databasen:

1. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan. 

2. Välj i hello inställningsbladet **granskning och Hotidentifiering**. Observera att servernivå granskning är diabled och att det finns en **visa inställningarna för** länk som gör att du tooview eller ändra hello server granskningsinställningar från den här kontexten.

    ![Granskning bladet](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Om du föredrar tooenable en Audit typ (eller plats?) skiljer sig från hello som har angetts på hello servernivå aktivera **ON** granskning, och välj hello **Blob** Granskningstypen. Om servern blobbgranskning är aktiverat, kommer att finnas hello konfigurerad databasgranskningen bredvid hello server Blob audit.

    ![Aktivera granskning](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Välj **lagringsinformation** tooopen hello granska loggarna lagring-bladet. Välj hello Azure storage-konto där loggar sparas och hello kvarhållningsperiod vilka hello gamla loggarna tas bort, klicka sedan på **OK** längst ned hello. 

   > [!TIP]
   > Använd hello samma lagringskonto för alla granskad databaser tooget hello mesta av hello granskning rapporterar mallar.
   > 

5. Klicka på **Spara**.

> [!IMPORTANT]
> Om du vill ha toocustomize hello granskade händelser kan du göra detta via PowerShell eller REST API - finns hello [Automation (PowerShell / REST-API)](sql-database-auditing.md#subheading-7) mer information.
>

## <a name="enable-sql-database-threat-detection"></a>Aktivera hotidentifiering för SQL-databas

Hotidentifiering innehåller ett nytt lager av säkerhet som gör att kunder toodetect och svarar toopotential hot som de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter. Användare kan utforska hello misstänkta händelser med hjälp av SQL Database Auditing toodetermine om de kommer från en försök tooaccess, överträdelse eller utnyttja data i hello-databas. Hotidentifiering gör det enkelt tooaddress potentiella hot toohello databas utan hello måste toobe en säkerhetsexpert på eller hantera avancerade säkerhetsövervakning system.
Hotidentifiering identifierar till exempel vissa avvikande databasaktiviteter som indikerar potentiella försök med SQL injection. SQL injection är en av hello vanliga Web application säkerhetsproblem på hello Internet, används tooattack datadrivna program. Angripare dra nytta av programmet säkerhetsrisker tooinject skadliga SQL-instruktioner i programmet post fält för brott mot eller ändra data i hello-databas.

1. Navigera toohello configuration bladet för hello SQL-databas som du vill toomonitor. Välj i hello inställningsbladet **granskning och Hotidentifiering**.

    ![Navigeringsfönstret](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. I hello **granskning och Hotidentifiering** configuration bladet Stäng **ON** granskning, som visar hello threat detection inställningar.

3. Aktivera **på** hotidentifiering.

4. Konfigurera hello lista med e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande databasaktiviteter.

5. Klicka på **spara** i hello **Auditing & Threat detection** bladet toosave hello nya eller uppdaterade granskning och hotidentifiering princip.

    ![Navigeringsfönstret](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Om avvikande databasaktiviteter identifieras, får du ett e-postmeddelande vid identifiering av avvikande databasaktiviteter. hello e-post ger information om hello misstänkta säkerhetshändelse inklusive hello uppbyggnad hello avvikande aktiviteter, databasnamn, namn och hello händelse servertid. Dessutom den ger information om möjliga orsaker och rekommenderade åtgärder tooinvestigate och minska hello potentiella hot toohello databas. hello nästa steg gå igenom vilka toodo bör du ta emot sådan ett e-postmeddelande:

    ![Threat detection e-post](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. I hello e-post, klickar du på hello **Azure SQL-granskning logg** länk som startar hello Azure-portalen och visa hello relevanta granskning poster runt hello tidpunkt hello misstänkta händelsen.

    ![Granskningsposter](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Klicka på hello audit poster tooview mer information om hello misstänkt databasaktiviteter, till exempel SQL-instruktionen, fel orsak och klient-IP.

    ![Registrera information](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. I hello granskning poster bladet, klickar du på **öppna i Excel** tooopen förkonfigurerad excel mallen tooimport och kör djupare analys av hello granskningsloggen runt hello tidpunkt hello misstänkta händelsen.

    > [!NOTE]
    > I Excel 2010 eller senare, Power Query och hello **snabb kombinera** inställningen är obligatorisk.

    ![Öppna poster i Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. tooconfigure hello **snabb kombinera** inställningen - i hello **POWER QUERY** menyfliksområdet fliken **alternativ** toodisplay hello dialogrutan. Välj hello sekretess avsnitt och välj hello andra alternativet - ”Ignorera hello sekretessnivåerna och förbättra prestanda”:

    ![Excel snabb kombinera](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. tooload granskningsloggarna för SQL, kontrollera att hello parametrar på fliken Inställningar för hello är korrekt inställda Välj hello ”uppgifter” menyfliksområdet och klickar på ”Uppdatera alla' hello-knappen.

    ![Excel-parametrar](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. hello resultat visas i hello **SQL granskningsloggar** blad som gör att du toorun djupare analys av hello avvikande aktiviteter som har upptäckts och minska hello effekten av hello säkerhetshändelse i ditt program.


## <a name="next-steps"></a>Nästa steg
Du kan förbättra hello skyddet av databasen mot obehöriga användare eller obehörig åtkomst med bara några få enkla steg. I den här kursen lär du dig: 

> [!div class="checklist"]
> * Konfigurera brandväggsregler för servern och eller-databas
> * Ansluta tooyour databasen med hjälp av en säker anslutningssträng
> * Hantera användarnas åtkomst
> * Skydda dina data med kryptering
> * Aktivera granskning av SQL-databas
> * Aktivera hotidentifiering för SQL-databas

> [!div class="nextstepaction"]
>[Förbättra prestanda för SQL-databasen](sql-database-performance-tutorial.md)

