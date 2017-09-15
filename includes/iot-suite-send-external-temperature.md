## <a name="configure-the-nodejs-simulated-device"></a><span data-ttu-id="75c68-101">Konfigurera Node.js simulerade enheten</span><span class="sxs-lookup"><span data-stu-id="75c68-101">Configure the Node.js simulated device</span></span>
1. <span data-ttu-id="75c68-102">Klicka på fjärranslutna instrumentpanelen för övervakning, **+ Lägg till en enhet** och Lägg sedan till en *anpassade*.</span><span class="sxs-lookup"><span data-stu-id="75c68-102">On the remote monitoring dashboard, click **+ Add a device** and then add a *custom device*.</span></span> <span data-ttu-id="75c68-103">Anteckna IoT-hubben värdnamn, enhets-id och nyckeln för enheten.</span><span class="sxs-lookup"><span data-stu-id="75c68-103">Make a note of the IoT Hub hostname, device id, and device key.</span></span> <span data-ttu-id="75c68-104">Behöver du dem senare i den här självstudiekursen när du förbereder klientprogrammet remote_monitoring.js enhet.</span><span class="sxs-lookup"><span data-stu-id="75c68-104">You need them later in this tutorial when you prepare the remote_monitoring.js device client application.</span></span>
2. <span data-ttu-id="75c68-105">Se till att Node.js-version 0.12.x eller senare är installerat på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="75c68-105">Ensure that Node.js version 0.12.x or later is installed on your development machine.</span></span> <span data-ttu-id="75c68-106">Kör `node --version` vid en kommandotolk eller i ett gränssnitt för att kontrollera versionen.</span><span class="sxs-lookup"><span data-stu-id="75c68-106">Run `node --version` at a command prompt or in a shell to check the version.</span></span> <span data-ttu-id="75c68-107">Information om hur du använder en Pakethanteraren installera Node.js på Linux finns [installera Node.js via Pakethanteraren][node-linux].</span><span class="sxs-lookup"><span data-stu-id="75c68-107">For information about using a package manager to install Node.js on Linux, see [Installing Node.js via package manager][node-linux].</span></span>
3. <span data-ttu-id="75c68-108">När du har installerat Node.js klona den senaste versionen av den [azure iot-sdk-nod] [ lnk-github-repo] databasen på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="75c68-108">When you have installed Node.js, clone the latest version of the [azure-iot-sdk-node][lnk-github-repo] repository to your development machine.</span></span> <span data-ttu-id="75c68-109">Använd alltid den **master** grenen för den senaste versionen av bibliotek och exempel.</span><span class="sxs-lookup"><span data-stu-id="75c68-109">Always use the **master** branch for the latest version of the libraries and samples.</span></span>
4. <span data-ttu-id="75c68-110">Från den lokala kopian av den [azure iot-sdk-nod] [ lnk-github-repo] databasen, kopiera följande två filer från mappen nod-enhet-exempel till en tom mapp på utvecklingsdatorn:</span><span class="sxs-lookup"><span data-stu-id="75c68-110">From your local copy of the [azure-iot-sdk-node][lnk-github-repo] repository, copy the following two files from the node/device/samples folder to an empty folder on your development machine:</span></span>
   
   * <span data-ttu-id="75c68-111">Packages.JSON</span><span class="sxs-lookup"><span data-stu-id="75c68-111">packages.json</span></span>
   * <span data-ttu-id="75c68-112">remote_monitoring.js</span><span class="sxs-lookup"><span data-stu-id="75c68-112">remote_monitoring.js</span></span>
5. <span data-ttu-id="75c68-113">Öppna filen remote_monitoring.js och leta efter följande variabeldefinitionen:</span><span class="sxs-lookup"><span data-stu-id="75c68-113">Open the remote_monitoring.js file and look for the following variable definition:</span></span>
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. <span data-ttu-id="75c68-114">Ersätt **[anslutningssträngen för IoT-hubb enheten]** med anslutningssträngen enhet.</span><span class="sxs-lookup"><span data-stu-id="75c68-114">Replace **[IoT Hub device connection string]** with your device connection string.</span></span> <span data-ttu-id="75c68-115">Använd värdena för din IoT-hubb värdnamn, enhets-id och nyckeln för enheten som du antecknade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="75c68-115">Use the values for your IoT Hub hostname, device id, and device key that you made a note of in step 1.</span></span> <span data-ttu-id="75c68-116">En anslutningssträng för enheten har följande format:</span><span class="sxs-lookup"><span data-stu-id="75c68-116">A device connection string has the following format:</span></span>
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    <span data-ttu-id="75c68-117">Om din IoT Hub-värdnamnet är **contoso** och enhets-id är **mydevice**, anslutningssträngen ser ut som följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="75c68-117">If your IoT Hub hostname is **contoso** and your device id is **mydevice**, your connection string looks like the following snippet:</span></span>
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Spara filen. <span data-ttu-id="75c68-119">Kör följande kommandon i ett gränssnitt eller en kommandotolk i den mapp som innehåller filerna för att installera de nödvändiga paketen och kör exempelprogrammet:</span><span class="sxs-lookup"><span data-stu-id="75c68-119">Run the following commands in a shell or command prompt in the folder that contains these files to install the necessary packages and then run the sample application:</span></span>
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a><span data-ttu-id="75c68-120">Se dynamiska telemetri i praktiken</span><span class="sxs-lookup"><span data-stu-id="75c68-120">Observe dynamic telemetry in action</span></span>
<span data-ttu-id="75c68-121">Instrumentpanelen visar temperatur- och fuktighetskonsekvens telemetri från befintliga simulerade enheter:</span><span class="sxs-lookup"><span data-stu-id="75c68-121">The dashboard shows the temperature and humidity telemetry from the existing simulated devices:</span></span>

![Standardinstrumentpanelen][image1]

<span data-ttu-id="75c68-123">Om du väljer den simulerade enheten Node.js som du körde i föregående avsnitt visas temperatur, fuktighet och externa temperatur telemetri:</span><span class="sxs-lookup"><span data-stu-id="75c68-123">If you select the Node.js simulated device you ran in the previous section, you see temperature, humidity, and external temperature telemetry:</span></span>

![Lägg till externa temperatur på instrumentpanelen][image2]

<span data-ttu-id="75c68-125">Remote övervakningslösning automatiskt identifierar typen ytterligare externa temperatur telemetri och lägger till den diagram på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="75c68-125">The remote monitoring solution automatically detects the additional external temperature telemetry type and adds it to the chart on the dashboard.</span></span>

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png