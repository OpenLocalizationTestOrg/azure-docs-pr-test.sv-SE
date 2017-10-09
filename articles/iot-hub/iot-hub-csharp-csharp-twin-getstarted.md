---
title: "aaaGet igång med Azure IoT Hub-enhet twins (.NET/.NET) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooadd taggar och sedan använda en IoT-hubb-fråga. Du kan använda hello Azure IoT-enhet SDK för .NET tooimplement hello simulerade enheten appen och hello Azure IoT-tjänsten SDK för .NET tooimplement service-appen som lägger till hello taggar och kör hello IoT-hubb frågan."
services: iot-hub
documentationcenter: node
author: dsk-2015
manager: timlt
editor: 
ms.assetid: f7e23b6e-bfde-4fba-a6ec-dbb0f0e005f4
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: dkshir
ms.openlocfilehash: 7fa73ac896c44e79c6522d252cd1515bd6e7bb2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-netnet"></a><span data-ttu-id="e4d08-104">Kom igång med enheten twins (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="e4d08-104">Get started with device twins (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="e4d08-105">Hello slutet av den här självstudiekursen måste de här apparna för .NET-konsolen:</span><span class="sxs-lookup"><span data-stu-id="e4d08-105">At hello end of this tutorial, you will have these .NET console apps:</span></span>

* <span data-ttu-id="e4d08-106">**CreateDeviceIdentity**, en .NET-app som skapar en enhets-ID och tillhörande nyckeln tooconnect appen simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="e4d08-106">**CreateDeviceIdentity**, a .NET app which creates a device identity and associated security key tooconnect your simulated device app.</span></span>
* <span data-ttu-id="e4d08-107">**AddTagsAndQuery**, en .NET backend-app som lägger till taggar och frågar enheten twins.</span><span class="sxs-lookup"><span data-stu-id="e4d08-107">**AddTagsAndQuery**, a .NET back-end app which adds tags and queries device twins.</span></span>
* <span data-ttu-id="e4d08-108">**ReportConnectivity**, en enhet .NET-app som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och rapporterar tillståndet anslutningen.</span><span class="sxs-lookup"><span data-stu-id="e4d08-108">**ReportConnectivity**, a .NET device app which simulates a device that connects tooyour IoT hub with hello device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="e4d08-109">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="e4d08-109">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="e4d08-110">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="e4d08-110">toocomplete this tutorial you need hello following:</span></span>

* <span data-ttu-id="e4d08-111">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e4d08-111">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="e4d08-112">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e4d08-112">An active Azure account.</span></span> <span data-ttu-id="e4d08-113">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="e4d08-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="e4d08-114">Om du vill toocreate hello enhetsidentitet programmässigt i stället kan du läsa hello motsvarande avsnitt i hello [ansluta din simulerade enhet tooyour IoT-hubb med hjälp av .NET] [ lnk-device-identity-csharp] artikel.</span><span class="sxs-lookup"><span data-stu-id="e4d08-114">If you want toocreate hello device identity programmatically instead, read hello corresponding section in hello [Connect your simulated device tooyour IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="e4d08-115">Skapa hello service-appen</span><span class="sxs-lookup"><span data-stu-id="e4d08-115">Create hello service app</span></span>
<span data-ttu-id="e4d08-116">I det här avsnittet skapar du en .NET-konsolapp (med C#) som lägger till platsen metadata toohello enheten dubbla som är associerade med **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-116">In this section, you create a .NET console app (using C#) that adds location metadata toohello device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="e4d08-117">Därefter frågor hello enheten twins lagras i hello IoT-hubb hello enheter finns i hello oss och hello som rapporterats av en mobil anslutning.</span><span class="sxs-lookup"><span data-stu-id="e4d08-117">It then queries hello device twins stored in hello IoT hub selecting hello devices located in hello US, and then hello ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="e4d08-118">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="e4d08-118">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="e4d08-119">Namnet hello projektet **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-119">Name hello project **AddTagsAndQuery**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]
1. <span data-ttu-id="e4d08-121">I Solution Explorer högerklickar du på hello **AddTagsAndQuery** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="e4d08-121">In Solution Explorer, right-click hello **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="e4d08-122">I hello **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-122">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="e4d08-123">Välj **installera** tooinstall hello **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="e4d08-123">Select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="e4d08-124">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="e4d08-124">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="e4d08-126">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="e4d08-126">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="e4d08-127">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="e4d08-127">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="e4d08-128">Ersätt hello platshållarvärde med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="e4d08-128">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="e4d08-129">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="e4d08-129">Add hello following method toohello **Program** class:</span></span>
   
        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);
   
            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));
   
            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }
   
    <span data-ttu-id="e4d08-130">Hej **RegistryManager** klassen visar alla hello metoder krävs toointeract med enheten twins från hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e4d08-130">hello **RegistryManager** class exposes all hello methods required toointeract with device twins from hello service.</span></span> <span data-ttu-id="e4d08-131">hello föregående kod först initierar hello **registryManager** objekt, och sedan hämtar hello enheten dubbla för **myDeviceId**, och slutligen uppdateras taggarna hello önskad platsinformation.</span><span class="sxs-lookup"><span data-stu-id="e4d08-131">hello previous code first initializes hello **registryManager** object, then retrieves hello device twin for **myDeviceId**, and finally updates its tags with hello desired location information.</span></span>
   
    <span data-ttu-id="e4d08-132">När du har uppdaterat, körs två frågor: hello väljer först endast hello enheten twins av enheter som finns i hello **Redmond43** anläggningar och hello andra förfinar hello frågan tooselect bara hello enheter som även är anslutna via mobilnät.</span><span class="sxs-lookup"><span data-stu-id="e4d08-132">After updating, it executes two queries: hello first selects only hello device twins of devices located in hello **Redmond43** plant, and hello second refines hello query tooselect only hello devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="e4d08-133">Observera att hello föregående kod när den skapar hello **frågan** objekt, anger ett maximalt antal returnerade dokument.</span><span class="sxs-lookup"><span data-stu-id="e4d08-133">Note that hello previous code, when it creates hello **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="e4d08-134">Hej **frågan** objektet innehåller en **HasMoreResults** boolesk egenskap som du kan använda tooinvoke hello **GetNextAsTwinAsync** metoder flera gånger tooretrieve alla resultat.</span><span class="sxs-lookup"><span data-stu-id="e4d08-134">hello **query** object contains a **HasMoreResults** boolean property that you can use tooinvoke hello **GetNextAsTwinAsync** methods multiple times tooretrieve all results.</span></span> <span data-ttu-id="e4d08-135">En metod som kallas **GetNextAsJson** är tillgänglig för resultat som är inte enheten twins, till exempel resultatet av aggregering frågor.</span><span class="sxs-lookup"><span data-stu-id="e4d08-135">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="e4d08-136">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="e4d08-136">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter tooexit.");
        Console.ReadLine();

1. <span data-ttu-id="e4d08-137">Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **AddTagsAndQuery** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-137">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="e4d08-138">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="e4d08-138">Build hello solution.</span></span>
1. <span data-ttu-id="e4d08-139">Kör det här programmet genom att högerklicka på hello **AddTagsAndQuery** projekt och välja **felsöka**, följt av **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-139">Run this application by right-clicking on hello **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="e4d08-140">Du bör se en enhet i hello resultat för hello frågan ställa för alla enheter i **Redmond43** och ingen för hello-fråga som begränsar hello resultatet toodevices som använder ett mobilnät.</span><span class="sxs-lookup"><span data-stu-id="e4d08-140">You should see one device in hello results for hello query asking for all devices located in **Redmond43** and none for hello query that restricts hello results toodevices that use a cellular network.</span></span>
   
    ![Resultatet av frågan i fönstret][img-addtagapp]

<span data-ttu-id="e4d08-142">I nästa avsnitt om hello, skapar du en enhetsapp som rapporterar hello anslutningsinformation och ändringar hello resultatet av frågan hello hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e4d08-142">In hello next section, you create a device app that reports hello connectivity information and changes hello result of hello query in hello previous section.</span></span>

## <a name="create-hello-device-app"></a><span data-ttu-id="e4d08-143">Skapa hello enhetsapp</span><span class="sxs-lookup"><span data-stu-id="e4d08-143">Create hello device app</span></span>
<span data-ttu-id="e4d08-144">I det här avsnittet skapar du en .NET-konsolapp som ansluter tooyour hubb som **myDeviceId**, och sedan uppdaterar rapporterade egenskaper toocontain hello informationen att servern är ansluten med hjälp av ett mobilnät.</span><span class="sxs-lookup"><span data-stu-id="e4d08-144">In this section, you create a .NET console app that connects tooyour hub as **myDeviceId**, and then updates its reported properties toocontain hello information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="e4d08-145">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="e4d08-145">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="e4d08-146">Namnet hello projektet **ReportConnectivity**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-146">Name hello project **ReportConnectivity**.</span></span>
   
    ![Ny Visual C# Windows Classic enhetsapp][img-createdeviceapp]
    
1. <span data-ttu-id="e4d08-148">I Solution Explorer högerklickar du på hello **ReportConnectivity** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="e4d08-148">In Solution Explorer, right-click hello **ReportConnectivity** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="e4d08-149">I hello **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-149">In hello **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="e4d08-150">Välj **installera** tooinstall hello **Microsoft.Azure.Devices.Client** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="e4d08-150">Select **Install** tooinstall hello **Microsoft.Azure.Devices.Client** package, and accept hello terms of use.</span></span> <span data-ttu-id="e4d08-151">Den här proceduren hämtar, installerar och lägger till en referens toohello [Azure IoT-enhet SDK] [ lnk-nuget-client-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="e4d08-151">This procedure downloads, installs, and adds a reference toohello [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Pakethanteraren fönstret-klientappen][img-clientnuget]
1. <span data-ttu-id="e4d08-153">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="e4d08-153">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="e4d08-154">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="e4d08-154">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="e4d08-155">Ersätt hello platshållarvärde med anslutningssträngen för hello enheten som du antecknade i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e4d08-155">Replace hello placeholder value with hello device connection string that you noted in hello previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="e4d08-156">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="e4d08-156">Add hello following method toohello **Program** class:</span></span>

       public static async void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting toohub");
                Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
                Console.WriteLine("Retrieving twin");
                await Client.GetTwinAsync();
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

    <span data-ttu-id="e4d08-157">Hej **klienten** objektet innehåller alla hello-metoder som du behöver toointeract med enheten twins från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="e4d08-157">hello **Client** object exposes all hello methods you require toointeract with device twins from hello device.</span></span> <span data-ttu-id="e4d08-158">Hej koden som visas ovan, initierar hello **klienten** objekt och hämtar hello enheten dubbla för **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-158">hello code shown above, initializes hello **Client** object, and then retrieves hello device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="e4d08-159">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="e4d08-159">Add hello following method toohello **Program** class:</span></span>
   
        public static async void ReportConnectivity()
        {
            try
            {
                Console.WriteLine("Sending connectivity data as reported property");
                
                TwinCollection reportedProperties, connectivity;
                reportedProperties = new TwinCollection();
                connectivity = new TwinCollection();
                connectivity["type"] = "cellular";
                reportedProperties["connectivity"] = connectivity;
                await Client.UpdateReportedPropertiesAsync(reportedProperties);
            }
            catch (Exception ex)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", ex.Message);
            }
        }

   <span data-ttu-id="e4d08-160">Hej koden ovan uppdateringar **myDeviceId**har rapporterats egenskap med hello anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="e4d08-160">hello code above updates **myDeviceId**'s reported property with hello connectivity information.</span></span>

1. <span data-ttu-id="e4d08-161">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="e4d08-161">Finally, add hello following lines toohello **Main** method:</span></span>
   
       try
       {
            InitClient();
            ReportConnectivity();
       }
       catch (Exception ex)
       {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
       }
       Console.WriteLine("Press Enter tooexit.");
       Console.ReadLine();

1. <span data-ttu-id="e4d08-162">Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **ReportConnectivity** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-162">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **ReportConnectivity** project is **Start**.</span></span> <span data-ttu-id="e4d08-163">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="e4d08-163">Build hello solution.</span></span>
1. <span data-ttu-id="e4d08-164">Kör det här programmet genom att högerklicka på hello **ReportConnectivity** projekt och välja **felsöka**, följt av **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="e4d08-164">Run this application by right-clicking on hello **ReportConnectivity** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="e4d08-165">Du bör se information om hello dubbla och sedan skicka anslutning som en *rapporterade egenskapen*.</span><span class="sxs-lookup"><span data-stu-id="e4d08-165">You should see it getting hello twin information, and then sending connectivity as a *reported property*.</span></span>
   
    ![Kör enhetsanslutning app tooreport][img-rundeviceapp]
    
    
1. <span data-ttu-id="e4d08-167">Nu när hello enheten rapporterade dess anslutningsinformation, bör den visas i båda frågor.</span><span class="sxs-lookup"><span data-stu-id="e4d08-167">Now that hello device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="e4d08-168">Kör hello .NET **AddTagsAndQuery** app toorun hello frågar igen.</span><span class="sxs-lookup"><span data-stu-id="e4d08-168">Run hello .NET **AddTagsAndQuery** app toorun hello queries again.</span></span> <span data-ttu-id="e4d08-169">Den här gången **myDeviceId** ska visas i båda frågeresultaten.</span><span class="sxs-lookup"><span data-stu-id="e4d08-169">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![Enhetsanslutning som har rapporterats][img-tagappsuccess]

## <a name="next-steps"></a><span data-ttu-id="e4d08-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4d08-171">Next steps</span></span>
<span data-ttu-id="e4d08-172">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="e4d08-172">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="e4d08-173">Du lagt till enhetsmetadata som taggar från en backend-app och skrev en simulerad enhet app tooreport anslutning enhetsinformation i hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="e4d08-173">You added device metadata as tags from a back-end app, and wrote a simulated device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="e4d08-174">Du också lära sig hur tooquery informationen med hjälp av frågespråket för hello SQL-liknande IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="e4d08-174">You also learned how tooquery this information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="e4d08-175">Använd hello följande resurser toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="e4d08-175">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="e4d08-176">Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="e4d08-176">send telemetry from devices with hello [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="e4d08-177">Konfigurera enheter med hjälp av enheten två egenskaper med hello [Använd önskad egenskaper tooconfigure enheter] [ lnk-twin-how-to-configure] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="e4d08-177">configure devices using device twin's desired properties with hello [Use desired properties tooconfigure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="e4d08-178">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app) med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="e4d08-178">control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-csharp-twin-getstarted/addtagapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/createdeviceapp.png
[img-clientnuget]: media/iot-hub-csharp-csharp-twin-getstarted/clientsdknuget.png
[img-rundeviceapp]: media/iot-hub-csharp-csharp-twin-getstarted/rundeviceapp.png
[img-tagappsuccess]: media/iot-hub-csharp-csharp-twin-getstarted/tagappsuccess.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/

[lnk-device-identity-csharp]: iot-hub-csharp-csharp-getstarted.md#DeviceIdentity_csharp
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

