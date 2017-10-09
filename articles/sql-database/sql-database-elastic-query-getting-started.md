---
title: "aaaReport över utskalat moln databaser (horisontell partitionering) | Microsoft Docs"
description: "hur toouse mellan databasfrågor för databasen"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: c81ef5e3-41e9-4fd2-8631-868f2e168147
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: mlandzic
ms.openlocfilehash: e34f398f8d408cffd91a70fc2cfbda73daec3550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a>Rapporter över databaser som skalats ut molnet (förhandsgranskning)
Du kan skapa rapporter från flera Azure SQL-databaser från en enda anslutning med en [elastisk frågan](sql-database-elastic-query-overview.md). hello databaser måste vara partitionerade vågrätt (även kallat ”delat”).

Om du har en befintlig databas, se [migrera befintliga databaser tooscaled ut databaser](sql-database-elastic-convert-to-use-elastic-tools.md).

toounderstand hello SQL-objekt krävs tooquery, se [fråga över vågrätt partitionerade databaser](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="prerequisites"></a>Krav
Hämta och köra hello [komma igång med elastisk databas verktyg exempel](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a>Skapa en Fragmentera karta manager med hello sample-appen
Här skapar du en karta Fragmentera manager tillsammans med flera delar, följt av infogning av data i hello delar. Om du råkar tooalready har shards installation med shardade data, kan du hoppa över följande hello och flytta toohello nästa avsnitt.

1. Skapa och köra hello **komma igång med elastiska Databasverktyg** exempelprogrammet. Gör hello förrän steg 7 i hello avsnittet [hämta och köra hello exempelapp](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Hello slutet av steg 7 visas hello följande kommandotolk:

    ![kommandotolk][1]
2. Skriv ”1” i hello-kommandotolk och tryck på **RETUR**. Detta skapar hello Fragmentera kartan manager och lägger till två delar toohello server. Sedan skriver ”3” och tryck på **RETUR**; Upprepa hello åtgärd fyra gånger. Detta infogar exempel datarader i din shards.
3. Hej [Azure-portalen](https://portal.azure.com) ska visa tre nya databaser på servern:

   ![Visual Studio-bekräftelse][2]

   Nu stöds mellan databasfrågor via hello klientbibliotek för elastisk databas. Till exempel använda alternativ 4 i hello kommandofönstret. hello resultat från en fråga med flera Fragmentera är alltid en **UNION ALL** hello resultat från alla delar.

   I nästa avsnitt om hello skapa vi en exempel-databasen slutpunkt som har stöd för bättre frågor till hello data över shards.

## <a name="create-an-elastic-query-database"></a>Skapa en fråga för elastisk databas
1. Öppna hello [Azure-portalen](https://portal.azure.com) och logga in.
2. Skapa en ny Azure SQL-databas i hello samma server som Fragmentera-installationen. Databasen för hello ”ElasticDBQuery”.

    ![Azure-portalen och prisnivå][3]

    > [!NOTE]
    > Du kan använda en befintlig databas. Om du gör det, den får inte vara en hello delar som du vill att tooexecute dina frågor på. Den här databasen används för att skapa hello metadataobjekt för en elastisk databas-fråga.
    >

## <a name="create-database-objects"></a>Skapa databasobjekt
### <a name="database-scoped-master-key-and-credentials"></a>Databas-omfattande huvudnyckel och autentiseringsuppgifter
Dessa är används tooconnect toohello Fragmentera kartan manager och hello delar:

1. Öppna SQL Server Management Studio eller SQL Server Data Tools i Visual Studio.
2. Ansluta tooElasticDBQuery databasen och kör följande T-SQL-kommandon hello:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    ”användarnamn” och ”password” ska vara hello samma som de inloggningsuppgifter som används i steg 6 i [hämta och köra hello exempelapp](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) i [komma igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).

### <a name="external-data-sources"></a>Externa datakällor
toocreate en extern datakälla, kör följande kommando på hello ElasticDBQuery databasen hello:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 ”CustomerIDShardMap” är hello Fragmentera kartan hello namn om du har skapat hello Fragmentera karta och Fragmentera kartan manager använder hello elastisk databas verktyg exemplet. Om du har använt din anpassade konfiguration för det här exemplet bör sedan det dock hello Fragmentera mappningsnamn som du valde i ditt program.

### <a name="external-tables"></a>Externa tabeller
Skapa en extern tabell som matchar hello kunder tabellen på hello shards genom att köra följande kommando på ElasticDBQuery databasen hello:

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Köra en exempelfråga för elastisk databas T-SQL
När du har definierat den externa datakällan och externa tabeller kan du nu använda fullständig T-SQL via externa tabeller.

Kör frågan på hello ElasticDBQuery databasen:

    select count(CustomerId) from [dbo].[Customers]

Du kommer att märka hello frågan aggregerar resultat från alla hello shards och ger hello följande utdata:

![Information för utdata][4]

## <a name="import-elastic-database-query-results-tooexcel"></a>Importera elastisk databas frågan resultat tooExcel
 Du kan importera hello resultaten från av en fråga tooan Excel-fil.

1. Starta Excel 2013.
2. Navigera toohello **Data** menyfliksområdet.
3. Klicka på **från andra källor** och på **från SQL Server**.

   ![Excel-import från andra källor][5]
4. I hello **Dataanslutningsguiden** skriver hello server namnet och autentiseringsuppgifterna för inloggning. Klicka sedan på **Nästa**.
5. I dialogrutan för hello **väljer hello-databas som innehåller hello data som du vill**väljer hello **ElasticDBQuery** databas.
6. Välj hello **kunder** tabell i hello listvyn och klicka på **nästa**. Klicka på **Slutför**.
7. I hello **importera Data** formuläret under **väljer du hur tooview dessa data i arbetsboken**väljer **tabell** och på **OK**.

Alla hello rader från **kunder** tabell som sparas i olika delar fylla hello Excel-blad.

Du kan nu använda Excel-kraftfulla visualiseringen. Du kan använda hello anslutningssträngen med servernamnet, databasnamnet och autentiseringsuppgifter tooconnect BI och data integration verktyg toohello elastisk fråga databasen. Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver. Du kan referera toohello elastisk fråga databas och externa tabeller precis som andra SQL Server-databas och att du vill ansluta toowith verktyget du SQL Server-tabeller.

### <a name="cost"></a>Kostnad
Det finns utan extra kostnad för hello elastisk databasfrågan funktionen.

Mer information om priser finns [prisinformation för SQL-databasen](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Nästa steg

* En översikt över elastisk fråga, se [elastisk frågan översikt](sql-database-elastic-query-overview.md).
* Vertikal partitionering, finns [komma igång med flera databaser fråga (vertikal partitionering)](sql-database-elastic-query-getting-started-vertical.md).
* Syntax och exempel frågor för lodrätt partitionerade finns [frågar lodrätt partitionerad data)](sql-database-elastic-query-vertical-partitioning.md)
* Syntax och exempel frågor för vågrätt partitionerade finns [frågar vågrätt partitionerad data)](sql-database-elastic-query-horizontal-partitioning.md)
* Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
