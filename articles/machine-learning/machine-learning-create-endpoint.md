---
title: "aaaCreating slutpunkter för webbtjänster i Machine Learning | Microsoft Docs"
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
ms.openlocfilehash: 10a2bc586c6fe35e28d8bf0293854c578827c453
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-endpoints"></a><span data-ttu-id="16572-103">Skapa slutpunkter</span><span class="sxs-lookup"><span data-stu-id="16572-103">Creating Endpoints</span></span>
> [!NOTE]
>  <span data-ttu-id="16572-104">Det här avsnittet beskrivs tekniker tillämpliga tooa **klassiska** Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="16572-104">This topic describes techniques applicable tooa **Classic** Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="16572-105">När du skapar webbtjänster som du säljer vidarebefordra tooyour kunder måste tooprovide tränade modeller tooeach kunden är fortfarande länkade toohello experiment från vilka hello Web service skapades.</span><span class="sxs-lookup"><span data-stu-id="16572-105">When you create Web services that you sell forward tooyour customers, you need tooprovide trained models tooeach customer that are still linked toohello experiment from which hello Web service was created.</span></span> <span data-ttu-id="16572-106">Dessutom tillämpas uppdateringar toohello experiment ska selektivt tooan endpoint utan att skriva över hello anpassningar.</span><span class="sxs-lookup"><span data-stu-id="16572-106">In addition, any updates toohello experiment should be applied selectively tooan endpoint without overwriting hello customizations.</span></span>

<span data-ttu-id="16572-107">tooaccomplish, Azure Machine Learning kan du toocreate flera slutpunkter för en distribuerad webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="16572-107">tooaccomplish this, Azure Machine Learning allows you toocreate multiple endpoints for a deployed Web service.</span></span> <span data-ttu-id="16572-108">Varje slutpunkt i hello webbtjänsten är oberoende av varandra åtgärdas, begränsas och hanteras.</span><span class="sxs-lookup"><span data-stu-id="16572-108">Each endpoint in hello Web service is independently addressed, throttled, and managed.</span></span> <span data-ttu-id="16572-109">Varje slutpunkt är en unik URL och auktorisering nyckel som du distribuerar tooyour kunder.</span><span class="sxs-lookup"><span data-stu-id="16572-109">Each endpoint is a unique URL and authorization key that you can distribute tooyour customers.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-tooa-web-service"></a><span data-ttu-id="16572-110">Att lägga till slutpunkter tooa Web service</span><span class="sxs-lookup"><span data-stu-id="16572-110">Adding endpoints tooa Web service</span></span>
<span data-ttu-id="16572-111">Det finns tre sätt tooadd en slutpunkt tooa webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="16572-111">There are three ways tooadd an endpoint tooa Web service.</span></span>

* <span data-ttu-id="16572-112">Programmässigt</span><span class="sxs-lookup"><span data-stu-id="16572-112">Programmatically</span></span>
* <span data-ttu-id="16572-113">Hello Azure Machine Learning-webbtjänster-portalen</span><span class="sxs-lookup"><span data-stu-id="16572-113">Through hello Azure Machine Learning Web Services portal</span></span>
* <span data-ttu-id="16572-114">Även om hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="16572-114">Though hello Azure classic portal</span></span>

<span data-ttu-id="16572-115">När hello slutpunkten har skapats kan du använda via synkron API: er, batch-API: er, och excel-kalkylblad.</span><span class="sxs-lookup"><span data-stu-id="16572-115">Once hello endpoint is created, you can consume it through synchronous APIs, batch APIs, and excel worksheets.</span></span> <span data-ttu-id="16572-116">Dessutom tooadding slutpunkter via Användargränssnittet, du kan också använda hello Endpoint Management API: er tooprogrammatically lägga till slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="16572-116">In addition tooadding endpoints through this UI, you can also use hello Endpoint Management APIs tooprogrammatically add endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="16572-117">Om du har lagt till ytterligare slutpunkter toohello webbtjänsten, kan du ta bort hello standardslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="16572-117">If you have added additional endpoints toohello Web service, you cannot delete hello default endpoint.</span></span>
> 
> 

## <a name="adding-an-endpoint-programmatically"></a><span data-ttu-id="16572-118">Lägga till en slutpunkt programmässigt</span><span class="sxs-lookup"><span data-stu-id="16572-118">Adding an endpoint programmatically</span></span>
<span data-ttu-id="16572-119">Du kan lägga till en slutpunkt tooyour webbtjänst genom programmering med hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) exempelkod.</span><span class="sxs-lookup"><span data-stu-id="16572-119">You can add an endpoint tooyour Web service programmatically using hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>

## <a name="adding-an-endpoint-using-hello-azure-machine-learning-web-services-portal"></a><span data-ttu-id="16572-120">Lägga till en slutpunkt med hjälp av hello Azure Machine Learning-webbtjänster portal</span><span class="sxs-lookup"><span data-stu-id="16572-120">Adding an endpoint using hello Azure Machine Learning Web Services portal</span></span>
1. <span data-ttu-id="16572-121">Klicka på webbtjänster på hello navigeringen till vänster kolumn i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="16572-121">In Machine Learning Studio, on hello left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="16572-122">Hello längst ned på hello Web service instrumentpanelen, klickar du på **hantera slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="16572-122">At hello bottom of hello Web service dashboard, click **Manage endpoints**.</span></span> <span data-ttu-id="16572-123">hello Azure Machine Learning-webbtjänster portal öppnar toohello slutpunkter sida för hello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="16572-123">hello Azure Machine Learning Web Services portal opens toohello endpoints page for hello Web service.</span></span>
3. <span data-ttu-id="16572-124">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="16572-124">Click **New**.</span></span>
4. <span data-ttu-id="16572-125">Skriv ett namn och beskrivning för hello ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="16572-125">Type a name and description for hello new endpoint.</span></span> <span data-ttu-id="16572-126">Slutpunkten namn måste vara 24 tecken eller mindre längd och måste bestå av gemena bokstäver eller siffror.</span><span class="sxs-lookup"><span data-stu-id="16572-126">Endpoint names must be 24 character or less in length, and must be made up of lower-case alphabets or numbers.</span></span> <span data-ttu-id="16572-127">Välj hello loggningsnivån och om exempeldata är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="16572-127">Select hello logging level and whether sample data is enabled.</span></span> <span data-ttu-id="16572-128">Mer information om loggning finns [aktivera loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="16572-128">For more information on logging, see [Enable logging for Machine Learning Web services](machine-learning-web-services-logging.md).</span></span>

## <a name="adding-an-endpoint-using-hello-azure-classic-portal"></a><span data-ttu-id="16572-129">Lägga till en slutpunkt som använder hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="16572-129">Adding an endpoint using hello Azure classic portal</span></span>
1. <span data-ttu-id="16572-130">Logga in toohello [klassiska Azure-portalen](http://manage.windowsazure.com), klickar du på **Maskininlärning** i hello vänstra kolumnen.</span><span class="sxs-lookup"><span data-stu-id="16572-130">Sign in toohello [Azure classic portal](http://manage.windowsazure.com), click **Machine Learning** in hello left column.</span></span> <span data-ttu-id="16572-131">Klicka på hello-arbetsyta som innehåller hello-webbtjänst som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="16572-131">Click hello workspace which contains hello Web service in which you are interested.</span></span>
   
    ![Navigera tooworkspace](./media/machine-learning-create-endpoint/figure-1.png)
2. <span data-ttu-id="16572-133">Klicka på **webbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="16572-133">Click **Web Services**.</span></span>
   
    ![Navigera tooWeb tjänster](./media/machine-learning-create-endpoint/figure-2.png)
3. <span data-ttu-id="16572-135">Klicka på hello-webbtjänst som du är intresserad av toosee hello listan över tillgängliga slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="16572-135">Click hello Web service you're interested in toosee hello list of available endpoints.</span></span>
   
    ![Navigera tooendpoint](./media/machine-learning-create-endpoint/figure-3.png)
4. <span data-ttu-id="16572-137">Hello längst hello-sidan, klickar du på **Lägg till slutpunkt**.</span><span class="sxs-lookup"><span data-stu-id="16572-137">At hello bottom of hello page, click **Add Endpoint**.</span></span> <span data-ttu-id="16572-138">Ange ett namn och beskrivning, se till att det finns inga slutpunkter med samma namn i den här webbtjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="16572-138">Type a name and description, ensure there are no other endpoints with hello same name in this Web service.</span></span> <span data-ttu-id="16572-139">Lämna hello begränsning nivå med sitt standardvärde om du inte har särskilda krav.</span><span class="sxs-lookup"><span data-stu-id="16572-139">Leave hello throttle level with its default value unless you have special requirements.</span></span> <span data-ttu-id="16572-140">toolearn mer information om hur du begränsar, se [skalning API-slutpunkter](machine-learning-scaling-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="16572-140">toolearn more about throttling, see [Scaling API Endpoints](machine-learning-scaling-webservice.md).</span></span>
   
    ![Skapa slutpunkten](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a><span data-ttu-id="16572-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16572-142">Next Steps</span></span>
<span data-ttu-id="16572-143">[Hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="16572-143">[How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

