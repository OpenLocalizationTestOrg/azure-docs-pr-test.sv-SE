---
title: "aaaAzure IoT-enhet SDK för C - IoTHubClient | Microsoft Docs"
description: "Hur toouse hello IoTHubClient bibliotek i hello Azure IoT-enhet SDK för C toocreate enhetsappar som kommunicerar med IoT-hubben."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: 828cf2bf-999d-4b8a-8a28-c7c901629600
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2016
ms.author: obloch
ms.openlocfilehash: d1ece79e9ba6d1e5fd45cabb8fca393b24052e07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Azure IoT-enhet SDK för C – mer information om IoTHubClient
Hej [först artikel](iot-hub-device-sdk-c-intro.md) i den här serien introduceras hello **Azure IoT-enhet SDK för C**. Artikeln förklaras att det finns två arkitektur lager i SDK. Grundläggande hello är hello **IoTHubClient** bibliotek som hanterar direkt kommunikation med IoT-hubb. Det finns också hello **serialiseraren** bibliotek som bygger som tooprovide serialisering tjänster. I den här artikeln ska vi ge ytterligare information om hello **IoTHubClient** bibliotek.

hello föregående artikel beskrivs hur toouse hello **IoTHubClient** biblioteket toosend händelser tooIoT hubb och ta emot meddelanden. Den här artikeln utökar den beskrivning som förklarar hur toomore exakt hantera *när* du skicka och ta emot data, introducerar toohello **lågnivå-API: er**. Vi förklarar också hur tooattach egenskaper tooevents (och hämta dem från meddelanden) med hjälp av hello egenskapen hantering av funktioner i hello **IoTHubClient** bibliotek. Slutligen vi att ge ytterligare förklaring av olika sätt toohandle meddelanden som tagits emot från IoT-hubb.

hello artikel avslutas genom som omfattar ett antal olika ämnen, t.ex. information om autentiseringsuppgifter för enheten och hur toochange hello beteendet för hello **IoTHubClient** via konfigurationsalternativ.

Vi använder hello **IoTHubClient** SDK exempel tooexplain dessa avsnitt. Om du vill toofollow längs finns hello **iothub\_klienten\_exempel\_http** och **iothub\_klienten\_exempel\_amqp** program som ingår i hello Azure IoT-enhet SDK för C. allt beskrivs i följande avsnitt hello visas i exemplen.

Du kan hitta hello [ **Azure IoT-enhet SDK för C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen och visa information om hello API i hello [C API-referens för](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="hello-lower-level-apis"></a>hello lågnivå-API: er
hello föregående artikel beskrivs hello grundläggande drift av hello **IotHubClient** inom hello kontext för hello **iothub\_klienten\_exempel\_amqp** programmet. Till exempel beskrivs hur tooinitialize hello bibliotek med den här koden.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Här beskrivs också hur funktionsanropet toosend händelser med hjälp av detta.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

hello artikeln beskrivs också hur tooreceive meddelanden genom att registrera en callback-funktion.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

hello artikel visade också hur toofree resurser med hjälp av kod som hello som följer.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Det finns dock tillhörande funktioner tooeach dessa API: er:

* IoTHubClient\_lla\_CreateFromConnectionString
* IoTHubClient\_lla\_SendEventAsync
* IoTHubClient\_lla\_SetMessageCallback
* IoTHubClient\_lla\_förstör

Dessa alla funktionerna inkluderar ”lla” i namnet för hello-API: N. Än att är hello parametrar för var och en av dessa funktioner identiska tootheir icke lla motsvarigheter. Dock skiljer hello av dessa funktioner sig på ett sätt som viktiga.

När du anropar **IoTHubClient\_CreateFromConnectionString**, hello underliggande bibliotek skapa en ny tråd som körs i bakgrunden hello. Den här tråden skickar händelser till och tar emot meddelanden från IoT-hubb. Ingen sådan tråd skapas när du arbetar med hello ”lla” API: er. hello skapa hello bakgrundstråd är bekvämlighet toohello utvecklare. Du har inte tooworry om att skicka händelser och ta emot meddelanden från IoT-hubb – det sker automatiskt i bakgrunden hello explicit. Däremot hello ”lla” API: er ger dig explicit kontroll över kommunikation med IoT-hubb, om det behövs.

toounderstand bättre, ska vi titta på ett exempel:

När du anropar **IoTHubClient\_SendEventAsync**, vad du faktiskt gör är att placera hello händelse i en buffert. Hej bakgrundstråd som skapas när du anropar **IoTHubClient\_CreateFromConnectionString** kontinuerligt övervakar bufferten och skickar data den innehåller tooIoT hubb. Detta sker i bakgrunden hello vid hello samma tid som hello huvudtråden utför annat arbete.

På liknande sätt, när du registrerar en Återanropsfunktionen för meddelanden med **IoTHubClient\_SetMessageCallback**, du instruerar hello SDK toohave hello bakgrund tråd anropa hello Återanropsfunktionen när ett meddelande är mottagna, oberoende av hello huvudtråden.

Hej ”lla” API: er skapa inte en bakgrundstråd. I stället ett nytt API måste anropas tooexplicitly skicka och ta emot data från IoT-hubb. Detta visas i följande exempel hello.

Hej **iothub\_klienten\_exempel\_http** program som ingår i hello SDK visar hello lågnivå-API: er. I det här exemplet skickar vi händelser tooIoT hubb med kod, till exempel hello följande:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

hello tre första raderna skapa hello-meddelande och hello sista raden skickar hello-händelse. Dock som tidigare nämnts skicka ”” hello händelse innebär att hello data bara placeras i en buffert. Ingenting överförs i nätverket hello när vi kallar **IoTHubClient\_lla\_SendEventAsync**. Du måste anropa i ordning tooactually ingång hello data tooIoT hubb **IoTHubClient\_lla\_DoWork**, som i det här exemplet:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Den här koden (från hello **iothub\_klienten\_exempel\_http** program) upprepade gånger **IoTHubClient\_lla\_DoWork** . Varje gång **IoTHubClient\_lla\_DoWork** är anropas den skickar vissa händelser från hello buffert tooIoT hubb och hämtar ett meddelande som skickas toohello enheten i kön. hello senare fallet innebär att om vi har registrerat ett Återanropsfunktionen för meddelanden, och sedan hello återanropet anropas (förutsatt att alla meddelanden i kö). Vi skulle har registrerat en Återanropsfunktionen med kod, till exempel hello följande:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

hello skäl som **IoTHubClient\_lla\_DoWork** kallas ofta en slinga är att varje gång den anropas, skickar *vissa* buffras händelser tooIoT hubb och hämtar *hello bredvid* meddelandet i kö för hello enhet. Varje anrop är inte garanterat toosend alla buffrade händelser eller tooretrieve alla köade meddelanden. Om du vill toosend alla händelser i hello bufferten och fortsätt sedan med annan bearbetning ersätta du den här loop med kod exempelvis hello följande:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Den här koden anropar **IoTHubClient\_lla\_DoWork** förrän alla händelser i hello buffert har skickats tooIoT hubb. Observera att detta inte också innebär att alla köade meddelanden har tagits emot. En del av hello anledningen är att kontrollera om ”alla” meddelanden inte deterministisk som en åtgärd. Vad händer om du hämtar ”alla” hello meddelanden, men en annan skickas sedan toohello enheten omedelbart efter? Ett bättre sätt toodeal med som är med programmerade timeout. Hello-meddelande Återanropsfunktionen kan återställa en timer varje gång den startas. Nu kan du skriva logik toocontinue bearbetning, till exempel inga meddelanden har tagits emot i hello senast *X* sekunder.

När du är klar ingressing händelser och ta emot meddelanden, vara säker på att toocall hello motsvarande funktionen tooclean resurser.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Det finns i princip bara en uppsättning API: er toosend och ta emot data med en bakgrundstråd och en annan uppsättning API: er som hello samma sak utan hello bakgrundstråd. Många utvecklare kan också hello icke - lla API: er, men hello lågnivå-API: er är användbara när hello utvecklare vill explicit kontroll över nätverksöverföringar. Till exempel vissa enheter samla in data över tid och endast ingångshändelser vid angivna intervall (till exempel när en timme eller en gång om dagen). Hej lågnivå-API: er ger du hello möjlighet tooexplicitly kontroll när du skickar och tar emot data från IoT-hubb. Andra kommer bara hellre hello enkelhet som hello lågnivå-API: er tillhandahåller. Allt som händer på hello huvudtråden i stället för vissa arbete sker i bakgrunden hello.

Oavsett vilken modell som du väljer vara säker på att toobe konsekvent i vilka API: er som du använder. Om du startar genom att anropa **IoTHubClient\_lla\_CreateFromConnectionString**, bör du bara använda hello motsvarande lågnivå-API: er för all uppföljning:

* IoTHubClient\_lla\_SendEventAsync
* IoTHubClient\_lla\_SetMessageCallback
* IoTHubClient\_lla\_förstör
* IoTHubClient\_lla\_DoWork

hello motsatt gäller också. Om du börjar med **IoTHubClient\_CreateFromConnectionString**, och sedan använda hello icke - lla API: er för några andra processer.

Hello Azure IoT-enhet SDK för C, se hello **iothub\_klienten\_exempel\_http** program för en komplett exempel på hello lågnivå-API: er. Hej **iothub\_klienten\_exempel\_amqp** program kan refereras till en fullständig exempel av hello icke - lla API: er.

## <a name="property-handling"></a>Egenskapen hantering
Hittills när vi har beskrivs skickar data, har vi hänvisar toohello brödtext hello-meddelande. Anta till exempel att den här koden:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Det här exemplet skickar ett meddelande tooIoT hubb med hello texten ”Hello World”. IoT-hubb kan också egenskaper toobe bifogade tooeach meddelande. Egenskaperna är namn/värde-par som kan vara anslutna toohello meddelande. Vi kan till exempel ändra hello föregående kod tooattach meddelandet egenskapen toohello:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Vi börjar genom att anropa **IoTHubMessage\_egenskaper** och passerar den hello referensen för våra meddelandet. Vad vi komma är en **KARTAN\_hantera** referens som gör att vi toostart att lägga till egenskaper. hello senare görs genom att anropa **kartan\_AddOrUpdate**, som tar en referens tooa KARTAN\_REFERENSEN hello egenskapsnamn och hello egenskapsvärde. Vi kan detta API för att lägga till alla egenskaper som vi gärna.

När hello händelse har lästs från **Händelsehubbar**, hello mottagaren kan räkna upp hello egenskaper och hämta motsvarande värden. Till exempel i .NET detta kan åstadkommas genom att öppna hello [egenskapssamlingen på hello EventData objektet](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

I föregående exempel hello kopplar vi egenskaper tooan händelse som vi skickar tooIoT hubb. Egenskaper kan också vara anslutna toomessages togs emot från IoT-hubb. Om vi vill tooretrieve egenskaper från ett meddelande kan vi använda koden som hello följande i vår Återanropsfunktionen meddelande:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from hello message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Hej anrop för**IoTHubMessage\_egenskaper** returnerar hello **KARTAN\_hantera** referens. Vi sedan skicka som referens för**kartan\_GetInternals** tooobtain ett referens tooan matris av hello namn/värde-par (samt antalet hello egenskaper). Då är det en enkel fråga om uppräkning hello tooget toohello egenskapsvärden vi vill.

Du har inte toouse egenskaper i ditt program. Men om du behöver tooset dem på händelser eller hämta dem från meddelanden, hello **IoTHubClient** biblioteket kan du enkelt.

## <a name="message-handling"></a>Meddelandehantering
Som nämnts tidigare anger när meddelanden tas emot från IoT-hubb hello **IoTHubClient** biblioteket svarar genom att aktivera registrerade Återanropsfunktionen. Det finns en returparameter av den här funktionen som ska ha vissa ytterligare förklaring. Här är ett utdrag ur hello Återanropsfunktionen i hello **iothub\_klienten\_exempel\_http** exempelprogrammet:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Observera att hello returtyp är **IOTHUBMESSAGE\_DISPOSITION\_resultatet** och i detta fall returnerar vi **IOTHUBMESSAGE\_godkända**. Det finns andra värden som vi kan returnera från den här funktionen att ändra hur hello **IoTHubClient** biblioteket reagerar toohello meddelande återanrop. Här följer hello alternativ.

* **IOTHUBMESSAGE\_godkända** – hello-meddelande har bearbetats. Hej **IoTHubClient** biblioteket kommer inte att anropa hello Återanropsfunktionen igen med hello samma meddelande.
* **IOTHUBMESSAGE\_Avvisad** – hello-meddelande kunde inte bearbetas och det finns inga önskan toodo hello i framtida. Hej **IoTHubClient** biblioteket ska inte anropa hello Återanropsfunktionen igen med hello samma meddelande.
* **IOTHUBMESSAGE\_ABANDONED** – hello-meddelande har inte bearbetats men hello **IoTHubClient** biblioteket ska anropa hello Återanropsfunktionen igen med hello samma meddelande.

För hello först två returkoder, hello **IoTHubClient** biblioteket skickar ett meddelande tooIoT hubb som anger att hello-meddelande bör tas bort från kön av hello enhet och inte levereras igen. hello net effekt är hello samma (hello-meddelande tas bort från kön för hello-enhet), men om hello-meddelande godkännas eller avvisas fortfarande har registrerats.  Registrera denna skillnad är användbara toosenders av hello-meddelande som kan lyssna efter feedback och ta reda på om en enhet har godkänt eller avvisade ett visst meddelande.

I hello senaste fall skickas även ett meddelande tooIoT Hub, men det anger att hello-meddelande som ska levereras. Du kommer normalt Avbryt ett meddelande om du påträffar ett fel men vill tootry tooprocess hello meddelandet igen. Däremot är avvisa meddelandet lämplig när det uppstår ett oåterkalleligt fel (eller om du bara väljer du inte vill tooprocess hello-meddelande).

Dock vara medveten om hello olika returkoder så att du kan framkalla hello beteende från hello **IoTHubClient** bibliotek.

## <a name="alternate-device-credentials"></a>Autentiseringsuppgifter för annan enhet
Som tidigare förklarats hello först öppna toodo när du arbetar med hello **IoTHubClient** biblioteket är tooobtain en **IOTHUB\_klienten\_hantera** med ett anrop till exempel hello följande:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Hej argument för**IoTHubClient\_CreateFromConnectionString** hello anslutningssträngen för enheten och en parameter som anger hello-protokollet som vi använder toocommunicate med IoT-hubben. anslutningssträngen för hello enheten har ett format som visas på följande sätt:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Det finns fyra typer av information i den här strängen: IoT-hubb namn, IoT-hubb suffix, enhets-ID och nyckeln för delad åtkomst. Du kan hämta hello fullständigt kvalificerade domännamnet (FQDN) för en IoT-hubb när du skapar din IoT hub-instans i hello Azure-portalen – detta ger dig hello IoT-hubbnamnet (hello första delen av hello FQDN) och hello IoT hub-suffix (hello övriga hello FQDN). Du får hello enhets-ID och hello delade åtkomstnyckeln när du registrerar din enhet med IoT-hubben (enligt beskrivningen i hello [föregående artikel](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** ger dig ett sätt tooinitialize hello-biblioteket. Om du vill kan du skapa en ny **IOTHUB\_klienten\_hantera** med hjälp av dessa enskilda parametrar i stället för hello anslutningssträngen för enheten. Detta uppnås med hello följande kod:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Detta åstadkommer hello samma sak som **IoTHubClient\_CreateFromConnectionString**.

Det kan verka uppenbara som du vill ha toouse **IoTHubClient\_CreateFromConnectionString** i stället för den här mer utförlig metoden för initiering. Kom ihåg att när du registrerar en enhet i IoT-hubb vad du får är en enhets-ID och nyckeln för enheten (inte en anslutningssträng). Hej *enheten explorer* SDK-verktyg som introducerades i hello [föregående artikel](iot-hub-device-sdk-c-intro.md) använder libraries i hello **Azure IoT service SDK** toocreate hello enheten anslutningssträng från hello enhets-ID, enhetsnyckel och IoT Hub-värdnamnet. Anropar så **IoTHubClient\_lla\_skapa** kan det vara bättre eftersom du sparar du hello steg genererar en anslutningssträng. Använd metoden som är lämplig.

## <a name="configuration-options"></a>Konfigurationsalternativ
Hittills allt beskrivs om hello sätt hello **IoTHubClient** biblioteket fungerar visar dess standardbeteendet. Det finns dock några alternativ som du kan ange toochange hur hello biblioteket fungerar. Detta görs genom att utnyttja hello **IoTHubClient\_lla\_SetOption** API. Överväg att det här exemplet:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Det finns ett par olika alternativ som är vanliga:

* **SetBatching** (booleskt) – om **SANT**, som sedan skickas data som skickas tooIoT hubb i batchar. Om **FALSKT**, och sedan skickas meddelanden individuellt. hello standardvärdet är **FALSKT**. Observera att hello **SetBatching** alternativet gäller bara toohello HTTP-protokollet och inte toohello MQTT eller AMQP-protokoll.
* **Timeout** (osignerade int) – det här värdet anges i millisekunder. Om du skickar en HTTP-begäran eller svar erhålls tar längre tid än den angivna tiden och sedan hello-anslutning.

hello batchbearbetning alternativet är viktigt. Som standard hello ingresses händelser individuellt (en händelse är allt du skickar för**IoTHubClient\_lla\_SendEventAsync**). Om hello batchbearbetning alternativet **SANT**, hello bibliotek samlar in så många händelser som går från hello bufferten (upp toohello maximal meddelandestorlek som accepterar IoT-hubb).  hello händelse batch skickas tooIoT hubb i en enda http-anrop (hello enskilda händelser slås ihop till en JSON-matris). Aktivera batchbearbetning vanligtvis resulterar i stora prestandavinster eftersom du är att minska nätverket turer. Det minskar också avsevärt bandbredd eftersom du skickar en uppsättning av HTTP-huvuden med en händelse batch i stället för en uppsättning huvuden för varje enskild händelse. Om du inte har en särskild anledning toodo annars, normalt ska du tooenable batchbearbetning.

## <a name="next-steps"></a>Nästa steg
Den här artikeln beskrivs i detalj hello funktionssätt hello **IoTHubClient** biblioteket hittades i hello **Azure IoT-enhet SDK för C**. Med den här informationen bör du ha en god förståelse av hello funktionerna i hello **IoTHubClient** bibliotek. Hej [nästa artikel](iot-hub-device-sdk-c-serializer.md) innehåller liknande information om hello **serialiseraren** bibliotek.

toolearn mer information om hur du utvecklar för IoT-hubb finns hello [Azure IoT SDK][lnk-sdks].

toofurther utforska hello funktionerna i IoT Hub, se:

* [Simulera en enhet med Azure IoT kant][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
