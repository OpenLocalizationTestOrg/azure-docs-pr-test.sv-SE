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
# <a name="sql-data-warehouse----downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a><span data-ttu-id="632a1-103">SQL Data Warehouse - klientversioner stöd för granskning och dynamisk Datamaskning</span><span class="sxs-lookup"><span data-stu-id="632a1-103">SQL Data Warehouse -  Downlevel clients support for auditing and Dynamic Data Masking</span></span>
<span data-ttu-id="632a1-104">[Granskning](sql-data-warehouse-auditing-overview.md) fungerar med SQL-klienter som stöder TDS-omdirigering.</span><span class="sxs-lookup"><span data-stu-id="632a1-104">[Auditing](sql-data-warehouse-auditing-overview.md) works with SQL clients that support TDS redirection.</span></span>

<span data-ttu-id="632a1-105">Alla klienter som implementerar TDS 7.4 bör också stöd för omdirigering.</span><span class="sxs-lookup"><span data-stu-id="632a1-105">Any client which implements TDS 7.4 should also support redirection.</span></span> <span data-ttu-id="632a1-106">Undantag toothis inkluderar JDBC 4.0 i vilken funktion du hello omdirigering stöds inte fullt ut och Tedious för Node.JS där omdirigering inte har implementerats.</span><span class="sxs-lookup"><span data-stu-id="632a1-106">Exceptions toothis include JDBC 4.0 in which hello redirection feature is not fully supported and Tedious for Node.JS in which redirection was not implemented.</span></span>

<span data-ttu-id="632a1-107">För ”äldre klienter”, ska d.v.s. som stöder TDS version 7.3 och hello nedan - serverns FQDN i anslutningssträngen för hello ändras:</span><span class="sxs-lookup"><span data-stu-id="632a1-107">For "Downlevel clients", i.e. which support TDS version 7.3 and below - hello server FQDN in hello connection string should be modified:</span></span>

<span data-ttu-id="632a1-108">Ursprungliga serverns FQDN i anslutningssträngen för hello: <*servernamn*>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="632a1-108">Original server FQDN in hello connection string: <*server name*>.database.windows.net</span></span>

<span data-ttu-id="632a1-109">Ändrade serverns FQDN i anslutningssträngen för hello: <*servernamn*> .database. **säker**. windows.net</span><span class="sxs-lookup"><span data-stu-id="632a1-109">Modified server FQDN in hello connection string: <*server name*>.database.**secure**.windows.net</span></span>

<span data-ttu-id="632a1-110">Innehåller en lista över ”klientversioner”:</span><span class="sxs-lookup"><span data-stu-id="632a1-110">A partial list of "Downlevel clients" includes:</span></span>

* <span data-ttu-id="632a1-111">.NET 4.0 och nedan.</span><span class="sxs-lookup"><span data-stu-id="632a1-111">.NET 4.0 and below,</span></span>
* <span data-ttu-id="632a1-112">ODBC-10.0 och nedan.</span><span class="sxs-lookup"><span data-stu-id="632a1-112">ODBC 10.0 and below.</span></span>
* <span data-ttu-id="632a1-113">JDBC (medan JDBC stöder TDS 7.4, hello TDS omdirigering av funktionen inte stöds fullt ut)</span><span class="sxs-lookup"><span data-stu-id="632a1-113">JDBC (while JDBC does support TDS 7.4, hello TDS redirection feature is not fully supported)</span></span>
* <span data-ttu-id="632a1-114">Tråkigt (för Node.JS)</span><span class="sxs-lookup"><span data-stu-id="632a1-114">Tedious (for Node.JS)</span></span>

<span data-ttu-id="632a1-115">**Kommentar:** hello ovan server FDQN ändring kan vara användbara också för att tillämpa en princip för SQL Server-nivå granskning utan behov av en konfiguration steg i varje databas (tillfällig lösning).</span><span class="sxs-lookup"><span data-stu-id="632a1-115">**Remark:** hello above server FDQN modification may be useful also for applying a SQL Server Level Auditing policy without a need for a configuration step in each database (Temporary mitigation).</span></span>     

