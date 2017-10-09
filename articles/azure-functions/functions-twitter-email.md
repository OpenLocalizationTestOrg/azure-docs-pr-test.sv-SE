---
title: aaaCreate en funktion som kan integreras med Azure Logikappar | Microsoft Docs
description: "Skapa en funktion som kan integreras med Azure Logikappar och Azure kognitiva Services toocategorize tweet sentiment och skicka meddelanden när sentiment är dålig."
services: functions, logic-apps, cognitive-services
keywords: workflow, cloud apps, cloud services, business processes, system integration, enterprise application integration, EAI
documentationcenter: 
author: ggailey777
manager: erikre
editor: 
ms.assetid: 60495cc5-1638-4bf0-8174-52786d227734
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: e176bd946af9a3684b3ad0e4b1bed1c3ee344019
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a><span data-ttu-id="3c58d-104">Skapa en funktion som kan integreras med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="3c58d-104">Create a function that integrates with Azure Logic Apps</span></span>

<span data-ttu-id="3c58d-105">Azure Functions kan integreras med Azure Logikappar hello Logic Apps Designer.</span><span class="sxs-lookup"><span data-stu-id="3c58d-105">Azure Functions integrates with Azure Logic Apps in hello Logic Apps Designer.</span></span> <span data-ttu-id="3c58d-106">Den här integreringen kan du använda hello datorkraft av funktioner i orkestreringarna med andra Azure och tjänster från tredje part.</span><span class="sxs-lookup"><span data-stu-id="3c58d-106">This integration lets you use hello computing power of Functions in orchestrations with other Azure and third-party services.</span></span> 

<span data-ttu-id="3c58d-107">De här självstudierna visar hur toouse fungerar med Logic Apps och Azure kognitiva Services tooanalyze sentiment från Twitter meddelandena.</span><span class="sxs-lookup"><span data-stu-id="3c58d-107">This tutorial shows you how toouse Functions with Logic Apps and Azure Cognitive Services tooanalyze sentiment from Twitter posts.</span></span> <span data-ttu-id="3c58d-108">En funktion för HTTP-utlöses kategoriserar tweets som gröna, gula och röda baserat på hello sentiment poäng.</span><span class="sxs-lookup"><span data-stu-id="3c58d-108">An HTTP triggered function categorizes tweets as green, yellow, or red based on hello sentiment score.</span></span> <span data-ttu-id="3c58d-109">Ett e-postmeddelande skickas när dålig sentiment har identifierats.</span><span class="sxs-lookup"><span data-stu-id="3c58d-109">An email is sent when poor sentiment is detected.</span></span> 

![bild två första stegen i logik App Designer-appen](media/functions-twitter-email/designer1.png)

<span data-ttu-id="3c58d-111">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="3c58d-111">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c58d-112">Skapa ett kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-112">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="3c58d-113">Skapa en funktion som kategoriserar tweet sentiment.</span><span class="sxs-lookup"><span data-stu-id="3c58d-113">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="3c58d-114">Skapa en logikapp som ansluter tooTwitter.</span><span class="sxs-lookup"><span data-stu-id="3c58d-114">Create a logic app that connects tooTwitter.</span></span>
> * <span data-ttu-id="3c58d-115">Lägg till sentiment identifiering toohello logikapp.</span><span class="sxs-lookup"><span data-stu-id="3c58d-115">Add sentiment detection toohello logic app.</span></span> 
> * <span data-ttu-id="3c58d-116">Ansluta hello logik app toohello funktion.</span><span class="sxs-lookup"><span data-stu-id="3c58d-116">Connect hello logic app toohello function.</span></span>
> * <span data-ttu-id="3c58d-117">Skicka ett e-post baserat på hello svar från hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-117">Send an email based on hello response from hello function.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c58d-118">Krav</span><span class="sxs-lookup"><span data-stu-id="3c58d-118">Prerequisites</span></span>

+ <span data-ttu-id="3c58d-119">En aktiv [Twitter](https://twitter.com/) konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-119">An active [Twitter](https://twitter.com/) account.</span></span> 
+ <span data-ttu-id="3c58d-120">En [Outlook.com](https://outlook.com/) konto (för att skicka meddelanden).</span><span class="sxs-lookup"><span data-stu-id="3c58d-120">An [Outlook.com](https://outlook.com/) account (for sending notifications).</span></span>
+ <span data-ttu-id="3c58d-121">Det här avsnittet används som första plats hello resurserna skapas i [skapa din första funktion från hello Azure-portalen](functions-create-first-azure-function.md).</span><span class="sxs-lookup"><span data-stu-id="3c58d-121">This topic uses as its starting point hello resources created in [Create your first function from hello Azure portal](functions-create-first-azure-function.md).</span></span>  
<span data-ttu-id="3c58d-122">Om du inte redan har gjort slutföra dessa steg nu toocreate funktionen appen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-122">If you haven't already done so, complete these steps now toocreate your function app.</span></span>

## <a name="create-a-cognitive-services-account"></a><span data-ttu-id="3c58d-123">Skapa en kognitiva Services-konto</span><span class="sxs-lookup"><span data-stu-id="3c58d-123">Create a Cognitive Services account</span></span>

<span data-ttu-id="3c58d-124">Ett kognitiva Services-konto är obligatoriskt toodetect hello sentiment av tweets som övervakas.</span><span class="sxs-lookup"><span data-stu-id="3c58d-124">A Cognitive Services account is required toodetect hello sentiment of tweets being monitored.</span></span>

1. <span data-ttu-id="3c58d-125">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3c58d-125">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="3c58d-126">Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-126">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

3. <span data-ttu-id="3c58d-127">Klicka på **Data + analys** > **kognitiva Services**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-127">Click **Data + Analytics** > **Cognitive  Services**.</span></span> <span data-ttu-id="3c58d-128">Sedan hello inställningar som anges i hello tabell acceptera hello och kontrollera **PIN-kod toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-128">Then, use hello settings as specified in hello table, accept hello terms, and check **Pin toodashboard**.</span></span>

    ![Skapa kognitiva kontoblad](media/functions-twitter-email/cog_svcs_account.png)

    | <span data-ttu-id="3c58d-130">Inställning</span><span class="sxs-lookup"><span data-stu-id="3c58d-130">Setting</span></span>      |  <span data-ttu-id="3c58d-131">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="3c58d-131">Suggested value</span></span>   | <span data-ttu-id="3c58d-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3c58d-132">Description</span></span>                                        |
    | --- | --- | --- |
    | <span data-ttu-id="3c58d-133">**Namn**</span><span class="sxs-lookup"><span data-stu-id="3c58d-133">**Name**</span></span> | <span data-ttu-id="3c58d-134">MyCognitiveServicesAccnt</span><span class="sxs-lookup"><span data-stu-id="3c58d-134">MyCognitiveServicesAccnt</span></span> | <span data-ttu-id="3c58d-135">Välj ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="3c58d-135">Choose a unique account name.</span></span> |
    | <span data-ttu-id="3c58d-136">**API-typ**</span><span class="sxs-lookup"><span data-stu-id="3c58d-136">**API type**</span></span> | <span data-ttu-id="3c58d-137">API för textanalys</span><span class="sxs-lookup"><span data-stu-id="3c58d-137">Text Analytics API</span></span> | <span data-ttu-id="3c58d-138">API användes tooanalyze text.</span><span class="sxs-lookup"><span data-stu-id="3c58d-138">API used tooanalyze text.</span></span>  |
    | <span data-ttu-id="3c58d-139">**Plats**</span><span class="sxs-lookup"><span data-stu-id="3c58d-139">**Location**</span></span> | <span data-ttu-id="3c58d-140">Västra USA</span><span class="sxs-lookup"><span data-stu-id="3c58d-140">West US</span></span> | <span data-ttu-id="3c58d-141">För närvarande endast **västra USA** är tillgänglig för textanalys.</span><span class="sxs-lookup"><span data-stu-id="3c58d-141">Currently, only **West US** is available for text analytics.</span></span> |
    | <span data-ttu-id="3c58d-142">**prisnivå**</span><span class="sxs-lookup"><span data-stu-id="3c58d-142">**Pricing tier**</span></span> | <span data-ttu-id="3c58d-143">F0</span><span class="sxs-lookup"><span data-stu-id="3c58d-143">F0</span></span> | <span data-ttu-id="3c58d-144">Börja med hello lägsta nivån.</span><span class="sxs-lookup"><span data-stu-id="3c58d-144">Start with hello lowest tier.</span></span> <span data-ttu-id="3c58d-145">Om du kör out-of-anrop, skala tooa högre nivå.</span><span class="sxs-lookup"><span data-stu-id="3c58d-145">If you run out of calls, scale tooa higher tier.</span></span>|
    | <span data-ttu-id="3c58d-146">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="3c58d-146">**Resource group**</span></span> | <span data-ttu-id="3c58d-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="3c58d-147">myResourceGroup</span></span> | <span data-ttu-id="3c58d-148">Använd hello samma resursgrupp för alla tjänster i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-148">Use hello same resource group for all services in this tutorial.</span></span>|

4. <span data-ttu-id="3c58d-149">Klicka på **skapa** toocreate ditt konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-149">Click **Create** toocreate your account.</span></span> <span data-ttu-id="3c58d-150">Klicka på instrumentpanelen nya kognitiva Services för kontot Fäst toohello när hello kontot skapas.</span><span class="sxs-lookup"><span data-stu-id="3c58d-150">After hello account is created, click your new Cognitive Services account pinned toohello dashboard.</span></span> 

5. <span data-ttu-id="3c58d-151">I hello konto, klickar du på **nycklar**, och sedan kopiera hello värdet för **nyckel 1** och spara den.</span><span class="sxs-lookup"><span data-stu-id="3c58d-151">In hello account, click **Keys**, and then copy hello value of **Key 1** and save it.</span></span> <span data-ttu-id="3c58d-152">Du kan använda den här nyckeln tooconnect hello logik app tooyour kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-152">You use this key tooconnect hello logic app tooyour Cognitive Services account.</span></span> 
 
    ![Nycklar](media/functions-twitter-email/keys.png)

## <a name="create-hello-function"></a><span data-ttu-id="3c58d-154">Skapa hello-funktion</span><span class="sxs-lookup"><span data-stu-id="3c58d-154">Create hello function</span></span>

<span data-ttu-id="3c58d-155">Funktioner ger en bra sätt toooffload bearbetningar i ett arbetsflöde för logic apps.</span><span class="sxs-lookup"><span data-stu-id="3c58d-155">Functions provides a great way toooffload processing tasks in a logic apps workflow.</span></span> <span data-ttu-id="3c58d-156">Den här kursen använder en HTTP-aktiveras funktionen tooprocess tweet sentiment resultat från kognitiva tjänster och returnera ett kategorivärde.</span><span class="sxs-lookup"><span data-stu-id="3c58d-156">This tutorial uses an HTTP triggered function tooprocess tweet sentiment scores from Cognitive Services and return a category value.</span></span>  

1. <span data-ttu-id="3c58d-157">Expandera din funktionsapp, klickar du på hello  **+**  knappen för nästa**funktioner**, klicka på hello **HTTPTrigger** mall.</span><span class="sxs-lookup"><span data-stu-id="3c58d-157">Expand your function app, click hello **+** button next too**Functions**, click hello **HTTPTrigger** template.</span></span> <span data-ttu-id="3c58d-158">Typen `CategorizeSentiment` för hello funktionen **namn** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-158">Type `CategorizeSentiment` for hello function **Name** and click **Create**.</span></span>

    ![Funktionen appar bladet funktioner +](media/functions-twitter-email/add_fun.png)

2. <span data-ttu-id="3c58d-160">Ersätt hello innehållet i hello run.csx filen med följande kod hello och klicka sedan på **spara**:</span><span class="sxs-lookup"><span data-stu-id="3c58d-160">Replace hello contents of hello run.csx file with hello following code, then click **Save**:</span></span>

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // hello sentiment category defaults too'GREEN'. 
        string category = "GREEN";
    
        // Get hello sentiment score from hello request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("hello sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set hello category based on hello sentiment score.
        if (score < .3)
        {
            category = "RED";
        }
        else if (score < .6)
        {
            category = "YELLOW";
        }
        return req.CreateResponse(HttpStatusCode.OK, category);
    }
    ```
    <span data-ttu-id="3c58d-161">Den här Funktionskoden returnerar färg kategorin baserat på hello sentiment poäng togs emot i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="3c58d-161">This function code returns a color category based on hello sentiment score received in hello request.</span></span> 

3. <span data-ttu-id="3c58d-162">tootest hello funktion, klickar du på **Test** hello längst till höger tooexpand hello Test fliken. Ange ett värde för `0.2` för hello **Begärandetext**, och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-162">tootest hello function, click **Test** at hello far right tooexpand hello Test tab. Type a value of `0.2` for hello **Request body**, and then click **Run**.</span></span> <span data-ttu-id="3c58d-163">Ett värde av **RED** returneras i hello brödtext hello svar.</span><span class="sxs-lookup"><span data-stu-id="3c58d-163">A value of **RED** is returned in hello body of hello response.</span></span> 

    ![Testa hello-funktionen i hello Azure-portalen](./media/functions-twitter-email/test.png)

<span data-ttu-id="3c58d-165">Nu har du en funktion som kategoriserar sentiment resultat.</span><span class="sxs-lookup"><span data-stu-id="3c58d-165">Now you have a function that categorizes sentiment scores.</span></span> <span data-ttu-id="3c58d-166">Därefter kan du skapa en logikapp som integrerar din funktion med Twitter och kognitiva Services-konton.</span><span class="sxs-lookup"><span data-stu-id="3c58d-166">Next, you create a logic app that integrates your function with your Twitter and Cognitive Services accounts.</span></span> 

## <a name="create-a-logic-app"></a><span data-ttu-id="3c58d-167">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="3c58d-167">Create a logic app</span></span>   

1. <span data-ttu-id="3c58d-168">I hello Azure-portalen klickar du på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-168">In hello Azure portal, click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="3c58d-169">Klicka på **företagsintegration** > **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-169">Click **Enterprise Integration** > **Logic App**.</span></span> <span data-ttu-id="3c58d-170">Kontrollera sedan hello inställningar som anges i tabellen hello **PIN-kod toodashboard**, och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-170">Then, use hello settings as specified in hello table, check **Pin toodashboard**, and click **Create**.</span></span>
 
4. <span data-ttu-id="3c58d-171">Skriv en **namn** som `TweetSentiment`, använda hello inställningar som anges i hello tabell, acceptera hello och kontrollera **PIN-kod toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-171">Then, type a **Name** like `TweetSentiment`,  use hello settings as specified in hello table, accept hello terms, and check **Pin toodashboard**.</span></span>

    ![Skapa en logikapp i hello Azure-portalen](./media/functions-twitter-email/new_logicApp.png)

    | <span data-ttu-id="3c58d-173">Inställning</span><span class="sxs-lookup"><span data-stu-id="3c58d-173">Setting</span></span>      |  <span data-ttu-id="3c58d-174">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="3c58d-174">Suggested value</span></span>   | <span data-ttu-id="3c58d-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3c58d-175">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="3c58d-176">**Namn**</span><span class="sxs-lookup"><span data-stu-id="3c58d-176">**Name**</span></span> | <span data-ttu-id="3c58d-177">TweetSentiment</span><span class="sxs-lookup"><span data-stu-id="3c58d-177">TweetSentiment</span></span> | <span data-ttu-id="3c58d-178">Välj ett lämpligt namn för din app.</span><span class="sxs-lookup"><span data-stu-id="3c58d-178">Choose an appropriate name for your app.</span></span> |
    | <span data-ttu-id="3c58d-179">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="3c58d-179">**Resource group**</span></span> | <span data-ttu-id="3c58d-180">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="3c58d-180">myResourceGroup</span></span> | <span data-ttu-id="3c58d-181">API användes tooanalyze text.</span><span class="sxs-lookup"><span data-stu-id="3c58d-181">API used tooanalyze text.</span></span>  |
    | <span data-ttu-id="3c58d-182">**Plats**</span><span class="sxs-lookup"><span data-stu-id="3c58d-182">**Location**</span></span> | <span data-ttu-id="3c58d-183">Östra USA</span><span class="sxs-lookup"><span data-stu-id="3c58d-183">East US</span></span> | <span data-ttu-id="3c58d-184">Välj en plats Stäng tooyou.</span><span class="sxs-lookup"><span data-stu-id="3c58d-184">Choose a location close tooyou.</span></span> |
    | <span data-ttu-id="3c58d-185">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="3c58d-185">**Resource group**</span></span> | <span data-ttu-id="3c58d-186">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="3c58d-186">myResourceGroup</span></span> | <span data-ttu-id="3c58d-187">Välj hello samma befintlig resursgrupp som tidigare.</span><span class="sxs-lookup"><span data-stu-id="3c58d-187">Choose hello same existing resource group as before.</span></span>|

4. <span data-ttu-id="3c58d-188">Klicka på **skapa** toocreate logikappen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-188">Click **Create** toocreate your logic app.</span></span> <span data-ttu-id="3c58d-189">När hello app har skapats klickar du på din nya logik app Fäst toohello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="3c58d-189">After hello app is created, click your new logic app pinned toohello dashboard.</span></span> <span data-ttu-id="3c58d-190">Rulla ned i hello Logic Apps Designer, och klicka på hello sedan **tom Logikapp** mall.</span><span class="sxs-lookup"><span data-stu-id="3c58d-190">Then in hello Logic Apps Designer, scroll down and click hello **Blank Logic App** template.</span></span> 

    ![Tom mall för Logic Apps](media/functions-twitter-email/blank.png)

<span data-ttu-id="3c58d-192">Du kan nu använda hello Logic Apps Designer tooadd tjänster och utlösare tooyour app.</span><span class="sxs-lookup"><span data-stu-id="3c58d-192">You can now use hello Logic Apps Designer tooadd services and triggers tooyour app.</span></span>

## <a name="connect-tootwitter"></a><span data-ttu-id="3c58d-193">Ansluta tooTwitter</span><span class="sxs-lookup"><span data-stu-id="3c58d-193">Connect tooTwitter</span></span>

<span data-ttu-id="3c58d-194">Först skapa en anslutning tooyour Twitter-konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-194">First, create a connection tooyour Twitter account.</span></span> <span data-ttu-id="3c58d-195">Hej logikapp avsöker för tweets som utlöser hello app toorun.</span><span class="sxs-lookup"><span data-stu-id="3c58d-195">hello logic app polls for tweets, which trigger hello app toorun.</span></span>

1. <span data-ttu-id="3c58d-196">I hello designer klickar du på hello **Twitter** tjänsten och klickar på hello **när en ny tweet skickas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="3c58d-196">In hello designer, click hello **Twitter** service, and click hello **When a new tweet is posted** trigger.</span></span> <span data-ttu-id="3c58d-197">Logga in tooyour Twitter-konto och auktorisera Logic Apps toouse ditt konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-197">Sign in tooyour Twitter account and authorize Logic Apps toouse your account.</span></span>

2. <span data-ttu-id="3c58d-198">Använda hello Twitter utlösaren inställningar som anges i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="3c58d-198">Use hello Twitter trigger settings as specified in hello table.</span></span> 

    ![Twitter connector-inställningar](media/functions-twitter-email/azure_tweet.png)

    | <span data-ttu-id="3c58d-200">Inställning</span><span class="sxs-lookup"><span data-stu-id="3c58d-200">Setting</span></span>      |  <span data-ttu-id="3c58d-201">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="3c58d-201">Suggested value</span></span>   | <span data-ttu-id="3c58d-202">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3c58d-202">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="3c58d-203">**Söktext**</span><span class="sxs-lookup"><span data-stu-id="3c58d-203">**Search text**</span></span> | <span data-ttu-id="3c58d-204">#Azure</span><span class="sxs-lookup"><span data-stu-id="3c58d-204">#Azure</span></span> | <span data-ttu-id="3c58d-205">Använd en hashtaggar som populära för toogenerate nya tweets hello valt intervall.</span><span class="sxs-lookup"><span data-stu-id="3c58d-205">Use a hashtag that is popular enough for toogenerate new tweets in hello chosen interval.</span></span> <span data-ttu-id="3c58d-206">När det är för populära med hello kostnadsfria nivån och din hashtaggar kan använda du snabbt in hello transaktioner i kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-206">When using hello Free tier and your hashtag is too popular, you can quickly use up hello transactions in your Cognitive Services account.</span></span> |
    | <span data-ttu-id="3c58d-207">**Frekvens**</span><span class="sxs-lookup"><span data-stu-id="3c58d-207">**Frequency**</span></span> | <span data-ttu-id="3c58d-208">Minut</span><span class="sxs-lookup"><span data-stu-id="3c58d-208">Minute</span></span> | <span data-ttu-id="3c58d-209">hello frekvens enhet som används för avsökning Twitter.</span><span class="sxs-lookup"><span data-stu-id="3c58d-209">hello frequency unit used for polling Twitter.</span></span>  |
    | <span data-ttu-id="3c58d-210">**Intervall**</span><span class="sxs-lookup"><span data-stu-id="3c58d-210">**Interval**</span></span> | <span data-ttu-id="3c58d-211">15</span><span class="sxs-lookup"><span data-stu-id="3c58d-211">15</span></span> | <span data-ttu-id="3c58d-212">hello förfluten tid mellan Twitter begäranden i frekvens enheter.</span><span class="sxs-lookup"><span data-stu-id="3c58d-212">hello time elapsed between Twitter requests, in frequency units.</span></span> |

3.  <span data-ttu-id="3c58d-213">Klicka på **spara** tooconnect tooyour Twitter-konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-213">Click **Save** tooconnect tooyour Twitter account.</span></span> 

<span data-ttu-id="3c58d-214">Appen är nu ansluten tooTwitter.</span><span class="sxs-lookup"><span data-stu-id="3c58d-214">Now your app is connected tooTwitter.</span></span> <span data-ttu-id="3c58d-215">Därefter måste ansluta du tootext analytics toodetect hello sentiment av insamlade tweets.</span><span class="sxs-lookup"><span data-stu-id="3c58d-215">Next, you connect tootext analytics toodetect hello sentiment of collected tweets.</span></span>

## <a name="add-sentiment-detection"></a><span data-ttu-id="3c58d-216">Lägg till sentiment identifiering</span><span class="sxs-lookup"><span data-stu-id="3c58d-216">Add sentiment detection</span></span>

1. <span data-ttu-id="3c58d-217">Klicka på **nytt steg**, och sedan **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-217">Click **New Step**, and then **Add an action**.</span></span>

    ![Nytt steg och Lägg till en åtgärd](media/functions-twitter-email/new_step.png)

2. <span data-ttu-id="3c58d-219">I **välja en åtgärd**, klickar du på **textanalys**, och klicka sedan på hello **identifiera sentiment** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3c58d-219">In **Choose an action**, click **Text Analytics**, and then click hello **Detect sentiment** action.</span></span>

    ![Identifiera Sentiment](media/functions-twitter-email/detect_sent.png)

3. <span data-ttu-id="3c58d-221">Ange ett anslutningsnamn `MyCognitiveServicesConnection`klistra in hello-nyckel för dina kognitiva tjänster konto som du sparade och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-221">Type a connection name such as `MyCognitiveServicesConnection`, paste hello key for your Cognitive Services account that you saved, and click **Create**.</span></span>  

4. <span data-ttu-id="3c58d-222">Klicka på **Text tooanalyze** > **Twittra text**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-222">Click **Text tooanalyze** > **Tweet text**, and then click **Save**.</span></span>  

    ![Identifiera Sentiment](media/functions-twitter-email/ds_tta.png)

<span data-ttu-id="3c58d-224">Du kan lägga till en anslutning tooyour fungerar som förbrukar hello sentiment poäng utdata sentiment identifiering är konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="3c58d-224">Now that sentiment detection is configured, you can add a connection tooyour function that consumes hello sentiment score output.</span></span>

## <a name="connect-sentiment-output-tooyour-function"></a><span data-ttu-id="3c58d-225">Ansluta sentiment utdata tooyour funktion</span><span class="sxs-lookup"><span data-stu-id="3c58d-225">Connect sentiment output tooyour function</span></span>

1. <span data-ttu-id="3c58d-226">I hello Logic Apps Designer, klickar du på **nytt steg** > **lägga till en åtgärd**, och klicka sedan på **Azure Functions**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-226">In hello Logic Apps Designer, click **New step** > **Add an action**, and then click **Azure Functions**.</span></span> 

2. <span data-ttu-id="3c58d-227">Klicka på **välja en Azure-funktion**väljer hello **CategorizeSentiment** funktionen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="3c58d-227">Click **Choose an Azure function**, select hello **CategorizeSentiment** function you created earlier.</span></span>  

    ![Azure funktionsrutan visar Välj en Azure-funktion](media/functions-twitter-email/choose_fun.png)

3. <span data-ttu-id="3c58d-229">I **brödtext i begäran**, klickar du på **poäng** och sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-229">In **Request Body**, click **Score** and then **Save**.</span></span>

    ![Poäng](media/functions-twitter-email/trigger_score.png)

<span data-ttu-id="3c58d-231">Nu kan utlöses din funktion när en sentiment poäng skickas från hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="3c58d-231">Now, your function is triggered when a sentiment score is sent from hello logic app.</span></span> <span data-ttu-id="3c58d-232">En färgkodade kategori returneras toohello logikapp av hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-232">A color-coded category is returned toohello logic app by hello function.</span></span> <span data-ttu-id="3c58d-233">Sedan lägger du till ett e-postmeddelande som skickas när sentiment värdet **RED** returneras från hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-233">Next, you add an email notification that is sent when a sentiment value of **RED** is returned from hello function.</span></span> 

## <a name="add-email-notifications"></a><span data-ttu-id="3c58d-234">Lägg till e-postaviseringar</span><span class="sxs-lookup"><span data-stu-id="3c58d-234">Add email notifications</span></span>

<span data-ttu-id="3c58d-235">hello sista delen av hello arbetsflödet är tootrigger ett e-postmeddelande när hello sentiment beräknas som _RED_.</span><span class="sxs-lookup"><span data-stu-id="3c58d-235">hello last part of hello workflow is tootrigger an email when hello sentiment is scored as _RED_.</span></span> <span data-ttu-id="3c58d-236">Det här avsnittet använder en Outlook.com-koppling.</span><span class="sxs-lookup"><span data-stu-id="3c58d-236">This topic uses an Outlook.com connector.</span></span> <span data-ttu-id="3c58d-237">Du kan utföra liknande steg toouse Gmail eller Office 365 Outlook-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-237">You can perform similar steps toouse a Gmail or Office 365 Outlook connector.</span></span>   

1. <span data-ttu-id="3c58d-238">I hello Logic Apps Designer, klickar du på **nytt steg** > **Lägg till ett villkor**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-238">In hello Logic Apps Designer, click **New step** > **Add a condition**.</span></span> 

2. <span data-ttu-id="3c58d-239">Klicka på **väljer ett värde**, klicka på **brödtext**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-239">Click **Choose a value**, then click **Body**.</span></span> <span data-ttu-id="3c58d-240">Välj **är lika med**, klickar du på **väljer ett värde** och skriv `RED`, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-240">Select **is equal to**, click **Choose a value** and type `RED`, and click **Save**.</span></span> 

    ![Lägg till ett villkor toohello logikapp.](media/functions-twitter-email/condition.png)

3. <span data-ttu-id="3c58d-242">I **om Ja, gör INGENTING**, klickar du på **lägga till en åtgärd**, söka efter `outlook.com`, klickar du på **skickar ett e-**, och logga in tooyour Outlook.com-konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-242">In **IF YES, DO NOTHING**, click **Add an action**, search for `outlook.com`, click **Send an email**, and sign in tooyour Outlook.com account.</span></span>
    
    ![Välj en åtgärd för hello villkoret.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > <span data-ttu-id="3c58d-244">Om du inte har en Outlook.com-konto, kan du välja en annan koppling, t.ex Gmail eller Office 365 Outlook</span><span class="sxs-lookup"><span data-stu-id="3c58d-244">If you don't have an Outlook.com account, you can choose another connector, such as Gmail or Office 365 Outlook</span></span>

4. <span data-ttu-id="3c58d-245">I hello **skickar ett e-** åtgärd, Använd hello e-inställningar som anges i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="3c58d-245">In hello **Send an email** action, use hello email settings as specified in hello table.</span></span> 

    ![Konfigurera hello e-post för hello skicka en e-åtgärd.](media/functions-twitter-email/sendemail.png)

    | <span data-ttu-id="3c58d-247">Inställning</span><span class="sxs-lookup"><span data-stu-id="3c58d-247">Setting</span></span>      |  <span data-ttu-id="3c58d-248">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="3c58d-248">Suggested value</span></span>   | <span data-ttu-id="3c58d-249">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3c58d-249">Description</span></span>  |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="3c58d-250">**Att**</span><span class="sxs-lookup"><span data-stu-id="3c58d-250">**To**</span></span> | <span data-ttu-id="3c58d-251">Skriv din e-postadress</span><span class="sxs-lookup"><span data-stu-id="3c58d-251">Type your email address</span></span> | <span data-ttu-id="3c58d-252">hello e-postadress som tar emot hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="3c58d-252">hello email address that receives hello notification.</span></span> |
    | <span data-ttu-id="3c58d-253">**Ämne**</span><span class="sxs-lookup"><span data-stu-id="3c58d-253">**Subject**</span></span> | <span data-ttu-id="3c58d-254">Negativa tweets sentiment upptäcktes</span><span class="sxs-lookup"><span data-stu-id="3c58d-254">Negative tweet sentiment detected</span></span>  | <span data-ttu-id="3c58d-255">hello ämnesraden i hello e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="3c58d-255">hello subject line of hello email notification.</span></span>  |
    | <span data-ttu-id="3c58d-256">**Brödtext**</span><span class="sxs-lookup"><span data-stu-id="3c58d-256">**Body**</span></span> | <span data-ttu-id="3c58d-257">Tweet text, plats</span><span class="sxs-lookup"><span data-stu-id="3c58d-257">Tweet text, Location</span></span> | <span data-ttu-id="3c58d-258">Klicka på hello **Twittra text** och **plats** parametrar.</span><span class="sxs-lookup"><span data-stu-id="3c58d-258">Click hello **Tweet text** and **Location** parameters.</span></span> |

5.  <span data-ttu-id="3c58d-259">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3c58d-259">Click **Save**.</span></span>

<span data-ttu-id="3c58d-260">Hello arbetsflödet har slutförts, kan du aktivera hello logikapp och se hello-funktionen på arbetet.</span><span class="sxs-lookup"><span data-stu-id="3c58d-260">Now that hello workflow is complete, you can enable hello logic app and see hello function at work.</span></span>

## <a name="test-hello-workflow"></a><span data-ttu-id="3c58d-261">Testa hello arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="3c58d-261">Test hello workflow</span></span>

1. <span data-ttu-id="3c58d-262">I hello logik App Designer, klickar du på **kör** toostart hello app.</span><span class="sxs-lookup"><span data-stu-id="3c58d-262">In hello Logic App Designer, click **Run** toostart hello app.</span></span>

2. <span data-ttu-id="3c58d-263">Klicka på hello vänstra kolumnen **översikt** toosee hello status för hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="3c58d-263">In hello left column, click **Overview** toosee hello status of hello logic app.</span></span> 
 
    ![Logik app Körstatus](media/functions-twitter-email/over1.png)

3. <span data-ttu-id="3c58d-265">(Valfritt) Klicka på en hello körs toosee information om hello körning.</span><span class="sxs-lookup"><span data-stu-id="3c58d-265">(Optional) Click one of hello runs toosee details of hello execution.</span></span>

4. <span data-ttu-id="3c58d-266">Gå tooyour funktion, visa hello loggar och kontrollera att sentiment värden tagits emot och bearbetas.</span><span class="sxs-lookup"><span data-stu-id="3c58d-266">Go tooyour function, view hello logs, and verify that sentiment values were received and processed.</span></span>
 
    ![Visa funktionsloggar](media/functions-twitter-email/sent.png)

5. <span data-ttu-id="3c58d-268">Du får ett e-postmeddelande när potentiellt negativa sentiment har identifierats.</span><span class="sxs-lookup"><span data-stu-id="3c58d-268">When a potentially negative sentiment is detected, you receive an email.</span></span> <span data-ttu-id="3c58d-269">Om du inte har fått ett e-postmeddelande, kan du ändra hello funktionen kod tooreturn RED varje gång:</span><span class="sxs-lookup"><span data-stu-id="3c58d-269">If you haven't received an email, you can change hello function code tooreturn RED every time:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    <span data-ttu-id="3c58d-270">När du har kontrollerat e-postmeddelanden, ändra tillbaka toohello ursprungliga koden:</span><span class="sxs-lookup"><span data-stu-id="3c58d-270">After you have verified email notifications, change back toohello original code:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > <span data-ttu-id="3c58d-271">När du har slutfört den här självstudiekursen, bör du inaktivera hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="3c58d-271">After you have completed this tutorial, you should disable hello logic app.</span></span> <span data-ttu-id="3c58d-272">Genom att inaktivera hello app undviker du att debiteras för körningar och använder upp hello transaktioner i kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-272">By disabling hello app, you avoid being charged for executions and using up hello transactions in your Cognitive Services account.</span></span>

<span data-ttu-id="3c58d-273">Nu har du sett hur enkelt det är toointegrate funktioner i ett arbetsflöde för Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="3c58d-273">Now you have seen how easy it is toointegrate Functions into a Logic Apps workflow.</span></span>

## <a name="disable-hello-logic-app"></a><span data-ttu-id="3c58d-274">Inaktivera hello logikapp</span><span class="sxs-lookup"><span data-stu-id="3c58d-274">Disable hello logic app</span></span>

<span data-ttu-id="3c58d-275">toodisable hello logikapp, klickar du på **översikt** och klicka sedan på **inaktivera** hello överst i hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-275">toodisable hello logic app, click **Overview** and then click **Disable** at hello top of hello screen.</span></span> <span data-ttu-id="3c58d-276">Detta stoppar hello logikapp köra och medför kostnader utan att ta bort hello app.</span><span class="sxs-lookup"><span data-stu-id="3c58d-276">This stops hello logic app from running and incurring charges without deleting hello app.</span></span> 

![Funktionsloggar](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a><span data-ttu-id="3c58d-278">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c58d-278">Next steps</span></span>

<span data-ttu-id="3c58d-279">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="3c58d-279">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c58d-280">Skapa ett kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="3c58d-280">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="3c58d-281">Skapa en funktion som kategoriserar tweet sentiment.</span><span class="sxs-lookup"><span data-stu-id="3c58d-281">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="3c58d-282">Skapa en logikapp som ansluter tooTwitter.</span><span class="sxs-lookup"><span data-stu-id="3c58d-282">Create a logic app that connects tooTwitter.</span></span>
> * <span data-ttu-id="3c58d-283">Lägg till sentiment identifiering toohello logikapp.</span><span class="sxs-lookup"><span data-stu-id="3c58d-283">Add sentiment detection toohello logic app.</span></span> 
> * <span data-ttu-id="3c58d-284">Ansluta hello logik app toohello funktion.</span><span class="sxs-lookup"><span data-stu-id="3c58d-284">Connect hello logic app toohello function.</span></span>
> * <span data-ttu-id="3c58d-285">Skicka ett e-post baserat på hello svar från hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="3c58d-285">Send an email based on hello response from hello function.</span></span>

<span data-ttu-id="3c58d-286">I förväg toohello nästa självstudiekurs toolearn hur toocreate serverlösa API för din funktion.</span><span class="sxs-lookup"><span data-stu-id="3c58d-286">Advance toohello next tutorial toolearn how toocreate a serverless API for your function.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="3c58d-287">Skapa ett API utan server med Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3c58d-287">Create a serverless API using Azure Functions</span></span>](functions-create-serverless-api.md)

<span data-ttu-id="3c58d-288">toolearn mer om Logic Apps finns [Azure Logikappar](../logic-apps/logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="3c58d-288">toolearn more about Logic Apps, see [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span></span>

