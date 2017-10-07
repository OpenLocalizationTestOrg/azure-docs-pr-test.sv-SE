---
title: "aaaLogging för Machine Learning-webbtjänster | Microsoft Docs"
description: "Lär dig hur tooenable loggning för Machine Learning-webbtjänster. Loggning ger ytterligare information toohelp felsöka hello API: er."
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
ms.openlocfilehash: ed23933d52d2151af658af2307d7df8743071f65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a><span data-ttu-id="f2256-104">Aktivera loggning för Machine Learning-webbtjänster</span><span class="sxs-lookup"><span data-stu-id="f2256-104">Enable logging for Machine Learning web services</span></span>
<span data-ttu-id="f2256-105">Det här dokumentet innehåller information om hello loggning möjligheterna för Machine Learning-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="f2256-105">This document provides information on hello logging capability of Machine Learning web services.</span></span> <span data-ttu-id="f2256-106">Loggning ger ytterligare information, förutom bara ett felnummer och ett meddelande som kan hjälpa dig att felsöka din anrop toohello Machine Learning API: er.</span><span class="sxs-lookup"><span data-stu-id="f2256-106">Logging provides additional information, beyond just an error number and a message, that can help you troubleshoot your calls toohello Machine Learning APIs.</span></span>  

## <a name="how-tooenable-logging-for-a-web-service"></a><span data-ttu-id="f2256-107">Hur tooenable loggning för en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="f2256-107">How tooenable logging for a Web service</span></span>

<span data-ttu-id="f2256-108">Du aktiverar loggning från hello [Azure Machine Learning-webbtjänster](https://services.azureml.net) portal.</span><span class="sxs-lookup"><span data-stu-id="f2256-108">You enable logging from hello [Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span> 

1. <span data-ttu-id="f2256-109">Logga in toohello Azure Machine Learning-webbtjänster portalen på [https://services.azureml.net](https://services.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="f2256-109">Sign in toohello Azure Machine Learning Web Services portal at [https://services.azureml.net](https://services.azureml.net).</span></span> <span data-ttu-id="f2256-110">För en klassisk webbtjänst, du kan också få toohello portal genom att klicka på **nya Web Services upplevelsen** på hello Machine Learning-webbtjänster sida i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="f2256-110">For a Classic web service, you can also get toohello portal by clicking **New Web Services Experience** on hello Machine Learning Web Services page in Machine Learning Studio.</span></span>

   ![Ny Services webbupplevelse länk](media/machine-learning-web-services-logging/new-web-services-experience-link.png)

2. <span data-ttu-id="f2256-112">Hello översta menyraden klickar du på **Web Services** för en ny webbtjänst eller klicka på **klassiska webbtjänster** ett klassiskt webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f2256-112">On hello top menu bar, click **Web Services** for a New web service, or click **Classic Web Services** for a Classic web service.</span></span>

   ![Välj ny eller klassisk webbtjänster](media/machine-learning-web-services-logging/select-web-service.png)

3. <span data-ttu-id="f2256-114">Klicka på hello webbtjänstnamn för en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="f2256-114">For a New web service, click hello web service name.</span></span> <span data-ttu-id="f2256-115">Klicka på hello webbtjänstnamn och på hello nästa sida klickar du på lämplig hello-slutpunkten för en klassisk webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="f2256-115">For a Classic web service, click hello web service name and then on hello next page click hello appropriate endpoint.</span></span>

4. <span data-ttu-id="f2256-116">Klicka på hello huvudmenyn **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="f2256-116">On hello top menu bar, click **Configure**.</span></span>

5. <span data-ttu-id="f2256-117">Ange hello **aktivera loggning** alternativet för*fel* (toolog endast fel) eller *alla* (för fullständig loggning).</span><span class="sxs-lookup"><span data-stu-id="f2256-117">Set hello **Enable Logging** option too*Error* (toolog only errors) or *All* (for full logging).</span></span>

   ![Välj loggningsnivån](media/machine-learning-web-services-logging/enable-logging.png)

6. <span data-ttu-id="f2256-119">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f2256-119">Click **Save**.</span></span>

7. <span data-ttu-id="f2256-120">För klassisk webbtjänster, skapa hello **ml-diagnostik** behållare.</span><span class="sxs-lookup"><span data-stu-id="f2256-120">For Classic web services, create hello **ml-diagnostics** container.</span></span>

   <span data-ttu-id="f2256-121">Alla web service loggar sparas i en blobbbehållare med namnet **ml-diagnostik** i hello storage-konto som är associerade med hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f2256-121">All web service logs are kept in a blob container named **ml-diagnostics** in hello storage account associated with hello web service.</span></span> <span data-ttu-id="f2256-122">Nya webbtjänster, har den här behållaren skapats hello första gången du öppnar hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f2256-122">For New web services, this container is created hello first time you access hello web service.</span></span> <span data-ttu-id="f2256-123">För klassisk webbtjänster behöver du toocreate hello behållaren om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="f2256-123">For Classic web services, you need toocreate hello container if it doesn't already exist.</span></span> 

   1. <span data-ttu-id="f2256-124">I hello [Azure-portalen](https://portal.azure.com)går toohello storage-konto som är associerade med hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f2256-124">In hello [Azure portal](https://portal.azure.com), go toohello storage account associated with hello web service.</span></span>

   2. <span data-ttu-id="f2256-125">Under **Blob-tjänsten**, klickar du på **behållare**.</span><span class="sxs-lookup"><span data-stu-id="f2256-125">Under **Blob Service**, click **Containers**.</span></span>

   3. <span data-ttu-id="f2256-126">Om hello behållaren **ml-diagnostik** inte finns, klickar du på **+ behållare**, ge hello behållaren hello namnet ”ml-diagnostik” och välj hello **komma åt typen** som ”Blob”.</span><span class="sxs-lookup"><span data-stu-id="f2256-126">If hello container **ml-diagnostics** doesn't exist, click **+Container**, give hello container hello name "ml-diagnostics", and select hello **Access type** as "Blob".</span></span> <span data-ttu-id="f2256-127">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2256-127">Click **OK**.</span></span>

      ![Välj loggningsnivån](media/machine-learning-web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> <span data-ttu-id="f2256-129">Hello Web Services instrumentpanelen i Machine Learning Studio har också en växel tooenable loggning för en klassisk webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="f2256-129">For a Classic web service, hello Web Services Dashboard in Machine Learning Studio also has a switch tooenable logging.</span></span> <span data-ttu-id="f2256-130">Eftersom loggning hanteras nu via hello Web Services-portalen, behöver du dock tooenable via hello portal som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f2256-130">However, because logging is now managed through hello Web Services portal, you need tooenable logging through hello portal as described in this article.</span></span> <span data-ttu-id="f2256-131">Om du redan loggar in Studio, inaktivera loggning i hello Web Services-portalen och aktivera det igen.</span><span class="sxs-lookup"><span data-stu-id="f2256-131">If you already enabled logging in Studio, then in hello Web Services Portal, disable logging and enable it again.</span></span>


## <a name="hello-effects-of-enabling-logging"></a><span data-ttu-id="f2256-132">hello effekterna av att aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="f2256-132">hello effects of enabling logging</span></span>
<span data-ttu-id="f2256-133">När loggning är aktiverat hello diagnostik- och fel från hello webbtjänstslutpunkt loggas i hello **ml-diagnostik** blob-behållaren i hello Azure Storage-konto som är kopplad till hello användarens arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="f2256-133">When logging is enabled, hello diagnostics and errors from hello web service endpoint are logged in hello **ml-diagnostics** blob container in hello Azure Storage Account linked with hello user’s workspace.</span></span> <span data-ttu-id="f2256-134">Den här behållaren innehåller alla hello diagnostikinformationen för alla hello slutpunkter för webbtjänster för alla hello arbetsytor som är associerade med det här lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="f2256-134">This container holds all hello diagnostics information for all hello web service endpoints for all hello workspaces associated with this storage account.</span></span>

<span data-ttu-id="f2256-135">hello loggar kan visas med hjälp av hello flera verktyg tillgängliga tooexplore ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f2256-135">hello logs can be viewed using any of hello several tools available tooexplore an Azure Storage Account.</span></span> <span data-ttu-id="f2256-136">hello enklaste får toonavigate toohello storage-konto i hello Azure-portalen, klicka på **behållare**, och klicka sedan på hello behållaren **ml-diagnostik**.</span><span class="sxs-lookup"><span data-stu-id="f2256-136">hello easiest may be toonavigate toohello storage account in hello Azure portal, click **Containers**, and then click hello container **ml-diagnostics**.</span></span>  

## <a name="log-blob-detail-information"></a><span data-ttu-id="f2256-137">Information om blob logg</span><span class="sxs-lookup"><span data-stu-id="f2256-137">Log blob detail information</span></span>
<span data-ttu-id="f2256-138">Varje blobb i behållaren hello innehåller hello diagnostikinformationen för exakt ett av hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="f2256-138">Each blob in hello container holds hello diagnostics information for exactly one of hello following actions:</span></span>

* <span data-ttu-id="f2256-139">En körning av hello batchkörning metoden</span><span class="sxs-lookup"><span data-stu-id="f2256-139">An execution of hello Batch-Execution method</span></span>  
* <span data-ttu-id="f2256-140">En körning av metoden hello begäran och svar</span><span class="sxs-lookup"><span data-stu-id="f2256-140">An execution of hello Request-Response method</span></span>  
* <span data-ttu-id="f2256-141">Initieringen av en begäran och svar-behållare</span><span class="sxs-lookup"><span data-stu-id="f2256-141">Initialization of a Request-Response container</span></span>

<span data-ttu-id="f2256-142">hello namnet på varje blob har prefixet hello följande format:</span><span class="sxs-lookup"><span data-stu-id="f2256-142">hello name of each blob has a prefix of hello following form:</span></span> 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


<span data-ttu-id="f2256-143">Där _logga typen_ är en av hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="f2256-143">Where _Log type_ is one of hello following values:</span></span>  

* <span data-ttu-id="f2256-144">Batch</span><span class="sxs-lookup"><span data-stu-id="f2256-144">batch</span></span>  
* <span data-ttu-id="f2256-145">score-begäranden</span><span class="sxs-lookup"><span data-stu-id="f2256-145">score/requests</span></span>  
* <span data-ttu-id="f2256-146">poäng/init</span><span class="sxs-lookup"><span data-stu-id="f2256-146">score/init</span></span>  

