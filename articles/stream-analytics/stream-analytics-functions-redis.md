---
title: "aaaStream Analytics realtidsbearbetning för Azure Functions | Microsoft Docs"
description: "Lär dig hur toouse en Azure-funktion anslutna en Service Bus-kö, toopopulate ett Azure Redis-Cache från hello utdata för ett Stream Analytics-jobb."
keywords: "dataströmmen, redis-cache, service bus-kö"
services: stream-analytics
author: ryancrawcour
manager: jhubbard
documentationcenter: 
ms.assetid: d428bb33-4244-4001-b93d-c77bed816527
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: ryancraw
ms.openlocfilehash: 5ef4fe76c2cadf896a80eeaf421f010c315918af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostore-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a><span data-ttu-id="c4210-104">Hur toostore data från Azure Stream Analytics i en Azure Redis-Cache med hjälp av Azure-funktioner</span><span class="sxs-lookup"><span data-stu-id="c4210-104">How toostore data from Azure Stream Analytics in an Azure Redis Cache using Azure Functions</span></span>
<span data-ttu-id="c4210-105">Azure Stream Analytics kan du snabbt utveckla och distribuera lösningar för prisvärda toogain realtidsinsikter från enheter, sensorer, infrastruktur, och program eller en dataström.</span><span class="sxs-lookup"><span data-stu-id="c4210-105">Azure Stream Analytics lets you rapidly develop and deploy low-cost solutions toogain real-time insights from devices, sensors, infrastructure, and applications, or any stream of data.</span></span> <span data-ttu-id="c4210-106">Den gör olika användningsfall, till exempel realtid hantering och övervakning kommandot och, att upptäcka bedrägerier, anslutna bilar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="c4210-106">It enables various use cases such as real-time management and monitoring, command and control, fraud detection, connected cars and many more.</span></span> <span data-ttu-id="c4210-107">Du kanske vill toostore data i en distribuerad datalager, till exempel en Azure Redis-cache för utdata av Azure Stream Analytics i många sådana scenarier.</span><span class="sxs-lookup"><span data-stu-id="c4210-107">In many such scenarios, you may want toostore data outputted by Azure Stream Analytics into a distributed data store such as an Azure Redis cache.</span></span>

<span data-ttu-id="c4210-108">Anta att du är en del av en telekommunikation företag.</span><span class="sxs-lookup"><span data-stu-id="c4210-108">Suppose you are part of a telecommunications company.</span></span> <span data-ttu-id="c4210-109">Du försöker toodetect SIM bedrägeri där flera anrop från hello samma identitet, hello på samma gång, men i olika geografiskt platser.</span><span class="sxs-lookup"><span data-stu-id="c4210-109">You are trying toodetect SIM fraud where multiple calls coming from hello same identity, at hello same time, but in different geographically locations.</span></span> <span data-ttu-id="c4210-110">Du har gett med att lagra alla hello potentiella bedrägliga telefonsamtal i en Azure Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="c4210-110">You are tasked with storing all hello potential fraudulent phone calls in an Azure Redis cache.</span></span> <span data-ttu-id="c4210-111">I den här bloggen ger vi riktlinjer för hur du lätt kan utföra uppgiften.</span><span class="sxs-lookup"><span data-stu-id="c4210-111">In this blog, we provide guidance on how you can easily complete your task.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c4210-112">Krav</span><span class="sxs-lookup"><span data-stu-id="c4210-112">Prerequisites</span></span>
<span data-ttu-id="c4210-113">Fullständig hello [realtid att upptäcka bedrägerier] [ fraud-detection] hanteringspaketen för ASA</span><span class="sxs-lookup"><span data-stu-id="c4210-113">Complete hello [Real-time Fraud Detection][fraud-detection] walk-through for ASA</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="c4210-114">Arkitektur, översikt</span><span class="sxs-lookup"><span data-stu-id="c4210-114">Architecture Overview</span></span>
![Skärmbild av arkitektur](./media/stream-analytics-functions-redis/architecture-overview.png)

<span data-ttu-id="c4210-116">I hello föregående bild visas kan Stream Analytics strömning indata toobe efterfrågas och skickas tooan utdata.</span><span class="sxs-lookup"><span data-stu-id="c4210-116">As shown in hello preceding figure, Stream Analytics allows streaming input data toobe queried and sent tooan output.</span></span> <span data-ttu-id="c4210-117">Baserat på hello utdata kan Azure Functions sedan utlösa någon typ av händelse.</span><span class="sxs-lookup"><span data-stu-id="c4210-117">Based on hello output, Azure Functions can then trigger some type of event.</span></span> 

<span data-ttu-id="c4210-118">I den här bloggen vi fokusera på hello Azure Functions en del av denna pipeline eller mer specifikt hello utlösa en händelse som lagrar falska data till hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="c4210-118">In this blog, we focus on hello Azure Functions part of this pipeline, or more specifically hello triggering of an event that stores fraudulent data into hello cache.</span></span>
<span data-ttu-id="c4210-119">När du har slutfört hello [realtid att upptäcka bedrägerier] [ fraud-detection] kursen har du indata (en händelsehubb), en fråga och utdata (blob storage) redan konfigurerad och körs.</span><span class="sxs-lookup"><span data-stu-id="c4210-119">After completing hello [Real-time Fraud Detection][fraud-detection] tutorial, you have an input (an event hub), a query, and an output (blob storage) already configured and running.</span></span> <span data-ttu-id="c4210-120">I den här bloggen ändra vi hello utdata toouse en Service Bus-kö i stället.</span><span class="sxs-lookup"><span data-stu-id="c4210-120">In this blog, we change hello output toouse a Service Bus Queue instead.</span></span> <span data-ttu-id="c4210-121">Efter att ansluta vi en Azure-funktion toothis kö.</span><span class="sxs-lookup"><span data-stu-id="c4210-121">After that, we connect an Azure Function toothis queue.</span></span> 

## <a name="create-and-connect-a-service-bus-queue-output"></a><span data-ttu-id="c4210-122">Skapa och koppla en Service Bus-kö-utdata</span><span class="sxs-lookup"><span data-stu-id="c4210-122">Create and connect a Service Bus Queue output</span></span>
<span data-ttu-id="c4210-123">toocreate en Service Bus-kö följa steg 1 och 2 i hello .NET-avsnittet i [komma igång med Service Bus-köer][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="c4210-123">toocreate a Service Bus Queue, follow steps 1 and 2 of hello .NET section in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span>
<span data-ttu-id="c4210-124">Nu ska vi ansluta hello kön toohello Stream Analytics-jobbet som skapades i hello tidigare hanteringspaketen för upptäckt av bedrägerier.</span><span class="sxs-lookup"><span data-stu-id="c4210-124">Now let's connect hello queue toohello Stream Analytics job that was created in hello earlier fraud detection walk-through.</span></span>

1. <span data-ttu-id="c4210-125">I hello Azure-portalen, går toohello **utdata** bladet för ditt projekt och välj **Lägg till** hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="c4210-125">In hello Azure portal, go toohello **Outputs** blade of your job and select **Add** at hello top of hello page.</span></span>
   
    ![Lägger till utdata](./media/stream-analytics-functions-redis/adding-outputs.png)
2. <span data-ttu-id="c4210-127">Välj **Service Bus-kö** som hello **Sink** och följer instruktionerna för hello på hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="c4210-127">Choose **Service Bus Queue** as hello **Sink** and follow hello instructions on hello screen.</span></span> <span data-ttu-id="c4210-128">Skapas till toochoose hello namnområdet för hello Service Bus-kö i [komma igång med Service Bus-köer][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="c4210-128">Be sure toochoose hello namespace of hello Service Bus Queue you created in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span> <span data-ttu-id="c4210-129">Klicka hello ”höger” när du är klar.</span><span class="sxs-lookup"><span data-stu-id="c4210-129">Click hello "right" button when you are finished.</span></span>
3. <span data-ttu-id="c4210-130">Ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="c4210-130">Specify hello following values:</span></span>
   
   * <span data-ttu-id="c4210-131">**Händelsen serialiseraren Format**: JSON</span><span class="sxs-lookup"><span data-stu-id="c4210-131">**Event Serializer Format**: JSON</span></span>
   * <span data-ttu-id="c4210-132">**Kodning**: UTF8</span><span class="sxs-lookup"><span data-stu-id="c4210-132">**Encoding**: UTF8</span></span>
   * <span data-ttu-id="c4210-133">**FORMATET**: Radseparerade</span><span class="sxs-lookup"><span data-stu-id="c4210-133">**FORMAT**: Line separated</span></span>
4. <span data-ttu-id="c4210-134">Klicka på hello **skapa** knappen tooadd käll- och tooverify att Stream Analytics kan ansluta toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c4210-134">Click hello **Create** button tooadd this source and tooverify that Stream Analytics can successfully connect toohello storage account.</span></span>
5. <span data-ttu-id="c4210-135">I hello **frågan** fliken ersätter hello aktuell fråga med följande hello.</span><span class="sxs-lookup"><span data-stu-id="c4210-135">In hello **Query** tab, replace hello current query with hello following.</span></span> <span data-ttu-id="c4210-136">Ersätt * [YOUR SERVICE BUS NAME] * med hello utdatanamn som du skapade i steg 3.</span><span class="sxs-lookup"><span data-stu-id="c4210-136">Replace *[YOUR SERVICE BUS NAME] * with hello output name you created in step 3.</span></span> 
   
    ```    
   
        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
   
        INTO [YOUR SERVICE BUS NAME]
   
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
   
        WHERE CS1.SwitchNum != CS2.SwitchNum
   
    ```

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="c4210-137">Skapa en Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="c4210-137">Create an Azure Redis Cache</span></span>
<span data-ttu-id="c4210-138">Skapa ett Azure Redis-cache genom att följa hello .NET-avsnittet i [hur tooUse Azure Redis-Cache] [ use-rediscache] tills hello avsnittet ***konfigurera hello cacheklienter***.</span><span class="sxs-lookup"><span data-stu-id="c4210-138">Create an Azure Redis cache by following hello .NET section in [How tooUse Azure Redis Cache][use-rediscache] until hello section called ***Configure hello cache clients***.</span></span>
<span data-ttu-id="c4210-139">När borttagningen är klar har du en ny Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="c4210-139">Once complete, you have a new Redis Cache.</span></span> <span data-ttu-id="c4210-140">Under **alla inställningar**väljer **åtkomstnycklar** och Skriv ner hello ***primära anslutningssträngen***.</span><span class="sxs-lookup"><span data-stu-id="c4210-140">Under **All settings**, select **Access keys** and note down hello ***Primary connection string***.</span></span>

![Skärmbild av arkitektur](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a><span data-ttu-id="c4210-142">Skapa en Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="c4210-142">Create an Azure Function</span></span>
<span data-ttu-id="c4210-143">Följ [skapa din första Azure-funktion] [ functions-getstarted] självstudiekursen tooget igång med Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="c4210-143">Follow [Create your first Azure Function][functions-getstarted] tutorial tooget started with Azure Functions.</span></span> <span data-ttu-id="c4210-144">Om du redan har en Azure-funktion du skulle t.ex. toouse och sedan gå vidare för[skriva tooRedis Cache](#Writing-to-Redis-Cache)</span><span class="sxs-lookup"><span data-stu-id="c4210-144">If you already have an Azure function you would like toouse, then skip ahead too[Writing tooRedis Cache](#Writing-to-Redis-Cache)</span></span>

1. <span data-ttu-id="c4210-145">Välj Apptjänster hello vänstra navigeringsfönstret i hello-portalen och sedan på ditt Azure funktionen app name tooget toohello funktionens app webbplats.</span><span class="sxs-lookup"><span data-stu-id="c4210-145">In hello portal, select App Services from hello left-hand navigation, then click your Azure function app name tooget toohello Function's app website.</span></span>
    <span data-ttu-id="c4210-146">![Skärmbild av listan över appar services-funktionen](./media/stream-analytics-functions-redis/app-services-function-list.png)</span><span class="sxs-lookup"><span data-stu-id="c4210-146">![Screenshot of app services function list](./media/stream-analytics-functions-redis/app-services-function-list.png)</span></span>
2. <span data-ttu-id="c4210-147">Klicka på **nya funktionen > ServiceBusQueueTrigger – C#**.</span><span class="sxs-lookup"><span data-stu-id="c4210-147">Click **New Function > ServiceBusQueueTrigger – C#**.</span></span> <span data-ttu-id="c4210-148">För hello följer anvisningarna i följande fält:</span><span class="sxs-lookup"><span data-stu-id="c4210-148">For hello following fields, follow these instructions:</span></span>
   
   * <span data-ttu-id="c4210-149">**Könamnet**: hello samma namn som hello namn du angav när du skapade hello kö i [komma igång med Service Bus-köer] [ servicebus-getstarted] (inte hello namn hello service bus).</span><span class="sxs-lookup"><span data-stu-id="c4210-149">**Queue name**: hello same name as hello name you entered when you created hello queue in [Get Started with Service Bus Queues][servicebus-getstarted] (not hello name of hello service bus).</span></span> <span data-ttu-id="c4210-150">Kontrollera att du använder hello kö som är anslutna toohello Stream Analytics utdata.</span><span class="sxs-lookup"><span data-stu-id="c4210-150">Make sure you use hello queue that is connected toohello Stream Analytics output.</span></span>
   * <span data-ttu-id="c4210-151">**Service Bus-anslutning**: Välj **lägga till en anslutningssträng**.</span><span class="sxs-lookup"><span data-stu-id="c4210-151">**Service Bus connection**: Select **Add a connection string**.</span></span> <span data-ttu-id="c4210-152">toofind hello anslutningssträngen, gå toohello klassiska portal, Välj **Service Bus**, hello service bus som du skapade och **ANSLUTNINGSINFORMATIONEN** längst hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="c4210-152">toofind hello connection string, go toohello classic portal, select **Service Bus**, hello service bus you created, and **CONNECTION INFORMATION** at hello bottom of hello screen.</span></span> <span data-ttu-id="c4210-153">Kontrollera att hello huvudskärmen på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="c4210-153">Make sure you are on hello main screen on this page.</span></span> <span data-ttu-id="c4210-154">Kopiera och klistra in hello anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="c4210-154">Copy and paste hello connection string.</span></span> <span data-ttu-id="c4210-155">Känna sig fria tooenter alla anslutningsnamn.</span><span class="sxs-lookup"><span data-stu-id="c4210-155">Feel free tooenter any connection name.</span></span>
     
       ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * <span data-ttu-id="c4210-157">**AccessRights**: Välj **hantera**</span><span class="sxs-lookup"><span data-stu-id="c4210-157">**AccessRights**: Choose **Manage**</span></span>
3. <span data-ttu-id="c4210-158">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="c4210-158">Click **Create**</span></span>

## <a name="writing-tooredis-cache"></a><span data-ttu-id="c4210-159">Skrivning tooRedis Cache</span><span class="sxs-lookup"><span data-stu-id="c4210-159">Writing tooRedis Cache</span></span>
<span data-ttu-id="c4210-160">Vi har nu skapat en Azure-funktion som läser från en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="c4210-160">We have now created an Azure Function that reads from a Service Bus Queue.</span></span> <span data-ttu-id="c4210-161">Återstår toodo är att använda våra funktionen toowrite denna data toohello Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="c4210-161">All that is left toodo is use our Function toowrite this data toohello Redis Cache.</span></span> 

1. <span data-ttu-id="c4210-162">Markera den nyligen skapade **ServiceBusQueueTrigger**, och klicka på **fungerar appinställningar** på hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="c4210-162">Select your newly created **ServiceBusQueueTrigger**, and click **Function app settings** on hello top right corner.</span></span> <span data-ttu-id="c4210-163">Välj **gå tooApp Inställningar > Inställningar > programinställningar**</span><span class="sxs-lookup"><span data-stu-id="c4210-163">Select **Go tooApp Service Settings > Settings > Application settings**</span></span>
2. <span data-ttu-id="c4210-164">I hello anslutning strängar avsnitt, skapar du ett namn i hello **namn** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c4210-164">In hello Connection strings section, create a name in hello **Name** section.</span></span> <span data-ttu-id="c4210-165">Klistra in primära hello anslutningssträng som du hittade i hello **skapa Redis-Cache** steget till hello **värdet** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c4210-165">Paste hello primary connection string you found in hello **Create a Redis Cache** step into hello **Value** section.</span></span> <span data-ttu-id="c4210-166">Välj **anpassad** står det **SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="c4210-166">Select **Custom** where it says **SQL Database**.</span></span>
3. <span data-ttu-id="c4210-167">Klicka på **spara** hello överst.</span><span class="sxs-lookup"><span data-stu-id="c4210-167">Click **Save** at hello top.</span></span>
   
    ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/function-connection-string.png)
4. <span data-ttu-id="c4210-169">Gå nu tillbaka toohello App Service-inställningar och välj **Verktyg > App Service Editor (förhandsgranskning) > på > Gå**.</span><span class="sxs-lookup"><span data-stu-id="c4210-169">Now go back toohello App Service Settings and select **Tools > App Service Editor (Preview) > On > Go**.</span></span>
   
    ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/app-service-editor.png)
5. <span data-ttu-id="c4210-171">I ett redigeringsprogram, skapar du en JSON-fil med namnet **project.json** med hello följa och spara den tooyour lokal disk.</span><span class="sxs-lookup"><span data-stu-id="c4210-171">In an editor of your choice, create a JSON file named **project.json** with hello following and save it tooyour local disk.</span></span>
   
        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }
6. <span data-ttu-id="c4210-172">Ladda upp den här filen till hello rotkatalogen för din funktion (inte WWWROOT).</span><span class="sxs-lookup"><span data-stu-id="c4210-172">Upload this file into hello root directory of your function (not WWWROOT).</span></span> <span data-ttu-id="c4210-173">Du bör se en fil med namnet **project.lock.json** visas automatiskt, bekräfta att hello Nuget paket ”StackExchange.Redis” och ”Newtonsoft.Json” har importerats.</span><span class="sxs-lookup"><span data-stu-id="c4210-173">You should see a file named **project.lock.json** automatically appear, confirming that hello Nuget packages “StackExchange.Redis” and “Newtonsoft.Json” have been imported.</span></span>
7. <span data-ttu-id="c4210-174">I hello **run.csx** fil, Ersätt hello förgenererade koden med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="c4210-174">In hello **run.csx** file, replace hello pre-generated code with hello following code.</span></span> <span data-ttu-id="c4210-175">Ersätt ”ansluten NAME” i hello lazyConnection funktionen med hello-namnet som du skapade i steg 2 i **lagra data i hello Redis-cache**.</span><span class="sxs-lookup"><span data-stu-id="c4210-175">In hello lazyConnection function, replace “CONN NAME” with hello name you created in step 2 of **Store data into hello Redis cache**.</span></span>

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers tooa property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract hello time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using hello cache object...
        // Simple put of integral data types into hello cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from hello cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect toohello Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-hello-stream-analytics-job"></a><span data-ttu-id="c4210-176">Starta hello Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="c4210-176">Start hello Stream Analytics job</span></span>
1. <span data-ttu-id="c4210-177">Starta hello telcodatagen.exe programmet.</span><span class="sxs-lookup"><span data-stu-id="c4210-177">Start hello telcodatagen.exe application.</span></span> <span data-ttu-id="c4210-178">hello syntax är följande:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span><span class="sxs-lookup"><span data-stu-id="c4210-178">hello usage is as follows: ````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span></span>
2. <span data-ttu-id="c4210-179">Hello Stream Analytics-jobbet bladet hello-portalen klickar du på **starta** hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="c4210-179">From hello Stream Analytics Job blade in hello portal, click **Start** at hello top of hello page.</span></span>
   
    ![Skärmbild av startjobb](./media/stream-analytics-functions-redis/starting-job.png)
3. <span data-ttu-id="c4210-181">I hello **startjobb** bladet som visas, väljer **nu** och klicka sedan på hello **starta** knappen längst ned hello hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="c4210-181">In hello **Start job** blade that appears, select **Now** and then click hello **Start** button at hello bottom of hello screen.</span></span> <span data-ttu-id="c4210-182">hello jobbets status har ändrats tooStarting och när vissa tid ändringar tooRunning.</span><span class="sxs-lookup"><span data-stu-id="c4210-182">hello job status changes tooStarting and after some time changes tooRunning.</span></span>
   
    ![Skärmbild av valet av tid för start jobb](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a><span data-ttu-id="c4210-184">Kör lösningen och kontrollera resultaten</span><span class="sxs-lookup"><span data-stu-id="c4210-184">Run solution and check results</span></span>
<span data-ttu-id="c4210-185">Gå tillbaka tooyour **ServiceBusQueueTrigger** sidan du bör nu se logga instruktioner.</span><span class="sxs-lookup"><span data-stu-id="c4210-185">Going back tooyour **ServiceBusQueueTrigger** page, you should now see log statements.</span></span> <span data-ttu-id="c4210-186">De här loggarna visar att du har fått något från hello Service Bus-kö, placera den i hello-databas och hämtas ut använda hello tid som hello nyckel!</span><span class="sxs-lookup"><span data-stu-id="c4210-186">These logs show that you got something from hello Service Bus Queue, put it into hello database, and fetched it out using hello time as hello key!</span></span>

<span data-ttu-id="c4210-187">tooverify som dina data i Redis-cache gå tooyour Redis cache-sidan i nya hello-portalen (som visas i föregående hello [skapa ett Azure Redis-Cache](#Create-an-Azure-Redis-Cache) steg) och välj konsolen.</span><span class="sxs-lookup"><span data-stu-id="c4210-187">tooverify that your data is in your Redis cache, go tooyour Redis cache page in hello new portal (as shown in hello preceding [Create an Azure Redis Cache](#Create-an-Azure-Redis-Cache) step) and select Console.</span></span>

<span data-ttu-id="c4210-188">Nu kan du skriva Redis kommandon tooconfirm att data är i själva verket i hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="c4210-188">Now you can write Redis commands tooconfirm that data is in fact in hello cache.</span></span>

![Skärmbild av Redis-konsolen](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a><span data-ttu-id="c4210-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c4210-190">Next steps</span></span>
<span data-ttu-id="c4210-191">Vi är nöjda med hello nya saker Azure Functions Stream analytics kan göra tillsammans och vi hoppas det här låser upp nya möjligheter för dig.</span><span class="sxs-lookup"><span data-stu-id="c4210-191">We’re excited about hello new things Azure Functions and Stream analytics can do together, and we hope this unlocks new possibilities for you.</span></span> <span data-ttu-id="c4210-192">Om du har feedback på vad du vill att nästa anser ledigt toouse hello [Azure UserVoice-webbplatsen](https://feedback.azure.com/forums/270577-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="c4210-192">If you have any feedback on what you want next, feel free toouse hello [Azure UserVoice site](https://feedback.azure.com/forums/270577-stream-analytics).</span></span>

<span data-ttu-id="c4210-193">Om du är ny Microsoft Azure kan vi ber dig tootry den ut genom att registrera dig för en [kostnadsfria Azure utvärderingskonto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c4210-193">If you are new Microsoft Azure, we invite you tootry it out by signing up for a [free Azure trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="c4210-194">Om du är ny tooStream Analytics, så vi ber dig för[skapa ditt första Stream Analytics-jobb](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="c4210-194">If you are new tooStream Analytics, then we invite you too[create your first Stream Analytics job](stream-analytics-create-a-job.md).</span></span>

<span data-ttu-id="c4210-195">Om du behöver hjälp eller har frågor, gör ett inlägg i [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) eller [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forum.</span><span class="sxs-lookup"><span data-stu-id="c4210-195">If you need any help or have questions, post on [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) or [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forums.</span></span> 

<span data-ttu-id="c4210-196">Du kan också se hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="c4210-196">You can also see hello following resources:</span></span>

* [<span data-ttu-id="c4210-197">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="c4210-197">Azure Functions developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="c4210-198">Azure Functions C# för utvecklare</span><span class="sxs-lookup"><span data-stu-id="c4210-198">Azure Functions C# developer reference</span></span>](../azure-functions/functions-reference-csharp.md)
* [<span data-ttu-id="c4210-199">Azure Functions F # för utvecklare</span><span class="sxs-lookup"><span data-stu-id="c4210-199">Azure Functions F# developer reference</span></span>](../azure-functions/functions-reference-fsharp.md)
* [<span data-ttu-id="c4210-200">Azure Functions NodeJS för utvecklare</span><span class="sxs-lookup"><span data-stu-id="c4210-200">Azure Functions NodeJS developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="c4210-201">Azure Functions-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="c4210-201">Azure Functions triggers and bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
* [<span data-ttu-id="c4210-202">Hur toomonitor Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="c4210-202">How toomonitor Azure Redis Cache</span></span>](../redis-cache/cache-how-to-monitor.md)

<span data-ttu-id="c4210-203">toostay uppdaterad på alla hello nyheter och funktioner, Följ [ @AzureStreaming ](https://twitter.com/AzureStreaming) på Twitter.</span><span class="sxs-lookup"><span data-stu-id="c4210-203">toostay up-to-date on all hello latest news and features, follow [@AzureStreaming](https://twitter.com/AzureStreaming) on Twitter.</span></span>

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
