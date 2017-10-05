---
title: Skapa en Azure IoT-Edge-modul med C# | Microsoft Docs
description: "Den här kursen visar hur du skriver en TIVERA data konverteraren modul med de senaste Azure IoT kant NuGet-paket, Visual Studio Code och C#."
services: iot-hub
author: jeffreyCline
manager: timlt
keywords: "Azure iot, självstudier, modul, nuget, vscode, csharp, kant"
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: jcline
ms.openlocfilehash: 7175ffc8de2c043593d61143b402484d33e4a8cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a><span data-ttu-id="ae285-104">Skapa en Azure IoT-Edge-modul med C & #x23;</span><span class="sxs-lookup"><span data-stu-id="ae285-104">Create an Azure IoT Edge Module with C&#x23;</span></span>

<span data-ttu-id="ae285-105">Den här kursen visar hur du skapar en modul för `Azure IoT Edge` med `Visual Studio Code` och `C#`.</span><span class="sxs-lookup"><span data-stu-id="ae285-105">This tutorial showcases how to create a module for `Azure IoT Edge` using `Visual Studio Code` and `C#`.</span></span>

<span data-ttu-id="ae285-106">I den här självstudiekursen kommer vi att gå igenom miljön inställning och hur du skriver en [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data konverteraren modul med senast `Azure IoT Edge NuGet` paket.</span><span class="sxs-lookup"><span data-stu-id="ae285-106">In this tutorial, we walk through environment set-up and how to write a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using the latest `Azure IoT Edge NuGet` packages.</span></span> 

>[!NOTE]
<span data-ttu-id="ae285-107">Den här kursen använder den `.NET Core SDK`, som har stöd för flera plattformar kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="ae285-107">This tutorial is using the `.NET Core SDK`, which supports cross-platform compatibility.</span></span> <span data-ttu-id="ae285-108">Följande självstudierna är skrivna med hjälp av den `Windows 10` operativsystem.</span><span class="sxs-lookup"><span data-stu-id="ae285-108">The following tutorial is written using the `Windows 10` operating system.</span></span> <span data-ttu-id="ae285-109">Vissa kommandon i den här kursen kan vara olika beroende på din `development environment`.</span><span class="sxs-lookup"><span data-stu-id="ae285-109">Some of the commands in this tutorial may be different depending on your `development environment`.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="ae285-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ae285-110">Prerequisites</span></span>

<span data-ttu-id="ae285-111">I det här avsnittet vi ställa in miljön för `Azure IoT Edge` modulen utveckling.</span><span class="sxs-lookup"><span data-stu-id="ae285-111">In this section, we set-up your environment for `Azure IoT Edge` module development.</span></span> <span data-ttu-id="ae285-112">Det gäller både **64-bitars Windows** och **64-bitars Linux (8 Ubuntu/Debian)** operativsystem.</span><span class="sxs-lookup"><span data-stu-id="ae285-112">It applies to both **64-bit Windows** and **64-bit Linux (Ubuntu/Debian 8)** operating systems.</span></span>

<span data-ttu-id="ae285-113">Följande programvara krävs:</span><span class="sxs-lookup"><span data-stu-id="ae285-113">The following software is required:</span></span>

- [<span data-ttu-id="ae285-114">Git-klient</span><span class="sxs-lookup"><span data-stu-id="ae285-114">Git Client</span></span>](https://git-scm.com/downloads)
- [<span data-ttu-id="ae285-115">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="ae285-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#windowscmd)
- [<span data-ttu-id="ae285-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ae285-116">Visual Studio Code</span></span>](https://code.visualstudio.com/)

<span data-ttu-id="ae285-117">Du behöver inte klona lagringsplatsen för det här exemplet, men alla exempelkoden som beskrivs i den här kursen finns i följande lagringsplats:</span><span class="sxs-lookup"><span data-stu-id="ae285-117">You do not need to clone the repo for this sample, however all of the sample code discussed in this tutorial is located in the following repository:</span></span>

- <span data-ttu-id="ae285-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="ae285-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a><span data-ttu-id="ae285-119">Komma igång</span><span class="sxs-lookup"><span data-stu-id="ae285-119">Getting started</span></span>

1. <span data-ttu-id="ae285-120">Installera `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="ae285-120">Install `.NET Core SDK`.</span></span>
2. <span data-ttu-id="ae285-121">Installera `Visual Studio Code` och `C# extension` från Visual Studio Code Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ae285-121">Install `Visual Studio Code` and the `C# extension` from the Visual Studio Code Marketplace.</span></span>

<span data-ttu-id="ae285-122">Visa den här [snabb självstudiekurs](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) om hur du kommer igång med `Visual Studio Code` och `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="ae285-122">View this [quick video tutorial](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) about how to get started using `Visual Studio Code` and the `.NET Core SDK`.</span></span>

## <a name="creating-the-azure-iot-edge-converter-module"></a><span data-ttu-id="ae285-123">Azure IoT kant modulen konverteraren</span><span class="sxs-lookup"><span data-stu-id="ae285-123">Creating the Azure IoT Edge converter module</span></span>

1. <span data-ttu-id="ae285-124">Initiera en ny `.NET Core` C# klassbiblioteksprojektet:</span><span class="sxs-lookup"><span data-stu-id="ae285-124">Initialize a new `.NET Core` class library C# project:</span></span>
    - <span data-ttu-id="ae285-125">Öppna en kommandotolk (`Windows + R` -> `cmd` -> `enter`).</span><span class="sxs-lookup"><span data-stu-id="ae285-125">Open a command prompt (`Windows + R` -> `cmd` -> `enter`).</span></span>
    - <span data-ttu-id="ae285-126">Navigera till mappen där du vill skapa den `C#` projekt.</span><span class="sxs-lookup"><span data-stu-id="ae285-126">Navigate to the folder where you'd like to create the `C#` project.</span></span>
    - <span data-ttu-id="ae285-127">Typen **dotnet nya classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="ae285-127">Type **dotnet new classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span></span> 
    - <span data-ttu-id="ae285-128">Det här kommandot skapar en tom klass som heter `Class1.cs` i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="ae285-128">This command creates an empty class called `Class1.cs` in your projects directory.</span></span>
2. <span data-ttu-id="ae285-129">Navigera till mappen där vi just skapade klassbiblioteksprojektet genom att skriva **cd IoTEdgeConverterModule**.</span><span class="sxs-lookup"><span data-stu-id="ae285-129">Navigate to the folder where we just created the class library project by typing **cd IoTEdgeConverterModule**.</span></span>
3. <span data-ttu-id="ae285-130">Öppna projektet i `Visual Studio Code` genom att skriva **kod.**.</span><span class="sxs-lookup"><span data-stu-id="ae285-130">Open the project in `Visual Studio Code` by typing **code .**.</span></span>
4. <span data-ttu-id="ae285-131">När projektet har öppnats i `Visual Studio Code`, klicka på den **IoTEdgeConverterModule.csproj** att öppna filen som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ae285-131">Once the project is opened in `Visual Studio Code`, click on the **IoTEdgeConverterModule.csproj** to open the file as shown in the following image:</span></span>

    ![Visual Studio Code redigeringsfönstret](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. <span data-ttu-id="ae285-133">Infoga den `XML` blob som visas i följande kodavsnitt mellan stängningen `PropertyGroup` taggen och stängningen `Project` tagga; rad sex i den föregående bilden och spara filen genom att trycka på `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ae285-133">Insert the `XML` blob shown in the following code snippet between the closing `PropertyGroup` tag and the closing `Project` tag; line six in the preceding image and save the file by pressing `Ctrl` + `S`.</span></span>

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. <span data-ttu-id="ae285-134">När du sparar den `.csproj` filen `Visual Studio Code` fråga ska visas med en `unresolved dependencies` dialogrutan som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ae285-134">Once you save the `.csproj` file, `Visual Studio Code` should prompt you with an `unresolved dependencies` dialog as seen in the following image:</span></span> 

    ![Visual Studio Code återställning beroenden dialogrutan](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    <span data-ttu-id="ae285-136">en) Klicka på `Restore` att återställa alla referenser i projekten `.csproj` filen inklusive den `PackageReferences` vi har lagt till.</span><span class="sxs-lookup"><span data-stu-id="ae285-136">a) Click `Restore` to restore all of the references in the projects `.csproj` file including the `PackageReferences` we have added.</span></span> 

    <span data-ttu-id="ae285-137">b) `Visual Studio Code` skapar automatiskt den `project.assets.json` filen i projektet `obj` mapp.</span><span class="sxs-lookup"><span data-stu-id="ae285-137">b) `Visual Studio Code` automatically creates the `project.assets.json` file in your projects `obj` folder.</span></span> <span data-ttu-id="ae285-138">Den här filen innehåller information om projektberoenden att göra efterföljande återställningar snabbare.</span><span class="sxs-lookup"><span data-stu-id="ae285-138">This file contains information about your project's dependencies to make subsequent restores quicker.</span></span>
 
    >[!NOTE]
    <span data-ttu-id="ae285-139">`.NET Core Tools`är nu MSBuild-baserade.</span><span class="sxs-lookup"><span data-stu-id="ae285-139">`.NET Core Tools` are now MSBuild-based.</span></span> <span data-ttu-id="ae285-140">Vilket innebär en `.csproj` projektfilen har skapats i stället för en `project.json`.</span><span class="sxs-lookup"><span data-stu-id="ae285-140">Which means a `.csproj` project file is created instead of a `project.json`.</span></span>

    - <span data-ttu-id="ae285-141">Om `Visual Studio Code` visas inte som är det ok, vi kan göra det manuellt.</span><span class="sxs-lookup"><span data-stu-id="ae285-141">If `Visual Studio Code` does not prompt you that is ok, we can do it manually.</span></span> <span data-ttu-id="ae285-142">Öppna den `Visual Studio Code` integrerad terminalfönster genom att trycka på `Ctrl`  +  `backtick` nycklar eller använda menyerna `View`  ->  `Integrated Terminal`.</span><span class="sxs-lookup"><span data-stu-id="ae285-142">Open the `Visual Studio Code` integrated terminal window by pressing the `Ctrl` + `backtick` keys or using the menus `View` -> `Integrated Terminal`.</span></span>
    - <span data-ttu-id="ae285-143">I den `Integrated Terminal` fönstret typen **dotnet återställning**.</span><span class="sxs-lookup"><span data-stu-id="ae285-143">In the `Integrated Terminal` window type **dotnet restore**.</span></span>
    
7. <span data-ttu-id="ae285-144">Byt namn på den `Class1.cs` filen till `BleConverterModule.cs`.</span><span class="sxs-lookup"><span data-stu-id="ae285-144">Rename the `Class1.cs` file to `BleConverterModule.cs`.</span></span> 

    <span data-ttu-id="ae285-145">en) för att byta namn på filen först klickar du på filen tryck sedan på den `F2` nyckel.</span><span class="sxs-lookup"><span data-stu-id="ae285-145">a) To rename the file first click on the file then press the `F2` key.</span></span>
    
    <span data-ttu-id="ae285-146">b) typ i det nya namnet **BleConverterModule**, som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ae285-146">b) Type in the new name **BleConverterModule**, as seen in the following image:</span></span>

    ![Visual Studio Code byta namn på en klass](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. <span data-ttu-id="ae285-148">Ersätt den befintliga koden i den `BleConverterModule.cs` filen genom att kopiera och klistra in följande kodfragment i din `BleConverterModule.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="ae285-148">Replace the existing code in the `BleConverterModule.cs` file by copying and pasting the following code snippet into your `BleConverterModule.cs` file.</span></span>

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Globalization;
   using System.Linq;
   using System.Text;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace IoTEdgeConverterModule
   {
       public class BleConverterModule : IGatewayModule, IGatewayModuleStart
       {
           private Broker broker;
           private string configuration;
           private int messageCount;

           public void Create(Broker broker, byte[] configuration)
           {
               this.broker = broker;
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Start()
           {
           }

           public void Destroy()
           {
           }

           public void Receive(Message received_message)
           {
               string recMsg = Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               BleData receivedData = JsonConvert.DeserializeObject<BleData>(recMsg);

               float temperature = float.Parse(receivedData.Temperature, CultureInfo.InvariantCulture.NumberFormat); 
               Dictionary<string, string> receivedProperties = received_message.Properties;
            
               Dictionary<string, string> properties = new Dictionary<string, string>();
               properties.Add("source", receivedProperties["source"]);
               properties.Add("macAddress", receivedProperties["macAddress"]);
               properties.Add("temperatureAlert", temperature > 30 ? "true" : "false");
   
               String content = String.Format("{0} \"deviceId\": \"Intel NUC Gateway\", \"messageId\": {1}, \"temperature\": {2} {3}", "{", ++this.messageCount, temperature, "}");
               Message messageToPublish = new Message(content, properties);
   
               this.broker.Publish(messageToPublish);
           }
       }
   }
   ```

9. <span data-ttu-id="ae285-149">Spara filen genom att trycka på `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ae285-149">Save the file by pressing `Ctrl` + `S`.</span></span>

10. <span data-ttu-id="ae285-150">Skapa en ny fil med namnet `Untitled-1` genom att trycka på `Ctrl`  +  `N` nycklar som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ae285-150">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys as seen in the following image:</span></span>

    ![Visual Studio Code ny fil](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. <span data-ttu-id="ae285-152">Att deserialisera den `JSON` objekt som vi får från det simulerade `BLE` enhet, kopiera in följande kod i den `Untitled-1` filen kod editor-fönstret.</span><span class="sxs-lookup"><span data-stu-id="ae285-152">To deserialize the `JSON` object that we receive from the simulated `BLE` device, copy the following code into the `Untitled-1` file code editor window.</span></span> 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace IoTEdgeConverterModule
   {
       public class BleData
       {
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

12. <span data-ttu-id="ae285-153">Spara filen som `BleData.cs` genom att trycka på `Ctrl`  +  `Shift`  +  `S` nycklar.</span><span class="sxs-lookup"><span data-stu-id="ae285-153">Save the file as `BleData.cs` by pressing `Ctrl` + `Shift` + `S` keys.</span></span>
    - <span data-ttu-id="ae285-154">På den dialogrutan Spara som, i den `Save as Type` väljer du `C# (*.cs;*.csx)` som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ae285-154">On the save as dialog box, in the `Save as Type` dropdown menu, select `C# (*.cs;*.csx)` as seen in the following image:</span></span>

    ![Visual Studio Code Spara som dialog](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. <span data-ttu-id="ae285-156">Skapa en ny fil med namnet `Untitled-1` genom att trycka på `Ctrl`  +  `N` nycklar.</span><span class="sxs-lookup"><span data-stu-id="ae285-156">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys.</span></span>

14. <span data-ttu-id="ae285-157">Kopiera och klistra in följande kodavsnitt i den `Untitled-1` filen.</span><span class="sxs-lookup"><span data-stu-id="ae285-157">Copy and paste the following code snippet into the `Untitled-1` file.</span></span> <span data-ttu-id="ae285-158">Den här klassen är en `Azure IoT Edge` module, som vi använder för att spara data togs emot från våra `BleConverterModule`.</span><span class="sxs-lookup"><span data-stu-id="ae285-158">This class is a `Azure IoT Edge` module, which we use to output the data received from our `BleConverterModule`.</span></span>

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Gateway;
   using Newtonsoft.Json;
   
   namespace PrinterModule
   {
       public class DotNetPrinterModule : IGatewayModule
       {
           private string configuration;
           public void Create(Broker broker, byte[] configuration)
           {
               this.configuration = System.Text.Encoding.UTF8.GetString(configuration);
           }
   
           public void Destroy()
           {
           }
   
           public void Receive(Message received_message)
           {
               string recMsg = System.Text.Encoding.UTF8.GetString(received_message.Content, 0, received_message.Content.Length);
               Dictionary<string, string> receivedProperties = received_message.Properties;
               
               BleConverterData receivedData = JsonConvert.DeserializeObject<BleConverterData>(recMsg);
   
               Console.WriteLine();
               Console.WriteLine("Module           : [DotNetPrinterModule]");
               Console.WriteLine("received_message : {0}", recMsg);
   
               if(received_message.Properties["source"] == "bleTelemetry")
               {
                   Console.WriteLine("Source           : {0}", receivedProperties["source"]);
                   Console.WriteLine("MAC address      : {0}", receivedProperties["macAddress"]);
                   Console.WriteLine("Temperature Alert: {0}", receivedProperties["temperatureAlert"]);
                   Console.WriteLine("Deserialized Obj : [BleConverterData]");
                   Console.WriteLine("  DeviceId       : {0}", receivedData.DeviceId);
                   Console.WriteLine("  MessageId      : {0}", receivedData.MessageId);
                   Console.WriteLine("  Temperature    : {0}", receivedData.Temperature);
               }
   
               Console.WriteLine();
           }
       }
   }
   ```

15. <span data-ttu-id="ae285-159">Spara filen som `DotNetPrinterModule.cs` genom att trycka på `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ae285-159">Save the file as `DotNetPrinterModule.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="ae285-160">På den dialogrutan Spara som, i den `Save as Type` väljer du `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="ae285-160">On the save as dialog box, in the `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

16. <span data-ttu-id="ae285-161">Skapa en ny fil med namnet `Untitled-1` genom att trycka på `Ctrl`  +  `N` nycklar.</span><span class="sxs-lookup"><span data-stu-id="ae285-161">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys.</span></span>

17. <span data-ttu-id="ae285-162">Att deserialisera den `JSON` objekt som vi får från den `BleConverterModule`, kopiera och klistra in följande kod fragment till den `Untitled-1` filen.</span><span class="sxs-lookup"><span data-stu-id="ae285-162">To deserialize the `JSON` object that we receive from the `BleConverterModule`, Copy and paste the following code snippet into the `Untitled-1` file.</span></span> 

   ```csharp
   using System;
   using Newtonsoft.Json;

   namespace PrinterModule
   {
       public class BleConverterData
       {
           [JsonProperty(PropertyName = "deviceId")]
           public string DeviceId { get; set; }
   
           [JsonProperty(PropertyName = "messageId")]
           public string MessageId { get; set; }
   
           [JsonProperty(PropertyName = "temperature")]
           public string Temperature { get; set; }
       }
   }
   ```

18. <span data-ttu-id="ae285-163">Spara filen som `BleConverterData.cs` genom att trycka på `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ae285-163">Save the file as `BleConverterData.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="ae285-164">På den dialogrutan Spara som, i den `Save as Type` väljer du `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="ae285-164">On the save as dialog box, in the `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

19. <span data-ttu-id="ae285-165">Skapa en ny fil med namnet `Untitled-1` genom att trycka på `Ctrl`  +  `N` nycklar.</span><span class="sxs-lookup"><span data-stu-id="ae285-165">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys.</span></span>

20. <span data-ttu-id="ae285-166">Kopiera och klistra in följande kodavsnitt i den `Untitled-1` filen.</span><span class="sxs-lookup"><span data-stu-id="ae285-166">Copy and paste the following code snippet into the `Untitled-1` file.</span></span>

   ```json
   {
       "loaders": [
           {
               "type": "dotnetcore",
               "name": "dotnetcore",
               "configuration": {
                   "binding.path": "dotnetcore.dll",
                   "binding.coreclrpath": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\coreclr.dll",
                   "binding.trustedplatformassemblieslocation": "C:\\Program Files\\dotnet\\shared\\Microsoft.NETCore.App\\1.1.1\\"
               }
           }
       ],
          "modules": [
           {
               "name": "simulated_device",
               "loader": {
                   "name": "native",
                   "entrypoint": {
                       "module.path": "simulated_device.dll"
                   }
               },
               "args": {
                   "macAddress": "01:02:03:03:02:01",
                   "messagePeriod": 500
               }
           },
           {
               "name": "ble_converter_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "IoTEdgeConverterModule.BleConverterModule"
                   }
               },
               "args": ""
           },
           {
               "name": "printer_module",
               "loader": {
                   "name": "dotnetcore",
                   "entrypoint": {
                       "assembly.name": "IoTEdgeConverterModule",
                       "entry.type": "PrinterModule.DotNetPrinterModule"
                   }
               },
               "args": ""
           }
       ],
       "links": [
           {
               "source": "simulated_device", "sink": "ble_converter_module"
           },
           {
               "source": "ble_converter_module", "sink": "printer_module"
           }
   ]
   }
   ```

21. <span data-ttu-id="ae285-167">Spara filen som `gw-config.json` genom att trycka på `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ae285-167">Save the file as `gw-config.json` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="ae285-168">På den dialogrutan Spara som, i den `Save as Type` väljer du `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span><span class="sxs-lookup"><span data-stu-id="ae285-168">On the save as dialog box, in the `Save as Type` dropdown menu, select `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span></span>

22. <span data-ttu-id="ae285-169">Om du vill aktivera kopiering av konfigurationsfilen till den angivna katalogen, uppdatera det `IoTEdgeConverterModule.csproj` med följande XML-blob:</span><span class="sxs-lookup"><span data-stu-id="ae285-169">To enable copying of the configuration file to the output directory, update the `IoTEdgeConverterModule.csproj` with the following XML blob:</span></span>

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - <span data-ttu-id="ae285-170">Den uppdaterade `IoTEdgeConverterModule.csproj` bör se ut som följande bild:</span><span class="sxs-lookup"><span data-stu-id="ae285-170">The updated `IoTEdgeConverterModule.csproj` should look like the following image:</span></span>

    ![Visual Studio Code uppdaterade .csproj fil](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. <span data-ttu-id="ae285-172">Skapa en ny fil med namnet `Untitled-1` genom att trycka på `Ctrl`  +  `N` nycklar.</span><span class="sxs-lookup"><span data-stu-id="ae285-172">Create a new file called `Untitled-1` by pressing the `Ctrl` + `N` keys.</span></span>

24. <span data-ttu-id="ae285-173">Kopiera och klistra in följande kodavsnitt i den `Untitled-1` filen.</span><span class="sxs-lookup"><span data-stu-id="ae285-173">Copy and paste the following code snippet into the `Untitled-1` file.</span></span>

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. <span data-ttu-id="ae285-174">Spara filen som `binplace.ps1` genom att trycka på `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="ae285-174">Save the file as `binplace.ps1` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="ae285-175">På den dialogrutan Spara som, i den `Save as Type` väljer du `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span><span class="sxs-lookup"><span data-stu-id="ae285-175">On the save as dialog box, in the `Save as Type` dropdown menu, select `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span></span>

26. <span data-ttu-id="ae285-176">Skapa projektet genom att trycka på `Ctrl`  +  `Shift`  +  `B` nycklar.</span><span class="sxs-lookup"><span data-stu-id="ae285-176">Build the project by pressing the `Ctrl` + `Shift` + `B` keys.</span></span> <span data-ttu-id="ae285-177">När du skapar projektet för första gången `Visual Studio Code` uppmanar dig med de `No build task defined.` dialogrutan som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ae285-177">When you build the project for the first time, `Visual Studio Code` prompts you with the `No build task defined.` dialog as seen in the following image:</span></span>

    ![Visual Studio Code build-aktivitetsdialogrutan](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    <span data-ttu-id="ae285-179">en) Klicka på den `Configure Build Task` knappen.</span><span class="sxs-lookup"><span data-stu-id="ae285-179">a) Click the `Configure Build Task` button.</span></span>

    <span data-ttu-id="ae285-180">b) i den `Select a Task Runner` dialogrutan listrutan.</span><span class="sxs-lookup"><span data-stu-id="ae285-180">b) In the `Select a Task Runner` dialog dropdown menu.</span></span> <span data-ttu-id="ae285-181">Välj `.NET Core` som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ae285-181">Select `.NET Core` as seen in the following image:</span></span> 

    ![Visual Studio Code väljer en aktivitetsdialogrutan](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    <span data-ttu-id="ae285-183">c) Klicka på den `.NET Core` objekt skapar den `tasks.json` filen i din `.vscode` directory och öppnar filen i den `code editor` fönster.</span><span class="sxs-lookup"><span data-stu-id="ae285-183">c) Clicking the `.NET Core` item creates the `tasks.json` file in your `.vscode` directory and opens the file in the `code editor` window.</span></span> <span data-ttu-id="ae285-184">Det finns inget behov av att ändra den här filen, Stäng fliken.</span><span class="sxs-lookup"><span data-stu-id="ae285-184">There is no need to modify this file, close the tab.</span></span>

27.  <span data-ttu-id="ae285-185">Öppna den `Visual Studio Code` integrerad terminalfönster genom att trycka på `Ctrl`  +  `backtick` nycklar eller använda menyerna `View`  ->  `Integrated Terminal` och skriv **.\binplace.ps1** i den `PowerShell` kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="ae285-185">Open the `Visual Studio Code` integrated terminal window by pressing the `Ctrl` + `backtick` keys or using the menus `View` -> `Integrated Terminal` and type **.\binplace.ps1** into the `PowerShell` command prompt.</span></span> <span data-ttu-id="ae285-186">Det här kommandot kopierar alla våra beroenden till den angivna katalogen.</span><span class="sxs-lookup"><span data-stu-id="ae285-186">This command copies all our dependencies to the output directory.</span></span>

28. <span data-ttu-id="ae285-187">Navigera till utdatakatalogen projekt i den `Integrated Terminal` fönster genom att skriva **cd.\bin\Debug\netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="ae285-187">Navigate to the projects output directory in the `Integrated Terminal` window by typing **cd .\bin\Debug\netstandard1.3**.</span></span>

29. <span data-ttu-id="ae285-188">Köra exempelprojektet genom att skriva **. \gw.exe gw-config.json** till den `Integrated Terminal` fönstret Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="ae285-188">Run the sample project by typing **.\gw.exe gw-config.json** into the `Integrated Terminal` window prompt.</span></span> 
    - <span data-ttu-id="ae285-189">Om du har följt stegen i den här självstudiekursen noggrant kan du nu att köra den `Azure IoT Edge BLE Data Converter Module` exempelprojektet som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="ae285-189">If you have followed the steps in this tutorial closely, you should now be running the `Azure IoT Edge BLE Data Converter Module` sample project as seen in the following image:</span></span>
    
        ![Simulerade enheten exempel körs i Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - <span data-ttu-id="ae285-191">Om du vill avsluta programmet trycker du på den `<Enter>` nyckel.</span><span class="sxs-lookup"><span data-stu-id="ae285-191">If you want to terminate the application, press the `<Enter>` key.</span></span>

>[!IMPORTANT]
<span data-ttu-id="ae285-192">Det rekommenderas inte att använda `Ctrl`  +  `C` att avsluta den `IoT Edge` gateway-program (det vill säga **gw.exe**).</span><span class="sxs-lookup"><span data-stu-id="ae285-192">It is not recommended to use `Ctrl` + `C` to terminate the `IoT Edge` gateway application (that is, **gw.exe**).</span></span> <span data-ttu-id="ae285-193">Eftersom den här åtgärden kan orsaka processen att avbrytas onormalt.</span><span class="sxs-lookup"><span data-stu-id="ae285-193">As this action may cause the process to terminate abnormally.</span></span>

