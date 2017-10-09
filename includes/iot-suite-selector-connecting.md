> [!div class="op_single_selector"]
> * [<span data-ttu-id="3d726-101">C i Windows</span><span class="sxs-lookup"><span data-stu-id="3d726-101">C on Windows</span></span>](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [<span data-ttu-id="3d726-102">C i Linux</span><span class="sxs-lookup"><span data-stu-id="3d726-102">C on Linux</span></span>](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [<span data-ttu-id="3d726-103">Node.js</span><span class="sxs-lookup"><span data-stu-id="3d726-103">Node.js</span></span>](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a><span data-ttu-id="3d726-104">Scenarioöversikt</span><span class="sxs-lookup"><span data-stu-id="3d726-104">Scenario overview</span></span>
<span data-ttu-id="3d726-105">I det här scenariot skapar du en enhet som skickar hello följande telemetri toohello fjärrövervaknings [förkonfigurerade lösningen][lnk-what-are-preconfig-solutions]:</span><span class="sxs-lookup"><span data-stu-id="3d726-105">In this scenario, you create a device that sends hello following telemetry toohello remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="3d726-106">Extern temperatur</span><span class="sxs-lookup"><span data-stu-id="3d726-106">External temperature</span></span>
* <span data-ttu-id="3d726-107">Intern temperatur</span><span class="sxs-lookup"><span data-stu-id="3d726-107">Internal temperature</span></span>
* <span data-ttu-id="3d726-108">Fuktighet</span><span class="sxs-lookup"><span data-stu-id="3d726-108">Humidity</span></span>

<span data-ttu-id="3d726-109">För enkelhetens skull hello koden på hello enhet genererar exempelvärden, men vi rekommenderar att du tooextend hello exemplet genom att ansluta verkliga sensorer tooyour enheten och skicka verkliga telemetri.</span><span class="sxs-lookup"><span data-stu-id="3d726-109">For simplicity, hello code on hello device generates sample values, but we encourage you tooextend hello sample by connecting real sensors tooyour device and sending real telemetry.</span></span>

<span data-ttu-id="3d726-110">hello-enheten är också kan toorespond toomethods anropas från hello lösning instrumentpanelen och önskade egenskapsvärden i hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3d726-110">hello device is also able toorespond toomethods invoked from hello solution dashboard and desired property values set in hello solution dashboard.</span></span>

<span data-ttu-id="3d726-111">toocomplete den här självstudiekursen kommer du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="3d726-111">toocomplete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="3d726-112">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="3d726-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3d726-113">Mer information finns i [kostnadsfri utvärderingsversion av Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="3d726-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="3d726-114">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="3d726-114">Before you start</span></span>
<span data-ttu-id="3d726-115">Innan du kan skriva kod för enheten måste du etablera din förkonfigurerade lösning för fjärrövervakning och etablera en ny anpassad enhet i lösningen.</span><span class="sxs-lookup"><span data-stu-id="3d726-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="3d726-116">Etablera din förkonfigurerade lösning för fjärrövervakning</span><span class="sxs-lookup"><span data-stu-id="3d726-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="3d726-117">hello-enhet som du skapar i den här självstudiekursen skickar data tooan instans av hello [fjärrövervaknings] [ lnk-remote-monitoring] förkonfigurerade lösningen.</span><span class="sxs-lookup"><span data-stu-id="3d726-117">hello device you create in this tutorial sends data tooan instance of hello [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="3d726-118">Om du inte redan har etablerats hello fjärråtkomst övervakning förkonfigurerade lösning i ditt Azure-konto, använder du hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3d726-118">If you haven't already provisioned hello remote monitoring preconfigured solution in your Azure account, use hello following steps:</span></span>

1. <span data-ttu-id="3d726-119">På hello <https://www.azureiotsuite.com/> klickar du på  **+**  toocreate en lösning.</span><span class="sxs-lookup"><span data-stu-id="3d726-119">On hello <https://www.azureiotsuite.com/> page, click **+** toocreate a solution.</span></span>
2. <span data-ttu-id="3d726-120">Klicka på **Välj** på hello **fjärrövervaknings** panelen toocreate din lösning.</span><span class="sxs-lookup"><span data-stu-id="3d726-120">Click **Select** on hello **Remote monitoring** panel toocreate your solution.</span></span>
3. <span data-ttu-id="3d726-121">På hello **skapa Remote övervakningslösning** anger en **lösningsnamn** du själv väljer, Välj hello **Region** toodeploy till, och markera hello Azure prenumerationen toowant toouse.</span><span class="sxs-lookup"><span data-stu-id="3d726-121">On hello **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select hello **Region** you want toodeploy to, and select hello Azure subscription toowant toouse.</span></span> <span data-ttu-id="3d726-122">Klicka på **Skapa lösning**.</span><span class="sxs-lookup"><span data-stu-id="3d726-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="3d726-123">Vänta tills hello etableringsprocessen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3d726-123">Wait until hello provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="3d726-124">hello förkonfigurerade lösningar använder fakturerbar Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="3d726-124">hello preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="3d726-125">Glöm inte tooremove hello förkonfigurerade lösningen från prenumerationen när du är klar med den tooavoid eventuella onödiga kostnader.</span><span class="sxs-lookup"><span data-stu-id="3d726-125">Be sure tooremove hello preconfigured solution from your subscription when you are done with it tooavoid any unnecessary charges.</span></span> <span data-ttu-id="3d726-126">Du kan helt ta bort en förkonfigurerade lösning från prenumerationen genom att besöka hello <https://www.azureiotsuite.com/> sidan.</span><span class="sxs-lookup"><span data-stu-id="3d726-126">You can completely remove a preconfigured solution from your subscription by visiting hello <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="3d726-127">När hello etableringsprocessen för hello remote övervakningslösning är klar klickar du på **starta** tooopen hello lösning instrumentpanel i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="3d726-127">When hello provisioning process for hello remote monitoring solution finishes, click **Launch** tooopen hello solution dashboard in your browser.</span></span>

![Instrumentpanel för lösningen][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a><span data-ttu-id="3d726-129">Etablera din enhet i hello remote övervakningslösning</span><span class="sxs-lookup"><span data-stu-id="3d726-129">Provision your device in hello remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="3d726-130">Om du redan har etablerat en enhet i din lösning kan du hoppa över det här steget.</span><span class="sxs-lookup"><span data-stu-id="3d726-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="3d726-131">När du skapar hello klientprogrammet behöver du tooknow hello enheten autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3d726-131">You need tooknow hello device credentials when you create hello client application.</span></span>
> 
> 

<span data-ttu-id="3d726-132">För en enhet tooconnect toohello förkonfigurerade lösning, den måste identifiera sig själv tooIoT hubb med giltiga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3d726-132">For a device tooconnect toohello preconfigured solution, it must identify itself tooIoT Hub using valid credentials.</span></span> <span data-ttu-id="3d726-133">Du kan hämta hello enheten autentiseringsuppgifter från hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3d726-133">You can retrieve hello device credentials from hello solution dashboard.</span></span> <span data-ttu-id="3d726-134">Du inkludera hello enheten autentiseringsuppgifter i ditt klientprogram senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="3d726-134">You include hello device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="3d726-135">tooadd en enhet tooyour remote övervakningslösning fullständig hello följa stegen i hello lösning instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="3d726-135">tooadd a device tooyour remote monitoring solution, complete hello following steps in hello solution dashboard:</span></span>

1. <span data-ttu-id="3d726-136">I hello nedre vänstra hörnet av hello instrumentpanelen, klickar du på **lägger till en enhet**.</span><span class="sxs-lookup"><span data-stu-id="3d726-136">In hello lower left-hand corner of hello dashboard, click **Add a device**.</span></span>
   
   ![Lägg till en enhet][1]
2. <span data-ttu-id="3d726-138">I hello **anpassad enhet** klickar du på **Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="3d726-138">In hello **Custom Device** panel, click **Add new**.</span></span>
   
   ![Lägg till en anpassad enhet][2]
3. <span data-ttu-id="3d726-140">Välj **Låt mig ange mitt eget enhets-ID**.</span><span class="sxs-lookup"><span data-stu-id="3d726-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="3d726-141">Ange ett enhets-ID som **mydevice**, klickar du på **kontrollera ID** tooverify namnet inte redan används och klicka sedan på **skapa** tooprovision hello enhet.</span><span class="sxs-lookup"><span data-stu-id="3d726-141">Enter a Device ID such as **mydevice**, click **Check ID** tooverify that name isn't already in use, and then click **Create** tooprovision hello device.</span></span>
   
   ![Lägg till enhets-ID][3]
4. <span data-ttu-id="3d726-143">Gör en anteckning hello enheten autentiseringsuppgifter (enhets-ID, IoT Hub-värdnamnet och nyckeln enheten).</span><span class="sxs-lookup"><span data-stu-id="3d726-143">Make a note hello device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="3d726-144">Klientprogrammet måste dessa värden tooconnect toohello remote övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="3d726-144">Your client application needs these values tooconnect toohello remote monitoring solution.</span></span> <span data-ttu-id="3d726-145">Klicka sedan på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="3d726-145">Then click **Done**.</span></span>
   
    ![Visa enhetsautentiseringsuppgifter][4]
5. <span data-ttu-id="3d726-147">Välj din enhet i listan över enheter hello hello lösning instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="3d726-147">Select your device in hello device list in hello solution dashboard.</span></span> <span data-ttu-id="3d726-148">Sedan hello **enhetsinformation** klickar du på **Aktivera enhet**.</span><span class="sxs-lookup"><span data-stu-id="3d726-148">Then, in hello **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="3d726-149">hello statusen för din enhet är nu **kör**.</span><span class="sxs-lookup"><span data-stu-id="3d726-149">hello status of your device is now **Running**.</span></span> <span data-ttu-id="3d726-150">hello remote övervakningslösning kan nu ta emot telemetri från enheten och anropa metoder i hello enhet.</span><span class="sxs-lookup"><span data-stu-id="3d726-150">hello remote monitoring solution can now receive telemetry from your device and invoke methods on hello device.</span></span>

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/