---
title: "Azure IoT-enhet SDK för C - IoTHubClient | Microsoft Docs"
description: "Hur du använder IoTHubClient biblioteket i Azure IoT-enhet SDK för C för att skapa appar för enheter som kommunicerar med en IoT-hubb."
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
ms.openlocfilehash: 422d89014511f0d08ba57a893570ff7b253b7bc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a><span data-ttu-id="fd42e-103">Azure IoT-enhet SDK för C – mer information om IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="fd42e-103">Azure IoT device SDK for C – more about IoTHubClient</span></span>
<span data-ttu-id="fd42e-104">Den [först artikel](iot-hub-device-sdk-c-intro.md) i den här serien introduceras de **Azure IoT-enhet SDK för C**. Artikeln förklaras att det finns två arkitektur lager i SDK.</span><span class="sxs-lookup"><span data-stu-id="fd42e-104">The [first article](iot-hub-device-sdk-c-intro.md) in this series introduced the **Azure IoT device SDK for C**. That article explained that there are two architectural layers in SDK.</span></span> <span data-ttu-id="fd42e-105">I grunden är den **IoTHubClient** bibliotek som hanterar direkt kommunikation med IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd42e-105">At the base is the **IoTHubClient** library which directly manages communication with IoT Hub.</span></span> <span data-ttu-id="fd42e-106">Det finns också i **serialiseraren** bibliotek som bygger som för att tillhandahålla tjänster för serialisering.</span><span class="sxs-lookup"><span data-stu-id="fd42e-106">There's also the **serializer** library that builds on top of that to provide serialization services.</span></span> <span data-ttu-id="fd42e-107">I den här artikeln ska vi ge ytterligare information om den **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="fd42e-107">In this article we'll provide additional detail on the **IoTHubClient** library.</span></span>

<span data-ttu-id="fd42e-108">Föregående artikel beskrivs hur du använder den **IoTHubClient** biblioteket för att skicka händelser till IoT-hubb och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="fd42e-108">The previous article described how to use the **IoTHubClient** library to send events to IoT Hub and receive messages.</span></span> <span data-ttu-id="fd42e-109">Den här artikeln utökar den beskrivning som förklarar hur du hanterar mer exakt *när* du skicka och ta emot data, introducerar du den **lågnivå-API: er**.</span><span class="sxs-lookup"><span data-stu-id="fd42e-109">This article extends that discussion by explaining how to more precisely manage *when* you send and receive data, introducing you to the **lower-level APIs**.</span></span> <span data-ttu-id="fd42e-110">Vi förklarar också hur du ansluter egenskaper på händelser (och hämta dem från meddelanden) med hjälp av egenskapen hantering av funktioner i den **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="fd42e-110">We'll also explain how to attach properties to events (and retrieve them from messages) using the property handling features in the **IoTHubClient** library.</span></span> <span data-ttu-id="fd42e-111">Slutligen ska vi ger ytterligare förklaring av olika sätt att hantera meddelanden som tagits emot från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd42e-111">Finally, we'll provide additional explanation of different ways to handle messages received from IoT Hub.</span></span>

<span data-ttu-id="fd42e-112">Artikeln avslutas genom som omfattar ett antal olika ämnen, t.ex. mer om autentiseringsuppgifter för enheten och hur du ändrar beteendet för den **IoTHubClient** via konfigurationsalternativ.</span><span class="sxs-lookup"><span data-stu-id="fd42e-112">The article concludes by covering a couple of miscellaneous topics, including more about device credentials and how to change the behavior of the **IoTHubClient** through configuration options.</span></span>

<span data-ttu-id="fd42e-113">Vi använder den **IoTHubClient** SDK-exempel som förklarar dessa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fd42e-113">We'll use the **IoTHubClient** SDK samples to explain these topics.</span></span> <span data-ttu-id="fd42e-114">Om du vill följa med finns i **iothub\_klienten\_exempel\_http** och **iothub\_klienten\_exempel\_amqp**program som ingår i Azure IoT-enhet SDK för C. allt beskrivs i följande avsnitt visas i exemplen.</span><span class="sxs-lookup"><span data-stu-id="fd42e-114">If you want to follow along, see the **iothub\_client\_sample\_http** and **iothub\_client\_sample\_amqp** applications that are included in the Azure IoT device SDK for C. Everything described in the following sections is demonstrated in these samples.</span></span>

<span data-ttu-id="fd42e-115">Du hittar den [ **Azure IoT-enhet SDK för C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen och visa information om API i den [C API-referens för](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="fd42e-115">You can find the [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of the API in the [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="the-lower-level-apis"></a><span data-ttu-id="fd42e-116">Lågnivå-API: er</span><span class="sxs-lookup"><span data-stu-id="fd42e-116">The lower-level APIs</span></span>
<span data-ttu-id="fd42e-117">Föregående artikel beskrivs grundläggande funktioner i den **IotHubClient** inom ramen för den **iothub\_klienten\_exempel\_amqp** program.</span><span class="sxs-lookup"><span data-stu-id="fd42e-117">The previous article described the basic operation of the **IotHubClient** within the context of the **iothub\_client\_sample\_amqp** application.</span></span> <span data-ttu-id="fd42e-118">Till exempel förklaras den hur du initierar biblioteket med följande kod.</span><span class="sxs-lookup"><span data-stu-id="fd42e-118">For example, it explained how to initialize the library using this code.</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="fd42e-119">Det beskrivs även hur du skickar händelser med hjälp av den här funktionsanrop.</span><span class="sxs-lookup"><span data-stu-id="fd42e-119">It also described how to send events using this function call.</span></span>

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

<span data-ttu-id="fd42e-120">Artikeln beskrivs även ta emot meddelanden genom att registrera en callback-funktion.</span><span class="sxs-lookup"><span data-stu-id="fd42e-120">The article also described how to receive messages by registering a callback function.</span></span>

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

<span data-ttu-id="fd42e-121">Artikeln visade också hur du frigör resurser med hjälp av koden till exempel följande.</span><span class="sxs-lookup"><span data-stu-id="fd42e-121">The article also showed how to free resources using code such as the following.</span></span>

```
IoTHubClient_Destroy(iotHubClientHandle);
```

<span data-ttu-id="fd42e-122">Det finns dock tillhörande funktioner för var och en av dessa API: er:</span><span class="sxs-lookup"><span data-stu-id="fd42e-122">However there are companion functions to each of these APIs:</span></span>

* <span data-ttu-id="fd42e-123">IoTHubClient\_lla\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="fd42e-123">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="fd42e-124">IoTHubClient\_lla\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="fd42e-124">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="fd42e-125">IoTHubClient\_lla\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="fd42e-125">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="fd42e-126">IoTHubClient\_lla\_förstör</span><span class="sxs-lookup"><span data-stu-id="fd42e-126">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="fd42e-127">Dessa funktioner som alla inkludera ”lla” i API-namnet.</span><span class="sxs-lookup"><span data-stu-id="fd42e-127">These functions all include “LL” in the API name.</span></span> <span data-ttu-id="fd42e-128">Parametrarna för var och en av dessa funktioner är identiska med deras motsvarigheter i icke-lla än det som.</span><span class="sxs-lookup"><span data-stu-id="fd42e-128">Other than that, the parameters of each of these functions are identical to their non-LL counterparts.</span></span> <span data-ttu-id="fd42e-129">Beteendet för dessa funktioner är dock olika på ett sätt som viktiga.</span><span class="sxs-lookup"><span data-stu-id="fd42e-129">However, the behavior of these functions is different in one important way.</span></span>

<span data-ttu-id="fd42e-130">När du anropar **IoTHubClient\_CreateFromConnectionString**, underliggande bibliotek skapa en ny tråd som körs i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="fd42e-130">When you call **IoTHubClient\_CreateFromConnectionString**, the underlying libraries create a new thread that runs in the background.</span></span> <span data-ttu-id="fd42e-131">Den här tråden skickar händelser till och tar emot meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd42e-131">This thread sends events to, and receives messages from, IoT Hub.</span></span> <span data-ttu-id="fd42e-132">Ingen sådan tråd skapas när du arbetar med ”lla” API: er.</span><span class="sxs-lookup"><span data-stu-id="fd42e-132">No such thread is created when working with the "LL" APIs.</span></span> <span data-ttu-id="fd42e-133">Bakgrundstråden skapas i syfte att underlätta till utvecklaren.</span><span class="sxs-lookup"><span data-stu-id="fd42e-133">The creation of the background thread is a convenience to the developer.</span></span> <span data-ttu-id="fd42e-134">Du behöver inte bry dig om uttryckligen att skicka händelser och ta emot meddelanden från IoT-hubb – det sker automatiskt i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="fd42e-134">You don’t have to worry about explicitly sending events and receiving messages from IoT Hub -- it happens automatically in the background.</span></span> <span data-ttu-id="fd42e-135">Däremot ger ”lla” API: er dig explicit kontroll över kommunikation med IoT-hubb, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="fd42e-135">In contrast, the "LL" APIs give you explicit control over communication with IoT Hub, if you need it.</span></span>

<span data-ttu-id="fd42e-136">För att förstå det bättre ska vi titta på ett exempel:</span><span class="sxs-lookup"><span data-stu-id="fd42e-136">To understand this better, let’s look at an example:</span></span>

<span data-ttu-id="fd42e-137">När du anropar **IoTHubClient\_SendEventAsync**, vad du faktiskt gör är att placera händelsen i en buffert.</span><span class="sxs-lookup"><span data-stu-id="fd42e-137">When you call **IoTHubClient\_SendEventAsync**, what you're actually doing is putting the event in a buffer.</span></span> <span data-ttu-id="fd42e-138">Bakgrundstråden som skapas när du anropar **IoTHubClient\_CreateFromConnectionString** kontinuerligt övervakar bufferten och skickar data som den innehåller IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="fd42e-138">The background thread created when you call **IoTHubClient\_CreateFromConnectionString** continually monitors this buffer and sends any data that it contains to IoT Hub.</span></span> <span data-ttu-id="fd42e-139">Detta sker i bakgrunden på samma gång huvudtråden utförs annat arbete.</span><span class="sxs-lookup"><span data-stu-id="fd42e-139">This happens in the background at the same time that the main thread is performing other work.</span></span>

<span data-ttu-id="fd42e-140">På liknande sätt, när du registrerar en Återanropsfunktionen för meddelanden med **IoTHubClient\_SetMessageCallback**, du instruerar SDK om du vill ha bakgrundstråd anropa Återanropsfunktionen när ett meddelande är mottagna, oberoende av huvudtråden.</span><span class="sxs-lookup"><span data-stu-id="fd42e-140">Similarly, when you register a callback function for messages using **IoTHubClient\_SetMessageCallback**, you're instructing the SDK to have the background thread invoke the callback function when a message is received, independent of the main thread.</span></span>

<span data-ttu-id="fd42e-141">”DU” API: er skapa inte en bakgrundstråd.</span><span class="sxs-lookup"><span data-stu-id="fd42e-141">The "LL" APIs don’t create a background thread.</span></span> <span data-ttu-id="fd42e-142">I stället måste ett nytt API anropas för att skicka och ta emot data från IoT-hubb explicit.</span><span class="sxs-lookup"><span data-stu-id="fd42e-142">Instead, a new API must be called to explicitly send and receive data from IoT Hub.</span></span> <span data-ttu-id="fd42e-143">Detta visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="fd42e-143">This is demonstrated in the following example.</span></span>

<span data-ttu-id="fd42e-144">Den **iothub\_klienten\_exempel\_http** program som ingår i SDK visar på lägre nivå-API: er.</span><span class="sxs-lookup"><span data-stu-id="fd42e-144">The **iothub\_client\_sample\_http** application that’s included in the SDK demonstrates the lower-level APIs.</span></span> <span data-ttu-id="fd42e-145">I det här exemplet skicka vi händelser till IoT-hubb med kod till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="fd42e-145">In that sample, we send events to IoT Hub with code such as the following:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="fd42e-146">De tre första raderna skapa meddelandet och den sista raden skickar händelsen.</span><span class="sxs-lookup"><span data-stu-id="fd42e-146">The first three lines create the message, and the last line sends the event.</span></span> <span data-ttu-id="fd42e-147">Dock som tidigare nämnts skicka ”” händelsen innebär att data bara placeras i en buffert.</span><span class="sxs-lookup"><span data-stu-id="fd42e-147">However, as mentioned previously, "sending" the event means that the data is simply placed in a buffer.</span></span> <span data-ttu-id="fd42e-148">Ingenting överförs i nätverket när vi kallar **IoTHubClient\_lla\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="fd42e-148">Nothing is transmitted on the network when we call **IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="fd42e-149">Du måste anropa för att faktiskt ingång data till IoT-hubb **IoTHubClient\_lla\_DoWork**, som i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="fd42e-149">In order to actually ingress the data to IoT Hub, you must call **IoTHubClient\_LL\_DoWork**, as in this example:</span></span>

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="fd42e-150">Den här koden (från den **iothub\_klienten\_exempel\_http** program) upprepade gånger **IoTHubClient\_lla\_DoWork**.</span><span class="sxs-lookup"><span data-stu-id="fd42e-150">This code (from the **iothub\_client\_sample\_http** application) repeatedly calls **IoTHubClient\_LL\_DoWork**.</span></span> <span data-ttu-id="fd42e-151">Varje gång **IoTHubClient\_lla\_DoWork** är anropas den skickar vissa händelser från bufferten för IoT-hubb och hämtar en köade meddelandet som skickas till enheten.</span><span class="sxs-lookup"><span data-stu-id="fd42e-151">Each time **IoTHubClient\_LL\_DoWork** is called, it sends some events from the buffer to IoT Hub and it retrieves a queued message being sent to the device.</span></span> <span data-ttu-id="fd42e-152">Det senare fallet innebär att om vi registrerat ett Återanropsfunktionen för meddelanden, sedan återanropet anropas (förutsatt att alla meddelanden i kö).</span><span class="sxs-lookup"><span data-stu-id="fd42e-152">The latter case means that if we registered a callback function for messages, then the callback is invoked (assuming any messages are queued up).</span></span> <span data-ttu-id="fd42e-153">Vi skulle har registrerat en Återanropsfunktionen med kod till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="fd42e-153">We would have registered such a callback function with code such as the following:</span></span>

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

<span data-ttu-id="fd42e-154">Orsaken som **IoTHubClient\_lla\_DoWork** kallas ofta en slinga är att varje gång den anropas, skickar *vissa* buffras händelser som IoT-hubb och hämtar *nästa* meddelandet i kö för enheten.</span><span class="sxs-lookup"><span data-stu-id="fd42e-154">The reason that **IoTHubClient\_LL\_DoWork** is often called in a loop is that each time it’s called, it sends *some* buffered events to IoT Hub and retrieves *the next* message queued up for the device.</span></span> <span data-ttu-id="fd42e-155">Varje anrop är inte säkert att skicka alla buffertlagrade händelser eller om du vill hämta alla meddelanden i kön.</span><span class="sxs-lookup"><span data-stu-id="fd42e-155">Each call isn’t guaranteed to send all buffered events or to retrieve all queued messages.</span></span> <span data-ttu-id="fd42e-156">Om du vill skicka alla händelser i bufferten och fortsätt sedan med annan bearbetning ersätta du den här loop med kod till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="fd42e-156">If you want to send all events in the buffer and then continue on with other processing you can replace this loop with code such as the following:</span></span>

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="fd42e-157">Den här koden anropar **IoTHubClient\_lla\_DoWork** tills alla händelser i bufferten har skickats till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd42e-157">This code calls **IoTHubClient\_LL\_DoWork** until all events in the buffer have been sent to IoT Hub.</span></span> <span data-ttu-id="fd42e-158">Observera att detta inte också innebär att alla köade meddelanden har tagits emot.</span><span class="sxs-lookup"><span data-stu-id="fd42e-158">Note this does not also imply that all queued messages have been received.</span></span> <span data-ttu-id="fd42e-159">En del av anledningen till detta är att kontrollera om ”alla” meddelanden inte deterministisk som en åtgärd.</span><span class="sxs-lookup"><span data-stu-id="fd42e-159">Part of the reason for this is that checking for "all" messages isn’t as deterministic an action.</span></span> <span data-ttu-id="fd42e-160">Vad händer om du hämtar ”alla” meddelanden, men en annan skickas sedan till enheten omedelbart efter?</span><span class="sxs-lookup"><span data-stu-id="fd42e-160">What happens if you retrieve "all" of the messages, but then another one is sent to the device immediately after?</span></span> <span data-ttu-id="fd42e-161">Ett bättre sätt att åtgärda som med programmerade timeout.</span><span class="sxs-lookup"><span data-stu-id="fd42e-161">A better way to deal with that is with a programmed timeout.</span></span> <span data-ttu-id="fd42e-162">Återanropsfunktionen meddelande kunde återställa en timer varje gång den startas.</span><span class="sxs-lookup"><span data-stu-id="fd42e-162">For example, the message callback function could reset a timer every time it’s invoked.</span></span> <span data-ttu-id="fd42e-163">Du kan sedan skriva logik för att fortsätta bearbetningen om till exempel inga meddelanden har tagits emot under senaste *X* sekunder.</span><span class="sxs-lookup"><span data-stu-id="fd42e-163">You can then write logic to continue processing if, for example, no messages have been received in the last *X* seconds.</span></span>

<span data-ttu-id="fd42e-164">När du är klar ingressing händelser och ta emot meddelanden, måste du anropa funktionen motsvarande Rensa resurser.</span><span class="sxs-lookup"><span data-stu-id="fd42e-164">When you’re finished ingressing events and receiving messages, be sure to call the corresponding function to clean up resources.</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="fd42e-165">Det finns i princip bara en uppsättning API: er för att skicka och ta emot data med en bakgrundstråd och en annan uppsättning API: er som gör samma sak utan bakgrundstråden.</span><span class="sxs-lookup"><span data-stu-id="fd42e-165">Basically there’s only one set of APIs to send and receive data with a background thread and another set of APIs that does the same thing without the background thread.</span></span> <span data-ttu-id="fd42e-166">Många utvecklare kan också icke - lla API: erna, men på lägre nivå-API: er är användbara när utvecklare vill explicit kontroll över nätverksöverföringar.</span><span class="sxs-lookup"><span data-stu-id="fd42e-166">A lot of developers may prefer the non-LL APIs, but the lower-level APIs are useful when the developer wants explicit control over network transmissions.</span></span> <span data-ttu-id="fd42e-167">Till exempel vissa enheter samla in data över tid och endast ingångshändelser vid angivna intervall (till exempel när en timme eller en gång om dagen).</span><span class="sxs-lookup"><span data-stu-id="fd42e-167">For example, some devices collect data over time and only ingress events at specified intervals (for example, once an hour or once a day).</span></span> <span data-ttu-id="fd42e-168">Lågnivå-API: er ger dig möjlighet att styra explicit när du skickar och tar emot data från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd42e-168">The lower-level APIs give you the ability to explicitly control when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="fd42e-169">Andra kommer bara att föredra enkelheten med lägre nivå-API: er.</span><span class="sxs-lookup"><span data-stu-id="fd42e-169">Others will simply prefer the simplicity that the lower-level APIs provide.</span></span> <span data-ttu-id="fd42e-170">Allt som händer på huvudtråden i stället för vissa arbete sker i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="fd42e-170">Everything happens on the main thread rather than some work happening in the background.</span></span>

<span data-ttu-id="fd42e-171">Oavsett vilken modell som du väljer måste du ha ett enhetligt vilka API: er som du använder.</span><span class="sxs-lookup"><span data-stu-id="fd42e-171">Whichever model you choose, be sure to be consistent in which APIs you use.</span></span> <span data-ttu-id="fd42e-172">Om du startar genom att anropa **IoTHubClient\_lla\_CreateFromConnectionString**, bör du bara använda API: er för motsvarande på lägre nivå för all uppföljning:</span><span class="sxs-lookup"><span data-stu-id="fd42e-172">If you start by calling **IoTHubClient\_LL\_CreateFromConnectionString**, be sure you only use the corresponding lower-level APIs for any follow-up work:</span></span>

* <span data-ttu-id="fd42e-173">IoTHubClient\_lla\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="fd42e-173">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="fd42e-174">IoTHubClient\_lla\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="fd42e-174">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="fd42e-175">IoTHubClient\_lla\_förstör</span><span class="sxs-lookup"><span data-stu-id="fd42e-175">IoTHubClient\_LL\_Destroy</span></span>
* <span data-ttu-id="fd42e-176">IoTHubClient\_lla\_DoWork</span><span class="sxs-lookup"><span data-stu-id="fd42e-176">IoTHubClient\_LL\_DoWork</span></span>

<span data-ttu-id="fd42e-177">Motsatt gäller också.</span><span class="sxs-lookup"><span data-stu-id="fd42e-177">The opposite is true as well.</span></span> <span data-ttu-id="fd42e-178">Om du börjar med **IoTHubClient\_CreateFromConnectionString**, sedan använda icke - lla API: erna för några andra processer.</span><span class="sxs-lookup"><span data-stu-id="fd42e-178">If you start with **IoTHubClient\_CreateFromConnectionString**, then use the non-LL APIs for any additional processing.</span></span>

<span data-ttu-id="fd42e-179">I Azure IoT enheten SDK för C, visas den **iothub\_klienten\_exempel\_http** för en komplett exempel på lägre nivå-API: er.</span><span class="sxs-lookup"><span data-stu-id="fd42e-179">In the Azure IoT device SDK for C, see the **iothub\_client\_sample\_http** application for a complete example of the lower-level APIs.</span></span> <span data-ttu-id="fd42e-180">Den **iothub\_klienten\_exempel\_amqp** program kan refereras till en fullständig exempel av icke - lla API: erna.</span><span class="sxs-lookup"><span data-stu-id="fd42e-180">The **iothub\_client\_sample\_amqp** application can be referenced for a full example of the non-LL APIs.</span></span>

## <a name="property-handling"></a><span data-ttu-id="fd42e-181">Egenskapen hantering</span><span class="sxs-lookup"><span data-stu-id="fd42e-181">Property handling</span></span>
<span data-ttu-id="fd42e-182">Hittills när vi har beskrivs skickar data, har vi hänvisar till postmeddelandets brödtext.</span><span class="sxs-lookup"><span data-stu-id="fd42e-182">So far when we've described sending data, we've been referring to the body of the message.</span></span> <span data-ttu-id="fd42e-183">Anta till exempel att den här koden:</span><span class="sxs-lookup"><span data-stu-id="fd42e-183">For example, consider this code:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="fd42e-184">Det här exemplet skickar ett meddelande till IoT-hubb med texten ”Hello World”.</span><span class="sxs-lookup"><span data-stu-id="fd42e-184">This example sends a message to IoT Hub with the text "Hello World."</span></span> <span data-ttu-id="fd42e-185">IoT-hubb kan också egenskaper som ska kopplas till varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="fd42e-185">However, IoT Hub also allows properties to be attached to each message.</span></span> <span data-ttu-id="fd42e-186">Egenskaperna är namn/värde-par som kan bifogas i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="fd42e-186">Properties are name/value pairs that can be attached to the message.</span></span> <span data-ttu-id="fd42e-187">Vi kan till exempel ändra föregående kod om du vill bifoga en egenskap i meddelandet:</span><span class="sxs-lookup"><span data-stu-id="fd42e-187">For example, we can modify the previous code to attach a property to the message:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="fd42e-188">Vi börjar genom att anropa **IoTHubMessage\_egenskaper** och passerar den referensen till vår meddelandet.</span><span class="sxs-lookup"><span data-stu-id="fd42e-188">We start by calling **IoTHubMessage\_Properties** and passing it the handle of our message.</span></span> <span data-ttu-id="fd42e-189">Vad vi komma är en **KARTAN\_hantera** referens som gör att vi kan börja lägga till egenskaper.</span><span class="sxs-lookup"><span data-stu-id="fd42e-189">What we get back is a **MAP\_HANDLE** reference that enables us to start adding properties.</span></span> <span data-ttu-id="fd42e-190">Görs genom att anropa **kartan\_AddOrUpdate**, som tar en referens till en KARTA\_REFERENSEN och egenskapsnamnet egenskapens värde.</span><span class="sxs-lookup"><span data-stu-id="fd42e-190">The latter is accomplished by calling **Map\_AddOrUpdate**, which takes a reference to a MAP\_HANDLE, the property name, and the property value.</span></span> <span data-ttu-id="fd42e-191">Vi kan detta API för att lägga till alla egenskaper som vi gärna.</span><span class="sxs-lookup"><span data-stu-id="fd42e-191">With this API we can add as many properties as we like.</span></span>

<span data-ttu-id="fd42e-192">När händelsen läses från **Händelsehubbar**, mottagaren kan räkna upp egenskaper och hämta motsvarande värden.</span><span class="sxs-lookup"><span data-stu-id="fd42e-192">When the event is read from **Event Hubs**, the receiver can enumerate the properties and retrieve their corresponding values.</span></span> <span data-ttu-id="fd42e-193">Till exempel i .NET detta kan åstadkommas genom att öppna den [egenskapssamlingen på objektet EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd42e-193">For example, in .NET this would be accomplished by accessing the [Properties collection on the EventData object](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span></span>

<span data-ttu-id="fd42e-194">I exemplet ovan vi kopplar egenskaper till en händelse som vi skickar till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd42e-194">In the previous example, we’re attaching properties to an event that we send to IoT Hub.</span></span> <span data-ttu-id="fd42e-195">Egenskaper kan också kopplas till meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd42e-195">Properties can also be attached to messages received from IoT Hub.</span></span> <span data-ttu-id="fd42e-196">Om vi vill hämta egenskaper från ett meddelande använda vi kod exempelvis följande i vår Återanropsfunktionen meddelande:</span><span class="sxs-lookup"><span data-stu-id="fd42e-196">If we want to retrieve properties from a message, we can use code such as the following in our message callback function:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
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

<span data-ttu-id="fd42e-197">Anropet till **IoTHubMessage\_egenskaper** returnerar den **KARTAN\_hantera** referens.</span><span class="sxs-lookup"><span data-stu-id="fd42e-197">The call to **IoTHubMessage\_Properties** returns the **MAP\_HANDLE** reference.</span></span> <span data-ttu-id="fd42e-198">Vi skickar sedan som refererar till **kartan\_GetInternals** att hämta en referens till en matris med namn/värde-par (och även ett antal egenskaper).</span><span class="sxs-lookup"><span data-stu-id="fd42e-198">We then pass that reference to **Map\_GetInternals** to obtain a reference to an array of the name/value pairs (as well as a count of the properties).</span></span> <span data-ttu-id="fd42e-199">Då är det en enkel fråga om att egenskaperna för att hämta värden som vi vill använda.</span><span class="sxs-lookup"><span data-stu-id="fd42e-199">At that point it's a simple matter of enumerating the properties to get to the values we want.</span></span>

<span data-ttu-id="fd42e-200">Du behöver använda egenskaper i ditt program.</span><span class="sxs-lookup"><span data-stu-id="fd42e-200">You don't have to use properties in your application.</span></span> <span data-ttu-id="fd42e-201">Men om du behöver ange dem på händelser eller hämta dem från meddelanden, den **IoTHubClient** biblioteket kan du enkelt.</span><span class="sxs-lookup"><span data-stu-id="fd42e-201">However, if you need to set them on events or retrieve them from messages, the **IoTHubClient** library makes it easy.</span></span>

## <a name="message-handling"></a><span data-ttu-id="fd42e-202">Meddelandehantering</span><span class="sxs-lookup"><span data-stu-id="fd42e-202">Message handling</span></span>
<span data-ttu-id="fd42e-203">Som nämnts tidigare anger när meddelanden tas emot från IoT-hubb i **IoTHubClient** biblioteket svarar genom att aktivera registrerade Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="fd42e-203">As stated previously, when messages arrive from IoT Hub the **IoTHubClient** library responds by invoking a registered callback function.</span></span> <span data-ttu-id="fd42e-204">Det finns en returparameter av den här funktionen som ska ha vissa ytterligare förklaring.</span><span class="sxs-lookup"><span data-stu-id="fd42e-204">There is a return parameter of this function that deserves some additional explanation.</span></span> <span data-ttu-id="fd42e-205">Här är ett utdrag ur Återanropsfunktionen i den **iothub\_klienten\_exempel\_http** exempelprogrammet:</span><span class="sxs-lookup"><span data-stu-id="fd42e-205">Here’s an excerpt of the callback function in the **iothub\_client\_sample\_http** sample application:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

<span data-ttu-id="fd42e-206">Observera att returtypen **IOTHUBMESSAGE\_DISPOSITION\_resultatet** och i detta fall returnerar vi **IOTHUBMESSAGE\_godkända**.</span><span class="sxs-lookup"><span data-stu-id="fd42e-206">Note that the return type is **IOTHUBMESSAGE\_DISPOSITION\_RESULT** and in this particular case we return **IOTHUBMESSAGE\_ACCEPTED**.</span></span> <span data-ttu-id="fd42e-207">Det finns andra värden returnerar vi från den här funktionen som ändrar hur **IoTHubClient** biblioteket reagerar på meddelandet återanrop.</span><span class="sxs-lookup"><span data-stu-id="fd42e-207">There are other values we can return from this function that change how the **IoTHubClient** library reacts to the message callback.</span></span> <span data-ttu-id="fd42e-208">Här är alternativen.</span><span class="sxs-lookup"><span data-stu-id="fd42e-208">Here are the options.</span></span>

* <span data-ttu-id="fd42e-209">**IOTHUBMESSAGE\_godkända** – meddelandet har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="fd42e-209">**IOTHUBMESSAGE\_ACCEPTED** – The message has been processed successfully.</span></span> <span data-ttu-id="fd42e-210">Den **IoTHubClient** biblioteket kommer inte att anropa Återanropsfunktionen igen med samma meddelande.</span><span class="sxs-lookup"><span data-stu-id="fd42e-210">The **IoTHubClient** library will not invoke the callback function again with the same message.</span></span>
* <span data-ttu-id="fd42e-211">**IOTHUBMESSAGE\_Avvisad** – meddelandet kunde inte bearbetas och det finns inga önskan att göra det i framtiden.</span><span class="sxs-lookup"><span data-stu-id="fd42e-211">**IOTHUBMESSAGE\_REJECTED** – The message was not processed and there is no desire to do so in the future.</span></span> <span data-ttu-id="fd42e-212">Den **IoTHubClient** biblioteket ska inte anropa Återanropsfunktionen igen med samma meddelande.</span><span class="sxs-lookup"><span data-stu-id="fd42e-212">The **IoTHubClient** library should not invoke the callback function again with the same message.</span></span>
* <span data-ttu-id="fd42e-213">**IOTHUBMESSAGE\_ABANDONED** – meddelandet har inte bearbetats, men **IoTHubClient** biblioteket ska anropa Återanropsfunktionen igen med samma meddelande.</span><span class="sxs-lookup"><span data-stu-id="fd42e-213">**IOTHUBMESSAGE\_ABANDONED** – The message was not processed successfully, but the **IoTHubClient** library should invoke the callback function again with the same message.</span></span>

<span data-ttu-id="fd42e-214">För de första två returkoder den **IoTHubClient** biblioteket skickar ett meddelande till IoT-hubb som anger att meddelandet ska tas bort från kön enhet och inte levereras igen.</span><span class="sxs-lookup"><span data-stu-id="fd42e-214">For the first two return codes, the **IoTHubClient** library sends a message to IoT Hub indicating that the message should be deleted from the device queue and not delivered again.</span></span> <span data-ttu-id="fd42e-215">Net effekten är samma (meddelandet tas bort från kön enhet), men om meddelandet godkännas eller avvisas registreras fortfarande.</span><span class="sxs-lookup"><span data-stu-id="fd42e-215">The net effect is the same (the message is deleted from the device queue), but whether the message was accepted or rejected is still recorded.</span></span>  <span data-ttu-id="fd42e-216">Registrera denna skillnad är bra att avsändare av meddelandet som kan lyssna efter feedback och ta reda på om en enhet har godkänt eller avvisade ett visst meddelande.</span><span class="sxs-lookup"><span data-stu-id="fd42e-216">Recording this distinction is useful to senders of the message who can listen for feedback and find out if a device has accepted or rejected a particular message.</span></span>

<span data-ttu-id="fd42e-217">I det senaste fallet skickas även ett meddelande till IoT-hubb, men det anger att meddelandet ska levereras.</span><span class="sxs-lookup"><span data-stu-id="fd42e-217">In the last case a message is also sent to IoT Hub, but it indicates that the message should be redelivered.</span></span> <span data-ttu-id="fd42e-218">Du kommer normalt Avbryt ett meddelande om du påträffar ett fel men vill att försöka bearbeta meddelandet igen.</span><span class="sxs-lookup"><span data-stu-id="fd42e-218">Typically you’ll abandon a message if you encounter some error but want to try to process the message again.</span></span> <span data-ttu-id="fd42e-219">Däremot är avvisa meddelandet lämplig när det uppstår ett oåterkalleligt fel (eller om du bara väljer du inte vill bearbeta meddelandet).</span><span class="sxs-lookup"><span data-stu-id="fd42e-219">In contrast, rejecting a message is appropriate when you encounter an unrecoverable error (or if you simply decide you don’t want to process the message).</span></span>

<span data-ttu-id="fd42e-220">Dock vara medveten om olika returkoder så att du kan framkalla önskat beteende från den **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="fd42e-220">In any case, be aware of the different return codes so that you can elicit the behavior you want from the **IoTHubClient** library.</span></span>

## <a name="alternate-device-credentials"></a><span data-ttu-id="fd42e-221">Autentiseringsuppgifter för annan enhet</span><span class="sxs-lookup"><span data-stu-id="fd42e-221">Alternate device credentials</span></span>
<span data-ttu-id="fd42e-222">Enligt beskrivningen tidigare var det första du ska göra när du arbetar med den **IoTHubClient** biblioteket är att hämta en **IOTHUB\_klienten\_hantera** med ett anrop till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="fd42e-222">As explained previously, the first thing to do when working with the **IoTHubClient** library is to obtain a **IOTHUB\_CLIENT\_HANDLE** with a call such as the following:</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="fd42e-223">Argumenten för **IoTHubClient\_CreateFromConnectionString** är anslutningssträngen enhet och en parameter som anger det protokoll som vi använder för att kommunicera med IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="fd42e-223">The arguments to **IoTHubClient\_CreateFromConnectionString** are the device connection string and a parameter that indicates the protocol we use to communicate with IoT Hub.</span></span> <span data-ttu-id="fd42e-224">Anslutningssträngen enheten har ett format som visas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="fd42e-224">The device connection string has a format that appears as follows:</span></span>

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

<span data-ttu-id="fd42e-225">Det finns fyra typer av information i den här strängen: IoT-hubb namn, IoT-hubb suffix, enhets-ID och nyckeln för delad åtkomst.</span><span class="sxs-lookup"><span data-stu-id="fd42e-225">There are four pieces of information in this string: IoT Hub name, IoT Hub suffix, device ID, and shared access key.</span></span> <span data-ttu-id="fd42e-226">Du hämta det fullständigt kvalificerade domännamnet (FQDN) för en IoT-hubb när du skapar din IoT hub-instans i Azure portal – detta ger dig IoT-hubbnamnet (den första delen av FQDN) och suffixet IoT-hubb (resten av FQDN).</span><span class="sxs-lookup"><span data-stu-id="fd42e-226">You obtain the fully qualified domain name (FQDN) of an IoT hub when you create your IoT hub instance in the Azure portal — this gives you the IoT hub name (the first part of the FQDN) and the IoT hub suffix (the rest of the FQDN).</span></span> <span data-ttu-id="fd42e-227">Du får enhets-ID och den delade åtkomstnyckeln när du registrerar din enhet med IoT-hubben (som beskrivs i den [föregående artikel](iot-hub-device-sdk-c-intro.md)).</span><span class="sxs-lookup"><span data-stu-id="fd42e-227">You get the device ID and the shared access key when you register your device with IoT Hub (as described in the [previous article](iot-hub-device-sdk-c-intro.md)).</span></span>

<span data-ttu-id="fd42e-228">**IoTHubClient\_CreateFromConnectionString** ger dig ett sätt att starta biblioteket.</span><span class="sxs-lookup"><span data-stu-id="fd42e-228">**IoTHubClient\_CreateFromConnectionString** gives you one way to initialize the library.</span></span> <span data-ttu-id="fd42e-229">Om du vill kan du skapa en ny **IOTHUB\_klienten\_hantera** med hjälp av dessa enskilda parametrar i stället för anslutningssträngen för enheten.</span><span class="sxs-lookup"><span data-stu-id="fd42e-229">If you prefer, you can create a new **IOTHUB\_CLIENT\_HANDLE** by using these individual parameters rather than the device connection string.</span></span> <span data-ttu-id="fd42e-230">Detta uppnås med följande kod:</span><span class="sxs-lookup"><span data-stu-id="fd42e-230">This is achieved with the following code:</span></span>

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

<span data-ttu-id="fd42e-231">Detta åstadkommer samma sak som **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="fd42e-231">This accomplishes the same thing as **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="fd42e-232">Det kan verka uppenbara som du vill använda **IoTHubClient\_CreateFromConnectionString** i stället för den här mer utförlig metoden för initiering.</span><span class="sxs-lookup"><span data-stu-id="fd42e-232">It may seem obvious that you would want to use **IoTHubClient\_CreateFromConnectionString** rather than this more verbose method of initialization.</span></span> <span data-ttu-id="fd42e-233">Kom ihåg att när du registrerar en enhet i IoT-hubb vad du får är en enhets-ID och nyckeln för enheten (inte en anslutningssträng).</span><span class="sxs-lookup"><span data-stu-id="fd42e-233">Keep in mind, however, that when you register a device in IoT Hub what you get is a device ID and device key (not a connection string).</span></span> <span data-ttu-id="fd42e-234">Den *enheten explorer* SDK-verktyg som introducerades i den [föregående artikel](iot-hub-device-sdk-c-intro.md) använder biblioteken i de **Azure IoT service SDK** att skapa anslutningssträngen för enheten från enhets-ID , enhetsnyckel och IoT Hub-värdnamnet.</span><span class="sxs-lookup"><span data-stu-id="fd42e-234">The *device explorer* SDK tool introduced in the [previous article](iot-hub-device-sdk-c-intro.md) uses libraries in the **Azure IoT service SDK** to create the device connection string from the device ID, device key, and IoT Hub host name.</span></span> <span data-ttu-id="fd42e-235">Anropar så **IoTHubClient\_lla\_skapa** kan det vara bättre eftersom du sparar du steg för att generera en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="fd42e-235">So calling **IoTHubClient\_LL\_Create** may be preferable because it saves you the step of generating a connection string.</span></span> <span data-ttu-id="fd42e-236">Använd metoden som är lämplig.</span><span class="sxs-lookup"><span data-stu-id="fd42e-236">Use whichever method is convenient.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="fd42e-237">Konfigurationsalternativ</span><span class="sxs-lookup"><span data-stu-id="fd42e-237">Configuration options</span></span>
<span data-ttu-id="fd42e-238">Hittills allt beskrivs hur den **IoTHubClient** biblioteket fungerar visar dess standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="fd42e-238">So far everything described about the way the **IoTHubClient** library works reflects its default behavior.</span></span> <span data-ttu-id="fd42e-239">Det finns dock några alternativ som du kan ange för att ändra hur biblioteket fungerar.</span><span class="sxs-lookup"><span data-stu-id="fd42e-239">However, there are a few options that you can set to change how the library works.</span></span> <span data-ttu-id="fd42e-240">Detta görs genom att utnyttja den **IoTHubClient\_lla\_SetOption** API.</span><span class="sxs-lookup"><span data-stu-id="fd42e-240">This is accomplished by leveraging the **IoTHubClient\_LL\_SetOption** API.</span></span> <span data-ttu-id="fd42e-241">Överväg att det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="fd42e-241">Consider this example:</span></span>

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

<span data-ttu-id="fd42e-242">Det finns ett par olika alternativ som är vanliga:</span><span class="sxs-lookup"><span data-stu-id="fd42e-242">There are a couple of options that are commonly used:</span></span>

* <span data-ttu-id="fd42e-243">**SetBatching** (booleskt) – om **SANT**, som sedan skickas data som skickas till IoT-hubb i batchar.</span><span class="sxs-lookup"><span data-stu-id="fd42e-243">**SetBatching** (bool) – If **true**, then data sent to IoT Hub is sent in batches.</span></span> <span data-ttu-id="fd42e-244">Om **FALSKT**, och sedan skickas meddelanden individuellt.</span><span class="sxs-lookup"><span data-stu-id="fd42e-244">If **false**, then messages are sent individually.</span></span> <span data-ttu-id="fd42e-245">Standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="fd42e-245">The default is **false**.</span></span> <span data-ttu-id="fd42e-246">Observera att den **SetBatching** alternativet gäller endast HTTP-protokollet och inte MQTT eller AMQP-protokoll.</span><span class="sxs-lookup"><span data-stu-id="fd42e-246">Note that the **SetBatching** option only applies to the HTTP protocol and not to the MQTT or AMQP protocols.</span></span>
* <span data-ttu-id="fd42e-247">**Timeout** (osignerade int) – det här värdet anges i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="fd42e-247">**Timeout** (unsigned int) – This value is represented in milliseconds.</span></span> <span data-ttu-id="fd42e-248">Om du skickar en HTTP-begäran eller svar erhålls tar längre tid än den angivna tiden och sedan anslutningen på grund av timeout.</span><span class="sxs-lookup"><span data-stu-id="fd42e-248">If sending an HTTP request or receiving a response takes longer than this time, then the connection times out.</span></span>

<span data-ttu-id="fd42e-249">Alternativet batching är viktigt.</span><span class="sxs-lookup"><span data-stu-id="fd42e-249">The batching option is important.</span></span> <span data-ttu-id="fd42e-250">Som standard biblioteket ingresses händelser individuellt (en händelse är allt du skickar till **IoTHubClient\_lla\_SendEventAsync**).</span><span class="sxs-lookup"><span data-stu-id="fd42e-250">By default, the library ingresses events individually (a single event is whatever you pass to **IoTHubClient\_LL\_SendEventAsync**).</span></span> <span data-ttu-id="fd42e-251">Om alternativet batching är **SANT**, biblioteket samlar in så många händelser som går från bufferten (upp till den maximala storleken som accepterar IoT-hubb).</span><span class="sxs-lookup"><span data-stu-id="fd42e-251">If the batching option is **true**, the library collects as many events as it can from the buffer (up to the maximum message size that IoT Hub will accept).</span></span>  <span data-ttu-id="fd42e-252">Batch händelse skickas till IoT-hubb i ett enda HTTP-anrop (enskilda händelser slås ihop till en JSON-matris).</span><span class="sxs-lookup"><span data-stu-id="fd42e-252">The event batch is sent to IoT Hub in a single HTTP call (the individual events are bundled into a JSON array).</span></span> <span data-ttu-id="fd42e-253">Aktivera batchbearbetning vanligtvis resulterar i stora prestandavinster eftersom du är att minska nätverket turer.</span><span class="sxs-lookup"><span data-stu-id="fd42e-253">Enabling batching typically results in big performance gains since you’re reducing network round-trips.</span></span> <span data-ttu-id="fd42e-254">Det minskar också avsevärt bandbredd eftersom du skickar en uppsättning av HTTP-huvuden med en händelse batch i stället för en uppsättning huvuden för varje enskild händelse.</span><span class="sxs-lookup"><span data-stu-id="fd42e-254">It also significantly reduces bandwidth since you are sending one set of HTTP headers with an event batch rather than a set of headers for each individual event.</span></span> <span data-ttu-id="fd42e-255">Om du inte har en särskild anledning att göra något annat, vanligtvis vill du aktivera Batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="fd42e-255">Unless you have a specific reason to do otherwise, typically you’ll want to enable batching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd42e-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd42e-256">Next steps</span></span>
<span data-ttu-id="fd42e-257">Den här artikeln beskrivs i detalj hur den **IoTHubClient** biblioteket hittades i den **Azure IoT-enhet SDK för C**. Med den här informationen bör du ha en god förståelse av funktionerna i den **IoTHubClient** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="fd42e-257">This article describes in detail the behavior of the **IoTHubClient** library found in the **Azure IoT device SDK for C**. With this information, you should have a good understanding of the capabilities of the **IoTHubClient** library.</span></span> <span data-ttu-id="fd42e-258">Den [nästa artikel](iot-hub-device-sdk-c-serializer.md) innehåller liknande information om den **serialiseraren** bibliotek.</span><span class="sxs-lookup"><span data-stu-id="fd42e-258">The [next article](iot-hub-device-sdk-c-serializer.md) provides similar detail on the **serializer** library.</span></span>

<span data-ttu-id="fd42e-259">Mer information om hur du utvecklar för IoT-hubb finns i [Azure IoT SDK][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="fd42e-259">To learn more about developing for IoT Hub, see the [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="fd42e-260">Om du vill utforska ytterligare funktionerna i IoT-hubb, se:</span><span class="sxs-lookup"><span data-stu-id="fd42e-260">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="fd42e-261">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="fd42e-261">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
