---
title: "HDInsight Hadoop datavetenskap genomgång med Hive i Azure | Microsoft Docs"
description: "Exempel på Team av vetenskapliga data som beskriver genom att använda Hive på Azure HDInsight Hadoop att göra förutsägelseanalyser."
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
ms.openlocfilehash: fb06c3c1b1ae30d970a2e4d45a49e22e9d78b876
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a><span data-ttu-id="c58ca-103">HDInsight Hadoop datavetenskap genomgång med Hive på Azure</span><span class="sxs-lookup"><span data-stu-id="c58ca-103">HDInsight Hadoop data science walkthroughs using Hive on Azure</span></span> 

<span data-ttu-id="c58ca-104">Dessa genomgång använda Hive med HDInsight Hadoop-kluster för att göra förutsägelseanalyser.</span><span class="sxs-lookup"><span data-stu-id="c58ca-104">These walkthroughs use Hive with an HDInsight Hadoop cluster to do predictive analytics.</span></span> <span data-ttu-id="c58ca-105">De följer stegen som beskrivs i Team av vetenskapliga data.</span><span class="sxs-lookup"><span data-stu-id="c58ca-105">They follow the steps outlined in the Team Data Science Process.</span></span> <span data-ttu-id="c58ca-106">En översikt över Team av vetenskapliga data, se [datavetenskap Process](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c58ca-106">For an overview of the Team Data Science Process, see [Data Science Process](data-science-process-overview.md).</span></span> <span data-ttu-id="c58ca-107">En introduktion till Azure HDInsight, se [introduktion till Azure HDInsight, Hadoop-teknikstacken och Hadoop-kluster](../hdinsight/hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c58ca-107">For an introduction to Azure HDInsight, see [Introduction to Azure HDInsight, the Hadoop technology stack, and Hadoop clusters](../hdinsight/hdinsight-hadoop-introduction.md).</span></span>

<span data-ttu-id="c58ca-108">Ytterligare datavetenskap genomgångar som kör Team datavetenskap Process grupperas efter den **plattform** som de använder:</span><span class="sxs-lookup"><span data-stu-id="c58ca-108">Additional data science walkthroughs that execute the Team Data Science Process are grouped by the **platform** that they use:</span></span> 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="c58ca-109">Förutsäga taxi tips med Hive med HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="c58ca-109">Predict taxi tips using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="c58ca-110">Den [Använd HDInsight Hadoop-kluster](machine-learning-data-science-process-hive-walkthrough.md) genomgången använder data från New York taxibilar för att förutsäga:</span><span class="sxs-lookup"><span data-stu-id="c58ca-110">The [Use HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) walkthrough uses data from New York taxis to predict:</span></span> 

- <span data-ttu-id="c58ca-111">Om ett tips är betald</span><span class="sxs-lookup"><span data-stu-id="c58ca-111">Whether a tip is paid</span></span> 
- <span data-ttu-id="c58ca-112">Distribution av tips belopp</span><span class="sxs-lookup"><span data-stu-id="c58ca-112">The distribution of tip amounts</span></span>

<span data-ttu-id="c58ca-113">Scenariot implementeras med hjälp av Hive med en [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="c58ca-113">The scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/).</span></span> <span data-ttu-id="c58ca-114">Du lär dig hur du lagrar, utforska och funktionen tekniker data från en offentligt tillgängliga NYC Taxitransport resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="c58ca-114">You learn how to store, explore, and feature engineer data from a publicly available NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="c58ca-115">Du kan också använda Azure Machine Learning för att skapa och distribuera modeller.</span><span class="sxs-lookup"><span data-stu-id="c58ca-115">You also use Azure Machine Learning to build and deploy the models.</span></span>

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="c58ca-116">Förutsäga annons klick med Hive med HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="c58ca-116">Predict advertisement clicks using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="c58ca-117">Den [Använd Azure HDInsight Hadoop-kluster på en datamängd för 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md) genomgången använder offentligt tillgängliga [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) klickar du på dataset för att förutsäga om ett tips är betald och intervallet för belopp som förväntat.</span><span class="sxs-lookup"><span data-stu-id="c58ca-117">The [Use Azure HDInsight Hadoop Clusters on a 1-TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md) walkthrough uses a publicly available [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) click dataset to predict whether a tip is paid and the range of amounts expected.</span></span> <span data-ttu-id="c58ca-118">Scenariot implementeras med hjälp av Hive med en [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) för att lagra, utforska funktion tekniker och ned exempeldata.</span><span class="sxs-lookup"><span data-stu-id="c58ca-118">The scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) to store, explore, feature engineer, and down sample data.</span></span> <span data-ttu-id="c58ca-119">Azure Machine Learning används för att skapa, träna och betygsätta en binär klassificering modell förutsäga om en användare klickar på en annons.</span><span class="sxs-lookup"><span data-stu-id="c58ca-119">It uses Azure Machine Learning to build, train, and score a binary classification model predicting whether a user clicks on an advertisement.</span></span> <span data-ttu-id="c58ca-120">Avslutar den här genomgången visar hur du publicerar en av dessa modeller som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="c58ca-120">The walkthrough concludes showing how to publish one of these models as a Web service.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c58ca-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c58ca-121">Next steps</span></span>

<span data-ttu-id="c58ca-122">En beskrivning av de viktiga komponenter som utgör teamet datavetenskap Process finns [Team datavetenskap processöversikt](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c58ca-122">For a discussion of the key components that comprise the Team Data Science Process, see [Team Data Science Process overview](data-science-process-overview.md).</span></span>

<span data-ttu-id="c58ca-123">En beskrivning av livscykeln Team av vetenskapliga data som du kan använda för att strukturera datavetenskap projekt finns [Team datavetenskap Process livscykel](data-science-process-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="c58ca-123">For a discussion of the Team Data Science Process lifecycle that you can use to structure your data science projects, see [Team Data Science Process lifecycle](data-science-process-lifecycle.md).</span></span> <span data-ttu-id="c58ca-124">Livscykeln beskrivs stegen från början till slut att projekt vanligtvis följer när de utförs.</span><span class="sxs-lookup"><span data-stu-id="c58ca-124">The lifecycle outlines the steps, from start to finish, that projects usually follow when they are executed.</span></span> 

