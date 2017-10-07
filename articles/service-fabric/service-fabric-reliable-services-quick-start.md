---
title: "aaaCreate ditt första Service Fabric-program i C# | Microsoft Docs"
description: "Introduktion toocreating ett Microsoft Azure Service Fabric-program med tillståndslösa och tillståndskänsliga tjänster."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d9b44d75-e905-468e-b867-2190ce97379a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2017
ms.author: vturecek
ms.openlocfilehash: e95e67cc84be1b83c936b250cae9112ddc77b963
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

Ett Azure Service Fabric-program innehåller en eller flera tjänster som körs din kod. Den här guiden visar hur toocreate både tillståndslösa och tillståndskänsliga Service Fabric-program med [Reliable Services](service-fabric-reliable-services-introduction.md).  Den här videon Microsoft Virtual Academy visar också hur toocreate en tillståndslös tillförlitlig tjänst:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=s39AO76yC_7206218965">  
<img src="./media/service-fabric-reliable-services-quick-start/ReliableServicesVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="basic-concepts"></a>Grundläggande begrepp
tooget igång med tillförlitlig tjänster kan du bara behöver toounderstand några grundläggande begrepp:

* **Typen tjänst**: det här är din implementering. Den definieras av hello-klass som du skriver och som utökar `StatelessService` och annan kod eller beroenden, används tillsammans med ett namn och ett versionsnummer.
* **Med namnet tjänstinstans**: toorun din tjänst du skapar namngivna instanser av service-typen mycket som du skapar objektinstanser av en klasstyp. En tjänstinstans har ett namn i hello form av en URI med hello ”fabric: /” schemat, till exempel ”fabric: / MyApp/MyService”.
* **Tjänstvärden**: hello med namnet tjänstinstanser som du skapar måste toorun inom en värdprocess. hello tjänstvärden är bara en process där instanser av tjänsten kan köras.
* **Registrering av tjänst**: registrering sammanför allt. hello service-typen måste vara registrerad med hello Service Fabric runtime i en tjänst värd tooallow Service Fabric toocreate instanser av den toorun.  

## <a name="create-a-stateless-service"></a>Skapa en tillståndslös tjänst
En tillståndslös tjänst är en typ av tjänst som används för närvarande hello normen i molnprogram. Anses tillståndslös eftersom själva hello-tjänsten inte innehåller data som behöver toobe lagras på ett tillförlitligt sätt eller hög tillgänglighet. Om en instans av en tillståndslösa tjänsten stängs av, förloras alla det interna tillståndet. I den här typen av tjänsten tillstånd måste vara beständiga tooan extern butik, till exempel Azure-tabeller eller en SQL-databas i den toobe gjort hög tillgänglighet och tillförlitlig.

Starta Visual Studio 2015 eller Visual Studio 2017 som administratör och skapa ett nytt Service Fabric application projekt med namnet *HelloWorld*:

![Använd hello nytt projekt dialogrutan rutan toocreate ett nytt Service Fabric-program](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject.png)

Skapa ett projekt för tillståndslösa tjänsten med namnet *HelloWorldStateless*:

![Skapa ett projekt för tillståndslösa tjänsten i hello andra dialogrutan](media/service-fabric-reliable-services-quick-start/hello-stateless-NewProject2.png)

Din lösning innehåller nu två projekt:

* *HelloWorld*. Detta är hello *programmet* projekt som innehåller din *services*. Den innehåller också hello programmanifestet som beskriver hello program, samt ett antal PowerShell-skript som hjälper dig toodeploy ditt program.
* *HelloWorldStateless*. Detta är hello service-projekt. Den innehåller hello tillståndslösa tjänsten implementering.

## <a name="implement-hello-service"></a>Implementera hello-tjänsten
Öppna hello **HelloWorldStateless.cs** filen i hello service-projekt. En tjänst kan köra all affärslogik i Service Fabric. hello service API innehåller två startpunkter för din kod:

* En öppen metoden, kallas *RunAsync*, där du kan börja köra alla arbetsbelastningar, inklusive tidskrävande beräkning av arbetsbelastningar.

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    ...
}
```

* En kommunikation startpunkt där du kan ansluta din kommunikation stack föredrar, till exempel ASP.NET Core. Det är där du kan börja ta emot begäranden från användare och andra tjänster.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}
```

I den här självstudiekursen fokuserar vi på hello `RunAsync()` metoden. Det är där du kan starta direkt köra din kod.
hello projektmall innehåller ett exempel på implementering av `RunAsync()` som ökar en löpande uppräkning.

> [!NOTE]
> Mer information om hur toowork med en kommunikation stacken finns [Service Fabric Web API-tjänster med OWIN själva värd](service-fabric-reliable-services-communication-webapi.md)
> 
> 

### <a name="runasync"></a>RunAsync
```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    long iterations = 0;

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        ServiceEventSource.Current.ServiceMessage(this, "Working-{0}", ++iterations);

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

hello plattform anropar den här metoden när en instans av en tjänst är monterade och klar tooexecute. För en tillståndslös tjänst innebär som bara när hello tjänstinstans öppnas. En token för annullering tillhandahålls toocoordinate när service-instans måste toobe stängd. I Service Fabric inträffa cykeln Öppna/stänga av en tjänstinstans många gånger under hello livslängd för hello-tjänsten som helhet. Detta kan inträffa av olika orsaker, bland annat:

* hello flyttas instanser av tjänsten för resurser.
* Fel uppstår i koden.
* hello programmets eller systemets uppgraderas.
* hello underliggande maskinvara påträffar ett avbrott.

Den här orchestration hanteras av hello system tookeep tjänsten hög tillgänglighet och korrekt Balanserat.

`RunAsync()`bör inte blockera synkront. Implementeringen av RunAsync ska returnera en uppgift eller await på alla tidskrävande eller blockerar åtgärder tooallow hello runtime toocontinue. Notera i hello `while(true)` loop i hello föregående exempel är en åtgärdsreturnerande `await Task.Delay()` används. Om din arbetsbelastning måste blockerar synkront kan du schemalägga en ny uppgift med `Task.Run()` i din `RunAsync` implementering.

Annullering av din arbetsbelastning är en samverkande ansträngning styrd av hello tillhandahålls annullering token. hello systemet väntar på uppgiften-tooend (som lyckades, avbokning eller fel) innan den flyttas på. Det är viktigt toohonor hello annullering token, Slutför någon fungerar och avsluta `RunAsync()` så snabbt som möjligt när hello systemet begär annullering.

I det här exemplet tillståndslösa tjänsten lagras hello antal i en lokal variabel. Men eftersom detta är en tillståndslös tjänst hello-värde som lagras finns bara för hello aktuella livscykeln för dess tjänstinstansen. När tjänsten hello flyttas eller startas om, går hello värdet förlorad.

## <a name="create-a-stateful-service"></a>Skapa en tillståndskänslig tjänst
Service Fabric introducerar en ny typ av tjänst som är tillståndskänslig. En tillståndskänslig service kan upprätthålla tillstånd på ett tillförlitligt sätt inom hello själva tjänsten, samordnad med hello-kod som använder den. Tillstånd görs högtillgänglig av Service Fabric utan hello måste toopersist tillstånd tooan extern butik.

tooconvert ett värde för prestandaräknaren från tillståndslös toohighly tillgänglig och beständiga när hello service flyttas eller startas om, behöver du en tillståndskänslig tjänst.

Hej i samma *HelloWorld* programmet, du kan lägga till en ny tjänst genom att högerklicka på hello Services referenser i hello projektet och välja **Lägg till ny tjänst för Service Fabric ->**.

![Lägg till en service tooyour Service Fabric-program](media/service-fabric-reliable-services-quick-start/hello-stateful-NewService.png)

Välj **tillståndskänslig Service** och ger den namnet *HelloWorldStateful*. Klicka på **OK**.

![Använd hello nytt projekt dialogrutan rutan toocreate en ny tillståndskänslig Service Fabric-tjänst](media/service-fabric-reliable-services-quick-start/hello-stateful-NewProject.png)

Ditt program bör nu ha två tjänster: hello tillståndslösa tjänsten *HelloWorldStateless* och hello tillståndskänslig service *HelloWorldStateful*.

En tillståndskänslig tjänst har hello samma startpunkter som en tillståndslös tjänst. hello största skillnaden är hello tillgängligheten för en *tillståndsprovidern* som kan lagra tillstånd på ett tillförlitligt sätt. Service Fabric medföljer en implementering av provider för tillstånd kallas [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md), vilket kan du skapa replikerade datastrukturer via hello tillförlitliga Tillståndshanterare. En tillståndskänslig tillförlitlig tjänst använder den här tillståndsprovidern som standard.

Öppna **HelloWorldStateful.cs** i *HelloWorldStateful*, som innehåller hello följande RunAsync-metoden:

```csharp
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace hello following sample code with your own logic
    //       or remove this RunAsync override if it's not needed in your service.

    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");

            ServiceEventSource.Current.ServiceMessage(this, "Current Counter Value: {0}",
                result.HasValue ? result.Value.ToString() : "Value does not exist.");

            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, hello transaction aborts, all changes are
            // discarded, and nothing is saved toohello secondary replicas.
            await tx.CommitAsync();
        }

        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
```

### <a name="runasync"></a>RunAsync
`RunAsync()`fungerar på samma sätt i tillståndskänsliga och tillståndslösa tjänster. Dock i en tillståndskänslig service hello plattform utför ytterligare arbete för din räkning innan den kör `RunAsync()`. Detta verk kan inkludera att säkerställa att hello tillförlitliga Tillståndshanterare och tillförlitlig samlingar är klar toouse.

### <a name="reliable-collections-and-hello-reliable-state-manager"></a>Tillförlitliga samlingar och hello tillförlitliga Tillståndshanterare
```csharp
var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
```

[IReliableDictionary](https://msdn.microsoft.com/library/dn971511.aspx) är en dictionary-implementering som du kan använda tooreliably arkivtillståndet i hello-tjänsten. Med Service Fabric och tillförlitlig samlingar kan du lagra data direkt i din tjänst utan hello behovet av en extern beständiga arkivet. Tillförlitliga samlingar gör att dina data är hög tillgänglighet. Service Fabric åstadkommer detta genom att skapa och hantera flera *repliker* av tjänsten för dig. Det ger också en API som avlägsnar bort hello svårigheter för att hantera dessa repliker och deras tillståndsövergångar.

Tillförlitliga samlingar kan lagra alla .NET-typ, inklusive din anpassade typer med några varningar.

* Service Fabric gör ditt tillstånd hög tillgänglighet av *replikerar* tillstånd mellan noder och tillförlitlig samlingar lagra data toolocal disken på varje replik. Det innebär att allt som lagras i tillförlitliga samlingar måste vara *serialiserbara*. Som standard använder tillförlitliga samlingar [DataContract](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractattribute%28v=vs.110%29.aspx) för serialisering, så det är viktigt toomake till att din typer är [stöds av hello datakontraktserialiserren](https://msdn.microsoft.com/library/ms731923%28v=vs.110%29.aspx) när du använder hello standard serialiserare.
* Objekt replikeras för hög tillgänglighet när du gör transaktioner på tillförlitliga samlingar. Objekt som lagras i tillförlitliga samlingar sparas i lokalt minne i din tjänst. Det innebär att du har en lokal referens toohello objekt.
  
   Det är viktigt att du inte mutera lokala instanser av dessa objekt utan att utföra en uppdateringsåtgärd på hello tillförlitliga samling i en transaktion. Det beror på att ändringarna toolocal objektinstanser inte replikeras automatiskt. Du måste infoga igen hello objektet tillbaka till hello ordlista eller Använd en av hello *uppdatera* metoder i hello ordlistan.

hello tillförlitliga Tillståndshanterare hanterar tillförlitliga samlingar åt dig. Du kan bara begära hello tillförlitliga Tillståndshanterare för en tillförlitlig samling efter namn när som helst och var som helst i din tjänst. hello tillförlitliga Tillståndshanterare garanterar att du får en referens tillbaka. Inte rekommenderar vi att du sparar referenser tooreliable samling instanser i klassmedlemsvariabler eller egenskaper. Särskild försiktighet måste vidtas tooensure hello referens har angetts tooan instans alltid i hello service livscykel. hello tillförlitliga Tillståndshanterare hanterar det här fungerar och har optimerats för Upprepa besök.

### <a name="transactional-and-asynchronous-operations"></a>Transaktionell och asynkrona åtgärder
```C#
using (ITransaction tx = this.StateManager.CreateTransaction())
{
    var result = await myDictionary.TryGetValueAsync(tx, "Counter-1");

    await myDictionary.AddOrUpdateAsync(tx, "Counter-1", 0, (k, v) => ++v);

    await tx.CommitAsync();
}
```

Tillförlitliga samlingar har många hello samma åtgärder som deras `System.Collections.Generic` och `System.Collections.Concurrent` motsvarigheter göra, förutom LINQ. Åtgärder på tillförlitliga samlingar är asynkron. Detta är eftersom skrivåtgärder med tillförlitlig samlingar utföra tooreplicate för i/o-åtgärder och bevara data toodisk.

Åtgärder för insamling av tillförlitliga är *transaktionella*, så att du kan behålla tillstånd konsekvent över flera tillförlitliga samlingar och åtgärder. Du kan till exempel ett arbetsobjekt från en tillförlitlig kö har status Created, utföra en åtgärd på det och spara hello resultat i en tillförlitlig Dictionary alla inom en enskild transaktion. Detta behandlas som en atomisk åtgärd och det garanterar hello hela åtgärden lyckas eller hello hela åtgärden återställs. Om ett fel uppstår när du har status Created hello objektet men innan du sparar hello resultatet hello hela transaktionen återställs och hello objekt förblir i hello kö för bearbetning.

## <a name="run-hello-application"></a>Kör programmet hello
Returnerar vi nu toohello *HelloWorld* program. Du kan nu skapa och distribuera tjänster. När du trycker på **F5**, ditt program kommer att byggas och distribuerade tooyour lokala klustret.

När hello-tjänster startar körs, kan du visa hello genereras ETW Event Tracing for Windows ()-händelser i en **diagnostiska händelser** fönster. Observera att hello händelser visas från både hello tillståndslösa tjänsten och hello tillståndskänslig service i hello program. Du kan pausa hello dataström genom att klicka på hello **pausa** knappen. Du kan sedan kontrollera hello information om ett meddelande genom att expandera meddelandet.

> [!NOTE]
> Innan du kör programmet hello kan du kontrollera att du har en lokal utveckling kluster som kör. Kolla in hello [Kom igång med](service-fabric-get-started.md) information om hur du konfigurerar din lokala miljö.
> 
> 

![Visa diagnostik händelser i Visual Studio](media/service-fabric-reliable-services-quick-start/hello-stateful-Output.png)

## <a name="next-steps"></a>Nästa steg
[Felsöka Service Fabric-program i Visual Studio](service-fabric-debugging-your-application.md)

[Komma igång: Service Fabric Web API-tjänster med OWIN själva värd](service-fabric-reliable-services-communication-webapi.md)

[Mer information om tillförlitlig samlingar](service-fabric-reliable-services-reliable-collections.md)

[Distribuera ett program](service-fabric-deploy-remove-applications.md)

[Uppgradering av programmet](service-fabric-application-upgrade.md)

[För utvecklare för Reliable Services](https://msdn.microsoft.com/library/azure/dn706529.aspx)

