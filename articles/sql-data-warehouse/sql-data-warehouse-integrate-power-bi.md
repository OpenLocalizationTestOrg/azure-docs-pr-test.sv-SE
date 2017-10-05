---
title: "Använda Powerbi med SQL Data Warehouse | Microsoft Docs"
description: "Tips för att använda Power BI med Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: b12bee87-2268-40c2-81bf-ab27588b32e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: 4b7609fc5d6ce7bf0e3bd3ebf6d8f52e93a40a75
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-power-bi-with-sql-data-warehouse"></a><span data-ttu-id="0ac26-103">Använda Powerbi med SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0ac26-103">Use Power BI with SQL Data Warehouse</span></span>
<span data-ttu-id="0ac26-104">Som med Azure SQL Database tillåter SQL Data Warehouse Direct Connect användare att utnyttja kraftfulla logiska pushdown tillsammans med analytiska funktionerna i Power BI.</span><span class="sxs-lookup"><span data-stu-id="0ac26-104">As with Azure SQL Database, SQL Data Warehouse Direct Connect allows user to leverage powerful logical pushdown alongside the analytical capabilities of Power BI.</span></span>  <span data-ttu-id="0ac26-105">Med Direct Connect skickas frågor tillbaka till din Azure SQL Data Warehouse i realtid att utforska data.</span><span class="sxs-lookup"><span data-stu-id="0ac26-105">With Direct Connect, queries are sent back to your Azure SQL Data Warehouse in real time as you explore the data.</span></span>  <span data-ttu-id="0ac26-106">Detta kan kombineras med skalan för SQL Data Warehouse, kan användare skapa dynamiska rapporter i minuter mot terabyte data.</span><span class="sxs-lookup"><span data-stu-id="0ac26-106">This, combined with the scale of SQL Data Warehouse, enables users to create dynamic reports in minutes against terabytes of data.</span></span>  <span data-ttu-id="0ac26-107">Dessutom tillåter introduktionen av funktionen Öppna i Power BI-knappen användare att ansluta Power BI direkt till deras SQL Data Warehouse utan att samla in information från andra delar av Azure.</span><span class="sxs-lookup"><span data-stu-id="0ac26-107">In addition, the introduction of the Open in Power BI button allows users to directly connect Power BI to their SQL Data Warehouse without collecting information from other parts of Azure.</span></span>

<span data-ttu-id="0ac26-108">När du använder Direct Connect du Observera:</span><span class="sxs-lookup"><span data-stu-id="0ac26-108">When using Direct Connect please note:</span></span>

* <span data-ttu-id="0ac26-109">Ange det fullständigt kvalificerade servernamnet vid anslutning (se nedan för mer information)</span><span class="sxs-lookup"><span data-stu-id="0ac26-109">Specify the fully qualified server name when connecting (see below for more details)</span></span>
* <span data-ttu-id="0ac26-110">Se till att brandväggsreglerna för databasen är konfigurerade för ”Tillåt åtkomst till Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="0ac26-110">Ensure firewall rules for the database are configured to "Allow access to Azure services".</span></span>
* <span data-ttu-id="0ac26-111">Varje åtgärd som att markera en kolumn eller lägger till ett filter kommer direkt söka i datalagret</span><span class="sxs-lookup"><span data-stu-id="0ac26-111">Every action such as selecting a column or adding a filter will  directly query the data warehouse</span></span>
* <span data-ttu-id="0ac26-112">Paneler uppdateras ungefär var 15 minut (uppdatera inte behöver schemaläggas)</span><span class="sxs-lookup"><span data-stu-id="0ac26-112">Tiles are refreshed approximately every 15 minutes (refresh does not need to be scheduled)</span></span>
* <span data-ttu-id="0ac26-113">Frågor och svar är inte tillgängliga för datauppsättningar som Direct Connect</span><span class="sxs-lookup"><span data-stu-id="0ac26-113">Q&A is not available for Direct Connect datasets</span></span>
* <span data-ttu-id="0ac26-114">Schemaändringar har inte plockats automatiskt</span><span class="sxs-lookup"><span data-stu-id="0ac26-114">Schema changes are not picked up automatically</span></span>
* <span data-ttu-id="0ac26-115">Alla frågor med Direct Connect gör timeout efter två minuter</span><span class="sxs-lookup"><span data-stu-id="0ac26-115">All Direct Connect queries will time out after 2 minutes</span></span>

<span data-ttu-id="0ac26-116">Dessa begränsningar och anteckningar kan ändras när vi fortsätta att förbättra upplevelser.</span><span class="sxs-lookup"><span data-stu-id="0ac26-116">These restrictions and notes may change as we continue to improve the experiences.</span></span> <span data-ttu-id="0ac26-117">Nedan beskrivs stegen för att ansluta.</span><span class="sxs-lookup"><span data-stu-id="0ac26-117">The steps to connect are detailed below.</span></span>  

## <a name="using-the-open-in-power-bi-button"></a><span data-ttu-id="0ac26-118">Med knappen ”Öppna i Power BI-</span><span class="sxs-lookup"><span data-stu-id="0ac26-118">Using the ‘Open in Power BI’ button</span></span>
<span data-ttu-id="0ac26-119">Det enklaste sättet att flytta mellan din SQL Data Warehouse och Power BI är öppna i Power BI-knappen.</span><span class="sxs-lookup"><span data-stu-id="0ac26-119">The easiest way to move between your SQL Data Warehouse and Power BI is with the Open in Power BI button.</span></span> <span data-ttu-id="0ac26-120">Den här knappen kan du sömlöst börjar skapa nya instrumentpaneler i Power BI.</span><span class="sxs-lookup"><span data-stu-id="0ac26-120">This button allows you to seamlessly begin creating new dashboards in Power BI.</span></span>  

1. <span data-ttu-id="0ac26-121">Du kommer igång går du till din SQL Data Warehouse-instans i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0ac26-121">To get started navigate to your SQL Data Warehouse instance in the Azure Classic Portal.</span></span>
2. <span data-ttu-id="0ac26-122">Klicka på knappen Öppna i Power BI.</span><span class="sxs-lookup"><span data-stu-id="0ac26-122">Click the 'Open in Power BI' button.</span></span>
3. <span data-ttu-id="0ac26-123">Om vi kan inte logga in dig direkt, eller om du inte har en Power BI-konto behöver du logga in.</span><span class="sxs-lookup"><span data-stu-id="0ac26-123">If we are not able to sign you in directly, or if you do not have a Power BI account, you will need to sign-in.</span></span>  
4. <span data-ttu-id="0ac26-124">Du kommer till sidan SQL Data Warehouse-anslutning med information från ditt SQL Data Warehouse ifylld.</span><span class="sxs-lookup"><span data-stu-id="0ac26-124">You will be directed to the SQL Data Warehouse connection page, with the information from your SQL Data Warehouse pre-populated.</span></span>
5. <span data-ttu-id="0ac26-125">När du har angett dina autentiseringsuppgifter ansluts du fullständigt till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0ac26-125">After entering your credentials you will be fully connected to your SQL Data Warehouse.</span></span>

## <a name="connecting-through-the-power-bi-portal"></a><span data-ttu-id="0ac26-126">Ansluta Power BI-portalen</span><span class="sxs-lookup"><span data-stu-id="0ac26-126">Connecting through the Power BI portal</span></span>
<span data-ttu-id="0ac26-127">Förutom att använda funktionen Öppna i Power BI-knappen kan ansluta användarna också till sina SQL Data Warehouse på Power BI-portalen.</span><span class="sxs-lookup"><span data-stu-id="0ac26-127">In addition to using the Open in Power BI button, users can also connect to their SQL Data Warehouse through the Power BI Portal.</span></span>

1. <span data-ttu-id="0ac26-128">Klicka på ”Hämta uppgifter” längst ned i navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="0ac26-128">Click 'Get Data' at the bottom of the navigation pane.</span></span>
2. <span data-ttu-id="0ac26-129">Välj 'Databaser'.</span><span class="sxs-lookup"><span data-stu-id="0ac26-129">Select 'Databases'.</span></span>
3. <span data-ttu-id="0ac26-130">Markera en gång ”Azure SQL Data Warehouse' på sidan databaser och klicka på” Anslut ”.</span><span class="sxs-lookup"><span data-stu-id="0ac26-130">Once on the Databases page, select 'Azure SQL Data Warehouse' and then click 'Connect'.</span></span>
4. <span data-ttu-id="0ac26-131">Ange nödvändig anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="0ac26-131">Enter the necessary connection information.</span></span>  <span data-ttu-id="0ac26-132">Din servernamnet och databasnamnet kan hittas i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="0ac26-132">Your server name and database name can be found in the Azure Portal.</span></span>
5. <span data-ttu-id="0ac26-133">Du kommer tillbaka till huvudsidan för Power BI och när anslutningen görs en ny post under 'Datauppsättningar' visas med namnet på din instans.</span><span class="sxs-lookup"><span data-stu-id="0ac26-133">You will be directed back to the main page of Power BI and after your connection is made a new entry under 'Datasets' will appear with the name of your instance.</span></span>  
6. <span data-ttu-id="0ac26-134">Du kan klicka på ny datauppsättning och utforska alla tabeller och vyer i databasen.</span><span class="sxs-lookup"><span data-stu-id="0ac26-134">You can click on the new dataset to explore all of the tables and views in your database.</span></span> <span data-ttu-id="0ac26-135">Att markera en kolumn kommer att skicka en fråga till källan, att dynamiskt skapa din visual.</span><span class="sxs-lookup"><span data-stu-id="0ac26-135">Selecting a column will send a query back to the source, dynamically creating your visual.</span></span> <span data-ttu-id="0ac26-136">Dessa visuell information kan sparas i en ny rapport och tillbaka fäst på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="0ac26-136">These visuals can be saved in a new report and pinned back to your dashboard.</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
