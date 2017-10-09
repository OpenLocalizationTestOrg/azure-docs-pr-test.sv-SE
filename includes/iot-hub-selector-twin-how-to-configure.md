> [!div class="op_single_selector"]
> * [<span data-ttu-id="712c4-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="712c4-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [<span data-ttu-id="712c4-102">C#/node.js</span><span class="sxs-lookup"><span data-stu-id="712c4-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [<span data-ttu-id="712c4-103">C#</span><span class="sxs-lookup"><span data-stu-id="712c4-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a><span data-ttu-id="712c4-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="712c4-104">Introduction</span></span>

<span data-ttu-id="712c4-105">I [Kom igång med IoT-hubb enheten twins][lnk-twin-tutorial], du har lärt dig hur tooset enhetens metadata från lösningen tillbaka sluta använda *taggar*, rapportera enheten villkor från en enhetsapp med hjälp av *rapporterade egenskaper*, och fråga efter den här informationen med hjälp av en SQL-liknande språk.</span><span class="sxs-lookup"><span data-stu-id="712c4-105">In [Get started with IoT Hub device twins][lnk-twin-tutorial], you learned how tooset device metadata from your solution back end using *tags*, report device conditions from a device app using *reported properties*, and query this information using a SQL-like language.</span></span>

<span data-ttu-id="712c4-106">I den här kursen får du lära dig hur toouse hello hello enheten dubbla *önskade egenskaper* tillsammans med *rapporterade egenskaper*, tooremotely konfigurera appar för enheter.</span><span class="sxs-lookup"><span data-stu-id="712c4-106">In this tutorial, you will learn how toouse hello hello device twin's *desired properties* along with *reported properties*, tooremotely configure device apps.</span></span> <span data-ttu-id="712c4-107">Mer specifikt den här kursen visar hur en enhet dubbla rapporterade och egenskaper aktivera en konfiguration med flera steg i ett enhetsprogram och tillhandahålla hello synlighet toohello lösningens serverdel hello statusen för den här åtgärden på alla enheter.</span><span class="sxs-lookup"><span data-stu-id="712c4-107">More specifically, this tutorial shows how a device twin's reported and desired properties enable a multi-step configuration of a device application, and provide hello visibility toohello solution back end of hello status of this operation across all devices.</span></span> <span data-ttu-id="712c4-108">Du hittar mer information om hello rollen enhetskonfigurationer i [översikt över hantering av enheter med IoT-hubben][lnk-dm-overview].</span><span class="sxs-lookup"><span data-stu-id="712c4-108">You can find more information regarding hello role of device configurations in [Overview of device management with IoT Hub][lnk-dm-overview].</span></span>

<span data-ttu-id="712c4-109">Med enheten twins kan hello lösning serverdel toospecify hello önskad konfiguration för hello hanterade enheter, i stället för att skicka vissa kommandon på en hög nivå.</span><span class="sxs-lookup"><span data-stu-id="712c4-109">At a high level, using device twins enables hello solution back end toospecify hello desired configuration for hello managed devices, instead of sending specific commands.</span></span> <span data-ttu-id="712c4-110">Detta placerar hello enheten ansvarig konfigurerar hello bästa sätt tooupdate konfigurationen (mycket viktigt i IoT-scenarier där enheten villkor påverkar hello möjlighet tooimmediately utföra vissa kommandon), medan du kontinuerligt rapporterar toohello lösningen avslutas hello aktuella tillstånd och potentiella felvillkor för hello uppdateringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="712c4-110">This puts hello device in charge of setting up hello best way tooupdate its configuration (very important in IoT scenarios where specific device conditions affect hello ability tooimmediately carry out specific commands), while continually reporting toohello solution back end hello current state and potential error conditions of hello update process.</span></span> <span data-ttu-id="712c4-111">Det här mönstret är instrumentella toohello hantering av stora mängder av enheter, som kan hello lösning serverdel toohave full insyn i konfigurationsprocessen för hello hello som anges på alla enheter.</span><span class="sxs-lookup"><span data-stu-id="712c4-111">This pattern is instrumental toohello management of large sets of devices, as it enables hello solution back end toohave full visibility of hello state of hello configuration process across all devices.</span></span>

> [!NOTE]
> <span data-ttu-id="712c4-112">I scenarier där enheter kontrolleras på ett mer interaktiva sätt (aktivera en fläkt från en användare-kontrollerade app), Överväg att använda [direkt metoder][lnk-methods].</span><span class="sxs-lookup"><span data-stu-id="712c4-112">In scenarios where devices are controlled in a more interactive fashion (turn on a fan from a user-controlled app), consider using [direct methods][lnk-methods].</span></span>
> 
> 

<span data-ttu-id="712c4-113">I den här självstudiekursen hello lösning serverdel ändringar hello telemetri konfigurationen av en målenhet och, på grund av hello enhetsapp som följer en process med flera steg tooapply en konfiguration som uppdaterar (till exempel kräver programvara modulen startas om, där den här kursen simulerar med en enkel fördröjning).</span><span class="sxs-lookup"><span data-stu-id="712c4-113">In this tutorial, hello solution back end changes hello telemetry configuration of a target device and, as a result of that, hello device app follows a multi-step process tooapply a configuration update (for example, requiring a software module restart, which this tutorial simulates with a simple delay).</span></span>

<span data-ttu-id="712c4-114">hello lösningens serverdel lagras hello configuration hello enheten dubbla önskade egenskaper i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="712c4-114">hello solution back end stores hello configuration in hello device twin's desired properties in hello following way:</span></span>

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
> <span data-ttu-id="712c4-115">Eftersom konfigurationer kan vara komplexa objekt, vanligtvis tilldelas de unika ID: n (hashvärden eller [GUID][lnk-guid]) toosimplify sina jämförelser.</span><span class="sxs-lookup"><span data-stu-id="712c4-115">Since configurations can be complex objects, they are usually assigned unique ids (hashes or [GUIDs][lnk-guid]) toosimplify their comparisons.</span></span>
> 
> 

<span data-ttu-id="712c4-116">hello enhetsapp rapporterar den aktuella konfigurationen spegling hello önskad egenskapen **telemetryConfig** i hello rapporterade egenskaper:</span><span class="sxs-lookup"><span data-stu-id="712c4-116">hello device app reports its current configuration mirroring hello desired property **telemetryConfig** in hello reported properties:</span></span>

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

<span data-ttu-id="712c4-117">Observera hur hello rapporteras **telemetryConfig** har en annan egenskap **status**, används tooreport hello tillstånd hello konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="712c4-117">Note how hello reported **telemetryConfig** has an additional property **status**, used tooreport hello state of hello configuration update process.</span></span>

<span data-ttu-id="712c4-118">När en ny önskad konfiguration tas emot rapporterar hello enhetsapp en väntande konfiguration genom att ändra hello information:</span><span class="sxs-lookup"><span data-stu-id="712c4-118">When a new desired configuration is received, hello device app reports a pending configuration by changing hello information:</span></span>

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

<span data-ttu-id="712c4-119">Sedan vid ett senare tillfälle rapporterar hello enhetsapp hello lyckats eller misslyckats i den här åtgärden genom att uppdatera hello senare egenskapen.</span><span class="sxs-lookup"><span data-stu-id="712c4-119">Then, at some later time, hello device app will report hello success or failure of this operation by updating hello above property.</span></span>
<span data-ttu-id="712c4-120">Observera hur hello lösningens serverdel kan, när som helst tooquery hello status av hello konfigurationen på alla hello-enheter.</span><span class="sxs-lookup"><span data-stu-id="712c4-120">Note how hello solution back end is able, at any time, tooquery hello status of hello configuration process across all hello devices.</span></span>

<span data-ttu-id="712c4-121">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="712c4-121">This tutorial shows you how to:</span></span>

* <span data-ttu-id="712c4-122">Skapa en simulerad enhetsapp som tar emot konfigurationsuppdateringar från hello lösningens serverdel och rapporterar flera uppdateringar som *rapporterade egenskaper* på hello konfiguration uppdateringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="712c4-122">Create a simulated device app that receives configuration updates from hello solution back end, and reports multiple updates as *reported properties* on hello configuration update process.</span></span>
* <span data-ttu-id="712c4-123">Skapa en backend-app att uppdateringar hello önskad konfiguration för en enhet och sedan frågor hello konfigurationsprocessen för uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="712c4-123">Create a back-end app that updates hello desired configuration of a device, and then queries hello configuration update process.</span></span>

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
