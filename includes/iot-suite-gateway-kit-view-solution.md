## <a name="view-the-solution-dashboard"></a><span data-ttu-id="504a5-101">Visa instrumentpanelen för lösningen</span><span class="sxs-lookup"><span data-stu-id="504a5-101">View the solution dashboard</span></span>

<span data-ttu-id="504a5-102">På instrumentpanelen för lösningen kan du hantera den distribuerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="504a5-102">The solution dashboard enables you to manage the deployed solution.</span></span> <span data-ttu-id="504a5-103">Du kan till exempel visa telemetri, Lägg till enheter och anropa metoder.</span><span class="sxs-lookup"><span data-stu-id="504a5-103">For example, you can view telemetry, add devices, and invoke methods.</span></span>

1. <span data-ttu-id="504a5-104">När etableringen har slutförts och panelen för din förkonfigurerade lösning visar statusen **Klar** klickar du på **Starta** så öppnas portalen för fjärrövervakningslösningen på en ny flik.</span><span class="sxs-lookup"><span data-stu-id="504a5-104">When the provisioning is complete and the tile for your preconfigured solution indicates **Ready**, choose **Launch** to open your remote monitoring solution portal in a new tab.</span></span>

    ![Starta den förkonfigurerade lösningen][img-launch-solution]

1. <span data-ttu-id="504a5-106">Som standard visar lösningsportalen *instrumentpanelen*.</span><span class="sxs-lookup"><span data-stu-id="504a5-106">By default, the solution portal shows the *dashboard*.</span></span> <span data-ttu-id="504a5-107">Navigera till andra delar av lösningen portalen med hjälp av menyn till vänster på sidan.</span><span class="sxs-lookup"><span data-stu-id="504a5-107">Navigate to other areas of the solution portal using the menu on the left-hand side of the page.</span></span>

    ![Instrumentpanel för den förkonfigurerade fjärrövervakningslösningen][img-menu]

## <a name="add-a-device"></a><span data-ttu-id="504a5-109">Lägg till en enhet</span><span class="sxs-lookup"><span data-stu-id="504a5-109">Add a device</span></span>

<span data-ttu-id="504a5-110">För att en enhet ska kunna ansluta till den förkonfigurerade lösningen måste den identifiera sig för IoT Hub med giltiga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="504a5-110">For a device to connect to the preconfigured solution, it must identify itself to IoT Hub using valid credentials.</span></span> <span data-ttu-id="504a5-111">Du kan hämta enhetens autentiseringsuppgifter från lösningens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="504a5-111">You can retrieve the device credentials from the solution dashboard.</span></span> <span data-ttu-id="504a5-112">Du kan inkludera enhetsautentiseringsuppgifterna i klientprogrammet senare i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="504a5-112">You include the device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="504a5-113">Om du vill lägga till en enhet till din fjärrövervakningslösning utför du följande steg i lösningens instrumentpanel:</span><span class="sxs-lookup"><span data-stu-id="504a5-113">To add a device to your remote monitoring solution, complete the following steps in the solution dashboard:</span></span>

1. <span data-ttu-id="504a5-114">Klicka på **Lägg till en enhet** i det nedre vänstra hörnet på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="504a5-114">In the lower left-hand corner of the dashboard, click **Add a device**.</span></span>

   ![Lägg till en enhet][1]

1. <span data-ttu-id="504a5-116">I panelen **Anpassad enhet** klickar du på **Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="504a5-116">In the **Custom Device** panel, click **Add new**.</span></span>

   ![Lägg till en anpassad enhet][2]

1. <span data-ttu-id="504a5-118">Välj **Låt mig ange mitt eget enhets-ID**.</span><span class="sxs-lookup"><span data-stu-id="504a5-118">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="504a5-119">Ange ett enhets-ID som **device01**, klickar du på **kontrollera ID** att verifiera att du inte redan använt namnet i din lösning och klicka sedan på **skapa** att etablera enheten.</span><span class="sxs-lookup"><span data-stu-id="504a5-119">Enter a Device ID such as **device01**, click **Check ID** to verify you haven't already used the name in your solution, and then click **Create** to provision the device.</span></span>

   ![Lägg till enhets-ID][3]

1. <span data-ttu-id="504a5-121">Notera enheten autentiseringsuppgifter (**enhets-ID**, **IoT-hubb Hostname**, och **enhetsnyckel**).</span><span class="sxs-lookup"><span data-stu-id="504a5-121">Make a note the device credentials (**Device ID**, **IoT Hub Hostname**, and **Device Key**).</span></span> <span data-ttu-id="504a5-122">Ditt klientprogram på Intel NUC måste dessa värden för att ansluta till den fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="504a5-122">Your client application on the Intel NUC needs these values to connect to the remote monitoring solution.</span></span> <span data-ttu-id="504a5-123">Klicka sedan på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="504a5-123">Then click **Done**.</span></span>

    ![Visa enhetsautentiseringsuppgifter][4]

1. <span data-ttu-id="504a5-125">Välj enheten i enhetslistan i lösningens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="504a5-125">Select your device in the device list in the solution dashboard.</span></span> <span data-ttu-id="504a5-126">Klicka sedan på **Aktivera enhet** i panelen **Enhetsinformation**.</span><span class="sxs-lookup"><span data-stu-id="504a5-126">Then, in the **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="504a5-127">Statusen för din enhet är nu **Körs**.</span><span class="sxs-lookup"><span data-stu-id="504a5-127">The status of your device is now **Running**.</span></span> <span data-ttu-id="504a5-128">Fjärrövervakningslösningen kan nu ta emot telemetri från enheten och anropa metoder på enheten.</span><span class="sxs-lookup"><span data-stu-id="504a5-128">The remote monitoring solution can now receive telemetry from your device and invoke methods on the device.</span></span>

[img-launch-solution]: media/iot-suite-gateway-kit-view-solution/launch.png
[img-menu]: media/iot-suite-gateway-kit-view-solution/menu.png
[1]: media/iot-suite-gateway-kit-view-solution/suite0.png
[2]: media/iot-suite-gateway-kit-view-solution/suite1.png
[3]: media/iot-suite-gateway-kit-view-solution/suite2.png
[4]: media/iot-suite-gateway-kit-view-solution/suite3.png