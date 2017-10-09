---
title: "aaaCreate Frontwebb för din Azure Service Fabric-app med ASP.NET Core | Microsoft Docs"
description: "Exponera webbplatsen toohello Service Fabric-program med hjälp av en ASP.NET Core projektet och mellan tjänsten kommunikation via tjänsten fjärrkommunikation."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 96176149-69bb-4b06-a72e-ebbfea84454b
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: vturecek
ms.openlocfilehash: 0c4454d6cac4c2e343bd6e93e56d3d2f0ebfc4ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>Skapa en tjänst Frontwebb för ditt program med hjälp av ASP.NET Core
Som standard tillhandahåller Azure Service Fabric-tjänster inte en offentligt gränssnitt toohello webbplats. tooexpose programmets funktioner tooHTTP klienter du har toocreate en webb-projektet tooact som en startpunkt och sedan kommunicera från tooyour enskilda tjänster.

I den här självstudiekursen kommer vi ta vid där vi slutade i hello [skapa ditt första program i Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) självstudier och Lägg till en webbtjänst för ASP.NET Core framför hello tillståndskänslig räknaren service. Om du inte redan har gjort det, bör du gå tillbaka och steg för steg som kursen först.

## <a name="set-up-your-environment-for-aspnet-core"></a>Konfigurera din miljö för ASP.NET Core

Du måste Visual Studio 2017 toofollow tillsammans med den här kursen. Gör en utgåva. 

 - [Installera Visual Studio 2017](https://www.visualstudio.com/)

Du bör ha hello följande arbetsbelastningarna toodevelop ASP.NET Core Service Fabric-program:
 - **Azure-utveckling** (under *webb & moln*)
 - **ASP.NET och web development** (under *webb & moln*)
 - **.NET core plattformsoberoende development** (under *andra verktygsuppsättning*)

>[!Note] 
>hello .NET Core verktyg för Visual Studio 2015 inte längre uppdateras, men om du fortfarande använder Visual Studio 2015, behöver du toohave [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) installerad.

## <a name="add-an-aspnet-core-service-tooyour-application"></a>Lägga till ett ASP.NET Core service tooyour program
ASP.NET Core är ett lätt, plattformsoberoende utveckling att du kan använda toocreate moderna webbgränssnittet och webb-API: er. 

Lägg till ett ASP.NET Web API-projekt tooour befintliga program.

1. I Solution Explorer högerklickar du på **Services** inom hello projektet och välj **Lägg till > nya Service Fabric Service**.
   
    ![Lägga till ett nytt tooan befintliga tjänstprogram][vs-add-new-service]
2. På hello **skapar du en tjänst** väljer **ASP.NET Core** och ge det ett namn.
   
    ![Välja ASP.NET-webbtjänst i hello nya service dialog][vs-new-service-dialog]

3. hello nästa sida innehåller en uppsättning av ASP.NET Core projektmallar. Observera att dessa är hello samma alternativ som du vill se om du har skapat ett ASP.NET Core projekt utanför ett Service Fabric-program med en liten mängd ytterligare kod tooregister hello tjänsten med hello Service Fabric runtime. Den här kursen väljer **Web API**. Du kan dock använda hello samma begrepp toobuilding fullständig webbprogram.
   
    ![Om du väljer projekttypen ASP.NET][vs-new-aspnet-project-dialog]
   
    När Web API-projekt har skapats bör du ha två tjänster i ditt program. Fortsätter toobuild programmet, du kan lägga till fler tjänster i hello exakt samma sätt. Båda kan vara oberoende versionsnummer och uppgraderade.

## <a name="run-hello-application"></a>Kör programmet hello
tooget en uppfattning om vad vi har gjort kan vi distribuera hello nya programmet och ta en titt på hello standardbeteendet som hello ASP.NET Core Web API-mall innehåller.

1. Tryck på F5 i Visual Studio toodebug hello app.
2. När installationen är klar startar Visual Studio en webbläsare toohello rot hello ASP.NET Web API-tjänsten. hello ASP.NET Core Web API mallen ange inte standardbeteendet för hello rot, så du bör se ett 404-fel i hello webbläsare.
3. Lägg till `/api/values` toohello plats i hello webbläsare. Detta startar hello `Get` metoden på hello ValuesController i hello Web API-mallen. Den returnerar hello Standardsvar som tillhandahålls av hello-mallen – en JSON-matris som innehåller två strängar:
   
    ![Standardvärden som returneras från ASP.NET Core Web API-mall][browser-aspnet-template-values]
   
    Hello slutet av hello kursen visar den här sidan hello senaste räknarvärdet från våra tillståndskänslig service i stället för hello standard strängar.

## <a name="connect-hello-services"></a>Ansluta hello-tjänster
Service Fabric ger fullständig flexibilitet i hur du kommunicerar med tillförlitlig tjänster. Du kan ha tjänster som är tillgänglig via TCP, andra tjänster som är tillgängliga via en HTTP-REST-API och fortfarande andra tjänster som är tillgängliga via webben sockets inom ett enda program. Bakgrund hello alternativen och hello kompromisser ingår, se [kommunicerar med tjänster](service-fabric-connect-and-communicate-with-services.md). I den här självstudiekursen kommer vi använda [Service Fabric-tjänsten fjärrkommunikation](service-fabric-reliable-services-communication-remoting.md), som ingår i hello SDK.

I hello Remoting Service-metoden (modellerade på RPC-anrop eller RPC-anrop), definierar du ett gränssnitt tooact som hello offentliga kontraktet för hello-tjänsten. Sedan kan du använda den gränssnittet toogenerate en proxyklass för att interagera med hello-tjänsten.

### <a name="create-hello-remoting-interface"></a>Skapa hello fjärrkommunikation gränssnitt
Börja med att skapa hello gränssnittet tooact som hello avtal mellan hello tillståndskänslig service och andra tjänster i det här fallet hello webbprojekt i ASP.NET Core. Det här gränssnittet måste delas av alla tjänster som använder den toomake RPC-anrop, så vi ska skapa i sin egen klassbiblioteket projekt.

1. Högerklicka på lösningen i Solution Explorer och välj **Lägg till** > **nytt projekt**.

2. Välj hello **Visual C#** post i hello vänster navigeringsfönstret och välj sedan hello **klassbiblioteket** mall. Se till att hello .NET Framework-version har angetts för**4.5.2**.
   
    ![Skapa ett projekt för gränssnittet för tillståndskänsliga tjänsten][vs-add-class-library-project]

3. Installera hello **Microsoft.ServiceFabric.Services.Remoting** NuGet-paketet. Sök efter **Microsoft.ServiceFabric.Services.Remoting** i hello NuGet-paketet manager och installera det för alla projekt i hello lösning som använder tjänsten Remoting, inklusive:
   - hello-klassbiblioteket projekt som innehåller hello service-gränssnittet
   - hello tillståndskänslig Service-projekt
   - hello ASP.NET Core web service-projekt
   
    ![Lägga till hello Services NuGet-paketet][vs-services-nuget-package]

4. Skapa ett gränssnitt med en enda metod i hello klassbiblioteket `GetCountAsync`, och utöka hello-gränssnitt från `Microsoft.ServiceFabric.Services.Remoting.IService`. hello fjärrkommunikation gränssnittet måste härledas från det här gränssnittet tooindicate att det är ett gränssnitt för fjärrkommunikation med tjänsten.
   
    ```c#
    using Microsoft.ServiceFabric.Services.Remoting;
    using System.Threading.Tasks;
        
    ...

    namespace MyStatefulService.Interface
    {
        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```

### <a name="implement-hello-interface-in-your-stateful-service"></a>Implementera hello-gränssnittet i din tillståndskänslig service
Nu när vi har definierat hello-gränssnittet, vi behöver tooimplement i hello tillståndskänslig service.

1. Lägg till en referens toohello klassbiblioteksprojektet som innehåller hello-gränssnittet i din tillståndskänslig service.
   
    ![Lägga till en referens toohello klassbiblioteksprojektet i hello tillståndskänslig service][vs-add-class-library-reference]
2. Leta upp hello-klassen som ärver från `StatefulService`, som `MyStatefulService`, och utvidga den tooimplement hello `ICounter` gränssnitt.
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. Nu implementera hello enda metod som definieras i hello `ICounter` gränssnittet `GetCountAsync`.
   
    ```c#
    public async Task<long> GetCountAsync()
    {
        var myDictionary = 
            await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
   
        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```

### <a name="expose-hello-stateful-service-using-a-service-remoting-listener"></a>Exponera hello tillståndskänslig service med hjälp av en lyssnare för remoting service
Med hello `ICounter` gränssnittet implementeras hello sista steget är tooopen hello Remoting Service kommunikationskanalen. För tillståndskänsliga tjänster, Service Fabric ger en åsidosättningsbar metod som kallas `CreateServiceReplicaListeners`. Med den här metoden kan du ange en eller flera kommunikationslyssnarna, baserat på hello typ av kommunikation som du vill tooenable för din tjänst.

> [!NOTE]
> motsvarande hello anropas metoden för att öppna en kommunikationstjänsten kanal toostateless `CreateServiceInstanceListeners`.

I det här fallet vi ersätta hello befintliga `CreateServiceReplicaListeners` metod och ange en instans av `ServiceRemotingListener`, vilket skapar en RPC-slutpunkt som är callable från klienter via `ServiceProxy`.  

Hej `CreateServiceRemotingListener` metod på hello `IService` du kan använda gränssnittet tooeasily skapa en `ServiceRemotingListener` med alla standardinställningar. toouse denna metod, kontrollera att du har hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` namnområde som importeras. 

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-hello-serviceproxy-class-toointeract-with-hello-service"></a>Använda hello ServiceProxy klassen toointeract med hello-tjänsten
Vår tillståndskänslig service är nu redo tooreceive trafik från andra tjänster via RPC. Så allt som fortfarande att lägga till hello kod toocommunicate med den från hello ASP.NET-webbtjänst.

1. Lägg till en referens toohello klassbiblioteket som innehåller hello i ASP.NET-projekt `ICounter` gränssnitt.

2. Du har tidigare lagt till hello **Microsoft.ServiceFabric.Services.Remoting** NuGet-paketet toohello ASP.NET-projekt. Det här paketet innehåller hello `ServiceProxy` klass som du använder toomake RPC-anrop toohello tillståndskänslig service. Kontrollera NuGet-paketet har installerats i hello ASP.NET Core web service-projekt.

4. I hello **domänkontrollanter** mapp, öppna hello `ValuesController` klass. Observera att hello `Get` metoden returnerar för närvarande bara en hårdkodad strängmatris ”value1” och ”value2”--som matchar vad vi såg tidigare i hello webbläsare. Ersätt den här implementeringen med hello följande kod:
   
    ```c#
    using MyStatefulService.Interface;
    using Microsoft.ServiceFabric.Services.Client;
    using Microsoft.ServiceFabric.Services.Remoting.Client;
   
    ...

    [HttpGet]
    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));
   
        long count = await counter.GetCountAsync();
   
        return new string[] { count.ToString() };
    }
    ```
   
    hello första rad med kod är en nyckel som hello. toocreate hello ICounter toohello tillståndskänslig proxytjänsten, behöver du tooprovide två typer av information: ett ID och hello partitionsnamn för hello-tjänsten.
   
    Du kan använda partitionering tooscale tillståndskänsliga tjänster genom att bryta deras tillstånd i olika buckets, baserat på en nyckel som du definierar, till exempel en kund-ID eller postnummer. I vårt trivial program har hello tillståndskänslig service endast en partition så hello nyckeln ingen spelar roll. En nyckel som du anger kommer att leda toohello samma partition. toolearn mer information om partitionering services, se [hur toopartition Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).
   
    hello tjänstnamnet är en URI för hello formuläret fabric: /&lt;$application_name&gt;/&lt;service_name&gt;.
   
    Med dessa två typer av information identifiera Service Fabric hello-dator som ska skickas till. Hej `ServiceProxy` klassen hanterar också sömlöst hello fall där hello-dator som är värd för hello tillståndskänslig service partition misslyckas och en annan dator måste vara upphöjt tootake dess ställe. Denna framställning gör skrivning hello klienten kod toodeal med andra tjänster som är betydligt enklare.
   
    När vi har hello proxy kan vi helt enkelt anropa hello `GetCountAsync` metod och returnerar resultatet.

5. Tryck på F5 igen toorun hello ändrade programmet. Som startas tidigare, Visual Studio automatiskt hello webbläsare toohello roten för hello webbprojekt. Lägg till hello ”api/värdena” sökväg och du bör se hello aktuella räknarvärdet returneras.
   
    ![hello tillståndskänslig counter-värdet som visas i hello webbläsare][browser-aspnet-counter-value]
   
    Uppdatera hello webbläsare regelbundet toosee hello räknaren värdet uppdateringen.

## <a name="kestrel-and-weblistener"></a>Kestrel och WebListener

hello standard ASP.NET Core webbservern, kallas även Kestrel, är [stöds för närvarande inte för hantering av direkt Internettrafik](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel). Därför hello ASP.NET Core tillståndslösa tjänstmallen för Service Fabric använder [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) som standard. 

toolearn mer om Kestrel och WebListener i Service Fabric-tjänster, se för[ASP.NET Core i Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="connecting-tooa-reliable-actor-service"></a>Ansluta tooa tillförlitliga aktören service
Den här självstudiekursen fokuserar på att lägga till en frontwebb som kommunicerat med en tillståndskänslig tjänst. Du kan dock följa en mycket lik modellen tootalk tooactors. När du skapar en tillförlitlig aktören-projekt genererar automatiskt ett projekt för gränssnittet för Visual Studio. Du kan använda den gränssnittet toogenerate en aktören proxy i hello web project toocommunicate med hello. hello kommunikationskanalen tillhandahålls automatiskt. Så du inte behöver toodo något annat som är likvärdiga tooestablishing en `ServiceRemotingListener` som du gjorde för tillståndskänsliga hello-tjänsten i den här självstudiekursen.

## <a name="how-web-services-work-on-your-local-cluster"></a>Hur webbtjänster fungerar i din lokala klustret
I allmänhet kan du distribuera exakt hello samma Service Fabric application tooa flera dator kluster som du har distribuerat på din lokala klustret och vara mycket säker på att den fungerar som förväntat. Detta beror på att din lokala klustret är helt enkelt en fem-nodkonfigurationen som är dolda tooa enskild dator.

När det gäller tooweb tjänster, men är det en nyckel nuance. När klustret är placerad bakom en belastningsutjämnare i Azure, måste du se till att webbtjänster distribueras på varje dator eftersom hello belastningsutjämnaren bara round-resursallokering (robin) trafik över hello datorer. Du kan göra detta genom att ange hello `InstanceCount` för hello service toohello särskilda värdet ”1”.

Däremot när du kör en webbtjänst lokalt, måste du tooensure att endast en instans av hello-tjänsten körs. Annars kan du köra i konflikt från flera processer som lyssnar på hello samma sökväg och port. Därför hello web service-instanser ska anges för ”1” för lokala distributioner.

hur tooconfigure olika värden för annan miljö, se toolearn [hantera programparametrar för miljöer med flera](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Nästa steg
Nu när du har en web front hamna set för ditt program med ASP.NET Core, Lär dig mer om [ASP.NET Core i Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) för en djupgående förståelse av hur ASP.NET Core fungerar med Service Fabric.

Nästa [Lär dig mer om hur du kommunicerar med services](service-fabric-connect-and-communicate-with-services.md) i allmänhet tooget en klar bild av hur tjänsten kommunikation fungerar i Service Fabric.

När du har en god förståelse av hur tjänstkommunikation fungerar [skapa ett kluster i Azure och distribuera ditt program toohello moln](service-fabric-cluster-creation-via-portal.md).

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
