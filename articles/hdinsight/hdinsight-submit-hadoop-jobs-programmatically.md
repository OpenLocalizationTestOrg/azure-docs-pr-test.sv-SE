---
title: Skicka Hadoop-jobb i HDInsight | Microsoft Docs
description: "Lär dig mer om att skicka Hadoop-jobb på Azure HDInsight Hadoop."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 50430b96-2329-4775-9713-19c5795b775f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 6829ff82afc7fcea9e027ad14ec7ed0c8015a5fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="submit-hadoop-jobs-in-hdinsight"></a><span data-ttu-id="54d2d-103">Skicka Hadoop-jobb i HDInsight</span><span class="sxs-lookup"><span data-stu-id="54d2d-103">Submit Hadoop jobs in HDInsight</span></span>

<span data-ttu-id="54d2d-104">Du kan skicka Hadoop-jobb med hjälp av .NET SDK och Curl Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="54d2d-104">You can submit Hadoop jobs using .NET SDK, Curl, and Azure PowerShell:</span></span>

- <span data-ttu-id="54d2d-105">Använd .NET SDK</span><span class="sxs-lookup"><span data-stu-id="54d2d-105">Use .NET SDK</span></span>

  - [<span data-ttu-id="54d2d-106">Skapa icke-interaktiv autentisering .NET-program</span><span class="sxs-lookup"><span data-stu-id="54d2d-106">Create non-interactive authentication .NET applications</span></span>](hdinsight-create-non-interactive-authentication-dotnet-applications.md)
  - [<span data-ttu-id="54d2d-107">Köra Hive-frågor med HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="54d2d-107">Run Hive queries using HDInsight .NET SDK</span></span>](hdinsight-hadoop-use-hive-dotnet-sdk.md)
  - [<span data-ttu-id="54d2d-108">Köra Pig-jobb med hjälp av .NET SDK för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="54d2d-108">Run Pig jobs using the .NET SDK for Hadoop in HDInsight</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md)
  - [<span data-ttu-id="54d2d-109">Kör Sqoop jobb med hjälp av .NET SDK för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="54d2d-109">Run Sqoop jobs using .NET SDK for Hadoop in HDInsight</span></span>](hdinsight-hadoop-use-sqoop-dotnet-sdk.md)
  - [<span data-ttu-id="54d2d-110">Kör jobb för MapReduce med HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="54d2d-110">Run MapReduce jobs using HDInsight .NET SDK</span></span>](hdinsight-hadoop-use-mapreduce-dotnet-sdk.md)

- <span data-ttu-id="54d2d-111">CURL</span><span class="sxs-lookup"><span data-stu-id="54d2d-111">CURL</span></span>

  - [<span data-ttu-id="54d2d-112">Köra Hive-frågor med Hadoop i HDInsight med Curl</span><span class="sxs-lookup"><span data-stu-id="54d2d-112">Run Hive queries with Hadoop in HDInsight with Curl</span></span>](hdinsight-hadoop-use-hive-curl.md)
  - [<span data-ttu-id="54d2d-113">Köra Pig med Hadoop på HDInsight med Curl</span><span class="sxs-lookup"><span data-stu-id="54d2d-113">Run Pig jobs with Hadoop on HDInsight by using Curl</span></span>](hdinsight-hadoop-use-pig-curl.md)
  - [<span data-ttu-id="54d2d-114">Kör jobb för Sqoop med Hadoop i HDInsight med Curl</span><span class="sxs-lookup"><span data-stu-id="54d2d-114">Run Sqoop jobs with Hadoop in HDInsight with Curl</span></span>](hdinsight-hadoop-use-sqoop-curl.md)
  - [<span data-ttu-id="54d2d-115">Kör jobb för MapReduce med Hadoop i HDInsight med Curl</span><span class="sxs-lookup"><span data-stu-id="54d2d-115">Run MapReduce jobs with Hadoop on HDInsight using Curl</span></span>](hdinsight-hadoop-use-mapreduce-curl.md)

- <span data-ttu-id="54d2d-116">PowerShell</span><span class="sxs-lookup"><span data-stu-id="54d2d-116">PowerShell</span></span>

  - [<span data-ttu-id="54d2d-117">Köra Hive-frågor med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="54d2d-117">Run Hive queries using PowerShell</span></span>](hdinsight-hadoop-use-hive-powershell.md)
  - [<span data-ttu-id="54d2d-118">Köra Pig-jobb med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="54d2d-118">Run Pig jobs using PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md)
  - [<span data-ttu-id="54d2d-119">Använda Sqoop med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="54d2d-119">Use Sqoop with Hadoop in HDInsight</span></span>](hdinsight-hadoop-use-sqoop-powershell.md)
  - [<span data-ttu-id="54d2d-120">Kör jobb för MapReduce med Hadoop i HDInsight med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="54d2d-120">Run MapReduce jobs with Hadoop on HDInsight using PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md)

## <a name="see-also"></a><span data-ttu-id="54d2d-121">Se även</span><span class="sxs-lookup"><span data-stu-id="54d2d-121">See also</span></span>

- [<span data-ttu-id="54d2d-122">Azure HDInsight-dokumentation</span><span class="sxs-lookup"><span data-stu-id="54d2d-122">Azure HDInsight Documentation</span></span>](https://docs.microsoft.com/azure/hdinsight/)