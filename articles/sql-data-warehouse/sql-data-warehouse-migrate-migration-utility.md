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
# <a name="data-warehouse-migration-utility-preview"></a>Verktyg för migrering av data Warehouse (förhandsgranskning)
> [!div class="op_single_selector"]
> * [Hämta verktyg för migrering][Download Migration Utility]
> 
> 

hello Migreringsverktyget för Data Warehouse är ett verktyg som utformats toomigrate schema och data från SQL Server och Azure SQL Database tooAzure SQL Data Warehouse. Under migreringen av schemat mappas hello verktyget automatiskt hello motsvarande schema från källan toodestination. När hello schema har migrerats, tillhandahåller hello verktyg hello alternativet toomove data med automatiskt genererade skript.

Dessutom effektiv tooschema och datamigrering verktyget kan du hello alternativet toogenerate kompatibilitetsrapporter som sammanfattar inkompatibilitet mellan hello mål- och instanser som skulle kunna hindra migrering.

## <a name="get-started"></a>Kom igång
En förutsättning för installationen måste hello BCP kommandoradsverktyget toorun migreringsskript och Office tooview hello Kompatibilitetsrapport. När du startar hello kommer körbar fil som hämtas du att tillfrågas tooaccept standard LICENSAVTALET innan hello verktyget kommer att installeras.

Dessutom toorun hello migrering Utiliy du kommer måste hello en följande behörigheter på hello databasen söker toomigrate: CREATE DATABASE, ALTER ANY DATABASE eller ANY VYDEFINITIONEN.

### <a name="launching-hello-tool-and-connecting"></a>Starta verktyget hello och ansluta
Starta hello verktyget genom att klicka på hello skrivbordsikon som visas efter installera. När öppnas hello verktyget, uppmanas du att med en initial anslutning sida där du kan välja källan och målet för hello Migreringsverktyget. För närvarande kan stöder vi SQL Server och Azure SQL Database som källor och SQL Data Warehouse som mål. När du har valt det här blir du ombedd tooconnect tooyour källservern genom att fylla i servernamnet och autentisering och sedan klicka på ”Anslut”.

Efter autentisering, visas hello-verktyget en lista över databaser som finns i hello-server som du är ansluten till. Du kan börja hello migreringen genom att välja en databas som du vill att toomigrate och sedan klicka på 'Migrera valt'.

## <a name="migration-report"></a>Rapporten migrering
Att välja ”Kontrollera databasen kompatibilitet' i hello verktyget kommer Generera en rapport som sammanfattar objektinkompatibiliteter för alla i databasen hello begärda toomigrate. En bredare lista över hello SQL Server-funktioner som inte finns i SQL Data Warehouse finns i vår [Migreringsdokumentation][migration documentation]. Du kommer att kunna toosave och öppna hello rapport i Excel när hello rapporten genereras.

Observera att när du genererar hello migrering schema, de flesta problem som har identifierats som 'Object' justeras i ordning tooallow omedelbar migrering av data. Granska hello ändringar tooensure du inte vill toomake ytterligare justeringar innan du tillämpar hello schemat.

## <a name="migrate-schema"></a>Migrera schema
Efter anslutning genererar välja migrera schemat ett schema migrering skript för hello markerade tabeller. Det här skriptet portar hello struktur hello tabell mappar inkompatibla data typer toomore kompatibel formulär och skapar säkerhetsreferenser och schema om detta anges av hello användare i hello migreringsinställningarna. Den här koden kan köras mot hello riktade SQL Data Warehouse-instans, sparade tooa filen, kopieras tooyour Urklipp eller redigeras även i rad innan du vidtar åtgärder.  

Som beskrivs ovan när migrera schemat granska hello migrering ändrar den hello verktyget har gjort i ordning tooensure som att du förstår dem..  

## <a name="migrate-data"></a>Migrera data
Genom att klicka på alternativet för hello migrera Data kan du generera BCP-skript som flyttar dina första tooflat filer på servern, och sedan direkt till ditt SQL Data Warehouse. Vi rekommenderar den här processen för att flytta små mängder data, och som nya försök inte är inbyggda och fel kan uppstå om det finns en förlust av hello nätverksanslutning. I ordning toorun detta, behöver du toohave hello kommandoradsverktyget BCP installerat och hello schemat för hello data måste redan ha skapats.

När du har fyllt i hello parametrar ovan du bara behöver tooclick kör migrering och en uppsättning två paket kommer att skapas tooyour angivna plats. Kör hello exportfilen i ordning tooexport data från källa för migrering till flat-filer och köra hello importfilen i ordning tooimport dina data till SQL Data Warehouse.

## <a name="next-steps"></a>Nästa steg
Nu när du har migrerat data, kolla hur för[utveckla][develop].

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
