> [!div class="op_single_selector"]
> * [Device: Node.js Service: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Device: Node.js Service: C#](../articles/iot-hub/iot-hub-csharp-node-device-management-get-started.md)
> * [Enhet: Java-tjänsten: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)

Backend-appar kan använda Azure IoT Hub primitiver t.ex [enheten dubbla] [ lnk-devtwin] och [direkt metoder][lnk-c2dmethod], tooremotely start och övervaka enheten hanteringsåtgärder på enheter. Den här kursen visar hur en backend-app och en enhetsapp kan fungera tillsammans tooinitiate och övervaka fjärranslutna enheten startas om med IoT-hubb.

Använd en direkt metod tooinitiate device management åtgärder (till exempel omstart fabriksåterställning och firmware-uppdatering) från en backend-app i hello molnet. hello-enhet är ansvarig för:

* Hanterar hello metoden begäran som skickades från IoT-hubb.
* Initiera hello motsvarande enhetsspecifika åtgärd på hello enhet.
* Tillhandahåller statusuppdateringar via *rapporterade egenskaper* tooIoT hubb.

Du kan använda en backend-app i hello molnet toorun enheten dubbla frågor tooreport på hello förloppet för hanteringsåtgärder för enheten.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
