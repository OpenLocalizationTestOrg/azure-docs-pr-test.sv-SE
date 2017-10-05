---
title: "Använda en Machine Learning-webbtjänst från Excel | Microsoft Docs"
description: "Använda en Azure Machine Learning-webbtjänst från Excel"
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
ms.assetid: 3f3cdd2f-1816-487e-ab78-530e01e9788f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/13/2017
ms.author: tedway
ms.openlocfilehash: 9f1aac04d54221888ee9374317be339400dcf085
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a><span data-ttu-id="d45a8-103">Använda en Azure Machine Learning-webbtjänst från Excel</span><span class="sxs-lookup"><span data-stu-id="d45a8-103">Consuming an Azure Machine Learning Web Service from Excel</span></span>
 <span data-ttu-id="d45a8-104">Azure Machine Learning Studio gör det enkelt att anropa webbtjänster direkt från Excel utan att behöva skriva någon kod.</span><span class="sxs-lookup"><span data-stu-id="d45a8-104">Azure Machine Learning Studio makes it easy to call web services directly from Excel without the need to write any code.</span></span>

<span data-ttu-id="d45a8-105">Om du använder Excel 2013 (eller senare) eller Excel Online, så vi rekommenderar att du använder Excel [Excel-tillägg](machine-learning-excel-add-in-for-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="d45a8-105">If you are using Excel 2013 (or later) or Excel Online, then we recommend that you use the Excel [Excel add-in](machine-learning-excel-add-in-for-web-services.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a><span data-ttu-id="d45a8-106">Steg</span><span class="sxs-lookup"><span data-stu-id="d45a8-106">Steps</span></span>
<span data-ttu-id="d45a8-107">Publicera en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="d45a8-107">Publish a web service.</span></span> <span data-ttu-id="d45a8-108">[Den här sidan](machine-learning-walkthrough-5-publish-web-service.md) förklarar hur du gör.</span><span class="sxs-lookup"><span data-stu-id="d45a8-108">[This page](machine-learning-walkthrough-5-publish-web-service.md) explains how to do it.</span></span> <span data-ttu-id="d45a8-109">Funktionen Excel-arbetsboken stöds för närvarande endast för begäranden/svar-tjänsterna som har ett enda utflöde (det vill säga en enda bedömningsprofil etikett).</span><span class="sxs-lookup"><span data-stu-id="d45a8-109">Currently the Excel workbook feature is only supported for Request/Response services that have a single output (that is, a single scoring label).</span></span> 

<span data-ttu-id="d45a8-110">När du har en webbtjänst kan klicka på den **WEB SERVICES** avsnittet till vänster om studio och väljer sedan webbtjänsten för att kunna använda från Excel.</span><span class="sxs-lookup"><span data-stu-id="d45a8-110">Once you have a web service, click on the **WEB SERVICES** section on the left of the studio, and then select the web service to consume from Excel.</span></span>

<span data-ttu-id="d45a8-111">**Klassiska webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="d45a8-111">**Classic Web Service**</span></span>

1. <span data-ttu-id="d45a8-112">På den **INSTRUMENTPANELEN** fliken för webbtjänsten är en rad för den **frågor och svar** service.</span><span class="sxs-lookup"><span data-stu-id="d45a8-112">On the **DASHBOARD** tab for the web service is a row for the **REQUEST/RESPONSE** service.</span></span> <span data-ttu-id="d45a8-113">Om den här tjänsten har ett enda utflöde, bör du se den **hämta Excel-arbetsbok** länk på den raden.</span><span class="sxs-lookup"><span data-stu-id="d45a8-113">If this service had a single output, you should see the **Download Excel Workbook** link in that row.</span></span>
   
    ![][1]
2. <span data-ttu-id="d45a8-114">Klicka på **hämta Excel-arbetsbok**.</span><span class="sxs-lookup"><span data-stu-id="d45a8-114">Click on **Download Excel Workbook**.</span></span>

<span data-ttu-id="d45a8-115">**Ny webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="d45a8-115">**New Web Service**</span></span>

1. <span data-ttu-id="d45a8-116">Välj i Azure Machine Learning-webbtjänst-portal **förbruka**.</span><span class="sxs-lookup"><span data-stu-id="d45a8-116">In the Azure Machine Learning Web Service portal, select **Consume**.</span></span>
2. <span data-ttu-id="d45a8-117">På sidan förbruka i den **Web-förbrukning alternativ** klickar du på ikonen för Excel.</span><span class="sxs-lookup"><span data-stu-id="d45a8-117">On the Consume page, in the **Web service consumption options** section, click the Excel icon.</span></span>

<span data-ttu-id="d45a8-118">**Använda arbetsboken**</span><span class="sxs-lookup"><span data-stu-id="d45a8-118">**Using the workbook**</span></span>

1. <span data-ttu-id="d45a8-119">Öppna arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="d45a8-119">Open the workbook.</span></span>
2. <span data-ttu-id="d45a8-120">En säkerhetsvarning visas. Klicka på den **Aktivera redigering av** knappen.</span><span class="sxs-lookup"><span data-stu-id="d45a8-120">A Security Warning appears; click on the **Enable Editing** button.</span></span>
   
    ![][2]
3. <span data-ttu-id="d45a8-121">En säkerhetsvarning visas.</span><span class="sxs-lookup"><span data-stu-id="d45a8-121">A Security Warning appears.</span></span> <span data-ttu-id="d45a8-122">Klicka på den **Aktivera innehåll** för att köra makron i kalkylbladet.</span><span class="sxs-lookup"><span data-stu-id="d45a8-122">Click on the **Enable Content** button to run macros on your spreadsheet.</span></span>
   
    ![][3]
4. <span data-ttu-id="d45a8-123">När makron är aktiverade, skapas en tabell.</span><span class="sxs-lookup"><span data-stu-id="d45a8-123">Once macros are enabled, a table is generated.</span></span> <span data-ttu-id="d45a8-124">Kolumnerna i blå är obligatoriska som indata till webbtjänst Resursposter eller **parametrar**.</span><span class="sxs-lookup"><span data-stu-id="d45a8-124">Columns in blue are required as input into the RRS web service, or **PARAMETERS**.</span></span> <span data-ttu-id="d45a8-125">Observera utdata från tjänsten Resursposter **FÖRUTSADE värden** i grönt.</span><span class="sxs-lookup"><span data-stu-id="d45a8-125">Note the output of the RRS service, **PREDICTED VALUES** in green.</span></span> <span data-ttu-id="d45a8-126">När alla kolumner för en viss rad fylls arbetsboken automatiskt bedömningsprofil API-anrop och visar poängsatta resultat.</span><span class="sxs-lookup"><span data-stu-id="d45a8-126">When all columns for a given row are filled, the workbook automatically calls the scoring API, and displays the scored results.</span></span>
   
    ![][4]
5. <span data-ttu-id="d45a8-127">Fyll den andra raden med data och de förväntade värdena produceras för att poängsätta mer än en rad.</span><span class="sxs-lookup"><span data-stu-id="d45a8-127">To score more than one row, fill the second row with data and the predicted values are produced.</span></span> <span data-ttu-id="d45a8-128">Du kan även klistra in flera rader på samma gång.</span><span class="sxs-lookup"><span data-stu-id="d45a8-128">You can even paste several rows at once.</span></span>

<span data-ttu-id="d45a8-129">Du kan använda någon av funktionerna i Excel (diagram, power karta, villkorsstyrd formatering, etc.) med de förväntade värdena för att visualisera data.</span><span class="sxs-lookup"><span data-stu-id="d45a8-129">You can use any of the Excel features (graphs, power map, conditional formatting, etc.) with the predicted values to help visualize the data.</span></span>    

## <a name="sharing-your-workbook"></a><span data-ttu-id="d45a8-130">Dela din arbetsbok</span><span class="sxs-lookup"><span data-stu-id="d45a8-130">Sharing your workbook</span></span>
<span data-ttu-id="d45a8-131">API-nyckel måste vara en del av kalkylbladet för makron ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d45a8-131">For the macros to work, your API Key must be part of the spreadsheet.</span></span> <span data-ttu-id="d45a8-132">Det innebär att du ska dela arbetsboken endast med entiteter/personer som du litar på.</span><span class="sxs-lookup"><span data-stu-id="d45a8-132">That means that you should share the workbook only with entities/individuals you trust.</span></span>

## <a name="automatic-updates"></a><span data-ttu-id="d45a8-133">Automatiska uppdateringar</span><span class="sxs-lookup"><span data-stu-id="d45a8-133">Automatic updates</span></span>
<span data-ttu-id="d45a8-134">En RR-anrop i dessa två situationer:</span><span class="sxs-lookup"><span data-stu-id="d45a8-134">An RRS call is made in these two situations:</span></span>

1. <span data-ttu-id="d45a8-135">Första gången en rad har innehåll i alla dess **parametrar**</span><span class="sxs-lookup"><span data-stu-id="d45a8-135">The first time a row has content in all of its **PARAMETERS**</span></span>
2. <span data-ttu-id="d45a8-136">När någon av de **parametrar** ändringar i en rad som har alla dess **parametrar** anges.</span><span class="sxs-lookup"><span data-stu-id="d45a8-136">Any time any of the **PARAMETERS** changes in a row that had all of its **PARAMETERS** entered.</span></span>

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png
