---
title: "aaaGet igång med Azure IoT Hub (Python) | Microsoft Docs"
description: "Lär dig hur toosend enhet till moln meddelanden tooAzure IoT-hubb med IoT-SDK för Python. Skapa simulerade enheten och tjänsten appar tooregister enheten, skicka meddelanden och läsa meddelanden från IoT-hubb."
services: iot-hub
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: python
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: dkshir
ms.custom: na
ms.openlocfilehash: aa23e792fb144202e121274723bcfaeae0c04723
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-python"></a>Ansluta din simulerade enhet tooyour IoT-hubb som använder Python
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Hello slutet av den här självstudiekursen, kommer du har två Python-appar:

* **CreateDeviceIdentity.py**, vilket skapar en enhetsidentitet och tillhörande nyckeln tooconnect appen simulerade enheten.
* **SimulatedDevice.py**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och skickar en telemetri meddelande regelbundet med hello MQTT-protokollet.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.
> 
> 

toocomplete den här kursen behöver du hello följande:

* [Python 2.x eller 3.x][lnk-python-download]. Se till att toouse hello 32-bitars eller 64-bitars installation som krävs av din konfiguration. När du uppmanas till detta under installationen av hello se till att tooadd Python tooyour plattformsspecifika miljövariabeln. Om du använder Python 2.x kan du behöva för[installera eller uppgradera *pip*, hello Python paketet hanteringssystemet][lnk-install-pip].
* Om du använder Windows-Operativsystemet sedan [Visual C++ redistributable package] [ lnk-visual-c-redist] tooallow hello användning av ursprungliga DLLs från Python.
* [Node.js 4.0 eller senare][lnk-node-download]. Se till att toouse hello 32-bitars eller 64-bitars installation som krävs av din konfiguration. Det här är nödvändig tooinstall hello [för IoT-hubb Explorer][lnk-iot-hub-explorer].
* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.

> [!NOTE]
> Hej *pip* paket för `azure-iothub-service-client` och `azure-iothub-device-client` är endast tillgänglig för Windows-Operativsystemet. Linux/Mac OS x finns toohello Linux och Mac OS-specifika avsnitt på hello [förbereda din utvecklingsmiljö för Python] [ lnk-python-devbox] efter.
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nu har du skapat din IoT Hub. Använd hello IoT Hub-värdnamnet och hello IoT-hubb anslutningssträngen i hello resten av den här kursen.

> [!NOTE]
> Du kan också enkelt skapa din IoT-hubb på en kommandorad med hello Python eller Node.js baserad Azure CLI. hello artikel [skapar en IoT-hubb med hello Azure CLI 2.0] [ lnk-azure-cli-hub] visar du hello snabbsteg toodo så. 
> 

## <a name="create-a-device-identity"></a>Skapa en enhetsidentitet
Det här avsnittet innehåller hello steg toocreate en Python-konsolapp som skapar en enhetsidentitet i hello identitetsregistret för din IoT-hubb. En enhet kan bara ansluta tooIoT hubb om det finns en post i registret för hello identitet. Mer information finns i hello **Identitetsregistret** avsnitt i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity]. När du kör den här konsolen appen genereras ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den skickar enhet till moln meddelanden tooIoT hubb.

1. Öppna en kommandotolk och installera hello **Azure IoT-hubb Service SDK för Python**. Stäng kommandotolken hello när du har installerat hello SDK.

    ```
    pip install azure-iothub-service-client
    ```

2. Skapa en Python-fil med namnet **CreateDeviceIdentity.py**. Öppna den i [en Python-redigeraren/IDE önskat][lnk-python-ide-list], till exempel hello standard [INAKTIVT][lnk-idle].

3. Lägg till följande kodmoduler tooimport hello krävs från hello service SDK hello:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. Lägg till hello följande kod, ersätter hello platshållare för `[IoTHub Connection String]` med hello anslutningssträng för hello IoT-hubb som du skapade i föregående avsnitt i hello. Du kan använda ett namn som hello `DEVICE_ID`.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. Lägg till hello följande funktion tooprint vissa hello enhetsinformation.

    ```python
    def print_device_info(title, iothub_device):
        print ( title + ":" )
        print ( "iothubDevice.deviceId                    = {0}".format(iothub_device.deviceId) )
        print ( "iothubDevice.primaryKey                  = {0}".format(iothub_device.primaryKey) )
        print ( "iothubDevice.secondaryKey                = {0}".format(iothub_device.secondaryKey) )
        print ( "iothubDevice.connectionState             = {0}".format(iothub_device.connectionState) )
        print ( "iothubDevice.status                      = {0}".format(iothub_device.status) )
        print ( "iothubDevice.lastActivityTime            = {0}".format(iothub_device.lastActivityTime) )
        print ( "iothubDevice.cloudToDeviceMessageCount   = {0}".format(iothub_device.cloudToDeviceMessageCount) )
        print ( "iothubDevice.isManaged                   = {0}".format(iothub_device.isManaged) )
        print ( "iothubDevice.authMethod                  = {0}".format(iothub_device.authMethod) )
        print ( "" )
    ```
3. Lägg till följande funktion toocreate hello enheten identifiering med hjälp av hello registret Manager hello. 

    ```python
    def iothub_createdevice():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
            auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
            new_device = iothub_registry_manager.create_device(DEVICE_ID, "", "", auth_method)
            print_device_info("CreateDevice", new_device)

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "iothub_createdevice stopped" )
    ```
4. Slutligen lägger du till hello huvudsakliga funktion på följande sätt och spara hello-filen.

    ```python
    if __name__ == '__main__':
        print ( "" )
        print ( "Python {0}".format(sys.version) )
        print ( "Creating device using hello Azure IoT Hub Service SDK for Python" )
        print ( "" )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_createdevice()
    ```
5. Hello Kommandotolken kör hello **CreateDeviceIdentity.py** på följande sätt:

    ```python
    python CreateDeviceIdentity.py
    ```
6. Du bör se hello simulerade enheten komma skapas. Skriv ner hello **deviceId** och hello **primaryKey** för den här enheten. Du måste dessa värden senare när du skapar ett program som ansluter tooIoT hubb som en enhet.

    ![Enheten har skapats][1]

> [!NOTE]
> Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb. Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet. Om ditt program måste toostore andra enhetsspecifika metadata, använder den en programspecifika butik. Mer information finns i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].
> 
> 


## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
Det här avsnittet innehåller hello steg toocreate en Python-konsolapp som simulerar en enhet och skickar meddelanden från enhet till moln tooyour IoT-hubb.

1. Öppna en ny kommandotolk och installera hello Azure IoT Hub-enhet SDK för Python på följande sätt. Stäng kommandotolken hello efter hello-installationen.

    ```
    pip install azure-iothub-device-client
    ```
2. Skapa en fil med namnet **SimulatedDevice.py**. Öppna filen i valfri Python-redigerare/IDE (till exempel IDLE).

3. Lägg till hello följande kodmoduler tooimport hello krävs från hello enheten SDK.

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. Lägg till hello följande kod och Ersätt hello platshållare för `[IoTHub Device Connection String]` med hello anslutningssträng för din enhet. anslutningssträngen för hello enhet är vanligtvis i hello-format för `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`. Använd hello **deviceId** och **primaryKey** av hello-enhet som du skapade i hello föregående avsnitt tooreplace hello `<deviceId>` och `<primaryKey>` respektive. Ersätt `<hostName>` med värdnamnet för IoT Hub, vanligtvis är det `<IoT hub name>.azure-devices.net`.

    ```python
    # String containing Hostname, Device Id & Device Key in hello format
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    # choose HTTP, AMQP or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    MESSAGE_TIMEOUT = 10000
    AVG_WIND_SPEED = 10.0
    SEND_CALLBACKS = 0
    MSG_TXT = "{\"deviceId\": \"MyFirstPythonDevice\",\"windSpeed\": %.2f}"    
    ```
5. Lägg till följande kod toodefine skicka bekräftelse återanrop hello. 

    ```python
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        print ( "Confirmation[%d] received for message with result = %s" % (user_context, result) )
        map_properties = message.properties()
        print ( "    message_id: %s" % message.message_id )
        print ( "    correlation_id: %s" % message.correlation_id )
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        SEND_CALLBACKS += 1
        print ( "    Total calls confirmed: %d" % SEND_CALLBACKS )
    ```
6. Lägg till följande kod tooinitialize hello klientenheter hello.

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set hello time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. Lägg till följande hello fungera tooformat och skicka ett meddelande från din simulerade enhet tooyour IoT-hubb.

    ```python
    def iothub_client_telemetry_sample_run():

        try:
            client = iothub_client_init()
            print ( "IoT Hub device sending periodic messages, press Ctrl-C tooexit" )
            message_counter = 0

            while True:
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                # messages can be encoded as string or bytearray
                if (message_counter & 1) == 1:
                    message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
                else:
                    message = IoTHubMessage(msg_txt_formatted)
                # optional: assign ids
                message.message_id = "message_%d" % message_counter
                message.correlation_id = "correlation_%d" % message_counter
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % message_counter
                prop_map.add("Property", prop_text)

                client.send_event_async(message, send_confirmation_callback, message_counter)
                print ( "IoTHubClient.send_event_async accepted message [%d] for transmission tooIoT Hub." % message_counter )

                status = client.get_send_status()
                print ( "Send status: %s" % status )
                time.sleep(30)

                status = client.get_send_status()
                print ( "Send status: %s" % status )

                message_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```
8. Slutligen lägger du till hello huvudsakliga funktion. 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using hello Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. Spara och Stäng hello **SimulatedDevice.py** fil. Du är nu redo toorun den här appen.

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a>Ta emot meddelanden från den simulerade enheten
tooreceive telemetri meddelanden från enheten, behöver du toouse en [Händelsehubbar][lnk-event-hubs-overview]-kompatibel slutpunkt som exponeras av hello IoT-hubb som läser hello meddelanden från enhet till moln. Läs hello [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] självstudiekursen information om hur tooprocess meddelanden från Händelsehubbar för slutpunkten för din IoT-hubb Event Hub-kompatibel. Händelsehubbar har inte stöd för telemetri i Python ännu, så du kan antingen skapa en [Node.js](iot-hub-node-node-getstarted.md#D2C_node) eller en [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) Händelsehubbar-baserade konsolen tooread hello enhet till moln meddelanden från IoT-hubb. Den här kursen visar hur du kan använda hello [för IoT-hubb Explorer] [ lnk-iot-hub-explorer] tooread dessa meddelanden.

1. Öppna en kommandotolk och installera hello IoT-hubb Explorer. 

    ```
    npm install -g iothub-explorer
    ```

2. Kör följande kommando i Kommandotolken för hello hello toobegin övervakning hello meddelanden från enhet till moln från enheten. Använd din IoT-hubb anslutningssträngen i hello platshållare efter `--login`.

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. Öppna en ny kommandotolk och gå toohello katalog som innehåller hello **SimulatedDevice.py** fil.

4. Kör hello **SimulatedDevice.py** fil som skickar regelbundet telemetri data tooyour IoT-hubb. 
   
    ```
    python SimulatedDevice.py
    ```
5. Se hälsningsmeddelande för enheten på hello Kommandotolken kör hello IoT-hubb Explorer från hello föregående avsnitt. 

    ![Meddelanden från enheten till molnet (Python)][2]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du har använt den här enhetens identitet tooenable hello simulerade enheten app toosend meddelanden från enhet till moln toohello IoT-hubb. Du har sett hello som tagits emot av hello IoT-hubb med hjälp av hello hello IoT-hubb Explorer-verktyget. 

tooexplore hello Python SDK för Azure IoT Hub-användning i djup, besök [den här hubb för Git-lagringsplatsen][lnk-python-github]. tooreview hello meddelandefunktioner för hello Azure IoT-hubb Service SDK för Python, som du kan hämta och köra [iothub_messaging_sample.py][lnk-messaging-sample]. För enheten på klientsidan simuleringen med hello Azure IoT Hub-enhet SDK för Python, som du kan hämta och köra hello [iothub_client_sample.py][lnk-client-sample].

toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:

* [Connecting your device][lnk-connect-device] (Ansluta din enhet)
* [Connecting your device][lnk-device-management] (Komma igång med enhetshantering)
* [Komma igång med Azure IoT Edge][lnk-iot-edge]

toolearn hur tooextend IoT-lösningen och processen enhet till moln meddelandena i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[1]: ./media/iot-hub-python-getstarted/createdevice.png
[2]: ./media/iot-hub-python-getstarted/sendd2cmessage.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-azure-cli-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-using-cli
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-idle]: https://docs.python.org/3/library/idle.html
[lnk-python-ide-list]: https://wiki.python.org/moin/IntegratedDevelopmentEnvironments
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-python-github]: https://github.com/Azure/azure-iot-sdk-python
[lnk-messaging-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/service/samples/iothub_messaging_sample.py
[lnk-client-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample.py

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
