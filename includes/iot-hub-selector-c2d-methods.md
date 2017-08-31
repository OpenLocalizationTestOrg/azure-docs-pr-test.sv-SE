> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-direct-methods.md)
> * [C#/node.js](../articles/iot-hub/iot-hub-csharp-node-direct-methods.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-direct-methods.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-direct-methods.md)

Azure IoT Hub är en helt hanterad tjänst som gör tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning för serverdel. Föregående handledningar ([Kom igång med IoT-hubb] och [meddelanden moln till enhet med IoT-hubben]) visar grundläggande enhet till moln och moln till enhet meddelandetjänsten funktioner i IoT-hubb. IoT-hubb ger dig också möjligheten att anropa metoder i icke-beständiga på enheter från molnet. Direkta metoder representerar en request-reply-interaktion med en enhet som liknar ett HTTP-anrop i att de lyckas eller misslyckas omedelbart (efter en tidsgräns som användaren har angett) så att användarna vet status för anropet. [Anropa en metod som är direkt på en enhet] [ lnk-devguide-methods] beskriver direkt metoder i detalj och innehåller anvisningar om hur när du ska använda direkt metoder i stället moln till enhet meddelanden eller önskade egenskaper.

I den här självstudiekursen lär du dig att:

* Använda Azure portal för att skapa en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.
* Skapa en simulerad enhetsapp som har en direkt metod som kan anropas av molnet.
* Skapa en konsolapp som anropar en direkt metod i appen simulerade enheten via din IoT-hubb.

> [!NOTE]
> För tillfället stöds direkt metoder bara på enheter som ansluter till IoT-hubb med MQTT-protokollet. Mer information finns i [MQTT stöd] [ lnk-devguide-mqtt] artikel för instruktioner om hur du konverterar en befintlig enhetsapp att använda MQTT.


[lnk-devguide-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md

[meddelanden moln till enhet med IoT-hubben]: ../articles/iot-hub/iot-hub-csharp-csharp-c2d.md
[Kom igång med IoT-hubb]: ../articles/iot-hub/iot-hub-node-node-getstarted.md