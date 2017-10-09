---
title: "aaaThe Azure IoT-enhet SDK för C | Microsoft Docs"
description: "Komma igång med hello Azure IoT-enhet SDK för C och lära dig hur toocreate appar för enheter som kommunicerar med IoT-hubben."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: e448b061-6bdd-470a-a527-15ec03cca7b9
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: obloch
ms.openlocfilehash: 9e20742e6ea513c124bfaf28f02f6fba86170daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c"></a>Azure IoT-enhet SDK för C

Hej **Azure IoT-enhet SDK** är en uppsättning bibliotek utformats toosimplify hello för att skicka meddelanden tooand ta emot meddelanden från hello **Azure IoT Hub** service. Det finns olika varianter av hello SDK för riktad en viss plattform, men den här artikeln beskriver hello **Azure IoT-enhet SDK för C**.

hello Azure IoT-enhet SDK för C sparas i ANSI C (C99) toomaximize portability. Den här funktionen gör hello bibliotek väl lämpade toooperate på flera plattformar och enheter, särskilt om minimera disk och minneskrav är en prioritet.

Det finns en mängd olika plattformar på vilka hello SDK har testats (se hello [Azure certifierad för IoT-enhet katalogen](https://catalog.azureiotsuite.com/) information). Även om den här artikeln innehåller genomgångar av exempelkod som körs på Windows hello-plattformen, är hello koden beskrivs i den här artikeln identiska över hello mängd plattformar som stöds.

Den här artikeln ger en introduktion hello Azure IoT-enhet SDK toohello arkitektur för C. Den visar hur tooinitialize hello enheten biblioteket skicka data tooIoT hubb och ta emot meddelanden från den. hello informationen i den här artikeln bör vara tillräckligt med tooget igång med hello SDK, men innehåller även pekare tooadditional information om hello-bibliotek.

## <a name="sdk-architecture"></a>SDK-arkitektur

Du kan hitta hello [ **Azure IoT-enhet SDK för C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen och visa information om hello API i hello [C API-referens för](https://azure.github.io/azure-iot-sdk-c/index.html).

hello senaste versionen av hello-bibliotek finns i hello **master** grenen av hello databasen:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

* hello core implementering av hello SDK är i hello **iothub\_klienten** mapp som innehåller hello implementering av hello lägsta API-lagret i hello SDK: hello **IoTHubClient** bibliotek. Hej **IoTHubClient** biblioteket innehåller API: er som implementerar raw-meddelanden för att skicka meddelanden tooIoT hubb och ta emot meddelanden från IoT-hubb. När du använder det här biblioteket kan du ansvarar för att implementera meddelandet serialisering, men andra detaljer för kommunikation med IoT-hubb hanteras åt dig.
* Hej **serialiseraren** mappen innehåller hjälpfunktioner och exempel som visar hur tooserialize data innan du skickar tooAzure IoT-hubb med hello klientbiblioteket. hello användning av hello serialiseraren är inte obligatoriskt och tillhandahålls bekvämlighets skull. toouse hello **serialiseraren** bibliotek, definierar du en modell som anger data toosend tooIoT NAV- och hello hälsningsmeddelande du förväntar dig tooreceive från den. När hello modellen har definierats hello SDK ger dig en API-yta som gör att du tooeasily arbete med enhet till moln och moln till enhet meddelanden utan att oroa hello serialisering information. hello-biblioteket är beroende av andra bibliotek med öppen källkod som implementerar transport med hjälp av protokoll som MQTT och AMQP.
* Hej **IoTHubClient** biblioteket är beroende av andra bibliotek med öppen källkod:
  * Hej [Azure C delade verktyget](https://github.com/Azure/azure-c-shared-utility) biblioteket, som innehåller vanliga funktioner för grundläggande uppgifter (till exempel strängar, lista bearbetning och -i/o) som krävs för över flera Azure-relaterade C SDK: er.
  * Hej [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) biblioteket, som är en implementering för klientsidan av AMQP som är optimerade för resursen begränsad enheter.
  * Hej [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) biblioteket, vilket är ett allmänna implementering hello MQTT protokollet-bibliotek och optimeras för resursen begränsad enheter.

Användning av dessa bibliotek är enklare toounderstand genom att titta på exempelkod. hello följande avsnitt vägleder dig genom flera hello exempelprogram som ingår i hello SDK. Den här genomgången bör ge dig en bra anser för hello olika funktioner i hello arkitektur lager med hello SDK och en introduktion toohow hello-API: er fungerar.

## <a name="before-you-run-hello-samples"></a>Innan du kör hello-exempel

Innan du kan köra hello prover i hello Azure IoT-enhet SDK för C, måste du [skapa en instans av hello IoT-hubb service](iot-hub-create-through-portal.md) i din Azure-prenumeration. Slutför hello följande uppgifter:

* Förbereda utvecklingsmiljön
* Hämta autentiseringsuppgifter för enheten.

### <a name="prepare-your-development-environment"></a>Förbereda utvecklingsmiljön

Paket har angetts för vanliga plattformar (till exempel NuGet för Windows eller apt_get för Debian och Ubuntu) och hello exempel använder dessa paket när det är tillgängligt. I vissa fall behöver du toocompile hello SDK för eller på din enhet. Om du behöver toocompile hello SDK, se [förbereda din utvecklingsmiljö](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) i hello GitHub-lagringsplatsen.

tooobtain hello programmet exempelkod, hämta en kopia av hello SDK från GitHub. Hämta ditt exemplar av hello källa från hello **master** grenen av hello [GitHub-lagringsplatsen](https://github.com/Azure/azure-iot-sdk-c).


### <a name="obtain-hello-device-credentials"></a>Hämta hello enheten autentiseringsuppgifter

Nu när du har hello exempelkod för datakällan är hello nästa sak toodo tooget en uppsättning autentiseringsuppgifter för enheten. För en enhet toobe kan tooaccess en IoT-hubb, måste du först lägga till hello enheten toohello IoT-hubb identitetsregistret. När du lägger till en enhet kan du hämta en uppsättning autentiseringsuppgifter för enheten som du behöver för hello enheten toobe kan tooconnect toohello IoT-hubb. hello exempelprogram som beskrivs i nästa avsnitt om hello räknar autentiseringsuppgifterna i hello form av en **enheten anslutningssträngen**.

Det finns flera toohelp för öppen källkod verktyg du hanterar din IoT-hubb.

* Ett Windows-program som kallas [enheten explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).
* Ett plattformsoberoende node.js CLI-verktyg kallas [iothub explorer](https://github.com/azure/iothub-explorer).

Den här kursen använder hello grafiska *enheten explorer* verktyget. Du kan också använda hello *iothub explorer* verktyget om du föredrar toouse ett CLI-verktyg.

hello enheten explorer verktyget använder hello Azure IoT-tjänsten bibliotek tooperform olika funktioner på IoT Hub, inklusive lägga till enheter. Om du använder hello enheten explorer verktyget tooadd en enhet får du en anslutningssträng för enheten. Du behöver den här anslutningen sträng toorun hello exempelprogram.

Om du inte är bekant med hello enheten explorer verktyget hello följande procedur beskriver hur toouse den tooadd en enhet och få en anslutningssträng för enheten.

tooinstall hello enheten explorer verktyget, se [hur toouse hello enheten Explorer för IoT Hub-enheter](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).

När du kör programmet hello finns det här gränssnittet:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Ange din **IoT-hubb anslutningssträngen** i hello först fältet och klickar på **uppdatering**. Det här steget konfigurerar hello verktyget så att den kan kommunicera med IoT-hubb.

När hello IoT-hubb anslutningssträngen har konfigurerats klickar du på hello **Management** fliken:

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Den här fliken är där du hanterar hello enheter registrerade i din IoT-hubb.

Du skapar en enhet genom att klicka på hello **skapa** knappen. En dialogruta visas med en uppsättning i förväg nycklar (primär eller sekundär). Ange en **enhets-ID** och klicka sedan på **skapa**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

När hello enhet skapas listas hello enheter uppdateringar med alla hello registrerade enheter, inklusive hello som du nyss skapade. Om du högerklickar på den nya enheten visas den här menyn:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Om du väljer **kopiera anslutningssträngen för vald enhet**, hello enheten anslutningssträngen är kopierade toohello Urklipp. Spara en kopia av hello anslutningssträngen för enheten. Du behöver den när du kör hello exempelprogram som beskrivs i följande avsnitt hello.

När du har slutfört hello stegen ovan, är du redo toostart kör kod. Båda exempel har en konstant hello överst i hello huvudsakliga källfilen som möjliggör tooenter en anslutningssträng. Till exempel hello motsvarande rad från hello **iothub\_klienten\_exempel\_mqtt** programmet visas på följande sätt.

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-hello-iothubclient-library"></a>Använd hello IoTHubClient biblioteket

Inom hello **iothub\_klienten** mapp i hello [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) databasen, det finns en **exempel** mapp som innehåller ett program kallas **iothub\_klienten\_exempel\_mqtt**.

hello Windows-versionen av hello **iothub\_klienten\_exempel\_mqtt** program innehåller hello följande Visual Studio-lösning:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-mqtt.PNG)

> [!NOTE]
> Om du öppnar det här projektet i Visual Studio 2017 acceptera hello prompter tooretarget hello projektet toohello senaste versionen.

Den här lösningen innehåller ett projekt. Det finns fyra NuGet-paket i den här lösningen:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.umqtt

Du måste alltid hello **Microsoft.Azure.C.SharedUtility** när du arbetar med hello SDK-paketet. Det här exemplet använder hello MQTT protokoll, därför måste du inkludera hello **Microsoft.Azure.umqtt** och **Microsoft.Azure.IoTHub.MqttTransport** (det finns motsvarande paket för AMQP och HTTP-paket ). Eftersom hello används hello **IoTHubClient** bibliotek, måste du också inkludera hello **Microsoft.Azure.IoTHub.IoTHubClient** paketet i din lösning.

Du hittar hello implementering för hello exempelprogrammet i hello **iothub\_klienten\_exempel\_mqtt.c** källfilen.

hello följande använder det här exemplet programmet toowalk du vad krävs toouse hello **IoTHubClient** bibliotek.

### <a name="initialize-hello-library"></a>Initiera hello-bibliotek

> [!NOTE]
> Innan du börjar arbeta med hello bibliotek måste kanske du måste tooperform vissa plattformsspecifika initiering. Om du tänker toouse AMQP Linux måste du initiera hello OpenSSL-bibliotek. Hej prover i hello [GitHub-lagringsplatsen](https://github.com/Azure/azure-iot-sdk-c) anropa hello verktygsfunktionen **plattform\_init** när hello klienten startar och anropa hello **plattform\_deinit**  funktionen innan du avslutar. Dessa funktioner är deklarerad i hello platform.h huvudfilen. Granska hello definitioner av dessa funktioner för målplattformen i hello [databasen](https://github.com/Azure/azure-iot-sdk-c) toodetermine om du behöver tooinclude någon plattformsspecifika initieringskod i din klient.

Arbeta med hello bibliotek toostart allokera först en referens för IoT-hubb-klienten:

```c
if ((iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

Du skickar en kopia av hello enheten anslutningssträngen som du fick från hello explorer verktyget toothis enhetsfunktion. Du kan också ange hello kommunikation protokollet toouse. Det här exemplet används MQTT men AMQP och HTTP finns också alternativ.

När du har en giltig **IOTHUB\_klienten\_hantera**, du kan starta anropar hello-API: er toosend och ta emot meddelanden tooand från IoT-hubb.

### <a name="send-messages"></a>Skicka meddelanden

hello exempelprogrammet ställer in en loop toosend meddelanden tooyour IoT-hubb. Hej följande utdrag:

- Skapar ett meddelande.
- Lägger till en message-egenskap toohello.
- Skickar ett meddelande.

Skapa först ett meddelande:

```c
size_t iterator = 0;
do
{
    if (iterator < MESSAGE_COUNT)
    {
        sprintf_s(msgText, sizeof(msgText), "{\"deviceId\":\"myFirstDevice\",\"windSpeed\":%.2f}", avgWindSpeed + (rand() % 4 + 2));
        if ((messages[iterator].messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText))) == NULL)
        {
            (void)printf("ERROR: iotHubMessageHandle is NULL!\r\n");
        }
        else
        {
            messages[iterator].messageTrackingId = iterator;
            MAP_HANDLE propMap = IoTHubMessage_Properties(messages[iterator].messageHandle);
            (void)sprintf_s(propText, sizeof(propText), "PropMsg_%zu", iterator);
            if (Map_AddOrUpdate(propMap, "PropName", propText) != MAP_OK)
            {
                (void)printf("ERROR: Map_AddOrUpdate Failed!\r\n");
            }

            if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messages[iterator].messageHandle, SendConfirmationCallback, &messages[iterator]) != IOTHUB_CLIENT_OK)
            {
                (void)printf("ERROR: IoTHubClient_LL_SendEventAsync..........FAILED!\r\n");
            }
            else
            {
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission tooIoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

Varje gång du skickar ett meddelande kan ange du en referens tooa callback-funktion som anropas när hello data skickas. I det här exemplet hello Återanropsfunktionen kallas **SendConfirmationCallback**. hello följande fragment visar Återanropsfunktionen:

```c
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %zu with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Observera hello anropet toohello **IoTHubMessage\_förstöra** fungera när du är klar med hello-meddelande. Funktionen frigör hello resurser allokeras när du har skapat hello-meddelande.

### <a name="receive-messages"></a>Ta emot meddelanden

Ett meddelande är en asynkron åtgärd. Först registrera hello återanrop tooinvoke när hello enheten tar emot ett meddelande:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext) != IOTHUB_CLIENT_OK)
{
    (void)printf("ERROR: IoTHubClient_LL_SetMessageCallback..........FAILED!\r\n");
}
else
{
    (void)printf("IoTHubClient_LL_SetMessageCallback...successful.\r\n");
...
```

hello den sista parametern är en ogiltig pekare toowhatever som du vill använda. I exemplet hello är det en pekare tooan heltal, men det kan vara en pekare tooa mer komplexa datastruktur. Den här parametern aktiverar hello återanrop funktionen toooperate på delade tillståndet med hello anroparen av den här funktionen.

När hello enheten tar emot ett meddelande, anropas hello registrerade Återanropsfunktionen. Den här Återanropsfunktionen hämtar:

* hello-meddelande-id och Korrelations-id från hello-meddelande.
* hello meddelandeinnehåll.
* Eventuella anpassade egenskaper från hello-meddelande.

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    MAP_HANDLE mapProperties;
    const char* messageId;
    const char* correlationId;

    // Message properties
    if ((messageId = IoTHubMessage_GetMessageId(message)) == NULL)
    {
        messageId = "<null>";
    }

    if ((correlationId = IoTHubMessage_GetCorrelationId(message)) == NULL)
    {
        correlationId = "<null>";
    }

    // Message content
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        (void)printf("unable tooretrieve hello message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive hello work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from hello message
    mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                size_t index;

                printf(" Message Properties:\r\n");
                for (index = 0; index < propertyCount; index++)
                {
                    (void)printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                (void)printf("\r\n");
            }
        }
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Använd hello **IoTHubMessage\_GetByteArray** funktionen tooretrieve hello-meddelande, som i det här exemplet är en sträng.

### <a name="uninitialize-hello-library"></a>Uninitialize hello-bibliotek

När du är klar skickar händelser och ta emot meddelanden, kan du uninitialize hello IoT-biblioteket. toodo utfärda så hello följande funktionsanrop:

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Det här anropet Frigör hello resurser som tidigare har allokerats av hello **IoTHubClient\_CreateFromConnectionString** funktion.

Som du ser det är enkelt toosend och ta emot meddelanden med hello **IoTHubClient** bibliotek. hello biblioteket hanterar hello information om kommunikation med IoT-hubb, inklusive vilka protokoll toouse (från hello perspektiv hello utvecklare, detta är ett enkelt konfigurationsalternativ).

Hej **IoTHubClient** biblioteket innehåller också exakt kontroll över hur tooserialize hello data enheten skickar tooIoT hubb. I vissa fall denna behörighet är en fördel, men det är att du inte vill toobe bekymrad i andra. Om så är fallet hello kan du använda hello **serialiseraren** biblioteket, som beskrivs i nästa avsnitt om hello.

## <a name="use-hello-serializer-library"></a>Använd hello serialiserare-biblioteket

Begreppsmässigt hello **serialiseraren** biblioteket placeras ovanpå hello **IoTHubClient** bibliotek i hello SDK. Den använder hello **IoTHubClient** -biblioteket för kommunikation med IoT-hubb, men den underliggande hello lägger till modellering funktioner som tar bort hello belastningen för att behandla meddelande serialisering från hello-utvecklare. Hur framgår fungerar det här biblioteket bästa av ett exempel.

I hello **serialiseraren** mapp i hello [azure-iot-sdk-c databasen](https://github.com/Azure/azure-iot-sdk-c), är en **exempel** mapp som innehåller ett program kallas **simplesample \_mqtt**. hello Windows-versionen av det här exemplet innehåller hello följande Visual Studio-lösning:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_mqtt.PNG)

> [!NOTE]
> Om du öppnar det här projektet i Visual Studio 2017 acceptera hello prompter tooretarget hello projektet toohello senaste versionen.

Precis som med hello föregående exempel, innehåller den här flera NuGet-paket:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.IoTHub.Serializer
* Microsoft.Azure.umqtt

Du har sett de flesta av dessa paket i hello föregående exempel, men **Microsoft.Azure.IoTHub.Serializer** är ny. Det här paketet måste anges när du använder hello **serialiseraren** bibliotek.

Du hittar hello implementering av hello exempelprogrammet i hello **simplesample\_mqtt.c** fil.

hello vägleder följande avsnitt dig genom hello viktiga delar av det här exemplet.

### <a name="initialize-hello-library"></a>Initiera hello-bibliotek

Arbeta med hello toostart **serialiseraren** bibliotek, anrop hello initieringen API: er:

```c
if (serializer_init(NULL) != SERIALIZER_OK)
{
    (void)printf("Failed on serializer_init\r\n");
}
else
{
    IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol);
    srand((unsigned int)time(NULL));
    int avgWindSpeed = 10;

    if (iotHubClientHandle == NULL)
    {
        (void)printf("Failed on IoTHubClient_LL_Create\r\n");
    }
    else
    {
        ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
        if (myWeather == NULL)
        {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
        }
        else
        {
...
```

hello anropet toohello **serialiseraren\_init** funktionen är en enstaka anrop och initierar hello underliggande bibliotek. Anropa sedan hello **IoTHubClient\_lla\_CreateFromConnectionString** funktion, vilket är hello samma API som hello **IoTHubClient** exempel. Det här anropet anger anslutningssträngen enhet (det här anropet är också där du väljer hello-protokollet du vill toouse). Det här exemplet används MQTT som hello transport, men kan använda AMQP eller HTTP.

Slutligen anropa hello **skapa\_MODELLEN\_instans** funktion. **WeatherStation** hello namnområde för hello modellen och **ContosoAnemometer** är hello namnet på hello modell. När du har skapat hello modellen instans kan du använda den toostart skicka och ta emot meddelanden. Det är dock viktigt toounderstand vilka modellen.

### <a name="define-hello-model"></a>Definiera hello modellen

En modell i hello **serialiseraren** biblioteket definierar hälsningsmeddelande som enheten kan skicka tooIoT NAV- och hello meddelanden, som kallas *åtgärder* i hello modeling språk, vilket kan ta emot. Du definierar en modell med en uppsättning C makron som hello **simplesample\_mqtt** exempelprogrammet:

```c
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(int, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Hej **börjar\_namnområde** och **END\_namnområde** makron båda tar hello namnområdet för hello modellen som ett argument. Det förväntas att något mellan dessa makron är hello definition av din modell eller modeller och hello datastrukturer som använder hello modeller.

I det här exemplet är en modell kallas **ContosoAnemometer**. Den här modellen definierar två datadelar att enheten kan skicka tooIoT hubb: **DeviceId** och **vindhastigheten**. Den definierar även tre åtgärder (meddelanden) som kan ta emot din enhet: **TurnFanOn**, **TurnFanOff**, och **SetAirResistance**. Alla dataposter har en typ och varje åtgärd har ett namn (och eventuellt en uppsättning parametrar).

Definiera en API-yta som du kan använda toosend meddelanden tooIoT hubb och svara toomessages skickas toohello enheten hello data och åtgärder som definierats i hello modellen. Användning av den här modellen är bäst att förstå genom ett exempel.

### <a name="send-messages"></a>Skicka meddelanden

hello modellen definierar hello data som du kan skicka tooIoT hubb. I det här exemplet som innebär att en av hello två dataobjekt som definieras med hjälp av hello **WITH_DATA** makro. Det finns flera steg krävs toosend **DeviceId** och **vindhastigheten** värden tooan IoT-hubb. hello är först tooset hello data som du vill toosend:

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

hello modell som du angav tidigare kan du tooset hello värden genom att ange medlemmar i en **struct**. Därefter serialisera hello-meddelande som du vill toosend:

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed tooserialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

Den här koden Serialiserar hello enhet till moln tooa buffert (refereras av **mål**). hello koden anropar sedan hello **sendMessage** fungerar toosend hello meddelandet tooIoT hubb:

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable toocreate a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed toohand over hello message tooIoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted hello message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```


Hej andra toolast-parametern för **IoTHubClient\_lla\_SendEventAsync** är en referens tooa callback-funktion som anropas när hello data har skickat. Här är hello Återanropsfunktionen i hello exemplet:

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

hello andra parametern är en pekare toouser kontext. hello samma pekaren som skickades för**IoTHubClient\_lla\_SendEventAsync**. I det här fallet hello kontext är en enkel räknare, men det kan vara vad du vill.

Det är allt finns toosending meddelanden från enhet till moln. hello endast återstår toocover är hur tooreceive meddelanden.

### <a name="receive-messages"></a>Ta emot meddelanden

Ett meddelande fungerar på liknande sätt toohello arbetsmetoder meddelanden i hello **IoTHubClient** bibliotek. Först måste registrera du ett meddelande Återanropsfunktionen:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable tooIoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

Sedan kan skriva du hello återanropsfunktion som anropas när ett meddelande tas emot:

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable tooIoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed toomalloc\r\n");
            result = IOTHUBMESSAGE_ABANDONED;
        }
        else
        {
            (void)memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Den här koden är formaterad – det har hello samma för alla lösningar. Den här funktionen tar emot hello-meddelande och tar hand om vidarebefordring toohello önskad funktion via hello anrop för**kör\_kommandot**. hello-funktionen anropas beroende nu hello definition av hello åtgärder i din modell.

När du definierar en åtgärd i din modell kan du är obligatoriska tooimplement en funktion som anropas när enheten tar emot motsvarande hello-meddelande. Om till exempel din modell definierar den här åtgärden:

```c
WITH_ACTION(SetAirResistance, int, Position)
```

Definiera en funktion med den här signaturen:

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Observera hur hello funktionen hello namn matchar hello namnet på hello åtgärd i hello modellen och att hello hello-funktionen matchar hello parametrar angavs för hello-åtgärd. hello första parametern krävs alltid och innehåller en pekare toohello instans av din modell.

När hello enheten tar emot ett meddelande som matchar denna signatur, anropas motsvarande hello-funktionen. Därför utöver med tooinclude hello formaterad exempelkod från **IoTHubMessage**, ta emot meddelanden är bara en fråga för att definiera en enkel funktion för varje åtgärd som definierats i din modell.

### <a name="uninitialize-hello-library"></a>Uninitialize hello-bibliotek

När du är klar skickar data och ta emot meddelanden, kan du uninitialize hello IoT-biblioteket:

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Dessa tre funktioner justerar hello tre initieringen funktioner som beskrivs ovan. Anropar API: erna säkerställer du frigöra tidigare allokerade resurser.

## <a name="next-steps"></a>Nästa steg

Den här artikeln beskrivs hello grunderna i hello-biblioteken i hello **Azure IoT-enhet SDK för C**. Det medföljer tillräckligt med information toounderstand vad som ingår i hello SDK, dess arkitektur och hur tooget igång med hello Windows prover. hello nästa artikel fortsätter hello beskrivning av hello SDK genom att förklara [mer om hello IoTHubClient biblioteket](iot-hub-device-sdk-c-iothubclient.md).

toolearn mer information om hur du utvecklar för IoT-hubb finns hello [Azure IoT SDK][lnk-sdks].

toofurther utforska hello funktionerna i IoT Hub, se:

* [Simulera en enhet med Azure IoT kant][lnk-iotedge]

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
