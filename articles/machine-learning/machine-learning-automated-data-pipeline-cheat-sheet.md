---
title: Azure Machine Learning automated data pipeline fusklapp | Microsoft Docs
description: "En utskrivbar fusklapp som visar hur du ställer in en pipeline för automatisk data till din Azure Machine Learning-webbtjänst om dina data är lokalt, strömning i Azure, eller i en fristående molntjänst."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 22674d6b-4491-4805-a3ac-d423611177bb
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mithal;garye
ms.openlocfilehash: e212b2c935eb0ae64ed1cd2e6dc1564f8fcd503b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="cheat-sheet-for-an-automated-data-pipeline-for-azure-machine-learning-predictions"></a><span data-ttu-id="2b2ce-103">Facit för en automatiserad datapipeline för Azure Machine Learning-förutsägelser</span><span class="sxs-lookup"><span data-stu-id="2b2ce-103">Cheat sheet for an automated data pipeline for Azure Machine Learning predictions</span></span>
<span data-ttu-id="2b2ce-104">Den **Microsoft Azure Machine Learning automated data pipeline fusklapp** kan du bläddra igenom den teknik som du kan använda för att hämta data till Machine Learning-webbtjänst där den kan bedömas av din prediktiva analysmodell.</span><span class="sxs-lookup"><span data-stu-id="2b2ce-104">The **Microsoft Azure Machine Learning automated data pipeline cheat sheet** helps you navigate through the technology you can use to get your data to your Machine Learning web service where it can be scored by your predictive analytics model.</span></span>

<span data-ttu-id="2b2ce-105">Beroende på om dina data är lokalt, i molnet, eller direktuppspelning realtid, det finns olika metoder som är tillgängliga för att flytta data till din slutpunkt för webbtjänsten för resultatfunktioner.</span><span class="sxs-lookup"><span data-stu-id="2b2ce-105">Depending on whether your data is on-premises, in the cloud, or streaming real-time, there are different mechanisms available to move the data to your web service endpoint for scoring.</span></span>
<span data-ttu-id="2b2ce-106">Den här fusklapp vägleder dig genom de beslut du måste fatta och erbjuder länkar till artiklar som hjälper dig att utveckla din lösning.</span><span class="sxs-lookup"><span data-stu-id="2b2ce-106">This cheat sheet walks you through the decisions you need to make, and it offers links to articles that can help you develop your solution.</span></span>

## <a name="download-the-machine-learning-automated-data-pipeline-cheat-sheet"></a><span data-ttu-id="2b2ce-107">Hämta fusklapp för Machine Learning automatiserade data pipeline</span><span class="sxs-lookup"><span data-stu-id="2b2ce-107">Download the Machine Learning automated data pipeline cheat sheet</span></span>
<span data-ttu-id="2b2ce-108">När du har hämtat fusklapp du skriva ut det i tabloidformat (11 x 17 i).</span><span class="sxs-lookup"><span data-stu-id="2b2ce-108">Once you download the cheat sheet, you can print it in tabloid size (11 x 17 in.).</span></span>

<span data-ttu-id="2b2ce-109">Hämta fusklapp här:  **[Microsoft Azure Machine Learning automated data pipeline fusklapp](http://download.microsoft.com/download/C/C/7/CC726F8B-2E6F-4C20-9B6F-AFBEE8253023/microsoft-machine-learning-operationalization-cheat-sheet_v1.pdf)**</span><span class="sxs-lookup"><span data-stu-id="2b2ce-109">Download the cheat sheet here: **[Microsoft Azure Machine Learning automated data pipeline cheat sheet](http://download.microsoft.com/download/C/C/7/CC726F8B-2E6F-4C20-9B6F-AFBEE8253023/microsoft-machine-learning-operationalization-cheat-sheet_v1.pdf)**</span></span>

![Översikt över funktioner i Microsoft Azure Machine Learning Studio][op-cheat-sheet]

[op-cheat-sheet]: ./media/machine-learning-automated-data-pipeline-cheat-sheet/machine-learning-automated-data-pipeline-cheat-sheet_v1.1.png


## <a name="more-help-with-machine-learning-studio"></a><span data-ttu-id="2b2ce-111">Mer hjälp med Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="2b2ce-111">More help with Machine Learning Studio</span></span>
* <span data-ttu-id="2b2ce-112">En översikt över Microsoft Azure Machine Learning, se [introduktion till maskininlärning på Microsoft Azure](machine-learning-what-is-machine-learning.md).</span><span class="sxs-lookup"><span data-stu-id="2b2ce-112">For an overview of Microsoft Azure Machine Learning, see [Introduction to machine learning on Microsoft Azure](machine-learning-what-is-machine-learning.md).</span></span>
* <span data-ttu-id="2b2ce-113">En förklaring av hur du distribuerar en bedömningsprofil webbtjänst finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="2b2ce-113">For an explanation of how to deploy a scoring web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="2b2ce-114">En beskrivning av hur du använder en bedömningsprofil webbtjänst finns [använda en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="2b2ce-114">For a discussion of how to consume a scoring web service, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

