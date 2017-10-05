---
title: "Skapa slutpunkter för webbtjänster i Machine Learning | Microsoft Docs"
description: "Skapa slutpunkter för webbtjänster i Azure Machine Learning"
services: machine-learning
documentationcenter: 
author: hiteshmadan
manager: padou
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.author: himad
ms.openlocfilehash: 9f83ffc9cf7dbe37c1ce9980fd7f5b9133fe78f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="creating-endpoints"></a><span data-ttu-id="7fe00-103">Skapa slutpunkter</span><span class="sxs-lookup"><span data-stu-id="7fe00-103">Creating Endpoints</span></span>
> [!NOTE]
>  <span data-ttu-id="7fe00-104">Det här avsnittet beskrivs metoder som gäller för en **klassiska** Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="7fe00-104">This topic describes techniques applicable to a **Classic** Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="7fe00-105">Du måste ange tränade modeller för varje kund som fortfarande är kopplade till experiment som webbtjänsten skapades från när du skapar webbtjänster som du säljer vidarebefordra till dina kunder.</span><span class="sxs-lookup"><span data-stu-id="7fe00-105">When you create Web services that you sell forward to your customers, you need to provide trained models to each customer that are still linked to the experiment from which the Web service was created.</span></span> <span data-ttu-id="7fe00-106">Dessutom tillämpas alla uppdateringar experimentet selektivt till en slutpunkt utan att skriva över anpassningar.</span><span class="sxs-lookup"><span data-stu-id="7fe00-106">In addition, any updates to the experiment should be applied selectively to an endpoint without overwriting the customizations.</span></span>

<span data-ttu-id="7fe00-107">För att åstadkomma detta kan du skapa flera slutpunkter för en distribuerad webbtjänst i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7fe00-107">To accomplish this, Azure Machine Learning allows you to create multiple endpoints for a deployed Web service.</span></span> <span data-ttu-id="7fe00-108">Varje slutpunkt i webbtjänsten är oberoende av varandra åtgärdas, begränsas och hanteras.</span><span class="sxs-lookup"><span data-stu-id="7fe00-108">Each endpoint in the Web service is independently addressed, throttled, and managed.</span></span> <span data-ttu-id="7fe00-109">Varje slutpunkt är en unik URL och auktoriseringsnyckeln som du kan distribuera till dina kunder.</span><span class="sxs-lookup"><span data-stu-id="7fe00-109">Each endpoint is a unique URL and authorization key that you can distribute to your customers.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a><span data-ttu-id="7fe00-110">Att lägga till slutpunkter till en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="7fe00-110">Adding endpoints to a Web service</span></span>
<span data-ttu-id="7fe00-111">Det finns tre sätt att lägga till en slutpunkt till en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="7fe00-111">There are three ways to add an endpoint to a Web service.</span></span>

* <span data-ttu-id="7fe00-112">Programmässigt</span><span class="sxs-lookup"><span data-stu-id="7fe00-112">Programmatically</span></span>
* <span data-ttu-id="7fe00-113">Via portalen Azure Machine Learning-webbtjänster</span><span class="sxs-lookup"><span data-stu-id="7fe00-113">Through the Azure Machine Learning Web Services portal</span></span>
* <span data-ttu-id="7fe00-114">Även om den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7fe00-114">Though the Azure classic portal</span></span>

<span data-ttu-id="7fe00-115">När slutpunkten har skapats kan du använda via synkron API: er, batch-API: er, och excel-kalkylblad.</span><span class="sxs-lookup"><span data-stu-id="7fe00-115">Once the endpoint is created, you can consume it through synchronous APIs, batch APIs, and excel worksheets.</span></span> <span data-ttu-id="7fe00-116">Förutom att lägga till slutpunkter via Användargränssnittet, kan du också använda Endpoint Management-API: er att programmatiskt lägga till slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="7fe00-116">In addition to adding endpoints through this UI, you can also use the Endpoint Management APIs to programmatically add endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="7fe00-117">Om du har lagt till ytterligare slutpunkter webbtjänsten kan du inte ta bort standardslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7fe00-117">If you have added additional endpoints to the Web service, you cannot delete the default endpoint.</span></span>
> 
> 

## <a name="adding-an-endpoint-programmatically"></a><span data-ttu-id="7fe00-118">Lägga till en slutpunkt programmässigt</span><span class="sxs-lookup"><span data-stu-id="7fe00-118">Adding an endpoint programmatically</span></span>
<span data-ttu-id="7fe00-119">Du kan lägga till en slutpunkt för webbtjänsten via programmering med hjälp av [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) exempelkod.</span><span class="sxs-lookup"><span data-stu-id="7fe00-119">You can add an endpoint to your Web service programmatically using the [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a><span data-ttu-id="7fe00-120">Lägga till en slutpunkt med hjälp av Azure Machine Learning-webbtjänster-portalen</span><span class="sxs-lookup"><span data-stu-id="7fe00-120">Adding an endpoint using the Azure Machine Learning Web Services portal</span></span>
1. <span data-ttu-id="7fe00-121">Klicka på webbtjänster på kolumnen navigeringen till vänster i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="7fe00-121">In Machine Learning Studio, on the left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="7fe00-122">Längst ned i Web service instrumentpanelen klickar du på **hantera slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="7fe00-122">At the bottom of the Web service dashboard, click **Manage endpoints**.</span></span> <span data-ttu-id="7fe00-123">Azure Machine Learning-webbtjänster portal öppnar sidan slutpunkter för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7fe00-123">The Azure Machine Learning Web Services portal opens to the endpoints page for the Web service.</span></span>
3. <span data-ttu-id="7fe00-124">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="7fe00-124">Click **New**.</span></span>
4. <span data-ttu-id="7fe00-125">Skriv ett namn och beskrivning för den nya slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7fe00-125">Type a name and description for the new endpoint.</span></span> <span data-ttu-id="7fe00-126">Slutpunkten namn måste vara 24 tecken eller mindre längd och måste bestå av gemena bokstäver eller siffror.</span><span class="sxs-lookup"><span data-stu-id="7fe00-126">Endpoint names must be 24 character or less in length, and must be made up of lower-case alphabets or numbers.</span></span> <span data-ttu-id="7fe00-127">Välj loggningsnivån och om exempeldata är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="7fe00-127">Select the logging level and whether sample data is enabled.</span></span> <span data-ttu-id="7fe00-128">Mer information om loggning finns [aktivera loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="7fe00-128">For more information on logging, see [Enable logging for Machine Learning Web services](machine-learning-web-services-logging.md).</span></span>

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a><span data-ttu-id="7fe00-129">Lägga till en slutpunkt som använder den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7fe00-129">Adding an endpoint using the Azure classic portal</span></span>
1. <span data-ttu-id="7fe00-130">Logga in på den [klassiska Azure-portalen](http://manage.windowsazure.com), klickar du på **Maskininlärning** i den vänstra kolumnen.</span><span class="sxs-lookup"><span data-stu-id="7fe00-130">Sign in to the [Azure classic portal](http://manage.windowsazure.com), click **Machine Learning** in the left column.</span></span> <span data-ttu-id="7fe00-131">Klicka på arbetsytan som innehåller den webbtjänst som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="7fe00-131">Click the workspace which contains the Web service in which you are interested.</span></span>
   
    ![Gå till arbetsytan](./media/machine-learning-create-endpoint/figure-1.png)
2. <span data-ttu-id="7fe00-133">Klicka på **webbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="7fe00-133">Click **Web Services**.</span></span>
   
    ![Navigera till webbtjänster](./media/machine-learning-create-endpoint/figure-2.png)
3. <span data-ttu-id="7fe00-135">Klicka på den webbtjänst som du är intresserad av att se en lista över tillgängliga slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="7fe00-135">Click the Web service you're interested in to see the list of available endpoints.</span></span>
   
    ![Navigera till slutpunkten](./media/machine-learning-create-endpoint/figure-3.png)
4. <span data-ttu-id="7fe00-137">Längst ned på sidan klickar du på **Lägg till slutpunkt**.</span><span class="sxs-lookup"><span data-stu-id="7fe00-137">At the bottom of the page, click **Add Endpoint**.</span></span> <span data-ttu-id="7fe00-138">Ange ett namn och beskrivning, se till att det finns inga slutpunkter med samma namn i den här webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7fe00-138">Type a name and description, ensure there are no other endpoints with the same name in this Web service.</span></span> <span data-ttu-id="7fe00-139">Låt nivån begränsning med sitt standardvärde om du inte har särskilda krav.</span><span class="sxs-lookup"><span data-stu-id="7fe00-139">Leave the throttle level with its default value unless you have special requirements.</span></span> <span data-ttu-id="7fe00-140">Mer information om hur du begränsar finns [skalning API-slutpunkter](machine-learning-scaling-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="7fe00-140">To learn more about throttling, see [Scaling API Endpoints](machine-learning-scaling-webservice.md).</span></span>
   
    ![Skapa slutpunkten](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a><span data-ttu-id="7fe00-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7fe00-142">Next Steps</span></span>
<span data-ttu-id="7fe00-143">[Använda en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="7fe00-143">[How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

