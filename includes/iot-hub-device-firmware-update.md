## <a name="create-a-simulated-device-app"></a><span data-ttu-id="9a607-101">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="9a607-101">Create a simulated device app</span></span>
<span data-ttu-id="9a607-102">I det här avsnittet får du:</span><span class="sxs-lookup"><span data-stu-id="9a607-102">In this section, you:</span></span>

* <span data-ttu-id="9a607-103">Skapa en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln</span><span class="sxs-lookup"><span data-stu-id="9a607-103">Create a Node.js console app that responds tooa direct method called by hello cloud</span></span>
* <span data-ttu-id="9a607-104">Utlösa en simulerad uppdatering av inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="9a607-104">Trigger a simulated firmware update</span></span>
* <span data-ttu-id="9a607-105">Använd hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast slutförda en firmware-uppdatering</span><span class="sxs-lookup"><span data-stu-id="9a607-105">Use hello reported properties tooenable device twin queries tooidentify devices and when they last completed a firmware update</span></span>

<span data-ttu-id="9a607-106">Steg 1: Skapa en tom mapp som kallas **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="9a607-106">Step 1: Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="9a607-107">I hello **manageddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="9a607-107">In hello **manageddevice** folder, create a package.json file using hello following command at your command prompt.</span></span> <span data-ttu-id="9a607-108">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="9a607-108">Accept all hello defaults:</span></span>
   
    ```
    npm init
    ```

<span data-ttu-id="9a607-109">Steg 2: vid en kommandotolk i hello **manageddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** och **azure-iot-enhet – mqtt** enhet SDK-paket:</span><span class="sxs-lookup"><span data-stu-id="9a607-109">Step 2: At your command prompt in hello **manageddevice** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** Device SDK packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

<span data-ttu-id="9a607-110">Steg 3: Använd en textredigerare och skapa en **dmpatterns_fwupdate_device.js** filen i hello **manageddevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="9a607-110">Step 3: Using a text editor, create a **dmpatterns_fwupdate_device.js** file in hello **manageddevice** folder.</span></span>

<span data-ttu-id="9a607-111">Steg 4: Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_fwupdate_device.js** fil:</span><span class="sxs-lookup"><span data-stu-id="9a607-111">Step 4: Add hello following 'require' statements at hello start of hello **dmpatterns_fwupdate_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
<span data-ttu-id="9a607-112">Steg 5: Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans.</span><span class="sxs-lookup"><span data-stu-id="9a607-112">Step 5: Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span> <span data-ttu-id="9a607-113">Ersätt hello `{yourdeviceconnectionstring}` med hello anslutningssträngen som du tidigare antecknade i hello ”skapa en enhetsidentitet” tidigare:</span><span class="sxs-lookup"><span data-stu-id="9a607-113">Replace hello `{yourdeviceconnectionstring}` placeholder with hello connection string you previously made a note of in hello "Create a device identity" section previously:</span></span>
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

<span data-ttu-id="9a607-114">Steg 6: Lägg till hello följande funktion används tooupdate rapporterade egenskaper:</span><span class="sxs-lookup"><span data-stu-id="9a607-114">Step 6: Add hello following function that is used tooupdate reported properties:</span></span>
   
    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };
   
      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported: ' + firmwareUpdateValue.status);
      });
    };
    ```

<span data-ttu-id="9a607-115">Steg 7: Lägg till följande funktioner som simulerar hämta och tillämpa hello firmware avbildning hello:</span><span class="sxs-lookup"><span data-stu-id="9a607-115">Step 7: Add hello following functions that simulate downloading and applying hello firmware image:</span></span>
   
    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
   
      console.log("Downloading image from " + imageUrl);
   
      callback(error, image);
    }
   
    var simulateApplyImage = function(imageData, callback) {
      var error = null;
   
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
   
      callback(error);
    }
    ```

<span data-ttu-id="9a607-116">Steg 8: Lägg till följande funktion att uppdateringar hello firmware uppdateringsstatus via hello rapporterat egenskaper för hello**väntar på**.</span><span class="sxs-lookup"><span data-stu-id="9a607-116">Step 8: Add hello following function that updates hello firmware update status through hello reported properties too**waiting**.</span></span> <span data-ttu-id="9a607-117">Normalt informeras enheter om en tillgänglig uppdatering och en administratör som definierats princip orsakar hello enheten toostart hämtar och installerar hello uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="9a607-117">Typically, devices are informed of an available update and an administrator defined policy causes hello device toostart downloading and applying hello update.</span></span> <span data-ttu-id="9a607-118">Den här funktionen är där hello logik tooenable som principen ska köras.</span><span class="sxs-lookup"><span data-stu-id="9a607-118">This function is where hello logic tooenable that policy should run.</span></span> <span data-ttu-id="9a607-119">För enkelhetens skull hello exempel som väntar på fyra sekunder innan du fortsätter toodownload hello firmware avbildningen:</span><span class="sxs-lookup"><span data-stu-id="9a607-119">For simplicity, hello sample waits for four seconds before proceeding toodownload hello firmware image:</span></span>
   
    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
   
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```

<span data-ttu-id="9a607-120">Steg 9: Lägg till följande funktion att uppdateringar hello firmware uppdateringsstatus via hello rapporterat egenskaper för hello**hämtar**.</span><span class="sxs-lookup"><span data-stu-id="9a607-120">Step 9: Add hello following function that updates hello firmware update status through hello reported properties too**downloading**.</span></span> <span data-ttu-id="9a607-121">hello funktionen sedan simulerar en hämtning av inbyggd programvara och slutligen uppdateringar hello firmware update status tooeither **downloadFailed** eller **downloadComplete**:</span><span class="sxs-lookup"><span data-stu-id="9a607-121">hello function then simulates a firmware download and finally updates hello firmware update status tooeither **downloadFailed** or **downloadComplete**:</span></span>
   
    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
   
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
   
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
   
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
   
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
   
      }, 4000);
    }
    ```

<span data-ttu-id="9a607-122">Steg 10: Lägg till följande funktion att uppdateringar hello firmware uppdateringsstatus via hello rapporterat egenskaper för hello**tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="9a607-122">Step 10: Add hello following function that updates hello firmware update status through hello reported properties too**applying**.</span></span> <span data-ttu-id="9a607-123">hello funktionen sedan simulerar använder hello firmware avbildningen och slutligen uppdateringar hello firmware update status tooeither **applyFailed** eller **applyComplete**:</span><span class="sxs-lookup"><span data-stu-id="9a607-123">hello function then simulates applying hello firmware image and finally updates hello firmware update status tooeither **applyFailed** or **applyComplete**:</span></span>
    
    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
    
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
    
      setTimeout(function() {
    
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
    
          }
        });
    
        setTimeout(callback, 4000);
    
      }, 4000);
    }
    ```

<span data-ttu-id="9a607-124">Steg 11: Lägg till följande hello fungera som hanterar hello **firmwareUpdate** direkta metoden och initierar hello flera steg firmware uppdatera process:</span><span class="sxs-lookup"><span data-stu-id="9a607-124">Step 11: Add hello following function that handles hello **firmwareUpdate** direct method and initiates hello multi-stage firmware update process:</span></span>
    
    ```
    var onFirmwareUpdate = function(request, response) {
    
      // Respond hello cloud app for hello direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
        }
      });
    
      // Get hello parameter from hello body of hello method request
      var fwPackageUri = request.payload.fwPackageUri;
    
      // Obtain hello device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
    
          // Start hello multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });
    
        }
      });
    }
    ```

<span data-ttu-id="9a607-125">Steg 12: Slutligen lägger du till hello efter koden som ansluter tooyour IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="9a607-125">Step 12: Finally, add hello following code that connects tooyour IoT hub:</span></span>
    
    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect tooIotHub client');
      }  else {
        console.log('Client connected tooIoT Hub.  Waiting for firmwareUpdate direct method.');
      }
    
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate);
    });
    ```

> [!NOTE]
> <span data-ttu-id="9a607-126">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="9a607-126">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="9a607-127">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel](https://msdn.microsoft.com/library/hh675232.aspx).</span><span class="sxs-lookup"><span data-stu-id="9a607-127">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling](https://msdn.microsoft.com/library/hh675232.aspx).</span></span>
> 
> 