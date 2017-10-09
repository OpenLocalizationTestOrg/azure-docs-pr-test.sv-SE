## <a name="view-device-telemetry-in-hello-dashboard"></a><span data-ttu-id="c8098-101">Visa enhetstelemetrin hello instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="c8098-101">View device telemetry in hello dashboard</span></span>
<span data-ttu-id="c8098-102">hello instrumentpanelen i hello fjärråtkomst övervakning lösning aktiverar du tooview hello telemetri dina enheter skicka tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="c8098-102">hello dashboard in hello remote monitoring solution enables you tooview hello telemetry your devices send tooIoT Hub.</span></span>

1. <span data-ttu-id="c8098-103">I webbläsaren returnerade toohello remote lösning instrumentpanelen för övervakning, klickar du på **enheter** i hello vänstra panelen toonavigate toohello **enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="c8098-103">In your browser, return toohello remote monitoring solution dashboard, click **Devices** in hello left-hand panel toonavigate toohello **Devices list**.</span></span>
2. <span data-ttu-id="c8098-104">I hello **enhetslistan**, bör du se att hello statusen för din enhet är **kör**.</span><span class="sxs-lookup"><span data-stu-id="c8098-104">In hello **Devices list**, you should see that hello status of your device is **Running**.</span></span> <span data-ttu-id="c8098-105">Om inte, klickar du på **Aktivera enhet** i hello **enhetsinformation** panelen.</span><span class="sxs-lookup"><span data-stu-id="c8098-105">If not, click **Enable Device** in hello **Device Details** panel.</span></span>
   
    ![Visa enhetsstatus][18]
3. <span data-ttu-id="c8098-107">Klicka på **instrumentpanelen** tooreturn toohello instrumentpanelen, Välj din enhet i hello **enhet tooView** nedrullningsbara tooview dess telemetri.</span><span class="sxs-lookup"><span data-stu-id="c8098-107">Click **Dashboard** tooreturn toohello dashboard, select your device in hello **Device tooView** drop-down tooview its telemetry.</span></span> <span data-ttu-id="c8098-108">hello telemetri från hello exempelprogram är 50 enheter för inre temperatur, 55 enheter för externa temperatur och 50 enheter för fuktighet.</span><span class="sxs-lookup"><span data-stu-id="c8098-108">hello telemetry from hello sample application is 50 units for internal temperature, 55 units for external temperature, and 50 units for humidity.</span></span>
   
    ![Visa enhetstelemetri][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a><span data-ttu-id="c8098-110">Anropa en metod på enheten</span><span class="sxs-lookup"><span data-stu-id="c8098-110">Invoke a method on your device</span></span>
<span data-ttu-id="c8098-111">hello instrumentpanelen i hello remote övervakningslösning kan tooinvoke metoder på dina enheter via IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c8098-111">hello dashboard in hello remote monitoring solution enables you tooinvoke methods on your devices through IoT Hub.</span></span> <span data-ttu-id="c8098-112">Du kan till exempel anropa en metod toosimulate startas om en enhet i hello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="c8098-112">For example, in hello remote monitoring solution you can invoke a method toosimulate rebooting a device.</span></span>

1. <span data-ttu-id="c8098-113">Hello remote lösning instrumentpanelen för övervakning, klicka på **enheter** i hello vänstra panelen toonavigate toohello **enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="c8098-113">In hello remote monitoring solution dashboard, click **Devices** in hello left-hand panel toonavigate toohello **Devices list**.</span></span>
2. <span data-ttu-id="c8098-114">Klicka på **enhets-ID** för din enhet i hello **enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="c8098-114">Click **Device ID** for your device in hello **Devices list**.</span></span>
3. <span data-ttu-id="c8098-115">I hello **enhetsinformation** klickar du på **metoder**.</span><span class="sxs-lookup"><span data-stu-id="c8098-115">In hello **Device details** panel, click **Methods**.</span></span>
   
    ![Enhetsmetoder][13]
4. <span data-ttu-id="c8098-117">I hello **metoden** listrutan, Välj **InitiateFirmwareUpdate**, och klicka sedan på **FWPACKAGEURI** ange en URL för dummy.</span><span class="sxs-lookup"><span data-stu-id="c8098-117">In hello **Method** drop-down, select **InitiateFirmwareUpdate**, and then in **FWPACKAGEURI** enter a dummy URL.</span></span> <span data-ttu-id="c8098-118">Klicka på **anropa metoden** toocall hello-metoden på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="c8098-118">Click **Invoke Method** toocall hello method on hello device.</span></span>
   
    ![Anropa en enhetsmetod][14]
   

5. <span data-ttu-id="c8098-120">Du ser ett meddelande i hello-konsolen körs din kod för enheten när hello enheten hanterar hello-metoden.</span><span class="sxs-lookup"><span data-stu-id="c8098-120">You see a message in hello console running your device code when hello device handles hello method.</span></span> <span data-ttu-id="c8098-121">hello resultaten av hello metoden läggs toohello historik i hello lösning portal:</span><span class="sxs-lookup"><span data-stu-id="c8098-121">hello results of hello method are added toohello history in hello solution portal:</span></span>

    ![Visa metodhistorik][img-method-history]

## <a name="next-steps"></a><span data-ttu-id="c8098-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c8098-123">Next steps</span></span>
<span data-ttu-id="c8098-124">hello artikel [anpassa förkonfigurerade lösningar] [ lnk-customize] beskrivs några sätt som du kan utöka det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="c8098-124">hello article [Customizing preconfigured solutions][lnk-customize] describes some ways you can extend this sample.</span></span> <span data-ttu-id="c8098-125">Möjliga tillägg kan vara att använda riktiga sensorer och att implementera ytterligare kommandon.</span><span class="sxs-lookup"><span data-stu-id="c8098-125">Possible extensions include using real sensors and implementing additional commands.</span></span>

<span data-ttu-id="c8098-126">Du kan lära dig mer om hello [behörigheter på hello azureiotsuite.com platsen][lnk-permissions].</span><span class="sxs-lookup"><span data-stu-id="c8098-126">You can learn more about hello [permissions on hello azureiotsuite.com site][lnk-permissions].</span></span>

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
