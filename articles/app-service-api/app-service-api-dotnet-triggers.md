---
title: "Utlösare för aaaApp API Apps | Microsoft Docs"
description: "Hur tooimplement utlöser i en API-App i Azure App Service"
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
ms.openlocfilehash: 2d6b6a942a23c0a93987e9c48b69ecc739bfd814
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-api-app-triggers"></a><span data-ttu-id="57040-103">Utlösare för API Apps i Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="57040-103">Azure App Service API app triggers</span></span>
> [!NOTE]
> <span data-ttu-id="57040-104">Den här versionen av hello artikeln gäller tooAPI apps 2014-12-01-preview schemaversion.</span><span class="sxs-lookup"><span data-stu-id="57040-104">This version of hello article applies tooAPI apps 2014-12-01-preview schema version.</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="57040-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="57040-105">Overview</span></span>
<span data-ttu-id="57040-106">Den här artikeln förklarar hur tooimplement API-app utlöser och använda dem från en logikapp.</span><span class="sxs-lookup"><span data-stu-id="57040-106">This article explains how tooimplement API app triggers and consume them from a Logic app.</span></span>

<span data-ttu-id="57040-107">Alla hello kodfragment i det här avsnittet kopieras från hello [FileWatcher API-App kodexemplet](http://go.microsoft.com/fwlink/?LinkId=534802).</span><span class="sxs-lookup"><span data-stu-id="57040-107">All of hello code snippets in this topic are copied from hello [FileWatcher API App code sample](http://go.microsoft.com/fwlink/?LinkId=534802).</span></span>

<span data-ttu-id="57040-108">Observera att du behöver toodownload hello följande nuget-paket för hello koden i den här artikeln toobuild och kör: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span><span class="sxs-lookup"><span data-stu-id="57040-108">Note that you'll need toodownload hello following nuget package for hello code in this article toobuild and run: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).</span></span>

## <a name="what-are-api-app-triggers"></a><span data-ttu-id="57040-109">Vad är utlösare för API Apps?</span><span class="sxs-lookup"><span data-stu-id="57040-109">What are API app triggers?</span></span>
<span data-ttu-id="57040-110">Det är ett vanligt scenario för en händelse för toofire en API-app så att klienter på hello API-app kan vidta lämpliga åtgärder för hello i svaret toohello händelse.</span><span class="sxs-lookup"><span data-stu-id="57040-110">It's a common scenario for an API app toofire an event so that clients of hello API app can take hello appropriate action in response toohello event.</span></span> <span data-ttu-id="57040-111">hello REST API-baserad funktion som stöder det här scenariot kallas för en utlösare för API-app.</span><span class="sxs-lookup"><span data-stu-id="57040-111">hello REST API based mechanism that supports this scenario is called an API app trigger.</span></span>

<span data-ttu-id="57040-112">Till exempel anta att din klientkod använder hello [Twitter-anslutningen API-app](../connectors/connectors-create-api-twitter.md) och din kod måste tooperform en åtgärd baserat på nya tweets med specifika ord.</span><span class="sxs-lookup"><span data-stu-id="57040-112">For example, let's say your client code is using hello [Twitter Connector API app](../connectors/connectors-create-api-twitter.md) and your code needs tooperform an action based on new tweets that contain specific words.</span></span> <span data-ttu-id="57040-113">I så fall måste kan du ställa in en avsökning eller push-utlösare toofacilitate detta behov.</span><span class="sxs-lookup"><span data-stu-id="57040-113">In this case, you might set up a poll or push trigger toofacilitate this need.</span></span>

## <a name="poll-trigger-versus-push-trigger"></a><span data-ttu-id="57040-114">Avsökningen utlösaren jämfört med push-utlösare</span><span class="sxs-lookup"><span data-stu-id="57040-114">Poll trigger versus push trigger</span></span>
<span data-ttu-id="57040-115">För närvarande stöds två typer av utlösare:</span><span class="sxs-lookup"><span data-stu-id="57040-115">Currently, two types of triggers are supported:</span></span>

* <span data-ttu-id="57040-116">Avsökningen utlösaren - klienten ska avsöka hello API-app för meddelanden om en händelse med sagts upp</span><span class="sxs-lookup"><span data-stu-id="57040-116">Poll trigger - Client polls hello API app for notification of an event having been fired</span></span>
* <span data-ttu-id="57040-117">Push-utlösare - klienten meddelas av hello API-app när en händelse utlöses</span><span class="sxs-lookup"><span data-stu-id="57040-117">Push trigger - Client is notified by hello API app when an event fires</span></span>

### <a name="poll-trigger"></a><span data-ttu-id="57040-118">Avsökningen utlösare</span><span class="sxs-lookup"><span data-stu-id="57040-118">Poll trigger</span></span>
<span data-ttu-id="57040-119">En avsökning utlösare implementeras som en vanlig REST-API och förväntar att dess klienter (till exempel en logikapp) toopoll i ordning tooget meddelande.</span><span class="sxs-lookup"><span data-stu-id="57040-119">A poll trigger is implemented as a regular REST API and expects its clients (such as a Logic app) toopoll it in order tooget notification.</span></span> <span data-ttu-id="57040-120">När klienten hello kan upprätthålla tillstånd, är hello avsökning utlösaren själva tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="57040-120">While hello client may maintain state, hello poll trigger itself is stateless.</span></span>

<span data-ttu-id="57040-121">hello följande information om begäran och svar hälsningspaket illustrera vissa viktiga aspekter av hello avsökning utlösaren kontrakt:</span><span class="sxs-lookup"><span data-stu-id="57040-121">hello following information regarding hello request and response packets illustrate some key aspects of hello poll trigger contract:</span></span>

* <span data-ttu-id="57040-122">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="57040-122">Request</span></span>
  * <span data-ttu-id="57040-123">HTTP-metod: hämta</span><span class="sxs-lookup"><span data-stu-id="57040-123">HTTP method: GET</span></span>
  * <span data-ttu-id="57040-124">Parametrar</span><span class="sxs-lookup"><span data-stu-id="57040-124">Parameters</span></span>
    * <span data-ttu-id="57040-125">triggerState - den här valfria parametern klienterna toospecify deras tillstånd så som hello avsökning utlösaren korrekt kan bestämma om tooreturn meddelande eller inte baserat på hello angetts tillstånd.</span><span class="sxs-lookup"><span data-stu-id="57040-125">triggerState - This optional parameter allows clients toospecify their state so that hello poll trigger can properly decide whether tooreturn notification or not based on hello specified state.</span></span>
    * <span data-ttu-id="57040-126">API-specifika parametrar</span><span class="sxs-lookup"><span data-stu-id="57040-126">API-specific parameters</span></span>
* <span data-ttu-id="57040-127">Svar</span><span class="sxs-lookup"><span data-stu-id="57040-127">Response</span></span>
  * <span data-ttu-id="57040-128">Statuskoden **200** - begäran är giltig och att det finns ett meddelande från hello utlösare.</span><span class="sxs-lookup"><span data-stu-id="57040-128">Status code **200** - Request is valid and there is a notification from hello trigger.</span></span> <span data-ttu-id="57040-129">hello innehåll av hello-meddelande kommer att hello svarstexten.</span><span class="sxs-lookup"><span data-stu-id="57040-129">hello content of hello notification will be hello response body.</span></span> <span data-ttu-id="57040-130">Ett ”försök igen efter”-huvud i hello svaret anger att ytterligare meddelandedata måste hämtas med ett efterföljande begäran-anrop.</span><span class="sxs-lookup"><span data-stu-id="57040-130">A "Retry-After" header in hello response indicates that additional notification data must be retrieved with a subsequent request call.</span></span>
  * <span data-ttu-id="57040-131">Statuskoden **202** - begäran är giltig, men det finns inget nytt meddelande från hello utlösare.</span><span class="sxs-lookup"><span data-stu-id="57040-131">Status code **202** - Request is valid, but there is no new notification from hello trigger.</span></span>
  * <span data-ttu-id="57040-132">Statuskoden **4xx** -begäran är inte giltig.</span><span class="sxs-lookup"><span data-stu-id="57040-132">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="57040-133">hello klienten bör inte försöka hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="57040-133">hello client should not retry hello request.</span></span>
  * <span data-ttu-id="57040-134">Statuskoden **5xx** -förfrågan resulterade i ett internt serverfel och/eller ett tillfälligt problem.</span><span class="sxs-lookup"><span data-stu-id="57040-134">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="57040-135">hello klienten bör försöka hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="57040-135">hello client should retry hello request.</span></span>

<span data-ttu-id="57040-136">hello följande kodavsnitt är ett exempel på hur tooimplement röstning utlösa.</span><span class="sxs-lookup"><span data-stu-id="57040-136">hello following code snippet is an example of how tooimplement a poll trigger.</span></span>

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check toosee whether there is any file touched after hello timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after hello timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after hello timestamp, tell hello caller toopoll again after 1 mintue.
        else
        {
            // Extension method provided by hello AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

<span data-ttu-id="57040-137">tootest utlösa den här avsökningen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="57040-137">tootest this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="57040-138">Distribuera hello API App med en inställning för autentisering av **offentliga anonym**.</span><span class="sxs-lookup"><span data-stu-id="57040-138">Deploy hello API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="57040-139">Anropa hello **touch** åtgärden tootouch en fil.</span><span class="sxs-lookup"><span data-stu-id="57040-139">Call hello **touch** operation tootouch a file.</span></span> <span data-ttu-id="57040-140">hello följande bild visar ett exempel på begäran via Postman.</span><span class="sxs-lookup"><span data-stu-id="57040-140">hello following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="57040-141">![Anropa Touch åtgärden via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="57040-141">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
3. <span data-ttu-id="57040-142">Anropa hello avsökning utlösare med hello **triggerState** parameterinställning tooa tidsstämpel tidigare tooStep #2.</span><span class="sxs-lookup"><span data-stu-id="57040-142">Call hello poll trigger with hello **triggerState** parameter set tooa time stamp prior tooStep #2.</span></span> <span data-ttu-id="57040-143">hello visar följande bild hello exempelbegäran via Postman.</span><span class="sxs-lookup"><span data-stu-id="57040-143">hello following image shows hello sample request via Postman.</span></span>
   <span data-ttu-id="57040-144">![Anropa utlösare avsökning via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="57040-144">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)</span></span>

### <a name="push-trigger"></a><span data-ttu-id="57040-145">Push-utlösare</span><span class="sxs-lookup"><span data-stu-id="57040-145">Push trigger</span></span>
<span data-ttu-id="57040-146">En push-utlösare implementeras som en vanlig REST-API som skickar meddelanden tooclients som har registrerat toobe meddelas när specifika händelser eller.</span><span class="sxs-lookup"><span data-stu-id="57040-146">A push trigger is implemented as a regular REST API that pushes notifications tooclients who have registered toobe notified when specific events fire.</span></span>

<span data-ttu-id="57040-147">följande information om begäran och svar hälsningspaket hello illustrera vissa viktiga aspekter av hello push-utlösare kontraktet.</span><span class="sxs-lookup"><span data-stu-id="57040-147">hello following information regarding hello request and response packets illustrate some key aspects of hello push trigger contract.</span></span>

* <span data-ttu-id="57040-148">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="57040-148">Request</span></span>
  * <span data-ttu-id="57040-149">HTTP-metod: PLACERA</span><span class="sxs-lookup"><span data-stu-id="57040-149">HTTP method: PUT</span></span>
  * <span data-ttu-id="57040-150">Parametrar</span><span class="sxs-lookup"><span data-stu-id="57040-150">Parameters</span></span>
    * <span data-ttu-id="57040-151">Utlösarens ID: krävs – ogenomskinlig sträng (till exempel ett GUID) som representerar hello registreringen av push-utlösare.</span><span class="sxs-lookup"><span data-stu-id="57040-151">triggerId: required - Opaque string (such as a GUID) that represents hello registration of a push trigger.</span></span>
    * <span data-ttu-id="57040-152">callbackUrl: krävs - URL för hello återanrop tooinvoke när hello händelsen utlöses.</span><span class="sxs-lookup"><span data-stu-id="57040-152">callbackUrl: required - URL of hello callback tooinvoke when hello event fires.</span></span> <span data-ttu-id="57040-153">hello anrop är en enkel HTTP POST-anrop.</span><span class="sxs-lookup"><span data-stu-id="57040-153">hello invocation is a simple POST HTTP call.</span></span>
    * <span data-ttu-id="57040-154">API-specifika parametrar</span><span class="sxs-lookup"><span data-stu-id="57040-154">API-specific parameters</span></span>
* <span data-ttu-id="57040-155">Svar</span><span class="sxs-lookup"><span data-stu-id="57040-155">Response</span></span>
  * <span data-ttu-id="57040-156">Statuskoden **200** -begäran tooregister klienten lyckas.</span><span class="sxs-lookup"><span data-stu-id="57040-156">Status code **200** - Request tooregister client successful.</span></span>
  * <span data-ttu-id="57040-157">Statuskoden **4xx** -begäran är inte giltig.</span><span class="sxs-lookup"><span data-stu-id="57040-157">Status code **4xx** - Request is not valid.</span></span> <span data-ttu-id="57040-158">hello klienten bör inte försöka hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="57040-158">hello client should not retry hello request.</span></span>
  * <span data-ttu-id="57040-159">Statuskoden **5xx** -förfrågan resulterade i ett internt serverfel och/eller ett tillfälligt problem.</span><span class="sxs-lookup"><span data-stu-id="57040-159">Status code **5xx** - Request has resulted in an internal server error and/or temporary issue.</span></span> <span data-ttu-id="57040-160">hello klienten bör försöka hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="57040-160">hello client should retry hello request.</span></span>
* <span data-ttu-id="57040-161">Motringning</span><span class="sxs-lookup"><span data-stu-id="57040-161">Callback</span></span>
  * <span data-ttu-id="57040-162">HTTP-metod: POST</span><span class="sxs-lookup"><span data-stu-id="57040-162">HTTP method: POST</span></span>
  * <span data-ttu-id="57040-163">Begäran: meddelandeinnehåll.</span><span class="sxs-lookup"><span data-stu-id="57040-163">Request body: Notification content.</span></span>

<span data-ttu-id="57040-164">hello följande kodavsnitt är ett exempel på hur tooimplement en push utlösa:</span><span class="sxs-lookup"><span data-stu-id="57040-164">hello following code snippet is an example of how tooimplement a push trigger:</span></span>

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by hello AppService service SDK.
        // Here it defines hello input of hello push trigger is a string and hello output toohello callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register hello trigger toosome trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by hello AppService service SDK indicating hello registration is completed.
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
            // Use FileSystemWatcher toolisten toofile change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire hello push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate hello FileSystemWatcher object with hello triggerId.
            _store[triggerId] = watcher;

        }

        // Fire hello assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed tooinvoke hello callback.
            Runtime runtime,
            // hello callback tooinvoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK tooinvoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

<span data-ttu-id="57040-165">tootest utlösa den här avsökningen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="57040-165">tootest this poll trigger, follow these steps:</span></span>

1. <span data-ttu-id="57040-166">Distribuera hello API App med en inställning för autentisering av **offentliga anonym**.</span><span class="sxs-lookup"><span data-stu-id="57040-166">Deploy hello API App with an authentication setting of **public anonymous**.</span></span>
2. <span data-ttu-id="57040-167">Bläddra för[http://requestb.in/](http://requestb.in/) toocreate en RequestBin som fungerar som återanrop URL: en.</span><span class="sxs-lookup"><span data-stu-id="57040-167">Browse too[http://requestb.in/](http://requestb.in/) toocreate a RequestBin which will serve as your callback URL.</span></span>
3. <span data-ttu-id="57040-168">Anropa hello push-utlösare med ett GUID som **utlösarens ID** och hello RequestBin URL: en som **callbackUrl**.</span><span class="sxs-lookup"><span data-stu-id="57040-168">Call hello push trigger with a GUID as **triggerId** and hello RequestBin URL as **callbackUrl**.</span></span>
   <span data-ttu-id="57040-169">![Anropa Push-utlösare via Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="57040-169">![Call Push Trigger via Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)</span></span>
4. <span data-ttu-id="57040-170">Anropa hello **touch** åtgärden tootouch en fil.</span><span class="sxs-lookup"><span data-stu-id="57040-170">Call hello **touch** operation tootouch a file.</span></span> <span data-ttu-id="57040-171">hello följande bild visar ett exempel på begäran via Postman.</span><span class="sxs-lookup"><span data-stu-id="57040-171">hello following image shows a sample request via Postman.</span></span>
   <span data-ttu-id="57040-172">![Anropa Touch åtgärden via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span><span class="sxs-lookup"><span data-stu-id="57040-172">![Call Touch Operation via Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)</span></span>
5. <span data-ttu-id="57040-173">Kontrollera hello RequestBin tooconfirm som hello push-utlösare återanropet anropas med egenskapen utdata.</span><span class="sxs-lookup"><span data-stu-id="57040-173">Check hello RequestBin tooconfirm that hello push trigger callback is invoked with property output.</span></span>
   <span data-ttu-id="57040-174">![Anropa utlösare avsökning via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span><span class="sxs-lookup"><span data-stu-id="57040-174">![Call Poll Trigger via Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)</span></span>

### <a name="describe-triggers-in-api-definition"></a><span data-ttu-id="57040-175">Beskriv utlösare i API-definition</span><span class="sxs-lookup"><span data-stu-id="57040-175">Describe triggers in API definition</span></span>
<span data-ttu-id="57040-176">När du implementerar hello utlösare och distribuera din API app tooAzure, navigera toohello **API-Definition** bladet i hello Azure preview portal och du ser att identifieras automatiskt utlösare i hello-Gränssnittet som styrs av Hej Swagger 2.0 API-definition av hello API-app.</span><span class="sxs-lookup"><span data-stu-id="57040-176">After implementing hello triggers and deploying your API app tooAzure, navigate toohello **API Definition** blade in hello Azure preview portal and you'll see that triggers are automatically recognized in hello UI, which is driven by hello Swagger 2.0 API definition of hello API app.</span></span>

![Bladet för API-Definition](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

<span data-ttu-id="57040-178">Om du klickar på hello **hämta Swagger** knappen och öppna hello JSON-fil visas resultaten liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="57040-178">If you click hello **Download Swagger** button and open hello JSON file, you'll see results similar toohello following:</span></span>

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

<span data-ttu-id="57040-179">Hej tilläggsegenskapen **x-ms-schedular-utlösaren** är hur utlösare beskrivs i API-definitions- och läggs till automatiskt av gateway för hello API-app när du begär hello API-definition via hello gateway om hello begär tooone av Hej följande villkor.</span><span class="sxs-lookup"><span data-stu-id="57040-179">hello extension property **x-ms-schedular-trigger** is how triggers are described in API definition, and is automatically added by hello API app gateway when you request hello API definition via hello gateway if hello request tooone of hello following criteria.</span></span> <span data-ttu-id="57040-180">(Du kan också lägga till den här egenskapen manuellt.)</span><span class="sxs-lookup"><span data-stu-id="57040-180">(You can also add this property manually.)</span></span>

* <span data-ttu-id="57040-181">Avsökningen utlösare</span><span class="sxs-lookup"><span data-stu-id="57040-181">Poll trigger</span></span>
  * <span data-ttu-id="57040-182">Om hello HTTP-metoden är **hämta**.</span><span class="sxs-lookup"><span data-stu-id="57040-182">If hello HTTP method is **GET**.</span></span>
  * <span data-ttu-id="57040-183">Om hello **operationId** egenskapen innehåller hello sträng **utlösaren**.</span><span class="sxs-lookup"><span data-stu-id="57040-183">If hello **operationId** property contains hello string **trigger**.</span></span>
  * <span data-ttu-id="57040-184">Om hello **parametrar** -egenskapen innehåller en parameter med en **namn** egenskapsuppsättning för**triggerState**.</span><span class="sxs-lookup"><span data-stu-id="57040-184">If hello **parameters** property includes a parameter with a **name** property set too**triggerState**.</span></span>
* <span data-ttu-id="57040-185">Push-utlösare</span><span class="sxs-lookup"><span data-stu-id="57040-185">Push trigger</span></span>
  * <span data-ttu-id="57040-186">Om hello HTTP-metoden är **PLACERA**.</span><span class="sxs-lookup"><span data-stu-id="57040-186">If hello HTTP method is **PUT**.</span></span>
  * <span data-ttu-id="57040-187">Om hello **operationId** egenskapen innehåller hello sträng **utlösaren**.</span><span class="sxs-lookup"><span data-stu-id="57040-187">If hello **operationId** property contains hello string **trigger**.</span></span>
  * <span data-ttu-id="57040-188">Om hello **parametrar** -egenskapen innehåller en parameter med en **namn** egenskapsuppsättning för**utlösarens ID**.</span><span class="sxs-lookup"><span data-stu-id="57040-188">If hello **parameters** property includes a parameter with a **name** property set too**triggerId**.</span></span>

## <a name="use-api-app-triggers-in-logic-apps"></a><span data-ttu-id="57040-189">Använd utlösare för API Apps i Logic apps</span><span class="sxs-lookup"><span data-stu-id="57040-189">Use API app triggers in Logic apps</span></span>
### <a name="list-and-configure-api-app-triggers-in-hello-logic-apps-designer"></a><span data-ttu-id="57040-190">Visa och konfigurera utlösare för API Apps i hello Logic apps designer</span><span class="sxs-lookup"><span data-stu-id="57040-190">List and configure API app triggers in hello Logic apps designer</span></span>
<span data-ttu-id="57040-191">Om du skapar en logikapp i hello samma resursgrupp som Hej API-appen, kommer du att kunna tooadd den toohello designerytan genom att klicka på den.</span><span class="sxs-lookup"><span data-stu-id="57040-191">If you create a Logic app in hello same resource group as hello API app, you will be able tooadd it toohello designer canvas simply by clicking it.</span></span> <span data-ttu-id="57040-192">hello följande bilder visar detta:</span><span class="sxs-lookup"><span data-stu-id="57040-192">hello following images illustrate this:</span></span>

![Utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Konfigurera avsökning utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Konfigurera Push-utlösare i logik App Designer](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a><span data-ttu-id="57040-196">Optimera API app utlösare för Logic apps</span><span class="sxs-lookup"><span data-stu-id="57040-196">Optimize API app triggers for Logic apps</span></span>
<span data-ttu-id="57040-197">När du lägger till utlösare tooan API-app, finns det några saker du kan göra tooimprove hello upplevelse när du använder hello API-app i en logikapp.</span><span class="sxs-lookup"><span data-stu-id="57040-197">After you add triggers tooan API app, there are a few things you can do tooimprove hello experience when using hello API app in a Logic app.</span></span>

<span data-ttu-id="57040-198">Till exempel hello **triggerState** parameter för avsökningsutlösare ska anges toohello följande uttryck i hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="57040-198">For instance, hello **triggerState** parameter for poll triggers should be set toohello following expression in hello Logic app.</span></span> <span data-ttu-id="57040-199">Det här uttrycket ska utvärdera hello senaste anrop av hello utlösaren från hello logikapp och returnera värdet.</span><span class="sxs-lookup"><span data-stu-id="57040-199">This expression should evaluate hello last invocation of hello trigger from hello Logic app, and return that value.</span></span>  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

<span data-ttu-id="57040-200">: En förklaring av hello-funktioner som används i hello uttrycket ovan finns toohello dokumentation på [språk i Arbetsflödesdefinitionen för logik App](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span><span class="sxs-lookup"><span data-stu-id="57040-200">NOTE: For an explanation of hello functions used in hello expression above, refer toohello documentation on [Logic App Workflow Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx).</span></span>

<span data-ttu-id="57040-201">Logik appanvändare skulle behöva tooprovide hello-uttryck ovanför hello **triggerState** parameter när du använder hello utlösare.</span><span class="sxs-lookup"><span data-stu-id="57040-201">Logic app users would need tooprovide hello expression above for hello **triggerState** parameter while using hello trigger.</span></span> <span data-ttu-id="57040-202">Det är möjligt toohave värdet förinställningen av hello logik app designer via hello tilläggsegenskapen **x-ms-scheduler-rekommendation**.</span><span class="sxs-lookup"><span data-stu-id="57040-202">It is possible toohave this value preset by hello Logic app designer through hello extension property **x-ms-scheduler-recommendation**.</span></span>  <span data-ttu-id="57040-203">Hej **x-ms-synlighet** utökade egenskapen kan anges tooa värdet för *interna* så att hello parametern själva inte visas hello designer.</span><span class="sxs-lookup"><span data-stu-id="57040-203">hello **x-ms-visibility** extension property can be set tooa value of *internal* so that hello parameter itself is not shown on hello designer.</span></span>  <span data-ttu-id="57040-204">hello följande fragment visas som.</span><span class="sxs-lookup"><span data-stu-id="57040-204">hello following snippet illustrates that.</span></span>

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

<span data-ttu-id="57040-205">Push-utlösare hello **utlösarens ID** parameter måste identifiera hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="57040-205">For push triggers, hello **triggerId** parameter must uniquely identify hello Logic app.</span></span> <span data-ttu-id="57040-206">En rekommenderad metod är tooset toohello egenskapsnamnet för hello arbetsflöde med hjälp av hello följande uttryck:</span><span class="sxs-lookup"><span data-stu-id="57040-206">A recommended best practice is tooset this property toohello name of hello workflow by using hello following expression:</span></span>

    @workflow().name

<span data-ttu-id="57040-207">Med hjälp av hello **x-ms-scheduler-rekommendation** och **x-ms-synlighet** tilläggsegenskaper i dess API ingår, hello API-app kan förmedla toohello logik app designer tooautomatically Ställ in uttryck för hello användare.</span><span class="sxs-lookup"><span data-stu-id="57040-207">Using hello **x-ms-scheduler-recommendation** and **x-ms-visibility** extension properties in its API definiton, hello API app can convey toohello Logic app designer tooautomatically set this expression for hello user.</span></span>

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a><span data-ttu-id="57040-208">Lägga till tilläggsegenskaper i API attributdefinitionstabellen</span><span class="sxs-lookup"><span data-stu-id="57040-208">Add extension properties in API defintion</span></span>
<span data-ttu-id="57040-209">Information om ytterligare metadata -, till exempel hello tilläggsegenskaper **x-ms-scheduler-rekommendation** och **x-ms-synlighet** -kan läggas till i hello API attributdefinitionstabellen på något av två sätt: statisk eller dynamisk.</span><span class="sxs-lookup"><span data-stu-id="57040-209">Additional metadata information - such as hello extension properties **x-ms-scheduler-recommendation** and **x-ms-visibility** - can be added in hello API defintion in one of two ways: static or dynamic.</span></span>

<span data-ttu-id="57040-210">För statisk metadata, kan du direkt redigera hello */metadata/apiDefinition.swagger.json* i projektet och Lägg till hello egenskaper manuellt.</span><span class="sxs-lookup"><span data-stu-id="57040-210">For static metadata, you can directly edit hello */metadata/apiDefinition.swagger.json* file in your project and add hello properties manually.</span></span>

<span data-ttu-id="57040-211">Du kan redigera hello SwaggerConfig.cs filen tooadd ett åtgärden filter som kan lägga till dessa tillägg för API-appar som använder dynamiska metadata.</span><span class="sxs-lookup"><span data-stu-id="57040-211">For API apps using dynamic metadata, you can edit hello SwaggerConfig.cs file tooadd an operation filter which can add these extensions.</span></span>

    GlobalConfiguration.Configuration
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


<span data-ttu-id="57040-212">hello följande är ett exempel på hur den här klassen kan vara implementerad toofacilitate hello dynamiska metadata scenario.</span><span class="sxs-lookup"><span data-stu-id="57040-212">hello following is an example of how this class can be implemented toofacilitate hello dynamic metadata scenario.</span></span>

    // Add extension properties on hello triggerState parameter
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
                    // x-ms-visibility: set too'internal' toosignify this is an internal field
                    // x-ms-scheduler-recommendation: set tooa value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
