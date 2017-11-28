---
title: aaaConnect en enhet med Node.js | Microsoft Docs
description: "Beskriver hur tooconnect en enhet toohello Azure IoT Suite förkonfigurerade remote övervakningslösning som använder ett program som skrivits i Node.js."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 80bf2b70f15f539bfce4f135d533c46dd2b3f5a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-nodejs"></a><span data-ttu-id="7e750-103">Ansluta din enhet toohello remote förkonfigurerade övervakningslösning (Node.js)</span><span class="sxs-lookup"><span data-stu-id="7e750-103">Connect your device toohello remote monitoring preconfigured solution (Node.js)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a><span data-ttu-id="7e750-104">Skapa en lösning för node.js-exempel</span><span class="sxs-lookup"><span data-stu-id="7e750-104">Create a node.js sample solution</span></span>

<span data-ttu-id="7e750-105">Se till att Node.js version 0.11.5 eller senare är installerat på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="7e750-105">Ensure that Node.js version 0.11.5 or later is installed on your development machine.</span></span> <span data-ttu-id="7e750-106">Du kan köra `node --version` hello kommandoraden toocheck hello version.</span><span class="sxs-lookup"><span data-stu-id="7e750-106">You can run `node --version` at hello command line toocheck hello version.</span></span>

1. <span data-ttu-id="7e750-107">Skapa en mapp med namnet **RemoteMonitoring** på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="7e750-107">Create a folder called **RemoteMonitoring** on your development machine.</span></span> <span data-ttu-id="7e750-108">Navigera toothis mappen i kommandoradsverktyget miljön.</span><span class="sxs-lookup"><span data-stu-id="7e750-108">Navigate toothis folder in your command-line environment.</span></span>

1. <span data-ttu-id="7e750-109">Kör hello följande kommandon toodownload och installera hello-paket som du behöver toocomplete hello sample-appen:</span><span class="sxs-lookup"><span data-stu-id="7e750-109">Run hello following commands toodownload and install hello packages you need toocomplete hello sample app:</span></span>

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="7e750-110">I hello **RemoteMonitoring** mapp, skapa en fil med namnet **remote_monitoring.js**.</span><span class="sxs-lookup"><span data-stu-id="7e750-110">In hello **RemoteMonitoring** folder, create a file called **remote_monitoring.js**.</span></span> <span data-ttu-id="7e750-111">Öppna den här filen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="7e750-111">Open this file in a text editor.</span></span>

1. <span data-ttu-id="7e750-112">I hello **remote_monitoring.js** lägger du till följande hello `require` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="7e750-112">In hello **remote_monitoring.js** file, add hello following `require` statements:</span></span>

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. <span data-ttu-id="7e750-113">Lägg till följande variabeldeklarationer efter hello hello `require` instruktioner.</span><span class="sxs-lookup"><span data-stu-id="7e750-113">Add hello following variable declarations after hello `require` statements.</span></span> <span data-ttu-id="7e750-114">Ersätt hello platshållarvärdena [enhets-Id] och [enhetsnyckel] med värdena som du antecknade för din enhet i hello remote lösning instrumentpanelen för övervakning.</span><span class="sxs-lookup"><span data-stu-id="7e750-114">Replace hello placeholder values [Device Id] and [Device Key] with values you noted for your device in hello remote monitoring solution dashboard.</span></span> <span data-ttu-id="7e750-115">Använd hello IoT Hub-värdnamnet från hello lösning instrumentpanelen tooreplace [IoTHub Name].</span><span class="sxs-lookup"><span data-stu-id="7e750-115">Use hello IoT Hub Hostname from hello solution dashboard tooreplace [IoTHub Name].</span></span> <span data-ttu-id="7e750-116">Om ditt värdnamn för IoT Hub exempelvis är **contoso.azure-devices.net** ersätter du [IoTHub-namn] med **contoso**:</span><span class="sxs-lookup"><span data-stu-id="7e750-116">For example, if your IoT Hub Hostname is **contoso.azure-devices.net**, replace [IoTHub Name] with **contoso**:</span></span>

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. <span data-ttu-id="7e750-117">Lägg till följande variabler toodefine hello vissa grundläggande telemetridata:</span><span class="sxs-lookup"><span data-stu-id="7e750-117">Add hello following variables toodefine some base telemetry data:</span></span>

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. <span data-ttu-id="7e750-118">Lägg till hello följande helper funktionen tooprint Åtgärdsresultat:</span><span class="sxs-lookup"><span data-stu-id="7e750-118">Add hello following helper function tooprint operation results:</span></span>

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. <span data-ttu-id="7e750-119">Lägg till följande helper funktionen toouse toorandomize hello telemetri värden hello:</span><span class="sxs-lookup"><span data-stu-id="7e750-119">Add hello following helper function toouse toorandomize hello telemetry values:</span></span>

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. <span data-ttu-id="7e750-120">Lägg till följande definition för hello hello **DeviceInfo** objekt hello enheten skickar vid start:</span><span class="sxs-lookup"><span data-stu-id="7e750-120">Add hello following definition for hello **DeviceInfo** object hello device sends on startup:</span></span>

    ```nodejs
    var deviceMetaData = {
        'ObjectType': 'DeviceInfo',
        'IsSimulatedDevice': 0,
        'Version': '1.0',
        'DeviceProperties': {
            'DeviceID': deviceId,
            'HubEnabledState': 1
        }
    };
    ```

1. <span data-ttu-id="7e750-121">Lägg till följande hello definition för hello enheten dubbla rapporterade värden.</span><span class="sxs-lookup"><span data-stu-id="7e750-121">Add hello following definition for hello device twin reported values.</span></span> <span data-ttu-id="7e750-122">Den här definitionen innehåller beskrivningar av hello direkt metoder hello enhet stöder:</span><span class="sxs-lookup"><span data-stu-id="7e750-122">This definition includes descriptions of hello direct methods hello device supports:</span></span>

    ```nodejs
    var reportedProperties = {
        "Device": {
            "DeviceState": "normal",
            "Location": {
                "Latitude": 47.642877,
                "Longitude": -122.125497
            }
        },
        "Config": {
            "TemperatureMeanValue": 56.7,
            "TelemetryInterval": 45
        },
        "System": {
            "Manufacturer": "Contoso Inc.",
            "FirmwareVersion": "2.22",
            "InstalledRAM": "8 MB",
            "ModelNumber": "DB-14",
            "Platform": "Plat 9.75",
            "Processor": "i3-9",
            "SerialNumber": "SER99"
        },
        "Location": {
            "Latitude": 47.642877,
            "Longitude": -122.125497
        },
        "SupportedMethods": {
            "Reboot": "Reboot hello device",
            "InitiateFirmwareUpdate--FwPackageURI-string": "Updates device Firmware. Use parameter FwPackageURI toospecifiy hello URI of hello firmware file"
        },
    }
    ```

1. <span data-ttu-id="7e750-123">Lägg till följande funktion toohandle hello hello **omstart** direkt metodanrop:</span><span class="sxs-lookup"><span data-stu-id="7e750-123">Add hello following function toohandle hello **Reboot** direct method call:</span></span>

    ```nodejs
    function onReboot(request, response) {
        // Implement actual logic here.
        console.log('Simulated reboot...');

        // Complete hello response
        response.send(200, "Rebooting device", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

1. <span data-ttu-id="7e750-124">Lägg till följande funktion toohandle hello hello **InitiateFirmwareUpdate** direkt metodanrop.</span><span class="sxs-lookup"><span data-stu-id="7e750-124">Add hello following function toohandle hello **InitiateFirmwareUpdate** direct method call.</span></span> <span data-ttu-id="7e750-125">Den här direkta metoden använder en parameter toospecify hello plats av hello firmware avbildningen toodownload och initierar hello firmware-uppdatering på hello enhet asynkront:</span><span class="sxs-lookup"><span data-stu-id="7e750-125">This direct method uses a parameter toospecify hello location of hello firmware image toodownload, and initiates hello firmware update on hello device asynchronously:</span></span>

    ```nodejs
    function onInitiateFirmwareUpdate(request, response) {
        console.log('Simulated firmware update initiated, using: ' + request.payload.FwPackageURI);

        // Complete hello response
        response.send(200, "Firmware update initiated", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.' );
            }
        });

        // Add logic here tooperform hello firmware update asynchronously
    }
    ```

1. <span data-ttu-id="7e750-126">Lägg till hello följande kod toocreate en klientinstans:</span><span class="sxs-lookup"><span data-stu-id="7e750-126">Add hello following code toocreate a client instance:</span></span>

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="7e750-127">Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="7e750-127">Add hello following code to:</span></span>

    * <span data-ttu-id="7e750-128">Öppna hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="7e750-128">Open hello connection.</span></span>
    * <span data-ttu-id="7e750-129">Skicka hello **DeviceInfo** objekt.</span><span class="sxs-lookup"><span data-stu-id="7e750-129">Send hello **DeviceInfo** object.</span></span>
    * <span data-ttu-id="7e750-130">Ställ in en hanterare för egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7e750-130">Set up a handler for desired properties.</span></span>
    * <span data-ttu-id="7e750-131">Skicka rapporterat egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7e750-131">Send reported properties.</span></span>
    * <span data-ttu-id="7e750-132">Registrera hanterare för hello direkt metoder.</span><span class="sxs-lookup"><span data-stu-id="7e750-132">Register handlers for hello direct methods.</span></span>
    * <span data-ttu-id="7e750-133">Börja skicka telemetri.</span><span class="sxs-lookup"><span data-stu-id="7e750-133">Start sending telemetry.</span></span>

    ```nodejs
    client.open(function (err) {
        if (err) {
            printErrorFor('open')(err);
        } else {
            console.log('Sending device metadata:\n' + JSON.stringify(deviceMetaData));
            client.sendEvent(new Message(JSON.stringify(deviceMetaData)), printErrorFor('send metadata'));

            // Create device twin
            client.getTwin(function(err, twin) {
                if (err) {
                    console.error('Could not get device twin');
                } else {
                    console.log('Device twin created');

                    twin.on('properties.desired', function(delta) {
                        console.log('Received new desired properties:');
                        console.log(JSON.stringify(delta));
                    });

                    // Send reported properties
                    twin.properties.reported.update(reportedProperties, function(err) {
                        if (err) throw err;
                        console.log('twin state reported');
                    });

                    // Register handlers for direct methods
                    client.onDeviceMethod('Reboot', onReboot);
                    client.onDeviceMethod('InitiateFirmwareUpdate', onInitiateFirmwareUpdate);
                }
            });

            // Start sending telemetry
            var sendInterval = setInterval(function () {
                temperature += generateRandomIncrement();
                externalTemperature += generateRandomIncrement();
                humidity += generateRandomIncrement();

                var data = JSON.stringify({
                    'DeviceID': deviceId,
                    'Temperature': temperature,
                    'Humidity': humidity,
                    'ExternalTemperature': externalTemperature
                });

                console.log('Sending device event data:\n' + data);
                client.sendEvent(new Message(data), printErrorFor('send event'));
            }, 5000);

            client.on('error', function (err) {
                printErrorFor('client')(err);
                if (sendInterval) clearInterval(sendInterval);
                client.close(printErrorFor('client.close'));
            });
        }
    });
    ```

1. <span data-ttu-id="7e750-134">Spara hello ändringar toohello **remote_monitoring.js** fil.</span><span class="sxs-lookup"><span data-stu-id="7e750-134">Save hello changes toohello **remote_monitoring.js** file.</span></span>

1. <span data-ttu-id="7e750-135">Kör följande kommando vid en kommandotolk toolaunch hello exempelprogrammet hello:</span><span class="sxs-lookup"><span data-stu-id="7e750-135">Run hello following command at a command prompt toolaunch hello sample application:</span></span>
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
