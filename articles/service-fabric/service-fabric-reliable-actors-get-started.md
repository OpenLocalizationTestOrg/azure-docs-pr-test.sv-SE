---
title: "aaaCreate din första aktören-baserad Azure mikrotjänster i C# | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att skapa, felsöka och distribuera en enkel aktören-baserad tjänst med hjälp av Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ab4f75bef0adb6e70f0ead587475b3fb51e6e6a5
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

Den här artikeln beskriver hello grunderna i Azure Service Fabric Reliable Actors och vägleder dig genom att skapa, felsöka och distribuera ett enkelt tillförlitliga aktören program i Visual Studio.

## <a name="installation-and-setup"></a>Installation och konfiguration
Innan du börjar bör du kontrollera att du har hello Service Fabric-utvecklingsmiljö ställa in på din dator.
Om du behöver tooset den, se detaljerade anvisningar på [hur tooset hello utvecklingsmiljön](service-fabric-get-started.md).

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

## <a name="create-a-new-project-in-visual-studio"></a>Skapa ett nytt projekt i Visual Studio
Starta Visual Studio 2015 eller Visual Studio 2017 som administratör och skapa ett nytt projekt för Service Fabric-program:

![Service Fabric-verktyg för Visual Studio – nytt projekt][1]

Hello nästa i dialogrutan kan du välja hello typ av projekt som du vill toocreate.

![Service Fabric-projektmallar][5]

För hello HelloWorld projektet ska vi använda hello Reliable Actors för Service Fabric-tjänsten.

När du har skapat hello-lösning bör du se hello följande struktur:

![Struktur för Service Fabric-projekt][2]

## <a name="reliable-actors-basic-building-blocks"></a>Tillförlitliga aktörer grundläggande byggstenarna
En typisk Reliable Actors lösning består av tre projekt:

* **hello projektet (MyActorApplication)**. Detta är hello-projekt som paket alla hello tillsammans för distribution. Den innehåller hello *ApplicationManifest.xml* och PowerShell-skript för att hantera hello program.
* **hello gränssnittet projekt (MyActor.Interfaces)**. Detta är hello-projekt som innehåller hello gränssnittsdefinition för hello aktören. Du kan definiera hello-gränssnitt som ska användas av hello aktörer i hello lösning i hello MyActor.Interfaces-projektet. Gränssnitten aktören kan definieras i ett projekt med ett namn, men hello-gränssnittet definierar hello aktören kontrakt som delas av hello aktören implementering och hello klienter anropar hello aktören, så gör det vanligtvis meningsfullt toodefine den i en sammansättning som är separata från hello aktören implementering och kan delas av flera projekt.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **hello aktören service-projekt (MyActor)**. Detta är hello projekt används toodefine hello Service Fabric-tjänsten som är pågående toohost hello aktören. Den innehåller hello implementering av hello aktören. En aktören implementering är en klass som härleds från hello bastypen `Actor` och implementerar hello gränssnitt som är definierade i hello MyActor.Interfaces projekt. En aktörklass måste också implementera en konstruktor som accepterar ett `ActorService` instans och en `ActorId` och skickar dem toohello grundläggande `Actor` klass. Detta ger konstruktorn beroendeinmatning plattform beroenden.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

hello aktören service måste ha registrerats med en typ i hello Service Fabric-körningsmiljön. I ordning för hello aktören Service toorun aktören-instanser, aktören-typen måste också vara registrerad med hello aktören Service. Hej `ActorRuntime` registreringsmetod utför detta arbete för aktörer.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Om du startar från ett nytt projekt i Visual Studio och du bara ha en aktören definition, med hello registrering som standard i hello-kod som genereras av Visual Studio. Om du definierar andra aktörer i hello tjänsten behöver tooadd hello aktören registrering genom att använda:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> hello Service Fabric aktörer runtime skickar vissa [händelser och Prestandaräknare relaterade tooactor metoder](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). De är användbara i diagnostik- och prestandaövervakning.
> 
> 

## <a name="debugging"></a>Felsökning
hello Service Fabric-verktyg för Visual Studio stöder felsökning på den lokala datorn. Du kan starta en felsökning av hitting hello F5-tangenten. Visual Studio skapar (vid behov) paket. Den distribuerar programmet hello på hello lokala Service Fabric-kluster och bifogar hello felsökare.

Under processen för distribution av hello ser du hello förlopp i hello **utdata** fönster.

![Service Fabric felsökning utdatafönstret][3]

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [hur Reliable Actors använda hello Service Fabric-plattformen](service-fabric-reliable-actors-platform.md).

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
