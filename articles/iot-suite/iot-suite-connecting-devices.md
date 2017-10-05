---
title: "Ansluter en enhet med hjälp av C för Windows | Microsoft Docs"
description: "Beskriver hur du ansluter en enhet till Azure IoT Suite förkonfigurerade fjärråtkomst övervakning lösningen med hjälp av ett program som skrivits i C som körs på Windows."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: d222bcbd64f288d4091acb0ecd2922b9ceee57e5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a><span data-ttu-id="87ac1-103">Ansluta enheten till den fjärranslutna förkonfigurerade övervakningslösning (Windows)</span><span class="sxs-lookup"><span data-stu-id="87ac1-103">Connect your device to the remote monitoring preconfigured solution (Windows)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a><span data-ttu-id="87ac1-104">Skapa en C exempellösning i Windows</span><span class="sxs-lookup"><span data-stu-id="87ac1-104">Create a C sample solution on Windows</span></span>
<span data-ttu-id="87ac1-105">Följande steg visar hur du skapar ett klientprogram som kommunicerar med fjärråtkomst övervakning förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="87ac1-105">The following steps show you how to create a client application that communicates with the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="87ac1-106">Det här programmet är skrivna i C och inbyggda och körs i Windows.</span><span class="sxs-lookup"><span data-stu-id="87ac1-106">This application is written in C and built and run on Windows.</span></span>

<span data-ttu-id="87ac1-107">Skapa ett starter-projekt i Visual Studio 2015 eller Visual Studio 2017 och Lägg till NuGet-paket IoT-hubb enheten klienten:</span><span class="sxs-lookup"><span data-stu-id="87ac1-107">Create a starter project in Visual Studio 2015 or Visual Studio 2017 and add the IoT Hub device client NuGet packages:</span></span>

1. <span data-ttu-id="87ac1-108">Skapa ett med hjälp av Visual C++ C-konsolprogram i Visual Studio **Win32-konsolprogram** mall.</span><span class="sxs-lookup"><span data-stu-id="87ac1-108">In Visual Studio, create a C console application using the Visual C++ **Win32 Console Application** template.</span></span> <span data-ttu-id="87ac1-109">Namnge projektet **RMDevice**.</span><span class="sxs-lookup"><span data-stu-id="87ac1-109">Name the project **RMDevice**.</span></span>
2. <span data-ttu-id="87ac1-110">På den **programinställningar** sidan i den **Win32 programguiden**, se till att **konsolprogrammet** är markerad och avmarkera **Precompiled huvudet** och **Security Development Lifecycle (SDL) kontrollerar**.</span><span class="sxs-lookup"><span data-stu-id="87ac1-110">On the **Application Settings** page in the **Win32 Application Wizard**, ensure that **Console application** is selected, and uncheck **Precompiled header** and **Security Development Lifecycle (SDL) checks**.</span></span>
3. <span data-ttu-id="87ac1-111">I **Solution Explorer**, ta bort filer stdafx.h, targetver.h och stdafx.cpp.</span><span class="sxs-lookup"><span data-stu-id="87ac1-111">In **Solution Explorer**, delete the files stdafx.h, targetver.h, and stdafx.cpp.</span></span>
4. <span data-ttu-id="87ac1-112">I **Solution Explorer**, Byt namn på filen RMDevice.cpp till RMDevice.c.</span><span class="sxs-lookup"><span data-stu-id="87ac1-112">In **Solution Explorer**, rename the file RMDevice.cpp to RMDevice.c.</span></span>
5. <span data-ttu-id="87ac1-113">I **Solution Explorer**, högerklicka på den **RMDevice** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="87ac1-113">In **Solution Explorer**, right-click on the **RMDevice** project and then click **Manage NuGet packages**.</span></span> <span data-ttu-id="87ac1-114">Klicka på **Bläddra**, och Sök efter och installera följande NuGet-paket:</span><span class="sxs-lookup"><span data-stu-id="87ac1-114">Click **Browse**, then search for and install the following NuGet packages:</span></span>
   
   * <span data-ttu-id="87ac1-115">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="87ac1-115">Microsoft.Azure.IoTHub.Serializer</span></span>
   * <span data-ttu-id="87ac1-116">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="87ac1-116">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
   * <span data-ttu-id="87ac1-117">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="87ac1-117">Microsoft.Azure.IoTHub.MqttTransport</span></span>
6. <span data-ttu-id="87ac1-118">I **Solution Explorer**, högerklicka på den **RMDevice** projektet och klicka sedan på **egenskaper** att öppna projektets **egenskapssidor** i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="87ac1-118">In **Solution Explorer**, right-click on the **RMDevice** project and then click **Properties** to open the project's **Property Pages** dialog box.</span></span> <span data-ttu-id="87ac1-119">Mer information finns i [Visual C++-projekt inställningsegenskaper][lnk-c-project-properties].</span><span class="sxs-lookup"><span data-stu-id="87ac1-119">For details, see [Setting Visual C++ Project Properties][lnk-c-project-properties].</span></span> 
7. <span data-ttu-id="87ac1-120">Klicka på den **Linker** mappen, klicka sedan på den **indata** egenskapssidan.</span><span class="sxs-lookup"><span data-stu-id="87ac1-120">Click the **Linker** folder, then click the **Input** property page.</span></span>
8. <span data-ttu-id="87ac1-121">Lägg till **crypt32.lib** till den **Additional Dependencies** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="87ac1-121">Add **crypt32.lib** to the **Additional Dependencies** property.</span></span> <span data-ttu-id="87ac1-122">Klicka på **OK** och sedan **OK** igen för att spara projektet egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="87ac1-122">Click **OK** and then **OK** again to save the project property values.</span></span>

<span data-ttu-id="87ac1-123">Lägg till Parson JSON-biblioteket till det **RMDevice** projekt och Lägg till de nödvändiga `#include` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="87ac1-123">Add the Parson JSON library to the **RMDevice** project and add the required `#include` statements:</span></span>

1. <span data-ttu-id="87ac1-124">Klona Parson GitHub-lagret med hjälp av följande kommando i en lämplig mapp på datorn:</span><span class="sxs-lookup"><span data-stu-id="87ac1-124">In a suitable folder on your computer, clone the Parson GitHub repository using the following command:</span></span>

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. <span data-ttu-id="87ac1-125">Kopiera filerna parson.h och parson.c från den lokala kopian av databasen Parson till din **RMDevice** projektmappen.</span><span class="sxs-lookup"><span data-stu-id="87ac1-125">Copy the parson.h and parson.c files from the local copy of the Parson repository to your **RMDevice** project folder.</span></span>

1. <span data-ttu-id="87ac1-126">I Visual Studio högerklickar du på den **RMDevice** projektet, klicka på **Lägg till**, och klicka sedan på **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="87ac1-126">In Visual Studio, right-click the **RMDevice** project, click **Add**, and then click **Existing Item**.</span></span>

1. <span data-ttu-id="87ac1-127">I den **Lägg till befintligt objekt** dialogrutan Välj parson.h och parson.c filer i den **RMDevice** projektmappen.</span><span class="sxs-lookup"><span data-stu-id="87ac1-127">In the **Add Existing Item** dialog, select the parson.h and parson.c files in the **RMDevice** project folder.</span></span> <span data-ttu-id="87ac1-128">Klicka på **Lägg till** att lägga till de här två filerna i projektet.</span><span class="sxs-lookup"><span data-stu-id="87ac1-128">Then click **Add** to add these two files to your project.</span></span>

1. <span data-ttu-id="87ac1-129">Öppna filen RMDevice.c i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="87ac1-129">In Visual Studio, open the RMDevice.c file.</span></span> <span data-ttu-id="87ac1-130">Ersätta den befintliga `#include` instruktioner med följande kod:</span><span class="sxs-lookup"><span data-stu-id="87ac1-130">Replace the existing `#include` statements with the following code:</span></span>
   
    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

    > [!NOTE]
    > <span data-ttu-id="87ac1-131">Nu kan du kontrollera att projektet har rätt beroenden konfigurera genom att skapa den.</span><span class="sxs-lookup"><span data-stu-id="87ac1-131">Now you can verify that your project has the correct dependencies set up by building it.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a><span data-ttu-id="87ac1-132">Skapa och köra exemplet</span><span class="sxs-lookup"><span data-stu-id="87ac1-132">Build and run the sample</span></span>

<span data-ttu-id="87ac1-133">Lägg till kod för att anropa den **remote\_övervakning\_kör** fungera och sedan skapa och köra programmet för enheten.</span><span class="sxs-lookup"><span data-stu-id="87ac1-133">Add code to invoke the **remote\_monitoring\_run** function and then build and run the device application.</span></span>

1. <span data-ttu-id="87ac1-134">Ersätt den **huvudsakliga** funktionen med följande kod för att anropa den **remote\_övervakning\_kör** funktionen:</span><span class="sxs-lookup"><span data-stu-id="87ac1-134">Replace the **main** function with following code to invoke the **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="87ac1-135">Klicka på **skapa** och sedan **skapa lösning** att bygga enhetsprogram.</span><span class="sxs-lookup"><span data-stu-id="87ac1-135">Click **Build** and then **Build Solution** to build the device application.</span></span>

1. <span data-ttu-id="87ac1-136">I **Solution Explorer**, högerklicka på den **RMDevice** projektet, klicka på **felsöka**, och klicka sedan på **Starta ny instans** att köra exemplet.</span><span class="sxs-lookup"><span data-stu-id="87ac1-136">In **Solution Explorer**, right-click the **RMDevice** project, click **Debug**, and then click **Start new instance** to run the sample.</span></span> <span data-ttu-id="87ac1-137">Konsolen visar meddelanden som programmet skickar exempel telemetri till den förkonfigurerade lösningen, tar emot önskade egenskapsvärden i lösningen instrumentpanelen och svarar på metoderna som anropas från instrumentpanelen lösning.</span><span class="sxs-lookup"><span data-stu-id="87ac1-137">The console displays messages as the application sends sample telemetry to the preconfigured solution, receives desired property values set in the solution dashboard, and responds to methods invoked from the solution dashboard.</span></span>

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
