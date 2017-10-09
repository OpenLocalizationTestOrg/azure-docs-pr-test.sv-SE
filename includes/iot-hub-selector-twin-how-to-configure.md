> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [C#/node.js](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a>Introduktion

I [Kom igång med IoT-hubb enheten twins][lnk-twin-tutorial], du har lärt dig hur tooset enhetens metadata från lösningen tillbaka sluta använda *taggar*, rapportera enheten villkor från en enhetsapp med hjälp av *rapporterade egenskaper*, och fråga efter den här informationen med hjälp av en SQL-liknande språk.

I den här kursen får du lära dig hur toouse hello hello enheten dubbla *önskade egenskaper* tillsammans med *rapporterade egenskaper*, tooremotely konfigurera appar för enheter. Mer specifikt den här kursen visar hur en enhet dubbla rapporterade och egenskaper aktivera en konfiguration med flera steg i ett enhetsprogram och tillhandahålla hello synlighet toohello lösningens serverdel hello statusen för den här åtgärden på alla enheter. Du hittar mer information om hello rollen enhetskonfigurationer i [översikt över hantering av enheter med IoT-hubben][lnk-dm-overview].

Med enheten twins kan hello lösning serverdel toospecify hello önskad konfiguration för hello hanterade enheter, i stället för att skicka vissa kommandon på en hög nivå. Detta placerar hello enheten ansvarig konfigurerar hello bästa sätt tooupdate konfigurationen (mycket viktigt i IoT-scenarier där enheten villkor påverkar hello möjlighet tooimmediately utföra vissa kommandon), medan du kontinuerligt rapporterar toohello lösningen avslutas hello aktuella tillstånd och potentiella felvillkor för hello uppdateringsprocessen. Det här mönstret är instrumentella toohello hantering av stora mängder av enheter, som kan hello lösning serverdel toohave full insyn i konfigurationsprocessen för hello hello som anges på alla enheter.

> [!NOTE]
> I scenarier där enheter kontrolleras på ett mer interaktiva sätt (aktivera en fläkt från en användare-kontrollerade app), Överväg att använda [direkt metoder][lnk-methods].
> 
> 

I den här självstudiekursen hello lösning serverdel ändringar hello telemetri konfigurationen av en målenhet och, på grund av hello enhetsapp som följer en process med flera steg tooapply en konfiguration som uppdaterar (till exempel kräver programvara modulen startas om, där den här kursen simulerar med en enkel fördröjning).

hello lösningens serverdel lagras hello configuration hello enheten dubbla önskade egenskaper i hello följande sätt:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of hello configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [!NOTE]
> Eftersom konfigurationer kan vara komplexa objekt, vanligtvis tilldelas de unika ID: n (hashvärden eller [GUID][lnk-guid]) toosimplify sina jämförelser.
> 
> 

hello enhetsapp rapporterar den aktuella konfigurationen spegling hello önskad egenskapen **telemetryConfig** i hello rapporterade egenskaper:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of hello current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Observera hur hello rapporteras **telemetryConfig** har en annan egenskap **status**, används tooreport hello tillstånd hello konfigurationsprocessen för uppdateringen.

När en ny önskad konfiguration tas emot rapporterar hello enhetsapp en väntande konfiguration genom att ändra hello information:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of hello current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of hello pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Sedan vid ett senare tillfälle rapporterar hello enhetsapp hello lyckats eller misslyckats i den här åtgärden genom att uppdatera hello senare egenskapen.
Observera hur hello lösningens serverdel kan, när som helst tooquery hello status av hello konfigurationen på alla hello-enheter.

I den här självstudiekursen lär du dig att:

* Skapa en simulerad enhetsapp som tar emot konfigurationsuppdateringar från hello lösningens serverdel och rapporterar flera uppdateringar som *rapporterade egenskaper* på hello konfiguration uppdateringsprocessen.
* Skapa en backend-app att uppdateringar hello önskad konfiguration för en enhet och sedan frågor hello konfigurationsprocessen för uppdateringen.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
