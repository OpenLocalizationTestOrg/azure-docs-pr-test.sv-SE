---
title: "Konfigurera en utvecklingsmiljö i Mac OS X så att den fungerar med Azure Service Fabric | Microsoft Docs"
description: "Installera runtime, SDK och verktyg och skapa ett lokalt utvecklingskluster. När du har slutfört den här installationen är du redo att börja bygga program i Mac OS X."
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
ms.openlocfilehash: 8b4fc0ab9034263418cac42ced203035e0a8fcad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Konfigurera din utvecklingsmiljö i Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Du kan skapa Service Fabric-program som körs i Linux-kluster i Mac OS X. Den här artikeln visar hur du konfigurerar din utvecklingsmiljö i Mac.

## <a name="prerequisites"></a>Krav
Service Fabric kan inte köras internt i OS X. För att du ska kunna köra ett lokalt Service Fabric-kluster tillhandahåller vi en förkonfigurerad virtuell Ubuntu-dator med Vagrant och VirtualBox. Innan du börjar behöver du:

* [Vagrant (v1.8.4 eller senare)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> Du behöver använda versioner som både stöds av Vagrant och VirtualBox. Vagrant kan bete sig oförutsägbart med en VirtualBox-version som inte stöds.
>

## <a name="create-the-local-vm"></a>Skapa den lokala virtuella datorn
För att skapa den lokala virtuella datorn med ett 5-nods Service Fabric-kluster utför du följande steg:

1. Klona `Vagrantfile`-databasen

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Det här steget hämtar filen `Vagrantfile` som innehåller VM-konfigurationen tillsammans med den plats som den virtuella datorn laddas ned från.

2. Navigera till den lokala repoklonen

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (Valfritt) Ändra standardinställningarna för virtuell dator

    Som standard konfigureras den lokala virtuella datorn på följande sätt:

   * 3 GB minne allokeras
   * Det privata värdnätverket konfigureras på IP 192.168.50.50 för att möjliggöra genomströmning av trafik från Mac-värden

     Du kan ändra dessa inställningar eller lägga till en annan konfiguration för den virtuella datorn i `Vagrantfile`. En fullständig lista över konfigurationsalternativen finns i [Vagrant-dokumentationen](http://www.vagrantup.com/docs).
4. Skapa den virtuella datorn

    ```bash
    vagrant up
    ```

   I det här steget laddas den förkonfigurerade VM-avbildningen ned. Avbildningen startas lokalt och ett lokalt Service Fabric-kluster konfigureras sedan på datorn. Det här kan ta några minuter. Om installationen lyckas, får du upp ett meddelande som indikerar att klustret startas.

    ![Klusterinstallationen startar efter att den virtuella datorn har etablerats][cluster-setup-script]

    >[!TIP]
    > Om nedladdningen av den virtuella datorn tar lång tid kan du hämta den med hjälp av wget eller curl eller via en webbläsare genom att gå till länken som anges i **config.vm.box_url** i filen `Vagrantfile`. När du hämtat den lokalt redigerar du `Vagrantfile` så att den pekar på den lokala sökväg som du laddade ned avbildningen till. Om du till exempel laddade ned avbildningen till /home/users/test/azureservicefabric.tp8.box anger du **config.vm.box_url** till den sökvägen.
    >

5. Testa att klustret är korrekt installerat genom att gå till Service Fabric Explorer på http://192.168.50.50:19080/Explorer (förutsatt att du har behållit standard-IP för det privata nätverket).

    ![Service Fabric Explorer på Mac-värddatorn][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a>Skapa program på Mac med Yeoman
Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric-program från terminalen med en Yeoman-mallgenerator. Följ stegen nedan för att se till att du har Service Fabric Yeoman-mallgeneratorn på datorn.

1. Du måste ha Node.js och NPM installerade på din Mac. Annars kan du installera Node.js och NPM med Homebrew enligt följande. Om du vill kontrollera vilka versioner av Node.js och NPM som är installerade på din Mac kan du använda alternativet ``-v``.

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM

  ```bash
  npm install -g yo
  ```
3. Installera den Yeoman-generator som du vill använda enligt instruktionerna i [dokumentationen](service-fabric-get-started-linux.md) för att komma igång. Om du vill skapa Service Fabric-program med hjälp av Yeoman följer du stegen -

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. Om du vill skapa ett Service Fabric Java-program på Mac måste du ha JDK 1.8 och Gradle installerade på datorn.


## <a name="install-the-service-fabric-plugin-for-eclipse-neon"></a>Installera Service Fabric-plugin-programmet för Eclipse Neon

Service Fabric innehåller ett plugin-program för **Eclipse Neon för Java IDE** som förenklar processen med att skapa, bygga och distribuera Java-tjänster. Du kan följa installationsstegen i den här allmänna [dokumentationen](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) som beskriver hur du installerar eller uppdaterar Service Fabric Eclipse-plugin-programmet.

>[!TIP]
> Som standard stöds standard-IP-adressen som anges i ``Vagrantfile`` i ``Local.json`` för det genererade programmet. Om du ändrar den och distribuerar Vagrant med en annan IP-adress ska du även uppdatera motsvarande IP-adress i ``Local.json`` för programmet.

## <a name="next-steps"></a>Nästa steg
<!-- Links -->
* [Skapa och distribuera ditt första Service Fabric-program med Java i Linux med hjälp av Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Skapa och distribuera ditt första Service Fabric-program med Java i Linux med Service Fabric-plugin-programmet för Eclipse](service-fabric-get-started-eclipse.md)
* [Skapa ett Service Fabric-kluster i Azure-portalen](service-fabric-cluster-creation-via-portal.md)
* [Skapa ett Service Fabric-kluster med Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Förstå Service Fabric-programmodellen](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
