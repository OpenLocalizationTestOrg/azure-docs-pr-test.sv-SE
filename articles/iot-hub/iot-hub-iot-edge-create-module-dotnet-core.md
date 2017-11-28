---
title: aaaCreate en Azure IoT kant modul med C# | Microsoft Docs
description: "Den här kursen visar hur toowrite en TIVERA data konverteraren modulen med hjälp av hello senaste Azure IoT kant NuGet-paket, Visual Studio Code och C#."
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
ms.openlocfilehash: b104609c05d1613e21acc7d7bed547f311179151
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-iot-edge-module-with-cx23"></a><span data-ttu-id="139b4-104">Skapa en Azure IoT-Edge-modul med C & #x23;</span><span class="sxs-lookup"><span data-stu-id="139b4-104">Create an Azure IoT Edge Module with C&#x23;</span></span>

<span data-ttu-id="139b4-105">Den här kursen visar hur toocreate en modul för `Azure IoT Edge` med `Visual Studio Code` och `C#`.</span><span class="sxs-lookup"><span data-stu-id="139b4-105">This tutorial showcases how toocreate a module for `Azure IoT Edge` using `Visual Studio Code` and `C#`.</span></span>

<span data-ttu-id="139b4-106">I den här självstudiekursen kommer vi att gå igenom miljön ställa in och hur toowrite en [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data konverteraren modul med hello senaste `Azure IoT Edge NuGet` paket.</span><span class="sxs-lookup"><span data-stu-id="139b4-106">In this tutorial, we walk through environment set-up and how toowrite a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using hello latest `Azure IoT Edge NuGet` packages.</span></span> 

>[!NOTE]
<span data-ttu-id="139b4-107">Den här kursen använder hello `.NET Core SDK`, som har stöd för flera plattformar kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="139b4-107">This tutorial is using hello `.NET Core SDK`, which supports cross-platform compatibility.</span></span> <span data-ttu-id="139b4-108">hello följande självstudierna är skrivna med hjälp av hello `Windows 10` operativsystem.</span><span class="sxs-lookup"><span data-stu-id="139b4-108">hello following tutorial is written using hello `Windows 10` operating system.</span></span> <span data-ttu-id="139b4-109">Vissa hello-kommandon i den här kursen kan vara olika beroende på din `development environment`.</span><span class="sxs-lookup"><span data-stu-id="139b4-109">Some of hello commands in this tutorial may be different depending on your `development environment`.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="139b4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="139b4-110">Prerequisites</span></span>

<span data-ttu-id="139b4-111">I det här avsnittet vi ställa in miljön för `Azure IoT Edge` modulen utveckling.</span><span class="sxs-lookup"><span data-stu-id="139b4-111">In this section, we set-up your environment for `Azure IoT Edge` module development.</span></span> <span data-ttu-id="139b4-112">Det gäller tooboth **64-bitars Windows** och **64-bitars Linux (8 Ubuntu/Debian)** operativsystem.</span><span class="sxs-lookup"><span data-stu-id="139b4-112">It applies tooboth **64-bit Windows** and **64-bit Linux (Ubuntu/Debian 8)** operating systems.</span></span>

<span data-ttu-id="139b4-113">hello följande programvara krävs:</span><span class="sxs-lookup"><span data-stu-id="139b4-113">hello following software is required:</span></span>

- [<span data-ttu-id="139b4-114">Git-klient</span><span class="sxs-lookup"><span data-stu-id="139b4-114">Git Client</span></span>](https://git-scm.com/downloads)
- [<span data-ttu-id="139b4-115">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="139b4-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#windowscmd)
- [<span data-ttu-id="139b4-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="139b4-116">Visual Studio Code</span></span>](https://code.visualstudio.com/)

<span data-ttu-id="139b4-117">Du behöver inte tooclone hello lagringsplatsen för det här exemplet, men alla hello exempelkod beskrivs i den här kursen finns i följande databasen hello:</span><span class="sxs-lookup"><span data-stu-id="139b4-117">You do not need tooclone hello repo for this sample, however all of hello sample code discussed in this tutorial is located in hello following repository:</span></span>

- <span data-ttu-id="139b4-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="139b4-118">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
- `cd iot-edge-samples/dotnetcore/simulated_ble`

## <a name="getting-started"></a><span data-ttu-id="139b4-119">Komma igång</span><span class="sxs-lookup"><span data-stu-id="139b4-119">Getting started</span></span>

1. <span data-ttu-id="139b4-120">Installera `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="139b4-120">Install `.NET Core SDK`.</span></span>
2. <span data-ttu-id="139b4-121">Installera `Visual Studio Code` och hello `C# extension` från hello Visual Studio Code Marketplace.</span><span class="sxs-lookup"><span data-stu-id="139b4-121">Install `Visual Studio Code` and hello `C# extension` from hello Visual Studio Code Marketplace.</span></span>

<span data-ttu-id="139b4-122">Visa den här [snabb självstudiekurs](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) om hur tooget igång med `Visual Studio Code` och hello `.NET Core SDK`.</span><span class="sxs-lookup"><span data-stu-id="139b4-122">View this [quick video tutorial](https://channel9.msdn.com/Blogs/dotnet/Get-started-VSCode-Csharp-NET-Core-Windows) about how tooget started using `Visual Studio Code` and hello `.NET Core SDK`.</span></span>

## <a name="creating-hello-azure-iot-edge-converter-module"></a><span data-ttu-id="139b4-123">Skapa hello Azure IoT kant konverteraren modul</span><span class="sxs-lookup"><span data-stu-id="139b4-123">Creating hello Azure IoT Edge converter module</span></span>

1. <span data-ttu-id="139b4-124">Initiera en ny `.NET Core` C# klassbiblioteksprojektet:</span><span class="sxs-lookup"><span data-stu-id="139b4-124">Initialize a new `.NET Core` class library C# project:</span></span>
    - <span data-ttu-id="139b4-125">Öppna en kommandotolk (`Windows + R` -> `cmd` -> `enter`).</span><span class="sxs-lookup"><span data-stu-id="139b4-125">Open a command prompt (`Windows + R` -> `cmd` -> `enter`).</span></span>
    - <span data-ttu-id="139b4-126">Navigera toohello mappen där du vill ha toocreate hello `C#` projekt.</span><span class="sxs-lookup"><span data-stu-id="139b4-126">Navigate toohello folder where you'd like toocreate hello `C#` project.</span></span>
    - <span data-ttu-id="139b4-127">Typen **dotnet nya classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="139b4-127">Type **dotnet new classlib -o IoTEdgeConverterModule -f netstandard1.3**.</span></span> 
    - <span data-ttu-id="139b4-128">Det här kommandot skapar en tom klass som heter `Class1.cs` i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="139b4-128">This command creates an empty class called `Class1.cs` in your projects directory.</span></span>
2. <span data-ttu-id="139b4-129">Navigera toohello mapp där vi just har skapat hello klassbiblioteksprojektet genom att skriva **cd IoTEdgeConverterModule**.</span><span class="sxs-lookup"><span data-stu-id="139b4-129">Navigate toohello folder where we just created hello class library project by typing **cd IoTEdgeConverterModule**.</span></span>
3. <span data-ttu-id="139b4-130">Öppna hello-projekt i `Visual Studio Code` genom att skriva **kod.**.</span><span class="sxs-lookup"><span data-stu-id="139b4-130">Open hello project in `Visual Studio Code` by typing **code .**.</span></span>
4. <span data-ttu-id="139b4-131">När hello projektet öppnas i `Visual Studio Code`, klicka på hello **IoTEdgeConverterModule.csproj** tooopen hello filen enligt följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="139b4-131">Once hello project is opened in `Visual Studio Code`, click on hello **IoTEdgeConverterModule.csproj** tooopen hello file as shown in hello following image:</span></span>

    ![Visual Studio Code redigeringsfönstret](media/iot-hub-iot-edge-create-module/vscode-edit-csproj.png)

5. <span data-ttu-id="139b4-133">Infoga hello `XML` blob som visas i följande kodavsnitt mellan hello avslutande hello `PropertyGroup` tagga och hello stänger `Project` tagga; rad sex i föregående bild hello och spara hello-filen genom att trycka på `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="139b4-133">Insert hello `XML` blob shown in hello following code snippet between hello closing `PropertyGroup` tag and hello closing `Project` tag; line six in hello preceding image and save hello file by pressing `Ctrl` + `S`.</span></span>

   ```xml
     <ItemGroup>
       <PackageReference Include="Microsoft.Azure.Devices.Gateway.Module.NetStandard" Version="1.0.5" />
       <PackageReference Include="System.Threading.Thread" Version="4.3.0" />
       <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
     </ItemGroup> 
   ```

6. <span data-ttu-id="139b4-134">När du sparar hello `.csproj` filen `Visual Studio Code` fråga ska visas med en `unresolved dependencies` dialogrutan som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="139b4-134">Once you save hello `.csproj` file, `Visual Studio Code` should prompt you with an `unresolved dependencies` dialog as seen in hello following image:</span></span> 

    ![Visual Studio Code återställning beroenden dialogrutan](media/iot-hub-iot-edge-create-module/vscode-restore.png)

    <span data-ttu-id="139b4-136">en) Klicka på `Restore` toorestore alla hello refererar till i hello projekt `.csproj` inklusive hello `PackageReferences` vi har lagt till.</span><span class="sxs-lookup"><span data-stu-id="139b4-136">a) Click `Restore` toorestore all of hello references in hello projects `.csproj` file including hello `PackageReferences` we have added.</span></span> 

    <span data-ttu-id="139b4-137">b) `Visual Studio Code` skapas automatiskt hello `project.assets.json` filen i projektet `obj` mapp.</span><span class="sxs-lookup"><span data-stu-id="139b4-137">b) `Visual Studio Code` automatically creates hello `project.assets.json` file in your projects `obj` folder.</span></span> <span data-ttu-id="139b4-138">Den här filen innehåller information om ditt projekt beroenden toomake efterföljande återställningar snabbare.</span><span class="sxs-lookup"><span data-stu-id="139b4-138">This file contains information about your project's dependencies toomake subsequent restores quicker.</span></span>
 
    >[!NOTE]
    <span data-ttu-id="139b4-139">`.NET Core Tools`är nu MSBuild-baserade.</span><span class="sxs-lookup"><span data-stu-id="139b4-139">`.NET Core Tools` are now MSBuild-based.</span></span> <span data-ttu-id="139b4-140">Vilket innebär en `.csproj` projektfilen har skapats i stället för en `project.json`.</span><span class="sxs-lookup"><span data-stu-id="139b4-140">Which means a `.csproj` project file is created instead of a `project.json`.</span></span>

    - <span data-ttu-id="139b4-141">Om `Visual Studio Code` visas inte som är det ok, vi kan göra det manuellt.</span><span class="sxs-lookup"><span data-stu-id="139b4-141">If `Visual Studio Code` does not prompt you that is ok, we can do it manually.</span></span> <span data-ttu-id="139b4-142">Öppna hello `Visual Studio Code` integrerad terminalfönster genom att trycka på hello `Ctrl`  +  `backtick` nycklar eller använda hello menyer `View`  ->  `Integrated Terminal`.</span><span class="sxs-lookup"><span data-stu-id="139b4-142">Open hello `Visual Studio Code` integrated terminal window by pressing hello `Ctrl` + `backtick` keys or using hello menus `View` -> `Integrated Terminal`.</span></span>
    - <span data-ttu-id="139b4-143">I hello `Integrated Terminal` fönstret typen **dotnet återställning**.</span><span class="sxs-lookup"><span data-stu-id="139b4-143">In hello `Integrated Terminal` window type **dotnet restore**.</span></span>
    
7. <span data-ttu-id="139b4-144">Byt namn på hello `Class1.cs` filen för`BleConverterModule.cs`.</span><span class="sxs-lookup"><span data-stu-id="139b4-144">Rename hello `Class1.cs` file too`BleConverterModule.cs`.</span></span> 

    <span data-ttu-id="139b4-145">en) toorename hello filen först klicka på hello-filen och tryck sedan på hello `F2` nyckel.</span><span class="sxs-lookup"><span data-stu-id="139b4-145">a) toorename hello file first click on hello file then press hello `F2` key.</span></span>
    
    <span data-ttu-id="139b4-146">b) Skriv hello nytt namn **BleConverterModule**, som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="139b4-146">b) Type in hello new name **BleConverterModule**, as seen in hello following image:</span></span>

    ![Visual Studio Code byta namn på en klass](media/iot-hub-iot-edge-create-module/vscode-rename.png)

8. <span data-ttu-id="139b4-148">Ersätt hello befintlig kod i hello `BleConverterModule.cs` filen genom att kopiera och klistra in hello följande kodstycke i din `BleConverterModule.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="139b4-148">Replace hello existing code in hello `BleConverterModule.cs` file by copying and pasting hello following code snippet into your `BleConverterModule.cs` file.</span></span>

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

9. <span data-ttu-id="139b4-149">Spara hello-filen genom att trycka på `Ctrl`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="139b4-149">Save hello file by pressing `Ctrl` + `S`.</span></span>

10. <span data-ttu-id="139b4-150">Skapa en ny fil med namnet `Untitled-1` genom att trycka på hello `Ctrl`  +  `N` nycklar som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="139b4-150">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys as seen in hello following image:</span></span>

    ![Visual Studio Code ny fil](media/iot-hub-iot-edge-create-module/vscode-new-file.png)

11. <span data-ttu-id="139b4-152">toodeserialize hello `JSON` objekt som vi får från hello simulerade `BLE` enhet, kopiera hello följande kod i hello `Untitled-1` filen kod editor-fönstret.</span><span class="sxs-lookup"><span data-stu-id="139b4-152">toodeserialize hello `JSON` object that we receive from hello simulated `BLE` device, copy hello following code into hello `Untitled-1` file code editor window.</span></span> 

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

12. <span data-ttu-id="139b4-153">Spara hello som `BleData.cs` genom att trycka på `Ctrl`  +  `Shift`  +  `S` nycklar.</span><span class="sxs-lookup"><span data-stu-id="139b4-153">Save hello file as `BleData.cs` by pressing `Ctrl` + `Shift` + `S` keys.</span></span>
    - <span data-ttu-id="139b4-154">Spara som i dialogrutan hello på hello `Save as Type` väljer du `C# (*.cs;*.csx)` som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="139b4-154">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `C# (*.cs;*.csx)` as seen in hello following image:</span></span>

    ![Visual Studio Code Spara som dialog](media/iot-hub-iot-edge-create-module/vscode-save-as.png)

13. <span data-ttu-id="139b4-156">Skapa en ny fil med namnet `Untitled-1` genom att trycka på hello `Ctrl`  +  `N` nycklar.</span><span class="sxs-lookup"><span data-stu-id="139b4-156">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

14. <span data-ttu-id="139b4-157">Kopiera och klistra in följande kodfragment i hello hello `Untitled-1` fil.</span><span class="sxs-lookup"><span data-stu-id="139b4-157">Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span> <span data-ttu-id="139b4-158">Den här klassen är en `Azure IoT Edge` module, som vi använder toooutput hello data från våra `BleConverterModule`.</span><span class="sxs-lookup"><span data-stu-id="139b4-158">This class is a `Azure IoT Edge` module, which we use toooutput hello data received from our `BleConverterModule`.</span></span>

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

15. <span data-ttu-id="139b4-159">Spara hello som `DotNetPrinterModule.cs` genom att trycka på `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="139b4-159">Save hello file as `DotNetPrinterModule.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="139b4-160">Spara som i dialogrutan hello på hello `Save as Type` väljer du `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="139b4-160">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

16. <span data-ttu-id="139b4-161">Skapa en ny fil med namnet `Untitled-1` genom att trycka på hello `Ctrl`  +  `N` nycklar.</span><span class="sxs-lookup"><span data-stu-id="139b4-161">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

17. <span data-ttu-id="139b4-162">toodeserialize hello `JSON` objekt som vi får från hello `BleConverterModule`, kopiera och klistra in hello följande kodavsnitt i hello `Untitled-1` fil.</span><span class="sxs-lookup"><span data-stu-id="139b4-162">toodeserialize hello `JSON` object that we receive from hello `BleConverterModule`, Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span> 

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

18. <span data-ttu-id="139b4-163">Spara hello som `BleConverterData.cs` genom att trycka på `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="139b4-163">Save hello file as `BleConverterData.cs` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="139b4-164">Spara som i dialogrutan hello på hello `Save as Type` väljer du `C# (*.cs;*.csx)`.</span><span class="sxs-lookup"><span data-stu-id="139b4-164">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `C# (*.cs;*.csx)`.</span></span>

19. <span data-ttu-id="139b4-165">Skapa en ny fil med namnet `Untitled-1` genom att trycka på hello `Ctrl`  +  `N` nycklar.</span><span class="sxs-lookup"><span data-stu-id="139b4-165">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

20. <span data-ttu-id="139b4-166">Kopiera och klistra in följande kodfragment i hello hello `Untitled-1` fil.</span><span class="sxs-lookup"><span data-stu-id="139b4-166">Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span>

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

21. <span data-ttu-id="139b4-167">Spara hello som `gw-config.json` genom att trycka på `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="139b4-167">Save hello file as `gw-config.json` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="139b4-168">Spara som i dialogrutan hello på hello `Save as Type` väljer du `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span><span class="sxs-lookup"><span data-stu-id="139b4-168">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `JSON (*.json;*.bowerrc;*.jshintrc;*.jscsrc;*.eslintrc;*.babelrc;*webmanifest)`.</span></span>

22. <span data-ttu-id="139b4-169">tooenable kopiering av hello configuration file toohello utdata directory update hello `IoTEdgeConverterModule.csproj` med hello följande XML-blob:</span><span class="sxs-lookup"><span data-stu-id="139b4-169">tooenable copying of hello configuration file toohello output directory, update hello `IoTEdgeConverterModule.csproj` with hello following XML blob:</span></span>

   ```xml
     <ItemGroup>
       <None Update="gw-config.json" CopyToOutputDirectory="PreserveNewest" />
     </ItemGroup>
   ```
    
   - <span data-ttu-id="139b4-170">hello uppdateras `IoTEdgeConverterModule.csproj` ser ut som hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="139b4-170">hello updated `IoTEdgeConverterModule.csproj` should look like hello following image:</span></span>

    ![Visual Studio Code uppdaterade .csproj fil](media/iot-hub-iot-edge-create-module/vscode-update-csproj.png)

23. <span data-ttu-id="139b4-172">Skapa en ny fil med namnet `Untitled-1` genom att trycka på hello `Ctrl`  +  `N` nycklar.</span><span class="sxs-lookup"><span data-stu-id="139b4-172">Create a new file called `Untitled-1` by pressing hello `Ctrl` + `N` keys.</span></span>

24. <span data-ttu-id="139b4-173">Kopiera och klistra in följande kodfragment i hello hello `Untitled-1` fil.</span><span class="sxs-lookup"><span data-stu-id="139b4-173">Copy and paste hello following code snippet into hello `Untitled-1` file.</span></span>

   ```powershell
   Copy-Item -Path $env:userprofile\.nuget\packages\microsoft.azure.devices.gateway.native.windows.x64\1.1.3\runtimes\win-x64\native\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.formatters\4.3.0\lib\netstandard1.4\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.runtime.serialization.primitives\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\newtonsoft.json\10.0.2\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.componentmodel.typeconverter\4.3.0\lib\netstandard1.5\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.nongeneric\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   Copy-Item -Path $env:userprofile\.nuget\packages\system.collections.specialized\4.3.0\lib\netstandard1.3\* -Destination .\bin\Debug\netstandard1.3
   ```

25. <span data-ttu-id="139b4-174">Spara hello som `binplace.ps1` genom att trycka på `Ctrl`  +  `Shift`  +  `S`.</span><span class="sxs-lookup"><span data-stu-id="139b4-174">Save hello file as `binplace.ps1` by pressing `Ctrl` + `Shift` + `S`.</span></span>
    - <span data-ttu-id="139b4-175">Spara som i dialogrutan hello på hello `Save as Type` väljer du `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span><span class="sxs-lookup"><span data-stu-id="139b4-175">On hello save as dialog box, in hello `Save as Type` dropdown menu, select `PowerShell (*.ps1;*.psm1;*.psd1;*.pssc;*.psrc)`.</span></span>

26. <span data-ttu-id="139b4-176">Skapa hello projektet genom att trycka på hello `Ctrl`  +  `Shift`  +  `B` nycklar.</span><span class="sxs-lookup"><span data-stu-id="139b4-176">Build hello project by pressing hello `Ctrl` + `Shift` + `B` keys.</span></span> <span data-ttu-id="139b4-177">När du skapar hello-projekt för hello första gången `Visual Studio Code` efterfrågar hello `No build task defined.` dialogrutan som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="139b4-177">When you build hello project for hello first time, `Visual Studio Code` prompts you with hello `No build task defined.` dialog as seen in hello following image:</span></span>

    ![Visual Studio Code build-aktivitetsdialogrutan](media/iot-hub-iot-edge-create-module/vscode-build-task.png)

    <span data-ttu-id="139b4-179">en) klickar du på hello `Configure Build Task` knappen.</span><span class="sxs-lookup"><span data-stu-id="139b4-179">a) Click hello `Configure Build Task` button.</span></span>

    <span data-ttu-id="139b4-180">b) i hello `Select a Task Runner` dialogrutan listrutan.</span><span class="sxs-lookup"><span data-stu-id="139b4-180">b) In hello `Select a Task Runner` dialog dropdown menu.</span></span> <span data-ttu-id="139b4-181">Välj `.NET Core` som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="139b4-181">Select `.NET Core` as seen in hello following image:</span></span> 

    ![Visual Studio Code väljer en aktivitetsdialogrutan](media/iot-hub-iot-edge-create-module/vscode-build-task-runner.png)

    <span data-ttu-id="139b4-183">c) Klicka hello `.NET Core` objekt skapar hello `tasks.json` filen i din `.vscode` katalog och öppnas hello filen i hello `code editor` fönster.</span><span class="sxs-lookup"><span data-stu-id="139b4-183">c) Clicking hello `.NET Core` item creates hello `tasks.json` file in your `.vscode` directory and opens hello file in hello `code editor` window.</span></span> <span data-ttu-id="139b4-184">Det finns inget behov av toomodify den här filen, Stäng hello-fliken.</span><span class="sxs-lookup"><span data-stu-id="139b4-184">There is no need toomodify this file, close hello tab.</span></span>

27.  <span data-ttu-id="139b4-185">Öppna hello `Visual Studio Code` integrerad terminalfönster genom att trycka på hello `Ctrl`  +  `backtick` nycklar eller använda hello menyer `View`  ->  `Integrated Terminal` och skriv **.\binplace.ps1**i hello `PowerShell` kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="139b4-185">Open hello `Visual Studio Code` integrated terminal window by pressing hello `Ctrl` + `backtick` keys or using hello menus `View` -> `Integrated Terminal` and type **.\binplace.ps1** into hello `PowerShell` command prompt.</span></span> <span data-ttu-id="139b4-186">Det här kommandot kopieras alla våra beroenden toohello målkatalogen.</span><span class="sxs-lookup"><span data-stu-id="139b4-186">This command copies all our dependencies toohello output directory.</span></span>

28. <span data-ttu-id="139b4-187">Navigera toohello projekt målkatalogen i hello `Integrated Terminal` fönster genom att skriva **cd.\bin\Debug\netstandard1.3**.</span><span class="sxs-lookup"><span data-stu-id="139b4-187">Navigate toohello projects output directory in hello `Integrated Terminal` window by typing **cd .\bin\Debug\netstandard1.3**.</span></span>

29. <span data-ttu-id="139b4-188">Kör hello exempelprojektet genom att skriva **. \gw.exe gw-config.json** till hello `Integrated Terminal` fönstret Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="139b4-188">Run hello sample project by typing **.\gw.exe gw-config.json** into hello `Integrated Terminal` window prompt.</span></span> 
    - <span data-ttu-id="139b4-189">Om du har följt hello stegen i den här självstudiekursen noggrant kan du bör nu köra hello `Azure IoT Edge BLE Data Converter Module` exempelprojektet som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="139b4-189">If you have followed hello steps in this tutorial closely, you should now be running hello `Azure IoT Edge BLE Data Converter Module` sample project as seen in hello following image:</span></span>
    
        ![Simulerade enheten exempel körs i Visual Studio Code](media/iot-hub-iot-edge-create-module/vscode-run.png)
    
    - <span data-ttu-id="139b4-191">Om du vill tooterminate hello program, trycker du på hello `<Enter>` nyckel.</span><span class="sxs-lookup"><span data-stu-id="139b4-191">If you want tooterminate hello application, press hello `<Enter>` key.</span></span>

>[!IMPORTANT]
<span data-ttu-id="139b4-192">Det rekommenderas inte toouse `Ctrl`  +  `C` tooterminate hello `IoT Edge` gateway programmet (det vill säga **gw.exe**).</span><span class="sxs-lookup"><span data-stu-id="139b4-192">It is not recommended toouse `Ctrl` + `C` tooterminate hello `IoT Edge` gateway application (that is, **gw.exe**).</span></span> <span data-ttu-id="139b4-193">Eftersom den här åtgärden kan orsaka hello processen tooterminate onormalt.</span><span class="sxs-lookup"><span data-stu-id="139b4-193">As this action may cause hello process tooterminate abnormally.</span></span>

