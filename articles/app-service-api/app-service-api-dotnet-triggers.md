---
title: "Apptjänst API app utlösare | Microsoft Docs"
description: "Implementera utlösare i en API-App i Azure App Service"
services: logic-apps
documentationcenter: .net
author: guangyang
manager: erikre
editor: jimbe
ms.assetid: 493c3703-786d-4434-9dca-8f77744b2f5d
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: rachelap
ms.openlocfilehash: 3ddfb142e7f1a47e2a8564387da785acf36fa61f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-api-app-triggers"></a><span data-ttu-id="13aea-103">Utlösare för API Apps i Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="13aea-103">Azure App Service API app triggers</span></span>
> [!NOTE]
> <span data-ttu-id="13aea-104">Den här versionen av artikeln gäller för schemaversionen för API apps 2014-12-01-preview.</span><span class="sxs-lookup"><span data-stu-id="13aea-104">This version of the article applies to API apps 2014-12-01-preview schema version.</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="13aea-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="13aea-105">Overview</span></span>
<span data-ttu-id="13aea-106">Den här artikeln förklarar hur du implementerar utlösare för API Apps och använda dem från en logikapp.</span><span class="sxs-lookup"><span data-stu-id="13aea-106">This article explains how to implement API app triggers and consume them from a Logic app.</span></span>

<span data-ttu-id="13aea-107">Alla kodfragment i det här avsnittet kopieras från den [FileWatcher API-App kodexemplet](http://go.microsoft.com/fwlink/?LinkId=534802).</span><span class="sxs-lookup"><span data-stu-id="13aea-107">All of the code snippets in this topic are copied from the [FileWatcher API App code sample](http://go.microsoft.com/fwlink/?LinkId=534802).</span></span>

<span data-ttu-id="13aea-108">Observera att du behöver hämta följande nuget-paket för koden i den här artikeln för att skapa och köra: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span><span class="sxs-lookup"><span data-stu-id="13aea-108">Note that you'll need to download the following nuget package for the code in this article to build and run: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span></span>

## <a name="what-are-api-app-triggers"></a><span data-ttu-id="13aea-109">Vad är utlösare för API Apps?</span><span class="sxs-lookup"><span data-stu-id="13aea-109">What are API app triggers?</span></span>
<span data-ttu-id="13aea-110">Det är ett vanligt scenario för en API-app att utlösa en händelse så att klienter på API-appen kan vidta lämplig åtgärd som svar på händelsen.</span><span class="sxs-lookup"><span data-stu-id="13aea-110">It's a common scenario for an API app to fire an event so that clients of the API app can take the appropriate action in response to the event.</span></span> <span data-ttu-id="13aea-111">Mekanismen för REST API-baserade som har stöd för det här scenariot kallas för en utlösare för API-app.</span><span class="sxs-lookup"><span data-stu-id="13aea-111">The REST API based mechanism that supports this scenario is called an API app trigger.</span></span>

<span data-ttu-id="13aea-112">Till exempel anta att din klientkod använder den [Twitter-anslutningen API-app](../connectors/connectors-create-api-twitter.md) och din kod behöver utföra en åtgärd baserat på nya tweets med specifika ord.</span><span class="sxs-lookup"><span data-stu-id="13aea-112">For example, let's say your client code is using the [Twitter Connector API app](../connectors/connectors-create-api-twitter.md) and your code needs to perform an action based on new tweets that contain specific words.</span></span> <span data-ttu-id="13aea-113">I det här fallet kan du ställa in en avsökning eller push-utlösare att underlätta detta behov.</span><span class="sxs-lookup"><span data-stu-id="13aea-113">In this case, you might set up a poll or push trigger to facilitate this need.</span></span>

## <a name="poll-trigger-versus-push-trigger"></a><span data-ttu-id="13aea-114">Avsökningen utlösaren jämfört med push-utlösare</span><span class="sxs-lookup"><span data-stu-id="13aea-114">Poll trigger versus push trigger</span></span>
<span data-ttu-id="13aea-115">För närvarande stöds två typer av utlösare:</span><span class="sxs-lookup"><span data-stu-id="13aea-115">Currently, two types of triggers are supported:</span></span>

* <span data-ttu-id="13aea-116">Avsökningen utlösaren - klienten ska avsöka API-app för meddelanden om en händelse med sagts upp</span><span class="sxs-lookup"><span data-stu-id="13aea-116">Poll trigger - Client polls the API app for notification of an event having been fired</span></span>
* <span data-ttu-id="13aea-117">Push-utlösare - klienten meddelas av API-app när en händelse utlöses</span><span class="sxs-lookup"><span data-stu-id="13aea-117">Push trigger - Client is notified by the API app when an event fires</span></span>

### <a name="poll-trigger"></a><span data-ttu-id="13aea-118">Avsökningen utlösare</span><span class="sxs-lookup"><span data-stu-id="13aea-118">Poll trigger</span></span>
<span data-ttu-id="13aea-119">En avsökning utlösare implementeras som en vanlig REST-API och förväntar sig klienter (till exempel en logikapp) ska avsöka för att få meddelandet.</span><span class="sxs-lookup"><span data-stu-id="13aea-119">A poll trigger is implemented as a regular REST API and expects its clients (such as a Logic app) to poll it in order to get notification.</span></span> <span data-ttu-id="13aea-120">Medan klienten kan upprätthålla tillstånd, är utlösarens omröstning statslösa.</span><span class="sxs-lookup"><span data-stu-id="13aea-120">While the client may maintain state, the poll trigger itself is stateless.</span></span>

<span data-ttu-id="13aea-121">Följande information om begäran och svar paket illustrera vissa viktiga aspekter av omröstningen utlösaren kontrakt:</span><span class="sxs-lookup"><span data-stu-id="13aea-121">The following information regarding the request and response packets illustrate some key aspects of the poll trigger contract:</span></span>

* <span data-ttu-id="13aea-122">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="13aea-122">Request</span></span>
  * <span data-ttu-id="13aea-123">HTTP-metod: hämta</span><span class="sxs-lookup"><span data-stu-id="13aea-123">HTTP method: GET</span></span>
  * <span data-ttu-id="13aea-124">Parametrar</span><span class="sxs-lookup"><span data-stu-id="13aea-124">Parameters</span></span>
    * <span data-ttu-id="13aea-125">triggerState - den här valfria parametern klienterna ange deras tillstånd så att utlösaren avsökning korrekt avgöra om du vill returnera meddelande baserat på det angivna tillståndet.</span><span class="sxs-lookup"><span data-stu-id="13aea-125">triggerState - This optional parameter allows clients to specify their state so that the poll trigger can properly decide whether to return notification or not based on the specified state.</span></span>
    * <span data-ttu-id="13aea-126">API-specifika parametrar</span><span class="sxs-lookup"><span data-stu-id="13aea-126">API-specific parameters</span></span>
* <span data-ttu-id="13aea-127">Svar</span><span class="sxs-lookup"><span data-stu-id="13aea-127">Response</span></span>
  * <span data-ttu-id="13aea-128">Statuskoden **200** - begäran är giltig och att det finns ett meddelande från utlösaren.</span><span class="sxs-lookup"><span data-stu-id="13aea-128">Status code **200** - Request is valid and there is a notification from the trigger.</span></span> <span data-ttu-id="13aea-129">Innehållet i meddelandet kommer att brödtext för svar.</span><span class="sxs-lookup"><span data-stu-id="13aea-129">The content of the notification will be the response body.</span></span> <span data-ttu-id="13aea-130">En ”försök igen efter”-huvudet i svaret anger att ytterligare meddelandedata måste hämtas med ett efterföljande begäran-anrop.</span><span class="sxs-lookup"><span data-stu-id="13aea-130">A "Retry-After" header in the response indicates that additional notification data must be retrieved with a subsequent request call.</span></span>
  * <span data-ttu-id="13aea-131">Statuskoden **202** - begäran är giltig, men det finns inget nytt meddelande från utlösaren.</span><span class="sxs-lookup"><span data-stu-id="13aea-131">Status code **202** - Request is valid, but there is no new notification from the trigger.</span></span>
  * <span data-ttu-id="13aea-132">Statuskoden **4xx** -begäran är inte giltig.</span><span class="sxs-lookup"><span data-stu-id="13aea-132">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="13aea-133">Klienten bör inte försöka.</span><span class="sxs-lookup"><span data-stu-id="13aea-133">The client should not retry the request.</span></span>
  * <span data-ttu-id="13aea-134">Statuskoden **5xx** -förfrågan resulterade i ett internt serverfel och/eller ett tillfälligt problem.</span><span class="sxs-lookup"><span data-stu-id="13aea-134">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="13aea-135">Klienten bör försöka.</span><span class="sxs-lookup"><span data-stu-id="13aea-135">The client should retry the request.</span></span>

<span data-ttu-id="13aea-136">Följande kodavsnitt är ett exempel på hur du implementerar en omröstning utlösare.</span><span class="sxs-lookup"><span data-stu-id="13aea-136">The following code snippet is an example of how to implement a poll trigger.</span></span>

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

<span data-ttu-id="13aea-137">Följ dessa steg om du vill testa den här avsökningen utlösaren:</span><span class="sxs-lookup"><span data-stu-id="13aea-137">To test this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="13aea-138">Distribuera API-App med en inställning för autentisering av **offentliga anonym**.</span><span class="sxs-lookup"><span data-stu-id="13aea-138">Deploy the API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="13aea-139">Anropa den **touch** åtgärden touch en fil.</span><span class="sxs-lookup"><span data-stu-id="13aea-139">Call the **touch** operation to touch a file.</span></span> <span data-ttu-id="13aea-140">Följande bild visar ett exempel på begäran via Postman.</span><span class="sxs-lookup"><span data-stu-id="13aea-140">The following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="13aea-141">![Anropa Touch åtgärden via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="13aea-141">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
3. <span data-ttu-id="13aea-142">Anropa avsökning utlösaren med den **triggerState** parametern inställd på en tidsstämpel innan steg #2.</span><span class="sxs-lookup"><span data-stu-id="13aea-142">Call the poll trigger with the **triggerState** parameter set to a time stamp prior to Step #2.</span></span> <span data-ttu-id="13aea-143">Följande bild visar exempelbegäran via Postman.</span><span class="sxs-lookup"><span data-stu-id="13aea-143">The following image shows the sample request via Postman.</span></span>
   <span data-ttu-id="13aea-144">![Anropa utlösare avsökning via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="13aea-144">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span></span>

### <a name="push-trigger"></a><span data-ttu-id="13aea-145">Push-utlösare</span><span class="sxs-lookup"><span data-stu-id="13aea-145">Push trigger</span></span>
<span data-ttu-id="13aea-146">En push-utlösare implementeras som en vanlig REST-API som skickar meddelanden till klienter som har registrerat som ska meddelas när specifika händelser eller.</span><span class="sxs-lookup"><span data-stu-id="13aea-146">A push trigger is implemented as a regular REST API that pushes notifications to clients who have registered to be notified when specific events fire.</span></span>

<span data-ttu-id="13aea-147">Följande information om begäran och svar paket illustrera vissa viktiga aspekter av kontraktet för push-utlösare.</span><span class="sxs-lookup"><span data-stu-id="13aea-147">The following information regarding the request and response packets illustrate some key aspects of the push trigger contract.</span></span>

* <span data-ttu-id="13aea-148">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="13aea-148">Request</span></span>
  * <span data-ttu-id="13aea-149">HTTP-metod: PLACERA</span><span class="sxs-lookup"><span data-stu-id="13aea-149">HTTP method: PUT</span></span>
  * <span data-ttu-id="13aea-150">Parametrar</span><span class="sxs-lookup"><span data-stu-id="13aea-150">Parameters</span></span>
    * <span data-ttu-id="13aea-151">Utlösarens ID: krävs – täckande sträng (till exempel ett GUID) som representerar registreringen av push-utlösare.</span><span class="sxs-lookup"><span data-stu-id="13aea-151">triggerId: required - Opaque string (such as a GUID) that represents the registration of a push trigger.</span></span>
    * <span data-ttu-id="13aea-152">callbackUrl: krävs - URL för återanropet ska anropa när händelsen utlöses.</span><span class="sxs-lookup"><span data-stu-id="13aea-152">callbackUrl: required - URL of the callback to invoke when the event fires.</span></span> <span data-ttu-id="13aea-153">Anropet är en enkel HTTP POST-anrop.</span><span class="sxs-lookup"><span data-stu-id="13aea-153">The invocation is a simple POST HTTP call.</span></span>
    * <span data-ttu-id="13aea-154">API-specifika parametrar</span><span class="sxs-lookup"><span data-stu-id="13aea-154">API-specific parameters</span></span>
* <span data-ttu-id="13aea-155">Svar</span><span class="sxs-lookup"><span data-stu-id="13aea-155">Response</span></span>
  * <span data-ttu-id="13aea-156">Statuskoden **200** -begäran om att registrera klienten lyckas.</span><span class="sxs-lookup"><span data-stu-id="13aea-156">Status code **200** - Request to register client successful.</span></span>
  * <span data-ttu-id="13aea-157">Statuskoden **4xx** -begäran är inte giltig.</span><span class="sxs-lookup"><span data-stu-id="13aea-157">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="13aea-158">Klienten bör inte försöka.</span><span class="sxs-lookup"><span data-stu-id="13aea-158">The client should not retry the request.</span></span>
  * <span data-ttu-id="13aea-159">Statuskoden **5xx** -förfrågan resulterade i ett internt serverfel och/eller ett tillfälligt problem.</span><span class="sxs-lookup"><span data-stu-id="13aea-159">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="13aea-160">Klienten bör försöka.</span><span class="sxs-lookup"><span data-stu-id="13aea-160">The client should retry the request.</span></span>
* <span data-ttu-id="13aea-161">Motringning</span><span class="sxs-lookup"><span data-stu-id="13aea-161">Callback</span></span>
  * <span data-ttu-id="13aea-162">HTTP-metod: POST</span><span class="sxs-lookup"><span data-stu-id="13aea-162">HTTP method: POST</span></span>
  * <span data-ttu-id="13aea-163">Begäran: meddelandeinnehåll.</span><span class="sxs-lookup"><span data-stu-id="13aea-163">Request body: Notification content.</span></span>

<span data-ttu-id="13aea-164">Följande kodavsnitt är ett exempel på hur du implementerar en push-utlösare:</span><span class="sxs-lookup"><span data-stu-id="13aea-164">The following code snippet is an example of how to implement a push trigger:</span></span>

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

<span data-ttu-id="13aea-165">Följ dessa steg om du vill testa den här avsökningen utlösaren:</span><span class="sxs-lookup"><span data-stu-id="13aea-165">To test this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="13aea-166">Distribuera API-App med en inställning för autentisering av **offentliga anonym**.</span><span class="sxs-lookup"><span data-stu-id="13aea-166">Deploy the API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="13aea-167">Bläddra till [http://requestb.in/](http://requestb.in/) att skapa en RequestBin som fungerar som återanrop URL: en.</span><span class="sxs-lookup"><span data-stu-id="13aea-167">Browse to [http://requestb.in/](http://requestb.in/) to create a RequestBin which will serve as your callback URL.</span></span>
3. <span data-ttu-id="13aea-168">Anropa push-utlösare med ett GUID som **utlösarens ID** och RequestBin-URL: en som **callbackUrl**.</span><span class="sxs-lookup"><span data-stu-id="13aea-168">Call the push trigger with a GUID as **triggerId** and the RequestBin URL as **callbackUrl**.</span></span>
   <span data-ttu-id="13aea-169">![Anropa Push-utlösare via Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="13aea-169">![Call Push Trigger via Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span></span>
4. <span data-ttu-id="13aea-170">Anropa den **touch** åtgärden touch en fil.</span><span class="sxs-lookup"><span data-stu-id="13aea-170">Call the **touch** operation to touch a file.</span></span> <span data-ttu-id="13aea-171">Följande bild visar ett exempel på begäran via Postman.</span><span class="sxs-lookup"><span data-stu-id="13aea-171">The following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="13aea-172">![Anropa Touch åtgärden via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="13aea-172">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
5. <span data-ttu-id="13aea-173">Kontrollera RequestBin för att bekräfta att push-utlösare återanropet anropas med egenskapen utdata.</span><span class="sxs-lookup"><span data-stu-id="13aea-173">Check the RequestBin to confirm that the push trigger callback is invoked with property output.</span></span>
   <span data-ttu-id="13aea-174">![Anropa utlösare avsökning via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span><span class="sxs-lookup"><span data-stu-id="13aea-174">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span></span>

### <a name="describe-triggers-in-api-definition"></a><span data-ttu-id="13aea-175">Beskriv utlösare i API-definition</span><span class="sxs-lookup"><span data-stu-id="13aea-175">Describe triggers in API definition</span></span>
<span data-ttu-id="13aea-176">När du implementerar utlösare och distribuera din API-app till Azure, navigera till den **API-Definition** bladet i Azure preview portal och du ser att identifieras automatiskt utlösare i Gränssnittet som drivs av Swagger 2.0 API-definition av API-app.</span><span class="sxs-lookup"><span data-stu-id="13aea-176">After implementing the triggers and deploying your API app to Azure, navigate to the **API Definition** blade in the Azure preview portal and you'll see that triggers are automatically recognized in the UI, which is driven by the Swagger 2.0 API definition of the API app.</span></span>

![Bladet för API-Definition](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

<span data-ttu-id="13aea-178">Om du klickar på den **hämta Swagger** knappen och öppna JSON-filen, visas resultatet liknar följande:</span><span class="sxs-lookup"><span data-stu-id="13aea-178">If you click the **Download Swagger** button and open the JSON file, you'll see results similar to the following:</span></span>

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

<span data-ttu-id="13aea-179">Den utökade egenskapen **x-ms-schedular-utlösaren** är hur utlösare beskrivs i API-definitions- och läggs till automatiskt av gateway för API-app när du begär API-definition via gatewayen om begäran till en av de följande villkor.</span><span class="sxs-lookup"><span data-stu-id="13aea-179">The extension property **x-ms-schedular-trigger** is how triggers are described in API definition, and is automatically added by the API app gateway when you request the API definition via the gateway if the request to one of the following criteria.</span></span> <span data-ttu-id="13aea-180">(Du kan också lägga till den här egenskapen manuellt.)</span><span class="sxs-lookup"><span data-stu-id="13aea-180">(You can also add this property manually.)</span></span>

* <span data-ttu-id="13aea-181">Avsökningen utlösare</span><span class="sxs-lookup"><span data-stu-id="13aea-181">Poll trigger</span></span>
  * <span data-ttu-id="13aea-182">Om HTTP-metoden är **hämta**.</span><span class="sxs-lookup"><span data-stu-id="13aea-182">If the HTTP method is **GET**.</span></span>
  * <span data-ttu-id="13aea-183">Om den **operationId** egenskap innehåller strängen **utlösaren**.</span><span class="sxs-lookup"><span data-stu-id="13aea-183">If the **operationId** property contains the string **trigger**.</span></span>
  * <span data-ttu-id="13aea-184">Om den **parametrar** -egenskapen innehåller en parameter med en **namn** egenskapen **triggerState**.</span><span class="sxs-lookup"><span data-stu-id="13aea-184">If the **parameters** property includes a parameter with a **name** property set to **triggerState**.</span></span>
* <span data-ttu-id="13aea-185">Push-utlösare</span><span class="sxs-lookup"><span data-stu-id="13aea-185">Push trigger</span></span>
  * <span data-ttu-id="13aea-186">Om HTTP-metoden är **PLACERA**.</span><span class="sxs-lookup"><span data-stu-id="13aea-186">If the HTTP method is **PUT**.</span></span>
  * <span data-ttu-id="13aea-187">Om den **operationId** egenskap innehåller strängen **utlösaren**.</span><span class="sxs-lookup"><span data-stu-id="13aea-187">If the **operationId** property contains the string **trigger**.</span></span>
  * <span data-ttu-id="13aea-188">Om den **parametrar** -egenskapen innehåller en parameter med en **namn** egenskapen **utlösarens ID**.</span><span class="sxs-lookup"><span data-stu-id="13aea-188">If the **parameters** property includes a parameter with a **name** property set to **triggerId**.</span></span>

## <a name="use-api-app-triggers-in-logic-apps"></a><span data-ttu-id="13aea-189">Använd utlösare för API Apps i Logic apps</span><span class="sxs-lookup"><span data-stu-id="13aea-189">Use API app triggers in Logic apps</span></span>
### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a><span data-ttu-id="13aea-190">Visa och konfigurera utlösare för API Apps i Logic apps designer</span><span class="sxs-lookup"><span data-stu-id="13aea-190">List and configure API app triggers in the Logic apps designer</span></span>
<span data-ttu-id="13aea-191">Om du skapar en logikapp i samma resursgrupp som API-app kommer du att kunna lägga till den till på designerytan genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="13aea-191">If you create a Logic app in the same resource group as the API app, you will be able to add it to the designer canvas simply by clicking it.</span></span> <span data-ttu-id="13aea-192">Följande bilder visar detta:</span><span class="sxs-lookup"><span data-stu-id="13aea-192">The following images illustrate this:</span></span>

![Utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Konfigurera avsökning utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Konfigurera Push-utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a><span data-ttu-id="13aea-196">Optimera API app utlösare för Logic apps</span><span class="sxs-lookup"><span data-stu-id="13aea-196">Optimize API app triggers for Logic apps</span></span>
<span data-ttu-id="13aea-197">När du lägger till utlösare en API-app, finns det några saker du kan göra för att förbättra upplevelsen när du använder API-app i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="13aea-197">After you add triggers to an API app, there are a few things you can do to improve the experience when using the API app in a Logic app.</span></span>

<span data-ttu-id="13aea-198">Till exempel den **triggerState** parameter för avsökningsutlösare ska anges till följande uttryck i logikappen.</span><span class="sxs-lookup"><span data-stu-id="13aea-198">For instance, the **triggerState** parameter for poll triggers should be set to the following expression in the Logic app.</span></span> <span data-ttu-id="13aea-199">Det här uttrycket ska utvärdera senaste anrop av utlösaren från logikappen och returnera värdet.</span><span class="sxs-lookup"><span data-stu-id="13aea-199">This expression should evaluate the last invocation of the trigger from the Logic app, and return that value.</span></span>  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

<span data-ttu-id="13aea-200">Obs: En förklaring av de funktioner som används i uttrycket ovan finns i dokumentationen på [språk i Arbetsflödesdefinitionen för logik App](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span><span class="sxs-lookup"><span data-stu-id="13aea-200">NOTE: For an explanation of the functions used in the expression above, refer to the documentation on [Logic App Workflow Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span></span>

<span data-ttu-id="13aea-201">Logik för app-användare måste ange uttrycket ovan för den **triggerState** parameter när du använder utlösaren.</span><span class="sxs-lookup"><span data-stu-id="13aea-201">Logic app users would need to provide the expression above for the **triggerState** parameter while using the trigger.</span></span> <span data-ttu-id="13aea-202">Det är möjligt att har det här värdet av logik app designer via egenskapen extension **x-ms-scheduler-rekommendation**.</span><span class="sxs-lookup"><span data-stu-id="13aea-202">It is possible to have this value preset by the Logic app designer through the extension property **x-ms-scheduler-recommendation**.</span></span>  <span data-ttu-id="13aea-203">Den **x-ms-synlighet** utökade egenskapen kan anges till ett värde av *interna* så att parametern själva inte visas i designern.</span><span class="sxs-lookup"><span data-stu-id="13aea-203">The **x-ms-visibility** extension property can be set to a value of *internal* so that the parameter itself is not shown on the designer.</span></span>  <span data-ttu-id="13aea-204">Följande utdrag visar som.</span><span class="sxs-lookup"><span data-stu-id="13aea-204">The following snippet illustrates that.</span></span>

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

<span data-ttu-id="13aea-205">För push-utlösare i **utlösarens ID** parameter måste identifiera logikappen.</span><span class="sxs-lookup"><span data-stu-id="13aea-205">For push triggers, the **triggerId** parameter must uniquely identify the Logic app.</span></span> <span data-ttu-id="13aea-206">En rekommenderad metod är att ange egenskapen till namnet på arbetsflödet med hjälp av följande uttryck:</span><span class="sxs-lookup"><span data-stu-id="13aea-206">A recommended best practice is to set this property to the name of the workflow by using the following expression:</span></span>

    @workflow().name

<span data-ttu-id="13aea-207">Med hjälp av den **x-ms-scheduler-rekommendation** och **x-ms-synlighet** tilläggsegenskaper i dess API ingår API-appen kan förmedla logik app Designer att automatiskt ange det här uttrycket för den användaren.</span><span class="sxs-lookup"><span data-stu-id="13aea-207">Using the **x-ms-scheduler-recommendation** and **x-ms-visibility** extension properties in its API definiton, the API app can convey to the Logic app designer to automatically set this expression for the user.</span></span>

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a><span data-ttu-id="13aea-208">Lägga till tilläggsegenskaper i API attributdefinitionstabellen</span><span class="sxs-lookup"><span data-stu-id="13aea-208">Add extension properties in API defintion</span></span>
<span data-ttu-id="13aea-209">Information om ytterligare metadata -, till exempel egenskaper för webbtjänsttillägg **x-ms-scheduler-rekommendation** och **x-ms-synlighet** -kan läggas till i API-attributdefinitionstabellen på något av två sätt: statisk eller dynamisk.</span><span class="sxs-lookup"><span data-stu-id="13aea-209">Additional metadata information - such as the extension properties **x-ms-scheduler-recommendation** and **x-ms-visibility** - can be added in the API defintion in one of two ways: static or dynamic.</span></span>

<span data-ttu-id="13aea-210">För statisk metadata, du kan redigera direkt i */metadata/apiDefinition.swagger.json* filen i projektet och Lägg till egenskaper manuellt.</span><span class="sxs-lookup"><span data-stu-id="13aea-210">For static metadata, you can directly edit the */metadata/apiDefinition.swagger.json* file in your project and add the properties manually.</span></span>

<span data-ttu-id="13aea-211">Du kan redigera filen SwaggerConfig.cs för att lägga till ett filter för åtgärden som du kan lägga till dessa tillägg för API-appar som använder dynamiska metadata.</span><span class="sxs-lookup"><span data-stu-id="13aea-211">For API apps using dynamic metadata, you can edit the SwaggerConfig.cs file to add an operation filter which can add these extensions.</span></span>

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


<span data-ttu-id="13aea-212">Följande är ett exempel på hur den här klassen kan implementeras för att underlätta scenariot för dynamisk metadata.</span><span class="sxs-lookup"><span data-stu-id="13aea-212">The following is an example of how this class can be implemented to facilitate the dynamic metadata scenario.</span></span>

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
