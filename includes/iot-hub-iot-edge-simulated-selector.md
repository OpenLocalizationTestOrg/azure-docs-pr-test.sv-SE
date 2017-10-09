> [!div class="op_single_selector"]
> * [<span data-ttu-id="5f41f-101">Linux</span><span class="sxs-lookup"><span data-stu-id="5f41f-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [<span data-ttu-id="5f41f-102">Windows</span><span class="sxs-lookup"><span data-stu-id="5f41f-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

<span data-ttu-id="5f41f-103">Den här genomgången av hello [simulerade enheten molnet överför exempel] visas hur toouse [Azure IoT kant] [ lnk-sdk] toosend enhet till moln telemetri tooIoT hubb från simulerade enheter.</span><span class="sxs-lookup"><span data-stu-id="5f41f-103">This walkthrough of hello [Simulated Device Cloud Upload sample] shows you how toouse [Azure IoT Edge][lnk-sdk] toosend device-to-cloud telemetry tooIoT Hub from simulated devices.</span></span>

<span data-ttu-id="5f41f-104">Den här genomgången omfattar:</span><span class="sxs-lookup"><span data-stu-id="5f41f-104">This walkthrough covers:</span></span>

* <span data-ttu-id="5f41f-105">**Arkitektur för**: arkitektur information om hello [simulerade enheten molnet överför exempel].</span><span class="sxs-lookup"><span data-stu-id="5f41f-105">**Architecture**: architectural information about hello [Simulated Device Cloud Upload sample].</span></span>
* <span data-ttu-id="5f41f-106">**Skapa och köra**: hello steg krävs toobuild och kör hello exempel.</span><span class="sxs-lookup"><span data-stu-id="5f41f-106">**Build and run**: hello steps required toobuild and run hello sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="5f41f-107">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="5f41f-107">Architecture</span></span>

<span data-ttu-id="5f41f-108">Hej [simulerade enheten molnet överför exempel] visar hur toocreate en gateway som skickar telemetri från simulerade enheter tooan IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5f41f-108">hello [Simulated Device Cloud Upload sample] shows how toocreate a gateway that sends telemetry from simulated devices tooan IoT hub.</span></span> <span data-ttu-id="5f41f-109">En enhet kanske inte kan tooconnect direkt tooIoT hubb eftersom hello enhet:</span><span class="sxs-lookup"><span data-stu-id="5f41f-109">A device may not be able tooconnect directly tooIoT Hub because hello device:</span></span>

* <span data-ttu-id="5f41f-110">Använder inte ett kommunikationsprotokoll som tolkas av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5f41f-110">Does not use a communications protocol understood by IoT Hub.</span></span>
* <span data-ttu-id="5f41f-111">Är inte tillräckligt smart tooremember hello identitet som tilldelades tooit i IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="5f41f-111">Is not smart enough tooremember hello identity assigned tooit by IoT Hub.</span></span>

<span data-ttu-id="5f41f-112">En IoT-gräns-gatewayen kan lösa dessa problem i hello följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5f41f-112">An IoT Edge gateway can solve these problems in hello following ways:</span></span>

* <span data-ttu-id="5f41f-113">hello gateway förstår hello-protokoll som används av hello enheten tar emot enhet till moln telemetri från hello enhet och vidarebefordrar dessa meddelanden tooIoT hubb med ett protokoll som tolkas av hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5f41f-113">hello gateway understands hello protocol used by hello device, receives device-to-cloud telemetry from hello device, and forwards those messages tooIoT Hub using a protocol understood by hello IoT hub.</span></span>

* <span data-ttu-id="5f41f-114">hello gateway mappar IoT-hubb identiteter toodevices och fungerar som proxy när en enhet skickar meddelanden tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="5f41f-114">hello gateway maps IoT Hub identities toodevices and acts as a proxy when a device sends messages tooIoT Hub.</span></span>

<span data-ttu-id="5f41f-115">hello följande diagram visar hello huvudkomponenter hello exemplet, inklusive hello IoT kant moduler:</span><span class="sxs-lookup"><span data-stu-id="5f41f-115">hello following diagram shows hello main components of hello sample, including hello IoT Edge modules:</span></span>

![][1]

<span data-ttu-id="5f41f-116">hello moduler inte klarar meddelanden direkt tooeach andra.</span><span class="sxs-lookup"><span data-stu-id="5f41f-116">hello modules do not pass messages directly tooeach other.</span></span> <span data-ttu-id="5f41f-117">hello moduler publicera meddelanden tooan internt Service broker som levererar hello meddelanden toohello andra moduler med hjälp av en mekanism för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="5f41f-117">hello modules publish messages tooan internal broker that delivers hello messages toohello other modules using a subscription mechanism.</span></span> <span data-ttu-id="5f41f-118">Mer information finns i [Kom igång med Azure IoT kant][lnk-gw-getstarted].</span><span class="sxs-lookup"><span data-stu-id="5f41f-118">For more information, see [Get started with Azure IoT Edge][lnk-gw-getstarted].</span></span>

### <a name="protocol-ingestion-module"></a><span data-ttu-id="5f41f-119">Modul för protokollinhämtning</span><span class="sxs-lookup"><span data-stu-id="5f41f-119">Protocol ingestion module</span></span>

<span data-ttu-id="5f41f-120">Denna modul är hello som utgångspunkt för att ta emot data från enheter, via hello gateway och i hello moln.</span><span class="sxs-lookup"><span data-stu-id="5f41f-120">This module is hello starting point for receiving data from devices, through hello gateway, and into hello cloud.</span></span> <span data-ttu-id="5f41f-121">I exemplet hello hello modulen:</span><span class="sxs-lookup"><span data-stu-id="5f41f-121">In hello sample, hello module:</span></span>

1. <span data-ttu-id="5f41f-122">Skapar simulerade temperatur data.</span><span class="sxs-lookup"><span data-stu-id="5f41f-122">Creates simulated temperature data.</span></span> <span data-ttu-id="5f41f-123">Om du använder fysiska enheter läser hello modulen data från de fysiska enheterna.</span><span class="sxs-lookup"><span data-stu-id="5f41f-123">If you use physical devices, hello module reads data from those physical devices.</span></span>
1. <span data-ttu-id="5f41f-124">Skapar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="5f41f-124">Creates a message.</span></span>
1. <span data-ttu-id="5f41f-125">Placerar hello simulerade temperatur data i hello meddelandeinnehåll.</span><span class="sxs-lookup"><span data-stu-id="5f41f-125">Places hello simulated temperature data into hello message content.</span></span>
1. <span data-ttu-id="5f41f-126">Lägger till en egenskap med ett falska toohello meddelande för MAC-adress.</span><span class="sxs-lookup"><span data-stu-id="5f41f-126">Adds a property with a fake MAC address toohello message.</span></span>
1. <span data-ttu-id="5f41f-127">Gör hello meddelandet tillgängliga toohello nästa modul i hello kedja.</span><span class="sxs-lookup"><span data-stu-id="5f41f-127">Makes hello message available toohello next module in hello chain.</span></span>

<span data-ttu-id="5f41f-128">hello modulen kallas **protokollet X införandet** i hello föregående diagram kallas **simulerade enheten** i hello källkod.</span><span class="sxs-lookup"><span data-stu-id="5f41f-128">hello module called **Protocol X ingestion** in hello previous diagram is called **Simulated device** in hello source code.</span></span>

### <a name="mac-lt-gt-iot-hub-id-module"></a><span data-ttu-id="5f41f-129">MAC &lt;-&gt; IoT Hub ID-modul</span><span class="sxs-lookup"><span data-stu-id="5f41f-129">MAC &lt;-&gt; IoT Hub ID module</span></span>

<span data-ttu-id="5f41f-130">Den här modulen söker efter meddelanden som har en egenskap för Mac-adress.</span><span class="sxs-lookup"><span data-stu-id="5f41f-130">This module scans for messages that have a Mac address property.</span></span> <span data-ttu-id="5f41f-131">I exemplet hello lägger hello protokollet införandet modulen hello MAC-Adressegenskapen.</span><span class="sxs-lookup"><span data-stu-id="5f41f-131">In hello sample, hello protocol ingestion module adds hello MAC address property.</span></span> <span data-ttu-id="5f41f-132">Om modulen hello hittar sådan egenskap, läggs en annan egenskap med ett IoT-hubb enheten viktiga toohello meddelande.</span><span class="sxs-lookup"><span data-stu-id="5f41f-132">If hello module finds such a property, it adds another property with an IoT Hub device key toohello message.</span></span> <span data-ttu-id="5f41f-133">hello modulen gör sedan hello meddelandet tillgängliga toohello nästa modul i hello kedja.</span><span class="sxs-lookup"><span data-stu-id="5f41f-133">hello module then makes hello message available toohello next module in hello chain.</span></span>

<span data-ttu-id="5f41f-134">hello developer ställer in en mappning mellan MAC-adresser och IoT-hubb identiteter tooassociate hello simulerade enheter med IoT-hubb enheten identiteter.</span><span class="sxs-lookup"><span data-stu-id="5f41f-134">hello developer sets up a mapping between MAC addresses and IoT Hub identities tooassociate hello simulated devices with IoT Hub device identities.</span></span> <span data-ttu-id="5f41f-135">hello utvecklare lägger till hello mappning manuellt som en del av hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5f41f-135">hello developer adds hello mapping manually as part of hello module configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="5f41f-136">I det här exemplet används en MAC-adress som en unik enhetsidentifierare och kopplar den till en IoT Hub-enhetsidentitet.</span><span class="sxs-lookup"><span data-stu-id="5f41f-136">This sample uses a MAC address as a unique device identifier and correlates it with an IoT Hub device identity.</span></span> <span data-ttu-id="5f41f-137">Du kan emellertid skriva din egen modul som använder en annan unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="5f41f-137">However, you can write your own module that uses a different unique identifier.</span></span> <span data-ttu-id="5f41f-138">Till exempel dina enheter eller kan ha unika serienummer hello telemetridata kan innehålla ett unikt inbäddade enhetsnamn.</span><span class="sxs-lookup"><span data-stu-id="5f41f-138">For example, your devices may have unique serial numbers or hello telemetry data may include a unique embedded device name.</span></span>

### <a name="iot-hub-communication-module"></a><span data-ttu-id="5f41f-139">IoT Hub-kommunikationsmodul</span><span class="sxs-lookup"><span data-stu-id="5f41f-139">IoT Hub communication module</span></span>

<span data-ttu-id="5f41f-140">Den här modulen tar meddelanden med en IoT-hubb nyckelegenskapen för enheter som har tilldelats av föregående hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="5f41f-140">This module takes messages with an IoT Hub device key property that was assigned by hello previous module.</span></span> <span data-ttu-id="5f41f-141">hello modulen skickar hälsningsmeddelande innehåll tooIoT hubb med hello HTTP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="5f41f-141">hello module sends hello message content tooIoT Hub using hello HTTP protocol.</span></span> <span data-ttu-id="5f41f-142">HTTP är en av hello tre protokoll tolkas av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5f41f-142">HTTP is one of hello three protocols understood by IoT Hub.</span></span>

<span data-ttu-id="5f41f-143">I stället för att öppna en anslutning för varje simulerad enhet öppnar den här modulen en enskild HTTP-anslutning från hello gateway toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="5f41f-143">Instead of opening a connection for each simulated device, this module opens a single HTTP connection from hello gateway toohello IoT hub.</span></span> <span data-ttu-id="5f41f-144">hello modulen multiplexes sedan anslutningar från alla hello simulerade enheter över den anslutningen.</span><span class="sxs-lookup"><span data-stu-id="5f41f-144">hello module then multiplexes connections from all hello simulated devices over that connection.</span></span> <span data-ttu-id="5f41f-145">På så sätt kan en enda gateway-tooconnect många fler enheter.</span><span class="sxs-lookup"><span data-stu-id="5f41f-145">This approach enables a single gateway tooconnect many more devices.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="5f41f-146">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="5f41f-146">Before you get started</span></span>

<span data-ttu-id="5f41f-147">Innan du börjar måste du:</span><span class="sxs-lookup"><span data-stu-id="5f41f-147">Before you get started, you must:</span></span>

* <span data-ttu-id="5f41f-148">[Skapa en IoT-hubb] [ lnk-create-hub] i din Azure-prenumeration behöver du hello namnet på din hubb toocomplete den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="5f41f-148">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need hello name of your hub toocomplete this walkthrough.</span></span> <span data-ttu-id="5f41f-149">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="5f41f-149">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="5f41f-150">Lägg till två enheter tooyour IoT-hubb och anteckna deras ID och nycklar för enheten.</span><span class="sxs-lookup"><span data-stu-id="5f41f-150">Add two devices tooyour IoT hub and make a note of their ids and device keys.</span></span> <span data-ttu-id="5f41f-151">Du kan använda hello [enheten explorer] [ lnk-device-explorer] eller [iothub explorer] [ lnk-iothub-explorer] verktyget tooadd din enheter toohello IoT-hubb som du skapade i hello föregående steg och hämta sina nycklar.</span><span class="sxs-lookup"><span data-stu-id="5f41f-151">You can use hello [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool tooadd your devices toohello IoT hub you created in hello previous step and retrieve their keys.</span></span>

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[simulerade enheten molnet överför exempel]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md