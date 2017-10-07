---
title: "aaaAzure IoT-enhet SDK för C - serialiseraren | Microsoft Docs"
description: "Hur toouse hello serialiseraren bibliotek i hello Azure IoT-enhet SDK för C toocreate enhetsappar som kommunicerar med IoT-hubben."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: defbed34-de73-429c-8592-cd863a38e4dd
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2016
ms.author: obloch
ms.openlocfilehash: c5776e9b50ffea71df96cb2d342ea2fc045f5a0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a>Azure IoT-enhet SDK för C – mer information om serialiseraren
Hej [först artikel](iot-hub-device-sdk-c-intro.md) i den här serien introduceras hello **Azure IoT-enhet SDK för C**. hello nästa artikel tillhandahålls en mer detaljerad beskrivning av hello [ **IoTHubClient** ](iot-hub-device-sdk-c-iothubclient.md). Den här artikeln har slutförts av hello SDK genom att tillhandahålla en mer detaljerad beskrivning av hello återstående komponenten: hello **serialiseraren** bibliotek.

hello inledande artikeln beskrivs hur toouse hello **serialiseraren** biblioteket toosend händelser tooand ta emot meddelanden från IoT-hubb. I den här artikeln vi utöka denna diskussion genom att tillhandahålla en fullständig förklaring av hur toomodel dina data med hello **serialiseraren** makrospråk. hello artikeln innehåller mer information om hur hello biblioteket Serialiserar meddelanden (och i vissa fall hur du kan styra hello serialisering). Vi kommer också att beskriva vissa parametrar som du kan ändra som bestämmer hello storleken på hello modeller som du skapar.

Slutligen revisits hello artikel vissa avsnitt som beskrivs i föregående artiklar, till exempel meddelande och egenskapen hantering. Som vi får veta, de fungerar i hello samma sätt med hjälp av hello **serialiseraren** bibliotek som med hello **IoTHubClient** bibliotek.

Allt i den här artikeln är baserad på hello **serialiseraren** SDK-exempel. Om du vill toofollow längs finns hello **simplesample\_amqp** och **simplesample\_http** program som ingår i hello Azure IoT-enhet SDK för C.

Du kan hitta hello [ **Azure IoT-enhet SDK för C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen och visa information om hello API i hello [C API-referens för](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="hello-modeling-language"></a>Hej modelleringsspråk
Hej [inledande artikel](iot-hub-device-sdk-c-intro.md) i den här serien introduceras hello **Azure IoT-enhet SDK för C** modeling language genom hello exempel i hello **simplesample\_amqp**  program:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Som du ser utifrån hello modeling language C makron. Du börjar din definition med alltid **BEGIN\_namnområde** och alltid sluta med **slutet\_namnområde**. Det är vanligt tooname hello namnområdet för ditt företag eller, som i det här exemplet hello-projekt som du arbetar med.

Vad som finns inuti hello namnområdet är definitioner för modellen. I det här fallet finns en modell för en anemometer. Återigen hello modell kan vara namn som helst, men detta är vanligtvis namnet för hello enhet eller en typ av data du vill tooexchange med IoT-hubben.  

Innehåller en definition av hello händelser kan du ingång tooIoT hubb (hello *data*) samt hälsningsmeddelande som du kan få från IoT-hubb (hello *åtgärder*). Som du ser i exemplet hello har händelser en typ och ett namn. åtgärder som har ett namn och valfria parametrar (var och en med en typ).

Vad visas inte i det här exemplet är ytterligare datatyper som stöds av hello SDK. Vi rapporterar nästa avsnitt.

> [!NOTE]
> IoT-hubb refererar toohello data som en enhet skickar tooit som *händelser*, medan hello modeling language refererar tooit som *data* (definieras med hjälp av **WITH_DATA**). På samma sätt IoT-hubb refererar toohello data som du skickar toodevices som *meddelanden*, medan hello modeling language refererar tooit som *åtgärder* (definieras med hjälp av **WITH_ACTION**). Tänk på att dessa villkor synonymt får användas i den här artikeln.
> 
> 

### <a name="supported-data-types"></a>Datatyper som stöds
hello följande datatyper stöds i modeller som skapats med hello **serialiseraren** bibliotek:

| Typ | Beskrivning |
| --- | --- |
| dubbla |dubbel precision flyttal |
| int |32-bitars heltal |
| flyttal |flyttal med enkel precision |
| lång |långt heltal |
| int8\_t |8-bitars heltal |
| Int16\_t |16-bitars heltal |
| Int32\_t |32-bitars heltal |
| Int64\_t |64-bitars heltal |
| bool |Booleskt värde |
| ASCII\_char\_ptr |ASCII-sträng |
| EDM\_DATUM\_TID\_FÖRSKJUTNING |tidsförskjutningen för datum |
| EDM\_GUID |GUID |
| EDM\_BINÄRA |Binär |
| DEKLARERA\_STRUKTUR |Komplex datatyp |

Vi börjar med hello senaste datatyp. Hej **DECLARE\_STRUCT** kan du toodefine komplexa datatyper, som är grupper med hello andra primitiva typer. Grupperingarna kan vi toodefine en modell som ser ut så här:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Vår modell innehåller en enda data händelse av typen **TestType**. **TestType** är en komplex typ som innehåller flera medlemmar som sammantagna visar hello primitiva typer som stöds av hello **serialiseraren** modeling language.

Med en modell som detta skriva vi kod toosend data tooIoT-hubb som visas på följande sätt:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Vi i princip, tilldela värdet tooevery tillhör hello **Test** struktur och sedan anropa **SendAsync** toosend hello **Test** data händelse toohello moln. **SendAsync** är en hjälpfunktion som skickar en enskild händelse tooIoT hubb:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate hello string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Den här funktionen Serialiserar hello data händelse och skickar den tooIoT hubb med **IoTHubClient\_SendEventAsync**. Det här är samma kod som beskrivs i föregående artiklar hello (**SendAsync** kapslar in hello logik i en lämplig funktion).

En andra hjälpfunktion som används i hello föregående kod är **GetDateTimeOffset**. Den här funktionen omvandlar hello angivna tid till ett värde av typen **EDM\_datum\_tid\_OFFSET**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Om du kör den här koden skickas tooIoT hubb med hello följande meddelande:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Observera att hello serialisering är JSON som är hello-format som genererats av hello **serialiseraren** bibliotek. Också Observera att varje medlem i hello serialiseras JSON-objekt matchar hello medlemmar i hello **TestType** som vi har definierat i vår modell. hello värdena matchar också exakt de som används i hello kod. Observera dock att hello binära data är base64-kodad: ”AQID” är hello base64-kodning av {0x01, 0x02, 0x03}.

Det här exemplet visar hello fördelen med att använda hello **serialiseraren** -biblioteket kan oss toosend JSON toohello moln, utan att behöva tooexplicitly behandlar serialisering i vårt program. Vi har tooworry om inställningsvärden hello av hello Datahändelser i vår modell och sedan anropa enkla API: er toosend dessa händelser toohello moln.

Med den här informationen kan vi definiera modeller som innehåller hello mängd stöds datatyper, inklusive komplexa typer (vi kan även inkludera komplexa typer i andra komplexa typer). Dock han serialiseras JSON som genereras av hello exemplet ovan öppnar en viktig aspekt. *Hur* vi skickar data med hello **serialiseraren** biblioteket anger exakt hur hello JSON format. Viss är vad vi rapporterar nästa.

## <a name="more-about-serialization"></a>Mer information om serialisering
hello föregående avsnitt visar ett exempel på hello utdata som genererats av hello **serialiseraren** bibliotek. I det här avsnittet förklarar vi hur hello biblioteket Serialiserar data och hur du kan styra beteendet med hello serialisering API: er.

I ordning tooadvance hello diskussion om serialisering arbetar vi med en ny modell utifrån en termostat. Först ska vi använda vissa bakgrund på hello scenario vi försöker tooaddress.

Vi vill toomodel en termostat som mäter temperatur- och fuktighetskonsekvens. Varje datadel kommer toobe skickas tooIoT hubb på olika sätt. Som standard hello termostat ingresses en temperatur händelse en gång var 2 minuter. en fuktighet händelse är ingressed var 15: e minut. När antingen händelsen ingressed det måste innehålla en tidsstämpel som visar hello tid att motsvarande temperatur hello eller fuktighet var mäts.

Anges i det här scenariot ska vi visa två olika sätt toomodel hello data och förklarar vi hello effekt att har modellering på hello serialiseras utdata.

### <a name="model-1"></a>Modell 1
Här är hello första versionen av en modell som stöder hello scenariot ovan:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Observera att hello modellen innehåller två Datahändelser: **temperatur** och **fuktighet**. Till skillnad från föregående exempel hello varje händelse är en struktur som definieras med hjälp av **DECLARE\_STRUCT**. **TemperatureEvent** innehåller ett mått för temperatur- och en tidsstämpel; **HumidityEvent** innehåller ett fuktighet mått och en tidsstämpel. Den här modellen ger oss en naturlig sätt toomodel hello data för hello-scenario som beskrivs ovan. När vi skickar ett händelse toohello moln skickar vi antingen en temperatur/tidsstämpel eller ett par fuktighet/timestamp.

Vi kan skicka ett temperatur händelse toohello moln med kod som hello följande:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Vi ska använda hårdkodade värden för temperatur- och fuktighetskonsekvens i hello exempelkod men anta att vi faktiskt hämtar dessa värden genom att ta prov hello motsvarande sensorer på hello termostat.

hello koden ovan använder hello **GetDateTimeOffset** helper som infördes tidigare. Skäl som ska bli Rensa senare skiljer den här koden uttryckligen hello uppgift att serialisering och skicka hello händelser. hello föregående kod Serialiserar hello temperatur händelse i en buffert. Sedan **sendMessage** är en hjälpfunktion (ingår i **simplesample\_amqp**) som skickar hello händelse tooIoT hubb:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Den här koden är en delmängd av hello **SendAsync** helper som beskrivs i föregående avsnitt med hello, så vi inte kommer gå över den igen.

När vi kör hello föregående kod toosend hello temperatur händelse skickas den här serialiserade formuläret hello händelse tooIoT hubb:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Vi skickar en temperatur som är av typen **TemperatureEvent** och att strukturen innehåller en **temperatur** och **tid** medlem. Detta återspeglas direkt i hello serialiserade data.

På liknande sätt kan vi skicka en fuktighet händelse med den här koden:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

hello serialiseras formulär som har skickats tooIoT hubb visas på följande sätt:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Detta är igen som förväntat.

Med den här modellen kan du föreställa dig hur ytterligare händelser kan enkelt läggas till. Du kan definiera flera strukturer med **DECLARE\_STRUCT**, och inkludera motsvarande hello-händelse i hello modellen med hjälp av **WITH\_DATA**.

Nu ska vi ändra hello modellen så att den inkluderar hello samma data, men med en annan struktur.

### <a name="model-2"></a>Modell 2
Överväg att den här alternativa modellen toohello en ovan:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

I det här fallet har vi tagit bort hello **DECLARE\_STRUCT** makron och bara definierar hello dataobjekt från vårt scenario med enkla typer från hello modeling language.

För tillfället hello vi Ignorera hello **tid** händelse. Här är hello kod tooingress med att reservera **temperatur**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Den här koden skickar hello följande serialiseras händelse tooIoT hubb:

```
{"Temperature":75}
```

Och hello kod för att skicka hello fuktighet händelse visas på följande sätt:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Den här koden skickar den här tooIoT hubb:

```
{"Humidity":45}
```

Så länge finns det inga överraskningar. Nu ska vi ändra hur vi kan använda hello SERIALISERA makro.

Hej **SERIALISERA** makrot kan ta flera Datahändelser som argument. Detta gör att vi tooserialize hello **temperatur** och **fuktighet** händelse tillsammans och skicka dem tooIoT hubb i ett anrop:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Du kanske tror att hello resultatet av den här koden är två händelser skickas tooIoT hubb:

[

{”Temperatur”: 75},

{”Fuktighet”: 45}

]

Med andra ord du tror att den här koden hello detsamma som att skicka **temperatur** och **fuktighet** separat. Det är bara en bekvämlighet toopass båda händelser för**SERIALISERA** i hello samma anropa. Men är som inte fallet hello. I stället skickar hello koden ovan denna enda data händelse tooIoT hubb:

{”Temperatur”: 75, ”fuktighet”: 45}

Det kan verka underligt eftersom vår modell definierar **temperatur** och **fuktighet** som två *separat* händelser:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Flera toohello punkt vi inte modellen dessa händelser där **temperatur** och **fuktighet** i hello samma struktur:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Om vi använde den här modellen är det inte enklare toounderstand hur **temperatur** och **fuktighet** skulle skickas i hello samma serialiserade meddelandet. Men det kanske inte rensa anledningen till att den fungerar på så sätt när du skickar Datahändelser för båda för**SERIALISERA** med hjälp av modellen 2.

Det här beteendet är enklare toounderstand om du vet hello antaganden som hello **serialiseraren** biblioteket gör. toomake uppfattning om detta går vi tillbaka tooour modellen:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Tänk på den här modellen i objektorienterad villkor. I det här fallet vi modellering en fysisk enhet (en termostat) och den enheten innehåller attribut som **temperatur** och **fuktighet**.

Vi kan skicka hello hela tillståndet för vår modell med kod, till exempel hello följande:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Vi skulle förutsatt hello värdena för temperatur, fuktighet och tid anges, för att se en händelse som den här skickade tooIoT hubb:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Ibland kan du bara vill toosend *vissa* egenskaperna för hello modellen toohello moln (detta är särskilt viktigt om modellen innehåller ett stort antal Datahändelser). Det är användbart toosend endast en delmängd av Datahändelser, som i det tidigare exemplet:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Detta genererar hello exakt samma serialiseras händelse som om vi har definierat en **TemperatureEvent** med en **temperatur** och **tid** medlem, precis som vi med modellen är 1. I det här fallet kunde kan toogenerate exakt hello samma serialiseras händelse med hjälp av en annan modell (modell 2) eftersom vi har ringt **SERIALISERA** på olika sätt.

hello viktiga är att om du skickar flera Datahändelser för**SERIALISERA,** sedan förutsätts varje händelse är en egenskap i ett enda JSON-objekt.

hello bästa sättet är beroende av du och hur du tycker om din modell. Om du skickar ”händelser” toohello molnet och varje händelse innehåller en definierad uppsättning egenskaper, gör hello första tillvägagångssättet mycket bra. I så fall kan du använda **DECLARE\_STRUCT** toodefine hello strukturen för varje händelse och inkludera dem i din modell med hello **WITH\_DATA** makro. Du skickar sedan varje händelse som vi gjorde i hello första exemplet ovan. I den här metoden som du skulle bara överföra en enskild händelse för**SERIALISERAREN**.

Om du tycker om din modell sätt objektorienterad kan sedan hello andra sättet passar dig. I det här fallet hello-element har definierats med **WITH\_DATA** är hello ”egenskaper” för objektet. Du kan skicka valfri del av händelser för**SERIALISERA** att du gillar, beroende på hur mycket av ditt ”objektets” tillstånd önskade toosend toohello moln.

Nether metod är rätt eller fel. Bara vara medvetna om hur hello **serialiseraren** biblioteket fungerar och välj hello modellering metod som bäst passar dina behov.

## <a name="message-handling"></a>Meddelandehantering
Hittills i den här artikeln har endast beskrivs skicka händelser tooIoT hubb och har inte åtgärdas ta emot meddelanden. Hej orsak till det här är som vi behöver tooknow ta emot meddelanden i stort sett har beskrivits i ett [tidigare artikel](iot-hub-device-sdk-c-intro.md). Återkalla från artikeln du bearbetar meddelanden genom att registrera en Återanropsfunktionen meddelande:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Du kan sedan skriva hello återanropsfunktion som anropas när ett meddelande tas emot:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable tooIoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed toomalloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
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

Den här implementeringen av **IoTHubMessage** anrop hello specifik funktion för varje åtgärd i modellen. Om till exempel din modell definierar den här åtgärden:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Du måste definiera en funktion med den här signaturen:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** sedan anropas när meddelandet skickas tooyour enhet.

Vilka hello serialiseras versionen för meddelande ser ut som om är vi inte har förklaras ännu. Med andra ord, om du vill toosend en **SetAirResistance** meddelandet tooyour enhet, vad att se ut?

Om du skickar ett meddelande tooa enhet kan göra du det via hello Azure IoT-tjänsten SDK. Du måste fortfarande tooknow vad string toosend tooinvoke en viss åtgärd. hello allmänna format för att skicka ett meddelande visas på följande sätt:

```
{"Name" : "", "Parameters" : "" }
```

Du skickar ett serialiserat JSON-objekt med två egenskaper: **namn** är hello namnet på hello-åtgärd (meddelande) och **parametrar** innehåller hello parametrar för åtgärden.

Till exempel tooinvoke **SetAirResistance** kan du skicka meddelandet tooa enheten:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

hello åtgärdsnamn måste exakt matcha en åtgärd som definierats i din modell. hello parameternamn måste matcha samt. Tänk också på skiftlägeskänslighet. **Namnet** och **parametrar** är alltid versaler. Se till att toomatch hello fallet åtgärdsnamn och parametrar i modellen. I det här exemplet är hello åtgärdsnamn ”SetAirResistance” och inte ”setairresistance”.

Hej två åtgärder **TurnFanOn** och **TurnFanOff** kan anropas genom att skicka dessa meddelanden tooa enhet:

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

Det här avsnittet beskrivs allt du behöver tooknow när händelser skickades och ta emot meddelanden med hello **serialiseraren** bibliotek. Innan du går vidare vi beskriver vissa parametrar som du kan konfigurera som styr hur stor din modell är.

## <a name="macro-configuration"></a>Makrot-konfiguration
Om du använder hello **serialiseraren** biblioteket som en viktig del av hello SDK toobe medveten om finns i biblioteket för hello azure-c-delad-verktyget.
Om du har klonas hello Azure-iot-sdk-c-databasen från GitHub med alternativet för hello--rekursiv hittar du här delade verktygsbiblioteket:

```
.\\c-utility
```

Om du inte har klona hello bibliotek, hittar du den [här](https://github.com/Azure/azure-c-shared-utility).

I ett bibliotek för hello delade verktyg hittar du hello följande mapp:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Den här mappen innehåller ett Visual Studio-lösning som kallas **makrot\_verktyg för webbplatsuppgradering\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

hello-programmet i den här lösningen genererar hello **makrot\_utils.h** fil. Det finns en standardmakrot\_utils.h-fil som ingår i hello SDK. Den här lösningen kan du toomodify vissa parametrar och sedan återskapa hello huvudfilen baserat på dessa parametrar.

hello två viktiga parametrar toobe bekymrad är **nArithmetic** och **nMacroParameters** som definierats i dessa två rader som hittades i makro\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Dessa värden är hello-standardparametrar som ingår i hello SDK. Varje parameter har hello enligt följande:

* nMacroParameters – styr hur många parametrar som du kan ha i en DECLARE\_makrodefinition i MODELLEN.
* nArithmetic – kontroller hello Totalt antal medlemmar som tillåts i en modell.

hello beror parametrarna är viktiga eftersom de styr hur stor din modell kan vara. Anta till exempel att den här modelldefinitionen:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Som nämnts tidigare **DECLARE\_MODELLEN** är bara ett C makro. Hej namnen på hello modellen och hello **WITH\_DATA** instruktionen (har ett annat makro) har parametrar av **DECLARE\_MODELLEN**. **nMacroParameters** definierar hur många parametrar kan ingå i **DECLARE\_MODELLEN**. Detta definierar effektivt, hur många data händelse och åtgärd deklarationer att du kan ha. Därför med hello Standardgränsen för 124 innebär detta att du kan definiera en modell med en kombination av om 60 åtgärder och Datahändelser. Om du försöker tooexceed denna gräns får Kompilatorfel som ser ut ungefär toothis:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

Hej **nArithmetic** parametern är mer om hello interna bearbetningen i hello makrospråk än ditt program.  Den styr hello Totalt antal medlemmar som du kan ha i din modell, inklusive **DECLARE_STRUCT** makron. Om du börjar Se Kompilatorfel sådana här kommer bör du öka **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Om du vill toochange parametrarna, ändra hello värden i hello makro\_utils.tt fil, recompile hello makrot\_verktyg för webbplatsuppgradering\_h\_generator.sln lösningen och kör hello kompilerat program. När du gör ett nytt makro\_utils.h fil skapas och placeras i hello.\\ vanliga\\inc directory.

I ordning toouse hello ny version av makrot\_utils.h, ta bort hello **serialiseraren** NuGet-paketet från din lösning och i dess ställe inkluderar hello **serialiseraren** Visual Studio-projekt. Detta gör att din kod toocompile mot hello källkoden för hello serialiseraren bibliotek. Detta inkluderar hello uppdateras makrot\_utils.h. Om du vill toodo för **simplesample\_amqp**, starta genom att ta bort hello NuGet-paket för hello serialiseraren bibliotek från hello lösning:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Lägg sedan till det här projektet tooyour Visual Studio-lösning:

> . \\c\\serialiseraren\\skapa\\windows\\serializer.vcxproj
> 
> 

När du är klar bör din lösning se ut så här:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Nu när du kompilerar din lösning hello uppdateras makrot\_utils.h ingår i din binary.

Observera att öka dessa värden som är tillräckligt högt kan överstiga kompileraren gränser. toothis pekar, hello **nMacroParameters** är hello huvudsakliga parameter med vilka berörda toobe. Hej C99-specifikationen anger att minst 127 parametrar tillåts i en makrodefinition. hello Microsoft-kompilatorn följer hello spec exakt (och är begränsad till 127), så att du inte kan tooincrease **nMacroParameters** utöver hello standard. Andra kompilerare kan tillåta dig toodo så (till exempel hello GNU kompilatorn stöder en högre gräns).

Hittills har vi omfattas nästan allt du behöver tooknow om hur toowrite code med hello **serialiseraren** bibliotek. Innan du att vi Ångra vissa avsnitt från föregående artiklar som du kanske undrar om.

## <a name="hello-lower-level-apis"></a>hello lågnivå-API: er
hello exempelprogram som den här artikeln fokuserar är **simplesample\_amqp**. Det här exemplet använder hello på högre nivå (hello icke-”lla”) API: er toosend händelser och ta emot meddelanden. Om du använder dessa API: er körs en bakgrundstråd som tar hand om både skicka händelser och ta emot meddelanden. Du kan dock använda hello på lägre nivå (alla) API: er tooeliminate denna bakgrundstråd och dra explicit kontroll över när du skickar händelser eller ta emot meddelanden från hello molnet.

Enligt beskrivningen i en [föregående artikel](iot-hub-device-sdk-c-iothubclient.md), det finns en uppsättning funktioner som består av hello på högre nivå API: er:

* IoTHubClient\_CreateFromConnectionString
* IoTHubClient\_SendEventAsync
* IoTHubClient\_SetMessageCallback
* IoTHubClient\_förstör

Dessa API: er visas i **simplesample\_amqp**.

Det finns också en liknande uppsättning API: er på lägre nivå.

* IoTHubClient\_lla\_CreateFromConnectionString
* IoTHubClient\_lla\_SendEventAsync
* IoTHubClient\_lla\_SetMessageCallback
* IoTHubClient\_lla\_förstör

Observera att hello lågnivå-API: er arbete exakt hello samma sätt som beskrivs i föregående hello-artiklar. Du kan använda hello första uppsättning API: er om du vill att en bakgrund tråd toohandle skicka händelser och ta emot meddelanden. Hello andra uppsättning API: er använder du om du vill ha explicit kontroll över när du skickar och tar emot data från IoT-hubb. Antingen uppsättning API: er fungerar lika bra med hello **serialiseraren** bibliotek.

Ett exempel på hur hello lågnivå-API: er används med hello **serialiseraren** biblioteket finns hello **simplesample\_http** program.

## <a name="additional-topics"></a>Ytterligare information
Några andra avsnitt värt att nämna igen är egenskapen hantering, med autentiseringsuppgifter för en annan enhet och konfigurationsalternativ. Dessa är alla avsnitt som beskrivs i en [föregående artikel](iot-hub-device-sdk-c-iothubclient.md). hello utgångspunkten är att alla dessa funktioner fungerar i hello samma sätt med hello **serialiseraren** bibliotek som med hello **IoTHubClient** bibliotek. Till exempel om du vill tooattach egenskaper tooan händelse från din modell kan du använda **IoTHubMessage\_egenskaper** och **kartan**\_**AddorUpdate**, hello samma sätt som beskrivs ovan:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Om hello händelsen skapades från hello **serialiseraren** bibliotek eller skapas manuellt med hjälp av hello **IoTHubClient** biblioteket spelar ingen roll.

Alternativ för hello enheten autentiseringsuppgifterna, med hjälp av **IoTHubClient\_lla\_skapa** fungerar lika bra som **IoTHubClient\_CreateFromConnectionString** för allokera en **IOTHUB\_klienten\_hantera**.

Slutligen, om du använder hello **serialiseraren** bibliotek, kan du ange konfigurationsalternativ med **IoTHubClient\_lla\_SetOption** precis som du gjorde när du använder hello **IoTHubClient** bibliotek.

En funktion som är unik toohello **serialiseraren** biblioteket är hello initieringen API: er. Innan du kan börja arbeta med hello bibliotek, måste du anropa **serialiseraren\_init**:

```
serializer_init(NULL);
```

Detta görs precis innan du anropar **IoTHubClient\_CreateFromConnectionString**.

På liknande sätt, när du är klar arbeta med hello biblioteket hello senaste anropet ska du göra är för**serialiseraren\_deinit**:

```
serializer_deinit();
```

Annars alla hello andra funktioner som anges ovan fungerar hello samma i hello **serialiseraren** biblioteket som i hello **IoTHubClient** bibliotek. Mer information om något av följande avsnitt finns hello [föregående artikel](iot-hub-device-sdk-c-iothubclient.md) i den här serien.

## <a name="next-steps"></a>Nästa steg
Den här artikeln beskrivs i detalj hello unika aspekter av hello **serialiseraren** bibliotek i hello **Azure IoT-enhet SDK för C**. Med hello informationen bör du ha en god förståelse av hur toouse modeller toosend händelser och ta emot meddelanden från IoT-hubb.

Detta avslutar också hello serie i tre delar på hur toodevelop program med hello **Azure IoT-enhet SDK för C**. Detta bör vara tillräckligt med information toonot endast get du startade men ger dig goda kunskaper om hur hello API: er fungerar. För ytterligare information finns det några exempel i hello SDK inte som beskrivs här. Annars hello [SDK-dokumentationen](https://github.com/Azure/azure-iot-sdk-c) är en bra resurs för ytterligare information.

toolearn mer information om hur du utvecklar för IoT-hubb finns hello [Azure IoT SDK][lnk-sdks].

toofurther utforska hello funktionerna i IoT Hub, se:

* [Simulera en enhet med Azure IoT kant][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
