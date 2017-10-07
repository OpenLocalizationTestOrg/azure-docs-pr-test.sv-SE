---
title: "aaaConnect en enhet med hjälp av C för Windows | Microsoft Docs"
description: "Beskriver hur tooconnect en enhet toohello Azure IoT Suite förkonfigurerade remote övervakningslösning som använder ett program som skrivits i C som körs på Windows."
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
ms.openlocfilehash: 51041e0cec113a5cfa006ab2276096baf928eef5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-windows"></a><span data-ttu-id="b8c20-103">Ansluta din enhet toohello remote förkonfigurerade övervakningslösning (Windows)</span><span class="sxs-lookup"><span data-stu-id="b8c20-103">Connect your device toohello remote monitoring preconfigured solution (Windows)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a><span data-ttu-id="b8c20-104">Skapa en C exempellösning i Windows</span><span class="sxs-lookup"><span data-stu-id="b8c20-104">Create a C sample solution on Windows</span></span>
<span data-ttu-id="b8c20-105">hello följande steg visar hur toocreate ett klientprogram som kommunicerar med hello fjärrövervaknings förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="b8c20-105">hello following steps show you how toocreate a client application that communicates with hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="b8c20-106">Det här programmet är skrivna i C och inbyggda och körs i Windows.</span><span class="sxs-lookup"><span data-stu-id="b8c20-106">This application is written in C and built and run on Windows.</span></span>

<span data-ttu-id="b8c20-107">Skapa ett starter-projekt i Visual Studio 2015 eller Visual Studio 2017 och Lägg till NuGet-paket för hello IoT-hubb enheten klienten:</span><span class="sxs-lookup"><span data-stu-id="b8c20-107">Create a starter project in Visual Studio 2015 or Visual Studio 2017 and add hello IoT Hub device client NuGet packages:</span></span>

1. <span data-ttu-id="b8c20-108">Skapa ett med hjälp av hello Visual C++ C-konsolprogram i Visual Studio **Win32-konsolprogram** mall.</span><span class="sxs-lookup"><span data-stu-id="b8c20-108">In Visual Studio, create a C console application using hello Visual C++ **Win32 Console Application** template.</span></span> <span data-ttu-id="b8c20-109">Namnet hello projektet **RMDevice**.</span><span class="sxs-lookup"><span data-stu-id="b8c20-109">Name hello project **RMDevice**.</span></span>
2. <span data-ttu-id="b8c20-110">På hello **programinställningar** sida i hello **Win32 programguiden**, se till att **konsolprogrammet** är markerad och avmarkera **Precompiled huvudet** och **Security Development Lifecycle (SDL) kontrollerar**.</span><span class="sxs-lookup"><span data-stu-id="b8c20-110">On hello **Application Settings** page in hello **Win32 Application Wizard**, ensure that **Console application** is selected, and uncheck **Precompiled header** and **Security Development Lifecycle (SDL) checks**.</span></span>
3. <span data-ttu-id="b8c20-111">I **Solution Explorer**, ta bort hello filer stdafx.h, targetver.h och stdafx.cpp.</span><span class="sxs-lookup"><span data-stu-id="b8c20-111">In **Solution Explorer**, delete hello files stdafx.h, targetver.h, and stdafx.cpp.</span></span>
4. <span data-ttu-id="b8c20-112">I **Solution Explorer**, byta namn på hello filen RMDevice.cpp tooRMDevice.c.</span><span class="sxs-lookup"><span data-stu-id="b8c20-112">In **Solution Explorer**, rename hello file RMDevice.cpp tooRMDevice.c.</span></span>
5. <span data-ttu-id="b8c20-113">I **Solution Explorer**, högerklicka på hello **RMDevice** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="b8c20-113">In **Solution Explorer**, right-click on hello **RMDevice** project and then click **Manage NuGet packages**.</span></span> <span data-ttu-id="b8c20-114">Klicka på **Bläddra**, och Sök efter och installera hello följande NuGet-paket:</span><span class="sxs-lookup"><span data-stu-id="b8c20-114">Click **Browse**, then search for and install hello following NuGet packages:</span></span>
   
   * <span data-ttu-id="b8c20-115">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="b8c20-115">Microsoft.Azure.IoTHub.Serializer</span></span>
   * <span data-ttu-id="b8c20-116">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="b8c20-116">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
   * <span data-ttu-id="b8c20-117">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="b8c20-117">Microsoft.Azure.IoTHub.MqttTransport</span></span>
6. <span data-ttu-id="b8c20-118">I **Solution Explorer**, högerklicka på hello **RMDevice** projektet och klicka sedan på **egenskaper** tooopen hello projektet **egenskapssidor**dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b8c20-118">In **Solution Explorer**, right-click on hello **RMDevice** project and then click **Properties** tooopen hello project's **Property Pages** dialog box.</span></span> <span data-ttu-id="b8c20-119">Mer information finns i [Visual C++-projekt inställningsegenskaper][lnk-c-project-properties].</span><span class="sxs-lookup"><span data-stu-id="b8c20-119">For details, see [Setting Visual C++ Project Properties][lnk-c-project-properties].</span></span> 
7. <span data-ttu-id="b8c20-120">Klicka på hello **Linker** mappen, klicka på hello **indata** egenskapssidan.</span><span class="sxs-lookup"><span data-stu-id="b8c20-120">Click hello **Linker** folder, then click hello **Input** property page.</span></span>
8. <span data-ttu-id="b8c20-121">Lägg till **crypt32.lib** toohello **Additional Dependencies** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b8c20-121">Add **crypt32.lib** toohello **Additional Dependencies** property.</span></span> <span data-ttu-id="b8c20-122">Klicka på **OK** och sedan **OK** egenskapsvärden toosave igen hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="b8c20-122">Click **OK** and then **OK** again toosave hello project property values.</span></span>

<span data-ttu-id="b8c20-123">Lägg till hello Parson JSON biblioteket toohello **RMDevice** projekt och Lägg till hello krävs `#include` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="b8c20-123">Add hello Parson JSON library toohello **RMDevice** project and add hello required `#include` statements:</span></span>

1. <span data-ttu-id="b8c20-124">Klona hello Parson GitHub-databas med hjälp av hello följande kommando i en lämplig mapp på datorn:</span><span class="sxs-lookup"><span data-stu-id="b8c20-124">In a suitable folder on your computer, clone hello Parson GitHub repository using hello following command:</span></span>

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. <span data-ttu-id="b8c20-125">Kopiera hello parson.h och parson.c från hello lokal kopia av hello Parson databasen tooyour **RMDevice** projektmappen.</span><span class="sxs-lookup"><span data-stu-id="b8c20-125">Copy hello parson.h and parson.c files from hello local copy of hello Parson repository tooyour **RMDevice** project folder.</span></span>

1. <span data-ttu-id="b8c20-126">I Visual Studio högerklickar du på hello **RMDevice** projektet, klicka på **Lägg till**, och klicka sedan på **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="b8c20-126">In Visual Studio, right-click hello **RMDevice** project, click **Add**, and then click **Existing Item**.</span></span>

1. <span data-ttu-id="b8c20-127">I hello **Lägg till befintligt objekt** dialogrutan, Välj hello parson.h och parson.c filer i hello **RMDevice** projektmappen.</span><span class="sxs-lookup"><span data-stu-id="b8c20-127">In hello **Add Existing Item** dialog, select hello parson.h and parson.c files in hello **RMDevice** project folder.</span></span> <span data-ttu-id="b8c20-128">Klicka på **Lägg till** tooadd dessa två filer tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="b8c20-128">Then click **Add** tooadd these two files tooyour project.</span></span>

1. <span data-ttu-id="b8c20-129">Öppna hello RMDevice.c filen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8c20-129">In Visual Studio, open hello RMDevice.c file.</span></span> <span data-ttu-id="b8c20-130">Ersätta befintliga hello `#include` instruktioner med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b8c20-130">Replace hello existing `#include` statements with hello following code:</span></span>
   
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
    > <span data-ttu-id="b8c20-131">Nu kan du kontrollera att projektet har hello rätt beroenden konfigurera genom att skapa den.</span><span class="sxs-lookup"><span data-stu-id="b8c20-131">Now you can verify that your project has hello correct dependencies set up by building it.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a><span data-ttu-id="b8c20-132">Skapa och köra hello-exempel</span><span class="sxs-lookup"><span data-stu-id="b8c20-132">Build and run hello sample</span></span>

<span data-ttu-id="b8c20-133">Lägg till kod tooinvoke hello **remote\_övervakning\_kör** fungera och sedan skapa och köra hello enhetsprogram.</span><span class="sxs-lookup"><span data-stu-id="b8c20-133">Add code tooinvoke hello **remote\_monitoring\_run** function and then build and run hello device application.</span></span>

1. <span data-ttu-id="b8c20-134">Ersätt hello **huvudsakliga** funktionen med följande kod tooinvoke hello **remote\_övervakning\_kör** funktionen:</span><span class="sxs-lookup"><span data-stu-id="b8c20-134">Replace hello **main** function with following code tooinvoke hello **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="b8c20-135">Klicka på **skapa** och sedan **skapa lösning** toobuild hello enhetsprogram.</span><span class="sxs-lookup"><span data-stu-id="b8c20-135">Click **Build** and then **Build Solution** toobuild hello device application.</span></span>

1. <span data-ttu-id="b8c20-136">I **Solution Explorer**, högerklicka på hello **RMDevice** projektet, klicka på **felsöka**, och klicka sedan på **Starta ny instans** toorun hello exempel.</span><span class="sxs-lookup"><span data-stu-id="b8c20-136">In **Solution Explorer**, right-click hello **RMDevice** project, click **Debug**, and then click **Start new instance** toorun hello sample.</span></span> <span data-ttu-id="b8c20-137">hello-konsolen visas meddelanden hello skickar exempel telemetri toohello förkonfigurerade lösningen, tar emot önskade egenskapsvärden i hello lösning instrumentpanelen och svarar toomethods anropas från hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b8c20-137">hello console displays messages as hello application sends sample telemetry toohello preconfigured solution, receives desired property values set in hello solution dashboard, and responds toomethods invoked from hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
