> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-direct-methods.md)
> * [C#/node.js](../articles/iot-hub/iot-hub-csharp-node-direct-methods.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-direct-methods.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-direct-methods.md)

Azure IoT Hub är en helt hanterad tjänst som gör tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning för serverdel. Föregående handledningar ([Kom igång med IoT-hubb] och [meddelanden moln till enhet med IoT-hubben]) visar hello grundläggande enhet till moln och moln till enhet meddelandefunktioner för IoT-hubb. IoT-hubb kan du också hello möjlighet tooinvoke icke varaktiga metoder på enheter från hello molnet. Dirigera metoder representerar en request-reply-interaktion med en enhet liknande tooan HTTP anropa i att de lyckas eller misslyckas omedelbart (efter en tidsgräns som användaren har angett) toolet hello användaren vet hello status för hello-anrop. [Anropa en metod som är direkt på en enhet] [ lnk-devguide-methods] beskriver direkt metoder i detalj och innehåller anvisningar om hur när toouse direkt metoder i stället moln till enhet meddelanden eller önskade egenskaper.

I den här självstudiekursen lär du dig att:

* Använd hello Azure portal toocreate en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.
* Skapa en simulerad enhetsapp som har en direkt metod som kan anropas av hello molnet.
* Skapa en konsolapp som anropar en direkt metod i hello simulerade enheten appen via din IoT-hubb.

> [!NOTE]
> För tillfället är direkt metoderna endast stöds på enheter som ansluter tooIoT Hub via hello MQTT-protokollet. Se toohello [MQTT stöd] [ lnk-devguide-mqtt] artikel anvisningar för hur tooconvert befintliga enheten app toouse MQTT.


[lnk-devguide-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md

[meddelanden moln till enhet med IoT-hubben]: ../articles/iot-hub/iot-hub-csharp-csharp-c2d.md
[Kom igång med IoT-hubb]: ../articles/iot-hub/iot-hub-node-node-getstarted.md