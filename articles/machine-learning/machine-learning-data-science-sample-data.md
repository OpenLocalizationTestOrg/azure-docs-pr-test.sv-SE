---
title: "aaaSample data i Azure blob-behållare, SQL Server och Hive tabeller | Microsoft Docs"
description: Hur tooexplore data lagras i olika Azure enviromnents.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 80a9dfae-e3a6-4cfb-aecc-5701cfc7e39d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 5a5295b59fa02f91da680fc7495a92ca135e26c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="795f0-103"><a name="heading"></a>Exempeldata i Azure blob-behållare, SQL Server och Hive tabeller</span><span class="sxs-lookup"><span data-stu-id="795f0-103"><a name="heading"></a>Sample data in Azure blob containers, SQL Server, and Hive tables</span></span>
<span data-ttu-id="795f0-104">Det här dokumentet länkar tootopics som täcker hur toosample data som lagras i ett av tre olika Azure platser:</span><span class="sxs-lookup"><span data-stu-id="795f0-104">This document links tootopics that covers how toosample data that is stored in one of three different Azure locations:</span></span>

* <span data-ttu-id="795f0-105">**Azure blob-behållardata** samplas genom att hämta den programmässigt och provtagning den med Python exempelkod.</span><span class="sxs-lookup"><span data-stu-id="795f0-105">**Azure blob container data** is sampled by downloading it programmatically and then sampling it with sample Python code.</span></span>
* <span data-ttu-id="795f0-106">**SQL Server-data** samplas med både SQL och hello Python Programming språk.</span><span class="sxs-lookup"><span data-stu-id="795f0-106">**SQL Server data** is sampled using both SQL and hello Python Programming Language.</span></span> 
* <span data-ttu-id="795f0-107">**Hive tabelldata** samplas med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="795f0-107">**Hive table data** is sampled using Hive queries.</span></span>

<span data-ttu-id="795f0-108">hello följande **menyn** länkar toohello avsnitt som beskriver hur toosample data från var och en av dessa Azure storage-miljöer.</span><span class="sxs-lookup"><span data-stu-id="795f0-108">hello following **menu** links toohello topics that describe how toosample data from each of these Azure storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="795f0-109">Sampling uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="795f0-109">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

<span data-ttu-id="795f0-110">**Varför exempeldata?**</span><span class="sxs-lookup"><span data-stu-id="795f0-110">**Why sample data?**</span></span>

<span data-ttu-id="795f0-111">Om hello dataset som du planerar tooanalyze är stor, är det vanligtvis en god idé toodown sampel hello data tooreduce den tooa mindre men representativt och mer användarvänlig storlek.</span><span class="sxs-lookup"><span data-stu-id="795f0-111">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="795f0-112">Detta underlättar data förstå undersökning och funktionen tekniker.</span><span class="sxs-lookup"><span data-stu-id="795f0-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="795f0-113">Roll i hello Cortana Analytics processen är tooenable snabb prototyper hello databearbetning funktioner och maskininlärning modeller.</span><span class="sxs-lookup"><span data-stu-id="795f0-113">Its role in hello Cortana Analytics Process is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

