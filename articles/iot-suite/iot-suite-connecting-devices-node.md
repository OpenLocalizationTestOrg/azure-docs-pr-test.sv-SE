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
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-nodejs"></a>Ansluta din enhet toohello remote förkonfigurerade övervakningslösning (Node.js)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a>Skapa en lösning för node.js-exempel

Se till att Node.js version 0.11.5 eller senare är installerat på utvecklingsdatorn. Du kan köra `node --version` hello kommandoraden toocheck hello version.

1. Skapa en mapp med namnet **RemoteMonitoring** på utvecklingsdatorn. Navigera toothis mappen i kommandoradsverktyget miljön.

1. Kör hello följande kommandon toodownload och installera hello-paket som du behöver toocomplete hello sample-appen:

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. I hello **RemoteMonitoring** mapp, skapa en fil med namnet **remote_monitoring.js**. Öppna den här filen i en textredigerare.

1. I hello **remote_monitoring.js** lägger du till följande hello `require` instruktioner:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Lägg till följande variabeldeklarationer efter hello hello `require` instruktioner. Ersätt hello platshållarvärdena [enhets-Id] och [enhetsnyckel] med värdena som du antecknade för din enhet i hello remote lösning instrumentpanelen för övervakning. Använd hello IoT Hub-värdnamnet från hello lösning instrumentpanelen tooreplace [IoTHub Name]. Om ditt värdnamn för IoT Hub exempelvis är **contoso.azure-devices.net** ersätter du [IoTHub-namn] med **contoso**:

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Lägg till följande variabler toodefine hello vissa grundläggande telemetridata:

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. Lägg till hello följande helper funktionen tooprint Åtgärdsresultat:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Lägg till följande helper funktionen toouse toorandomize hello telemetri värden hello:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Lägg till följande definition för hello hello **DeviceInfo** objekt hello enheten skickar vid start:

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

1. Lägg till följande hello definition för hello enheten dubbla rapporterade värden. Den här definitionen innehåller beskrivningar av hello direkt metoder hello enhet stöder:

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

1. Lägg till följande funktion toohandle hello hello **omstart** direkt metodanrop:

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

1. Lägg till följande funktion toohandle hello hello **InitiateFirmwareUpdate** direkt metodanrop. Den här direkta metoden använder en parameter toospecify hello plats av hello firmware avbildningen toodownload och initierar hello firmware-uppdatering på hello enhet asynkront:

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

1. Lägg till hello följande kod toocreate en klientinstans:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Lägg till följande kod hello:

    * Öppna hello-anslutning.
    * Skicka hello **DeviceInfo** objekt.
    * Ställ in en hanterare för egenskaper.
    * Skicka rapporterat egenskaper.
    * Registrera hanterare för hello direkt metoder.
    * Börja skicka telemetri.

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

1. Spara hello ändringar toohello **remote_monitoring.js** fil.

1. Kör följande kommando vid en kommandotolk toolaunch hello exempelprogrammet hello:
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
