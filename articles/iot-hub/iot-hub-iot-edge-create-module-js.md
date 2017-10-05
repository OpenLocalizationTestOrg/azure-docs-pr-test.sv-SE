---
title: Skapa en Azure IoT-Edge-modul med Node.js | Microsoft Docs
description: "Den här kursen visar hur du skriver en TIVERA data konverteraren modul med de senaste Azure IoT kant NPM-paketen och Yeoman generator."
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
ms.openlocfilehash: ba466f47e157d805600c41fa3d84ed5a0363969c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-azure-iot-edge-module-with-nodejs"></a><span data-ttu-id="5af3f-103">Skapa en Azure IoT-Edge-modul med Node.js</span><span class="sxs-lookup"><span data-stu-id="5af3f-103">Create an Azure IoT Edge Module with Node.js</span></span>

<span data-ttu-id="5af3f-104">Den här kursen visar hur du skapar en modul för Azure IoT gränsen i JS.</span><span class="sxs-lookup"><span data-stu-id="5af3f-104">This tutorial showcases how to create a module for Azure IoT Edge in JS.</span></span>

<span data-ttu-id="5af3f-105">I den här självstudiekursen kommer vi att gå igenom miljökonfiguration och hur du skriver en [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data konverteraren modul med de senaste Azure IoT kant NPM-paket.</span><span class="sxs-lookup"><span data-stu-id="5af3f-105">In this tutorial, we walk through environment setup and how to write a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using the latest Azure IoT Edge NPM packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5af3f-106">Krav</span><span class="sxs-lookup"><span data-stu-id="5af3f-106">Prerequisites</span></span>

<span data-ttu-id="5af3f-107">I det här avsnittet kan du ställa in din miljö för utveckling av IoT kant-modulen.</span><span class="sxs-lookup"><span data-stu-id="5af3f-107">In this section, you set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="5af3f-108">Det gäller både *64-bitars Windows* och *64-bitars Linux (Ubuntu 14 +)* operativsystem.</span><span class="sxs-lookup"><span data-stu-id="5af3f-108">It applies to both *64-bit Windows* and *64-bit Linux (Ubuntu 14+)* operating systems.</span></span>

<span data-ttu-id="5af3f-109">Följande programvara krävs:</span><span class="sxs-lookup"><span data-stu-id="5af3f-109">The following software is required:</span></span>
* <span data-ttu-id="5af3f-110">[Git klienten](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="5af3f-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="5af3f-111">[Noden LTS](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="5af3f-111">[Node LTS](https://nodejs.org).</span></span>
* <span data-ttu-id="5af3f-112">`npm install -g yo`.</span><span class="sxs-lookup"><span data-stu-id="5af3f-112">`npm install -g yo`.</span></span>
* `npm install -g generator-az-iot-gw-module`

## <a name="architecture"></a><span data-ttu-id="5af3f-113">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="5af3f-113">Architecture</span></span>

<span data-ttu-id="5af3f-114">Azure IoT kant-plattformen kraftigt antar den [Von Neumann arkitektur](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="5af3f-114">The Azure IoT Edge platform heavily adopts the [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="5af3f-115">Vilket innebär att hela kant för Azure IoT-arkitekturen är ett system som bearbetar indata och utdata; och att varje enskild modul är en liten input-output-undersystemet.</span><span class="sxs-lookup"><span data-stu-id="5af3f-115">Which means that the entire Azure IoT Edge architecture is a system that processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="5af3f-116">I den här självstudiekursen introducerar vi följande två moduler:</span><span class="sxs-lookup"><span data-stu-id="5af3f-116">In this tutorial, we introduce the following two modules:</span></span>

1. <span data-ttu-id="5af3f-117">En modul som tar emot en simulerad [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signalerar och konverterar den till en formaterad [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.</span><span class="sxs-lookup"><span data-stu-id="5af3f-117">A module that receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="5af3f-118">En modul som skriver ut den mottagna [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.</span><span class="sxs-lookup"><span data-stu-id="5af3f-118">A module that prints the received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="5af3f-119">Följande bild visar den vanliga slutpunkt till slutpunkt-dataflöde för det här projektet:</span><span class="sxs-lookup"><span data-stu-id="5af3f-119">The following image displays the typical end to end dataflow for this project:</span></span>

<span data-ttu-id="5af3f-120">![Dataflöde mellan tre moduler](media/iot-hub-iot-edge-create-module/dataflow.png "indata: simulerade TIVERA modulen. Processor: Konverterare modulen. Utdata: Skrivare modul")</span><span class="sxs-lookup"><span data-stu-id="5af3f-120">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="set-up-the-environment"></a><span data-ttu-id="5af3f-121">Konfigurera miljön</span><span class="sxs-lookup"><span data-stu-id="5af3f-121">Set up the environment</span></span>
<span data-ttu-id="5af3f-122">Nedan hur vi du snabbt ställa in miljön att börja skriva ditt första TIVERA konverteraren modulen med JS.</span><span class="sxs-lookup"><span data-stu-id="5af3f-122">Below we show you how to quickly set up environment to start to write your first BLE converter module with JS.</span></span>

### <a name="create-module-project"></a><span data-ttu-id="5af3f-123">Skapa modul-projekt</span><span class="sxs-lookup"><span data-stu-id="5af3f-123">Create module project</span></span>
1. <span data-ttu-id="5af3f-124">Öppna ett kommandoradsfönster, kör `yo az-iot-gw-module`.</span><span class="sxs-lookup"><span data-stu-id="5af3f-124">Open a command-line window, run `yo az-iot-gw-module`.</span></span>
2. <span data-ttu-id="5af3f-125">Följ anvisningarna på skärmen för att slutföra initieringen av projektet modulen.</span><span class="sxs-lookup"><span data-stu-id="5af3f-125">Follow the steps on the screen to finish the initialization of your module project.</span></span>

### <a name="project-structure"></a><span data-ttu-id="5af3f-126">Projektstruktur</span><span class="sxs-lookup"><span data-stu-id="5af3f-126">Project structure</span></span>
<span data-ttu-id="5af3f-127">Ett JS modulen Projekt består av följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="5af3f-127">A JS module project consists of the following components:</span></span>

<span data-ttu-id="5af3f-128">`modules`-Anpassade JS modulen källfilerna.</span><span class="sxs-lookup"><span data-stu-id="5af3f-128">`modules` - The customized JS module source files.</span></span> <span data-ttu-id="5af3f-129">Ersätt standard `sensor.js` och `printer.js` med modulen filerna.</span><span class="sxs-lookup"><span data-stu-id="5af3f-129">Replace the default `sensor.js` and `printer.js` with your own module files.</span></span>

<span data-ttu-id="5af3f-130">`app.js`-Posten filen som ska starta Edge-instans.</span><span class="sxs-lookup"><span data-stu-id="5af3f-130">`app.js` - The entry file to start the Edge instance.</span></span>

<span data-ttu-id="5af3f-131">`gw.config.json`-Konfigurationsfilen att anpassa modulerna som ska startas av kant.</span><span class="sxs-lookup"><span data-stu-id="5af3f-131">`gw.config.json` - The configuration file to customize the modules to be loaded by Edge.</span></span>

<span data-ttu-id="5af3f-132">`package.json`-Metadata-information för modulen projekt.</span><span class="sxs-lookup"><span data-stu-id="5af3f-132">`package.json` - The metadata information for module project.</span></span>

<span data-ttu-id="5af3f-133">`README.md`-Den grundläggande dokumentationen för modulen projekt.</span><span class="sxs-lookup"><span data-stu-id="5af3f-133">`README.md` - The basic documentation for module project.</span></span>


### <a name="package-file"></a><span data-ttu-id="5af3f-134">Paketfilen</span><span class="sxs-lookup"><span data-stu-id="5af3f-134">Package file</span></span>

<span data-ttu-id="5af3f-135">Detta `package.json` deklarerar alla metadata-information som krävs av en modul-projekt som innehåller namn, version, post, skript, runtime och utveckling beroenden.</span><span class="sxs-lookup"><span data-stu-id="5af3f-135">This `package.json` declares all the metadata information needed by a module project that includes the name, version, entry, scripts, runtime, and development dependencies.</span></span>

<span data-ttu-id="5af3f-136">Följande kodavsnitt visar hur du konfigurerar för TIVERA konverteraren exempelprojektet.</span><span class="sxs-lookup"><span data-stu-id="5af3f-136">Following code snippet shows how to configure for BLE converter sample project.</span></span>
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


### <a name="entry-file"></a><span data-ttu-id="5af3f-137">Den första filen</span><span class="sxs-lookup"><span data-stu-id="5af3f-137">Entry file</span></span>
<span data-ttu-id="5af3f-138">Den `app.js` definierar sätt att initiera edge-instans.</span><span class="sxs-lookup"><span data-stu-id="5af3f-138">The `app.js` defines the way to initialize the edge instance.</span></span> <span data-ttu-id="5af3f-139">Här behöver vi inte göra några ändringar.</span><span class="sxs-lookup"><span data-stu-id="5af3f-139">Here we don't need to make any change.</span></span>

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

### <a name="interface-of-module"></a><span data-ttu-id="5af3f-140">Gränssnittet för modulen</span><span class="sxs-lookup"><span data-stu-id="5af3f-140">Interface of module</span></span>
<span data-ttu-id="5af3f-141">Du kan hantera en Azure IoT kant-modul som en data-processor vars uppgift är att: indata, behandlas och resultat.</span><span class="sxs-lookup"><span data-stu-id="5af3f-141">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="5af3f-142">Indata kan vara data från maskinvara (till exempel en rörelsedetektor), ett meddelande från andra moduler eller något annat (till exempel ett slumptal som genererats med jämna mellanrum av en timer).</span><span class="sxs-lookup"><span data-stu-id="5af3f-142">The input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="5af3f-143">Utdata liknar indata, den kan utlösa beteendet för maskinvara (till exempel blinkande LED), ett meddelande till andra moduler eller något annat (till exempel utskrift i konsolen).</span><span class="sxs-lookup"><span data-stu-id="5af3f-143">The output is similar to the input, it could trigger hardware behavior (like the blinking LED), a message to other modules, or anything else (like printing to the console).</span></span>

<span data-ttu-id="5af3f-144">Moduler som kommunicerar med varandra med hjälp av `message` objekt.</span><span class="sxs-lookup"><span data-stu-id="5af3f-144">Modules communicate with each other using `message` object.</span></span> <span data-ttu-id="5af3f-145">Den **innehåll** av en `message` är en bytematris som kan som representerar alla typer av data som du vill.</span><span class="sxs-lookup"><span data-stu-id="5af3f-145">The **content** of a `message` is a byte array that is capable of representing any kind of data you like.</span></span> <span data-ttu-id="5af3f-146">**Egenskaper för** är också tillgängliga i den `message` och är helt enkelt en string-string-mappning.</span><span class="sxs-lookup"><span data-stu-id="5af3f-146">**Properties** are also available in the `message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="5af3f-147">Du kan tänka dig **egenskaper** som rubriker i en HTTP-begäran eller metadata för en fil.</span><span class="sxs-lookup"><span data-stu-id="5af3f-147">You may think of **properties** as the headers in an HTTP request, or the metadata of a file.</span></span>

<span data-ttu-id="5af3f-148">För att utveckla en kant för Azure IoT-modul i JS, måste du skapa en ny Modulobjekt som implementerar obligatoriska metoder `receive()`.</span><span class="sxs-lookup"><span data-stu-id="5af3f-148">In order to develop an Azure IoT Edge module in JS, you need to create a new module object that implements the required methods `receive()`.</span></span> <span data-ttu-id="5af3f-149">Du kan nu även välja att implementera den valfria `create()` eller `start()`, eller `destroy()` samt metoder.</span><span class="sxs-lookup"><span data-stu-id="5af3f-149">At this point, you may also choose to implement the optional `create()` or `start()`, or `destroy()` methods as well.</span></span> <span data-ttu-id="5af3f-150">Följande kodavsnitt visar JS Modulobjekt scaffold-teknik.</span><span class="sxs-lookup"><span data-stu-id="5af3f-150">The following code snippet shows you the scaffolding of JS module object.</span></span>

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

### <a name="converter-module"></a><span data-ttu-id="5af3f-151">Konverteraren modul</span><span class="sxs-lookup"><span data-stu-id="5af3f-151">Converter module</span></span>
| <span data-ttu-id="5af3f-152">Indata</span><span class="sxs-lookup"><span data-stu-id="5af3f-152">Input</span></span>                    | <span data-ttu-id="5af3f-153">Processor</span><span class="sxs-lookup"><span data-stu-id="5af3f-153">Processor</span></span>                              | <span data-ttu-id="5af3f-154">Resultat</span><span class="sxs-lookup"><span data-stu-id="5af3f-154">Output</span></span>                 | <span data-ttu-id="5af3f-155">Källfilen</span><span class="sxs-lookup"><span data-stu-id="5af3f-155">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="5af3f-156">Meddelande för temperatur-data</span><span class="sxs-lookup"><span data-stu-id="5af3f-156">Temperature data message</span></span> | <span data-ttu-id="5af3f-157">Analysera och skapa ett nytt JSON-meddelande</span><span class="sxs-lookup"><span data-stu-id="5af3f-157">Parse and construct a new JSON message</span></span> | <span data-ttu-id="5af3f-158">Strukturen JSON-meddelande</span><span class="sxs-lookup"><span data-stu-id="5af3f-158">Structure JSON message</span></span> | `converter.js` |

<span data-ttu-id="5af3f-159">Denna modul är en typisk Azure IoT kant-modul.</span><span class="sxs-lookup"><span data-stu-id="5af3f-159">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="5af3f-160">Den godkänner temperatur meddelanden från andra moduler (maskinvara modulen, eller i det här fallet våra simulerade Bell-modulen); och normaliserar temperatur meddelande i en strukturerad JSON-meddelandet (inklusive bifoga meddelande-ID, ställer in egenskapen för om vi behöver utlösa aviseringen temperatur och så vidare).</span><span class="sxs-lookup"><span data-stu-id="5af3f-160">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes the temperature message in to a structured JSON message (including appending the message ID, setting the property of whether we need to trigger the temperature alert, and so on).</span></span>

```javascript
receive: function (message) {
  // Initialize the messageCount in global object at first time.
  if (!global.messageCount) {
    global.messageCount = 0;
  }

  // Read the content and properties objects from message.
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

  // Publish the new message to broker.
  this.broker.publish(
    {
      properties: newProperties,
      content: new Uint8Array(Buffer.from(JSON.stringify(newContent), 'utf8'))
    }
  );
},
```

### <a name="printer-module"></a><span data-ttu-id="5af3f-161">Modul för skrivare</span><span class="sxs-lookup"><span data-stu-id="5af3f-161">Printer module</span></span>
| <span data-ttu-id="5af3f-162">Indata</span><span class="sxs-lookup"><span data-stu-id="5af3f-162">Input</span></span>                          | <span data-ttu-id="5af3f-163">Processor</span><span class="sxs-lookup"><span data-stu-id="5af3f-163">Processor</span></span> | <span data-ttu-id="5af3f-164">Resultat</span><span class="sxs-lookup"><span data-stu-id="5af3f-164">Output</span></span>                     | <span data-ttu-id="5af3f-165">Källfilen</span><span class="sxs-lookup"><span data-stu-id="5af3f-165">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="5af3f-166">Alla meddelanden från andra moduler</span><span class="sxs-lookup"><span data-stu-id="5af3f-166">Any message from other modules</span></span> | <span data-ttu-id="5af3f-167">Saknas</span><span class="sxs-lookup"><span data-stu-id="5af3f-167">N/A</span></span>       | <span data-ttu-id="5af3f-168">Logga meddelandet-konsolen</span><span class="sxs-lookup"><span data-stu-id="5af3f-168">Log the message to console</span></span> | `printer.js` |

<span data-ttu-id="5af3f-169">Denna modul är enkla, självförklarande, som matar ut de mottagna meddelandena (egenskap, innehåll) till fönstret terminal.</span><span class="sxs-lookup"><span data-stu-id="5af3f-169">This module is simple, self-explanatory, which outputs the received messages(property, content) to the terminal window.</span></span>

```javascript
receive: function (message) {
  let properties = JSON.stringify(message.properties);
  let content = Buffer.from(message.content).toString('utf8');

  console.log(`printer.receive.properties - ${properties}`);
  console.log(`printer.receive.content - ${content}\n`);
}
```

### <a name="configuration"></a><span data-ttu-id="5af3f-170">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="5af3f-170">Configuration</span></span>
<span data-ttu-id="5af3f-171">Det sista steget innan du kör modulerna som är att konfigurera Azure IoT kant och upprätta anslutningar mellan moduler.</span><span class="sxs-lookup"><span data-stu-id="5af3f-171">The final step before running the modules is to configure the Azure IoT Edge and to establish the connections between modules.</span></span>

<span data-ttu-id="5af3f-172">Vi måste först deklarera vår `node` inläsaren (eftersom Azure IoT kant stöder inläsare med olika språk) som kan refereras till av dess `name` i avsnitten efteråt.</span><span class="sxs-lookup"><span data-stu-id="5af3f-172">First we need to declare our `node` loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in the sections afterward.</span></span>

```json
"loaders": [
  {
    "type": "node",
    "name": "node"
  }
]
```

<span data-ttu-id="5af3f-173">När vi har deklarerat våra inläsare, behöver vi även deklarera samt våra moduler.</span><span class="sxs-lookup"><span data-stu-id="5af3f-173">Once we have declared our loaders, we also need to declare our modules as well.</span></span> <span data-ttu-id="5af3f-174">Precis som deklarerar inläsare, de kan också refereras av deras `name` attribut.</span><span class="sxs-lookup"><span data-stu-id="5af3f-174">Similar to declaring the loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="5af3f-175">När du deklarerar en modul, vi behöver ange ska det använda inläsaren (som ska vara det som vi definierade innan) och startpunkten (ska vara normaliserade klassnamnet för våra modulen) för varje modul.</span><span class="sxs-lookup"><span data-stu-id="5af3f-175">When declaring a module, we need to specify the loader it should use (which should be the one we defined before) and the entry-point (should be the normalized class name of our module) for each module.</span></span> <span data-ttu-id="5af3f-176">Den `simulated_device` modul är en inbyggd modul som ingår i Azure IoT kant core runtime-paketet.</span><span class="sxs-lookup"><span data-stu-id="5af3f-176">The `simulated_device` module is a native module that is included in the Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="5af3f-177">Inkludera `args` i JSON-filen även om det är `null`.</span><span class="sxs-lookup"><span data-stu-id="5af3f-177">Include `args` in the JSON file even if it is `null`.</span></span>

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

<span data-ttu-id="5af3f-178">I slutet av konfigurationen upprätta vi anslutningar.</span><span class="sxs-lookup"><span data-stu-id="5af3f-178">At the end of the configuration, we establish the connections.</span></span> <span data-ttu-id="5af3f-179">Varje anslutning uttrycks av `source` och `sink`.</span><span class="sxs-lookup"><span data-stu-id="5af3f-179">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="5af3f-180">De bör både referera en fördefinierad modul.</span><span class="sxs-lookup"><span data-stu-id="5af3f-180">They should both reference a pre-defined module.</span></span> <span data-ttu-id="5af3f-181">Det utgående meddelandet i `source` modulen vidarebefordras till indata för `sink` modul.</span><span class="sxs-lookup"><span data-stu-id="5af3f-181">The output message of `source` module is forwarded to the input of `sink` module.</span></span>

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

## <a name="running-the-modules"></a><span data-ttu-id="5af3f-182">Kör moduler</span><span class="sxs-lookup"><span data-stu-id="5af3f-182">Running the modules</span></span>
1. `npm install`
2. `npm start`

<span data-ttu-id="5af3f-183">Om du vill avsluta programmet trycker du på `<Enter>` nyckel.</span><span class="sxs-lookup"><span data-stu-id="5af3f-183">If you want to terminate the application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5af3f-184">Det rekommenderas inte att använda Ctrl + C för att avsluta programmet IoT kant.</span><span class="sxs-lookup"><span data-stu-id="5af3f-184">It is not recommended to use Ctrl + C to terminate the IoT Edge application.</span></span> <span data-ttu-id="5af3f-185">Som det här sättet kan orsaka processen att avbrytas onormalt.</span><span class="sxs-lookup"><span data-stu-id="5af3f-185">As this way may cause the process to terminate abnormally.</span></span>
