---
title: "Samla in data i Azure blob-behållare, SQL Server och Hive tabeller | Microsoft Docs"
description: "Så här utforska data som lagras i olika Azure enviromnents."
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
ms.openlocfilehash: 0683be564a88ef54882e8d882d196851ecac243d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="d941c-103"><a name="heading"></a>Exempeldata i Azure blob-behållare, SQL Server och Hive tabeller</span><span class="sxs-lookup"><span data-stu-id="d941c-103"><a name="heading"></a>Sample data in Azure blob containers, SQL Server, and Hive tables</span></span>
<span data-ttu-id="d941c-104">Det här dokumentlänkar till avsnitt som beskriver hur du samla in data som lagras i något av tre olika Azure platser:</span><span class="sxs-lookup"><span data-stu-id="d941c-104">This document links to topics that covers how to sample data that is stored in one of three different Azure locations:</span></span>

* <span data-ttu-id="d941c-105">**Azure blob-behållardata** samplas genom att hämta den programmässigt och provtagning den med Python exempelkod.</span><span class="sxs-lookup"><span data-stu-id="d941c-105">**Azure blob container data** is sampled by downloading it programmatically and then sampling it with sample Python code.</span></span>
* <span data-ttu-id="d941c-106">**SQL Server-data** samplas med både SQL och Python Programming språk.</span><span class="sxs-lookup"><span data-stu-id="d941c-106">**SQL Server data** is sampled using both SQL and the Python Programming Language.</span></span> 
* <span data-ttu-id="d941c-107">**Hive tabelldata** samplas med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="d941c-107">**Hive table data** is sampled using Hive queries.</span></span>

<span data-ttu-id="d941c-108">Följande **menyn** länkar till avsnitt som beskriver hur du exempeldata från var och en av dessa Azure storage-miljöer.</span><span class="sxs-lookup"><span data-stu-id="d941c-108">The following **menu** links to the topics that describe how to sample data from each of these Azure storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="d941c-109">Sampling uppgiften är ett steg i den [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="d941c-109">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

<span data-ttu-id="d941c-110">**Varför exempeldata?**</span><span class="sxs-lookup"><span data-stu-id="d941c-110">**Why sample data?**</span></span>

<span data-ttu-id="d941c-111">Om datamängden som du planerar att analysera är stort, men det är vanligtvis en bra idé att ned-sample data för att minska det till en mindre men representativt och mer användarvänlig storlek.</span><span class="sxs-lookup"><span data-stu-id="d941c-111">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="d941c-112">Detta underlättar data förstå undersökning och funktionen tekniker.</span><span class="sxs-lookup"><span data-stu-id="d941c-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="d941c-113">Roll i Cortana Analytics processen är att aktivera snabb prototyper för databearbetning funktions- och maskininlärning modeller.</span><span class="sxs-lookup"><span data-stu-id="d941c-113">Its role in the Cortana Analytics Process is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

