---
title: "Azure IoT-enhet SDK för C - serialiseraren | Microsoft Docs"
description: "Hur du använder serialiseraren biblioteket i Azure IoT-enhet SDK för C för att skapa appar för enheter som kommunicerar med en IoT-hubb."
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
ms.openlocfilehash: aa03c29c54d75538b1fdf987cac5f09d5d344f73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a><span data-ttu-id="bc433-103">Azure IoT-enhet SDK för C – mer information om serialiseraren</span><span class="sxs-lookup"><span data-stu-id="bc433-103">Azure IoT device SDK for C – more about serializer</span></span>
<span data-ttu-id="bc433-104">Den [först artikel](iot-hub-device-sdk-c-intro.md) i den här serien introduceras de **Azure IoT-enhet SDK för C**. I nästa artikel tillhandahålls en mer detaljerad beskrivning av den [ **IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="bc433-104">The [first article](iot-hub-device-sdk-c-intro.md) in this series introduced the **Azure IoT device SDK for C**. The next article provided a more detailed description of the [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="bc433-105">Den här artikeln är klar täckning av SDK genom att tillhandahålla en mer detaljerad beskrivning av återstående komponenten: den **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-105">This article completes coverage of the SDK by providing a more detailed description of the remaining component: the **serializer** library.</span></span>

<span data-ttu-id="bc433-106">Inledande artikeln beskrivs hur du använder den **serialiseraren** biblioteket för att skicka händelser till och ta emot meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc433-106">The introductory article described how to use the **serializer** library to send events to and receive messages from IoT Hub.</span></span> <span data-ttu-id="bc433-107">I den här artikeln vi utöka denna diskussion genom att tillhandahålla en fullständig förklaring av hur du modellera dina data med den **serialiseraren** makrospråk.</span><span class="sxs-lookup"><span data-stu-id="bc433-107">In this article, we extend that discussion by providing a more complete explanation of how to model your data with the **serializer** macro language.</span></span> <span data-ttu-id="bc433-108">Artikeln också innehåller mer information om hur biblioteket Serialiserar meddelanden (och i vissa fall hur du kan styra beteendet serialisering).</span><span class="sxs-lookup"><span data-stu-id="bc433-108">The article also includes more detail about how the library serializes messages (and in some cases how you can control the serialization behavior).</span></span> <span data-ttu-id="bc433-109">Vi kommer också att beskriva vissa parametrar som du kan ändra som bestämmer storleken på modeller som du skapar.</span><span class="sxs-lookup"><span data-stu-id="bc433-109">We'll also describe some parameters you can modify that determine the size of the models you create.</span></span>

<span data-ttu-id="bc433-110">Slutligen revisits artikeln vissa avsnitt som beskrivs i föregående artiklar, till exempel meddelande och egenskapen hantering.</span><span class="sxs-lookup"><span data-stu-id="bc433-110">Finally, the article revisits some topics covered in previous articles such as message and property handling.</span></span> <span data-ttu-id="bc433-111">Som vi ska ta reda dessa funktioner fungerar på samma sätt med hjälp av den **serialiseraren** bibliotek som med den **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-111">As we'll find out, those features work in the same way using the **serializer** library as they do with the **IoTHubClient** library.</span></span>

<span data-ttu-id="bc433-112">Allt i den här artikeln är baserad på den **serialiseraren** SDK-exempel.</span><span class="sxs-lookup"><span data-stu-id="bc433-112">Everything described in this article is based on the **serializer** SDK samples.</span></span> <span data-ttu-id="bc433-113">Om du vill följa med finns i **simplesample\_amqp** och **simplesample\_http** program som ingår i Azure IoT-enhet SDK för C.</span><span class="sxs-lookup"><span data-stu-id="bc433-113">If you want to follow along, see the **simplesample\_amqp** and **simplesample\_http** applications included in the Azure IoT device SDK for C.</span></span>

<span data-ttu-id="bc433-114">Du hittar den [ **Azure IoT-enhet SDK för C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen och visa information om API i den [C API-referens för](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="bc433-114">You can find the [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of the API in the [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="the-modeling-language"></a><span data-ttu-id="bc433-115">Modelleringsspråk</span><span class="sxs-lookup"><span data-stu-id="bc433-115">The modeling language</span></span>
<span data-ttu-id="bc433-116">Den [inledande artikel](iot-hub-device-sdk-c-intro.md) i den här serien introduceras de **Azure IoT-enhet SDK för C** modeling language genom exempel finns i den **simplesample\_amqp** program:</span><span class="sxs-lookup"><span data-stu-id="bc433-116">The [introductory article](iot-hub-device-sdk-c-intro.md) in this series introduced the **Azure IoT device SDK for C** modeling language through the example provided in the **simplesample\_amqp** application:</span></span>

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

<span data-ttu-id="bc433-117">Som du kan se utifrån modelleringsspråk C makron.</span><span class="sxs-lookup"><span data-stu-id="bc433-117">As you can see, the modeling language is based on C macros.</span></span> <span data-ttu-id="bc433-118">Du börjar din definition med alltid **BEGIN\_namnområde** och alltid sluta med **slutet\_namnområde**.</span><span class="sxs-lookup"><span data-stu-id="bc433-118">You always begin your definition with **BEGIN\_NAMESPACE** and always end with **END\_NAMESPACE**.</span></span> <span data-ttu-id="bc433-119">Det är vanligt att namnge namnområdet för ditt företag eller, som i följande exempel projektet som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="bc433-119">It's common to name the namespace for your company or, as in this example, the project that you're working on.</span></span>

<span data-ttu-id="bc433-120">Vad som ska ingå i namnområdet är definitioner för modellen.</span><span class="sxs-lookup"><span data-stu-id="bc433-120">What goes inside the namespace are model definitions.</span></span> <span data-ttu-id="bc433-121">I det här fallet finns en modell för en anemometer.</span><span class="sxs-lookup"><span data-stu-id="bc433-121">In this case, there is a single model for an anemometer.</span></span> <span data-ttu-id="bc433-122">Återigen modellen kan vara namn som helst, men detta är vanligtvis namnet för enheten eller typ av data som du vill utbyta med IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc433-122">Once again, the model can be named anything, but typically this is named for the device or type of data you want to exchange with IoT Hub.</span></span>  

<span data-ttu-id="bc433-123">Innehåller en definition av händelserna som du kan telemetriingång till IoT-hubb (den *data*) samt de meddelanden som du kan få från IoT-hubb (den *åtgärder*).</span><span class="sxs-lookup"><span data-stu-id="bc433-123">Models contain a definition of the events you can ingress to IoT Hub (the *data*) as well as the messages you can receive from IoT Hub (the *actions*).</span></span> <span data-ttu-id="bc433-124">Som du ser i exemplet har händelser en typ och ett namn. åtgärder som har ett namn och valfria parametrar (var och en med en typ).</span><span class="sxs-lookup"><span data-stu-id="bc433-124">As you can see from the example, events have a type and a name; actions have a name and optional parameters (each with a type).</span></span>

<span data-ttu-id="bc433-125">Vad visas inte i det här exemplet är ytterligare datatyper som stöds av SDK.</span><span class="sxs-lookup"><span data-stu-id="bc433-125">What’s not demonstrated in this sample are additional data types that are supported by the SDK.</span></span> <span data-ttu-id="bc433-126">Vi rapporterar nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bc433-126">We'll cover that next.</span></span>

> [!NOTE]
> <span data-ttu-id="bc433-127">IoT-hubb som refererar till de data som en enhet skickar till den som *händelser*, medan modelleringsspråk refererar till den som *data* (definieras med hjälp av **WITH_DATA**).</span><span class="sxs-lookup"><span data-stu-id="bc433-127">IoT Hub refers to the data a device sends to it as *events*, while the modeling language refers to it as *data* (defined using **WITH_DATA**).</span></span> <span data-ttu-id="bc433-128">På samma sätt IoT-hubb refererar till de data som du skickar till enheter som *meddelanden*, medan modelleringsspråk refererar till den som *åtgärder* (definieras med hjälp av **WITH_ACTION**).</span><span class="sxs-lookup"><span data-stu-id="bc433-128">Likewise, IoT Hub refers to the data you send to devices as *messages*, while the modeling language refers to it as *actions* (defined using **WITH_ACTION**).</span></span> <span data-ttu-id="bc433-129">Tänk på att dessa villkor synonymt får användas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="bc433-129">Be aware that these terms may be used interchangeably in this article.</span></span>
> 
> 

### <a name="supported-data-types"></a><span data-ttu-id="bc433-130">Datatyper som stöds</span><span class="sxs-lookup"><span data-stu-id="bc433-130">Supported data types</span></span>
<span data-ttu-id="bc433-131">Följande datatyper stöds i modeller som skapats med den **serialiseraren** bibliotek:</span><span class="sxs-lookup"><span data-stu-id="bc433-131">The following data types are supported in models created with the **serializer** library:</span></span>

| <span data-ttu-id="bc433-132">Typ</span><span class="sxs-lookup"><span data-stu-id="bc433-132">Type</span></span> | <span data-ttu-id="bc433-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bc433-133">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bc433-134">dubbla</span><span class="sxs-lookup"><span data-stu-id="bc433-134">double</span></span> |<span data-ttu-id="bc433-135">dubbel precision flyttal</span><span class="sxs-lookup"><span data-stu-id="bc433-135">double precision floating point number</span></span> |
| <span data-ttu-id="bc433-136">int</span><span class="sxs-lookup"><span data-stu-id="bc433-136">int</span></span> |<span data-ttu-id="bc433-137">32-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="bc433-137">32 bit integer</span></span> |
| <span data-ttu-id="bc433-138">flyttal</span><span class="sxs-lookup"><span data-stu-id="bc433-138">float</span></span> |<span data-ttu-id="bc433-139">flyttal med enkel precision</span><span class="sxs-lookup"><span data-stu-id="bc433-139">single precision floating point number</span></span> |
| <span data-ttu-id="bc433-140">lång</span><span class="sxs-lookup"><span data-stu-id="bc433-140">long</span></span> |<span data-ttu-id="bc433-141">långt heltal</span><span class="sxs-lookup"><span data-stu-id="bc433-141">long integer</span></span> |
| <span data-ttu-id="bc433-142">int8\_t</span><span class="sxs-lookup"><span data-stu-id="bc433-142">int8\_t</span></span> |<span data-ttu-id="bc433-143">8-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="bc433-143">8 bit integer</span></span> |
| <span data-ttu-id="bc433-144">Int16\_t</span><span class="sxs-lookup"><span data-stu-id="bc433-144">int16\_t</span></span> |<span data-ttu-id="bc433-145">16-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="bc433-145">16 bit integer</span></span> |
| <span data-ttu-id="bc433-146">Int32\_t</span><span class="sxs-lookup"><span data-stu-id="bc433-146">int32\_t</span></span> |<span data-ttu-id="bc433-147">32-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="bc433-147">32 bit integer</span></span> |
| <span data-ttu-id="bc433-148">Int64\_t</span><span class="sxs-lookup"><span data-stu-id="bc433-148">int64\_t</span></span> |<span data-ttu-id="bc433-149">64-bitars heltal</span><span class="sxs-lookup"><span data-stu-id="bc433-149">64 bit integer</span></span> |
| <span data-ttu-id="bc433-150">bool</span><span class="sxs-lookup"><span data-stu-id="bc433-150">bool</span></span> |<span data-ttu-id="bc433-151">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="bc433-151">boolean</span></span> |
| <span data-ttu-id="bc433-152">ASCII\_char\_ptr</span><span class="sxs-lookup"><span data-stu-id="bc433-152">ascii\_char\_ptr</span></span> |<span data-ttu-id="bc433-153">ASCII-sträng</span><span class="sxs-lookup"><span data-stu-id="bc433-153">ASCII string</span></span> |
| <span data-ttu-id="bc433-154">EDM\_DATUM\_TID\_FÖRSKJUTNING</span><span class="sxs-lookup"><span data-stu-id="bc433-154">EDM\_DATE\_TIME\_OFFSET</span></span> |<span data-ttu-id="bc433-155">tidsförskjutningen för datum</span><span class="sxs-lookup"><span data-stu-id="bc433-155">date time offset</span></span> |
| <span data-ttu-id="bc433-156">EDM\_GUID</span><span class="sxs-lookup"><span data-stu-id="bc433-156">EDM\_GUID</span></span> |<span data-ttu-id="bc433-157">GUID</span><span class="sxs-lookup"><span data-stu-id="bc433-157">GUID</span></span> |
| <span data-ttu-id="bc433-158">EDM\_BINÄRA</span><span class="sxs-lookup"><span data-stu-id="bc433-158">EDM\_BINARY</span></span> |<span data-ttu-id="bc433-159">Binär</span><span class="sxs-lookup"><span data-stu-id="bc433-159">binary</span></span> |
| <span data-ttu-id="bc433-160">DEKLARERA\_STRUKTUR</span><span class="sxs-lookup"><span data-stu-id="bc433-160">DECLARE\_STRUCT</span></span> |<span data-ttu-id="bc433-161">Komplex datatyp</span><span class="sxs-lookup"><span data-stu-id="bc433-161">complex data type</span></span> |

<span data-ttu-id="bc433-162">Vi börjar med den senaste datatypen.</span><span class="sxs-lookup"><span data-stu-id="bc433-162">Let’s start with the last data type.</span></span> <span data-ttu-id="bc433-163">Den **DECLARE\_STRUCT** kan du definiera komplexa datatyper som är grupper med primitiva typer.</span><span class="sxs-lookup"><span data-stu-id="bc433-163">The **DECLARE\_STRUCT** allows you to define complex data types, which are groupings of the other primitive types.</span></span> <span data-ttu-id="bc433-164">Grupperingarna ger oss möjlighet att definiera en modell som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="bc433-164">These groupings allow us to define a model that looks like this:</span></span>

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

<span data-ttu-id="bc433-165">Vår modell innehåller en enda data händelse av typen **TestType**.</span><span class="sxs-lookup"><span data-stu-id="bc433-165">Our model contains a single data event of type **TestType**.</span></span> <span data-ttu-id="bc433-166">**TestType** är en komplex typ som innehåller flera medlemmar som sammantagna visar primitiva typer som stöds av den **serialiseraren** modeling language.</span><span class="sxs-lookup"><span data-stu-id="bc433-166">**TestType** is a complex type that includes several members, which collectively demonstrate the primitive types supported by the **serializer** modeling language.</span></span>

<span data-ttu-id="bc433-167">Med en modell som detta skriva vi kod för att skicka data till IoT-hubb som visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bc433-167">With a model like this, we can write code to send data to IoT Hub that appears as follows:</span></span>

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

<span data-ttu-id="bc433-168">Vi i princip, tilldela ett värde för varje medlem i den **Test** struktur och sedan anropa **SendAsync** att skicka den **Test** data händelsen till molnet.</span><span class="sxs-lookup"><span data-stu-id="bc433-168">Basically, we’re assigning a value to every member of the **Test** structure and then calling **SendAsync** to send the **Test** data event to the cloud.</span></span> <span data-ttu-id="bc433-169">**SendAsync** är en hjälpfunktion som skickar en enda data-händelse till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="bc433-169">**SendAsync** is a helper function that sends a single data event to IoT Hub:</span></span>

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
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

<span data-ttu-id="bc433-170">Den här funktionen Serialiserar angivna händelsen och skickar det till IoT-hubb med **IoTHubClient\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="bc433-170">This function serializes the given data event and sends it to IoT Hub using **IoTHubClient\_SendEventAsync**.</span></span> <span data-ttu-id="bc433-171">Det här är samma kod som beskrivs i föregående artiklar (**SendAsync** kapslar in logiken i en lämplig funktion).</span><span class="sxs-lookup"><span data-stu-id="bc433-171">This is the same code discussed in previous articles (**SendAsync** encapsulates the logic into a convenient function).</span></span>

<span data-ttu-id="bc433-172">En andra hjälpfunktion som används i föregående kod är **GetDateTimeOffset**.</span><span class="sxs-lookup"><span data-stu-id="bc433-172">One other helper function used in the previous code is **GetDateTimeOffset**.</span></span> <span data-ttu-id="bc433-173">Den här funktionen omvandlar angiven tid till ett värde av typen **EDM\_datum\_tid\_OFFSET**:</span><span class="sxs-lookup"><span data-stu-id="bc433-173">This function transforms the given time into a value of type **EDM\_DATE\_TIME\_OFFSET**:</span></span>

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

<span data-ttu-id="bc433-174">Om du kör den här koden skickas följande meddelande till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="bc433-174">If you run this code, the following message is sent to IoT Hub:</span></span>

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

<span data-ttu-id="bc433-175">Observera att serialisering är JSON som är det format som genereras av den **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-175">Note that the serialization is JSON, which is the format generated by the **serializer** library.</span></span> <span data-ttu-id="bc433-176">Observera också att varje medlem i det serialiserade JSON-objektet matchar medlemmarna i den **TestType** som vi har definierat i vår modell.</span><span class="sxs-lookup"><span data-stu-id="bc433-176">Also note that each member of the serialized JSON object matches the members of the **TestType** that we defined in our model.</span></span> <span data-ttu-id="bc433-177">Värdena matchar också exakt de som används i koden.</span><span class="sxs-lookup"><span data-stu-id="bc433-177">The values also exactly match those used in the code.</span></span> <span data-ttu-id="bc433-178">Observera dock att binära data är base64-kodad: ”AQID” är base64-kodning av {0x01, 0x02, 0x03}.</span><span class="sxs-lookup"><span data-stu-id="bc433-178">However, note that the binary data is base64-encoded: "AQID" is the base64 encoding of {0x01, 0x02, 0x03}.</span></span>

<span data-ttu-id="bc433-179">Det här exemplet visar fördelen med att använda den **serialiseraren** -biblioteket kan vi ska skicka JSON till molnet, utan att behöva hantera uttryckligen serialisering i vårt program.</span><span class="sxs-lookup"><span data-stu-id="bc433-179">This example demonstrates the advantage of using the **serializer** library -- it enables us to send JSON to the cloud, without having to explicitly deal with serialization in our application.</span></span> <span data-ttu-id="bc433-180">Alla vi behöver bekymra sig om inställningsvärden Datahändelser i vår modell och sedan anropa enkla API: er för att skicka händelser till molnet.</span><span class="sxs-lookup"><span data-stu-id="bc433-180">All we have to worry about is setting the values of the data events in our model and then calling simple APIs to send those events to the cloud.</span></span>

<span data-ttu-id="bc433-181">Med den här informationen kan vi definiera modeller som innehåller en uppsättning stöds datatyper, inklusive komplexa typer (vi kan även inkludera komplexa typer i andra komplexa typer).</span><span class="sxs-lookup"><span data-stu-id="bc433-181">With this information, we can define models that include the range of supported data types, including complex types (we could even include complex types within other complex types).</span></span> <span data-ttu-id="bc433-182">Dock han serialiseras JSON som genererats av exemplet ovan öppnar en viktig aspekt.</span><span class="sxs-lookup"><span data-stu-id="bc433-182">However, he serialized JSON generated by the example above brings up an important point.</span></span> <span data-ttu-id="bc433-183">*Hur* vi skickar data med den **serialiseraren** biblioteket anger exakt hur JSON format.</span><span class="sxs-lookup"><span data-stu-id="bc433-183">*How* we send data with the **serializer** library determines exactly how the JSON is formed.</span></span> <span data-ttu-id="bc433-184">Viss är vad vi rapporterar nästa.</span><span class="sxs-lookup"><span data-stu-id="bc433-184">That particular point is what we'll cover next.</span></span>

## <a name="more-about-serialization"></a><span data-ttu-id="bc433-185">Mer information om serialisering</span><span class="sxs-lookup"><span data-stu-id="bc433-185">More about serialization</span></span>
<span data-ttu-id="bc433-186">I föregående avsnitt visar ett exempel på utdata som genererats av den **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-186">The previous section highlights an example of the output generated by the **serializer** library.</span></span> <span data-ttu-id="bc433-187">I det här avsnittet förklarar vi hur biblioteket Serialiserar data och hur du kan styra beteendet med hjälp av serialisering API: er.</span><span class="sxs-lookup"><span data-stu-id="bc433-187">In this section, we'll explain how the library serializes data and how you can control that behavior using the serialization APIs.</span></span>

<span data-ttu-id="bc433-188">För att kunna gå vidare diskussion om serialisering arbetar vi med en ny modell utifrån en termostat.</span><span class="sxs-lookup"><span data-stu-id="bc433-188">In order to advance the discussion on serialization, we'll work with a new model based on a thermostat.</span></span> <span data-ttu-id="bc433-189">Först ska vi använda vissa bakgrund på scenariot vi försöker adress.</span><span class="sxs-lookup"><span data-stu-id="bc433-189">First, let's provide some background on the scenario we're trying to address.</span></span>

<span data-ttu-id="bc433-190">Vi vill utforma en termostat som mäter temperatur- och fuktighetskonsekvens.</span><span class="sxs-lookup"><span data-stu-id="bc433-190">We want to model a thermostat that measures temperature and humidity.</span></span> <span data-ttu-id="bc433-191">Varje typ av data ska skickas till IoT-hubb på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="bc433-191">Each piece of data is going to be sent to IoT Hub differently.</span></span> <span data-ttu-id="bc433-192">Som standard termostat ingresses temperatur-händelse en gång var 2 minuter. en fuktighet händelse är ingressed var 15: e minut.</span><span class="sxs-lookup"><span data-stu-id="bc433-192">By default, the thermostat ingresses a temperature event once every 2 minutes; a humidity event is ingressed once every 15 minutes.</span></span> <span data-ttu-id="bc433-193">När antingen händelsen ingressed måste den innehålla en tidsstämpel som visar tid att motsvarande temperatur eller fuktighet var mäts.</span><span class="sxs-lookup"><span data-stu-id="bc433-193">When either event is ingressed, it must include a timestamp that shows the time that the corresponding temperature or humidity was measured.</span></span>

<span data-ttu-id="bc433-194">På det här scenariot ser två olika sätt att modellera data, och förklarar vi effekten att modellering har på serialiserade utdata.</span><span class="sxs-lookup"><span data-stu-id="bc433-194">Given this scenario, we'll demonstrate two different ways to model the data, and we'll explain the effect that modeling has on the serialized output.</span></span>

### <a name="model-1"></a><span data-ttu-id="bc433-195">Modell 1</span><span class="sxs-lookup"><span data-stu-id="bc433-195">Model 1</span></span>
<span data-ttu-id="bc433-196">Här är den första versionen av en modell som stöder det föregående scenariot:</span><span class="sxs-lookup"><span data-stu-id="bc433-196">Here's the first version of a model that supports the previous scenario:</span></span>

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

<span data-ttu-id="bc433-197">Observera att modellen innehåller två Datahändelser: **temperatur** och **fuktighet**.</span><span class="sxs-lookup"><span data-stu-id="bc433-197">Note that the model includes two data events: **Temperature** and **Humidity**.</span></span> <span data-ttu-id="bc433-198">Till skillnad från föregående exempel är typ av varje händelse en struktur som definieras med hjälp av **DECLARE\_STRUCT**.</span><span class="sxs-lookup"><span data-stu-id="bc433-198">Unlike previous examples, the type of each event is a structure defined using **DECLARE\_STRUCT**.</span></span> <span data-ttu-id="bc433-199">**TemperatureEvent** innehåller ett mått för temperatur- och en tidsstämpel; **HumidityEvent** innehåller ett fuktighet mått och en tidsstämpel.</span><span class="sxs-lookup"><span data-stu-id="bc433-199">**TemperatureEvent** includes a temperature measurement and a timestamp; **HumidityEvent** contains a humidity measurement and a timestamp.</span></span> <span data-ttu-id="bc433-200">Den här modellen ger oss en naturlig sätt beskriva data för det scenario som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="bc433-200">This model gives us a natural way to model the data for the scenario described above.</span></span> <span data-ttu-id="bc433-201">När vi skickar en händelse till molnet, skickar vi antingen en temperatur/tidsstämpel eller ett par fuktighet/timestamp.</span><span class="sxs-lookup"><span data-stu-id="bc433-201">When we send an event to the cloud, we'll either send a temperature/timestamp or a humidity/timestamp pair.</span></span>

<span data-ttu-id="bc433-202">Vi kan skicka en händelse för temperatur på molnet med kod till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="bc433-202">We can send a temperature event to the cloud using code such as the following:</span></span>

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

<span data-ttu-id="bc433-203">Vi ska använda hårdkodade värden för temperatur- och fuktighetskonsekvens i exempelkoden men anta att vi faktiskt hämtar dessa värden genom att ta prov på termostat motsvarande sensorerna.</span><span class="sxs-lookup"><span data-stu-id="bc433-203">We'll use hard-coded values for temperature and humidity in the sample code, but imagine that we’re actually retrieving these values by sampling the corresponding sensors on the thermostat.</span></span>

<span data-ttu-id="bc433-204">Koden ovan använder den **GetDateTimeOffset** helper som infördes tidigare.</span><span class="sxs-lookup"><span data-stu-id="bc433-204">The code above uses the **GetDateTimeOffset** helper that was introduced previously.</span></span> <span data-ttu-id="bc433-205">Skäl som ska bli Rensa senare skiljer den här koden uttryckligen uppgiften att serialisering och skicka händelsen.</span><span class="sxs-lookup"><span data-stu-id="bc433-205">For reasons that will become clear later, this code explicitly separates the task of serializing and sending the event.</span></span> <span data-ttu-id="bc433-206">Föregående kod Serialiserar temperatur händelsen i en buffert.</span><span class="sxs-lookup"><span data-stu-id="bc433-206">The previous code serializes the temperature event into a buffer.</span></span> <span data-ttu-id="bc433-207">Sedan **sendMessage** är en hjälpfunktion (ingår i **simplesample\_amqp**) som skickar händelsen till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="bc433-207">Then, **sendMessage** is a helper function (included in **simplesample\_amqp**) that sends the event to IoT Hub:</span></span>

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

<span data-ttu-id="bc433-208">Den här koden är en delmängd av den **SendAsync** helper som beskrivs i föregående avsnitt, så vi inte kommer gå över den igen.</span><span class="sxs-lookup"><span data-stu-id="bc433-208">This code is a subset of the **SendAsync** helper described in the previous section, so we won’t go over it again here.</span></span>

<span data-ttu-id="bc433-209">När vi kör föregående kod för att skicka händelsen temperatur skickas den här serialiserade form av händelsen till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="bc433-209">When we run the previous code to send the Temperature event, this serialized form of the event is sent to IoT Hub:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="bc433-210">Vi skickar en temperatur som är av typen **TemperatureEvent** och att strukturen innehåller en **temperatur** och **tid** medlem.</span><span class="sxs-lookup"><span data-stu-id="bc433-210">We're sending a temperature which is of type **TemperatureEvent** and that struct contains a **Temperature** and **Time** member.</span></span> <span data-ttu-id="bc433-211">Detta återspeglas direkt i den serialiserade informationen.</span><span class="sxs-lookup"><span data-stu-id="bc433-211">This is directly reflected in the serialized data.</span></span>

<span data-ttu-id="bc433-212">På liknande sätt kan vi skicka en fuktighet händelse med den här koden:</span><span class="sxs-lookup"><span data-stu-id="bc433-212">Similarly, we can send a humidity event with this code:</span></span>

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="bc433-213">Det serialiserade formulär som skickas till IoT-hubb visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bc433-213">The serialized form that’s sent to IoT Hub appears as follows:</span></span>

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="bc433-214">Detta är igen som förväntat.</span><span class="sxs-lookup"><span data-stu-id="bc433-214">Again, this is as expected.</span></span>

<span data-ttu-id="bc433-215">Med den här modellen kan du föreställa dig hur ytterligare händelser kan enkelt läggas till.</span><span class="sxs-lookup"><span data-stu-id="bc433-215">With this model, you can imagine how additional events can easily be added.</span></span> <span data-ttu-id="bc433-216">Du kan definiera flera strukturer med **DECLARE\_STRUCT**, och inkludera motsvarande händelse i en modell med hjälp av **WITH\_DATA**.</span><span class="sxs-lookup"><span data-stu-id="bc433-216">You define more structures using **DECLARE\_STRUCT**, and include the corresponding event in the model using **WITH\_DATA**.</span></span>

<span data-ttu-id="bc433-217">Nu ska vi ändra modellen så att den innehåller samma data, men med en annan struktur.</span><span class="sxs-lookup"><span data-stu-id="bc433-217">Now, let’s modify the model so that it includes the same data but with a different structure.</span></span>

### <a name="model-2"></a><span data-ttu-id="bc433-218">Modell 2</span><span class="sxs-lookup"><span data-stu-id="bc433-218">Model 2</span></span>
<span data-ttu-id="bc433-219">Överväg alternativa modellen till det ovan:</span><span class="sxs-lookup"><span data-stu-id="bc433-219">Consider this alternative model to the one above:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="bc433-220">I det här fallet har vi tagit bort den **DECLARE\_STRUCT** makron och bara definierar dataobjekt från vårt scenario med enkla typer från modelleringsspråk.</span><span class="sxs-lookup"><span data-stu-id="bc433-220">In this case we've eliminated the **DECLARE\_STRUCT** macros and are simply defining the data items from our scenario using simple types from the modeling language.</span></span>

<span data-ttu-id="bc433-221">Bara för tillfället har vi ignorera den **tid** händelse.</span><span class="sxs-lookup"><span data-stu-id="bc433-221">Just for the moment let’s ignore the **Time** event.</span></span> <span data-ttu-id="bc433-222">Här är koden till ingång med att reservera **temperatur**:</span><span class="sxs-lookup"><span data-stu-id="bc433-222">With that aside, here’s the code to ingress **Temperature**:</span></span>

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

<span data-ttu-id="bc433-223">Den här koden skickar följande serialiserade händelse till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="bc433-223">This code sends the following serialized event to IoT Hub:</span></span>

```
{"Temperature":75}
```

<span data-ttu-id="bc433-224">Och koden för att skicka händelsen fuktighet visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bc433-224">And the code for sending the Humidity event appears as follows:</span></span>

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="bc433-225">Den här koden skickar detta till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="bc433-225">This code sends this to IoT Hub:</span></span>

```
{"Humidity":45}
```

<span data-ttu-id="bc433-226">Så länge finns det inga överraskningar.</span><span class="sxs-lookup"><span data-stu-id="bc433-226">So far there are still no surprises.</span></span> <span data-ttu-id="bc433-227">Nu ska vi ändra hur vi kan använda SERIALISERA makro.</span><span class="sxs-lookup"><span data-stu-id="bc433-227">Now let's change how we use the SERIALIZE macro.</span></span>

<span data-ttu-id="bc433-228">Den **SERIALISERA** makrot kan ta flera Datahändelser som argument.</span><span class="sxs-lookup"><span data-stu-id="bc433-228">The **SERIALIZE** macro can take multiple data events as arguments.</span></span> <span data-ttu-id="bc433-229">Detta gör det möjligt för oss att serialisera den **temperatur** och **fuktighet** händelse tillsammans och skicka dem till IoT-hubb i ett anrop:</span><span class="sxs-lookup"><span data-stu-id="bc433-229">This enables us to serialize the **Temperature** and **Humidity** event together and send them to IoT Hub in one call:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="bc433-230">Du kanske tror att resultatet av den här koden är att två händelser skickas till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="bc433-230">You might guess that the result of this code is that two data events are sent to IoT Hub:</span></span>

<span data-ttu-id="bc433-231">[</span><span class="sxs-lookup"><span data-stu-id="bc433-231">[</span></span>

<span data-ttu-id="bc433-232">{”Temperatur”: 75},</span><span class="sxs-lookup"><span data-stu-id="bc433-232">{"Temperature":75},</span></span>

<span data-ttu-id="bc433-233">{”Fuktighet”: 45}</span><span class="sxs-lookup"><span data-stu-id="bc433-233">{"Humidity":45}</span></span>

<span data-ttu-id="bc433-234">]</span><span class="sxs-lookup"><span data-stu-id="bc433-234">]</span></span>

<span data-ttu-id="bc433-235">Med andra ord du tror att den här koden är detsamma som att skicka **temperatur** och **fuktighet** separat.</span><span class="sxs-lookup"><span data-stu-id="bc433-235">In other words, you might expect that this code is the same as sending **Temperature** and **Humidity** separately.</span></span> <span data-ttu-id="bc433-236">Det är bara i syfte att underlätta vid båda händelser till **SERIALISERA** i samma anropet.</span><span class="sxs-lookup"><span data-stu-id="bc433-236">It’s just a convenience to pass both events to **SERIALIZE** in the same call.</span></span> <span data-ttu-id="bc433-237">Men är som inte fallet.</span><span class="sxs-lookup"><span data-stu-id="bc433-237">However, that’s not the case.</span></span> <span data-ttu-id="bc433-238">I stället skickar koden ovan enda data händelsen till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="bc433-238">Instead, the code above sends this single data event to IoT Hub:</span></span>

<span data-ttu-id="bc433-239">{”Temperatur”: 75, ”fuktighet”: 45}</span><span class="sxs-lookup"><span data-stu-id="bc433-239">{"Temperature":75, "Humidity":45}</span></span>

<span data-ttu-id="bc433-240">Det kan verka underligt eftersom vår modell definierar **temperatur** och **fuktighet** som två *separat* händelser:</span><span class="sxs-lookup"><span data-stu-id="bc433-240">This may seem strange because our model defines **Temperature** and **Humidity** as two *separate* events:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="bc433-241">Punkt, inte har vi mer modellen dessa händelser där **temperatur** och **fuktighet** finns i samma struktur:</span><span class="sxs-lookup"><span data-stu-id="bc433-241">More to the point, we didn’t model these events where **Temperature** and **Humidity** are in the same structure:</span></span>

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

<span data-ttu-id="bc433-242">Om vi använder den här modellen, skulle det vara lättare att förstå hur **temperatur** och **fuktighet** skulle skickas i samma serialiserade meddelandet.</span><span class="sxs-lookup"><span data-stu-id="bc433-242">If we used this model, it would be easier to understand how **Temperature** and **Humidity** would be sent in the same serialized message.</span></span> <span data-ttu-id="bc433-243">Men det kanske inte rensa anledningen till att den fungerar på så sätt när du skickar både Datahändelser till **SERIALISERA** med hjälp av modellen 2.</span><span class="sxs-lookup"><span data-stu-id="bc433-243">However it may not be clear why it works that way when you pass both data events to **SERIALIZE** using model 2.</span></span>

<span data-ttu-id="bc433-244">Det här beteendet är lättare att förstå om du känner till antaganden som den **serialiseraren** biblioteket gör.</span><span class="sxs-lookup"><span data-stu-id="bc433-244">This behavior is easier to understand if you know the assumptions that the **serializer** library is making.</span></span> <span data-ttu-id="bc433-245">Om du vill vara meningsfullt på detta går vi tillbaka till vår modell:</span><span class="sxs-lookup"><span data-stu-id="bc433-245">To make sense of this let’s go back to our model:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="bc433-246">Tänk på den här modellen i objektorienterad villkor.</span><span class="sxs-lookup"><span data-stu-id="bc433-246">Think of this model in object-oriented terms.</span></span> <span data-ttu-id="bc433-247">I det här fallet vi modellering en fysisk enhet (en termostat) och den enheten innehåller attribut som **temperatur** och **fuktighet**.</span><span class="sxs-lookup"><span data-stu-id="bc433-247">In this case we’re modeling a physical device (a thermostat) and that device includes attributes like **Temperature** and **Humidity**.</span></span>

<span data-ttu-id="bc433-248">Vi kan skicka hela tillståndet för vår modell med kod till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="bc433-248">We can send the entire state of our model with code such as the following:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="bc433-249">Om du värdena för temperatur, fuktighet och tid är inställda, ser vi en händelse inträffar skickas till IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="bc433-249">Assuming the values of Temperature, Humidity and Time are set, we would see an event like this sent to IoT Hub:</span></span>

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="bc433-250">Ibland kan du bara vill skicka *vissa* egenskaper för modellen till molnet (detta är särskilt viktigt om modellen innehåller ett stort antal Datahändelser).</span><span class="sxs-lookup"><span data-stu-id="bc433-250">Sometimes you may only want to send *some* properties of the model to the cloud (this is especially true if your model contains a large number of data events).</span></span> <span data-ttu-id="bc433-251">Det är användbart att skicka en delmängd data händelser, som i det tidigare exemplet:</span><span class="sxs-lookup"><span data-stu-id="bc433-251">It’s useful to send only a subset of data events, such as in our earlier example:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="bc433-252">Detta genererar exakt samma serialiserade händelse som om vi har definierat en **TemperatureEvent** med en **temperatur** och **tid** medlem, precis som vi med modellen är 1.</span><span class="sxs-lookup"><span data-stu-id="bc433-252">This generates exactly the same serialized event as if we had defined a **TemperatureEvent** with a **Temperature** and **Time** member, just as we did with model 1.</span></span> <span data-ttu-id="bc433-253">I det här fallet kunde vi generera exakt samma serialiserade händelse med hjälp av en annan modell (modell 2) eftersom vi har ringt **SERIALISERA** på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="bc433-253">In this case we were able to generate exactly the same serialized event by using a different model (model 2) because we called **SERIALIZE** in a different way.</span></span>

<span data-ttu-id="bc433-254">Det viktiga är som om du skickar flera Datahändelser som ska **SERIALISERA,** sedan förutsätts varje händelse är en egenskap i ett enda JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="bc433-254">The important point is that if you pass multiple data events to **SERIALIZE,** then it assumes each event is a property in a single JSON object.</span></span>

<span data-ttu-id="bc433-255">Det bästa sättet är beroende av du och hur du tycker om din modell.</span><span class="sxs-lookup"><span data-stu-id="bc433-255">The best approach depends on you and how you think about your model.</span></span> <span data-ttu-id="bc433-256">Om du skickar ”händelser” till molnet och varje händelse innehåller en definierad uppsättning egenskaper, är det första tillvägagångssättet mycket bra.</span><span class="sxs-lookup"><span data-stu-id="bc433-256">If you’re sending "events" to the cloud and each event contains a defined set of properties, then the first approach makes a lot of sense.</span></span> <span data-ttu-id="bc433-257">I så fall kan du använda **DECLARE\_STRUCT** att definiera strukturen för varje händelse och inkludera dem i din modell med den **WITH\_DATA** makro.</span><span class="sxs-lookup"><span data-stu-id="bc433-257">In that case you would use **DECLARE\_STRUCT** to define the structure of each event and then include them in your model with the **WITH\_DATA** macro.</span></span> <span data-ttu-id="bc433-258">Du skickar sedan varje händelse som vi gjorde i det första exemplet ovan.</span><span class="sxs-lookup"><span data-stu-id="bc433-258">Then you send each event as we did in the first example above.</span></span> <span data-ttu-id="bc433-259">I den här metoden kan du bara skicka en enskild händelse **SERIALISERAREN**.</span><span class="sxs-lookup"><span data-stu-id="bc433-259">In this approach you would only pass a single data event to **SERIALIZER**.</span></span>

<span data-ttu-id="bc433-260">Om du tycker om din modell objektorienterad sätt kanske sedan den andra metoden passar dig.</span><span class="sxs-lookup"><span data-stu-id="bc433-260">If you think about your model in an object-oriented fashion, then the second approach may suit you.</span></span> <span data-ttu-id="bc433-261">I det här fallet element definieras med hjälp av **WITH\_DATA** ”egenskaper” för objektet.</span><span class="sxs-lookup"><span data-stu-id="bc433-261">In this case, the elements defined using **WITH\_DATA** are the "properties" of your object.</span></span> <span data-ttu-id="bc433-262">Du skickar oavsett delmängd av händelser till **SERIALISERA** som du vill, beroende på hur mycket av ditt ”objektets” tillstånd som du vill skicka till molnet.</span><span class="sxs-lookup"><span data-stu-id="bc433-262">You pass whatever subset of events to **SERIALIZE** that you like, depending on how much of your "object’s" state you want to send to the cloud.</span></span>

<span data-ttu-id="bc433-263">Nether metod är rätt eller fel.</span><span class="sxs-lookup"><span data-stu-id="bc433-263">Nether approach is right or wrong.</span></span> <span data-ttu-id="bc433-264">Bara vara medveten om hur **serialiseraren** biblioteket fungerar och välj modellering-metod som bäst passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="bc433-264">Just be aware of how the **serializer** library works, and pick the modeling approach that best fits your needs.</span></span>

## <a name="message-handling"></a><span data-ttu-id="bc433-265">Meddelandehantering</span><span class="sxs-lookup"><span data-stu-id="bc433-265">Message handling</span></span>
<span data-ttu-id="bc433-266">Så länge den här artikeln har endast beskrivs skicka händelser till IoT-hubb som åtgärdas inte ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bc433-266">So far this article has only discussed sending events to IoT Hub, and hasn't addressed receiving messages.</span></span> <span data-ttu-id="bc433-267">Orsaken till detta är som vi behöver känna till att ta emot meddelanden har i stort sett beskrivits i en [tidigare artikel](iot-hub-device-sdk-c-intro.md).</span><span class="sxs-lookup"><span data-stu-id="bc433-267">The reason for this is that what we need to know about receiving messages has largely been covered in an [earlier article](iot-hub-device-sdk-c-intro.md).</span></span> <span data-ttu-id="bc433-268">Återkalla från artikeln du bearbetar meddelanden genom att registrera en Återanropsfunktionen meddelande:</span><span class="sxs-lookup"><span data-stu-id="bc433-268">Recall from that article that you process messages by registering a message callback function:</span></span>

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

<span data-ttu-id="bc433-269">Du kan sedan skriva den återanropsfunktion som anropas när ett meddelande tas emot:</span><span class="sxs-lookup"><span data-stu-id="bc433-269">You then write the callback function that’s invoked when a message is received:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
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

<span data-ttu-id="bc433-270">Den här implementeringen av **IoTHubMessage** anropar funktionen specifika för varje åtgärd i modellen.</span><span class="sxs-lookup"><span data-stu-id="bc433-270">This implementation of **IoTHubMessage** calls the specific function for each action in your model.</span></span> <span data-ttu-id="bc433-271">Om till exempel din modell definierar den här åtgärden:</span><span class="sxs-lookup"><span data-stu-id="bc433-271">For example, if your model defines this action:</span></span>

```
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="bc433-272">Du måste definiera en funktion med den här signaturen:</span><span class="sxs-lookup"><span data-stu-id="bc433-272">You must define a function with this signature:</span></span>

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="bc433-273">**SetAirResistance** sedan anropas när meddelandet skickas till din enhet.</span><span class="sxs-lookup"><span data-stu-id="bc433-273">**SetAirResistance** is then called when that message is sent to your device.</span></span>

<span data-ttu-id="bc433-274">Vi har inte förklaras ännu är hur den serialiserade versionen av meddelande som ser ut.</span><span class="sxs-lookup"><span data-stu-id="bc433-274">What we haven't explained yet is what the serialized version of message looks like.</span></span> <span data-ttu-id="bc433-275">Med andra ord, om du vill skicka en **SetAirResistance** meddelande till din enhet, vad att se ut?</span><span class="sxs-lookup"><span data-stu-id="bc433-275">In other words, if you want to send a **SetAirResistance** message to your device, what does that look like?</span></span>

<span data-ttu-id="bc433-276">Om du skickar ett meddelande till en enhet kan göra du det via tjänsten Azure IoT SDK.</span><span class="sxs-lookup"><span data-stu-id="bc433-276">If you're sending a message to a device, you would do so through the Azure IoT service SDK.</span></span> <span data-ttu-id="bc433-277">Du måste veta vilka sträng för att skicka att anropa en viss åtgärd.</span><span class="sxs-lookup"><span data-stu-id="bc433-277">You still need to know what string to send to invoke a particular action.</span></span> <span data-ttu-id="bc433-278">Allmänna format för att skicka ett meddelande visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bc433-278">The general format for sending a message appears as follows:</span></span>

```
{"Name" : "", "Parameters" : "" }
```

<span data-ttu-id="bc433-279">Du skickar ett serialiserat JSON-objekt med två egenskaper: **namn** är namnet på åtgärden (meddelande) och **parametrar** innehåller parametrar för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="bc433-279">You're sending a serialized JSON object with two properties: **Name** is the name of the action (message) and **Parameters** contains the parameters of that action.</span></span>

<span data-ttu-id="bc433-280">Till exempel för att anropa **SetAirResistance** du kan skicka det här meddelandet till en enhet:</span><span class="sxs-lookup"><span data-stu-id="bc433-280">For example, to invoke **SetAirResistance** you can send this message to a device:</span></span>

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

<span data-ttu-id="bc433-281">Åtgärdsnamn måste exakt matcha en åtgärd som definierats i din modell.</span><span class="sxs-lookup"><span data-stu-id="bc433-281">The action name must exactly match an action defined in your model.</span></span> <span data-ttu-id="bc433-282">Parameternamnet måste matcha samt.</span><span class="sxs-lookup"><span data-stu-id="bc433-282">The parameter names must match as well.</span></span> <span data-ttu-id="bc433-283">Tänk också på skiftlägeskänslighet.</span><span class="sxs-lookup"><span data-stu-id="bc433-283">Also note case sensitivity.</span></span> <span data-ttu-id="bc433-284">**Namnet** och **parametrar** är alltid versaler.</span><span class="sxs-lookup"><span data-stu-id="bc433-284">**Name** and **Parameters** are always uppercase.</span></span> <span data-ttu-id="bc433-285">Se till att matcha skiftläget för ditt namn och parametrar i din modell.</span><span class="sxs-lookup"><span data-stu-id="bc433-285">Make sure to match the case of your action name and parameters in your model.</span></span> <span data-ttu-id="bc433-286">I det här exemplet är namnet på åtgärden ”SetAirResistance” och inte ”setairresistance”.</span><span class="sxs-lookup"><span data-stu-id="bc433-286">In this example, the action name is "SetAirResistance" and not "setairresistance".</span></span>

<span data-ttu-id="bc433-287">De två andra åtgärderna **TurnFanOn** och **TurnFanOff** kan anropas genom att skicka meddelanden till en enhet:</span><span class="sxs-lookup"><span data-stu-id="bc433-287">The two other actions **TurnFanOn** and **TurnFanOff** can be invoked by sending these messages to a device:</span></span>

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

<span data-ttu-id="bc433-288">Det här avsnittet beskrivs allt du behöver veta när skicka händelser och ta emot meddelanden med den **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-288">This section described everything you need to know when sending events and receiving messages with the **serializer** library.</span></span> <span data-ttu-id="bc433-289">Innan du går vidare vi beskriver vissa parametrar som du kan konfigurera som styr hur stor din modell är.</span><span class="sxs-lookup"><span data-stu-id="bc433-289">Before moving on, let's cover some parameters you can configure that control how large your model is.</span></span>

## <a name="macro-configuration"></a><span data-ttu-id="bc433-290">Makrot-konfiguration</span><span class="sxs-lookup"><span data-stu-id="bc433-290">Macro configuration</span></span>
<span data-ttu-id="bc433-291">Om du använder den **serialiseraren** biblioteket som en viktig del av att vara medveten om SDK finns i biblioteket för azure-c-delad-verktyget.</span><span class="sxs-lookup"><span data-stu-id="bc433-291">If you’re using the **Serializer** library an important part of the SDK to be aware of is found in the azure-c-shared-utility library.</span></span>
<span data-ttu-id="bc433-292">Om du har klonas Azure-iot-sdk-c-databasen från GitHub med alternativet – rekursiv, hittar du här delade verktygsbiblioteket:</span><span class="sxs-lookup"><span data-stu-id="bc433-292">If you have cloned the Azure-iot-sdk-c repository from GitHub using the --recursive option, then you will find this shared utility library here:</span></span>

```
.\\c-utility
```

<span data-ttu-id="bc433-293">Om du inte har klona biblioteket, hittar du den [här](https://github.com/Azure/azure-c-shared-utility).</span><span class="sxs-lookup"><span data-stu-id="bc433-293">If you have not cloned the library, you can find it [here](https://github.com/Azure/azure-c-shared-utility).</span></span>

<span data-ttu-id="bc433-294">I biblioteket delade verktyg hittar du följande mapp:</span><span class="sxs-lookup"><span data-stu-id="bc433-294">Within the shared utility library, you will find the following folder:</span></span>

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

<span data-ttu-id="bc433-295">Den här mappen innehåller ett Visual Studio-lösning som kallas **makrot\_verktyg för webbplatsuppgradering\_h\_generator.sln**:</span><span class="sxs-lookup"><span data-stu-id="bc433-295">This folder contains a Visual Studio solution called **macro\_utils\_h\_generator.sln**:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

<span data-ttu-id="bc433-296">I den här lösningen skapas den **makrot\_utils.h** fil.</span><span class="sxs-lookup"><span data-stu-id="bc433-296">The program in this solution generates the **macro\_utils.h** file.</span></span> <span data-ttu-id="bc433-297">Det finns en standardmakrot\_utils.h-fil som medföljer SDK.</span><span class="sxs-lookup"><span data-stu-id="bc433-297">There’s a default macro\_utils.h file included with the SDK.</span></span> <span data-ttu-id="bc433-298">Den här lösningen kan du ändra vissa parametrar och sedan återskapa huvudfilen baserat på dessa parametrar.</span><span class="sxs-lookup"><span data-stu-id="bc433-298">This solution allows you to modify some parameters and then recreate the header file based on these parameters.</span></span>

<span data-ttu-id="bc433-299">Två viktiga parametrar är berörda med är **nArithmetic** och **nMacroParameters** som definierats i dessa två rader som hittades i makro\_utils.tt:</span><span class="sxs-lookup"><span data-stu-id="bc433-299">The two key parameters to be concerned with are **nArithmetic** and **nMacroParameters** which are defined in these two lines found in macro\_utils.tt:</span></span>

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

<span data-ttu-id="bc433-300">Dessa värden är standardparametrar som medföljer SDK.</span><span class="sxs-lookup"><span data-stu-id="bc433-300">These values are the default parameters included with the SDK.</span></span> <span data-ttu-id="bc433-301">Varje parameter har enligt följande:</span><span class="sxs-lookup"><span data-stu-id="bc433-301">Each parameter has the following meaning:</span></span>

* <span data-ttu-id="bc433-302">nMacroParameters – styr hur många parametrar som du kan ha i en DECLARE\_makrodefinition i MODELLEN.</span><span class="sxs-lookup"><span data-stu-id="bc433-302">nMacroParameters – Controls how many parameters you can have in one DECLARE\_MODEL macro definition.</span></span>
* <span data-ttu-id="bc433-303">nArithmetic – styr det totala antalet medlemmar som tillåts i en modell.</span><span class="sxs-lookup"><span data-stu-id="bc433-303">nArithmetic – Controls the total number of members allowed in a model.</span></span>

<span data-ttu-id="bc433-304">Dessa parametrar är viktiga orsaken är eftersom de styr hur stor din modell kan vara.</span><span class="sxs-lookup"><span data-stu-id="bc433-304">The reason these parameters are important is because they control how large your model can be.</span></span> <span data-ttu-id="bc433-305">Anta till exempel att den här modelldefinitionen:</span><span class="sxs-lookup"><span data-stu-id="bc433-305">For example, consider this model definition:</span></span>

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

<span data-ttu-id="bc433-306">Som nämnts tidigare **DECLARE\_MODELLEN** är bara ett C makro.</span><span class="sxs-lookup"><span data-stu-id="bc433-306">As mentioned previously, **DECLARE\_MODEL** is just a C macro.</span></span> <span data-ttu-id="bc433-307">Namnen på modellen och **WITH\_DATA** instruktionen (har ett annat makro) har parametrar av **DECLARE\_MODELLEN**.</span><span class="sxs-lookup"><span data-stu-id="bc433-307">The names of the model and the **WITH\_DATA** statement (yet another macro) are parameters of **DECLARE\_MODEL**.</span></span> <span data-ttu-id="bc433-308">**nMacroParameters** definierar hur många parametrar kan ingå i **DECLARE\_MODELLEN**.</span><span class="sxs-lookup"><span data-stu-id="bc433-308">**nMacroParameters** defines how many parameters can be included in **DECLARE\_MODEL**.</span></span> <span data-ttu-id="bc433-309">Detta definierar effektivt, hur många data händelse och åtgärd deklarationer att du kan ha.</span><span class="sxs-lookup"><span data-stu-id="bc433-309">Effectively, this defines how many data event and action declarations you can have.</span></span> <span data-ttu-id="bc433-310">Därför med Standardgränsen för 124 innebär detta att du kan definiera en modell med en kombination av om 60 åtgärder och Datahändelser.</span><span class="sxs-lookup"><span data-stu-id="bc433-310">As such, with the default limit of 124 this means that you can define a model with a combination of about 60 actions and data events.</span></span> <span data-ttu-id="bc433-311">Om du överskrider den här gränsen, får du Kompilatorfel som ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="bc433-311">If you try to exceed this limit, you'll receive compiler errors that look similar to this:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

<span data-ttu-id="bc433-312">Den **nArithmetic** parametern är mer om den interna bearbetningen i makrospråk än ditt program.</span><span class="sxs-lookup"><span data-stu-id="bc433-312">The **nArithmetic** parameter is more about the internal workings of the macro language than your application.</span></span>  <span data-ttu-id="bc433-313">Den styr det totala antalet medlemmar i din modell, inklusive **DECLARE_STRUCT** makron.</span><span class="sxs-lookup"><span data-stu-id="bc433-313">It controls the total number of members you can have in your model, including **DECLARE_STRUCT** macros.</span></span> <span data-ttu-id="bc433-314">Om du börjar Se Kompilatorfel sådana här kommer bör du öka **nArithmetic**:</span><span class="sxs-lookup"><span data-stu-id="bc433-314">If you start seeing compiler errors such as this, then you should try increasing **nArithmetic**:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

<span data-ttu-id="bc433-315">Om du vill ändra dessa parametrar kan du ändra värdena för makrot\_utils.tt fil, kompilera om makrot\_verktyg för webbplatsuppgradering\_h\_generator.sln lösningen och kör kompilerade programmet.</span><span class="sxs-lookup"><span data-stu-id="bc433-315">If you want to change these parameters, modify the values in the macro\_utils.tt file, recompile the macro\_utils\_h\_generator.sln solution, and run the compiled program.</span></span> <span data-ttu-id="bc433-316">När du gör ett nytt makro\_utils.h fil skapas och placeras i det.\\ vanliga\\inc directory.</span><span class="sxs-lookup"><span data-stu-id="bc433-316">When you do so, a new macro\_utils.h file is generated and placed in the .\\common\\inc directory.</span></span>

<span data-ttu-id="bc433-317">För att kunna använda den nya versionen av makrot\_utils.h, ta bort den **serialiseraren** NuGet-paketet från din lösning och i dess ställe inkluderar den **serialiseraren** Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="bc433-317">In order to use the new version of macro\_utils.h, remove the **serializer** NuGet package from your solution and in its place include the **serializer** Visual Studio project.</span></span> <span data-ttu-id="bc433-318">Detta gör att din kod ska kompileras mot källkoden för serialiserare-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="bc433-318">This enables your code to compile against the source code of the serializer library.</span></span> <span data-ttu-id="bc433-319">Detta inkluderar uppdaterade makrot\_utils.h.</span><span class="sxs-lookup"><span data-stu-id="bc433-319">This includes the updated macro\_utils.h.</span></span> <span data-ttu-id="bc433-320">Om du vill göra detta för **simplesample\_amqp**, starta genom att ta bort NuGet-paket för serialisering biblioteket från lösningen:</span><span class="sxs-lookup"><span data-stu-id="bc433-320">If you want to do this for **simplesample\_amqp**, start by removing the NuGet package for the serializer library from the solution:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

<span data-ttu-id="bc433-321">Lägg sedan till det här projektet i Visual Studio-lösningen:</span><span class="sxs-lookup"><span data-stu-id="bc433-321">Then add this project to your Visual Studio solution:</span></span>

> <span data-ttu-id="bc433-322">. \\c\\serialiseraren\\skapa\\windows\\serializer.vcxproj</span><span class="sxs-lookup"><span data-stu-id="bc433-322">.\\c\\serializer\\build\\windows\\serializer.vcxproj</span></span>
> 
> 

<span data-ttu-id="bc433-323">När du är klar bör din lösning se ut så här:</span><span class="sxs-lookup"><span data-stu-id="bc433-323">When you're done, your solution should look like this:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

<span data-ttu-id="bc433-324">Nu när du kompilerar din lösning uppdaterade makrot\_utils.h ingår i din binary.</span><span class="sxs-lookup"><span data-stu-id="bc433-324">Now when you compile your solution, the updated macro\_utils.h is included in your binary.</span></span>

<span data-ttu-id="bc433-325">Observera att öka dessa värden som är tillräckligt högt kan överstiga kompileraren gränser.</span><span class="sxs-lookup"><span data-stu-id="bc433-325">Note that increasing these values high enough can exceed compiler limits.</span></span> <span data-ttu-id="bc433-326">Så här långt den **nMacroParameters** är den viktigaste parameter som berördes.</span><span class="sxs-lookup"><span data-stu-id="bc433-326">To this point, the **nMacroParameters** is the main parameter with which to be concerned.</span></span> <span data-ttu-id="bc433-327">C99-specifikationen anger att minst 127 parametrar tillåts i en makrodefinition.</span><span class="sxs-lookup"><span data-stu-id="bc433-327">The C99 spec specifies that a minimum of 127 parameters are allowed in a macro definition.</span></span> <span data-ttu-id="bc433-328">Microsoft-kompilatorn följer specifikationen exakt (och är begränsad till 127), så du kan inte öka **nMacroParameters** bortom standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="bc433-328">The Microsoft compiler follows the spec exactly (and has a limit of 127), so you won't be able to increase **nMacroParameters** beyond the default.</span></span> <span data-ttu-id="bc433-329">Andra kompilerare kan tillåta dig att göra det (till exempel GNU kompilatorn stöder en högre gräns).</span><span class="sxs-lookup"><span data-stu-id="bc433-329">Other compilers might allow you to do so (for example, the GNU compiler supports a higher limit).</span></span>

<span data-ttu-id="bc433-330">Hittills har vi omfattas nästan allt du behöver veta om hur du skriva kod med den **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-330">So far we've covered just about everything you need to know about how to write code with the **serializer** library.</span></span> <span data-ttu-id="bc433-331">Innan du att vi Ångra vissa avsnitt från föregående artiklar som du kanske undrar om.</span><span class="sxs-lookup"><span data-stu-id="bc433-331">Before concluding, let's revisit some topics from previous articles that you may be wondering about.</span></span>

## <a name="the-lower-level-apis"></a><span data-ttu-id="bc433-332">Lågnivå-API: er</span><span class="sxs-lookup"><span data-stu-id="bc433-332">The lower-level APIs</span></span>
<span data-ttu-id="bc433-333">Exempelprogrammet som den här artikeln fokuserar är **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="bc433-333">The sample application on which this article focused is **simplesample\_amqp**.</span></span> <span data-ttu-id="bc433-334">Det här exemplet använder den högre nivån (den icke-”lla”) API: er för att skicka händelser och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bc433-334">This sample uses the higher-level (the non-"LL") APIs to send events and receive messages.</span></span> <span data-ttu-id="bc433-335">Om du använder dessa API: er körs en bakgrundstråd som tar hand om både skicka händelser och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bc433-335">If you use these APIs, a background thread runs which takes care of both sending events and receiving messages.</span></span> <span data-ttu-id="bc433-336">Du kan dock använda API: er för lägre (alla) för att ta bort den här bakgrundstråd och dra explicit kontroll över när du skickar händelser eller ta emot meddelanden från molnet.</span><span class="sxs-lookup"><span data-stu-id="bc433-336">However, you can use the lower-level (LL) APIs to eliminate this background thread and take explicit control over when you send events or receive messages from the cloud.</span></span>

<span data-ttu-id="bc433-337">Enligt beskrivningen i en [föregående artikel](iot-hub-device-sdk-c-iothubclient.md), det finns en uppsättning funktioner som består av att API: er:</span><span class="sxs-lookup"><span data-stu-id="bc433-337">As described in a [previous article](iot-hub-device-sdk-c-iothubclient.md), there is a set of functions that consists of the higher-level APIs:</span></span>

* <span data-ttu-id="bc433-338">IoTHubClient\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="bc433-338">IoTHubClient\_CreateFromConnectionString</span></span>
* <span data-ttu-id="bc433-339">IoTHubClient\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="bc433-339">IoTHubClient\_SendEventAsync</span></span>
* <span data-ttu-id="bc433-340">IoTHubClient\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="bc433-340">IoTHubClient\_SetMessageCallback</span></span>
* <span data-ttu-id="bc433-341">IoTHubClient\_förstör</span><span class="sxs-lookup"><span data-stu-id="bc433-341">IoTHubClient\_Destroy</span></span>

<span data-ttu-id="bc433-342">Dessa API: er visas i **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="bc433-342">These APIs are demonstrated in **simplesample\_amqp**.</span></span>

<span data-ttu-id="bc433-343">Det finns också en liknande uppsättning API: er på lägre nivå.</span><span class="sxs-lookup"><span data-stu-id="bc433-343">There is also an analogous set of lower-level APIs.</span></span>

* <span data-ttu-id="bc433-344">IoTHubClient\_lla\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="bc433-344">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="bc433-345">IoTHubClient\_lla\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="bc433-345">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="bc433-346">IoTHubClient\_lla\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="bc433-346">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="bc433-347">IoTHubClient\_lla\_förstör</span><span class="sxs-lookup"><span data-stu-id="bc433-347">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="bc433-348">Observera att på lägre nivå-API: er fungerar på samma sätt som beskrivs i föregående artiklar.</span><span class="sxs-lookup"><span data-stu-id="bc433-348">Note that the lower-level APIs work exactly the same way as described in the previous articles.</span></span> <span data-ttu-id="bc433-349">Du kan använda den första uppsättningen av API: er om du vill att en bakgrundstråd som hanterar skicka händelser och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bc433-349">You can use the first set of APIs if you want a background thread to handle sending events and receiving messages.</span></span> <span data-ttu-id="bc433-350">Du använder den andra uppsättningen av API: er om du vill ha explicit kontroll över när du skickar och tar emot data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc433-350">You use the second set of APIs if you want explicit control over when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="bc433-351">Antingen uppsättning API: er fungerar lika bra med den **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-351">Either set of APIs work equally well with the **serializer** library.</span></span>

<span data-ttu-id="bc433-352">Ett exempel på hur du använder API: er för lägre nivå med den **serialiseraren** biblioteket finns det **simplesample\_http** program.</span><span class="sxs-lookup"><span data-stu-id="bc433-352">For an example of how the lower-level APIs are used with the **serializer** library, see the **simplesample\_http** application.</span></span>

## <a name="additional-topics"></a><span data-ttu-id="bc433-353">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="bc433-353">Additional topics</span></span>
<span data-ttu-id="bc433-354">Några andra avsnitt värt att nämna igen är egenskapen hantering, med autentiseringsuppgifter för en annan enhet och konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="bc433-354">A few other topics worth mentioning again are property handling, using alternate device credentials, and configuration options.</span></span> <span data-ttu-id="bc433-355">Dessa är alla avsnitt som beskrivs i en [föregående artikel](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="bc433-355">These are all topics covered in a [previous article](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="bc433-356">Utgångspunkten är att alla dessa funktioner fungerar på samma sätt med den **serialiseraren** bibliotek som med den **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-356">The main point is that all of these features work in the same way with the **serializer** library as they do with the **IoTHubClient** library.</span></span> <span data-ttu-id="bc433-357">Till exempel om du vill bifoga egenskaper till en händelse från din modell kan du använda **IoTHubMessage\_egenskaper** och **kartan**\_**AddorUpdate**, på samma sätt som beskrivits tidigare:</span><span class="sxs-lookup"><span data-stu-id="bc433-357">For example, if you want to attach properties to an event from your model, you use **IoTHubMessage\_Properties** and **Map**\_**AddorUpdate**, the same way as described previously:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="bc433-358">Om händelsen skapades från den **serialiseraren** bibliotek eller skapas manuellt med hjälp av den **IoTHubClient** biblioteket spelar ingen roll.</span><span class="sxs-lookup"><span data-stu-id="bc433-358">Whether the event was generated from the **serializer** library or created manually using the **IoTHubClient** library does not matter.</span></span>

<span data-ttu-id="bc433-359">För annan enhet autentiseringsuppgifterna, med **IoTHubClient\_lla\_skapa** fungerar lika bra som **IoTHubClient\_CreateFromConnectionString** för allokera en **IOTHUB\_klienten\_hantera**.</span><span class="sxs-lookup"><span data-stu-id="bc433-359">For the alternate device credentials, using **IoTHubClient\_LL\_Create** works just as well as **IoTHubClient\_CreateFromConnectionString** for allocating an **IOTHUB\_CLIENT\_HANDLE**.</span></span>

<span data-ttu-id="bc433-360">Slutligen, om du använder den **serialiseraren** bibliotek, kan du ange konfigurationsalternativ med **IoTHubClient\_lla\_SetOption** precis som du gjorde när du använder **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-360">Finally, if you're using the **serializer** library, you can set configuration options with **IoTHubClient\_LL\_SetOption** just as you did when using the **IoTHubClient** library.</span></span>

<span data-ttu-id="bc433-361">En funktion som är unik för den **serialiseraren** biblioteket är initieringen API: er.</span><span class="sxs-lookup"><span data-stu-id="bc433-361">A feature that is unique to the **serializer** library are the initialization APIs.</span></span> <span data-ttu-id="bc433-362">Innan du kan börja arbeta med biblioteket, måste du anropa **serialiseraren\_init**:</span><span class="sxs-lookup"><span data-stu-id="bc433-362">Before you can start working with the library, you must call **serializer\_init**:</span></span>

```
serializer_init(NULL);
```

<span data-ttu-id="bc433-363">Detta görs precis innan du anropar **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="bc433-363">This is done just before you call **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="bc433-364">På liknande sätt, när du är klar arbetar med biblioteket, det senaste anropet ska du göra är att **serialiseraren\_deinit**:</span><span class="sxs-lookup"><span data-stu-id="bc433-364">Similarly, when you're done working with the library, the last call you’ll make is to **serializer\_deinit**:</span></span>

```
serializer_deinit();
```

<span data-ttu-id="bc433-365">Annars alla andra funktioner som anges ovan fungerar på samma sätt den **serialiseraren** biblioteket som i den **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="bc433-365">Otherwise, all of the other features listed above work the same in the **serializer** library as they do in the **IoTHubClient** library.</span></span> <span data-ttu-id="bc433-366">Mer information om något av de här ämnena finns i [föregående artikel](iot-hub-device-sdk-c-iothubclient.md) i den här serien.</span><span class="sxs-lookup"><span data-stu-id="bc433-366">For more information about any of these topics, see the [previous article](iot-hub-device-sdk-c-iothubclient.md) in this series.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc433-367">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc433-367">Next steps</span></span>
<span data-ttu-id="bc433-368">Den här artikeln beskrivs i detalj unika aspekter av den **serialiseraren** biblioteket finns i den **Azure IoT-enhet SDK för C**. Med informationen som du bör ha en god förståelse av hur du använder modeller att skicka händelser och ta emot meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="bc433-368">This article describes in detail the unique aspects of the **serializer** library contained in the **Azure IoT device SDK for C**. With the information provided you should have a good understanding of how to use models to send events and receive messages from IoT Hub.</span></span>

<span data-ttu-id="bc433-369">Detta avslutar också på hur du utvecklar program med tre delar serien i **Azure IoT-enhet SDK för C**. Detta bör vara tillräckligt med information för att inte bara komma igång utan att ge goda kunskaper om hur de API: er fungerar.</span><span class="sxs-lookup"><span data-stu-id="bc433-369">This also concludes the three-part series on how to develop applications with the **Azure IoT device SDK for C**. This should be enough information to not only get you started but give you a thorough understanding of how the APIs work.</span></span> <span data-ttu-id="bc433-370">Mer information finns några exempel i SDK beskrivs inte här.</span><span class="sxs-lookup"><span data-stu-id="bc433-370">For additional information, there are a few samples in the SDK not covered here.</span></span> <span data-ttu-id="bc433-371">I annat fall den [SDK-dokumentationen](https://github.com/Azure/azure-iot-sdk-c) är en bra resurs för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="bc433-371">Otherwise, the [SDK documentation](https://github.com/Azure/azure-iot-sdk-c) is a good resource for additional information.</span></span>

<span data-ttu-id="bc433-372">Mer information om hur du utvecklar för IoT-hubb finns i [Azure IoT SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="bc433-372">To learn more about developing for IoT Hub, see the [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="bc433-373">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="bc433-373">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="bc433-374">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="bc433-374">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
