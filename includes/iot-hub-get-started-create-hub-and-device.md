## <a name="create-an-iot-hub"></a><span data-ttu-id="f0514-101">Skapa en IoT Hub</span><span class="sxs-lookup"><span data-stu-id="f0514-101">Create an IoT hub</span></span>

1. <span data-ttu-id="f0514-102">På [Azure-portalen](https://portal.azure.com/) klickar du på **Nytt** > **Sakernas Internet** > **IoT Hub**.</span><span class="sxs-lookup"><span data-stu-id="f0514-102">In the [Azure portal](https://portal.azure.com/), click **New** > **Internet of Things** > **IoT Hub**.</span></span>

   ![Skapa en IoT-hubb på Azure-portalen](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. <span data-ttu-id="f0514-104">I rutan **IoT-hubb** anger du följande information för IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="f0514-104">In the **IoT hub** pane, enter the following information for your IoT hub:</span></span>

     <span data-ttu-id="f0514-105">**Namn**: Ange namnet på IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="f0514-105">**Name**: Enter the name of your IoT hub.</span></span> <span data-ttu-id="f0514-106">Om namnet som du anger är giltigt visas en grön bockmarkering.</span><span class="sxs-lookup"><span data-stu-id="f0514-106">If the name you enter is valid, a green check mark appears.</span></span>

     <span data-ttu-id="f0514-107">**Pris- och skalnivå**: Välj nivån **F1 - Kostnadsfri**.</span><span class="sxs-lookup"><span data-stu-id="f0514-107">**Pricing and scale tier**: Select the **F1 - Free** tier.</span></span> <span data-ttu-id="f0514-108">Det här alternativet är tillräckligt för den här demonstrationen.</span><span class="sxs-lookup"><span data-stu-id="f0514-108">This option is sufficient for this demo.</span></span> <span data-ttu-id="f0514-109">Mer information finns i avsnittet om [pris- och skalnivåer](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="f0514-109">For more information, see the [Pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

     <span data-ttu-id="f0514-110">**Resursgrupp**: Skapa en resursgrupp som ska vara värd för IoT-hubben eller använd en befintlig.</span><span class="sxs-lookup"><span data-stu-id="f0514-110">**Resource group**: Create a resource group to host the IoT hub or use an existing one.</span></span> <span data-ttu-id="f0514-111">Mer information finns i [Använda resursgrupper för att hantera Azure-resurser](../articles/azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f0514-111">For more information, see [Use resource groups to manage your Azure resources](../articles/azure-resource-manager/resource-group-portal.md).</span></span>

     <span data-ttu-id="f0514-112">**Plats**: Välj den plats som är närmast dig där IoT-hubben ska skapas.</span><span class="sxs-lookup"><span data-stu-id="f0514-112">**Location**: Select the closest location to you where the IoT hub is created.</span></span>

     <span data-ttu-id="f0514-113">**Fäst på instrumentpanelen**: Välj det här alternativet för enkel åtkomst till IoT-hubben från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f0514-113">**Pin to dashboard**: Select this option for easy access to your IoT hub from the dashboard.</span></span>

   ![Ange information för att skapa IoT-hubben](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. <span data-ttu-id="f0514-115">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f0514-115">Click **Create**.</span></span> <span data-ttu-id="f0514-116">Det kan ta några minuter innan IoT-hubben har skapats.</span><span class="sxs-lookup"><span data-stu-id="f0514-116">Your IoT hub might take a few minutes to create.</span></span> <span data-ttu-id="f0514-117">Du kan se förloppet i **meddelandefönstret**.</span><span class="sxs-lookup"><span data-stu-id="f0514-117">You can see progress in the **Notifications** pane.</span></span>

   ![Visa meddelanden om IoT-hubbens förlopp](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. <span data-ttu-id="f0514-119">När IoT-hubben har skapats klickar du på den på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="f0514-119">After your IoT hub is created, click it on the dashboard.</span></span> <span data-ttu-id="f0514-120">Anteckna **värdnamnet** och klicka sedan på **Principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="f0514-120">Make a note of the **Hostname**, and then click **Shared access policies**.</span></span>

   ![Hämta värdnamnet för IoT-hubben](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. <span data-ttu-id="f0514-122">I rutan **Policyer för delad åtkomst** klickar du på principen **iothubowner** och kopierar och skriver ner IoT-hubbens **anslutningssträng**.</span><span class="sxs-lookup"><span data-stu-id="f0514-122">In the **Shared access policies** pane, click the **iothubowner** policy, and then copy and make a note of the **Connection string** of your IoT hub.</span></span> <span data-ttu-id="f0514-123">Mer information finns i [Control access to IoT Hub](../articles/iot-hub/iot-hub-devguide-security.md) (Kontrollera åtkomsten till IoT Hub).</span><span class="sxs-lookup"><span data-stu-id="f0514-123">For more information, see [Control access to IoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).</span></span>

> [!NOTE] 
<span data-ttu-id="f0514-124">Du behöver inte den här iothubowner-anslutningssträngen i den här konfigurationskursen.</span><span class="sxs-lookup"><span data-stu-id="f0514-124">You will not need this iothubowner connection string for this set-up tutorial.</span></span> <span data-ttu-id="f0514-125">Men du kan behöva den i vissa självstudiekurser om olika IoT-scenarier när du har slutfört den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f0514-125">However, you may need it for some of the tutorials on different IoT scenarios after you complete this set-up.</span></span>

   ![Hämta IoT-hubbens anslutningssträng](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a><span data-ttu-id="f0514-127">Registrera en enhet i IoT-hubben för din enhet</span><span class="sxs-lookup"><span data-stu-id="f0514-127">Register a device in the IoT hub for your device</span></span>

1. <span data-ttu-id="f0514-128">Öppna IoT-hubben på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f0514-128">In the [Azure portal](https://portal.azure.com/), open your IoT hub.</span></span>

2. <span data-ttu-id="f0514-129">Klicka på **Enhetsutforskare**.</span><span class="sxs-lookup"><span data-stu-id="f0514-129">Click **Device Explorer**.</span></span>
3. <span data-ttu-id="f0514-130">Klicka på **Lägg till** i rutan Enhetsutforskare för att lägga till en enhet till IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="f0514-130">In the Device Explorer pane, click **Add** to add a device to your IoT hub.</span></span> <span data-ttu-id="f0514-131">Gör något av följande:</span><span class="sxs-lookup"><span data-stu-id="f0514-131">Then do the following:</span></span>

   <span data-ttu-id="f0514-132">**Enhets-ID**: Ange ID:t för den nya enheten.</span><span class="sxs-lookup"><span data-stu-id="f0514-132">**Device ID**: Enter the ID of the new device.</span></span> <span data-ttu-id="f0514-133">Enhets-ID är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="f0514-133">Device IDs are case sensitive.</span></span>

   <span data-ttu-id="f0514-134">**Autentiseringstyp**: Välj **Symmetrisk nyckel**.</span><span class="sxs-lookup"><span data-stu-id="f0514-134">**Authentication Type**: Select **Symmetric Key**.</span></span>

   <span data-ttu-id="f0514-135">**Generera nycklar automatiskt**: Markera den här kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="f0514-135">**Auto Generate Keys**: Select this check box.</span></span>

   <span data-ttu-id="f0514-136">**Anslut enhet till IoT Hub**: Klicka på **Aktivera**.</span><span class="sxs-lookup"><span data-stu-id="f0514-136">**Connect device to IoT Hub**: Click **Enable**.</span></span>

   ![Lägg till en enhet i Enhetsutforskaren för IoT-hubben](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. <span data-ttu-id="f0514-138">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f0514-138">Click **Save**.</span></span>
5. <span data-ttu-id="f0514-139">När enheten har skapats öppnar du enheten i rutan **Enhetsutforskare**.</span><span class="sxs-lookup"><span data-stu-id="f0514-139">After the device is created, open the device in the **Device Explorer** pane.</span></span>
6. <span data-ttu-id="f0514-140">Skriv ner anslutningssträngens primära nyckel.</span><span class="sxs-lookup"><span data-stu-id="f0514-140">Make a note of the primary key of the connection string.</span></span>

   ![Hämta enhetens anslutningssträng](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
