---
title: "aaaMulti Geo hjälpdokumentationen | Microsoft Docs"
description: "Lär dig hur toocreate en arbetsyta och publicera en webbtjänst i en Azure-region skiljer sig från hello södra centrala USA (SCUS) Azure-region."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: 77b055950ebfe329131b40e5f0a2f6be1e33c51e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multi-geo-help-documentation"></a><span data-ttu-id="52664-103">Hjälpdokumentation för flera geografiska platser</span><span class="sxs-lookup"><span data-stu-id="52664-103">Multi-Geo Help documentation</span></span>
<span data-ttu-id="52664-104">Den här artikeln beskriver hur du kan skapa en arbetsyta och publicera en webbtjänst i olika Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="52664-104">This article describes how you can create a workspace and publish a web service in different Azure regions.</span></span>  <span data-ttu-id="52664-105">Hej [Azure produkter efter Region sidan](https://azure.microsoft.com/en-us/regions/services/) visar en lista över regioner där Azure Machine Learning är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="52664-105">hello [Azure Products by Region page](https://azure.microsoft.com/en-us/regions/services/) lists regions where Azure Machine Learning is available.</span></span>

## <a name="create-a-workspace"></a><span data-ttu-id="52664-106">Skapa en arbetsyta</span><span class="sxs-lookup"><span data-stu-id="52664-106">Create a workspace</span></span>
1. <span data-ttu-id="52664-107">Logga in toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="52664-107">Sign in toohello Azure Classic Portal.</span></span>
2. <span data-ttu-id="52664-108">Klicka på **+ ny** > **DATATJÄNSTER** > **MASKININLÄRNING** > **SNABBREGISTRERING**.</span><span class="sxs-lookup"><span data-stu-id="52664-108">Click **+NEW** > **DATA SERVICES** > **MACHINE LEARNING** > **QUICK CREATE**.</span></span>  <span data-ttu-id="52664-109">Under **plats** välja en annan region, till exempel **Sydostasien**.</span><span class="sxs-lookup"><span data-stu-id="52664-109">Under **LOCATION** select another region, such as **Southeast Asia**.</span></span>
   <span data-ttu-id="52664-110">![Flera Geo hjälpa bild 1][1]</span><span class="sxs-lookup"><span data-stu-id="52664-110">![Multi-Geo Help image 1][1]</span></span>
3. <span data-ttu-id="52664-111">Välj hello arbetsyta och klicka sedan på **inloggning tooML Studio**.</span><span class="sxs-lookup"><span data-stu-id="52664-111">Select hello workspace, and then click **Sign-in tooML Studio**.</span></span>
   <span data-ttu-id="52664-112">![Flera Geo hjälpa bild 2][2]</span><span class="sxs-lookup"><span data-stu-id="52664-112">![Multi-Geo Help image 2][2]</span></span>
4. <span data-ttu-id="52664-113">Nu har du en arbetsyta i en annan region som du kan använda precis som andra arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="52664-113">You now have a workspace in another region that you may use just like any other workspace.</span></span> <span data-ttu-id="52664-114">tooswitch bland dina arbetsytor utseende toohello övre högra hörnet på skärmen.</span><span class="sxs-lookup"><span data-stu-id="52664-114">tooswitch among your workspaces, look toohello upper right of your screen.</span></span> <span data-ttu-id="52664-115">Klicka på hello listrutan, Välj hello region och välj sedan hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="52664-115">Click hello dropdown, select hello region, and then select hello workspace.</span></span> <span data-ttu-id="52664-116">Allt är lokala toohello arbetsytan region.</span><span class="sxs-lookup"><span data-stu-id="52664-116">Everything is local toohello workspace region.</span></span>  <span data-ttu-id="52664-117">Exempelvis blir alla webbtjänster som skapas från en arbetsyta i hello finns i samma region hello arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="52664-117">For example, all of your web services created from a workspace will be in hello same region hello workspace is located in.</span></span>
   <span data-ttu-id="52664-118">![Flera Geo hjälpa bild 3][3]</span><span class="sxs-lookup"><span data-stu-id="52664-118">![Multi-Geo Help image 3][3]</span></span>

## <a name="open-an-experiment-from-gallery"></a><span data-ttu-id="52664-119">Öppna ett experiment från galleriet</span><span class="sxs-lookup"><span data-stu-id="52664-119">Open an experiment from Gallery</span></span>
<span data-ttu-id="52664-120">Om du öppnar ett experiment från galleriet måste välja du även vilken region som du vill använda toocopy hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="52664-120">If you open an experiment from Gallery, you can also select which region you want toocopy hello experiment to.</span></span>

![Flera Geo hjälpa bild 4][4a]

## <a name="web-service-management"></a><span data-ttu-id="52664-122">Webbtjänsthantering</span><span class="sxs-lookup"><span data-stu-id="52664-122">Web service management</span></span>
<span data-ttu-id="52664-123">tooprogrammatically hantera webbtjänster, t.ex för omtränings, Använd hello regionspecifika adress:</span><span class="sxs-lookup"><span data-stu-id="52664-123">tooprogrammatically manage web services, such as for retraining, use hello region-specific address:</span></span>

* <span data-ttu-id="52664-124">https://asiasoutheast.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="52664-124">https://asiasoutheast.management.azureml.net</span></span>
* <span data-ttu-id="52664-125">https://europewest.Management.azureml.NET</span><span class="sxs-lookup"><span data-stu-id="52664-125">https://europewest.management.azureml.net</span></span>

### <a name="things-toonote"></a><span data-ttu-id="52664-126">Saker toonote</span><span class="sxs-lookup"><span data-stu-id="52664-126">Things toonote</span></span>
1. <span data-ttu-id="52664-127">Du kan bara kopiera experiment mellan arbetsytor som tillhör toohello samma region det här sättet.</span><span class="sxs-lookup"><span data-stu-id="52664-127">You can only copy experiments between workspaces that belong toohello same region this way.</span></span> <span data-ttu-id="52664-128">Om du behöver toocopy experiment över arbetsytor i olika regioner, kan du använda hello [PowerShell](http://aka.ms/amlps) kommandot [ *kopiera AmlExperiment* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tooaccomplish som.</span><span class="sxs-lookup"><span data-stu-id="52664-128">If you need toocopy experiment across workspaces in different regions, you can use hello [PowerShell](http://aka.ms/amlps) commandlet [*Copy-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) tooaccomplish that.</span></span> <span data-ttu-id="52664-129">En annan lösning är toopublish hello experiment i galleriet i olistade läge och sedan öppna den i hello arbetsyta från hello annan region.</span><span class="sxs-lookup"><span data-stu-id="52664-129">Another workaround is toopublish hello experiment into Gallery in unlisted mode, then open it in hello workspace from hello other region.</span></span>
2. <span data-ttu-id="52664-130">hello region selector visar endast arbetsytor för en region i taget.</span><span class="sxs-lookup"><span data-stu-id="52664-130">hello region selector will only show workspaces for one region at a time.</span></span>  
3. <span data-ttu-id="52664-131">En kostnadsfri arbetsyta eller gästbehörighet (anonym) arbetsytan ska skapas och hanteras i södra centrala USA</span><span class="sxs-lookup"><span data-stu-id="52664-131">A free workspace or Guest Access (anonymous) workspace will be created and hosted in South Central U.S.</span></span>  
4. <span data-ttu-id="52664-132">Webbtjänster som distribueras från en arbetsyta i Sydostasien ska också finnas i Sydostasien.</span><span class="sxs-lookup"><span data-stu-id="52664-132">Web services deployed from a workspace in Southeast Asia will also be hosted in Southeast Asia.</span></span>  

## <a name="more-information"></a><span data-ttu-id="52664-133">Mer information</span><span class="sxs-lookup"><span data-stu-id="52664-133">More information</span></span>
<span data-ttu-id="52664-134">Ställ en fråga på hello [Azure Machine Learning-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span><span class="sxs-lookup"><span data-stu-id="52664-134">Ask a question on hello [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).</span></span>

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
