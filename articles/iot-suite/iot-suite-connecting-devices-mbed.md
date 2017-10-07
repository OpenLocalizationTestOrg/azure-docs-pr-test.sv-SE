---
title: "aaaConnect en enhet med hjälp av C på mbed | Microsoft Docs"
description: "Beskriver hur tooconnect en enhet toohello Azure IoT Suite förkonfigurerade remote övervakningslösning som använder ett program som skrivits i C som körs på en mbed enhet."
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
ms.openlocfilehash: dcd1e74635e8dec678a59bff060a73f7cfabd124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-mbed"></a>Ansluta din enhet toohello remote förkonfigurerade övervakningslösning (mbed)

## <a name="scenario-overview"></a>Scenarioöversikt
I det här scenariot skapar du en enhet som skickar hello följande telemetri toohello fjärrövervaknings [förkonfigurerade lösningen][lnk-what-are-preconfig-solutions]:

* Extern temperatur
* Intern temperatur
* Fuktighet

För enkelhetens skull hello koden på hello enhet genererar exempelvärden, men vi rekommenderar att du tooextend hello exemplet genom att ansluta verkliga sensorer tooyour enheten och skicka verkliga telemetri.

hello-enheten är också kan toorespond toomethods anropas från hello lösning instrumentpanelen och önskade egenskapsvärden i hello lösning instrumentpanelen.

toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].

## <a name="before-you-start"></a>Innan du börjar
Innan du kan skriva kod för enheten måste du etablera din förkonfigurerade lösning för fjärrövervakning och etablera en ny anpassad enhet i lösningen.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Etablera din förkonfigurerade lösning för fjärrövervakning
hello-enhet som du skapar i den här självstudiekursen skickar data tooan instans av hello [fjärrövervaknings] [ lnk-remote-monitoring] förkonfigurerade lösningen. Om du inte redan har etablerats hello fjärråtkomst övervakning förkonfigurerade lösning i ditt Azure-konto, använder du hello följande steg:

1. På hello <https://www.azureiotsuite.com/> klickar du på  **+**  toocreate en lösning.
2. Klicka på **Välj** på hello **fjärrövervaknings** panelen toocreate din lösning.
3. På hello **skapa Remote övervakningslösning** anger en **lösningsnamn** du själv väljer, Välj hello **Region** toodeploy till, och markera hello Azure prenumerationen toowant toouse. Klicka på **Skapa lösning**.
4. Vänta tills hello etableringsprocessen har slutförts.

> [!WARNING]
> hello förkonfigurerade lösningar använder fakturerbar Azure-tjänster. Glöm inte tooremove hello förkonfigurerade lösningen från prenumerationen när du är klar med den tooavoid eventuella onödiga kostnader. Du kan helt ta bort en förkonfigurerade lösning från prenumerationen genom att besöka hello <https://www.azureiotsuite.com/> sidan.
> 
> 

När hello etableringsprocessen för hello remote övervakningslösning är klar klickar du på **starta** tooopen hello lösning instrumentpanel i din webbläsare.

![Instrumentpanel för lösningen][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a>Etablera din enhet i hello remote övervakningslösning
> [!NOTE]
> Om du redan har etablerat en enhet i din lösning kan du hoppa över det här steget. När du skapar hello klientprogrammet behöver du tooknow hello enheten autentiseringsuppgifter.
> 
> 

För en enhet tooconnect toohello förkonfigurerade lösning, den måste identifiera sig själv tooIoT hubb med giltiga autentiseringsuppgifter. Du kan hämta hello enheten autentiseringsuppgifter från hello lösning instrumentpanelen. Du inkludera hello enheten autentiseringsuppgifter i ditt klientprogram senare i den här kursen.

tooadd en enhet tooyour remote övervakningslösning fullständig hello följa stegen i hello lösning instrumentpanelen:

1. I hello nedre vänstra hörnet av hello instrumentpanelen, klickar du på **lägger till en enhet**.
   
   ![Lägg till en enhet][1]
2. I hello **anpassad enhet** klickar du på **Lägg till ny**.
   
   ![Lägg till en anpassad enhet][2]
3. Välj **Låt mig ange mitt eget enhets-ID**. Ange ett enhets-ID som **mydevice**, klickar du på **kontrollera ID** tooverify namnet inte redan används och klicka sedan på **skapa** tooprovision hello enhet.
   
   ![Lägg till enhets-ID][3]
4. Gör en anteckning hello enheten autentiseringsuppgifter (enhets-ID, IoT Hub-värdnamnet och nyckeln enheten). Klientprogrammet måste dessa värden tooconnect toohello remote övervakningslösning. Klicka sedan på **Klar**.
   
    ![Visa enhetsautentiseringsuppgifter][4]
5. Välj din enhet i listan över enheter hello hello lösning instrumentpanelen. Sedan hello **enhetsinformation** klickar du på **Aktivera enhet**. hello statusen för din enhet är nu **kör**. hello remote övervakningslösning kan nu ta emot telemetri från enheten och anropa metoder i hello enhet.

[img-dashboard]: ./media/iot-suite-connecting-devices-mbed/dashboard.png
[1]: ./media/iot-suite-connecting-devices-mbed/suite0.png
[2]: ./media/iot-suite-connecting-devices-mbed/suite1.png
[3]: ./media/iot-suite-connecting-devices-mbed/suite2.png
[4]: ./media/iot-suite-connecting-devices-mbed/suite3.png

[lnk-what-are-preconfig-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

## <a name="build-and-run-hello-c-sample-solution"></a>Skapa och köra hello C exempellösning

hello följande anvisningar beskrivs hello åtgärder för att ansluta en [mbed-aktiverade Freescale FRDM-K64F] [ lnk-mbed-home] enhet toohello remote övervakningslösning.

### <a name="connect-hello-mbed-device-tooyour-network-and-desktop-machine"></a>Ansluta hello mbed enhet tooyour nätverks- och stationära datorer

1. Ansluta hello mbed enhet tooyour nätverk med hjälp av en Ethernet-kabel. Det här steget är nödvändigt eftersom hello exempelprogrammet kräver tillgång till internet.

1. Se [komma igång med mbed] [ lnk-mbed-getstarted] tooconnect din mbed enhet tooyour stationär dator.

1. Om din dator kör Windows, se [Datorkonfiguration] [ lnk-mbed-pcconnect] tooconfigure serieport tooyour mbed enhet.

### <a name="create-an-mbed-project-and-import-hello-sample-code"></a>Skapa ett mbed projekt och importera hello exempelkod

Följ dessa steg tooadd vissa kod tooan mbed exempelprojektet. Du importerar hello fjärråtkomst övervakning starter-projekt och ändra sedan hello projektet toouse hello MQTT protokollet i stället för hello AMQP-protokollet. För närvarande måste toouse hello MQTT protokollet toouse hello enhetshanteringsfunktioner för IoT-hubb.

1. I webbläsaren, går toohello mbed.org [developer plats](https://developer.mbed.org/). Om du inte har registrerat dig kan se du en alternativet toocreate ett konto (som är ledigt). Annars kan logga in med autentiseringsuppgifterna för ditt konto. Klicka på **kompileraren** i hello övre högra hörnet av hello-sidan. Den här åtgärden ger dig toohello *arbetsytan* gränssnitt.

1. Kontrollera hello maskinvaruplattform du använder visas i hello övre högra hörnet av hello-fönstret eller klicka på hello ikon i hello höger tooselect plattform för maskinvaran.

1. Klicka på **importera** på hello huvudmenyn. Klicka på **Klicka här tooimport från URL**.
   
    ![Starta import toombed arbetsytan][6]

1. Ange hello länk för hello exempel kod https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ i hello popup-fönster, och klicka sedan på **importera**.
   
    ![Importera exempel kod toombed arbetsytan][7]

1. Du kan se i hello mbed kompileraren-fönstret att importera det här projektet också importerar olika bibliotek. Vissa tillhandahålls och underhålls av hello Azure IoT-teamet ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), medan andra är tillgängliga i hello mbed bibliotek katalog från tredje part-bibliotek.
   
    ![Visa mbed projekt][8]

1. I hello **programmet arbetsytan**, högerklicka på hello **iothub\_amqp\_transport** bibliotek, klickar du på **ta bort**, och klicka sedan på **OK** tooconfirm.

1. I hello **programmet arbetsytan**, högerklicka på hello **azure\_amqp\_c** bibliotek, klickar du på **ta bort**, och klicka sedan på **OK**  tooconfirm.

1. Högerklicka på hello **remote_monitoring** projekt i hello **programmet arbetsytan**väljer **importera bibliotek**och välj **från URL: en**.
   
    ![Starta biblioteket Importera toombed arbetsytan][6]

1. I hello popup-fönster, ange hello länk hello MQTT transport biblioteket https://developer.mbed.org/users/AzureIoTClient/code/iothub\_mqtt\_transport / Klicka **importera**.
   
    ![Importera bibliotek toombed arbetsytan][12]

1. Hello Upprepa föregående steg tooadd hello MQTT bibliotek från https://developer.mbed.org/users/AzureIoTClient/code/azure\_umqtt\_c /.

1. Arbetsytan nu ser ut som följande hello:

    ![Visa mbed arbetsyta][13]

1. Öppna hello remote\_monitoring\remote_monitoring.c fil- och Ersätt hello befintliga `#include` instruktioner med hello följande kod:

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
1. Ta bort alla hello återstående koden i hello remote\_monitoring\remote\_monitoring.c fil.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a>Skapa och köra hello-exempel

Lägg till kod tooinvoke hello **remote\_övervakning\_kör** fungera och sedan skapa och köra hello enhetsprogram.

1. Lägg till en **huvudsakliga** funktionen med följande kod hello slutet av hello remote\_monitoring.c filen tooinvoke hello **remote\_övervakning\_kör** funktionen:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Klicka på **Kompilera** toobuild hello program. Du kan på ett säkert sätt ignoreras alla varningar, men om hello build genererar fel, åtgärda dem innan du fortsätter.

1. Om det lyckas hello build hello mbed kompileraren webbplats genererar en .bin-filen med hello namnet på ditt projekt och hämtar den tooyour lokal dator. Kopiera hello .bin-filen toohello enhet. Spara hello .bin-filen toohello enhet gör hello enheten toorestart och kör hello program som finns i hello .bin-filen. Du kan starta om programmet hello manuellt när som helst genom att trycka på hello Återställ-knappen på hello mbed enhet.

1. Ansluta toohello enhet med hjälp av en SSH-klientprogram, till exempel PuTTY. Du kan fastställa hello serieport enheten använder genom att kontrollera i Enhetshanteraren Windows.
   
    ![][11]

1. Klicka på hello i PuTTY, **seriella** anslutningstypen. hello enheten ansluts vanligtvis på 9 600 baud, så ange 9600 i hello **hastighet** rutan. Klicka på **öppna**.

1. hello postprogram körs. Du kan ha tooreset hello board (tryck på CTRL + Break eller tryck på hello board Återställ-knapp) om hello inte startar automatiskt när du ansluter.
   
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
