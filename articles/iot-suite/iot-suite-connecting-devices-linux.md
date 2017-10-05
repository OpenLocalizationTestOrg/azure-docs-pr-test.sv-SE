---
title: "Ansluter en enhet med hjälp av C på Linux | Microsoft Docs"
description: "Beskriver hur du ansluter en enhet till Azure IoT Suite förkonfigurerade fjärråtkomst övervakning lösningen med hjälp av ett program som skrivits i C som körs på Linux."
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
ms.openlocfilehash: 9adbc9cc13f0b4cafa3a3a7703c46f8085b15232
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a><span data-ttu-id="b3bc2-103">Ansluta enheten till den fjärranslutna förkonfigurerade övervakningslösning (Linux)</span><span class="sxs-lookup"><span data-stu-id="b3bc2-103">Connect your device to the remote monitoring preconfigured solution (Linux)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a><span data-ttu-id="b3bc2-104">Skapa och kör ett exempel på C-klienten Linux</span><span class="sxs-lookup"><span data-stu-id="b3bc2-104">Build and run a sample C client Linux</span></span>
<span data-ttu-id="b3bc2-105">Följande steg visar hur du skapar ett klientprogram som kommunicerar med fjärråtkomst övervakning förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-105">The following steps show you how to create a client application that communicates with the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="b3bc2-106">Det här programmet har skrivits i C och bygger och kör på Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-106">This application is written in C and built and run on Ubuntu Linux.</span></span>

<span data-ttu-id="b3bc2-107">För att slutföra de här stegen behöver du en enhet som kör Ubuntu version 15.04 eller 15.10.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-107">To complete these steps, you need a device running Ubuntu version 15.04 or 15.10.</span></span> <span data-ttu-id="b3bc2-108">Innan du fortsätter bör du installera de nödvändiga paketen på Ubuntu enheten med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-108">Before proceeding, install the prerequisite packages on your Ubuntu device using the following command:</span></span>

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a><span data-ttu-id="b3bc2-109">Installera klientbiblioteken på din enhet</span><span class="sxs-lookup"><span data-stu-id="b3bc2-109">Install the client libraries on your device</span></span>
<span data-ttu-id="b3bc2-110">Azure IoT Hub-klientbibliotek är tillgänglig som ett paket som du kan installera på din Ubuntu enheter med hjälp av den **lgh get** kommando.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-110">The Azure IoT Hub client libraries are available as a package you can install on your Ubuntu device using the **apt-get** command.</span></span> <span data-ttu-id="b3bc2-111">Utför följande steg för att installera det paket som innehåller IoT-hubb klienten bibliotek och huvudet filerna på din Ubuntu-dator:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-111">Complete the following steps to install the package that contains the IoT Hub client library and header files on your Ubuntu computer:</span></span>

1. <span data-ttu-id="b3bc2-112">Lägg till AzureIoT databasen till datorn i ett gränssnitt:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-112">In a shell, add the AzureIoT repository to your computer:</span></span>
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. <span data-ttu-id="b3bc2-113">Installera paketet azure iot-sdk-c-utveckling</span><span class="sxs-lookup"><span data-stu-id="b3bc2-113">Install the azure-iot-sdk-c-dev package</span></span>
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-the-parson-json-parser"></a><span data-ttu-id="b3bc2-114">Installera Parson JSON-parser</span><span class="sxs-lookup"><span data-stu-id="b3bc2-114">Install the Parson JSON parser</span></span>
<span data-ttu-id="b3bc2-115">Klientbibliotek för IoT-hubb använda Parson JSON-parsern för att parsa meddelandet nyttolaster.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-115">The IoT Hub client libraries use the Parson JSON parser to parse message payloads.</span></span> <span data-ttu-id="b3bc2-116">Klona Parson GitHub-lagret med hjälp av följande kommando i en lämplig mapp på datorn:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-116">In a suitable folder on your computer, clone the Parson GitHub repository using the following command:</span></span>

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a><span data-ttu-id="b3bc2-117">Förbered ditt projekt</span><span class="sxs-lookup"><span data-stu-id="b3bc2-117">Prepare your project</span></span>
<span data-ttu-id="b3bc2-118">Skapa en mapp med namnet på datorn Ubuntu **remote\_övervakning**.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-118">On your Ubuntu machine, create a folder called **remote\_monitoring**.</span></span> <span data-ttu-id="b3bc2-119">I den **remote\_övervakning** mapp:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-119">In the **remote\_monitoring** folder:</span></span>

- <span data-ttu-id="b3bc2-120">Skapa fyra filerna **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h**, och **CMakeLists.txt**.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-120">Create the four files **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h**, and **CMakeLists.txt**.</span></span>
- <span data-ttu-id="b3bc2-121">Skapa mapp med namnet **parson**.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-121">Create folder called **parson**.</span></span>

<span data-ttu-id="b3bc2-122">Kopiera filerna **parson.c** och **parson.h** från den lokala kopian av Parson databasen till den **remote\_övervakning/parson** mapp.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-122">Copy the files **parson.c** and **parson.h** from your local copy of the Parson repository into the **remote\_monitoring/parson** folder.</span></span>

<span data-ttu-id="b3bc2-123">I en textredigerare, öppnar den **remote\_monitoring.c** fil.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-123">In a text editor, open the **remote\_monitoring.c** file.</span></span> <span data-ttu-id="b3bc2-124">Lägg till följande `#include`-uttryck:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-124">Add the following `#include` statements:</span></span>
   
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

## <a name="call-the-remotemonitoringrun-function"></a><span data-ttu-id="b3bc2-125">Anropa fjärransluten\_övervakning\_köra funktionen</span><span class="sxs-lookup"><span data-stu-id="b3bc2-125">Call the remote\_monitoring\_run function</span></span>
<span data-ttu-id="b3bc2-126">I en textredigerare, öppnar den **remote_monitoring.h** fil.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-126">In a text editor, open the **remote_monitoring.h** file.</span></span> <span data-ttu-id="b3bc2-127">Lägg till följande kod:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-127">Add the following code:</span></span>

```
void remote_monitoring_run(void);
```

<span data-ttu-id="b3bc2-128">I en textredigerare, öppnar den **main.c** fil.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-128">In a text editor, open the **main.c** file.</span></span> <span data-ttu-id="b3bc2-129">Lägg till följande kod:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-129">Add the following code:</span></span>

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-the-application"></a><span data-ttu-id="b3bc2-130">Skapa och kör appen</span><span class="sxs-lookup"><span data-stu-id="b3bc2-130">Build and run the application</span></span>
<span data-ttu-id="b3bc2-131">Följande steg beskriver hur du använder *CMake* att skapa ditt klientprogram.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-131">The following steps describe how to use *CMake* to build your client application.</span></span>

1. <span data-ttu-id="b3bc2-132">I en textredigerare, öppnar den **CMakeLists.txt** filen i den **remote_monitoring** mapp.</span><span class="sxs-lookup"><span data-stu-id="b3bc2-132">In a text editor, open the **CMakeLists.txt** file in the **remote_monitoring** folder.</span></span>

1. <span data-ttu-id="b3bc2-133">Lägg till följande instruktioner för att definiera hur du skapar ditt klientprogram:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-133">Add the following instructions to define how to build your client application:</span></span>
   
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
1. <span data-ttu-id="b3bc2-134">I den **remote_monitoring** mapp, skapa en mapp för att lagra den *Se* filer som CMake genererar och kör sedan den **cmake** och **Se** följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-134">In the **remote_monitoring** folder, create a folder to store the *make* files that CMake generates and then run the **cmake** and **make** commands as follows:</span></span>
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. <span data-ttu-id="b3bc2-135">Kör klientprogrammet och skicka telemetri till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="b3bc2-135">Run the client application and send telemetry to IoT Hub:</span></span>
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

