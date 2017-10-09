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
# <a name="create-an-azure-iot-edge-module-with-java"></a>Skapa ett Azure IoT-Edge-modulen med Java

Den här kursen visar hur en kan skapa en modul för Azure IoT gränsen i Java.

I den här självstudiekursen kommer vi att gå igenom miljökonfiguration och hur toowrite en [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) data konverteraren modul med hello senaste Azure IoT kant Maven-paket.

## <a name="prerequisites"></a>Krav

I det här avsnittet ska du konfigurera din miljö för utveckling av IoT kant modulen. Det gäller tooboth *64-bitars Windows* och *64-bitars Linux (8 Ubuntu/Debian)* operativsystem.

hello följande programvara krävs:

* [Git klienten](https://git-scm.com/downloads).
* [**x64** JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* [Maven](https://maven.apache.org/install.html).

Öppna en kommandorad terminal fönster och klona hello efter databasen:

1. `git clone https://github.com/Azure-Samples/iot-edge-samples.git`.
2. `cd iot-edge-samples/java/simulated_ble`

## <a name="overall-architecture"></a>Övergripande arkitektur

hello Azure IoT kant plattform kraftigt antar hello [Von Neumann arkitektur](https://en.wikipedia.org/wiki/Von_Neumann_architecture). Vilket innebär att hela hello Azure IoT kant arkitekturen är ett system som bearbetar indata och utdata; och att varje enskild modul är en liten input-output-undersystemet. I den här självstudiekursen kommer vi introducerar hello följande två moduler:

1. En modul som tar emot en simulerad [TIVERA](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy) signalerar och konverterar den till en formaterad [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.
2. En modul som skriver ut hello emot [JSON](https://en.wikipedia.org/wiki/JSON) meddelande.

hello visar följande bild hello vanliga slutpunkt till slutpunkt dataflöde för det här projektet:

![Dataflöde mellan tre moduler](media/iot-hub-iot-edge-create-module/dataflow.png "indata: simulerade TIVERA modulen. Processor: Konverterare modulen. Utdata: Skrivare modul")

## <a name="understanding-hello-code"></a>Förstå hello kod

### <a name="maven-project-structure"></a>Struktur för maven-projekt

Eftersom Azure IoT kant paket baseras på Maven, vi behöver toocreate en typisk Maven-projekt-struktur som innehåller en `pom.xml` fil.

Hej POM ärver från hello `com.microsoft.azure.gateway.gateway-module-base` paket som deklarerar alla hello beroenden som krävs av en modul-projekt som innehåller hello runtime binärfiler, hello gateway sökvägen konfigurationsfilen och hello körningsbeteende. Detta innebär att vi sparar mycket tid och eliminera hello måste toowrite och skriv om hundratals rader med kod upprepade gånger.

Vi behöver tooupdate filen pom.xml genom att deklarera hello krävs beroenden/plugin-program och hello configuration file toobe används av våra modulen som visas i följande kodutdrag hello hello namn.

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

### <a name="basic-understanding-of-an-azure-iot-edge-module"></a>Grundläggande förståelse för en gräns för Azure IoT-modul

Du kan hantera en Azure IoT kant-modul som en data-processor vars uppgift är att: indata, behandlas och resultat.

hello indata kan inte data från maskinvara (till exempel en rörelsedetektor), ett meddelande från andra moduler eller något annat (till exempel ett slumptal som genererats med jämna mellanrum av en timer).

hello utdata är liknande toohello indata, det kan utlösa beteendet för maskinvara (till exempel hello blinka Indikator), en meddelandet tooother moduler eller något annat (till exempel utskrift toohello console).

Moduler som kommunicerar med varandra med hjälp av `com.microsoft.azure.gateway.messaging.Message` klass. Hej **innehåll** av en `Message` är en bytematris som klarar av som representerar alla typer av data som du vill. **Egenskaper för** är också tillgängliga i hello `Message` och är helt enkelt en string-string-mappning. Du kan tänka dig **egenskaper** som hello rubriker i en HTTP-begäran eller hello metadata för en fil.

I ordning toodevelop en gräns för Azure IoT-modul i Java, behöver du toocreate en ny modul-klass som ärver från `com.microsoft.azure.gateway.core.GatewayModule` och implementera hello krävs abstrakta metoder `receive()` och `destroy()`. Du kan nu även välja tooimplement hello valfria `start()` eller `create()` samt metoder. hello följande kodfragment som visar hur tooget igång redigering av en Azure IoT kant-modul.

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

### <a name="converter-module"></a>Konverteraren modul

| Indata                    | Processor                              | Resultat                 | Källfilen            |
| ------------------------ | -------------------------------------- | ---------------------- | ---------------------- |
| Meddelande för temperatur-data | Analysera och skapa ett nytt JSON-meddelande | Strukturen JSON-meddelande | `ConverterModule.java` |

Denna modul är en typisk Azure IoT kant-modul. Den godkänner temperatur meddelanden från andra moduler (maskinvara modulen, eller i det här fallet våra simulerade Bell-modulen); och normaliserar temperatur hälsningsmeddelande i tooa strukturerad JSON-meddelande (inklusive bifoga hello meddelande-ID, hello-egenskapen för om vi behöver tootrigger hello temperatur aviseringen och så vidare).

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

### <a name="printer-module"></a>Modul för skrivare

| Indata                          | Processor | Resultat                     | Källfilen          |
| ------------------------------ | --------- | -------------------------- | -------------------- |
| Alla meddelanden från andra moduler | Saknas       | Logga tooconsole för hello-meddelande | `PrinterModule.java` |

Det här är en enkel, självförklarande, modul som matar ut hello mottagna meddelanden toohello terminalfönster.

```java
@Override
public void receive(Message message) {
  System.out.println(message.toString());
}
```

### <a name="azure-iot-edge-configuration"></a>Azure IoT kant-konfiguration

hello sista steget innan du kör hello moduler är tooconfigure hello Azure IoT kant och tooestablish hello anslutningar mellan moduler.

Vi måste först toodeclare våra Java-inläsaren (eftersom Azure IoT kant stöder inläsare med olika språk) som kan refereras till av dess `name` i hello avsnitt efteråt.

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

När vi har deklarerat våra inläsare, behöver du också toodeclare samt våra moduler. Liknande toodeclaring hello inläsare de kan också refereras av deras `name` attribut. När du deklarerar en modul måste toospecify hello inläsaren ska det använda (som ska vara hello en vi definierade innan) och hello startpunkten (ska vara hello normaliserade klassnamnet för våra modulen) för varje modul. Hej `simulated_device` modul är en inbyggd modul som ingår i hello Azure IoT kant core runtime-paketet. Du bör alltid innehålla `args` i hello JSON-filen även om den `null`.

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

Hello slutet av hello konfiguration upprätta vi hello anslutningar. Varje anslutning uttrycks av `source` och `sink`. De bör både referera en fördefinierad modul. hälsningsmeddelande för utdata av `source` modulen vidarebefordras toohello indata för `sink` modul.

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

## <a name="running-hello-modules"></a>Kör hello-moduler

Använd `mvn package` toobuild allt i hello `target/` mapp. `mvn clean package`rekommenderas för Rensa.

Använd `mvn exec:exec` toorun hello Azure IoT kant och du bör se hello temperatur data och alla hello egenskaper är tryckt toohello konsolen till en fast kostnad.

Om du vill tooterminate hello program, trycker du på `<Enter>` nyckel.

> [!IMPORTANT]
> Det rekommenderas inte toouse Ctrl + C tooterminate hello IoT Edge gateway-program. Eftersom detta kan orsaka hello processen tooterminate onormalt.

