---
title: "(föråldrad) Vanliga frågor och svar - publicera och använda Machine Learning-appar i Azure Marketplace | Microsoft Docs"
description: "(föråldrad) Vanliga frågor och svar om publishing Machine Learning-appar i Azure Marketplace"
services: machine-learning
documentationcenter: 
author: bharaths
manager: jhubbard
editor: cgronlun
ms.assetid: 26b3a1e7-8b9a-4004-98bc-17456d4c25e8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: TRUE
ms.openlocfilehash: a4631dfeb2f817b3a3c612a8f6af48398e4f2ab9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-the-azure-marketplace-faq"></a><span data-ttu-id="ee5ce-103">(föråldrad) Publicera och använda Machine Learning-appar i Azure Marketplace: vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="ee5ce-103">(deprecated) Publishing and using Machine Learning apps in the Azure Marketplace: FAQ</span></span>

> [!NOTE]
> <span data-ttu-id="ee5ce-104">Tjänsterna och DataMarket är som har återkallats och befintliga prenumerationer ska tas bort och avbrutits startar 3/31/2017.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-104">DataMarket and Data Services are being retired, and existing subscriptions will be retired and cancelled starting 3/31/2017.</span></span> <span data-ttu-id="ee5ce-105">Därför kan är den här artikeln inaktuell.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-105">As a result, this article is being deprecated.</span></span> 
> 
> <span data-ttu-id="ee5ce-106">Alternativt kan du publicera Machine Learning-experiment till den [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) för datavetenskap community.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-106">As an alternative, you can publish your Machine Learning experiments to the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) for the benefit of the data science community.</span></span> <span data-ttu-id="ee5ce-107">Mer information finns i [resursen och identifiera resurser i Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span><span class="sxs-lookup"><span data-stu-id="ee5ce-107">For more information, see [Share and discover resources in the Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span></span>


## <a name="questions-about-consuming-from-marketplace"></a><span data-ttu-id="ee5ce-108">Frågor om förbrukar från Marketplace</span><span class="sxs-lookup"><span data-stu-id="ee5ce-108">Questions about consuming from Marketplace</span></span>
<span data-ttu-id="ee5ce-109">**1. Varför visas följande felmeddelande när du har angett indata för webbtjänsten:**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-109">**1. Why do I get the following error message after I enter input for the web service:**</span></span>

<span data-ttu-id="ee5ce-110">**Förfrågan resulterade i ett backend-timeout eller backend-fel. Teamet undersöker problemet. Vi kan Vi beklagar besväret. (500)**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-110">**The request resulted in a back-end time out or back-end error. The team is investigating the issue. We are sorry for the inconvenience. (500)**</span></span>

<span data-ttu-id="ee5ce-111">Din indataparametrar överensstämmer inte med formatet som krävs för specifika webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-111">Your input parameter(s) may not conform to the required format for the specific web service.</span></span> <span data-ttu-id="ee5ce-112">Hänvisa till länken motsvarande att hitta rätt format för indataparametrar och begränsningar för den här webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-112">Please refer to the corresponding documentation link to find the correct format for input parameters and the limitations of this web service.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="ee5ce-113">**2. Om jag kopiera API-länk för den webbtjänst som visas på sidan ”Utforska den här datauppsättningen” och klistra in det i ett nytt webbläsarfönster, vilka autentiseringsuppgifter ska jag använda för att komma åt resultaten och hur kan jag se dem?**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-113">**2. If I copy the API link for the web service that I see on the "Explore this dataset" page and paste it into another browser window, what credentials should I use to access the results, and how do I see them?**</span></span>

<span data-ttu-id="ee5ce-114">Du bör använda Marketplace-konto som användarnamnet och den primära konto nyckeln som lösenord.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-114">You should use your Marketplace account as the username and the primary account key as the password.</span></span> <span data-ttu-id="ee5ce-115">Den primära konto nyckeln finns på den **Utforska den här datauppsättningen** sidan under beskrivningen av webbtjänsten (klickar du på den **visa** knappen).</span><span class="sxs-lookup"><span data-stu-id="ee5ce-115">The primary account key can be found on the **Explore this dataset** page under the description of the web service (click the **show** button).</span></span> <span data-ttu-id="ee5ce-116">Resultatet visas i webbläsaren eller det kan vara tillgänglig för nedladdning, beroende på vilken webbläsare du använder.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-116">The result may display in the browser or it may be available to  download, depending on which browser you are using.</span></span>

<span data-ttu-id="ee5ce-117">**3. Varför visas följande felmeddelande när jag ange indata för webbtjänsten på sidan ”Utforska den här datauppsättningen”:**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-117">**3. Why do I get the following error message after I enter the input for the web service on the "Explore this dataset" page:**</span></span> 

<span data-ttu-id="ee5ce-118">**Ett oväntat fel uppstod när din begäran behandlades. Försök igen.**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-118">**An unexpected error occurred while processing your request. Please try again.**</span></span>

<span data-ttu-id="ee5ce-119">En eller flera indataparametrarna för webbtjänsten kanske har överskridit maxlängden när förbrukar webbtjänsten på marketplace **Utforska den här datauppsättningen** sidan.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-119">One or more input parameters of your web service may have exceeded the length limit when consuming the web service on the marketplace **Explore this dataset** page.</span></span> <span data-ttu-id="ee5ce-120">Tjänsterna kan anropas med ett längre indatalängden med hjälp av HTTP POST-metoderna.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-120">The services can be called with a longer input length by using HTTP POST methods.</span></span> <span data-ttu-id="ee5ce-121">Exempel finns i [exempel lösningar med hjälp av R på Machine Learning och publiceras på Marketplace](machine-learning-r-csharp-web-service-examples.md).</span><span class="sxs-lookup"><span data-stu-id="ee5ce-121">For examples, see [Sample solutions using R on Machine Learning and published to Marketplace](machine-learning-r-csharp-web-service-examples.md).</span></span>

<span data-ttu-id="ee5ce-122">**4. Varför kan jag inte se någonting i ”API EXPLORER” fliken int arkivet på den klassiska Azure-portalen?**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-122">**4. Why do I not see anything in the "API EXPLORER" tab int the Store in the Azure Classic Portal?**</span></span> 

<span data-ttu-id="ee5ce-123">Detta är ett känt problem med Azure klassiska Portal Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-123">This is a known issue with the Azure Classic Portal Marketplace.</span></span> <span data-ttu-id="ee5ce-124">Teamet arbetar för att lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-124">The team is working to resolve this issue.</span></span> 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a><span data-ttu-id="ee5ce-125">Frågor om hur du publicerar från Azure Machine Learning på Marketplace</span><span class="sxs-lookup"><span data-stu-id="ee5ce-125">Questions about publishing from Azure Machine Learning on Marketplace</span></span>
<span data-ttu-id="ee5ce-126">**1. Varför är min transaktioner av logotyper eller bilder inte uppdaterar för min webbtjänst?**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-126">**1. Why are my transactions of logos or images not refreshing for my web service?**</span></span> 

<span data-ttu-id="ee5ce-127">Logotyper och bilder cachelagras i publishing portal och det kan ta upp till 10 dagar för nya logotypen eller bilden att uppdatera på portalen.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-127">Logos and images are cached in the publishing portal, and it may take up to 10 days for the new logo or image to update on the portal.</span></span>

<span data-ttu-id="ee5ce-128">**2. Varför är min webbtjänst på Marketplace visar ett felmeddelande ”information” fliken?**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-128">**2. Why is the “Detail" tab of my web service on Marketplace showing an error message?**</span></span>

<span data-ttu-id="ee5ce-129">Det finns ett känt problem i Marketplace vid anslutning till Azure Machine Learning för information om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-129">There is a known Marketplace issue when connecting to Azure Machine Learning for service details.</span></span> <span data-ttu-id="ee5ce-130">Teamet arbetar för att lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-130">The team is working to resolve this issue.</span></span>

<span data-ttu-id="ee5ce-131">**3. Varför fungerar inte R exempelkoden i Azure Machine Learning-webbtjänster för förbrukning av webbtjänster på Marketplace?**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-131">**3. Why does the R sample code in the Azure Machine Learning web services not work for consuming the web services in Marketplace?**</span></span>

<span data-ttu-id="ee5ce-132">Autentiseringssystem är olika vid anslutning till Azure Machine Learning-webbtjänster direkt jämfört med att ansluta till dessa webbtjänster på marknadsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-132">The authentication systems are different when connecting to Azure Machine Learning web services directly compared to connecting to these web services through the Marketplace.</span></span> <span data-ttu-id="ee5ce-133">Tjänsterna i Marketplace är OData och de kan anropas med metoderna GET eller POST.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-133">The services in Marketplace are OData services, and they can be called with GET or POST methods.</span></span> 

<span data-ttu-id="ee5ce-134">**4. Varför är support länkar för min webbtjänst erbjuder inte uppdateras korrekt för vissa min erbjudanden?**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-134">**4. Why are the support links of my web service offers not updating correctly for some of my offers?**</span></span>

<span data-ttu-id="ee5ce-135">Stöd för länkar är globala per utgivare, inte per erbjudandet.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-135">The support links are global per publisher, not per offer.</span></span> 

<span data-ttu-id="ee5ce-136">**5. Hur publicera en webbtjänst med indata batchläge i Marketplace?**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-136">**5. How do I publish a web service with batch input mode in Marketplace?**</span></span>

<span data-ttu-id="ee5ce-137">Inkommande batchläge stöds för närvarande inte i Marketplace-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-137">The batch input mode is currently not supported in Marketplace web services.</span></span>

<span data-ttu-id="ee5ce-138">**6. Vem ska jag kontakta för att få hjälp om jag har frågor om att bli en utgivare, eller om jag har problem vid publicering?**</span><span class="sxs-lookup"><span data-stu-id="ee5ce-138">**6. Who should I contact to get help if I have questions about becoming a data publisher, or if I have issues during publishing?**</span></span>

<span data-ttu-id="ee5ce-139">Kontakta Azure Marketplace-teamet på < mailto:datamarketbd@microsoft.com > för mer information.</span><span class="sxs-lookup"><span data-stu-id="ee5ce-139">Please contact the Azure Marketplace team at <mailto:datamarketbd@microsoft.com> for more information.</span></span>

