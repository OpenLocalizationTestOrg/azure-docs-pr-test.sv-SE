---
title: "Egenskaper för aaaUse Azure IoT Hub enhet dubbla (nod) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooconfigure enheter. Du kan använda hello Azure IoT SDK för Node.js tooimplement en simulerad enhetsapp och en service-app som ändrar en konfiguration för enheter med hjälp av en enhet dubbla."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: elioda
ms.openlocfilehash: 7ebfe2dfa0876bf04fdbaceae55db76456523e8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-desired-properties-tooconfigure-devices-node"></a>Använd önskade egenskaper tooconfigure enheter (nod)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Hello slutet av den här självstudiekursen har du två Node.js-konsolappar:

* **SimulateDeviceConfiguration.js**, en simulerad enhetsapp som väntar på en uppdatering för önskad konfiguration och rapporterar hello status för en simulerad konfigurationsprocessen för uppdateringen.
* **SetDesiredConfigurationAndQuery.js**, en Node.js backend-app, vilket anger hello önskad konfiguration på en enhet och frågor hello konfigurationsprocessen för uppdateringen.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.
> 
> 

toocomplete den här kursen behöver du hello följande:

* Node.js version 0.10.x eller senare.
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

Om du har följt hello [Kom igång med enheten twins] [ lnk-twin-tutorial] kursen har du redan en IoT-hubb och en enhetsidentitet kallas **myDeviceId**, och du kan hoppa över toohello [ Skapa hello simulerade enhetsapp] [ lnk-how-to-configure-createapp] avsnitt.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-hello-simulated-device-app"></a>Skapa hello simulerade enhetsapp
I det här avsnittet skapar du en Node.js-konsolprogram som ansluter tooyour hubb som **myDeviceId**, väntar på en uppdatering för önskad konfiguration och rapporterar uppdateringar på hello simulerade konfigurationsprocessen för uppdateringen.

1. Skapa en ny tom mapp som kallas **simulatedeviceconfiguration**. I hello **simulatedeviceconfiguration** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **simulatedeviceconfiguration** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet**, och **azure-iot-enhet – mqtt**paketet:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Med hjälp av en textredigerare, skapa en ny **SimulateDeviceConfiguration.js** filen i hello **simulatedeviceconfiguration** mapp.
4. Lägg till följande kod toohello hello **SimulateDeviceConfiguration.js** filen och ersätta hello **{enheten anslutningssträngen}** med hello enheten anslutningssträngen som du kopierade när du Skapa hello **myDeviceId** enhets-ID:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    Hej **klienten** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello enhet. Hej föregående kod när den initierar hello **klienten** objekt, hämtar hello enheten dubbla för **myDeviceId**, och bifogar en hanterare för hello uppdatering på Egenskaper. hello hanterare verifierar att det är en verklig configuration ändringsbegäran genom att jämföra hello configIds sedan anropar en metod som startar hello konfigurationsändring.
   
    Observera att för hello dig ut av enkelhet hello föregående kod använder en hårdkodad standard för hello inledande konfiguration. En verklig app skulle förmodligen att läsa in konfigurationen från en lokal lagring.
   
   > [!IMPORTANT]
   > Önskade egenskapen Ändringshändelser släpps alltid en gång vid enhetsanslutning, se till att det finns en verklig ändring i hello toocheck önskade egenskaper innan du utför några åtgärder.
   > 
   > 
5. Lägg till följande metoder innan hello hello `client.open()` anrop:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    Hej **initConfigChange** metoden uppdateringar rapporteras egenskaper på hello lokala dubbla enhetsobjekt med hello konfigurationsbegäran för uppdatering och anger hello status för**väntande**, och sedan uppdateringar hello enhet dubbla på hello-tjänsten. När en uppdatering hello enheten dubbla är den simulerar en tidskrävande process som avslutar hello körning av **completeConfigChange**. Den här metoden uppdateringar hello lokala enhet dubbla har rapporterats egenskaper som anger hello status för**lyckade** och ta bort hello **pendingConfig** objekt. Sedan uppdateras hello enheten dubbla på hello-tjänsten.
   
    Observera toosave bandbredd som rapporteras egenskaper uppdateras genom att ange endast hello egenskaper toobe ändrade (med namnet **korrigering** i hello ovan kod), i stället för att ersätta hello hela dokumentet.
   
   > [!NOTE]
   > Den här självstudiekursen simulerar inte någon funktion för samtidiga konfigurationsuppdateringar. Vissa processer för uppdatering av konfiguration kan vara kan tooaccommodate ändringar av konfigurationen för målet medan hello uppdateringen körs, andra kan ha tooqueue dem och andra kan avvisa dem med ett feltillstånd. Se till att tooconsider hello önskat beteende för din specifika konfigurationsprocessen och lägga till lämpliga hello-logik innan du påbörjar hello konfigurationsändring.
   > 
   > 
6. Kör hello enhetsapp:
   
        node SimulateDeviceConfiguration.js
   
    Du bör se hello-meddelande `retrieved device twin`. Behåll hello appen körs.

## <a name="create-hello-service-app"></a>Skapa hello service-appen
I det här avsnittet skapar du en Node.js-konsolprogram som uppdateringar hello *önskade egenskaper* på hello enheten dubbla som är associerade med **myDeviceId** med ett nytt telemetri configuration-objekt. Sedan frågar hello enheten twins lagras i hello IoT-hubb och visar hello skillnaden mellan hello önskad och rapporterade konfigurationer av hello enhet.

1. Skapa en ny tom mapp som kallas **setdesiredandqueryapp**. I hello **setdesiredandqueryapp** mapp, skapa en ny package.json-fil med hello följande kommando vid en kommandotolk. Acceptera alla standardvärden för hello:
   
    ```
    npm init
    ```
2. Vid en kommandotolk i hello **setdesiredandqueryapp** mapp, kör följande kommando tooinstall hello hello **azure iothub** paketet:
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. Med hjälp av en textredigerare, skapa en ny **SetDesiredAndQuery.js** filen i hello **addtagsandqueryapp** mapp.
4. Lägg till följande kod toohello hello **SetDesiredAndQuery.js** filen och ersätta hello **{anslutningssträngen för iot-hubb}** med hello IoT-hubb anslutningssträngen som du kopierade när du skapade din hubb :
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    Hej **registret** objektet innehåller alla hello metoder krävs toointeract med enheten twins från hello-tjänsten. Hej föregående kod när den initierar hello **registret** objekt, hämtar hello enheten dubbla för **myDeviceId**, och uppdaterar dess egenskaper med ett nytt telemetri configuration-objekt. Därefter anropar hello **queryTwins** fungerar händelse 10 sekunder.

    > [!IMPORTANT]
    > Det här programmet frågar IoT-hubb var 10: e sekund som illustration. Använd frågar toogenerate användarinriktad rapporter över flera enheter, och inte toodetect ändringar. Om din lösning kräver meddelanden i realtid av enhetshändelser, använder [dubbla meddelanden][lnk-twin-notifications].
    > 
    >.

1. Lägg till följande kod precis innan hello hello `registry.getDeviceTwin()` anrop tooimplement hello **queryTwins** funktionen:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed toofetch hello results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    hello föregående kod frågor hello enheten twins lagras i hello IoT-hubb och utskrifter hello önskad och rapporterade telemetri konfigurationer. Se toohello [IoT-hubb frågespråket] [ lnk-query] toolearn hur toogenerate omfattande rapporter på alla enheter.
2. Med **SimulateDeviceConfiguration.js** körs, kör hello program med:
   
        node SetDesiredAndQuery.js 5m
   
    Du bör se hello rapporterade konfigurationsändring från **lyckade** för**väntande** för**lyckade** igen med hello ny aktiv frekvens som skickar fem minuter i stället för 24 timmar.
   
   > [!IMPORTANT]
   > Det finns en fördröjning på upp tooa minut mellan hello enheten rapporten igen och hello frågeresultatet. Detta är tooenable hello frågan infrastruktur toowork i hög skala. tooretrieve konsekvent vyer för en enskild enhet dubbla använda hello **getDeviceTwin** metod i hello **registret** klass.
   > 
   > 

## <a name="next-steps"></a>Nästa steg
I kursen får du ange en önskad konfiguration som *önskade egenskaper* från en backend-app och skrev en simulerad enhet app toodetect som ändras och simulera en flera steg uppdateringsprocess reporting statusen  *rapporterade egenskaper* toohello enheten dubbla.

Använd hello följande resurser toolearn hur till:

* Skicka telemetri från enheter med hello [Kom igång med IoT-hubb] [ lnk-iothub-getstarted] självstudiekursen
* schemalägga eller utföra åtgärder på stora mängder enheter finns hello [schema och broadcast jobb] [ lnk-schedule-jobs] kursen.
* Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app), med hello [metoder från direkt] [ lnk-methods-tutorial] kursen.

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
