## <a name="view-the-solution-dashboard"></a><span data-ttu-id="fdd8e-101">Visa instrumentpanelen för lösningen</span><span class="sxs-lookup"><span data-stu-id="fdd8e-101">View the solution dashboard</span></span>

<span data-ttu-id="fdd8e-102">På instrumentpanelen för lösningen kan du hantera den distribuerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-102">The solution dashboard enables you to manage the deployed solution.</span></span> <span data-ttu-id="fdd8e-103">Du kan till exempel visa telemetri, Lägg till enheter och anropa metoder.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-103">For example, you can view telemetry, add devices, and invoke methods.</span></span>

1. <span data-ttu-id="fdd8e-104">När etableringen har slutförts och panelen för din förkonfigurerade lösning visar statusen **Klar** klickar du på **Starta** så öppnas portalen för fjärrövervakningslösningen på en ny flik.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-104">When the provisioning is complete and the tile for your preconfigured solution indicates **Ready**, choose **Launch** to open your remote monitoring solution portal in a new tab.</span></span>

    ![Starta den förkonfigurerade lösningen][img-launch-solution]

1. <span data-ttu-id="fdd8e-106">Som standard visar lösningsportalen *instrumentpanelen*.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-106">By default, the solution portal shows the *dashboard*.</span></span> <span data-ttu-id="fdd8e-107">Du kan navigera till andra delar av lösningsportalen via menyn till vänster på sidan.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-107">You can navigate to other areas of the solution portal using the menu on the left-hand side of the page.</span></span>

    ![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-menu]

## <a name="add-a-device"></a><span data-ttu-id="fdd8e-109">Lägg till en enhet</span><span class="sxs-lookup"><span data-stu-id="fdd8e-109">Add a device</span></span>

<span data-ttu-id="fdd8e-110">För att en enhet ska kunna ansluta till den förkonfigurerade lösningen måste den identifiera sig för IoT Hub med giltiga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-110">For a device to connect to the preconfigured solution, it must identify itself to IoT Hub using valid credentials.</span></span> <span data-ttu-id="fdd8e-111">Du kan hämta enhetens autentiseringsuppgifter från lösningens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-111">You can retrieve the device credentials from the solution dashboard.</span></span> <span data-ttu-id="fdd8e-112">Du kan inkludera enhetsautentiseringsuppgifterna i klientprogrammet senare i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-112">You include the device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="fdd8e-113">Om du inte redan gjort det, lägger du till en anpassad enhet din fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-113">If you haven't already done so, add a custom device to your remote monitoring solution.</span></span> <span data-ttu-id="fdd8e-114">Utför följande steg i instrumentpanelen för lösningen:</span><span class="sxs-lookup"><span data-stu-id="fdd8e-114">Complete the following steps in the solution dashboard:</span></span>

1. <span data-ttu-id="fdd8e-115">Klicka på **Lägg till en enhet** i det nedre vänstra hörnet på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-115">In the lower left-hand corner of the dashboard, click **Add a device**.</span></span>

   ![Lägg till en enhet][1]

1. <span data-ttu-id="fdd8e-117">I panelen **Anpassad enhet** klickar du på **Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-117">In the **Custom Device** panel, click **Add new**.</span></span>

   ![Lägg till en anpassad enhet][2]

1. <span data-ttu-id="fdd8e-119">Välj **Låt mig ange mitt eget enhets-ID**.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-119">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="fdd8e-120">Ange ett enhets-ID som **rasppi**, klickar du på **kontrollera ID** att verifiera att du inte redan använt namnet i din lösning och klicka sedan på **skapa** att etablera enheten.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-120">Enter a Device ID such as **rasppi**, click **Check ID** to verify you haven't already used the name in your solution, and then click **Create** to provision the device.</span></span>

   ![Lägg till enhets-ID][3]

1. <span data-ttu-id="fdd8e-122">Notera enheten autentiseringsuppgifter (**enhets-ID**, **IoT-hubb Hostname**, och **enhetsnyckel**).</span><span class="sxs-lookup"><span data-stu-id="fdd8e-122">Make a note the device credentials (**Device ID**, **IoT Hub Hostname**, and **Device Key**).</span></span> <span data-ttu-id="fdd8e-123">Ditt klientprogram på hallon Pi måste dessa värden för att ansluta till den fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-123">Your client application on the Raspberry Pi needs these values to connect to the remote monitoring solution.</span></span> <span data-ttu-id="fdd8e-124">Klicka sedan på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-124">Then click **Done**.</span></span>

    ![Visa enhetsautentiseringsuppgifter][4]

1. <span data-ttu-id="fdd8e-126">Välj enheten i enhetslistan i lösningens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-126">Select your device in the device list in the solution dashboard.</span></span> <span data-ttu-id="fdd8e-127">Klicka sedan på **Aktivera enhet** i panelen **Enhetsinformation**.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-127">Then, in the **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="fdd8e-128">Statusen för din enhet är nu **Körs**.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-128">The status of your device is now **Running**.</span></span> <span data-ttu-id="fdd8e-129">Fjärrövervakningslösningen kan nu ta emot telemetri från enheten och anropa metoder på enheten.</span><span class="sxs-lookup"><span data-stu-id="fdd8e-129">The remote monitoring solution can now receive telemetry from your device and invoke methods on the device.</span></span>

[img-launch-solution]: media/iot-suite-raspberry-pi-kit-view-solution/launch.png
[img-menu]: media/iot-suite-raspberry-pi-kit-view-solution/menu.png
[1]: media/iot-suite-raspberry-pi-kit-view-solution/suite0.png
[2]: media/iot-suite-raspberry-pi-kit-view-solution/suite1.png
[3]: media/iot-suite-raspberry-pi-kit-view-solution/suite2.png
[4]: media/iot-suite-raspberry-pi-kit-view-solution/suite3.png