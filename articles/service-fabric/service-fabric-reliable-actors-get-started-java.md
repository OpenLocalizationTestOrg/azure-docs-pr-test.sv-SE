---
title: "aaaCreate din första aktören-baserad Azure mikrotjänster i Java | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att skapa, felsöka och distribuera en enkel aktören-baserad tjänst med hjälp av Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d31dc8ab-9760-4619-a641-facb8324c759
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2017
ms.author: vturecek
ms.openlocfilehash: 24718a8d7034360c53597f139169580f1a6ce732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a>Komma igång med Reliable Actors
> [!div class="op_single_selector"]
> * [C# i Windows](service-fabric-reliable-actors-get-started.md)
> * [Java i Linux](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Den här artikeln beskriver hello grunderna i Azure Service Fabric Reliable Actors och vägleder dig genom att skapa och distribuera ett enkelt program tillförlitliga aktören i Java.

## <a name="installation-and-setup"></a>Installation och konfiguration
Innan du börjar kontrollera att du har hello Service Fabric-utvecklingsmiljö ställa in på din dator.
Om du behöver tooset den, gå för[komma igång på Mac](service-fabric-get-started-mac.md) eller [komma igång med Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Grundläggande begrepp
tooget igång med Reliable Actors du bara behöver toounderstand några grundläggande begrepp:

* **Aktören tjänsten**. Reliable Actors paketeras i Reliable Services som kan distribueras i hello Service Fabric-infrastruktur. Aktören instanser aktiveras på en namngiven tjänstinstans.
* **Aktören registrering**. Som med Reliable Services en tillförlitlig aktören tjänst behöver toobe som registrerats med hello Service Fabric runtime. Hello aktörstyp måste dessutom toobe som registrerats med hello aktören körning.
* **Aktören gränssnittet**. hello aktören gränssnittet är används toodefine ett starkt typifierad offentliga gränssnitt för en aktör. I hello tillförlitliga aktören modellen terminologi definierar hello aktören-gränssnittet hello typer av meddelanden som hello aktören kan förstå och bearbeta. hello aktören gränssnitt som används av andra aktörer och klientprogram för ”skicka” (asynkront) meddelanden toohello aktören. Reliable Actors kan implementera flera gränssnitt.
* **ActorProxy klassen**. Hej ActorProxy klass som används av klienten program tooinvoke hello metoder som exponeras via hello aktören gränssnitt. Hej ActorProxy klassen innehåller två viktiga funktioner:
  
  * Namnmatchning: det är kan toolocate hello aktören i hello kluster (Sök hello-nod i hello kluster där den finns).
  * Hantering av fel: den gör anrop av metoden och nytt lösa hello aktören platsen efter, t.ex, ett fel som kräver hello aktören toobe flyttas tooanother nod i klustret för hello.

hello enligt reglerna som gäller tooactor gränssnitt är värt att nämna:

* Aktören gränssnittsmetoder kan vara överbelastad.
* Aktören gränssnittet metoder inte får ha ut, ref eller valfria parametrar.
* Allmänt gränssnitt stöds inte.

## <a name="create-an-actor-service"></a>Skapa en aktören service
Börja med att skapa ett nytt Service Fabric-program. hello Service Fabric-SDK för Linux innehåller en Yeoman generator tooprovide hello scaffold-teknik för ett Service Fabric-program med en tillståndslös tjänst. Starta genom att köra hello följande Yeoman kommando:

```bash
$ yo azuresfjava
```

Följ hello instruktioner toocreate en **tillförlitliga aktören tjänsten**. Namn för den här självstudiekursen hello programmet ”HelloWorldActorApplication” och hello aktör ”HelloWorldActor”. hello följande scaffold-teknik kommer att skapas:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Tillförlitliga aktörer grundläggande byggstenarna
hello grundläggande koncept beskrivs tidigare översätta till hello grundläggande byggstenarna för en tillförlitlig aktören-tjänst.

### <a name="actor-interface"></a>Aktören gränssnitt
Hello gränssnittsdefinition för hello aktören innehåller. Det här gränssnittet definierar hello aktören kontrakt som delas av hello aktören implementering och hello klienter anropar hello aktören, så gör det vanligtvis meningsfullt toodefine den på en plats som är separata från hello aktören implementering och kan delas av flera andra tjänster eller program.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Aktören service
Innehåller den aktören implementeringen och aktören Registreringskod. hello aktören klassen implementerar hello aktören gränssnitt. Det är där din aktören sitt arbete.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Aktören registrering
hello aktören service måste ha registrerats med en typ i hello Service Fabric-körningsmiljön. I ordning för hello aktören Service toorun aktören-instanser, aktören-typen måste också vara registrerad med hello aktören Service. Hej `ActorRuntime` registreringsmetod utför detta arbete för aktörer.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {

    public static void main(String[] args) throws Exception {

        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);

        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testklienten
Detta är ett enkelt test-klientprogram kan du köra separat från hello Service Fabric application tootest aktören-tjänsten. Detta är ett exempel på där hello ActorProxy kan använda tooactivate och kommunicera med aktören instanser. Det inte distribueras med din tjänst.

### <a name="hello-application"></a>hello-program
Slutligen hello hello programpaket aktören tjänsten och andra tjänster som du kan lägga till i hello framtida tillsammans för distribution. Den innehåller hello *ApplicationManifest.xml* och platshållare för hello aktören service-paketet.

## <a name="run-hello-application"></a>Kör programmet hello

Hej Yeoman scaffold-teknik som omfattar en gradle skriptet toobuild hello program och bash-skript toodeploy och ta bort programmet. toodeploy hello program, första build hello med gradle:

```bash
$ gradle
```

Detta genererar ett Service Fabric-programpaket som kan distribueras med hjälp av Service Fabric CLI-verktygen.

### <a name="deploy-service-fabric-cli"></a>Distribuera Service Fabric CLI

Hej install.sh skriptet innehåller hello nödvändiga Service Fabric CLI (sfctl) kommandon toodeploy hello programpaket.
Kör hello install.sh skriptet toodeploy hello program.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Nästa steg

* [Kom igång med Service Fabric CLI](service-fabric-cli.md)
