---
title: "Skapa din första Azure-mikrotjänstapp i Linux med hjälp av C# | Microsoft Docs"
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
ms.date: 9/19/2017
ms.author: subramar
ms.openlocfilehash: e18dcad73486ab7610c53c269fbc81de73b5147e
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/18/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a>Skapa ditt första Azure Service Fabric-program
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux (förhandsversion)](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux (förhandsversion)](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Service Fabric innehåller SDK:er för att skapa tjänster i Linux i både .NET Core och Java. I den här självstudien visar vi hur man skapar ett program för Linux och en tjänst med C# i .NET Core 2.0.

## <a name="prerequisites"></a>Krav
Du måste [konfigurera Linux-utvecklingsmiljön](service-fabric-get-started-linux.md) innan du börjar. Om du använder Mac OS X kan du [konfigurera en Linux-miljö på en virtuell dator med hjälp av Vagrant](service-fabric-get-started-mac.md).

Du bör även installera [Service Fabric CLI](service-fabric-cli.md)

### <a name="install-and-set-up-the-generators-for-c"></a>Installera och konfigurera generatorerna för C#
Service Fabric tillhandahåller ramverktyg som hjälper dig att skapa Service Fabric-program från en terminal med en Yeoman-mallgenerator. Följ dessa steg för att konfigurera Service Fabric Yeoman-mallgeneratorer för C#:

1. Installera nodejs och NPM på datorn

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Installera [Yeoman](http://yeoman.io/)-mallgeneratorn på datorn från NPM

  ```bash
  sudo npm install -g yo
  ```
3. Installera Service Fabric Yeo Java-programgeneratorn från NPM

  ```bash
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-the-application"></a>Skapa programmet
Ett Service Fabric-program kan innehålla en eller flera tjänster, som var och en ansvarar för att leverera programmets funktioner. Service Fabric [Yeoman](http://yeoman.io/)-generatorn för C#, som du installerade i förra steget, gör det enkelt att skapa din första tjänst och lägga till fler senare. Använd Yeoman för att skapa ett program med en enskild tjänst.

1. Skriv följande kommando i en terminal, för att börja bygga ställningarna: `yo azuresfcsharp`
2. Namnge ditt program.
3. Välj vilken typ din första tjänst ska ha och namnge den. För den här självstudien väljer vi en Reliable Actor-tjänst.

   ![Service Fabric Yeoman-generator för C#][sf-yeoman]

> [!NOTE]
> Mer information om alternativen finns i [Översikt över Service Fabric-programmeringsmodell](service-fabric-choose-framework.md).
>
>

## <a name="build-the-application"></a>Skapa programmet
Service Fabric Yeoman-mallarna inkluderar ett byggskript som du kan använda för att skapa programmet från terminalen (efter att du navigerat till programmappen).

  ```sh
 cd myapp
 ./build.sh
  ```

## <a name="deploy-the-application"></a>Distribuera programmet

När du har skapat programmet kan du distribuera det till det lokala klustret.

1. Anslut till det lokala Service Fabric-klustret.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Kör installationsskriptet som medföljer mallen för att kopiera programpaketet till klustrets avbildningsarkiv, registrera programtypen och skapa en instans av programmet.

    ```bash
    ./install.sh
    ```

Distributionen går till på samma sätt som för andra Service Fabric-program. Detaljerade instruktioner finns i dokumentationen om att [hantera ett Service Fabric-program med Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md).

Du hittar parametrarna till de här kommandona i de genererade manifesten i programpaketet.

När programmet har distribuerats öppnar du en webbläsare och går till [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) på adressen [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Expandera sedan noden **Program** och observera att det nu finns en post för din programtyp och en post för den första instansen av den typen.

## <a name="start-the-test-client-and-perform-a-failover"></a>Starta testklienten och utför en redundansväxling
Aktörsprojekt gör ingenting på egen hand. Det behövs en annan tjänst eller klient för att skicka meddelanden till dem. Aktörsmallen innehåller ett enkelt testskript som du kan använda för att interagera med aktörstjänsten.

1. Kör skriptet med övervakningsverktyget för att se resultatet av aktörstjänsten.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. I Service Fabric Explorer letar du reda på noden där den primära repliken för aktörstjänsten finns. På skärmbilden nedan är det nod 3.

    ![Hitta den primära repliken i Service Fabric Explorer][sfx-primary]
3. Klicka på noden du hittade i föregående steg och välj sedan **Deactivate (restart)** (Inaktivera (omstart)) på menyn Actions (Åtgärder). Den här åtgärden startar om en nod i ditt lokala kluster, vilket framtvingar en redundansväxling till en sekundär replik som körs på en annan nod. När du utför åtgärden, ska du vara uppmärksam på utdata från testklienten och notera att räknaren fortsätter att öka trots redundansen.

## <a name="adding-more-services-to-an-existing-application"></a>Lägga till fler tjänster till ett befintligt program

Om du vill lägga till en till tjänst till ett program som redan har skapats med hjälp av `yo` utför du följande steg:
1. Ändra katalogen till roten för det befintliga programmet.  Till exempel `cd ~/YeomanSamples/MyApplication` om `MyApplication` är programmet som skapats av Yeoman.
2. Kör `yo azuresfcsharp:AddService`

## <a name="migrating-from-projectjson-to-csproj"></a>Migrera från project.json till .csproj
1. Om du kör ”dotnet migrate” i projektets rotkatalog migreras alla project.json-filer till csproj-format.
2. Uppdatera projektreferenserna till csproj-filer i projektfiler.
3. Uppdatera projektfilnamnen till csproj-filer i build.sh.

## <a name="next-steps"></a>Nästa steg

* [Interagera med Service Fabric-kluster med Service Fabric CLI](service-fabric-cli.md)
* Lär dig mer om [Service Fabric-supportalternativen](service-fabric-support.md)
* [Kom igång med Service Fabric CLI](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
