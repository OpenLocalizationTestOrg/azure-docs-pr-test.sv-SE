---
title: "Läs in data till Azure storage-miljöer för analytics | Microsoft Docs"
description: "Flytta data till och från Azure Blob Storage"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 7fbf3bfedca8fa57a5e9428c9399558992b4acbd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a><span data-ttu-id="9217d-103">Läs in data i lagringsmiljöer för analys</span><span class="sxs-lookup"><span data-stu-id="9217d-103">Load data into storage environments for analytics</span></span>
<span data-ttu-id="9217d-104">Team av vetenskapliga data kräver att data ska inhämtas eller läses in i en mängd olika lagringsplatser miljöer bearbetas eller analyseras i det lämpligaste sättet i varje steg i processen.</span><span class="sxs-lookup"><span data-stu-id="9217d-104">The Team Data Science Process requires that data be ingested or loaded into a variety of different storage environments to be processed or analyzed in the most appropriate way in each stage of the process.</span></span> <span data-ttu-id="9217d-105">Datamål som ofta används för bearbetning inkluderar Azure Blob Storage, SQL Azure-databaser, SQL Server på Azure VM, HDInsight (Hadoop) och Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9217d-105">Data destinations commonly used for processing include Azure Blob Storage, SQL Azure databases, SQL Server on Azure VM, HDInsight (Hadoop), and Azure Machine Learning.</span></span> 

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="9217d-106">Detta **menyn** länkar till avsnitt som beskriver hur du mata in data i dessa mål miljöer där data lagras och bearbetas.</span><span class="sxs-lookup"><span data-stu-id="9217d-106">This **menu** links to topics that describe how to ingest data into these target environments where the data is stored and processed.</span></span>

<span data-ttu-id="9217d-107">Teknisk och affärsbehov samt deras ursprungliga plats formatera och storleken på dina data avgör mål-miljöer där data behöver inhämtas för att uppnå målen för din analys.</span><span class="sxs-lookup"><span data-stu-id="9217d-107">Technical and business needs, as well as the initial location, format and size of your data will determine the target environments into which the data needs to be ingested to achieve the goals of your analysis.</span></span> <span data-ttu-id="9217d-108">Det är inte ovanligt att ett scenario för kräver att data flyttas mellan flera miljöer för olika uppgifter som krävs för att skapa en förutsägelsemodell.</span><span class="sxs-lookup"><span data-stu-id="9217d-108">It is not uncommon for a scenario to require data to be moved between several environments to achieve the variety of tasks required to construct a predictive model.</span></span> <span data-ttu-id="9217d-109">Den här aktivitetssekvensen kan exempelvis innehålla datagranskning, föregående bearbetning, rensa, ned provtagning och modellen utbildning.</span><span class="sxs-lookup"><span data-stu-id="9217d-109">This sequence of tasks can include, for example, data exploration, pre-processing, cleaning, down-sampling, and model training.</span></span>

