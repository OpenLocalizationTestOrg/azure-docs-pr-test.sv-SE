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
# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a><span data-ttu-id="b8928-103">Skapa en tjänst Frontwebb för ditt program med hjälp av ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8928-103">Build a web service front end for your application using ASP.NET Core</span></span>
<span data-ttu-id="b8928-104">Som standard tillhandahåller Azure Service Fabric-tjänster inte en offentligt gränssnitt toohello webbplats.</span><span class="sxs-lookup"><span data-stu-id="b8928-104">By default, Azure Service Fabric services do not provide a public interface toohello web.</span></span> <span data-ttu-id="b8928-105">tooexpose programmets funktioner tooHTTP klienter du har toocreate en webb-projektet tooact som en startpunkt och sedan kommunicera från tooyour enskilda tjänster.</span><span class="sxs-lookup"><span data-stu-id="b8928-105">tooexpose your application's functionality tooHTTP clients, you have toocreate a web project tooact as an entry point and then communicate from there tooyour individual services.</span></span>

<span data-ttu-id="b8928-106">I den här självstudiekursen kommer vi ta vid där vi slutade i hello [skapa ditt första program i Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) självstudier och Lägg till en webbtjänst för ASP.NET Core framför hello tillståndskänslig räknaren service.</span><span class="sxs-lookup"><span data-stu-id="b8928-106">In this tutorial, we pick up where we left off in hello [Creating your first application in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md) tutorial and add an ASP.NET Core web service in front of hello stateful counter service.</span></span> <span data-ttu-id="b8928-107">Om du inte redan har gjort det, bör du gå tillbaka och steg för steg som kursen först.</span><span class="sxs-lookup"><span data-stu-id="b8928-107">If you have not already done so, you should go back and step through that tutorial first.</span></span>

## <a name="set-up-your-environment-for-aspnet-core"></a><span data-ttu-id="b8928-108">Konfigurera din miljö för ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b8928-108">Set up your environment for ASP.NET Core</span></span>

<span data-ttu-id="b8928-109">Du måste Visual Studio 2017 toofollow tillsammans med den här kursen.</span><span class="sxs-lookup"><span data-stu-id="b8928-109">You need Visual Studio 2017 toofollow along with this tutorial.</span></span> <span data-ttu-id="b8928-110">Gör en utgåva.</span><span class="sxs-lookup"><span data-stu-id="b8928-110">Any edition will do.</span></span> 

 - [<span data-ttu-id="b8928-111">Installera Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b8928-111">Install Visual Studio 2017</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="b8928-112">Du bör ha hello följande arbetsbelastningarna toodevelop ASP.NET Core Service Fabric-program:</span><span class="sxs-lookup"><span data-stu-id="b8928-112">toodevelop ASP.NET Core Service Fabric applications, you should have hello following workloads installed:</span></span>
 - <span data-ttu-id="b8928-113">**Azure-utveckling** (under *webb & moln*)</span><span class="sxs-lookup"><span data-stu-id="b8928-113">**Azure development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="b8928-114">**ASP.NET och web development** (under *webb & moln*)</span><span class="sxs-lookup"><span data-stu-id="b8928-114">**ASP.NET and web development** (under *Web & Cloud*)</span></span>
 - <span data-ttu-id="b8928-115">**.NET core plattformsoberoende development** (under *andra verktygsuppsättning*)</span><span class="sxs-lookup"><span data-stu-id="b8928-115">**.NET Core cross-platform development** (under *Other Toolsets*)</span></span>

>[!Note] 
><span data-ttu-id="b8928-116">hello .NET Core verktyg för Visual Studio 2015 inte längre uppdateras, men om du fortfarande använder Visual Studio 2015, behöver du toohave [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) installerad.</span><span class="sxs-lookup"><span data-stu-id="b8928-116">hello .NET Core tools for Visual Studio 2015 are no longer being updated, however if you are still using Visual Studio 2015, you need toohave [.NET Core VS 2015 Tooling Preview 2](https://www.microsoft.com/net/download/core) installed.</span></span>

## <a name="add-an-aspnet-core-service-tooyour-application"></a><span data-ttu-id="b8928-117">Lägga till ett ASP.NET Core service tooyour program</span><span class="sxs-lookup"><span data-stu-id="b8928-117">Add an ASP.NET Core service tooyour application</span></span>
<span data-ttu-id="b8928-118">ASP.NET Core är ett lätt, plattformsoberoende utveckling att du kan använda toocreate moderna webbgränssnittet och webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="b8928-118">ASP.NET Core is a lightweight, cross-platform web development framework that you can use toocreate modern web UI and web APIs.</span></span> 

<span data-ttu-id="b8928-119">Lägg till ett ASP.NET Web API-projekt tooour befintliga program.</span><span class="sxs-lookup"><span data-stu-id="b8928-119">Let's add an ASP.NET Web API project tooour existing application.</span></span>

1. <span data-ttu-id="b8928-120">I Solution Explorer högerklickar du på **Services** inom hello projektet och välj **Lägg till > nya Service Fabric Service**.</span><span class="sxs-lookup"><span data-stu-id="b8928-120">In Solution Explorer, right-click **Services** within hello application project and choose **Add > New Service Fabric Service**.</span></span>
   
    ![Lägga till ett nytt tooan befintliga tjänstprogram][vs-add-new-service]
2. <span data-ttu-id="b8928-122">På hello **skapar du en tjänst** väljer **ASP.NET Core** och ge det ett namn.</span><span class="sxs-lookup"><span data-stu-id="b8928-122">On hello **Create a Service** page, choose **ASP.NET Core** and give it a name.</span></span>
   
    ![Välja ASP.NET-webbtjänst i hello nya service dialog][vs-new-service-dialog]

3. <span data-ttu-id="b8928-124">hello nästa sida innehåller en uppsättning av ASP.NET Core projektmallar.</span><span class="sxs-lookup"><span data-stu-id="b8928-124">hello next page provides a set of ASP.NET Core project templates.</span></span> <span data-ttu-id="b8928-125">Observera att dessa är hello samma alternativ som du vill se om du har skapat ett ASP.NET Core projekt utanför ett Service Fabric-program med en liten mängd ytterligare kod tooregister hello tjänsten med hello Service Fabric runtime.</span><span class="sxs-lookup"><span data-stu-id="b8928-125">Note that these are hello same choices that you would see if you created an ASP.NET Core project outside of a Service Fabric application, with a small amount of additional code tooregister hello service with hello Service Fabric runtime.</span></span> <span data-ttu-id="b8928-126">Den här kursen väljer **Web API**.</span><span class="sxs-lookup"><span data-stu-id="b8928-126">For this tutorial, choose **Web API**.</span></span> <span data-ttu-id="b8928-127">Du kan dock använda hello samma begrepp toobuilding fullständig webbprogram.</span><span class="sxs-lookup"><span data-stu-id="b8928-127">However, you can apply hello same concepts toobuilding a full web application.</span></span>
   
    ![Om du väljer projekttypen ASP.NET][vs-new-aspnet-project-dialog]
   
    <span data-ttu-id="b8928-129">När Web API-projekt har skapats bör du ha två tjänster i ditt program.</span><span class="sxs-lookup"><span data-stu-id="b8928-129">Once your Web API project is created, you should have two services in your application.</span></span> <span data-ttu-id="b8928-130">Fortsätter toobuild programmet, du kan lägga till fler tjänster i hello exakt samma sätt.</span><span class="sxs-lookup"><span data-stu-id="b8928-130">As you continue toobuild your application, you can add more services in exactly hello same way.</span></span> <span data-ttu-id="b8928-131">Båda kan vara oberoende versionsnummer och uppgraderade.</span><span class="sxs-lookup"><span data-stu-id="b8928-131">Each can be independently versioned and upgraded.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="b8928-132">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="b8928-132">Run hello application</span></span>
<span data-ttu-id="b8928-133">tooget en uppfattning om vad vi har gjort kan vi distribuera hello nya programmet och ta en titt på hello standardbeteendet som hello ASP.NET Core Web API-mall innehåller.</span><span class="sxs-lookup"><span data-stu-id="b8928-133">tooget a sense of what we've done, let's deploy hello new application and take a look at hello default behavior that hello ASP.NET Core Web API template provides.</span></span>

1. <span data-ttu-id="b8928-134">Tryck på F5 i Visual Studio toodebug hello app.</span><span class="sxs-lookup"><span data-stu-id="b8928-134">Press F5 in Visual Studio toodebug hello app.</span></span>
2. <span data-ttu-id="b8928-135">När installationen är klar startar Visual Studio en webbläsare toohello rot hello ASP.NET Web API-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b8928-135">When deployment is complete, Visual Studio launches a browser toohello root of hello ASP.NET Web API service.</span></span> <span data-ttu-id="b8928-136">hello ASP.NET Core Web API mallen ange inte standardbeteendet för hello rot, så du bör se ett 404-fel i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8928-136">hello ASP.NET Core Web API template doesn't provide default behavior for hello root, so you should see a 404 error in hello browser.</span></span>
3. <span data-ttu-id="b8928-137">Lägg till `/api/values` toohello plats i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8928-137">Add `/api/values` toohello location in hello browser.</span></span> <span data-ttu-id="b8928-138">Detta startar hello `Get` metoden på hello ValuesController i hello Web API-mallen.</span><span class="sxs-lookup"><span data-stu-id="b8928-138">This invokes hello `Get` method on hello ValuesController in hello Web API template.</span></span> <span data-ttu-id="b8928-139">Den returnerar hello Standardsvar som tillhandahålls av hello-mallen – en JSON-matris som innehåller två strängar:</span><span class="sxs-lookup"><span data-stu-id="b8928-139">It returns hello default response that is provided by hello template--a JSON array that contains two strings:</span></span>
   
    ![Standardvärden som returneras från ASP.NET Core Web API-mall][browser-aspnet-template-values]
   
    <span data-ttu-id="b8928-141">Hello slutet av hello kursen visar den här sidan hello senaste räknarvärdet från våra tillståndskänslig service i stället för hello standard strängar.</span><span class="sxs-lookup"><span data-stu-id="b8928-141">By hello end of hello tutorial, this page will show hello most recent counter value from our stateful service instead of hello default strings.</span></span>

## <a name="connect-hello-services"></a><span data-ttu-id="b8928-142">Ansluta hello-tjänster</span><span class="sxs-lookup"><span data-stu-id="b8928-142">Connect hello services</span></span>
<span data-ttu-id="b8928-143">Service Fabric ger fullständig flexibilitet i hur du kommunicerar med tillförlitlig tjänster.</span><span class="sxs-lookup"><span data-stu-id="b8928-143">Service Fabric provides complete flexibility in how you communicate with reliable services.</span></span> <span data-ttu-id="b8928-144">Du kan ha tjänster som är tillgänglig via TCP, andra tjänster som är tillgängliga via en HTTP-REST-API och fortfarande andra tjänster som är tillgängliga via webben sockets inom ett enda program.</span><span class="sxs-lookup"><span data-stu-id="b8928-144">Within a single application, you might have services that are accessible via TCP, other services that are accessible via an HTTP REST API, and still other services that are accessible via web sockets.</span></span> <span data-ttu-id="b8928-145">Bakgrund hello alternativen och hello kompromisser ingår, se [kommunicerar med tjänster](service-fabric-connect-and-communicate-with-services.md).</span><span class="sxs-lookup"><span data-stu-id="b8928-145">For background on hello options available and hello tradeoffs involved, see [Communicating with services](service-fabric-connect-and-communicate-with-services.md).</span></span> <span data-ttu-id="b8928-146">I den här självstudiekursen kommer vi använda [Service Fabric-tjänsten fjärrkommunikation](service-fabric-reliable-services-communication-remoting.md), som ingår i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="b8928-146">In this tutorial, we use [Service Fabric Service Remoting](service-fabric-reliable-services-communication-remoting.md), provided in hello SDK.</span></span>

<span data-ttu-id="b8928-147">I hello Remoting Service-metoden (modellerade på RPC-anrop eller RPC-anrop), definierar du ett gränssnitt tooact som hello offentliga kontraktet för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b8928-147">In hello Service Remoting approach (modeled on remote procedure calls or RPCs), you define an interface tooact as hello public contract for hello service.</span></span> <span data-ttu-id="b8928-148">Sedan kan du använda den gränssnittet toogenerate en proxyklass för att interagera med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b8928-148">Then, you use that interface toogenerate a proxy class for interacting with hello service.</span></span>

### <a name="create-hello-remoting-interface"></a><span data-ttu-id="b8928-149">Skapa hello fjärrkommunikation gränssnitt</span><span class="sxs-lookup"><span data-stu-id="b8928-149">Create hello remoting interface</span></span>
<span data-ttu-id="b8928-150">Börja med att skapa hello gränssnittet tooact som hello avtal mellan hello tillståndskänslig service och andra tjänster i det här fallet hello webbprojekt i ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b8928-150">Let's start by creating hello interface tooact as hello contract between hello stateful service and other services, in this case hello ASP.NET Core web project.</span></span> <span data-ttu-id="b8928-151">Det här gränssnittet måste delas av alla tjänster som använder den toomake RPC-anrop, så vi ska skapa i sin egen klassbiblioteket projekt.</span><span class="sxs-lookup"><span data-stu-id="b8928-151">This interface must be shared by all services that use it toomake RPC calls, so we'll create it in its own Class Library project.</span></span>

1. <span data-ttu-id="b8928-152">Högerklicka på lösningen i Solution Explorer och välj **Lägg till** > **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="b8928-152">In Solution Explorer, right-click your solution and choose **Add** > **New Project**.</span></span>

2. <span data-ttu-id="b8928-153">Välj hello **Visual C#** post i hello vänster navigeringsfönstret och välj sedan hello **klassbiblioteket** mall.</span><span class="sxs-lookup"><span data-stu-id="b8928-153">Choose hello **Visual C#** entry in hello left navigation pane and then select hello **Class Library** template.</span></span> <span data-ttu-id="b8928-154">Se till att hello .NET Framework-version har angetts för**4.5.2**.</span><span class="sxs-lookup"><span data-stu-id="b8928-154">Ensure that hello .NET Framework version is set too**4.5.2**.</span></span>
   
    ![Skapa ett projekt för gränssnittet för tillståndskänsliga tjänsten][vs-add-class-library-project]

3. <span data-ttu-id="b8928-156">Installera hello **Microsoft.ServiceFabric.Services.Remoting** NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="b8928-156">Install hello **Microsoft.ServiceFabric.Services.Remoting** NuGet package.</span></span> <span data-ttu-id="b8928-157">Sök efter **Microsoft.ServiceFabric.Services.Remoting** i hello NuGet-paketet manager och installera det för alla projekt i hello lösning som använder tjänsten Remoting, inklusive:</span><span class="sxs-lookup"><span data-stu-id="b8928-157">Search for  **Microsoft.ServiceFabric.Services.Remoting** in hello NuGet package manager and install it for all projects in hello solution that use Service Remoting, including:</span></span>
   - <span data-ttu-id="b8928-158">hello-klassbiblioteket projekt som innehåller hello service-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="b8928-158">hello Class Library project that contains hello service interface</span></span>
   - <span data-ttu-id="b8928-159">hello tillståndskänslig Service-projekt</span><span class="sxs-lookup"><span data-stu-id="b8928-159">hello Stateful Service project</span></span>
   - <span data-ttu-id="b8928-160">hello ASP.NET Core web service-projekt</span><span class="sxs-lookup"><span data-stu-id="b8928-160">hello ASP.NET Core web service project</span></span>
   
    ![Lägga till hello Services NuGet-paketet][vs-services-nuget-package]

4. <span data-ttu-id="b8928-162">Skapa ett gränssnitt med en enda metod i hello klassbiblioteket `GetCountAsync`, och utöka hello-gränssnitt från `Microsoft.ServiceFabric.Services.Remoting.IService`.</span><span class="sxs-lookup"><span data-stu-id="b8928-162">In hello class library, create an interface with a single method, `GetCountAsync`, and extend hello interface from `Microsoft.ServiceFabric.Services.Remoting.IService`.</span></span> <span data-ttu-id="b8928-163">hello fjärrkommunikation gränssnittet måste härledas från det här gränssnittet tooindicate att det är ett gränssnitt för fjärrkommunikation med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b8928-163">hello remoting interface must derive from this interface tooindicate that it is a Service Remoting interface.</span></span>
   
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

### <a name="implement-hello-interface-in-your-stateful-service"></a><span data-ttu-id="b8928-164">Implementera hello-gränssnittet i din tillståndskänslig service</span><span class="sxs-lookup"><span data-stu-id="b8928-164">Implement hello interface in your stateful service</span></span>
<span data-ttu-id="b8928-165">Nu när vi har definierat hello-gränssnittet, vi behöver tooimplement i hello tillståndskänslig service.</span><span class="sxs-lookup"><span data-stu-id="b8928-165">Now that we have defined hello interface, we need tooimplement it in hello stateful service.</span></span>

1. <span data-ttu-id="b8928-166">Lägg till en referens toohello klassbiblioteksprojektet som innehåller hello-gränssnittet i din tillståndskänslig service.</span><span class="sxs-lookup"><span data-stu-id="b8928-166">In your stateful service, add a reference toohello class library project that contains hello interface.</span></span>
   
    ![Lägga till en referens toohello klassbiblioteksprojektet i hello tillståndskänslig service][vs-add-class-library-reference]
2. <span data-ttu-id="b8928-168">Leta upp hello-klassen som ärver från `StatefulService`, som `MyStatefulService`, och utvidga den tooimplement hello `ICounter` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="b8928-168">Locate hello class that inherits from `StatefulService`, such as `MyStatefulService`, and extend it tooimplement hello `ICounter` interface.</span></span>
   
    ```c#
    using MyStatefulService.Interface;
   
    ...
   
    public class MyStatefulService : StatefulService, ICounter
    {        
         ...
    }
    ```
3. <span data-ttu-id="b8928-169">Nu implementera hello enda metod som definieras i hello `ICounter` gränssnittet `GetCountAsync`.</span><span class="sxs-lookup"><span data-stu-id="b8928-169">Now implement hello single method that is defined in hello `ICounter` interface, `GetCountAsync`.</span></span>
   
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

### <a name="expose-hello-stateful-service-using-a-service-remoting-listener"></a><span data-ttu-id="b8928-170">Exponera hello tillståndskänslig service med hjälp av en lyssnare för remoting service</span><span class="sxs-lookup"><span data-stu-id="b8928-170">Expose hello stateful service using a service remoting listener</span></span>
<span data-ttu-id="b8928-171">Med hello `ICounter` gränssnittet implementeras hello sista steget är tooopen hello Remoting Service kommunikationskanalen.</span><span class="sxs-lookup"><span data-stu-id="b8928-171">With hello `ICounter` interface implemented, hello final step is tooopen hello Service Remoting communication channel.</span></span> <span data-ttu-id="b8928-172">För tillståndskänsliga tjänster, Service Fabric ger en åsidosättningsbar metod som kallas `CreateServiceReplicaListeners`.</span><span class="sxs-lookup"><span data-stu-id="b8928-172">For stateful services, Service Fabric provides an overridable method called `CreateServiceReplicaListeners`.</span></span> <span data-ttu-id="b8928-173">Med den här metoden kan du ange en eller flera kommunikationslyssnarna, baserat på hello typ av kommunikation som du vill tooenable för din tjänst.</span><span class="sxs-lookup"><span data-stu-id="b8928-173">With this method, you can specify one or more communication listeners, based on hello type of communication that you want tooenable for your service.</span></span>

> [!NOTE]
> <span data-ttu-id="b8928-174">motsvarande hello anropas metoden för att öppna en kommunikationstjänsten kanal toostateless `CreateServiceInstanceListeners`.</span><span class="sxs-lookup"><span data-stu-id="b8928-174">hello equivalent method for opening a communication channel toostateless services is called `CreateServiceInstanceListeners`.</span></span>

<span data-ttu-id="b8928-175">I det här fallet vi ersätta hello befintliga `CreateServiceReplicaListeners` metod och ange en instans av `ServiceRemotingListener`, vilket skapar en RPC-slutpunkt som är callable från klienter via `ServiceProxy`.</span><span class="sxs-lookup"><span data-stu-id="b8928-175">In this case, we replace hello existing `CreateServiceReplicaListeners` method and provide an instance of `ServiceRemotingListener`, which creates an RPC endpoint that is callable from clients through `ServiceProxy`.</span></span>  

<span data-ttu-id="b8928-176">Hej `CreateServiceRemotingListener` metod på hello `IService` du kan använda gränssnittet tooeasily skapa en `ServiceRemotingListener` med alla standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="b8928-176">hello `CreateServiceRemotingListener` extension method on hello `IService` interface allows you tooeasily create a `ServiceRemotingListener` with all default settings.</span></span> <span data-ttu-id="b8928-177">toouse denna metod, kontrollera att du har hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` namnområde som importeras.</span><span class="sxs-lookup"><span data-stu-id="b8928-177">toouse this extension method, ensure you have hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` namespace imported.</span></span> 

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


### <a name="use-hello-serviceproxy-class-toointeract-with-hello-service"></a><span data-ttu-id="b8928-178">Använda hello ServiceProxy klassen toointeract med hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="b8928-178">Use hello ServiceProxy class toointeract with hello service</span></span>
<span data-ttu-id="b8928-179">Vår tillståndskänslig service är nu redo tooreceive trafik från andra tjänster via RPC.</span><span class="sxs-lookup"><span data-stu-id="b8928-179">Our stateful service is now ready tooreceive traffic from other services over RPC.</span></span> <span data-ttu-id="b8928-180">Så allt som fortfarande att lägga till hello kod toocommunicate med den från hello ASP.NET-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="b8928-180">So all that remains is adding hello code toocommunicate with it from hello ASP.NET web service.</span></span>

1. <span data-ttu-id="b8928-181">Lägg till en referens toohello klassbiblioteket som innehåller hello i ASP.NET-projekt `ICounter` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="b8928-181">In your ASP.NET project, add a reference toohello class library that contains hello `ICounter` interface.</span></span>

2. <span data-ttu-id="b8928-182">Du har tidigare lagt till hello **Microsoft.ServiceFabric.Services.Remoting** NuGet-paketet toohello ASP.NET-projekt.</span><span class="sxs-lookup"><span data-stu-id="b8928-182">Earlier you added hello **Microsoft.ServiceFabric.Services.Remoting** NuGet package toohello ASP.NET project.</span></span> <span data-ttu-id="b8928-183">Det här paketet innehåller hello `ServiceProxy` klass som du använder toomake RPC-anrop toohello tillståndskänslig service.</span><span class="sxs-lookup"><span data-stu-id="b8928-183">This package provides hello `ServiceProxy` class which you use toomake RPC calls toohello stateful service.</span></span> <span data-ttu-id="b8928-184">Kontrollera NuGet-paketet har installerats i hello ASP.NET Core web service-projekt.</span><span class="sxs-lookup"><span data-stu-id="b8928-184">Ensure this NuGet package is installed in hello ASP.NET Core web service project.</span></span>

4. <span data-ttu-id="b8928-185">I hello **domänkontrollanter** mapp, öppna hello `ValuesController` klass.</span><span class="sxs-lookup"><span data-stu-id="b8928-185">In hello **Controllers** folder, open hello `ValuesController` class.</span></span> <span data-ttu-id="b8928-186">Observera att hello `Get` metoden returnerar för närvarande bara en hårdkodad strängmatris ”value1” och ”value2”--som matchar vad vi såg tidigare i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="b8928-186">Note that hello `Get` method currently just returns a hard-coded string array of "value1" and "value2"--which matches what we saw earlier in hello browser.</span></span> <span data-ttu-id="b8928-187">Ersätt den här implementeringen med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b8928-187">Replace this implementation with hello following code:</span></span>
   
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
   
    <span data-ttu-id="b8928-188">hello första rad med kod är en nyckel som hello.</span><span class="sxs-lookup"><span data-stu-id="b8928-188">hello first line of code is hello key one.</span></span> <span data-ttu-id="b8928-189">toocreate hello ICounter toohello tillståndskänslig proxytjänsten, behöver du tooprovide två typer av information: ett ID och hello partitionsnamn för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b8928-189">toocreate hello ICounter proxy toohello stateful service, you need tooprovide two pieces of information: a partition ID and hello name of hello service.</span></span>
   
    <span data-ttu-id="b8928-190">Du kan använda partitionering tooscale tillståndskänsliga tjänster genom att bryta deras tillstånd i olika buckets, baserat på en nyckel som du definierar, till exempel en kund-ID eller postnummer.</span><span class="sxs-lookup"><span data-stu-id="b8928-190">You can use partitioning tooscale stateful services by breaking up their state into different buckets, based on a key that you define, such as a customer ID or postal code.</span></span> <span data-ttu-id="b8928-191">I vårt trivial program har hello tillståndskänslig service endast en partition så hello nyckeln ingen spelar roll.</span><span class="sxs-lookup"><span data-stu-id="b8928-191">In our trivial application, hello stateful service only has one partition, so hello key doesn't matter.</span></span> <span data-ttu-id="b8928-192">En nyckel som du anger kommer att leda toohello samma partition.</span><span class="sxs-lookup"><span data-stu-id="b8928-192">Any key that you provide will lead toohello same partition.</span></span> <span data-ttu-id="b8928-193">toolearn mer information om partitionering services, se [hur toopartition Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="b8928-193">toolearn more about partitioning services, see [How toopartition Service Fabric Reliable Services](service-fabric-concepts-partitioning.md).</span></span>
   
    <span data-ttu-id="b8928-194">hello tjänstnamnet är en URI för hello formuläret fabric: /&lt;$application_name&gt;/&lt;service_name&gt;.</span><span class="sxs-lookup"><span data-stu-id="b8928-194">hello service name is a URI of hello form fabric:/&lt;application_name&gt;/&lt;service_name&gt;.</span></span>
   
    <span data-ttu-id="b8928-195">Med dessa två typer av information identifiera Service Fabric hello-dator som ska skickas till.</span><span class="sxs-lookup"><span data-stu-id="b8928-195">With these two pieces of information, Service Fabric can uniquely identify hello machine that requests should be sent to.</span></span> <span data-ttu-id="b8928-196">Hej `ServiceProxy` klassen hanterar också sömlöst hello fall där hello-dator som är värd för hello tillståndskänslig service partition misslyckas och en annan dator måste vara upphöjt tootake dess ställe.</span><span class="sxs-lookup"><span data-stu-id="b8928-196">hello `ServiceProxy` class also seamlessly handles hello case where hello machine that hosts hello stateful service partition fails and another machine must be promoted tootake its place.</span></span> <span data-ttu-id="b8928-197">Denna framställning gör skrivning hello klienten kod toodeal med andra tjänster som är betydligt enklare.</span><span class="sxs-lookup"><span data-stu-id="b8928-197">This abstraction makes writing hello client code toodeal with other services significantly simpler.</span></span>
   
    <span data-ttu-id="b8928-198">När vi har hello proxy kan vi helt enkelt anropa hello `GetCountAsync` metod och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="b8928-198">Once we have hello proxy, we simply invoke hello `GetCountAsync` method and return its result.</span></span>

5. <span data-ttu-id="b8928-199">Tryck på F5 igen toorun hello ändrade programmet.</span><span class="sxs-lookup"><span data-stu-id="b8928-199">Press F5 again toorun hello modified application.</span></span> <span data-ttu-id="b8928-200">Som startas tidigare, Visual Studio automatiskt hello webbläsare toohello roten för hello webbprojekt.</span><span class="sxs-lookup"><span data-stu-id="b8928-200">As before, Visual Studio automatically launches hello browser toohello root of hello web project.</span></span> <span data-ttu-id="b8928-201">Lägg till hello ”api/värdena” sökväg och du bör se hello aktuella räknarvärdet returneras.</span><span class="sxs-lookup"><span data-stu-id="b8928-201">Add hello "api/values" path, and you should see hello current counter value returned.</span></span>
   
    ![hello tillståndskänslig counter-värdet som visas i hello webbläsare][browser-aspnet-counter-value]
   
    <span data-ttu-id="b8928-203">Uppdatera hello webbläsare regelbundet toosee hello räknaren värdet uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="b8928-203">Refresh hello browser periodically toosee hello counter value update.</span></span>

## <a name="kestrel-and-weblistener"></a><span data-ttu-id="b8928-204">Kestrel och WebListener</span><span class="sxs-lookup"><span data-stu-id="b8928-204">Kestrel and WebListener</span></span>

<span data-ttu-id="b8928-205">hello standard ASP.NET Core webbservern, kallas även Kestrel, är [stöds för närvarande inte för hantering av direkt Internettrafik](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="b8928-205">hello default ASP.NET Core web server, known as Kestrel, is [not currently supported for handling direct internet traffic](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel).</span></span> <span data-ttu-id="b8928-206">Därför hello ASP.NET Core tillståndslösa tjänstmallen för Service Fabric använder [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) som standard.</span><span class="sxs-lookup"><span data-stu-id="b8928-206">As a result, hello ASP.NET Core stateless service template for Service Fabric uses [WebListener](https://docs.microsoft.com/aspnet/core/fundamentals/servers/weblistener) by default.</span></span> 

<span data-ttu-id="b8928-207">toolearn mer om Kestrel och WebListener i Service Fabric-tjänster, se för[ASP.NET Core i Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).</span><span class="sxs-lookup"><span data-stu-id="b8928-207">toolearn more about Kestrel and WebListener in Service Fabric services, please refer too[ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md).</span></span>

## <a name="connecting-tooa-reliable-actor-service"></a><span data-ttu-id="b8928-208">Ansluta tooa tillförlitliga aktören service</span><span class="sxs-lookup"><span data-stu-id="b8928-208">Connecting tooa Reliable Actor service</span></span>
<span data-ttu-id="b8928-209">Den här självstudiekursen fokuserar på att lägga till en frontwebb som kommunicerat med en tillståndskänslig tjänst.</span><span class="sxs-lookup"><span data-stu-id="b8928-209">This tutorial focused on adding a web front end that communicated with a stateful service.</span></span> <span data-ttu-id="b8928-210">Du kan dock följa en mycket lik modellen tootalk tooactors.</span><span class="sxs-lookup"><span data-stu-id="b8928-210">However, you can follow a very similar model tootalk tooactors.</span></span> <span data-ttu-id="b8928-211">När du skapar en tillförlitlig aktören-projekt genererar automatiskt ett projekt för gränssnittet för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8928-211">When you create a Reliable Actor project, Visual Studio automatically generates an interface project for you.</span></span> <span data-ttu-id="b8928-212">Du kan använda den gränssnittet toogenerate en aktören proxy i hello web project toocommunicate med hello.</span><span class="sxs-lookup"><span data-stu-id="b8928-212">You can use that interface toogenerate an actor proxy in hello web project toocommunicate with hello actor.</span></span> <span data-ttu-id="b8928-213">hello kommunikationskanalen tillhandahålls automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b8928-213">hello communication channel is provided automatically.</span></span> <span data-ttu-id="b8928-214">Så du inte behöver toodo något annat som är likvärdiga tooestablishing en `ServiceRemotingListener` som du gjorde för tillståndskänsliga hello-tjänsten i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="b8928-214">So you do not need toodo anything that is equivalent tooestablishing a `ServiceRemotingListener` like you did for hello stateful service in this tutorial.</span></span>

## <a name="how-web-services-work-on-your-local-cluster"></a><span data-ttu-id="b8928-215">Hur webbtjänster fungerar i din lokala klustret</span><span class="sxs-lookup"><span data-stu-id="b8928-215">How web services work on your local cluster</span></span>
<span data-ttu-id="b8928-216">I allmänhet kan du distribuera exakt hello samma Service Fabric application tooa flera dator kluster som du har distribuerat på din lokala klustret och vara mycket säker på att den fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="b8928-216">In general, you can deploy exactly hello same Service Fabric application tooa multi-machine cluster that you deployed on your local cluster and be highly confident that it works as you expect.</span></span> <span data-ttu-id="b8928-217">Detta beror på att din lokala klustret är helt enkelt en fem-nodkonfigurationen som är dolda tooa enskild dator.</span><span class="sxs-lookup"><span data-stu-id="b8928-217">This is because your local cluster is simply a five-node configuration that is collapsed tooa single machine.</span></span>

<span data-ttu-id="b8928-218">När det gäller tooweb tjänster, men är det en nyckel nuance.</span><span class="sxs-lookup"><span data-stu-id="b8928-218">When it comes tooweb services, however, there is one key nuance.</span></span> <span data-ttu-id="b8928-219">När klustret är placerad bakom en belastningsutjämnare i Azure, måste du se till att webbtjänster distribueras på varje dator eftersom hello belastningsutjämnaren bara round-resursallokering (robin) trafik över hello datorer.</span><span class="sxs-lookup"><span data-stu-id="b8928-219">When your cluster sits behind a load balancer, as it does in Azure, you must ensure that your web services are deployed on every machine since hello load balancer simply round-robins traffic across hello machines.</span></span> <span data-ttu-id="b8928-220">Du kan göra detta genom att ange hello `InstanceCount` för hello service toohello särskilda värdet ”1”.</span><span class="sxs-lookup"><span data-stu-id="b8928-220">You can do this by setting hello `InstanceCount` for hello service toohello special value of "-1".</span></span>

<span data-ttu-id="b8928-221">Däremot när du kör en webbtjänst lokalt, måste du tooensure att endast en instans av hello-tjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="b8928-221">By contrast, when you run a web service locally, you need tooensure that only one instance of hello service is running.</span></span> <span data-ttu-id="b8928-222">Annars kan du köra i konflikt från flera processer som lyssnar på hello samma sökväg och port.</span><span class="sxs-lookup"><span data-stu-id="b8928-222">Otherwise, you run into conflicts from multiple processes that are listening on hello same path and port.</span></span> <span data-ttu-id="b8928-223">Därför hello web service-instanser ska anges för ”1” för lokala distributioner.</span><span class="sxs-lookup"><span data-stu-id="b8928-223">As a result, hello web service instance count should be set too"1" for local deployments.</span></span>

<span data-ttu-id="b8928-224">hur tooconfigure olika värden för annan miljö, se toolearn [hantera programparametrar för miljöer med flera](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="b8928-224">toolearn how tooconfigure different values for different environment, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8928-225">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b8928-225">Next steps</span></span>
<span data-ttu-id="b8928-226">Nu när du har en web front hamna set för ditt program med ASP.NET Core, Lär dig mer om [ASP.NET Core i Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) för en djupgående förståelse av hur ASP.NET Core fungerar med Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b8928-226">Now that you have a web front end set up for your application with ASP.NET Core, learn more about [ASP.NET Core in Service Fabric Reliable Services](service-fabric-reliable-services-communication-aspnetcore.md) for an in-depth understanding of how ASP.NET Core works with Service Fabric.</span></span>

<span data-ttu-id="b8928-227">Nästa [Lär dig mer om hur du kommunicerar med services](service-fabric-connect-and-communicate-with-services.md) i allmänhet tooget en klar bild av hur tjänsten kommunikation fungerar i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="b8928-227">Next, [learn more about communicating with services](service-fabric-connect-and-communicate-with-services.md) in general tooget a complete picture of how service communication works in Service Fabric.</span></span>

<span data-ttu-id="b8928-228">När du har en god förståelse av hur tjänstkommunikation fungerar [skapa ett kluster i Azure och distribuera ditt program toohello moln](service-fabric-cluster-creation-via-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b8928-228">Once you have a good understanding of how service communication works, [create a cluster in Azure and deploy your application toohello cloud](service-fabric-cluster-creation-via-portal.md).</span></span>

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
