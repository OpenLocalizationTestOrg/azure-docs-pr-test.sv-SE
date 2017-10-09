## <a name="create-an-iot-hub"></a><span data-ttu-id="de1cf-101">Skapa en IoT Hub</span><span class="sxs-lookup"><span data-stu-id="de1cf-101">Create an IoT hub</span></span>

1. <span data-ttu-id="de1cf-102">I hello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** > **Sakernas Internet** > **IoT-hubb**.</span><span class="sxs-lookup"><span data-stu-id="de1cf-102">In hello [Azure portal](https://portal.azure.com/), click **New** > **Internet of Things** > **IoT Hub**.</span></span>

   ![Skapa en IoT-hubb i hello Azure-portalen](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. <span data-ttu-id="de1cf-104">I hello **IoT-hubb** rutan Ange följande information för din IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="de1cf-104">In hello **IoT hub** pane, enter hello following information for your IoT hub:</span></span>

     <span data-ttu-id="de1cf-105">**Namnet**: Ange hello namn för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="de1cf-105">**Name**: Enter hello name of your IoT hub.</span></span> <span data-ttu-id="de1cf-106">Om hello-namn som du anger är giltigt, visas en grön bock.</span><span class="sxs-lookup"><span data-stu-id="de1cf-106">If hello name you enter is valid, a green check mark appears.</span></span>

     <span data-ttu-id="de1cf-107">**Pris-och skala**: Välj hello **F1 - ledigt** nivå.</span><span class="sxs-lookup"><span data-stu-id="de1cf-107">**Pricing and scale tier**: Select hello **F1 - Free** tier.</span></span> <span data-ttu-id="de1cf-108">Det här alternativet är tillräckligt för den här demonstrationen.</span><span class="sxs-lookup"><span data-stu-id="de1cf-108">This option is sufficient for this demo.</span></span> <span data-ttu-id="de1cf-109">Mer information finns i hello [priser och skalnivå](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="de1cf-109">For more information, see hello [Pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

     <span data-ttu-id="de1cf-110">**Resursgruppen**: skapa en resurs grupp toohost hello IoT-hubb eller använda en befintlig.</span><span class="sxs-lookup"><span data-stu-id="de1cf-110">**Resource group**: Create a resource group toohost hello IoT hub or use an existing one.</span></span> <span data-ttu-id="de1cf-111">Mer information finns i [Använd resursgrupper toomanage resurserna i Azure](../articles/azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="de1cf-111">For more information, see [Use resource groups toomanage your Azure resources](../articles/azure-resource-manager/resource-group-portal.md).</span></span>

     <span data-ttu-id="de1cf-112">**Plats**: Välj hello den närmaste platsen tooyou där hello IoT-hubben har skapats.</span><span class="sxs-lookup"><span data-stu-id="de1cf-112">**Location**: Select hello closest location tooyou where hello IoT hub is created.</span></span>

     <span data-ttu-id="de1cf-113">**PIN-kod toodashboard**: Välj det här alternativet för enkel åtkomst tooyour IoT-hubb hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="de1cf-113">**Pin toodashboard**: Select this option for easy access tooyour IoT hub from hello dashboard.</span></span>

   ![Ange information toocreate din IoT-hubb](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. <span data-ttu-id="de1cf-115">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="de1cf-115">Click **Create**.</span></span> <span data-ttu-id="de1cf-116">IoT-hubb kan ta några minuter toocreate.</span><span class="sxs-lookup"><span data-stu-id="de1cf-116">Your IoT hub might take a few minutes toocreate.</span></span> <span data-ttu-id="de1cf-117">Du kan se förloppet på hello **meddelanden** fönstret.</span><span class="sxs-lookup"><span data-stu-id="de1cf-117">You can see progress in hello **Notifications** pane.</span></span>

   ![Visa meddelanden om IoT-hubbens förlopp](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. <span data-ttu-id="de1cf-119">När du har skapat din IoT-hubb klickar du på hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="de1cf-119">After your IoT hub is created, click it on hello dashboard.</span></span> <span data-ttu-id="de1cf-120">Anteckna hello **värdnamn**, och klicka sedan på **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="de1cf-120">Make a note of hello **Hostname**, and then click **Shared access policies**.</span></span>

   ![Hämta hello värdnamnet för din IoT-hubb](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. <span data-ttu-id="de1cf-122">I hello **principer för delad åtkomst** rutan klickar du på hello **iothubowner** princip, och sedan kopiera och gjort en notering om hello **anslutningssträngen** för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="de1cf-122">In hello **Shared access policies** pane, click hello **iothubowner** policy, and then copy and make a note of hello **Connection string** of your IoT hub.</span></span> <span data-ttu-id="de1cf-123">Mer information finns i [kontroll åtkomst tooIoT hubb](../articles/iot-hub/iot-hub-devguide-security.md).</span><span class="sxs-lookup"><span data-stu-id="de1cf-123">For more information, see [Control access tooIoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).</span></span>

> [!NOTE] 
<span data-ttu-id="de1cf-124">Du behöver inte den här iothubowner-anslutningssträngen i den här konfigurationskursen.</span><span class="sxs-lookup"><span data-stu-id="de1cf-124">You will not need this iothubowner connection string for this set-up tutorial.</span></span> <span data-ttu-id="de1cf-125">Men kanske du måste den för några av hello självstudier om olika IoT-scenarier när du har slutfört den här uppsättningen.</span><span class="sxs-lookup"><span data-stu-id="de1cf-125">However, you may need it for some of hello tutorials on different IoT scenarios after you complete this set-up.</span></span>

   ![Hämta IoT-hubbens anslutningssträng](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-hello-iot-hub-for-your-device"></a><span data-ttu-id="de1cf-127">Registrera en enhet i hello IoT-hubb för din enhet</span><span class="sxs-lookup"><span data-stu-id="de1cf-127">Register a device in hello IoT hub for your device</span></span>

1. <span data-ttu-id="de1cf-128">I hello [Azure-portalen](https://portal.azure.com/), öppna din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="de1cf-128">In hello [Azure portal](https://portal.azure.com/), open your IoT hub.</span></span>

2. <span data-ttu-id="de1cf-129">Klicka på **Enhetsutforskare**.</span><span class="sxs-lookup"><span data-stu-id="de1cf-129">Click **Device Explorer**.</span></span>
3. <span data-ttu-id="de1cf-130">I hello enheten Explorer-fönstret klickar du på **Lägg till** tooadd enhet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="de1cf-130">In hello Device Explorer pane, click **Add** tooadd a device tooyour IoT hub.</span></span> <span data-ttu-id="de1cf-131">Sedan hello följande:</span><span class="sxs-lookup"><span data-stu-id="de1cf-131">Then do hello following:</span></span>

   <span data-ttu-id="de1cf-132">**Enhets-ID**: Ange hello ID hello ny enhet.</span><span class="sxs-lookup"><span data-stu-id="de1cf-132">**Device ID**: Enter hello ID of hello new device.</span></span> <span data-ttu-id="de1cf-133">Enhets-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="de1cf-133">Device IDs are case sensitive.</span></span>

   <span data-ttu-id="de1cf-134">**Autentiseringstyp**: Välj **Symmetrisk nyckel**.</span><span class="sxs-lookup"><span data-stu-id="de1cf-134">**Authentication Type**: Select **Symmetric Key**.</span></span>

   <span data-ttu-id="de1cf-135">**Generera nycklar automatiskt**: Markera den här kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="de1cf-135">**Auto Generate Keys**: Select this check box.</span></span>

   <span data-ttu-id="de1cf-136">**Ansluta enheten tooIoT hubb**: Klicka på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="de1cf-136">**Connect device tooIoT Hub**: Click **Enable**.</span></span>

   ![Lägga till en enhet i hello enheten Explorer för din IoT-hubb](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. <span data-ttu-id="de1cf-138">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="de1cf-138">Click **Save**.</span></span>
5. <span data-ttu-id="de1cf-139">När du har skapat hello enheten öppna hello enhet i hello **enheten Explorer** fönstret.</span><span class="sxs-lookup"><span data-stu-id="de1cf-139">After hello device is created, open hello device in hello **Device Explorer** pane.</span></span>
6. <span data-ttu-id="de1cf-140">Anteckna hello primärnyckel för hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="de1cf-140">Make a note of hello primary key of hello connection string.</span></span>

   ![Hämta anslutningssträngen för hello-enhet](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
