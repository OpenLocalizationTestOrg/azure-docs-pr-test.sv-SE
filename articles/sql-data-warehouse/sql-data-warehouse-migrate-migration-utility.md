---
title: 'Migrera: Datalager migrationsverktyget | Microsoft Docs'
description: Migrera tooSQL Data Warehouse.
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
ms.openlocfilehash: c89909883fb42b0b04dd87a9973e5ee3e30d8f0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-warehouse-migration-utility-preview"></a><span data-ttu-id="3a900-103">Verktyg för migrering av data Warehouse (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="3a900-103">Data Warehouse Migration Utility (Preview)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="3a900-104">[Hämta verktyg för migrering][Download Migration Utility]</span><span class="sxs-lookup"><span data-stu-id="3a900-104">[Download Migration Utility][Download Migration Utility]</span></span>
> 
> 

<span data-ttu-id="3a900-105">hello Migreringsverktyget för Data Warehouse är ett verktyg som utformats toomigrate schema och data från SQL Server och Azure SQL Database tooAzure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3a900-105">hello Data Warehouse Migration Utility is a tool designed toomigrate schema and data from SQL Server and Azure SQL Database tooAzure SQL Data Warehouse.</span></span> <span data-ttu-id="3a900-106">Under migreringen av schemat mappas hello verktyget automatiskt hello motsvarande schema från källan toodestination.</span><span class="sxs-lookup"><span data-stu-id="3a900-106">During schema migration, hello tool automatically maps hello corresponding schema from source toodestination.</span></span> <span data-ttu-id="3a900-107">När hello schema har migrerats, tillhandahåller hello verktyg hello alternativet toomove data med automatiskt genererade skript.</span><span class="sxs-lookup"><span data-stu-id="3a900-107">After hello schema has been migrated, hello tools provides hello option toomove data with automatically generated scripts.</span></span>

<span data-ttu-id="3a900-108">Dessutom effektiv tooschema och datamigrering verktyget kan du hello alternativet toogenerate kompatibilitetsrapporter som sammanfattar inkompatibilitet mellan hello mål- och instanser som skulle kunna hindra migrering.</span><span class="sxs-lookup"><span data-stu-id="3a900-108">In addition tooschema and data migration, this tool gives you hello option toogenerate compatibility reports which summarize incompatibilities between hello target and source instances which would prevent streamlined migration.</span></span>

## <a name="get-started"></a><span data-ttu-id="3a900-109">Kom igång</span><span class="sxs-lookup"><span data-stu-id="3a900-109">Get started</span></span>
<span data-ttu-id="3a900-110">En förutsättning för installationen måste hello BCP kommandoradsverktyget toorun migreringsskript och Office tooview hello Kompatibilitetsrapport.</span><span class="sxs-lookup"><span data-stu-id="3a900-110">As a prerequisite for installation, you will need hello BCP command-line utility toorun migration scripts and Office tooview hello compatibility report.</span></span> <span data-ttu-id="3a900-111">När du startar hello kommer körbar fil som hämtas du att tillfrågas tooaccept standard LICENSAVTALET innan hello verktyget kommer att installeras.</span><span class="sxs-lookup"><span data-stu-id="3a900-111">After launching hello executable that is downloaded you will be prompted tooaccept a standard EULA before hello tool will be installed.</span></span>

<span data-ttu-id="3a900-112">Dessutom toorun hello migrering Utiliy du kommer måste hello en följande behörigheter på hello databasen söker toomigrate: CREATE DATABASE, ALTER ANY DATABASE eller ANY VYDEFINITIONEN.</span><span class="sxs-lookup"><span data-stu-id="3a900-112">In addition, toorun hello Migration Utiliy, you will need hello one following permissions on hello database that you are looking toomigrate: CREATE DATABASE, ALTER ANY DATABASE or VIEW ANY DEFINITION.</span></span>

### <a name="launching-hello-tool-and-connecting"></a><span data-ttu-id="3a900-113">Starta verktyget hello och ansluta</span><span class="sxs-lookup"><span data-stu-id="3a900-113">Launching hello tool and connecting</span></span>
<span data-ttu-id="3a900-114">Starta hello verktyget genom att klicka på hello skrivbordsikon som visas efter installera.</span><span class="sxs-lookup"><span data-stu-id="3a900-114">Launch hello tool by clicking on hello desktop icon which appears post install.</span></span> <span data-ttu-id="3a900-115">När öppnas hello verktyget, uppmanas du att med en initial anslutning sida där du kan välja källan och målet för hello Migreringsverktyget.</span><span class="sxs-lookup"><span data-stu-id="3a900-115">Upon opening hello tool, you will be prompted with an initial connection page where you can choose your source and destination for hello migration tool.</span></span> <span data-ttu-id="3a900-116">För närvarande kan stöder vi SQL Server och Azure SQL Database som källor och SQL Data Warehouse som mål.</span><span class="sxs-lookup"><span data-stu-id="3a900-116">At this time, we support SQL Server and Azure SQL Database as sources and SQL Data Warehouse as a destination.</span></span> <span data-ttu-id="3a900-117">När du har valt det här blir du ombedd tooconnect tooyour källservern genom att fylla i servernamnet och autentisering och sedan klicka på ”Anslut”.</span><span class="sxs-lookup"><span data-stu-id="3a900-117">After selecting this you will be asked tooconnect tooyour source server by filling in server name and authenticating and then clicking ‘Connect’.</span></span>

<span data-ttu-id="3a900-118">Efter autentisering, visas hello-verktyget en lista över databaser som finns i hello-server som du är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="3a900-118">After authenticating, hello tool will show a list of databases that are present in hello server which you are connected to.</span></span> <span data-ttu-id="3a900-119">Du kan börja hello migreringen genom att välja en databas som du vill att toomigrate och sedan klicka på 'Migrera valt'.</span><span class="sxs-lookup"><span data-stu-id="3a900-119">You can begin hello migration by selecting a database that you would like toomigrate and then clicking on ‘Migrate selected’.</span></span>

## <a name="migration-report"></a><span data-ttu-id="3a900-120">Rapporten migrering</span><span class="sxs-lookup"><span data-stu-id="3a900-120">Migration report</span></span>
<span data-ttu-id="3a900-121">Att välja ”Kontrollera databasen kompatibilitet' i hello verktyget kommer Generera en rapport som sammanfattar objektinkompatibiliteter för alla i databasen hello begärda toomigrate.</span><span class="sxs-lookup"><span data-stu-id="3a900-121">Selecting ‘Check Database Compatibility’ in hello tool will generate a report summarizing all object incompatibilities in hello database you requested toomigrate.</span></span> <span data-ttu-id="3a900-122">En bredare lista över hello SQL Server-funktioner som inte finns i SQL Data Warehouse finns i vår [Migreringsdokumentation][migration documentation].</span><span class="sxs-lookup"><span data-stu-id="3a900-122">A broader list of some of hello SQL Server functionality that is not present in SQL Data Warehouse can be found in our [migration documentation][migration documentation].</span></span> <span data-ttu-id="3a900-123">Du kommer att kunna toosave och öppna hello rapport i Excel när hello rapporten genereras.</span><span class="sxs-lookup"><span data-stu-id="3a900-123">After hello report is generated you will be able toosave and open hello report in Excel.</span></span>

<span data-ttu-id="3a900-124">Observera att när du genererar hello migrering schema, de flesta problem som har identifierats som 'Object' justeras i ordning tooallow omedelbar migrering av data.</span><span class="sxs-lookup"><span data-stu-id="3a900-124">Please note that when generating hello migration schema, most issues identified as ‘Object’ will be adjusted in order tooallow immediate migration of that data.</span></span> <span data-ttu-id="3a900-125">Granska hello ändringar tooensure du inte vill toomake ytterligare justeringar innan du tillämpar hello schemat.</span><span class="sxs-lookup"><span data-stu-id="3a900-125">Please review hello changes tooensure you do not want toomake additional adjustments before applying hello schema.</span></span>

## <a name="migrate-schema"></a><span data-ttu-id="3a900-126">Migrera schema</span><span class="sxs-lookup"><span data-stu-id="3a900-126">Migrate schema</span></span>
<span data-ttu-id="3a900-127">Efter anslutning genererar välja migrera schemat ett schema migrering skript för hello markerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="3a900-127">After connecting, selecting ‘Migrate Schema’ will generate a schema migration script for hello selected tables.</span></span> <span data-ttu-id="3a900-128">Det här skriptet portar hello struktur hello tabell mappar inkompatibla data typer toomore kompatibel formulär och skapar säkerhetsreferenser och schema om detta anges av hello användare i hello migreringsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="3a900-128">This script ports hello structure of hello table, maps incompatible data types toomore compatible forms, and creates security credentials and schema if this is indicated by hello user in hello migration settings.</span></span> <span data-ttu-id="3a900-129">Den här koden kan köras mot hello riktade SQL Data Warehouse-instans, sparade tooa filen, kopieras tooyour Urklipp eller redigeras även i rad innan du vidtar åtgärder.</span><span class="sxs-lookup"><span data-stu-id="3a900-129">This code can be run against hello targeted SQL Data Warehouse instance, saved tooa file, copied tooyour clipboard, or even edited in-line before taking further action.</span></span>  

<span data-ttu-id="3a900-130">Som beskrivs ovan när migrera schemat granska hello migrering ändrar den hello verktyget har gjort i ordning tooensure som att du förstår dem..</span><span class="sxs-lookup"><span data-stu-id="3a900-130">As noted above, when migrating schema review hello migration changes that hello tool has made in order tooensure that that you fully understand them.</span></span>  

## <a name="migrate-data"></a><span data-ttu-id="3a900-131">Migrera data</span><span class="sxs-lookup"><span data-stu-id="3a900-131">Migrate data</span></span>
<span data-ttu-id="3a900-132">Genom att klicka på alternativet för hello migrera Data kan du generera BCP-skript som flyttar dina första tooflat filer på servern, och sedan direkt till ditt SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3a900-132">By clicking hello ‘Migrate Data’ option you can generate BCP scripts that will move your data first tooflat files on your server, and then directly into your SQL Data Warehouse.</span></span> <span data-ttu-id="3a900-133">Vi rekommenderar den här processen för att flytta små mängder data, och som nya försök inte är inbyggda och fel kan uppstå om det finns en förlust av hello nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="3a900-133">We recommend this process for moving small amounts of data and, as retries are not built-in and failures may occur if there is a loss of hello network connection.</span></span> <span data-ttu-id="3a900-134">I ordning toorun detta, behöver du toohave hello kommandoradsverktyget BCP installerat och hello schemat för hello data måste redan ha skapats.</span><span class="sxs-lookup"><span data-stu-id="3a900-134">In order toorun this, you will need toohave hello BCP command-line utility installed and hello schema for hello data must already have been created.</span></span>

<span data-ttu-id="3a900-135">När du har fyllt i hello parametrar ovan du bara behöver tooclick kör migrering och en uppsättning två paket kommer att skapas tooyour angivna plats.</span><span class="sxs-lookup"><span data-stu-id="3a900-135">After you have filled out hello parameters above you simply need tooclick run migration and a set of two packages will be generated tooyour specified location.</span></span> <span data-ttu-id="3a900-136">Kör hello exportfilen i ordning tooexport data från källa för migrering till flat-filer och köra hello importfilen i ordning tooimport dina data till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3a900-136">Run hello export file in order tooexport data from your migration source into flat files, and run hello import file in order tooimport your data into SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a900-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a900-137">Next steps</span></span>
<span data-ttu-id="3a900-138">Nu när du har migrerat data, kolla hur för[utveckla][develop].</span><span class="sxs-lookup"><span data-stu-id="3a900-138">Now that you've migrated some data, check out how too[develop][develop].</span></span>

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
