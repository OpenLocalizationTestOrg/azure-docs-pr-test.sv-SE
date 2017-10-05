---
title: "Förstå Azure IoT-hubbslutpunkter | Microsoft Docs"
description: "Utvecklarhandbok - referensinformation om IoT-hubb riktade enheten och tjänsten riktade slutpunkter."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 57ba52ae-19c6-43e4-bc6c-d8a5c2476e95
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 93ada731fe70cf7d294537241f8104c0b89940ed
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="reference---iot-hub-endpoints"></a><span data-ttu-id="47056-103">Referens - slutpunkter för IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="47056-103">Reference - IoT Hub endpoints</span></span>

## <a name="iot-hub-names"></a><span data-ttu-id="47056-104">IoT-hubb namn</span><span class="sxs-lookup"><span data-stu-id="47056-104">IoT Hub names</span></span>

<span data-ttu-id="47056-105">Du hittar namnet på IoT-hubb som är värd för dina slutpunkter i portalen på den **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="47056-105">You can find the name of the IoT hub that hosts your endpoints in the portal on the **Overview** blade.</span></span> <span data-ttu-id="47056-106">Som standard är DNS-namnet för en IoT-hubb ser ut som: `{your iot hub name}.azure-devices.net`.</span><span class="sxs-lookup"><span data-stu-id="47056-106">By default, the DNS name of an IoT hub looks like: `{your iot hub name}.azure-devices.net`.</span></span>

<span data-ttu-id="47056-107">Du kan använda Azure DNS för att skapa en anpassad DNS-namn för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="47056-107">You can use Azure DNS to create a custom DNS name for your IoT hub.</span></span> <span data-ttu-id="47056-108">Mer information finns i [Använd Azure DNS för att ange inställningar för anpassad domän för en Azure-tjänst](../dns/dns-custom-domain.md#azure-iot).</span><span class="sxs-lookup"><span data-stu-id="47056-108">For more information, see [Use Azure DNS to provide custom domain settings for an Azure service](../dns/dns-custom-domain.md#azure-iot).</span></span>

## <a name="list-of-built-in-iot-hub-endpoints"></a><span data-ttu-id="47056-109">Lista över inbyggda IoT-hubb-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="47056-109">List of built-in IoT Hub endpoints</span></span>

<span data-ttu-id="47056-110">Azure IoT-hubb är en tjänst för flera innehavare som visar dess funktioner till olika aktörer.</span><span class="sxs-lookup"><span data-stu-id="47056-110">Azure IoT Hub is a multi-tenant service that exposes its functionality to various actors.</span></span> <span data-ttu-id="47056-111">I följande diagram visas de olika slutpunkterna som visar IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="47056-111">The following diagram shows the various endpoints that IoT Hub exposes.</span></span>

![IoT Hub-slutpunkter][img-endpoints]

<span data-ttu-id="47056-113">I följande lista beskrivs slutpunkterna:</span><span class="sxs-lookup"><span data-stu-id="47056-113">The following list describes the endpoints:</span></span>

* <span data-ttu-id="47056-114">**Resursprovidern**.</span><span class="sxs-lookup"><span data-stu-id="47056-114">**Resource provider**.</span></span> <span data-ttu-id="47056-115">IoT-hubb resursprovidern Exponerar en [Azure Resource Manager] [ lnk-arm] gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="47056-115">The IoT Hub resource provider exposes an [Azure Resource Manager][lnk-arm] interface.</span></span> <span data-ttu-id="47056-116">Det här gränssnittet aktiverar Azure-prenumeration ägare att skapa och ta bort IoT-hubbar och för att uppdatera IoT-hubb egenskaper.</span><span class="sxs-lookup"><span data-stu-id="47056-116">This interface enables Azure subscription owners to create and delete IoT hubs, and to update IoT hub properties.</span></span> <span data-ttu-id="47056-117">IoT-hubb egenskaper styr [hubb nivå säkerhetsprinciper][lnk-accesscontrol], till skillnad från enheten på användarnivå och funktionella alternativ för moln till enhet och enheten till molnet.</span><span class="sxs-lookup"><span data-stu-id="47056-117">IoT Hub properties govern [hub-level security policies][lnk-accesscontrol], as opposed to device-level access control, and functional options for cloud-to-device and device-to-cloud messaging.</span></span> <span data-ttu-id="47056-118">Resursprovidern IoT-hubb kan du också [exportera enheten identiteter][lnk-importexport].</span><span class="sxs-lookup"><span data-stu-id="47056-118">The IoT Hub resource provider also enables you to [export device identities][lnk-importexport].</span></span>
* <span data-ttu-id="47056-119">**Enheten Identitetshantering**.</span><span class="sxs-lookup"><span data-stu-id="47056-119">**Device identity management**.</span></span> <span data-ttu-id="47056-120">Varje IoT-hubb uppvisar en uppsättning http-REST-slutpunkter för att hantera identiteter för enheten (skapa, hämta, uppdatera och ta bort).</span><span class="sxs-lookup"><span data-stu-id="47056-120">Each IoT hub exposes a set of HTTP REST endpoints to manage device identities (create, retrieve, update, and delete).</span></span> <span data-ttu-id="47056-121">[Enheten identiteter] [ lnk-device-identities] används för autentisering och åtkomstkontroll för enheten.</span><span class="sxs-lookup"><span data-stu-id="47056-121">[Device identities][lnk-device-identities] are used for device authentication and access control.</span></span>
* <span data-ttu-id="47056-122">**Dubbla enhetshantering**.</span><span class="sxs-lookup"><span data-stu-id="47056-122">**Device twin management**.</span></span> <span data-ttu-id="47056-123">Varje IoT-hubb uppvisar en uppsättning service-riktade HTTP-REST-slutpunkt som frågor och uppdaterar [enhet twins] [ lnk-twins] (uppdatering taggar och egenskaper).</span><span class="sxs-lookup"><span data-stu-id="47056-123">Each IoT hub exposes a set of service-facing HTTP REST endpoint to query and update [device twins][lnk-twins] (update tags and properties).</span></span>
* <span data-ttu-id="47056-124">**Jobb management**.</span><span class="sxs-lookup"><span data-stu-id="47056-124">**Jobs management**.</span></span> <span data-ttu-id="47056-125">Varje IoT-hubb uppvisar en uppsättning service-riktade http-REST-slutpunkt för att fråga efter och hantera [jobb][lnk-jobs].</span><span class="sxs-lookup"><span data-stu-id="47056-125">Each IoT hub exposes a set of service-facing HTTP REST endpoint to query and manage [jobs][lnk-jobs].</span></span>
* <span data-ttu-id="47056-126">**Enheten slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="47056-126">**Device endpoints**.</span></span> <span data-ttu-id="47056-127">För varje enhet i identitetsregistret visar IoT-hubb en uppsättning slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="47056-127">For each device in the identity registry, IoT Hub exposes a set of endpoints:</span></span>

  * <span data-ttu-id="47056-128">*Skicka meddelanden från enhet till moln*.</span><span class="sxs-lookup"><span data-stu-id="47056-128">*Send device-to-cloud messages*.</span></span> <span data-ttu-id="47056-129">En enhet använder den här slutpunkten till [skicka meddelanden från enhet till moln][lnk-d2c].</span><span class="sxs-lookup"><span data-stu-id="47056-129">A device uses this endpoint to [send device-to-cloud messages][lnk-d2c].</span></span>
  * <span data-ttu-id="47056-130">*Ta emot meddelanden moln till enhet*.</span><span class="sxs-lookup"><span data-stu-id="47056-130">*Receive cloud-to-device messages*.</span></span> <span data-ttu-id="47056-131">En enhet använder den här slutpunkten för att ta emot riktade [moln till enhet meddelanden][lnk-c2d].</span><span class="sxs-lookup"><span data-stu-id="47056-131">A device uses this endpoint to receive targeted [cloud-to-device messages][lnk-c2d].</span></span>
  * <span data-ttu-id="47056-132">*Initierar filöverföringar*.</span><span class="sxs-lookup"><span data-stu-id="47056-132">*Initiate file uploads*.</span></span> <span data-ttu-id="47056-133">En enhet använder den här slutpunkten för att ta emot en Azure Storage SAS URI från IoT-hubb för att [överför en fil][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="47056-133">A device uses this endpoint to receive an Azure Storage SAS URI from IoT Hub to [upload a file][lnk-upload].</span></span>
  * <span data-ttu-id="47056-134">*Hämta och uppdatera egenskaper för enhet dubbla*.</span><span class="sxs-lookup"><span data-stu-id="47056-134">*Retrieve and update device twin properties*.</span></span> <span data-ttu-id="47056-135">En enhet använder den här slutpunkten för åtkomst till dess [enheten dubbla][lnk-twins]'s egenskaper.</span><span class="sxs-lookup"><span data-stu-id="47056-135">A device uses this endpoint to access its [device twin][lnk-twins]'s properties.</span></span>
  * <span data-ttu-id="47056-136">*Ta emot begäranden om direkta metoden*.</span><span class="sxs-lookup"><span data-stu-id="47056-136">*Receive direct method requests*.</span></span> <span data-ttu-id="47056-137">En enhet använder den här slutpunkten för att lyssna efter [direkt metod][lnk-methods]'s begäranden.</span><span class="sxs-lookup"><span data-stu-id="47056-137">A device uses this endpoint to listen for [direct method][lnk-methods]'s requests.</span></span>

    <span data-ttu-id="47056-138">Dessa slutpunkter som exponeras med hjälp av [MQTT v3.1.1][lnk-mqtt], HTTP 1.1 och [AMQP 1.0] [ lnk-amqp] protokoll.</span><span class="sxs-lookup"><span data-stu-id="47056-138">These endpoints are exposed using [MQTT v3.1.1][lnk-mqtt], HTTP 1.1, and [AMQP 1.0][lnk-amqp] protocols.</span></span> <span data-ttu-id="47056-139">AMQP är också tillgänglig via [WebSockets] [ lnk-websockets] på port 443.</span><span class="sxs-lookup"><span data-stu-id="47056-139">AMQP is also available over [WebSockets][lnk-websockets] on port 443.</span></span>

    <span data-ttu-id="47056-140">Enheten twins och metoder slutpunkter är bara tillgängliga när du använder den [MQTT v3.1.1] [ lnk-mqtt] protokoll.</span><span class="sxs-lookup"><span data-stu-id="47056-140">The device twins and methods endpoints are available only when you use the [MQTT v3.1.1][lnk-mqtt] protocol.</span></span>

* <span data-ttu-id="47056-141">**Tjänstens slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="47056-141">**Service endpoints**.</span></span> <span data-ttu-id="47056-142">Varje IoT-hubb uppvisar en uppsättning slutpunkter för din lösningens serverdel att kommunicera med dina enheter.</span><span class="sxs-lookup"><span data-stu-id="47056-142">Each IoT hub exposes a set of endpoints  for your solution back end to communicate with your devices.</span></span> <span data-ttu-id="47056-143">Med ett undantag dessa slutpunkter är bara tillgängliga med den [AMQP] [ lnk-amqp] protokoll.</span><span class="sxs-lookup"><span data-stu-id="47056-143">With one exception, these endpoints are only exposed using the [AMQP][lnk-amqp] protocol.</span></span> <span data-ttu-id="47056-144">Metoden anrop slutpunkten exponeras via HTTP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="47056-144">The method invocation endpoint is exposed over the HTTP protocol.</span></span>
  
  * <span data-ttu-id="47056-145">*Ta emot meddelanden från enhet till moln*.</span><span class="sxs-lookup"><span data-stu-id="47056-145">*Receive device-to-cloud messages*.</span></span> <span data-ttu-id="47056-146">Den här slutpunkten är kompatibel med [Azure Event Hubs][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="47056-146">This endpoint is compatible with [Azure Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="47056-147">En backend tjänst kan använda den för att läsa den [meddelanden från enhet till moln] [ lnk-d2c] skickas av dina enheter.</span><span class="sxs-lookup"><span data-stu-id="47056-147">A back-end service can use it to read the [device-to-cloud messages][lnk-d2c] sent by your devices.</span></span> <span data-ttu-id="47056-148">Du kan skapa anpassade slutpunkter på din IoT-hubb förutom den här inbyggda slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="47056-148">You can create custom endpoints on your IoT hub in addition to this built-in endpoint.</span></span>
  * <span data-ttu-id="47056-149">*Skicka meddelanden moln till enhet och ta emot bekräftelser för leverans av*.</span><span class="sxs-lookup"><span data-stu-id="47056-149">*Send cloud-to-device messages and receive delivery acknowledgments*.</span></span> <span data-ttu-id="47056-150">Dessa slutpunkter aktivera din lösningens serverdel ska skickas pålitlig [moln till enhet meddelanden][lnk-c2d], samt för att få motsvarande leverans eller upphör att gälla bekräftelser.</span><span class="sxs-lookup"><span data-stu-id="47056-150">These endpoints enable your solution back end to send reliable [cloud-to-device messages][lnk-c2d], and to receive the corresponding delivery or expiration acknowledgments.</span></span>
  * <span data-ttu-id="47056-151">*Ta emot meddelanden i filen*.</span><span class="sxs-lookup"><span data-stu-id="47056-151">*Receive file notifications*.</span></span> <span data-ttu-id="47056-152">Den här asynkrona slutpunkten kan du ta emot meddelanden om när dina enheter har överfört en fil.</span><span class="sxs-lookup"><span data-stu-id="47056-152">This messaging endpoint allows you to receive notifications of when your devices successfully upload a file.</span></span> 
  * <span data-ttu-id="47056-153">*Dirigera metodanropet*.</span><span class="sxs-lookup"><span data-stu-id="47056-153">*Direct method invocation*.</span></span> <span data-ttu-id="47056-154">Den här slutpunkten kan en backend-tjänst att anropa en [direkt metod] [ lnk-methods] på en enhet.</span><span class="sxs-lookup"><span data-stu-id="47056-154">This endpoint allows a back-end service to invoke a [direct method][lnk-methods] on a device.</span></span>
  * <span data-ttu-id="47056-155">*Ta emot operations händelseövervakning*.</span><span class="sxs-lookup"><span data-stu-id="47056-155">*Receive operations monitoring events*.</span></span> <span data-ttu-id="47056-156">Den här slutpunkten kan du få övervaka händelser om din IoT-hubb har konfigurerats för att generera dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="47056-156">This endpoint allows you to receive operations monitoring events if your IoT hub has been configured to emit them.</span></span> <span data-ttu-id="47056-157">Mer information finns i [IoT-hubb operations övervakning][lnk-operations-mon].</span><span class="sxs-lookup"><span data-stu-id="47056-157">For more information, see [IoT Hub operations monitoring][lnk-operations-mon].</span></span>

<span data-ttu-id="47056-158">Den [Azure IoT SDK] [ lnk-sdks] beskrivs olika sätt att få åtkomst till dessa slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="47056-158">The [Azure IoT SDKs][lnk-sdks] article describes the various ways to access these endpoints.</span></span>

<span data-ttu-id="47056-159">Alla IoT-hubbslutpunkter använder den [TLS] [ lnk-tls] protokoll och ingen slutpunkt exponeras aldrig på okrypterade/oskyddad kanaler.</span><span class="sxs-lookup"><span data-stu-id="47056-159">All IoT Hub endpoints use the [TLS][lnk-tls] protocol, and no endpoint is ever exposed on unencrypted/unsecured channels.</span></span>

## <a name="custom-endpoints"></a><span data-ttu-id="47056-160">Anpassade slutpunkter</span><span class="sxs-lookup"><span data-stu-id="47056-160">Custom endpoints</span></span>

<span data-ttu-id="47056-161">Du kan länka befintliga Azure-tjänster i din prenumeration för din IoT-hubb som fungerar som slutpunkter för routning av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="47056-161">You can link existing Azure services in your subscription to your IoT hub to act as endpoints for message routing.</span></span> <span data-ttu-id="47056-162">Dessa slutpunkter fungerar som slutpunkter och används som sänkor för meddelandevägar.</span><span class="sxs-lookup"><span data-stu-id="47056-162">These endpoints act as service endpoints and are used as sinks for message routes.</span></span> <span data-ttu-id="47056-163">Enheter kan inte skriva direkt till ytterligare slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="47056-163">Devices cannot write directly to the additional endpoints.</span></span> <span data-ttu-id="47056-164">Mer information om meddelandevägar Se developer guide även [skicka och ta emot meddelanden med IoT-hubb][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="47056-164">To learn more about message routes, see the developer guide entry on [sending and receiving messages with IoT hub][lnk-devguide-messaging].</span></span>

<span data-ttu-id="47056-165">IoT-hubb stöder för närvarande följande Azure-tjänster som ytterligare slutpunkter:</span><span class="sxs-lookup"><span data-stu-id="47056-165">IoT Hub currently supports the following Azure services as additional endpoints:</span></span>

* <span data-ttu-id="47056-166">Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="47056-166">Event Hubs</span></span>
* <span data-ttu-id="47056-167">Service Bus-köer</span><span class="sxs-lookup"><span data-stu-id="47056-167">Service Bus Queues</span></span>
* <span data-ttu-id="47056-168">Avsnitt om Service Bus</span><span class="sxs-lookup"><span data-stu-id="47056-168">Service Bus Topics</span></span>

<span data-ttu-id="47056-169">IoT-hubb måste ha skrivbehörighet till dessa Tjänsteslutpunkter för meddelanderoutning för att fungera.</span><span class="sxs-lookup"><span data-stu-id="47056-169">IoT Hub needs write access to these service endpoints for message routing to work.</span></span> <span data-ttu-id="47056-170">Om du konfigurerar dina slutpunkter via Azure portal, läggs behörigheterna som krävs för dig.</span><span class="sxs-lookup"><span data-stu-id="47056-170">If you configure your endpoints through the Azure portal, the necessary permissions are added for you.</span></span> <span data-ttu-id="47056-171">Kontrollera att du konfigurerar dina tjänster för att stödja det förväntade genomflödet.</span><span class="sxs-lookup"><span data-stu-id="47056-171">Make sure you configure your services to support the expected throughput.</span></span> <span data-ttu-id="47056-172">När du först konfigurera din IoT-lösning kan du behöva övervaka dina ytterligare slutpunkter och gör eventuella ändringar för den faktiska belastningen.</span><span class="sxs-lookup"><span data-stu-id="47056-172">When you first configure your IoT solution, you may need to monitor your additional endpoints and make any necessary adjustments for the actual load.</span></span>

<span data-ttu-id="47056-173">Om ett meddelande matchar flera vägar som alla pekar till samma slutpunkt, levererar IoT-hubb meddelandet till denna slutpunkt bara en gång.</span><span class="sxs-lookup"><span data-stu-id="47056-173">If a message matches multiple routes that all point to the same endpoint, IoT Hub delivers message to that endpoint only once.</span></span> <span data-ttu-id="47056-174">Därför behöver du inte konfigurera deduplicering på din Service Bus-kö eller ett ämne.</span><span class="sxs-lookup"><span data-stu-id="47056-174">Therefore, you do not need to configure deduplication on your Service Bus queue or topic.</span></span> <span data-ttu-id="47056-175">I den partitionerade köer garanterar partition tillhörighet meddelandet ordning.</span><span class="sxs-lookup"><span data-stu-id="47056-175">In partitioned queues, partition affinity guarantees message ordering.</span></span>

> [!NOTE]
> <span data-ttu-id="47056-176">Service Bus-köer och ämnen som används som IoT-hubbslutpunkter inte får ha **sessioner** eller **dubblettidentifiering** aktiverat.</span><span class="sxs-lookup"><span data-stu-id="47056-176">Service Bus queues and topics used as IoT Hub endpoints must not have **Sessions** or **Duplicate Detection** enabled.</span></span> <span data-ttu-id="47056-177">Om något av dessa alternativ är aktiverade ändpunkt som **inte åtkomlig** i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="47056-177">If either of those options are enabled, the endpoint appears as **Unreachable** in the Azure portal.</span></span>

<span data-ttu-id="47056-178">Begränsningar för antalet slutpunkter som du kan lägga till finns [kvoter och begränsning][lnk-devguide-quotas].</span><span class="sxs-lookup"><span data-stu-id="47056-178">For the limits on the number of endpoints you can add, see [Quotas and throttling][lnk-devguide-quotas].</span></span>

## <a name="field-gateways"></a><span data-ttu-id="47056-179">Fältet gateways</span><span class="sxs-lookup"><span data-stu-id="47056-179">Field gateways</span></span>

<span data-ttu-id="47056-180">I en IoT-lösningen en *fältet gateway* placeras mellan dina enheter och slutpunkter för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="47056-180">In an IoT solution, a *field gateway* sits between your devices and your IoT Hub endpoints.</span></span> <span data-ttu-id="47056-181">Det finns normalt sett nära dina enheter.</span><span class="sxs-lookup"><span data-stu-id="47056-181">It is typically located close to your devices.</span></span> <span data-ttu-id="47056-182">Dina enheter kommunicerar direkt med fältet gateway med hjälp av ett protokoll som stöds av enheter.</span><span class="sxs-lookup"><span data-stu-id="47056-182">Your devices communicate directly with the field gateway by using a protocol supported by the devices.</span></span> <span data-ttu-id="47056-183">Fältet gatewayen ansluter till en IoT-hubb slutpunkt med ett protokoll som stöds av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="47056-183">The field gateway connects to an IoT Hub endpoint using a protocol that is supported by IoT Hub.</span></span> <span data-ttu-id="47056-184">En gateway för fältet kan vara en särskild maskinvaruenhet eller en låg energiförbrukning-dator som kör anpassade gateway-programvaran.</span><span class="sxs-lookup"><span data-stu-id="47056-184">A field gateway might be a dedicated hardware device or a low-power computer running custom gateway software.</span></span>

<span data-ttu-id="47056-185">Du kan använda [Azure IoT kant] [ lnk-iot-edge] att implementera en gateway för fältet.</span><span class="sxs-lookup"><span data-stu-id="47056-185">You can use [Azure IoT Edge][lnk-iot-edge] to implement a field gateway.</span></span> <span data-ttu-id="47056-186">IoT-Edge erbjuder funktioner, till exempel multiplexering kommunikation från flera enheter på samma IoT-hubb-anslutning.</span><span class="sxs-lookup"><span data-stu-id="47056-186">IoT Edge offers functionality such as multiplexing communications from multiple devices onto the same IoT Hub connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47056-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="47056-187">Next steps</span></span>

<span data-ttu-id="47056-188">Andra referensavsnitten i den här IoT-hubb Utvecklarhandbok inkluderar:</span><span class="sxs-lookup"><span data-stu-id="47056-188">Other reference topics in this IoT Hub developer guide include:</span></span>

* <span data-ttu-id="47056-189">[IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning][lnk-devguide-query]</span><span class="sxs-lookup"><span data-stu-id="47056-189">[IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query]</span></span>
* <span data-ttu-id="47056-190">[Kvoter och begränsning][lnk-devguide-quotas]</span><span class="sxs-lookup"><span data-stu-id="47056-190">[Quotas and throttling][lnk-devguide-quotas]</span></span>
* <span data-ttu-id="47056-191">[IoT-hubb MQTT stöd][lnk-devguide-mqtt]</span><span class="sxs-lookup"><span data-stu-id="47056-191">[IoT Hub MQTT support][lnk-devguide-mqtt]</span></span>

[lnk-iot-edge]: https://github.com/Azure/iot-edge

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[lnk-operations-mon]: iot-hub-operations-monitoring.md
