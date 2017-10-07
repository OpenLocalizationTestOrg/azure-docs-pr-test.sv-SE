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
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a><span data-ttu-id="b2f7c-103">Azure IoT-enhet SDK för C – mer information om serialiseraren</span><span class="sxs-lookup"><span data-stu-id="b2f7c-103">Azure IoT device SDK for C – more about serializer</span></span>
<span data-ttu-id="b2f7c-104">Hej [först artikel](iot-hub-device-sdk-c-intro.md) i den här serien introduceras hello **Azure IoT-enhet SDK för C**. hello nästa artikel tillhandahålls en mer detaljerad beskrivning av hello [ **IoTHubClient** ](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-104">hello [first article](iot-hub-device-sdk-c-intro.md) in this series introduced hello **Azure IoT device SDK for C**. hello next article provided a more detailed description of hello [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="b2f7c-105">Den här artikeln har slutförts av hello SDK genom att tillhandahålla en mer detaljerad beskrivning av hello återstående komponenten: hello **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-105">This article completes coverage of hello SDK by providing a more detailed description of hello remaining component: hello **serializer** library.</span></span>

<span data-ttu-id="b2f7c-106">hello inledande artikeln beskrivs hur toouse hello **serialiseraren** biblioteket toosend händelser tooand ta emot meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-106">hello introductory article described how toouse hello **serializer** library toosend events tooand receive messages from IoT Hub.</span></span> <span data-ttu-id="b2f7c-107">I den här artikeln vi utöka denna diskussion genom att tillhandahålla en fullständig förklaring av hur toomodel dina data med hello **serialiseraren** makrospråk.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-107">In this article, we extend that discussion by providing a more complete explanation of how toomodel your data with hello **serializer** macro language.</span></span> <span data-ttu-id="b2f7c-108">hello artikeln innehåller mer information om hur hello biblioteket Serialiserar meddelanden (och i vissa fall hur du kan styra hello serialisering).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-108">hello article also includes more detail about how hello library serializes messages (and in some cases how you can control hello serialization behavior).</span></span> <span data-ttu-id="b2f7c-109">Vi kommer också att beskriva vissa parametrar som du kan ändra som bestämmer hello storleken på hello modeller som du skapar.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-109">We'll also describe some parameters you can modify that determine hello size of hello models you create.</span></span>

<span data-ttu-id="b2f7c-110">Slutligen revisits hello artikel vissa avsnitt som beskrivs i föregående artiklar, till exempel meddelande och egenskapen hantering.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-110">Finally, hello article revisits some topics covered in previous articles such as message and property handling.</span></span> <span data-ttu-id="b2f7c-111">Som vi får veta, de fungerar i hello samma sätt med hjälp av hello **serialiseraren** bibliotek som med hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-111">As we'll find out, those features work in hello same way using hello **serializer** library as they do with hello **IoTHubClient** library.</span></span>

<span data-ttu-id="b2f7c-112">Allt i den här artikeln är baserad på hello **serialiseraren** SDK-exempel.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-112">Everything described in this article is based on hello **serializer** SDK samples.</span></span> <span data-ttu-id="b2f7c-113">Om du vill toofollow längs finns hello **simplesample\_amqp** och **simplesample\_http** program som ingår i hello Azure IoT-enhet SDK för C.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-113">If you want toofollow along, see hello **simplesample\_amqp** and **simplesample\_http** applications included in hello Azure IoT device SDK for C.</span></span>

<span data-ttu-id="b2f7c-114">Du kan hitta hello [ **Azure IoT-enhet SDK för C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen och visa information om hello API i hello [C API-referens för](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-114">You can find hello [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of hello API in hello [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="hello-modeling-language"></a><span data-ttu-id="b2f7c-115">Hej modelleringsspråk</span><span class="sxs-lookup"><span data-stu-id="b2f7c-115">hello modeling language</span></span>
<span data-ttu-id="b2f7c-116">Hej [inledande artikel](iot-hub-device-sdk-c-intro.md) i den här serien introduceras hello **Azure IoT-enhet SDK för C** modeling language genom hello exempel i hello **simplesample\_amqp**  program:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-116">hello [introductory article](iot-hub-device-sdk-c-intro.md) in this series introduced hello **Azure IoT device SDK for C** modeling language through hello example provided in hello **simplesample\_amqp** application:</span></span>

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

<span data-ttu-id="b2f7c-117">Som du ser utifrån hello modeling language C makron.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-117">As you can see, hello modeling language is based on C macros.</span></span> <span data-ttu-id="b2f7c-118">Du börjar din definition med alltid **BEGIN\_namnområde** och alltid sluta med **slutet\_namnområde**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-118">You always begin your definition with **BEGIN\_NAMESPACE** and always end with **END\_NAMESPACE**.</span></span> <span data-ttu-id="b2f7c-119">Det är vanligt tooname hello namnområdet för ditt företag eller, som i det här exemplet hello-projekt som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-119">It's common tooname hello namespace for your company or, as in this example, hello project that you're working on.</span></span>

<span data-ttu-id="b2f7c-120">Vad som finns inuti hello namnområdet är definitioner för modellen.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-120">What goes inside hello namespace are model definitions.</span></span> <span data-ttu-id="b2f7c-121">I det här fallet finns en modell för en anemometer.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-121">In this case, there is a single model for an anemometer.</span></span> <span data-ttu-id="b2f7c-122">Återigen hello modell kan vara namn som helst, men detta är vanligtvis namnet för hello enhet eller en typ av data du vill tooexchange med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-122">Once again, hello model can be named anything, but typically this is named for hello device or type of data you want tooexchange with IoT Hub.</span></span>  

<span data-ttu-id="b2f7c-123">Innehåller en definition av hello händelser kan du ingång tooIoT hubb (hello *data*) samt hälsningsmeddelande som du kan få från IoT-hubb (hello *åtgärder*).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-123">Models contain a definition of hello events you can ingress tooIoT Hub (hello *data*) as well as hello messages you can receive from IoT Hub (hello *actions*).</span></span> <span data-ttu-id="b2f7c-124">Som du ser i exemplet hello har händelser en typ och ett namn. åtgärder som har ett namn och valfria parametrar (var och en med en typ).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-124">As you can see from hello example, events have a type and a name; actions have a name and optional parameters (each with a type).</span></span>

<span data-ttu-id="b2f7c-125">Vad visas inte i det här exemplet är ytterligare datatyper som stöds av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-125">What’s not demonstrated in this sample are additional data types that are supported by hello SDK.</span></span> <span data-ttu-id="b2f7c-126">Vi rapporterar nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-126">We'll cover that next.</span></span>

> [!NOTE]
> <span data-ttu-id="b2f7c-127">IoT-hubb refererar toohello data som en enhet skickar tooit som *händelser*, medan hello modeling language refererar tooit som *data* (definieras med hjälp av **WITH_DATA**).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-127">IoT Hub refers toohello data a device sends tooit as *events*, while hello modeling language refers tooit as *data* (defined using **WITH_DATA**).</span></span> <span data-ttu-id="b2f7c-128">På samma sätt IoT-hubb refererar toohello data som du skickar toodevices som *meddelanden*, medan hello modeling language refererar tooit som *åtgärder* (definieras med hjälp av **WITH_ACTION**).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-128">Likewise, IoT Hub refers toohello data you send toodevices as *messages*, while hello modeling language refers tooit as *actions* (defined using **WITH_ACTION**).</span></span> <span data-ttu-id="b2f7c-129">Tänk på att dessa villkor synonymt får användas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-129">Be aware that these terms may be used interchangeably in this article.</span></span>
> 
> 

### <a name="supported-data-types"></a><span data-ttu-id="b2f7c-130">Datatyper som stöds</span><span class="sxs-lookup"><span data-stu-id="b2f7c-130">Supported data types</span></span>
<span data-ttu-id="b2f7c-131">hello följande datatyper stöds i modeller som skapats med hello **serialiseraren** bibliotek:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-131">hello following data types are supported in models created with hello **serializer** library:</span></span>

| <span data-ttu-id="b2f7c-132">Typ</span><span class="sxs-lookup"><span data-stu-id="b2f7c-132">Type</span></span> | <span data-ttu-id="b2f7c-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b2f7c-133">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b2f7c-134">dubbla</span><span class="sxs-lookup"><span data-stu-id="b2f7c-134">double</span></span> |<span data-ttu-id="b2f7c-135">dubbel precision flyttal</span><span class="sxs-lookup"><span data-stu-id="b2f7c-135">double precision floating point number</span></span> |
| <span data-ttu-id="b2f7c-136">int</span><span class="sxs-lookup"><span data-stu-id="b2f7c-136">int</span></span> |<span data-ttu-id="b2f7c-137">32-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="b2f7c-137">32 bit integer</span></span> |
| <span data-ttu-id="b2f7c-138">flyttal</span><span class="sxs-lookup"><span data-stu-id="b2f7c-138">float</span></span> |<span data-ttu-id="b2f7c-139">flyttal med enkel precision</span><span class="sxs-lookup"><span data-stu-id="b2f7c-139">single precision floating point number</span></span> |
| <span data-ttu-id="b2f7c-140">lång</span><span class="sxs-lookup"><span data-stu-id="b2f7c-140">long</span></span> |<span data-ttu-id="b2f7c-141">långt heltal</span><span class="sxs-lookup"><span data-stu-id="b2f7c-141">long integer</span></span> |
| <span data-ttu-id="b2f7c-142">int8\_t</span><span class="sxs-lookup"><span data-stu-id="b2f7c-142">int8\_t</span></span> |<span data-ttu-id="b2f7c-143">8-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="b2f7c-143">8 bit integer</span></span> |
| <span data-ttu-id="b2f7c-144">Int16\_t</span><span class="sxs-lookup"><span data-stu-id="b2f7c-144">int16\_t</span></span> |<span data-ttu-id="b2f7c-145">16-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="b2f7c-145">16 bit integer</span></span> |
| <span data-ttu-id="b2f7c-146">Int32\_t</span><span class="sxs-lookup"><span data-stu-id="b2f7c-146">int32\_t</span></span> |<span data-ttu-id="b2f7c-147">32-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="b2f7c-147">32 bit integer</span></span> |
| <span data-ttu-id="b2f7c-148">Int64\_t</span><span class="sxs-lookup"><span data-stu-id="b2f7c-148">int64\_t</span></span> |<span data-ttu-id="b2f7c-149">64-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="b2f7c-149">64 bit integer</span></span> |
| <span data-ttu-id="b2f7c-150">bool</span><span class="sxs-lookup"><span data-stu-id="b2f7c-150">bool</span></span> |<span data-ttu-id="b2f7c-151">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="b2f7c-151">boolean</span></span> |
| <span data-ttu-id="b2f7c-152">ASCII\_char\_ptr</span><span class="sxs-lookup"><span data-stu-id="b2f7c-152">ascii\_char\_ptr</span></span> |<span data-ttu-id="b2f7c-153">ASCII-sträng</span><span class="sxs-lookup"><span data-stu-id="b2f7c-153">ASCII string</span></span> |
| <span data-ttu-id="b2f7c-154">EDM\_DATUM\_TID\_FÖRSKJUTNING</span><span class="sxs-lookup"><span data-stu-id="b2f7c-154">EDM\_DATE\_TIME\_OFFSET</span></span> |<span data-ttu-id="b2f7c-155">tidsförskjutningen för datum</span><span class="sxs-lookup"><span data-stu-id="b2f7c-155">date time offset</span></span> |
| <span data-ttu-id="b2f7c-156">EDM\_GUID</span><span class="sxs-lookup"><span data-stu-id="b2f7c-156">EDM\_GUID</span></span> |<span data-ttu-id="b2f7c-157">GUID</span><span class="sxs-lookup"><span data-stu-id="b2f7c-157">GUID</span></span> |
| <span data-ttu-id="b2f7c-158">EDM\_BINÄRA</span><span class="sxs-lookup"><span data-stu-id="b2f7c-158">EDM\_BINARY</span></span> |<span data-ttu-id="b2f7c-159">Binär</span><span class="sxs-lookup"><span data-stu-id="b2f7c-159">binary</span></span> |
| <span data-ttu-id="b2f7c-160">DEKLARERA\_STRUKTUR</span><span class="sxs-lookup"><span data-stu-id="b2f7c-160">DECLARE\_STRUCT</span></span> |<span data-ttu-id="b2f7c-161">Komplex datatyp</span><span class="sxs-lookup"><span data-stu-id="b2f7c-161">complex data type</span></span> |

<span data-ttu-id="b2f7c-162">Vi börjar med hello senaste datatyp.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-162">Let’s start with hello last data type.</span></span> <span data-ttu-id="b2f7c-163">Hej **DECLARE\_STRUCT** kan du toodefine komplexa datatyper, som är grupper med hello andra primitiva typer.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-163">hello **DECLARE\_STRUCT** allows you toodefine complex data types, which are groupings of hello other primitive types.</span></span> <span data-ttu-id="b2f7c-164">Grupperingarna kan vi toodefine en modell som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-164">These groupings allow us toodefine a model that looks like this:</span></span>

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

<span data-ttu-id="b2f7c-165">Vår modell innehåller en enda data händelse av typen **TestType**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-165">Our model contains a single data event of type **TestType**.</span></span> <span data-ttu-id="b2f7c-166">**TestType** är en komplex typ som innehåller flera medlemmar som sammantagna visar hello primitiva typer som stöds av hello **serialiseraren** modeling language.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-166">**TestType** is a complex type that includes several members, which collectively demonstrate hello primitive types supported by hello **serializer** modeling language.</span></span>

<span data-ttu-id="b2f7c-167">Med en modell som detta skriva vi kod toosend data tooIoT-hubb som visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-167">With a model like this, we can write code toosend data tooIoT Hub that appears as follows:</span></span>

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

<span data-ttu-id="b2f7c-168">Vi i princip, tilldela värdet tooevery tillhör hello **Test** struktur och sedan anropa **SendAsync** toosend hello **Test** data händelse toohello moln.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-168">Basically, we’re assigning a value tooevery member of hello **Test** structure and then calling **SendAsync** toosend hello **Test** data event toohello cloud.</span></span> <span data-ttu-id="b2f7c-169">**SendAsync** är en hjälpfunktion som skickar en enskild händelse tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-169">**SendAsync** is a helper function that sends a single data event tooIoT Hub:</span></span>

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

<span data-ttu-id="b2f7c-170">Den här funktionen Serialiserar hello data händelse och skickar den tooIoT hubb med **IoTHubClient\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-170">This function serializes hello given data event and sends it tooIoT Hub using **IoTHubClient\_SendEventAsync**.</span></span> <span data-ttu-id="b2f7c-171">Det här är samma kod som beskrivs i föregående artiklar hello (**SendAsync** kapslar in hello logik i en lämplig funktion).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-171">This is hello same code discussed in previous articles (**SendAsync** encapsulates hello logic into a convenient function).</span></span>

<span data-ttu-id="b2f7c-172">En andra hjälpfunktion som används i hello föregående kod är **GetDateTimeOffset**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-172">One other helper function used in hello previous code is **GetDateTimeOffset**.</span></span> <span data-ttu-id="b2f7c-173">Den här funktionen omvandlar hello angivna tid till ett värde av typen **EDM\_datum\_tid\_OFFSET**:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-173">This function transforms hello given time into a value of type **EDM\_DATE\_TIME\_OFFSET**:</span></span>

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

<span data-ttu-id="b2f7c-174">Om du kör den här koden skickas tooIoT hubb med hello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-174">If you run this code, hello following message is sent tooIoT Hub:</span></span>

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

<span data-ttu-id="b2f7c-175">Observera att hello serialisering är JSON som är hello-format som genererats av hello **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-175">Note that hello serialization is JSON, which is hello format generated by hello **serializer** library.</span></span> <span data-ttu-id="b2f7c-176">Också Observera att varje medlem i hello serialiseras JSON-objekt matchar hello medlemmar i hello **TestType** som vi har definierat i vår modell.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-176">Also note that each member of hello serialized JSON object matches hello members of hello **TestType** that we defined in our model.</span></span> <span data-ttu-id="b2f7c-177">hello värdena matchar också exakt de som används i hello kod.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-177">hello values also exactly match those used in hello code.</span></span> <span data-ttu-id="b2f7c-178">Observera dock att hello binära data är base64-kodad: ”AQID” är hello base64-kodning av {0x01, 0x02, 0x03}.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-178">However, note that hello binary data is base64-encoded: "AQID" is hello base64 encoding of {0x01, 0x02, 0x03}.</span></span>

<span data-ttu-id="b2f7c-179">Det här exemplet visar hello fördelen med att använda hello **serialiseraren** -biblioteket kan oss toosend JSON toohello moln, utan att behöva tooexplicitly behandlar serialisering i vårt program.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-179">This example demonstrates hello advantage of using hello **serializer** library -- it enables us toosend JSON toohello cloud, without having tooexplicitly deal with serialization in our application.</span></span> <span data-ttu-id="b2f7c-180">Vi har tooworry om inställningsvärden hello av hello Datahändelser i vår modell och sedan anropa enkla API: er toosend dessa händelser toohello moln.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-180">All we have tooworry about is setting hello values of hello data events in our model and then calling simple APIs toosend those events toohello cloud.</span></span>

<span data-ttu-id="b2f7c-181">Med den här informationen kan vi definiera modeller som innehåller hello mängd stöds datatyper, inklusive komplexa typer (vi kan även inkludera komplexa typer i andra komplexa typer).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-181">With this information, we can define models that include hello range of supported data types, including complex types (we could even include complex types within other complex types).</span></span> <span data-ttu-id="b2f7c-182">Dock han serialiseras JSON som genereras av hello exemplet ovan öppnar en viktig aspekt.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-182">However, he serialized JSON generated by hello example above brings up an important point.</span></span> <span data-ttu-id="b2f7c-183">*Hur* vi skickar data med hello **serialiseraren** biblioteket anger exakt hur hello JSON format.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-183">*How* we send data with hello **serializer** library determines exactly how hello JSON is formed.</span></span> <span data-ttu-id="b2f7c-184">Viss är vad vi rapporterar nästa.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-184">That particular point is what we'll cover next.</span></span>

## <a name="more-about-serialization"></a><span data-ttu-id="b2f7c-185">Mer information om serialisering</span><span class="sxs-lookup"><span data-stu-id="b2f7c-185">More about serialization</span></span>
<span data-ttu-id="b2f7c-186">hello föregående avsnitt visar ett exempel på hello utdata som genererats av hello **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-186">hello previous section highlights an example of hello output generated by hello **serializer** library.</span></span> <span data-ttu-id="b2f7c-187">I det här avsnittet förklarar vi hur hello biblioteket Serialiserar data och hur du kan styra beteendet med hello serialisering API: er.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-187">In this section, we'll explain how hello library serializes data and how you can control that behavior using hello serialization APIs.</span></span>

<span data-ttu-id="b2f7c-188">I ordning tooadvance hello diskussion om serialisering arbetar vi med en ny modell utifrån en termostat.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-188">In order tooadvance hello discussion on serialization, we'll work with a new model based on a thermostat.</span></span> <span data-ttu-id="b2f7c-189">Först ska vi använda vissa bakgrund på hello scenario vi försöker tooaddress.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-189">First, let's provide some background on hello scenario we're trying tooaddress.</span></span>

<span data-ttu-id="b2f7c-190">Vi vill toomodel en termostat som mäter temperatur- och fuktighetskonsekvens.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-190">We want toomodel a thermostat that measures temperature and humidity.</span></span> <span data-ttu-id="b2f7c-191">Varje datadel kommer toobe skickas tooIoT hubb på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-191">Each piece of data is going toobe sent tooIoT Hub differently.</span></span> <span data-ttu-id="b2f7c-192">Som standard hello termostat ingresses en temperatur händelse en gång var 2 minuter. en fuktighet händelse är ingressed var 15: e minut.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-192">By default, hello thermostat ingresses a temperature event once every 2 minutes; a humidity event is ingressed once every 15 minutes.</span></span> <span data-ttu-id="b2f7c-193">När antingen händelsen ingressed det måste innehålla en tidsstämpel som visar hello tid att motsvarande temperatur hello eller fuktighet var mäts.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-193">When either event is ingressed, it must include a timestamp that shows hello time that hello corresponding temperature or humidity was measured.</span></span>

<span data-ttu-id="b2f7c-194">Anges i det här scenariot ska vi visa två olika sätt toomodel hello data och förklarar vi hello effekt att har modellering på hello serialiseras utdata.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-194">Given this scenario, we'll demonstrate two different ways toomodel hello data, and we'll explain hello effect that modeling has on hello serialized output.</span></span>

### <a name="model-1"></a><span data-ttu-id="b2f7c-195">Modell 1</span><span class="sxs-lookup"><span data-stu-id="b2f7c-195">Model 1</span></span>
<span data-ttu-id="b2f7c-196">Här är hello första versionen av en modell som stöder hello scenariot ovan:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-196">Here's hello first version of a model that supports hello previous scenario:</span></span>

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

<span data-ttu-id="b2f7c-197">Observera att hello modellen innehåller två Datahändelser: **temperatur** och **fuktighet**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-197">Note that hello model includes two data events: **Temperature** and **Humidity**.</span></span> <span data-ttu-id="b2f7c-198">Till skillnad från föregående exempel hello varje händelse är en struktur som definieras med hjälp av **DECLARE\_STRUCT**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-198">Unlike previous examples, hello type of each event is a structure defined using **DECLARE\_STRUCT**.</span></span> <span data-ttu-id="b2f7c-199">**TemperatureEvent** innehåller ett mått för temperatur- och en tidsstämpel; **HumidityEvent** innehåller ett fuktighet mått och en tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-199">**TemperatureEvent** includes a temperature measurement and a timestamp; **HumidityEvent** contains a humidity measurement and a timestamp.</span></span> <span data-ttu-id="b2f7c-200">Den här modellen ger oss en naturlig sätt toomodel hello data för hello-scenario som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-200">This model gives us a natural way toomodel hello data for hello scenario described above.</span></span> <span data-ttu-id="b2f7c-201">När vi skickar ett händelse toohello moln skickar vi antingen en temperatur/tidsstämpel eller ett par fuktighet/timestamp.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-201">When we send an event toohello cloud, we'll either send a temperature/timestamp or a humidity/timestamp pair.</span></span>

<span data-ttu-id="b2f7c-202">Vi kan skicka ett temperatur händelse toohello moln med kod som hello följande:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-202">We can send a temperature event toohello cloud using code such as hello following:</span></span>

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

<span data-ttu-id="b2f7c-203">Vi ska använda hårdkodade värden för temperatur- och fuktighetskonsekvens i hello exempelkod men anta att vi faktiskt hämtar dessa värden genom att ta prov hello motsvarande sensorer på hello termostat.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-203">We'll use hard-coded values for temperature and humidity in hello sample code, but imagine that we’re actually retrieving these values by sampling hello corresponding sensors on hello thermostat.</span></span>

<span data-ttu-id="b2f7c-204">hello koden ovan använder hello **GetDateTimeOffset** helper som infördes tidigare.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-204">hello code above uses hello **GetDateTimeOffset** helper that was introduced previously.</span></span> <span data-ttu-id="b2f7c-205">Skäl som ska bli Rensa senare skiljer den här koden uttryckligen hello uppgift att serialisering och skicka hello händelser.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-205">For reasons that will become clear later, this code explicitly separates hello task of serializing and sending hello event.</span></span> <span data-ttu-id="b2f7c-206">hello föregående kod Serialiserar hello temperatur händelse i en buffert.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-206">hello previous code serializes hello temperature event into a buffer.</span></span> <span data-ttu-id="b2f7c-207">Sedan **sendMessage** är en hjälpfunktion (ingår i **simplesample\_amqp**) som skickar hello händelse tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-207">Then, **sendMessage** is a helper function (included in **simplesample\_amqp**) that sends hello event tooIoT Hub:</span></span>

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

<span data-ttu-id="b2f7c-208">Den här koden är en delmängd av hello **SendAsync** helper som beskrivs i föregående avsnitt med hello, så vi inte kommer gå över den igen.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-208">This code is a subset of hello **SendAsync** helper described in hello previous section, so we won’t go over it again here.</span></span>

<span data-ttu-id="b2f7c-209">När vi kör hello föregående kod toosend hello temperatur händelse skickas den här serialiserade formuläret hello händelse tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-209">When we run hello previous code toosend hello Temperature event, this serialized form of hello event is sent tooIoT Hub:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="b2f7c-210">Vi skickar en temperatur som är av typen **TemperatureEvent** och att strukturen innehåller en **temperatur** och **tid** medlem.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-210">We're sending a temperature which is of type **TemperatureEvent** and that struct contains a **Temperature** and **Time** member.</span></span> <span data-ttu-id="b2f7c-211">Detta återspeglas direkt i hello serialiserade data.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-211">This is directly reflected in hello serialized data.</span></span>

<span data-ttu-id="b2f7c-212">På liknande sätt kan vi skicka en fuktighet händelse med den här koden:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-212">Similarly, we can send a humidity event with this code:</span></span>

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="b2f7c-213">hello serialiseras formulär som har skickats tooIoT hubb visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-213">hello serialized form that’s sent tooIoT Hub appears as follows:</span></span>

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="b2f7c-214">Detta är igen som förväntat.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-214">Again, this is as expected.</span></span>

<span data-ttu-id="b2f7c-215">Med den här modellen kan du föreställa dig hur ytterligare händelser kan enkelt läggas till.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-215">With this model, you can imagine how additional events can easily be added.</span></span> <span data-ttu-id="b2f7c-216">Du kan definiera flera strukturer med **DECLARE\_STRUCT**, och inkludera motsvarande hello-händelse i hello modellen med hjälp av **WITH\_DATA**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-216">You define more structures using **DECLARE\_STRUCT**, and include hello corresponding event in hello model using **WITH\_DATA**.</span></span>

<span data-ttu-id="b2f7c-217">Nu ska vi ändra hello modellen så att den inkluderar hello samma data, men med en annan struktur.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-217">Now, let’s modify hello model so that it includes hello same data but with a different structure.</span></span>

### <a name="model-2"></a><span data-ttu-id="b2f7c-218">Modell 2</span><span class="sxs-lookup"><span data-stu-id="b2f7c-218">Model 2</span></span>
<span data-ttu-id="b2f7c-219">Överväg att den här alternativa modellen toohello en ovan:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-219">Consider this alternative model toohello one above:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="b2f7c-220">I det här fallet har vi tagit bort hello **DECLARE\_STRUCT** makron och bara definierar hello dataobjekt från vårt scenario med enkla typer från hello modeling language.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-220">In this case we've eliminated hello **DECLARE\_STRUCT** macros and are simply defining hello data items from our scenario using simple types from hello modeling language.</span></span>

<span data-ttu-id="b2f7c-221">För tillfället hello vi Ignorera hello **tid** händelse.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-221">Just for hello moment let’s ignore hello **Time** event.</span></span> <span data-ttu-id="b2f7c-222">Här är hello kod tooingress med att reservera **temperatur**:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-222">With that aside, here’s hello code tooingress **Temperature**:</span></span>

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

<span data-ttu-id="b2f7c-223">Den här koden skickar hello följande serialiseras händelse tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-223">This code sends hello following serialized event tooIoT Hub:</span></span>

```
{"Temperature":75}
```

<span data-ttu-id="b2f7c-224">Och hello kod för att skicka hello fuktighet händelse visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-224">And hello code for sending hello Humidity event appears as follows:</span></span>

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="b2f7c-225">Den här koden skickar den här tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-225">This code sends this tooIoT Hub:</span></span>

```
{"Humidity":45}
```

<span data-ttu-id="b2f7c-226">Så länge finns det inga överraskningar.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-226">So far there are still no surprises.</span></span> <span data-ttu-id="b2f7c-227">Nu ska vi ändra hur vi kan använda hello SERIALISERA makro.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-227">Now let's change how we use hello SERIALIZE macro.</span></span>

<span data-ttu-id="b2f7c-228">Hej **SERIALISERA** makrot kan ta flera Datahändelser som argument.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-228">hello **SERIALIZE** macro can take multiple data events as arguments.</span></span> <span data-ttu-id="b2f7c-229">Detta gör att vi tooserialize hello **temperatur** och **fuktighet** händelse tillsammans och skicka dem tooIoT hubb i ett anrop:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-229">This enables us tooserialize hello **Temperature** and **Humidity** event together and send them tooIoT Hub in one call:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="b2f7c-230">Du kanske tror att hello resultatet av den här koden är två händelser skickas tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-230">You might guess that hello result of this code is that two data events are sent tooIoT Hub:</span></span>

<span data-ttu-id="b2f7c-231">[</span><span class="sxs-lookup"><span data-stu-id="b2f7c-231">[</span></span>

<span data-ttu-id="b2f7c-232">{”Temperatur”: 75},</span><span class="sxs-lookup"><span data-stu-id="b2f7c-232">{"Temperature":75},</span></span>

<span data-ttu-id="b2f7c-233">{”Fuktighet”: 45}</span><span class="sxs-lookup"><span data-stu-id="b2f7c-233">{"Humidity":45}</span></span>

<span data-ttu-id="b2f7c-234">]</span><span class="sxs-lookup"><span data-stu-id="b2f7c-234">]</span></span>

<span data-ttu-id="b2f7c-235">Med andra ord du tror att den här koden hello detsamma som att skicka **temperatur** och **fuktighet** separat.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-235">In other words, you might expect that this code is hello same as sending **Temperature** and **Humidity** separately.</span></span> <span data-ttu-id="b2f7c-236">Det är bara en bekvämlighet toopass båda händelser för**SERIALISERA** i hello samma anropa.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-236">It’s just a convenience toopass both events too**SERIALIZE** in hello same call.</span></span> <span data-ttu-id="b2f7c-237">Men är som inte fallet hello.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-237">However, that’s not hello case.</span></span> <span data-ttu-id="b2f7c-238">I stället skickar hello koden ovan denna enda data händelse tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-238">Instead, hello code above sends this single data event tooIoT Hub:</span></span>

<span data-ttu-id="b2f7c-239">{”Temperatur”: 75, ”fuktighet”: 45}</span><span class="sxs-lookup"><span data-stu-id="b2f7c-239">{"Temperature":75, "Humidity":45}</span></span>

<span data-ttu-id="b2f7c-240">Det kan verka underligt eftersom vår modell definierar **temperatur** och **fuktighet** som två *separat* händelser:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-240">This may seem strange because our model defines **Temperature** and **Humidity** as two *separate* events:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="b2f7c-241">Flera toohello punkt vi inte modellen dessa händelser där **temperatur** och **fuktighet** i hello samma struktur:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-241">More toohello point, we didn’t model these events where **Temperature** and **Humidity** are in hello same structure:</span></span>

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

<span data-ttu-id="b2f7c-242">Om vi använde den här modellen är det inte enklare toounderstand hur **temperatur** och **fuktighet** skulle skickas i hello samma serialiserade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-242">If we used this model, it would be easier toounderstand how **Temperature** and **Humidity** would be sent in hello same serialized message.</span></span> <span data-ttu-id="b2f7c-243">Men det kanske inte rensa anledningen till att den fungerar på så sätt när du skickar Datahändelser för båda för**SERIALISERA** med hjälp av modellen 2.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-243">However it may not be clear why it works that way when you pass both data events too**SERIALIZE** using model 2.</span></span>

<span data-ttu-id="b2f7c-244">Det här beteendet är enklare toounderstand om du vet hello antaganden som hello **serialiseraren** biblioteket gör.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-244">This behavior is easier toounderstand if you know hello assumptions that hello **serializer** library is making.</span></span> <span data-ttu-id="b2f7c-245">toomake uppfattning om detta går vi tillbaka tooour modellen:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-245">toomake sense of this let’s go back tooour model:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="b2f7c-246">Tänk på den här modellen i objektorienterad villkor.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-246">Think of this model in object-oriented terms.</span></span> <span data-ttu-id="b2f7c-247">I det här fallet vi modellering en fysisk enhet (en termostat) och den enheten innehåller attribut som **temperatur** och **fuktighet**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-247">In this case we’re modeling a physical device (a thermostat) and that device includes attributes like **Temperature** and **Humidity**.</span></span>

<span data-ttu-id="b2f7c-248">Vi kan skicka hello hela tillståndet för vår modell med kod, till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-248">We can send hello entire state of our model with code such as hello following:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="b2f7c-249">Vi skulle förutsatt hello värdena för temperatur, fuktighet och tid anges, för att se en händelse som den här skickade tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-249">Assuming hello values of Temperature, Humidity and Time are set, we would see an event like this sent tooIoT Hub:</span></span>

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="b2f7c-250">Ibland kan du bara vill toosend *vissa* egenskaperna för hello modellen toohello moln (detta är särskilt viktigt om modellen innehåller ett stort antal Datahändelser).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-250">Sometimes you may only want toosend *some* properties of hello model toohello cloud (this is especially true if your model contains a large number of data events).</span></span> <span data-ttu-id="b2f7c-251">Det är användbart toosend endast en delmängd av Datahändelser, som i det tidigare exemplet:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-251">It’s useful toosend only a subset of data events, such as in our earlier example:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="b2f7c-252">Detta genererar hello exakt samma serialiseras händelse som om vi har definierat en **TemperatureEvent** med en **temperatur** och **tid** medlem, precis som vi med modellen är 1.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-252">This generates exactly hello same serialized event as if we had defined a **TemperatureEvent** with a **Temperature** and **Time** member, just as we did with model 1.</span></span> <span data-ttu-id="b2f7c-253">I det här fallet kunde kan toogenerate exakt hello samma serialiseras händelse med hjälp av en annan modell (modell 2) eftersom vi har ringt **SERIALISERA** på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-253">In this case we were able toogenerate exactly hello same serialized event by using a different model (model 2) because we called **SERIALIZE** in a different way.</span></span>

<span data-ttu-id="b2f7c-254">hello viktiga är att om du skickar flera Datahändelser för**SERIALISERA,** sedan förutsätts varje händelse är en egenskap i ett enda JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-254">hello important point is that if you pass multiple data events too**SERIALIZE,** then it assumes each event is a property in a single JSON object.</span></span>

<span data-ttu-id="b2f7c-255">hello bästa sättet är beroende av du och hur du tycker om din modell.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-255">hello best approach depends on you and how you think about your model.</span></span> <span data-ttu-id="b2f7c-256">Om du skickar ”händelser” toohello molnet och varje händelse innehåller en definierad uppsättning egenskaper, gör hello första tillvägagångssättet mycket bra.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-256">If you’re sending "events" toohello cloud and each event contains a defined set of properties, then hello first approach makes a lot of sense.</span></span> <span data-ttu-id="b2f7c-257">I så fall kan du använda **DECLARE\_STRUCT** toodefine hello strukturen för varje händelse och inkludera dem i din modell med hello **WITH\_DATA** makro.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-257">In that case you would use **DECLARE\_STRUCT** toodefine hello structure of each event and then include them in your model with hello **WITH\_DATA** macro.</span></span> <span data-ttu-id="b2f7c-258">Du skickar sedan varje händelse som vi gjorde i hello första exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-258">Then you send each event as we did in hello first example above.</span></span> <span data-ttu-id="b2f7c-259">I den här metoden som du skulle bara överföra en enskild händelse för**SERIALISERAREN**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-259">In this approach you would only pass a single data event too**SERIALIZER**.</span></span>

<span data-ttu-id="b2f7c-260">Om du tycker om din modell sätt objektorienterad kan sedan hello andra sättet passar dig.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-260">If you think about your model in an object-oriented fashion, then hello second approach may suit you.</span></span> <span data-ttu-id="b2f7c-261">I det här fallet hello-element har definierats med **WITH\_DATA** är hello ”egenskaper” för objektet.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-261">In this case, hello elements defined using **WITH\_DATA** are hello "properties" of your object.</span></span> <span data-ttu-id="b2f7c-262">Du kan skicka valfri del av händelser för**SERIALISERA** att du gillar, beroende på hur mycket av ditt ”objektets” tillstånd önskade toosend toohello moln.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-262">You pass whatever subset of events too**SERIALIZE** that you like, depending on how much of your "object’s" state you want toosend toohello cloud.</span></span>

<span data-ttu-id="b2f7c-263">Nether metod är rätt eller fel.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-263">Nether approach is right or wrong.</span></span> <span data-ttu-id="b2f7c-264">Bara vara medvetna om hur hello **serialiseraren** biblioteket fungerar och välj hello modellering metod som bäst passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-264">Just be aware of how hello **serializer** library works, and pick hello modeling approach that best fits your needs.</span></span>

## <a name="message-handling"></a><span data-ttu-id="b2f7c-265">Meddelandehantering</span><span class="sxs-lookup"><span data-stu-id="b2f7c-265">Message handling</span></span>
<span data-ttu-id="b2f7c-266">Hittills i den här artikeln har endast beskrivs skicka händelser tooIoT hubb och har inte åtgärdas ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-266">So far this article has only discussed sending events tooIoT Hub, and hasn't addressed receiving messages.</span></span> <span data-ttu-id="b2f7c-267">Hej orsak till det här är som vi behöver tooknow ta emot meddelanden i stort sett har beskrivits i ett [tidigare artikel](iot-hub-device-sdk-c-intro.md).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-267">hello reason for this is that what we need tooknow about receiving messages has largely been covered in an [earlier article](iot-hub-device-sdk-c-intro.md).</span></span> <span data-ttu-id="b2f7c-268">Återkalla från artikeln du bearbetar meddelanden genom att registrera en Återanropsfunktionen meddelande:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-268">Recall from that article that you process messages by registering a message callback function:</span></span>

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

<span data-ttu-id="b2f7c-269">Du kan sedan skriva hello återanropsfunktion som anropas när ett meddelande tas emot:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-269">You then write hello callback function that’s invoked when a message is received:</span></span>

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

<span data-ttu-id="b2f7c-270">Den här implementeringen av **IoTHubMessage** anrop hello specifik funktion för varje åtgärd i modellen.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-270">This implementation of **IoTHubMessage** calls hello specific function for each action in your model.</span></span> <span data-ttu-id="b2f7c-271">Om till exempel din modell definierar den här åtgärden:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-271">For example, if your model defines this action:</span></span>

```
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="b2f7c-272">Du måste definiera en funktion med den här signaturen:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-272">You must define a function with this signature:</span></span>

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="b2f7c-273">**SetAirResistance** sedan anropas när meddelandet skickas tooyour enhet.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-273">**SetAirResistance** is then called when that message is sent tooyour device.</span></span>

<span data-ttu-id="b2f7c-274">Vilka hello serialiseras versionen för meddelande ser ut som om är vi inte har förklaras ännu.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-274">What we haven't explained yet is what hello serialized version of message looks like.</span></span> <span data-ttu-id="b2f7c-275">Med andra ord, om du vill toosend en **SetAirResistance** meddelandet tooyour enhet, vad att se ut?</span><span class="sxs-lookup"><span data-stu-id="b2f7c-275">In other words, if you want toosend a **SetAirResistance** message tooyour device, what does that look like?</span></span>

<span data-ttu-id="b2f7c-276">Om du skickar ett meddelande tooa enhet kan göra du det via hello Azure IoT-tjänsten SDK.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-276">If you're sending a message tooa device, you would do so through hello Azure IoT service SDK.</span></span> <span data-ttu-id="b2f7c-277">Du måste fortfarande tooknow vad string toosend tooinvoke en viss åtgärd.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-277">You still need tooknow what string toosend tooinvoke a particular action.</span></span> <span data-ttu-id="b2f7c-278">hello allmänna format för att skicka ett meddelande visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-278">hello general format for sending a message appears as follows:</span></span>

```
{"Name" : "", "Parameters" : "" }
```

<span data-ttu-id="b2f7c-279">Du skickar ett serialiserat JSON-objekt med två egenskaper: **namn** är hello namnet på hello-åtgärd (meddelande) och **parametrar** innehåller hello parametrar för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-279">You're sending a serialized JSON object with two properties: **Name** is hello name of hello action (message) and **Parameters** contains hello parameters of that action.</span></span>

<span data-ttu-id="b2f7c-280">Till exempel tooinvoke **SetAirResistance** kan du skicka meddelandet tooa enheten:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-280">For example, tooinvoke **SetAirResistance** you can send this message tooa device:</span></span>

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

<span data-ttu-id="b2f7c-281">hello åtgärdsnamn måste exakt matcha en åtgärd som definierats i din modell.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-281">hello action name must exactly match an action defined in your model.</span></span> <span data-ttu-id="b2f7c-282">hello parameternamn måste matcha samt.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-282">hello parameter names must match as well.</span></span> <span data-ttu-id="b2f7c-283">Tänk också på skiftlägeskänslighet.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-283">Also note case sensitivity.</span></span> <span data-ttu-id="b2f7c-284">**Namnet** och **parametrar** är alltid versaler.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-284">**Name** and **Parameters** are always uppercase.</span></span> <span data-ttu-id="b2f7c-285">Se till att toomatch hello fallet åtgärdsnamn och parametrar i modellen.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-285">Make sure toomatch hello case of your action name and parameters in your model.</span></span> <span data-ttu-id="b2f7c-286">I det här exemplet är hello åtgärdsnamn ”SetAirResistance” och inte ”setairresistance”.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-286">In this example, hello action name is "SetAirResistance" and not "setairresistance".</span></span>

<span data-ttu-id="b2f7c-287">Hej två åtgärder **TurnFanOn** och **TurnFanOff** kan anropas genom att skicka dessa meddelanden tooa enhet:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-287">hello two other actions **TurnFanOn** and **TurnFanOff** can be invoked by sending these messages tooa device:</span></span>

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

<span data-ttu-id="b2f7c-288">Det här avsnittet beskrivs allt du behöver tooknow när händelser skickades och ta emot meddelanden med hello **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-288">This section described everything you need tooknow when sending events and receiving messages with hello **serializer** library.</span></span> <span data-ttu-id="b2f7c-289">Innan du går vidare vi beskriver vissa parametrar som du kan konfigurera som styr hur stor din modell är.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-289">Before moving on, let's cover some parameters you can configure that control how large your model is.</span></span>

## <a name="macro-configuration"></a><span data-ttu-id="b2f7c-290">Makrot-konfiguration</span><span class="sxs-lookup"><span data-stu-id="b2f7c-290">Macro configuration</span></span>
<span data-ttu-id="b2f7c-291">Om du använder hello **serialiseraren** biblioteket som en viktig del av hello SDK toobe medveten om finns i biblioteket för hello azure-c-delad-verktyget.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-291">If you’re using hello **Serializer** library an important part of hello SDK toobe aware of is found in hello azure-c-shared-utility library.</span></span>
<span data-ttu-id="b2f7c-292">Om du har klonas hello Azure-iot-sdk-c-databasen från GitHub med alternativet för hello--rekursiv hittar du här delade verktygsbiblioteket:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-292">If you have cloned hello Azure-iot-sdk-c repository from GitHub using hello --recursive option, then you will find this shared utility library here:</span></span>

```
.\\c-utility
```

<span data-ttu-id="b2f7c-293">Om du inte har klona hello bibliotek, hittar du den [här](https://github.com/Azure/azure-c-shared-utility).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-293">If you have not cloned hello library, you can find it [here](https://github.com/Azure/azure-c-shared-utility).</span></span>

<span data-ttu-id="b2f7c-294">I ett bibliotek för hello delade verktyg hittar du hello följande mapp:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-294">Within hello shared utility library, you will find hello following folder:</span></span>

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

<span data-ttu-id="b2f7c-295">Den här mappen innehåller ett Visual Studio-lösning som kallas **makrot\_verktyg för webbplatsuppgradering\_h\_generator.sln**:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-295">This folder contains a Visual Studio solution called **macro\_utils\_h\_generator.sln**:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

<span data-ttu-id="b2f7c-296">hello-programmet i den här lösningen genererar hello **makrot\_utils.h** fil.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-296">hello program in this solution generates hello **macro\_utils.h** file.</span></span> <span data-ttu-id="b2f7c-297">Det finns en standardmakrot\_utils.h-fil som ingår i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-297">There’s a default macro\_utils.h file included with hello SDK.</span></span> <span data-ttu-id="b2f7c-298">Den här lösningen kan du toomodify vissa parametrar och sedan återskapa hello huvudfilen baserat på dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-298">This solution allows you toomodify some parameters and then recreate hello header file based on these parameters.</span></span>

<span data-ttu-id="b2f7c-299">hello två viktiga parametrar toobe bekymrad är **nArithmetic** och **nMacroParameters** som definierats i dessa två rader som hittades i makro\_utils.tt:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-299">hello two key parameters toobe concerned with are **nArithmetic** and **nMacroParameters** which are defined in these two lines found in macro\_utils.tt:</span></span>

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

<span data-ttu-id="b2f7c-300">Dessa värden är hello-standardparametrar som ingår i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-300">These values are hello default parameters included with hello SDK.</span></span> <span data-ttu-id="b2f7c-301">Varje parameter har hello enligt följande:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-301">Each parameter has hello following meaning:</span></span>

* <span data-ttu-id="b2f7c-302">nMacroParameters – styr hur många parametrar som du kan ha i en DECLARE\_makrodefinition i MODELLEN.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-302">nMacroParameters – Controls how many parameters you can have in one DECLARE\_MODEL macro definition.</span></span>
* <span data-ttu-id="b2f7c-303">nArithmetic – kontroller hello Totalt antal medlemmar som tillåts i en modell.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-303">nArithmetic – Controls hello total number of members allowed in a model.</span></span>

<span data-ttu-id="b2f7c-304">hello beror parametrarna är viktiga eftersom de styr hur stor din modell kan vara.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-304">hello reason these parameters are important is because they control how large your model can be.</span></span> <span data-ttu-id="b2f7c-305">Anta till exempel att den här modelldefinitionen:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-305">For example, consider this model definition:</span></span>

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

<span data-ttu-id="b2f7c-306">Som nämnts tidigare **DECLARE\_MODELLEN** är bara ett C makro.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-306">As mentioned previously, **DECLARE\_MODEL** is just a C macro.</span></span> <span data-ttu-id="b2f7c-307">Hej namnen på hello modellen och hello **WITH\_DATA** instruktionen (har ett annat makro) har parametrar av **DECLARE\_MODELLEN**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-307">hello names of hello model and hello **WITH\_DATA** statement (yet another macro) are parameters of **DECLARE\_MODEL**.</span></span> <span data-ttu-id="b2f7c-308">**nMacroParameters** definierar hur många parametrar kan ingå i **DECLARE\_MODELLEN**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-308">**nMacroParameters** defines how many parameters can be included in **DECLARE\_MODEL**.</span></span> <span data-ttu-id="b2f7c-309">Detta definierar effektivt, hur många data händelse och åtgärd deklarationer att du kan ha.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-309">Effectively, this defines how many data event and action declarations you can have.</span></span> <span data-ttu-id="b2f7c-310">Därför med hello Standardgränsen för 124 innebär detta att du kan definiera en modell med en kombination av om 60 åtgärder och Datahändelser.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-310">As such, with hello default limit of 124 this means that you can define a model with a combination of about 60 actions and data events.</span></span> <span data-ttu-id="b2f7c-311">Om du försöker tooexceed denna gräns får Kompilatorfel som ser ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-311">If you try tooexceed this limit, you'll receive compiler errors that look similar toothis:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

<span data-ttu-id="b2f7c-312">Hej **nArithmetic** parametern är mer om hello interna bearbetningen i hello makrospråk än ditt program.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-312">hello **nArithmetic** parameter is more about hello internal workings of hello macro language than your application.</span></span>  <span data-ttu-id="b2f7c-313">Den styr hello Totalt antal medlemmar som du kan ha i din modell, inklusive **DECLARE_STRUCT** makron.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-313">It controls hello total number of members you can have in your model, including **DECLARE_STRUCT** macros.</span></span> <span data-ttu-id="b2f7c-314">Om du börjar Se Kompilatorfel sådana här kommer bör du öka **nArithmetic**:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-314">If you start seeing compiler errors such as this, then you should try increasing **nArithmetic**:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

<span data-ttu-id="b2f7c-315">Om du vill toochange parametrarna, ändra hello värden i hello makro\_utils.tt fil, recompile hello makrot\_verktyg för webbplatsuppgradering\_h\_generator.sln lösningen och kör hello kompilerat program.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-315">If you want toochange these parameters, modify hello values in hello macro\_utils.tt file, recompile hello macro\_utils\_h\_generator.sln solution, and run hello compiled program.</span></span> <span data-ttu-id="b2f7c-316">När du gör ett nytt makro\_utils.h fil skapas och placeras i hello.\\ vanliga\\inc directory.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-316">When you do so, a new macro\_utils.h file is generated and placed in hello .\\common\\inc directory.</span></span>

<span data-ttu-id="b2f7c-317">I ordning toouse hello ny version av makrot\_utils.h, ta bort hello **serialiseraren** NuGet-paketet från din lösning och i dess ställe inkluderar hello **serialiseraren** Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-317">In order toouse hello new version of macro\_utils.h, remove hello **serializer** NuGet package from your solution and in its place include hello **serializer** Visual Studio project.</span></span> <span data-ttu-id="b2f7c-318">Detta gör att din kod toocompile mot hello källkoden för hello serialiseraren bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-318">This enables your code toocompile against hello source code of hello serializer library.</span></span> <span data-ttu-id="b2f7c-319">Detta inkluderar hello uppdateras makrot\_utils.h.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-319">This includes hello updated macro\_utils.h.</span></span> <span data-ttu-id="b2f7c-320">Om du vill toodo för **simplesample\_amqp**, starta genom att ta bort hello NuGet-paket för hello serialiseraren bibliotek från hello lösning:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-320">If you want toodo this for **simplesample\_amqp**, start by removing hello NuGet package for hello serializer library from hello solution:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

<span data-ttu-id="b2f7c-321">Lägg sedan till det här projektet tooyour Visual Studio-lösning:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-321">Then add this project tooyour Visual Studio solution:</span></span>

> <span data-ttu-id="b2f7c-322">. \\c\\serialiseraren\\skapa\\windows\\serializer.vcxproj</span><span class="sxs-lookup"><span data-stu-id="b2f7c-322">.\\c\\serializer\\build\\windows\\serializer.vcxproj</span></span>
> 
> 

<span data-ttu-id="b2f7c-323">När du är klar bör din lösning se ut så här:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-323">When you're done, your solution should look like this:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

<span data-ttu-id="b2f7c-324">Nu när du kompilerar din lösning hello uppdateras makrot\_utils.h ingår i din binary.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-324">Now when you compile your solution, hello updated macro\_utils.h is included in your binary.</span></span>

<span data-ttu-id="b2f7c-325">Observera att öka dessa värden som är tillräckligt högt kan överstiga kompileraren gränser.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-325">Note that increasing these values high enough can exceed compiler limits.</span></span> <span data-ttu-id="b2f7c-326">toothis pekar, hello **nMacroParameters** är hello huvudsakliga parameter med vilka berörda toobe.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-326">toothis point, hello **nMacroParameters** is hello main parameter with which toobe concerned.</span></span> <span data-ttu-id="b2f7c-327">Hej C99-specifikationen anger att minst 127 parametrar tillåts i en makrodefinition.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-327">hello C99 spec specifies that a minimum of 127 parameters are allowed in a macro definition.</span></span> <span data-ttu-id="b2f7c-328">hello Microsoft-kompilatorn följer hello spec exakt (och är begränsad till 127), så att du inte kan tooincrease **nMacroParameters** utöver hello standard.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-328">hello Microsoft compiler follows hello spec exactly (and has a limit of 127), so you won't be able tooincrease **nMacroParameters** beyond hello default.</span></span> <span data-ttu-id="b2f7c-329">Andra kompilerare kan tillåta dig toodo så (till exempel hello GNU kompilatorn stöder en högre gräns).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-329">Other compilers might allow you toodo so (for example, hello GNU compiler supports a higher limit).</span></span>

<span data-ttu-id="b2f7c-330">Hittills har vi omfattas nästan allt du behöver tooknow om hur toowrite code med hello **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-330">So far we've covered just about everything you need tooknow about how toowrite code with hello **serializer** library.</span></span> <span data-ttu-id="b2f7c-331">Innan du att vi Ångra vissa avsnitt från föregående artiklar som du kanske undrar om.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-331">Before concluding, let's revisit some topics from previous articles that you may be wondering about.</span></span>

## <a name="hello-lower-level-apis"></a><span data-ttu-id="b2f7c-332">hello lågnivå-API: er</span><span class="sxs-lookup"><span data-stu-id="b2f7c-332">hello lower-level APIs</span></span>
<span data-ttu-id="b2f7c-333">hello exempelprogram som den här artikeln fokuserar är **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-333">hello sample application on which this article focused is **simplesample\_amqp**.</span></span> <span data-ttu-id="b2f7c-334">Det här exemplet använder hello på högre nivå (hello icke-”lla”) API: er toosend händelser och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-334">This sample uses hello higher-level (hello non-"LL") APIs toosend events and receive messages.</span></span> <span data-ttu-id="b2f7c-335">Om du använder dessa API: er körs en bakgrundstråd som tar hand om både skicka händelser och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-335">If you use these APIs, a background thread runs which takes care of both sending events and receiving messages.</span></span> <span data-ttu-id="b2f7c-336">Du kan dock använda hello på lägre nivå (alla) API: er tooeliminate denna bakgrundstråd och dra explicit kontroll över när du skickar händelser eller ta emot meddelanden från hello molnet.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-336">However, you can use hello lower-level (LL) APIs tooeliminate this background thread and take explicit control over when you send events or receive messages from hello cloud.</span></span>

<span data-ttu-id="b2f7c-337">Enligt beskrivningen i en [föregående artikel](iot-hub-device-sdk-c-iothubclient.md), det finns en uppsättning funktioner som består av hello på högre nivå API: er:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-337">As described in a [previous article](iot-hub-device-sdk-c-iothubclient.md), there is a set of functions that consists of hello higher-level APIs:</span></span>

* <span data-ttu-id="b2f7c-338">IoTHubClient\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="b2f7c-338">IoTHubClient\_CreateFromConnectionString</span></span>
* <span data-ttu-id="b2f7c-339">IoTHubClient\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="b2f7c-339">IoTHubClient\_SendEventAsync</span></span>
* <span data-ttu-id="b2f7c-340">IoTHubClient\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="b2f7c-340">IoTHubClient\_SetMessageCallback</span></span>
* <span data-ttu-id="b2f7c-341">IoTHubClient\_förstör</span><span class="sxs-lookup"><span data-stu-id="b2f7c-341">IoTHubClient\_Destroy</span></span>

<span data-ttu-id="b2f7c-342">Dessa API: er visas i **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-342">These APIs are demonstrated in **simplesample\_amqp**.</span></span>

<span data-ttu-id="b2f7c-343">Det finns också en liknande uppsättning API: er på lägre nivå.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-343">There is also an analogous set of lower-level APIs.</span></span>

* <span data-ttu-id="b2f7c-344">IoTHubClient\_lla\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="b2f7c-344">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="b2f7c-345">IoTHubClient\_lla\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="b2f7c-345">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="b2f7c-346">IoTHubClient\_lla\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="b2f7c-346">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="b2f7c-347">IoTHubClient\_lla\_förstör</span><span class="sxs-lookup"><span data-stu-id="b2f7c-347">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="b2f7c-348">Observera att hello lågnivå-API: er arbete exakt hello samma sätt som beskrivs i föregående hello-artiklar.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-348">Note that hello lower-level APIs work exactly hello same way as described in hello previous articles.</span></span> <span data-ttu-id="b2f7c-349">Du kan använda hello första uppsättning API: er om du vill att en bakgrund tråd toohandle skicka händelser och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-349">You can use hello first set of APIs if you want a background thread toohandle sending events and receiving messages.</span></span> <span data-ttu-id="b2f7c-350">Hello andra uppsättning API: er använder du om du vill ha explicit kontroll över när du skickar och tar emot data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-350">You use hello second set of APIs if you want explicit control over when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="b2f7c-351">Antingen uppsättning API: er fungerar lika bra med hello **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-351">Either set of APIs work equally well with hello **serializer** library.</span></span>

<span data-ttu-id="b2f7c-352">Ett exempel på hur hello lågnivå-API: er används med hello **serialiseraren** biblioteket finns hello **simplesample\_http** program.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-352">For an example of how hello lower-level APIs are used with hello **serializer** library, see hello **simplesample\_http** application.</span></span>

## <a name="additional-topics"></a><span data-ttu-id="b2f7c-353">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="b2f7c-353">Additional topics</span></span>
<span data-ttu-id="b2f7c-354">Några andra avsnitt värt att nämna igen är egenskapen hantering, med autentiseringsuppgifter för en annan enhet och konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-354">A few other topics worth mentioning again are property handling, using alternate device credentials, and configuration options.</span></span> <span data-ttu-id="b2f7c-355">Dessa är alla avsnitt som beskrivs i en [föregående artikel](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="b2f7c-355">These are all topics covered in a [previous article](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="b2f7c-356">hello utgångspunkten är att alla dessa funktioner fungerar i hello samma sätt med hello **serialiseraren** bibliotek som med hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-356">hello main point is that all of these features work in hello same way with hello **serializer** library as they do with hello **IoTHubClient** library.</span></span> <span data-ttu-id="b2f7c-357">Till exempel om du vill tooattach egenskaper tooan händelse från din modell kan du använda **IoTHubMessage\_egenskaper** och **kartan**\_**AddorUpdate**, hello samma sätt som beskrivs ovan:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-357">For example, if you want tooattach properties tooan event from your model, you use **IoTHubMessage\_Properties** and **Map**\_**AddorUpdate**, hello same way as described previously:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="b2f7c-358">Om hello händelsen skapades från hello **serialiseraren** bibliotek eller skapas manuellt med hjälp av hello **IoTHubClient** biblioteket spelar ingen roll.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-358">Whether hello event was generated from hello **serializer** library or created manually using hello **IoTHubClient** library does not matter.</span></span>

<span data-ttu-id="b2f7c-359">Alternativ för hello enheten autentiseringsuppgifterna, med hjälp av **IoTHubClient\_lla\_skapa** fungerar lika bra som **IoTHubClient\_CreateFromConnectionString** för allokera en **IOTHUB\_klienten\_hantera**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-359">For hello alternate device credentials, using **IoTHubClient\_LL\_Create** works just as well as **IoTHubClient\_CreateFromConnectionString** for allocating an **IOTHUB\_CLIENT\_HANDLE**.</span></span>

<span data-ttu-id="b2f7c-360">Slutligen, om du använder hello **serialiseraren** bibliotek, kan du ange konfigurationsalternativ med **IoTHubClient\_lla\_SetOption** precis som du gjorde när du använder hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-360">Finally, if you're using hello **serializer** library, you can set configuration options with **IoTHubClient\_LL\_SetOption** just as you did when using hello **IoTHubClient** library.</span></span>

<span data-ttu-id="b2f7c-361">En funktion som är unik toohello **serialiseraren** biblioteket är hello initieringen API: er.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-361">A feature that is unique toohello **serializer** library are hello initialization APIs.</span></span> <span data-ttu-id="b2f7c-362">Innan du kan börja arbeta med hello bibliotek, måste du anropa **serialiseraren\_init**:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-362">Before you can start working with hello library, you must call **serializer\_init**:</span></span>

```
serializer_init(NULL);
```

<span data-ttu-id="b2f7c-363">Detta görs precis innan du anropar **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-363">This is done just before you call **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="b2f7c-364">På liknande sätt, när du är klar arbeta med hello biblioteket hello senaste anropet ska du göra är för**serialiseraren\_deinit**:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-364">Similarly, when you're done working with hello library, hello last call you’ll make is too**serializer\_deinit**:</span></span>

```
serializer_deinit();
```

<span data-ttu-id="b2f7c-365">Annars alla hello andra funktioner som anges ovan fungerar hello samma i hello **serialiseraren** biblioteket som i hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-365">Otherwise, all of hello other features listed above work hello same in hello **serializer** library as they do in hello **IoTHubClient** library.</span></span> <span data-ttu-id="b2f7c-366">Mer information om något av följande avsnitt finns hello [föregående artikel](iot-hub-device-sdk-c-iothubclient.md) i den här serien.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-366">For more information about any of these topics, see hello [previous article](iot-hub-device-sdk-c-iothubclient.md) in this series.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2f7c-367">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2f7c-367">Next steps</span></span>
<span data-ttu-id="b2f7c-368">Den här artikeln beskrivs i detalj hello unika aspekter av hello **serialiseraren** bibliotek i hello **Azure IoT-enhet SDK för C**. Med hello informationen bör du ha en god förståelse av hur toouse modeller toosend händelser och ta emot meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-368">This article describes in detail hello unique aspects of hello **serializer** library contained in hello **Azure IoT device SDK for C**. With hello information provided you should have a good understanding of how toouse models toosend events and receive messages from IoT Hub.</span></span>

<span data-ttu-id="b2f7c-369">Detta avslutar också hello serie i tre delar på hur toodevelop program med hello **Azure IoT-enhet SDK för C**. Detta bör vara tillräckligt med information toonot endast get du startade men ger dig goda kunskaper om hur hello API: er fungerar.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-369">This also concludes hello three-part series on how toodevelop applications with hello **Azure IoT device SDK for C**. This should be enough information toonot only get you started but give you a thorough understanding of how hello APIs work.</span></span> <span data-ttu-id="b2f7c-370">För ytterligare information finns det några exempel i hello SDK inte som beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-370">For additional information, there are a few samples in hello SDK not covered here.</span></span> <span data-ttu-id="b2f7c-371">Annars hello [SDK-dokumentationen](https://github.com/Azure/azure-iot-sdk-c) är en bra resurs för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="b2f7c-371">Otherwise, hello [SDK documentation](https://github.com/Azure/azure-iot-sdk-c) is a good resource for additional information.</span></span>

<span data-ttu-id="b2f7c-372">toolearn mer information om hur du utvecklar för IoT-hubb finns hello [Azure IoT SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="b2f7c-372">toolearn more about developing for IoT Hub, see hello [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="b2f7c-373">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="b2f7c-373">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="b2f7c-374">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="b2f7c-374">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
