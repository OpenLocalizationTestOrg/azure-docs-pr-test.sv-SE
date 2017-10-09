## <a name="configure-hello-nodejs-simulated-device"></a><span data-ttu-id="ad14b-101">Konfigurera hello Node.js simulerade enhet</span><span class="sxs-lookup"><span data-stu-id="ad14b-101">Configure hello Node.js simulated device</span></span>
1. <span data-ttu-id="ad14b-102">Klicka på hello remote instrumentpanelen för övervakning **+ Lägg till en enhet** och Lägg sedan till en *anpassade*.</span><span class="sxs-lookup"><span data-stu-id="ad14b-102">On hello remote monitoring dashboard, click **+ Add a device** and then add a *custom device*.</span></span> <span data-ttu-id="ad14b-103">Anteckna hello IoT-hubb värdnamn, enhets-id och nyckeln för enheten.</span><span class="sxs-lookup"><span data-stu-id="ad14b-103">Make a note of hello IoT Hub hostname, device id, and device key.</span></span> <span data-ttu-id="ad14b-104">Behöver du dem senare i den här självstudiekursen när du förbereder hello remote_monitoring.js enheten klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="ad14b-104">You need them later in this tutorial when you prepare hello remote_monitoring.js device client application.</span></span>
2. <span data-ttu-id="ad14b-105">Se till att Node.js-version 0.12.x eller senare är installerat på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="ad14b-105">Ensure that Node.js version 0.12.x or later is installed on your development machine.</span></span> <span data-ttu-id="ad14b-106">Kör `node --version` i Kommandotolken eller i en shell toocheck hello-version.</span><span class="sxs-lookup"><span data-stu-id="ad14b-106">Run `node --version` at a command prompt or in a shell toocheck hello version.</span></span> <span data-ttu-id="ad14b-107">Information om hur du använder en package manager tooinstall Node.js på Linux finns [installera Node.js via Pakethanteraren][node-linux].</span><span class="sxs-lookup"><span data-stu-id="ad14b-107">For information about using a package manager tooinstall Node.js on Linux, see [Installing Node.js via package manager][node-linux].</span></span>
3. <span data-ttu-id="ad14b-108">När du har installerat Node.js klona hello senaste versionen av hello [azure iot-sdk-nod] [ lnk-github-repo] databasen tooyour utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="ad14b-108">When you have installed Node.js, clone hello latest version of hello [azure-iot-sdk-node][lnk-github-repo] repository tooyour development machine.</span></span> <span data-ttu-id="ad14b-109">Använd alltid hello **master** grenen för hello senaste versionen av hello bibliotek och exempel.</span><span class="sxs-lookup"><span data-stu-id="ad14b-109">Always use hello **master** branch for hello latest version of hello libraries and samples.</span></span>
4. <span data-ttu-id="ad14b-110">Från den lokala kopian av hello [azure iot-sdk-nod] [ lnk-github-repo] databasen, kopiera hello följande två filer från hello nod-enhet-exempel mappen tooan tom mapp på utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="ad14b-110">From your local copy of hello [azure-iot-sdk-node][lnk-github-repo] repository, copy hello following two files from hello node/device/samples folder tooan empty folder on your development machine:</span></span>
   
   * <span data-ttu-id="ad14b-111">Packages.JSON</span><span class="sxs-lookup"><span data-stu-id="ad14b-111">packages.json</span></span>
   * <span data-ttu-id="ad14b-112">remote_monitoring.js</span><span class="sxs-lookup"><span data-stu-id="ad14b-112">remote_monitoring.js</span></span>
5. <span data-ttu-id="ad14b-113">Öppna hello remote_monitoring.js filen och leta efter hello som följer variabeldefinitionen:</span><span class="sxs-lookup"><span data-stu-id="ad14b-113">Open hello remote_monitoring.js file and look for hello following variable definition:</span></span>
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. <span data-ttu-id="ad14b-114">Ersätt **[anslutningssträngen för IoT-hubb enheten]** med anslutningssträngen enhet.</span><span class="sxs-lookup"><span data-stu-id="ad14b-114">Replace **[IoT Hub device connection string]** with your device connection string.</span></span> <span data-ttu-id="ad14b-115">Använda hello värden för din IoT-hubb värdnamn, enhets-id och nyckeln för enheten som du antecknade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="ad14b-115">Use hello values for your IoT Hub hostname, device id, and device key that you made a note of in step 1.</span></span> <span data-ttu-id="ad14b-116">En anslutningssträng för enheten har hello följande format:</span><span class="sxs-lookup"><span data-stu-id="ad14b-116">A device connection string has hello following format:</span></span>
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    <span data-ttu-id="ad14b-117">Om din IoT Hub-värdnamnet är **contoso** och enhets-id är **mydevice**, anslutningssträngen ser ut som följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="ad14b-117">If your IoT Hub hostname is **contoso** and your device id is **mydevice**, your connection string looks like hello following snippet:</span></span>
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Spara hello-filen. <span data-ttu-id="ad14b-119">Kör följande kommandon i ett gränssnitt eller en kommandotolk i hello-mapp som innehåller filer tooinstall hello nödvändiga paketen hello och kör sedan hello exempelprogrammet:</span><span class="sxs-lookup"><span data-stu-id="ad14b-119">Run hello following commands in a shell or command prompt in hello folder that contains these files tooinstall hello necessary packages and then run hello sample application:</span></span>
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a><span data-ttu-id="ad14b-120">Se dynamiska telemetri i praktiken</span><span class="sxs-lookup"><span data-stu-id="ad14b-120">Observe dynamic telemetry in action</span></span>
<span data-ttu-id="ad14b-121">hello instrumentpanelen visar hello temperatur- och fuktighetskonsekvens telemetri från hello befintliga simulerade enheter:</span><span class="sxs-lookup"><span data-stu-id="ad14b-121">hello dashboard shows hello temperature and humidity telemetry from hello existing simulated devices:</span></span>

![hello standardinstrumentpanelen][image1]

<span data-ttu-id="ad14b-123">Om du väljer hello Node.js simulerade enhet du körde hello föregående avsnitt visas temperatur, fuktighet och externa temperatur telemetri:</span><span class="sxs-lookup"><span data-stu-id="ad14b-123">If you select hello Node.js simulated device you ran in hello previous section, you see temperature, humidity, and external temperature telemetry:</span></span>

![Lägga till externa temperatur toohello instrumentpanel][image2]

<span data-ttu-id="ad14b-125">hello remote övervakningslösning identifierar hello ytterligare externa temperatur telemetri typ och lägger till den toohello diagram på instrumentpanelen för hello automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ad14b-125">hello remote monitoring solution automatically detects hello additional external temperature telemetry type and adds it toohello chart on hello dashboard.</span></span>

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png