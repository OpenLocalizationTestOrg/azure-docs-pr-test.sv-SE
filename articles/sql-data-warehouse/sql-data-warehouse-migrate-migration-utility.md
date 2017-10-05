---
title: 'Migrera: Datalager migrationsverktyget | Microsoft Docs'
description: Migrera till SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 4d7ad981-ef31-4513-b337-50bdc4709c09
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 2466e823c448ada4dc7bc5769b1b7f10bbb5dc7d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="data-warehouse-migration-utility-preview"></a><span data-ttu-id="cb0ed-103">Verktyg för migrering av data Warehouse (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="cb0ed-103">Data Warehouse Migration Utility (Preview)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="cb0ed-104">[Hämta verktyg för migrering][Download Migration Utility]</span><span class="sxs-lookup"><span data-stu-id="cb0ed-104">[Download Migration Utility][Download Migration Utility]</span></span>
> 
> 

<span data-ttu-id="cb0ed-105">Data Warehouse Migreringsverktyget är ett verktyg som utformats för att migrera schema och data från SQL Server och Azure SQL Database till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-105">The Data Warehouse Migration Utility is a tool designed to migrate schema and data from SQL Server and Azure SQL Database to Azure SQL Data Warehouse.</span></span> <span data-ttu-id="cb0ed-106">Under migreringen av schemat mappas verktyget automatiskt motsvarande schemat från källan till målet.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-106">During schema migration, the tool automatically maps the corresponding schema from source to destination.</span></span> <span data-ttu-id="cb0ed-107">När schemat har migrerats, verktyg ger möjlighet att flytta data med automatiskt genererade skript.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-107">After the schema has been migrated, the tools provides the option to move data with automatically generated scripts.</span></span>

<span data-ttu-id="cb0ed-108">Förutom schemat och datamigrering kan det här verktyget du välja att generera kompatibilitetsrapporter som sammanfattar inkompatibilitet mellan mål- och instanser som skulle kunna hindra effektiviserad migrering.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-108">In addition to schema and data migration, this tool gives you the option to generate compatibility reports which summarize incompatibilities between the target and source instances which would prevent streamlined migration.</span></span>

## <a name="get-started"></a><span data-ttu-id="cb0ed-109">Kom igång</span><span class="sxs-lookup"><span data-stu-id="cb0ed-109">Get started</span></span>
<span data-ttu-id="cb0ed-110">En förutsättning för installationen måste kommandoradsverktyget BCP att köra migreringsskript och Office för att visa kompatibilitetsrapporten.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-110">As a prerequisite for installation, you will need the BCP command-line utility to run migration scripts and Office to view the compatibility report.</span></span> <span data-ttu-id="cb0ed-111">När du startar den körbara filen som har hämtats uppmanas du att acceptera en standard EULA innan verktyget kommer att installeras.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-111">After launching the executable that is downloaded you will be prompted to accept a standard EULA before the tool will be installed.</span></span>

<span data-ttu-id="cb0ed-112">Om du vill köra Utiliy för migreringen, kommer du behöver det följande behörigheter på databasen som du vill migrera: CREATE DATABASE, ALTER ANY DATABASE eller ANY VYDEFINITIONEN.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-112">In addition, to run the Migration Utiliy, you will need the one following permissions on the database that you are looking to migrate: CREATE DATABASE, ALTER ANY DATABASE or VIEW ANY DEFINITION.</span></span>

### <a name="launching-the-tool-and-connecting"></a><span data-ttu-id="cb0ed-113">Starta verktyget och ansluta</span><span class="sxs-lookup"><span data-stu-id="cb0ed-113">Launching the tool and connecting</span></span>
<span data-ttu-id="cb0ed-114">Starta verktyget genom att klicka på ikonen på skrivbordet som visas efter installation.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-114">Launch the tool by clicking on the desktop icon which appears post install.</span></span> <span data-ttu-id="cb0ed-115">När verktyget öppnas, uppmanas du att med en initial anslutning sida där du kan välja källan och målet för Migreringsverktyget.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-115">Upon opening the tool, you will be prompted with an initial connection page where you can choose your source and destination for the migration tool.</span></span> <span data-ttu-id="cb0ed-116">För närvarande kan stöder vi SQL Server och Azure SQL Database som källor och SQL Data Warehouse som mål.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-116">At this time, we support SQL Server and Azure SQL Database as sources and SQL Data Warehouse as a destination.</span></span> <span data-ttu-id="cb0ed-117">När du har valt detta blir du ombedd att ansluta till källservern genom att fylla i servernamnet och autentisering och sedan klicka på ”Anslut”.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-117">After selecting this you will be asked to connect to your source server by filling in server name and authenticating and then clicking ‘Connect’.</span></span>

<span data-ttu-id="cb0ed-118">Efter autentisering, visas en lista över databaser som finns på den server som du är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-118">After authenticating, the tool will show a list of databases that are present in the server which you are connected to.</span></span> <span data-ttu-id="cb0ed-119">Du kan börja migreringen genom att välja en databas som du vill migrera och sedan klicka på 'Migrera valt'.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-119">You can begin the migration by selecting a database that you would like to migrate and then clicking on ‘Migrate selected’.</span></span>

## <a name="migration-report"></a><span data-ttu-id="cb0ed-120">Rapporten migrering</span><span class="sxs-lookup"><span data-stu-id="cb0ed-120">Migration report</span></span>
<span data-ttu-id="cb0ed-121">Att välja ”Kontrollera databasen kompatibilitet” i verktyget genererar en rapport som sammanfattar objektinkompatibiliteter för alla i databasen du begärt för att migrera.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-121">Selecting ‘Check Database Compatibility’ in the tool will generate a report summarizing all object incompatibilities in the database you requested to migrate.</span></span> <span data-ttu-id="cb0ed-122">En bredare lista över SQL Server-funktioner som inte finns i SQL Data Warehouse finns i vår [Migreringsdokumentation][migration documentation].</span><span class="sxs-lookup"><span data-stu-id="cb0ed-122">A broader list of some of the SQL Server functionality that is not present in SQL Data Warehouse can be found in our [migration documentation][migration documentation].</span></span> <span data-ttu-id="cb0ed-123">När rapporten har skapats kommer du att kunna spara och öppna rapporten i Excel.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-123">After the report is generated you will be able to save and open the report in Excel.</span></span>

<span data-ttu-id="cb0ed-124">Observera att när migreringen schemat, de flesta problem som har identifierats som 'Object' kommer att justeras för att tillåta att omedelbar datamigreringen.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-124">Please note that when generating the migration schema, most issues identified as ‘Object’ will be adjusted in order to allow immediate migration of that data.</span></span> <span data-ttu-id="cb0ed-125">Granska ändringar för att säkerställa att du inte vill göra ytterligare anpassningar innan du tillämpar schemat.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-125">Please review the changes to ensure you do not want to make additional adjustments before applying the schema.</span></span>

## <a name="migrate-schema"></a><span data-ttu-id="cb0ed-126">Migrera schema</span><span class="sxs-lookup"><span data-stu-id="cb0ed-126">Migrate schema</span></span>
<span data-ttu-id="cb0ed-127">Efter anslutning genererar välja migrera schemat ett skript för migrering av schemat för de markerade tabellerna.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-127">After connecting, selecting ‘Migrate Schema’ will generate a schema migration script for the selected tables.</span></span> <span data-ttu-id="cb0ed-128">Det här skriptet portar strukturen i tabellen, maps inkompatibla data typer till fler kompatibla formulär och skapar säkerhetsreferenser och schema om detta anges av användaren i migreringsinställningarna för.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-128">This script ports the structure of the table, maps incompatible data types to more compatible forms, and creates security credentials and schema if this is indicated by the user in the migration settings.</span></span> <span data-ttu-id="cb0ed-129">Den här koden kan köras mot den avsedda SQL Data Warehouse-databasinstansen, sparas i en fil, kopieras till Urklipp eller redigeras även i rad innan du vidtar åtgärder.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-129">This code can be run against the targeted SQL Data Warehouse instance, saved to a file, copied to your clipboard, or even edited in-line before taking further action.</span></span>  

<span data-ttu-id="cb0ed-130">Som beskrivs ovan, när du migrerar schemat granska migreringen ändringar som verktyget har gjort för att säkerställa att att du förstår dem..</span><span class="sxs-lookup"><span data-stu-id="cb0ed-130">As noted above, when migrating schema review the migration changes that the tool has made in order to ensure that that you fully understand them.</span></span>  

## <a name="migrate-data"></a><span data-ttu-id="cb0ed-131">Migrera data</span><span class="sxs-lookup"><span data-stu-id="cb0ed-131">Migrate data</span></span>
<span data-ttu-id="cb0ed-132">Genom att klicka på alternativet Migrera Data kan du generera BCP-skript som flyttar data först till flat-filer på servern, och sedan direkt till ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-132">By clicking the ‘Migrate Data’ option you can generate BCP scripts that will move your data first to flat files on your server, and then directly into your SQL Data Warehouse.</span></span> <span data-ttu-id="cb0ed-133">Vi rekommenderar den här processen för att flytta små mängder data, och som nya försök inte är inbyggda och fel kan uppstå om det finns en förlust av nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-133">We recommend this process for moving small amounts of data and, as retries are not built-in and failures may occur if there is a loss of the network connection.</span></span> <span data-ttu-id="cb0ed-134">För att kunna köra detta behöver du ha kommandoradsverktyget BCP installerat och att schemat innehåller data måste redan ha skapats.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-134">In order to run this, you will need to have the BCP command-line utility installed and the schema for the data must already have been created.</span></span>

<span data-ttu-id="cb0ed-135">När du har fyllt i parametrarna ovan behöver du helt enkelt klicka på Kör migrering och en uppsättning två paket kommer att genereras till den angivna platsen.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-135">After you have filled out the parameters above you simply need to click run migration and a set of two packages will be generated to your specified location.</span></span> <span data-ttu-id="cb0ed-136">Kör export-filen för att exportera data från din källa för migrering till flat-filer och kör importfilen för att importera data till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="cb0ed-136">Run the export file in order to export data from your migration source into flat files, and run the import file in order to import your data into SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb0ed-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb0ed-137">Next steps</span></span>
<span data-ttu-id="cb0ed-138">Nu när du har migrerat data, kolla hur man [utveckla][develop].</span><span class="sxs-lookup"><span data-stu-id="cb0ed-138">Now that you've migrated some data, check out how to [develop][develop].</span></span>

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
