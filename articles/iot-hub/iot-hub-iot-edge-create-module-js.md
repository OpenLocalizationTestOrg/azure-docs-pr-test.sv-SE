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
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a><span data-ttu-id="33442-103">Skapa en Azure IoT-Edge-modul med Node.js</span><span class="sxs-lookup"><span data-stu-id="33442-103">Create an Azure IoT Edge Module with Node.js</span></span>

<span data-ttu-id="33442-104">Den här kursen visar hur toocreate en modul för Azure IoT gränsen i JS.</span><span class="sxs-lookup"><span data-stu-id="33442-104">This tutorial showcases how toocreate a module for Azure IoT Edge in JS.</span></span>

<span data-ttu-id="33442-105">I den här självstudiekursen kommer vi att gå igenom miljökonfiguration och hur toowrite en [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data konverteraren modul med hello senaste Azure IoT kant NPM-paket.</span><span class="sxs-lookup"><span data-stu-id="33442-105">In this tutorial, we walk through environment setup and how toowrite a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using hello latest Azure IoT Edge NPM packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33442-106">Krav</span><span class="sxs-lookup"><span data-stu-id="33442-106">Prerequisites</span></span>

<span data-ttu-id="33442-107">I det här avsnittet kan du ställa in din miljö för utveckling av IoT kant-modulen.</span><span class="sxs-lookup"><span data-stu-id="33442-107">In this section, you set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="33442-108">Det gäller tooboth *64-bitars Windows* och *64-bitars Linux (Ubuntu 14 +)* operativsystem.</span><span class="sxs-lookup"><span data-stu-id="33442-108">It applies tooboth *64-bit Windows* and *64-bit Linux (Ubuntu 14+)* operating systems.</span></span>

<span data-ttu-id="33442-109">hello följande programvara krävs:</span><span class="sxs-lookup"><span data-stu-id="33442-109">hello following software is required:</span></span>
* <span data-ttu-id="33442-110">[Git klienten](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="33442-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="33442-111">[Noden LTS](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="33442-111">[Node LTS](https://nodejs.org).</span></span>
* <span data-ttu-id="33442-112">`npm install -g yo`.</span><span class="sxs-lookup"><span data-stu-id="33442-112">`npm install -g yo`.</span></span>
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a><span data-ttu-id="33442-113">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="33442-113">Architecture</span></span>

<span data-ttu-id="33442-114">hello Azure IoT kant plattform kraftigt antar hello [Von Neumann arkitektur](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="33442-114">hello Azure IoT Edge platform heavily adopts hello [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="33442-115">Vilket innebär att hela hello Azure IoT kant arkitekturen är ett system som bearbetar indata och utdata; och att varje enskild modul är en liten input-output-undersystemet.</span><span class="sxs-lookup"><span data-stu-id="33442-115">Which means that hello entire Azure IoT Edge architecture is a system that processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="33442-116">I den här självstudiekursen kommer introducerar vi hello följande två moduler:</span><span class="sxs-lookup"><span data-stu-id="33442-116">In this tutorial, we introduce hello following two modules:</span></span>

1. <span data-ttu-id="33442-117">En modul som tar emot en simulerad [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signalerar och konverterar den till en formaterad [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.</span><span class="sxs-lookup"><span data-stu-id="33442-117">A module that receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="33442-118">En modul som skriver ut hello emot [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.</span><span class="sxs-lookup"><span data-stu-id="33442-118">A module that prints hello received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="33442-119">hello visar följande bild hello vanliga slutet tooend dataflöde för det här projektet:</span><span class="sxs-lookup"><span data-stu-id="33442-119">hello following image displays hello typical end tooend dataflow for this project:</span></span>

<span data-ttu-id="33442-120">![Dataflöde mellan tre moduler](media/iot-hub-iot-edge-create-module/dataflow.png "indata: simulerade TIVERA modulen. Processor: Konverterare modulen. Utdata: Skrivare modul")</span><span class="sxs-lookup"><span data-stu-id="33442-120">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="set-up-hello-environment"></a><span data-ttu-id="33442-121">Konfigurera hello-miljö</span><span class="sxs-lookup"><span data-stu-id="33442-121">Set up hello environment</span></span>
<span data-ttu-id="33442-122">Nedan visar vi hur tooquickly du konfigurerar miljön toostart toowrite din första TIVERA konverteraren modulen med JS.</span><span class="sxs-lookup"><span data-stu-id="33442-122">Below we show you how tooquickly set up environment toostart toowrite your first BLE converter module with JS.</span></span>

### <a name="create-module-project"></a><span data-ttu-id="33442-123">Skapa modul-projekt</span><span class="sxs-lookup"><span data-stu-id="33442-123">Create module project</span></span>
1. <span data-ttu-id="33442-124">Öppna ett kommandoradsfönster, kör `yo az-iot-gw-module`.</span><span class="sxs-lookup"><span data-stu-id="33442-124">Open a command-line window, run `yo az-iot-gw-module`.</span></span>
2. <span data-ttu-id="33442-125">Följ hello stegen på skärmen hello toofinish hello initiering av modul-projekt.</span><span class="sxs-lookup"><span data-stu-id="33442-125">Follow hello steps on hello screen toofinish hello initialization of your module project.</span></span>

### <a name="project-structure"></a><span data-ttu-id="33442-126">Projektstruktur</span><span class="sxs-lookup"><span data-stu-id="33442-126">Project structure</span></span>
<span data-ttu-id="33442-127">Ett JS modulen Projekt består av hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="33442-127">A JS module project consists of hello following components:</span></span>

<span data-ttu-id="33442-128">`modules`-hello anpassade JS modulen källfiler.</span><span class="sxs-lookup"><span data-stu-id="33442-128">`modules` - hello customized JS module source files.</span></span> <span data-ttu-id="33442-129">Ersätt hello standard `sensor.js` och `printer.js` med modulen filerna.</span><span class="sxs-lookup"><span data-stu-id="33442-129">Replace hello default `sensor.js` and `printer.js` with your own module files.</span></span>

<span data-ttu-id="33442-130">`app.js`-hello post fil toostart hello Edge-instans.</span><span class="sxs-lookup"><span data-stu-id="33442-130">`app.js` - hello entry file toostart hello Edge instance.</span></span>

<span data-ttu-id="33442-131">`gw.config.json`-hello configuration file toocustomize hello moduler toobe läses in av kant.</span><span class="sxs-lookup"><span data-stu-id="33442-131">`gw.config.json` - hello configuration file toocustomize hello modules toobe loaded by Edge.</span></span>

<span data-ttu-id="33442-132">`package.json`-hello metadatainformation för modulen projekt.</span><span class="sxs-lookup"><span data-stu-id="33442-132">`package.json` - hello metadata information for module project.</span></span>

<span data-ttu-id="33442-133">`README.md`-hello grundläggande dokumentation för modulen projekt.</span><span class="sxs-lookup"><span data-stu-id="33442-133">`README.md` - hello basic documentation for module project.</span></span>


### <a name="package-file"></a><span data-ttu-id="33442-134">Paketfilen</span><span class="sxs-lookup"><span data-stu-id="33442-134">Package file</span></span>

<span data-ttu-id="33442-135">Detta `package.json` deklarerar alla hello metadata-information som krävs av en modul-projekt som innehåller hello namn, version, post, skript, runtime och utveckling beroenden.</span><span class="sxs-lookup"><span data-stu-id="33442-135">This `package.json` declares all hello metadata information needed by a module project that includes hello name, version, entry, scripts, runtime, and development dependencies.</span></span>

<span data-ttu-id="33442-136">Följande kodfragment visas hur tooconfigure TIVERA konverterare exempel projekt.</span><span class="sxs-lookup"><span data-stu-id="33442-136">Following code snippet shows how tooconfigure for BLE converter sample project.</span></span>
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


### <a name="entry-file"></a><span data-ttu-id="33442-137">Den första filen</span><span class="sxs-lookup"><span data-stu-id="33442-137">Entry file</span></span>
<span data-ttu-id="33442-138">Hej `app.js` definierar hello sätt tooinitialize hello edge-instans.</span><span class="sxs-lookup"><span data-stu-id="33442-138">hello `app.js` defines hello way tooinitialize hello edge instance.</span></span> <span data-ttu-id="33442-139">Här behöver vi inte toomake ändringar.</span><span class="sxs-lookup"><span data-stu-id="33442-139">Here we don't need toomake any change.</span></span>

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

### <a name="interface-of-module"></a><span data-ttu-id="33442-140">Gränssnittet för modulen</span><span class="sxs-lookup"><span data-stu-id="33442-140">Interface of module</span></span>
<span data-ttu-id="33442-141">Du kan hantera en Azure IoT kant-modul som en data-processor vars uppgift är att: indata, behandlas och resultat.</span><span class="sxs-lookup"><span data-stu-id="33442-141">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="33442-142">hello indata kan inte data från maskinvara (till exempel en rörelsedetektor), ett meddelande från andra moduler eller något annat (till exempel ett slumptal som genererats med jämna mellanrum av en timer).</span><span class="sxs-lookup"><span data-stu-id="33442-142">hello input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="33442-143">hello utdata är liknande toohello indata, det kan utlösa beteendet för maskinvara (till exempel hello blinka Indikator), en meddelandet tooother moduler eller något annat (till exempel utskrift toohello console).</span><span class="sxs-lookup"><span data-stu-id="33442-143">hello output is similar toohello input, it could trigger hardware behavior (like hello blinking LED), a message tooother modules, or anything else (like printing toohello console).</span></span>

<span data-ttu-id="33442-144">Moduler som kommunicerar med varandra med hjälp av `message` objekt.</span><span class="sxs-lookup"><span data-stu-id="33442-144">Modules communicate with each other using `message` object.</span></span> <span data-ttu-id="33442-145">Hej **innehåll** av en `message` är en bytematris som kan som representerar alla typer av data som du vill.</span><span class="sxs-lookup"><span data-stu-id="33442-145">hello **content** of a `message` is a byte array that is capable of representing any kind of data you like.</span></span> <span data-ttu-id="33442-146">**Egenskaper för** är också tillgängliga i hello `message` och är helt enkelt en string-string-mappning.</span><span class="sxs-lookup"><span data-stu-id="33442-146">**Properties** are also available in hello `message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="33442-147">Du kan tänka dig **egenskaper** som hello rubriker i en HTTP-begäran eller hello metadata för en fil.</span><span class="sxs-lookup"><span data-stu-id="33442-147">You may think of **properties** as hello headers in an HTTP request, or hello metadata of a file.</span></span>

<span data-ttu-id="33442-148">I ordning toodevelop en kant för Azure IoT-modul i JS, behöver du toocreate ett nytt Modulobjekt som implementerar metoderna hello krävs `receive()`.</span><span class="sxs-lookup"><span data-stu-id="33442-148">In order toodevelop an Azure IoT Edge module in JS, you need toocreate a new module object that implements hello required methods `receive()`.</span></span> <span data-ttu-id="33442-149">Du kan nu även välja tooimplement hello valfria `create()` eller `start()`, eller `destroy()` samt metoder.</span><span class="sxs-lookup"><span data-stu-id="33442-149">At this point, you may also choose tooimplement hello optional `create()` or `start()`, or `destroy()` methods as well.</span></span> <span data-ttu-id="33442-150">hello följande kodfragment som visar du hello scaffold-teknik för JS module-objektet.</span><span class="sxs-lookup"><span data-stu-id="33442-150">hello following code snippet shows you hello scaffolding of JS module object.</span></span>

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

### <a name="converter-module"></a><span data-ttu-id="33442-151">Konverteraren modul</span><span class="sxs-lookup"><span data-stu-id="33442-151">Converter module</span></span>
| <span data-ttu-id="33442-152">Indata</span><span class="sxs-lookup"><span data-stu-id="33442-152">Input</span></span>                    | <span data-ttu-id="33442-153">Processor</span><span class="sxs-lookup"><span data-stu-id="33442-153">Processor</span></span>                              | <span data-ttu-id="33442-154">Resultat</span><span class="sxs-lookup"><span data-stu-id="33442-154">Output</span></span>                 | <span data-ttu-id="33442-155">Källfilen</span><span class="sxs-lookup"><span data-stu-id="33442-155">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="33442-156">Meddelande för temperatur-data</span><span class="sxs-lookup"><span data-stu-id="33442-156">Temperature data message</span></span> | <span data-ttu-id="33442-157">Analysera och skapa ett nytt JSON-meddelande</span><span class="sxs-lookup"><span data-stu-id="33442-157">Parse and construct a new JSON message</span></span> | <span data-ttu-id="33442-158">Strukturen JSON-meddelande</span><span class="sxs-lookup"><span data-stu-id="33442-158">Structure JSON message</span></span> | `converter.js` |

<span data-ttu-id="33442-159">Denna modul är en typisk Azure IoT kant-modul.</span><span class="sxs-lookup"><span data-stu-id="33442-159">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="33442-160">Den godkänner temperatur meddelanden från andra moduler (maskinvara modulen, eller i det här fallet våra simulerade Bell-modulen); och normaliserar temperatur hälsningsmeddelande i tooa strukturerad JSON-meddelande (inklusive bifoga hello meddelande-ID, hello-egenskapen för om vi behöver tootrigger hello temperatur aviseringen och så vidare).</span><span class="sxs-lookup"><span data-stu-id="33442-160">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes hello temperature message in tooa structured JSON message (including appending hello message ID, setting hello property of whether we need tootrigger hello temperature alert, and so on).</span></span>

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

### <a name="printer-module"></a><span data-ttu-id="33442-161">Modul för skrivare</span><span class="sxs-lookup"><span data-stu-id="33442-161">Printer module</span></span>
| <span data-ttu-id="33442-162">Indata</span><span class="sxs-lookup"><span data-stu-id="33442-162">Input</span></span>                          | <span data-ttu-id="33442-163">Processor</span><span class="sxs-lookup"><span data-stu-id="33442-163">Processor</span></span> | <span data-ttu-id="33442-164">Resultat</span><span class="sxs-lookup"><span data-stu-id="33442-164">Output</span></span>                     | <span data-ttu-id="33442-165">Källfilen</span><span class="sxs-lookup"><span data-stu-id="33442-165">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="33442-166">Alla meddelanden från andra moduler</span><span class="sxs-lookup"><span data-stu-id="33442-166">Any message from other modules</span></span> | <span data-ttu-id="33442-167">Saknas</span><span class="sxs-lookup"><span data-stu-id="33442-167">N/A</span></span>       | <span data-ttu-id="33442-168">Logga tooconsole för hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="33442-168">Log hello message tooconsole</span></span> | `printer.js` |

<span data-ttu-id="33442-169">Denna modul är enkla, självförklarande, som matar ut hello mottagna meddelanden (egenskap, innehåll) toohello terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="33442-169">This module is simple, self-explanatory, which outputs hello received messages(property, content) toohello terminal window.</span></span>

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a><span data-ttu-id="33442-170">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="33442-170">Configuration</span></span>
<span data-ttu-id="33442-171">hello sista steget innan du kör hello moduler är tooconfigure hello Azure IoT kant och tooestablish hello anslutningar mellan moduler.</span><span class="sxs-lookup"><span data-stu-id="33442-171">hello final step before running hello modules is tooconfigure hello Azure IoT Edge and tooestablish hello connections between modules.</span></span>

<span data-ttu-id="33442-172">Vi måste först toodeclare vår `node` inläsaren (eftersom Azure IoT kant stöder inläsare med olika språk) som kan refereras till av dess `name` i hello avsnitt efteråt.</span><span class="sxs-lookup"><span data-stu-id="33442-172">First we need toodeclare our `node` loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in hello sections afterward.</span></span>

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

<span data-ttu-id="33442-173">När vi har deklarerat våra inläsare måste vi också toodeclare samt våra moduler.</span><span class="sxs-lookup"><span data-stu-id="33442-173">Once we have declared our loaders, we also need toodeclare our modules as well.</span></span> <span data-ttu-id="33442-174">Liknande toodeclaring hello inläsare de kan också refereras av deras `name` attribut.</span><span class="sxs-lookup"><span data-stu-id="33442-174">Similar toodeclaring hello loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="33442-175">När du deklarerar en modul måste toospecify hello inläsaren ska det använda (som ska vara hello en vi definierade innan) och hello startpunkten (ska vara hello normaliserade klassnamnet för våra modulen) för varje modul.</span><span class="sxs-lookup"><span data-stu-id="33442-175">When declaring a module, we need toospecify hello loader it should use (which should be hello one we defined before) and hello entry-point (should be hello normalized class name of our module) for each module.</span></span> <span data-ttu-id="33442-176">Hej `simulated_device` modul är en inbyggd modul som ingår i hello Azure IoT kant core runtime-paketet.</span><span class="sxs-lookup"><span data-stu-id="33442-176">hello `simulated_device` module is a native module that is included in hello Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="33442-177">Inkludera `args` i hello JSON-filen även om den `null`.</span><span class="sxs-lookup"><span data-stu-id="33442-177">Include `args` in hello JSON file even if it is `null`.</span></span>

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

<span data-ttu-id="33442-178">Hello slutet av hello konfiguration upprätta vi hello anslutningar.</span><span class="sxs-lookup"><span data-stu-id="33442-178">At hello end of hello configuration, we establish hello connections.</span></span> <span data-ttu-id="33442-179">Varje anslutning uttrycks av `source` och `sink`.</span><span class="sxs-lookup"><span data-stu-id="33442-179">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="33442-180">De bör både referera en fördefinierad modul.</span><span class="sxs-lookup"><span data-stu-id="33442-180">They should both reference a pre-defined module.</span></span> <span data-ttu-id="33442-181">hälsningsmeddelande för utdata av `source` modulen vidarebefordras toohello indata för `sink` modul.</span><span class="sxs-lookup"><span data-stu-id="33442-181">hello output message of `source` module is forwarded toohello input of `sink` module.</span></span>

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

## <a name="running-hello-modules"></a><span data-ttu-id="33442-182">Kör hello-moduler</span><span class="sxs-lookup"><span data-stu-id="33442-182">Running hello modules</span></span>
1. `npm install`
2. `npm start`

<span data-ttu-id="33442-183">Om du vill tooterminate hello program, trycker du på `<Enter>` nyckel.</span><span class="sxs-lookup"><span data-stu-id="33442-183">If you want tooterminate hello application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33442-184">Det rekommenderas inte toouse Ctrl + C tooterminate hello IoT kant program.</span><span class="sxs-lookup"><span data-stu-id="33442-184">It is not recommended toouse Ctrl + C tooterminate hello IoT Edge application.</span></span> <span data-ttu-id="33442-185">Som det här sättet kan orsaka hello processen tooterminate onormalt.</span><span class="sxs-lookup"><span data-stu-id="33442-185">As this way may cause hello process tooterminate abnormally.</span></span>
