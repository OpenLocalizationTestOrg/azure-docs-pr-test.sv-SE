## <a name="view-the-telemetry"></a><span data-ttu-id="1270d-101">Visa telemetrin</span><span class="sxs-lookup"><span data-stu-id="1270d-101">View the telemetry</span></span>

<span data-ttu-id="1270d-102">Hallon Pi nu skicka telemetri till fjärranslutna övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="1270d-102">The Raspberry Pi is now sending telemetry to the remote monitoring solution.</span></span> <span data-ttu-id="1270d-103">Du kan visa telemetrin på instrumentpanelen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="1270d-103">You can view the telemetry on the solution dashboard.</span></span> <span data-ttu-id="1270d-104">Du kan också skicka meddelanden till din hallon Pi från instrumentpanelen lösning.</span><span class="sxs-lookup"><span data-stu-id="1270d-104">You can also send messages to your Raspberry Pi from the solution dashboard.</span></span>

- <span data-ttu-id="1270d-105">Gå till instrumentpanelen lösning.</span><span class="sxs-lookup"><span data-stu-id="1270d-105">Navigate to the solution dashboard.</span></span>
- <span data-ttu-id="1270d-106">Välj din enhet i den **enhet för att visa** listrutan.</span><span class="sxs-lookup"><span data-stu-id="1270d-106">Select your device in the **Device to View** dropdown.</span></span>
- <span data-ttu-id="1270d-107">Telemetri från hallon Pi visas på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="1270d-107">The telemetry from the Raspberry Pi displays on the dashboard.</span></span>

![Visa telemetri från hallon Pi][img-telemetry-display]

## <a name="act-on-the-device"></a><span data-ttu-id="1270d-109">Fungerar på enheten</span><span class="sxs-lookup"><span data-stu-id="1270d-109">Act on the device</span></span>

<span data-ttu-id="1270d-110">Du kan anropa metoder på din hallon Pi från instrumentpanelen lösning.</span><span class="sxs-lookup"><span data-stu-id="1270d-110">From the solution dashboard, you can invoke methods on your Raspberry Pi.</span></span> <span data-ttu-id="1270d-111">När hallon Pi ansluter till den fjärranslutna övervakningslösning, skickar information om hur den stöder.</span><span class="sxs-lookup"><span data-stu-id="1270d-111">When the Raspberry Pi connects to the remote monitoring solution, it sends information about the methods it supports.</span></span>

- <span data-ttu-id="1270d-112">I lösningen instrumentpanelen, klickar du på **enheter** besöka den **enheter** sidan.</span><span class="sxs-lookup"><span data-stu-id="1270d-112">In the solution dashboard, click **Devices** to visit the **Devices** page.</span></span> <span data-ttu-id="1270d-113">Välj din Raspberry Pi i den **enhetslistan**.</span><span class="sxs-lookup"><span data-stu-id="1270d-113">Select your Raspberry Pi in the **Device List**.</span></span> <span data-ttu-id="1270d-114">Välj **metoder**:</span><span class="sxs-lookup"><span data-stu-id="1270d-114">Then choose **Methods**:</span></span>

    ![Lista över enheter i instrumentpanelen][img-list-devices]

- <span data-ttu-id="1270d-116">På den **anropa metoden** väljer **LightBlink** i den **metoden** listrutan.</span><span class="sxs-lookup"><span data-stu-id="1270d-116">On the **Invoke Method** page, choose **LightBlink** in the **Method** dropdown.</span></span>

- <span data-ttu-id="1270d-117">Välj **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="1270d-117">Choose **InvokeMethod**.</span></span> <span data-ttu-id="1270d-118">Indikatorn är anslutna till hallon Pi gånger flera gånger.</span><span class="sxs-lookup"><span data-stu-id="1270d-118">The LED connected to the Raspberry Pi flashes several times.</span></span> <span data-ttu-id="1270d-119">Appen på hallon Pi skickar en bekräftelse tillbaka till instrumentpanelen lösningen:</span><span class="sxs-lookup"><span data-stu-id="1270d-119">The app on the Raspberry Pi sends an acknowledgment back to the solution dashboard:</span></span>

    ![Visa historiken för metoden][img-method-history]

- <span data-ttu-id="1270d-121">Du kan växla Indikatorn och inaktivera med hjälp av den **ChangeLightStatus** metod med en **LightStatusValue** inställd på **1** för på eller **0** för av.</span><span class="sxs-lookup"><span data-stu-id="1270d-121">You can switch the LED on and off using the **ChangeLightStatus** method with a **LightStatusValue** set to **1** for on or **0** for off.</span></span>

> [!WARNING]
> <span data-ttu-id="1270d-122">Om du lämnar den fjärranslutna övervakningslösning som körs i ditt Azure-konto, debiteras du för den tid som den körs.</span><span class="sxs-lookup"><span data-stu-id="1270d-122">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="1270d-123">Mer information om hur du minskar användning när den fjärranslutna övervakningslösning körs finns [konfigurerar Azure IoT Suite förkonfigurerade lösningar för demonstration][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="1270d-123">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="1270d-124">Ta bort den förkonfigurerade lösningen från ditt Azure-konto när du är klar.</span><span class="sxs-lookup"><span data-stu-id="1270d-124">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md