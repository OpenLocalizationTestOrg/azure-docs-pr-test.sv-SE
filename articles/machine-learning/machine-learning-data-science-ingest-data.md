---
title: "aaaLoad data till Azure storage-miljöer för analytics | Microsoft Docs"
description: "Flytta Data tooand från Azure Blob Storage"
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
ms.openlocfilehash: 0fea2290991f9fa63d9e46c3a657000e27d95289
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a><span data-ttu-id="d28be-103">Läs in data i lagringsmiljöer för analys</span><span class="sxs-lookup"><span data-stu-id="d28be-103">Load data into storage environments for analytics</span></span>
<span data-ttu-id="d28be-104">hello Team datavetenskap processen kräver att data att inhämtas eller läses in i en mängd olika lagringsplatser miljöer toobe analyseras i hello lämpligaste sättet i varje steg i processen för hello eller bearbetades.</span><span class="sxs-lookup"><span data-stu-id="d28be-104">hello Team Data Science Process requires that data be ingested or loaded into a variety of different storage environments toobe processed or analyzed in hello most appropriate way in each stage of hello process.</span></span> <span data-ttu-id="d28be-105">Datamål som ofta används för bearbetning inkluderar Azure Blob Storage, SQL Azure-databaser, SQL Server på Azure VM, HDInsight (Hadoop) och Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d28be-105">Data destinations commonly used for processing include Azure Blob Storage, SQL Azure databases, SQL Server on Azure VM, HDInsight (Hadoop), and Azure Machine Learning.</span></span> 

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<span data-ttu-id="d28be-106">Detta **menyn** länkar tootopics som beskriver hur tooingest data till dessa mål miljöer där hello data lagras och bearbetas.</span><span class="sxs-lookup"><span data-stu-id="d28be-106">This **menu** links tootopics that describe how tooingest data into these target environments where hello data is stored and processed.</span></span>

<span data-ttu-id="d28be-107">Teknisk och affärsbehov samt hello ursprungliga plats, formatera och storleken på dina data avgör hello mål miljöer i vilka hello data måste toobe inhämtas tooachieve hello mål av dina analyser.</span><span class="sxs-lookup"><span data-stu-id="d28be-107">Technical and business needs, as well as hello initial location, format and size of your data will determine hello target environments into which hello data needs toobe ingested tooachieve hello goals of your analysis.</span></span> <span data-ttu-id="d28be-108">Det är inte ovanligt att en scenariot toorequire data toobe flyttas mellan flera miljöer tooachieve hello olika uppgifter krävs tooconstruct en förutsägelsemodell.</span><span class="sxs-lookup"><span data-stu-id="d28be-108">It is not uncommon for a scenario toorequire data toobe moved between several environments tooachieve hello variety of tasks required tooconstruct a predictive model.</span></span> <span data-ttu-id="d28be-109">Den här aktivitetssekvensen kan exempelvis innehålla datagranskning, föregående bearbetning, rensa, ned provtagning och modellen utbildning.</span><span class="sxs-lookup"><span data-stu-id="d28be-109">This sequence of tasks can include, for example, data exploration, pre-processing, cleaning, down-sampling, and model training.</span></span>

