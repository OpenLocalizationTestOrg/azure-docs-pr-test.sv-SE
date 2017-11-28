---
title: "Steg 6: Få åtkomst till webbtjänsten för Machine Learning | Microsoft Docs"
description: "Steg 6 i utveckla en förutsägelselösning genomgång: komma åt en aktiva Azure Machine Learning-webbtjänst."
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
ms.openlocfilehash: d309f6c4749a80c81859b693a2bd5927e8fe0e54
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a><span data-ttu-id="86f3d-103">Genomgång steg 6: Få åtkomst till Azure Machine Learning-webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="86f3d-103">Walkthrough Step 6: Access the Azure Machine Learning web service</span></span>

<span data-ttu-id="86f3d-104">Detta är det sista steget i den här genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="86f3d-104">This is the last step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="86f3d-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="86f3d-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="86f3d-106">Överför befintliga data</span><span class="sxs-lookup"><span data-stu-id="86f3d-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="86f3d-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="86f3d-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="86f3d-108">Träna och utvärdera modellerna</span><span class="sxs-lookup"><span data-stu-id="86f3d-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="86f3d-109">Distribuera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="86f3d-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. <span data-ttu-id="86f3d-110">**Få åtkomst till webbtjänsten**</span><span class="sxs-lookup"><span data-stu-id="86f3d-110">**Access the Web service**</span></span>

- - -
<span data-ttu-id="86f3d-111">I föregående steg i den här genomgången distribuerade vi en webbtjänst som använder vår kredit risk förutsägelse modell.</span><span class="sxs-lookup"><span data-stu-id="86f3d-111">In the previous step in this walkthrough we deployed a web service that uses our credit risk prediction model.</span></span> <span data-ttu-id="86f3d-112">Användare ska nu kunna skicka data till den och få resultat.</span><span class="sxs-lookup"><span data-stu-id="86f3d-112">Now users are able to send data to it and receive results.</span></span> 

<span data-ttu-id="86f3d-113">Webbtjänsten är en Azure-webbtjänst som kan ta emot och returnera data med hjälp av REST API: er i ett av två sätt:</span><span class="sxs-lookup"><span data-stu-id="86f3d-113">The Web service is an Azure web service that can receive and return data using REST APIs in one of two ways:</span></span>  

* <span data-ttu-id="86f3d-114">**Frågor och svar** – användaren skickar en eller flera datarader kredit till tjänsten med hjälp av en HTTP-protokollet och tjänsten svarar med en eller flera uppsättningar av resultaten.</span><span class="sxs-lookup"><span data-stu-id="86f3d-114">**Request/Response** - The user sends one or more rows of credit data to the service by using an HTTP protocol, and the service responds with one or more sets of results.</span></span>
* <span data-ttu-id="86f3d-115">**Batch-körningen** – användaren lagrar en eller fler rader med kredit data i ett Azure blob- och skickar sedan blobbplats till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="86f3d-115">**Batch Execution** - The user stores one or more rows of credit data in an Azure blob and then sends the blob location to the service.</span></span> <span data-ttu-id="86f3d-116">Tjänsten poäng alla rader med data i indata blob, lagrar resultaten i en annan blob och returnerar URL för behållaren.</span><span class="sxs-lookup"><span data-stu-id="86f3d-116">The service scores all the rows of data in the input blob, stores the results in another blob, and returns the URL of that container.</span></span>  

<span data-ttu-id="86f3d-117">Det snabbaste och enklaste sättet att komma åt en klassisk webbtjänst är via den [Azure ML-svar på begäranden Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) eller [Azure ML Batch Execution Service Web Appmallen](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span><span class="sxs-lookup"><span data-stu-id="86f3d-117">The quickest and easiest way to access a Classic web service is through the [Azure ML Request-Response Service Web App](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) or [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).</span></span>

<span data-ttu-id="86f3d-118">Mallarna web app kan skapa ett anpassat webbprogram som känner din webbtjänsten indata och vad returneras.</span><span class="sxs-lookup"><span data-stu-id="86f3d-118">These web app templates can build a custom web app that knows your web service's input data and what it will return.</span></span> <span data-ttu-id="86f3d-119">Allt du behöver göra är att ge åtkomst till webbtjänsten och data och mallen gör resten.</span><span class="sxs-lookup"><span data-stu-id="86f3d-119">All you need to do is provide access to your web service and data, and the template does the rest.</span></span>

<span data-ttu-id="86f3d-120">Mer information om hur du använder app webbmallar finns [använda en Azure Machine Learning webbtjänst med en mall för app](machine-learning-consume-web-service-with-web-app-template.md).</span><span class="sxs-lookup"><span data-stu-id="86f3d-120">For more information on using the web app templates, see [Consume an Azure Machine Learning Web service with a web app template](machine-learning-consume-web-service-with-web-app-template.md).</span></span>

<span data-ttu-id="86f3d-121">Du kan också skapa ett anpassat program för att få åtkomst till webbtjänsten genom att använda starter code enligt du R, C# och Python programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="86f3d-121">You can also develop a custom application to access the web service using starter code provided for you in R, C#, and Python programming languages.</span></span>

<span data-ttu-id="86f3d-122">Du hittar information i [använda en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="86f3d-122">You can find complete details in [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

