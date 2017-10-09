---
title: "aaaCreate din första tillförlitliga mikrotjänster i Java Azure | Microsoft Docs"
description: "Introduktion toocreating ett Microsoft Azure Service Fabric-program med tillståndslösa och tillståndskänsliga tjänster."
services: service-fabric
documentationcenter: java
author: vturecek
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 577d96591797bbfe6be5c1094426b5f1435cca0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a>Kom igång med Reliable Services
> [!div class="op_single_selector"]
> * [C# i Windows](service-fabric-reliable-services-quick-start.md)
> * [Java i Linux](service-fabric-reliable-services-quick-start-java.md)
>
>

Den här artikeln beskriver hello grunderna i Azure Service Fabric Reliable Services och vägleder dig genom att skapa och distribuera en enkel tillförlitliga tjänstprogrammet skriven i Java. Den här videon Microsoft Virtual Academy visar också hur toocreate en tillståndslös tillförlitlig tjänst:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a>Installation och konfiguration
Innan du börjar kontrollera att du har hello Service Fabric-utvecklingsmiljö ställa in på din dator.
Om du behöver tooset den, gå för[komma igång på Mac](service-fabric-get-started-mac.md) eller [komma igång med Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Grundläggande begrepp
tooget igång med tillförlitlig tjänster kan du bara behöver toounderstand några grundläggande begrepp:

* **Typen tjänst**: det här är din implementering. Den definieras av hello-klass som du skriver och som utökar `StatelessService` och annan kod eller beroenden, används tillsammans med ett namn och ett versionsnummer.
* **Med namnet tjänstinstans**: toorun din tjänst du skapar namngivna instanser av service-typen mycket som du skapar objektinstanser av en klasstyp. Instanser av tjänsten är i själva verket objektinstansieringar av din tjänstklass som du skriver.
* **Tjänstvärden**: hello med namnet tjänstinstanser som du skapar måste toorun inuti en värd. hello tjänstvärden är bara en process där instanser av tjänsten kan köras.
* **Registrering av tjänst**: registrering sammanför allt. hello service-typen måste vara registrerad med hello Service Fabric runtime i en tjänst värd tooallow Service Fabric toocreate instanser av den toorun.  

## <a name="create-a-stateless-service"></a>Skapa en tillståndslös tjänst
Börja med att skapa ett Service Fabric-program. hello Service Fabric-SDK för Linux innehåller en Yeoman generator tooprovide hello scaffold-teknik för ett Service Fabric-program med en tillståndslös tjänst. Starta genom att köra hello följande Yeoman kommando:

```bash
$ yo azuresfjava
```

Följ hello instruktioner toocreate en **tillförlitliga tillståndslösa tjänsten**. Den här självstudien namn hello programmet ”HelloWorldApplication” och hello-tjänsten ”HelloWorld”. omfattar hello resultatet kataloger för hello `HelloWorldApplication` och `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-hello-service"></a>Implementera hello-tjänsten
Öppna **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Den här klassen definierar hello service-typen och köra all kod. hello service API innehåller två startpunkter för din kod:

* En öppen metoden, kallas `runAsync()`, där du kan börja köra alla arbetsbelastningar, inklusive tidskrävande beräkning av arbetsbelastningar.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* En kommunikation startpunkt där du kan ansluta i din kommunikation stack med val. Det är där du kan börja ta emot begäranden från användare och andra tjänster.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

I den här självstudiekursen kommer vi fokusera på hello `runAsync()` metoden. Det är där du kan starta direkt köra din kod.

### <a name="runasync"></a>RunAsync
hello plattform anropar den här metoden när en instans av en tjänst är monterade och klar tooexecute. För en tillståndslös tjänst innebär som bara när hello tjänstinstans öppnas. En token för annullering tillhandahålls toocoordinate när service-instans måste toobe stängd. I Service Fabric inträffa cykeln Öppna/stänga av en tjänstinstans många gånger under hello livslängd för hello-tjänsten som helhet. Detta kan inträffa av olika orsaker, bland annat:

* hello flyttas instanser av tjänsten för resurser.
* Fel uppstår i koden.
* hello programmets eller systemets uppgraderas.
* hello underliggande maskinvara påträffar ett avbrott.

Den här orchestration hanteras av Service Fabric tookeep tjänsten hög tillgänglighet och korrekt Balanserat.

`runAsync()`bör inte blockera synkront. Implementeringen av runAsync ska returnera en CompletableFuture tooallow hello runtime toocontinue. Om din arbetsbelastning behöver tooimplement en tidskrävande uppgift som bör göras i hello CompletableFuture.

#### <a name="cancellation"></a>Annullering
Annullering av din arbetsbelastning är en samverkande ansträngning styrd av hello tillhandahålls annullering token. hello systemet väntar aktiviteten-tooend (av lyckades, avbokning eller fel) innan den flyttas på. Det är viktigt toohonor hello annullering token, Slutför någon fungerar och avsluta `runAsync()` så snabbt som möjligt när hello systemet begär annullering. hello exemplet nedan visar hur toohandle en annullering händelse:

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace hello following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a>Tjänstregistrering
Tjänsttyper måste registreras med hello Service Fabric runtime. hello service-typen är definierad i hello `ServiceManifest.xml` och tjänstklassen som implementerar `StatelessService`. Registreringen för tjänsten utförs i hello processen startpunkten. I det här exemplet hello processen Huvudstartadressen är `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-hello-application"></a>Kör programmet hello

Hej Yeoman scaffold-teknik som omfattar en gradle skriptet toobuild hello program och bash-skript toodeploy och ta bort programmet. toorun hello program, första build hello med gradle:

```bash
$ gradle
```

Detta ger ett Service Fabric-programpaket som kan distribueras med hjälp av Service Fabric CLI.

### <a name="deploy-with-service-fabric-cli"></a>Distribuera med Service Fabric CLI

Hej install.sh skriptet innehåller hello nödvändiga Service Fabric CLI-kommandon toodeploy hello programpaket. Kör install.sh skriptet toodeploy hello program.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Nästa steg

* [Kom igång med Service Fabric CLI](service-fabric-cli.md)
