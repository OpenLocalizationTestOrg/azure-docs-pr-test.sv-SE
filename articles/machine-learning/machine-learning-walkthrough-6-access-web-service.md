---
title: "Steg 6: Komma åt hello på Machine Learning-webbtjänst | Microsoft Docs"
description: "Steg 6 i hello utveckla en förutsägelselösning genomgång: komma åt en aktiva Azure Machine Learning-webbtjänst."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 211de0294092c6a6b5e6eb608d5d3b88107674c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-6-access-hello-azure-machine-learning-web-service"></a><span data-ttu-id="36908-103">Genomgång steg 6: Komma åt hello Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="36908-103">Walkthrough Step 6: Access hello Azure Machine Learning web service</span></span>

<span data-ttu-id="36908-104">Detta är hello sista steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="36908-104">This is hello last step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="36908-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="36908-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="36908-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="36908-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="36908-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="36908-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="36908-108">Träna och utvärdera hello modeller</span><span class="sxs-lookup"><span data-stu-id="36908-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="36908-109">Distribuera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="36908-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. <span data-ttu-id="36908-110">**Få åtkomst till hello-webbtjänsten**</span><span class="sxs-lookup"><span data-stu-id="36908-110">**Access hello Web service**</span></span>

- - -
<span data-ttu-id="36908-111">I hello föregående steg i den här genomgången distribuerade vi en webbtjänst som använder vår kredit risk förutsägelse modell.</span><span class="sxs-lookup"><span data-stu-id="36908-111">In hello previous step in this walkthrough we deployed a web service that uses our credit risk prediction model.</span></span> <span data-ttu-id="36908-112">Nu användare kan toosend data tooit och få resultat.</span><span class="sxs-lookup"><span data-stu-id="36908-112">Now users are able toosend data tooit and receive results.</span></span> 

<span data-ttu-id="36908-113">hello Web service är en Azure-webbtjänst som kan ta emot och returnera data med hjälp av REST API: er i ett av två sätt:</span><span class="sxs-lookup"><span data-stu-id="36908-113">hello Web service is an Azure web service that can receive and return data using REST APIs in one of two ways:</span></span>  

* <span data-ttu-id="36908-114">**Frågor och svar** – hello användare skickar en eller fler rader med kredit data toohello tjänsten med hjälp av en HTTP-protokollet och hello tjänsten svarar med en eller flera uppsättningar av resultaten.</span><span class="sxs-lookup"><span data-stu-id="36908-114">**Request/Response** - hello user sends one or more rows of credit data toohello service by using an HTTP protocol, and hello service responds with one or more sets of results.</span></span>
* <span data-ttu-id="36908-115">**Batch-körningen** – hello användaren lagrar en eller fler rader med kredit data i ett Azure blob- och skickar sedan hello blob plats toohello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="36908-115">**Batch Execution** - hello user stores one or more rows of credit data in an Azure blob and then sends hello blob location toohello service.</span></span> <span data-ttu-id="36908-116">hello service poäng alla hello rader med data i Hej inkommande blob, lagrar hello resulterar i en annan blob och returnerar hello URL för behållaren.</span><span class="sxs-lookup"><span data-stu-id="36908-116">hello service scores all hello rows of data in hello input blob, stores hello results in another blob, and returns hello URL of that container.</span></span>  

<span data-ttu-id="36908-117">Hej snabbaste och enklaste sättet tooaccess en klassisk webbtjänst är via hello [Azure ML-svar på begäranden Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) eller [Azure ML Batch Execution Service Web Appmallen](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span><span class="sxs-lookup"><span data-stu-id="36908-117">hello quickest and easiest way tooaccess a Classic web service is through hello [Azure ML Request-Response Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) or [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span></span>

<span data-ttu-id="36908-118">Mallarna web app kan skapa ett anpassat webbprogram som känner din webbtjänsten indata och vad returneras.</span><span class="sxs-lookup"><span data-stu-id="36908-118">These web app templates can build a custom web app that knows your web service's input data and what it will return.</span></span> <span data-ttu-id="36908-119">Allt du behöver toodo är att ge åtkomst tooyour webbtjänsten och data och hello mallen hello rest.</span><span class="sxs-lookup"><span data-stu-id="36908-119">All you need toodo is provide access tooyour web service and data, and hello template does hello rest.</span></span>

<span data-ttu-id="36908-120">Mer information om hur du använder hello app mallar finns [använda en Azure Machine Learning webbtjänst med en mall för app](machine-learning-consume-web-service-with-web-app-template.md).</span><span class="sxs-lookup"><span data-stu-id="36908-120">For more information on using hello web app templates, see [Consume an Azure Machine Learning Web service with a web app template](machine-learning-consume-web-service-with-web-app-template.md).</span></span>

<span data-ttu-id="36908-121">Du kan också skapa ett anpassat program tooaccess hello webbtjänsten med starter koden du i R, C# och Python programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="36908-121">You can also develop a custom application tooaccess hello web service using starter code provided for you in R, C#, and Python programming languages.</span></span>

<span data-ttu-id="36908-122">Du hittar information i [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="36908-122">You can find complete details in [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

