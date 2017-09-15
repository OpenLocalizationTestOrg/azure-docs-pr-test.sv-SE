> [!div class="op_single_selector"]
> * [<span data-ttu-id="41214-101">Linux</span><span class="sxs-lookup"><span data-stu-id="41214-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [<span data-ttu-id="41214-102">Windows</span><span class="sxs-lookup"><span data-stu-id="41214-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

<span data-ttu-id="41214-103">Den här genomgången av den [simulerade enheten molnet överför exempel] visar hur du använder [Azure IoT kant] [ lnk-sdk] skicka enhet till moln telemetri till IoT-hubb från simulerade enheter .</span><span class="sxs-lookup"><span data-stu-id="41214-103">This walkthrough of the [Simulated Device Cloud Upload sample] shows you how to use [Azure IoT Edge][lnk-sdk] to send device-to-cloud telemetry to IoT Hub from simulated devices.</span></span>

<span data-ttu-id="41214-104">Den här genomgången omfattar:</span><span class="sxs-lookup"><span data-stu-id="41214-104">This walkthrough covers:</span></span>

* <span data-ttu-id="41214-105">**Arkitektur för**: arkitektur information om den [simulerade enheten molnet överför exempel].</span><span class="sxs-lookup"><span data-stu-id="41214-105">**Architecture**: architectural information about the [Simulated Device Cloud Upload sample].</span></span>
* <span data-ttu-id="41214-106">**Skapa och kör**: stegen som krävs för att bygga och köra exemplet.</span><span class="sxs-lookup"><span data-stu-id="41214-106">**Build and run**: the steps required to build and run the sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="41214-107">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="41214-107">Architecture</span></span>

<span data-ttu-id="41214-108">Den [simulerade enheten molnet överför exempel] visar hur du skapar en gateway som skickar telemetri från simulerade enheter till en IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="41214-108">The [Simulated Device Cloud Upload sample] shows how to create a gateway that sends telemetry from simulated devices to an IoT hub.</span></span> <span data-ttu-id="41214-109">En enhet kanske inte kan ansluta direkt till IoT-hubb eftersom enheten:</span><span class="sxs-lookup"><span data-stu-id="41214-109">A device may not be able to connect directly to IoT Hub because the device:</span></span>

* <span data-ttu-id="41214-110">Använder inte ett kommunikationsprotokoll som tolkas av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="41214-110">Does not use a communications protocol understood by IoT Hub.</span></span>
* <span data-ttu-id="41214-111">Är inte smart komma ihåg identiteten som tilldelats av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="41214-111">Is not smart enough to remember the identity assigned to it by IoT Hub.</span></span>

<span data-ttu-id="41214-112">En IoT-gräns-gatewayen kan lösa dessa problem på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="41214-112">An IoT Edge gateway can solve these problems in the following ways:</span></span>

* <span data-ttu-id="41214-113">Gatewayen förstår protokollet som används av enheten tar emot enhet till moln telemetri från enheten och vidarebefordrar meddelandena till IoT-hubb med ett protokoll som tolkas av IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="41214-113">The gateway understands the protocol used by the device, receives device-to-cloud telemetry from the device, and forwards those messages to IoT Hub using a protocol understood by the IoT hub.</span></span>

* <span data-ttu-id="41214-114">Gatewayen mappar IoT-hubb identiteter till enheter och fungerar som proxy när en enhet skickar meddelanden till IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="41214-114">The gateway maps IoT Hub identities to devices and acts as a proxy when a device sends messages to IoT Hub.</span></span>

<span data-ttu-id="41214-115">Följande diagram visar huvudkomponenterna i exemplet, inklusive IoT kant-moduler:</span><span class="sxs-lookup"><span data-stu-id="41214-115">The following diagram shows the main components of the sample, including the IoT Edge modules:</span></span>

![][1]

<span data-ttu-id="41214-116">Moduler skickar inte meddelanden direkt till varandra.</span><span class="sxs-lookup"><span data-stu-id="41214-116">The modules do not pass messages directly to each other.</span></span> <span data-ttu-id="41214-117">Moduler publicera meddelanden i ett internt Service broker som levererar meddelanden till de moduler som använder en mekanism för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="41214-117">The modules publish messages to an internal broker that delivers the messages to the other modules using a subscription mechanism.</span></span> <span data-ttu-id="41214-118">Mer information finns i [Kom igång med Azure IoT kant][lnk-gw-getstarted].</span><span class="sxs-lookup"><span data-stu-id="41214-118">For more information, see [Get started with Azure IoT Edge][lnk-gw-getstarted].</span></span>

### <a name="protocol-ingestion-module"></a><span data-ttu-id="41214-119">Modul för protokollinhämtning</span><span class="sxs-lookup"><span data-stu-id="41214-119">Protocol ingestion module</span></span>

<span data-ttu-id="41214-120">Denna modul är startpunkten för att ta emot data från enheter, via gatewayen och i molnet.</span><span class="sxs-lookup"><span data-stu-id="41214-120">This module is the starting point for receiving data from devices, through the gateway, and into the cloud.</span></span> <span data-ttu-id="41214-121">I det här exemplet modulen:</span><span class="sxs-lookup"><span data-stu-id="41214-121">In the sample, the module:</span></span>

1. <span data-ttu-id="41214-122">Skapar simulerade temperatur data.</span><span class="sxs-lookup"><span data-stu-id="41214-122">Creates simulated temperature data.</span></span> <span data-ttu-id="41214-123">Om du använder fysiska enheter läser modulen data från de fysiska enheterna.</span><span class="sxs-lookup"><span data-stu-id="41214-123">If you use physical devices, the module reads data from those physical devices.</span></span>
1. <span data-ttu-id="41214-124">Skapar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="41214-124">Creates a message.</span></span>
1. <span data-ttu-id="41214-125">Placerar simulerade temperatur-data i innehållet i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="41214-125">Places the simulated temperature data into the message content.</span></span>
1. <span data-ttu-id="41214-126">Lägger till en egenskap med en MAC-adress som falska meddelandet.</span><span class="sxs-lookup"><span data-stu-id="41214-126">Adds a property with a fake MAC address to the message.</span></span>
1. <span data-ttu-id="41214-127">Gör meddelandet tillgängligt till nästa modul i kedjan.</span><span class="sxs-lookup"><span data-stu-id="41214-127">Makes the message available to the next module in the chain.</span></span>

<span data-ttu-id="41214-128">Modulen kallas **protokollet X införandet** i föregående diagram kallas **simulerade enheten** i källkoden.</span><span class="sxs-lookup"><span data-stu-id="41214-128">The module called **Protocol X ingestion** in the previous diagram is called **Simulated device** in the source code.</span></span>

### <a name="mac-lt-gt-iot-hub-id-module"></a><span data-ttu-id="41214-129">MAC &lt;-&gt; IoT Hub ID-modul</span><span class="sxs-lookup"><span data-stu-id="41214-129">MAC &lt;-&gt; IoT Hub ID module</span></span>

<span data-ttu-id="41214-130">Den här modulen söker efter meddelanden som har en egenskap för Mac-adress.</span><span class="sxs-lookup"><span data-stu-id="41214-130">This module scans for messages that have a Mac address property.</span></span> <span data-ttu-id="41214-131">I det här exemplet protokollet införandet modulen lägger du till egenskapen MAC-adressen.</span><span class="sxs-lookup"><span data-stu-id="41214-131">In the sample, the protocol ingestion module adds the MAC address property.</span></span> <span data-ttu-id="41214-132">Om modulen hittar sådan egenskap, den lägger till en annan egenskap med en IoT-hubb enhet för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="41214-132">If the module finds such a property, it adds another property with an IoT Hub device key to the message.</span></span> <span data-ttu-id="41214-133">Modulen sedan gör meddelandet tillgängligt till nästa modul i kedjan.</span><span class="sxs-lookup"><span data-stu-id="41214-133">The module then makes the message available to the next module in the chain.</span></span>

<span data-ttu-id="41214-134">Utvecklaren anger en mappning mellan MAC-adresser och IoT-hubb identiteter att associera simulerade enheterna med IoT-hubb enheten identiteter.</span><span class="sxs-lookup"><span data-stu-id="41214-134">The developer sets up a mapping between MAC addresses and IoT Hub identities to associate the simulated devices with IoT Hub device identities.</span></span> <span data-ttu-id="41214-135">Utvecklaren lägger till mappningen manuellt som en del av modulkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="41214-135">The developer adds the mapping manually as part of the module configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="41214-136">I det här exemplet används en MAC-adress som en unik enhetsidentifierare och kopplar den till en IoT Hub-enhetsidentitet.</span><span class="sxs-lookup"><span data-stu-id="41214-136">This sample uses a MAC address as a unique device identifier and correlates it with an IoT Hub device identity.</span></span> <span data-ttu-id="41214-137">Du kan emellertid skriva din egen modul som använder en annan unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="41214-137">However, you can write your own module that uses a different unique identifier.</span></span> <span data-ttu-id="41214-138">Till exempel kan dina enheter har unika serienummer eller telemetridata kan innehålla ett unikt inbäddade enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="41214-138">For example, your devices may have unique serial numbers or the telemetry data may include a unique embedded device name.</span></span>

### <a name="iot-hub-communication-module"></a><span data-ttu-id="41214-139">IoT Hub-kommunikationsmodul</span><span class="sxs-lookup"><span data-stu-id="41214-139">IoT Hub communication module</span></span>

<span data-ttu-id="41214-140">Den här modulen tar meddelanden med en IoT-hubb nyckelegenskapen för enheter som har tilldelats av den föregående modulen.</span><span class="sxs-lookup"><span data-stu-id="41214-140">This module takes messages with an IoT Hub device key property that was assigned by the previous module.</span></span> <span data-ttu-id="41214-141">Modulen skickar meddelandet till IoT-hubb med hjälp av HTTP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="41214-141">The module sends the message content to IoT Hub using the HTTP protocol.</span></span> <span data-ttu-id="41214-142">HTTP är ett av de tre protokollen som kan tolkas av IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="41214-142">HTTP is one of the three protocols understood by IoT Hub.</span></span>

<span data-ttu-id="41214-143">I stället för att öppna en anslutning för varje simulerad enhet öppnar den här modulen en HTTP-anslutning från gateway för IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="41214-143">Instead of opening a connection for each simulated device, this module opens a single HTTP connection from the gateway to the IoT hub.</span></span> <span data-ttu-id="41214-144">Modulen multiplexes sedan anslutningar från alla simulerade enheter över den anslutningen.</span><span class="sxs-lookup"><span data-stu-id="41214-144">The module then multiplexes connections from all the simulated devices over that connection.</span></span> <span data-ttu-id="41214-145">På så sätt kan en enda gateway för att ansluta många fler enheter.</span><span class="sxs-lookup"><span data-stu-id="41214-145">This approach enables a single gateway to connect many more devices.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="41214-146">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="41214-146">Before you get started</span></span>

<span data-ttu-id="41214-147">Innan du börjar måste du:</span><span class="sxs-lookup"><span data-stu-id="41214-147">Before you get started, you must:</span></span>

* <span data-ttu-id="41214-148">[Skapa en IoT-hubb] [ lnk-create-hub] i din Azure-prenumeration, måste namnet på din hubb för att slutföra den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="41214-148">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need the name of your hub to complete this walkthrough.</span></span> <span data-ttu-id="41214-149">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="41214-149">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="41214-150">Lägga till två enheter till din IoT-hubb och anteckna deras ID och nycklar för enheten.</span><span class="sxs-lookup"><span data-stu-id="41214-150">Add two devices to your IoT hub and make a note of their ids and device keys.</span></span> <span data-ttu-id="41214-151">Du kan använda den [enheten explorer] [ lnk-device-explorer] eller [iothub explorer] [ lnk-iothub-explorer] verktyg för att lägga till dina enheter till IoT-hubb som du skapade i föregående steg och hämta sina nycklar.</span><span class="sxs-lookup"><span data-stu-id="41214-151">You can use the [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool to add your devices to the IoT hub you created in the previous step and retrieve their keys.</span></span>

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
<span data-ttu-id="41214-152">[simulerade enheten molnet överför exempel]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md</span><span class="sxs-lookup"><span data-stu-id="41214-152">[Simulated Device Cloud Upload sample]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md</span></span>
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md