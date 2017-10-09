## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp
I det här avsnittet får du:

* Skapa en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln
* Utlösa en simulerad uppdatering av inbyggd programvara
* Använd hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast slutförda en firmware-uppdatering

Steg 1: Skapa en tom mapp som kallas **manageddevice**.  I hello **manageddevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```

Steg 2: vid en kommandotolk i hello **manageddevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** och **azure-iot-enhet – mqtt** enhet SDK-paket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

Steg 3: Använd en textredigerare och skapa en **dmpatterns_fwupdate_device.js** filen i hello **manageddevice** mapp.

Steg 4: Lägg till följande hello 'krävs, instruktioner hello början av hello **dmpatterns_fwupdate_device.js** fil:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
Steg 5: Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans. Ersätt hello `{yourdeviceconnectionstring}` med hello anslutningssträngen som du tidigare antecknade i hello ”skapa en enhetsidentitet” tidigare:
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

Steg 6: Lägg till hello följande funktion används tooupdate rapporterade egenskaper:
   
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

Steg 7: Lägg till följande funktioner som simulerar hämta och tillämpa hello firmware avbildning hello:
   
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

Steg 8: Lägg till följande funktion att uppdateringar hello firmware uppdateringsstatus via hello rapporterat egenskaper för hello**väntar på**. Normalt informeras enheter om en tillgänglig uppdatering och en administratör som definierats princip orsakar hello enheten toostart hämtar och installerar hello uppdateringen. Den här funktionen är där hello logik tooenable som principen ska köras. För enkelhetens skull hello exempel som väntar på fyra sekunder innan du fortsätter toodownload hello firmware avbildningen:
   
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

Steg 9: Lägg till följande funktion att uppdateringar hello firmware uppdateringsstatus via hello rapporterat egenskaper för hello**hämtar**. hello funktionen sedan simulerar en hämtning av inbyggd programvara och slutligen uppdateringar hello firmware update status tooeither **downloadFailed** eller **downloadComplete**:
   
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

Steg 10: Lägg till följande funktion att uppdateringar hello firmware uppdateringsstatus via hello rapporterat egenskaper för hello**tillämpa**. hello funktionen sedan simulerar använder hello firmware avbildningen och slutligen uppdateringar hello firmware update status tooeither **applyFailed** eller **applyComplete**:
    
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

Steg 11: Lägg till följande hello fungera som hanterar hello **firmwareUpdate** direkta metoden och initierar hello flera steg firmware uppdatera process:
    
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

Steg 12: Slutligen lägger du till hello efter koden som ansluter tooyour IoT-hubb:
    
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
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel](https://msdn.microsoft.com/library/hh675232.aspx).
> 
> 