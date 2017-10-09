> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#/node.js](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)

Enhetstvillingar är JSON-dokument som lagrar information om enhetstillstånd (metadata, konfigurationer och villkor). IoT-hubb kvarstår en enhet dubbla för varje enhet som ansluter tooit.

Använd enhet twins till:

* Lagra metadata om enheter från din lösningens serverdel.
* Rapportera aktuell statusinformation, till exempel tillgängliga funktioner och villkor (till exempel hello anslutningsmetod som används) från din enhet.
* Synkronisera hello tillståndet för tidskrävande arbetsflöden (till exempel uppdateringarna av inbyggd och konfiguration) mellan en enhetsapp och en backend-app.
* Fråga din enhetsmetadata, konfiguration eller tillstånd.

> [!NOTE]
> Enheten twins är utformade för synkronisering och för att fråga efter enhetskonfigurationer och villkor. Mer information om när toouse enhet twins finns i [förstå enheten twins][lnk-twins].

Enheten twins lagras i en IoT-hubb och innehålla:

* *taggar*, enhetens metadata endast kan nås av hello lösningens serverdel;
* *Egenskaper för Desired*, JSON-objekt som kan ändras av hello lösning tillbaka slutet och observeras av hello enhetsapp, och
* *rapporterade egenskaper*, JSON-objekt kan ändras av hello enhetsapp och läsas av hello lösningens serverdel. Taggar och egenskaper får inte innehålla matriser, men kan vara kapslade objekt.

![][img-twin]

Dessutom kan hello lösningens serverdel fråga enheten twins baserat på alla hello ovanför data.
Se för[förstå enheten twins] [ lnk-twins] för mer information om enheten twins och toohello [IoT-hubb frågespråket] [ lnk-query] referens för frågor.

> [!NOTE]
> Just nu är enheten twins kan endast nås från enheter som ansluter tooIoT hubb med hello MQTT-protokollet. Se toohello [MQTT stöd] [ lnk-devguide-mqtt] artikel anvisningar för hur tooconvert befintliga enheten app toouse MQTT.

I den här självstudiekursen lär du dig att:

* Skapa en backend-app som lägger till *taggar* tooa enheten dubbla och en simulerad enhetsapp som rapporterar anslutningen channel som en *rapporterade egenskapen* på hello enheten dubbla.
* Fråga enheter från en backend-app med hjälp av filter på hello taggar och egenskaper som tidigare har skapat.

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md