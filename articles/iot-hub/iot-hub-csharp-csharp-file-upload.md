---
title: "aaaUpload filer från enheter tooAzure IoT-hubb med .NET | Microsoft Docs"
description: "Hur tooupload filer från en enhet toohello moln med Azure IoT-enhet SDK för .NET. Överförda filer lagras i ett Azure storage blob-behållaren."
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
ms.openlocfilehash: 07d555f6ba8b067bbd3233bc8eebaa220ad2388b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub-using-net"></a><span data-ttu-id="e1810-104">Ladda upp filer från din enhet toohello moln med IoT-hubben med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="e1810-104">Upload files from your device toohello cloud with IoT Hub using .NET</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="e1810-105">Den här kursen bygger på hello koden i hello [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) självstudiekursen tooshow du hur toouse hello filen överför funktionerna i IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e1810-105">This tutorial builds on hello code in hello [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial tooshow you how toouse hello file upload capabilities of IoT Hub.</span></span> <span data-ttu-id="e1810-106">Den visar hur till:</span><span class="sxs-lookup"><span data-stu-id="e1810-106">It shows you how to:</span></span>

- <span data-ttu-id="e1810-107">Ange en enhet på ett säkert sätt med en Azure blob-URI för att överföra en fil.</span><span class="sxs-lookup"><span data-stu-id="e1810-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="e1810-108">Använd hello IoT-hubb överför meddelanden tootrigger filbearbetning hello-filen i din app-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="e1810-108">Use hello IoT Hub file upload notifications tootrigger processing hello file in your app back end.</span></span>

<span data-ttu-id="e1810-109">Hej [Kom igång med IoT-hubb](iot-hub-csharp-csharp-getstarted.md) och [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) självstudiekurser visar hello grundläggande enhet till moln och moln till enhet meddelandefunktioner för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e1810-109">hello [Get started with IoT Hub](iot-hub-csharp-csharp-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorials show hello basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="e1810-110">Hej [meddelanden processen enhet till moln](iot-hub-csharp-csharp-process-d2c.md) självstudiekursen beskriver ett sätt tooreliably spara enhet till moln meddelanden i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="e1810-110">hello [Process Device-to-Cloud messages](iot-hub-csharp-csharp-process-d2c.md) tutorial describes a way tooreliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="e1810-111">Men i vissa fall kan du enkelt kartdata hello enheterna skickar till relativt liten enhet till moln hälsningsmeddelande som accepterar IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e1810-111">However, in some scenarios you cannot easily map hello data your devices send into hello relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="e1810-112">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e1810-112">For example:</span></span>

* <span data-ttu-id="e1810-113">Stora filer som innehåller bilder</span><span class="sxs-lookup"><span data-stu-id="e1810-113">Large files that contain images</span></span>
* <span data-ttu-id="e1810-114">Videoklipp</span><span class="sxs-lookup"><span data-stu-id="e1810-114">Videos</span></span>
* <span data-ttu-id="e1810-115">Vibration data samplas vid hög frekvens</span><span class="sxs-lookup"><span data-stu-id="e1810-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="e1810-116">Någon form av förbearbetade data</span><span class="sxs-lookup"><span data-stu-id="e1810-116">Some form of preprocessed data</span></span>

<span data-ttu-id="e1810-117">Dessa filer finns vanligtvis i hello molnet med hjälp av verktyg som bearbetas i batch [Azure Data Factory](../data-factory/index.md) eller hello [Hadoop](../hdinsight/index.md) stacken.</span><span class="sxs-lookup"><span data-stu-id="e1810-117">These files are typically batch processed in hello cloud using tools such as [Azure Data Factory](../data-factory/index.md) or hello [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="e1810-118">När du behöver tooupload filer från en enhet, kan du fortfarande använda hello säkerheten och pålitligheten för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e1810-118">When you need tooupload files from a device, you can still use hello security and reliability of IoT Hub.</span></span>

<span data-ttu-id="e1810-119">Hello slutet av den här kursen kan du köra två .NET konsolappar:</span><span class="sxs-lookup"><span data-stu-id="e1810-119">At hello end of this tutorial you run two .NET console apps:</span></span>

* <span data-ttu-id="e1810-120">**SimulatedDevice**, en modifierad version av hello-app som skapats i hello [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="e1810-120">**SimulatedDevice**, a modified version of hello app created in hello [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial.</span></span> <span data-ttu-id="e1810-121">Den här appen överför en fil toostorage med en SAS-URI som tillhandahålls av din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e1810-121">This app uploads a file toostorage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="e1810-122">**ReadFileUploadNotification**, som tar emot filen överföra meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e1810-122">**ReadFileUploadNotification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="e1810-123">IoT-hubb stöder många vilka plattformar och språk (inklusive C, Java och Javascript) via Azure IoT-enhet SDK: er.</span><span class="sxs-lookup"><span data-stu-id="e1810-123">IoT Hub supports many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="e1810-124">Se toohello [Azure IoT Developer Center] stegvisa anvisningar för hur tooconnect din enhet tooAzure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e1810-124">Refer toohello [Azure IoT Developer Center] for step-by-step instructions on how tooconnect your device tooAzure IoT Hub.</span></span>

<span data-ttu-id="e1810-125">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="e1810-125">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="e1810-126">Visual Studio 2015 eller Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e1810-126">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="e1810-127">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e1810-127">An active Azure account.</span></span> <span data-ttu-id="e1810-128">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="e1810-128">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="e1810-129">Överför en fil från en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="e1810-129">Upload a file from a device app</span></span>

<span data-ttu-id="e1810-130">I det här avsnittet kan du ändra hello enhetsapp som du skapade i [meddelanden moln till enhet med IoT-hubben](iot-hub-csharp-csharp-c2d.md) tooreceive moln till enhet meddelanden från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e1810-130">In this section, you modify hello device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="e1810-131">I Visual Studio högerklickar du på hello **SimulatedDevice** projektet, klicka på **Lägg till**, och klicka sedan på **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="e1810-131">In Visual Studio, right-click hello **SimulatedDevice** project, click **Add**, and then click **Existing Item**.</span></span> <span data-ttu-id="e1810-132">Navigera tooan image-filen och inkludera den i projektet.</span><span class="sxs-lookup"><span data-stu-id="e1810-132">Navigate tooan image file and include it in your project.</span></span> <span data-ttu-id="e1810-133">Den här självstudiekursen förutsätts hello avbildningen heter `image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="e1810-133">This tutorial assumes hello image is named `image.jpg`.</span></span>

1. <span data-ttu-id="e1810-134">Högerklicka på hello avbildningen och klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="e1810-134">Right-click on hello image, and then click **Properties**.</span></span> <span data-ttu-id="e1810-135">Se till att **kopiera tooOutput Directory** har angetts för**Kopiera alltid**.</span><span class="sxs-lookup"><span data-stu-id="e1810-135">Make sure that **Copy tooOutput Directory** is set too**Copy always**.</span></span>

    ![][1]

1. <span data-ttu-id="e1810-136">I hello **Program.cs** lägger du till följande instruktioner hello överst i filen hello hello:</span><span class="sxs-lookup"><span data-stu-id="e1810-136">In hello **Program.cs** file, add hello following statements at hello top of hello file:</span></span>

    ```csharp
    using System.IO;
    ```

1. <span data-ttu-id="e1810-137">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="e1810-137">Add hello following method toohello **Program** class:</span></span>

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
        Console.WriteLine("Time tooupload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    <span data-ttu-id="e1810-138">Hej `UploadToBlobAsync` metoden tar i hello filnamnet och dataström källan för hello filen toobe överförs och hanterar hello överför toostorage.</span><span class="sxs-lookup"><span data-stu-id="e1810-138">hello `UploadToBlobAsync` method takes in hello file name and stream source of hello file toobe uploaded and handles hello upload toostorage.</span></span> <span data-ttu-id="e1810-139">hello konsolprogram visar hello tid det tar tooupload hello-filen.</span><span class="sxs-lookup"><span data-stu-id="e1810-139">hello console app displays hello time it takes tooupload hello file.</span></span>

1. <span data-ttu-id="e1810-140">Lägg till följande metod i hello hello **Main** metoden precis före hello `Console.ReadLine()` rad:</span><span class="sxs-lookup"><span data-stu-id="e1810-140">Add hello following method in hello **Main** method, right before hello `Console.ReadLine()` line:</span></span>

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> <span data-ttu-id="e1810-141">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="e1810-141">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="e1810-142">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="e1810-142">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="e1810-143">Ta emot ett meddelande om överföringen</span><span class="sxs-lookup"><span data-stu-id="e1810-143">Receive a file upload notification</span></span>

<span data-ttu-id="e1810-144">I det här avsnittet kan du skriva en .NET-konsolapp som tar emot filen överföra meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e1810-144">In this section, you write a .NET console app that receives file upload notification messages from IoT Hub.</span></span>

1. <span data-ttu-id="e1810-145">Skapa ett Visual C# Windows-projekt i hello aktuella Visual Studio-lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="e1810-145">In hello current Visual Studio solution, create a Visual C# Windows project by using hello **Console Application** project template.</span></span> <span data-ttu-id="e1810-146">Namnet hello projektet **ReadFileUploadNotification**.</span><span class="sxs-lookup"><span data-stu-id="e1810-146">Name hello project **ReadFileUploadNotification**.</span></span>

    ![Nytt projekt i Visual Studio][2]

1. <span data-ttu-id="e1810-148">I Solution Explorer högerklickar du på hello **ReadFileUploadNotification** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="e1810-148">In Solution Explorer, right-click hello **ReadFileUploadNotification** project, and then click **Manage NuGet Packages...**.</span></span>

1. <span data-ttu-id="e1810-149">I hello **NuGet Package Manager** fönster, söka efter **Microsoft.Azure.Devices**, klickar du på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="e1810-149">In hello **NuGet Package Manager** window, search for **Microsoft.Azure.Devices**, click **Install**, and accept hello terms of use.</span></span>

    <span data-ttu-id="e1810-150">Den här åtgärden hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK NuGet-paketet] i hello **ReadFileUploadNotification** projekt.</span><span class="sxs-lookup"><span data-stu-id="e1810-150">This action downloads, installs, and adds a reference toohello [Azure IoT service SDK NuGet package] in hello **ReadFileUploadNotification** project.</span></span>

1. <span data-ttu-id="e1810-151">I hello **Program.cs** lägger du till följande instruktioner hello överst i filen hello hello:</span><span class="sxs-lookup"><span data-stu-id="e1810-151">In hello **Program.cs** file, add hello following statements at hello top of hello file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. <span data-ttu-id="e1810-152">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="e1810-152">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="e1810-153">Ersätt hello platshållare värdet med hello IoT hub-anslutningssträng från [Kom igång med IoT-hubb]:</span><span class="sxs-lookup"><span data-stu-id="e1810-153">Substitute hello placeholder value with hello IoT hub connection string from [Get started with IoT Hub]:</span></span>

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. <span data-ttu-id="e1810-154">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="e1810-154">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="e1810-155">Obs den här ta emot mönstret är hello samma en används tooreceive moln till enhet meddelanden från hello enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="e1810-155">Note this receive pattern is hello same one used tooreceive cloud-to-device messages from hello device app.</span></span>

1. <span data-ttu-id="e1810-156">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="e1810-156">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter tooexit\n");
    Console.ReadLine();
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="e1810-157">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="e1810-157">Run hello applications</span></span>

<span data-ttu-id="e1810-158">Nu är du redo toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="e1810-158">Now you are ready toorun hello applications.</span></span>

1. <span data-ttu-id="e1810-159">Högerklicka på lösningen i Visual Studio och välj **ange Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="e1810-159">In Visual Studio, right-click your solution, and select **Set StartUp projects**.</span></span> <span data-ttu-id="e1810-160">Välj **flera Startprojekt**och välj hello **starta** åtgärd för **ReadFileUploadNotification** och **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="e1810-160">Select **Multiple startup projects**, then select hello **Start** action for **ReadFileUploadNotification** and **SimulatedDevice**.</span></span>

1. <span data-ttu-id="e1810-161">Tryck på **F5**.</span><span class="sxs-lookup"><span data-stu-id="e1810-161">Press **F5**.</span></span> <span data-ttu-id="e1810-162">Båda programmen ska starta.</span><span class="sxs-lookup"><span data-stu-id="e1810-162">Both applications should start.</span></span> <span data-ttu-id="e1810-163">Du bör se hello överföringen har slutförts i ett konsolprogram och hello överför meddelandet tas emot av hello andra konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="e1810-163">You should see hello upload completed in one console app and hello upload notification message received by hello other console app.</span></span> <span data-ttu-id="e1810-164">Du kan använda hello [Azure-portalen] eller Visual Studio Server Explorer toocheck hello förekomst av hello överförs filen i ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e1810-164">You can use hello [Azure portal] or Visual Studio Server Explorer toocheck for hello presence of hello uploaded file in your Azure Storage account.</span></span>

    ![][50]

## <a name="next-steps"></a><span data-ttu-id="e1810-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e1810-165">Next steps</span></span>

<span data-ttu-id="e1810-166">I kursen får du lärt dig hur toouse hello filöverföring funktionerna i IoT-hubb toosimplify filöverföringar från enheter.</span><span class="sxs-lookup"><span data-stu-id="e1810-166">In this tutorial, you learned how toouse hello file upload capabilities of IoT Hub toosimplify file uploads from devices.</span></span> <span data-ttu-id="e1810-167">Du kan fortsätta tooexplore IoT-hubb funktioner och scenarier med hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="e1810-167">You can continue tooexplore IoT hub features and scenarios with hello following articles:</span></span>

* <span data-ttu-id="e1810-168">[Skapa en IoT-hubb programmässigt][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="e1810-168">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="e1810-169">[Introduktion tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="e1810-169">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="e1810-170">[Azure IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="e1810-170">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="e1810-171">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="e1810-171">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="e1810-172">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="e1810-172">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png

<!-- Links -->

[Azure-portalen]: https://portal.azure.com/

[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure IoT service SDK NuGet-paketet]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md
