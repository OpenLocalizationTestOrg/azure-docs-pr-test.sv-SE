---
title: aaaUse Power BI med SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: a3a347493d07af6824a561567f05894cfe3c0471
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-with-sql-data-warehouse"></a><span data-ttu-id="80ba6-103">Använda Powerbi med SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="80ba6-103">Use Power BI with SQL Data Warehouse</span></span>
<span data-ttu-id="80ba6-104">Precis som med Azure SQL Database, kan SQL Data Warehouse Direct Connect användaren tooleverage kraftfulla logiska pushdown tillsammans med hello analytiska funktionerna i Power BI.</span><span class="sxs-lookup"><span data-stu-id="80ba6-104">As with Azure SQL Database, SQL Data Warehouse Direct Connect allows user tooleverage powerful logical pushdown alongside hello analytical capabilities of Power BI.</span></span>  <span data-ttu-id="80ba6-105">Med Direct Connect skickas frågor tillbaka tooyour Azure SQL Data Warehouse i realtid att utforska hello data.</span><span class="sxs-lookup"><span data-stu-id="80ba6-105">With Direct Connect, queries are sent back tooyour Azure SQL Data Warehouse in real time as you explore hello data.</span></span>  <span data-ttu-id="80ba6-106">Detta kan kombineras med hello skalan för SQL Data Warehouse, kan användare toocreate dynamiska rapporter i minuter mot terabyte data.</span><span class="sxs-lookup"><span data-stu-id="80ba6-106">This, combined with hello scale of SQL Data Warehouse, enables users toocreate dynamic reports in minutes against terabytes of data.</span></span>  <span data-ttu-id="80ba6-107">Dessutom hello introduktionen av hello öppna i Power BI-knappen kan användare toodirectly ansluta Power BI tootheir SQL Data Warehouse utan att samla in information från andra delar av Azure.</span><span class="sxs-lookup"><span data-stu-id="80ba6-107">In addition, hello introduction of hello Open in Power BI button allows users toodirectly connect Power BI tootheir SQL Data Warehouse without collecting information from other parts of Azure.</span></span>

<span data-ttu-id="80ba6-108">När du använder Direct Connect du Observera:</span><span class="sxs-lookup"><span data-stu-id="80ba6-108">When using Direct Connect please note:</span></span>

* <span data-ttu-id="80ba6-109">Ange hello fullständigt kvalificerade servernamnet vid anslutning (se nedan för mer information)</span><span class="sxs-lookup"><span data-stu-id="80ba6-109">Specify hello fully qualified server name when connecting (see below for more details)</span></span>
* <span data-ttu-id="80ba6-110">Se till att brandväggsreglerna för hello databasen är konfigurerad för ”Tillåt åtkomst tooAzure services”.</span><span class="sxs-lookup"><span data-stu-id="80ba6-110">Ensure firewall rules for hello database are configured too"Allow access tooAzure services".</span></span>
* <span data-ttu-id="80ba6-111">Varje åtgärd som att markera en kolumn eller lägger till ett filter kommer direkt söka hello datalagret</span><span class="sxs-lookup"><span data-stu-id="80ba6-111">Every action such as selecting a column or adding a filter will  directly query hello data warehouse</span></span>
* <span data-ttu-id="80ba6-112">Paneler uppdateras ungefär var femtonde minut (uppdatera inte behöver toobe schemalagd)</span><span class="sxs-lookup"><span data-stu-id="80ba6-112">Tiles are refreshed approximately every 15 minutes (refresh does not need toobe scheduled)</span></span>
* <span data-ttu-id="80ba6-113">Frågor och svar är inte tillgängliga för datauppsättningar som Direct Connect</span><span class="sxs-lookup"><span data-stu-id="80ba6-113">Q&A is not available for Direct Connect datasets</span></span>
* <span data-ttu-id="80ba6-114">Schemaändringar har inte plockats automatiskt</span><span class="sxs-lookup"><span data-stu-id="80ba6-114">Schema changes are not picked up automatically</span></span>
* <span data-ttu-id="80ba6-115">Alla frågor med Direct Connect gör timeout efter två minuter</span><span class="sxs-lookup"><span data-stu-id="80ba6-115">All Direct Connect queries will time out after 2 minutes</span></span>

<span data-ttu-id="80ba6-116">Dessa begränsningar och anteckningar kan ändras när vi fortsätta tooimprove hello upplevelser.</span><span class="sxs-lookup"><span data-stu-id="80ba6-116">These restrictions and notes may change as we continue tooimprove hello experiences.</span></span> <span data-ttu-id="80ba6-117">hello steg tooconnect beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="80ba6-117">hello steps tooconnect are detailed below.</span></span>  

## <a name="using-hello-open-in-power-bi-button"></a><span data-ttu-id="80ba6-118">Med hjälp av hello ”öppna i Power BI-knappen</span><span class="sxs-lookup"><span data-stu-id="80ba6-118">Using hello ‘Open in Power BI’ button</span></span>
<span data-ttu-id="80ba6-119">hello enklaste sättet toomove mellan din SQL Data Warehouse och Power BI är med hello öppna i Power BI-knappen.</span><span class="sxs-lookup"><span data-stu-id="80ba6-119">hello easiest way toomove between your SQL Data Warehouse and Power BI is with hello Open in Power BI button.</span></span> <span data-ttu-id="80ba6-120">Den här knappen om du tooseamlessly börjar skapa nya instrumentpaneler i Power BI.</span><span class="sxs-lookup"><span data-stu-id="80ba6-120">This button allows you tooseamlessly begin creating new dashboards in Power BI.</span></span>  

1. <span data-ttu-id="80ba6-121">tooget igång navigera tooyour SQL Data Warehouse-instans i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80ba6-121">tooget started navigate tooyour SQL Data Warehouse instance in hello Azure Classic Portal.</span></span>
2. <span data-ttu-id="80ba6-122">Klicka på hello ”öppna i Power BI-knappen.</span><span class="sxs-lookup"><span data-stu-id="80ba6-122">Click hello 'Open in Power BI' button.</span></span>
3. <span data-ttu-id="80ba6-123">Om vi inte kan toosign i direkt, eller om du inte har en Power BI-konto behöver du toosign i.</span><span class="sxs-lookup"><span data-stu-id="80ba6-123">If we are not able toosign you in directly, or if you do not have a Power BI account, you will need toosign-in.</span></span>  
4. <span data-ttu-id="80ba6-124">Du kommer toohello SQL Data Warehouse-anslutningssidan, med hello information från ditt SQL Data Warehouse ifylld.</span><span class="sxs-lookup"><span data-stu-id="80ba6-124">You will be directed toohello SQL Data Warehouse connection page, with hello information from your SQL Data Warehouse pre-populated.</span></span>
5. <span data-ttu-id="80ba6-125">När du har angett dina autentiseringsuppgifter kommer du att fullständigt anslutna tooyour SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="80ba6-125">After entering your credentials you will be fully connected tooyour SQL Data Warehouse.</span></span>

## <a name="connecting-through-hello-power-bi-portal"></a><span data-ttu-id="80ba6-126">Ansluta via hello Power BI-portalen</span><span class="sxs-lookup"><span data-stu-id="80ba6-126">Connecting through hello Power BI portal</span></span>
<span data-ttu-id="80ba6-127">I tillägg toousing hello öppna i Power BI-knappen, kan användarna också ansluta tootheir SQL Data Warehouse via hello Power BI-portalen.</span><span class="sxs-lookup"><span data-stu-id="80ba6-127">In addition toousing hello Open in Power BI button, users can also connect tootheir SQL Data Warehouse through hello Power BI Portal.</span></span>

1. <span data-ttu-id="80ba6-128">Klicka på ”Hämta uppgifter” längst ned hello hello navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="80ba6-128">Click 'Get Data' at hello bottom of hello navigation pane.</span></span>
2. <span data-ttu-id="80ba6-129">Välj 'Databaser'.</span><span class="sxs-lookup"><span data-stu-id="80ba6-129">Select 'Databases'.</span></span>
3. <span data-ttu-id="80ba6-130">Välj en gång 'Azure SQL Data Warehouse' hello databaser på sidan och klicka på ”Anslut”.</span><span class="sxs-lookup"><span data-stu-id="80ba6-130">Once on hello Databases page, select 'Azure SQL Data Warehouse' and then click 'Connect'.</span></span>
4. <span data-ttu-id="80ba6-131">Ange hello nödvändig anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="80ba6-131">Enter hello necessary connection information.</span></span>  <span data-ttu-id="80ba6-132">Din servernamnet och databasnamnet kan hittas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80ba6-132">Your server name and database name can be found in hello Azure Portal.</span></span>
5. <span data-ttu-id="80ba6-133">Du kommer tillbaka toohello huvudsidan i Power BI och när anslutningen görs en ny post under 'Datauppsättningar' visas med hello namnet på din instans.</span><span class="sxs-lookup"><span data-stu-id="80ba6-133">You will be directed back toohello main page of Power BI and after your connection is made a new entry under 'Datasets' will appear with hello name of your instance.</span></span>  
6. <span data-ttu-id="80ba6-134">Du kan klicka på hello nya dataset tooexplore alla hello tabeller och vyer i databasen.</span><span class="sxs-lookup"><span data-stu-id="80ba6-134">You can click on hello new dataset tooexplore all of hello tables and views in your database.</span></span> <span data-ttu-id="80ba6-135">Att markera en kolumn kommer att skicka en fråga tillbaka toohello källa, att dynamiskt skapa din visual.</span><span class="sxs-lookup"><span data-stu-id="80ba6-135">Selecting a column will send a query back toohello source, dynamically creating your visual.</span></span> <span data-ttu-id="80ba6-136">Dessa visuell information kan sparas i en ny rapport och Fäst tillbaka tooyour instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="80ba6-136">These visuals can be saved in a new report and pinned back tooyour dashboard.</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
