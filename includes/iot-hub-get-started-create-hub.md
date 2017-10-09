## <a name="create-an-iot-hub"></a><span data-ttu-id="87f86-101">Skapa en IoT Hub</span><span class="sxs-lookup"><span data-stu-id="87f86-101">Create an IoT hub</span></span>
<span data-ttu-id="87f86-102">Skapa en IoT-hubb som din simulerade enhet app tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="87f86-102">Create an IoT hub for your simulated device app tooconnect to.</span></span> <span data-ttu-id="87f86-103">hello följande steg visar hur toocomplete detta uppgift med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="87f86-103">hello following steps show you how toocomplete this task by using hello Azure portal.</span></span>

1. <span data-ttu-id="87f86-104">Logga in toohello [Azure-portalen][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="87f86-104">Sign in toohello [Azure portal][lnk-portal].</span></span>
1. <span data-ttu-id="87f86-105">I hello Jumpbar klickar du på **ny** > **Sakernas Internet** > **IoT-hubb**.</span><span class="sxs-lookup"><span data-stu-id="87f86-105">In hello Jumpbar, click **New** > **Internet of Things** > **IoT Hub**.</span></span>
   
    ![Jumpbar i Azure Portal][1]
1. <span data-ttu-id="87f86-107">I hello **IoT-hubb** bladet välj hello konfiguration för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="87f86-107">In hello **IoT hub** blade, choose hello configuration for your IoT hub.</span></span>
   
    ![IoT Hub-bladet][2]
   
   1. <span data-ttu-id="87f86-109">I hello **namn** anger du ett namn för din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="87f86-109">In hello **Name** box, enter a name for your IoT hub.</span></span> <span data-ttu-id="87f86-110">Om hello **namn** är giltig och tillgänglig, visas en grön bock i hello **namn** rutan.</span><span class="sxs-lookup"><span data-stu-id="87f86-110">If hello **Name** is valid and available, a green check mark appears in hello **Name** box.</span></span>
    [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]
   
   1. <span data-ttu-id="87f86-111">Välj en [pris- och skalningsnivå][lnk-pricing].</span><span class="sxs-lookup"><span data-stu-id="87f86-111">Select a [pricing and scale tier][lnk-pricing].</span></span> <span data-ttu-id="87f86-112">Den här guiden kräver inte en specifik nivå.</span><span class="sxs-lookup"><span data-stu-id="87f86-112">This tutorial does not require a specific tier.</span></span> <span data-ttu-id="87f86-113">Använd hello kostnadsfria F1-nivån för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="87f86-113">For this tutorial, use hello free F1 tier.</span></span>
   1. <span data-ttu-id="87f86-114">I **Resursgrupp** skapar du antingen en resursgrupp eller väljer en befintlig.</span><span class="sxs-lookup"><span data-stu-id="87f86-114">In **Resource group**, either create a resource group, or select an existing one.</span></span> <span data-ttu-id="87f86-115">Mer information finns i [resursnamnet grupper toomanage resurserna i Azure][lnk-resource-groups].</span><span class="sxs-lookup"><span data-stu-id="87f86-115">For more information, see [Using resource groups toomanage your Azure resources][lnk-resource-groups].</span></span>
   1. <span data-ttu-id="87f86-116">I **plats**, Välj hello plats toohost din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="87f86-116">In **Location**, select hello location toohost your IoT hub.</span></span> <span data-ttu-id="87f86-117">Välj den plats som är närmast dig för den här guiden.</span><span class="sxs-lookup"><span data-stu-id="87f86-117">For this tutorial, choose your nearest location.</span></span>
1. <span data-ttu-id="87f86-118">När du har valt dina konfigurationsalternativ för IoT Hub, klickar du på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="87f86-118">When you have chosen your IoT hub configuration options, click **Create**.</span></span>  <span data-ttu-id="87f86-119">Det kan ta några minuter för Azure toocreate din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="87f86-119">It can take a few minutes for Azure toocreate your IoT hub.</span></span> <span data-ttu-id="87f86-120">Du kan övervaka hello förlopp på hello startsidan eller i meddelandepanelen hello toocheck hello status.</span><span class="sxs-lookup"><span data-stu-id="87f86-120">toocheck hello status, you can monitor hello progress on hello Startboard or in hello Notifications panel.</span></span>
   
    ![Status för ny IoT Hub][3]
1. <span data-ttu-id="87f86-122">När hello IoT-hubben har skapats, klickar du på hello nya ikonen för din IoT-hubb i hello Azure portal tooopen hello bladet för hello ny IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="87f86-122">When hello IoT hub has been created successfully, click hello new tile for your IoT hub in hello Azure portal tooopen hello blade for hello new IoT hub.</span></span> <span data-ttu-id="87f86-123">Anteckna hello **värdnamn**, och klicka sedan på **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="87f86-123">Make a note of hello **Hostname**, and then click **Shared access policies**.</span></span>
   
    ![Ny IoT Hub-blad][4]
1. <span data-ttu-id="87f86-125">I hello **principer för delad åtkomst** bladet, klickar du på hello **iothubowner** princip, kopiera och anteckna hello IoT-hubb anslutningssträngen hello **iothubowner** bladet.</span><span class="sxs-lookup"><span data-stu-id="87f86-125">In hello **Shared access policies** blade, click hello **iothubowner** policy, and then copy and make note of hello IoT Hub connection string in hello **iothubowner** blade.</span></span> <span data-ttu-id="87f86-126">Mer information finns i [åtkomstkontroll] [ lnk-access-control] i hello ”IoT-hubb guide för utvecklare”.</span><span class="sxs-lookup"><span data-stu-id="87f86-126">For more information, see [Access control][lnk-access-control] in hello "IoT Hub developer guide."</span></span>
   
    ![Bladet Principer för delad åtkomst][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
