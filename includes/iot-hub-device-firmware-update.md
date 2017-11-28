## <a name="create-a-simulated-device-app"></a><span data-ttu-id="afebb-101">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="afebb-101">Create a simulated device app</span></span>
<span data-ttu-id="afebb-102">I det här avsnittet får du:</span><span class="sxs-lookup"><span data-stu-id="afebb-102">In this section, you:</span></span>

* <span data-ttu-id="afebb-103">Skapa en Node.js-konsolapp som svarar på en direkt metod som anropas via molnet</span><span class="sxs-lookup"><span data-stu-id="afebb-103">Create a Node.js console app that responds to a direct method called by the cloud</span></span>
* <span data-ttu-id="afebb-104">Utlösa en simulerad uppdatering av inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="afebb-104">Trigger a simulated firmware update</span></span>
* <span data-ttu-id="afebb-105">Använda rapporterade egenskaper för att aktivera enhetstvillingfrågor för att identifiera enheter och ta reda på när de senast slutfört en uppdatering av en inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="afebb-105">Use the reported properties to enable device twin queries to identify devices and when they last completed a firmware update</span></span>

<span data-ttu-id="afebb-106">Steg 1: Skapa en tom mapp som kallas **manageddevice**.</span><span class="sxs-lookup"><span data-stu-id="afebb-106">Step 1: Create an empty folder called **manageddevice**.</span></span>  <span data-ttu-id="afebb-107">I mappen **manageddevice** skapar du en package.json-fil med hjälp av följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="afebb-107">In the **manageddevice** folder, create a package.json file using the following command at your command prompt.</span></span> <span data-ttu-id="afebb-108">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="afebb-108">Accept all the defaults:</span></span>
   
    ```
    npm init
    ```

<span data-ttu-id="afebb-109">Steg 2: vid en kommandotolk i den **manageddevice** mapp, kör följande kommando för att installera den **azure iot-enhet** och **azure-iot-enhet – mqtt** enheten SDK paket:</span><span class="sxs-lookup"><span data-stu-id="afebb-109">Step 2: At your command prompt in the **manageddevice** folder, run the following command to install the **azure-iot-device** and **azure-iot-device-mqtt** Device SDK packages:</span></span>
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

<span data-ttu-id="afebb-110">Steg 3: Använd en textredigerare och skapa en **dmpatterns_fwupdate_device.js** filen i den **manageddevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="afebb-110">Step 3: Using a text editor, create a **dmpatterns_fwupdate_device.js** file in the **manageddevice** folder.</span></span>

<span data-ttu-id="afebb-111">Steg 4: Lägg till följande 'krävs, instruktioner i början av den **dmpatterns_fwupdate_device.js** fil:</span><span class="sxs-lookup"><span data-stu-id="afebb-111">Step 4: Add the following 'require' statements at the start of the **dmpatterns_fwupdate_device.js** file:</span></span>
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
<span data-ttu-id="afebb-112">Steg 5: Lägg till en **connectionString** variabel och använda den för att skapa en **klienten** instans.</span><span class="sxs-lookup"><span data-stu-id="afebb-112">Step 5: Add a **connectionString** variable and use it to create a **Client** instance.</span></span> <span data-ttu-id="afebb-113">Ersätt platshållaren `{yourdeviceconnectionstring}` med den anslutningssträng som du noterade tidigare i avsnittet Skapa en enhetsidentifitet:</span><span class="sxs-lookup"><span data-stu-id="afebb-113">Replace the `{yourdeviceconnectionstring}` placeholder with the connection string you previously made a note of in the "Create a device identity" section previously:</span></span>
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

<span data-ttu-id="afebb-114">Steg 6: Lägg till följande funktion som används för att uppdatera rapporterade egenskaper:</span><span class="sxs-lookup"><span data-stu-id="afebb-114">Step 6: Add the following function that is used to update reported properties:</span></span>
   
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

<span data-ttu-id="afebb-115">Steg 7: Lägg till följande funktioner som simulerar hämtar och inbyggd programvara avbildning tillämpas:</span><span class="sxs-lookup"><span data-stu-id="afebb-115">Step 7: Add the following functions that simulate downloading and applying the firmware image:</span></span>
   
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

<span data-ttu-id="afebb-116">Steg 8: Lägg till följande funktion som uppdaterar uppdateringsstatus för inbyggd programvara via egenskaperna rapporterade att **väntar på**.</span><span class="sxs-lookup"><span data-stu-id="afebb-116">Step 8: Add the following function that updates the firmware update status through the reported properties to **waiting**.</span></span> <span data-ttu-id="afebb-117">Normalt informeras enheter när en uppdatering är tillgänglig och när en princip som en administaratör har definierat börjar ladda ned och tillämpa uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="afebb-117">Typically, devices are informed of an available update and an administrator defined policy causes the device to start downloading and applying the update.</span></span> <span data-ttu-id="afebb-118">Logiken för att aktivera principen ska köras i den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="afebb-118">This function is where the logic to enable that policy should run.</span></span> <span data-ttu-id="afebb-119">För enkelhetens skull väntar exemplet på fyra sekunder innan du fortsätter att ladda ned firmware-avbildningen:</span><span class="sxs-lookup"><span data-stu-id="afebb-119">For simplicity, the sample waits for four seconds before proceeding to download the firmware image:</span></span>
   
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

<span data-ttu-id="afebb-120">Steg 9: Lägg till följande funktion som uppdaterar uppdateringsstatus för inbyggd programvara via egenskaperna rapporterade att **hämtar**.</span><span class="sxs-lookup"><span data-stu-id="afebb-120">Step 9: Add the following function that updates the firmware update status through the reported properties to **downloading**.</span></span> <span data-ttu-id="afebb-121">Funktionen simulerar sedan en nedladdning av den inbyggda programvaran och uppdaterar slutligen uppdateringsstatusen för den inbyggda programvaran till antingen **downloadFailed** (nedladdningen misslyckades) eller **downloadComplete** (nedladdningen har slutförts):</span><span class="sxs-lookup"><span data-stu-id="afebb-121">The function then simulates a firmware download and finally updates the firmware update status to either **downloadFailed** or **downloadComplete**:</span></span>
   
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

<span data-ttu-id="afebb-122">Steg 10: Lägg till följande funktion som uppdaterar uppdateringsstatus för inbyggd programvara via egenskaperna rapporterade att **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="afebb-122">Step 10: Add the following function that updates the firmware update status through the reported properties to **applying**.</span></span> <span data-ttu-id="afebb-123">Funktionen simulerar sedan tillämpning av avbildningen för den inbyggda programvaran och uppdaterar slutligen uppdateringsstatusen för den inbyggda programvaran till antingen **applyFailed** (tillämpningen misslyckades) eller **applyComplete** (tillämpningen har slutförts):</span><span class="sxs-lookup"><span data-stu-id="afebb-123">The function then simulates applying the firmware image and finally updates the firmware update status to either **applyFailed** or **applyComplete**:</span></span>
    
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

<span data-ttu-id="afebb-124">Steg 11: Lägg till följande funktion som hanterar den **firmwareUpdate** direkta metoden och initierar bearbeta flera steg firmware-uppdatering:</span><span class="sxs-lookup"><span data-stu-id="afebb-124">Step 11: Add the following function that handles the **firmwareUpdate** direct method and initiates the multi-stage firmware update process:</span></span>
    
    ```
    var onFirmwareUpdate = function(request, response) {
    
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });
    
      // Get the parameter from the body of the method request
      var fwPackageUri = request.payload.fwPackageUri;
    
      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
    
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });
    
        }
      });
    }
    ```

<span data-ttu-id="afebb-125">Steg 12: Slutligen lägger du till följande kod som ansluter till din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="afebb-125">Step 12: Finally, add the following code that connects to your IoT hub:</span></span>
    
    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
    
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate);
    });
    ```

> [!NOTE]
> <span data-ttu-id="afebb-126">För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="afebb-126">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="afebb-127">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i MSDN-artikel [hantering av tillfälliga fel](https://msdn.microsoft.com/library/hh675232.aspx).</span><span class="sxs-lookup"><span data-stu-id="afebb-127">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling](https://msdn.microsoft.com/library/hh675232.aspx).</span></span>
> 
> 