---
title: "aaaMove data tooan Azure SQL Database för Azure Machine Learning | Microsoft Docs"
description: "Skapa SQL-tabell och hämta data tooSQL tabell"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 50f8b862-4d32-44b2-a1e2-4fbc8024acaa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: b33ef836f42c17a56794baf763281e9998b16bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooan-azure-sql-database-for-azure-machine-learning"></a>Flytta data tooan Azure SQL Database för Azure Machine Learning
Det här avsnittet beskriver hello alternativ för att flytta data från flata filer (CSV eller TVS format) eller data som lagras i en lokal SQL Server tooan Azure SQL database. Dessa uppgifter för glidande data toohello moln är en del av hello Team datavetenskap Process.

Ett avsnitt som visar hello alternativ för att flytta data tooan lokal SQL Server för Machine Learning finns [flytta data tooSQL Server på en virtuell dator i Azure](machine-learning-data-science-move-sql-server-virtual-machine.md).

hello följande **menyn** länkar tootopics som beskriver hur tooingest data till mål-miljöer där hello data kan lagras och behandlas under hello Team Data vetenskap processen (TDSP).

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

hello sammanfattas följande tabell hello alternativ för att flytta data tooan Azure SQL Database.

| <b>KÄLLA</b> | <b>MÅL: Azure SQL-databas</b> |
| --- | --- |
| <b>Flat-fil (CSV eller TVS formaterad)</b> |<a href="#bulk-insert-sql-query">Bulk Insert SQL-fråga |
| <b>Lokal SQLServer</b> |1. <a href="#export-flat-file">Exportera tooFlat fil<br> 2. <a href="#insert-tables-bcp">Migreringsguiden för SQL-databas<br> 3. <a href="#db-migration">Databasen tillbaka in och återställa<br> 4. <a href="#adf">Azure Data Factory |

## <a name="prereqs"></a>Förhandskrav
hello-procedurer som beskrivs här måste du ha:

* En **Azure-prenumeration**. Om du inte har någon prenumeration kan du registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* En **Azure storage-konto**. Du kan använda ett Azure storage-konto för att lagra hello data i den här självstudiekursen. Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) artikel. När du har skapat hello storage-konto behöver du tooobtain hello konto nyckel som används för tooaccess hello lagring. Se [hantera åtkomstnycklar för lagring](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Åtkomst tooan **Azure SQL Database**. Om du måste ställa in en Azure SQL Database [komma igång med Microsoft Azure SQL Database](../sql-database/sql-database-get-started.md) innehåller information om hur tooprovision en ny instans av en Azure SQL Database.
* Installerat och konfigurerat **Azure PowerShell** lokalt. Instruktioner finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

**Data**: hello migrering processer kan visas med hjälp av hello [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/). hello NYC Taxi datamängden innehåller information om resa data och mässor och är tillgängligt på Azure-blobblagring: [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/). Ett exempel och en beskrivning av dessa filer finns i [NYC Taxi resor Dataset beskrivning](machine-learning-data-science-process-sql-walkthrough.md#dataset).

Du kan anpassa hello förfaranden som beskrivs här tooa uppsättning dina egna data eller hello gör enligt med hjälp av hello NYC Taxi dataset. tooupload hello NYC Taxi dataset i dina lokala SQL Server-databas, följ hello proceduren som beskrivs i [Bulk importera Data till SQL Server-databas](machine-learning-data-science-process-sql-walkthrough.md#dbload). Dessa instruktioner är för en SQL Server på en virtuell dator i Azure, men hello proceduren för att ladda upp toohello på lokal SQL Server är hello samma.

## <a name="file-to-azure-sql-database"></a>Flytta data från en flat fil källa tooan Azure SQL-databas
Data i flat-filer (CSV eller TVS formaterad) kan vara flyttade tooan Azure SQL database med en Bulk Insert SQL-fråga.

### <a name="bulk-insert-sql-query"></a>Bulk Insert SQL-fråga
hello stegen för hello proceduren med hjälp av hello Bulk Insert SQL-frågan är liknande toothose upp i hello avsnitt för att flytta data från en flat fil källa tooSQL Server på en Azure VM. Mer information finns i [Bulk Insert SQL-frågan](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).

## <a name="sql-on-prem-to-sazure-sql-database"></a>Flytta Data från lokala SQL Server tooan Azure SQL-databas
Om hello källdata lagras i en lokal SQL Server, finns det olika möjligheter för glidande hello data tooan Azure SQL-databas:

1. [Exportera tooFlat fil](#export-flat-file)
2. [Migreringsguiden för SQL-databas](#insert-tables-bcp)
3. [Databasen tillbaka in och återställa](#db-migration)
4. [Azure Data Factory](#adf)

hello stegen för hello först tre är mycket lik toothose avsnitt i [flytta data tooSQL Server på en virtuell dator i Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) som omfattar samma sätt. Länkar toohello relevanta avsnitt i det avsnittet finns i hello följa anvisningar.

### <a name="export-flat-file"></a>Exportera tooFlat fil
hello stegen för den här exporteras tooa flat fil är liknande toothose som beskrivs i [exportera tooFlat filen](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

### <a name="insert-tables-bcp"></a>Migreringsguiden för SQL-databas
hello steg för att använda hello Migreringsguiden för SQL-databasen är liknande toothose som beskrivs i [Migreringsguiden för SQL-databasen](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

### <a name="db-migration"></a>Databasen tillbaka in och återställa
hello steg för att använda databasen säkerhetskopiera och Återställ är liknande toothose som beskrivs i [databasen tillbaka in och återställa](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

### <a name="adf"></a>Azure Data Factory
hello proceduren för att flytta data tooan Azure SQL-databas med Azure Data Factory (ADM) finns i avsnittet hello [flytta data från en lokal SQL server tooSQL Azure med Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md). Det här avsnittet visar hur toomove data från en lokal SQL Server-databasen tooan Azure SQL database via Azure Blob Storage med hjälp av ADF.

Överväg att använda ADF när data måste toobe migreras kontinuerligt i ett hybridscenario som har åtkomst till både lokalt och molnresurser och när hello data är överförd eller måste toobe ändras eller har affärslogik läggs till tooit när migreras. ADF tillåter hello schemaläggning och övervakning av jobb med hjälp av enkla JSON-skript som hanterar hello flödet av data regelbundet. ADF har även andra funktioner som stöd för komplex.
