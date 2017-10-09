---
title: "aaaMigrate din lösning tooSQL Data Warehouse | Microsoft Docs"
description: "Riktlinjer för migrering för att skapa din lösning tooAzure SQL Data Warehouse-plattform."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/27/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 27b51f15247603f054070f360ede7f24541c0288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-solution-tooazure-sql-data-warehouse"></a>Migrera din lösning tooAzure SQL Data Warehouse
Se vad ingår i att migrera en befintlig databas lösning tooAzure SQL Data Warehouse. 

## <a name="profile-your-workload"></a>Profilen din arbetsbelastning
Innan du migrerar, vill du toobe vissa SQL Data Warehouse är hello rätta lösningen för din arbetsbelastning. SQL Data Warehouse är ett distribuerat system som utformats för tooperform analyser på stora mängder data.  Migrera tooSQL datalager kräver vissa ändringar av design som inte är för svårt toounderstand men kan ta viss tid tooimplement. Om ditt företag kräver ett informationslager i företagsklass, är hello fördelar värda hello arbete. Men om du inte behöver hello kraften i SQL Data Warehouse, är det mer kostnadseffektivt toouse SQL Server eller Azure SQL Database.

Överväg att använda SQL Data Warehouse när du:
- Har en eller flera terabyte data
- Planera toorun analytics på stora mängder data
- Behöver hello möjlighet tooscale beräkning och lagring 
- Vill toosave kostnader genom att pausa beräkningsresurser när de inte behövs.

Använd inte SQL Data Warehouse för operativa (OLTP) arbetsbelastningar som har:
- Hög frekvens läser och skriver
- Stort antal singleton väljer
- Stora mängder enradig infogningar
- Rad för rad bearbetning behov
- Inkompatibla format (JSON, XML)


## <a name="plan-hello-migration"></a>Planera migrering av hello

När du har valt toomigrate en befintlig lösning tooSQL Data Warehouse, är det viktigt tooplan hello migrering för att komma igång. 

Ett mål för att planera tooensure dina data, din tabellscheman och koden är kompatibel med SQL Data Warehouse. Det finns vissa kompatibilitet skillnader toowork runt mellan din aktuella systemet och SQL Data Warehouse. Dessutom kan migrera stora mängder data tooAzure tar tid. Noggrann planering påskyndar hämtar dina data tooAzure. 

Ett annat mål av planering är toomake designen justeringar tooensure din lösning kan du utnyttja hello hög frågeprestanda SQL Data Warehouse är utformad tooprovide. Designa datalager för att skala introducerar olika designmönster och så traditionella metoder är inte alltid hello bäst. Även om du kan göra vissa design justeringar efter migreringen, sparar gör ändringar snabbare i hello processen tid senare.

tooperform en framgångsrik migrering, behöver du toomigrate din tabellscheman koden och dina data. Information om dessa migreringsavsnitt finns:

-  [Migrera dina scheman](sql-data-warehouse-migrate-schema.md)
-  [Migrera koden](sql-data-warehouse-migrate-code.md)
-  [Migrera dina data](sql-data-warehouse-migrate-data.md). 

<!--
## Perform hello migration


## Deploy hello solution


## Validate hello migration

-->

## <a name="next-steps"></a>Nästa steg
hello CAT (Customer Advisory Team) har också bra vägledning SQL Data Warehouse, som de publicerar via bloggar.  Ta en titt på deras artikel [migrera data tooAzure SQL Data Warehouse i praktiken] [ Migrating data tooAzure SQL Data Warehouse in practice] för ytterligare information om migrering.

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data tooAzure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
