---
title: 'Uppgradering av programmet: dataserialisering | Microsoft Docs'
description: "Metodtips för dataserialisering och hur den påverkar rullande programuppgraderingar."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: a5f36366-a2ab-4ae3-bb08-bc2f9533bc5a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 1b65dfd3813423550631490640a81953864f58e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>Hur dataserialisering påverkar en uppgradering av programmet
I en [löpande uppgradering av programmet](service-fabric-application-upgrade.md), hello uppgraderingen är tillämpade tooa del noder, en domän i taget. Under den här processen vissa uppgraderingsdomäner finns på hello nyare version av programmet, och vissa uppgraderingsdomäner hello äldre versioner av programmet. Under den här fasen hello hello ny version av programmet måste vara kan tooread hello gammal version av dina data och hello gammal version av programmet måste vara kan tooread hello ny version av dina data. Om hello dataformat inte framåt och bakåt kompatibla, hello-uppgraderingen kan misslyckas eller värre, data går förlorat eller skadat. Den här artikeln beskrivs vad som utgör ditt dataformat och ger bästa praxis för att säkerställa att dina data är framåt och bakåt kompatibel.

## <a name="what-makes-up-your-data-format"></a>Hur får din dataformat?
I Azure Service Fabric hello data som är sparade som replikeras som kommer från C#-klasser. För program som använder [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md), att data är hello objekt i hello tillförlitliga ordböcker och köer. För program som använder [Reliable Actors](service-fabric-reliable-actors-introduction.md), som är hello säkerhetskopiering tillstånd för hello aktören. Dessa klasser i C# måste kunna serialiseras toobe beständiga och replikeras. Därför definieras hello dataformatet av hello fält och egenskaper som serialiseras, samt hur de serialiseras. Till exempel i en `IReliableDictionary<int, MyClass>` hello data är en serialiserad `int` och en serialiserad `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Kod ändrar som resulterar i en data-formatändring
Eftersom hello dataformat bestäms av klasser i C#, kan ändringar toohello klasser orsaka en formatändring data. Var försiktig tooensure som en uppgradering kan hantera hello dataformat ändringen. Exempel som kan orsaka dataändringar format:

* Lägga till eller ta bort fält eller egenskaper
* Byta namn på fält eller egenskaper
* Ändra hello typer av fält eller egenskaper
* Ändra hello klassnamn eller namnområde

### <a name="data-contract-as-hello-default-serializer"></a>Datakontrakt som hello standard serialiseraren
hello serialiseraren används vanligtvis för Datainläsningen hello och avserialisering till hello aktuella versionen, även om hello data i en äldre eller *nyare* version. hello standard serialiseraren är hello [datakontrakt serialiseraren](https://msdn.microsoft.com/library/ms733127.aspx), som har väldefinierade versionshantering regler. Tillförlitliga samlingar kan hello serialiseraren toobe åsidosätts, men Reliable Actors för närvarande inte. hello data serialiseraren spelar en viktig roll i att aktivera löpande uppgraderingar. hello datakontrakt serialiseraren är hello serialiseraren som vi rekommenderar för Service Fabric-program.

## <a name="how-hello-data-format-affects-a-rolling-upgrade"></a>Hur påverkar hello dataformat löpande uppgradering
Det finns två huvudscenarier där hello serialiseraren kan stöta på en äldre under en uppgradering eller *nyare* version av dina data:

1. När en nod har uppgraderats och börjar säkerhetskopiera, laddar hello nya serialiseraren hello data beständiga toodisk av hello gamla versionen.
2. Under hello löpande uppgradering, innehåller hello kluster en blandning av hello gamla och nya versioner av din kod. Eftersom repliker kan placeras i olika uppgraderingsdomäner och repliker skicka data tooeach andra hello nya eller gamla kan versionen av data påträffas i hello nya eller gamla versionen av din serialiseraren.

> [!NOTE]
> Hej ”ny” och ”gamla versionen” här finns toohello version av din kod som körs. Hej ”nya serialiseraren” refererar toohello serialiseraren kod som körs i hello nya versionen av programmet. Hej ”nya data” refererar toohello serialiseras C#-klass från hello ny version av programmet.
> 
> 

Hej två versioner av koden och dataformat måste vara både framåt och bakåt kompatibel. Om de inte är kompatibla hello löpande uppgradering misslyckas eller data kan gå förlorade. hello löpande uppgradering misslyckas eftersom hello-kod eller serialiseraren kan utlösa undantag eller ett fel när den stöter på hello andra versionen. Data kan gå förlorade om du till exempel en ny egenskap har lagts till men hello gamla serialiseraren den under deserialisering.

Datakontrakt är hello rekommenderade lösningen för att säkerställa att dina data är kompatibel. Det har väldefinierade versionshantering regler för att lägga till, ta bort och ändra fält. Det har också stöd för behandlar okänt fält, koppla upp i hello serialisering och deserialisering processen och behandlar klassarv. Mer information finns i [med datakontrakt](https://msdn.microsoft.com/library/ms733127.aspx).

## <a name="next-steps"></a>Nästa steg
[Uppgradera ditt program med hjälp av Visual Studio](service-fabric-application-upgrade-tutorial.md) vägleder dig genom en uppgradering av programmet med hjälp av Visual Studio.

[Uppgradera ditt program med hjälp av Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vägleder dig genom en uppgradering av programmet med hjälp av PowerShell.

Styra hur programmet ska uppgraderas med hjälp av [uppgradera parametrar](service-fabric-application-upgrade-parameters.md).

Lär dig hur toouse avancerade funktioner när du uppgraderar ditt program genom att referera för[avancerade ämnen](service-fabric-application-upgrade-advanced.md).

Lösa vanliga problem i programuppgraderingar genom att referera toohello stegen i [felsökning programuppgraderingar ](service-fabric-application-upgrade-troubleshooting.md).

