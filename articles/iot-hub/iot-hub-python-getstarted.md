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
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-python"></a><span data-ttu-id="bc88b-104">Ansluta din simulerade enhet tooyour IoT-hubb som använder Python</span><span class="sxs-lookup"><span data-stu-id="bc88b-104">Connect your simulated device tooyour IoT hub using Python</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="bc88b-105">Hello slutet av den här självstudiekursen, kommer du har två Python-appar:</span><span class="sxs-lookup"><span data-stu-id="bc88b-105">At hello end of this tutorial, you will have two Python apps:</span></span>

* <span data-ttu-id="bc88b-106">**CreateDeviceIdentity.py**, vilket skapar en enhetsidentitet och tillhörande nyckeln tooconnect appen simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="bc88b-106">**CreateDeviceIdentity.py**, which creates a device identity and associated security key tooconnect your simulated device app.</span></span>
* <span data-ttu-id="bc88b-107">**SimulatedDevice.py**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och skickar en telemetri meddelande regelbundet med hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="bc88b-107">**SimulatedDevice.py**, which connects tooyour IoT hub with hello device identity created earlier, and periodically sends a telemetry message using hello MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="bc88b-108">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild toorun både program på enheter och din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="bc88b-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="bc88b-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="bc88b-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="bc88b-110">[Python 2.x eller 3.x][lnk-python-download].</span><span class="sxs-lookup"><span data-stu-id="bc88b-110">[Python 2.x or 3.x][lnk-python-download].</span></span> <span data-ttu-id="bc88b-111">Se till att toouse hello 32-bitars eller 64-bitars installation som krävs av din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="bc88b-111">Make sure toouse hello 32-bit or 64-bit installation as required by your setup.</span></span> <span data-ttu-id="bc88b-112">När du uppmanas till detta under installationen av hello se till att tooadd Python tooyour plattformsspecifika miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="bc88b-112">When prompted during hello installation, make sure tooadd Python tooyour platform-specific environment variable.</span></span> <span data-ttu-id="bc88b-113">Om du använder Python 2.x kan du behöva för[installera eller uppgradera *pip*, hello Python paketet hanteringssystemet][lnk-install-pip].</span><span class="sxs-lookup"><span data-stu-id="bc88b-113">If you are using Python 2.x, you may need too[install or upgrade *pip*, hello Python package management system][lnk-install-pip].</span></span>
* <span data-ttu-id="bc88b-114">Om du använder Windows-Operativsystemet sedan [Visual C++ redistributable package] [ lnk-visual-c-redist] tooallow hello användning av ursprungliga DLLs från Python.</span><span class="sxs-lookup"><span data-stu-id="bc88b-114">If you are using Windows OS, then [Visual C++ redistributable package][lnk-visual-c-redist] tooallow hello use of native DLLs from Python.</span></span>
* <span data-ttu-id="bc88b-115">[Node.js 4.0 eller senare][lnk-node-download].</span><span class="sxs-lookup"><span data-stu-id="bc88b-115">[Node.js 4.0 or later][lnk-node-download].</span></span> <span data-ttu-id="bc88b-116">Se till att toouse hello 32-bitars eller 64-bitars installation som krävs av din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="bc88b-116">Make sure toouse hello 32-bit or 64-bit installation as required by your setup.</span></span> <span data-ttu-id="bc88b-117">Det här är nödvändig tooinstall hello [för IoT-hubb Explorer][lnk-iot-hub-explorer].</span><span class="sxs-lookup"><span data-stu-id="bc88b-117">This is needed tooinstall hello [IoT Hub Explorer tool][lnk-iot-hub-explorer].</span></span>
* <span data-ttu-id="bc88b-118">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="bc88b-118">An active Azure account.</span></span> <span data-ttu-id="bc88b-119">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="bc88b-119">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="bc88b-120">Hej *pip* paket för `azure-iothub-service-client` och `azure-iothub-device-client` är endast tillgänglig för Windows-Operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="bc88b-120">hello *pip* packages for `azure-iothub-service-client` and `azure-iothub-device-client` are currently available only for Windows OS.</span></span> <span data-ttu-id="bc88b-121">Linux/Mac OS x finns toohello Linux och Mac OS-specifika avsnitt på hello [förbereda din utvecklingsmiljö för Python] [ lnk-python-devbox] efter.</span><span class="sxs-lookup"><span data-stu-id="bc88b-121">For Linux/Mac OS, please refer toohello Linux and Mac OS-specific sections on hello [Prepare your development environment for Python][lnk-python-devbox] post.</span></span>
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="bc88b-122">Nu har du skapat din IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bc88b-122">You have now created your IoT hub.</span></span> <span data-ttu-id="bc88b-123">Använd hello IoT Hub-värdnamnet och hello IoT-hubb anslutningssträngen i hello resten av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="bc88b-123">Use hello IoT Hub host name and hello IoT Hub connection string in hello rest of this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="bc88b-124">Du kan också enkelt skapa din IoT-hubb på en kommandorad med hello Python eller Node.js baserad Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="bc88b-124">You can also easily create your IoT hub on a command line, using hello Python or Node.js based Azure CLI.</span></span> <span data-ttu-id="bc88b-125">hello artikel [skapar en IoT-hubb med hello Azure CLI 2.0] [ lnk-azure-cli-hub] visar du hello snabbsteg toodo så.</span><span class="sxs-lookup"><span data-stu-id="bc88b-125">hello article [Create an IoT hub using hello Azure CLI 2.0][lnk-azure-cli-hub] shows you hello quick steps toodo so.</span></span> 
> 

## <a name="create-a-device-identity"></a><span data-ttu-id="bc88b-126">Skapa en enhetsidentitet</span><span class="sxs-lookup"><span data-stu-id="bc88b-126">Create a device identity</span></span>
<span data-ttu-id="bc88b-127">Det här avsnittet innehåller hello steg toocreate en Python-konsolapp som skapar en enhetsidentitet i hello identitetsregistret för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc88b-127">This section lists hello steps toocreate a Python console app, that creates a device identity in hello identity registry of your IoT hub.</span></span> <span data-ttu-id="bc88b-128">En enhet kan bara ansluta tooIoT hubb om det finns en post i registret för hello identitet.</span><span class="sxs-lookup"><span data-stu-id="bc88b-128">A device can only connect tooIoT Hub if it has an entry in hello identity registry.</span></span> <span data-ttu-id="bc88b-129">Mer information finns i hello **Identitetsregistret** avsnitt i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="bc88b-129">For more information, see hello **Identity Registry** section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="bc88b-130">När du kör den här konsolen appen genereras ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den skickar enhet till moln meddelanden tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="bc88b-130">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span>

1. <span data-ttu-id="bc88b-131">Öppna en kommandotolk och installera hello **Azure IoT-hubb Service SDK för Python**.</span><span class="sxs-lookup"><span data-stu-id="bc88b-131">Open a command prompt and install hello **Azure IoT Hub Service SDK for Python**.</span></span> <span data-ttu-id="bc88b-132">Stäng kommandotolken hello när du har installerat hello SDK.</span><span class="sxs-lookup"><span data-stu-id="bc88b-132">Close hello command prompt after you install hello SDK.</span></span>

    ```
    pip install azure-iothub-service-client
    ```

2. <span data-ttu-id="bc88b-133">Skapa en Python-fil med namnet **CreateDeviceIdentity.py**.</span><span class="sxs-lookup"><span data-stu-id="bc88b-133">Create a Python file named **CreateDeviceIdentity.py**.</span></span> <span data-ttu-id="bc88b-134">Öppna den i [en Python-redigeraren/IDE önskat][lnk-python-ide-list], till exempel hello standard [INAKTIVT][lnk-idle].</span><span class="sxs-lookup"><span data-stu-id="bc88b-134">Open it in [a Python editor/IDE of your choice][lnk-python-ide-list], for example, hello default [IDLE][lnk-idle].</span></span>

3. <span data-ttu-id="bc88b-135">Lägg till följande kodmoduler tooimport hello krävs från hello service SDK hello:</span><span class="sxs-lookup"><span data-stu-id="bc88b-135">Add hello following code tooimport hello required modules from hello service SDK:</span></span>

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. <span data-ttu-id="bc88b-136">Lägg till hello följande kod, ersätter hello platshållare för `[IoTHub Connection String]` med hello anslutningssträng för hello IoT-hubb som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="bc88b-136">Add hello following code, replacing hello placeholder for `[IoTHub Connection String]` with hello connection string for hello IoT hub you created in hello previous section.</span></span> <span data-ttu-id="bc88b-137">Du kan använda ett namn som hello `DEVICE_ID`.</span><span class="sxs-lookup"><span data-stu-id="bc88b-137">You can use any name as hello `DEVICE_ID`.</span></span>
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. <span data-ttu-id="bc88b-138">Lägg till hello följande funktion tooprint vissa hello enhetsinformation.</span><span class="sxs-lookup"><span data-stu-id="bc88b-138">Add hello following function tooprint some of hello device information.</span></span>

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
3. <span data-ttu-id="bc88b-139">Lägg till följande funktion toocreate hello enheten identifiering med hjälp av hello registret Manager hello.</span><span class="sxs-lookup"><span data-stu-id="bc88b-139">Add hello following function toocreate hello device identification using hello Registry Manager.</span></span> 

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
4. <span data-ttu-id="bc88b-140">Slutligen lägger du till hello huvudsakliga funktion på följande sätt och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="bc88b-140">Finally, add hello main function as follows and save hello file.</span></span>

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
5. <span data-ttu-id="bc88b-141">Hello Kommandotolken kör hello **CreateDeviceIdentity.py** på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bc88b-141">On hello command prompt, run hello **CreateDeviceIdentity.py** as follows:</span></span>

    ```python
    python CreateDeviceIdentity.py
    ```
6. <span data-ttu-id="bc88b-142">Du bör se hello simulerade enheten komma skapas.</span><span class="sxs-lookup"><span data-stu-id="bc88b-142">You should see hello simulated device getting created.</span></span> <span data-ttu-id="bc88b-143">Skriv ner hello **deviceId** och hello **primaryKey** för den här enheten.</span><span class="sxs-lookup"><span data-stu-id="bc88b-143">Note down hello **deviceId** and hello **primaryKey** of this device.</span></span> <span data-ttu-id="bc88b-144">Du måste dessa värden senare när du skapar ett program som ansluter tooIoT hubb som en enhet.</span><span class="sxs-lookup"><span data-stu-id="bc88b-144">You need these values later when you create an application that connects tooIoT Hub as a device.</span></span>

    ![Enheten har skapats][1]

> [!NOTE]
> <span data-ttu-id="bc88b-146">Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc88b-146">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="bc88b-147">Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet.</span><span class="sxs-lookup"><span data-stu-id="bc88b-147">It stores device IDs and keys toouse as security credentials and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="bc88b-148">Om ditt program måste toostore andra enhetsspecifika metadata, använder den en programspecifika butik.</span><span class="sxs-lookup"><span data-stu-id="bc88b-148">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="bc88b-149">Mer information finns i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="bc88b-149">For more information, see hello [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 


## <a name="create-a-simulated-device-app"></a><span data-ttu-id="bc88b-150">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="bc88b-150">Create a simulated device app</span></span>
<span data-ttu-id="bc88b-151">Det här avsnittet innehåller hello steg toocreate en Python-konsolapp som simulerar en enhet och skickar meddelanden från enhet till moln tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc88b-151">This section lists hello steps toocreate a Python console app, that simulates a device and sends device-to-cloud messages tooyour IoT hub.</span></span>

1. <span data-ttu-id="bc88b-152">Öppna en ny kommandotolk och installera hello Azure IoT Hub-enhet SDK för Python på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="bc88b-152">Open a new command prompt and install hello Azure IoT Hub Device SDK for Python as follows.</span></span> <span data-ttu-id="bc88b-153">Stäng kommandotolken hello efter hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="bc88b-153">Close hello command prompt after hello installation.</span></span>

    ```
    pip install azure-iothub-device-client
    ```
2. <span data-ttu-id="bc88b-154">Skapa en fil med namnet **SimulatedDevice.py**.</span><span class="sxs-lookup"><span data-stu-id="bc88b-154">Create a file named **SimulatedDevice.py**.</span></span> <span data-ttu-id="bc88b-155">Öppna filen i valfri Python-redigerare/IDE (till exempel IDLE).</span><span class="sxs-lookup"><span data-stu-id="bc88b-155">Open this file in a Python editor/IDE of your choice (for example, IDLE).</span></span>

3. <span data-ttu-id="bc88b-156">Lägg till hello följande kodmoduler tooimport hello krävs från hello enheten SDK.</span><span class="sxs-lookup"><span data-stu-id="bc88b-156">Add hello following code tooimport hello required modules from hello device SDK.</span></span>

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. <span data-ttu-id="bc88b-157">Lägg till hello följande kod och Ersätt hello platshållare för `[IoTHub Device Connection String]` med hello anslutningssträng för din enhet.</span><span class="sxs-lookup"><span data-stu-id="bc88b-157">Add hello following code and replace hello placeholder for `[IoTHub Device Connection String]` with hello connection string for your device.</span></span> <span data-ttu-id="bc88b-158">anslutningssträngen för hello enhet är vanligtvis i hello-format för `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`.</span><span class="sxs-lookup"><span data-stu-id="bc88b-158">hello device connection string is usually in hello format of `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`.</span></span> <span data-ttu-id="bc88b-159">Använd hello **deviceId** och **primaryKey** av hello-enhet som du skapade i hello föregående avsnitt tooreplace hello `<deviceId>` och `<primaryKey>` respektive.</span><span class="sxs-lookup"><span data-stu-id="bc88b-159">Use hello **deviceId** and **primaryKey** of hello device you created in hello previous section tooreplace hello `<deviceId>` and `<primaryKey>` respectively.</span></span> <span data-ttu-id="bc88b-160">Ersätt `<hostName>` med värdnamnet för IoT Hub, vanligtvis är det `<IoT hub name>.azure-devices.net`.</span><span class="sxs-lookup"><span data-stu-id="bc88b-160">Replace `<hostName>` with your IoT hub's host name, usually as `<IoT hub name>.azure-devices.net`.</span></span>

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
5. <span data-ttu-id="bc88b-161">Lägg till följande kod toodefine skicka bekräftelse återanrop hello.</span><span class="sxs-lookup"><span data-stu-id="bc88b-161">Add hello following code toodefine a send confirmation callback.</span></span> 

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
6. <span data-ttu-id="bc88b-162">Lägg till följande kod tooinitialize hello klientenheter hello.</span><span class="sxs-lookup"><span data-stu-id="bc88b-162">Add hello following code tooinitialize hello device client.</span></span>

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set hello time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. <span data-ttu-id="bc88b-163">Lägg till följande hello fungera tooformat och skicka ett meddelande från din simulerade enhet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc88b-163">Add hello following function tooformat and send a message from your simulated device tooyour IoT hub.</span></span>

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
8. <span data-ttu-id="bc88b-164">Slutligen lägger du till hello huvudsakliga funktion.</span><span class="sxs-lookup"><span data-stu-id="bc88b-164">Finally, add hello main function.</span></span> 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using hello Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. <span data-ttu-id="bc88b-165">Spara och Stäng hello **SimulatedDevice.py** fil.</span><span class="sxs-lookup"><span data-stu-id="bc88b-165">Save and close hello **SimulatedDevice.py** file.</span></span> <span data-ttu-id="bc88b-166">Du är nu redo toorun den här appen.</span><span class="sxs-lookup"><span data-stu-id="bc88b-166">You are now ready toorun this app.</span></span>

> [!NOTE]
> <span data-ttu-id="bc88b-167">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="bc88b-167">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="bc88b-168">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="bc88b-168">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a><span data-ttu-id="bc88b-169">Ta emot meddelanden från den simulerade enheten</span><span class="sxs-lookup"><span data-stu-id="bc88b-169">Receive messages from your simulated device</span></span>
<span data-ttu-id="bc88b-170">tooreceive telemetri meddelanden från enheten, behöver du toouse en [Händelsehubbar][lnk-event-hubs-overview]-kompatibel slutpunkt som exponeras av hello IoT-hubb som läser hello meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="bc88b-170">tooreceive telemetry messages from your device, you need toouse an [Event Hubs][lnk-event-hubs-overview]-compatible endpoint exposed by hello IoT Hub, which reads hello device-to-cloud messages.</span></span> <span data-ttu-id="bc88b-171">Läs hello [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] självstudiekursen information om hur tooprocess meddelanden från Händelsehubbar för slutpunkten för din IoT-hubb Event Hub-kompatibel.</span><span class="sxs-lookup"><span data-stu-id="bc88b-171">Read hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial for information on how tooprocess messages from Event Hubs for your IoT hub's Event Hub-compatible endpoint.</span></span> <span data-ttu-id="bc88b-172">Händelsehubbar har inte stöd för telemetri i Python ännu, så du kan antingen skapa en [Node.js](iot-hub-node-node-getstarted.md#D2C_node) eller en [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) Händelsehubbar-baserade konsolen tooread hello enhet till moln meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc88b-172">Event Hubs does not support telemetry in Python yet, so you can either create a [Node.js](iot-hub-node-node-getstarted.md#D2C_node) or a [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) Event Hubs-based console app tooread hello device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="bc88b-173">Den här kursen visar hur du kan använda hello [för IoT-hubb Explorer] [ lnk-iot-hub-explorer] tooread dessa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bc88b-173">This tutorial shows how you can use hello [IoT Hub Explorer tool][lnk-iot-hub-explorer] tooread these device messages.</span></span>

1. <span data-ttu-id="bc88b-174">Öppna en kommandotolk och installera hello IoT-hubb Explorer.</span><span class="sxs-lookup"><span data-stu-id="bc88b-174">Open a command prompt and install hello IoT Hub Explorer.</span></span> 

    ```
    npm install -g iothub-explorer
    ```

2. <span data-ttu-id="bc88b-175">Kör följande kommando i Kommandotolken för hello hello toobegin övervakning hello meddelanden från enhet till moln från enheten.</span><span class="sxs-lookup"><span data-stu-id="bc88b-175">Run hello following command on hello command prompt, toobegin monitoring hello device-to-cloud messages from your device.</span></span> <span data-ttu-id="bc88b-176">Använd din IoT-hubb anslutningssträngen i hello platshållare efter `--login`.</span><span class="sxs-lookup"><span data-stu-id="bc88b-176">Use your IoT hub's connection string in hello placeholder after `--login`.</span></span>

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. <span data-ttu-id="bc88b-177">Öppna en ny kommandotolk och gå toohello katalog som innehåller hello **SimulatedDevice.py** fil.</span><span class="sxs-lookup"><span data-stu-id="bc88b-177">Open a new command prompt and navigate toohello directory containing hello **SimulatedDevice.py** file.</span></span>

4. <span data-ttu-id="bc88b-178">Kör hello **SimulatedDevice.py** fil som skickar regelbundet telemetri data tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc88b-178">Run hello **SimulatedDevice.py** file, which periodically sends telemetry data tooyour IoT hub.</span></span> 
   
    ```
    python SimulatedDevice.py
    ```
5. <span data-ttu-id="bc88b-179">Se hälsningsmeddelande för enheten på hello Kommandotolken kör hello IoT-hubb Explorer från hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bc88b-179">Observe hello device messages on hello command prompt running hello IoT Hub Explorer from hello previous section.</span></span> 

    ![Meddelanden från enheten till molnet (Python)][2]

## <a name="next-steps"></a><span data-ttu-id="bc88b-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc88b-181">Next steps</span></span>
<span data-ttu-id="bc88b-182">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="bc88b-182">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="bc88b-183">Du har använt den här enhetens identitet tooenable hello simulerade enheten app toosend meddelanden från enhet till moln toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc88b-183">You used this device identity tooenable hello simulated device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="bc88b-184">Du har sett hello som tagits emot av hello IoT-hubb med hjälp av hello hello IoT-hubb Explorer-verktyget.</span><span class="sxs-lookup"><span data-stu-id="bc88b-184">You observed hello messages received by hello IoT hub with hello help of hello IoT Hub Explorer tool.</span></span> 

<span data-ttu-id="bc88b-185">tooexplore hello Python SDK för Azure IoT Hub-användning i djup, besök [den här hubb för Git-lagringsplatsen][lnk-python-github].</span><span class="sxs-lookup"><span data-stu-id="bc88b-185">tooexplore hello Python SDK for Azure IoT Hub usage in depth, visit [this Git Hub repo][lnk-python-github].</span></span> <span data-ttu-id="bc88b-186">tooreview hello meddelandefunktioner för hello Azure IoT-hubb Service SDK för Python, som du kan hämta och köra [iothub_messaging_sample.py][lnk-messaging-sample].</span><span class="sxs-lookup"><span data-stu-id="bc88b-186">tooreview hello messaging capabilities of hello Azure IoT Hub Service SDK for Python, you can download and run [iothub_messaging_sample.py][lnk-messaging-sample].</span></span> <span data-ttu-id="bc88b-187">För enheten på klientsidan simuleringen med hello Azure IoT Hub-enhet SDK för Python, som du kan hämta och köra hello [iothub_client_sample.py][lnk-client-sample].</span><span class="sxs-lookup"><span data-stu-id="bc88b-187">For device side simulation using hello Azure IoT Hub Device SDK for Python, you can download and run hello [iothub_client_sample.py][lnk-client-sample].</span></span>

<span data-ttu-id="bc88b-188">toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:</span><span class="sxs-lookup"><span data-stu-id="bc88b-188">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="bc88b-189">[Connecting your device][lnk-connect-device] (Ansluta din enhet)</span><span class="sxs-lookup"><span data-stu-id="bc88b-189">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="bc88b-190">[Connecting your device][lnk-device-management] (Komma igång med enhetshantering)</span><span class="sxs-lookup"><span data-stu-id="bc88b-190">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="bc88b-191">[Komma igång med Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="bc88b-191">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="bc88b-192">toolearn hur tooextend IoT-lösningen och processen enhet till moln meddelandena i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="bc88b-192">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
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
