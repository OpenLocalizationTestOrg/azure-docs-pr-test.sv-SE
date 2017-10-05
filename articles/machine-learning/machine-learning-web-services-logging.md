---
title: "Loggning för Machine Learning-webbtjänster | Microsoft Docs"
description: "Lär dig hur du aktiverar loggning för Machine Learning-webbtjänster. Loggning ger ytterligare information för att felsöka de API: er."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: raymondl;garye
ms.openlocfilehash: 7d0b2db01427430d6b0a317cdfefc265dd4b06e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a><span data-ttu-id="3993a-104">Aktivera loggning för Machine Learning-webbtjänster</span><span class="sxs-lookup"><span data-stu-id="3993a-104">Enable logging for Machine Learning web services</span></span>
<span data-ttu-id="3993a-105">Det här dokumentet innehåller information om loggningsfunktioner av Machine Learning-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="3993a-105">This document provides information on the logging capability of Machine Learning web services.</span></span> <span data-ttu-id="3993a-106">Loggning ger ytterligare information, förutom bara ett felnummer och ett meddelande som kan hjälpa dig att felsöka dina anrop till API: er för Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3993a-106">Logging provides additional information, beyond just an error number and a message, that can help you troubleshoot your calls to the Machine Learning APIs.</span></span>  

## <a name="how-to-enable-logging-for-a-web-service"></a><span data-ttu-id="3993a-107">Så här aktiverar du loggning för en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="3993a-107">How to enable logging for a Web service</span></span>

<span data-ttu-id="3993a-108">Du aktiverar loggning från den [Azure Machine Learning-webbtjänster](https://services.azureml.net) portal.</span><span class="sxs-lookup"><span data-stu-id="3993a-108">You enable logging from the [Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span> 

1. <span data-ttu-id="3993a-109">Logga in på Azure Machine Learning-webbtjänster portalen på [https://services.azureml.net](https://services.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="3993a-109">Sign in to the Azure Machine Learning Web Services portal at [https://services.azureml.net](https://services.azureml.net).</span></span> <span data-ttu-id="3993a-110">För en klassisk webbtjänst, du kan också få till portalen genom att klicka på **nya Web Services upplevelsen** på sidan Machine Learning-webbtjänster i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3993a-110">For a Classic web service, you can also get to the portal by clicking **New Web Services Experience** on the Machine Learning Web Services page in Machine Learning Studio.</span></span>

   ![Ny Services webbupplevelse länk](media/machine-learning-web-services-logging/new-web-services-experience-link.png)

2. <span data-ttu-id="3993a-112">På översta menyraden klickar du på **Web Services** för en ny webbtjänst eller klicka på **klassiska webbtjänster** ett klassiskt webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="3993a-112">On the top menu bar, click **Web Services** for a New web service, or click **Classic Web Services** for a Classic web service.</span></span>

   ![Välj ny eller klassisk webbtjänster](media/machine-learning-web-services-logging/select-web-service.png)

3. <span data-ttu-id="3993a-114">Klicka på namnet på webben för en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="3993a-114">For a New web service, click the web service name.</span></span> <span data-ttu-id="3993a-115">För en klassisk webbtjänst klickar du på namnet på webben och sedan på nästa sida klickar du på lämplig slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="3993a-115">For a Classic web service, click the web service name and then on the next page click the appropriate endpoint.</span></span>

4. <span data-ttu-id="3993a-116">Klicka på översta verktygsfältet och **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="3993a-116">On the top menu bar, click **Configure**.</span></span>

5. <span data-ttu-id="3993a-117">Ange den **aktivera loggning** att *fel* (för att logga endast fel) eller *alla* (för fullständig loggning).</span><span class="sxs-lookup"><span data-stu-id="3993a-117">Set the **Enable Logging** option to *Error* (to log only errors) or *All* (for full logging).</span></span>

   ![Välj loggningsnivån](media/machine-learning-web-services-logging/enable-logging.png)

6. <span data-ttu-id="3993a-119">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3993a-119">Click **Save**.</span></span>

7. <span data-ttu-id="3993a-120">Klassisk webbtjänster, skapa den **ml-diagnostik** behållare.</span><span class="sxs-lookup"><span data-stu-id="3993a-120">For Classic web services, create the **ml-diagnostics** container.</span></span>

   <span data-ttu-id="3993a-121">Alla web service loggar sparas i en blobbbehållare med namnet **ml-diagnostik** i storage-konto som är kopplade till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="3993a-121">All web service logs are kept in a blob container named **ml-diagnostics** in the storage account associated with the web service.</span></span> <span data-ttu-id="3993a-122">För nya webbtjänster skapas den här behållaren första gången du få åtkomst till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="3993a-122">For New web services, this container is created the first time you access the web service.</span></span> <span data-ttu-id="3993a-123">För klassisk webbtjänster måste du skapa behållaren om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="3993a-123">For Classic web services, you need to create the container if it doesn't already exist.</span></span> 

   1. <span data-ttu-id="3993a-124">I den [Azure-portalen](https://portal.azure.com)går du till lagringskontot som är associerade med webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="3993a-124">In the [Azure portal](https://portal.azure.com), go to the storage account associated with the web service.</span></span>

   2. <span data-ttu-id="3993a-125">Under **Blob-tjänsten**, klickar du på **behållare**.</span><span class="sxs-lookup"><span data-stu-id="3993a-125">Under **Blob Service**, click **Containers**.</span></span>

   3. <span data-ttu-id="3993a-126">Om behållaren **ml-diagnostik** inte finns, klickar du på **+ behållare**, och ge behållaren namnet ”ml-diagnostik” och välj den **komma åt typen** som ”Blob”.</span><span class="sxs-lookup"><span data-stu-id="3993a-126">If the container **ml-diagnostics** doesn't exist, click **+Container**, give the container the name "ml-diagnostics", and select the **Access type** as "Blob".</span></span> <span data-ttu-id="3993a-127">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3993a-127">Click **OK**.</span></span>

      ![Välj loggningsnivån](media/machine-learning-web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> <span data-ttu-id="3993a-129">Web Services instrumentpanelen i Machine Learning Studio har också en växel för att aktivera loggning för en klassisk webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="3993a-129">For a Classic web service, the Web Services Dashboard in Machine Learning Studio also has a switch to enable logging.</span></span> <span data-ttu-id="3993a-130">Men eftersom loggning hanteras nu via Web Services-portalen, måste du aktivera loggning på portalen, enligt beskrivningen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="3993a-130">However, because logging is now managed through the Web Services portal, you need to enable logging through the portal as described in this article.</span></span> <span data-ttu-id="3993a-131">Om du redan loggar in Studio, inaktivera loggning i Web Services-portalen och aktivera det igen.</span><span class="sxs-lookup"><span data-stu-id="3993a-131">If you already enabled logging in Studio, then in the Web Services Portal, disable logging and enable it again.</span></span>


## <a name="the-effects-of-enabling-logging"></a><span data-ttu-id="3993a-132">Effekterna av att aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="3993a-132">The effects of enabling logging</span></span>
<span data-ttu-id="3993a-133">När loggning är aktiverat, diagnostik och fel från webbtjänstslutpunkt loggas i den **ml-diagnostik** blob-behållaren i Azure Storage-konto som är kopplad till användarens arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="3993a-133">When logging is enabled, the diagnostics and errors from the web service endpoint are logged in the **ml-diagnostics** blob container in the Azure Storage Account linked with the user’s workspace.</span></span> <span data-ttu-id="3993a-134">Den här behållaren innehåller diagnostikinformation för alla slutpunkterna för webbtjänster för alla arbetsytor som är associerade med det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="3993a-134">This container holds all the diagnostics information for all the web service endpoints for all the workspaces associated with this storage account.</span></span>

<span data-ttu-id="3993a-135">Loggarna kan visas med hjälp av flera verktyg som finns tillgängliga att utforska Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3993a-135">The logs can be viewed using any of the several tools available to explore an Azure Storage Account.</span></span> <span data-ttu-id="3993a-136">Den enklaste kanske vill navigera till storage-konto i Azure-portalen klickar du på **behållare**, och klicka sedan på behållaren **ml-diagnostik**.</span><span class="sxs-lookup"><span data-stu-id="3993a-136">The easiest may be to navigate to the storage account in the Azure portal, click **Containers**, and then click the container **ml-diagnostics**.</span></span>  

## <a name="log-blob-detail-information"></a><span data-ttu-id="3993a-137">Information om blob logg</span><span class="sxs-lookup"><span data-stu-id="3993a-137">Log blob detail information</span></span>
<span data-ttu-id="3993a-138">Varje blobb i behållaren innehåller diagnostikinformationen för exakt ett av följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="3993a-138">Each blob in the container holds the diagnostics information for exactly one of the following actions:</span></span>

* <span data-ttu-id="3993a-139">En körning av metoden Batch Execution</span><span class="sxs-lookup"><span data-stu-id="3993a-139">An execution of the Batch-Execution method</span></span>  
* <span data-ttu-id="3993a-140">En körning av metoden begäran och svar</span><span class="sxs-lookup"><span data-stu-id="3993a-140">An execution of the Request-Response method</span></span>  
* <span data-ttu-id="3993a-141">Initieringen av en begäran och svar-behållare</span><span class="sxs-lookup"><span data-stu-id="3993a-141">Initialization of a Request-Response container</span></span>

<span data-ttu-id="3993a-142">Namnet på varje blobb har ett prefix i följande format:</span><span class="sxs-lookup"><span data-stu-id="3993a-142">The name of each blob has a prefix of the following form:</span></span> 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


<span data-ttu-id="3993a-143">Där _logga typen_ är ett av följande värden:</span><span class="sxs-lookup"><span data-stu-id="3993a-143">Where _Log type_ is one of the following values:</span></span>  

* <span data-ttu-id="3993a-144">Batch</span><span class="sxs-lookup"><span data-stu-id="3993a-144">batch</span></span>  
* <span data-ttu-id="3993a-145">score-begäranden</span><span class="sxs-lookup"><span data-stu-id="3993a-145">score/requests</span></span>  
* <span data-ttu-id="3993a-146">poäng/init</span><span class="sxs-lookup"><span data-stu-id="3993a-146">score/init</span></span>  

