---
title: "aaaCreate din första Azure mikrotjänster app i Linux med C# | Microsoft Docs"
description: Skapa och distribuera ett Service Fabric-program med C#
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/21/2017
ms.author: subramar
ms.openlocfilehash: 68d685e130be338ebcdb2f1af24b66d1e14f580a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a>Skapa ditt första Azure Service Fabric-program
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Service Fabric innehåller SDK:er för att skapa tjänster i Linux i både .NET Core och Java. I den här självstudiekursen kommer vi titta på hur toocreate ett program för Linux och skapa en tjänst med C# (.NET Core).

## <a name="prerequisites"></a>Krav
Du måste [konfigurera Linux-utvecklingsmiljön](service-fabric-get-started-linux.md) innan du börjar. Om du använder Mac OS X kan du [konfigurera en Linux-miljö på en virtuell dator med hjälp av Vagrant](service-fabric-get-started-mac.md).

Vill du även tooinstall hello [Service Fabric CLI](service-fabric-cli.md)

### <a name="install-and-set-up-hello-generators-for-csharp"></a>Installera och konfigurera hello generatorer för CSharp
Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa ett Service Fabric CSharp-program från terminalen med en Yeoman-mallgenerator. Följ hello stegen nedan tooensure som du har hello Service Fabric yeoman mall generator för CSharp som arbetar på din dator.
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
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-hello-application"></a>Skapa hello program
Ett Service Fabric-program kan innehålla en eller flera tjänster med en viss roll i att leverera hello-funktionerna. hello Service Fabric [Yeoman](http://yeoman.io/) generatorn för CSharp som du installerade i sista steget, gör det enkelt toocreate din första tjänst och tooadd mer senare. Nu ska vi använda Yeoman toocreate ett program med en enskild tjänst.

1. Skriv hello efter kommandot toostart skapa hello scaffold-teknik i en terminal:`yo azuresfcsharp`
2. Namnge ditt program.
3. Välj hello typ av din första tjänst och ge den namnet. Vi väljer en tillförlitlig tjänst för aktören hello enligt den här kursen.

   ![Service Fabric Yeoman-generator för C#][sf-yeoman]

> [!NOTE]
> Läs mer om alternativen för hello [Service Fabric programming översikt över säkerhetsmodell](service-fabric-choose-framework.md).
>
>

## <a name="build-hello-application"></a>Skapa hello program
hello Service Fabric Yeoman mallar innehåller ett build-skript som du kan använda toobuild hello-app från hello terminal (efter att navigera toohello programmappen).

  ```sh
 cd myapp
 ./build.sh
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

När programmet hello har distribuerats, öppna en webbläsare och gå till [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) på [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Expandera sedan hello **program** nod och Observera att det finns en post för din typ av program och en annan för hello första instansen av den typen.

## <a name="start-hello-test-client-and-perform-a-failover"></a>Starta hello test-klienten och utför en växling vid fel
Aktörsprojekt gör ingenting på egen hand. De kräver en annan tjänst eller klient toosend dem meddelanden. hello aktören mallen innehåller ett enkelt testskript som du kan använda toointeract med hello aktören-tjänsten.

1. Köra hello skript med hello titta på verktyget toosee hello utdata från hello aktören service.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. Leta upp nod som värd för hello primära repliken för hello aktören tjänsten i Service Fabric Explorer. I hello skärmbilden nedan är noden 3.

    ![Hitta hello primära repliken i Service Fabric Explorer][sfx-primary]
3. Klicka på hello nod du hittades i hello föregående steg och sedan välja **inaktivera (omstart)** hello Åtgärder-menyn. Den här åtgärden startar om en nod i klustret lokala för att tvinga en växling vid fel tooa sekundär replik körs på en annan nod. När du utför den här åtgärden betala uppmärksamhet toohello utdata från hello testklienten och Observera hello räknaren fortsätter tooincrement trots hello växling vid fel.

## <a name="adding-more-services-tooan-existing-application"></a>Lägga till fler tjänster tooan befintliga program

tooadd en annan tooan tjänstprogrammet redan skapats med hjälp av `yo`, utföra hello följande steg:
1. Ändra toohello rotkatalog till hello befintliga program.  Till exempel `cd ~/YeomanSamples/MyApplication`om `MyApplication` är hello-program som skapats av Yeoman.
2. Kör `yo azuresfcsharp:AddService`

## <a name="migrating-from-projectjson-toocsproj"></a>Migrera från project.json too.csproj
1. Kör 'dotnet migrera' i projektets rotkatalog kommer att migrera alla hello project.json toocsproj format.
2. Uppdatera hello projekt refererar till detta toocsproj filer i projektfiler.
3. Uppdatera hello projektet namn toocsproj-filer i build.sh.

## <a name="next-steps"></a>Nästa steg

* [Läs mer om Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interaktion med Service Fabric-kluster med hello Service Fabric CLI](service-fabric-cli.md)
* Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)
* [Kom igång med Service Fabric CLI](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
