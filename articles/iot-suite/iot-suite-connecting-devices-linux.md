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
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-linux"></a>Ansluta din enhet toohello remote förkonfigurerade övervakningslösning (Linux)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Skapa och kör ett exempel på C-klienten Linux
hello följande steg visar hur toocreate ett klientprogram som kommunicerar med hello fjärrövervaknings förkonfigurerade lösningen. Det här programmet har skrivits i C och bygger och kör på Ubuntu Linux.

toocomplete som stegen nedan måste en enhet som kör Ubuntu version 15.04 eller 15.10. Innan du fortsätter att installera hello nödvändiga paket på enheten Ubuntu med hello följande kommando:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-hello-client-libraries-on-your-device"></a>Installera hello klientbibliotek på din enhet
hello Azure IoT Hub-klientbibliotek är tillgänglig som ett paket som du kan installera på enheten Ubuntu med hello **lgh get** kommando. Utför följande steg tooinstall hello paket som innehåller hello klientbibliotek för IoT-hubb och rubrikfiler på datorn Ubuntu hello:

1. Lägg till hello AzureIoT databasen tooyour datorn i ett gränssnitt:
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. Installationspaket för hello azure iot-sdk-c-utveckling
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-hello-parson-json-parser"></a>Installera hello Parson JSON-parsern
Hej IoT-hubb klienten bibliotek använder hello Parson JSON-parsern tooparse meddelande nyttolaster. Klona hello Parson GitHub-databas med hjälp av hello följande kommando i en lämplig mapp på datorn:

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a>Förbered ditt projekt
Skapa en mapp med namnet på datorn Ubuntu **remote\_övervakning**. I hello **remote\_övervakning** mapp:

- Skapa hello fyra filer **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h**, och **CMakeLists.txt**.
- Skapa mapp med namnet **parson**.

Kopiera filerna hello **parson.c** och **parson.h** från den lokala kopian av hello Parson databasen till hello **remote\_övervakning/parson** mapp.

Öppna hello i en textredigerare, **remote\_monitoring.c** fil. Lägg till följande hello `#include` instruktioner:
   
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

## <a name="call-hello-remotemonitoringrun-function"></a>Anropa hello remote\_övervakning\_köra funktionen
Öppna hello i en textredigerare, **remote_monitoring.h** fil. Lägg till följande kod hello:

```
void remote_monitoring_run(void);
```

Öppna hello i en textredigerare, **main.c** fil. Lägg till följande kod hello:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-hello-application"></a>Skapa och köra programmet hello
hello följande steg beskriver hur toouse *CMake* toobuild klientprogrammet.

1. Öppna hello i en textredigerare, **CMakeLists.txt** filen i hello **remote_monitoring** mapp.

1. Lägg till följande instruktioner toodefine hur hello toobuild klientprogrammet:
   
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
1. I hello **remote_monitoring** mapp, skapa en mapp toostore hello *Se* filer som CMake genererar och kör sedan hello **cmake** och **Se** kommandon på följande sätt:
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Köra hello-klientprogram och skicka telemetri tooIoT hubb:
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

