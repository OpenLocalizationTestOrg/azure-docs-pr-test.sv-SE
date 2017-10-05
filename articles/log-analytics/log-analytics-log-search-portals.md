---
title: "Portaler för att skapa och redigera loggen frågor i Azure Log Analytics | Microsoft Docs"
description: "Den här artikeln beskriver portalerna som du kan använda i Azure Log Analytics för att skapa och redigera logga sökningar."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: bwren
ms.openlocfilehash: 29dfa31d38f85574f84ed351bc5c26224b1a7e16
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a><span data-ttu-id="73210-103">Portaler för att skapa och redigera loggen frågor i Azure Log Analytics</span><span class="sxs-lookup"><span data-stu-id="73210-103">Portals for creating and editing log queries in Azure Log Analytics</span></span>

> [!NOTE]
> <span data-ttu-id="73210-104">Den här artikeln beskriver portaler i Azure Log Analytics med hjälp av det nya språket i fråga.</span><span class="sxs-lookup"><span data-stu-id="73210-104">This article describes portals in Azure Log Analytics using the new query language.</span></span>  <span data-ttu-id="73210-105">Du lär dig mer om det nya språket och få proceduren för att uppgradera din arbetsyta på [uppgradera Azure logganalys-arbetsytan till ny logg sökning](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="73210-105">You can learn more about the new language and get the procedure to upgrade your workspace at [Upgrade your Azure Log Analytics workspace to new log search](log-analytics-log-search-upgrade.md).</span></span>  
>
> <span data-ttu-id="73210-106">Om ditt arbetsområde inte har uppgraderats till det nya språket i fråga, bör du gå till [söka efter data med hjälp av loggen sökningar i logganalys](log-analytics-log-searches.md) information om den aktuella versionen av loggen Sök-portalen.</span><span class="sxs-lookup"><span data-stu-id="73210-106">If your workspace hasn't been upgraded to the new query language, you should refer to [Find data using log searches in Log Analytics](log-analytics-log-searches.md) for information on the current version of the Log Search portal.</span></span>

<span data-ttu-id="73210-107">Du kan använda loggen sökningar på olika ställen i Log Analytics för att hämta data från arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="73210-107">You use log searches in a variety of places throughout Log Analytics to retrieve data from the workspace.</span></span>  <span data-ttu-id="73210-108">Faktiskt skapar och redigerar frågor förutom att arbeta interaktivt med data som returneras om, har du två alternativ som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="73210-108">For actually creating and editing queries in addition to working interactively with returned data though, you have two options that are described below.</span></span>  

## <a name="log-search-portal"></a><span data-ttu-id="73210-109">Loggen sökportal</span><span class="sxs-lookup"><span data-stu-id="73210-109">Log search portal</span></span>
<span data-ttu-id="73210-110">Loggen Sök-portalen är åtkomlig från Azure-portalen eller OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="73210-110">The Log Search portal is accessible from the Azure portal or the OMS portal.</span></span>  <span data-ttu-id="73210-111">Det är lämpligt för att skapa grundläggande frågor som kan skapas på en enda rad.</span><span class="sxs-lookup"><span data-stu-id="73210-111">It's suitable for creating basic queries that can be created on a single line.</span></span>  <span data-ttu-id="73210-112">Loggen Sök-portalen kan användas utan att starta en extern portal och du kan använda för att utföra en mängd funktioner med loggen sökningar, inklusive att skapa Varningsregler, skapar datorgrupper och exportera resultatet av frågan.</span><span class="sxs-lookup"><span data-stu-id="73210-112">The Log Search portal can be used without launching an external portal, and you can use it to perform a variety of functions with log searches including creating alert rules, creating computer groups, and exporting the results of the query.</span></span>  

<span data-ttu-id="73210-113">Loggen Sök portal innehåller flera funktioner för att redigera frågan utan att ha fullständiga kunskaper om frågespråket.</span><span class="sxs-lookup"><span data-stu-id="73210-113">The Log Search portal provides multiple features for editing the query without having a full knowledge of the query language.</span></span>  <span data-ttu-id="73210-114">Du kan få en översikt över funktionerna i [skapa loggen söker i Azure Log Analytics med hjälp av loggen Sök portal](log-analytics-log-search-log-search-portal.md).</span><span class="sxs-lookup"><span data-stu-id="73210-114">You can get a summary of these features in [Create log searches in Azure Log Analytics using the Log Search portal](log-analytics-log-search-log-search-portal.md).</span></span>


![Logga sökportal](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a><span data-ttu-id="73210-116">Avancerade Analytics-portalen</span><span class="sxs-lookup"><span data-stu-id="73210-116">Advanced Analytics portal</span></span>
<span data-ttu-id="73210-117">Avancerade analyser portal är en dedikerad portal som ger avancerade funktioner som inte är tillgängliga i loggen Sök-portalen.</span><span class="sxs-lookup"><span data-stu-id="73210-117">The Advanced Analytics portal is a dedicated portal that provides advanced functionality not available in the Log Search portal.</span></span>  <span data-ttu-id="73210-118">Innefattar möjligheten att redigera en fråga på flera rader, selektivt köra kod, sammanhangsberoende Intellisense och smarta Analytics.</span><span class="sxs-lookup"><span data-stu-id="73210-118">Features include the ability to edit a query on multiple lines, selectively execute code, context sensitive Intellisense, and Smart Analytics.</span></span>  <span data-ttu-id="73210-119">Advanced Analytics-portalen är mest lämpliga för att utforma komplexa frågor som antingen sparas som en logg sökning eller kopieras och klistras in i andra logganalys-element.</span><span class="sxs-lookup"><span data-stu-id="73210-119">The Advanced Analytics portal is most suitable for designing complex queries that are either saved as a log search or copied and pasted into other Log Analytics elements.</span></span>  <span data-ttu-id="73210-120">Du kan starta Advanced Analytics-portalen från en länk i loggen Sök-portalen.</span><span class="sxs-lookup"><span data-stu-id="73210-120">You launch the Advanced Analytics portal from a link in the Log Search portal.</span></span>

![Avancerade Analytics-portalen](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


<span data-ttu-id="73210-122">På grund av dess avancerade funktioner, ska du vanligtvis använda avancerade analyser portal som din primära verktyget för att skapa och redigera frågor.</span><span class="sxs-lookup"><span data-stu-id="73210-122">Because of its advanced features, you'll usually use the Advanced Analytics portal as your primary tool for creating and editing queries.</span></span>  <span data-ttu-id="73210-123">När du har bestämt att frågan fungerar som förväntat och sedan du kopierar och klistrar in den någon annanstans, till exempel loggen Sök portalen eller View Designer.</span><span class="sxs-lookup"><span data-stu-id="73210-123">Once you've determined that the query works as expected, then you'll copy and paste it elsewhere such as the Log Search portal or View Designer.</span></span>  <span data-ttu-id="73210-124">Eftersom Advanced Analytics-portalen stöder frågor med flera rader om du behöver ta hänsyn till följande när du kopierar en fråga från den här portalen.</span><span class="sxs-lookup"><span data-stu-id="73210-124">Because the Advanced Analytics portal supports queries with multiple lines though, you need to take the following into consideration when copying a query from this portal.</span></span>

- <span data-ttu-id="73210-125">Kommentarer måste tas bort från frågan innan den har kopieras och klistras in i en annan plats.</span><span class="sxs-lookup"><span data-stu-id="73210-125">Comments must be removed from the query before it's copied and pasted into another location.</span></span>  <span data-ttu-id="73210-126">Du kan kommentera en rad genom att låta det med två snedstreck (/ /).</span><span class="sxs-lookup"><span data-stu-id="73210-126">You can comment a line by preceding it with two slashes (//).</span></span>  <span data-ttu-id="73210-127">När du klistrar in en flera rad-fråga till en enda rad tas radbrytningar bort.</span><span class="sxs-lookup"><span data-stu-id="73210-127">When you paste a multiple line query into a single line, line breaks are removed.</span></span>  <span data-ttu-id="73210-128">Om kommentarer ingår betraktas alla tecken efter den första kommentaren som en del av kommentaren.</span><span class="sxs-lookup"><span data-stu-id="73210-128">If comments are included, all characters after the first comment are considered part of the comment.</span></span>


## <a name="next-steps"></a><span data-ttu-id="73210-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="73210-129">Next steps</span></span>

- <span data-ttu-id="73210-130">Gå igenom en självstudiekurs om hur du använder den [loggen Sök portal](log-analytics-log-search-log-search-portal.md) eller [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) att skapa frågor.</span><span class="sxs-lookup"><span data-stu-id="73210-130">Walk through a tutorial on using the [Log Search portal](log-analytics-log-search-log-search-portal.md) or the [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) to create queries.</span></span>
- <span data-ttu-id="73210-131">Checka ut en [självstudier om hur du skriver frågor](https://go.microsoft.com/fwlink/?linkid=856078) med det nya språket i fråga.</span><span class="sxs-lookup"><span data-stu-id="73210-131">Check out a [tutorial on writing queries](https://go.microsoft.com/fwlink/?linkid=856078) using the new query language.</span></span>
