---
title: aaaCreate en Azure IoT kant modul med Java | Microsoft Docs
description: "Den här kursen visar hur toowrite en TIVERA data konverteraren modulen med hjälp av hello senaste Azure IoT kant Maven-paket."
services: iot-hub
author: junyi
manager: timlt
ms.service: iot-hub
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 06/28/2017
ms.author: junyi
ms.openlocfilehash: abb560933d13d133ae9a1da08b503d5735b230e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-iot-edge-module-with-java"></a><span data-ttu-id="d20b6-103">Skapa ett Azure IoT-Edge-modulen med Java</span><span class="sxs-lookup"><span data-stu-id="d20b6-103">Create an Azure IoT Edge Module with Java</span></span>

<span data-ttu-id="d20b6-104">Den här kursen visar hur en kan skapa en modul för Azure IoT gränsen i Java.</span><span class="sxs-lookup"><span data-stu-id="d20b6-104">This tutorial showcases how one might build a module for Azure IoT Edge in Java.</span></span>

<span data-ttu-id="d20b6-105">I den här självstudiekursen kommer vi att gå igenom miljökonfiguration och hur toowrite en [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data konverteraren modul med hello senaste Azure IoT kant Maven-paket.</span><span class="sxs-lookup"><span data-stu-id="d20b6-105">In this tutorial, we will walk through environment setup and how toowrite a [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data converter module using hello latest Azure IoT Edge Maven packages.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d20b6-106">Krav</span><span class="sxs-lookup"><span data-stu-id="d20b6-106">Prerequisites</span></span>

<span data-ttu-id="d20b6-107">I det här avsnittet ska du konfigurera din miljö för utveckling av IoT kant modulen.</span><span class="sxs-lookup"><span data-stu-id="d20b6-107">In this section, you will set up your environment for IoT Edge module development.</span></span> <span data-ttu-id="d20b6-108">Det gäller tooboth *64-bitars Windows* och *64-bitars Linux (8 Ubuntu/Debian)* operativsystem.</span><span class="sxs-lookup"><span data-stu-id="d20b6-108">It applies tooboth *64-bit Windows* and *64-bit Linux (Ubuntu/Debian 8)* operating systems.</span></span>

<span data-ttu-id="d20b6-109">hello följande programvara krävs:</span><span class="sxs-lookup"><span data-stu-id="d20b6-109">hello following software is required:</span></span>

* <span data-ttu-id="d20b6-110">[Git klienten](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="d20b6-110">[Git Client](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="d20b6-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="d20b6-111">[**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="d20b6-112">[Maven](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="d20b6-112">[Maven](https://maven.apache.org/install.html).</span></span>

<span data-ttu-id="d20b6-113">Öppna en kommandorad terminal fönster och klona hello efter databasen:</span><span class="sxs-lookup"><span data-stu-id="d20b6-113">Open a command-line terminal window and clone hello following repository:</span></span>

1. <span data-ttu-id="d20b6-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span><span class="sxs-lookup"><span data-stu-id="d20b6-114">`git clone https://github.com/Azure-Samples/iot-edge-samples.git`.</span></span>
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a><span data-ttu-id="d20b6-115">Övergripande arkitektur</span><span class="sxs-lookup"><span data-stu-id="d20b6-115">Overall architecture</span></span>

<span data-ttu-id="d20b6-116">hello Azure IoT kant plattform kraftigt antar hello [Von Neumann arkitektur](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span><span class="sxs-lookup"><span data-stu-id="d20b6-116">hello Azure IoT Edge platform heavily adopts hello [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture).</span></span> <span data-ttu-id="d20b6-117">Vilket innebär att hela hello Azure IoT kant arkitekturen är ett system som bearbetar indata och utdata; och att varje enskild modul är en liten input-output-undersystemet.</span><span class="sxs-lookup"><span data-stu-id="d20b6-117">Which means that hello entire Azure IoT Edge architecture is a system which processes input and produces output; and that each individual module is also a tiny input-output subsystem.</span></span> <span data-ttu-id="d20b6-118">I den här självstudiekursen kommer vi introducerar hello följande två moduler:</span><span class="sxs-lookup"><span data-stu-id="d20b6-118">In this tutorial, we will introduce hello following two modules:</span></span>

1. <span data-ttu-id="d20b6-119">En modul som tar emot en simulerad [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signalerar och konverterar den till en formaterad [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.</span><span class="sxs-lookup"><span data-stu-id="d20b6-119">A module which receives a simulated [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signal and converts it into a formatted [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>
2. <span data-ttu-id="d20b6-120">En modul som skriver ut hello emot [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.</span><span class="sxs-lookup"><span data-stu-id="d20b6-120">A module which prints hello received [JSON](https://en.wikipedia.org/wiki/JSON) message.</span></span>

<span data-ttu-id="d20b6-121">hello visar följande bild hello vanliga slutpunkt till slutpunkt dataflöde för det här projektet:</span><span class="sxs-lookup"><span data-stu-id="d20b6-121">hello following image displays hello typical end-to-end dataflow for this project:</span></span>

<span data-ttu-id="d20b6-122">![Dataflöde mellan tre moduler](media/iot-hub-iot-edge-create-module/dataflow.png "indata: simulerade TIVERA modulen. Processor: Konverterare modulen. Utdata: Skrivare modul")</span><span class="sxs-lookup"><span data-stu-id="d20b6-122">![Dataflow between three modules](media/iot-hub-iot-edge-create-module/dataflow.png "Input: Simulated BLE Module; Processor: Converter Module; Output: Printer Module")</span></span>

## <a name="understanding-hello-code"></a><span data-ttu-id="d20b6-123">Förstå hello kod</span><span class="sxs-lookup"><span data-stu-id="d20b6-123">Understanding hello code</span></span>

### <a name="maven-project-structure"></a><span data-ttu-id="d20b6-124">Struktur för maven-projekt</span><span class="sxs-lookup"><span data-stu-id="d20b6-124">Maven project structure</span></span>

<span data-ttu-id="d20b6-125">Eftersom Azure IoT kant paket baseras på Maven, vi behöver toocreate en typisk Maven-projekt-struktur som innehåller en `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="d20b6-125">Since Azure IoT Edge packages are based on Maven, we need toocreate a typical Maven project structure, which contains a `pom.xml` file.</span></span>

<span data-ttu-id="d20b6-126">Hej POM ärver från hello `com.microsoft.azure.gateway.gateway-module-base` paket som deklarerar alla hello beroenden som krävs av en modul-projekt som innehåller hello runtime binärfiler, hello gateway sökvägen konfigurationsfilen och hello körningsbeteende.</span><span class="sxs-lookup"><span data-stu-id="d20b6-126">hello POM inherits from hello `com.microsoft.azure.gateway.gateway-module-base` package, which declares all of hello dependencies needed by a module project which includes hello runtime binaries, hello gateway configuration file path, and hello execution behavior.</span></span> <span data-ttu-id="d20b6-127">Detta innebär att vi sparar mycket tid och eliminera hello måste toowrite och skriv om hundratals rader med kod upprepade gånger.</span><span class="sxs-lookup"><span data-stu-id="d20b6-127">This saves us lots of time and eliminate hello need toowrite and rewrite hundreds of lines of code over and over again.</span></span>

<span data-ttu-id="d20b6-128">Vi behöver tooupdate filen pom.xml genom att deklarera hello krävs beroenden/plugin-program och hello configuration file toobe används av våra modulen som visas i följande kodutdrag hello hello namn.</span><span class="sxs-lookup"><span data-stu-id="d20b6-128">We need tooupdate the pom.xml file by declaring hello required dependencies/plugins and hello name of hello configuration file toobe used by our module as shown in hello following code snippet.</span></span>

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- Inherit from parent -->
  <parent>
    <groupId>com.microsoft.azure.gateway</groupId>
    <artifactId>gateway-module-base</artifactId>
    <version>1.0.1</version>
  </parent>
  
  <groupId>com.microsoft.azure.gateway</groupId>
  <artifactId>ble-converter</artifactId>
  <version>1.0</version>
  <packaging>jar</packaging>

  <!-- Set hello filename of hello Azure IoT Edge configuration located
       under ./src/main/resources/gateway/ which is used in parent -->
  <properties>
    <gw.config.fileName>gw-config.json</gw.config.fileName>
  </properties>

  <!-- Re-declare dependencies used in parent -->
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.gateway</groupId>
      <artifactId>gateway-java-binding</artifactId>
    </dependency>
    <dependency>
      <groupId>${dependency.runtime.group}</groupId>
      <artifactId>${dependency.runtime.name}</artifactId>
    </dependency>
  </dependencies>

  <!-- Re-declare plugins used in parent -->
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
</project>
```

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a><span data-ttu-id="d20b6-129">Grundläggande förståelse för en gräns för Azure IoT-modul</span><span class="sxs-lookup"><span data-stu-id="d20b6-129">Basic understanding of an Azure IoT Edge module</span></span>

<span data-ttu-id="d20b6-130">Du kan hantera en Azure IoT kant-modul som en data-processor vars uppgift är att: indata, behandlas och resultat.</span><span class="sxs-lookup"><span data-stu-id="d20b6-130">You can treat an Azure IoT Edge module as a data processor whose job is to: receive input, process it, and produce output.</span></span>

<span data-ttu-id="d20b6-131">hello indata kan inte data från maskinvara (till exempel en rörelsedetektor), ett meddelande från andra moduler eller något annat (till exempel ett slumptal som genererats med jämna mellanrum av en timer).</span><span class="sxs-lookup"><span data-stu-id="d20b6-131">hello input might be data from hardware (like a motion detector), a message from other modules, or anything else (like a random number generated periodically by a timer).</span></span>

<span data-ttu-id="d20b6-132">hello utdata är liknande toohello indata, det kan utlösa beteendet för maskinvara (till exempel hello blinka Indikator), en meddelandet tooother moduler eller något annat (till exempel utskrift toohello console).</span><span class="sxs-lookup"><span data-stu-id="d20b6-132">hello output is similar toohello input, it could trigger hardware behavior (like hello blinking LED), a message tooother modules, or anything else (like printing toohello console).</span></span>

<span data-ttu-id="d20b6-133">Moduler som kommunicerar med varandra med hjälp av `com.microsoft.azure.gateway.messaging.Message` klass.</span><span class="sxs-lookup"><span data-stu-id="d20b6-133">Modules communicate with each other using `com.microsoft.azure.gateway.messaging.Message` class.</span></span> <span data-ttu-id="d20b6-134">Hej **innehåll** av en `Message` är en bytematris som klarar av som representerar alla typer av data som du vill.</span><span class="sxs-lookup"><span data-stu-id="d20b6-134">hello **Content** of a `Message` is a byte array which is capable of representing any kind of data you like.</span></span> <span data-ttu-id="d20b6-135">**Egenskaper för** är också tillgängliga i hello `Message` och är helt enkelt en string-string-mappning.</span><span class="sxs-lookup"><span data-stu-id="d20b6-135">**Properties** are also available in hello `Message` and are simply a string-to-string mapping.</span></span> <span data-ttu-id="d20b6-136">Du kan tänka dig **egenskaper** som hello rubriker i en HTTP-begäran eller hello metadata för en fil.</span><span class="sxs-lookup"><span data-stu-id="d20b6-136">You may think of **Properties** as hello headers in an HTTP request, or hello metadata of a file.</span></span>

<span data-ttu-id="d20b6-137">I ordning toodevelop en gräns för Azure IoT-modul i Java, behöver du toocreate en ny modul-klass som ärver från `com.microsoft.azure.gateway.core.GatewayModule` och implementera hello krävs abstrakta metoder `receive()` och `destroy()`.</span><span class="sxs-lookup"><span data-stu-id="d20b6-137">In order toodevelop an Azure IoT Edge module in Java, you need toocreate a new module class which inherits from `com.microsoft.azure.gateway.core.GatewayModule` and implement hello required abstract methods `receive()` and `destroy()`.</span></span> <span data-ttu-id="d20b6-138">Du kan nu även välja tooimplement hello valfria `start()` eller `create()` samt metoder.</span><span class="sxs-lookup"><span data-stu-id="d20b6-138">At this point, you may also choose tooimplement hello optional `start()` or `create()` methods as well.</span></span> <span data-ttu-id="d20b6-139">hello följande kodfragment som visar hur tooget igång redigering av en Azure IoT kant-modul.</span><span class="sxs-lookup"><span data-stu-id="d20b6-139">hello following code snippet shows you how tooget started authoring an Azure IoT Edge module.</span></span>

```java
import com.microsoft.azure.gateway.core.Broker;
import com.microsoft.azure.gateway.core.GatewayModule;
import com.microsoft.azure.gateway.messaging.Message;

public class MyEdgeModule extends GatewayModule {
  public MyEdgeModule(long address, Broker broker, String configuration) {
    /* Let hello GatewayModule do hello dirty work of initialization. It's also
       a good time tooparse your own configuration defined in Azure IoT Edge
       configuration file (typically ./src/main/resources/gateway/gw-config.json) */
    super(address, broker, configuration);
  }

  @Override
  public void start() {
    /* Acquire hello resources you need. If you don't
       need any resources, you may omit this method. */
  }

  @Override
  public void destroy() {
    /* It's time toorelease all resources. This method is required. */
  }

  @Override
  public void receive(Message message) {
    /* Logic tooprocess hello input message. This method is required. */
    // ...
    /* Use publish() method toodo hello output. You are
       allowed toopublish your new Message instance. */
    this.publish(message);
  }
}
```

### <a name="converter-module"></a><span data-ttu-id="d20b6-140">Konverteraren modul</span><span class="sxs-lookup"><span data-stu-id="d20b6-140">Converter module</span></span>

| <span data-ttu-id="d20b6-141">Indata</span><span class="sxs-lookup"><span data-stu-id="d20b6-141">Input</span></span>                    | <span data-ttu-id="d20b6-142">Processor</span><span class="sxs-lookup"><span data-stu-id="d20b6-142">Processor</span></span>                              | <span data-ttu-id="d20b6-143">Resultat</span><span class="sxs-lookup"><span data-stu-id="d20b6-143">Output</span></span>                 | <span data-ttu-id="d20b6-144">Källfilen</span><span class="sxs-lookup"><span data-stu-id="d20b6-144">Source File</span></span>            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| <span data-ttu-id="d20b6-145">Meddelande för temperatur-data</span><span class="sxs-lookup"><span data-stu-id="d20b6-145">Temperature data message</span></span> | <span data-ttu-id="d20b6-146">Analysera och skapa ett nytt JSON-meddelande</span><span class="sxs-lookup"><span data-stu-id="d20b6-146">Parse and construct a new JSON message</span></span> | <span data-ttu-id="d20b6-147">Strukturen JSON-meddelande</span><span class="sxs-lookup"><span data-stu-id="d20b6-147">Structure JSON message</span></span> | `ConverterModule.java` |

<span data-ttu-id="d20b6-148">Denna modul är en typisk Azure IoT kant-modul.</span><span class="sxs-lookup"><span data-stu-id="d20b6-148">This module is a typical Azure IoT Edge module.</span></span> <span data-ttu-id="d20b6-149">Den godkänner temperatur meddelanden från andra moduler (maskinvara modulen, eller i det här fallet våra simulerade Bell-modulen); och normaliserar temperatur hälsningsmeddelande i tooa strukturerad JSON-meddelande (inklusive bifoga hello meddelande-ID, hello-egenskapen för om vi behöver tootrigger hello temperatur aviseringen och så vidare).</span><span class="sxs-lookup"><span data-stu-id="d20b6-149">It accepts temperature messages from other modules (a hardware module, or in this case our simulated BLE module); and then normalizes hello temperature message in tooa structured JSON message (including appending hello message ID, setting hello property of whether we need tootrigger hello temperature alert, and so on).</span></span>

```java
@Override
public void receive(Message message) {
  try {
    JSONObject messageFromBle = new JSONObject(new String(message.getContent()));
    double temperature = messageFromBle.getDouble("temperature");
    Map<String, String> inputProperties = message.getProperties();

    HashMap<String, String> properties = new HashMap<>();
    properties.put("source", inputProperties.get("source"));
    properties.put("macAddress", inputProperties.get("macAddress"));
    properties.put("temperatureAlert", temperature > 30 ? "true" : "false");

    String content = String.format(
        "{ \"deviceId\": \"Intel NUC Gateway\", \"messageId\": %d, \"temperature\": %f }",
        ++this.messageCount, temperature);

    this.publish(new Message(content.getBytes(), properties));
  } catch (Exception ex) {
    ex.printStackTrace();
  }
}
```

### <a name="printer-module"></a><span data-ttu-id="d20b6-150">Modul för skrivare</span><span class="sxs-lookup"><span data-stu-id="d20b6-150">Printer module</span></span>

| <span data-ttu-id="d20b6-151">Indata</span><span class="sxs-lookup"><span data-stu-id="d20b6-151">Input</span></span>                          | <span data-ttu-id="d20b6-152">Processor</span><span class="sxs-lookup"><span data-stu-id="d20b6-152">Processor</span></span> | <span data-ttu-id="d20b6-153">Resultat</span><span class="sxs-lookup"><span data-stu-id="d20b6-153">Output</span></span>                     | <span data-ttu-id="d20b6-154">Källfilen</span><span class="sxs-lookup"><span data-stu-id="d20b6-154">Source File</span></span>          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| <span data-ttu-id="d20b6-155">Alla meddelanden från andra moduler</span><span class="sxs-lookup"><span data-stu-id="d20b6-155">Any message from other modules</span></span> | <span data-ttu-id="d20b6-156">Saknas</span><span class="sxs-lookup"><span data-stu-id="d20b6-156">N/A</span></span>       | <span data-ttu-id="d20b6-157">Logga tooconsole för hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="d20b6-157">Log hello message tooconsole</span></span> | `PrinterModule.java` |

<span data-ttu-id="d20b6-158">Det här är en enkel, självförklarande, modul som matar ut hello mottagna meddelanden toohello terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="d20b6-158">This is a simple, self-explanatory, module which outputs hello received messages toohello terminal window.</span></span>

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a><span data-ttu-id="d20b6-159">Azure IoT kant-konfiguration</span><span class="sxs-lookup"><span data-stu-id="d20b6-159">Azure IoT Edge configuration</span></span>

<span data-ttu-id="d20b6-160">hello sista steget innan du kör hello moduler är tooconfigure hello Azure IoT kant och tooestablish hello anslutningar mellan moduler.</span><span class="sxs-lookup"><span data-stu-id="d20b6-160">hello final step before running hello modules is tooconfigure hello Azure IoT Edge and tooestablish hello connections between modules.</span></span>

<span data-ttu-id="d20b6-161">Vi måste först toodeclare våra Java-inläsaren (eftersom Azure IoT kant stöder inläsare med olika språk) som kan refereras till av dess `name` i hello avsnitt efteråt.</span><span class="sxs-lookup"><span data-stu-id="d20b6-161">First we need toodeclare our Java loader (since Azure IoT Edge supports loaders of different languages) which could be referenced by its `name` in hello sections afterward.</span></span>

```json
"loaders": [{
  "type": "java",
  "name": "java",
  "configuration": {
    "jvm.options": {
      "library.path": "./"
    }
  }
}]
```

<span data-ttu-id="d20b6-162">När vi har deklarerat våra inläsare, behöver du också toodeclare samt våra moduler.</span><span class="sxs-lookup"><span data-stu-id="d20b6-162">Once we have declared our loaders, we will also need toodeclare our modules as well.</span></span> <span data-ttu-id="d20b6-163">Liknande toodeclaring hello inläsare de kan också refereras av deras `name` attribut.</span><span class="sxs-lookup"><span data-stu-id="d20b6-163">Similar toodeclaring hello loaders, they can also be referenced by their `name` attribute.</span></span> <span data-ttu-id="d20b6-164">När du deklarerar en modul måste toospecify hello inläsaren ska det använda (som ska vara hello en vi definierade innan) och hello startpunkten (ska vara hello normaliserade klassnamnet för våra modulen) för varje modul.</span><span class="sxs-lookup"><span data-stu-id="d20b6-164">When declaring a module, we need toospecify hello loader it should use (which should be hello one we defined before) and hello entry-point (should be hello normalized class name of our module) for each module.</span></span> <span data-ttu-id="d20b6-165">Hej `simulated_device` modul är en inbyggd modul som ingår i hello Azure IoT kant core runtime-paketet.</span><span class="sxs-lookup"><span data-stu-id="d20b6-165">hello `simulated_device` module is a native module which is included in hello Azure IoT Edge core runtime package.</span></span> <span data-ttu-id="d20b6-166">Du bör alltid innehålla `args` i hello JSON-filen även om den `null`.</span><span class="sxs-lookup"><span data-stu-id="d20b6-166">You should always include `args` in hello JSON file even if it is `null`.</span></span>

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
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/ConverterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  },
  {
    "name": "print",
    "loader": {
      "name": "java",
      "entrypoint": {
        "class.name": "com/microsoft/azure/gateway/PrinterModule",
        "class.path": "./ble-converter-1.0-with-deps.jar"
      }
    },
    "args": null
  }
]
```

<span data-ttu-id="d20b6-167">Hello slutet av hello konfiguration upprätta vi hello anslutningar.</span><span class="sxs-lookup"><span data-stu-id="d20b6-167">At hello end of hello configuration, we establish hello connections.</span></span> <span data-ttu-id="d20b6-168">Varje anslutning uttrycks av `source` och `sink`.</span><span class="sxs-lookup"><span data-stu-id="d20b6-168">Each connection is expressed by `source` and `sink`.</span></span> <span data-ttu-id="d20b6-169">De bör både referera en fördefinierad modul.</span><span class="sxs-lookup"><span data-stu-id="d20b6-169">They should both reference a pre-defined module.</span></span> <span data-ttu-id="d20b6-170">hälsningsmeddelande för utdata av `source` modulen vidarebefordras toohello indata för `sink` modul.</span><span class="sxs-lookup"><span data-stu-id="d20b6-170">hello output message of `source` module will be forwarded toohello input of `sink` module.</span></span>

```json
"links": [
  {
    "source": "simulated_device",
    "sink": "converter"
  },
  {
    "source": "converter",
    "sink": "print"
  }
]
```

## <a name="running-hello-modules"></a><span data-ttu-id="d20b6-171">Kör hello-moduler</span><span class="sxs-lookup"><span data-stu-id="d20b6-171">Running hello modules</span></span>

<span data-ttu-id="d20b6-172">Använd `mvn package` toobuild allt i hello `target/` mapp.</span><span class="sxs-lookup"><span data-stu-id="d20b6-172">Use `mvn package` toobuild everything into hello `target/` folder.</span></span> <span data-ttu-id="d20b6-173">`mvn clean package`rekommenderas för Rensa.</span><span class="sxs-lookup"><span data-stu-id="d20b6-173">`mvn clean package` is also recommended for a clean build.</span></span>

<span data-ttu-id="d20b6-174">Använd `mvn exec:exec` toorun hello Azure IoT kant och du bör se hello temperatur data och alla hello egenskaper är tryckt toohello konsolen till en fast kostnad.</span><span class="sxs-lookup"><span data-stu-id="d20b6-174">Use `mvn exec:exec` toorun hello Azure IoT Edge and you should observe that hello temperature data and all hello properties are printed toohello console at a fixed rate.</span></span>

<span data-ttu-id="d20b6-175">Om du vill tooterminate hello program, trycker du på `<Enter>` nyckel.</span><span class="sxs-lookup"><span data-stu-id="d20b6-175">If you want tooterminate hello application, press `<Enter>` key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d20b6-176">Det rekommenderas inte toouse Ctrl + C tooterminate hello IoT Edge gateway-program.</span><span class="sxs-lookup"><span data-stu-id="d20b6-176">It is not recommended toouse Ctrl + C tooterminate hello IoT Edge gateway application.</span></span> <span data-ttu-id="d20b6-177">Eftersom detta kan orsaka hello processen tooterminate onormalt.</span><span class="sxs-lookup"><span data-stu-id="d20b6-177">As this may cause hello process tooterminate abnormally.</span></span>

