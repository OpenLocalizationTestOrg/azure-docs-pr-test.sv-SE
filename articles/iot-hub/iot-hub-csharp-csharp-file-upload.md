---
title: "Överföra filer från enheter till Azure IoT-hubb med .NET | Microsoft Docs"
description: "Hur du överför filer från en enhet till molnet med Azure IoT-enhet SDK för .NET. Överförda filer lagras i ett Azure storage blob-behållaren."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: elioda
ms.openlocfilehash: b45d85d0d77cf47f36cb793bc8c0dbe2d5c12634
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a><span data-ttu-id="f0857-104">Ladda upp filer från enheten till molnet med IoT-hubben med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="f0857-104">Upload files from your device to the cloud with IoT Hub using .NET</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="f0857-105">Den här kursen bygger på koden i den [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) vägledning till hur du kan använda funktionerna för överför filen för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f0857-105">This tutorial builds on the code in the [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial to show you how to use the file upload capabilities of IoT Hub.</span></span> <span data-ttu-id="f0857-106">Den visar hur till:</span><span class="sxs-lookup"><span data-stu-id="f0857-106">It shows you how to:</span></span>

- <span data-ttu-id="f0857-107">Ange en enhet på ett säkert sätt med en Azure blob-URI för att överföra en fil.</span><span class="sxs-lookup"><span data-stu-id="f0857-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="f0857-108">Använd IoT-hubb filen överför meddelanden för att utlösa bearbetning av filen i din app-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="f0857-108">Use the IoT Hub file upload notifications to trigger processing the file in your app back end.</span></span>

<span data-ttu-id="f0857-109">Den [Kom igång med IoT-hubb](iot-hub-csharp-csharp-getstarted.md) och [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) självstudiekurser visar grundläggande enhet till moln och moln till enhet meddelandetjänsten funktioner i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f0857-109">The [Get started with IoT Hub](iot-hub-csharp-csharp-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorials show the basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="f0857-110">Den [meddelanden processen enhet till moln](iot-hub-csharp-csharp-process-d2c.md) självstudiekursen beskriver ett sätt att på ett tillförlitligt sätt lagra meddelanden från enhet till moln i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="f0857-110">The [Process Device-to-Cloud messages](iot-hub-csharp-csharp-process-d2c.md) tutorial describes a way to reliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="f0857-111">Men i vissa fall kan du enkelt mappa data enheterna skickar till relativt liten enhet till moln meddelanden som accepterar IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f0857-111">However, in some scenarios you cannot easily map the data your devices send into the relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="f0857-112">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f0857-112">For example:</span></span>

* <span data-ttu-id="f0857-113">Stora filer som innehåller bilder</span><span class="sxs-lookup"><span data-stu-id="f0857-113">Large files that contain images</span></span>
* <span data-ttu-id="f0857-114">Videoklipp</span><span class="sxs-lookup"><span data-stu-id="f0857-114">Videos</span></span>
* <span data-ttu-id="f0857-115">Vibration data samplas vid hög frekvens</span><span class="sxs-lookup"><span data-stu-id="f0857-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="f0857-116">Någon form av förbearbetade data</span><span class="sxs-lookup"><span data-stu-id="f0857-116">Some form of preprocessed data</span></span>

<span data-ttu-id="f0857-117">Dessa filer finns vanligtvis i molnet med hjälp av verktyg som bearbetas i batch [Azure Data Factory](../data-factory/index.md) eller [Hadoop](../hdinsight/index.md) stacken.</span><span class="sxs-lookup"><span data-stu-id="f0857-117">These files are typically batch processed in the cloud using tools such as [Azure Data Factory](../data-factory/index.md) or the [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="f0857-118">När du behöver överföra filer från en enhet kan du fortfarande använda säkerheten och pålitligheten för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f0857-118">When you need to upload files from a device, you can still use the security and reliability of IoT Hub.</span></span>

<span data-ttu-id="f0857-119">I slutet av den här kursen kan du köra två .NET konsolappar:</span><span class="sxs-lookup"><span data-stu-id="f0857-119">At the end of this tutorial you run two .NET console apps:</span></span>

* <span data-ttu-id="f0857-120">**SimulatedDevice**, en modifierad version av app som skapats i den [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="f0857-120">**SimulatedDevice**, a modified version of the app created in the [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial.</span></span> <span data-ttu-id="f0857-121">Den här appen överför en fil till lagring med hjälp av en SAS-URI som tillhandahålls av din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f0857-121">This app uploads a file to storage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="f0857-122">**ReadFileUploadNotification**, som tar emot filen överföra meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f0857-122">**ReadFileUploadNotification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="f0857-123">IoT-hubb stöder många vilka plattformar och språk (inklusive C, Java och Javascript) via Azure IoT-enhet SDK: er.</span><span class="sxs-lookup"><span data-stu-id="f0857-123">IoT Hub supports many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="f0857-124">Referera till den [Azure IoT Developer Center] stegvisa instruktioner om hur du ansluter enheten till Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="f0857-124">Refer to the [Azure IoT Developer Center] for step-by-step instructions on how to connect your device to Azure IoT Hub.</span></span>

<span data-ttu-id="f0857-125">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f0857-125">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="f0857-126">Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f0857-126">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="f0857-127">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f0857-127">An active Azure account.</span></span> <span data-ttu-id="f0857-128">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="f0857-128">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="f0857-129">Överför en fil från en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="f0857-129">Upload a file from a device app</span></span>

<span data-ttu-id="f0857-130">I det här avsnittet kan du ändra appen för enheter som du skapade i [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) ta emot meddelanden moln till enhet från IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="f0857-130">In this section, you modify the device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) to receive cloud-to-device messages from the IoT hub.</span></span>

1. <span data-ttu-id="f0857-131">I Visual Studio högerklickar du på den **SimulatedDevice** projektet, klicka på **Lägg till**, och klicka sedan på **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="f0857-131">In Visual Studio, right-click the **SimulatedDevice** project, click **Add**, and then click **Existing Item**.</span></span> <span data-ttu-id="f0857-132">Navigera till en bildfil och inkludera den i projektet.</span><span class="sxs-lookup"><span data-stu-id="f0857-132">Navigate to an image file and include it in your project.</span></span> <span data-ttu-id="f0857-133">Den här självstudiekursen förutsätts bilden heter `image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="f0857-133">This tutorial assumes the image is named `image.jpg`.</span></span>

1. <span data-ttu-id="f0857-134">Högerklicka på bilden och klicka sedan på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="f0857-134">Right-click on the image, and then click **Properties**.</span></span> <span data-ttu-id="f0857-135">Se till att **kopiera till utdatakatalog** är inställd på **Kopiera alltid**.</span><span class="sxs-lookup"><span data-stu-id="f0857-135">Make sure that **Copy to Output Directory** is set to **Copy always**.</span></span>

    ![][1]

1. <span data-ttu-id="f0857-136">I den **Program.cs** lägger du till följande uttryck överst i filen:</span><span class="sxs-lookup"><span data-stu-id="f0857-136">In the **Program.cs** file, add the following statements at the top of the file:</span></span>

    ```csharp
    using System.IO;
    ```

1. <span data-ttu-id="f0857-137">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="f0857-137">Add the following method to the **Program** class:</span></span>

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    <span data-ttu-id="f0857-138">Den `UploadToBlobAsync` metoden hämtar i filen namn och dataströmmen källan för filen ska överföras och hanterar överföringen till lagring.</span><span class="sxs-lookup"><span data-stu-id="f0857-138">The `UploadToBlobAsync` method takes in the file name and stream source of the file to be uploaded and handles the upload to storage.</span></span> <span data-ttu-id="f0857-139">Appen konsolen visar den tid det tar för att överföra filen.</span><span class="sxs-lookup"><span data-stu-id="f0857-139">The console app displays the time it takes to upload the file.</span></span>

1. <span data-ttu-id="f0857-140">Lägg till följande metod i den **Main** metod, precis innan den `Console.ReadLine()` rad:</span><span class="sxs-lookup"><span data-stu-id="f0857-140">Add the following method in the **Main** method, right before the `Console.ReadLine()` line:</span></span>

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> <span data-ttu-id="f0857-141">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="f0857-141">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="f0857-142">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="f0857-142">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="f0857-143">Ta emot ett meddelande om överföringen</span><span class="sxs-lookup"><span data-stu-id="f0857-143">Receive a file upload notification</span></span>

<span data-ttu-id="f0857-144">I det här avsnittet kan du skriva en .NET-konsolapp som tar emot filen överföra meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f0857-144">In this section, you write a .NET console app that receives file upload notification messages from IoT Hub.</span></span>

1. <span data-ttu-id="f0857-145">Skapa ett Visual C# Windows-projekt i den aktuella Visual Studio-lösningen med hjälp av den **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="f0857-145">In the current Visual Studio solution, create a Visual C# Windows project by using the **Console Application** project template.</span></span> <span data-ttu-id="f0857-146">Namnge projektet **ReadFileUploadNotification**.</span><span class="sxs-lookup"><span data-stu-id="f0857-146">Name the project **ReadFileUploadNotification**.</span></span>

    ![Nytt projekt i Visual Studio][2]

1. <span data-ttu-id="f0857-148">I Solution Explorer högerklickar du på den **ReadFileUploadNotification** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="f0857-148">In Solution Explorer, right-click the **ReadFileUploadNotification** project, and then click **Manage NuGet Packages...**.</span></span>

1. <span data-ttu-id="f0857-149">I den **NuGet Package Manager** fönster, söka efter **Microsoft.Azure.Devices**, klickar du på **installera**, och Godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="f0857-149">In the **NuGet Package Manager** window, search for **Microsoft.Azure.Devices**, click **Install**, and accept the terms of use.</span></span>

    <span data-ttu-id="f0857-150">Den här åtgärden hämtar, installerar och lägger till en referens till den [Azure IoT service SDK NuGet-paketet] i den **ReadFileUploadNotification** projekt.</span><span class="sxs-lookup"><span data-stu-id="f0857-150">This action downloads, installs, and adds a reference to the [Azure IoT service SDK NuGet package] in the **ReadFileUploadNotification** project.</span></span>

1. <span data-ttu-id="f0857-151">I den **Program.cs** lägger du till följande uttryck överst i filen:</span><span class="sxs-lookup"><span data-stu-id="f0857-151">In the **Program.cs** file, add the following statements at the top of the file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. <span data-ttu-id="f0857-152">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="f0857-152">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="f0857-153">Ersätt platshållaren värdet med anslutningssträngen från IoT-hubb [Kom igång med IoT-hubb]:</span><span class="sxs-lookup"><span data-stu-id="f0857-153">Substitute the placeholder value with the IoT hub connection string from [Get started with IoT Hub]:</span></span>

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. <span data-ttu-id="f0857-154">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="f0857-154">Add the following method to the **Program** class:</span></span>

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    <span data-ttu-id="f0857-155">Observera att ta emot mönstret är det samma som används för att ta emot meddelanden moln till enhet från appen enhet.</span><span class="sxs-lookup"><span data-stu-id="f0857-155">Note this receive pattern is the same one used to receive cloud-to-device messages from the device app.</span></span>

1. <span data-ttu-id="f0857-156">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="f0857-156">Finally, add the following lines to the **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a><span data-ttu-id="f0857-157">Köra programmen</span><span class="sxs-lookup"><span data-stu-id="f0857-157">Run the applications</span></span>

<span data-ttu-id="f0857-158">Du är nu redo att köra programmen.</span><span class="sxs-lookup"><span data-stu-id="f0857-158">Now you are ready to run the applications.</span></span>

1. <span data-ttu-id="f0857-159">Högerklicka på lösningen i Visual Studio och välj **ange Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="f0857-159">In Visual Studio, right-click your solution, and select **Set StartUp projects**.</span></span> <span data-ttu-id="f0857-160">Välj **flera Startprojekt**och välj den **starta** åtgärd för **ReadFileUploadNotification** och **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="f0857-160">Select **Multiple startup projects**, then select the **Start** action for **ReadFileUploadNotification** and **SimulatedDevice**.</span></span>

1. <span data-ttu-id="f0857-161">Tryck på **F5**.</span><span class="sxs-lookup"><span data-stu-id="f0857-161">Press **F5**.</span></span> <span data-ttu-id="f0857-162">Båda programmen ska starta.</span><span class="sxs-lookup"><span data-stu-id="f0857-162">Both applications should start.</span></span> <span data-ttu-id="f0857-163">Du bör se överföringen slutförts i ett konsolprogram och överför meddelandet tas emot av andra konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="f0857-163">You should see the upload completed in one console app and the upload notification message received by the other console app.</span></span> <span data-ttu-id="f0857-164">Du kan använda den [Azure-portalen] eller Visual Studio Server Explorer för att söka efter förekomst av den överförda filen i ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f0857-164">You can use the [Azure portal] or Visual Studio Server Explorer to check for the presence of the uploaded file in your Azure Storage account.</span></span>

    ![][50]

## <a name="next-steps"></a><span data-ttu-id="f0857-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f0857-165">Next steps</span></span>

<span data-ttu-id="f0857-166">I kursen får du har lärt dig hur du använder filen överför funktionerna i IoT-hubb för att förenkla filöverföringar från enheter.</span><span class="sxs-lookup"><span data-stu-id="f0857-166">In this tutorial, you learned how to use the file upload capabilities of IoT Hub to simplify file uploads from devices.</span></span> <span data-ttu-id="f0857-167">Du kan fortsätta att utforska IoT-hubb funktioner och scenarier i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="f0857-167">You can continue to explore IoT hub features and scenarios with the following articles:</span></span>

* <span data-ttu-id="f0857-168">[Skapa en IoT-hubb programmässigt][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="f0857-168">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="f0857-169">[Introduktion till C SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="f0857-169">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="f0857-170">[Azure IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="f0857-170">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="f0857-171">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="f0857-171">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="f0857-172">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="f0857-172">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png

<!-- Links -->

<span data-ttu-id="f0857-173">[Azure-portalen]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="f0857-173">[Azure portal]: https://portal.azure.com/</span></span>

<span data-ttu-id="f0857-174">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="f0857-174">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span></span>

<span data-ttu-id="f0857-175">[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="f0857-175">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
<span data-ttu-id="f0857-176">[Azure IoT service SDK NuGet-paketet]: https://www.nuget.org/packages/Microsoft.Azure.Devices/</span><span class="sxs-lookup"><span data-stu-id="f0857-176">[Azure IoT service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/</span></span>
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md
