---
title: aaaCloud till enhet meddelanden med Azure IoT Hub (.NET) | Microsoft Docs
description: "Hur meddelanden toosend moln till enhet tooa enhet från en Azure IoT-hubb med hello Azure IoT SDK för .NET. Du ändrar en enhet tooreceive moln till enhet meddelanden och ändra en backend-app toosend moln till enhet hälsningsmeddelande."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: a31c05ed-6ec0-40f3-99ab-8fdd28b1a89a
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: f6a7618b164d95c8ddaf28943f244aeeb568217f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-messages-from-hello-cloud-tooyour-device-with-iot-hub-net"></a><span data-ttu-id="0f9ac-104">Skicka meddelanden från hello moln tooyour enhet med IoT Hub (.NET)</span><span class="sxs-lookup"><span data-stu-id="0f9ac-104">Send messages from hello cloud tooyour device with IoT Hub (.NET)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="0f9ac-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="0f9ac-105">Introduction</span></span>
<span data-ttu-id="0f9ac-106">Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="0f9ac-107">Hej [Kom igång med IoT-hubb] kursen visar hur toocreate en IoT-hubb etablera en enhetsidentitet i den och code en enhetsapp som skickar meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-107">hello [Get started with IoT Hub] tutorial shows how toocreate an IoT hub, provision a device identity in it, and code a device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="0f9ac-108">Den här kursen bygger på [Kom igång med IoT-hubb].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="0f9ac-109">Den visar hur till:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-109">It shows you how to:</span></span>

* <span data-ttu-id="0f9ac-110">Skicka meddelanden moln till enhet tooa enhet via IoT-hubb från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-110">From your solution back end, send cloud-to-device messages tooa single device through IoT Hub.</span></span>
* <span data-ttu-id="0f9ac-111">Ta emot meddelanden moln till enhet på en enhet.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="0f9ac-112">Begära leverans bekräftelse från din lösningens serverdel (*feedback*) för meddelanden som skickas tooa enheten från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent tooa device from IoT Hub.</span></span>

<span data-ttu-id="0f9ac-113">Du hittar mer information om moln till enhet meddelanden i hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-113">You can find more information on cloud-to-device messages in hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="0f9ac-114">Hello slutet av den här självstudiekursen kör du två .NET konsolappar:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-114">At hello end of this tutorial, you run two .NET console apps:</span></span>

* <span data-ttu-id="0f9ac-115">**SimulatedDevice**, en modifierad version av hello-app som skapats i [Kom igång med IoT-hubb], som ansluter tooyour IoT-hubb och tar emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-115">**SimulatedDevice**, a modified version of hello app created in [Get started with IoT Hub], which connects tooyour IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="0f9ac-116">**SendCloudToDevice**, som skickar ett moln-till-enhetsmeddelande toohello enheten appen via IoT-hubb och tar emot dess leverans bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-116">**SendCloudToDevice**, which sends a cloud-to-device message toohello device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="0f9ac-117">IoT-hubben har SDK stöd för många vilka plattformar och språk (inklusive C, Java och Javascript) via [Azure IoT-enhet SDK].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through [Azure IoT device SDKs].</span></span> <span data-ttu-id="0f9ac-118">Stegvisa instruktioner om hur tooconnect enhet toothis kursens kod och vanligtvis tooAzure IoT Hub, se hello [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-118">For step-by-step instructions on how tooconnect your device toothis tutorial's code, and generally tooAzure IoT Hub, see hello [IoT Hub developer guide].</span></span>
> 
> 

<span data-ttu-id="0f9ac-119">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-119">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="0f9ac-120">Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="0f9ac-120">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="0f9ac-121">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-121">An active Azure account.</span></span> <span data-ttu-id="0f9ac-122">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="0f9ac-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-hello-device-app"></a><span data-ttu-id="0f9ac-123">Ta emot meddelanden i hello enhetsapp</span><span class="sxs-lookup"><span data-stu-id="0f9ac-123">Receive messages in hello device app</span></span>
<span data-ttu-id="0f9ac-124">I det här avsnittet ska du ändra hello enhetsapp som du skapade i [Kom igång med IoT-hubb] tooreceive moln till enhet meddelanden från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-124">In this section, you'll modify hello device app you created in [Get started with IoT Hub] tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="0f9ac-125">I Visual Studio i hello **SimulatedDevice** projekt, Lägg till följande metod toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-125">In Visual Studio, in hello **SimulatedDevice** project, add hello following method toohello **Program** class.</span></span>
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud toodevice messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    <span data-ttu-id="0f9ac-126">Hej `ReceiveAsync` returnerar metoden asynkront emot hälsningsmeddelande hello samtidigt som den tas emot av hello enhet.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-126">hello `ReceiveAsync` method asynchronously returns hello received message at hello time that it is received by hello device.</span></span> <span data-ttu-id="0f9ac-127">Den returnerar *null* efter en specifiable tidsgräns (i det här fallet används hello standardvärdet på en minut).</span><span class="sxs-lookup"><span data-stu-id="0f9ac-127">It returns *null* after a specifiable timeout period (in this case, hello default of one minute is used).</span></span> <span data-ttu-id="0f9ac-128">När hello appen tar emot en *null*, den ska fortsätta toowait för nya meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-128">When hello app receives a *null*, it should continue toowait for new messages.</span></span> <span data-ttu-id="0f9ac-129">Det här kravet är hello orsak hello `if (receivedMessage == null) continue` rad.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-129">This requirement is hello reason for hello `if (receivedMessage == null) continue` line.</span></span>
   
    <span data-ttu-id="0f9ac-130">Hej anrop för`CompleteAsync()` meddelar IoT-hubb hello meddelandet har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-130">hello call too`CompleteAsync()` notifies IoT Hub that hello message has been successfully processed.</span></span> <span data-ttu-id="0f9ac-131">hello-meddelande kan tas bort på ett säkert sätt från hello enhetskö.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-131">hello message can be safely removed from hello device queue.</span></span> <span data-ttu-id="0f9ac-132">Om något hände förhindrade hello enheten appen från att slutföras hello bearbetningen av hello-meddelande ger IoT-hubb den igen.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-132">If something happened that prevented hello device app from completing hello processing of hello message, IoT Hub delivers it again.</span></span> <span data-ttu-id="0f9ac-133">Sedan är det viktigt att meddelandebehandling logiken i hello enhetsapp är *idempotent*, så att ta emot hello samma meddelande flera gånger producerar hello samma resultat.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-133">It is then important that message processing logic in hello device app is *idempotent*, so that receiving hello same message multiple times produces hello same result.</span></span> <span data-ttu-id="0f9ac-134">Ett program kan också tillfälligt Avbryt ett meddelande som resulterar i IoT-hubb behålla hello-meddelande i hello kö för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-134">An application can also temporarily abandon a message, which results in IoT hub retaining hello message in hello queue for future consumption.</span></span> <span data-ttu-id="0f9ac-135">Eller hello programmet kan avvisa ett meddelande som tar permanent bort hello-meddelande från kön hello.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-135">Or, hello application can reject a message, which permanently removes hello message from hello queue.</span></span> <span data-ttu-id="0f9ac-136">Mer information om hello moln-till-enhetsmeddelande livscykel finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-136">For more information about hello cloud-to-device message lifecycle, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0f9ac-137">När du använder HTTP i stället för MQTT eller AMQP som transport, hello `ReceiveAsync` metoden returnerar omedelbart.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-137">When using HTTP instead of MQTT or AMQP as a transport, hello `ReceiveAsync` method returns immediately.</span></span> <span data-ttu-id="0f9ac-138">hello stöds mönstret för moln till enhet meddelanden med HTTP är periodvis anslutna enheter som kontrollerar för meddelanden sällan (mindre än var 25: e minut).</span><span class="sxs-lookup"><span data-stu-id="0f9ac-138">hello supported pattern for cloud-to-device messages with HTTP is intermittently connected devices that check for messages infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="0f9ac-139">Utfärda mer HTTP får resultat i IoT-hubb bandbreddsbegränsning hello-begäranden.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-139">Issuing more HTTP receives results in IoT Hub throttling hello requests.</span></span> <span data-ttu-id="0f9ac-140">Mer information om hello skillnader mellan MQTT, AMQP och HTTP-stöd och IoT-hubb begränsning finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-140">For more information about hello differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 
2. <span data-ttu-id="0f9ac-141">Lägg till följande metod i hello hello **Main** metoden precis före hello `Console.ReadLine()` rad:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-141">Add hello following method in hello **Main** method, right before hello `Console.ReadLine()` line:</span></span>
   
        ReceiveC2dAsync();

> [!NOTE]
> <span data-ttu-id="0f9ac-142">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-142">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="0f9ac-143">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-143">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="0f9ac-144">Skicka ett meddelande moln till enhet</span><span class="sxs-lookup"><span data-stu-id="0f9ac-144">Send a cloud-to-device message</span></span>
<span data-ttu-id="0f9ac-145">I det här avsnittet kan du skriva en .NET-konsolapp som skickar meddelanden moln till enhet toohello enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-145">In this section, you write a .NET console app that sends cloud-to-device messages toohello device app.</span></span>

1. <span data-ttu-id="0f9ac-146">Skapa ett Visual C#-Skrivbordsapprojekt-projekt i hello aktuella Visual Studio-lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-146">In hello current Visual Studio solution, create a Visual C# Desktop App project by using hello **Console Application** project template.</span></span> <span data-ttu-id="0f9ac-147">Namnet hello projektet **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-147">Name hello project **SendCloudToDevice**.</span></span>
   
    ![Nytt projekt i Visual Studio][20]
2. <span data-ttu-id="0f9ac-149">Högerklicka på hello lösningen i Solution Explorer och klicka sedan på **hantera NuGet-paket för lösningen...** .</span><span class="sxs-lookup"><span data-stu-id="0f9ac-149">In Solution Explorer, right-click hello solution, and then click **Manage NuGet Packages for Solution...**.</span></span> 
   
    <span data-ttu-id="0f9ac-150">Den här åtgärden öppnar hello **hantera NuGet-paket** fönster.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-150">This action opens hello **Manage NuGet Packages** window.</span></span>
3. <span data-ttu-id="0f9ac-151">Sök efter **Microsoft.Azure.Devices**, klickar du på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-151">Search for **Microsoft.Azure.Devices**, click **Install**, and accept hello terms of use.</span></span> 
   
    <span data-ttu-id="0f9ac-152">Detta hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK NuGet-paketet].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-152">This downloads, installs, and adds a reference toohello [Azure IoT service SDK NuGet package].</span></span>

4. <span data-ttu-id="0f9ac-153">Lägg till följande hello `using` instruktionen hello överst i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-153">Add hello following `using` statement at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="0f9ac-154">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-154">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="0f9ac-155">Ersätt hello platshållare värdet med hello IoT hub-anslutningssträng från [Kom igång med IoT-hubb]:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-155">Substitute hello placeholder value with hello IoT hub connection string from [Get started with IoT Hub]:</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="0f9ac-156">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-156">Add hello following method toohello **Program** class:</span></span>
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud toodevice message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    <span data-ttu-id="0f9ac-157">Den här metoden skickar en ny moln-till-enhetsmeddelande toohello enhet med hello ID `myFirstDevice`.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-157">This method sends a new cloud-to-device message toohello device with hello ID, `myFirstDevice`.</span></span> <span data-ttu-id="0f9ac-158">Ändra den här parametern endast om den ändrats från hello som används i [Kom igång med IoT-hubb].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-158">Change this parameter only if you modified it from hello one used in [Get started with IoT Hub].</span></span>
7. <span data-ttu-id="0f9ac-159">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-159">Finally, add hello following lines toohello **Main** method:</span></span>
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key toosend a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="0f9ac-160">I Visual Studio högerklickar du på din lösning och välj **ange Startprojekt...** . Välj **flera Startprojekt**och välj hello **starta** åtgärd för **ReadDeviceToCloudMessages**, **SimulatedDevice**, och **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-160">From within Visual Studio, right-click your solution, and select **Set StartUp projects...**. Select **Multiple startup projects**, then select hello **Start** action for **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **SendCloudToDevice**.</span></span>
9. <span data-ttu-id="0f9ac-161">Tryck på **F5**.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-161">Press **F5**.</span></span> <span data-ttu-id="0f9ac-162">Alla tre program ska starta.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-162">All three applications should start.</span></span> <span data-ttu-id="0f9ac-163">Välj hello **SendCloudToDevice** windows och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-163">Select hello **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="0f9ac-164">Du bör se hello-meddelande tas emot av hello enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-164">You should see hello message being received by hello device app.</span></span>
   
   ![Appen tar emot meddelandet][21]

## <a name="receive-delivery-feedback"></a><span data-ttu-id="0f9ac-166">Ta emot leverans feedback</span><span class="sxs-lookup"><span data-stu-id="0f9ac-166">Receive delivery feedback</span></span>
<span data-ttu-id="0f9ac-167">Det är möjligt toorequest leverans (eller sista) bekräftelser från IoT-hubb för varje moln till enhet-meddelande.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-167">It is possible toorequest delivery (or expiration) acknowledgements from IoT Hub for each cloud-to-device message.</span></span> <span data-ttu-id="0f9ac-168">Det här alternativet aktiverar hello lösning serverdel tooeasily informera logik för återförsök eller ersättning.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-168">This option enables hello solution back end tooeasily inform retry or compensation logic.</span></span> <span data-ttu-id="0f9ac-169">Mer information om moln till enhet feedback finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-169">For more information about cloud-to-device feedback, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="0f9ac-170">I det här avsnittet kan du ändra hello **SendCloudToDevice** app toorequest feedback och tar emot det från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-170">In this section, you modify hello **SendCloudToDevice** app toorequest feedback, and receive it from IoT Hub.</span></span>

1. <span data-ttu-id="0f9ac-171">I Visual Studio i hello **SendCloudToDevice** projekt, Lägg till följande metod toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-171">In Visual Studio, in hello **SendCloudToDevice** project, add hello following method toohello **Program** class.</span></span>
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    <span data-ttu-id="0f9ac-172">Obs den här ta emot mönstret är hello samma en används tooreceive moln till enhet meddelanden från hello enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-172">Note this receive pattern is hello same one used tooreceive cloud-to-device messages from hello device app.</span></span>
2. <span data-ttu-id="0f9ac-173">Lägg till följande metod i hello hello **Main** metod, direkt efter hello `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` rad:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-173">Add hello following method in hello **Main** method, right after hello `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` line:</span></span>
   
        ReceiveFeedbackAsync();
3. <span data-ttu-id="0f9ac-174">toorequest feedback för hello leverans av meddelandet moln till enhet du har toospecify en egenskap i hello **SendCloudToDeviceMessageAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-174">toorequest feedback for hello delivery of your cloud-to-device message, you have toospecify a property in hello **SendCloudToDeviceMessageAsync** method.</span></span> <span data-ttu-id="0f9ac-175">Lägg till följande rad direkt efter hello hello `var commandMessage = new Message(...);` rad:</span><span class="sxs-lookup"><span data-stu-id="0f9ac-175">Add hello following line, right after hello `var commandMessage = new Message(...);` line:</span></span>
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. <span data-ttu-id="0f9ac-176">Köra hello appar genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-176">Run hello apps by pressing **F5**.</span></span> <span data-ttu-id="0f9ac-177">Du bör se alla tre program startar.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-177">You should see all three applications start.</span></span> <span data-ttu-id="0f9ac-178">Välj hello **SendCloudToDevice** windows och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-178">Select hello **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="0f9ac-179">Du bör se hello meddelandet tas emot av hello enhetsapp och efter några sekunder, hello feedback meddelandet tas emot av din **SendCloudToDevice** program.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-179">You should see hello message being received by hello device app, and after a few seconds, hello feedback message being received by your **SendCloudToDevice** application.</span></span>
   
   ![Appen tar emot meddelandet][22]

> [!NOTE]
> <span data-ttu-id="0f9ac-181">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-181">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="0f9ac-182">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-182">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="0f9ac-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f9ac-183">Next steps</span></span>
<span data-ttu-id="0f9ac-184">I kursen får du lära sig hur toosend och ta emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="0f9ac-184">In this tutorial, you learned how toosend and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="0f9ac-185">toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-185">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="0f9ac-186">toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="0f9ac-186">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT service SDK NuGet-paketet]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md

[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md
[Kom igång med IoT-hubb]: iot-hub-csharp-csharp-getstarted.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://docs.microsoft.com/en-us/azure/iot-suite/
[Azure IoT-enhet SDK]: iot-hub-devguide-sdks.md