---
title: "Kom igång med Azure IoT Hub-enhet twins (.NET/.NET) | Microsoft Docs"
description: "Hur du använder Azure IoT Hub-enhet twins att lägga till taggar och sedan använda en IoT-hubb-fråga. Du kan använda Azure IoT-enhet SDK för .NET för att implementera appen simulerade enheten och tjänsten Azure IoT SDK för .NET att implementera en service-app som lägger till taggar och IoT-hubb-frågan körs."
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
ms.openlocfilehash: 6073d594117e69676b753a1e3af25fffa3583a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-device-twins-netnet"></a><span data-ttu-id="da458-104">Kom igång med enheten twins (.NET/.NET)</span><span class="sxs-lookup"><span data-stu-id="da458-104">Get started with device twins (.NET/.NET)</span></span>
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="da458-105">I slutet av den här kursen har du apparna .NET-konsolen:</span><span class="sxs-lookup"><span data-stu-id="da458-105">At the end of this tutorial, you will have these .NET console apps:</span></span>

* <span data-ttu-id="da458-106">**CreateDeviceIdentity**, en .NET-app som skapar en enhets-ID och associerade säkerhetsnyckeln ansluta din simulerade enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="da458-106">**CreateDeviceIdentity**, a .NET app which creates a device identity and associated security key to connect your simulated device app.</span></span>
* <span data-ttu-id="da458-107">**AddTagsAndQuery**, en .NET backend-app som lägger till taggar och frågar enheten twins.</span><span class="sxs-lookup"><span data-stu-id="da458-107">**AddTagsAndQuery**, a .NET back-end app which adds tags and queries device twins.</span></span>
* <span data-ttu-id="da458-108">**ReportConnectivity**, en enhet .NET-app som simulerar en enhet som ansluter till din IoT-hubb med enhetens identitet skapade tidigare och rapporterar tillståndet anslutningen.</span><span class="sxs-lookup"><span data-stu-id="da458-108">**ReportConnectivity**, a .NET device app which simulates a device that connects to your IoT hub with the device identity created earlier, and reports its connectivity condition.</span></span>

> [!NOTE]
> <span data-ttu-id="da458-109">Artikeln [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om Azure IoT-SDK: er som du kan använda för att skapa både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="da458-109">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>
> 
> 

<span data-ttu-id="da458-110">Den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="da458-110">To complete this tutorial you need the following:</span></span>

* <span data-ttu-id="da458-111">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="da458-111">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="da458-112">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="da458-112">An active Azure account.</span></span> <span data-ttu-id="da458-113">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="da458-113">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="da458-114">Om du vill skapa enhetens identitet programmässigt i stället läsa motsvarande avsnitt i den [ansluta din simulerade enhet till din IoT-hubb med hjälp av .NET] [ lnk-device-identity-csharp] artikel.</span><span class="sxs-lookup"><span data-stu-id="da458-114">If you want to create the device identity programmatically instead, read the corresponding section in the [Connect your simulated device to your IoT hub using .NET][lnk-device-identity-csharp] article.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="da458-115">Skapa service-appen</span><span class="sxs-lookup"><span data-stu-id="da458-115">Create the service app</span></span>
<span data-ttu-id="da458-116">I det här avsnittet skapar du en .NET-konsolapp (med C#) som lägger till platsen metadata i enheten-dubbla som är associerade med **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="da458-116">In this section, you create a .NET console app (using C#) that adds location metadata to the device twin associated with **myDeviceId**.</span></span> <span data-ttu-id="da458-117">Frågar sedan enheten-twins lagras i IoT-hubb som att markera de enheter som finns i USA och de som rapporterats av en mobil anslutning.</span><span class="sxs-lookup"><span data-stu-id="da458-117">It then queries the device twins stored in the IoT hub selecting the devices located in the US, and then the ones that reported a cellular connection.</span></span>

1. <span data-ttu-id="da458-118">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den aktuella lösningen med hjälp av projektmallen **Konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="da458-118">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="da458-119">Namnge projektet **AddTagsAndQuery**.</span><span class="sxs-lookup"><span data-stu-id="da458-119">Name the project **AddTagsAndQuery**.</span></span>
   
    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]
1. <span data-ttu-id="da458-121">I Solution Explorer högerklickar du på den **AddTagsAndQuery** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="da458-121">In Solution Explorer, right-click the **AddTagsAndQuery** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="da458-122">I den **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices**.</span><span class="sxs-lookup"><span data-stu-id="da458-122">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices**.</span></span> <span data-ttu-id="da458-123">Välj **installera** att installera den **Microsoft.Azure.Devices** paketet och Godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="da458-123">Select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="da458-124">Denna procedur hämtar, installerar och lägger till en referens för [NuGet-paketetet SDK för Azure IoT-tjänster][lnk-nuget-service-sdk] och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="da458-124">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="da458-126">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="da458-126">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
1. <span data-ttu-id="da458-127">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="da458-127">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="da458-128">Ersätt platshållarvärdet med IoT Hub-anslutningssträngen som du skapade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="da458-128">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
1. <span data-ttu-id="da458-129">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="da458-129">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="da458-130">Den **RegistryManager** klassen innehåller alla metoder som krävs för att interagera med enheten twins från tjänsten.</span><span class="sxs-lookup"><span data-stu-id="da458-130">The **RegistryManager** class exposes all the methods required to interact with device twins from the service.</span></span> <span data-ttu-id="da458-131">Föregående kod först initierar den **registryManager** objekt och hämtar enheten dubbla för **myDeviceId**, och slutligen uppdateras taggarna med informationen som önskad plats.</span><span class="sxs-lookup"><span data-stu-id="da458-131">The previous code first initializes the **registryManager** object, then retrieves the device twin for **myDeviceId**, and finally updates its tags with the desired location information.</span></span>
   
    <span data-ttu-id="da458-132">När du har uppdaterat, körs två frågor: först väljer enheten twins av enheter som finns i den **Redmond43** anläggningen och andra förfinar frågan för att markera de enheter som även är anslutna via mobilnät.</span><span class="sxs-lookup"><span data-stu-id="da458-132">After updating, it executes two queries: the first selects only the device twins of devices located in the **Redmond43** plant, and the second refines the query to select only the devices that are also connected through cellular network.</span></span>
   
    <span data-ttu-id="da458-133">Observera att föregående kod när det skapas den **frågan** objekt, anger ett maximalt antal returnerade dokument.</span><span class="sxs-lookup"><span data-stu-id="da458-133">Note that the previous code, when it creates the **query** object, specifies a maximum number of returned documents.</span></span> <span data-ttu-id="da458-134">Den **frågan** objektet innehåller en **HasMoreResults** boolesk egenskap som du kan använda för att anropa den **GetNextAsTwinAsync** metoder flera gånger för att hämta alla resultat.</span><span class="sxs-lookup"><span data-stu-id="da458-134">The **query** object contains a **HasMoreResults** boolean property that you can use to invoke the **GetNextAsTwinAsync** methods multiple times to retrieve all results.</span></span> <span data-ttu-id="da458-135">En metod som kallas **GetNextAsJson** är tillgänglig för resultat som är inte enheten twins, till exempel resultatet av aggregering frågor.</span><span class="sxs-lookup"><span data-stu-id="da458-135">A method called **GetNextAsJson** is available for results that are not device twins, for example, results of aggregation queries.</span></span>
1. <span data-ttu-id="da458-136">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="da458-136">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

1. <span data-ttu-id="da458-137">I Solution Explorer, öppna den **ange Startprojekt...**  och kontrollera att den **åtgärd** för **AddTagsAndQuery** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="da458-137">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **AddTagsAndQuery** project is **Start**.</span></span> <span data-ttu-id="da458-138">Skapa lösningen.</span><span class="sxs-lookup"><span data-stu-id="da458-138">Build the solution.</span></span>
1. <span data-ttu-id="da458-139">Kör det här programmet genom att högerklicka på den **AddTagsAndQuery** projekt och välja **felsöka**, följt av **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="da458-139">Run this application by right-clicking on the **AddTagsAndQuery** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="da458-140">Du bör se en enhet i resultaten för frågan ställa för alla enheter i **Redmond43** och ingen för frågan som begränsar resultat till enheter som använder ett mobilnät.</span><span class="sxs-lookup"><span data-stu-id="da458-140">You should see one device in the results for the query asking for all devices located in **Redmond43** and none for the query that restricts the results to devices that use a cellular network.</span></span>
   
    ![Resultatet av frågan i fönstret][img-addtagapp]

<span data-ttu-id="da458-142">I nästa avsnitt skapar du en enhetsapp som rapporterar information om anslutningar och ändras resultatet av frågan i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="da458-142">In the next section, you create a device app that reports the connectivity information and changes the result of the query in the previous section.</span></span>

## <a name="create-the-device-app"></a><span data-ttu-id="da458-143">Skapa enhetsapp</span><span class="sxs-lookup"><span data-stu-id="da458-143">Create the device app</span></span>
<span data-ttu-id="da458-144">I det här avsnittet skapar du en .NET-konsolapp som ansluter till din hubb som **myDeviceId**, och sedan uppdaterar egenskaperna rapporterade att innehålla den information som den är ansluten med hjälp av ett mobilnät.</span><span class="sxs-lookup"><span data-stu-id="da458-144">In this section, you create a .NET console app that connects to your hub as **myDeviceId**, and then updates its reported properties to contain the information that it is connected using a cellular network.</span></span>

1. <span data-ttu-id="da458-145">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den aktuella lösningen med hjälp av projektmallen **Konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="da458-145">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="da458-146">Namnge projektet **ReportConnectivity**.</span><span class="sxs-lookup"><span data-stu-id="da458-146">Name the project **ReportConnectivity**.</span></span>
   
    ![Ny Visual C# Windows Classic enhetsapp][img-createdeviceapp]
    
1. <span data-ttu-id="da458-148">I Solution Explorer högerklickar du på den **ReportConnectivity** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="da458-148">In Solution Explorer, right-click the **ReportConnectivity** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="da458-149">I den **NuGet Package Manager** väljer **Bläddra** och Sök efter **microsoft.azure.devices.client**.</span><span class="sxs-lookup"><span data-stu-id="da458-149">In the **NuGet Package Manager** window, select **Browse** and search for **microsoft.azure.devices.client**.</span></span> <span data-ttu-id="da458-150">Välj **installera** att installera den **Microsoft.Azure.Devices.Client** paketet och Godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="da458-150">Select **Install** to install the **Microsoft.Azure.Devices.Client** package, and accept the terms of use.</span></span> <span data-ttu-id="da458-151">Den här proceduren hämtar, installerar och lägger till en referens till den [Azure IoT-enhet SDK] [ lnk-nuget-client-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="da458-151">This procedure downloads, installs, and adds a reference to the [Azure IoT device SDK][lnk-nuget-client-sdk] NuGet package and its dependencies.</span></span>
   
    ![NuGet-Pakethanteraren fönstret-klientappen][img-clientnuget]
1. <span data-ttu-id="da458-153">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="da458-153">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using Newtonsoft.Json;

1. <span data-ttu-id="da458-154">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="da458-154">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="da458-155">Ersätt platshållaren värdet med den anslutningssträng för enheten som du antecknade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="da458-155">Replace the placeholder value with the device connection string that you noted in the previous section.</span></span>
   
        static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
        static DeviceClient Client = null;

1. <span data-ttu-id="da458-156">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="da458-156">Add the following method to the **Program** class:</span></span>

       public static async void InitClient()
        {
            try
            {
                Console.WriteLine("Connecting to hub");
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

    <span data-ttu-id="da458-157">Den **klienten** objektet innehåller de metoder som du behöver för att interagera med enheten twins från enheten.</span><span class="sxs-lookup"><span data-stu-id="da458-157">The **Client** object exposes all the methods you require to interact with device twins from the device.</span></span> <span data-ttu-id="da458-158">Koden som visas ovan, initierar den **klienten** objekt och hämtar sedan enheten dubbla för **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="da458-158">The code shown above, initializes the **Client** object, and then retrieves the device twin for **myDeviceId**.</span></span>

1. <span data-ttu-id="da458-159">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="da458-159">Add the following method to the **Program** class:</span></span>
   
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

   <span data-ttu-id="da458-160">Koden ovan uppdateringar **myDeviceId**har rapporterats egenskapen med anslutningsinformation om.</span><span class="sxs-lookup"><span data-stu-id="da458-160">The code above updates **myDeviceId**'s reported property with the connectivity information.</span></span>

1. <span data-ttu-id="da458-161">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="da458-161">Finally, add the following lines to the **Main** method:</span></span>
   
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
       Console.WriteLine("Press Enter to exit.");
       Console.ReadLine();

1. <span data-ttu-id="da458-162">I Solution Explorer, öppna den **ange Startprojekt...**  och kontrollera att den **åtgärd** för **ReportConnectivity** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="da458-162">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **ReportConnectivity** project is **Start**.</span></span> <span data-ttu-id="da458-163">Skapa lösningen.</span><span class="sxs-lookup"><span data-stu-id="da458-163">Build the solution.</span></span>
1. <span data-ttu-id="da458-164">Kör det här programmet genom att högerklicka på den **ReportConnectivity** projekt och välja **felsöka**, följt av **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="da458-164">Run this application by right-clicking on the **ReportConnectivity** project and selecting **Debug**, followed by **Start new instance**.</span></span> <span data-ttu-id="da458-165">Du bör se får dubbla information och sedan skickar anslutning som en *rapporterade egenskapen*.</span><span class="sxs-lookup"><span data-stu-id="da458-165">You should see it getting the twin information, and then sending connectivity as a *reported property*.</span></span>
   
    ![Kör enhetsapp till rapporten-anslutning][img-rundeviceapp]
    
    
1. <span data-ttu-id="da458-167">Nu när enheten rapporteras dess anslutningsinformation, bör den visas i båda frågor.</span><span class="sxs-lookup"><span data-stu-id="da458-167">Now that the device reported its connectivity information, it should appear in both queries.</span></span> <span data-ttu-id="da458-168">Kör .NET **AddTagsAndQuery** app att köras frågorna igen.</span><span class="sxs-lookup"><span data-stu-id="da458-168">Run the .NET **AddTagsAndQuery** app to run the queries again.</span></span> <span data-ttu-id="da458-169">Den här gången **myDeviceId** ska visas i båda frågeresultaten.</span><span class="sxs-lookup"><span data-stu-id="da458-169">This time **myDeviceId** should appear in both query results.</span></span>
   
    ![Enhetsanslutning som har rapporterats][img-tagappsuccess]

## <a name="next-steps"></a><span data-ttu-id="da458-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da458-171">Next steps</span></span>
<span data-ttu-id="da458-172">I den här självstudiekursen konfigurerade du en ny IoT Hub på Azure Portal och skapade sedan en enhetsidentitet i IoT-hubbens identitetsregister.</span><span class="sxs-lookup"><span data-stu-id="da458-172">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="da458-173">Du lagt till enhetsmetadata som taggar från en backend-app och skrev en simulerad enhetsapp till enheten anslutningsinformation i dubbla för enheten.</span><span class="sxs-lookup"><span data-stu-id="da458-173">You added device metadata as tags from a back-end app, and wrote a simulated device app to report device connectivity information in the device twin.</span></span> <span data-ttu-id="da458-174">Du också fått lära dig hur man frågar den här informationen med hjälp av frågespråket i SQL-liknande IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="da458-174">You also learned how to query this information using the SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="da458-175">Använd följande resurser för att lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="da458-175">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="da458-176">Skicka telemetri från enheter med den [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="da458-176">send telemetry from devices with the [Get started with IoT Hub][lnk-iothub-getstarted] tutorial,</span></span>
* <span data-ttu-id="da458-177">Konfigurera enheter med hjälp av enheten två egenskaper med den [Använd önskad egenskaper att konfigurera enheter] [ lnk-twin-how-to-configure] självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="da458-177">configure devices using device twin's desired properties with the [Use desired properties to configure devices][lnk-twin-how-to-configure] tutorial,</span></span>
* <span data-ttu-id="da458-178">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app) med den [metoder från direkt] [ lnk-methods-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="da458-178">control devices interactively (such as turning on a fan from a user-controlled app) with the [Use direct methods][lnk-methods-tutorial] tutorial.</span></span>

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

