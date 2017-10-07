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
# <a name="getting-started-with-azure-sql-data-sync-preview"></a>Komma igång med datasynkronisering för Azure SQL (förhandsgranskning)
I kursen får du lära dig hur tooset upp Azure SQL Data Sync genom att skapa en hybrid sync-grupp som innehåller både Azure SQL Database och SQL Server-instanser. hello ny synkronisering grupp helt har konfigurerats och synkroniserar på hello schema du anger.

Den här kursen förutsätter att du har minst tidigare erfarenhet med SQL Database och SQL Server. 

En översikt över SQL datasynkronisering finns [data synkroniseras](sql-database-sync-data.md).

Fullständig PowerShell-exempel som visar hur tooconfigure SQL datasynkronisering Se hello följande artiklar:
-   [Använd PowerShell toosync mellan flera Azure SQL-databaser](scripts/sql-database-sync-data-between-sql-databases.md)
-   [Använd PowerShell toosync mellan en Azure SQL Database och en lokal SQL Server-databas](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!NOTE]
> hello omfattande teknisk dokumentation för Azure SQL datasynkronisering som tidigare fanns på MSDN, är tillgänglig som en. PDF-dokument. Hämta den [här](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).

## <a name="step-1---create-sync-group"></a>Steg 1 – Skapa sync-grupp

### <a name="locate-hello-data-sync-settings"></a>Hitta inställningarna för hello datasynkronisering

1.  Navigera toohello Azure-portalen i webbläsaren.

2.  Leta upp din SQL-databaser från instrumentpanelen eller hello SQL-databaser ikon hello verktygsfältet i hello-portalen.

    ![Lista över Azure SQL-databaser](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  På hello **SQL-databaser** bladet, Välj hello befintliga SQL-databas som du vill toouse som hello hubb databasen för Data Sync. hello SQL-databasbladet öppnas.

4.  Välj på hello SQL-databasbladet för hello valda databasen **synkronisera tooother databaser**. hello datasynkronisering blad öppnas.

    ![Synkroniseringsalternativet tooother databaser](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Skapa en ny grupp för synkronisering

1.  Hello datasynkronisering bladet välj **ny synkronisering grupp**. Hej **ny synkronisering grupp** blad öppnas med steg 1, **skapa sync grupp**, markerade. Hej **skapa Sync datagruppen** också öppnas i blad.

2.  På hello **skapa Sync datagruppen** bladet hello följande saker:

    1.  I hello **Sync gruppnamn** , ange ett namn för hello nya sync-gruppen.

    2.  I hello **synkronisera Metadata-databasen** väljer du om toocreate en ny databas (rekommenderas) eller toouse en befintlig databas.

        > [!NOTE]
        > Microsoft rekommenderar att du skapar en ny, tom databas toouse som hello synkronisera Metadata-databasen. Datasynkronisering skapar tabeller i databasen och kör en frekventa arbetsbelastning. Den här databasen delas automatiskt som hello synkronisera Metadata-databasen för alla dina synkroniseringsgrupper i hello valda regionen. Du kan inte ändra hello synkronisera Metadata-databasen, namnet eller dess servicenivå utan att släppa den.

        Om du väljer **ny databas**väljer **Skapa ny databas.** Hej **SQL-databas** blad öppnas. På hello **SQL-databas** bladet namn och konfigurera hello ny databas. Välj sedan **OK**.

        Om du väljer **Använd befintlig databas**väljer hello databasen hello-listan.

    3.  I hello **automatisk synkronisering** väljer du först **på** eller **av**.

        Om du väljer **på**, i hello **Synkroniseringsfrekvensen** , ange ett tal och välj sekunder, minuter, timmar eller dagar.

        ![Ange frekvens för synkronisering](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  I hello **konfliktlösning** väljer du ”hubb wins” eller ”medlem wins”.

        ![Ange hur konflikter löses](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  Välj **OK** och vänta tills hello nya sync grupp toobe skapas och distribueras.

## <a name="step-2---add-sync-members"></a>Steg 2 – lägga till sync-medlemmar

När hello ny synkronisering grupp skapas och distribueras, steg 2, **lägga till medlemmar i synkronisering**, markeras i hello **ny synkronisering grupp** bladet.

I hello **hubb databasen** ange hello befintliga autentiseringsuppgifter för hello SQL Database-server som hello hubb databasen finns. Ange inte *nya* autentiseringsuppgifter i det här avsnittet.

![NAV-databasen har lagts till toosync grupp](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a>Lägg till en Azure SQL-databas

I hello **medlem databasen** avsnittet om du vill lägga till en Azure SQL Database toohello sync-grupp genom att välja **lägga till en Azure-databas**. Hej **konfigurera Azure Database** blad öppnas.

På hello **konfigurera Azure Database** bladet hello följande saker:

1.  I hello **Sync medlemsnamn** anger ett namn för hello nya sync medlemmen. Det här namnet skiljer sig från hello namnet på själva hello-databasen.

2.  I hello **prenumeration** , markera hello som är associerade med Azure-prenumeration för fakturering.

3.  I hello **Azure SQL Server** , markera hello befintliga SQL-databasservern.

4.  I hello **Azure SQL Database** , markera hello befintliga SQL-databas.

5.  I hello **Sync anvisningarna** väljer dubbelriktad synkronisering toohello hubben, eller från hello hubb.

    ![Lägga till en ny SQL-databas sync medlem](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  I hello **användarnamn** och **lösenord** ange hello befintliga autentiseringsuppgifter för hello SQL Database-server på vilken hello medlem databasen är placerad. Ange inte *nya* autentiseringsuppgifter i det här avsnittet.

7.  Välj **OK** och vänta tills hello nya sync medlem toobe skapas och distribueras.

    ![Ny SQL-databas sync medlem har lagts till](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a>Lägg till en lokal SQL Server-databas

I hello **medlem databasen** avsnittet om du vill lägga till en lokal SQL Server toohello sync-grupp genom att välja **lägga till en lokal databas**. Hej **konfigurera lokalt** blad öppnas.

På hello **konfigurera lokalt** bladet hello följande saker:

1.  Välj **Välj hello Sync Agent Gateway**. Hej **Välj Agent för Sync** blad öppnas.

    ![Välj hello sync agent gateway](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  På hello **Välj hello Sync Agent Gateway** bladet Välj om toouse en befintlig agent eller skapa en ny agent.

    Om du väljer **befintliga agenter**, Välj hello befintliga agenten hello-listan.

    Om du väljer **skapa en ny agent**, hello följande saker:

    1.  Hämta hello klientprogrammet sync-agenten från hello länken och installera den på hello datorn där hello SQL Server finns.
 
        > [!IMPORTANT]
        > Du har tooopen utgående TCP-port 1433 i hello brandväggen toolet hello klientagenten kommunicera med hello-servern.


    2.  Ange ett namn för hello agent.

    3.  Välj **skapa och generera nyckel**.

    4.  Kopiera hello agent viktiga toohello Urklipp.
        
        ![Skapa en ny agent för synkronisering](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  Välj **OK** tooclose hello **Välj Agent för Sync** bladet.

    6.  Leta reda på hello SQL Server-datorn och kör hello Sync-Klientagenten app.

        ![hello data synkroniseras agent-klientappen](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  I hello sync agent app väljer **skicka Agentnyckeln**. Hej **synkronisera Metadata databaskonfiguration** öppnas.

    8.  I hello **synkronisera Metadata databaskonfiguration** dialogrutan Klistra in hello agentnyckeln kopieras från hello Azure-portalen. Dessutom hello befintliga autentiseringsuppgifter för hello Azure SQL Database-server på vilken hello metadata-databasen finns. (Om du har skapat en ny databas för metadata för databasen finns i hello samma server som hello hubb databasen.) Välj **OK** och vänta tills hello configuration toofinish.

        ![Ange hello och autentiseringsuppgifter för agenten](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   Om du får ett Brandväggsfel nu har du toocreate en brandväggsregel på Azure tooallow inkommande trafik från hello SQL Server-datorn. Du kan skapa hello regel manuellt i hello-portalen, men kan det vara lättare toocreate den i SQL Server Management Studio (SSMS). Försök i SSMS, tooconnect toohello NAV-databas på Azure. Ange namnet som \<hub_database_name\>. database.windows.net. Hello åtgärderna i hello dialogrutan rutan tooconfigure hello Azure brandväggsregel. Returnera toohello Sync-Klientagenten app.

    9.  Klicka i hello Sync-Klientagenten app **registrera** tooregister en SQL Server-databas med hello agent. Hej **SQL Server-konfigurationsfilen** öppnas.

        ![Lägga till och konfigurera en SQL Server-databas](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. I hello **SQL Server-konfigurationsfilen** dialogrutan Välj om tooconnect med hjälp av SQL Server-autentisering eller Windows-autentisering. Om du väljer SQL Server-autentisering måste du ange hello befintliga autentiseringsuppgifter. Ange hello SQL Server-namn och hello namnet på hello-databasen som du vill toosync. Välj **Testanslutningen** tootest dina inställningar. Välj sedan **spara**. hello registrerade databasen visas i hello.

        ![SQL Server-databasen är nu registrerad](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. Nu kan du stänga hello Sync-Klientagenten app.

    12. I hello-portalen på hello **konfigurera lokalt** bladet väljer **Välj hello databasen.** Hej **Välj databas** blad öppnas.

    13. På hello **Välj databas** bladet i hello **Sync medlemsnamn** anger ett namn för hello nya sync medlemmen. Det här namnet skiljer sig från hello namnet på själva hello-databasen. Välj hello databasen hello-listan. I hello **Sync anvisningarna** väljer dubbelriktad synkronisering toohello hubben, eller från hello hubb.

        ![Välj hello på lokal databas](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. Välj **OK** tooclose hello **Välj databas** bladet. Välj sedan **OK** tooclose hello **konfigurera lokalt** bladet och vänta tills hello nya synkroniseras medlem toobe skapas och distribueras. Klicka slutligen på **OK** tooclose hello **Välj sync medlemmar** bladet.

        ![På en lokal databas som lagts till toosync grupp](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  tooconnect tooSQL datasynkronisering och hello lokal agent, lägger du till din användarroll namn toohello `DataSync_Executor`. Datasynkronisering skapar den här rollen på hello SQL Server-instansen.

## <a name="step-3---configure-sync-group"></a>Steg 3 – Konfigurera synkronisering grupp

När gruppmedlemmar hello nya sync skapas och distribueras, steg3 **Konfigurera synkronisering grupp**, markeras i hello **ny synkronisering grupp** bladet.

1.  På hello **tabeller** bladet Välj en databas från hello lista över medlemmar och välj sedan **Uppdatera schema**.

2.  Välj hello tabeller som du vill toosync hello listan över tillgängliga tabeller.

    ![Välj tabeller toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  Alla kolumner i tabellen hello är markerade som standard. Om du inte vill toosync alla hello kolumner, inaktivera hello kryssrutan för hello kolumner som du inte vill toosync. Kontrollera att väljas tooleave hello primärnyckelkolumnen.

    ![Välj fält toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Välj slutligen **spara**.

## <a name="next-steps"></a>Nästa steg
Grattis! Du har skapat en sync-grupp som innehåller både en instans av SQL Database och SQL Server-databasen.

För mer information om SQL Database och SQL-datasynkronisering, se:

-   [Hämta hello Fullständig datasynkronisering SQL teknisk dokumentation](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [Hämta hello SQL Data Sync REST API-dokumentation](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [Översikt över SQL-databas](sql-database-technical-overview.md)
-   [Livscykelhantering för databasen](https://msdn.microsoft.com/library/jj907294.aspx)
