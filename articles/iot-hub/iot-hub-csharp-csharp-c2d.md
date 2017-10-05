---
title: Moln till enhet meddelanden med Azure IoT Hub (.NET) | Microsoft Docs
description: "Hur du skickar meddelanden moln till enhet till en enhet från en Azure IoT-hubb med Azure IoT-SDK för .NET. Du ändrar en enhetsapp för att ta emot meddelanden moln till enhet och ändra en backend-app för att skicka meddelanden moln till enhet."
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
ms.openlocfilehash: 3f5f83671054c30afde3d7f18ff0edcdb8f78a01
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a><span data-ttu-id="2976b-104">Skicka meddelanden från moln till enheten med IoT Hub (.NET)</span><span class="sxs-lookup"><span data-stu-id="2976b-104">Send messages from the cloud to your device with IoT Hub (.NET)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="2976b-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="2976b-105">Introduction</span></span>
<span data-ttu-id="2976b-106">Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="2976b-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="2976b-107">Den [Kom igång med IoT-hubb] kursen visar hur du skapar en IoT-hubb, etablera en enhetsidentitet i den och code en enhetsapp som skickar meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="2976b-107">The [Get started with IoT Hub] tutorial shows how to create an IoT hub, provision a device identity in it, and code a device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="2976b-108">Den här kursen bygger på [Kom igång med IoT-hubb].</span><span class="sxs-lookup"><span data-stu-id="2976b-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="2976b-109">Den visar hur till:</span><span class="sxs-lookup"><span data-stu-id="2976b-109">It shows you how to:</span></span>

* <span data-ttu-id="2976b-110">Skicka meddelanden moln till enhet på en enhet till IoT-hubb från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="2976b-110">From your solution back end, send cloud-to-device messages to a single device through IoT Hub.</span></span>
* <span data-ttu-id="2976b-111">Ta emot meddelanden moln till enhet på en enhet.</span><span class="sxs-lookup"><span data-stu-id="2976b-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="2976b-112">Begära leverans bekräftelse från din lösningens serverdel (*feedback*) för meddelanden som skickas till en enhet från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2976b-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent to a device from IoT Hub.</span></span>

<span data-ttu-id="2976b-113">Du hittar mer information om moln till enhet meddelanden i den [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="2976b-113">You can find more information on cloud-to-device messages in the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="2976b-114">I slutet av den här kursen kan du köra två .NET konsolappar:</span><span class="sxs-lookup"><span data-stu-id="2976b-114">At the end of this tutorial, you run two .NET console apps:</span></span>

* <span data-ttu-id="2976b-115">**SimulatedDevice**, en modifierad version av app som skapats i [Kom igång med IoT-hubb], som ansluter till din IoT-hubb och tar emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="2976b-115">**SimulatedDevice**, a modified version of the app created in [Get started with IoT Hub], which connects to your IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="2976b-116">**SendCloudToDevice**, som skickar ett moln till enhet-meddelande till appen enheten via IoT-hubb och tar emot dess leverans bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="2976b-116">**SendCloudToDevice**, which sends a cloud-to-device message to the device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="2976b-117">IoT-hubben har SDK stöd för många vilka plattformar och språk (inklusive C, Java och Javascript) via [Azure IoT-enhet SDK].</span><span class="sxs-lookup"><span data-stu-id="2976b-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through [Azure IoT device SDKs].</span></span> <span data-ttu-id="2976b-118">Stegvisa instruktioner om hur du ansluter enheten till den här självstudiekursen kod och vanligtvis Azure IoT Hub finns i [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="2976b-118">For step-by-step instructions on how to connect your device to this tutorial's code, and generally to Azure IoT Hub, see the [IoT Hub developer guide].</span></span>
> 
> 

<span data-ttu-id="2976b-119">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2976b-119">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="2976b-120">Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2976b-120">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="2976b-121">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="2976b-121">An active Azure account.</span></span> <span data-ttu-id="2976b-122">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="2976b-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-the-device-app"></a><span data-ttu-id="2976b-123">Ta emot meddelanden i appen enhet</span><span class="sxs-lookup"><span data-stu-id="2976b-123">Receive messages in the device app</span></span>
<span data-ttu-id="2976b-124">I det här avsnittet ska du ändra appen för enheter som du skapade i [Kom igång med IoT-hubb] ta emot meddelanden moln till enhet från IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="2976b-124">In this section, you'll modify the device app you created in [Get started with IoT Hub] to receive cloud-to-device messages from the IoT hub.</span></span>

1. <span data-ttu-id="2976b-125">I Visual Studio, i den **SimulatedDevice** projekt, Lägg till följande metod för att den **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="2976b-125">In Visual Studio, in the **SimulatedDevice** project, add the following method to the **Program** class.</span></span>
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
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
   
    <span data-ttu-id="2976b-126">Den `ReceiveAsync` metoden returnerar asynkront det mottagna meddelandet vid den tidpunkt då den tas emot av enheten.</span><span class="sxs-lookup"><span data-stu-id="2976b-126">The `ReceiveAsync` method asynchronously returns the received message at the time that it is received by the device.</span></span> <span data-ttu-id="2976b-127">Den returnerar *null* efter en specifiable tidsgräns (i det här fallet används standardvärdet på en minut).</span><span class="sxs-lookup"><span data-stu-id="2976b-127">It returns *null* after a specifiable timeout period (in this case, the default of one minute is used).</span></span> <span data-ttu-id="2976b-128">När appen tar emot en *null*, den ska fortsätta att vänta på att nya meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2976b-128">When the app receives a *null*, it should continue to wait for new messages.</span></span> <span data-ttu-id="2976b-129">Det här kravet är orsaken till det `if (receivedMessage == null) continue` rad.</span><span class="sxs-lookup"><span data-stu-id="2976b-129">This requirement is the reason for the `if (receivedMessage == null) continue` line.</span></span>
   
    <span data-ttu-id="2976b-130">Anropet till `CompleteAsync()` meddelar IoT-hubb att meddelandet har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="2976b-130">The call to `CompleteAsync()` notifies IoT Hub that the message has been successfully processed.</span></span> <span data-ttu-id="2976b-131">Meddelandet kan tas bort på ett säkert sätt från enheten kön.</span><span class="sxs-lookup"><span data-stu-id="2976b-131">The message can be safely removed from the device queue.</span></span> <span data-ttu-id="2976b-132">Om något hände som förhindrade att appen enheten slutföra bearbetningen av meddelandet ger IoT-hubb den igen.</span><span class="sxs-lookup"><span data-stu-id="2976b-132">If something happened that prevented the device app from completing the processing of the message, IoT Hub delivers it again.</span></span> <span data-ttu-id="2976b-133">Sedan är det viktigt att meddelandebehandling logiken i appen enheten är *idempotent*, så att ta emot samma meddelande flera gånger ger samma resultat.</span><span class="sxs-lookup"><span data-stu-id="2976b-133">It is then important that message processing logic in the device app is *idempotent*, so that receiving the same message multiple times produces the same result.</span></span> <span data-ttu-id="2976b-134">Ett program kan också tillfälligt Avbryt ett meddelande som resulterar i IoT-hubb behålla meddelandet i kön för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="2976b-134">An application can also temporarily abandon a message, which results in IoT hub retaining the message in the queue for future consumption.</span></span> <span data-ttu-id="2976b-135">Eller programmet kan avvisa ett meddelande som tar permanent bort meddelandet från kön.</span><span class="sxs-lookup"><span data-stu-id="2976b-135">Or, the application can reject a message, which permanently removes the message from the queue.</span></span> <span data-ttu-id="2976b-136">Mer information om livscykeln för moln-till-enhetsmeddelande finns i [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="2976b-136">For more information about the cloud-to-device message lifecycle, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2976b-137">När du använder HTTP i stället för MQTT eller AMQP som transport, den `ReceiveAsync` metoden returnerar omedelbart.</span><span class="sxs-lookup"><span data-stu-id="2976b-137">When using HTTP instead of MQTT or AMQP as a transport, the `ReceiveAsync` method returns immediately.</span></span> <span data-ttu-id="2976b-138">Stöds mönstret för moln till enhet meddelanden med HTTP är periodvis anslutna enheter som kontrollerar för meddelanden sällan (mindre än var 25: e minut).</span><span class="sxs-lookup"><span data-stu-id="2976b-138">The supported pattern for cloud-to-device messages with HTTP is intermittently connected devices that check for messages infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="2976b-139">Utfärda mer HTTP får resultat i IoT-hubb begränsning av begäranden.</span><span class="sxs-lookup"><span data-stu-id="2976b-139">Issuing more HTTP receives results in IoT Hub throttling the requests.</span></span> <span data-ttu-id="2976b-140">Mer information om skillnaderna mellan MQTT, AMQP och HTTP-stöd och IoT-hubb begränsning finns på [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="2976b-140">For more information about the differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 
2. <span data-ttu-id="2976b-141">Lägg till följande metod i den **Main** metod, precis innan den `Console.ReadLine()` rad:</span><span class="sxs-lookup"><span data-stu-id="2976b-141">Add the following method in the **Main** method, right before the `Console.ReadLine()` line:</span></span>
   
        ReceiveC2dAsync();

> [!NOTE]
> <span data-ttu-id="2976b-142">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="2976b-142">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="2976b-143">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="2976b-143">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="2976b-144">Skicka ett meddelande moln till enhet</span><span class="sxs-lookup"><span data-stu-id="2976b-144">Send a cloud-to-device message</span></span>
<span data-ttu-id="2976b-145">I det här avsnittet kan du skriva en .NET-konsolapp som skickar moln till enhet meddelanden till appen med enheten.</span><span class="sxs-lookup"><span data-stu-id="2976b-145">In this section, you write a .NET console app that sends cloud-to-device messages to the device app.</span></span>

1. <span data-ttu-id="2976b-146">Skapa ett Visual C#-Skrivbordsapprojekt-projekt i den aktuella Visual Studio-lösningen med hjälp av den **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="2976b-146">In the current Visual Studio solution, create a Visual C# Desktop App project by using the **Console Application** project template.</span></span> <span data-ttu-id="2976b-147">Namnge projektet **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="2976b-147">Name the project **SendCloudToDevice**.</span></span>
   
    ![Nytt projekt i Visual Studio][20]
2. <span data-ttu-id="2976b-149">Högerklicka på lösningen i Solution Explorer och klicka sedan på **hantera NuGet-paket för lösningen...** .</span><span class="sxs-lookup"><span data-stu-id="2976b-149">In Solution Explorer, right-click the solution, and then click **Manage NuGet Packages for Solution...**.</span></span> 
   
    <span data-ttu-id="2976b-150">Den här åtgärden öppnar den **hantera NuGet-paket** fönster.</span><span class="sxs-lookup"><span data-stu-id="2976b-150">This action opens the **Manage NuGet Packages** window.</span></span>
3. <span data-ttu-id="2976b-151">Sök efter **Microsoft.Azure.Devices**, klickar du på **installera**, och Godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="2976b-151">Search for **Microsoft.Azure.Devices**, click **Install**, and accept the terms of use.</span></span> 
   
    <span data-ttu-id="2976b-152">Detta hämtar, installerar och lägger till en referens till den [Azure IoT service SDK NuGet-paketet].</span><span class="sxs-lookup"><span data-stu-id="2976b-152">This downloads, installs, and adds a reference to the [Azure IoT service SDK NuGet package].</span></span>

4. <span data-ttu-id="2976b-153">Lägg till följande `using`-instruktion högst upp i filen **Program.cs**:</span><span class="sxs-lookup"><span data-stu-id="2976b-153">Add the following `using` statement at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="2976b-154">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="2976b-154">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="2976b-155">Ersätt platshållaren värdet med anslutningssträngen från IoT-hubb [Kom igång med IoT-hubb]:</span><span class="sxs-lookup"><span data-stu-id="2976b-155">Substitute the placeholder value with the IoT hub connection string from [Get started with IoT Hub]:</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="2976b-156">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="2976b-156">Add the following method to the **Program** class:</span></span>
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    <span data-ttu-id="2976b-157">Den här metoden skickar ett nytt meddelande moln till enhet till enheten med ID, `myFirstDevice`.</span><span class="sxs-lookup"><span data-stu-id="2976b-157">This method sends a new cloud-to-device message to the device with the ID, `myFirstDevice`.</span></span> <span data-ttu-id="2976b-158">Ändra den här parametern endast om den ändrats från den som används i [Kom igång med IoT-hubb].</span><span class="sxs-lookup"><span data-stu-id="2976b-158">Change this parameter only if you modified it from the one used in [Get started with IoT Hub].</span></span>
7. <span data-ttu-id="2976b-159">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="2976b-159">Finally, add the following lines to the **Main** method:</span></span>
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="2976b-160">I Visual Studio högerklickar du på din lösning och välj **ange Startprojekt...** . Välj **flera Startprojekt**och välj den **starta** åtgärd för **ReadDeviceToCloudMessages**, **SimulatedDevice**, och **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="2976b-160">From within Visual Studio, right-click your solution, and select **Set StartUp projects...**. Select **Multiple startup projects**, then select the **Start** action for **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **SendCloudToDevice**.</span></span>
9. <span data-ttu-id="2976b-161">Tryck på **F5**.</span><span class="sxs-lookup"><span data-stu-id="2976b-161">Press **F5**.</span></span> <span data-ttu-id="2976b-162">Alla tre program ska starta.</span><span class="sxs-lookup"><span data-stu-id="2976b-162">All three applications should start.</span></span> <span data-ttu-id="2976b-163">Välj den **SendCloudToDevice** windows och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="2976b-163">Select the **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="2976b-164">Du bör se meddelandet tas emot av enheten appen.</span><span class="sxs-lookup"><span data-stu-id="2976b-164">You should see the message being received by the device app.</span></span>
   
   ![Appen tar emot meddelandet][21]

## <a name="receive-delivery-feedback"></a><span data-ttu-id="2976b-166">Ta emot leverans feedback</span><span class="sxs-lookup"><span data-stu-id="2976b-166">Receive delivery feedback</span></span>
<span data-ttu-id="2976b-167">Det är möjligt att begäran leverans (eller sista) bekräftelser från IoT-hubb för varje moln till enhet-meddelande.</span><span class="sxs-lookup"><span data-stu-id="2976b-167">It is possible to request delivery (or expiration) acknowledgements from IoT Hub for each cloud-to-device message.</span></span> <span data-ttu-id="2976b-168">Det här alternativet kan lösningens serverdel enkelt informera logik för återförsök eller ersättning.</span><span class="sxs-lookup"><span data-stu-id="2976b-168">This option enables the solution back end to easily inform retry or compensation logic.</span></span> <span data-ttu-id="2976b-169">Mer information om moln till enhet feedback finns i [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="2976b-169">For more information about cloud-to-device feedback, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="2976b-170">I det här avsnittet kan du ändra den **SendCloudToDevice** app att begära feedback och tar emot det från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="2976b-170">In this section, you modify the **SendCloudToDevice** app to request feedback, and receive it from IoT Hub.</span></span>

1. <span data-ttu-id="2976b-171">I Visual Studio, i den **SendCloudToDevice** projekt, Lägg till följande metod för att den **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="2976b-171">In Visual Studio, in the **SendCloudToDevice** project, add the following method to the **Program** class.</span></span>
   
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
   
    <span data-ttu-id="2976b-172">Observera att ta emot mönstret är det samma som används för att ta emot meddelanden moln till enhet från appen enhet.</span><span class="sxs-lookup"><span data-stu-id="2976b-172">Note this receive pattern is the same one used to receive cloud-to-device messages from the device app.</span></span>
2. <span data-ttu-id="2976b-173">Lägg till följande metod i den **Main** metod, rätt efter den `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` rad:</span><span class="sxs-lookup"><span data-stu-id="2976b-173">Add the following method in the **Main** method, right after the `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` line:</span></span>
   
        ReceiveFeedbackAsync();
3. <span data-ttu-id="2976b-174">Om du vill begära feedback för leverans av meddelandet moln till enhet, måste du ange en egenskap i den **SendCloudToDeviceMessageAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="2976b-174">To request feedback for the delivery of your cloud-to-device message, you have to specify a property in the **SendCloudToDeviceMessageAsync** method.</span></span> <span data-ttu-id="2976b-175">Lägg till följande rad direkt efter den `var commandMessage = new Message(...);` rad:</span><span class="sxs-lookup"><span data-stu-id="2976b-175">Add the following line, right after the `var commandMessage = new Message(...);` line:</span></span>
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. <span data-ttu-id="2976b-176">Kör appar genom att trycka på **F5**.</span><span class="sxs-lookup"><span data-stu-id="2976b-176">Run the apps by pressing **F5**.</span></span> <span data-ttu-id="2976b-177">Du bör se alla tre program startar.</span><span class="sxs-lookup"><span data-stu-id="2976b-177">You should see all three applications start.</span></span> <span data-ttu-id="2976b-178">Välj den **SendCloudToDevice** windows och tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="2976b-178">Select the **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="2976b-179">Du bör se meddelandet tas emot av enheten appen och efter några sekunder feedback meddelandet tas emot av din **SendCloudToDevice** program.</span><span class="sxs-lookup"><span data-stu-id="2976b-179">You should see the message being received by the device app, and after a few seconds, the feedback message being received by your **SendCloudToDevice** application.</span></span>
   
   ![Appen tar emot meddelandet][22]

> [!NOTE]
> <span data-ttu-id="2976b-181">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="2976b-181">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="2976b-182">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="2976b-182">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="2976b-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2976b-183">Next steps</span></span>
<span data-ttu-id="2976b-184">I den här självstudiekursen beskrivs hur du skickar och tar emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="2976b-184">In this tutorial, you learned how to send and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="2976b-185">Exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="2976b-185">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="2976b-186">Mer information om hur du utvecklar lösningar med IoT-hubb finns i [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="2976b-186">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

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