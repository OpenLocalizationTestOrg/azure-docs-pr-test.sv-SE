---
title: "aaaCopy data från Blob Storage tooSQL databasen - Azure | Microsoft Docs"
description: "Den här kursen visar hur toouse Kopieringsaktiviteten i ett Azure Data Factory pipeline toocopy data från Blob storage tooSQL databas."
keywords: BLOB-sql, blob storage, kopiering av data
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: a2c3fb8a4ddd63b0b6b3e75903b7a7eaf188fda4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-copy-data-from-blob-storage-toosql-database-using-data-factory"></a>Självstudier: Kopiera data från Blob Storage tooSQL databasen med hjälp av Data Factory
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Guiden Kopiera](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

I kursen får skapa du en datafabrik med en rörledning toocopy data från Blob storage tooSQL databas.

Hej Kopieringsaktiviteten utför hello dataflyttning i Azure Data Factory. Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt. Se [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln för information om hello Kopieringsaktiviteten.  

> [!NOTE]
> En detaljerad översikt över hello Data Factory-tjänsten finns hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel.
>
>

## <a name="prerequisites-for-hello-tutorial"></a>Förutsättningar för självstudiekursen hello
Innan du börjar den här kursen måste du ha hello följande krav:

* **Azure-prenumeration**.  Om du inte har någon Azure-prenumeration kan du skapa ett kostnadsfritt konto på ett par minuter. Se hello [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/) artikeln för information.
* **Azure Storage-konto**. Du använder hello blob storage som en **källa** datalager i den här självstudiekursen. Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toocreate i steg en artikel.
* **Azure SQL Database**. Du använder en Azure SQL-databas som en **mål** datalager i den här självstudiekursen. Om du inte har en Azure SQL-databas som du kan använda i hello självstudien finns [hur toocreate och konfigurera en Azure SQL Database](../sql-database/sql-database-get-started.md) toocreate en.
* **2014-SQL Server 2012 eller Visual Studio 2013**. Du använder SQL Server Management Studio eller Visual Studio toocreate en exempeldatabas och tooview hello Resultatdata i hello-databasen.  

## <a name="collect-blob-storage-account-name-and-key"></a>Samla in blob storage-kontonamnet och nyckeln
Du behöver hello konto namn och åtkomstnyckel för ditt Azure storage-konto toodo den här kursen. Notera **kontonamn** och **kontonyckel** för Azure storage-konto.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Klicka på **fler tjänster** på hello vänster-menyn och välj **Lagringskonton**.

    ![Bläddra - Storage-konton](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. I hello **Lagringskonton** bladet, Välj hello **Azure storage-konto** som du vill toouse i den här självstudiekursen.
4. Välj **åtkomstnycklar** länken under **inställningar**.
5. Klicka på **kopiera** (bild) bredvid knappen för**lagringskontonamnet** text rutan och spara och klistra in den någonstans (till exempel: i en textfil).
6. Upprepa föregående steg toocopy för hello eller Skriv ner hello **key1**.

    ![Lagringsåtkomstnyckel](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. Stäng alla hello blad genom att klicka på **X**.

## <a name="collect-sql-server-database-user-names"></a>Samla in SQLServer, databas, användarnamn
Hello namnen på Azure SQL server-databasen och användaren toodo måste den här kursen. Skriv ner namnen på **server**, **databasen**, och **användaren** för din Azure SQL-databas.

1. I hello **Azure-portalen**, klickar du på **fler tjänster** på hello vänster och välj **SQL-databaser**.
2. I hello **SQL-databaser bladet**väljer hello **databasen** som du vill toouse i den här självstudiekursen. Skriv ner hello **databasnamnet**.  
3. I hello **SQL-databas** bladet, klickar du på **egenskaper** under **inställningar**.
4. Skriv ner hello värden för **servernamn** och **inloggning för SERVERADMINISTRATÖR**.
5. Stäng alla hello blad genom att klicka på **X**.

## <a name="allow-azure-services-tooaccess-sql-server"></a>Tillåt Azure-tjänster tooaccess SQLServer
Se till att **och ge åtkomst tooAzure tjänster** inställningen aktiverade **på** för Azure SQL-servern så att hello Data Factory-tjänsten har åtkomst till Azure SQL-servern. tooverify och aktivera den här inställningen hello följande steg:

1. Klicka på **fler tjänster** hubb hello vänster och klicka på **SQL-servrar**.
2. Välj din server och klicka på **Brandvägg** under **INSTÄLLNINGAR**.
3. I hello **brandväggsinställningar** bladet, klickar du på **ON** för **och ge åtkomst tooAzure tjänster**.
4. Stäng alla hello blad genom att klicka på **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Förbereda Blob Storage och SQL-databas
Nu kan förbereda Azure blob storage och Azure SQL-databas för hello kursen genom att utföra följande steg hello:  

1. Öppna Anteckningar. Kopiera hello följande text och spara den som **emp.txt** för**C:\ADFGetStarted** mapp på hårddisken.

    ```
    John, Doe
    Jane, Doe
    ```
2. Använd verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/) toocreate hello **adftutorial** behållare och tooupload hello **emp.txt** toohello filbehållare.

    ![Azure Lagringsutforskaren. Kopiera data från Blob storage tooSQL databas](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Använd hello följande SQL-skript toocreate hello **tomma** tabell i Azure SQL-databasen.  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    **Om du har SQL Server 2012 2014 installerad på datorn:** följer du anvisningarna från [hantera Azure SQL Database med SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) tooconnect tooyour Azure SQL server och kör hello SQL-skript. Den här artikeln använder hello [klassiska Azure-portalen](http://manage.windowsazure.com), inte hello [nya Azure-portalen](https://portal.azure.com), tooconfigure Brandvägg för en Azure SQL-server.

    Om klienten inte är tillåtet tooaccess hello Azure SQL-server behöver du tooconfigure Brandvägg för din Azure SQL server tooallow åtkomst från din dator (IP-adress). Se [i den här artikeln](../sql-database/sql-database-configure-firewall-settings.md) för steg tooconfigure hello-brandväggen för din Azure SQL-server.

## <a name="create-a-data-factory"></a>Skapa en datafabrik
Du har slutfört hello krav. Du kan skapa en datafabrik med någon av följande sätt hello. Klicka på en hello alternativ i hello listrutan på hello top eller hello följande länkar tooperform hello kursen.     

* [Guiden Kopiera](data-factory-copy-data-wizard-tutorial.md)
* [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
* [Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
* [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data. Det inte transformera indata tooproduce utdata. En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa din första pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).
> 
> Du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory). 
