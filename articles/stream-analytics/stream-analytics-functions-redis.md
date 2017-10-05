---
title: "Strömma Analytics realtid bearbetning för Azure Functions | Microsoft Docs"
description: "Lär dig hur du använder en Azure-funktion anslutna en Service Bus-kö, för att fylla i en Azure Redis-Cache från utdata från ett Stream Analytics-jobb."
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
ms.openlocfilehash: ad14cc858ff513573e2718a26a9ab5c524e1adc6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a><span data-ttu-id="95ad6-104">Hur du lagrar data från Azure Stream Analytics i en Azure Redis-Cache med hjälp av Azure-funktioner</span><span class="sxs-lookup"><span data-stu-id="95ad6-104">How to store data from Azure Stream Analytics in an Azure Redis Cache using Azure Functions</span></span>
<span data-ttu-id="95ad6-105">Azure Stream Analytics kan du snabbt utveckla och distribuera prisvärda lösningar för att få realtidsinsikter från enheter, sensorer, infrastruktur, och program eller en dataström.</span><span class="sxs-lookup"><span data-stu-id="95ad6-105">Azure Stream Analytics lets you rapidly develop and deploy low-cost solutions to gain real-time insights from devices, sensors, infrastructure, and applications, or any stream of data.</span></span> <span data-ttu-id="95ad6-106">Den gör olika användningsfall, till exempel realtid hantering och övervakning kommandot och, att upptäcka bedrägerier, anslutna bilar och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="95ad6-106">It enables various use cases such as real-time management and monitoring, command and control, fraud detection, connected cars and many more.</span></span> <span data-ttu-id="95ad6-107">I många sådana scenarier kanske du vill lagra data i en distribuerad datalager, till exempel en Azure Redis-cache för utdata av Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="95ad6-107">In many such scenarios, you may want to store data outputted by Azure Stream Analytics into a distributed data store such as an Azure Redis cache.</span></span>

<span data-ttu-id="95ad6-108">Anta att du är en del av en telekommunikation företag.</span><span class="sxs-lookup"><span data-stu-id="95ad6-108">Suppose you are part of a telecommunications company.</span></span> <span data-ttu-id="95ad6-109">Du försöker identifiera SIM bedrägeri där flera anrop som kommer från samma identitet, på samma gång, men i olika geografiskt platser.</span><span class="sxs-lookup"><span data-stu-id="95ad6-109">You are trying to detect SIM fraud where multiple calls coming from the same identity, at the same time, but in different geographically locations.</span></span> <span data-ttu-id="95ad6-110">Du har gett med att lagra alla potentiella bedrägliga telefonsamtal i en Azure Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="95ad6-110">You are tasked with storing all the potential fraudulent phone calls in an Azure Redis cache.</span></span> <span data-ttu-id="95ad6-111">I den här bloggen ger vi riktlinjer för hur du lätt kan utföra uppgiften.</span><span class="sxs-lookup"><span data-stu-id="95ad6-111">In this blog, we provide guidance on how you can easily complete your task.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="95ad6-112">Krav</span><span class="sxs-lookup"><span data-stu-id="95ad6-112">Prerequisites</span></span>
<span data-ttu-id="95ad6-113">Slutför den [realtid att upptäcka bedrägerier] [ fraud-detection] hanteringspaketen för ASA</span><span class="sxs-lookup"><span data-stu-id="95ad6-113">Complete the [Real-time Fraud Detection][fraud-detection] walk-through for ASA</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="95ad6-114">Arkitektur, översikt</span><span class="sxs-lookup"><span data-stu-id="95ad6-114">Architecture Overview</span></span>
![Skärmbild av arkitektur](./media/stream-analytics-functions-redis/architecture-overview.png)

<span data-ttu-id="95ad6-116">I föregående bild visas kan Stream Analytics strömning indata för att fråga och skickas till utdata.</span><span class="sxs-lookup"><span data-stu-id="95ad6-116">As shown in the preceding figure, Stream Analytics allows streaming input data to be queried and sent to an output.</span></span> <span data-ttu-id="95ad6-117">Baserat på resultatet kan Azure Functions sedan utlösa någon typ av händelse.</span><span class="sxs-lookup"><span data-stu-id="95ad6-117">Based on the output, Azure Functions can then trigger some type of event.</span></span> 

<span data-ttu-id="95ad6-118">I den här bloggen fokusera på Azure Functions en del av denna pipeline eller mer specifikt utlösa en händelse som lagrar falska data till cachen.</span><span class="sxs-lookup"><span data-stu-id="95ad6-118">In this blog, we focus on the Azure Functions part of this pipeline, or more specifically the triggering of an event that stores fraudulent data into the cache.</span></span>
<span data-ttu-id="95ad6-119">När du har slutfört den [realtid att upptäcka bedrägerier] [ fraud-detection] kursen har du indata (en händelsehubb), en fråga och utdata (blob storage) redan konfigurerad och körs.</span><span class="sxs-lookup"><span data-stu-id="95ad6-119">After completing the [Real-time Fraud Detection][fraud-detection] tutorial, you have an input (an event hub), a query, and an output (blob storage) already configured and running.</span></span> <span data-ttu-id="95ad6-120">I den här bloggen ändra vi utdata om du vill använda en Service Bus-kö i stället.</span><span class="sxs-lookup"><span data-stu-id="95ad6-120">In this blog, we change the output to use a Service Bus Queue instead.</span></span> <span data-ttu-id="95ad6-121">Sedan kan ansluta vi en Azure-funktion för kön.</span><span class="sxs-lookup"><span data-stu-id="95ad6-121">After that, we connect an Azure Function to this queue.</span></span> 

## <a name="create-and-connect-a-service-bus-queue-output"></a><span data-ttu-id="95ad6-122">Skapa och koppla en Service Bus-kö-utdata</span><span class="sxs-lookup"><span data-stu-id="95ad6-122">Create and connect a Service Bus Queue output</span></span>
<span data-ttu-id="95ad6-123">Om du vill skapa en Service Bus-kö följa steg 1 och 2 i avsnittet .NET i [komma igång med Service Bus-köer][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="95ad6-123">To create a Service Bus Queue, follow steps 1 and 2 of the .NET section in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span>
<span data-ttu-id="95ad6-124">Nu ska vi ansluta kön till Stream Analytics-jobbet har skapats i tidigare bedrägeri identifiering genomgången.</span><span class="sxs-lookup"><span data-stu-id="95ad6-124">Now let's connect the queue to the Stream Analytics job that was created in the earlier fraud detection walk-through.</span></span>

1. <span data-ttu-id="95ad6-125">I Azure-portalen går du till den **utdata** bladet för ditt projekt och välj **Lägg till** överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="95ad6-125">In the Azure portal, go to the **Outputs** blade of your job and select **Add** at the top of the page.</span></span>
   
    ![Lägger till utdata](./media/stream-analytics-functions-redis/adding-outputs.png)
2. <span data-ttu-id="95ad6-127">Välj **Service Bus-kö** som den **Sink** och följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="95ad6-127">Choose **Service Bus Queue** as the **Sink** and follow the instructions on the screen.</span></span> <span data-ttu-id="95ad6-128">Kontrollera att namnområdet för Service Bus-kö som du skapade i [komma igång med Service Bus-köer][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="95ad6-128">Be sure to choose the namespace of the Service Bus Queue you created in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span> <span data-ttu-id="95ad6-129">Klicka på knappen ”höger” när du är klar.</span><span class="sxs-lookup"><span data-stu-id="95ad6-129">Click the "right" button when you are finished.</span></span>
3. <span data-ttu-id="95ad6-130">Ange följande värden:</span><span class="sxs-lookup"><span data-stu-id="95ad6-130">Specify the following values:</span></span>
   
   * <span data-ttu-id="95ad6-131">**Händelsen serialiseraren Format**: JSON</span><span class="sxs-lookup"><span data-stu-id="95ad6-131">**Event Serializer Format**: JSON</span></span>
   * <span data-ttu-id="95ad6-132">**Kodning**: UTF8</span><span class="sxs-lookup"><span data-stu-id="95ad6-132">**Encoding**: UTF8</span></span>
   * <span data-ttu-id="95ad6-133">**FORMATET**: Radseparerade</span><span class="sxs-lookup"><span data-stu-id="95ad6-133">**FORMAT**: Line separated</span></span>
4. <span data-ttu-id="95ad6-134">Klicka på den **skapa** knappen för att lägga till den här datakällan och kontrollera att Stream Analytics kan ansluta till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="95ad6-134">Click the **Create** button to add this source and to verify that Stream Analytics can successfully connect to the storage account.</span></span>
5. <span data-ttu-id="95ad6-135">I den **frågan** fliken ersätter den aktuella frågan med följande.</span><span class="sxs-lookup"><span data-stu-id="95ad6-135">In the **Query** tab, replace the current query with the following.</span></span> <span data-ttu-id="95ad6-136">Ersätt * [YOUR SERVICE BUS NAME] * med utdatanamnet som du skapade i steg 3.</span><span class="sxs-lookup"><span data-stu-id="95ad6-136">Replace *[YOUR SERVICE BUS NAME] * with the output name you created in step 3.</span></span> 
   
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

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="95ad6-137">Skapa en Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="95ad6-137">Create an Azure Redis Cache</span></span>
<span data-ttu-id="95ad6-138">Skapa ett Azure Redis-cache genom att följa avsnittet .NET i [så Använd Azure Redis-Cache] [ use-rediscache] tills avsnittet kallas ***konfigurera cache-klienter***.</span><span class="sxs-lookup"><span data-stu-id="95ad6-138">Create an Azure Redis cache by following the .NET section in [How to Use Azure Redis Cache][use-rediscache] until the section called ***Configure the cache clients***.</span></span>
<span data-ttu-id="95ad6-139">När borttagningen är klar har du en ny Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="95ad6-139">Once complete, you have a new Redis Cache.</span></span> <span data-ttu-id="95ad6-140">Under **alla inställningar**väljer **åtkomstnycklar** och notera den ***primära anslutningssträngen***.</span><span class="sxs-lookup"><span data-stu-id="95ad6-140">Under **All settings**, select **Access keys** and note down the ***Primary connection string***.</span></span>

![Skärmbild av arkitektur](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a><span data-ttu-id="95ad6-142">Skapa en Azure-funktion</span><span class="sxs-lookup"><span data-stu-id="95ad6-142">Create an Azure Function</span></span>
<span data-ttu-id="95ad6-143">Följ [skapa din första Azure-funktion] [ functions-getstarted] kursen och kom igång med Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="95ad6-143">Follow [Create your first Azure Function][functions-getstarted] tutorial to get started with Azure Functions.</span></span> <span data-ttu-id="95ad6-144">Om du redan har en Azure funktion som du vill använda sedan gå vidare till [skrivning till Redis-Cache](#Writing-to-Redis-Cache)</span><span class="sxs-lookup"><span data-stu-id="95ad6-144">If you already have an Azure function you would like to use, then skip ahead to [Writing to Redis Cache](#Writing-to-Redis-Cache)</span></span>

1. <span data-ttu-id="95ad6-145">Välj Apptjänster från det vänstra navigeringsfönstret i portalen och sedan på ditt Azure funktionsnamn app att hämta funktionens app webbplats.</span><span class="sxs-lookup"><span data-stu-id="95ad6-145">In the portal, select App Services from the left-hand navigation, then click your Azure function app name to get to the Function's app website.</span></span>
    <span data-ttu-id="95ad6-146">![Skärmbild av listan över appar services-funktionen](./media/stream-analytics-functions-redis/app-services-function-list.png)</span><span class="sxs-lookup"><span data-stu-id="95ad6-146">![Screenshot of app services function list](./media/stream-analytics-functions-redis/app-services-function-list.png)</span></span>
2. <span data-ttu-id="95ad6-147">Klicka på **nya funktionen > ServiceBusQueueTrigger – C#**.</span><span class="sxs-lookup"><span data-stu-id="95ad6-147">Click **New Function > ServiceBusQueueTrigger – C#**.</span></span> <span data-ttu-id="95ad6-148">Följ dessa instruktioner för följande fält:</span><span class="sxs-lookup"><span data-stu-id="95ad6-148">For the following fields, follow these instructions:</span></span>
   
   * <span data-ttu-id="95ad6-149">**Könamnet**: samma namn som det namn du angav när du skapade i kön i [komma igång med Service Bus-köer] [ servicebus-getstarted] (inte namnet på service bus).</span><span class="sxs-lookup"><span data-stu-id="95ad6-149">**Queue name**: The same name as the name you entered when you created the queue in [Get Started with Service Bus Queues][servicebus-getstarted] (not the name of the service bus).</span></span> <span data-ttu-id="95ad6-150">Kontrollera att du använder den kö som är ansluten till Stream Analytics-utdata.</span><span class="sxs-lookup"><span data-stu-id="95ad6-150">Make sure you use the queue that is connected to the Stream Analytics output.</span></span>
   * <span data-ttu-id="95ad6-151">**Service Bus-anslutning**: Välj **lägga till en anslutningssträng**.</span><span class="sxs-lookup"><span data-stu-id="95ad6-151">**Service Bus connection**: Select **Add a connection string**.</span></span> <span data-ttu-id="95ad6-152">För att hitta anslutningssträngen, gå till den klassiska portalen väljer **Service Bus**, service bus som du skapade och **ANSLUTNINGSINFORMATIONEN** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="95ad6-152">To find the connection string, go to the classic portal, select **Service Bus**, the service bus you created, and **CONNECTION INFORMATION** at the bottom of the screen.</span></span> <span data-ttu-id="95ad6-153">Kontrollera att du är i huvudfönstret på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="95ad6-153">Make sure you are on the main screen on this page.</span></span> <span data-ttu-id="95ad6-154">Kopiera och klistra in anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="95ad6-154">Copy and paste the connection string.</span></span> <span data-ttu-id="95ad6-155">Du kan ange ett anslutningsnamn.</span><span class="sxs-lookup"><span data-stu-id="95ad6-155">Feel free to enter any connection name.</span></span>
     
       ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * <span data-ttu-id="95ad6-157">**AccessRights**: Välj **hantera**</span><span class="sxs-lookup"><span data-stu-id="95ad6-157">**AccessRights**: Choose **Manage**</span></span>
3. <span data-ttu-id="95ad6-158">Klicka på **Skapa**</span><span class="sxs-lookup"><span data-stu-id="95ad6-158">Click **Create**</span></span>

## <a name="writing-to-redis-cache"></a><span data-ttu-id="95ad6-159">Skrivning till Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="95ad6-159">Writing to Redis Cache</span></span>
<span data-ttu-id="95ad6-160">Vi har nu skapat en Azure-funktion som läser från en Service Bus-kö.</span><span class="sxs-lookup"><span data-stu-id="95ad6-160">We have now created an Azure Function that reads from a Service Bus Queue.</span></span> <span data-ttu-id="95ad6-161">Återstår gör är vår funktionen för att skriva data till Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="95ad6-161">All that is left to do is use our Function to write this data to the Redis Cache.</span></span> 

1. <span data-ttu-id="95ad6-162">Markera den nyligen skapade **ServiceBusQueueTrigger**, och klicka på **fungerar appinställningar** på övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="95ad6-162">Select your newly created **ServiceBusQueueTrigger**, and click **Function app settings** on the top right corner.</span></span> <span data-ttu-id="95ad6-163">Välj **går du till App Service-Inställningar > Inställningar > programinställningar**</span><span class="sxs-lookup"><span data-stu-id="95ad6-163">Select **Go to App Service Settings > Settings > Application settings**</span></span>
2. <span data-ttu-id="95ad6-164">I avsnittet anslutning strängar skapar du ett namn i den **namn** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="95ad6-164">In the Connection strings section, create a name in the **Name** section.</span></span> <span data-ttu-id="95ad6-165">Klistra in den primära anslutningssträng som du hittade i den **skapa Redis-Cache** steg i den **värdet** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="95ad6-165">Paste the primary connection string you found in the **Create a Redis Cache** step into the **Value** section.</span></span> <span data-ttu-id="95ad6-166">Välj **anpassad** står det **SQL-databas**.</span><span class="sxs-lookup"><span data-stu-id="95ad6-166">Select **Custom** where it says **SQL Database**.</span></span>
3. <span data-ttu-id="95ad6-167">Klicka på **spara** längst upp.</span><span class="sxs-lookup"><span data-stu-id="95ad6-167">Click **Save** at the top.</span></span>
   
    ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/function-connection-string.png)
4. <span data-ttu-id="95ad6-169">Nu gå tillbaka till App Service-inställningar och välj **Verktyg > App Service Editor (förhandsgranskning) > på > Gå**.</span><span class="sxs-lookup"><span data-stu-id="95ad6-169">Now go back to the App Service Settings and select **Tools > App Service Editor (Preview) > On > Go**.</span></span>
   
    ![Skärmbild av service bus-anslutning](./media/stream-analytics-functions-redis/app-service-editor.png)
5. <span data-ttu-id="95ad6-171">I ett redigeringsprogram, skapar du en JSON-fil med namnet **project.json** med följande och spara den till den lokala hårddisken.</span><span class="sxs-lookup"><span data-stu-id="95ad6-171">In an editor of your choice, create a JSON file named **project.json** with the following and save it to your local disk.</span></span>
   
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
6. <span data-ttu-id="95ad6-172">Ladda upp den här filen till rotkatalogen för din funktion (inte WWWROOT).</span><span class="sxs-lookup"><span data-stu-id="95ad6-172">Upload this file into the root directory of your function (not WWWROOT).</span></span> <span data-ttu-id="95ad6-173">Du bör se en fil med namnet **project.lock.json** visas automatiskt bekräftar att Nuget-paketen ”StackExchange.Redis” och ”Newtonsoft.Json” har importerats.</span><span class="sxs-lookup"><span data-stu-id="95ad6-173">You should see a file named **project.lock.json** automatically appear, confirming that the Nuget packages “StackExchange.Redis” and “Newtonsoft.Json” have been imported.</span></span>
7. <span data-ttu-id="95ad6-174">I den **run.csx** fil, Ersätt förgenererade koden med följande kod.</span><span class="sxs-lookup"><span data-stu-id="95ad6-174">In the **run.csx** file, replace the pre-generated code with the following code.</span></span> <span data-ttu-id="95ad6-175">Ersätt ”ansluten NAME” i lazyConnection-funktionen med namnet som du skapade i steg 2 i **lagra data i Redis-cache**.</span><span class="sxs-lookup"><span data-stu-id="95ad6-175">In the lazyConnection function, replace “CONN NAME” with the name you created in step 2 of **Store data into the Redis cache**.</span></span>

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
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

## <a name="start-the-stream-analytics-job"></a><span data-ttu-id="95ad6-176">Starta Stream Analytics-jobbet</span><span class="sxs-lookup"><span data-stu-id="95ad6-176">Start the Stream Analytics job</span></span>
1. <span data-ttu-id="95ad6-177">Starta programmet telcodatagen.exe.</span><span class="sxs-lookup"><span data-stu-id="95ad6-177">Start the telcodatagen.exe application.</span></span> <span data-ttu-id="95ad6-178">Användningen är följande:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span><span class="sxs-lookup"><span data-stu-id="95ad6-178">The usage is as follows: ````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span></span>
2. <span data-ttu-id="95ad6-179">I bladet Stream Analytics-jobbet i portalen klickar du på **starta** överst på sidan.</span><span class="sxs-lookup"><span data-stu-id="95ad6-179">From the Stream Analytics Job blade in the portal, click **Start** at the top of the page.</span></span>
   
    ![Skärmbild av startjobb](./media/stream-analytics-functions-redis/starting-job.png)
3. <span data-ttu-id="95ad6-181">I den **startjobb** bladet som visas, väljer **nu** och klicka sedan på den **starta** längst ned på skärmen.</span><span class="sxs-lookup"><span data-stu-id="95ad6-181">In the **Start job** blade that appears, select **Now** and then click the **Start** button at the bottom of the screen.</span></span> <span data-ttu-id="95ad6-182">Jobbet status ändras till Start och efter vissa tidsändringar körs.</span><span class="sxs-lookup"><span data-stu-id="95ad6-182">The job status changes to Starting and after some time changes to Running.</span></span>
   
    ![Skärmbild av valet av tid för start jobb](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a><span data-ttu-id="95ad6-184">Kör lösningen och kontrollera resultaten</span><span class="sxs-lookup"><span data-stu-id="95ad6-184">Run solution and check results</span></span>
<span data-ttu-id="95ad6-185">Gå tillbaka till din **ServiceBusQueueTrigger** sidan du bör nu se logga instruktioner.</span><span class="sxs-lookup"><span data-stu-id="95ad6-185">Going back to your **ServiceBusQueueTrigger** page, you should now see log statements.</span></span> <span data-ttu-id="95ad6-186">De här loggarna visar att du har fått något från Service Bus-kö, placera den i databasen och hämtas ut på tid som nyckel!</span><span class="sxs-lookup"><span data-stu-id="95ad6-186">These logs show that you got something from the Service Bus Queue, put it into the database, and fetched it out using the time as the key!</span></span>

<span data-ttu-id="95ad6-187">Kontrollera att dina data är i Redis-cache, gå till sidan Redis-cache i den nya portalen (som visas i den föregående [skapa ett Azure Redis-Cache](#Create-an-Azure-Redis-Cache) steg) och välj konsolen.</span><span class="sxs-lookup"><span data-stu-id="95ad6-187">To verify that your data is in your Redis cache, go to your Redis cache page in the new portal (as shown in the preceding [Create an Azure Redis Cache](#Create-an-Azure-Redis-Cache) step) and select Console.</span></span>

<span data-ttu-id="95ad6-188">Nu kan du skriva Redis-kommandon för att bekräfta att data är i själva verket i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="95ad6-188">Now you can write Redis commands to confirm that data is in fact in the cache.</span></span>

![Skärmbild av Redis-konsolen](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a><span data-ttu-id="95ad6-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95ad6-190">Next steps</span></span>
<span data-ttu-id="95ad6-191">Vi är nöjda med nya saker Azure Functions Stream analytics kan göra tillsammans och vi hoppas det här låser upp nya möjligheter för dig.</span><span class="sxs-lookup"><span data-stu-id="95ad6-191">We’re excited about the new things Azure Functions and Stream analytics can do together, and we hope this unlocks new possibilities for you.</span></span> <span data-ttu-id="95ad6-192">Om du har feedback på vad du vill nu passa på att använda den [Azure UserVoice-webbplatsen](https://feedback.azure.com/forums/270577-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="95ad6-192">If you have any feedback on what you want next, feel free to use the [Azure UserVoice site](https://feedback.azure.com/forums/270577-stream-analytics).</span></span>

<span data-ttu-id="95ad6-193">Om du är ny Microsoft Azure kan vi bjuda in dig på detta genom att registrera dig för en [kostnadsfria Azure utvärderingskonto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="95ad6-193">If you are new Microsoft Azure, we invite you to try it out by signing up for a [free Azure trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="95ad6-194">Om du är nybörjare på Stream Analytics och gärna [skapa ditt första Stream Analytics-jobb](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="95ad6-194">If you are new to Stream Analytics, then we invite you to [create your first Stream Analytics job](stream-analytics-create-a-job.md).</span></span>

<span data-ttu-id="95ad6-195">Om du behöver hjälp eller har frågor, gör ett inlägg i [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) eller [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forum.</span><span class="sxs-lookup"><span data-stu-id="95ad6-195">If you need any help or have questions, post on [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) or [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forums.</span></span> 

<span data-ttu-id="95ad6-196">Du kan också se följande resurser:</span><span class="sxs-lookup"><span data-stu-id="95ad6-196">You can also see the following resources:</span></span>

* [<span data-ttu-id="95ad6-197">Azure Functions, info för utvecklare</span><span class="sxs-lookup"><span data-stu-id="95ad6-197">Azure Functions developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="95ad6-198">Azure Functions C# för utvecklare</span><span class="sxs-lookup"><span data-stu-id="95ad6-198">Azure Functions C# developer reference</span></span>](../azure-functions/functions-reference-csharp.md)
* [<span data-ttu-id="95ad6-199">Azure Functions F # för utvecklare</span><span class="sxs-lookup"><span data-stu-id="95ad6-199">Azure Functions F# developer reference</span></span>](../azure-functions/functions-reference-fsharp.md)
* [<span data-ttu-id="95ad6-200">Azure Functions NodeJS för utvecklare</span><span class="sxs-lookup"><span data-stu-id="95ad6-200">Azure Functions NodeJS developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="95ad6-201">Azure Functions-utlösare och bindningar</span><span class="sxs-lookup"><span data-stu-id="95ad6-201">Azure Functions triggers and bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
* [<span data-ttu-id="95ad6-202">Hur du övervakar Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="95ad6-202">How to monitor Azure Redis Cache</span></span>](../redis-cache/cache-how-to-monitor.md)

<span data-ttu-id="95ad6-203">Om du vill hålla dig uppdaterad på alla de senaste nyheterna och funktioner, Följ [ @AzureStreaming ](https://twitter.com/AzureStreaming) på Twitter.</span><span class="sxs-lookup"><span data-stu-id="95ad6-203">To stay up-to-date on all the latest news and features, follow [@AzureStreaming](https://twitter.com/AzureStreaming) on Twitter.</span></span>

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
