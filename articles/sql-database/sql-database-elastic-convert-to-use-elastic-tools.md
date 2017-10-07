---
title: aaaMigrate befintliga databaser tooscale ut | Microsoft Docs
description: Konvertera delat databaser toouse elastisk Databasverktyg genom att skapa en Fragmentera kartan manager
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 8c851d8e-8fd5-4327-89c1-9178b20ddd69
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: fa2c9e3699f30667cf547d1faadf4504609199be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-existing-databases-tooscale-out"></a>Migrera befintliga databaser tooscale ut
Enkelt hantera dina befintliga delat utskalat-databaser som använder Azure SQL Database databaser (till exempel hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md)). Först måste du konvertera en befintlig uppsättning databaser toouse hello [Fragmentera kartan manager](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Översikt
toomigrate en befintlig delat databas: 

1. Förbereda hello [Fragmentera kartan manager-databasen](sql-database-elastic-scale-shard-map-management.md).
2. Skapa hello Fragmentera karta.
3. Förbered hello enskilda delar.  
4. Lägga till mappningar toohello Fragmentera karta.

Dessa tekniker kan implementeras med hjälp av antingen hello [klientbiblioteket för .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), eller hello PowerShell-skript som finns på [Azure SQL DB - skript för elastisk databas verktyg](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). här hello exemplen använder hello PowerShell-skript.

Läs mer om hello ShardMapManager [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md). En översikt över hello elastisk Databasverktyg finns [elastisk databas funktionsöversikt](sql-database-elastic-scale-introduction.md).

## <a name="prepare-hello-shard-map-manager-database"></a>Förbereda hello Fragmentera kartan manager-databasen
hello Fragmentera kartan manager är en särskild databas som innehåller hello data toomanage utskalat databaser. Du kan använda en befintlig databas eller skapa en ny databas. Observera att en databas som fungerar som Fragmentera kartan manager inte får vara hello samma databas som en Fragmentera. Observera också att hello PowerShell-skript inte skapar hello databasen för dig. 

## <a name="step-1-create-a-shard-map-manager"></a>Steg 1: skapa en Fragmentera kartan manager
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are hello server name and database name 
    # for hello new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="tooretrieve-hello-shard-map-manager"></a>tooretrieve hello Fragmentera kartan manager
När den har skapats, kan du hämta hello Fragmentera kartan manager med denna cmdlet. Det här steget krävs varje gång du behöver toouse hello ShardMapManager objekt.

    # Try tooget a reference toohello Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-hello-shard-map"></a>Steg 2: skapa hello Fragmentera karta
Du måste ange hello Fragmentera kartan toocreate. hello dig beror på hello database-arkitektur: 

1. En organisation per databas (villkor, finns hello [ordlista](sql-database-elastic-scale-glossary.md).) 
2. Flera klienter per databas (två typer):
   1. Lista över-mappning
   2. Mappning av intervallet

En enskild klient-modell, skapa en **lista mappning** Fragmentera kartan. modell för hello single-klienter tilldelas en databas per klient. Detta är en effektiv modell för SaaS-utvecklare som det förenklar hanteringen.

![Lista över-mappning][1]

hello modell för flera klienter tilldelas flera innehavare tooa enskild databas (och du kan distribuera grupper av klienter över flera databaser). Använd den här modellen när du förväntar dig varje klient toohave små databehov. I den här modellen vi tilldela ett intervall med klienter tooa en databas med hjälp av **intervallet mappning**. 

![Mappning av intervallet][2]

Eller du kan implementera en databas för flera innehavare modellen med hjälp av en *lista mappning* tooassign flera innehavare tooa enskild databas. Till exempel DB1 är används toostore information om klient-id 1 och 5 och DB2 lagrar data för klienten 7 och 10-klient. 

![Muliple klienter i samma DB][3] 

**Baserat på ditt val, välja ett av följande alternativ:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Alternativ 1: skapa en Fragmentera karta för en lista över-mappning
Skapa en Fragmentera karta med hello ShardMapManager-objektet. 

    # $ShardMapManager is hello shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Alternativ 2: skapa en Fragmentera karta för mappning av intervallet
Observera att tooutilize det här mönstret för mappning, klient-ID-värden måste toobe kontinuerlig intervall och det är acceptabel toohave lucka i hello intervall genom att helt enkelt hoppa hello intervallet när du skapar hello databaser.

    # $ShardMapManager is hello shard map manager object 
    # 'RangeShardMap' is hello unique identifier for hello range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Alternativ 3: Visa en lista över mappningar på en enskild databas
Ställa in det här mönstret kräver också att skapa en karta i listan som visas i steg 2, alternativ 1.

## <a name="step-3-prepare-individual-shards"></a>Steg 3: Förbered enskilda delar
Lägg till varje Fragmentera (databasen) toohello Fragmentera kartan manager. Detta förbereder hello enskilda databaser för att lagra mappningsinformation. Köra den här metoden på varje Fragmentera.

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # hello $ShardMap is hello shard map created in step 2.


## <a name="step-4-add-mappings"></a>Steg 4: Lägga till mappningar
hello lägga till mappningar beror på hello slags Fragmentera karta som du skapade. Om du har skapat en karta i listan kan du lägga till listan mappningar. Om du har skapat en karta intervallet du lägga till mappningar för intervallet.

### <a name="option-1-map-hello-data-for-a-list-mapping"></a>Alternativ 1: mappa hello data för en lista över-mappning
Mappa hello data genom att lägga till en lista över-mappning för varje klient.  

    # Create hello mappings and associate it with hello new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-hello-data-for-a-range-mapping"></a>Alternativ 2: mappa hello data för mappning av intervallet
Lägg till hello intervallet mappningar för alla hello klient-id-intervallet - kopplingarna till databasen:

    # Create hello mappings and associate it with hello new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-hello-data-for-multiple-tenants-on-a-single-database"></a>Steg 4 alternativ 3: mappa hello data för flera klienter på en enskild databas
För varje klient kör hello Lägg till ListMapping (alternativ 1, ovan). 

## <a name="checking-hello-mappings"></a>Kontrollera hello mappningar
Information om hello befintliga delar och hello mappningar som är associerade med dem kan efterfrågas med följande kommandon:  

    # List hello shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Sammanfattning
När du har slutfört hello installationen, kan du börja toouse hello-klientbibliotek för elastisk databas. Du kan också använda [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md) och [flera Fragmentera frågan](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Nästa steg
Hämta hello PowerShell-skript från [Azure SQL DB-elastiska Database tools sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

hello verktyg finns även på GitHub: [Azure/elastisk-db-tools](https://github.com/Azure/elastic-db-tools).

Använd hello dela merge tool toomove data tooor från en modell för flera klienter tooa enda innehavare modell. Se [dela merge tool](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Ytterligare resurser
Information om vanliga mönster för dataarkitekturen i SaaS-databasprogram (Software-as-a-Service) med flera klienter finns i [Designmönster för SaaS-program med flera klienter i Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Frågor och Funktionsbegäranden
För frågor kan du nå toous på hello [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) och för funktionsbegäranden, Lägg till dem toohello [SQL-databas Feedbackforum](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

