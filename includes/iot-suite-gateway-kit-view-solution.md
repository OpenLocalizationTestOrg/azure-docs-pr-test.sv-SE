## <a name="view-hello-solution-dashboard"></a><span data-ttu-id="91598-101">Visa hello lösning instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="91598-101">View hello solution dashboard</span></span>

<span data-ttu-id="91598-102">hello lösning instrumentpanelen kan du toomanage hello distribueras lösning.</span><span class="sxs-lookup"><span data-stu-id="91598-102">hello solution dashboard enables you toomanage hello deployed solution.</span></span> <span data-ttu-id="91598-103">Du kan till exempel visa telemetri, Lägg till enheter och anropa metoder.</span><span class="sxs-lookup"><span data-stu-id="91598-103">For example, you can view telemetry, add devices, and invoke methods.</span></span>

1. <span data-ttu-id="91598-104">När hello etableringen har slutförts och hello panelen för din förkonfigurerade lösning anger **klar**, Välj **starta** tooopen fjärråtkomst övervakning lösning portalen på en ny flik.</span><span class="sxs-lookup"><span data-stu-id="91598-104">When hello provisioning is complete and hello tile for your preconfigured solution indicates **Ready**, choose **Launch** tooopen your remote monitoring solution portal in a new tab.</span></span>

    ![Starta hello förkonfigurerade lösningen][img-launch-solution]

1. <span data-ttu-id="91598-106">Som standard visar hello lösning portal hello *instrumentpanelen*.</span><span class="sxs-lookup"><span data-stu-id="91598-106">By default, hello solution portal shows hello *dashboard*.</span></span> <span data-ttu-id="91598-107">Navigera tooother områden i hello lösning portal med hello menyn hello vänster hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="91598-107">Navigate tooother areas of hello solution portal using hello menu on hello left-hand side of hello page.</span></span>

    ![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-menu]

## <a name="add-a-device"></a><span data-ttu-id="91598-109">Lägg till en enhet</span><span class="sxs-lookup"><span data-stu-id="91598-109">Add a device</span></span>

<span data-ttu-id="91598-110">För en enhet tooconnect toohello förkonfigurerade lösning, den måste identifiera sig själv tooIoT hubb med giltiga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="91598-110">For a device tooconnect toohello preconfigured solution, it must identify itself tooIoT Hub using valid credentials.</span></span> <span data-ttu-id="91598-111">Du kan hämta hello enheten autentiseringsuppgifter från hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="91598-111">You can retrieve hello device credentials from hello solution dashboard.</span></span> <span data-ttu-id="91598-112">Du inkludera hello enheten autentiseringsuppgifter i ditt klientprogram senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="91598-112">You include hello device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="91598-113">tooadd en enhet tooyour remote övervakningslösning fullständig hello följa stegen i hello lösning instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="91598-113">tooadd a device tooyour remote monitoring solution, complete hello following steps in hello solution dashboard:</span></span>

1. <span data-ttu-id="91598-114">I hello nedre vänstra hörnet av hello instrumentpanelen, klickar du på **lägger till en enhet**.</span><span class="sxs-lookup"><span data-stu-id="91598-114">In hello lower left-hand corner of hello dashboard, click **Add a device**.</span></span>

   ![Lägg till en enhet][1]

1. <span data-ttu-id="91598-116">I hello **anpassad enhet** klickar du på **Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="91598-116">In hello **Custom Device** panel, click **Add new**.</span></span>

   ![Lägg till en anpassad enhet][2]

1. <span data-ttu-id="91598-118">Välj **Låt mig ange mitt eget enhets-ID**.</span><span class="sxs-lookup"><span data-stu-id="91598-118">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="91598-119">Ange ett enhets-ID som **device01**, klickar du på **kontrollera ID** tooverify du inte redan använt hello namn i din lösning och klicka sedan på **skapa** tooprovision hello enhet.</span><span class="sxs-lookup"><span data-stu-id="91598-119">Enter a Device ID such as **device01**, click **Check ID** tooverify you haven't already used hello name in your solution, and then click **Create** tooprovision hello device.</span></span>

   ![Lägg till enhets-ID][3]

1. <span data-ttu-id="91598-121">Göra en anteckning hello enhet autentiseringsuppgifter (**enhets-ID**, **IoT-hubb Hostname**, och **enhetsnyckel**).</span><span class="sxs-lookup"><span data-stu-id="91598-121">Make a note hello device credentials (**Device ID**, **IoT Hub Hostname**, and **Device Key**).</span></span> <span data-ttu-id="91598-122">Ditt klientprogram på hello Intel NUC måste dessa värden tooconnect toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="91598-122">Your client application on hello Intel NUC needs these values tooconnect toohello remote monitoring solution.</span></span> <span data-ttu-id="91598-123">Klicka sedan på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="91598-123">Then click **Done**.</span></span>

    ![Visa enhetsautentiseringsuppgifter][4]

1. <span data-ttu-id="91598-125">Välj din enhet i listan över enheter hello hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="91598-125">Select your device in hello device list in hello solution dashboard.</span></span> <span data-ttu-id="91598-126">Sedan hello **enhetsinformation** klickar du på **Aktivera enhet**.</span><span class="sxs-lookup"><span data-stu-id="91598-126">Then, in hello **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="91598-127">hello statusen för din enhet är nu **kör**.</span><span class="sxs-lookup"><span data-stu-id="91598-127">hello status of your device is now **Running**.</span></span> <span data-ttu-id="91598-128">hello remote övervakningslösning kan nu ta emot telemetri från enheten och anropa metoder i hello enhet.</span><span class="sxs-lookup"><span data-stu-id="91598-128">hello remote monitoring solution can now receive telemetry from your device and invoke methods on hello device.</span></span>

[img-launch-solution]: media/iot-suite-gateway-kit-view-solution/launch.png
[img-menu]: media/iot-suite-gateway-kit-view-solution/menu.png
[1]: media/iot-suite-gateway-kit-view-solution/suite0.png
[2]: media/iot-suite-gateway-kit-view-solution/suite1.png
[3]: media/iot-suite-gateway-kit-view-solution/suite2.png
[4]: media/iot-suite-gateway-kit-view-solution/suite3.png