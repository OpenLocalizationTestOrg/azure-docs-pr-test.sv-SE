> [!div class="op_single_selector"]
> * [<span data-ttu-id="60cd0-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="60cd0-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [<span data-ttu-id="60cd0-102">C#/node.js</span><span class="sxs-lookup"><span data-stu-id="60cd0-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [<span data-ttu-id="60cd0-103">C#</span><span class="sxs-lookup"><span data-stu-id="60cd0-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [<span data-ttu-id="60cd0-104">Java</span><span class="sxs-lookup"><span data-stu-id="60cd0-104">Java</span></span>](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)

<span data-ttu-id="60cd0-105">Enhetstvillingar är JSON-dokument som lagrar information om enhetstillstånd (metadata, konfigurationer och villkor).</span><span class="sxs-lookup"><span data-stu-id="60cd0-105">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="60cd0-106">IoT-hubb kvarstår en enhet dubbla för varje enhet som ansluter till den.</span><span class="sxs-lookup"><span data-stu-id="60cd0-106">IoT Hub persists a device twin for each device that connects to it.</span></span>

<span data-ttu-id="60cd0-107">Använd enhet twins till:</span><span class="sxs-lookup"><span data-stu-id="60cd0-107">Use device twins to:</span></span>

* <span data-ttu-id="60cd0-108">Lagra metadata om enheter från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="60cd0-108">Store device metadata from your solution back end.</span></span>
* <span data-ttu-id="60cd0-109">Rapportera aktuell statusinformation, till exempel tillgängliga funktioner och villkor (till exempel anslutning metoden används) från din enhet.</span><span class="sxs-lookup"><span data-stu-id="60cd0-109">Report current state information such as available capabilities and conditions (for example, the connectivity method used) from your device app.</span></span>
* <span data-ttu-id="60cd0-110">Synkronisera tillståndet för tidskrävande arbetsflöden (till exempel uppdateringarna av inbyggd och konfiguration) mellan en enhetsapp och en backend-app.</span><span class="sxs-lookup"><span data-stu-id="60cd0-110">Synchronize the state of long-running workflows (such as firmware and configuration updates) between a device app and a back-end app.</span></span>
* <span data-ttu-id="60cd0-111">Fråga din enhetsmetadata, konfiguration eller tillstånd.</span><span class="sxs-lookup"><span data-stu-id="60cd0-111">Query your device metadata, configuration, or state.</span></span>

> [!NOTE]
> <span data-ttu-id="60cd0-112">Enheten twins är utformade för synkronisering och för att fråga efter enhetskonfigurationer och villkor.</span><span class="sxs-lookup"><span data-stu-id="60cd0-112">Device twins are designed for synchronization and for querying device configurations and conditions.</span></span> <span data-ttu-id="60cd0-113">Mer information om när enheten twins finns i [förstå enheten twins][lnk-twins].</span><span class="sxs-lookup"><span data-stu-id="60cd0-113">More informations on when to use device twins can be found in [Understand device twins][lnk-twins].</span></span>

<span data-ttu-id="60cd0-114">Enheten twins lagras i en IoT-hubb och innehålla:</span><span class="sxs-lookup"><span data-stu-id="60cd0-114">Device twins are stored in an IoT hub and contain:</span></span>

* <span data-ttu-id="60cd0-115">*taggar*, enhetens metadata endast kan nås av lösningens serverdel;</span><span class="sxs-lookup"><span data-stu-id="60cd0-115">*tags*, device metadata accessible only by the solution back end;</span></span>
* <span data-ttu-id="60cd0-116">*Egenskaper för Desired*, JSON-objekt som kan ändras av lösningen tillbaka slutet och observeras via appen för enheter, och</span><span class="sxs-lookup"><span data-stu-id="60cd0-116">*desired properties*, JSON objects modifiable by the solution back end and observable by the device app; and</span></span>
* <span data-ttu-id="60cd0-117">*rapporterade egenskaper*, JSON-objekt kan ändras via appen för enheter och läsas av lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="60cd0-117">*reported properties*, JSON objects modifiable by the device app and readable by the solution back end.</span></span> <span data-ttu-id="60cd0-118">Taggar och egenskaper får inte innehålla matriser, men kan vara kapslade objekt.</span><span class="sxs-lookup"><span data-stu-id="60cd0-118">Tags and properties cannot contain arrays, but objects can be nested.</span></span>

![][img-twin]

<span data-ttu-id="60cd0-119">Dessutom kan lösningens serverdel fråga enheten twins baserat på data som ovan.</span><span class="sxs-lookup"><span data-stu-id="60cd0-119">Additionally, the solution back end can query device twins based on all the above data.</span></span>
<span data-ttu-id="60cd0-120">Referera till [förstå enheten twins] [ lnk-twins] för mer information om enheten twins och den [IoT-hubb frågespråket] [ lnk-query] referera för fråga.</span><span class="sxs-lookup"><span data-stu-id="60cd0-120">Refer to [Understand device twins][lnk-twins] for more information about device twins, and to the [IoT Hub query language][lnk-query] reference for querying.</span></span>

> [!NOTE]
> <span data-ttu-id="60cd0-121">Just nu är kan enheten twins endast nås från enheter som ansluter till IoT-hubb med MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="60cd0-121">At this time, device twins are accessible only from devices that connect to IoT Hub using the MQTT protocol.</span></span> <span data-ttu-id="60cd0-122">Mer information finns i [MQTT stöd] [ lnk-devguide-mqtt] artikel för instruktioner om hur du konverterar en befintlig enhetsapp att använda MQTT.</span><span class="sxs-lookup"><span data-stu-id="60cd0-122">Please refer to the [MQTT support][lnk-devguide-mqtt] article for instructions on how to convert existing device app to use MQTT.</span></span>

<span data-ttu-id="60cd0-123">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="60cd0-123">This tutorial shows you how to:</span></span>

* <span data-ttu-id="60cd0-124">Skapa en backend-app som lägger till *taggar* dubbla en enhet och en simulerad enhetsapp som rapporterar sin anslutning kanal som en *rapporterade egenskapen* på enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="60cd0-124">Create a back-end app that adds *tags* to a device twin, and a simulated device app that reports its connectivity channel as a *reported property* on the device twin.</span></span>
* <span data-ttu-id="60cd0-125">Fråga enheter från en backend-app med hjälp av filter på taggar och egenskaper som tidigare har skapat.</span><span class="sxs-lookup"><span data-stu-id="60cd0-125">Query devices from your back-end app using filters on the tags and properties previously created.</span></span>

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md