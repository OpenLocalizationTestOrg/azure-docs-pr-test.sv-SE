---
title: "aaaCreate ett Azure Service Fabric tillförlitliga aktörer Java-program på Linux | Microsoft Docs"
description: "Lär dig hur toocreate och distribuera en Java Service Fabric tillförlitliga aktörer program inom fem minuter."
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: ryanwi
ms.openlocfilehash: 11496b767811c89969c65d1682d843448eb6a922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Skapa ditt första Java Service Fabric Reliable Actors-program på Linux
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Den här snabbstartsguiden hjälper dig att skapa ditt första Azure Service Fabric Java-program i en Linux-utvecklingsmiljö på bara några minuter.  När du är klar har du ett enkelt Java tjänsten single program som körs på hello lokal utveckling klustret.  

## <a name="prerequisites"></a>Krav
Innan du börjar installera hello Service Fabric SDK, hello Service Fabric CLI och konfigurera ett kluster för utveckling i din [Linux utvecklingsmiljö](service-fabric-get-started-linux.md). Om du använder Mac OS X kan du [konfigurera en Linux-utvecklingsmiljö på en virtuell dator med hjälp av Vagrant](service-fabric-get-started-mac.md).

Vill du även tooinstall hello [Service Fabric CLI](service-fabric-cli.md).

### <a name="install-and-set-up-hello-generators-for-java"></a>Installera och konfigurera hello generatorer för Java
Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric Java-program från terminalen med en Yeoman-mallgenerator. Följ hello stegen nedan tooensure har hello Service Fabric yeoman mall generatorn för Java fungerar på din dator.
1. Installera nodejs och NPM på datorn

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM

  ```bash
  sudo npm install -g yo
  ```
3. Installera hello Service Fabric Yeo Java-program generator från NPM

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-hello-application"></a>Skapa hello program
Ett Service Fabric-program innehåller en eller flera tjänster med en viss roll i att leverera hello-funktionerna. hello generator som du har installerat under hello senaste gör det enkelt toocreate din första tjänst och tooadd mer senare.  Du kan också skapa, bygga och distribuera Service Fabric Java-program med ett plugin-program för Eclipse. Läs mer i [Service Fabric-plugin-program för utveckling av Java-program i Eclipse](service-fabric-get-started-eclipse.md). Använd Yeoman toocreate ett program med en enda tjänst som lagrar och hämtar ett värde för prestandaräknaren för den här snabbstartsguide.

1. I en terminal, skriver du in ``yo azuresfjava``.
2. Namnge ditt program.
3. Välj hello typ av din första tjänst och ge den namnet. I den här guiden väljer vi en Reliable Actor-tjänst. Mer information om hello finns andra typer av tjänster, [Service Fabric programming översikt över säkerhetsmodell](service-fabric-choose-framework.md).
   ![Service Fabric Yeoman-generator för Java][sf-yeoman]

## <a name="build-hello-application"></a>Skapa hello program
hello Service Fabric Yeoman mallar innehåller ett build-skript för [Gradle](https://gradle.org/), som du kan använda toobuild hello programmet från hello terminal.
Service Fabric Java-beroenden hämtas från Maven. toobuild och arbete på hello Service Fabric Java-program, måste du tooensure att JDK och Gradle är installerade. Om du ännu inte har installerats kan du köra hello efter tooinstall JDK(openjdk-8-jdk) och Gradle -

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

toobuild och paketet hello program, köra hello följande:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-hello-application"></a>Distribuera programmet hello
När hello programmet är skapat kan distribuera du den toohello lokala klustret.

1. Ansluta toohello lokala Service Fabric-klustret.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Kör hello installationsskriptet hello mallen toocopy programmet hello paketet toohello klustret avbildningsarkivet, registrera hello programtyp och skapa en instans av programmet hello.

    ```bash
    ./install.sh
    ```

Distribuera hello inbyggda program är hello samma som andra Service Fabric-program. Hello-dokumentationen på [hantera ett Service Fabric-program med hello Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) detaljerade anvisningar.

Parametrarna toothese kommandon finns i manifest hello genereras inuti hello programpaket.

När programmet hello har distribuerats, öppna en webbläsare och gå till [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) på [http://localhost:19080/Explorer](http://localhost:19080/Explorer).
Expandera sedan hello **program** nod och Observera att det finns en post för din typ av program och en annan för hello första instansen av den typen.

## <a name="start-hello-test-client-and-perform-a-failover"></a>Starta hello test-klienten och utför en växling vid fel
Aktörer inte göra någonting på sina egna, de kräver en annan tjänst eller klient toosend dem meddelanden. hello aktören mallen innehåller ett enkelt testskript som du kan använda toointeract med hello aktören-tjänsten.

1. Köra hello skript med hello titta på verktyget toosee hello utdata från hello aktören service.  hello test skriptet anropar hello `setCountAsync()` på hello aktören tooincrement en räknare metodanrop hello `getCountAsync()` metoden på hello aktören tooget hello nya räknarvärdet och visar som värdet toohello-konsolen.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Leta upp hello nod värd hello primära repliken för hello aktören tjänsten i Service Fabric Explorer. I hello skärmbilden nedan är noden 3. hello primära tjänsten replik handtag Läs- och skrivåtgärder.  Ändringar i tjänstens tillstånd replikeras sedan ut toohello sekundära repliker, som körs på noder 0 och 1 i hello skärmbilden nedan.

    ![Hitta hello primära repliken i Service Fabric Explorer][sfx-primary]

3. I **noder**, klicka på hello nod du hittades i hello föregående steg och sedan välja **inaktivera (omstart)** hello Åtgärder-menyn. Den här åtgärden startar om hello-noden som kör hello primära tjänsten replik och tvingar en växling vid fel tooone hello sekundära repliker som körs på en annan nod.  Den sekundära repliken är upphöjt tooprimary, en annan sekundär replik skapas på en annan nod och hello primära repliken börjar tootake läs-/ skrivåtgärder. Hello omstarter av noden, titta på hello utdata från hello testa klient och Observera hello räknaren fortsätter tooincrement trots hello växling vid fel.

## <a name="remove-hello-application"></a>Ta bort programmet hello
Använd hello avinstallera skriptet i hello toodelete hello programmet mallinstansen, avregistrera hello programpaket och ta bort hello programpaketet från avbildningsarkivet hello klustret.

```bash
./uninstall.sh
```

Du ser att programmet hello och programtyp inte längre visas i hello i Service Fabric explorer **program** nod.

## <a name="service-fabric-java-libraries-on-maven"></a>Service Fabric Java-bibliotek på Maven
Service Fabric Java-bibliotek har lagrats i Maven. Du kan lägga till hello beroenden i hello ``pom.xml`` eller ``build.gradle`` av projekt toouse Service Fabric Java-bibliotek från **mavenCentral**.

### <a name="actors"></a>Aktörer

Service Fabric-stöd för tillförlitliga aktörer för ditt program.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-actors-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-actors-preview:0.10.0'
  }
  ```

### <a name="services"></a>Tjänster

Service Fabric-stöd för tillståndslös tjänst för ditt program.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-services-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-services-preview:0.10.0'
  }
  ```

### <a name="others"></a>Andra
#### <a name="transport"></a>Transport

Transportnivåstöd för Service Fabric Java-program. Du behöver inte tooexplicitly lägga till den här beroende tooyour tillförlitliga aktören eller tjänstprogram, såvida inte du programmerar på hello Transportskiktet.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-transport-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-transport-preview:0.10.0'
  }
  ```

#### <a name="fabric-support"></a>Fabric-stöd

Nivån stöd för Service Fabric som nämns toonative Service Fabric runtime. Du behöver inte tooexplicitly lägga till det här beroendet tooyour tillförlitliga aktören eller tjänstprogram. Detta hämtar hämtats automatiskt från Maven, när du inkluderar hello andra beroenden som ovan.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-preview:0.10.0'
  }
  ```

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a>Migrera gamla Service Fabric Java-program toobe används med Maven
Vi har nyligen flyttat Service Fabric Java-bibliotek från Service Fabric Java SDK tooMaven databasen. Medan hello nya program som du skapar med hjälp av Yeoman eller Eclipse genererar senaste uppdaterade projekt (som kommer att kunna toowork med Maven), kan du uppdatera din befintliga Service Fabric tillståndslös eller aktören Java-program som använder hello Service Fabric Java SDK tidigare toouse hello Service Fabric Java beroenden från Maven. Följ anvisningarna för hello [här](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure äldre programmet fungerar med Maven.

## <a name="next-steps"></a>Nästa steg

* [Skapa ditt första Service Fabric Java-program för Linux med hjälp av Eclipse](service-fabric-get-started-eclipse.md)
* [Läs mer om Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interagera med Service Fabric-kluster med hello Service Fabric CLI](service-fabric-cli.md)
* Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)
* [Kom igång med Service Fabric CLI](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
