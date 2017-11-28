## <a name="view-device-telemetry-in-the-dashboard"></a><span data-ttu-id="8389e-101">Visa enhetstelemetri i instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="8389e-101">View device telemetry in the dashboard</span></span>
<span data-ttu-id="8389e-102">Instrumentpanelen i fjärrövervakningslösningen visar den telemetri som enheterna skickar till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8389e-102">The dashboard in the remote monitoring solution enables you to view the telemetry your devices send to IoT Hub.</span></span>

1. <span data-ttu-id="8389e-103">Gå tillbaka till fjärrövervakningslösningens instrumentpanel i webbläsaren och klicka på **Enheter** i den vänstra panelen för att navigera till **Enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="8389e-103">In your browser, return to the remote monitoring solution dashboard, click **Devices** in the left-hand panel to navigate to the **Devices list**.</span></span>
2. <span data-ttu-id="8389e-104">I **enhetslistan** bör du se att statusen för din enhet är **Körs**.</span><span class="sxs-lookup"><span data-stu-id="8389e-104">In the **Devices list**, you should see that the status of your device is **Running**.</span></span> <span data-ttu-id="8389e-105">Om den inte körs klickar du på **Aktivera enhet** i panelen **Enhetsinformation**.</span><span class="sxs-lookup"><span data-stu-id="8389e-105">If not, click **Enable Device** in the **Device Details** panel.</span></span>
   
    ![Visa enhetsstatus][18]
3. <span data-ttu-id="8389e-107">Klicka på **Instrumentpanel** för att gå tillbaka till instrumentpanelen och välj din enhet i listrutan **Enhet att visa** för att se dess telemetri.</span><span class="sxs-lookup"><span data-stu-id="8389e-107">Click **Dashboard** to return to the dashboard, select your device in the **Device to View** drop-down to view its telemetry.</span></span> <span data-ttu-id="8389e-108">Telemetrin från exempelprogrammet är 50 enheter för intern temperatur, 55 enheter för extern temperatur och 50 enheter för fuktighet.</span><span class="sxs-lookup"><span data-stu-id="8389e-108">The telemetry from the sample application is 50 units for internal temperature, 55 units for external temperature, and 50 units for humidity.</span></span>
   
    ![Visa enhetstelemetri][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a><span data-ttu-id="8389e-110">Anropa en metod på enheten</span><span class="sxs-lookup"><span data-stu-id="8389e-110">Invoke a method on your device</span></span>
<span data-ttu-id="8389e-111">Instrumentpanelen i fjärrövervakningslösningen låter dig anropa metoder på enheterna genom IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8389e-111">The dashboard in the remote monitoring solution enables you to invoke methods on your devices through IoT Hub.</span></span> <span data-ttu-id="8389e-112">Du kan till exempel anropa en metod för att simulera en omstart av en enhet i fjärrövervakningslösningen.</span><span class="sxs-lookup"><span data-stu-id="8389e-112">For example, in the remote monitoring solution you can invoke a method to simulate rebooting a device.</span></span>

1. <span data-ttu-id="8389e-113">I fjärrövervakningslösningens instrumentpanel klickar du på **Enheter** i den vänstra panelen för att navigera till **Enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="8389e-113">In the remote monitoring solution dashboard, click **Devices** in the left-hand panel to navigate to the **Devices list**.</span></span>
2. <span data-ttu-id="8389e-114">Klicka på din enhets **Enhets-ID** i **enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="8389e-114">Click **Device ID** for your device in the **Devices list**.</span></span>
3. <span data-ttu-id="8389e-115">I panelen **Enhetsinformation** klickar du på **Metoder**.</span><span class="sxs-lookup"><span data-stu-id="8389e-115">In the **Device details** panel, click **Methods**.</span></span>
   
    ![Enhetsmetoder][13]
4. <span data-ttu-id="8389e-117">I listrutan **Metod**väljer du **InitiateFirmwareUpdate** och sedan anger du en dummy-URL i **FWPACKAGEURI**.</span><span class="sxs-lookup"><span data-stu-id="8389e-117">In the **Method** drop-down, select **InitiateFirmwareUpdate**, and then in **FWPACKAGEURI** enter a dummy URL.</span></span> <span data-ttu-id="8389e-118">Klicka på **Anropa metod** för att anropa metoden på enheten.</span><span class="sxs-lookup"><span data-stu-id="8389e-118">Click **Invoke Method** to call the method on the device.</span></span>
   
    ![Anropa en enhetsmetod][14]
   

5. <span data-ttu-id="8389e-120">Du ser ett meddelande i konsolen som kör enhetskoden när enheten hanterar metoden.</span><span class="sxs-lookup"><span data-stu-id="8389e-120">You see a message in the console running your device code when the device handles the method.</span></span> <span data-ttu-id="8389e-121">Resultaten av metoden läggs till i historiken i lösningsportalen:</span><span class="sxs-lookup"><span data-stu-id="8389e-121">The results of the method are added to the history in the solution portal:</span></span>

    ![Visa metodhistorik][img-method-history]

## <a name="next-steps"></a><span data-ttu-id="8389e-123">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8389e-123">Next steps</span></span>
<span data-ttu-id="8389e-124">Artikeln [Anpassning av förkonfigurerade lösningar][lnk-customize] beskriver några sätt du kan utöka det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="8389e-124">The article [Customizing preconfigured solutions][lnk-customize] describes some ways you can extend this sample.</span></span> <span data-ttu-id="8389e-125">Möjliga tillägg kan vara att använda riktiga sensorer och att implementera ytterligare kommandon.</span><span class="sxs-lookup"><span data-stu-id="8389e-125">Possible extensions include using real sensors and implementing additional commands.</span></span>

<span data-ttu-id="8389e-126">Du kan lära dig mer om [behörigheter på webbplatsen azureiotsuite.com][lnk-permissions].</span><span class="sxs-lookup"><span data-stu-id="8389e-126">You can learn more about the [permissions on the azureiotsuite.com site][lnk-permissions].</span></span>

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
