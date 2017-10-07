---
title: "aaaSQL Data Warehouse-klientversioner har stöd för granskning av data | Microsoft Docs"
description: "Lär dig mer om SQL Data Warehouse äldre klienter stöd för granskning av data"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: dfe29ff3-dfeb-4309-83c0-c1a300f4f44e
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 377488680eb297c3e9b1dc754c003c5b19b47996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>SQL Data Warehouse - klientversioner stöd för granskning och dynamisk Datamaskning
[Granskning](sql-data-warehouse-auditing-overview.md) fungerar med SQL-klienter som stöder TDS-omdirigering.

Alla klienter som implementerar TDS 7.4 bör också stöd för omdirigering. Undantag toothis inkluderar JDBC 4.0 i vilken funktion du hello omdirigering stöds inte fullt ut och Tedious för Node.JS där omdirigering inte har implementerats.

För ”äldre klienter”, ska d.v.s. som stöder TDS version 7.3 och hello nedan - serverns FQDN i anslutningssträngen för hello ändras:

Ursprungliga serverns FQDN i anslutningssträngen för hello: <*servernamn*>. database.windows.net

Ändrade serverns FQDN i anslutningssträngen för hello: <*servernamn*> .database. **säker**. windows.net

Innehåller en lista över ”klientversioner”:

* .NET 4.0 och nedan.
* ODBC-10.0 och nedan.
* JDBC (medan JDBC stöder TDS 7.4, hello TDS omdirigering av funktionen inte stöds fullt ut)
* Tråkigt (för Node.JS)

**Kommentar:** hello ovan server FDQN ändring kan vara användbara också för att tillämpa en princip för SQL Server-nivå granskning utan behov av en konfiguration steg i varje databas (tillfällig lösning).     

