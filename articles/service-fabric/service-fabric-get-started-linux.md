---
title: "aaaSet in din utvecklingsmiljö i Linux | Microsoft Docs"
description: "Installera hello runtime och SDK och skapa en lokal utveckling kluster på Linux. När du har slutfört den här installationen kommer du att redo toobuild program."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/23/2017
ms.author: subramar
ms.openlocfilehash: 9d82c2015f9e2c6fb55f2052c7cdb1e906c5deeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment-on-linux"></a>Förbereda utvecklingsmiljön i Linux
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

toodeploy och kör [Azure Service Fabric program](service-fabric-application-model.md) på utvecklingsdatorn Linux installera hello körning och gemensamma SDK. Du kan även installera SDK:er för Java och .NET Core.

## <a name="prerequisites"></a>Krav

följande versioner av operativsystemet hello stöds för utveckling:

* Ubuntu 16.04 (`Xenial Xerus`)

## <a name="update-your-apt-sources"></a>Uppdatera dina APT-källor
tooinstall hello SDK och hello associerade runtime-paketet via hello lgh get-kommandoradsverktyg, måste du först uppdatera avancerade paketering verktyget (LGH)-datakällor.

1. Öppna en terminal.
2. Lägg till hello Service Fabric lagringsplatsen tooyour källor lista.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Lägg till hello `dotnet` lagringsplatsen tooyour källor lista.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Lägg till hello nya Gnu sekretess Guard (GnuPG eller GPG) nyckeln tooyour LGH nyckelring.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Lägg till hello officiella Docker GPG viktiga tooyour LGH nyckelring.

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Ställ in hello Docker-databasen.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. Uppdatera paketet innehåller baserat på hello nyligen lagt till databaser.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-hello-sdk-for-local-cluster-setup"></a>Installera och konfigurera hello SDK för konfiguration av lokal

När du har uppdaterat dina datakällor kan du installera hello SDK. Installera hello Service Fabric SDK-paketet, bekräfta hello installationen och accepterar toohello licensavtalet (EULA).

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   hello automatisera följande kommandon accepterar hello-licens för Service Fabric-paket:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

## <a name="set-up-a-local-cluster"></a>Konfigurera ett lokalt kluster
  Om hello-installationen har slutförts ska kunna toostart lokala klustret.

  1. Kör hello installationsskriptet för klustret.

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. Öppna en webbläsare och gå för[Service Fabric Explorer](http://localhost:19080/Explorer). Du bör se hello Service Fabric Explorer instrumentpanelen om hello klustret har startats.

      ![Service Fabric Explorer på Linux][sfx-linux]

  Nu kan du distribuera fördefinierade Service Fabric-programpaket eller nya paket baserat på gästbehållare eller körbara gästprogram. toobuild nya tjänster med hjälp av hello Java eller .NET Core SDK följer hello valfria konfigurationssteg som finns i följande avsnitt.


  > [!NOTE]
  > Fristående kluster stöds inte i Linux. hello preview stöder endast en ruta och flera datorer Azure Linux-kluster.
  >

## <a name="set-up-hello-service-fabric-cli"></a>Ställ in hello Service Fabric CLI

Hej [Service Fabric CLI](service-fabric-cli.md) har kommandon för att interagera med Service Fabric-enheter, inklusive kluster och program. Den är baserad på python, så att toohave python och pip-installeras innan du fortsätter med hello följande kommando:

```bash
pip install sfctl
```

## <a name="install-and-set-up-hello-generators-for-containers-and-guest-executables"></a>Installera och konfigurera hello generatorer för behållare och Gäst-körbara filer
Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric-program från terminalen med en Yeoman-mallgenerator. Följ hello stegen nedan tooensure som du har hello Service Fabric yeoman mall generatorn för att arbeta på din dator.

1. Installera nodejs och NPM på datorn

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM

  ```bash
  sudo npm install -g yo
  ```
3. Installera hello Service Fabric Yeo behållare generator och Gäst execuatble generator från NPM

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

När du har installerat hello ovan generatorer, bör du kunna toocreate appar med gästtjänster för körbara filer eller behållare genom att köra `yo azuresfguest` eller `yo azuresfcontainer` respektive.

## <a name="install-hello-necessary-java-artifacts-optional-if-you-want-toouse-hello-java-programming-models"></a>Installera hello nödvändiga Java artefakter (valfritt, om du vill toouse hello Java programmeringsmodeller)

toobuild Service Fabric-tjänster med hjälp av Java, kontrollera att du har JDK 1.8 installeras tillsammans med Gradle som används för att köra build-uppgifter. följande fragment hello installerar öppna JDK 1.8 tillsammans med Gradle. hello Service Fabric Java bibliotek hämtas från Maven.

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-hello-eclipse-neon-plug-in-optional"></a>Installera hello Eclipse Neon plugin-programmet (valfritt)

Du kan installera hello plugin-programmet Eclipse för Service Fabric från inom hello **Eclipse IDE för Java-utvecklare**. Du kan använda Eclipse toocreate Service Fabric gäst körbara program och behållarprogram i tillägg tooService Fabric Java-program.

1. Se till att du har den senaste Eclipse Neon och hello senaste Buildship versionen i Eclipse (1.0.17 eller senare) installerat. Du kan kontrollera hello versioner av installerade komponenter genom att välja **hjälp** > **installationsinformationen**. Du kan uppdatera Buildship med hjälp av hello instruktionerna på [Eclipse Buildship: Eclipse plugin-program för Gradle][buildship-update].

2. tooinstall hello Service Fabric-plugin-program, Välj **hjälp** > **installera ny programvara**.

3. I hello **arbeta med** skriver **http://dl.microsoft.com/eclipse**.

4. Klicka på **Lägg till**.

    ![hello tillgänglig programvara sida][sf-eclipse-plugin]

5. Välj hello **ServiceFabric** plugin-program och klicka sedan på **nästa**.

6. Slutföra hello installationssteg och acceptera hello licensavtalet.

Kontrollera att du har hello senaste versionen om du redan har hello Service Fabric Eclipse plugin-program installerat. Du kan kontrollera genom att välja **hjälp** > **installationsinformationen** och sedan söka efter Service Fabric hello listan över installerade plugin-program. Välj **Uppdatera** om det finns en nyare version.

Mer information finns i [Service Fabric-plugin-program för utveckling av Java-program i Eclipse](service-fabric-get-started-eclipse.md).


## <a name="install-hello-net-core-sdk-optional-if-you-want-toouse-hello-net-core-programming-models"></a>Installera hello .NET Core SDK (valfritt, om du vill toouse hello .NET Core programmeringsmodeller)
hello .NET Core SDK innehåller hello bibliotek och mallar som är nödvändiga toobuild Service Fabric-tjänster med .NET Core. Installera hello .NET Core SDK-paketet genom att köra hello följande:

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-hello-sdk-and-runtime"></a>Uppdatera hello SDK och körning

tooupdate toohello senaste versionen av hello SDK och körning, kör följande kommandon hello (Avmarkera hello SDK: er som du inte vill):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
tooupdate hello Java SDK binärfiler från Maven du behöver tooupdate hello version information hello motsvarande binära i hello ``build.gradle`` filen toopoint toohello senaste versionen. tooknow exakt där du behöver tooupdate hello version, kan du läsa tooany ``build.gradle`` filen i Service Fabric Kom igång-exempel [här](https://github.com/Azure-Samples/service-fabric-java-getting-started).

> [!NOTE]
> Uppdatera hello-paket kan det leda till att din toostop för lokal utveckling-kluster som körs. Starta om dina lokala kluster efter en uppgradering genom att följa hello anvisningar på den här sidan.

## <a name="next-steps"></a>Nästa steg

* [Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse](service-fabric-get-started-eclipse.md)
* [Skapa ditt första CSharp-program i Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Förbereda utvecklingsmiljön i OSX](service-fabric-get-started-mac.md)
* [Använd hello Service Fabric CLI toomanage dina program](service-fabric-application-lifecycle-sfctl.md)
* [Skillnader mellan Service Fabric i Windows och Linux](service-fabric-linux-windows-differences.md)
* [Kom igång med Service Fabric CLI](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
