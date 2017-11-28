---
title: "aaaUnderstand stöd för Azure IoT-hubb MQTT | Microsoft Docs"
description: "Utvecklare guide - stöd för enheter som ansluter tooan IoT-hubb enheten riktade slutpunkten med hello MQTT-protokollet. Innehåller information om stöd för inbyggda MQTT i hello SDK för Azure IoT-enhet."
services: iot-hub
documentationcenter: .net
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 1d71c27c-b466-4a40-b95b-d6550cf85144
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e461687963138987acdf1f4e0e34c453744ea191
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="communicate-with-your-iot-hub-using-hello-mqtt-protocol"></a><span data-ttu-id="cd3f2-104">Kommunicera med din IoT-hubb med hello MQTT-protokollet</span><span class="sxs-lookup"><span data-stu-id="cd3f2-104">Communicate with your IoT hub using hello MQTT protocol</span></span>

<span data-ttu-id="cd3f2-105">IoT-hubb kan enheter toocommunicate med hello IoT-hubb enheten slutpunkter med hello [MQTT v3.1.1] [ lnk-mqtt-org] -protokollet på port 8883 eller MQTT v3.1.1 via WebSocket-protokollet på port 443.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-105">IoT Hub enables devices toocommunicate with hello IoT Hub device endpoints using hello [MQTT v3.1.1][lnk-mqtt-org] protocol on port 8883 or MQTT v3.1.1 over WebSocket protocol on port 443.</span></span> <span data-ttu-id="cd3f2-106">IoT-hubb kräver att alla enheter kommunikation toobe kan skyddas med TLS/SSL (därför IoT-hubb stöder inte är säkra anslutningar via port 1883).</span><span class="sxs-lookup"><span data-stu-id="cd3f2-106">IoT Hub requires all device communication toobe secured using TLS/SSL (hence, IoT Hub doesn’t support non-secure connections over port 1883).</span></span>

## <a name="connecting-tooiot-hub"></a><span data-ttu-id="cd3f2-107">Ansluta tooIoT Hub</span><span class="sxs-lookup"><span data-stu-id="cd3f2-107">Connecting tooIoT Hub</span></span>

<span data-ttu-id="cd3f2-108">En enhet kan du använda hello MQTT protokollet tooconnect tooan IoT-hubb antingen genom att använda hello bibliotek i hello [Azure IoT SDK] [ lnk-device-sdks] eller via protokollet hello MQTT direkt.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-108">A device can use hello MQTT protocol tooconnect tooan IoT hub either by using hello libraries in hello [Azure IoT SDKs][lnk-device-sdks] or by using hello MQTT protocol directly.</span></span>

## <a name="using-hello-device-sdks"></a><span data-ttu-id="cd3f2-109">Använder hello enhet SDK</span><span class="sxs-lookup"><span data-stu-id="cd3f2-109">Using hello device SDKs</span></span>

<span data-ttu-id="cd3f2-110">[Enheten SDK] [ lnk-device-sdks] som stöd hello MQTT protokollet är tillgängliga för Java, Node.js, C, C# och Python.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-110">[Device SDKs][lnk-device-sdks] that support hello MQTT protocol are available for Java, Node.js, C, C#, and Python.</span></span> <span data-ttu-id="cd3f2-111">hello SDK enhetsanvändning hello standard IoT-hubb anslutning sträng tooestablish anslutning tooan IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-111">hello device SDKs use hello standard IoT Hub connection string tooestablish a connection tooan IoT hub.</span></span> <span data-ttu-id="cd3f2-112">toouse hello MQTT protokollet hello klienten protokollet parametern måste anges för**MQTT**.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-112">toouse hello MQTT protocol, hello client protocol parameter must be set too**MQTT**.</span></span> <span data-ttu-id="cd3f2-113">Som standard ansluter hello enheten SDK tooan IoT-hubb med hello **CleanSession** flaggan för**0** och använda **QoS 1** för utbyte av meddelanden med hello IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-113">By default, hello device SDKs connect tooan IoT Hub with hello **CleanSession** flag set too**0** and use **QoS 1** for message exchange with hello IoT hub.</span></span>

<span data-ttu-id="cd3f2-114">När en enhet är ansluten tooan IoT-hubb kan ange hello enheten SDK metoder som gör att enheten toosend hälsningsmeddelande tooand ta emot meddelanden från en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-114">When a device is connected tooan IoT hub, hello device SDKs provide methods that enable hello device toosend messages tooand receive messages from an IoT hub.</span></span>

<span data-ttu-id="cd3f2-115">hello följande tabell innehåller länkar toocode prov för varje språk som stöds och anger hello parametern toouse tooestablish en anslutning tooIoT hubb med hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-115">hello following table contains links toocode samples for each supported language and specifies hello parameter toouse tooestablish a connection tooIoT Hub using hello MQTT protocol.</span></span>

| <span data-ttu-id="cd3f2-116">Språk</span><span class="sxs-lookup"><span data-stu-id="cd3f2-116">Language</span></span> | <span data-ttu-id="cd3f2-117">Protokoll-parameter</span><span class="sxs-lookup"><span data-stu-id="cd3f2-117">Protocol parameter</span></span> |
| --- | --- |
| <span data-ttu-id="cd3f2-118">[Node.js][lnk-sample-node]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-118">[Node.js][lnk-sample-node]</span></span> |<span data-ttu-id="cd3f2-119">Azure-iot-enhet – mqtt</span><span class="sxs-lookup"><span data-stu-id="cd3f2-119">azure-iot-device-mqtt</span></span> |
| <span data-ttu-id="cd3f2-120">[Java][lnk-sample-java]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-120">[Java][lnk-sample-java]</span></span> |<span data-ttu-id="cd3f2-121">IotHubClientProtocol.MQTT</span><span class="sxs-lookup"><span data-stu-id="cd3f2-121">IotHubClientProtocol.MQTT</span></span> |
| <span data-ttu-id="cd3f2-122">[C][lnk-sample-c]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-122">[C][lnk-sample-c]</span></span> |<span data-ttu-id="cd3f2-123">MQTT_Protocol</span><span class="sxs-lookup"><span data-stu-id="cd3f2-123">MQTT_Protocol</span></span> |
| <span data-ttu-id="cd3f2-124">[C#][lnk-sample-csharp]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-124">[C#][lnk-sample-csharp]</span></span> |<span data-ttu-id="cd3f2-125">TransportType.Mqtt</span><span class="sxs-lookup"><span data-stu-id="cd3f2-125">TransportType.Mqtt</span></span> |
| <span data-ttu-id="cd3f2-126">[Python][lnk-sample-python]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-126">[Python][lnk-sample-python]</span></span> |<span data-ttu-id="cd3f2-127">IoTHubTransportProvider.MQTT</span><span class="sxs-lookup"><span data-stu-id="cd3f2-127">IoTHubTransportProvider.MQTT</span></span> |

### <a name="migrating-a-device-app-from-amqp-toomqtt"></a><span data-ttu-id="cd3f2-128">Migrera en enhetsapp från AMQP tooMQTT</span><span class="sxs-lookup"><span data-stu-id="cd3f2-128">Migrating a device app from AMQP tooMQTT</span></span>

<span data-ttu-id="cd3f2-129">Om du använder hello [enheten SDK][lnk-device-sdks], växlar från att använda AMQP kräver tooMQTT ändring hello protocol-parametern i hello Klientinitiering som nämnts tidigare anger.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-129">If you are using hello [device SDKs][lnk-device-sdks], switching from using AMQP tooMQTT requires changing hello protocol parameter in hello client initialization as stated previously.</span></span>

<span data-ttu-id="cd3f2-130">När du gör det gör att toocheck hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-130">When doing so, make sure toocheck hello following items:</span></span>

* <span data-ttu-id="cd3f2-131">AMQP returnerar fel för många villkor när MQTT avbryter hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-131">AMQP returns errors for many conditions, while MQTT terminates hello connection.</span></span> <span data-ttu-id="cd3f2-132">Din undantagshantering logik kan därför kräva några ändringar.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-132">As a result your exception handling logic might require some changes.</span></span>
* <span data-ttu-id="cd3f2-133">MQTT stöder inte hello *avvisa* åtgärder när du tar emot [moln till enhet meddelanden][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="cd3f2-133">MQTT does not support hello *reject* operations when receiving [cloud-to-device messages][lnk-messaging].</span></span> <span data-ttu-id="cd3f2-134">Om backend-app måste tooreceive svar från hello enhetsapp, bör du använda [direkt metoder][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="cd3f2-134">If your back-end app needs tooreceive a response from hello device app, consider using [direct methods][lnk-methods].</span></span>

## <a name="using-hello-mqtt-protocol-directly"></a><span data-ttu-id="cd3f2-135">Protokollet hello MQTT direkt</span><span class="sxs-lookup"><span data-stu-id="cd3f2-135">Using hello MQTT protocol directly</span></span>
<span data-ttu-id="cd3f2-136">Om en enhet inte kan använda hello enheten SDK: er, kan den fortfarande ansluta toohello offentliga enheter slutpunkter med hello MQTT protokollet på port 8883.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-136">If a device cannot use hello device SDKs, it can still connect toohello public device endpoints using hello MQTT protocol on port 8883.</span></span> <span data-ttu-id="cd3f2-137">I hello **Anslut** paket hello enheten ska använda hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-137">In hello **CONNECT** packet hello device should use hello following values:</span></span>

* <span data-ttu-id="cd3f2-138">För hello **ClientId** , ska du använda hello **deviceId**.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-138">For hello **ClientId** field, use hello **deviceId**.</span></span>
* <span data-ttu-id="cd3f2-139">För hello **användarnamn** , ska du använda `{iothubhostname}/{device_id}/api-version=2016-11-14`, där {iothubhostname} är hello fullständig CName för hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-139">For hello **Username** field, use `{iothubhostname}/{device_id}/api-version=2016-11-14`, where {iothubhostname} is hello full CName of hello IoT hub.</span></span>

    <span data-ttu-id="cd3f2-140">Till exempel om hello namnet på din IoT-hubb är **contoso.azure devices.net** och hello namnet på din enhet är **MyDevice01**, hello fullständig **användarnamn** fältet ska innehålla `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-140">For example, if hello name of your IoT hub is **contoso.azure-devices.net** and if hello name of your device is **MyDevice01**, hello full **Username** field should contain `contoso.azure-devices.net/MyDevice01/api-version=2016-11-14`.</span></span>
* <span data-ttu-id="cd3f2-141">För hello **lösenord** , ska du använda en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-141">For hello **Password** field, use a SAS token.</span></span> <span data-ttu-id="cd3f2-142">hello format hello SAS-token är hello samma för både hello HTTP och AMQP protokoll:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-142">hello format of hello SAS token is hello same as for both hello HTTP and AMQP protocols:</span></span><br/><span data-ttu-id="cd3f2-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-143">`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.</span></span>

    <span data-ttu-id="cd3f2-144">Mer information om hur toogenerate SAS-token i avsnittet hello enhet av [med IoT-hubb säkerhetstoken][lnk-sas-tokens].</span><span class="sxs-lookup"><span data-stu-id="cd3f2-144">For more information about how toogenerate SAS tokens, see hello device section of [Using IoT Hub security tokens][lnk-sas-tokens].</span></span>

    <span data-ttu-id="cd3f2-145">När du testar, du kan också använda hello [enheten explorer] [ lnk-device-explorer] verktyget tooquickly Generera en SAS-token som du kan kopiera och klistra in i din egen kod:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-145">When testing, you can also use hello [device explorer][lnk-device-explorer] tool tooquickly generate a SAS token that you can copy and paste into your own code:</span></span>

  1. <span data-ttu-id="cd3f2-146">Gå toohello **Management** fliken i **enheten Explorer**.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-146">Go toohello **Management** tab in **Device Explorer**.</span></span>
  2. <span data-ttu-id="cd3f2-147">Klicka på **SAS-Token** (övre högra).</span><span class="sxs-lookup"><span data-stu-id="cd3f2-147">Click **SAS Token** (top right).</span></span>
  3. <span data-ttu-id="cd3f2-148">På **SASTokenForm**, Välj din enhet i hello **DeviceID** listrutan.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-148">On **SASTokenForm**, select your device in hello **DeviceID** drop down.</span></span> <span data-ttu-id="cd3f2-149">Ange din **TTL**.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-149">Set your **TTL**.</span></span>
  4. <span data-ttu-id="cd3f2-150">Klicka på **generera** toocreate din token.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-150">Click **Generate** toocreate your token.</span></span>

     <span data-ttu-id="cd3f2-151">hello SAS-token som skapas har den här strukturen: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-151">hello SAS token that's generated has this structure: `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

     <span data-ttu-id="cd3f2-152">en del av den här token toouse som hello hello **lösenord** fältet tooconnect med MQTT är: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-152">hello part of this token toouse as hello **Password** field tooconnect using MQTT is: `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.</span></span>

<span data-ttu-id="cd3f2-153">MQTT ansluta och koppla från paket, IoT-hubb utfärdar en händelse på hello **Operations Monitoring** kanalen med ytterligare information som kan hjälpa dig att tootroubleshoot anslutningsproblem.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-153">For MQTT connect and disconnect packets, IoT Hub issues an event on hello **Operations Monitoring** channel with additional information that can help you tootroubleshoot connectivity issues.</span></span>

### <a name="sending-device-to-cloud-messages"></a><span data-ttu-id="cd3f2-154">Skicka meddelanden från enhet till moln</span><span class="sxs-lookup"><span data-stu-id="cd3f2-154">Sending device-to-cloud messages</span></span>

<span data-ttu-id="cd3f2-155">När du har gjort en lyckad anslutning en enhet kan skicka meddelanden tooIoT hubb med `devices/{device_id}/messages/events/` eller `devices/{device_id}/messages/events/{property_bag}` som en **avsnittsnamn**.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-155">After making a successful connection, a device can send messages tooIoT Hub using `devices/{device_id}/messages/events/` or `devices/{device_id}/messages/events/{property_bag}` as a **Topic Name**.</span></span> <span data-ttu-id="cd3f2-156">Hej `{property_bag}` -elementet låter hälsningsmeddelande enheten toosend med ytterligare egenskaper i en url-kodat format.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-156">hello `{property_bag}` element enables hello device toosend messages with additional properties in a url-encoded format.</span></span> <span data-ttu-id="cd3f2-157">Exempel:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-157">For example:</span></span>

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> <span data-ttu-id="cd3f2-158">Detta `{property_bag}` element använder hello samma kodning som frågesträngar i hello HTTP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-158">This `{property_bag}` element uses hello same encoding as for query strings in hello HTTP protocol.</span></span>
>
>

<span data-ttu-id="cd3f2-159">Hej enhetsapp kan också använda `devices/{device_id}/messages/events/{property_bag}` som hello **kommer avsnittsnamn** toodefine *kommer meddelanden* toobe vidarebefordras som meddelandet telemetri.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-159">hello device app can also use `devices/{device_id}/messages/events/{property_bag}` as hello **Will topic name** toodefine *Will messages* toobe forwarded as a telemetry message.</span></span>

- <span data-ttu-id="cd3f2-160">IoT-hubben har inte stöd för QoS-2-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-160">IoT Hub does not support QoS 2 messages.</span></span> <span data-ttu-id="cd3f2-161">Om en enhetsapp publicerar ett meddelande med **QoS 2**, IoT-hubb stängs hello nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-161">If a device app publishes a message with **QoS 2**, IoT Hub closes hello network connection.</span></span>
- <span data-ttu-id="cd3f2-162">IoT-hubb sparas inte behålla meddelanden.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-162">IoT Hub does not persist Retain messages.</span></span> <span data-ttu-id="cd3f2-163">Om en enhet skickar ett meddelande med hello **behålla** too1-flaggan, IoT-hubb läggs hello **x-opt-behålla** egenskapen toohello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-163">If a device sends a message with hello **RETAIN** flag set too1, IoT Hub adds hello **x-opt-retain** application property toohello message.</span></span> <span data-ttu-id="cd3f2-164">I så fall behålla meddelandet i stället för att spara hello, IoT-hubb skickar den toohello backend-app.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-164">In this case, instead of persisting hello retain message, IoT Hub passes it toohello backend app.</span></span>
- <span data-ttu-id="cd3f2-165">IoT-hubb stöder endast en aktiv MQTT-anslutning per enhet.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-165">IoT Hub only supports one active MQTT connection per device.</span></span> <span data-ttu-id="cd3f2-166">En ny anslutning MQTT uppdrag hello samma enhets-ID orsakar IoT-hubb toodrop hello befintlig anslutning.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-166">Any new MQTT connection on behalf of hello same device ID causes IoT Hub toodrop hello existing connection.</span></span>

<span data-ttu-id="cd3f2-167">Mer information finns i [Messaging Utvecklingsguide][lnk-messaging].</span><span class="sxs-lookup"><span data-stu-id="cd3f2-167">For more information, see [Messaging developer's guide][lnk-messaging].</span></span>

### <a name="receiving-cloud-to-device-messages"></a><span data-ttu-id="cd3f2-168">Ta emot meddelanden moln till enhet</span><span class="sxs-lookup"><span data-stu-id="cd3f2-168">Receiving cloud-to-device messages</span></span>

<span data-ttu-id="cd3f2-169">tooreceive meddelanden från IoT-hubb, en enhet ska prenumerera med `devices/{device_id}/messages/devicebound/#` som en **avsnittet Filter**.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-169">tooreceive messages from IoT Hub, a device should subscribe using `devices/{device_id}/messages/devicebound/#` as a **Topic Filter**.</span></span> <span data-ttu-id="cd3f2-170">Hej med flera nivåer med jokertecken  **#**  i hello avsnittet Filter används bara för tooallow hello ytterligare egenskaper för enhet tooreceive i hello avsnittsnamn.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-170">hello multi-level wildcard **#** in hello Topic Filter is used only tooallow hello device tooreceive additional properties in hello topic name.</span></span> <span data-ttu-id="cd3f2-171">IoT-hubb tillåter inte hello användning av hello  **#**  eller **?**</span><span class="sxs-lookup"><span data-stu-id="cd3f2-171">IoT Hub does not allow hello usage of hello **#** or **?**</span></span> <span data-ttu-id="cd3f2-172">jokertecken för filtrering av underordnade ämnen.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-172">wildcards for filtering of sub-topics.</span></span> <span data-ttu-id="cd3f2-173">Eftersom IoT-hubb som inte är förhandlare generella pub-sub meddelanden stöder endast hello dokumenterade avsnittsnamn och avsnittet filter.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-173">Since IoT Hub is not a general purpose pub-sub messaging broker, it only supports hello documented topic names and topic filters.</span></span>

<span data-ttu-id="cd3f2-174">hello enheten inte tar emot meddelanden från IoT-hubb förrän den har prenumererar tooits enhetsspecifika slutpunkt, som representeras av hello `devices/{device_id}/messages/devicebound/#` avsnittet filter.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-174">hello device does not receive any messages from IoT Hub, until it has successfully subscribed tooits device-specific endpoint, represented by hello `devices/{device_id}/messages/devicebound/#` topic filter.</span></span> <span data-ttu-id="cd3f2-175">Efter lyckad prenumeration har upprättats startar hello enheten tar emot endast moln till enhet meddelanden som har skickats tooit stund hello hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-175">After successful subscription has been established, hello device starts receiving only cloud-to-device messages that have been sent tooit after hello time of hello subscription.</span></span> <span data-ttu-id="cd3f2-176">Hello enhet ansluter med **CleanSession** flaggan för**0**, hello prenumeration är beständig mellan olika sessioner.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-176">If hello device connects with **CleanSession** flag set too**0**, hello subscription is persisted across different sessions.</span></span> <span data-ttu-id="cd3f2-177">I det här fallet hello nästa gång datorn ansluts med **CleanSession 0** hello enheten tar emot utestående meddelanden som har skickats tooit när den har kopplats från.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-177">In this case, hello next time it connects with **CleanSession 0** hello device receives outstanding messages that have been sent tooit while it was disconnected.</span></span> <span data-ttu-id="cd3f2-178">Om hello enhet använder **CleanSession** flaggan för**1** men den inte tar emot meddelanden från IoT-hubb tills den prenumererar tooits enhet – slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-178">If hello device uses **CleanSession** flag set too**1** though, it does not receive any messages from IoT Hub until it subscribes tooits device-endpoint.</span></span>

<span data-ttu-id="cd3f2-179">IoT-hubb levererar meddelanden med hello **avsnittsnamn** `devices/{device_id}/messages/devicebound/`, eller `devices/{device_id}/messages/devicebound/{property_bag}` om det inte finns några egenskaper för meddelande.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-179">IoT Hub delivers messages with hello **Topic Name** `devices/{device_id}/messages/devicebound/`, or `devices/{device_id}/messages/devicebound/{property_bag}` if there are any message properties.</span></span> <span data-ttu-id="cd3f2-180">`{property_bag}`innehåller url-kodade nyckel/värde-par för meddelandeegenskaper.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-180">`{property_bag}` contains url-encoded key/value pairs of message properties.</span></span> <span data-ttu-id="cd3f2-181">Endast egenskaper för program och användaren går Systemegenskaper (exempelvis **messageId** eller **correlationId**) ingår i hello egenskapsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-181">Only application properties and user-settable system properties (such as **messageId** or **correlationId**) are included in hello property bag.</span></span> <span data-ttu-id="cd3f2-182">Egenskapsnamn för systemet har hello prefix  **$** , programegenskaper använda hello ursprungliga egenskapsnamn med inget prefix.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-182">System property names have hello prefix **$**, application properties use hello original property name with no prefix.</span></span>

<span data-ttu-id="cd3f2-183">När en enhetsapp prenumererar tooa ämne med **QoS 2**, IoT-hubb ger maximal QoS nivå 1 i hello **SUBACK** paket.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-183">When a device app subscribes tooa topic with **QoS 2**, IoT Hub grants maximum QoS level 1 in hello **SUBACK** packet.</span></span> <span data-ttu-id="cd3f2-184">Därefter ger IoT-hubb meddelanden toohello enhet med hjälp av QoS-1.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-184">After that, IoT Hub delivers messages toohello device using QoS 1.</span></span>

### <a name="retrieving-a-device-twins-properties"></a><span data-ttu-id="cd3f2-185">När en enhet dubbla egenskaper</span><span class="sxs-lookup"><span data-stu-id="cd3f2-185">Retrieving a device twin's properties</span></span>

<span data-ttu-id="cd3f2-186">Först måste en enhet prenumererar för`$iothub/twin/res/#`, tooreceive hello åtgärden svar.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-186">First, a device subscribes too`$iothub/twin/res/#`, tooreceive hello operation's responses.</span></span> <span data-ttu-id="cd3f2-187">Sedan skickar den ett tomt meddelande tootopic `$iothub/twin/GET/?$rid={request id}`, med en ifylld värde för **id för förfrågan**. hello tjänsten skickar sedan ett svarsmeddelande med hello enheten identiska data om avsnittet `$iothub/twin/res/{status}/?$rid={request id}`, med hjälp av hello samma  **id för förfrågan** som hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-187">Then, it sends an empty message tootopic `$iothub/twin/GET/?$rid={request id}`, with a populated value for **request id**. hello service then sends a response message containing hello device twin data on topic `$iothub/twin/res/{status}/?$rid={request id}`, using hello same **request id** as hello request.</span></span>

<span data-ttu-id="cd3f2-188">Id för förfrågan kan vara ett giltigt värde för ett meddelande egenskapsvärde enligt [IoT-hubb messaging Utvecklingsguide][lnk-messaging], och status har verifierats som ett heltal.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-188">Request id can be any valid value for a message property value, as per [IoT Hub messaging developer's guide][lnk-messaging], and status is validated as an integer.</span></span>
<span data-ttu-id="cd3f2-189">hello svarstexten innehåller hello properties-avsnittet för hello enheten dubbla:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-189">hello response body contains hello properties section of hello device twin:</span></span>

<span data-ttu-id="cd3f2-190">hello brödtext hello identitet registerposten begränsad toohello ”egenskaper” medlem, till exempel:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-190">hello body of hello identity registry entry limited toohello “properties” member, for example:</span></span>

        {
            "properties": {
                "desired": {
                    "telemetrySendFrequency": "5m",
                    "$version": 12
                },
                "reported": {
                    "telemetrySendFrequency": "5m",
                    "batteryLevel": 55,
                    "$version": 123
                }
            }
        }

<span data-ttu-id="cd3f2-191">hello statuskoder är:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-191">hello possible status codes are:</span></span>

|<span data-ttu-id="cd3f2-192">Status</span><span class="sxs-lookup"><span data-stu-id="cd3f2-192">Status</span></span> | <span data-ttu-id="cd3f2-193">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd3f2-193">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="cd3f2-194">200</span><span class="sxs-lookup"><span data-stu-id="cd3f2-194">200</span></span> | <span data-ttu-id="cd3f2-195">Lyckades</span><span class="sxs-lookup"><span data-stu-id="cd3f2-195">Success</span></span> |
| <span data-ttu-id="cd3f2-196">429</span><span class="sxs-lookup"><span data-stu-id="cd3f2-196">429</span></span> | <span data-ttu-id="cd3f2-197">För många förfrågningar (begränsas), enligt [IoT-hubb begränsning][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-197">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="cd3f2-198">5**</span><span class="sxs-lookup"><span data-stu-id="cd3f2-198">5**</span></span> | <span data-ttu-id="cd3f2-199">Serverfel</span><span class="sxs-lookup"><span data-stu-id="cd3f2-199">Server errors</span></span> |

<span data-ttu-id="cd3f2-200">Mer information finns i [utvecklarhandboken för enheten twins][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="cd3f2-200">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="update-device-twins-reported-properties"></a><span data-ttu-id="cd3f2-201">Uppdatera enheten dubbla rapporterade egenskaper</span><span class="sxs-lookup"><span data-stu-id="cd3f2-201">Update device twin's reported properties</span></span>

<span data-ttu-id="cd3f2-202">hello följande anvisningar beskriver hur en enhet uppdaterar hello rapporterade egenskaper i hello enheten dubbla i IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-202">hello following sequence describes how a device updates hello reported properties in hello device twin in IoT Hub:</span></span>

1. <span data-ttu-id="cd3f2-203">En enhet måste först prenumerera toohello `$iothub/twin/res/#` avsnittet tooreceive hello åtgärdens svar från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-203">A device must first subscribe toohello `$iothub/twin/res/#` topic tooreceive hello operation's responses from IoT Hub.</span></span>

1. <span data-ttu-id="cd3f2-204">En enhet skickar ett meddelande som innehåller hello enheten dubbla uppdatering toohello `$iothub/twin/PATCH/properties/reported/?$rid={request id}` avsnittet.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-204">A device sends a message that contains hello device twin update toohello `$iothub/twin/PATCH/properties/reported/?$rid={request id}` topic.</span></span> <span data-ttu-id="cd3f2-205">Detta meddelande innehåller en **id för förfrågan** värde.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-205">This message includes a **request id** value.</span></span>

1. <span data-ttu-id="cd3f2-206">Hej tjänsten och sedan skickar ett svarsmeddelande som innehåller hello nytt ETag-värde för hello rapporterade egenskapssamlingen om avsnittet `$iothub/twin/res/{status}/?$rid={request id}`.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-206">hello service then sends a response message that contains hello new ETag value for hello reported properties collection on topic `$iothub/twin/res/{status}/?$rid={request id}`.</span></span> <span data-ttu-id="cd3f2-207">Den här svarsmeddelande använder hello samma **id för förfrågan** som hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-207">This response message uses hello same **request id** as hello request.</span></span>

<span data-ttu-id="cd3f2-208">meddelandetexten för hello begäran innehåller ett JSON-dokument som innehåller nya värden för rapporterade egenskaper (ingen egenskap eller metadata inte kan ändras).</span><span class="sxs-lookup"><span data-stu-id="cd3f2-208">hello request message body contains a JSON document, which provides new values for reported properties (no other property or metadata can be modified).</span></span>
<span data-ttu-id="cd3f2-209">Varje medlem i hello JSON-dokumentet uppdateringar eller Lägg till hello motsvarande medlem i hello enheten dubbla dokumentet.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-209">Each member in hello JSON document updates or add hello corresponding member in hello device twin’s document.</span></span> <span data-ttu-id="cd3f2-210">Ange en medlem för`null`, borttagningar hello medlem från hello som innehåller objektet.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-210">A member set too`null`, deletes hello member from hello containing object.</span></span> <span data-ttu-id="cd3f2-211">Exempel:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-211">For example:</span></span>

        {
            "telemetrySendFrequency": "35m",
            "batteryLevel": 60
        }

<span data-ttu-id="cd3f2-212">hello statuskoder är:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-212">hello possible status codes are:</span></span>

|<span data-ttu-id="cd3f2-213">Status</span><span class="sxs-lookup"><span data-stu-id="cd3f2-213">Status</span></span> | <span data-ttu-id="cd3f2-214">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cd3f2-214">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="cd3f2-215">200</span><span class="sxs-lookup"><span data-stu-id="cd3f2-215">200</span></span> | <span data-ttu-id="cd3f2-216">Lyckades</span><span class="sxs-lookup"><span data-stu-id="cd3f2-216">Success</span></span> |
| <span data-ttu-id="cd3f2-217">400</span><span class="sxs-lookup"><span data-stu-id="cd3f2-217">400</span></span> | <span data-ttu-id="cd3f2-218">Felaktig begäran.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-218">Bad Request.</span></span> <span data-ttu-id="cd3f2-219">Felaktig JSON</span><span class="sxs-lookup"><span data-stu-id="cd3f2-219">Malformed JSON</span></span> |
| <span data-ttu-id="cd3f2-220">429</span><span class="sxs-lookup"><span data-stu-id="cd3f2-220">429</span></span> | <span data-ttu-id="cd3f2-221">För många förfrågningar (begränsas), enligt [IoT-hubb begränsning][lnk-quotas]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-221">Too many requests (throttled), as per [IoT Hub throttling][lnk-quotas]</span></span> |
| <span data-ttu-id="cd3f2-222">5**</span><span class="sxs-lookup"><span data-stu-id="cd3f2-222">5**</span></span> | <span data-ttu-id="cd3f2-223">Serverfel</span><span class="sxs-lookup"><span data-stu-id="cd3f2-223">Server errors</span></span> |

<span data-ttu-id="cd3f2-224">Mer information finns i [utvecklarhandboken för enheten twins][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="cd3f2-224">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="receiving-desired-properties-update-notifications"></a><span data-ttu-id="cd3f2-225">Ta emot meddelanden om uppdateringar egenskaper</span><span class="sxs-lookup"><span data-stu-id="cd3f2-225">Receiving desired properties update notifications</span></span>

<span data-ttu-id="cd3f2-226">När en enhet är ansluten, IoT-hubb skickar meddelanden toohello avsnittet `$iothub/twin/PATCH/properties/desired/?$version={new version}`, som innehåller hello innehåll hello uppdatering utförs av hello lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-226">When a device is connected, IoT Hub sends notifications toohello topic `$iothub/twin/PATCH/properties/desired/?$version={new version}`, which contain hello content of hello update performed by hello solution back end.</span></span> <span data-ttu-id="cd3f2-227">Exempel:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-227">For example:</span></span>

        {
            "telemetrySendFrequency": "5m",
            "route": null
        }

<span data-ttu-id="cd3f2-228">För egenskapen uppdateringar `null` värden innebär att hello JSON objekt medlemmen tas bort.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-228">As for property updates, `null` values means that hello JSON object member is being deleted.</span></span>


> [!IMPORTANT] 
> <span data-ttu-id="cd3f2-229">IoT-hubb genererar ändringsmeddelanden endast när enheterna är anslutna.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-229">IoT Hub generates change notifications only when devices are connected.</span></span> <span data-ttu-id="cd3f2-230">Se till att tooimplement hello [enheten återanslutning flödet] [ lnk-devguide-twin-reconnection] tookeep hello önskade egenskaper mellan IoT-hubb och hello enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-230">Make sure tooimplement hello [device reconnection flow][lnk-devguide-twin-reconnection] tookeep hello desired properties synchronized between IoT Hub and hello device app.</span></span>

<span data-ttu-id="cd3f2-231">Mer information finns i [utvecklarhandboken för enheten twins][lnk-devguide-twin].</span><span class="sxs-lookup"><span data-stu-id="cd3f2-231">For more information, see [Device twins developer's guide][lnk-devguide-twin].</span></span>

### <a name="respond-tooa-direct-method"></a><span data-ttu-id="cd3f2-232">Svara tooa direkt metod</span><span class="sxs-lookup"><span data-stu-id="cd3f2-232">Respond tooa direct method</span></span>

<span data-ttu-id="cd3f2-233">Först måste en enhet har toosubscribe för`$iothub/methods/POST/#`.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-233">First, a device has toosubscribe too`$iothub/methods/POST/#`.</span></span> <span data-ttu-id="cd3f2-234">IoT-hubb skickar metoden begäranden toohello avsnittet `$iothub/methods/POST/{method name}/?$rid={request id}`, med en giltig JSON eller en brödtext.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-234">IoT Hub sends method requests toohello topic `$iothub/methods/POST/{method name}/?$rid={request id}`, with either a valid JSON or an empty body.</span></span>

<span data-ttu-id="cd3f2-235">toorespond, hello enheten skickar ett meddelande med en giltig JSON eller brödtext toohello avsnittet `$iothub/methods/res/{status}/?$rid={request id}`, där **id för förfrågan** har toomatch hello i hello begärandemeddelandet och **status** har toobe ett heltal .</span><span class="sxs-lookup"><span data-stu-id="cd3f2-235">toorespond, hello device sends a message with a valid JSON or empty body toohello topic `$iothub/methods/res/{status}/?$rid={request id}`, where **request id** has toomatch hello one in hello request message, and **status** has toobe an integer.</span></span>

<span data-ttu-id="cd3f2-236">Mer information finns i [direkt metod Utvecklarhandbok][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="cd3f2-236">For more information, see [Direct method developer's guide][lnk-methods].</span></span>

### <a name="additional-considerations"></a><span data-ttu-id="cd3f2-237">Annat som är bra att tänka på</span><span class="sxs-lookup"><span data-stu-id="cd3f2-237">Additional considerations</span></span>

<span data-ttu-id="cd3f2-238">Som en slutlig faktor om du behöver toocustomize hello MQTT protokollbeteendena på hello molnet sida, bör du granska hello [Azure IoT-protokollgatewayen] [ lnk-azure-protocol-gateway] som du kan använda toodeploy hög prestanda anpassade protokollet gateway som direkt med IoT-hubb-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-238">As a final consideration, if you need toocustomize hello MQTT protocol behavior on hello cloud side, you should review hello [Azure IoT protocol gateway][lnk-azure-protocol-gateway] that enables you toodeploy a high-performance custom protocol gateway that interfaces directly with IoT Hub.</span></span> <span data-ttu-id="cd3f2-239">hello Azure IoT-protokollet gateway aktiverar du toocustomize hello enheten protokollet tooaccommodate brownfield MQTT distributioner eller andra anpassade protokoll.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-239">hello Azure IoT protocol gateway enables you toocustomize hello device protocol tooaccommodate brownfield MQTT deployments or other custom protocols.</span></span> <span data-ttu-id="cd3f2-240">Den här metoden kräver dock att du kör och använda en anpassad protocol-gateway.</span><span class="sxs-lookup"><span data-stu-id="cd3f2-240">This approach does require, however, that you run and operate a custom protocol gateway.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd3f2-241">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cd3f2-241">Next steps</span></span>

<span data-ttu-id="cd3f2-242">toolearn mer om hello MQTT protokoll finns hello [MQTT dokumentationen][lnk-mqtt-docs].</span><span class="sxs-lookup"><span data-stu-id="cd3f2-242">toolearn more about hello MQTT protocol, see hello [MQTT documentation][lnk-mqtt-docs].</span></span>

<span data-ttu-id="cd3f2-243">toolearn mer information om hur du planerar din IoT-hubb-distribution, se:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-243">toolearn more about planning your IoT Hub deployment, see:</span></span>

* <span data-ttu-id="cd3f2-244">[Azure certifierad för katalogen för IoT-enhet][lnk-devices]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-244">[Azure Certified for IoT device catalog][lnk-devices]</span></span>
* <span data-ttu-id="cd3f2-245">[Stöd för fler protokoll][lnk-protocols]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-245">[Support additional protocols][lnk-protocols]</span></span>
* <span data-ttu-id="cd3f2-246">[Jämför med Händelsehubbar][lnk-compare]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-246">[Compare with Event Hubs][lnk-compare]</span></span>
* <span data-ttu-id="cd3f2-247">[Skalning, hög tillgänglighet och Katastrofåterställning][lnk-scaling]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-247">[Scaling, HA, and DR][lnk-scaling]</span></span>

<span data-ttu-id="cd3f2-248">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="cd3f2-248">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="cd3f2-249">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-249">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="cd3f2-250">[Simulera en enhet med Azure IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="cd3f2-250">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
