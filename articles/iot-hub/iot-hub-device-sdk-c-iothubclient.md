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
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a><span data-ttu-id="f3774-103">Azure IoT-enhet SDK för C – mer information om IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="f3774-103">Azure IoT device SDK for C – more about IoTHubClient</span></span>
<span data-ttu-id="f3774-104">Hej [först artikel](iot-hub-device-sdk-c-intro.md) i den här serien introduceras hello **Azure IoT-enhet SDK för C**. Artikeln förklaras att det finns två arkitektur lager i SDK.</span><span class="sxs-lookup"><span data-stu-id="f3774-104">hello [first article](iot-hub-device-sdk-c-intro.md) in this series introduced hello **Azure IoT device SDK for C**. That article explained that there are two architectural layers in SDK.</span></span> <span data-ttu-id="f3774-105">Grundläggande hello är hello **IoTHubClient** bibliotek som hanterar direkt kommunikation med IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f3774-105">At hello base is hello **IoTHubClient** library which directly manages communication with IoT Hub.</span></span> <span data-ttu-id="f3774-106">Det finns också hello **serialiseraren** bibliotek som bygger som tooprovide serialisering tjänster.</span><span class="sxs-lookup"><span data-stu-id="f3774-106">There's also hello **serializer** library that builds on top of that tooprovide serialization services.</span></span> <span data-ttu-id="f3774-107">I den här artikeln ska vi ge ytterligare information om hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="f3774-107">In this article we'll provide additional detail on hello **IoTHubClient** library.</span></span>

<span data-ttu-id="f3774-108">hello föregående artikel beskrivs hur toouse hello **IoTHubClient** biblioteket toosend händelser tooIoT hubb och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f3774-108">hello previous article described how toouse hello **IoTHubClient** library toosend events tooIoT Hub and receive messages.</span></span> <span data-ttu-id="f3774-109">Den här artikeln utökar den beskrivning som förklarar hur toomore exakt hantera *när* du skicka och ta emot data, introducerar toohello **lågnivå-API: er**.</span><span class="sxs-lookup"><span data-stu-id="f3774-109">This article extends that discussion by explaining how toomore precisely manage *when* you send and receive data, introducing you toohello **lower-level APIs**.</span></span> <span data-ttu-id="f3774-110">Vi förklarar också hur tooattach egenskaper tooevents (och hämta dem från meddelanden) med hjälp av hello egenskapen hantering av funktioner i hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="f3774-110">We'll also explain how tooattach properties tooevents (and retrieve them from messages) using hello property handling features in hello **IoTHubClient** library.</span></span> <span data-ttu-id="f3774-111">Slutligen vi att ge ytterligare förklaring av olika sätt toohandle meddelanden som tagits emot från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f3774-111">Finally, we'll provide additional explanation of different ways toohandle messages received from IoT Hub.</span></span>

<span data-ttu-id="f3774-112">hello artikel avslutas genom som omfattar ett antal olika ämnen, t.ex. information om autentiseringsuppgifter för enheten och hur toochange hello beteendet för hello **IoTHubClient** via konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="f3774-112">hello article concludes by covering a couple of miscellaneous topics, including more about device credentials and how toochange hello behavior of hello **IoTHubClient** through configuration options.</span></span>

<span data-ttu-id="f3774-113">Vi använder hello **IoTHubClient** SDK exempel tooexplain dessa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f3774-113">We'll use hello **IoTHubClient** SDK samples tooexplain these topics.</span></span> <span data-ttu-id="f3774-114">Om du vill toofollow längs finns hello **iothub\_klienten\_exempel\_http** och **iothub\_klienten\_exempel\_amqp** program som ingår i hello Azure IoT-enhet SDK för C. allt beskrivs i följande avsnitt hello visas i exemplen.</span><span class="sxs-lookup"><span data-stu-id="f3774-114">If you want toofollow along, see hello **iothub\_client\_sample\_http** and **iothub\_client\_sample\_amqp** applications that are included in hello Azure IoT device SDK for C. Everything described in hello following sections is demonstrated in these samples.</span></span>

<span data-ttu-id="f3774-115">Du kan hitta hello [ **Azure IoT-enhet SDK för C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen och visa information om hello API i hello [C API-referens för](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="f3774-115">You can find hello [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of hello API in hello [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="hello-lower-level-apis"></a><span data-ttu-id="f3774-116">hello lågnivå-API: er</span><span class="sxs-lookup"><span data-stu-id="f3774-116">hello lower-level APIs</span></span>
<span data-ttu-id="f3774-117">hello föregående artikel beskrivs hello grundläggande drift av hello **IotHubClient** inom hello kontext för hello **iothub\_klienten\_exempel\_amqp** programmet.</span><span class="sxs-lookup"><span data-stu-id="f3774-117">hello previous article described hello basic operation of hello **IotHubClient** within hello context of hello **iothub\_client\_sample\_amqp** application.</span></span> <span data-ttu-id="f3774-118">Till exempel beskrivs hur tooinitialize hello bibliotek med den här koden.</span><span class="sxs-lookup"><span data-stu-id="f3774-118">For example, it explained how tooinitialize hello library using this code.</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="f3774-119">Här beskrivs också hur funktionsanropet toosend händelser med hjälp av detta.</span><span class="sxs-lookup"><span data-stu-id="f3774-119">It also described how toosend events using this function call.</span></span>

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

<span data-ttu-id="f3774-120">hello artikeln beskrivs också hur tooreceive meddelanden genom att registrera en callback-funktion.</span><span class="sxs-lookup"><span data-stu-id="f3774-120">hello article also described how tooreceive messages by registering a callback function.</span></span>

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

<span data-ttu-id="f3774-121">hello artikel visade också hur toofree resurser med hjälp av kod som hello som följer.</span><span class="sxs-lookup"><span data-stu-id="f3774-121">hello article also showed how toofree resources using code such as hello following.</span></span>

```
IoTHubClient_Destroy(iotHubClientHandle);
```

<span data-ttu-id="f3774-122">Det finns dock tillhörande funktioner tooeach dessa API: er:</span><span class="sxs-lookup"><span data-stu-id="f3774-122">However there are companion functions tooeach of these APIs:</span></span>

* <span data-ttu-id="f3774-123">IoTHubClient\_lla\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="f3774-123">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="f3774-124">IoTHubClient\_lla\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="f3774-124">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="f3774-125">IoTHubClient\_lla\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="f3774-125">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="f3774-126">IoTHubClient\_lla\_förstör</span><span class="sxs-lookup"><span data-stu-id="f3774-126">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="f3774-127">Dessa alla funktionerna inkluderar ”lla” i namnet för hello-API: N.</span><span class="sxs-lookup"><span data-stu-id="f3774-127">These functions all include “LL” in hello API name.</span></span> <span data-ttu-id="f3774-128">Än att är hello parametrar för var och en av dessa funktioner identiska tootheir icke lla motsvarigheter.</span><span class="sxs-lookup"><span data-stu-id="f3774-128">Other than that, hello parameters of each of these functions are identical tootheir non-LL counterparts.</span></span> <span data-ttu-id="f3774-129">Dock skiljer hello av dessa funktioner sig på ett sätt som viktiga.</span><span class="sxs-lookup"><span data-stu-id="f3774-129">However, hello behavior of these functions is different in one important way.</span></span>

<span data-ttu-id="f3774-130">När du anropar **IoTHubClient\_CreateFromConnectionString**, hello underliggande bibliotek skapa en ny tråd som körs i bakgrunden hello.</span><span class="sxs-lookup"><span data-stu-id="f3774-130">When you call **IoTHubClient\_CreateFromConnectionString**, hello underlying libraries create a new thread that runs in hello background.</span></span> <span data-ttu-id="f3774-131">Den här tråden skickar händelser till och tar emot meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f3774-131">This thread sends events to, and receives messages from, IoT Hub.</span></span> <span data-ttu-id="f3774-132">Ingen sådan tråd skapas när du arbetar med hello ”lla” API: er.</span><span class="sxs-lookup"><span data-stu-id="f3774-132">No such thread is created when working with hello "LL" APIs.</span></span> <span data-ttu-id="f3774-133">hello skapa hello bakgrundstråd är bekvämlighet toohello utvecklare.</span><span class="sxs-lookup"><span data-stu-id="f3774-133">hello creation of hello background thread is a convenience toohello developer.</span></span> <span data-ttu-id="f3774-134">Du har inte tooworry om att skicka händelser och ta emot meddelanden från IoT-hubb – det sker automatiskt i bakgrunden hello explicit.</span><span class="sxs-lookup"><span data-stu-id="f3774-134">You don’t have tooworry about explicitly sending events and receiving messages from IoT Hub -- it happens automatically in hello background.</span></span> <span data-ttu-id="f3774-135">Däremot hello ”lla” API: er ger dig explicit kontroll över kommunikation med IoT-hubb, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="f3774-135">In contrast, hello "LL" APIs give you explicit control over communication with IoT Hub, if you need it.</span></span>

<span data-ttu-id="f3774-136">toounderstand bättre, ska vi titta på ett exempel:</span><span class="sxs-lookup"><span data-stu-id="f3774-136">toounderstand this better, let’s look at an example:</span></span>

<span data-ttu-id="f3774-137">När du anropar **IoTHubClient\_SendEventAsync**, vad du faktiskt gör är att placera hello händelse i en buffert.</span><span class="sxs-lookup"><span data-stu-id="f3774-137">When you call **IoTHubClient\_SendEventAsync**, what you're actually doing is putting hello event in a buffer.</span></span> <span data-ttu-id="f3774-138">Hej bakgrundstråd som skapas när du anropar **IoTHubClient\_CreateFromConnectionString** kontinuerligt övervakar bufferten och skickar data den innehåller tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="f3774-138">hello background thread created when you call **IoTHubClient\_CreateFromConnectionString** continually monitors this buffer and sends any data that it contains tooIoT Hub.</span></span> <span data-ttu-id="f3774-139">Detta sker i bakgrunden hello vid hello samma tid som hello huvudtråden utför annat arbete.</span><span class="sxs-lookup"><span data-stu-id="f3774-139">This happens in hello background at hello same time that hello main thread is performing other work.</span></span>

<span data-ttu-id="f3774-140">På liknande sätt, när du registrerar en Återanropsfunktionen för meddelanden med **IoTHubClient\_SetMessageCallback**, du instruerar hello SDK toohave hello bakgrund tråd anropa hello Återanropsfunktionen när ett meddelande är mottagna, oberoende av hello huvudtråden.</span><span class="sxs-lookup"><span data-stu-id="f3774-140">Similarly, when you register a callback function for messages using **IoTHubClient\_SetMessageCallback**, you're instructing hello SDK toohave hello background thread invoke hello callback function when a message is received, independent of hello main thread.</span></span>

<span data-ttu-id="f3774-141">Hej ”lla” API: er skapa inte en bakgrundstråd.</span><span class="sxs-lookup"><span data-stu-id="f3774-141">hello "LL" APIs don’t create a background thread.</span></span> <span data-ttu-id="f3774-142">I stället ett nytt API måste anropas tooexplicitly skicka och ta emot data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f3774-142">Instead, a new API must be called tooexplicitly send and receive data from IoT Hub.</span></span> <span data-ttu-id="f3774-143">Detta visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="f3774-143">This is demonstrated in hello following example.</span></span>

<span data-ttu-id="f3774-144">Hej **iothub\_klienten\_exempel\_http** program som ingår i hello SDK visar hello lågnivå-API: er.</span><span class="sxs-lookup"><span data-stu-id="f3774-144">hello **iothub\_client\_sample\_http** application that’s included in hello SDK demonstrates hello lower-level APIs.</span></span> <span data-ttu-id="f3774-145">I det här exemplet skickar vi händelser tooIoT hubb med kod, till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3774-145">In that sample, we send events tooIoT Hub with code such as hello following:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="f3774-146">hello tre första raderna skapa hello-meddelande och hello sista raden skickar hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="f3774-146">hello first three lines create hello message, and hello last line sends hello event.</span></span> <span data-ttu-id="f3774-147">Dock som tidigare nämnts skicka ”” hello händelse innebär att hello data bara placeras i en buffert.</span><span class="sxs-lookup"><span data-stu-id="f3774-147">However, as mentioned previously, "sending" hello event means that hello data is simply placed in a buffer.</span></span> <span data-ttu-id="f3774-148">Ingenting överförs i nätverket hello när vi kallar **IoTHubClient\_lla\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="f3774-148">Nothing is transmitted on hello network when we call **IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="f3774-149">Du måste anropa i ordning tooactually ingång hello data tooIoT hubb **IoTHubClient\_lla\_DoWork**, som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="f3774-149">In order tooactually ingress hello data tooIoT Hub, you must call **IoTHubClient\_LL\_DoWork**, as in this example:</span></span>

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="f3774-150">Den här koden (från hello **iothub\_klienten\_exempel\_http** program) upprepade gånger **IoTHubClient\_lla\_DoWork** .</span><span class="sxs-lookup"><span data-stu-id="f3774-150">This code (from hello **iothub\_client\_sample\_http** application) repeatedly calls **IoTHubClient\_LL\_DoWork**.</span></span> <span data-ttu-id="f3774-151">Varje gång **IoTHubClient\_lla\_DoWork** är anropas den skickar vissa händelser från hello buffert tooIoT hubb och hämtar ett meddelande som skickas toohello enheten i kön.</span><span class="sxs-lookup"><span data-stu-id="f3774-151">Each time **IoTHubClient\_LL\_DoWork** is called, it sends some events from hello buffer tooIoT Hub and it retrieves a queued message being sent toohello device.</span></span> <span data-ttu-id="f3774-152">hello senare fallet innebär att om vi har registrerat ett Återanropsfunktionen för meddelanden, och sedan hello återanropet anropas (förutsatt att alla meddelanden i kö).</span><span class="sxs-lookup"><span data-stu-id="f3774-152">hello latter case means that if we registered a callback function for messages, then hello callback is invoked (assuming any messages are queued up).</span></span> <span data-ttu-id="f3774-153">Vi skulle har registrerat en Återanropsfunktionen med kod, till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3774-153">We would have registered such a callback function with code such as hello following:</span></span>

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

<span data-ttu-id="f3774-154">hello skäl som **IoTHubClient\_lla\_DoWork** kallas ofta en slinga är att varje gång den anropas, skickar *vissa* buffras händelser tooIoT hubb och hämtar *hello bredvid* meddelandet i kö för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="f3774-154">hello reason that **IoTHubClient\_LL\_DoWork** is often called in a loop is that each time it’s called, it sends *some* buffered events tooIoT Hub and retrieves *hello next* message queued up for hello device.</span></span> <span data-ttu-id="f3774-155">Varje anrop är inte garanterat toosend alla buffrade händelser eller tooretrieve alla köade meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f3774-155">Each call isn’t guaranteed toosend all buffered events or tooretrieve all queued messages.</span></span> <span data-ttu-id="f3774-156">Om du vill toosend alla händelser i hello bufferten och fortsätt sedan med annan bearbetning ersätta du den här loop med kod exempelvis hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3774-156">If you want toosend all events in hello buffer and then continue on with other processing you can replace this loop with code such as hello following:</span></span>

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="f3774-157">Den här koden anropar **IoTHubClient\_lla\_DoWork** förrän alla händelser i hello buffert har skickats tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="f3774-157">This code calls **IoTHubClient\_LL\_DoWork** until all events in hello buffer have been sent tooIoT Hub.</span></span> <span data-ttu-id="f3774-158">Observera att detta inte också innebär att alla köade meddelanden har tagits emot.</span><span class="sxs-lookup"><span data-stu-id="f3774-158">Note this does not also imply that all queued messages have been received.</span></span> <span data-ttu-id="f3774-159">En del av hello anledningen är att kontrollera om ”alla” meddelanden inte deterministisk som en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f3774-159">Part of hello reason for this is that checking for "all" messages isn’t as deterministic an action.</span></span> <span data-ttu-id="f3774-160">Vad händer om du hämtar ”alla” hello meddelanden, men en annan skickas sedan toohello enheten omedelbart efter?</span><span class="sxs-lookup"><span data-stu-id="f3774-160">What happens if you retrieve "all" of hello messages, but then another one is sent toohello device immediately after?</span></span> <span data-ttu-id="f3774-161">Ett bättre sätt toodeal med som är med programmerade timeout.</span><span class="sxs-lookup"><span data-stu-id="f3774-161">A better way toodeal with that is with a programmed timeout.</span></span> <span data-ttu-id="f3774-162">Hello-meddelande Återanropsfunktionen kan återställa en timer varje gång den startas.</span><span class="sxs-lookup"><span data-stu-id="f3774-162">For example, hello message callback function could reset a timer every time it’s invoked.</span></span> <span data-ttu-id="f3774-163">Nu kan du skriva logik toocontinue bearbetning, till exempel inga meddelanden har tagits emot i hello senast *X* sekunder.</span><span class="sxs-lookup"><span data-stu-id="f3774-163">You can then write logic toocontinue processing if, for example, no messages have been received in hello last *X* seconds.</span></span>

<span data-ttu-id="f3774-164">När du är klar ingressing händelser och ta emot meddelanden, vara säker på att toocall hello motsvarande funktionen tooclean resurser.</span><span class="sxs-lookup"><span data-stu-id="f3774-164">When you’re finished ingressing events and receiving messages, be sure toocall hello corresponding function tooclean up resources.</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="f3774-165">Det finns i princip bara en uppsättning API: er toosend och ta emot data med en bakgrundstråd och en annan uppsättning API: er som hello samma sak utan hello bakgrundstråd.</span><span class="sxs-lookup"><span data-stu-id="f3774-165">Basically there’s only one set of APIs toosend and receive data with a background thread and another set of APIs that does hello same thing without hello background thread.</span></span> <span data-ttu-id="f3774-166">Många utvecklare kan också hello icke - lla API: er, men hello lågnivå-API: er är användbara när hello utvecklare vill explicit kontroll över nätverksöverföringar.</span><span class="sxs-lookup"><span data-stu-id="f3774-166">A lot of developers may prefer hello non-LL APIs, but hello lower-level APIs are useful when hello developer wants explicit control over network transmissions.</span></span> <span data-ttu-id="f3774-167">Till exempel vissa enheter samla in data över tid och endast ingångshändelser vid angivna intervall (till exempel när en timme eller en gång om dagen).</span><span class="sxs-lookup"><span data-stu-id="f3774-167">For example, some devices collect data over time and only ingress events at specified intervals (for example, once an hour or once a day).</span></span> <span data-ttu-id="f3774-168">Hej lågnivå-API: er ger du hello möjlighet tooexplicitly kontroll när du skickar och tar emot data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f3774-168">hello lower-level APIs give you hello ability tooexplicitly control when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="f3774-169">Andra kommer bara hellre hello enkelhet som hello lågnivå-API: er tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="f3774-169">Others will simply prefer hello simplicity that hello lower-level APIs provide.</span></span> <span data-ttu-id="f3774-170">Allt som händer på hello huvudtråden i stället för vissa arbete sker i bakgrunden hello.</span><span class="sxs-lookup"><span data-stu-id="f3774-170">Everything happens on hello main thread rather than some work happening in hello background.</span></span>

<span data-ttu-id="f3774-171">Oavsett vilken modell som du väljer vara säker på att toobe konsekvent i vilka API: er som du använder.</span><span class="sxs-lookup"><span data-stu-id="f3774-171">Whichever model you choose, be sure toobe consistent in which APIs you use.</span></span> <span data-ttu-id="f3774-172">Om du startar genom att anropa **IoTHubClient\_lla\_CreateFromConnectionString**, bör du bara använda hello motsvarande lågnivå-API: er för all uppföljning:</span><span class="sxs-lookup"><span data-stu-id="f3774-172">If you start by calling **IoTHubClient\_LL\_CreateFromConnectionString**, be sure you only use hello corresponding lower-level APIs for any follow-up work:</span></span>

* <span data-ttu-id="f3774-173">IoTHubClient\_lla\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="f3774-173">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="f3774-174">IoTHubClient\_lla\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="f3774-174">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="f3774-175">IoTHubClient\_lla\_förstör</span><span class="sxs-lookup"><span data-stu-id="f3774-175">IoTHubClient\_LL\_Destroy</span></span>
* <span data-ttu-id="f3774-176">IoTHubClient\_lla\_DoWork</span><span class="sxs-lookup"><span data-stu-id="f3774-176">IoTHubClient\_LL\_DoWork</span></span>

<span data-ttu-id="f3774-177">hello motsatt gäller också.</span><span class="sxs-lookup"><span data-stu-id="f3774-177">hello opposite is true as well.</span></span> <span data-ttu-id="f3774-178">Om du börjar med **IoTHubClient\_CreateFromConnectionString**, och sedan använda hello icke - lla API: er för några andra processer.</span><span class="sxs-lookup"><span data-stu-id="f3774-178">If you start with **IoTHubClient\_CreateFromConnectionString**, then use hello non-LL APIs for any additional processing.</span></span>

<span data-ttu-id="f3774-179">Hello Azure IoT-enhet SDK för C, se hello **iothub\_klienten\_exempel\_http** program för en komplett exempel på hello lågnivå-API: er.</span><span class="sxs-lookup"><span data-stu-id="f3774-179">In hello Azure IoT device SDK for C, see hello **iothub\_client\_sample\_http** application for a complete example of hello lower-level APIs.</span></span> <span data-ttu-id="f3774-180">Hej **iothub\_klienten\_exempel\_amqp** program kan refereras till en fullständig exempel av hello icke - lla API: er.</span><span class="sxs-lookup"><span data-stu-id="f3774-180">hello **iothub\_client\_sample\_amqp** application can be referenced for a full example of hello non-LL APIs.</span></span>

## <a name="property-handling"></a><span data-ttu-id="f3774-181">Egenskapen hantering</span><span class="sxs-lookup"><span data-stu-id="f3774-181">Property handling</span></span>
<span data-ttu-id="f3774-182">Hittills när vi har beskrivs skickar data, har vi hänvisar toohello brödtext hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="f3774-182">So far when we've described sending data, we've been referring toohello body of hello message.</span></span> <span data-ttu-id="f3774-183">Anta till exempel att den här koden:</span><span class="sxs-lookup"><span data-stu-id="f3774-183">For example, consider this code:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="f3774-184">Det här exemplet skickar ett meddelande tooIoT hubb med hello texten ”Hello World”.</span><span class="sxs-lookup"><span data-stu-id="f3774-184">This example sends a message tooIoT Hub with hello text "Hello World."</span></span> <span data-ttu-id="f3774-185">IoT-hubb kan också egenskaper toobe bifogade tooeach meddelande.</span><span class="sxs-lookup"><span data-stu-id="f3774-185">However, IoT Hub also allows properties toobe attached tooeach message.</span></span> <span data-ttu-id="f3774-186">Egenskaperna är namn/värde-par som kan vara anslutna toohello meddelande.</span><span class="sxs-lookup"><span data-stu-id="f3774-186">Properties are name/value pairs that can be attached toohello message.</span></span> <span data-ttu-id="f3774-187">Vi kan till exempel ändra hello föregående kod tooattach meddelandet egenskapen toohello:</span><span class="sxs-lookup"><span data-stu-id="f3774-187">For example, we can modify hello previous code tooattach a property toohello message:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="f3774-188">Vi börjar genom att anropa **IoTHubMessage\_egenskaper** och passerar den hello referensen för våra meddelandet.</span><span class="sxs-lookup"><span data-stu-id="f3774-188">We start by calling **IoTHubMessage\_Properties** and passing it hello handle of our message.</span></span> <span data-ttu-id="f3774-189">Vad vi komma är en **KARTAN\_hantera** referens som gör att vi toostart att lägga till egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f3774-189">What we get back is a **MAP\_HANDLE** reference that enables us toostart adding properties.</span></span> <span data-ttu-id="f3774-190">hello senare görs genom att anropa **kartan\_AddOrUpdate**, som tar en referens tooa KARTAN\_REFERENSEN hello egenskapsnamn och hello egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="f3774-190">hello latter is accomplished by calling **Map\_AddOrUpdate**, which takes a reference tooa MAP\_HANDLE, hello property name, and hello property value.</span></span> <span data-ttu-id="f3774-191">Vi kan detta API för att lägga till alla egenskaper som vi gärna.</span><span class="sxs-lookup"><span data-stu-id="f3774-191">With this API we can add as many properties as we like.</span></span>

<span data-ttu-id="f3774-192">När hello händelse har lästs från **Händelsehubbar**, hello mottagaren kan räkna upp hello egenskaper och hämta motsvarande värden.</span><span class="sxs-lookup"><span data-stu-id="f3774-192">When hello event is read from **Event Hubs**, hello receiver can enumerate hello properties and retrieve their corresponding values.</span></span> <span data-ttu-id="f3774-193">Till exempel i .NET detta kan åstadkommas genom att öppna hello [egenskapssamlingen på hello EventData objektet](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span><span class="sxs-lookup"><span data-stu-id="f3774-193">For example, in .NET this would be accomplished by accessing hello [Properties collection on hello EventData object](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span></span>

<span data-ttu-id="f3774-194">I föregående exempel hello kopplar vi egenskaper tooan händelse som vi skickar tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="f3774-194">In hello previous example, we’re attaching properties tooan event that we send tooIoT Hub.</span></span> <span data-ttu-id="f3774-195">Egenskaper kan också vara anslutna toomessages togs emot från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f3774-195">Properties can also be attached toomessages received from IoT Hub.</span></span> <span data-ttu-id="f3774-196">Om vi vill tooretrieve egenskaper från ett meddelande kan vi använda koden som hello följande i vår Återanropsfunktionen meddelande:</span><span class="sxs-lookup"><span data-stu-id="f3774-196">If we want tooretrieve properties from a message, we can use code such as hello following in our message callback function:</span></span>

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

<span data-ttu-id="f3774-197">Hej anrop för**IoTHubMessage\_egenskaper** returnerar hello **KARTAN\_hantera** referens.</span><span class="sxs-lookup"><span data-stu-id="f3774-197">hello call too**IoTHubMessage\_Properties** returns hello **MAP\_HANDLE** reference.</span></span> <span data-ttu-id="f3774-198">Vi sedan skicka som referens för**kartan\_GetInternals** tooobtain ett referens tooan matris av hello namn/värde-par (samt antalet hello egenskaper).</span><span class="sxs-lookup"><span data-stu-id="f3774-198">We then pass that reference too**Map\_GetInternals** tooobtain a reference tooan array of hello name/value pairs (as well as a count of hello properties).</span></span> <span data-ttu-id="f3774-199">Då är det en enkel fråga om uppräkning hello tooget toohello egenskapsvärden vi vill.</span><span class="sxs-lookup"><span data-stu-id="f3774-199">At that point it's a simple matter of enumerating hello properties tooget toohello values we want.</span></span>

<span data-ttu-id="f3774-200">Du har inte toouse egenskaper i ditt program.</span><span class="sxs-lookup"><span data-stu-id="f3774-200">You don't have toouse properties in your application.</span></span> <span data-ttu-id="f3774-201">Men om du behöver tooset dem på händelser eller hämta dem från meddelanden, hello **IoTHubClient** biblioteket kan du enkelt.</span><span class="sxs-lookup"><span data-stu-id="f3774-201">However, if you need tooset them on events or retrieve them from messages, hello **IoTHubClient** library makes it easy.</span></span>

## <a name="message-handling"></a><span data-ttu-id="f3774-202">Meddelandehantering</span><span class="sxs-lookup"><span data-stu-id="f3774-202">Message handling</span></span>
<span data-ttu-id="f3774-203">Som nämnts tidigare anger när meddelanden tas emot från IoT-hubb hello **IoTHubClient** biblioteket svarar genom att aktivera registrerade Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="f3774-203">As stated previously, when messages arrive from IoT Hub hello **IoTHubClient** library responds by invoking a registered callback function.</span></span> <span data-ttu-id="f3774-204">Det finns en returparameter av den här funktionen som ska ha vissa ytterligare förklaring.</span><span class="sxs-lookup"><span data-stu-id="f3774-204">There is a return parameter of this function that deserves some additional explanation.</span></span> <span data-ttu-id="f3774-205">Här är ett utdrag ur hello Återanropsfunktionen i hello **iothub\_klienten\_exempel\_http** exempelprogrammet:</span><span class="sxs-lookup"><span data-stu-id="f3774-205">Here’s an excerpt of hello callback function in hello **iothub\_client\_sample\_http** sample application:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

<span data-ttu-id="f3774-206">Observera att hello returtyp är **IOTHUBMESSAGE\_DISPOSITION\_resultatet** och i detta fall returnerar vi **IOTHUBMESSAGE\_godkända**.</span><span class="sxs-lookup"><span data-stu-id="f3774-206">Note that hello return type is **IOTHUBMESSAGE\_DISPOSITION\_RESULT** and in this particular case we return **IOTHUBMESSAGE\_ACCEPTED**.</span></span> <span data-ttu-id="f3774-207">Det finns andra värden som vi kan returnera från den här funktionen att ändra hur hello **IoTHubClient** biblioteket reagerar toohello meddelande återanrop.</span><span class="sxs-lookup"><span data-stu-id="f3774-207">There are other values we can return from this function that change how hello **IoTHubClient** library reacts toohello message callback.</span></span> <span data-ttu-id="f3774-208">Här följer hello alternativ.</span><span class="sxs-lookup"><span data-stu-id="f3774-208">Here are hello options.</span></span>

* <span data-ttu-id="f3774-209">**IOTHUBMESSAGE\_godkända** – hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="f3774-209">**IOTHUBMESSAGE\_ACCEPTED** – hello message has been processed successfully.</span></span> <span data-ttu-id="f3774-210">Hej **IoTHubClient** biblioteket kommer inte att anropa hello Återanropsfunktionen igen med hello samma meddelande.</span><span class="sxs-lookup"><span data-stu-id="f3774-210">hello **IoTHubClient** library will not invoke hello callback function again with hello same message.</span></span>
* <span data-ttu-id="f3774-211">**IOTHUBMESSAGE\_Avvisad** – hello-meddelande kunde inte bearbetas och det finns inga önskan toodo hello i framtida.</span><span class="sxs-lookup"><span data-stu-id="f3774-211">**IOTHUBMESSAGE\_REJECTED** – hello message was not processed and there is no desire toodo so in hello future.</span></span> <span data-ttu-id="f3774-212">Hej **IoTHubClient** biblioteket ska inte anropa hello Återanropsfunktionen igen med hello samma meddelande.</span><span class="sxs-lookup"><span data-stu-id="f3774-212">hello **IoTHubClient** library should not invoke hello callback function again with hello same message.</span></span>
* <span data-ttu-id="f3774-213">**IOTHUBMESSAGE\_ABANDONED** – hello-meddelande har inte bearbetats men hello **IoTHubClient** biblioteket ska anropa hello Återanropsfunktionen igen med hello samma meddelande.</span><span class="sxs-lookup"><span data-stu-id="f3774-213">**IOTHUBMESSAGE\_ABANDONED** – hello message was not processed successfully, but hello **IoTHubClient** library should invoke hello callback function again with hello same message.</span></span>

<span data-ttu-id="f3774-214">För hello först två returkoder, hello **IoTHubClient** biblioteket skickar ett meddelande tooIoT hubb som anger att hello-meddelande bör tas bort från kön av hello enhet och inte levereras igen.</span><span class="sxs-lookup"><span data-stu-id="f3774-214">For hello first two return codes, hello **IoTHubClient** library sends a message tooIoT Hub indicating that hello message should be deleted from hello device queue and not delivered again.</span></span> <span data-ttu-id="f3774-215">hello net effekt är hello samma (hello-meddelande tas bort från kön för hello-enhet), men om hello-meddelande godkännas eller avvisas fortfarande har registrerats.</span><span class="sxs-lookup"><span data-stu-id="f3774-215">hello net effect is hello same (hello message is deleted from hello device queue), but whether hello message was accepted or rejected is still recorded.</span></span>  <span data-ttu-id="f3774-216">Registrera denna skillnad är användbara toosenders av hello-meddelande som kan lyssna efter feedback och ta reda på om en enhet har godkänt eller avvisade ett visst meddelande.</span><span class="sxs-lookup"><span data-stu-id="f3774-216">Recording this distinction is useful toosenders of hello message who can listen for feedback and find out if a device has accepted or rejected a particular message.</span></span>

<span data-ttu-id="f3774-217">I hello senaste fall skickas även ett meddelande tooIoT Hub, men det anger att hello-meddelande som ska levereras.</span><span class="sxs-lookup"><span data-stu-id="f3774-217">In hello last case a message is also sent tooIoT Hub, but it indicates that hello message should be redelivered.</span></span> <span data-ttu-id="f3774-218">Du kommer normalt Avbryt ett meddelande om du påträffar ett fel men vill tootry tooprocess hello meddelandet igen.</span><span class="sxs-lookup"><span data-stu-id="f3774-218">Typically you’ll abandon a message if you encounter some error but want tootry tooprocess hello message again.</span></span> <span data-ttu-id="f3774-219">Däremot är avvisa meddelandet lämplig när det uppstår ett oåterkalleligt fel (eller om du bara väljer du inte vill tooprocess hello-meddelande).</span><span class="sxs-lookup"><span data-stu-id="f3774-219">In contrast, rejecting a message is appropriate when you encounter an unrecoverable error (or if you simply decide you don’t want tooprocess hello message).</span></span>

<span data-ttu-id="f3774-220">Dock vara medveten om hello olika returkoder så att du kan framkalla hello beteende från hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="f3774-220">In any case, be aware of hello different return codes so that you can elicit hello behavior you want from hello **IoTHubClient** library.</span></span>

## <a name="alternate-device-credentials"></a><span data-ttu-id="f3774-221">Autentiseringsuppgifter för annan enhet</span><span class="sxs-lookup"><span data-stu-id="f3774-221">Alternate device credentials</span></span>
<span data-ttu-id="f3774-222">Som tidigare förklarats hello först öppna toodo när du arbetar med hello **IoTHubClient** biblioteket är tooobtain en **IOTHUB\_klienten\_hantera** med ett anrop till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="f3774-222">As explained previously, hello first thing toodo when working with hello **IoTHubClient** library is tooobtain a **IOTHUB\_CLIENT\_HANDLE** with a call such as hello following:</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="f3774-223">Hej argument för**IoTHubClient\_CreateFromConnectionString** hello anslutningssträngen för enheten och en parameter som anger hello-protokollet som vi använder toocommunicate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="f3774-223">hello arguments too**IoTHubClient\_CreateFromConnectionString** are hello device connection string and a parameter that indicates hello protocol we use toocommunicate with IoT Hub.</span></span> <span data-ttu-id="f3774-224">anslutningssträngen för hello enheten har ett format som visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f3774-224">hello device connection string has a format that appears as follows:</span></span>

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

<span data-ttu-id="f3774-225">Det finns fyra typer av information i den här strängen: IoT-hubb namn, IoT-hubb suffix, enhets-ID och nyckeln för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f3774-225">There are four pieces of information in this string: IoT Hub name, IoT Hub suffix, device ID, and shared access key.</span></span> <span data-ttu-id="f3774-226">Du kan hämta hello fullständigt kvalificerade domännamnet (FQDN) för en IoT-hubb när du skapar din IoT hub-instans i hello Azure-portalen – detta ger dig hello IoT-hubbnamnet (hello första delen av hello FQDN) och hello IoT hub-suffix (hello övriga hello FQDN).</span><span class="sxs-lookup"><span data-stu-id="f3774-226">You obtain hello fully qualified domain name (FQDN) of an IoT hub when you create your IoT hub instance in hello Azure portal — this gives you hello IoT hub name (hello first part of hello FQDN) and hello IoT hub suffix (hello rest of hello FQDN).</span></span> <span data-ttu-id="f3774-227">Du får hello enhets-ID och hello delade åtkomstnyckeln när du registrerar din enhet med IoT-hubben (enligt beskrivningen i hello [föregående artikel](iot-hub-device-sdk-c-intro.md)).</span><span class="sxs-lookup"><span data-stu-id="f3774-227">You get hello device ID and hello shared access key when you register your device with IoT Hub (as described in hello [previous article](iot-hub-device-sdk-c-intro.md)).</span></span>

<span data-ttu-id="f3774-228">**IoTHubClient\_CreateFromConnectionString** ger dig ett sätt tooinitialize hello-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="f3774-228">**IoTHubClient\_CreateFromConnectionString** gives you one way tooinitialize hello library.</span></span> <span data-ttu-id="f3774-229">Om du vill kan du skapa en ny **IOTHUB\_klienten\_hantera** med hjälp av dessa enskilda parametrar i stället för hello anslutningssträngen för enheten.</span><span class="sxs-lookup"><span data-stu-id="f3774-229">If you prefer, you can create a new **IOTHUB\_CLIENT\_HANDLE** by using these individual parameters rather than hello device connection string.</span></span> <span data-ttu-id="f3774-230">Detta uppnås med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="f3774-230">This is achieved with hello following code:</span></span>

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

<span data-ttu-id="f3774-231">Detta åstadkommer hello samma sak som **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="f3774-231">This accomplishes hello same thing as **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="f3774-232">Det kan verka uppenbara som du vill ha toouse **IoTHubClient\_CreateFromConnectionString** i stället för den här mer utförlig metoden för initiering.</span><span class="sxs-lookup"><span data-stu-id="f3774-232">It may seem obvious that you would want toouse **IoTHubClient\_CreateFromConnectionString** rather than this more verbose method of initialization.</span></span> <span data-ttu-id="f3774-233">Kom ihåg att när du registrerar en enhet i IoT-hubb vad du får är en enhets-ID och nyckeln för enheten (inte en anslutningssträng).</span><span class="sxs-lookup"><span data-stu-id="f3774-233">Keep in mind, however, that when you register a device in IoT Hub what you get is a device ID and device key (not a connection string).</span></span> <span data-ttu-id="f3774-234">Hej *enheten explorer* SDK-verktyg som introducerades i hello [föregående artikel](iot-hub-device-sdk-c-intro.md) använder libraries i hello **Azure IoT service SDK** toocreate hello enheten anslutningssträng från hello enhets-ID, enhetsnyckel och IoT Hub-värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="f3774-234">hello *device explorer* SDK tool introduced in hello [previous article](iot-hub-device-sdk-c-intro.md) uses libraries in hello **Azure IoT service SDK** toocreate hello device connection string from hello device ID, device key, and IoT Hub host name.</span></span> <span data-ttu-id="f3774-235">Anropar så **IoTHubClient\_lla\_skapa** kan det vara bättre eftersom du sparar du hello steg genererar en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="f3774-235">So calling **IoTHubClient\_LL\_Create** may be preferable because it saves you hello step of generating a connection string.</span></span> <span data-ttu-id="f3774-236">Använd metoden som är lämplig.</span><span class="sxs-lookup"><span data-stu-id="f3774-236">Use whichever method is convenient.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="f3774-237">Konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="f3774-237">Configuration options</span></span>
<span data-ttu-id="f3774-238">Hittills allt beskrivs om hello sätt hello **IoTHubClient** biblioteket fungerar visar dess standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="f3774-238">So far everything described about hello way hello **IoTHubClient** library works reflects its default behavior.</span></span> <span data-ttu-id="f3774-239">Det finns dock några alternativ som du kan ange toochange hur hello biblioteket fungerar.</span><span class="sxs-lookup"><span data-stu-id="f3774-239">However, there are a few options that you can set toochange how hello library works.</span></span> <span data-ttu-id="f3774-240">Detta görs genom att utnyttja hello **IoTHubClient\_lla\_SetOption** API.</span><span class="sxs-lookup"><span data-stu-id="f3774-240">This is accomplished by leveraging hello **IoTHubClient\_LL\_SetOption** API.</span></span> <span data-ttu-id="f3774-241">Överväg att det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="f3774-241">Consider this example:</span></span>

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

<span data-ttu-id="f3774-242">Det finns ett par olika alternativ som är vanliga:</span><span class="sxs-lookup"><span data-stu-id="f3774-242">There are a couple of options that are commonly used:</span></span>

* <span data-ttu-id="f3774-243">**SetBatching** (booleskt) – om **SANT**, som sedan skickas data som skickas tooIoT hubb i batchar.</span><span class="sxs-lookup"><span data-stu-id="f3774-243">**SetBatching** (bool) – If **true**, then data sent tooIoT Hub is sent in batches.</span></span> <span data-ttu-id="f3774-244">Om **FALSKT**, och sedan skickas meddelanden individuellt.</span><span class="sxs-lookup"><span data-stu-id="f3774-244">If **false**, then messages are sent individually.</span></span> <span data-ttu-id="f3774-245">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="f3774-245">hello default is **false**.</span></span> <span data-ttu-id="f3774-246">Observera att hello **SetBatching** alternativet gäller bara toohello HTTP-protokollet och inte toohello MQTT eller AMQP-protokoll.</span><span class="sxs-lookup"><span data-stu-id="f3774-246">Note that hello **SetBatching** option only applies toohello HTTP protocol and not toohello MQTT or AMQP protocols.</span></span>
* <span data-ttu-id="f3774-247">**Timeout** (osignerade int) – det här värdet anges i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="f3774-247">**Timeout** (unsigned int) – This value is represented in milliseconds.</span></span> <span data-ttu-id="f3774-248">Om du skickar en HTTP-begäran eller svar erhålls tar längre tid än den angivna tiden och sedan hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="f3774-248">If sending an HTTP request or receiving a response takes longer than this time, then hello connection times out.</span></span>

<span data-ttu-id="f3774-249">hello batchbearbetning alternativet är viktigt.</span><span class="sxs-lookup"><span data-stu-id="f3774-249">hello batching option is important.</span></span> <span data-ttu-id="f3774-250">Som standard hello ingresses händelser individuellt (en händelse är allt du skickar för**IoTHubClient\_lla\_SendEventAsync**).</span><span class="sxs-lookup"><span data-stu-id="f3774-250">By default, hello library ingresses events individually (a single event is whatever you pass too**IoTHubClient\_LL\_SendEventAsync**).</span></span> <span data-ttu-id="f3774-251">Om hello batchbearbetning alternativet **SANT**, hello bibliotek samlar in så många händelser som går från hello bufferten (upp toohello maximal meddelandestorlek som accepterar IoT-hubb).</span><span class="sxs-lookup"><span data-stu-id="f3774-251">If hello batching option is **true**, hello library collects as many events as it can from hello buffer (up toohello maximum message size that IoT Hub will accept).</span></span>  <span data-ttu-id="f3774-252">hello händelse batch skickas tooIoT hubb i en enda http-anrop (hello enskilda händelser slås ihop till en JSON-matris).</span><span class="sxs-lookup"><span data-stu-id="f3774-252">hello event batch is sent tooIoT Hub in a single HTTP call (hello individual events are bundled into a JSON array).</span></span> <span data-ttu-id="f3774-253">Aktivera batchbearbetning vanligtvis resulterar i stora prestandavinster eftersom du är att minska nätverket turer.</span><span class="sxs-lookup"><span data-stu-id="f3774-253">Enabling batching typically results in big performance gains since you’re reducing network round-trips.</span></span> <span data-ttu-id="f3774-254">Det minskar också avsevärt bandbredd eftersom du skickar en uppsättning av HTTP-huvuden med en händelse batch i stället för en uppsättning huvuden för varje enskild händelse.</span><span class="sxs-lookup"><span data-stu-id="f3774-254">It also significantly reduces bandwidth since you are sending one set of HTTP headers with an event batch rather than a set of headers for each individual event.</span></span> <span data-ttu-id="f3774-255">Om du inte har en särskild anledning toodo annars, normalt ska du tooenable batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="f3774-255">Unless you have a specific reason toodo otherwise, typically you’ll want tooenable batching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3774-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f3774-256">Next steps</span></span>
<span data-ttu-id="f3774-257">Den här artikeln beskrivs i detalj hello funktionssätt hello **IoTHubClient** biblioteket hittades i hello **Azure IoT-enhet SDK för C**. Med den här informationen bör du ha en god förståelse av hello funktionerna i hello **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="f3774-257">This article describes in detail hello behavior of hello **IoTHubClient** library found in hello **Azure IoT device SDK for C**. With this information, you should have a good understanding of hello capabilities of hello **IoTHubClient** library.</span></span> <span data-ttu-id="f3774-258">Hej [nästa artikel](iot-hub-device-sdk-c-serializer.md) innehåller liknande information om hello **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="f3774-258">hello [next article](iot-hub-device-sdk-c-serializer.md) provides similar detail on hello **serializer** library.</span></span>

<span data-ttu-id="f3774-259">toolearn mer information om hur du utvecklar för IoT-hubb finns hello [Azure IoT SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="f3774-259">toolearn more about developing for IoT Hub, see hello [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="f3774-260">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="f3774-260">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="f3774-261">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="f3774-261">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
