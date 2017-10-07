---
title: aaaCreate en Azure IoT kant modul med Node.js | Microsoft Docs
description: "Den här kursen visar hur toowrite en TIVERA data konverteraren modulen med hjälp av hello senaste Azure IoT kant NPM-paket och Yeoman generator."
services: iot-hub
author: sushi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: article
ms.date: 06/28/2017
ms.author: sushi
ms.openlocfilehash: d3e696b5a310377ffb8e99998ff0714bf7c0bb41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a>Skapa en Azure IoT-Edge-modul med Node.js

Den här kursen visar hur toocreate en modul för Azure IoT gränsen i JS.

I den här självstudiekursen kommer vi att gå igenom miljökonfiguration och hur toowrite en [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data konverteraren modul med hello senaste Azure IoT kant NPM-paket.

## <a name="prerequisites"></a>Krav

I det här avsnittet kan du ställa in din miljö för utveckling av IoT kant-modulen. Det gäller tooboth *64-bitars Windows* och *64-bitars Linux (Ubuntu 14 +)* operativsystem.

hello följande programvara krävs:
* [Git klienten](https://git-scm.com/downloads).
* [Noden LTS](https://nodejs.org).
* `npm install -g yo`.
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a>Arkitektur

hello Azure IoT kant plattform kraftigt antar hello [Von Neumann arkitektur](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Vilket innebär att hela hello Azure IoT kant arkitekturen är ett system som bearbetar indata och utdata; och att varje enskild modul är en liten input-output-undersystemet. I den här självstudiekursen kommer introducerar vi hello följande två moduler:

1. En modul som tar emot en simulerad [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signalerar och konverterar den till en formaterad [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.
2. En modul som skriver ut hello emot [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.

hello visar följande bild hello vanliga slutet tooend dataflöde för det här projektet:

![Dataflöde mellan tre moduler](media/iot-hub-iot-edge-create-module/dataflow.png "indata: simulerade TIVERA modulen. Processor: Konverterare modulen. Utdata: Skrivare modul")

## <a name="set-up-hello-environment"></a>Konfigurera hello-miljö
Nedan visar vi hur tooquickly du konfigurerar miljön toostart toowrite din första TIVERA konverteraren modulen med JS.

### <a name="create-module-project"></a>Skapa modul-projekt
1. Öppna ett kommandoradsfönster, kör `yo az-iot-gw-module`.
2. Följ hello stegen på skärmen hello toofinish hello initiering av modul-projekt.

### <a name="project-structure"></a>Projektstruktur
Ett JS modulen Projekt består av hello följande komponenter:

`modules`-hello anpassade JS modulen källfiler. Ersätt hello standard `sensor.js` och `printer.js` med modulen filerna.

`app.js`-hello post fil toostart hello Edge-instans.

`gw.config.json`-hello configuration file toocustomize hello moduler toobe läses in av kant.

`package.json`-hello metadatainformation för modulen projekt.

`README.md`-hello grundläggande dokumentation för modulen projekt.


### <a name="package-file"></a>Paketfilen

Detta `package.json` deklarerar alla hello metadata-information som krävs av en modul-projekt som innehåller hello namn, version, post, skript, runtime och utveckling beroenden.

Följande kodfragment visas hur tooconfigure TIVERA konverterare exempel projekt.
```json
{
  "name": "converter",
  "version": "1.0.0",
  "description": "BLE data converter sample for Azure IoT Edge.",
  "repository": {
    "type": "git",
    "url": "https://github.com/Azure-Samples/iot-edge-samples"
  },
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Microsoft Corporation",
  "license": "MIT",
  "dependencies": {
  },
  "devDependencies": {
    "azure-iot-gateway": "~1.1.3"
  }
}
```


### <a name="entry-file"></a>Den första filen
Hej `app.js` definierar hello sätt tooinitialize hello edge-instans. Här behöver vi inte toomake ändringar.

```javascript
(function() {
  'use strict';

  const Gateway = require('azure-iot-gateway');
  let config_path = './gw.config.json';

  // node app.js
  if (process.argv.length < 2) {
    throw 'Calling pattern should be node app.js.';
  }

  const gw = new Gateway(config_path);
  gw.run();
})();
```

### <a name="interface-of-module"></a>Gränssnittet för modulen
Du kan hantera en Azure IoT kant-modul som en data-processor vars uppgift är att: indata, behandlas och resultat.

hello indata kan inte data från maskinvara (till exempel en rörelsedetektor), ett meddelande från andra moduler eller något annat (till exempel ett slumptal som genererats med jämna mellanrum av en timer).

hello utdata är liknande toohello indata, det kan utlösa beteendet för maskinvara (till exempel hello blinka Indikator), en meddelandet tooother moduler eller något annat (till exempel utskrift toohello console).

Moduler som kommunicerar med varandra med hjälp av `message` objekt. Hej **innehåll** av en `message` är en bytematris som kan som representerar alla typer av data som du vill. **Egenskaper för** är också tillgängliga i hello `message` och är helt enkelt en string-string-mappning. Du kan tänka dig **egenskaper** som hello rubriker i en HTTP-begäran eller hello metadata för en fil.

I ordning toodevelop en kant för Azure IoT-modul i JS, behöver du toocreate ett nytt Modulobjekt som implementerar metoderna hello krävs `receive()`. Du kan nu även välja tooimplement hello valfria `create()` eller `start()`, eller `destroy()` samt metoder. hello följande kodfragment som visar du hello scaffold-teknik för JS module-objektet.

```javascript
'use strict';

module.exports = {
  broker: null,
  configuration: null,

  create: function (broker, configuration) {
    // Default implementation.
    this.broker = broker;
    this.configuration = configuration;

    return true;
  },

  start: function () {
    // Produce
  },

  receive: function (message) {
    // Consume
  },

  destroy: function () {
  }
};
```

### <a name="converter-module"></a>Konverteraren modul
| Indata                    | Processor                              | Resultat                 | Källfilen            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Meddelande för temperatur-data | Analysera och skapa ett nytt JSON-meddelande | Strukturen JSON-meddelande | `converter.js` |

Denna modul är en typisk Azure IoT kant-modul. Den godkänner temperatur meddelanden från andra moduler (maskinvara modulen, eller i det här fallet våra simulerade Bell-modulen); och normaliserar temperatur hälsningsmeddelande i tooa strukturerad JSON-meddelande (inklusive bifoga hello meddelande-ID, hello-egenskapen för om vi behöver tootrigger hello temperatur aviseringen och så vidare).

```javascript
receive: function (message) {
  // Initialize hello messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read hello content and properties objects from message.
  let rawContent = JSON.parse(Buffer.from(message.content).toString('utf8'));
  let rawProperties = message.properties;

  // Generate new properties object.
  let newProperties = {
    source: rawProperties.source,
    macAddress: rawProperties.macAddress,
    temperatureAlert: rawContent.temperature > 30 ? 'true' : 'false'
  };

  // Generate new content object.
  let newContent = {
    deviceId: 'Intel NUC Gateway',
    messageId: ++global.messageCount,
    temperature: rawContent.temperature
  };

  // Publish hello new message toobroker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a>Modul för skrivare
| Indata                          | Processor | Resultat                     | Källfilen          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Alla meddelanden från andra moduler | Saknas       | Logga tooconsole för hello-meddelande | `printer.js` |

Denna modul är enkla, självförklarande, som matar ut hello mottagna meddelanden (egenskap, innehåll) toohello terminalfönster.

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a>Konfiguration
hello sista steget innan du kör hello moduler är tooconfigure hello Azure IoT kant och tooestablish hello anslutningar mellan moduler.

Vi måste först toodeclare vår `node` inläsaren (eftersom Azure IoT kant stöder inläsare med olika språk) som kan refereras till av dess `name` i hello avsnitt efteråt.

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

När vi har deklarerat våra inläsare måste vi också toodeclare samt våra moduler. Liknande toodeclaring hello inläsare de kan också refereras av deras `name` attribut. När du deklarerar en modul måste toospecify hello inläsaren ska det använda (som ska vara hello en vi definierade innan) och hello startpunkten (ska vara hello normaliserade klassnamnet för våra modulen) för varje modul. Hej `simulated_device` modul är en inbyggd modul som ingår i hello Azure IoT kant core runtime-paketet. Inkludera `args` i hello JSON-filen även om den `null`.

```json
"modules": [
  {
    "name": "simulated_device",
    "loader": {
      "name": "native",
      "entrypoint": {
        "module.path": "simulated_device"
      }
    },
    "args": {
      "macAddress": "01:02:03:03:02:01",
      "messagePeriod": 500
    }
  },
  {
    "name": "converter",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/converter.js"
      }
    },
    "args": null
  },
  {
    "name": "printer",
    "loader": {
      "name": "node",
      "entrypoint": {
        "main.path": "modules/printer.js"
      }
    },
    "args": null
  }
]
```

Hello slutet av hello konfiguration upprätta vi hello anslutningar. Varje anslutning uttrycks av `source` och `sink`. De bör både referera en fördefinierad modul. hälsningsmeddelande för utdata av `source` modulen vidarebefordras toohello indata för `sink` modul.

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "printer"
  }
]
```

## <a name="running-hello-modules"></a>Kör hello-moduler
1. `npm install`
2. `npm start`

Om du vill tooterminate hello program, trycker du på `<Enter>` nyckel.

> [!IMPORTANT]
> Det rekommenderas inte toouse Ctrl + C tooterminate hello IoT kant program. Som det här sättet kan orsaka hello processen tooterminate onormalt.
