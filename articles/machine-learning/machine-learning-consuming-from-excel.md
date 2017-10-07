---
title: "aaaConsume en Machine Learning-webbtjänst från Excel | Microsoft Docs"
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
ms.openlocfilehash: e2e8bbf7ba75b6618a0285539555ce175ec03c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consuming-an-azure-machine-learning-web-service-from-excel"></a><span data-ttu-id="61583-103">Använda en Azure Machine Learning-webbtjänst från Excel</span><span class="sxs-lookup"><span data-stu-id="61583-103">Consuming an Azure Machine Learning Web Service from Excel</span></span>
 <span data-ttu-id="61583-104">Azure Machine Learning Studio gör det enkelt toocall webbtjänster direkt från Excel utan hello måste toowrite någon kod.</span><span class="sxs-lookup"><span data-stu-id="61583-104">Azure Machine Learning Studio makes it easy toocall web services directly from Excel without hello need toowrite any code.</span></span>

<span data-ttu-id="61583-105">Om du använder Excel 2013 (eller senare) eller Excel Online, så vi rekommenderar att du använder hello Excel [Excel-tillägg](machine-learning-excel-add-in-for-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="61583-105">If you are using Excel 2013 (or later) or Excel Online, then we recommend that you use hello Excel [Excel add-in](machine-learning-excel-add-in-for-web-services.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="steps"></a><span data-ttu-id="61583-106">Steg</span><span class="sxs-lookup"><span data-stu-id="61583-106">Steps</span></span>
<span data-ttu-id="61583-107">Publicera en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="61583-107">Publish a web service.</span></span> <span data-ttu-id="61583-108">[Den här sidan](machine-learning-walkthrough-5-publish-web-service.md) förklarar hur toodo den.</span><span class="sxs-lookup"><span data-stu-id="61583-108">[This page](machine-learning-walkthrough-5-publish-web-service.md) explains how toodo it.</span></span> <span data-ttu-id="61583-109">För närvarande stöds endast hello Excel-arbetsboken funktion för frågor och svar tjänsterna som har ett enda utflöde (det vill säga en enda bedömningsprofil etikett).</span><span class="sxs-lookup"><span data-stu-id="61583-109">Currently hello Excel workbook feature is only supported for Request/Response services that have a single output (that is, a single scoring label).</span></span> 

<span data-ttu-id="61583-110">När du har en webbtjänst kan klicka på hello **WEB SERVICES** avsnittet hello vänster hello Studio och väljer sedan hello web service tooconsume från Excel.</span><span class="sxs-lookup"><span data-stu-id="61583-110">Once you have a web service, click on hello **WEB SERVICES** section on hello left of hello studio, and then select hello web service tooconsume from Excel.</span></span>

<span data-ttu-id="61583-111">**Klassiska webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="61583-111">**Classic Web Service**</span></span>

1. <span data-ttu-id="61583-112">På hello **INSTRUMENTPANELEN** för hello-webbtjänsten är en rad för hello **frågor och svar** service.</span><span class="sxs-lookup"><span data-stu-id="61583-112">On hello **DASHBOARD** tab for hello web service is a row for hello **REQUEST/RESPONSE** service.</span></span> <span data-ttu-id="61583-113">Om den här tjänsten har ett enda utflöde, bör du se hello **hämta Excel-arbetsbok** länk på den raden.</span><span class="sxs-lookup"><span data-stu-id="61583-113">If this service had a single output, you should see hello **Download Excel Workbook** link in that row.</span></span>
   
    ![][1]
2. <span data-ttu-id="61583-114">Klicka på **hämta Excel-arbetsbok**.</span><span class="sxs-lookup"><span data-stu-id="61583-114">Click on **Download Excel Workbook**.</span></span>

<span data-ttu-id="61583-115">**Ny webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="61583-115">**New Web Service**</span></span>

1. <span data-ttu-id="61583-116">Markera i hello Azure Machine Learning-webbtjänst portal **förbruka**.</span><span class="sxs-lookup"><span data-stu-id="61583-116">In hello Azure Machine Learning Web Service portal, select **Consume**.</span></span>
2. <span data-ttu-id="61583-117">Hello förbruka i sidan hello **Web-förbrukning alternativ** klickar hello Excel-ikonen.</span><span class="sxs-lookup"><span data-stu-id="61583-117">On hello Consume page, in hello **Web service consumption options** section, click hello Excel icon.</span></span>

<span data-ttu-id="61583-118">**Med hjälp av hello arbetsboken**</span><span class="sxs-lookup"><span data-stu-id="61583-118">**Using hello workbook**</span></span>

1. <span data-ttu-id="61583-119">Öppna hello arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="61583-119">Open hello workbook.</span></span>
2. <span data-ttu-id="61583-120">En säkerhetsvarning visas. Klicka på hello **Aktivera redigering av** knappen.</span><span class="sxs-lookup"><span data-stu-id="61583-120">A Security Warning appears; click on hello **Enable Editing** button.</span></span>
   
    ![][2]
3. <span data-ttu-id="61583-121">En säkerhetsvarning visas.</span><span class="sxs-lookup"><span data-stu-id="61583-121">A Security Warning appears.</span></span> <span data-ttu-id="61583-122">Klicka på hello **Aktivera innehåll** knappen toorun makron i kalkylbladet.</span><span class="sxs-lookup"><span data-stu-id="61583-122">Click on hello **Enable Content** button toorun macros on your spreadsheet.</span></span>
   
    ![][3]
4. <span data-ttu-id="61583-123">När makron är aktiverade, skapas en tabell.</span><span class="sxs-lookup"><span data-stu-id="61583-123">Once macros are enabled, a table is generated.</span></span> <span data-ttu-id="61583-124">Kolumner i blå är krävs som indata till hello Resursposter webbtjänst eller **parametrar**.</span><span class="sxs-lookup"><span data-stu-id="61583-124">Columns in blue are required as input into hello RRS web service, or **PARAMETERS**.</span></span> <span data-ttu-id="61583-125">Observera hello utdata från hello RR-tjänsten, **FÖRUTSADE värden** i grönt.</span><span class="sxs-lookup"><span data-stu-id="61583-125">Note hello output of hello RRS service, **PREDICTED VALUES** in green.</span></span> <span data-ttu-id="61583-126">När alla kolumner för en viss rad fylls hello arbetsboken automatiskt hello bedömningen API-anrop och visar hello bedömas resultat.</span><span class="sxs-lookup"><span data-stu-id="61583-126">When all columns for a given row are filled, hello workbook automatically calls hello scoring API, and displays hello scored results.</span></span>
   
    ![][4]
5. <span data-ttu-id="61583-127">tooscore mer än en rad, förutsade fill hello andra raden med data och hello värden genereras.</span><span class="sxs-lookup"><span data-stu-id="61583-127">tooscore more than one row, fill hello second row with data and hello predicted values are produced.</span></span> <span data-ttu-id="61583-128">Du kan även klistra in flera rader på samma gång.</span><span class="sxs-lookup"><span data-stu-id="61583-128">You can even paste several rows at once.</span></span>

<span data-ttu-id="61583-129">Du kan använda någon av hello Excel-funktioner (diagram, power karta, villkorlig formatering, etc.) med hello förutsade toohelp värdena visualisera hello data.</span><span class="sxs-lookup"><span data-stu-id="61583-129">You can use any of hello Excel features (graphs, power map, conditional formatting, etc.) with hello predicted values toohelp visualize hello data.</span></span>    

## <a name="sharing-your-workbook"></a><span data-ttu-id="61583-130">Dela din arbetsbok</span><span class="sxs-lookup"><span data-stu-id="61583-130">Sharing your workbook</span></span>
<span data-ttu-id="61583-131">API-nyckel måste vara en del av hello kalkylblad för hello makron toowork.</span><span class="sxs-lookup"><span data-stu-id="61583-131">For hello macros toowork, your API Key must be part of hello spreadsheet.</span></span> <span data-ttu-id="61583-132">Det innebär att du ska dela hello arbetsboken endast med entiteter/personer som du litar på.</span><span class="sxs-lookup"><span data-stu-id="61583-132">That means that you should share hello workbook only with entities/individuals you trust.</span></span>

## <a name="automatic-updates"></a><span data-ttu-id="61583-133">Automatiska uppdateringar</span><span class="sxs-lookup"><span data-stu-id="61583-133">Automatic updates</span></span>
<span data-ttu-id="61583-134">En RR-anrop i dessa två situationer:</span><span class="sxs-lookup"><span data-stu-id="61583-134">An RRS call is made in these two situations:</span></span>

1. <span data-ttu-id="61583-135">Hej första gången en rad har innehåll i alla dess **parametrar**</span><span class="sxs-lookup"><span data-stu-id="61583-135">hello first time a row has content in all of its **PARAMETERS**</span></span>
2. <span data-ttu-id="61583-136">Varje gång någon av hello **parametrar** ändringar i en rad som har alla dess **parametrar** anges.</span><span class="sxs-lookup"><span data-stu-id="61583-136">Any time any of hello **PARAMETERS** changes in a row that had all of its **PARAMETERS** entered.</span></span>

[1]: ./media/machine-learning-consuming-from-excel/excellink.png
[2]: ./media/machine-learning-consuming-from-excel/enableeditting.png
[3]: ./media/machine-learning-consuming-from-excel/enablecontent.png
[4]: ./media/machine-learning-consuming-from-excel/sampletable.png
