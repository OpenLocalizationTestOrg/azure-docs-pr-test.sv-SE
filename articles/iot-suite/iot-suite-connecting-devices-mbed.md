---
title: "Ansluter en enhet med hjälp av C på mbed | Microsoft Docs"
description: "Beskriver hur du ansluter en enhet till Azure IoT Suite förkonfigurerade fjärråtkomst övervakning lösningen med hjälp av ett program som skrivits i C som körs på en mbed enhet."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 9551075e-dcf9-488f-943e-d0eb0e6260be
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: dobett
ROBOTS: NOINDEX
ms.openlocfilehash: ef7b78f85a787f8fbe22c0e26aa34f0cd1685d58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Ansluta enheten till den fjärranslutna förkonfigurerade övervakningslösning (mbed)

## <a name="scenario-overview"></a>Scenarioöversikt
I det här scenariot skapar du en enhet som skickar följande telemetri till den [förkonfigurerade lösningen][lnk-what-are-preconfig-solutions] för fjärrövervakning:

* Extern temperatur
* Intern temperatur
* Fuktighet

För enkelhetens skull genererar koden på enheten exempelvärden men vi rekommenderar att du utökar exemplet genom att ansluta riktiga sensorer till enheten och skicka riktig telemetri.

Enheten kan även svara på metoderna som anropas från lösningens instrumentpanel och de önskade egenskapsvärden som är angivna i lösningens instrumentpanel.

Du behöver ett Azure-konto för att slutföra den här självstudiekursen. Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].

## <a name="before-you-start"></a>Innan du börjar
Innan du kan skriva kod för enheten måste du etablera din förkonfigurerade lösning för fjärrövervakning och etablera en ny anpassad enhet i lösningen.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Etablera din förkonfigurerade lösning för fjärrövervakning
Enheten som du skapar i den här självstudien skickar data till en instans av den förkonfigurerade lösningen för [fjärrövervakning][lnk-remote-monitoring]. Om du inte redan har etablerat den förkonfigurerade lösningen för fjärrövervakning i ditt Azure-konto använder du följande steg:

1. Gå till <https://www.azureiotsuite.com/> och klicka på **+** för att skapa en lösning.
2. Klicka på **Välj** på panelen **Fjärrövervakning** för att skapa din lösning.
3. På sidan **Create Remote monitoring solution** (Skapa fjärrövervakningslösning) anger du ett **Lösningsnamn** och väljer den **Region** du vill distribuera till samt väljer den Azure-prenumerationen som du vill använda. Klicka på **Skapa lösning**.
4. Vänta tills etableringsprocessen har slutförts.

> [!WARNING]
> De förkonfigurerade lösningarna använder fakturerbara Azure-tjänster. Se till att ta bort den förkonfigurerade lösningen från prenumerationen när du är färdig med den för att undvika onödiga kostnader. Du kan ta bort en förkonfigurerad lösning helt från din prenumeration genom att besöka sidan <https://www.azureiotsuite.com/>.
> 
> 

När etableringen av fjärrövervakningslösningen är klar klickar du på **Starta** för att öppna lösningens instrumentpanel i webbläsaren.

![Instrumentpanel för lösningen][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Etablera enheten i fjärrövervakningslösningen
> [!NOTE]
> Om du redan har etablerat en enhet i din lösning kan du hoppa över det här steget. Du behöver känna till enhetens autentiseringsuppgifter när du skapar klientprogrammet.
> 
> 

För att en enhet ska kunna ansluta till den förkonfigurerade lösningen måste den identifiera sig för IoT Hub med giltiga autentiseringsuppgifter. Du kan hämta enhetens autentiseringsuppgifter från lösningens instrumentpanel. Du kan inkludera enhetsautentiseringsuppgifterna i klientprogrammet senare i den här självstudien.

Om du vill lägga till en enhet till din fjärrövervakningslösning utför du följande steg i lösningens instrumentpanel:

1. Klicka på **Lägg till en enhet** i det nedre vänstra hörnet på instrumentpanelen.
   
   ![Lägg till en enhet][1]
2. I panelen **Anpassad enhet** klickar du på **Lägg till ny**.
   
   ![Lägg till en anpassad enhet][2]
3. Välj **Låt mig ange mitt eget enhets-ID**. Ange ett enhets-ID, t.ex. **mydevice** och klicka på **Kontrollera ID** för att kontrollera att namnet inte redan används. Klicka sedan på **Skapa** för att etablera enheten.
   
   ![Lägg till enhets-ID][3]
4. Notera enhetsautentiseringsuppgifterna (Enhets-ID, IoT Hub-värdnamn och Enhetsnyckel). Klientprogrammet behöver dessa värden för att ansluta till fjärrövervakningslösningen. Klicka sedan på **Klar**.
   
    ![Visa enhetsautentiseringsuppgifter][4]
5. Välj enheten i enhetslistan i lösningens instrumentpanel. Klicka sedan på **Aktivera enhet** i panelen **Enhetsinformation**. Statusen för din enhet är nu **Körs**. Fjärrövervakningslösningen kan nu ta emot telemetri från enheten och anropa metoder på enheten.

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-the-c-sample-solution"></a>Skapa och köra exempellösning C

I följande anvisningar beskrivs stegen för att ansluta en [mbed-aktiverade Freescale FRDM-K64F] [ lnk-mbed-home] enheten till den fjärranslutna övervakningslösning.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Anslut mbed enheten till din dator för Nätverks- och fjärrskrivbordsanslutning

1. Anslut mbed enheten till nätverket med hjälp av en Ethernet-kabel. Det här steget är nödvändigt eftersom exempelprogrammet kräver tillgång till internet.

1. Se [komma igång med mbed] [ lnk-mbed-getstarted] ansluta enheten mbed till din dator.

1. Om din dator kör Windows, se [Datorkonfiguration] [ lnk-mbed-pcconnect] att konfigurera serieport åtkomst till enheten mbed.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Skapa ett mbed projekt och importera exempelkoden

Följ dessa steg för att lägga till vissa exempelkod till en mbed-projekt. Du importerar fjärråtkomst övervakning starter-projektet och ändra sedan projektet om du vill använda protokollet MQTT i stället för AMQP-protokollet. För närvarande kan behöver du använda MQTT-protokollet för att använda hanteringsfunktioner för enheter i IoT-hubb.

1. I webbläsaren, går du till mbed.org [developer plats](https://developer.mbed.org/). Om du inte har registrerat dig kan se du ett alternativ för att skapa ett konto (som är ledigt). Annars kan logga in med autentiseringsuppgifterna för ditt konto. Klicka på **kompileraren** i det övre högra hörnet på sidan. Den här åtgärden ger dig den *arbetsytan* gränssnitt.

1. Kontrollera att maskinvaruplattform som du använder som visas i det övre högra hörnet i fönstret eller klicka på ikonen i det högra hörnet och välj din maskinvaruplattform.

1. Klicka på **importera** på huvudmenyn. Klicka på **Klicka här om du vill importera från URL: en**.
   
    ![Starta import till mbed arbetsyta][6]

1. Ange länken för exemplet kod https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ i popup-fönstret och klicka sedan på **importera**.
   
    ![Importera exempelkod till mbed arbetsytan][7]

1. Du kan se i fönstret mbed kompilatorn att importera det här projektet också importerar olika bibliotek. Vissa tillhandahålls och underhålls av Azure IoT-teamet ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), medan andra är tredjeparts-bibliotek finns i katalogen mbed bibliotek.
   
    ![Visa mbed projekt][8]

1. I den **programmet arbetsytan**, högerklicka på den **iothub\_amqp\_transport** bibliotek, klickar du på **ta bort**, och klicka sedan på  **OK** att bekräfta.

1. I den **programmet arbetsytan**, högerklicka på den **azure\_amqp\_c** bibliotek, klickar du på **ta bort**, och klicka sedan på **OK** att bekräfta.

1. Högerklicka på den **remote_monitoring** projektet i den **programmet arbetsytan**väljer **importera bibliotek**och välj **från URL: en**.
   
    ![Starta import av typbibliotek till mbed arbetsyta][6]

1. Ange länken i popup-fönstret för MQTT transport biblioteket https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport / Klicka **importera**.
   
    ![Importera bibliotek till mbed arbetsyta][12]

1. Upprepa det föregående steget för att lägga till biblioteket MQTT från https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.

1. Arbetsytan nu ser ut ungefär så här:

    ![Visa mbed arbetsyta][13]

1. Öppna fjärransluten\_monitoring\remote_monitoring.c fil- och Ersätt den befintliga `#include` instruktioner med följande kod:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"

    #ifdef MBED_BUILD_TIMESTAMP
    #include "certs.h"
    #endif // MBED_BUILD_TIMESTAMP
    ```
1. Ta bort den återstående koden i fjärransluten\_monitoring\remote\_monitoring.c fil.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Skapa och köra exemplet

Lägg till kod för att anropa den **remote\_övervakning\_kör** fungera och sedan skapa och köra programmet för enheten.

1. Lägg till en **huvudsakliga** funktionen med följande kod i slutet av fjärransluten\_monitoring.c fil att anropa den **remote\_övervakning\_kör** funktionen:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klicka på **Kompilera** att skapa programmet. Du kan på ett säkert sätt ignoreras alla varningar, men om bygga genererar fel, åtgärda dem innan du fortsätter.

1. Om det lyckas bygga mbed kompileraren webbplatsen genererar en .bin-filen med namnet på ditt projekt och hämtas till din lokala dator. Kopiera .bin-filen till enheten. Spara .bin-filen till enheten gör att enheten att starta om och kör programmet i .bin-filen. Du kan starta om programmet manuellt när som helst genom att trycka på återställningsknappen på mbed enheten.

1. Ansluta till den enhet som använder en SSH-klientprogram, till exempel PuTTY. Du kan fastställa serieporten enheten använder genom att kontrollera i Enhetshanteraren Windows.
   
    ![][11]

1. I PuTTY, klickar du på den **seriella** anslutningstypen. Enheten ansluts vanligtvis på 9 600 baud, så ange 9600 i den **hastighet** rutan. Klicka på **öppna**.

1. Programmet startar körs. Du kan behöva återställa kortet (tryck på CTRL + Break eller tryck på den board Återställ-knapp) om programmet inte startar automatiskt när du ansluter.
   
    ![][10]

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png
[12]: ./media/iot-suite-connecting-devices-mbed/mbed7.png
[13]: ./media/iot-suite-connecting-devices-mbed/mbed8.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
