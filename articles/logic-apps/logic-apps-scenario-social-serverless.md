---
title: "aaaScenario - skapa en kund insikter instrumentpanel med Azure serverlösa | Microsoft Docs"
description: "Ett exempel på hur du kan skapa en instrumentpanel toomanage kund feedback, sociala data och mycket mer med Azure Logikappar och Azure Functions."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: db175e895e37aa795a9c34bf4d65566bf68f8c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a><span data-ttu-id="6da5d-103">Skapa en realtid kunden insikter instrumentpanel med Azure Logikappar och Azure Functions</span><span class="sxs-lookup"><span data-stu-id="6da5d-103">Create a real-time customer insights dashboard with Azure Logic Apps and Azure Functions</span></span>

<span data-ttu-id="6da5d-104">Azure-serverlösa verktyg tillhandahålla kraftfulla funktioner tooquickly bygg- och program i hello molnet utan att behöva toothink om infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="6da5d-104">Azure Serverless tools provide powerful capabilities tooquickly build and host applications in hello cloud, without having toothink about infrastructure.</span></span>  <span data-ttu-id="6da5d-105">I det här scenariot ska vi skapa en instrumentpanel tootrigger på kundfeedback, analysera feedback med machine learning och publicera insikter en källa som Power BI eller Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="6da5d-105">In this scenario, we will create a dashboard tootrigger on customer feedback, analyze feedback with machine learning, and publish insights a source like Power BI or Azure Data Lake.</span></span>

## <a name="overview-of-hello-scenario-and-tools-used"></a><span data-ttu-id="6da5d-106">Översikt över scenariot hello och verktyg som används</span><span class="sxs-lookup"><span data-stu-id="6da5d-106">Overview of hello scenario and tools used</span></span>

<span data-ttu-id="6da5d-107">I ordning tooimplement den här lösningen, vi använder hello två viktiga komponenter av serverlösa appar i Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) och [Azure Logikappar](https://azure.microsoft.com/services/logic-apps/).</span><span class="sxs-lookup"><span data-stu-id="6da5d-107">In order tooimplement this solution, we will leverage hello two key components of serverless apps in Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).</span></span>

<span data-ttu-id="6da5d-108">Logic Apps är en serverlösa arbetsflödesmotorn i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="6da5d-108">Logic Apps is a serverless workflow engine in hello cloud.</span></span>  <span data-ttu-id="6da5d-109">Det ger orchestration över serverlösa komponenter och även ansluter tooover 100 tjänster och API: er.</span><span class="sxs-lookup"><span data-stu-id="6da5d-109">It provides orchestration across serverless components, and also connects tooover 100 services and APIs.</span></span>  <span data-ttu-id="6da5d-110">I det här scenariot skapar vi en logik app tootrigger kommentarer från kunder.</span><span class="sxs-lookup"><span data-stu-id="6da5d-110">For this scenario, we will create a logic app tootrigger on feedback from customers.</span></span>  <span data-ttu-id="6da5d-111">Hello kopplingar som kan underlätta korsreagerande toocustomer feedback bland Outlook.com, Office 365, undersökning apa, Twitter och en HTTP-begäran [från ett webbformulär](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span><span class="sxs-lookup"><span data-stu-id="6da5d-111">Some of hello connectors that can assist in reacting toocustomer feedback include Outlook.com, Office 365, Survey Monkey, Twitter, and an HTTP Request [from a web form](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).</span></span>  <span data-ttu-id="6da5d-112">Hello arbetsflödet nedan, kommer vi övervaka en hashtaggar på Twitter.</span><span class="sxs-lookup"><span data-stu-id="6da5d-112">For hello workflow below, we will monitor a hashtag on Twitter.</span></span>

<span data-ttu-id="6da5d-113">Funktioner ger serverlösa beräkning i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="6da5d-113">Functions provide serverless compute in hello cloud.</span></span>  <span data-ttu-id="6da5d-114">I det här scenariot ska vi använda Azure Functions tooflag tweets från kunder baserat på ett antal fördefinierade nyckelord.</span><span class="sxs-lookup"><span data-stu-id="6da5d-114">In this scenario, we will use Azure Functions tooflag tweets from customers based on a series of pre-defined key words.</span></span>

<span data-ttu-id="6da5d-115">hello hela lösningen kan vara [skapa i Visual Studio](logic-apps-deploy-from-vs.md) och [distribueras som en del av en resursmall för](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="6da5d-115">hello entire solution can be [build in Visual Studio](logic-apps-deploy-from-vs.md) and [deployed as part of a resource template](logic-apps-create-deploy-template.md).</span></span>  <span data-ttu-id="6da5d-116">Det finns också videogenomgång av hello scenariot [på Channel 9](http://aka.ms/logicappsdemo).</span><span class="sxs-lookup"><span data-stu-id="6da5d-116">There is also video walkthrough of hello scenario [on Channel 9](http://aka.ms/logicappsdemo).</span></span>

## <a name="build-hello-logic-app-tootrigger-on-customer-data"></a><span data-ttu-id="6da5d-117">Skapa hello logik app tootrigger på kundinformation</span><span class="sxs-lookup"><span data-stu-id="6da5d-117">Build hello logic app tootrigger on customer data</span></span>

<span data-ttu-id="6da5d-118">Efter [att skapa en logikapp](logic-apps-create-a-logic-app.md) i Visual Studio eller hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="6da5d-118">After [creating a logic app](logic-apps-create-a-logic-app.md) in Visual Studio or hello Azure portal:</span></span>

1. <span data-ttu-id="6da5d-119">Lägga till en utlösare för **på nya Tweets** från Twitter</span><span class="sxs-lookup"><span data-stu-id="6da5d-119">Add a trigger for **On New Tweets** from Twitter</span></span>
2. <span data-ttu-id="6da5d-120">Konfigurera hello utlösaren toolisten tootweets på ett nyckelord eller en hashtaggar.</span><span class="sxs-lookup"><span data-stu-id="6da5d-120">Configure hello trigger toolisten tootweets on a keyword or hashtag.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6da5d-121">hello upprepning egenskapen hello utlösaren avgör hur ofta hello logikapp söker efter nya objekt på avsökning-baserade utlösare</span><span class="sxs-lookup"><span data-stu-id="6da5d-121">hello recurrence property on hello trigger will determine how frequently hello logic app checks for new items on polling-based triggers</span></span>

   ![Exempel på Twitter-utlösare][1]

<span data-ttu-id="6da5d-123">Den här appen används nu på alla nya tweets.</span><span class="sxs-lookup"><span data-stu-id="6da5d-123">This app will now fire on all new tweets.</span></span>  <span data-ttu-id="6da5d-124">Vi kan sedan ta tweet data och lära dig mer hello sentiment uttryckt.</span><span class="sxs-lookup"><span data-stu-id="6da5d-124">We can then take that tweet data and understand more of hello sentiment expressed.</span></span>  <span data-ttu-id="6da5d-125">Detta vi använder hello [Azure kognitiva Service](https://azure.microsoft.com/services/cognitive-services/) toodetect sentiment text.</span><span class="sxs-lookup"><span data-stu-id="6da5d-125">For this we use hello [Azure Cognitive Service](https://azure.microsoft.com/services/cognitive-services/) toodetect sentiment of text.</span></span>

1. <span data-ttu-id="6da5d-126">Klicka på **nytt steg**</span><span class="sxs-lookup"><span data-stu-id="6da5d-126">Click **New Step**</span></span>
1. <span data-ttu-id="6da5d-127">Välj eller söka efter Hej **textanalys** koppling</span><span class="sxs-lookup"><span data-stu-id="6da5d-127">Select or search for hello **Text Analytics** connector</span></span>
1. <span data-ttu-id="6da5d-128">Välj hello **identifiera Sentiment** igen</span><span class="sxs-lookup"><span data-stu-id="6da5d-128">Select hello **Detect Sentiment** operation</span></span>
1. <span data-ttu-id="6da5d-129">Om du uppmanas ange en giltig nyckel kognitiva tjänster för hello Text Analytics-tjänsten</span><span class="sxs-lookup"><span data-stu-id="6da5d-129">If prompted, provide a valid Cognitive Services key for hello Text Analytics service</span></span>
1. <span data-ttu-id="6da5d-130">Lägg till hello **Tweet Text** som hello text tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="6da5d-130">Add hello **Tweet Text** as hello text tooanalyze.</span></span>

<span data-ttu-id="6da5d-131">Nu när vi har hello tweet data och insikter på hello tweet kan ett antal andra kopplingar vara relevanta:</span><span class="sxs-lookup"><span data-stu-id="6da5d-131">Now that we have hello tweet data, and insights on hello tweet, a number of other connectors may be relevant:</span></span>
* <span data-ttu-id="6da5d-132">Power BI - Lägg till rader tooStreaming Dataset: Visa tweets realtid på en Power BI-instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="6da5d-132">Power BI - Add Rows tooStreaming Dataset: View tweets real time on a Power BI dashboard.</span></span>
* <span data-ttu-id="6da5d-133">Azure Data Lake - tilläggsfilen: lägga till kunden data tooan Azure Data Lake dataset tooinclude i analytics-jobb.</span><span class="sxs-lookup"><span data-stu-id="6da5d-133">Azure Data Lake - Append file: Add customer data tooan Azure Data Lake dataset tooinclude in analytics jobs.</span></span>
* <span data-ttu-id="6da5d-134">SQL - lägga till rader: lagra data i en databas för senare hämtning.</span><span class="sxs-lookup"><span data-stu-id="6da5d-134">SQL - Add rows: Store data in a database for later retrieval.</span></span>
* <span data-ttu-id="6da5d-135">Slack - skicka meddelande: varning en slack-kanal på negativ feedback som kräver åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6da5d-135">Slack - Send message: Alert a slack channel on negative feedback that requires actions.</span></span>

<span data-ttu-id="6da5d-136">En Azure-funktion kan också vara används toodo mer anpassad compute ovanpå hello data.</span><span class="sxs-lookup"><span data-stu-id="6da5d-136">An Azure Function can also be used toodo more custom compute on top of hello data.</span></span>

## <a name="enriching-hello-data-with-an-azure-function"></a><span data-ttu-id="6da5d-137">Utöka hello data med en Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="6da5d-137">Enriching hello data with an Azure Function</span></span>

<span data-ttu-id="6da5d-138">Innan vi kan skapa en funktion måste toohave en funktionsapp i vår Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6da5d-138">Before we can create a function, we need toohave a function app in our Azure subscription.</span></span>  <span data-ttu-id="6da5d-139">Information om hur du skapar en Azure-funktion i hello portal kan [finns här](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="6da5d-139">Details on creating an Azure Function in hello portal can [be found here](../azure-functions/functions-create-first-azure-function-azure-portal.md)</span></span>

<span data-ttu-id="6da5d-140">För en funktion toobe anropas direkt från en logikapp, den behöver toohave HTTP aktivera bindning.</span><span class="sxs-lookup"><span data-stu-id="6da5d-140">For a function toobe called directly from a logic app, it needs toohave an HTTP trigger binding.</span></span>  <span data-ttu-id="6da5d-141">Vi rekommenderar att du använder hello **HttpTrigger** mall.</span><span class="sxs-lookup"><span data-stu-id="6da5d-141">We recommend using hello **HttpTrigger** template.</span></span>

<span data-ttu-id="6da5d-142">I det här scenariot är hello begärandetexten för hello Azure-funktion hello tweet text.</span><span class="sxs-lookup"><span data-stu-id="6da5d-142">In this scenario, hello request body of hello Azure Function would be hello tweet text.</span></span>  <span data-ttu-id="6da5d-143">I Funktionskoden hello, bara definiera logik på om hello tweet texten innehåller ett nyckelord eller en fras.</span><span class="sxs-lookup"><span data-stu-id="6da5d-143">In hello function code, simply define logic on if hello tweet text contains a key word or phrase.</span></span>  <span data-ttu-id="6da5d-144">själva hello-funktionen kan hållas enligt enkla eller avancerade som behövs för hello scenario.</span><span class="sxs-lookup"><span data-stu-id="6da5d-144">hello function itself could be kept as simple or complex as needed for hello scenario.</span></span>

<span data-ttu-id="6da5d-145">Hello slutet av funktionen hello, bara returnera ett svar toohello logikapp med data.</span><span class="sxs-lookup"><span data-stu-id="6da5d-145">At hello end of hello function, simply return a response toohello logic app with some data.</span></span>  <span data-ttu-id="6da5d-146">Detta kan bero på ett enkelt booleskt värde (t.ex. `containsKeyword`), eller ett komplext objekt.</span><span class="sxs-lookup"><span data-stu-id="6da5d-146">This could be a simple boolean value (e.g. `containsKeyword`), or a complex object.</span></span>

![Steg för konfigurerade Azure-funktion][2]

> [!TIP]
> <span data-ttu-id="6da5d-148">Vid åtkomst till ett komplext svar från en funktion i en logikapp instruktionen hello parsa JSON.</span><span class="sxs-lookup"><span data-stu-id="6da5d-148">When accessing a complex response from a function in a logic app, use hello Parse JSON action.</span></span>

<span data-ttu-id="6da5d-149">När du har sparat hello funktionen går att lägga till hello logikapp skapade ovan.</span><span class="sxs-lookup"><span data-stu-id="6da5d-149">Once hello function is saved, it can be added into hello logic app created above.</span></span>  <span data-ttu-id="6da5d-150">I hello logikapp:</span><span class="sxs-lookup"><span data-stu-id="6da5d-150">In hello logic app:</span></span>

1. <span data-ttu-id="6da5d-151">Klicka på tooadd en **nytt steg**</span><span class="sxs-lookup"><span data-stu-id="6da5d-151">Click tooadd a **New Step**</span></span>
1. <span data-ttu-id="6da5d-152">Välj hello **Azure Functions** koppling</span><span class="sxs-lookup"><span data-stu-id="6da5d-152">Select hello **Azure Functions** connector</span></span>
1. <span data-ttu-id="6da5d-153">Välj toochoose en befintlig funktion och bläddra toohello funktionen skapas</span><span class="sxs-lookup"><span data-stu-id="6da5d-153">Select toochoose an existing function, and browse toohello function created</span></span>
1. <span data-ttu-id="6da5d-154">Skicka i hello **Tweet Text** för hello **brödtext i begäran**</span><span class="sxs-lookup"><span data-stu-id="6da5d-154">Send in hello **Tweet Text** for hello **Request Body**</span></span>

## <a name="running-and-monitoring-hello-solution"></a><span data-ttu-id="6da5d-155">Kör och övervakningslösning hello</span><span class="sxs-lookup"><span data-stu-id="6da5d-155">Running and monitoring hello solution</span></span>

<span data-ttu-id="6da5d-156">En av hello fördelarna med redigering serverlösa orkestreringarna i Logic Apps är hello omfattande felsöknings- och övervakningsfunktionerna.</span><span class="sxs-lookup"><span data-stu-id="6da5d-156">One of hello benefits of authoring serverless orchestrations in Logic Apps is hello rich debug and monitoring capabilities.</span></span>  <span data-ttu-id="6da5d-157">Alla kör (aktuella eller historiska) kan visas från Visual Studio och hello Azure-portalen eller via hello REST-API och SDK.</span><span class="sxs-lookup"><span data-stu-id="6da5d-157">Any run (current or historic) can be viewed from within Visual Studio, hello Azure portal, or via hello REST API and SDKs.</span></span>

<span data-ttu-id="6da5d-158">Ett av hello enklaste sätt tootest en logikapp använder hello **kör** knappen i hello designer.</span><span class="sxs-lookup"><span data-stu-id="6da5d-158">One of hello easiest ways tootest a logic app is using hello **Run** button in hello designer.</span></span>  <span data-ttu-id="6da5d-159">Klicka på **kör** fortsätter toopoll hello utlösaren 5 sekunder tills en händelse identifieras, och ger en live-vyn som hello kör fortskrider.</span><span class="sxs-lookup"><span data-stu-id="6da5d-159">Clicking **Run** will continue toopoll hello trigger every 5 seconds until an event is detected, and give a live view as hello run progresses.</span></span>

<span data-ttu-id="6da5d-160">Tidigare kör Historik kan visas på hello översikt blad i hello Azure-portalen, eller via hello Visual Studio Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="6da5d-160">Previous run histories can be viewed on hello Overview blade in hello Azure portal, or using hello Visual Studio Cloud Explorer.</span></span>

## <a name="creating-a-deployment-template-for-automated-deployments"></a><span data-ttu-id="6da5d-161">Skapa en Distributionsmall för automatiserad distribution</span><span class="sxs-lookup"><span data-stu-id="6da5d-161">Creating a deployment template for automated deployments</span></span>

<span data-ttu-id="6da5d-162">När en lösning har utvecklats, kan de hämtats och distribueras via en Azure-distribution mallen tooany Azure-region i hello world.</span><span class="sxs-lookup"><span data-stu-id="6da5d-162">Once a solution has been developed, it can be captured and deployed via an Azure deployment template tooany Azure region in hello world.</span></span>  <span data-ttu-id="6da5d-163">Detta är användbart för båda ändra parametrarna för olika versioner av det här arbetsflödet, men också för att integrera den här lösningen i en version och utgåva pipeline.</span><span class="sxs-lookup"><span data-stu-id="6da5d-163">This is useful for both modifying parameters for different versions of this workflow, but also for integrating this solution in a build and release pipeline.</span></span>  <span data-ttu-id="6da5d-164">Information om hur du skapar en Distributionsmall finns [i den här artikeln](logic-apps-create-deploy-template.md).</span><span class="sxs-lookup"><span data-stu-id="6da5d-164">Details on creating a deployment template can be found [in this article](logic-apps-create-deploy-template.md).</span></span>

<span data-ttu-id="6da5d-165">Azure Functions kan också införlivas i mallen för distribution av hello - så hello hela lösningen med alla beroenden kan hanteras som en mall.</span><span class="sxs-lookup"><span data-stu-id="6da5d-165">Azure Functions can also be incorporated in hello deployment template - so hello entire solution with all dependencies can be managed as a single template.</span></span>  <span data-ttu-id="6da5d-166">Ett exempel på en mall för distribution av funktionen finns i hello [Azure quickstart mallen databasen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span><span class="sxs-lookup"><span data-stu-id="6da5d-166">An example of a function deployment template can be found in hello [Azure quickstart template repository](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6da5d-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6da5d-167">Next steps</span></span>

* [<span data-ttu-id="6da5d-168">Visa andra exempel och scenarier för Logikappar i Azure</span><span class="sxs-lookup"><span data-stu-id="6da5d-168">See other examples and scenarios for Azure Logic Apps</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="6da5d-169">Titta på en videogenomgång om hur du skapar den här lösningen slutpunkt till slutpunkt</span><span class="sxs-lookup"><span data-stu-id="6da5d-169">Watch a video walkthrough on creating this solution end-to-end</span></span>](http://aka.ms/logicappsdemo)
* [<span data-ttu-id="6da5d-170">Lär dig hur toohandle och catch-undantag inom en logikapp</span><span class="sxs-lookup"><span data-stu-id="6da5d-170">Learn how toohandle and catch exceptions within a logic app</span></span>](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png