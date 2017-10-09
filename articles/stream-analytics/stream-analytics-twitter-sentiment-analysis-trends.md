---
title: aaaReal tid Twitter sentiment analys med Azure Stream Analytics | Microsoft Docs
description: "Lär dig hur toouse Stream Analytics för analys av realtidsskyddet Twitter-sentiment. Stegvisa anvisningar från händelsen generation toodata på en levande instrumentpanel."
keywords: trendanalys i realtid twitter, sentiment analys, sociala media analys, trend analys exempel
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42068691-074b-4c3b-a527-acafa484fda2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: jeffstok
ms.openlocfilehash: 157790caa7ea6f5570dd9c9d3bd9694d437eb4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a><span data-ttu-id="9e35a-105">Realtid Twitter-sentiment analys i Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9e35a-105">Real-time Twitter sentiment analysis in Azure Stream Analytics</span></span>

<span data-ttu-id="9e35a-106">Lär dig hur toobuild en sentiment analys lösning för social media analytics genom att ta realtid Twitter händelser i Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="9e35a-106">Learn how toobuild a sentiment analysis solution for social media analytics by bringing real-time Twitter events into Azure Event Hubs.</span></span> <span data-ttu-id="9e35a-107">Du kan sedan skriva en Azure Stream Analytics query tooanalyze hello data och antingen store hello resultat för senare användning eller använda en instrumentpanel och [Power BI](https://powerbi.com/) tooprovide insikter i realtid.</span><span class="sxs-lookup"><span data-stu-id="9e35a-107">You can then write an Azure Stream Analytics query tooanalyze hello data and either store hello results for later use or use a dashboard and [Power BI](https://powerbi.com/) tooprovide insights in real time.</span></span>

<span data-ttu-id="9e35a-108">Verktyg för sociala media analytics hjälper företag att förstå trender avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9e35a-108">Social media analytics tools help organizations understand trending topics.</span></span> <span data-ttu-id="9e35a-109">Trender avsnitt är ämnen och attityder som har en stor volym med inlägg i sociala medier.</span><span class="sxs-lookup"><span data-stu-id="9e35a-109">Trending topics are subjects and attitudes that have a high volume of posts in social media.</span></span> <span data-ttu-id="9e35a-110">Sentiment analys, som också kallas *åsikt utvinningsmodellen*, använder sociala media analytics verktyg toodetermine attityder mot en produkt, idé och så vidare.</span><span class="sxs-lookup"><span data-stu-id="9e35a-110">Sentiment analysis, which is also called *opinion mining*, uses social media analytics tools toodetermine attitudes toward a product, idea, and so on.</span></span> 

<span data-ttu-id="9e35a-111">Trendanalys för realtid Twitter är ett bra exempel på ett webbanalysverktyg för eftersom hello hashtaggar prenumeration kan du toolisten toospecific nyckelord (hash-taggar) och utveckla sentiment analys av hello feed.</span><span class="sxs-lookup"><span data-stu-id="9e35a-111">Real-time Twitter trend analysis is a great example of an analytics tool, because hello hashtag subscription model enables you toolisten toospecific keywords (hashtags) and develop sentiment analysis of hello feed.</span></span>

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a><span data-ttu-id="9e35a-112">Scenario: Sociala medier sentiment analys i realtid</span><span class="sxs-lookup"><span data-stu-id="9e35a-112">Scenario: Social media sentiment analysis in real time</span></span>

<span data-ttu-id="9e35a-113">Ett företag som har en webbplats för Nyheter media är intresserad av att få flera fördelar jämfört med dess konkurrenter med innehåll som är direkt tillämpliga tooits läsare.</span><span class="sxs-lookup"><span data-stu-id="9e35a-113">A company that has a news media website is interested in gaining an advantage over its competitors by featuring site content that is immediately relevant tooits readers.</span></span> <span data-ttu-id="9e35a-114">hello företaget använder analys av sociala medier på information som är relevanta tooreaders genom att göra realtid sentiment analys av Twitter-data.</span><span class="sxs-lookup"><span data-stu-id="9e35a-114">hello company uses social media analysis on topics that are relevant tooreaders by doing real-time sentiment analysis of Twitter data.</span></span>

<span data-ttu-id="9e35a-115">tooidentify trender avsnitt i realtid på Twitter, hello företagets behov analys i realtid om hello tweet volym och sentiment för viktiga ämnen.</span><span class="sxs-lookup"><span data-stu-id="9e35a-115">tooidentify trending topics in real time on Twitter, hello company needs real-time analytics about hello tweet volume and sentiment for key topics.</span></span> <span data-ttu-id="9e35a-116">Med andra ord är hello behöver en sentiment analys analytics-motor som är baserad på den här sociala medier feed.</span><span class="sxs-lookup"><span data-stu-id="9e35a-116">In other words, hello need is a sentiment analysis analytics engine that's based on this social media feed.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e35a-117">Krav</span><span class="sxs-lookup"><span data-stu-id="9e35a-117">Prerequisites</span></span>
<span data-ttu-id="9e35a-118">I den här kursen använder du ett klientprogram som ansluter tooTwitter och söker efter tweets som har vissa hash-taggar (som du kan ange).</span><span class="sxs-lookup"><span data-stu-id="9e35a-118">In this tutorial, you use a client application that connects tooTwitter and looks for tweets that have certain hashtags (which you can set).</span></span> <span data-ttu-id="9e35a-119">I ordning toorun hello program och analysera hello tweets med hjälp av Azure Streaming Analytics, måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="9e35a-119">In order toorun hello application and analyze hello tweets using Azure Streaming Analytics, you must have hello following:</span></span>

* <span data-ttu-id="9e35a-120">En Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9e35a-120">An Azure subscription</span></span>
* <span data-ttu-id="9e35a-121">Ett Twitter-konto</span><span class="sxs-lookup"><span data-stu-id="9e35a-121">A Twitter account</span></span> 
* <span data-ttu-id="9e35a-122">Ett Twitter-program och hello [OAuth-åtkomsttoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) för programmet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-122">A Twitter application, and hello [OAuth access token](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) for that application.</span></span> <span data-ttu-id="9e35a-123">Vi ger avancerade anvisningar för hur toocreate Twitter programmet senare.</span><span class="sxs-lookup"><span data-stu-id="9e35a-123">We provide high-level instructions for how toocreate a Twitter application later.</span></span>
* <span data-ttu-id="9e35a-124">Hej TwitterWPFClient program som läser hello Twitter-feed.</span><span class="sxs-lookup"><span data-stu-id="9e35a-124">hello TwitterWPFClient application, which reads hello Twitter feed.</span></span> <span data-ttu-id="9e35a-125">tooget detta program, hämta hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) från GitHub och sedan packa hello paketet till en mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="9e35a-125">tooget this application, download hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) file from GitHub and then unzip hello package into a folder on your computer.</span></span> <span data-ttu-id="9e35a-126">Om du vill toosee hello källkoden och köra programmet hello i en felsökare, du kan hämta hello källkod från [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator).</span><span class="sxs-lookup"><span data-stu-id="9e35a-126">If you want toosee hello source code and run hello application in a debugger, you can get hello source code from [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator).</span></span> 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a><span data-ttu-id="9e35a-127">Skapa en händelsehubb för Streaming Analytics indata</span><span class="sxs-lookup"><span data-stu-id="9e35a-127">Create an event hub for Streaming Analytics input</span></span>

<span data-ttu-id="9e35a-128">hello exempelprogrammet genererar händelser och skickar dem tooan Azure-händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="9e35a-128">hello sample application generates events and pushes them tooan Azure event hub.</span></span> <span data-ttu-id="9e35a-129">Händelsehubbar är hello önskad metod för händelsen införandet för Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="9e35a-129">Azure event hubs are hello preferred method of event ingestion for Stream Analytics.</span></span> <span data-ttu-id="9e35a-130">Mer information finns i hello [Händelsehubbar i Azure-dokumentationen](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="9e35a-130">For more information, see hello [Azure Event Hubs documentation](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>


### <a name="create-an-event-hub-namespace-and-event-hub"></a><span data-ttu-id="9e35a-131">Skapa en event hub namnområde och händelsehubb</span><span class="sxs-lookup"><span data-stu-id="9e35a-131">Create an event hub namespace and event hub</span></span>
<span data-ttu-id="9e35a-132">I den här proceduren du först skapa en event hub-namnområde och sedan lägger du till ett event hub toothat namnområde.</span><span class="sxs-lookup"><span data-stu-id="9e35a-132">In this procedure, you first create an event hub namespace, and then you add an event hub toothat namespace.</span></span> <span data-ttu-id="9e35a-133">Event hub namnområden används toologically gruppera relaterade händelser-bussen instanser.</span><span class="sxs-lookup"><span data-stu-id="9e35a-133">Event hub namespaces are used toologically group related event bus instances.</span></span> 

1. <span data-ttu-id="9e35a-134">Logga in toohello Azure-portalen och klicka på **ny** > **Sakernas Internet** > **Händelsehubb**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-134">Log  in toohello Azure portal and click **New** > **Internet of Things** > **Event Hub**.</span></span> 

2. <span data-ttu-id="9e35a-135">I hello **skapa namnområdet** bladet, ange ett namn för namnområdet som `<yourname>-socialtwitter-eh-ns`.</span><span class="sxs-lookup"><span data-stu-id="9e35a-135">In hello **Create namespace** blade, enter a namespace name such as `<yourname>-socialtwitter-eh-ns`.</span></span> <span data-ttu-id="9e35a-136">Du kan använda valfritt namn för hello namnområde, men hello-namnet måste vara giltigt för en URL och det måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="9e35a-136">You can use any name for hello namespace, but hello name must be valid for a URL and it must be unique across Azure.</span></span> 
    
3. <span data-ttu-id="9e35a-137">Välj en prenumeration och skapa eller välja en resursgrupp och sedan klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-137">Select a subscription and create or choose a resource group, then click **Create**.</span></span> 

    ![Skapa ett namnområde för event hub](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. <span data-ttu-id="9e35a-139">Hitta hello event hub namnområde i din lista över Azure-resurser när distribution hello namnområde har slutförts.</span><span class="sxs-lookup"><span data-stu-id="9e35a-139">When hello namespace has finished deploying, find hello event hub namespace in your list of Azure resources.</span></span> 

5. <span data-ttu-id="9e35a-140">Klicka på hello Nytt namnområde och hello namnområde bladet på  **+ &nbsp;Händelsehubb**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-140">Click hello new namespace, and in hello namespace blade, click **+&nbsp;Event Hub**.</span></span> 

    ![<span data-ttu-id="9e35a-141">hello lägga till Event Hub-knappen för att skapa en ny händelsehubb</span><span class="sxs-lookup"><span data-stu-id="9e35a-141">hello Add Event Hub button for creating a new event hub</span></span> ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. <span data-ttu-id="9e35a-142">Namnet hello ny händelsehubb `socialtwitter-eh`.</span><span class="sxs-lookup"><span data-stu-id="9e35a-142">Name hello new event hub `socialtwitter-eh`.</span></span> <span data-ttu-id="9e35a-143">Du kan använda ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="9e35a-143">You can use a different name.</span></span> <span data-ttu-id="9e35a-144">Om du vill anteckna, eftersom du måste ha namnet hello senare.</span><span class="sxs-lookup"><span data-stu-id="9e35a-144">If you do, make a note of it, because you need hello name later.</span></span> <span data-ttu-id="9e35a-145">Du behöver inte tooset andra alternativ för hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="9e35a-145">You don't need tooset any other options for hello event hub.</span></span>

    ![Bladet för att skapa en ny händelsehubb](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. <span data-ttu-id="9e35a-147">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-147">Click **Create**.</span></span>


### <a name="grant-access-toohello-event-hub"></a><span data-ttu-id="9e35a-148">Bevilja åtkomst toohello händelsehubb</span><span class="sxs-lookup"><span data-stu-id="9e35a-148">Grant access toohello event hub</span></span>

<span data-ttu-id="9e35a-149">Innan en process kan skicka data tooan händelsehubb, måste hello händelsehubb ha en princip som tillåter åtkomstbehörighet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-149">Before a process can send data tooan event hub, hello event hub must have a policy that allows appropriate access.</span></span> <span data-ttu-id="9e35a-150">hello åtkomstprincip producerar en anslutningssträng som innehåller auktoriseringsinformation om.</span><span class="sxs-lookup"><span data-stu-id="9e35a-150">hello access policy produces a connection string that includes authorization information.</span></span>

1.  <span data-ttu-id="9e35a-151">I hello händelse namnområde bladet, klickar du på **Händelsehubbar** och klicka sedan på hello namnet på din nya event hub.</span><span class="sxs-lookup"><span data-stu-id="9e35a-151">In hello event namespace blade, click **Event Hubs** and then click hello name of your new event hub.</span></span>

2.  <span data-ttu-id="9e35a-152">I hello event hub-blad klickar du på **principer för delad åtkomst** och klicka sedan på  **+ &nbsp;Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-152">In hello event hub blade, click **Shared access policies** and then click **+&nbsp;Add**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="9e35a-153">Kontrollera att du arbetar med hello händelsehubb, inte hello event hub namnområde.</span><span class="sxs-lookup"><span data-stu-id="9e35a-153">Make sure you're working with hello event hub, not hello event hub namespace.</span></span>

3.  <span data-ttu-id="9e35a-154">Lägg till en princip med namnet `socialtwitter-access` och **anspråk**väljer **hantera**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-154">Add a policy named `socialtwitter-access` and for **Claim**, select **Manage**.</span></span>

    ![Bladet för att skapa en ny händelse hubb princip](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  <span data-ttu-id="9e35a-156">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-156">Click **Create**.</span></span>

5.  <span data-ttu-id="9e35a-157">Efter hello principen har distribuerats, klickar du på hello lista över principer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9e35a-157">After hello policy has been deployed, click it in hello list of shared access policies.</span></span>

6.  <span data-ttu-id="9e35a-158">Hitta hello rutan **sträng-primära ANSLUTNINGSNYCKEL** och klicka på hello Kopiera knappen Nästa toohello-anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="9e35a-158">Find hello box labeled **CONNECTION STRING-PRIMARY KEY** and click hello copy button next toohello connection string.</span></span> 
    
    ![Kopiera hello primära sträng anslutningsnyckel från hello åtkomstprincipen](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  <span data-ttu-id="9e35a-160">Klistra in hello anslutningssträngen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="9e35a-160">Paste hello connection string into a text editor.</span></span> <span data-ttu-id="9e35a-161">Du behöver den här anslutningssträngen för hello nästa avsnitt, när du har gjort vissa små redigeringar tooit.</span><span class="sxs-lookup"><span data-stu-id="9e35a-161">You need this connection string for hello next section, after you make some small edits tooit.</span></span>

    <span data-ttu-id="9e35a-162">hello anslutningssträngen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="9e35a-162">hello connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    <span data-ttu-id="9e35a-163">Observera att hello-anslutningssträngen innehåller flera nyckel-värdepar, avgränsade med semikolon: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, och `EntityPath`.</span><span class="sxs-lookup"><span data-stu-id="9e35a-163">Notice that hello connection string contains multiple key-value pairs, separated with semicolons: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, and `EntityPath`.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="9e35a-164">Delar av hello anslutningssträngen i hello exempel för säkerhet, har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="9e35a-164">For security, parts of hello connection string in hello example have been removed.</span></span>

8.  <span data-ttu-id="9e35a-165">Ta bort hello i hello textredigerare `EntityPath` par från hello anslutningssträng (Glöm inte tooremove hello semikolon som föregår den).</span><span class="sxs-lookup"><span data-stu-id="9e35a-165">In hello text editor, remove hello `EntityPath` pair from hello connection string (don't forget tooremove hello semicolon that precedes it).</span></span> <span data-ttu-id="9e35a-166">När du är klar hello anslutningssträngen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="9e35a-166">When you're done, hello connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-hello-twitter-client-application"></a><span data-ttu-id="9e35a-167">Konfigurera och starta hello Twitter-klientprogrammet</span><span class="sxs-lookup"><span data-stu-id="9e35a-167">Configure and start hello Twitter client application</span></span>
<span data-ttu-id="9e35a-168">hello klientprogrammet hämtar tweet händelser direkt från Twitter.</span><span class="sxs-lookup"><span data-stu-id="9e35a-168">hello client application gets tweet events directly from Twitter.</span></span> <span data-ttu-id="9e35a-169">I order toodo så, måste behörighet toocall hello Twitter Streaming API: er.</span><span class="sxs-lookup"><span data-stu-id="9e35a-169">In order toodo so, it needs permission toocall hello Twitter Streaming APIs.</span></span> <span data-ttu-id="9e35a-170">tooconfigure att behörigheten kan du skapa ett program i Twitter, vilket genererar unika autentiseringsuppgifter (t.ex en OAuth-token).</span><span class="sxs-lookup"><span data-stu-id="9e35a-170">tooconfigure that permission, you create an application in Twitter, which generates unique credentials (such as an OAuth token).</span></span> <span data-ttu-id="9e35a-171">Du kan sedan konfigurera hello klienten programmet toouse dessa autentiseringsuppgifter när gör det API-anrop.</span><span class="sxs-lookup"><span data-stu-id="9e35a-171">You can then configure hello client application toouse these credentials when it makes API calls.</span></span> 

### <a name="create-a-twitter-application"></a><span data-ttu-id="9e35a-172">Skapa ett Twitter-program</span><span class="sxs-lookup"><span data-stu-id="9e35a-172">Create a Twitter application</span></span>
<span data-ttu-id="9e35a-173">Om du inte redan har ett Twitter-program som du kan använda för den här kursen kan skapa du en.</span><span class="sxs-lookup"><span data-stu-id="9e35a-173">If you do not already have a Twitter application that you can use for this tutorial, you can create one.</span></span> <span data-ttu-id="9e35a-174">Du måste redan ha ett Twitter-konto.</span><span class="sxs-lookup"><span data-stu-id="9e35a-174">You must already have a Twitter account.</span></span>

> [!NOTE]
> <span data-ttu-id="9e35a-175">hello exakt processen i Twitter för att skapa ett program och hämtar hello nycklar och hemligheter token kan ändras.</span><span class="sxs-lookup"><span data-stu-id="9e35a-175">hello exact process in Twitter for creating an application and getting hello keys, secrets, and token might change.</span></span> <span data-ttu-id="9e35a-176">Om dessa instruktioner inte matchar det som visas på hello Twitter plats läser du toohello Twitter utvecklardokumentation.</span><span class="sxs-lookup"><span data-stu-id="9e35a-176">If these instructions don't match what you see on hello Twitter site, refer toohello Twitter developer documentation.</span></span>

1. <span data-ttu-id="9e35a-177">Gå toohello [sidan Twitter programhantering](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="9e35a-177">Go toohello [Twitter application management page](https://apps.twitter.com/).</span></span> 

2. <span data-ttu-id="9e35a-178">Skapa ett nytt program.</span><span class="sxs-lookup"><span data-stu-id="9e35a-178">Create a new application.</span></span> 

    * <span data-ttu-id="9e35a-179">Ange en giltig URL för hello Webbadress.</span><span class="sxs-lookup"><span data-stu-id="9e35a-179">For hello website URL, specify a valid URL.</span></span> <span data-ttu-id="9e35a-180">Det har inte toobe en live-webbplats.</span><span class="sxs-lookup"><span data-stu-id="9e35a-180">It does not have toobe a live site.</span></span> <span data-ttu-id="9e35a-181">(Du kan inte ange bara `localhost`.)</span><span class="sxs-lookup"><span data-stu-id="9e35a-181">(You can't specify just `localhost`.)</span></span>
    * <span data-ttu-id="9e35a-182">Lämna hello återanrop fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="9e35a-182">Leave hello callback field blank.</span></span> <span data-ttu-id="9e35a-183">hello-klientprogram som du använder för den här kursen kräver inte återanrop.</span><span class="sxs-lookup"><span data-stu-id="9e35a-183">hello client application you use for this tutorial doesn't require callbacks.</span></span>

    ![Skapa ett program i Twitter](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. <span data-ttu-id="9e35a-185">Du kan ändra behörigheter för hello program bara tooread.</span><span class="sxs-lookup"><span data-stu-id="9e35a-185">Optionally, change hello application's permissions tooread-only.</span></span>

4. <span data-ttu-id="9e35a-186">När programmet hello skapas gå toohello **nycklar och åtkomst-token** sidan.</span><span class="sxs-lookup"><span data-stu-id="9e35a-186">When hello application is created, go toohello **Keys and Access Tokens** page.</span></span>

5. <span data-ttu-id="9e35a-187">Klicka på hello knappen toogenerate en åtkomst-token och åtkomst-token hemlighet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-187">Click hello button toogenerate an access token and access token secret.</span></span>

<span data-ttu-id="9e35a-188">Behåll den här informationen praktisk, eftersom du behöver i hello nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="9e35a-188">Keep this information handy, because you will need it in hello next procedure.</span></span>

>[!NOTE]
><span data-ttu-id="9e35a-189">Ange åtkomstkonto tooyour Twitter hello nycklar och hemligheter för hello Twitter-programmet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-189">hello keys and secrets for hello Twitter application provide access tooyour Twitter account.</span></span> <span data-ttu-id="9e35a-190">Behandla den här informationen som känslig, hello samma som du har lösenordet Twitter.</span><span class="sxs-lookup"><span data-stu-id="9e35a-190">Treat this information as sensitive, hello same as you do your Twitter password.</span></span> <span data-ttu-id="9e35a-191">Till exempel inte att bädda in den här informationen i ett program som du ger tooothers.</span><span class="sxs-lookup"><span data-stu-id="9e35a-191">For example, don't embed this information in an application that you give tooothers.</span></span> 


### <a name="configure-hello-client-application"></a><span data-ttu-id="9e35a-192">Konfigurera hello klientprogrammet</span><span class="sxs-lookup"><span data-stu-id="9e35a-192">Configure hello client application</span></span>
<span data-ttu-id="9e35a-193">Vi har skapat ett klientprogram som ansluter tooTwitter data med hjälp av [Twitter's Streaming API: er](https://dev.twitter.com/streaming/overview) toocollect tweet händelser för en specifik uppsättning avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9e35a-193">We've created a client application that connects tooTwitter data using [Twitter's Streaming APIs](https://dev.twitter.com/streaming/overview) toocollect tweet events about a specific set of topics.</span></span> <span data-ttu-id="9e35a-194">hello programmet använder hello [Sentiment140](http://help.sentiment140.com/) öppen källkod verktyg som tilldelar hello följande sentiment värdet tooeach tweet:</span><span class="sxs-lookup"><span data-stu-id="9e35a-194">hello application uses hello [Sentiment140](http://help.sentiment140.com/) open source tool, which assigns hello following sentiment value tooeach tweet:</span></span>

* <span data-ttu-id="9e35a-195">0 = negativt</span><span class="sxs-lookup"><span data-stu-id="9e35a-195">0 = negative</span></span>
* <span data-ttu-id="9e35a-196">2 = neutral</span><span class="sxs-lookup"><span data-stu-id="9e35a-196">2 = neutral</span></span>
* <span data-ttu-id="9e35a-197">4 = positivt</span><span class="sxs-lookup"><span data-stu-id="9e35a-197">4 = positive</span></span>

<span data-ttu-id="9e35a-198">När hello tweet händelser har tilldelats ett värde för sentiment, är de pushas toohello händelsehubb som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9e35a-198">After hello tweet events have been assigned a sentiment value, they are pushed toohello event hub that you created earlier.</span></span>

<span data-ttu-id="9e35a-199">Innan hello programmet körs, kräver vissa information från dig, t.ex. hello Twitter nycklar och anslutningssträngen för hello event hub.</span><span class="sxs-lookup"><span data-stu-id="9e35a-199">Before hello application runs, it requires certain information from you, like hello Twitter keys and hello event hub connection string.</span></span> <span data-ttu-id="9e35a-200">Du kan ange hello konfigurationsinformation på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9e35a-200">You can provide hello configuration information in these ways:</span></span>

* <span data-ttu-id="9e35a-201">Kör hello programmet och sedan använda hello programmets användargränssnitt tooenter hello nycklar hemligheter och anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="9e35a-201">Run hello application, and then use hello application's UI tooenter hello keys, secrets, and connection string.</span></span> <span data-ttu-id="9e35a-202">Om du gör detta hello konfigurationsinformation används för den aktuella sessionen, men sparas inte.</span><span class="sxs-lookup"><span data-stu-id="9e35a-202">If you do this, hello configuration information is used for your current session, but it isn't saved.</span></span>
* <span data-ttu-id="9e35a-203">Redigera hello programmet .config-fil och ange hello värdena.</span><span class="sxs-lookup"><span data-stu-id="9e35a-203">Edit hello application's .config file and set hello values there.</span></span> <span data-ttu-id="9e35a-204">Den här metoden kvarstår hello konfigurationsinformation, men det innebär också att potentiellt känslig information lagras i klartext på datorn.</span><span class="sxs-lookup"><span data-stu-id="9e35a-204">This approach persists hello configuration information, but it also means that this potentially sensitive information is stored in plain text on your computer.</span></span>

<span data-ttu-id="9e35a-205">hello dokument följande procedur båda metoderna.</span><span class="sxs-lookup"><span data-stu-id="9e35a-205">hello following procedure documents both approaches.</span></span> 

1. <span data-ttu-id="9e35a-206">Kontrollera att du har hämtat och uppackade hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) program som anges i hello krav.</span><span class="sxs-lookup"><span data-stu-id="9e35a-206">Make sure you've downloaded and unzipped hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) application, as listed in hello prerequisites.</span></span>

2. <span data-ttu-id="9e35a-207">tooset hello värden vid körning (och endast för hello aktuell session), kör hello `TwitterWPFClient.exe` program.</span><span class="sxs-lookup"><span data-stu-id="9e35a-207">tooset hello values at run time (and only for hello current session), run hello `TwitterWPFClient.exe` application.</span></span> <span data-ttu-id="9e35a-208">När du uppmanas av programmet hello ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="9e35a-208">When hello application prompts you, enter hello following values:</span></span>

    * <span data-ttu-id="9e35a-209">hello Twitter konsumentnyckel (API-nyckel).</span><span class="sxs-lookup"><span data-stu-id="9e35a-209">hello Twitter Consumer Key (API Key).</span></span>
    * <span data-ttu-id="9e35a-210">hello Twitter konsumenten hemligheten (hemliga API).</span><span class="sxs-lookup"><span data-stu-id="9e35a-210">hello Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="9e35a-211">hello Twitter åtkomst-Token.</span><span class="sxs-lookup"><span data-stu-id="9e35a-211">hello Twitter Access Token.</span></span>
    * <span data-ttu-id="9e35a-212">hello Twitter åtkomst-Token hemlighet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-212">hello Twitter Access Token Secret.</span></span>
    * <span data-ttu-id="9e35a-213">hello informationen i anslutningssträngen som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9e35a-213">hello connection string information that you saved earlier.</span></span> <span data-ttu-id="9e35a-214">Kontrollera att du använder hello anslutningssträngen som du tog bort hello `EntityPath` nyckel / värde-par från.</span><span class="sxs-lookup"><span data-stu-id="9e35a-214">Make sure that you use hello connection string that you removed hello `EntityPath` key-value pair from.</span></span>
    * <span data-ttu-id="9e35a-215">hello Twitter nyckelord som du vill toodetermine sentiment för.</span><span class="sxs-lookup"><span data-stu-id="9e35a-215">hello Twitter keywords that you want toodetermine sentiment for.</span></span>

   ![TwitterWpfClient programmet körs, visar dolda inställningarna](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. <span data-ttu-id="9e35a-217">tooset hello värden beständigt, använda en redigerare tooopen hello TwitterWpfClient.exe.config textfil.</span><span class="sxs-lookup"><span data-stu-id="9e35a-217">tooset hello values persistently, use a text editor tooopen hello TwitterWpfClient.exe.config file.</span></span> <span data-ttu-id="9e35a-218">I hello `<appSettings>` element, göra detta:</span><span class="sxs-lookup"><span data-stu-id="9e35a-218">Then in hello `<appSettings>` element, do this:</span></span>

    * <span data-ttu-id="9e35a-219">Ange `oauth_consumer_key` toohello Twitter konsumentnyckel (API-nyckel).</span><span class="sxs-lookup"><span data-stu-id="9e35a-219">Set `oauth_consumer_key` toohello Twitter Consumer Key (API Key).</span></span> 
    * <span data-ttu-id="9e35a-220">Ange `oauth_consumer_secret` toohello Twitter konsumenten hemligheten (hemliga API).</span><span class="sxs-lookup"><span data-stu-id="9e35a-220">Set `oauth_consumer_secret` toohello Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="9e35a-221">Ange `oauth_token` toohello Twitter åtkomst-Token.</span><span class="sxs-lookup"><span data-stu-id="9e35a-221">Set `oauth_token` toohello Twitter Access Token.</span></span>
    * <span data-ttu-id="9e35a-222">Ange `oauth_token_secret` toohello Twitter åtkomst-Token hemlighet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-222">Set `oauth_token_secret` toohello Twitter Access Token Secret.</span></span>

    <span data-ttu-id="9e35a-223">Senare i hello `<appSettings>` element, göra dessa ändringar:</span><span class="sxs-lookup"><span data-stu-id="9e35a-223">Later in hello `<appSettings>` element, make these changes:</span></span>

    * <span data-ttu-id="9e35a-224">Ange `EventHubName` toohello händelsehubbens namn (det vill säga toohello värdet hello entitet sökväg).</span><span class="sxs-lookup"><span data-stu-id="9e35a-224">Set `EventHubName` toohello event hub name (that is, toohello value of hello entity path).</span></span>
    * <span data-ttu-id="9e35a-225">Ange `EventHubNameConnectionString` toohello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="9e35a-225">Set `EventHubNameConnectionString` toohello connection string.</span></span> <span data-ttu-id="9e35a-226">Kontrollera att du använder hello anslutningssträngen som du tog bort hello `EntityPath` nyckel / värde-par från.</span><span class="sxs-lookup"><span data-stu-id="9e35a-226">Make sure that you use hello connection string that you removed hello `EntityPath` key-value pair from.</span></span>

    <span data-ttu-id="9e35a-227">Hej `<appSettings>` avsnitt ser ut som följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="9e35a-227">hello `<appSettings>` section looks like hello following example.</span></span> <span data-ttu-id="9e35a-228">(För tydlighetens skull och säkerhet, vi omsluten några rader och ta bort vissa tecken.)</span><span class="sxs-lookup"><span data-stu-id="9e35a-228">(For clarity and security, we wrapped some lines and removed some characters.)</span></span>

    ![TwitterWpfClient programkonfigurationsfilen i en textredigerare som visar hello Twitter nycklar och hemligheter samt information om hello event hub anslutningssträngar](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. <span data-ttu-id="9e35a-230">Om du inte redan startade hello program, köra TwitterWpfClient.exe.</span><span class="sxs-lookup"><span data-stu-id="9e35a-230">If you didn't already start hello application, run TwitterWpfClient.exe now.</span></span> 

5. <span data-ttu-id="9e35a-231">Klicka på hello gröna start-knappen toocollect sociala sentiment.</span><span class="sxs-lookup"><span data-stu-id="9e35a-231">Click hello green start button toocollect social sentiment.</span></span> <span data-ttu-id="9e35a-232">Du ser Tweet händelser med hello **CreatedAt**, **avsnittet**, och **SentimentScore** värden som skickas till tooyour händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="9e35a-232">You see Tweet events with hello **CreatedAt**, **Topic**, and **SentimentScore** values being sent tooyour event hub.</span></span>

    ![TwitterWpfClient program som körs, visar en lista över tweets](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    ><span data-ttu-id="9e35a-234">Om du får felmeddelanden och du inte ser en dataström med tweets som visas i hello nedre delen av hello fönster, kontrollera hello nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="9e35a-234">If you see errors, and you don't see a stream of tweets displayed in hello lower part of hello window, double-check hello keys and secrets.</span></span> <span data-ttu-id="9e35a-235">Kontrollera även hello anslutningssträngen (se till att den inte innehåller hello `EntityPath` nyckel och värde.)</span><span class="sxs-lookup"><span data-stu-id="9e35a-235">Also check hello connection string (make sure that it does not include hello `EntityPath` key and value.)</span></span>


## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="9e35a-236">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="9e35a-236">Create a Stream Analytics job</span></span>

<span data-ttu-id="9e35a-237">Nu när tweet händelser strömning i realtid från Twitter, kan du ställa in en Stream Analytics-jobbet tooanalyze dessa händelser i realtid.</span><span class="sxs-lookup"><span data-stu-id="9e35a-237">Now that tweet events are streaming in real time from Twitter, you can set up a Stream Analytics job tooanalyze these events in real time.</span></span>

1. <span data-ttu-id="9e35a-238">I hello Azure-portalen klickar du på **ny** > **Sakernas Internet** > **Stream Analytics-jobbet**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-238">In hello Azure portal, click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>

2. <span data-ttu-id="9e35a-239">Hello jobb `socialtwitter-sa-job` och ange en prenumeration, resursgrupp och plats.</span><span class="sxs-lookup"><span data-stu-id="9e35a-239">Name hello job `socialtwitter-sa-job` and specify a subscription, resource group, and location.</span></span>

    <span data-ttu-id="9e35a-240">Det är en bra idé tooplace hello jobbet och hello händelsehubb i hello samma region för bästa prestanda och så att du inte betalar tootransfer data mellan regioner.</span><span class="sxs-lookup"><span data-stu-id="9e35a-240">It's a good idea tooplace hello job and hello event hub in hello same region for best performance and so that you don't pay tootransfer data between regions.</span></span>

    ![Skapar ett nytt Stream Analytics-jobb](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. <span data-ttu-id="9e35a-242">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-242">Click **Create**.</span></span>

    <span data-ttu-id="9e35a-243">hello jobb skapas och hello portalen visar jobbinformation.</span><span class="sxs-lookup"><span data-stu-id="9e35a-243">hello job is created and hello portal displays job details.</span></span>


## <a name="specify-hello-job-input"></a><span data-ttu-id="9e35a-244">Ange hello jobbet indata</span><span class="sxs-lookup"><span data-stu-id="9e35a-244">Specify hello job input</span></span>

1. <span data-ttu-id="9e35a-245">I Stream Analytics-jobbet under **jobbet topologi** hello mitten av hello jobb-bladet, klickar du på **indata**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-245">In your Stream Analytics job, under **Job Topology** in hello middle of hello job blade, click **Inputs**.</span></span> 

2. <span data-ttu-id="9e35a-246">I hello **indata** bladet, klickar du på  **+ &nbsp;Lägg till** och fyller sedan hello bladet med dessa värden:</span><span class="sxs-lookup"><span data-stu-id="9e35a-246">In hello **Inputs** blade, click **+&nbsp;Add** and then fill out hello blade with these values:</span></span>

    * <span data-ttu-id="9e35a-247">**Ett inmatat alias**: Använd hello namn `TwitterStream`.</span><span class="sxs-lookup"><span data-stu-id="9e35a-247">**Input alias**: Use hello name `TwitterStream`.</span></span> <span data-ttu-id="9e35a-248">Om du använder ett annat namn, notera den eftersom du behöver den senare.</span><span class="sxs-lookup"><span data-stu-id="9e35a-248">If you use a different name, make a note of it because you need it later.</span></span>
    * <span data-ttu-id="9e35a-249">**Typ av datakälla**: Välj **dataströmmen**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-249">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="9e35a-250">**Källan**: Välj **händelsehubb**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-250">**Source**: Select **Event hub**.</span></span>
    * <span data-ttu-id="9e35a-251">**Importera alternativet**: Välj **Använd händelsehubb från aktuell prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-251">**Import option**: Select **Use event hub from current subscription**.</span></span> 
    * <span data-ttu-id="9e35a-252">**Service bus-namnrymd**: Markera hello event hub namnområde som du skapade tidigare (`<yourname>-socialtwitter-eh-ns`).</span><span class="sxs-lookup"><span data-stu-id="9e35a-252">**Service bus namespace**: Select hello event hub namespace that you created earlier (`<yourname>-socialtwitter-eh-ns`).</span></span>
    * <span data-ttu-id="9e35a-253">**Händelsehubb**: Välj hello händelsehubb som du skapade tidigare (`socialtwitter-eh`).</span><span class="sxs-lookup"><span data-stu-id="9e35a-253">**Event hub**: Select hello event hub that you created earlier (`socialtwitter-eh`).</span></span>
    * <span data-ttu-id="9e35a-254">**Namnet på händelsehubben princip**: Välj hello åtkomstprincip som du skapade tidigare (`socialtwitter-access`).</span><span class="sxs-lookup"><span data-stu-id="9e35a-254">**Event hub policy name**: Select hello access policy that you created earlier (`socialtwitter-access`).</span></span>

    ![Skapa nya indata för Streaming Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. <span data-ttu-id="9e35a-256">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-256">Click **Create**.</span></span>


## <a name="specify-hello-job-query"></a><span data-ttu-id="9e35a-257">Ange hello jobbfråga</span><span class="sxs-lookup"><span data-stu-id="9e35a-257">Specify hello job query</span></span>

<span data-ttu-id="9e35a-258">Stream Analytics stöder en enkel, deklarativ frågemodell som beskriver transformationer.</span><span class="sxs-lookup"><span data-stu-id="9e35a-258">Stream Analytics supports a simple, declarative query model that describes transformations.</span></span> <span data-ttu-id="9e35a-259">toolearn mer om hello språk finns hello [referens för Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="9e35a-259">toolearn more about hello language, see hello [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>  <span data-ttu-id="9e35a-260">Den här kursen hjälper dig att skapa och testa flera frågor via Twitter-data.</span><span class="sxs-lookup"><span data-stu-id="9e35a-260">This tutorial helps you author and test several queries over Twitter data.</span></span>

<span data-ttu-id="9e35a-261">toocompare hello antalet nämns mellan ämnen, som du kan använda en [rullande fönster](https://msdn.microsoft.com/library/azure/dn835055.aspx) tooget hello antal nämns i avsnittet var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="9e35a-261">toocompare hello number of mentions among topics, you can use a [Tumbling window](https://msdn.microsoft.com/library/azure/dn835055.aspx) tooget hello count of mentions by topic every five seconds.</span></span>

1. <span data-ttu-id="9e35a-262">Stäng hello **indata** bladet om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="9e35a-262">Close hello **Inputs** blade if you haven't already.</span></span>

2. <span data-ttu-id="9e35a-263">Hello jobb-bladet, klickar du på hello **frågan** rutan.</span><span class="sxs-lookup"><span data-stu-id="9e35a-263">In hello job blade, click hello **Query** box.</span></span> <span data-ttu-id="9e35a-264">Azure visar hello indata och utdata som är konfigurerade för hello jobbet och kan du skapa en fråga som kan du omvandla hello Indataströmmen som toohello utdata skickas.</span><span class="sxs-lookup"><span data-stu-id="9e35a-264">Azure lists hello inputs and outputs that are configured for hello job, and lets you create a query that lets you transform hello input stream as it is sent toohello output.</span></span>

3. <span data-ttu-id="9e35a-265">Kontrollera att hello TwitterWpfClient program körs.</span><span class="sxs-lookup"><span data-stu-id="9e35a-265">Make sure that hello TwitterWpfClient application is running.</span></span> 

3. <span data-ttu-id="9e35a-266">I hello **frågan** bladet, klickar du på hello punkter nästa toohello `TwitterStream` indata och välj sedan **exempeldata från indata**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-266">In hello **Query** blade, click hello dots next toohello `TwitterStream` input and then select **Sample data from input**.</span></span>

    ![Menyn Alternativ toouse exempeldata för hello Streaming Analytics-jobbet post med ”exempeldata från indata” markerad](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    <span data-ttu-id="9e35a-268">Då öppnas ett blad som du kan ange hur mycket exempel data tooget som definierats i hur länge tooread hello inkommande dataström.</span><span class="sxs-lookup"><span data-stu-id="9e35a-268">This opens a blade that lets you specify how much sample data tooget, defined in terms of how long tooread hello input stream.</span></span>

4. <span data-ttu-id="9e35a-269">Ange **minuter** too3 och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-269">Set **Minutes** too3 and then click **OK**.</span></span> 
    
    ![Alternativ för provtagning hello Indataströmmen, med ”3 minuter” markerad.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    <span data-ttu-id="9e35a-271">Azure exempel 3 minuter kan du se från hello Indataströmmen och meddelar dig när hello exempeldata är klar.</span><span class="sxs-lookup"><span data-stu-id="9e35a-271">Azure samples 3 minutes' worth of data from hello input stream and notifies you when hello sample data is ready.</span></span> <span data-ttu-id="9e35a-272">(Detta tar en stund.)</span><span class="sxs-lookup"><span data-stu-id="9e35a-272">(This takes a short while.)</span></span> 

    <span data-ttu-id="9e35a-273">hello exempeldata lagras tillfälligt och är tillgänglig när du har hello frågefönster öppnas.</span><span class="sxs-lookup"><span data-stu-id="9e35a-273">hello sample data is stored temporarily and is available while you have hello query window open.</span></span> <span data-ttu-id="9e35a-274">Om du stänger hello frågefönstret hello exempeldata ignoreras och du har toocreate en ny uppsättning exempeldata.</span><span class="sxs-lookup"><span data-stu-id="9e35a-274">If you close hello query window, hello sample data is discarded, and you have toocreate a new set of sample data.</span></span> 

5. <span data-ttu-id="9e35a-275">Ändra hello frågan i hello kod editor toohello följande:</span><span class="sxs-lookup"><span data-stu-id="9e35a-275">Change hello query in hello code editor toohello following:</span></span>

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    <span data-ttu-id="9e35a-276">Om inte `TwitterStream` som hello alias för hello indata ersätta ditt alias för `TwitterStream` i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="9e35a-276">If didn't use `TwitterStream` as hello alias for hello input, substitute your alias for `TwitterStream` in hello query.</span></span>  

    <span data-ttu-id="9e35a-277">Den här frågan använder hello **TIMESTAMP BY** nyckelordet toospecify något tidsstämpelsfält i hello nyttolast toobe används i hello temporala beräkningar.</span><span class="sxs-lookup"><span data-stu-id="9e35a-277">This query uses hello **TIMESTAMP BY** keyword toospecify a timestamp field in hello payload toobe used in hello temporal computation.</span></span> <span data-ttu-id="9e35a-278">Om det här fältet har inte angetts, utförs hello fönsterhantering åtgärden med hjälp av hello länge varje händelse som anlänt på hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="9e35a-278">If this field isn't specified, hello windowing operation is performed by using hello time that each event arrived at hello event hub.</span></span> <span data-ttu-id="9e35a-279">Läs mer i avsnittet hello ”ankomsttid vs programmet tid” i [referens för Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="9e35a-279">Learn more in hello "Arrival Time vs Application Time" section of [Stream Analytics Query Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>

    <span data-ttu-id="9e35a-280">Den här frågan också har åtkomst till en tidstämpel för hello slutet av varje fönster med hjälp av hello **System.Timestamp** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="9e35a-280">This query also accesses a timestamp for hello end of each window by using hello **System.Timestamp** property.</span></span>

5. <span data-ttu-id="9e35a-281">Klicka på **Test**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-281">Click **Test**.</span></span> <span data-ttu-id="9e35a-282">hello frågan körs mot hello data som du tog prov.</span><span class="sxs-lookup"><span data-stu-id="9e35a-282">hello query runs against hello data that you sampled.</span></span>
    
6. <span data-ttu-id="9e35a-283">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-283">Click **Save**.</span></span> <span data-ttu-id="9e35a-284">Detta sparar hello frågan som en del av hello Streaming Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-284">This saves hello query as part of hello Streaming Analytics job.</span></span> <span data-ttu-id="9e35a-285">(Den inte spara hello exempeldata.)</span><span class="sxs-lookup"><span data-stu-id="9e35a-285">(It doesn't save hello sample data.)</span></span>


## <a name="experiment-using-different-fields-from-hello-stream"></a><span data-ttu-id="9e35a-286">Experimentera med olika fält från hello dataström</span><span class="sxs-lookup"><span data-stu-id="9e35a-286">Experiment using different fields from hello stream</span></span> 

<span data-ttu-id="9e35a-287">hello visar följande tabell hello fält som ingår i hello JSON strömmande data.</span><span class="sxs-lookup"><span data-stu-id="9e35a-287">hello following table lists hello fields that are part of hello JSON streaming data.</span></span> <span data-ttu-id="9e35a-288">Känna sig fria tooexperiment i hello frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="9e35a-288">Feel free tooexperiment in hello query editor.</span></span>

|<span data-ttu-id="9e35a-289">JSON-egenskap</span><span class="sxs-lookup"><span data-stu-id="9e35a-289">JSON property</span></span> | <span data-ttu-id="9e35a-290">Definition</span><span class="sxs-lookup"><span data-stu-id="9e35a-290">Definition</span></span>|
|--- | ---|
|<span data-ttu-id="9e35a-291">CreatedAt</span><span class="sxs-lookup"><span data-stu-id="9e35a-291">CreatedAt</span></span> | <span data-ttu-id="9e35a-292">hello tid att hello tweet skapades</span><span class="sxs-lookup"><span data-stu-id="9e35a-292">hello time that hello tweet was created</span></span>|
|<span data-ttu-id="9e35a-293">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="9e35a-293">Topic</span></span> | <span data-ttu-id="9e35a-294">hello avsnitt som matchar hello angivna nyckelordet</span><span class="sxs-lookup"><span data-stu-id="9e35a-294">hello topic that matches hello specified keyword</span></span>|
|<span data-ttu-id="9e35a-295">SentimentScore</span><span class="sxs-lookup"><span data-stu-id="9e35a-295">SentimentScore</span></span> | <span data-ttu-id="9e35a-296">Hej sentiment poäng från Sentiment140</span><span class="sxs-lookup"><span data-stu-id="9e35a-296">hello sentiment score from Sentiment140</span></span>|
|<span data-ttu-id="9e35a-297">Skapa</span><span class="sxs-lookup"><span data-stu-id="9e35a-297">Author</span></span> | <span data-ttu-id="9e35a-298">hello Twitter-referensen som skickas hello tweet</span><span class="sxs-lookup"><span data-stu-id="9e35a-298">hello Twitter handle that sent hello tweet</span></span>|
|<span data-ttu-id="9e35a-299">Text</span><span class="sxs-lookup"><span data-stu-id="9e35a-299">Text</span></span> | <span data-ttu-id="9e35a-300">hello fullständig brödtext hello tweet</span><span class="sxs-lookup"><span data-stu-id="9e35a-300">hello full body of hello tweet</span></span>|


## <a name="create-an-output-sink"></a><span data-ttu-id="9e35a-301">Skapa utdatamottagare</span><span class="sxs-lookup"><span data-stu-id="9e35a-301">Create an output sink</span></span>

<span data-ttu-id="9e35a-302">Du har nu definierat en händelseström, event hub inkommande tooingest händelser och en fråga tooperform en omvandling över hello dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="9e35a-302">You have now defined an event stream, an event hub input tooingest events, and a query tooperform a transformation over hello stream.</span></span> <span data-ttu-id="9e35a-303">hello sista steget är toodefine utdatamottagare för hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-303">hello last step is toodefine an output sink for hello job.</span></span>  

<span data-ttu-id="9e35a-304">I den här självstudiekursen skriva hello samman tweet händelser från hello jobbet frågan tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="9e35a-304">In this tutorial, you write hello aggregated tweet events from hello job query tooAzure Blob storage.</span></span>  <span data-ttu-id="9e35a-305">Du kan också push dina resultat tooAzure SQL-databas, Azure Table storage Event Hubs eller Power BI, beroende på ditt program behöver.</span><span class="sxs-lookup"><span data-stu-id="9e35a-305">You can also push your results tooAzure SQL Database, Azure Table storage, Event Hubs, or Power BI, depending on your application needs.</span></span>

## <a name="specify-hello-job-output"></a><span data-ttu-id="9e35a-306">Ange hello jobbutdata</span><span class="sxs-lookup"><span data-stu-id="9e35a-306">Specify hello job output</span></span>

1. <span data-ttu-id="9e35a-307">I hello **jobbet topologi** klickar du på hello **utdata** rutan.</span><span class="sxs-lookup"><span data-stu-id="9e35a-307">In hello **Job Topology** section, click hello **Output** box.</span></span> 

2. <span data-ttu-id="9e35a-308">I hello **utdata** bladet, klickar du på  **+ &nbsp;Lägg till** och fyller sedan hello bladet med dessa värden:</span><span class="sxs-lookup"><span data-stu-id="9e35a-308">In hello **Outputs** blade, click **+&nbsp;Add** and then fill out hello blade with these values:</span></span>

    * <span data-ttu-id="9e35a-309">**Ett utdataalias**: Använd hello namn `TwitterStream-Output`.</span><span class="sxs-lookup"><span data-stu-id="9e35a-309">**Output alias**: Use hello name `TwitterStream-Output`.</span></span> 
    * <span data-ttu-id="9e35a-310">**Sink**: Välj **Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-310">**Sink**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="9e35a-311">**Importalternativ**: Välj **använda blob storage från aktuell prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-311">**Import options**: Select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="9e35a-312">**Lagringskontot**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-312">**Storage account**.</span></span> <span data-ttu-id="9e35a-313">Välj **skapa ett nytt lagringskonto.**</span><span class="sxs-lookup"><span data-stu-id="9e35a-313">Select **Create a new storage account.**</span></span>
    * <span data-ttu-id="9e35a-314">**Lagringskontot** (andra rutan).</span><span class="sxs-lookup"><span data-stu-id="9e35a-314">**Storage account** (second box).</span></span> <span data-ttu-id="9e35a-315">Ange `YOURNAMEsa`, där `YOURNAME` är ditt namn eller en annan unik sträng.</span><span class="sxs-lookup"><span data-stu-id="9e35a-315">Enter `YOURNAMEsa`, where `YOURNAME` is your name or another unique string.</span></span> <span data-ttu-id="9e35a-316">hello namn kan använda bara gemena bokstäver och siffror, och den måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="9e35a-316">hello name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 
    * <span data-ttu-id="9e35a-317">**Behållaren**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-317">**Container**.</span></span> <span data-ttu-id="9e35a-318">Ange `socialtwitter`.</span><span class="sxs-lookup"><span data-stu-id="9e35a-318">Enter `socialtwitter`.</span></span>
    <span data-ttu-id="9e35a-319">Hej lagringskontonamn och behållarnamn är används tillsammans tooprovide en URI för hello blob storage, så här:</span><span class="sxs-lookup"><span data-stu-id="9e35a-319">hello storage account name and container name are used together tooprovide a URI for hello blob storage, like this:</span></span> 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    ![”Nya utdata” bladet för Stream Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. <span data-ttu-id="9e35a-321">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-321">Click **Create**.</span></span> 

    <span data-ttu-id="9e35a-322">Azure skapar hello storage-konto och skapar automatiskt en nyckel.</span><span class="sxs-lookup"><span data-stu-id="9e35a-322">Azure creates hello storage account and generates a key automatically.</span></span> 

5. <span data-ttu-id="9e35a-323">Stäng hello **utdata** bladet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-323">Close hello **Outputs** blade.</span></span> 


## <a name="start-hello-job"></a><span data-ttu-id="9e35a-324">Starta hello jobbet</span><span class="sxs-lookup"><span data-stu-id="9e35a-324">Start hello job</span></span>

<span data-ttu-id="9e35a-325">Jobbet indata-, fråge- och utdata har angetts.</span><span class="sxs-lookup"><span data-stu-id="9e35a-325">A job input, query, and output are specified.</span></span> <span data-ttu-id="9e35a-326">Är du redo toostart hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="9e35a-326">You are ready toostart hello Stream Analytics job.</span></span>

1. <span data-ttu-id="9e35a-327">Kontrollera att hello TwitterWpfClient program körs.</span><span class="sxs-lookup"><span data-stu-id="9e35a-327">Make sure that hello TwitterWpfClient application is running.</span></span> 

2. <span data-ttu-id="9e35a-328">I hello jobb-bladet, klickar du på **starta**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-328">In hello job blade, click **Start**.</span></span>

    ![Starta hello Stream Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. <span data-ttu-id="9e35a-330">I hello **startjobb** bladet för **jobbutdata starttid**väljer **nu** och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-330">In hello **Start job** blade, for **Job output start time**, select **Now** and then click **Start**.</span></span> 

    ![”Starta jobbet” bladet för hello Stream Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    <span data-ttu-id="9e35a-332">Azure meddelar dig när hello jobb har startat, och i hello jobb-bladet hello visas statusen **kör**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-332">Azure notifies you when hello job has started, and in hello job blade, hello status is displayed as **Running**.</span></span>

    ![Köra jobb](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a><span data-ttu-id="9e35a-334">Visa utdata för sentiment analys</span><span class="sxs-lookup"><span data-stu-id="9e35a-334">View output for sentiment analysis</span></span>

<span data-ttu-id="9e35a-335">När jobbet har startats och bearbetar hello realtid Twitter-dataströmmen kan visa du hello utdata för sentiment analys.</span><span class="sxs-lookup"><span data-stu-id="9e35a-335">After your job has started running and is processing hello real-time Twitter stream, you can view hello output for sentiment analysis.</span></span>

<span data-ttu-id="9e35a-336">Du kan använda ett verktyg som [Azure Lagringsutforskaren](https://http://storageexplorer.com/) eller [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) tooview jobbet utdata i realtid.</span><span class="sxs-lookup"><span data-stu-id="9e35a-336">You can use a tool like [Azure Storage Explorer](https://http://storageexplorer.com/) or [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) tooview your job output in real time.</span></span> <span data-ttu-id="9e35a-337">Härifrån kan du använda [Power BI](https://powerbi.com/) tooextend ditt program tooinclude en anpassade instrumentpanel som hello som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="9e35a-337">From here, you can use [Power BI](https://powerbi.com/) tooextend your application tooinclude a customized dashboard like hello one shown in hello following screenshot:</span></span>

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-tooidentify-trending-topics"></a><span data-ttu-id="9e35a-339">Skapa en annan fråga tooidentify trender avsnitt</span><span class="sxs-lookup"><span data-stu-id="9e35a-339">Create another query tooidentify trending topics</span></span>

<span data-ttu-id="9e35a-340">En annan fråga som du kan använda toounderstand Twitter-sentiment baseras på en [glidande fönstret](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span><span class="sxs-lookup"><span data-stu-id="9e35a-340">Another query you can use toounderstand Twitter sentiment is based on a [Sliding Window](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span></span> <span data-ttu-id="9e35a-341">tooidentify trender ämnen, du söka efter avsnitt som går via ett tröskelvärde för nämns i en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="9e35a-341">tooidentify trending topics, you look for topics that cross a threshold value for mentions in a specified amount of time.</span></span>

<span data-ttu-id="9e35a-342">Hello enligt den här kursen kan du söka efter avsnitt som nämns fler än 20 gånger i hello sista 5 sekunder.</span><span class="sxs-lookup"><span data-stu-id="9e35a-342">For hello purposes of this tutorial, you check for topics that are mentioned more than 20 times in hello last 5 seconds.</span></span>

1. <span data-ttu-id="9e35a-343">I hello jobb-bladet, klickar du på **stoppa** toostop hello jobb.</span><span class="sxs-lookup"><span data-stu-id="9e35a-343">In hello job blade, click **Stop** toostop hello job.</span></span> 

2. <span data-ttu-id="9e35a-344">I hello **jobbet topologi** klickar du på hello **frågan** rutan.</span><span class="sxs-lookup"><span data-stu-id="9e35a-344">In hello **Job Topology** section, click hello **Query** box.</span></span> 

3. <span data-ttu-id="9e35a-345">Ändra hello frågan toohello följande:</span><span class="sxs-lookup"><span data-stu-id="9e35a-345">Change hello query toohello following:</span></span>

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. <span data-ttu-id="9e35a-346">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9e35a-346">Click **Save**.</span></span>

5. <span data-ttu-id="9e35a-347">Kontrollera att hello TwitterWpfClient program körs.</span><span class="sxs-lookup"><span data-stu-id="9e35a-347">Make sure that hello TwitterWpfClient application is running.</span></span> 

6. <span data-ttu-id="9e35a-348">Klicka på **starta** toorestart hello jobb med hjälp av hello ny fråga.</span><span class="sxs-lookup"><span data-stu-id="9e35a-348">Click **Start** toorestart hello job using hello new query.</span></span>


## <a name="get-support"></a><span data-ttu-id="9e35a-349">Få support</span><span class="sxs-lookup"><span data-stu-id="9e35a-349">Get support</span></span>
<span data-ttu-id="9e35a-350">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="9e35a-350">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e35a-351">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9e35a-351">Next steps</span></span>
* [<span data-ttu-id="9e35a-352">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9e35a-352">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="9e35a-353">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="9e35a-353">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="9e35a-354">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="9e35a-354">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="9e35a-355">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="9e35a-355">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="9e35a-356">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="9e35a-356">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
