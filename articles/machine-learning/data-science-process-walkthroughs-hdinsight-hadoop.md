---
title: "aaaHDInsight Hadoop datavetenskap genomgång med Hive i Azure | Microsoft Docs"
description: "Exempel på hello Team datavetenskap Process som går igenom hello användning av Hive på Azure HDInsight Hadoop toodo förutsägelseanalys."
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
ms.openlocfilehash: 77efbe4ea6377f309987849d9f44e8b2b859ae9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a><span data-ttu-id="3f1d0-103">HDInsight Hadoop datavetenskap genomgång med Hive på Azure</span><span class="sxs-lookup"><span data-stu-id="3f1d0-103">HDInsight Hadoop data science walkthroughs using Hive on Azure</span></span> 

<span data-ttu-id="3f1d0-104">Dessa genomgång använda Hive med förutsägelseanalyser en HDInsight Hadoop-kluster toodo.</span><span class="sxs-lookup"><span data-stu-id="3f1d0-104">These walkthroughs use Hive with an HDInsight Hadoop cluster toodo predictive analytics.</span></span> <span data-ttu-id="3f1d0-105">De följer hello steg som beskrivs i hello Team datavetenskap Process.</span><span class="sxs-lookup"><span data-stu-id="3f1d0-105">They follow hello steps outlined in hello Team Data Science Process.</span></span> <span data-ttu-id="3f1d0-106">En översikt över hello Team datavetenskap Process, se [datavetenskap Process](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3f1d0-106">For an overview of hello Team Data Science Process, see [Data Science Process](data-science-process-overview.md).</span></span> <span data-ttu-id="3f1d0-107">En introduktion tooAzure HDInsight finns [introduktion tooAzure HDInsight, hello Hadoop-teknikstacken och Hadoop-kluster](../hdinsight/hdinsight-hadoop-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3f1d0-107">For an introduction tooAzure HDInsight, see [Introduction tooAzure HDInsight, hello Hadoop technology stack, and Hadoop clusters](../hdinsight/hdinsight-hadoop-introduction.md).</span></span>

<span data-ttu-id="3f1d0-108">Ytterligare datavetenskap genomgångar som kör hello Team av vetenskapliga data är grupperade efter hello **plattform** som de använder:</span><span class="sxs-lookup"><span data-stu-id="3f1d0-108">Additional data science walkthroughs that execute hello Team Data Science Process are grouped by hello **platform** that they use:</span></span> 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="3f1d0-109">Förutsäga taxi tips med Hive med HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="3f1d0-109">Predict taxi tips using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="3f1d0-110">Hej [Använd HDInsight Hadoop-kluster](machine-learning-data-science-process-hive-walkthrough.md) genomgången använder data från New York taxibilar toopredict:</span><span class="sxs-lookup"><span data-stu-id="3f1d0-110">hello [Use HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) walkthrough uses data from New York taxis toopredict:</span></span> 

- <span data-ttu-id="3f1d0-111">Om ett tips är betald</span><span class="sxs-lookup"><span data-stu-id="3f1d0-111">Whether a tip is paid</span></span> 
- <span data-ttu-id="3f1d0-112">hello fördelning av tips belopp</span><span class="sxs-lookup"><span data-stu-id="3f1d0-112">hello distribution of tip amounts</span></span>

<span data-ttu-id="3f1d0-113">hello scenariot implementeras med hjälp av Hive med en [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="3f1d0-113">hello scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/).</span></span> <span data-ttu-id="3f1d0-114">Du lär dig hur toostore, utforska, och funktionen tekniker data från en offentligt tillgängliga NYC taxi resa och färdavgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="3f1d0-114">You learn how toostore, explore, and feature engineer data from a publicly available NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="3f1d0-115">Du kan också använda Azure Machine Learning toobuild och distribuera hello modeller.</span><span class="sxs-lookup"><span data-stu-id="3f1d0-115">You also use Azure Machine Learning toobuild and deploy hello models.</span></span>

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a><span data-ttu-id="3f1d0-116">Förutsäga annons klick med Hive med HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="3f1d0-116">Predict advertisement clicks using Hive with HDInsight Hadoop</span></span>

<span data-ttu-id="3f1d0-117">Hej [Använd Azure HDInsight Hadoop-kluster på en datamängd för 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md) genomgången använder offentligt tillgängliga [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) klickar du på dataset toopredict om ett tips är betald och hello mängd belopp som förväntat.</span><span class="sxs-lookup"><span data-stu-id="3f1d0-117">hello [Use Azure HDInsight Hadoop Clusters on a 1-TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md) walkthrough uses a publicly available [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) click dataset toopredict whether a tip is paid and hello range of amounts expected.</span></span> <span data-ttu-id="3f1d0-118">hello scenariot implementeras med hjälp av Hive med en [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) toostore, utforska funktion tekniker och ned exempeldata.</span><span class="sxs-lookup"><span data-stu-id="3f1d0-118">hello scenario is implemented using Hive with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) toostore, explore, feature engineer, and down sample data.</span></span> <span data-ttu-id="3f1d0-119">Den använder Azure Machine Learning toobuild, träna och betygsätta en binär klassificering modell förutsäga om en användare klickar på en annons.</span><span class="sxs-lookup"><span data-stu-id="3f1d0-119">It uses Azure Machine Learning toobuild, train, and score a binary classification model predicting whether a user clicks on an advertisement.</span></span> <span data-ttu-id="3f1d0-120">hello genomgången slutsatsen visar hur toopublish en av dessa modeller som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="3f1d0-120">hello walkthrough concludes showing how toopublish one of these models as a Web service.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3f1d0-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3f1d0-121">Next steps</span></span>

<span data-ttu-id="3f1d0-122">En beskrivning av hello viktiga komponenter som utgör hello Team datavetenskap Process finns [Team datavetenskap processöversikt](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3f1d0-122">For a discussion of hello key components that comprise hello Team Data Science Process, see [Team Data Science Process overview](data-science-process-overview.md).</span></span>

<span data-ttu-id="3f1d0-123">En beskrivning av hello Team datavetenskap Process livscykel som du kan använda toostructure datavetenskap projekt, se [Team datavetenskap Process livscykel](data-science-process-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="3f1d0-123">For a discussion of hello Team Data Science Process lifecycle that you can use toostructure your data science projects, see [Team Data Science Process lifecycle](data-science-process-lifecycle.md).</span></span> <span data-ttu-id="3f1d0-124">hello livscykel beskrivs hello steg från start toofinish som projekt följer vanligtvis när de utförs.</span><span class="sxs-lookup"><span data-stu-id="3f1d0-124">hello lifecycle outlines hello steps, from start toofinish, that projects usually follow when they are executed.</span></span> 

