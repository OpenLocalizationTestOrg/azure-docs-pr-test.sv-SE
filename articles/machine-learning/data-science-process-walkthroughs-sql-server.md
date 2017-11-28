---
title: "SQL Server datavetenskap genomgång med R, Python och T-SQL | Microsoft Docs"
description: "Exempel som går igenom hur R, Python och T-SQL i SQL Server för att göra förutsägelseanalyser."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev
ms.openlocfilehash: 333fd4ee6dcdcbfd347d6b5e8cb900474f6ec8cc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="sql-server-data-science-walkthroughs-using-r-python-and-t-sql"></a><span data-ttu-id="a6f6c-103">SQL Server datavetenskap genomgång med R, Python och T-SQL</span><span class="sxs-lookup"><span data-stu-id="a6f6c-103">SQL Server data science walkthroughs using R, Python and T-SQL</span></span>

<span data-ttu-id="a6f6c-104">Dessa genomgång använda SQL Server, SQL Server R Services och SQL Server Python Services för att göra förutsägelseanalyser.</span><span class="sxs-lookup"><span data-stu-id="a6f6c-104">These walkthroughs use SQL Server, SQL Server R Services, and SQL Server Python Services to do predictive analytics.</span></span> <span data-ttu-id="a6f6c-105">R och Python-kod distribueras i lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="a6f6c-105">R and Python code is deployed in stored procedures.</span></span> <span data-ttu-id="a6f6c-106">De följer stegen som beskrivs i Team av vetenskapliga data.</span><span class="sxs-lookup"><span data-stu-id="a6f6c-106">They follow the steps outlined in the Team Data Science Process.</span></span> <span data-ttu-id="a6f6c-107">En översikt över Team av vetenskapliga data, se [datavetenskap Process](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a6f6c-107">For an overview of the Team Data Science Process, see [Data Science Process](data-science-process-overview.md).</span></span> 

<span data-ttu-id="a6f6c-108">Ytterligare datavetenskap genomgångar som kör Team datavetenskap Process grupperas efter den **plattform** som de använder:</span><span class="sxs-lookup"><span data-stu-id="a6f6c-108">Additional data science walkthroughs that execute the Team Data Science Process are grouped by the **platform** that they use:</span></span> 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]


## <a name="predict-taxi-tips-using-python-and-sql-queries-with-sql-server"></a><span data-ttu-id="a6f6c-109">Förutsäga taxi tips Python och SQL-frågor med SQL Server</span><span class="sxs-lookup"><span data-stu-id="a6f6c-109">Predict taxi tips using Python and SQL queries with SQL Server</span></span> 

<span data-ttu-id="a6f6c-110">Den [används SQL Server](machine-learning-data-science-process-sql-walkthrough.md) genomgången visar hur du skapar och distribuerar machine learning klassificering och regression modeller med hjälp av SQL Server och ett offentligt tillgängliga NYC Taxitransport resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="a6f6c-110">The [Use SQL Server](machine-learning-data-science-process-sql-walkthrough.md) walkthrough shows how you build and deploy machine learning classification and regression models using SQL Server and a publicly available NYC taxi trip and fare dataset.</span></span>


## <a name="predict-taxi-tips-using-microsoft-r-with-sql-server"></a><span data-ttu-id="a6f6c-111">Förutsäga taxi tips Microsoft R med SQL Server</span><span class="sxs-lookup"><span data-stu-id="a6f6c-111">Predict taxi tips using Microsoft R with SQL Server</span></span> 

<span data-ttu-id="a6f6c-112">Den [används SQL Server R Services](https://msdn.microsoft.com/library/mt612857.aspx) genomgången innehåller datavetare med en kombination av R-koden, SQL Server-data och anpassade SQL-funktioner för att skapa och distribuera ett R modell till SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a6f6c-112">The [Use SQL Server R Services](https://msdn.microsoft.com/library/mt612857.aspx) walkthrough provides data scientists with a combination of R code, SQL Server data, and custom SQL functions to build and deploy an R model to SQL Server.</span></span> <span data-ttu-id="a6f6c-113">Den här genomgången är utformat för att införa R utvecklare till R-tjänster (i databasen).</span><span class="sxs-lookup"><span data-stu-id="a6f6c-113">The walkthrough is designed to introduce R developers to R Services (In-Database).</span></span>


## <a name="predict-taxi-tips-using-r-from-t-sql-or-stored-procedures-with-sql-server"></a><span data-ttu-id="a6f6c-114">Förutsäga taxi tips R från T-SQL eller lagrade procedurer med SQL Server</span><span class="sxs-lookup"><span data-stu-id="a6f6c-114">Predict taxi tips using R from T-SQL or stored procedures with SQL Server</span></span>

<span data-ttu-id="a6f6c-115">Den [datavetenskap genomgång för R och SQL Server](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough) ger SQL-programmerare upplevelse som skapar en lösning för avancerade analyser med Transact-SQL med SQL Server R Services för att operationalisera en R-lösning.</span><span class="sxs-lookup"><span data-stu-id="a6f6c-115">The [Data science walkthrough for R and SQL Server](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough) provides SQL programmers with experience building an advanced analytics solution with Transact-SQL using SQL Server R Services to operationalize an R solution.</span></span> 


## <a name="predict-taxi-tips-using-python-in-sql-server-stored-procedures"></a><span data-ttu-id="a6f6c-116">Förutsäga taxi tips med Python i SQL-lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="a6f6c-116">Predict taxi tips using Python in SQL Server stored procedures</span></span>

<span data-ttu-id="a6f6c-117">Den [Använd T-SQL med SQL Server Python Services](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/sqldev-in-database-python-for-sql-developers) genomgången innehåller SQL-programmerare skapa machine learning-lösning i SQL Server-upplevelse.</span><span class="sxs-lookup"><span data-stu-id="a6f6c-117">The [Use T-SQL with SQL Server Python Services](https://docs.microsoft.com/en-us/sql/advanced-analytics/tutorials/sqldev-in-database-python-for-sql-developers) walkthrough provides SQL programmers with experience building a machine learning solution in SQL Server.</span></span> <span data-ttu-id="a6f6c-118">Den visar hur du införliva Python i ett program genom att lägga till Python-kod till lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="a6f6c-118">It demonstrates how to incorporate Python into an application by adding Python code to stored procedures.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a6f6c-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a6f6c-119">Next steps</span></span>

<span data-ttu-id="a6f6c-120">En beskrivning av de viktiga komponenter som utgör teamet datavetenskap Process finns [Team datavetenskap processöversikt](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a6f6c-120">For a discussion of the key components that comprise the Team Data Science Process, see [Team Data Science Process overview](data-science-process-overview.md).</span></span>

<span data-ttu-id="a6f6c-121">En beskrivning av livscykeln Team av vetenskapliga data som du kan använda för att strukturera datavetenskap projekt finns [Team datavetenskap Process livscykel](data-science-process-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="a6f6c-121">For a discussion of the Team Data Science Process lifecycle that you can use to structure your data science projects, see [Team Data Science Process lifecycle](data-science-process-lifecycle.md).</span></span> <span data-ttu-id="a6f6c-122">Livscykeln beskrivs stegen från början till slut att projekt vanligtvis följer när de utförs.</span><span class="sxs-lookup"><span data-stu-id="a6f6c-122">The lifecycle outlines the steps, from start to finish, that projects usually follow when they are executed.</span></span> 