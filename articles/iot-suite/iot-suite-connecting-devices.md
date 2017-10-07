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
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-windows"></a>Ansluta din enhet toohello remote förkonfigurerade övervakningslösning (Windows)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Skapa en C exempellösning i Windows
hello följande steg visar hur toocreate ett klientprogram som kommunicerar med hello fjärrövervaknings förkonfigurerade lösningen. Det här programmet är skrivna i C och inbyggda och körs i Windows.

Skapa ett starter-projekt i Visual Studio 2015 eller Visual Studio 2017 och Lägg till NuGet-paket för hello IoT-hubb enheten klienten:

1. Skapa ett med hjälp av hello Visual C++ C-konsolprogram i Visual Studio **Win32-konsolprogram** mall. Namnet hello projektet **RMDevice**.
2. På hello **programinställningar** sida i hello **Win32 programguiden**, se till att **konsolprogrammet** är markerad och avmarkera **Precompiled huvudet** och **Security Development Lifecycle (SDL) kontrollerar**.
3. I **Solution Explorer**, ta bort hello filer stdafx.h, targetver.h och stdafx.cpp.
4. I **Solution Explorer**, byta namn på hello filen RMDevice.cpp tooRMDevice.c.
5. I **Solution Explorer**, högerklicka på hello **RMDevice** projektet och klicka sedan på **hantera NuGet-paket**. Klicka på **Bläddra**, och Sök efter och installera hello följande NuGet-paket:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. I **Solution Explorer**, högerklicka på hello **RMDevice** projektet och klicka sedan på **egenskaper** tooopen hello projektet **egenskapssidor**dialogrutan. Mer information finns i [Visual C++-projekt inställningsegenskaper][lnk-c-project-properties]. 
7. Klicka på hello **Linker** mappen, klicka på hello **indata** egenskapssidan.
8. Lägg till **crypt32.lib** toohello **Additional Dependencies** egenskapen. Klicka på **OK** och sedan **OK** egenskapsvärden toosave igen hello-projekt.

Lägg till hello Parson JSON biblioteket toohello **RMDevice** projekt och Lägg till hello krävs `#include` instruktioner:

1. Klona hello Parson GitHub-databas med hjälp av hello följande kommando i en lämplig mapp på datorn:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopiera hello parson.h och parson.c från hello lokal kopia av hello Parson databasen tooyour **RMDevice** projektmappen.

1. I Visual Studio högerklickar du på hello **RMDevice** projektet, klicka på **Lägg till**, och klicka sedan på **befintlig artikel**.

1. I hello **Lägg till befintligt objekt** dialogrutan, Välj hello parson.h och parson.c filer i hello **RMDevice** projektmappen. Klicka på **Lägg till** tooadd dessa två filer tooyour projekt.

1. Öppna hello RMDevice.c filen i Visual Studio. Ersätta befintliga hello `#include` instruktioner med hello följande kod:
   
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
    > Nu kan du kontrollera att projektet har hello rätt beroenden konfigurera genom att skapa den.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a>Skapa och köra hello-exempel

Lägg till kod tooinvoke hello **remote\_övervakning\_kör** fungera och sedan skapa och köra hello enhetsprogram.

1. Ersätt hello **huvudsakliga** funktionen med följande kod tooinvoke hello **remote\_övervakning\_kör** funktionen:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klicka på **skapa** och sedan **skapa lösning** toobuild hello enhetsprogram.

1. I **Solution Explorer**, högerklicka på hello **RMDevice** projektet, klicka på **felsöka**, och klicka sedan på **Starta ny instans** toorun hello exempel. hello-konsolen visas meddelanden hello skickar exempel telemetri toohello förkonfigurerade lösningen, tar emot önskade egenskapsvärden i hello lösning instrumentpanelen och svarar toomethods anropas från hello lösning instrumentpanelen.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
