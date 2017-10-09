## <a name="view-hello-telemetry"></a><span data-ttu-id="7c06c-101">Visa hello telemetri</span><span class="sxs-lookup"><span data-stu-id="7c06c-101">View hello telemetry</span></span>

<span data-ttu-id="7c06c-102">hello hallon Pi nu skicka telemetri toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="7c06c-102">hello Raspberry Pi is now sending telemetry toohello remote monitoring solution.</span></span> <span data-ttu-id="7c06c-103">Du kan visa hello telemetri på instrumentpanelen för hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="7c06c-103">You can view hello telemetry on hello solution dashboard.</span></span> <span data-ttu-id="7c06c-104">Du kan också skicka meddelanden tooyour hallon Pi hello lösning instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="7c06c-104">You can also send messages tooyour Raspberry Pi from hello solution dashboard.</span></span>

- <span data-ttu-id="7c06c-105">Navigera toohello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7c06c-105">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="7c06c-106">Välj din enhet i hello **enhet tooView** listrutan.</span><span class="sxs-lookup"><span data-stu-id="7c06c-106">Select your device in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="7c06c-107">hello telemetri från hello hallon Pi visar hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7c06c-107">hello telemetry from hello Raspberry Pi displays on hello dashboard.</span></span>

![Visa telemetri från hello hallon Pi][img-telemetry-display]

## <a name="act-on-hello-device"></a><span data-ttu-id="7c06c-109">Fungerar på hello-enhet</span><span class="sxs-lookup"><span data-stu-id="7c06c-109">Act on hello device</span></span>

<span data-ttu-id="7c06c-110">Du kan anropa metoder på din hallon Pi hello lösning instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="7c06c-110">From hello solution dashboard, you can invoke methods on your Raspberry Pi.</span></span> <span data-ttu-id="7c06c-111">När hello hallon Pi ansluter toohello remote övervakningslösning, skickar information om hello-metoder stöder.</span><span class="sxs-lookup"><span data-stu-id="7c06c-111">When hello Raspberry Pi connects toohello remote monitoring solution, it sends information about hello methods it supports.</span></span>

- <span data-ttu-id="7c06c-112">I hello lösning instrumentpanelen, klickar du på **enheter** toovisit hello **enheter** sidan.</span><span class="sxs-lookup"><span data-stu-id="7c06c-112">In hello solution dashboard, click **Devices** toovisit hello **Devices** page.</span></span> <span data-ttu-id="7c06c-113">Välj din hallon Pi i hello **enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="7c06c-113">Select your Raspberry Pi in hello **Device List**.</span></span> <span data-ttu-id="7c06c-114">Välj **metoder**:</span><span class="sxs-lookup"><span data-stu-id="7c06c-114">Then choose **Methods**:</span></span>

    ![Lista över enheter i instrumentpanelen][img-list-devices]

- <span data-ttu-id="7c06c-116">På hello **anropa metoden** väljer **LightBlink** i hello **metoden** listrutan.</span><span class="sxs-lookup"><span data-stu-id="7c06c-116">On hello **Invoke Method** page, choose **LightBlink** in hello **Method** dropdown.</span></span>

- <span data-ttu-id="7c06c-117">Välj **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="7c06c-117">Choose **InvokeMethod**.</span></span> <span data-ttu-id="7c06c-118">hello simulator skriver ut ett meddelande i hello-konsolen på hello hallon Pi.</span><span class="sxs-lookup"><span data-stu-id="7c06c-118">hello simulator prints a message in hello console on hello Raspberry Pi.</span></span> <span data-ttu-id="7c06c-119">hello-appen på hello hallon Pi skickar en bekräftelse tillbaka toohello lösning instrumentpanel:</span><span class="sxs-lookup"><span data-stu-id="7c06c-119">hello app on hello Raspberry Pi sends an acknowledgment back toohello solution dashboard:</span></span>

    ![Visa historiken för metoden][img-method-history]

- <span data-ttu-id="7c06c-121">Du kan växla hello Indikator och inaktivera med hello **ChangeLightStatus** metod med en **LightStatusValue** ställa in också**1** för på eller **0** för av.</span><span class="sxs-lookup"><span data-stu-id="7c06c-121">You can switch hello LED on and off using hello **ChangeLightStatus** method with a **LightStatusValue** set too**1** for on or **0** for off.</span></span>

> [!WARNING]
> <span data-ttu-id="7c06c-122">Om du lämnar hello remote övervakningslösning som körs i ditt Azure-konto, debiteras du för hello gång den körs.</span><span class="sxs-lookup"><span data-stu-id="7c06c-122">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="7c06c-123">Läs mer om att minskas när hello fjärråtkomst övervakning lösningen körs [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="7c06c-123">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="7c06c-124">Ta bort hello förkonfigurerade lösningen från ditt Azure-konto när du är klar.</span><span class="sxs-lookup"><span data-stu-id="7c06c-124">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md