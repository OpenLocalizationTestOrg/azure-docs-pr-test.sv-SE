---
title: "aaaSet in din utvecklingsmiljö för Mac OS X toowork med Azure Service Fabric | Microsoft Docs"
description: "Installera hello runtime, verktyg och SDK och skapa ett kluster för lokal utveckling. När du har slutfört den här installationen kommer du att redo toobuild program på Mac OS X."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2017
ms.author: saysa
ms.openlocfilehash: 0b8a6c1fc1871fa76f3e21cefbc7f66f79072797
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Konfigurera din utvecklingsmiljö i Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Du kan skapa toorun för Service Fabric-program på Linux-kluster med Mac OS X. Den här artikeln beskriver hur tooset upp din Mac för utveckling.

## <a name="prerequisites"></a>Krav
Service Fabric kan inte köras internt i OS X. toorun lokala Service Fabric-kluster, vi erbjuder en förkonfigurerad Ubuntu virtuell dator med Vagrant och VirtualBox. Innan du börjar behöver du:

* [Vagrant (v1.8.4 eller senare)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> Du måste toouse ömsesidigt stöds versioner av Vagrant och VirtualBox. Vagrant kan bete sig oförutsägbart med en VirtualBox-version som inte stöds.
>

## <a name="create-hello-local-vm"></a>Skapa hello lokala VM
toocreate Hej lokala VM som innehåller ett Service Fabric-kluster med 5 noder, utföra hello följande steg:

1. Klona hello `Vagrantfile` lagringsplatsen

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Ta med det här steget används hello filen `Vagrantfile` som innehåller hello VM configuration tillsammans med hello plats hello VM hämtas från.

2. Navigera toohello lokala kloning av hello lagringsplatsen

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (Valfritt) Ändra hello standardinställningarna för VM

    Som standard konfigureras hello lokala VM på följande sätt:

   * 3 GB minne allokeras
   * Privata värden datornätverk IP-192.168.50.50 aktiverar genomströmning av trafik från hello Mac-värden

     Du kan ändra dessa inställningar eller lägga till andra configuration toohello VM i hello `Vagrantfile`. Se hello [Vagrant dokumentationen](http://www.vagrantup.com/docs) hello fullständig lista över konfigurationsalternativ.
4. Skapa hello VM

    ```bash
    vagrant up
    ```

   Det här steget hämtar hello förkonfigurerade VM-avbildning, starta den lokalt, och sedan skapa en lokal Service Fabric-klustret i den. Du bör den tootake några minuter. Om installationen är klar visas ett meddelande i hello utdata som visar hello klustret startas.

    ![Klusterinstallationen startar efter att den virtuella datorn har etablerats][cluster-setup-script]

    >[!TIP]
    > Om hello VM download tar lång tid kan du hämta den med hjälp av wget eller curl eller via en webbläsare genom att gå toohello länk som anges av **config.vm.box_url** i hello filen `Vagrantfile`. När du har hämtat lokalt, redigera `Vagrantfile` toopoint toohello lokal sökväg där du hämtade hello avbildningen. För exempel om du har hämtat hello avbildningen too/home/users/test/azureservicefabric.tp8.box sedan ange **config.vm.box_url** toothat sökväg.
    >

5. Testa att hello klustret har ställts in korrekt genom att gå tooService Fabric-Utforskaren på http://192.168.50.50:19080/Explorer (förutsatt att du behöll hello standard privata nätverks-IP).

    ![Service Fabric Explorer visas från hello värden Mac][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a>Skapa program på Mac med Yeoman
Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric-program från terminalen med en Yeoman-mallgenerator. Följ hello stegen nedan tooensure som du har hello Service Fabric yeoman mall generator fungerar på din dator.

1. Du behöver toohave Node.js och NPM installerad på du mac. Annars kan du installera Node.js och NPM med Homebrew med hello följande. toocheck hello versioner av Node.js och NPM installeras på en Mac, kan du använda hello ``-v`` alternativet.

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM

  ```bash
  npm install -g yo
  ```
3. Installera hello Yeoman generator önskade toouse, följande hello i hello komma igång [dokumentationen](service-fabric-get-started-linux.md). toocreate Service Fabric-program med hjälp av Yeoman, gör hello-

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. toobuild ett Service Fabric Java-program på Mac måste - JDK 1.8 och Gradle installeras på hello-dator.


## <a name="install-hello-service-fabric-plugin-for-eclipse-neon"></a>Installera Service Fabric-plugin-programmet hello för Eclipse Neon

Service Fabric innehåller ett plugin-program för hello **Eclipse Neon för Java IDE** som kan förenkla hello processen att skapa, skapa och distribuera Java-tjänster. Du kan följa hello installationssteg som nämns i detta allmänna [dokumentationen](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) om att installera eller uppdatera Service Fabric Eclipse-plugin-programmet.

>[!TIP]
> Som standard stöder vi hello standard IP som anges i hello ``Vagrantfile`` i hello ``Local.json`` av programmet hello genereras. Om du ändrar som och distribuera Vagrant med en annan IP-adress, uppdatera hello motsvarande IP i ``Local.json`` för din App samt.

## <a name="next-steps"></a>Nästa steg
<!-- Links -->
* [Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse](service-fabric-get-started-eclipse.md)
* [Skapa ett Service Fabric-kluster i hello Azure-portalen](service-fabric-cluster-creation-via-portal.md)
* [Skapa ett Service Fabric-kluster med hello Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Förstå hello programmodell för Service Fabric](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
