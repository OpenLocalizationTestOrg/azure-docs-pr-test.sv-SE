---
title: "Skapa din första Azure aktören-baserade-mikrotjänster i Java | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom stegen för att skapa, felsöka och distribuera en enkel aktören-baserad tjänst med hjälp av Service Fabric Reliable Actors."
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
ms.openlocfilehash: 288f1ed1016f50031065e66444d2562427194dc7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-reliable-actors"></a>Komma igång med Reliable Actors
> [!div class="op_single_selector"]
> * [C# i Windows](service-fabric-reliable-actors-get-started.md)
> * [Java i Linux](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Den här artikeln beskriver grunderna i Azure Service Fabric Reliable Actors och vägleder dig genom att skapa och distribuera ett enkelt program tillförlitliga aktören i Java.

## <a name="installation-and-setup"></a>Installation och konfiguration
Innan du börjar bör du kontrollera du har Service Fabric-utvecklingsmiljö ställa in på din dator.
Om du behöver konfigurera den går du till [komma igång på Mac](service-fabric-get-started-mac.md) eller [komma igång med Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Grundläggande begrepp
Om du vill komma igång med Reliable Actors, behöver du bara förstå några grundläggande begrepp:

* **Aktören tjänsten**. Reliable Actors paketeras i Reliable Services som kan distribueras i Service Fabric-infrastruktur. Aktören instanser aktiveras på en namngiven tjänstinstans.
* **Aktören registrering**. Som med Reliable Services måste en tillförlitlig aktören tjänst vara registrerad med Service Fabric-körning. Dessutom måste aktören-typen vara registrerad med aktören körningsmiljön.
* **Aktören gränssnittet**. Gränssnittet aktören används för att definiera en strikt typkontroll offentliga gränssnittet för en aktör. I den tillförlitliga aktören modellen terminologin definierar aktören-gränssnittet vilka typer av meddelanden som aktören kan förstå och processen. Gränssnittet aktören används av andra aktörer och klientprogram för att ”skicka” (asynkront) meddelanden för aktören. Reliable Actors kan implementera flera gränssnitt.
* **ActorProxy klassen**. Klassen ActorProxy används av klientprogram för att anropa metoder som exponeras via gränssnittet aktören. Klassen ActorProxy innehåller två viktiga funktioner:
  
  * Namnmatchning: det är att hitta aktören i klustret (hitta nod i klustret där den finns).
  * Hantering av fel: den kan gör anrop av metoden och lösa aktören platsen igen efter, till exempel ett fel som kräver aktören att flyttas till en annan nod i klustret.

Följande regler som hör till aktören gränssnitt är värt att nämna:

* Aktören gränssnittsmetoder kan vara överbelastad.
* Aktören gränssnittet metoder inte får ha ut, ref eller valfria parametrar.
* Allmänt gränssnitt stöds inte.

## <a name="create-an-actor-service"></a>Skapa en aktören service
Börja med att skapa ett nytt Service Fabric-program. Service Fabric-SDK för Linux innehåller en Yeoman generator att tillhandahålla scaffold-teknik för ett Service Fabric-program med en tillståndslös tjänst. Starta genom att köra följande Yeoman kommando:

```bash
$ yo azuresfjava
```

Följ instruktionerna för att skapa en **tillförlitliga aktören tjänsten**. Namn för programmet ”HelloWorldActorApplication” för den här självstudiekursen och aktör ”HelloWorldActor”. Scaffold-teknik för följande skapas:

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
Grundläggande begrepp som beskrivs ovan översätta till de grundläggande byggstenarna för en tillförlitlig aktören-tjänst.

### <a name="actor-interface"></a>Aktören gränssnitt
Innehåller gränssnittsdefinition för aktören. Det här gränssnittet definierar aktören kontraktet som delas av aktören implementering och klienterna som anropar aktören så vanligtvis är det praktiskt att definiera den på en plats som är separat från aktören implementering och kan delas av flera tjänster eller klientprogram.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Aktören service
Innehåller den aktören implementeringen och aktören Registreringskod. Aktören-klassen implementerar gränssnittet aktören. Det är där din aktören sitt arbete.

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
Tjänsten aktören måste ha registrerats med en typ i Service Fabric-körningsmiljön. För att tjänsten aktören körs aktören-instanser måste aktörstyp registreras med tjänsten aktören. Den `ActorRuntime` registreringsmetod utför detta arbete för aktörer.

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
Detta är ett enkelt test-klientprogram kan du köra separat från Service Fabric-programmet för att testa aktören tjänsten. Detta är ett exempel där ActorProxy kan användas för att aktivera och kommunicera med aktören instanser. Det inte distribueras med din tjänst.

### <a name="the-application"></a>Programmet
Slutligen paket programmet för tjänsten aktören och andra tjänster som du kan lägga till i framtiden tillsammans för distribution. Den innehåller den *ApplicationManifest.xml* och platshållare för servicepaket aktören.

## <a name="run-the-application"></a>Köra programmet

Yeoman scaffold-teknik innehåller ett gradle-skript för att skapa programmet och bash-skript för att distribuera och ta bort programmet. Om du vill distribuera programmet först skapa programmet med gradle:

```bash
$ gradle
```

Detta genererar ett Service Fabric-programpaket som kan distribueras med hjälp av Service Fabric CLI-verktygen.

### <a name="deploy-service-fabric-cli"></a>Distribuera Service Fabric CLI

Skriptet install.sh innehåller de nödvändiga Service Fabric CLI (sfctl)-kommandona för att distribuera programpaketet.
Kör skriptet install.sh om du vill distribuera programmet.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Nästa steg

* [Kom igång med Service Fabric CLI](service-fabric-cli.md)
