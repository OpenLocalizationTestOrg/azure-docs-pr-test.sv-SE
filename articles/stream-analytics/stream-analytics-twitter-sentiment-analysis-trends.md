---
title: Realtid Twitter-sentiment analys med Azure Stream Analytics | Microsoft Docs
description: "Lär dig använda Stream Analytics för analys av realtidsskyddet Twitter-sentiment. Stegvisa anvisningar från händelse genereras till data på en levande instrumentpanel."
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
ms.openlocfilehash: 8de6850964700f5b3f71d144b40af927f2e52d7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a><span data-ttu-id="13c3b-105">Realtid Twitter-sentiment analys i Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="13c3b-105">Real-time Twitter sentiment analysis in Azure Stream Analytics</span></span>

<span data-ttu-id="13c3b-106">Lär dig hur du skapar en lösning för analys av sentiment för analys av sociala medier genom att ta realtid Twitter-händelser i Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="13c3b-106">Learn how to build a sentiment analysis solution for social media analytics by bringing real-time Twitter events into Azure Event Hubs.</span></span> <span data-ttu-id="13c3b-107">Du kan sedan skriva en Azure Stream Analytics-fråga för att analysera data och antingen lagra resultaten för senare använda eller använda en instrumentpanel och [Power BI](https://powerbi.com/) till ger inblick i realtid.</span><span class="sxs-lookup"><span data-stu-id="13c3b-107">You can then write an Azure Stream Analytics query to analyze the data and either store the results for later use or use a dashboard and [Power BI](https://powerbi.com/) to provide insights in real time.</span></span>

<span data-ttu-id="13c3b-108">Verktyg för sociala media analytics hjälper företag att förstå trender avsnitt.</span><span class="sxs-lookup"><span data-stu-id="13c3b-108">Social media analytics tools help organizations understand trending topics.</span></span> <span data-ttu-id="13c3b-109">Trender avsnitt är ämnen och attityder som har en stor volym med inlägg i sociala medier.</span><span class="sxs-lookup"><span data-stu-id="13c3b-109">Trending topics are subjects and attitudes that have a high volume of posts in social media.</span></span> <span data-ttu-id="13c3b-110">Sentiment analys, som också kallas *åsikt utvinningsmodellen*, använder sociala media analytics verktyg för att avgöra attityder mot en produkt, idé och så vidare.</span><span class="sxs-lookup"><span data-stu-id="13c3b-110">Sentiment analysis, which is also called *opinion mining*, uses social media analytics tools to determine attitudes toward a product, idea, and so on.</span></span> 

<span data-ttu-id="13c3b-111">Trendanalys för realtid Twitter är ett bra exempel på ett webbanalysverktyg för eftersom hashtaggar prenumeration modellen gör det möjligt att lyssna på specifika nyckelord (hash-taggar) och utveckla sentiment analys av feeden.</span><span class="sxs-lookup"><span data-stu-id="13c3b-111">Real-time Twitter trend analysis is a great example of an analytics tool, because the hashtag subscription model enables you to listen to specific keywords (hashtags) and develop sentiment analysis of the feed.</span></span>

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a><span data-ttu-id="13c3b-112">Scenario: Sociala medier sentiment analys i realtid</span><span class="sxs-lookup"><span data-stu-id="13c3b-112">Scenario: Social media sentiment analysis in real time</span></span>

<span data-ttu-id="13c3b-113">Ett företag som har en webbplats för Nyheter media är intresserad av att få flera fördelar jämfört med dess konkurrenter med innehåll som är direkt relevant för dess läsare.</span><span class="sxs-lookup"><span data-stu-id="13c3b-113">A company that has a news media website is interested in gaining an advantage over its competitors by featuring site content that is immediately relevant to its readers.</span></span> <span data-ttu-id="13c3b-114">Företaget använder analys av sociala medier på ämnen som är relevanta för läsare genom att göra realtid sentiment analys av Twitter-data.</span><span class="sxs-lookup"><span data-stu-id="13c3b-114">The company uses social media analysis on topics that are relevant to readers by doing real-time sentiment analysis of Twitter data.</span></span>

<span data-ttu-id="13c3b-115">Företaget måste analys i realtid om tweet volym och sentiment för viktiga ämnen för att identifiera trender avsnitt i realtid på Twitter.</span><span class="sxs-lookup"><span data-stu-id="13c3b-115">To identify trending topics in real time on Twitter, the company needs real-time analytics about the tweet volume and sentiment for key topics.</span></span> <span data-ttu-id="13c3b-116">Med andra ord är behovet av en sentiment analys analytics-motor som är baserad på den här sociala medier feed.</span><span class="sxs-lookup"><span data-stu-id="13c3b-116">In other words, the need is a sentiment analysis analytics engine that's based on this social media feed.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13c3b-117">Krav</span><span class="sxs-lookup"><span data-stu-id="13c3b-117">Prerequisites</span></span>
<span data-ttu-id="13c3b-118">I den här kursen använder du ett klientprogram som ansluter till Twitter och söker efter tweets som har vissa hash-taggar (som du kan ange).</span><span class="sxs-lookup"><span data-stu-id="13c3b-118">In this tutorial, you use a client application that connects to Twitter and looks for tweets that have certain hashtags (which you can set).</span></span> <span data-ttu-id="13c3b-119">För att köra programmet och analysera tweets med hjälp av Azure Streaming Analytics, måste du ha följande:</span><span class="sxs-lookup"><span data-stu-id="13c3b-119">In order to run the application and analyze the tweets using Azure Streaming Analytics, you must have the following:</span></span>

* <span data-ttu-id="13c3b-120">En Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="13c3b-120">An Azure subscription</span></span>
* <span data-ttu-id="13c3b-121">Ett Twitter-konto</span><span class="sxs-lookup"><span data-stu-id="13c3b-121">A Twitter account</span></span> 
* <span data-ttu-id="13c3b-122">Ett Twitter-program och [OAuth-åtkomsttoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) för programmet.</span><span class="sxs-lookup"><span data-stu-id="13c3b-122">A Twitter application, and the [OAuth access token](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) for that application.</span></span> <span data-ttu-id="13c3b-123">Vi ger avancerade anvisningar för hur du skapar en Twitter-programmet senare.</span><span class="sxs-lookup"><span data-stu-id="13c3b-123">We provide high-level instructions for how to create a Twitter application later.</span></span>
* <span data-ttu-id="13c3b-124">TwitterWPFClient-program som läser Twitter-flöde.</span><span class="sxs-lookup"><span data-stu-id="13c3b-124">The TwitterWPFClient application, which reads the Twitter feed.</span></span> <span data-ttu-id="13c3b-125">För att få det här programmet kan hämta den [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) från GitHub och packa upp paketet till en mapp på datorn.</span><span class="sxs-lookup"><span data-stu-id="13c3b-125">To get this application, download the [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) file from GitHub and then unzip the package into a folder on your computer.</span></span> <span data-ttu-id="13c3b-126">Om du vill visa källan code och kör programmet i en felsökare kan du få källkod från [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator).</span><span class="sxs-lookup"><span data-stu-id="13c3b-126">If you want to see the source code and run the application in a debugger, you can get the source code from [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator).</span></span> 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a><span data-ttu-id="13c3b-127">Skapa en händelsehubb för Streaming Analytics indata</span><span class="sxs-lookup"><span data-stu-id="13c3b-127">Create an event hub for Streaming Analytics input</span></span>

<span data-ttu-id="13c3b-128">Exempelprogrammet genererar händelser och skickar dem till en Azure-händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="13c3b-128">The sample application generates events and pushes them to an Azure event hub.</span></span> <span data-ttu-id="13c3b-129">Azure event hubs är den bästa metoden för händelsen införandet för Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="13c3b-129">Azure event hubs are the preferred method of event ingestion for Stream Analytics.</span></span> <span data-ttu-id="13c3b-130">Mer information finns i [Händelsehubbar i Azure-dokumentationen](../event-hubs/event-hubs-what-is-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="13c3b-130">For more information, see the [Azure Event Hubs documentation](../event-hubs/event-hubs-what-is-event-hubs.md).</span></span>


### <a name="create-an-event-hub-namespace-and-event-hub"></a><span data-ttu-id="13c3b-131">Skapa en event hub namnområde och händelsehubb</span><span class="sxs-lookup"><span data-stu-id="13c3b-131">Create an event hub namespace and event hub</span></span>
<span data-ttu-id="13c3b-132">I den här proceduren du först skapa en event hub-namnområde och sedan lägga till en händelsehubb i detta namnområde.</span><span class="sxs-lookup"><span data-stu-id="13c3b-132">In this procedure, you first create an event hub namespace, and then you add an event hub to that namespace.</span></span> <span data-ttu-id="13c3b-133">Event hub namnområden används för att gruppera relaterade händelser-bussen instanser.</span><span class="sxs-lookup"><span data-stu-id="13c3b-133">Event hub namespaces are used to logically group related event bus instances.</span></span> 

1. <span data-ttu-id="13c3b-134">Logga in på Azure portal och klicka på **ny** > **Sakernas Internet** > **Händelsehubb**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-134">Log  in to the Azure portal and click **New** > **Internet of Things** > **Event Hub**.</span></span> 

2. <span data-ttu-id="13c3b-135">I den **skapa namnområdet** bladet, ange ett namn för namnområdet som `<yourname>-socialtwitter-eh-ns`.</span><span class="sxs-lookup"><span data-stu-id="13c3b-135">In the **Create namespace** blade, enter a namespace name such as `<yourname>-socialtwitter-eh-ns`.</span></span> <span data-ttu-id="13c3b-136">Du kan använda valfritt namn för namnområdet, men namnet måste vara giltigt för en URL och det måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="13c3b-136">You can use any name for the namespace, but the name must be valid for a URL and it must be unique across Azure.</span></span> 
    
3. <span data-ttu-id="13c3b-137">Välj en prenumeration och skapa eller välja en resursgrupp och sedan klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-137">Select a subscription and create or choose a resource group, then click **Create**.</span></span> 

    ![Skapa ett namnområde för event hub](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. <span data-ttu-id="13c3b-139">När namnområdet distribution har slutförts, kan du hitta event hub namnområdet i din lista över Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="13c3b-139">When the namespace has finished deploying, find the event hub namespace in your list of Azure resources.</span></span> 

5. <span data-ttu-id="13c3b-140">Klicka på det nya namnområdet och i bladet namnområdet på  **+ &nbsp;Händelsehubb**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-140">Click the new namespace, and in the namespace blade, click **+&nbsp;Event Hub**.</span></span> 

    ![<span data-ttu-id="13c3b-141">Knappen Lägg till Händelsehubben för att skapa en ny händelsehubb</span><span class="sxs-lookup"><span data-stu-id="13c3b-141">The Add Event Hub button for creating a new event hub</span></span> ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. <span data-ttu-id="13c3b-142">Namn på ny händelsehubb `socialtwitter-eh`.</span><span class="sxs-lookup"><span data-stu-id="13c3b-142">Name the new event hub `socialtwitter-eh`.</span></span> <span data-ttu-id="13c3b-143">Du kan använda ett annat namn.</span><span class="sxs-lookup"><span data-stu-id="13c3b-143">You can use a different name.</span></span> <span data-ttu-id="13c3b-144">Om du vill anteckna, eftersom du måste ha namnet senare.</span><span class="sxs-lookup"><span data-stu-id="13c3b-144">If you do, make a note of it, because you need the name later.</span></span> <span data-ttu-id="13c3b-145">Du behöver inte ange andra alternativ för händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="13c3b-145">You don't need to set any other options for the event hub.</span></span>

    ![Bladet för att skapa en ny händelsehubb](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. <span data-ttu-id="13c3b-147">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-147">Click **Create**.</span></span>


### <a name="grant-access-to-the-event-hub"></a><span data-ttu-id="13c3b-148">Bevilja åtkomst till händelsehubben</span><span class="sxs-lookup"><span data-stu-id="13c3b-148">Grant access to the event hub</span></span>

<span data-ttu-id="13c3b-149">Innan en process kan skicka data till en händelsehubb, måste händelsehubben ha en princip som tillåter åtkomstbehörighet.</span><span class="sxs-lookup"><span data-stu-id="13c3b-149">Before a process can send data to an event hub, the event hub must have a policy that allows appropriate access.</span></span> <span data-ttu-id="13c3b-150">Åtkomstprincipen producerar en anslutningssträng som innehåller auktoriseringsinformation om.</span><span class="sxs-lookup"><span data-stu-id="13c3b-150">The access policy produces a connection string that includes authorization information.</span></span>

1.  <span data-ttu-id="13c3b-151">I bladet händelse namnområde klickar du på **Händelsehubbar** och klicka sedan på namnet på den nya händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="13c3b-151">In the event namespace blade, click **Event Hubs** and then click the name of your new event hub.</span></span>

2.  <span data-ttu-id="13c3b-152">Klicka i hubbladet händelse **principer för delad åtkomst** och klicka sedan på  **+ &nbsp;Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-152">In the event hub blade, click **Shared access policies** and then click **+&nbsp;Add**.</span></span>

    >[!NOTE]
    ><span data-ttu-id="13c3b-153">Kontrollera att du arbetar med händelsehubb, inte event hub namnområde.</span><span class="sxs-lookup"><span data-stu-id="13c3b-153">Make sure you're working with the event hub, not the event hub namespace.</span></span>

3.  <span data-ttu-id="13c3b-154">Lägg till en princip med namnet `socialtwitter-access` och **anspråk**väljer **hantera**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-154">Add a policy named `socialtwitter-access` and for **Claim**, select **Manage**.</span></span>

    ![Bladet för att skapa en ny händelse hubb princip](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  <span data-ttu-id="13c3b-156">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-156">Click **Create**.</span></span>

5.  <span data-ttu-id="13c3b-157">När principen har distribuerats, klickar du på den i listan med principer för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="13c3b-157">After the policy has been deployed, click it in the list of shared access policies.</span></span>

6.  <span data-ttu-id="13c3b-158">I listrutan **sträng-primära ANSLUTNINGSNYCKEL** och klicka på kopieringsknappen bredvid anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="13c3b-158">Find the box labeled **CONNECTION STRING-PRIMARY KEY** and click the copy button next to the connection string.</span></span> 
    
    ![Kopiera den primära anslutning strängnyckeln från principen för åtkomst](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  <span data-ttu-id="13c3b-160">Klistra in anslutningssträngen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="13c3b-160">Paste the connection string into a text editor.</span></span> <span data-ttu-id="13c3b-161">Du behöver den här anslutningssträngen för nästa avsnitt, när du har mindre ändringar.</span><span class="sxs-lookup"><span data-stu-id="13c3b-161">You need this connection string for the next section, after you make some small edits to it.</span></span>

    <span data-ttu-id="13c3b-162">Anslutningssträngen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="13c3b-162">The connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    <span data-ttu-id="13c3b-163">Observera att anslutningssträngen innehåller flera nyckel-värdepar, avgränsade med semikolon: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, och `EntityPath`.</span><span class="sxs-lookup"><span data-stu-id="13c3b-163">Notice that the connection string contains multiple key-value pairs, separated with semicolons: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, and `EntityPath`.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="13c3b-164">Delar av anslutningssträngen i exemplet för säkerhet, har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="13c3b-164">For security, parts of the connection string in the example have been removed.</span></span>

8.  <span data-ttu-id="13c3b-165">I textredigeraren kan ta bort den `EntityPath` par från anslutningssträngen (Glöm inte att ta bort semikolonet som föregår den).</span><span class="sxs-lookup"><span data-stu-id="13c3b-165">In the text editor, remove the `EntityPath` pair from the connection string (don't forget to remove the semicolon that precedes it).</span></span> <span data-ttu-id="13c3b-166">När du är klar anslutningssträngen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="13c3b-166">When you're done, the connection string looks like this:</span></span>

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-the-twitter-client-application"></a><span data-ttu-id="13c3b-167">Konfigurera och starta klientprogrammet Twitter</span><span class="sxs-lookup"><span data-stu-id="13c3b-167">Configure and start the Twitter client application</span></span>
<span data-ttu-id="13c3b-168">Klientprogrammet får tweet händelser direkt från Twitter.</span><span class="sxs-lookup"><span data-stu-id="13c3b-168">The client application gets tweet events directly from Twitter.</span></span> <span data-ttu-id="13c3b-169">För att göra så den behöver behörighet att anropa Twitter Streaming API: erna.</span><span class="sxs-lookup"><span data-stu-id="13c3b-169">In order to do so, it needs permission to call the Twitter Streaming APIs.</span></span> <span data-ttu-id="13c3b-170">Om du vill konfigurera behörigheten Skapa ett program i Twitter, vilket genererar unika autentiseringsuppgifter (t.ex en OAuth-token).</span><span class="sxs-lookup"><span data-stu-id="13c3b-170">To configure that permission, you create an application in Twitter, which generates unique credentials (such as an OAuth token).</span></span> <span data-ttu-id="13c3b-171">Du kan sedan konfigurera klientprogrammet använder dessa autentiseringsuppgifter när den gör API-anrop.</span><span class="sxs-lookup"><span data-stu-id="13c3b-171">You can then configure the client application to use these credentials when it makes API calls.</span></span> 

### <a name="create-a-twitter-application"></a><span data-ttu-id="13c3b-172">Skapa ett Twitter-program</span><span class="sxs-lookup"><span data-stu-id="13c3b-172">Create a Twitter application</span></span>
<span data-ttu-id="13c3b-173">Om du inte redan har ett Twitter-program som du kan använda för den här kursen kan skapa du en.</span><span class="sxs-lookup"><span data-stu-id="13c3b-173">If you do not already have a Twitter application that you can use for this tutorial, you can create one.</span></span> <span data-ttu-id="13c3b-174">Du måste redan ha ett Twitter-konto.</span><span class="sxs-lookup"><span data-stu-id="13c3b-174">You must already have a Twitter account.</span></span>

> [!NOTE]
> <span data-ttu-id="13c3b-175">Exakt processen i Twitter för att skapa ett program och hämta de nycklar och hemligheter token kan ändras.</span><span class="sxs-lookup"><span data-stu-id="13c3b-175">The exact process in Twitter for creating an application and getting the keys, secrets, and token might change.</span></span> <span data-ttu-id="13c3b-176">Om dessa instruktioner inte matchar det som visas på Twitter-plats finns i dokumentationen för Twitter-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="13c3b-176">If these instructions don't match what you see on the Twitter site, refer to the Twitter developer documentation.</span></span>

1. <span data-ttu-id="13c3b-177">Gå till den [sidan Twitter programhantering](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="13c3b-177">Go to the [Twitter application management page](https://apps.twitter.com/).</span></span> 

2. <span data-ttu-id="13c3b-178">Skapa ett nytt program.</span><span class="sxs-lookup"><span data-stu-id="13c3b-178">Create a new application.</span></span> 

    * <span data-ttu-id="13c3b-179">Ange en giltig URL för webbplats-URL.</span><span class="sxs-lookup"><span data-stu-id="13c3b-179">For the website URL, specify a valid URL.</span></span> <span data-ttu-id="13c3b-180">Det behöver inte vara en live-webbplats.</span><span class="sxs-lookup"><span data-stu-id="13c3b-180">It does not have to be a live site.</span></span> <span data-ttu-id="13c3b-181">(Du kan inte ange bara `localhost`.)</span><span class="sxs-lookup"><span data-stu-id="13c3b-181">(You can't specify just `localhost`.)</span></span>
    * <span data-ttu-id="13c3b-182">Lämna fältet återanrop tomt.</span><span class="sxs-lookup"><span data-stu-id="13c3b-182">Leave the callback field blank.</span></span> <span data-ttu-id="13c3b-183">Klientprogrammet som du använder för den här kursen kräver inte återanrop.</span><span class="sxs-lookup"><span data-stu-id="13c3b-183">The client application you use for this tutorial doesn't require callbacks.</span></span>

    ![Skapa ett program i Twitter](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. <span data-ttu-id="13c3b-185">Du kan också ändra programmets behörigheter till skrivskyddat läge.</span><span class="sxs-lookup"><span data-stu-id="13c3b-185">Optionally, change the application's permissions to read-only.</span></span>

4. <span data-ttu-id="13c3b-186">Gå till när programmet har skapats kan den **nycklar och åtkomst-token** sidan.</span><span class="sxs-lookup"><span data-stu-id="13c3b-186">When the application is created, go to the **Keys and Access Tokens** page.</span></span>

5. <span data-ttu-id="13c3b-187">Klicka på knappen för att generera en åtkomst-token och åtkomst-token hemlighet.</span><span class="sxs-lookup"><span data-stu-id="13c3b-187">Click the button to generate an access token and access token secret.</span></span>

<span data-ttu-id="13c3b-188">Behåll den här informationen praktisk, eftersom du behöver i nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="13c3b-188">Keep this information handy, because you will need it in the next procedure.</span></span>

>[!NOTE]
><span data-ttu-id="13c3b-189">Nycklar och hemligheter för programmets Twitter ger åtkomst till ditt Twitter-konto.</span><span class="sxs-lookup"><span data-stu-id="13c3b-189">The keys and secrets for the Twitter application provide access to your Twitter account.</span></span> <span data-ttu-id="13c3b-190">Behandla den här informationen som känsliga på samma sätt som du gör ditt Twitter-lösenord.</span><span class="sxs-lookup"><span data-stu-id="13c3b-190">Treat this information as sensitive, the same as you do your Twitter password.</span></span> <span data-ttu-id="13c3b-191">Till exempel Bädda inte in informationen i ett program som du ger.</span><span class="sxs-lookup"><span data-stu-id="13c3b-191">For example, don't embed this information in an application that you give to others.</span></span> 


### <a name="configure-the-client-application"></a><span data-ttu-id="13c3b-192">Konfigurera klientprogrammet</span><span class="sxs-lookup"><span data-stu-id="13c3b-192">Configure the client application</span></span>
<span data-ttu-id="13c3b-193">Vi har skapat ett klientprogram som ansluter till Twitter data med hjälp av [Twitter's Streaming API: er](https://dev.twitter.com/streaming/overview) att samla in händelser för tweet om en specifik uppsättning avsnitt.</span><span class="sxs-lookup"><span data-stu-id="13c3b-193">We've created a client application that connects to Twitter data using [Twitter's Streaming APIs](https://dev.twitter.com/streaming/overview) to collect tweet events about a specific set of topics.</span></span> <span data-ttu-id="13c3b-194">Programmet använder den [Sentiment140](http://help.sentiment140.com/) öppen källkod verktyg som tilldelas varje tweet sentiment följande värde:</span><span class="sxs-lookup"><span data-stu-id="13c3b-194">The application uses the [Sentiment140](http://help.sentiment140.com/) open source tool, which assigns the following sentiment value to each tweet:</span></span>

* <span data-ttu-id="13c3b-195">0 = negativt</span><span class="sxs-lookup"><span data-stu-id="13c3b-195">0 = negative</span></span>
* <span data-ttu-id="13c3b-196">2 = neutral</span><span class="sxs-lookup"><span data-stu-id="13c3b-196">2 = neutral</span></span>
* <span data-ttu-id="13c3b-197">4 = positivt</span><span class="sxs-lookup"><span data-stu-id="13c3b-197">4 = positive</span></span>

<span data-ttu-id="13c3b-198">När tweet händelser har tilldelats ett värde för sentiment, är de pushas till händelsehubben som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="13c3b-198">After the tweet events have been assigned a sentiment value, they are pushed to the event hub that you created earlier.</span></span>

<span data-ttu-id="13c3b-199">Innan programmet körs, kräver vissa information från dig, t.ex. Twitter-nycklar och anslutningssträngen event hub.</span><span class="sxs-lookup"><span data-stu-id="13c3b-199">Before the application runs, it requires certain information from you, like the Twitter keys and the event hub connection string.</span></span> <span data-ttu-id="13c3b-200">Du kan ange konfigurationsinformation på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="13c3b-200">You can provide the configuration information in these ways:</span></span>

* <span data-ttu-id="13c3b-201">Kör programmet och sedan använda programmets användargränssnitt för att ange de nycklar och hemligheter anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="13c3b-201">Run the application, and then use the application's UI to enter the keys, secrets, and connection string.</span></span> <span data-ttu-id="13c3b-202">Om du gör detta konfigurationsinformationen används för den aktuella sessionen, men sparas inte.</span><span class="sxs-lookup"><span data-stu-id="13c3b-202">If you do this, the configuration information is used for your current session, but it isn't saved.</span></span>
* <span data-ttu-id="13c3b-203">Redigera programmets .config-fil och ange värden för det.</span><span class="sxs-lookup"><span data-stu-id="13c3b-203">Edit the application's .config file and set the values there.</span></span> <span data-ttu-id="13c3b-204">Den här metoden kvarstår konfigurationsinformation, men det innebär också att potentiellt känslig information lagras i klartext på datorn.</span><span class="sxs-lookup"><span data-stu-id="13c3b-204">This approach persists the configuration information, but it also means that this potentially sensitive information is stored in plain text on your computer.</span></span>

<span data-ttu-id="13c3b-205">Följande procedur dokument båda metoderna.</span><span class="sxs-lookup"><span data-stu-id="13c3b-205">The following procedure documents both approaches.</span></span> 

1. <span data-ttu-id="13c3b-206">Kontrollera att du har hämtat och uppackade den [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) program enligt kraven.</span><span class="sxs-lookup"><span data-stu-id="13c3b-206">Make sure you've downloaded and unzipped the [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) application, as listed in the prerequisites.</span></span>

2. <span data-ttu-id="13c3b-207">Ange värden vid körning (och endast för den aktuella sessionen) kör den `TwitterWPFClient.exe` program.</span><span class="sxs-lookup"><span data-stu-id="13c3b-207">To set the values at run time (and only for the current session), run the `TwitterWPFClient.exe` application.</span></span> <span data-ttu-id="13c3b-208">När du uppmanas av programmet kan du ange följande värden:</span><span class="sxs-lookup"><span data-stu-id="13c3b-208">When the application prompts you, enter the following values:</span></span>

    * <span data-ttu-id="13c3b-209">Twitter konsumentnyckel (API-nyckel).</span><span class="sxs-lookup"><span data-stu-id="13c3b-209">The Twitter Consumer Key (API Key).</span></span>
    * <span data-ttu-id="13c3b-210">Twitter konsumenten hemligheten (hemliga API).</span><span class="sxs-lookup"><span data-stu-id="13c3b-210">The Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="13c3b-211">Twitter-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="13c3b-211">The Twitter Access Token.</span></span>
    * <span data-ttu-id="13c3b-212">Twitter åtkomst-Token hemligheten.</span><span class="sxs-lookup"><span data-stu-id="13c3b-212">The Twitter Access Token Secret.</span></span>
    * <span data-ttu-id="13c3b-213">Informationen i anslutningssträngen som du sparade tidigare.</span><span class="sxs-lookup"><span data-stu-id="13c3b-213">The connection string information that you saved earlier.</span></span> <span data-ttu-id="13c3b-214">Kontrollera att du använder anslutningssträngen som du har tagit bort den `EntityPath` nyckel / värde-par från.</span><span class="sxs-lookup"><span data-stu-id="13c3b-214">Make sure that you use the connection string that you removed the `EntityPath` key-value pair from.</span></span>
    * <span data-ttu-id="13c3b-215">Twitter-nyckelord som du vill fastställa sentiment för.</span><span class="sxs-lookup"><span data-stu-id="13c3b-215">The Twitter keywords that you want to determine sentiment for.</span></span>

   ![TwitterWpfClient programmet körs, visar dolda inställningarna](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. <span data-ttu-id="13c3b-217">Använd en textredigerare för att öppna filen TwitterWpfClient.exe.config för att ange värden för beständigt.</span><span class="sxs-lookup"><span data-stu-id="13c3b-217">To set the values persistently, use a text editor to open the TwitterWpfClient.exe.config file.</span></span> <span data-ttu-id="13c3b-218">I den `<appSettings>` element, göra detta:</span><span class="sxs-lookup"><span data-stu-id="13c3b-218">Then in the `<appSettings>` element, do this:</span></span>

    * <span data-ttu-id="13c3b-219">Ange `oauth_consumer_key` till Twitter konsumentnyckel (API-nyckel).</span><span class="sxs-lookup"><span data-stu-id="13c3b-219">Set `oauth_consumer_key` to the Twitter Consumer Key (API Key).</span></span> 
    * <span data-ttu-id="13c3b-220">Ange `oauth_consumer_secret` till Twitter konsumenten hemligheten (hemliga API).</span><span class="sxs-lookup"><span data-stu-id="13c3b-220">Set `oauth_consumer_secret` to the Twitter Consumer Secret (API Secret).</span></span>
    * <span data-ttu-id="13c3b-221">Ange `oauth_token` till Twitter-åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="13c3b-221">Set `oauth_token` to the Twitter Access Token.</span></span>
    * <span data-ttu-id="13c3b-222">Ange `oauth_token_secret` till Twitter åtkomst-Token hemlighet.</span><span class="sxs-lookup"><span data-stu-id="13c3b-222">Set `oauth_token_secret` to the Twitter Access Token Secret.</span></span>

    <span data-ttu-id="13c3b-223">Senare i den `<appSettings>` element, göra dessa ändringar:</span><span class="sxs-lookup"><span data-stu-id="13c3b-223">Later in the `<appSettings>` element, make these changes:</span></span>

    * <span data-ttu-id="13c3b-224">Ange `EventHubName` till händelsehubbens namn (det vill säga till värdet för entiteten sökväg).</span><span class="sxs-lookup"><span data-stu-id="13c3b-224">Set `EventHubName` to the event hub name (that is, to the value of the entity path).</span></span>
    * <span data-ttu-id="13c3b-225">Ange `EventHubNameConnectionString` i anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="13c3b-225">Set `EventHubNameConnectionString` to the connection string.</span></span> <span data-ttu-id="13c3b-226">Kontrollera att du använder anslutningssträngen som du har tagit bort den `EntityPath` nyckel / värde-par från.</span><span class="sxs-lookup"><span data-stu-id="13c3b-226">Make sure that you use the connection string that you removed the `EntityPath` key-value pair from.</span></span>

    <span data-ttu-id="13c3b-227">Den `<appSettings>` avsnitt ser ut som följande exempel.</span><span class="sxs-lookup"><span data-stu-id="13c3b-227">The `<appSettings>` section looks like the following example.</span></span> <span data-ttu-id="13c3b-228">(För tydlighetens skull och säkerhet, vi omsluten några rader och ta bort vissa tecken.)</span><span class="sxs-lookup"><span data-stu-id="13c3b-228">(For clarity and security, we wrapped some lines and removed some characters.)</span></span>

    ![TwitterWpfClient programkonfigurationsfilen i en textredigerare, visar Twitter-nycklar och hemligheter och event hub informationen i anslutningssträngen](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. <span data-ttu-id="13c3b-230">Om du inte redan startar programmet kan köra TwitterWpfClient.exe.</span><span class="sxs-lookup"><span data-stu-id="13c3b-230">If you didn't already start the application, run TwitterWpfClient.exe now.</span></span> 

5. <span data-ttu-id="13c3b-231">Klicka på den gröna start-knappen för att samla in sociala sentiment.</span><span class="sxs-lookup"><span data-stu-id="13c3b-231">Click the green start button to collect social sentiment.</span></span> <span data-ttu-id="13c3b-232">Du ser Tweet händelser med den **CreatedAt**, **avsnittet**, och **SentimentScore** värden som skickas till din event hub.</span><span class="sxs-lookup"><span data-stu-id="13c3b-232">You see Tweet events with the **CreatedAt**, **Topic**, and **SentimentScore** values being sent to your event hub.</span></span>

    ![TwitterWpfClient program som körs, visar en lista över tweets](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    ><span data-ttu-id="13c3b-234">Om du får felmeddelanden och du inte ser en dataström med tweets som visas i den nedre delen av fönstret, kontrollera nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="13c3b-234">If you see errors, and you don't see a stream of tweets displayed in the lower part of the window, double-check the keys and secrets.</span></span> <span data-ttu-id="13c3b-235">Kontrollera också anslutningssträngen (se till att den inte innehåller den `EntityPath` nyckel och värde.)</span><span class="sxs-lookup"><span data-stu-id="13c3b-235">Also check the connection string (make sure that it does not include the `EntityPath` key and value.)</span></span>


## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="13c3b-236">Skapa ett Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="13c3b-236">Create a Stream Analytics job</span></span>

<span data-ttu-id="13c3b-237">Nu när tweet händelser strömning i realtid från Twitter, kan du ställa in ett Stream Analytics-jobb att analysera dessa händelser i realtid.</span><span class="sxs-lookup"><span data-stu-id="13c3b-237">Now that tweet events are streaming in real time from Twitter, you can set up a Stream Analytics job to analyze these events in real time.</span></span>

1. <span data-ttu-id="13c3b-238">I Azure-portalen klickar du på **ny** > **Sakernas Internet** > **Stream Analytics-jobbet**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-238">In the Azure portal, click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>

2. <span data-ttu-id="13c3b-239">Namnge jobbet `socialtwitter-sa-job` och ange en prenumeration, resursgrupp och plats.</span><span class="sxs-lookup"><span data-stu-id="13c3b-239">Name the job `socialtwitter-sa-job` and specify a subscription, resource group, and location.</span></span>

    <span data-ttu-id="13c3b-240">Det är en bra idé att placera jobbet och händelsehubben i samma region för bästa prestanda och så att du inte betala för att överföra data mellan regioner.</span><span class="sxs-lookup"><span data-stu-id="13c3b-240">It's a good idea to place the job and the event hub in the same region for best performance and so that you don't pay to transfer data between regions.</span></span>

    ![Skapar ett nytt Stream Analytics-jobb](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. <span data-ttu-id="13c3b-242">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-242">Click **Create**.</span></span>

    <span data-ttu-id="13c3b-243">Jobbet har skapats och portalen visar jobbinformation.</span><span class="sxs-lookup"><span data-stu-id="13c3b-243">The job is created and the portal displays job details.</span></span>


## <a name="specify-the-job-input"></a><span data-ttu-id="13c3b-244">Ange indata för jobb</span><span class="sxs-lookup"><span data-stu-id="13c3b-244">Specify the job input</span></span>

1. <span data-ttu-id="13c3b-245">I Stream Analytics-jobbet under **jobbet topologi** mitt i jobb-bladet klickar du på **indata**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-245">In your Stream Analytics job, under **Job Topology** in the middle of the job blade, click **Inputs**.</span></span> 

2. <span data-ttu-id="13c3b-246">I den **indata** bladet, klickar du på  **+ &nbsp;Lägg till** och fyll sedan i bladet med dessa värden:</span><span class="sxs-lookup"><span data-stu-id="13c3b-246">In the **Inputs** blade, click **+&nbsp;Add** and then fill out the blade with these values:</span></span>

    * <span data-ttu-id="13c3b-247">**Ett inmatat alias**: Använd namnet `TwitterStream`.</span><span class="sxs-lookup"><span data-stu-id="13c3b-247">**Input alias**: Use the name `TwitterStream`.</span></span> <span data-ttu-id="13c3b-248">Om du använder ett annat namn, notera den eftersom du behöver den senare.</span><span class="sxs-lookup"><span data-stu-id="13c3b-248">If you use a different name, make a note of it because you need it later.</span></span>
    * <span data-ttu-id="13c3b-249">**Typ av datakälla**: Välj **dataströmmen**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-249">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="13c3b-250">**Källan**: Välj **händelsehubb**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-250">**Source**: Select **Event hub**.</span></span>
    * <span data-ttu-id="13c3b-251">**Importera alternativet**: Välj **Använd händelsehubb från aktuell prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-251">**Import option**: Select **Use event hub from current subscription**.</span></span> 
    * <span data-ttu-id="13c3b-252">**Service bus-namnrymd**: Välj händelsen hubb namnområdet som du skapade tidigare (`<yourname>-socialtwitter-eh-ns`).</span><span class="sxs-lookup"><span data-stu-id="13c3b-252">**Service bus namespace**: Select the event hub namespace that you created earlier (`<yourname>-socialtwitter-eh-ns`).</span></span>
    * <span data-ttu-id="13c3b-253">**Händelsehubb**: Välj händelsehubben som du skapade tidigare (`socialtwitter-eh`).</span><span class="sxs-lookup"><span data-stu-id="13c3b-253">**Event hub**: Select the event hub that you created earlier (`socialtwitter-eh`).</span></span>
    * <span data-ttu-id="13c3b-254">**Namnet på händelsehubben princip**: Välj den åtkomstprincip som du skapade tidigare (`socialtwitter-access`).</span><span class="sxs-lookup"><span data-stu-id="13c3b-254">**Event hub policy name**: Select the access policy that you created earlier (`socialtwitter-access`).</span></span>

    ![Skapa nya indata för Streaming Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. <span data-ttu-id="13c3b-256">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-256">Click **Create**.</span></span>


## <a name="specify-the-job-query"></a><span data-ttu-id="13c3b-257">Ange frågan som jobb</span><span class="sxs-lookup"><span data-stu-id="13c3b-257">Specify the job query</span></span>

<span data-ttu-id="13c3b-258">Stream Analytics stöder en enkel, deklarativ frågemodell som beskriver transformationer.</span><span class="sxs-lookup"><span data-stu-id="13c3b-258">Stream Analytics supports a simple, declarative query model that describes transformations.</span></span> <span data-ttu-id="13c3b-259">Mer information om språk finns i [referens för Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="13c3b-259">To learn more about the language, see the [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>  <span data-ttu-id="13c3b-260">Den här kursen hjälper dig att skapa och testa flera frågor via Twitter-data.</span><span class="sxs-lookup"><span data-stu-id="13c3b-260">This tutorial helps you author and test several queries over Twitter data.</span></span>

<span data-ttu-id="13c3b-261">Om du vill jämföra antalet nämns mellan ämnen som du kan använda en [rullande fönster](https://msdn.microsoft.com/library/azure/dn835055.aspx) att hämta antalet nämns i avsnittet var femte sekund.</span><span class="sxs-lookup"><span data-stu-id="13c3b-261">To compare the number of mentions among topics, you can use a [Tumbling window](https://msdn.microsoft.com/library/azure/dn835055.aspx) to get the count of mentions by topic every five seconds.</span></span>

1. <span data-ttu-id="13c3b-262">Stäng den **indata** bladet om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="13c3b-262">Close the **Inputs** blade if you haven't already.</span></span>

2. <span data-ttu-id="13c3b-263">I jobb-bladet klickar du på den **frågan** rutan.</span><span class="sxs-lookup"><span data-stu-id="13c3b-263">In the job blade, click the **Query** box.</span></span> <span data-ttu-id="13c3b-264">Azure visar in- och utgångar som är konfigurerade för jobbet och kan du skapa en fråga som kan du omvandla Indataströmmen som skickas till utdata.</span><span class="sxs-lookup"><span data-stu-id="13c3b-264">Azure lists the inputs and outputs that are configured for the job, and lets you create a query that lets you transform the input stream as it is sent to the output.</span></span>

3. <span data-ttu-id="13c3b-265">Kontrollera att programmet TwitterWpfClient körs.</span><span class="sxs-lookup"><span data-stu-id="13c3b-265">Make sure that the TwitterWpfClient application is running.</span></span> 

3. <span data-ttu-id="13c3b-266">I den **frågan** bladet, klickar du på punkter bredvid den `TwitterStream` indata och välj sedan **exempeldata från indata**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-266">In the **Query** blade, click the dots next to the `TwitterStream` input and then select **Sample data from input**.</span></span>

    ![Menyn Alternativ exempeldata för posten Streaming Analytics-jobb med ”exempeldata från indata” markerad](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    <span data-ttu-id="13c3b-268">Då öppnas ett blad som du kan ange hur mycket exempeldata som kan definieras enligt hur lång tid för att läsa Indataströmmen.</span><span class="sxs-lookup"><span data-stu-id="13c3b-268">This opens a blade that lets you specify how much sample data to get, defined in terms of how long to read the input stream.</span></span>

4. <span data-ttu-id="13c3b-269">Ange **minuter** till 3 och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-269">Set **Minutes** to 3 and then click **OK**.</span></span> 
    
    ![Alternativ för provtagning Indataströmmen, med ”3 minuter” markerad.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    <span data-ttu-id="13c3b-271">Azure exempel 3 minuter kan du se från Indataströmmen och meddelar dig när exempeldata är klar.</span><span class="sxs-lookup"><span data-stu-id="13c3b-271">Azure samples 3 minutes' worth of data from the input stream and notifies you when the sample data is ready.</span></span> <span data-ttu-id="13c3b-272">(Detta tar en stund.)</span><span class="sxs-lookup"><span data-stu-id="13c3b-272">(This takes a short while.)</span></span> 

    <span data-ttu-id="13c3b-273">Exempeldata lagras tillfälligt och är tillgänglig när du har frågefönster öppnas.</span><span class="sxs-lookup"><span data-stu-id="13c3b-273">The sample data is stored temporarily and is available while you have the query window open.</span></span> <span data-ttu-id="13c3b-274">Om du stänger frågefönstret exempeldata ignoreras och du måste skapa en ny uppsättning exempeldata.</span><span class="sxs-lookup"><span data-stu-id="13c3b-274">If you close the query window, the sample data is discarded, and you have to create a new set of sample data.</span></span> 

5. <span data-ttu-id="13c3b-275">Ändra frågan i kodredigeraren för till följande:</span><span class="sxs-lookup"><span data-stu-id="13c3b-275">Change the query in the code editor to the following:</span></span>

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    <span data-ttu-id="13c3b-276">Om inte `TwitterStream` som alias för indata ersätta ditt alias för `TwitterStream` i frågan.</span><span class="sxs-lookup"><span data-stu-id="13c3b-276">If didn't use `TwitterStream` as the alias for the input, substitute your alias for `TwitterStream` in the query.</span></span>  

    <span data-ttu-id="13c3b-277">Den här frågan använder den **TIMESTAMP BY** nyckelord för att ange något tidsstämpelsfält i nyttolasten som ska användas i den temporala beräkningen.</span><span class="sxs-lookup"><span data-stu-id="13c3b-277">This query uses the **TIMESTAMP BY** keyword to specify a timestamp field in the payload to be used in the temporal computation.</span></span> <span data-ttu-id="13c3b-278">Om det här fältet har inte angetts, utförs fönsterhantering åtgärden med hjälp av den tid som varje händelse som anlänt på händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="13c3b-278">If this field isn't specified, the windowing operation is performed by using the time that each event arrived at the event hub.</span></span> <span data-ttu-id="13c3b-279">Läs mer i avsnittet ”ankomsttid vs programmet tid” i [referens för Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span><span class="sxs-lookup"><span data-stu-id="13c3b-279">Learn more in the "Arrival Time vs Application Time" section of [Stream Analytics Query Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).</span></span>

    <span data-ttu-id="13c3b-280">Den här frågan använder också en tidsstämpel för slutet av varje fönster med hjälp av den **System.Timestamp** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="13c3b-280">This query also accesses a timestamp for the end of each window by using the **System.Timestamp** property.</span></span>

5. <span data-ttu-id="13c3b-281">Klicka på **Test**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-281">Click **Test**.</span></span> <span data-ttu-id="13c3b-282">Frågan körs mot de data som du tog prov.</span><span class="sxs-lookup"><span data-stu-id="13c3b-282">The query runs against the data that you sampled.</span></span>
    
6. <span data-ttu-id="13c3b-283">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-283">Click **Save**.</span></span> <span data-ttu-id="13c3b-284">Frågan sparas som en del av Streaming Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="13c3b-284">This saves the query as part of the Streaming Analytics job.</span></span> <span data-ttu-id="13c3b-285">(Den inte spara exempeldata.)</span><span class="sxs-lookup"><span data-stu-id="13c3b-285">(It doesn't save the sample data.)</span></span>


## <a name="experiment-using-different-fields-from-the-stream"></a><span data-ttu-id="13c3b-286">Experimentera med olika fält från dataströmmen</span><span class="sxs-lookup"><span data-stu-id="13c3b-286">Experiment using different fields from the stream</span></span> 

<span data-ttu-id="13c3b-287">I följande tabell visas de fält som är en del av JSON strömmande data.</span><span class="sxs-lookup"><span data-stu-id="13c3b-287">The following table lists the fields that are part of the JSON streaming data.</span></span> <span data-ttu-id="13c3b-288">Du kan experimentera i frågeredigeraren.</span><span class="sxs-lookup"><span data-stu-id="13c3b-288">Feel free to experiment in the query editor.</span></span>

|<span data-ttu-id="13c3b-289">JSON-egenskap</span><span class="sxs-lookup"><span data-stu-id="13c3b-289">JSON property</span></span> | <span data-ttu-id="13c3b-290">Definition</span><span class="sxs-lookup"><span data-stu-id="13c3b-290">Definition</span></span>|
|--- | ---|
|<span data-ttu-id="13c3b-291">CreatedAt</span><span class="sxs-lookup"><span data-stu-id="13c3b-291">CreatedAt</span></span> | <span data-ttu-id="13c3b-292">Den tid som tweet skapades</span><span class="sxs-lookup"><span data-stu-id="13c3b-292">The time that the tweet was created</span></span>|
|<span data-ttu-id="13c3b-293">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="13c3b-293">Topic</span></span> | <span data-ttu-id="13c3b-294">Avsnittet som matchar det angivna nyckelordet</span><span class="sxs-lookup"><span data-stu-id="13c3b-294">The topic that matches the specified keyword</span></span>|
|<span data-ttu-id="13c3b-295">SentimentScore</span><span class="sxs-lookup"><span data-stu-id="13c3b-295">SentimentScore</span></span> | <span data-ttu-id="13c3b-296">Sentiment poäng från Sentiment140</span><span class="sxs-lookup"><span data-stu-id="13c3b-296">The sentiment score from Sentiment140</span></span>|
|<span data-ttu-id="13c3b-297">Skapa</span><span class="sxs-lookup"><span data-stu-id="13c3b-297">Author</span></span> | <span data-ttu-id="13c3b-298">Twitter-referensen som skickats av tweet</span><span class="sxs-lookup"><span data-stu-id="13c3b-298">The Twitter handle that sent the tweet</span></span>|
|<span data-ttu-id="13c3b-299">Text</span><span class="sxs-lookup"><span data-stu-id="13c3b-299">Text</span></span> | <span data-ttu-id="13c3b-300">Själva tweet</span><span class="sxs-lookup"><span data-stu-id="13c3b-300">The full body of the tweet</span></span>|


## <a name="create-an-output-sink"></a><span data-ttu-id="13c3b-301">Skapa utdatamottagare</span><span class="sxs-lookup"><span data-stu-id="13c3b-301">Create an output sink</span></span>

<span data-ttu-id="13c3b-302">Du har nu definierat en händelseström, en händelsehubb som indata för att mata in händelser och en fråga för att genomföra en omvandling över dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="13c3b-302">You have now defined an event stream, an event hub input to ingest events, and a query to perform a transformation over the stream.</span></span> <span data-ttu-id="13c3b-303">Det sista steget är att definiera utdatamottagare för jobbet.</span><span class="sxs-lookup"><span data-stu-id="13c3b-303">The last step is to define an output sink for the job.</span></span>  

<span data-ttu-id="13c3b-304">I den här självstudiekursen skriva aggregerade tweet händelser från jobbet frågan till Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="13c3b-304">In this tutorial, you write the aggregated tweet events from the job query to Azure Blob storage.</span></span>  <span data-ttu-id="13c3b-305">Du kan också push-resultaten till Azure SQL Database, Azure Table storage Event Hubs eller Power BI, beroende på ditt program behöver.</span><span class="sxs-lookup"><span data-stu-id="13c3b-305">You can also push your results to Azure SQL Database, Azure Table storage, Event Hubs, or Power BI, depending on your application needs.</span></span>

## <a name="specify-the-job-output"></a><span data-ttu-id="13c3b-306">Ange jobbutdata</span><span class="sxs-lookup"><span data-stu-id="13c3b-306">Specify the job output</span></span>

1. <span data-ttu-id="13c3b-307">I den **jobbet topologi** klickar du på den **utdata** rutan.</span><span class="sxs-lookup"><span data-stu-id="13c3b-307">In the **Job Topology** section, click the **Output** box.</span></span> 

2. <span data-ttu-id="13c3b-308">I den **utdata** bladet, klickar du på  **+ &nbsp;Lägg till** och fyll sedan i bladet med dessa värden:</span><span class="sxs-lookup"><span data-stu-id="13c3b-308">In the **Outputs** blade, click **+&nbsp;Add** and then fill out the blade with these values:</span></span>

    * <span data-ttu-id="13c3b-309">**Ett utdataalias**: Använd namnet `TwitterStream-Output`.</span><span class="sxs-lookup"><span data-stu-id="13c3b-309">**Output alias**: Use the name `TwitterStream-Output`.</span></span> 
    * <span data-ttu-id="13c3b-310">**Sink**: Välj **Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-310">**Sink**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="13c3b-311">**Importalternativ**: Välj **använda blob storage från aktuell prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-311">**Import options**: Select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="13c3b-312">**Lagringskontot**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-312">**Storage account**.</span></span> <span data-ttu-id="13c3b-313">Välj **skapa ett nytt lagringskonto.**</span><span class="sxs-lookup"><span data-stu-id="13c3b-313">Select **Create a new storage account.**</span></span>
    * <span data-ttu-id="13c3b-314">**Lagringskontot** (andra rutan).</span><span class="sxs-lookup"><span data-stu-id="13c3b-314">**Storage account** (second box).</span></span> <span data-ttu-id="13c3b-315">Ange `YOURNAMEsa`, där `YOURNAME` är ditt namn eller en annan unik sträng.</span><span class="sxs-lookup"><span data-stu-id="13c3b-315">Enter `YOURNAMEsa`, where `YOURNAME` is your name or another unique string.</span></span> <span data-ttu-id="13c3b-316">Namnet kan använda bara gemena bokstäver och siffror, och den måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="13c3b-316">The name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 
    * <span data-ttu-id="13c3b-317">**Behållaren**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-317">**Container**.</span></span> <span data-ttu-id="13c3b-318">Ange `socialtwitter`.</span><span class="sxs-lookup"><span data-stu-id="13c3b-318">Enter `socialtwitter`.</span></span>
    <span data-ttu-id="13c3b-319">Lagringskontonamn och behållarnamn används tillsammans för att ge en URI för blobblagring, så här:</span><span class="sxs-lookup"><span data-stu-id="13c3b-319">The storage account name and container name are used together to provide a URI for the blob storage, like this:</span></span> 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    ![”Nya utdata” bladet för Stream Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. <span data-ttu-id="13c3b-321">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-321">Click **Create**.</span></span> 

    <span data-ttu-id="13c3b-322">Azure skapar lagringskontot och skapar automatiskt en nyckel.</span><span class="sxs-lookup"><span data-stu-id="13c3b-322">Azure creates the storage account and generates a key automatically.</span></span> 

5. <span data-ttu-id="13c3b-323">Stäng den **utdata** bladet.</span><span class="sxs-lookup"><span data-stu-id="13c3b-323">Close the **Outputs** blade.</span></span> 


## <a name="start-the-job"></a><span data-ttu-id="13c3b-324">Starta jobbet</span><span class="sxs-lookup"><span data-stu-id="13c3b-324">Start the job</span></span>

<span data-ttu-id="13c3b-325">Jobbet indata-, fråge- och utdata har angetts.</span><span class="sxs-lookup"><span data-stu-id="13c3b-325">A job input, query, and output are specified.</span></span> <span data-ttu-id="13c3b-326">Du är redo att börja Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="13c3b-326">You are ready to start the Stream Analytics job.</span></span>

1. <span data-ttu-id="13c3b-327">Kontrollera att programmet TwitterWpfClient körs.</span><span class="sxs-lookup"><span data-stu-id="13c3b-327">Make sure that the TwitterWpfClient application is running.</span></span> 

2. <span data-ttu-id="13c3b-328">I jobb-bladet klickar du på **starta**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-328">In the job blade, click **Start**.</span></span>

    ![Starta Stream Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. <span data-ttu-id="13c3b-330">I den **startjobb** bladet för **jobbutdata starttid**väljer **nu** och klicka sedan på **starta**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-330">In the **Start job** blade, for **Job output start time**, select **Now** and then click **Start**.</span></span> 

    ![”Starta jobbet” bladet för Stream Analytics-jobbet](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    <span data-ttu-id="13c3b-332">Azure meddelar dig när jobbet har startat och i bladet jobb visas status som **kör**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-332">Azure notifies you when the job has started, and in the job blade, the status is displayed as **Running**.</span></span>

    ![Köra jobb](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a><span data-ttu-id="13c3b-334">Visa utdata för sentiment analys</span><span class="sxs-lookup"><span data-stu-id="13c3b-334">View output for sentiment analysis</span></span>

<span data-ttu-id="13c3b-335">När jobbet har startats och bearbetas i realtid Twitter-dataströmmen kan visa du utdata för sentiment analys.</span><span class="sxs-lookup"><span data-stu-id="13c3b-335">After your job has started running and is processing the real-time Twitter stream, you can view the output for sentiment analysis.</span></span>

<span data-ttu-id="13c3b-336">Du kan använda ett verktyg som [Azure Lagringsutforskaren](https://http://storageexplorer.com/) eller [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) att visa din jobbutdata i realtid.</span><span class="sxs-lookup"><span data-stu-id="13c3b-336">You can use a tool like [Azure Storage Explorer](https://http://storageexplorer.com/) or [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) to view your job output in real time.</span></span> <span data-ttu-id="13c3b-337">Härifrån kan du använda [Power BI](https://powerbi.com/) att utöka ditt program att inkludera en anpassad instrumentpanel som det visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="13c3b-337">From here, you can use [Power BI](https://powerbi.com/) to extend your application to include a customized dashboard like the one shown in the following screenshot:</span></span>

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-to-identify-trending-topics"></a><span data-ttu-id="13c3b-339">Skapa en annan fråga för att identifiera trender avsnitt</span><span class="sxs-lookup"><span data-stu-id="13c3b-339">Create another query to identify trending topics</span></span>

<span data-ttu-id="13c3b-340">En annan fråga som du kan använda för att förstå Twitter-sentiment baseras på en [glidande fönstret](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span><span class="sxs-lookup"><span data-stu-id="13c3b-340">Another query you can use to understand Twitter sentiment is based on a [Sliding Window](https://msdn.microsoft.com/library/azure/dn835051.aspx).</span></span> <span data-ttu-id="13c3b-341">För att identifiera trender avsnitt kan söka du efter avsnitt som går via ett tröskelvärde för nämns i en angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="13c3b-341">To identify trending topics, you look for topics that cross a threshold value for mentions in a specified amount of time.</span></span>

<span data-ttu-id="13c3b-342">Vid tillämpningen av den här kursen kan du söka efter avsnitt som nämns fler än 20 gånger i de senaste 5 sekunderna.</span><span class="sxs-lookup"><span data-stu-id="13c3b-342">For the purposes of this tutorial, you check for topics that are mentioned more than 20 times in the last 5 seconds.</span></span>

1. <span data-ttu-id="13c3b-343">I jobb-bladet klickar du på **stoppa** att stoppa jobbet.</span><span class="sxs-lookup"><span data-stu-id="13c3b-343">In the job blade, click **Stop** to stop the job.</span></span> 

2. <span data-ttu-id="13c3b-344">I den **jobbet topologi** klickar du på den **frågan** rutan.</span><span class="sxs-lookup"><span data-stu-id="13c3b-344">In the **Job Topology** section, click the **Query** box.</span></span> 

3. <span data-ttu-id="13c3b-345">Ändra frågan till följande:</span><span class="sxs-lookup"><span data-stu-id="13c3b-345">Change the query to the following:</span></span>

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. <span data-ttu-id="13c3b-346">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="13c3b-346">Click **Save**.</span></span>

5. <span data-ttu-id="13c3b-347">Kontrollera att programmet TwitterWpfClient körs.</span><span class="sxs-lookup"><span data-stu-id="13c3b-347">Make sure that the TwitterWpfClient application is running.</span></span> 

6. <span data-ttu-id="13c3b-348">Klicka på **starta** att starta om jobbet med den nya frågan.</span><span class="sxs-lookup"><span data-stu-id="13c3b-348">Click **Start** to restart the job using the new query.</span></span>


## <a name="get-support"></a><span data-ttu-id="13c3b-349">Få support</span><span class="sxs-lookup"><span data-stu-id="13c3b-349">Get support</span></span>
<span data-ttu-id="13c3b-350">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="13c3b-350">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="13c3b-351">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="13c3b-351">Next steps</span></span>
* [<span data-ttu-id="13c3b-352">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="13c3b-352">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="13c3b-353">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="13c3b-353">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="13c3b-354">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="13c3b-354">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="13c3b-355">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="13c3b-355">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="13c3b-356">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="13c3b-356">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
