---
title: "aaaConnect en enhet med hjälp av C på Linux | Microsoft Docs"
description: "Beskriver hur tooconnect en enhet toohello Azure IoT Suite förkonfigurerade remote övervakningslösning som använder ett program som skrivits i C som körs på Linux."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0c7c8039-0bbf-4bb5-9e79-ed8cff433629
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 57393817d40d3555177956a01fa71058bc256988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-linux"></a><span data-ttu-id="54f0e-103">Ansluta din enhet toohello remote förkonfigurerade övervakningslösning (Linux)</span><span class="sxs-lookup"><span data-stu-id="54f0e-103">Connect your device toohello remote monitoring preconfigured solution (Linux)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a><span data-ttu-id="54f0e-104">Skapa och kör ett exempel på C-klienten Linux</span><span class="sxs-lookup"><span data-stu-id="54f0e-104">Build and run a sample C client Linux</span></span>
<span data-ttu-id="54f0e-105">hello följande steg visar hur toocreate ett klientprogram som kommunicerar med hello fjärrövervaknings förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="54f0e-105">hello following steps show you how toocreate a client application that communicates with hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="54f0e-106">Det här programmet har skrivits i C och bygger och kör på Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="54f0e-106">This application is written in C and built and run on Ubuntu Linux.</span></span>

<span data-ttu-id="54f0e-107">toocomplete som stegen nedan måste en enhet som kör Ubuntu version 15.04 eller 15.10.</span><span class="sxs-lookup"><span data-stu-id="54f0e-107">toocomplete these steps, you need a device running Ubuntu version 15.04 or 15.10.</span></span> <span data-ttu-id="54f0e-108">Innan du fortsätter att installera hello nödvändiga paket på enheten Ubuntu med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="54f0e-108">Before proceeding, install hello prerequisite packages on your Ubuntu device using hello following command:</span></span>

```
sudo apt-get install cmake gcc g++
```

## <a name="install-hello-client-libraries-on-your-device"></a><span data-ttu-id="54f0e-109">Installera hello klientbibliotek på din enhet</span><span class="sxs-lookup"><span data-stu-id="54f0e-109">Install hello client libraries on your device</span></span>
<span data-ttu-id="54f0e-110">hello Azure IoT Hub-klientbibliotek är tillgänglig som ett paket som du kan installera på enheten Ubuntu med hello **lgh get** kommando.</span><span class="sxs-lookup"><span data-stu-id="54f0e-110">hello Azure IoT Hub client libraries are available as a package you can install on your Ubuntu device using hello **apt-get** command.</span></span> <span data-ttu-id="54f0e-111">Utför följande steg tooinstall hello paket som innehåller hello klientbibliotek för IoT-hubb och rubrikfiler på datorn Ubuntu hello:</span><span class="sxs-lookup"><span data-stu-id="54f0e-111">Complete hello following steps tooinstall hello package that contains hello IoT Hub client library and header files on your Ubuntu computer:</span></span>

1. <span data-ttu-id="54f0e-112">Lägg till hello AzureIoT databasen tooyour datorn i ett gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="54f0e-112">In a shell, add hello AzureIoT repository tooyour computer:</span></span>
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. <span data-ttu-id="54f0e-113">Installationspaket för hello azure iot-sdk-c-utveckling</span><span class="sxs-lookup"><span data-stu-id="54f0e-113">Install hello azure-iot-sdk-c-dev package</span></span>
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-hello-parson-json-parser"></a><span data-ttu-id="54f0e-114">Installera hello Parson JSON-parsern</span><span class="sxs-lookup"><span data-stu-id="54f0e-114">Install hello Parson JSON parser</span></span>
<span data-ttu-id="54f0e-115">Hej IoT-hubb klienten bibliotek använder hello Parson JSON-parsern tooparse meddelande nyttolaster.</span><span class="sxs-lookup"><span data-stu-id="54f0e-115">hello IoT Hub client libraries use hello Parson JSON parser tooparse message payloads.</span></span> <span data-ttu-id="54f0e-116">Klona hello Parson GitHub-databas med hjälp av hello följande kommando i en lämplig mapp på datorn:</span><span class="sxs-lookup"><span data-stu-id="54f0e-116">In a suitable folder on your computer, clone hello Parson GitHub repository using hello following command:</span></span>

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a><span data-ttu-id="54f0e-117">Förbered ditt projekt</span><span class="sxs-lookup"><span data-stu-id="54f0e-117">Prepare your project</span></span>
<span data-ttu-id="54f0e-118">Skapa en mapp med namnet på datorn Ubuntu **remote\_övervakning**.</span><span class="sxs-lookup"><span data-stu-id="54f0e-118">On your Ubuntu machine, create a folder called **remote\_monitoring**.</span></span> <span data-ttu-id="54f0e-119">I hello **remote\_övervakning** mapp:</span><span class="sxs-lookup"><span data-stu-id="54f0e-119">In hello **remote\_monitoring** folder:</span></span>

- <span data-ttu-id="54f0e-120">Skapa hello fyra filer **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h**, och **CMakeLists.txt**.</span><span class="sxs-lookup"><span data-stu-id="54f0e-120">Create hello four files **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h**, and **CMakeLists.txt**.</span></span>
- <span data-ttu-id="54f0e-121">Skapa mapp med namnet **parson**.</span><span class="sxs-lookup"><span data-stu-id="54f0e-121">Create folder called **parson**.</span></span>

<span data-ttu-id="54f0e-122">Kopiera filerna hello **parson.c** och **parson.h** från den lokala kopian av hello Parson databasen till hello **remote\_övervakning/parson** mapp.</span><span class="sxs-lookup"><span data-stu-id="54f0e-122">Copy hello files **parson.c** and **parson.h** from your local copy of hello Parson repository into hello **remote\_monitoring/parson** folder.</span></span>

<span data-ttu-id="54f0e-123">Öppna hello i en textredigerare, **remote\_monitoring.c** fil.</span><span class="sxs-lookup"><span data-stu-id="54f0e-123">In a text editor, open hello **remote\_monitoring.c** file.</span></span> <span data-ttu-id="54f0e-124">Lägg till följande hello `#include` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="54f0e-124">Add hello following `#include` statements:</span></span>
   
```
#include "iothubtransportmqtt.h"
#include "schemalib.h"
#include "iothub_client.h"
#include "serializer_devicetwin.h"
#include "schemaserializer.h"
#include "azure_c_shared_utility/threadapi.h"
#include "azure_c_shared_utility/platform.h"
#include "parson.h"
```

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="call-hello-remotemonitoringrun-function"></a><span data-ttu-id="54f0e-125">Anropa hello remote\_övervakning\_köra funktionen</span><span class="sxs-lookup"><span data-stu-id="54f0e-125">Call hello remote\_monitoring\_run function</span></span>
<span data-ttu-id="54f0e-126">Öppna hello i en textredigerare, **remote_monitoring.h** fil.</span><span class="sxs-lookup"><span data-stu-id="54f0e-126">In a text editor, open hello **remote_monitoring.h** file.</span></span> <span data-ttu-id="54f0e-127">Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="54f0e-127">Add hello following code:</span></span>

```
void remote_monitoring_run(void);
```

<span data-ttu-id="54f0e-128">Öppna hello i en textredigerare, **main.c** fil.</span><span class="sxs-lookup"><span data-stu-id="54f0e-128">In a text editor, open hello **main.c** file.</span></span> <span data-ttu-id="54f0e-129">Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="54f0e-129">Add hello following code:</span></span>

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-hello-application"></a><span data-ttu-id="54f0e-130">Skapa och köra programmet hello</span><span class="sxs-lookup"><span data-stu-id="54f0e-130">Build and run hello application</span></span>
<span data-ttu-id="54f0e-131">hello följande steg beskriver hur toouse *CMake* toobuild klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="54f0e-131">hello following steps describe how toouse *CMake* toobuild your client application.</span></span>

1. <span data-ttu-id="54f0e-132">Öppna hello i en textredigerare, **CMakeLists.txt** filen i hello **remote_monitoring** mapp.</span><span class="sxs-lookup"><span data-stu-id="54f0e-132">In a text editor, open hello **CMakeLists.txt** file in hello **remote_monitoring** folder.</span></span>

1. <span data-ttu-id="54f0e-133">Lägg till följande instruktioner toodefine hur hello toobuild klientprogrammet:</span><span class="sxs-lookup"><span data-stu-id="54f0e-133">Add hello following instructions toodefine how toobuild your client application:</span></span>
   
    ```
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/parson" "/usr/include/azureiot" "/usr/include/azureiot/inc")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./parson/parson.c
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./parson/parson.h
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```
1. <span data-ttu-id="54f0e-134">I hello **remote_monitoring** mapp, skapa en mapp toostore hello *Se* filer som CMake genererar och kör sedan hello **cmake** och **Se** kommandon på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="54f0e-134">In hello **remote_monitoring** folder, create a folder toostore hello *make* files that CMake generates and then run hello **cmake** and **make** commands as follows:</span></span>
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. <span data-ttu-id="54f0e-135">Köra hello-klientprogram och skicka telemetri tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="54f0e-135">Run hello client application and send telemetry tooIoT Hub:</span></span>
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

