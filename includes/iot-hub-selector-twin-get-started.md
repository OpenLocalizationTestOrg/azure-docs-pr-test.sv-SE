> [!div class="op_single_selector"]
> * [<span data-ttu-id="e3c48-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="e3c48-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [<span data-ttu-id="e3c48-102">C#/node.js</span><span class="sxs-lookup"><span data-stu-id="e3c48-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [<span data-ttu-id="e3c48-103">C#</span><span class="sxs-lookup"><span data-stu-id="e3c48-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [<span data-ttu-id="e3c48-104">Java</span><span class="sxs-lookup"><span data-stu-id="e3c48-104">Java</span></span>](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)

<span data-ttu-id="e3c48-105">Enhetstvillingar är JSON-dokument som lagrar information om enhetstillstånd (metadata, konfigurationer och villkor).</span><span class="sxs-lookup"><span data-stu-id="e3c48-105">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="e3c48-106">IoT-hubb kvarstår en enhet dubbla för varje enhet som ansluter tooit.</span><span class="sxs-lookup"><span data-stu-id="e3c48-106">IoT Hub persists a device twin for each device that connects tooit.</span></span>

<span data-ttu-id="e3c48-107">Använd enhet twins till:</span><span class="sxs-lookup"><span data-stu-id="e3c48-107">Use device twins to:</span></span>

* <span data-ttu-id="e3c48-108">Lagra metadata om enheter från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="e3c48-108">Store device metadata from your solution back end.</span></span>
* <span data-ttu-id="e3c48-109">Rapportera aktuell statusinformation, till exempel tillgängliga funktioner och villkor (till exempel hello anslutningsmetod som används) från din enhet.</span><span class="sxs-lookup"><span data-stu-id="e3c48-109">Report current state information such as available capabilities and conditions (for example, hello connectivity method used) from your device app.</span></span>
* <span data-ttu-id="e3c48-110">Synkronisera hello tillståndet för tidskrävande arbetsflöden (till exempel uppdateringarna av inbyggd och konfiguration) mellan en enhetsapp och en backend-app.</span><span class="sxs-lookup"><span data-stu-id="e3c48-110">Synchronize hello state of long-running workflows (such as firmware and configuration updates) between a device app and a back-end app.</span></span>
* <span data-ttu-id="e3c48-111">Fråga din enhetsmetadata, konfiguration eller tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e3c48-111">Query your device metadata, configuration, or state.</span></span>

> [!NOTE]
> <span data-ttu-id="e3c48-112">Enheten twins är utformade för synkronisering och för att fråga efter enhetskonfigurationer och villkor.</span><span class="sxs-lookup"><span data-stu-id="e3c48-112">Device twins are designed for synchronization and for querying device configurations and conditions.</span></span> <span data-ttu-id="e3c48-113">Mer information om när toouse enhet twins finns i [förstå enheten twins][lnk-twins].</span><span class="sxs-lookup"><span data-stu-id="e3c48-113">More informations on when toouse device twins can be found in [Understand device twins][lnk-twins].</span></span>

<span data-ttu-id="e3c48-114">Enheten twins lagras i en IoT-hubb och innehålla:</span><span class="sxs-lookup"><span data-stu-id="e3c48-114">Device twins are stored in an IoT hub and contain:</span></span>

* <span data-ttu-id="e3c48-115">*taggar*, enhetens metadata endast kan nås av hello lösningens serverdel;</span><span class="sxs-lookup"><span data-stu-id="e3c48-115">*tags*, device metadata accessible only by hello solution back end;</span></span>
* <span data-ttu-id="e3c48-116">*Egenskaper för Desired*, JSON-objekt som kan ändras av hello lösning tillbaka slutet och observeras av hello enhetsapp, och</span><span class="sxs-lookup"><span data-stu-id="e3c48-116">*desired properties*, JSON objects modifiable by hello solution back end and observable by hello device app; and</span></span>
* <span data-ttu-id="e3c48-117">*rapporterade egenskaper*, JSON-objekt kan ändras av hello enhetsapp och läsas av hello lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="e3c48-117">*reported properties*, JSON objects modifiable by hello device app and readable by hello solution back end.</span></span> <span data-ttu-id="e3c48-118">Taggar och egenskaper får inte innehålla matriser, men kan vara kapslade objekt.</span><span class="sxs-lookup"><span data-stu-id="e3c48-118">Tags and properties cannot contain arrays, but objects can be nested.</span></span>

![][img-twin]

<span data-ttu-id="e3c48-119">Dessutom kan hello lösningens serverdel fråga enheten twins baserat på alla hello ovanför data.</span><span class="sxs-lookup"><span data-stu-id="e3c48-119">Additionally, hello solution back end can query device twins based on all hello above data.</span></span>
<span data-ttu-id="e3c48-120">Se för[förstå enheten twins] [ lnk-twins] för mer information om enheten twins och toohello [IoT-hubb frågespråket] [ lnk-query] referens för frågor.</span><span class="sxs-lookup"><span data-stu-id="e3c48-120">Refer too[Understand device twins][lnk-twins] for more information about device twins, and toohello [IoT Hub query language][lnk-query] reference for querying.</span></span>

> [!NOTE]
> <span data-ttu-id="e3c48-121">Just nu är enheten twins kan endast nås från enheter som ansluter tooIoT hubb med hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="e3c48-121">At this time, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span> <span data-ttu-id="e3c48-122">Se toohello [MQTT stöd] [ lnk-devguide-mqtt] artikel anvisningar för hur tooconvert befintliga enheten app toouse MQTT.</span><span class="sxs-lookup"><span data-stu-id="e3c48-122">Please refer toohello [MQTT support][lnk-devguide-mqtt] article for instructions on how tooconvert existing device app toouse MQTT.</span></span>

<span data-ttu-id="e3c48-123">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="e3c48-123">This tutorial shows you how to:</span></span>

* <span data-ttu-id="e3c48-124">Skapa en backend-app som lägger till *taggar* tooa enheten dubbla och en simulerad enhetsapp som rapporterar anslutningen channel som en *rapporterade egenskapen* på hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="e3c48-124">Create a back-end app that adds *tags* tooa device twin, and a simulated device app that reports its connectivity channel as a *reported property* on hello device twin.</span></span>
* <span data-ttu-id="e3c48-125">Fråga enheter från en backend-app med hjälp av filter på hello taggar och egenskaper som tidigare har skapat.</span><span class="sxs-lookup"><span data-stu-id="e3c48-125">Query devices from your back-end app using filters on hello tags and properties previously created.</span></span>

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md