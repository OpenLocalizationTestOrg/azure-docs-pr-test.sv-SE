---
title: Skapa en funktion som kan integreras med Azure Logikappar | Microsoft Docs
description: "Skapa en funktion som kan integreras med Azure Logikappar och kognitiva Azure-tjänster för att kategorisera sentiment tweets och skicka meddelanden när sentiment är dålig."
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
ms.openlocfilehash: 4a5dc668e21c5328b308c8f5852aaa922232374d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-that-integrates-with-azure-logic-apps"></a><span data-ttu-id="c15da-104">Skapa en funktion som kan integreras med Azure Logikappar</span><span class="sxs-lookup"><span data-stu-id="c15da-104">Create a function that integrates with Azure Logic Apps</span></span>

<span data-ttu-id="c15da-105">Azure Functions kan integreras med Azure Logikappar i Logic Apps Designer.</span><span class="sxs-lookup"><span data-stu-id="c15da-105">Azure Functions integrates with Azure Logic Apps in the Logic Apps Designer.</span></span> <span data-ttu-id="c15da-106">Den här integreringen kan du använda datorkraft av funktioner i orkestreringarna med andra Azure och tjänster från tredje part.</span><span class="sxs-lookup"><span data-stu-id="c15da-106">This integration lets you use the computing power of Functions in orchestrations with other Azure and third-party services.</span></span> 

<span data-ttu-id="c15da-107">Den här kursen visar hur du använder funktioner med Logic Apps och kognitiva Azure-tjänster för att analysera sentiment från Twitter meddelandena.</span><span class="sxs-lookup"><span data-stu-id="c15da-107">This tutorial shows you how to use Functions with Logic Apps and Azure Cognitive Services to analyze sentiment from Twitter posts.</span></span> <span data-ttu-id="c15da-108">En funktion för HTTP-utlöses kategoriserar tweets som gröna, gula och röda baserat på sentiment poäng.</span><span class="sxs-lookup"><span data-stu-id="c15da-108">An HTTP triggered function categorizes tweets as green, yellow, or red based on the sentiment score.</span></span> <span data-ttu-id="c15da-109">Ett e-postmeddelande skickas när dålig sentiment har identifierats.</span><span class="sxs-lookup"><span data-stu-id="c15da-109">An email is sent when poor sentiment is detected.</span></span> 

![bild två första stegen i logik App Designer-appen](media/functions-twitter-email/designer1.png)

<span data-ttu-id="c15da-111">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="c15da-111">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c15da-112">Skapa ett kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-112">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="c15da-113">Skapa en funktion som kategoriserar tweet sentiment.</span><span class="sxs-lookup"><span data-stu-id="c15da-113">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="c15da-114">Skapa en logikapp som ansluter till Twitter.</span><span class="sxs-lookup"><span data-stu-id="c15da-114">Create a logic app that connects to Twitter.</span></span>
> * <span data-ttu-id="c15da-115">Lägg till sentiment identifiering logikappen.</span><span class="sxs-lookup"><span data-stu-id="c15da-115">Add sentiment detection to the logic app.</span></span> 
> * <span data-ttu-id="c15da-116">Ansluta logikappen till funktionen.</span><span class="sxs-lookup"><span data-stu-id="c15da-116">Connect the logic app to the function.</span></span>
> * <span data-ttu-id="c15da-117">Skicka ett e-post baserat på svar från funktionen.</span><span class="sxs-lookup"><span data-stu-id="c15da-117">Send an email based on the response from the function.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c15da-118">Krav</span><span class="sxs-lookup"><span data-stu-id="c15da-118">Prerequisites</span></span>

+ <span data-ttu-id="c15da-119">En aktiv [Twitter](https://twitter.com/) konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-119">An active [Twitter](https://twitter.com/) account.</span></span> 
+ <span data-ttu-id="c15da-120">En [Outlook.com](https://outlook.com/) konto (för att skicka meddelanden).</span><span class="sxs-lookup"><span data-stu-id="c15da-120">An [Outlook.com](https://outlook.com/) account (for sending notifications).</span></span>
+ <span data-ttu-id="c15da-121">För det här avsnittet förutsätts de resurser som skapades i avsnittet [Create your first function from the Azure portal](functions-create-first-azure-function.md) (Skapa din första funktion i Azure Portal).</span><span class="sxs-lookup"><span data-stu-id="c15da-121">This topic uses as its starting point the resources created in [Create your first function from the Azure portal](functions-create-first-azure-function.md).</span></span>  
<span data-ttu-id="c15da-122">Om du inte redan gjort det, kan du slutföra de här stegen nu för att skapa funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="c15da-122">If you haven't already done so, complete these steps now to create your function app.</span></span>

## <a name="create-a-cognitive-services-account"></a><span data-ttu-id="c15da-123">Skapa en kognitiva Services-konto</span><span class="sxs-lookup"><span data-stu-id="c15da-123">Create a Cognitive Services account</span></span>

<span data-ttu-id="c15da-124">Ett kognitiva Services-konto krävs för att identifiera sentiment av tweets som övervakas.</span><span class="sxs-lookup"><span data-stu-id="c15da-124">A Cognitive Services account is required to detect the sentiment of tweets being monitored.</span></span>

1. <span data-ttu-id="c15da-125">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c15da-125">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="c15da-126">Klicka på knappen **New** (Nytt) i det övre vänstra hörnet i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c15da-126">Click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

3. <span data-ttu-id="c15da-127">Klicka på **Data + analys** > **kognitiva Services**.</span><span class="sxs-lookup"><span data-stu-id="c15da-127">Click **Data + Analytics** > **Cognitive  Services**.</span></span> <span data-ttu-id="c15da-128">Använd sedan inställningarna som anges i tabellen, accepterar villkoren och kontrollera **fäst på instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="c15da-128">Then, use the settings as specified in the table, accept the terms, and check **Pin to dashboard**.</span></span>

    ![Skapa kognitiva kontoblad](media/functions-twitter-email/cog_svcs_account.png)

    | <span data-ttu-id="c15da-130">Inställning</span><span class="sxs-lookup"><span data-stu-id="c15da-130">Setting</span></span>      |  <span data-ttu-id="c15da-131">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="c15da-131">Suggested value</span></span>   | <span data-ttu-id="c15da-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c15da-132">Description</span></span>                                        |
    | --- | --- | --- |
    | <span data-ttu-id="c15da-133">**Namn**</span><span class="sxs-lookup"><span data-stu-id="c15da-133">**Name**</span></span> | <span data-ttu-id="c15da-134">MyCognitiveServicesAccnt</span><span class="sxs-lookup"><span data-stu-id="c15da-134">MyCognitiveServicesAccnt</span></span> | <span data-ttu-id="c15da-135">Välj ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="c15da-135">Choose a unique account name.</span></span> |
    | <span data-ttu-id="c15da-136">**API-typ**</span><span class="sxs-lookup"><span data-stu-id="c15da-136">**API type**</span></span> | <span data-ttu-id="c15da-137">API för textanalys</span><span class="sxs-lookup"><span data-stu-id="c15da-137">Text Analytics API</span></span> | <span data-ttu-id="c15da-138">API som används för att analysera text.</span><span class="sxs-lookup"><span data-stu-id="c15da-138">API used to analyze text.</span></span>  |
    | <span data-ttu-id="c15da-139">**Plats**</span><span class="sxs-lookup"><span data-stu-id="c15da-139">**Location**</span></span> | <span data-ttu-id="c15da-140">Västra USA</span><span class="sxs-lookup"><span data-stu-id="c15da-140">West US</span></span> | <span data-ttu-id="c15da-141">För närvarande endast **västra USA** är tillgänglig för textanalys.</span><span class="sxs-lookup"><span data-stu-id="c15da-141">Currently, only **West US** is available for text analytics.</span></span> |
    | <span data-ttu-id="c15da-142">**prisnivå**</span><span class="sxs-lookup"><span data-stu-id="c15da-142">**Pricing tier**</span></span> | <span data-ttu-id="c15da-143">F0</span><span class="sxs-lookup"><span data-stu-id="c15da-143">F0</span></span> | <span data-ttu-id="c15da-144">Börja med den lägsta nivån.</span><span class="sxs-lookup"><span data-stu-id="c15da-144">Start with the lowest tier.</span></span> <span data-ttu-id="c15da-145">Om du kör out-of-anrop, skala till en högre nivå.</span><span class="sxs-lookup"><span data-stu-id="c15da-145">If you run out of calls, scale to a higher tier.</span></span>|
    | <span data-ttu-id="c15da-146">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="c15da-146">**Resource group**</span></span> | <span data-ttu-id="c15da-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c15da-147">myResourceGroup</span></span> | <span data-ttu-id="c15da-148">Använd samma resursgrupp för alla tjänster i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c15da-148">Use the same resource group for all services in this tutorial.</span></span>|

4. <span data-ttu-id="c15da-149">Klicka på **skapa** att skapa kontot.</span><span class="sxs-lookup"><span data-stu-id="c15da-149">Click **Create** to create your account.</span></span> <span data-ttu-id="c15da-150">Klicka på din nya kognitiva tjänster konto fäst på instrumentpanelen när kontot skapas.</span><span class="sxs-lookup"><span data-stu-id="c15da-150">After the account is created, click your new Cognitive Services account pinned to the dashboard.</span></span> 

5. <span data-ttu-id="c15da-151">I kontot, klickar du på **nycklar**, och sedan kopiera värdet för **nyckel 1** och spara den.</span><span class="sxs-lookup"><span data-stu-id="c15da-151">In the account, click **Keys**, and then copy the value of **Key 1** and save it.</span></span> <span data-ttu-id="c15da-152">Du kan använda denna nyckel för att ansluta logikappen till kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-152">You use this key to connect the logic app to your Cognitive Services account.</span></span> 
 
    ![Nycklar](media/functions-twitter-email/keys.png)

## <a name="create-the-function"></a><span data-ttu-id="c15da-154">Skapa funktionen</span><span class="sxs-lookup"><span data-stu-id="c15da-154">Create the function</span></span>

<span data-ttu-id="c15da-155">Functions erbjuder ett bra sätt att avlasta bearbetningen aktiviteter i ett arbetsflöde för logic apps.</span><span class="sxs-lookup"><span data-stu-id="c15da-155">Functions provides a great way to offload processing tasks in a logic apps workflow.</span></span> <span data-ttu-id="c15da-156">Den här kursen används en HTTP-utlöses funktion att bearbeta tweet sentiment resultat från kognitiva tjänster och returnera ett kategorivärde.</span><span class="sxs-lookup"><span data-stu-id="c15da-156">This tutorial uses an HTTP triggered function to process tweet sentiment scores from Cognitive Services and return a category value.</span></span>  

1. <span data-ttu-id="c15da-157">Expandera din funktionsapp, klicka på den  **+**  knappen bredvid **funktioner**, klicka på den **HTTPTrigger** mall.</span><span class="sxs-lookup"><span data-stu-id="c15da-157">Expand your function app, click the **+** button next to **Functions**, click the **HTTPTrigger** template.</span></span> <span data-ttu-id="c15da-158">Typen `CategorizeSentiment` för funktionen **namn** och på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c15da-158">Type `CategorizeSentiment` for the function **Name** and click **Create**.</span></span>

    ![Funktionen appar bladet funktioner +](media/functions-twitter-email/add_fun.png)

2. <span data-ttu-id="c15da-160">Ersätt innehållet i filen run.csx med följande kod och klicka sedan på **spara**:</span><span class="sxs-lookup"><span data-stu-id="c15da-160">Replace the contents of the run.csx file with the following code, then click **Save**:</span></span>

    ```c#
    using System.Net;
    
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // The sentiment category defaults to 'GREEN'. 
        string category = "GREEN";
    
        // Get the sentiment score from the request body.
        double score = await req.Content.ReadAsAsync<double>();
        log.Info(string.Format("The sentiment score received is '{0}'.",
                    score.ToString()));
    
        // Set the category based on the sentiment score.
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
    <span data-ttu-id="c15da-161">Den här Funktionskoden returnerar en färgkategori baserat på sentiment poängsättningen togs emot i begäran.</span><span class="sxs-lookup"><span data-stu-id="c15da-161">This function code returns a color category based on the sentiment score received in the request.</span></span> 

3. <span data-ttu-id="c15da-162">Testa funktionen genom att klicka på **testa** längst till höger att expandera fliken Test. Ange ett värde för `0.2` för den **Begärandetext**, och klicka sedan på **kör**.</span><span class="sxs-lookup"><span data-stu-id="c15da-162">To test the function, click **Test** at the far right to expand the Test tab. Type a value of `0.2` for the **Request body**, and then click **Run**.</span></span> <span data-ttu-id="c15da-163">Ett värde av **RED** returneras i brödtexten i svaret.</span><span class="sxs-lookup"><span data-stu-id="c15da-163">A value of **RED** is returned in the body of the response.</span></span> 

    ![Testa funktionen i Azure-portalen](./media/functions-twitter-email/test.png)

<span data-ttu-id="c15da-165">Nu har du en funktion som kategoriserar sentiment resultat.</span><span class="sxs-lookup"><span data-stu-id="c15da-165">Now you have a function that categorizes sentiment scores.</span></span> <span data-ttu-id="c15da-166">Därefter kan du skapa en logikapp som integrerar din funktion med Twitter och kognitiva Services-konton.</span><span class="sxs-lookup"><span data-stu-id="c15da-166">Next, you create a logic app that integrates your function with your Twitter and Cognitive Services accounts.</span></span> 

## <a name="create-a-logic-app"></a><span data-ttu-id="c15da-167">Skapa en logikapp</span><span class="sxs-lookup"><span data-stu-id="c15da-167">Create a logic app</span></span>   

1. <span data-ttu-id="c15da-168">I Azure-portalen klickar du på den **ny** knapp hittades i det övre vänstra hörnet i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c15da-168">In the Azure portal, click the **New** button found on the upper left-hand corner of the Azure portal.</span></span>

2. <span data-ttu-id="c15da-169">Klicka på **företagsintegration** > **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="c15da-169">Click **Enterprise Integration** > **Logic App**.</span></span> <span data-ttu-id="c15da-170">Använd sedan inställningarna som anges i tabellen, kontrollera **fäst på instrumentpanelen**, och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c15da-170">Then, use the settings as specified in the table, check **Pin to dashboard**, and click **Create**.</span></span>
 
4. <span data-ttu-id="c15da-171">Skriv en **namn** som `TweetSentiment`, använda inställningar som anges i tabellen, accepterar villkoren och kontrollera **fäst på instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="c15da-171">Then, type a **Name** like `TweetSentiment`,  use the settings as specified in the table, accept the terms, and check **Pin to dashboard**.</span></span>

    ![Skapa logikappen i Azure-portalen](./media/functions-twitter-email/new_logicApp.png)

    | <span data-ttu-id="c15da-173">Inställning</span><span class="sxs-lookup"><span data-stu-id="c15da-173">Setting</span></span>      |  <span data-ttu-id="c15da-174">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="c15da-174">Suggested value</span></span>   | <span data-ttu-id="c15da-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c15da-175">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="c15da-176">**Namn**</span><span class="sxs-lookup"><span data-stu-id="c15da-176">**Name**</span></span> | <span data-ttu-id="c15da-177">TweetSentiment</span><span class="sxs-lookup"><span data-stu-id="c15da-177">TweetSentiment</span></span> | <span data-ttu-id="c15da-178">Välj ett lämpligt namn för din app.</span><span class="sxs-lookup"><span data-stu-id="c15da-178">Choose an appropriate name for your app.</span></span> |
    | <span data-ttu-id="c15da-179">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="c15da-179">**Resource group**</span></span> | <span data-ttu-id="c15da-180">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c15da-180">myResourceGroup</span></span> | <span data-ttu-id="c15da-181">API som används för att analysera text.</span><span class="sxs-lookup"><span data-stu-id="c15da-181">API used to analyze text.</span></span>  |
    | <span data-ttu-id="c15da-182">**Plats**</span><span class="sxs-lookup"><span data-stu-id="c15da-182">**Location**</span></span> | <span data-ttu-id="c15da-183">Östra USA</span><span class="sxs-lookup"><span data-stu-id="c15da-183">East US</span></span> | <span data-ttu-id="c15da-184">Välj en plats nära dig.</span><span class="sxs-lookup"><span data-stu-id="c15da-184">Choose a location close to you.</span></span> |
    | <span data-ttu-id="c15da-185">**Resursgrupp**</span><span class="sxs-lookup"><span data-stu-id="c15da-185">**Resource group**</span></span> | <span data-ttu-id="c15da-186">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c15da-186">myResourceGroup</span></span> | <span data-ttu-id="c15da-187">Välj samma befintlig resursgrupp som innan.</span><span class="sxs-lookup"><span data-stu-id="c15da-187">Choose the same existing resource group as before.</span></span>|

4. <span data-ttu-id="c15da-188">Klicka på **skapa** att skapa din logikapp.</span><span class="sxs-lookup"><span data-stu-id="c15da-188">Click **Create** to create your logic app.</span></span> <span data-ttu-id="c15da-189">När appen har skapats klickar du på den nya logikappen fäst på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="c15da-189">After the app is created, click your new logic app pinned to the dashboard.</span></span> <span data-ttu-id="c15da-190">Sedan i Logic Apps Designer rulla nedåt och klicka på den **tom Logikapp** mall.</span><span class="sxs-lookup"><span data-stu-id="c15da-190">Then in the Logic Apps Designer, scroll down and click the **Blank Logic App** template.</span></span> 

    ![Tom mall för Logic Apps](media/functions-twitter-email/blank.png)

<span data-ttu-id="c15da-192">Du kan nu använda Logic Apps Designer för att lägga till tjänster och utlösare i din app.</span><span class="sxs-lookup"><span data-stu-id="c15da-192">You can now use the Logic Apps Designer to add services and triggers to your app.</span></span>

## <a name="connect-to-twitter"></a><span data-ttu-id="c15da-193">Ansluta till Twitter</span><span class="sxs-lookup"><span data-stu-id="c15da-193">Connect to Twitter</span></span>

<span data-ttu-id="c15da-194">Först skapa en anslutning till Twitter-konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-194">First, create a connection to your Twitter account.</span></span> <span data-ttu-id="c15da-195">Logikappen avsöker för tweets som utlöser app att köras.</span><span class="sxs-lookup"><span data-stu-id="c15da-195">The logic app polls for tweets, which trigger the app to run.</span></span>

1. <span data-ttu-id="c15da-196">I designer klickar du på den **Twitter** tjänst och klicka på den **när en ny tweet skickas** utlösare.</span><span class="sxs-lookup"><span data-stu-id="c15da-196">In the designer, click the **Twitter** service, and click the **When a new tweet is posted** trigger.</span></span> <span data-ttu-id="c15da-197">Logga in med Twitter-konto och auktorisera Logic Apps att använda ditt konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-197">Sign in to your Twitter account and authorize Logic Apps to use your account.</span></span>

2. <span data-ttu-id="c15da-198">Använda Twitter inställningarna som anges i tabellen.</span><span class="sxs-lookup"><span data-stu-id="c15da-198">Use the Twitter trigger settings as specified in the table.</span></span> 

    ![Twitter connector-inställningar](media/functions-twitter-email/azure_tweet.png)

    | <span data-ttu-id="c15da-200">Inställning</span><span class="sxs-lookup"><span data-stu-id="c15da-200">Setting</span></span>      |  <span data-ttu-id="c15da-201">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="c15da-201">Suggested value</span></span>   | <span data-ttu-id="c15da-202">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c15da-202">Description</span></span>                                        |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="c15da-203">**Söktext**</span><span class="sxs-lookup"><span data-stu-id="c15da-203">**Search text**</span></span> | <span data-ttu-id="c15da-204">#Azure</span><span class="sxs-lookup"><span data-stu-id="c15da-204">#Azure</span></span> | <span data-ttu-id="c15da-205">Använd en hashtaggar är populära för att generera nya tweets i det valda intervallet.</span><span class="sxs-lookup"><span data-stu-id="c15da-205">Use a hashtag that is popular enough for to generate new tweets in the chosen interval.</span></span> <span data-ttu-id="c15da-206">När det är för populära med hjälp av den kostnadsfria nivån och din hashtaggar, kan du snabbt använda in transaktioner i kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-206">When using the Free tier and your hashtag is too popular, you can quickly use up the transactions in your Cognitive Services account.</span></span> |
    | <span data-ttu-id="c15da-207">**Frekvens**</span><span class="sxs-lookup"><span data-stu-id="c15da-207">**Frequency**</span></span> | <span data-ttu-id="c15da-208">Minut</span><span class="sxs-lookup"><span data-stu-id="c15da-208">Minute</span></span> | <span data-ttu-id="c15da-209">Frekvens enhet som används för avsökning av Twitter.</span><span class="sxs-lookup"><span data-stu-id="c15da-209">The frequency unit used for polling Twitter.</span></span>  |
    | <span data-ttu-id="c15da-210">**Intervall**</span><span class="sxs-lookup"><span data-stu-id="c15da-210">**Interval**</span></span> | <span data-ttu-id="c15da-211">15</span><span class="sxs-lookup"><span data-stu-id="c15da-211">15</span></span> | <span data-ttu-id="c15da-212">Tid mellan Twitter begäranden i frekvens enheter.</span><span class="sxs-lookup"><span data-stu-id="c15da-212">The time elapsed between Twitter requests, in frequency units.</span></span> |

3.  <span data-ttu-id="c15da-213">Klicka på **spara** att ansluta till ditt Twitter-konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-213">Click **Save** to connect to your Twitter account.</span></span> 

<span data-ttu-id="c15da-214">Appen är nu ansluten till Twitter.</span><span class="sxs-lookup"><span data-stu-id="c15da-214">Now your app is connected to Twitter.</span></span> <span data-ttu-id="c15da-215">Därefter ansluta till text analytics identifiera sentiment av insamlade tweets.</span><span class="sxs-lookup"><span data-stu-id="c15da-215">Next, you connect to text analytics to detect the sentiment of collected tweets.</span></span>

## <a name="add-sentiment-detection"></a><span data-ttu-id="c15da-216">Lägg till sentiment identifiering</span><span class="sxs-lookup"><span data-stu-id="c15da-216">Add sentiment detection</span></span>

1. <span data-ttu-id="c15da-217">Klicka på **nytt steg**, och sedan **lägga till en åtgärd**.</span><span class="sxs-lookup"><span data-stu-id="c15da-217">Click **New Step**, and then **Add an action**.</span></span>

    ![Nytt steg och Lägg till en åtgärd](media/functions-twitter-email/new_step.png)

2. <span data-ttu-id="c15da-219">I **välja en åtgärd**, klickar du på **textanalys**, och klicka sedan på den **identifiera sentiment** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c15da-219">In **Choose an action**, click **Text Analytics**, and then click the **Detect sentiment** action.</span></span>

    ![Identifiera Sentiment](media/functions-twitter-email/detect_sent.png)

3. <span data-ttu-id="c15da-221">Ange ett anslutningsnamn `MyCognitiveServicesConnection`, klistra in nyckeln för dina kognitiva tjänster konto som du sparade och klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="c15da-221">Type a connection name such as `MyCognitiveServicesConnection`, paste the key for your Cognitive Services account that you saved, and click **Create**.</span></span>  

4. <span data-ttu-id="c15da-222">Klicka på **Text för att analysera** > **Twittra text**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c15da-222">Click **Text to analyze** > **Tweet text**, and then click **Save**.</span></span>  

    ![Identifiera Sentiment](media/functions-twitter-email/ds_tta.png)

<span data-ttu-id="c15da-224">Du kan lägga till en anslutning till din funktion som förbrukar sentiment poäng utdata sentiment identifiering är konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="c15da-224">Now that sentiment detection is configured, you can add a connection to your function that consumes the sentiment score output.</span></span>

## <a name="connect-sentiment-output-to-your-function"></a><span data-ttu-id="c15da-225">Ansluta sentiment utdata till din funktion</span><span class="sxs-lookup"><span data-stu-id="c15da-225">Connect sentiment output to your function</span></span>

1. <span data-ttu-id="c15da-226">I Logic Apps Designer klickar du på **nytt steg** > **lägga till en åtgärd**, och klicka sedan på **Azure Functions**.</span><span class="sxs-lookup"><span data-stu-id="c15da-226">In the Logic Apps Designer, click **New step** > **Add an action**, and then click **Azure Functions**.</span></span> 

2. <span data-ttu-id="c15da-227">Klicka på **välja en Azure-funktion**, Välj den **CategorizeSentiment** funktionen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c15da-227">Click **Choose an Azure function**, select the **CategorizeSentiment** function you created earlier.</span></span>  

    ![Azure funktionsrutan visar Välj en Azure-funktion](media/functions-twitter-email/choose_fun.png)

3. <span data-ttu-id="c15da-229">I **brödtext i begäran**, klickar du på **poäng** och sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="c15da-229">In **Request Body**, click **Score** and then **Save**.</span></span>

    ![Poäng](media/functions-twitter-email/trigger_score.png)

<span data-ttu-id="c15da-231">Nu kan utlöses din funktion när en sentiment poäng skickas från logikappen.</span><span class="sxs-lookup"><span data-stu-id="c15da-231">Now, your function is triggered when a sentiment score is sent from the logic app.</span></span> <span data-ttu-id="c15da-232">En färgkodade kategori returneras till logikappen av funktionen.</span><span class="sxs-lookup"><span data-stu-id="c15da-232">A color-coded category is returned to the logic app by the function.</span></span> <span data-ttu-id="c15da-233">Sedan lägger du till ett e-postmeddelande som skickas när sentiment värdet **RED** returneras från funktionen.</span><span class="sxs-lookup"><span data-stu-id="c15da-233">Next, you add an email notification that is sent when a sentiment value of **RED** is returned from the function.</span></span> 

## <a name="add-email-notifications"></a><span data-ttu-id="c15da-234">Lägg till e-postaviseringar</span><span class="sxs-lookup"><span data-stu-id="c15da-234">Add email notifications</span></span>

<span data-ttu-id="c15da-235">Den sista delen av arbetsflödet är att utlösa ett e-postmeddelande när sentiment beräknas som _RED_.</span><span class="sxs-lookup"><span data-stu-id="c15da-235">The last part of the workflow is to trigger an email when the sentiment is scored as _RED_.</span></span> <span data-ttu-id="c15da-236">Det här avsnittet använder en Outlook.com-koppling.</span><span class="sxs-lookup"><span data-stu-id="c15da-236">This topic uses an Outlook.com connector.</span></span> <span data-ttu-id="c15da-237">Du kan utföra liknande steg för att använda en Gmail eller Office 365 Outlook-koppling.</span><span class="sxs-lookup"><span data-stu-id="c15da-237">You can perform similar steps to use a Gmail or Office 365 Outlook connector.</span></span>   

1. <span data-ttu-id="c15da-238">I Logic Apps Designer klickar du på **nytt steg** > **Lägg till ett villkor**.</span><span class="sxs-lookup"><span data-stu-id="c15da-238">In the Logic Apps Designer, click **New step** > **Add a condition**.</span></span> 

2. <span data-ttu-id="c15da-239">Klicka på **väljer ett värde**, klicka på **brödtext**.</span><span class="sxs-lookup"><span data-stu-id="c15da-239">Click **Choose a value**, then click **Body**.</span></span> <span data-ttu-id="c15da-240">Välj **är lika med**, klickar du på **väljer ett värde** och skriv `RED`, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c15da-240">Select **is equal to**, click **Choose a value** and type `RED`, and click **Save**.</span></span> 

    ![Lägga till ett villkor i logikappen.](media/functions-twitter-email/condition.png)

3. <span data-ttu-id="c15da-242">I **om Ja, gör INGENTING**, klickar du på **lägga till en åtgärd**, söka efter `outlook.com`, klickar du på **skickar ett e-**, och logga in på Outlook.com-konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-242">In **IF YES, DO NOTHING**, click **Add an action**, search for `outlook.com`, click **Send an email**, and sign in to your Outlook.com account.</span></span>
    
    ![Välj en åtgärd för villkoret.](media/functions-twitter-email/outlook.png)

    > [!NOTE]
    > <span data-ttu-id="c15da-244">Om du inte har en Outlook.com-konto, kan du välja en annan koppling, t.ex Gmail eller Office 365 Outlook</span><span class="sxs-lookup"><span data-stu-id="c15da-244">If you don't have an Outlook.com account, you can choose another connector, such as Gmail or Office 365 Outlook</span></span>

4. <span data-ttu-id="c15da-245">I den **skickar ett e-** åtgärd, Använd e-postinställningar som angetts i tabellen.</span><span class="sxs-lookup"><span data-stu-id="c15da-245">In the **Send an email** action, use the email settings as specified in the table.</span></span> 

    ![Konfigurera e-postmeddelandet skickas en e-åtgärd.](media/functions-twitter-email/sendemail.png)

    | <span data-ttu-id="c15da-247">Inställning</span><span class="sxs-lookup"><span data-stu-id="c15da-247">Setting</span></span>      |  <span data-ttu-id="c15da-248">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="c15da-248">Suggested value</span></span>   | <span data-ttu-id="c15da-249">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c15da-249">Description</span></span>  |
    | ----------------- | ------------ | ------------- |
    | <span data-ttu-id="c15da-250">**Att**</span><span class="sxs-lookup"><span data-stu-id="c15da-250">**To**</span></span> | <span data-ttu-id="c15da-251">Skriv din e-postadress</span><span class="sxs-lookup"><span data-stu-id="c15da-251">Type your email address</span></span> | <span data-ttu-id="c15da-252">E-postadressen som tar emot ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="c15da-252">The email address that receives the notification.</span></span> |
    | <span data-ttu-id="c15da-253">**Ämne**</span><span class="sxs-lookup"><span data-stu-id="c15da-253">**Subject**</span></span> | <span data-ttu-id="c15da-254">Negativa tweets sentiment upptäcktes</span><span class="sxs-lookup"><span data-stu-id="c15da-254">Negative tweet sentiment detected</span></span>  | <span data-ttu-id="c15da-255">Ämnesraden för e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="c15da-255">The subject line of the email notification.</span></span>  |
    | <span data-ttu-id="c15da-256">**Brödtext**</span><span class="sxs-lookup"><span data-stu-id="c15da-256">**Body**</span></span> | <span data-ttu-id="c15da-257">Tweet text, plats</span><span class="sxs-lookup"><span data-stu-id="c15da-257">Tweet text, Location</span></span> | <span data-ttu-id="c15da-258">Klicka på den **Twittra text** och **plats** parametrar.</span><span class="sxs-lookup"><span data-stu-id="c15da-258">Click the **Tweet text** and **Location** parameters.</span></span> |

5.  <span data-ttu-id="c15da-259">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c15da-259">Click **Save**.</span></span>

<span data-ttu-id="c15da-260">Arbetsflödet har slutförts kan du aktivera logikappen och finns funktionen på arbetet.</span><span class="sxs-lookup"><span data-stu-id="c15da-260">Now that the workflow is complete, you can enable the logic app and see the function at work.</span></span>

## <a name="test-the-workflow"></a><span data-ttu-id="c15da-261">Testa arbetsflödet</span><span class="sxs-lookup"><span data-stu-id="c15da-261">Test the workflow</span></span>

1. <span data-ttu-id="c15da-262">I logik App Designer klickar du på **kör** starta appen.</span><span class="sxs-lookup"><span data-stu-id="c15da-262">In the Logic App Designer, click **Run** to start the app.</span></span>

2. <span data-ttu-id="c15da-263">Klicka i den vänstra kolumnen **översikt** att se status för logikappen.</span><span class="sxs-lookup"><span data-stu-id="c15da-263">In the left column, click **Overview** to see the status of the logic app.</span></span> 
 
    ![Logik app Körstatus](media/functions-twitter-email/over1.png)

3. <span data-ttu-id="c15da-265">(Valfritt) Klicka på Kör visas detaljerad information om körningen.</span><span class="sxs-lookup"><span data-stu-id="c15da-265">(Optional) Click one of the runs to see details of the execution.</span></span>

4. <span data-ttu-id="c15da-266">Gå till din funktion, visa loggfilerna och kontrollera att sentiment värden tagits emot och bearbetas.</span><span class="sxs-lookup"><span data-stu-id="c15da-266">Go to your function, view the logs, and verify that sentiment values were received and processed.</span></span>
 
    ![Visa funktionsloggar](media/functions-twitter-email/sent.png)

5. <span data-ttu-id="c15da-268">Du får ett e-postmeddelande när potentiellt negativa sentiment har identifierats.</span><span class="sxs-lookup"><span data-stu-id="c15da-268">When a potentially negative sentiment is detected, you receive an email.</span></span> <span data-ttu-id="c15da-269">Om du inte har fått ett e-postmeddelande, kan du ändra Funktionskoden för att returnera RED varje gång:</span><span class="sxs-lookup"><span data-stu-id="c15da-269">If you haven't received an email, you can change the function code to return RED every time:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, "RED");

    <span data-ttu-id="c15da-270">När du har kontrollerat e-postmeddelanden, ändra tillbaka till den ursprungliga koden:</span><span class="sxs-lookup"><span data-stu-id="c15da-270">After you have verified email notifications, change back to the original code:</span></span>

        return req.CreateResponse(HttpStatusCode.OK, category);

    > [!IMPORTANT]
    > <span data-ttu-id="c15da-271">När du har slutfört den här kursen ska du inaktivera logikappen.</span><span class="sxs-lookup"><span data-stu-id="c15da-271">After you have completed this tutorial, you should disable the logic app.</span></span> <span data-ttu-id="c15da-272">Genom att inaktivera appen undviker du att debiteras för körningar och använder upp transaktioner i kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-272">By disabling the app, you avoid being charged for executions and using up the transactions in your Cognitive Services account.</span></span>

<span data-ttu-id="c15da-273">Nu har du sett hur enkelt det är att integrera funktioner i ett arbetsflöde för Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="c15da-273">Now you have seen how easy it is to integrate Functions into a Logic Apps workflow.</span></span>

## <a name="disable-the-logic-app"></a><span data-ttu-id="c15da-274">Inaktivera logikappen</span><span class="sxs-lookup"><span data-stu-id="c15da-274">Disable the logic app</span></span>

<span data-ttu-id="c15da-275">Om du vill inaktivera logikappen **översikt** och klicka sedan på **inaktivera** överst på skärmen.</span><span class="sxs-lookup"><span data-stu-id="c15da-275">To disable the logic app, click **Overview** and then click **Disable** at the top of the screen.</span></span> <span data-ttu-id="c15da-276">Detta stoppar logikappen köra och medför kostnader utan att ta bort appen.</span><span class="sxs-lookup"><span data-stu-id="c15da-276">This stops the logic app from running and incurring charges without deleting the app.</span></span> 

![Funktionsloggar](media/functions-twitter-email/disable-logic-app.png)

## <a name="next-steps"></a><span data-ttu-id="c15da-278">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c15da-278">Next steps</span></span>

<span data-ttu-id="c15da-279">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="c15da-279">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c15da-280">Skapa ett kognitiva Services-konto.</span><span class="sxs-lookup"><span data-stu-id="c15da-280">Create a Cognitive Services account.</span></span>
> * <span data-ttu-id="c15da-281">Skapa en funktion som kategoriserar tweet sentiment.</span><span class="sxs-lookup"><span data-stu-id="c15da-281">Create a function that categorizes tweet sentiment.</span></span>
> * <span data-ttu-id="c15da-282">Skapa en logikapp som ansluter till Twitter.</span><span class="sxs-lookup"><span data-stu-id="c15da-282">Create a logic app that connects to Twitter.</span></span>
> * <span data-ttu-id="c15da-283">Lägg till sentiment identifiering logikappen.</span><span class="sxs-lookup"><span data-stu-id="c15da-283">Add sentiment detection to the logic app.</span></span> 
> * <span data-ttu-id="c15da-284">Ansluta logikappen till funktionen.</span><span class="sxs-lookup"><span data-stu-id="c15da-284">Connect the logic app to the function.</span></span>
> * <span data-ttu-id="c15da-285">Skicka ett e-post baserat på svar från funktionen.</span><span class="sxs-lookup"><span data-stu-id="c15da-285">Send an email based on the response from the function.</span></span>

<span data-ttu-id="c15da-286">Gå vidare till nästa kurs att lära dig hur du skapar ett serverlösa API för din funktion.</span><span class="sxs-lookup"><span data-stu-id="c15da-286">Advance to the next tutorial to learn how to create a serverless API for your function.</span></span>

> [!div class="nextstepaction"] 
> [<span data-ttu-id="c15da-287">Skapa ett API utan server med Azure Functions</span><span class="sxs-lookup"><span data-stu-id="c15da-287">Create a serverless API using Azure Functions</span></span>](functions-create-serverless-api.md)

<span data-ttu-id="c15da-288">Läs mer om Logic Apps i [Azure Logikappar](../logic-apps/logic-apps-what-are-logic-apps.md).</span><span class="sxs-lookup"><span data-stu-id="c15da-288">To learn more about Logic Apps, see [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md).</span></span>

