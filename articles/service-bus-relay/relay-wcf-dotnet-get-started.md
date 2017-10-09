---
title: "aaaGet igång med Azure Relay WCF-reläer i .NET | Microsoft Docs"
description: "Lär dig hur toouse Azure Relay WCF vidarebefordrar tooconnect två program som finns på olika platser."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5493281a-c2e5-49f2-87ee-9d3ffb782c75
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: a652617fc2e9b7c8d62d39fa914f77df6e3a1771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-relay-wcf-relays-with-net"></a><span data-ttu-id="9bba8-103">Hur toouse Azure Relay WCF vidarebefordrar med .NET</span><span class="sxs-lookup"><span data-stu-id="9bba8-103">How toouse Azure Relay WCF relays with .NET</span></span>
<span data-ttu-id="9bba8-104">Den här artikeln beskriver hur toouse hello Azure vidarebefordrande tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9bba8-104">This article describes how toouse hello Azure Relay service.</span></span> <span data-ttu-id="9bba8-105">hello exemplen är skrivna i C# och använder API för hello Windows Communication Foundation (WCF) med tillägg som finns i hello Service Bus-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="9bba8-105">hello samples are written in C# and use hello Windows Communication Foundation (WCF) API with extensions contained in hello Service Bus assembly.</span></span> <span data-ttu-id="9bba8-106">Mer information om Azure relay finns hello [översikt över Azure Relay](relay-what-is-it.md).</span><span class="sxs-lookup"><span data-stu-id="9bba8-106">For more information about Azure relay, see hello [Azure Relay overview](relay-what-is-it.md).</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a><span data-ttu-id="9bba8-107">Vad är WCF Relay?</span><span class="sxs-lookup"><span data-stu-id="9bba8-107">What is WCF Relay?</span></span>

<span data-ttu-id="9bba8-108">hello Azure [ *vidarebefordrande WCF* ](relay-what-is-it.md) kan du toobuild hybridprogram som körs i ett Azure-datacenter och din egen lokala företagsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9bba8-108">hello Azure [*WCF Relay*](relay-what-is-it.md) service enables you toobuild hybrid applications that run in both an Azure datacenter and your own on-premises enterprise environment.</span></span> <span data-ttu-id="9bba8-109">hello vidarebefordrande tjänsten förenklar detta genom att du toosecurely exponera tjänster för Windows Communication Foundation (WCF) som finns i ett företagsnätverk som kopplar nätverk toohello offentliga moln, utan med tooopen en brandväggsanslutning eller kräva störande ändringar tooa företagets nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="9bba8-109">hello relay service facilitates this by enabling you toosecurely expose Windows Communication Foundation (WCF) services that reside within a corporate enterprise network toohello public cloud, without having tooopen a firewall connection, or requiring intrusive changes tooa corporate network infrastructure.</span></span>

![WCF Relay-begrepp](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

<span data-ttu-id="9bba8-111">Azure Relay gör toohost WCF-tjänster i din befintliga företagsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9bba8-111">Azure Relay enables you toohost WCF services within your existing enterprise environment.</span></span> <span data-ttu-id="9bba8-112">Du kan sedan delegera lyssningen för inkommande sessioner och förfrågningar toothese WCF services toohello relay-tjänsten körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="9bba8-112">You can then delegate listening for incoming sessions and requests toothese WCF services toohello relay service running within Azure.</span></span> <span data-ttu-id="9bba8-113">Detta gör du tooexpose dessa tjänster tooapplication kod som körs i Azure, eller toomobile arbetare eller miljöer för extranätpartner.</span><span class="sxs-lookup"><span data-stu-id="9bba8-113">This enables you tooexpose these services tooapplication code running in Azure, or toomobile workers or extranet partner environments.</span></span> <span data-ttu-id="9bba8-114">Relay gör toosecurely styra vem som kan komma åt dessa tjänster på en detaljerad nivå.</span><span class="sxs-lookup"><span data-stu-id="9bba8-114">Relay enables you toosecurely control who can access these services at a fine-grained level.</span></span> <span data-ttu-id="9bba8-115">Det ger ett kraftfullt och säkert sätt tooexpose programfunktioner och data från dina befintliga företagslösningar och dra nytta av den från hello molnet.</span><span class="sxs-lookup"><span data-stu-id="9bba8-115">It provides a powerful and secure way tooexpose application functionality and data from your existing enterprise solutions and take advantage of it from hello cloud.</span></span>

<span data-ttu-id="9bba8-116">Den här artikeln beskrivs hur toouse Azure Relay toocreate WCF-webbtjänst exponeras med hjälp av en TCP-kanalbindning och som implementerar en säker konversation mellan två parter.</span><span class="sxs-lookup"><span data-stu-id="9bba8-116">This article discusses how toouse Azure Relay toocreate a WCF web service, exposed using a TCP channel binding, that implements a secure conversation between two parties.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-hello-service-bus-nuget-package"></a><span data-ttu-id="9bba8-117">Hämta hello Service Bus-NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="9bba8-117">Get hello Service Bus NuGet package</span></span>
<span data-ttu-id="9bba8-118">Hej [Service Bus NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) är hello enklaste sättet tooget hello Service Bus-API och tooconfigure ditt program med alla hello Service Bus-beroenden.</span><span class="sxs-lookup"><span data-stu-id="9bba8-118">hello [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is hello easiest way tooget hello Service Bus API and tooconfigure your application with all of hello Service Bus dependencies.</span></span> <span data-ttu-id="9bba8-119">tooinstall hello NuGet-paketet i ditt projekt hello följande:</span><span class="sxs-lookup"><span data-stu-id="9bba8-119">tooinstall hello NuGet package in your project, do hello following:</span></span>

1. <span data-ttu-id="9bba8-120">Högerklicka på **Referenser** i Solution Explorer och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="9bba8-120">In Solution Explorer, right-click **References**, then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="9bba8-121">Sök efter ”Service Bus” och välj hello **Microsoft Azure Service Bus** objekt.</span><span class="sxs-lookup"><span data-stu-id="9bba8-121">Search for "Service Bus" and select hello **Microsoft Azure Service Bus** item.</span></span> <span data-ttu-id="9bba8-122">Klicka på **installera** toocomplete hello installationen och sedan stänga hello följande dialogruta:</span><span class="sxs-lookup"><span data-stu-id="9bba8-122">Click **Install** toocomplete hello installation, then close hello following dialog box:</span></span>
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a><span data-ttu-id="9bba8-123">Exponera och använda en SOAP-webbtjänst med TCP</span><span class="sxs-lookup"><span data-stu-id="9bba8-123">Expose and consume a SOAP web service with TCP</span></span>
<span data-ttu-id="9bba8-124">tooexpose en befintlig WCF SOAP-webbtjänst för extern användning, måste du göra ändringar toohello tjänstbindningar och adresser.</span><span class="sxs-lookup"><span data-stu-id="9bba8-124">tooexpose an existing WCF SOAP web service for external consumption, you must make changes toohello service bindings and addresses.</span></span> <span data-ttu-id="9bba8-125">Du kan behöva ändringar tooyour konfigurationsfilen eller kan det krävas kodändringar, beroende på hur du har skapat och konfigurerat dina WCF-tjänster.</span><span class="sxs-lookup"><span data-stu-id="9bba8-125">This may require changes tooyour configuration file or it could require code changes, depending on how you have set up and configured your WCF services.</span></span> <span data-ttu-id="9bba8-126">Observera att WCF gör du toohave flera nätverksslutpunkter över Hej samma tjänst, så du kan behålla hello befintliga interna slutpunkter när du lägger till relay slutpunkter för extern åtkomst på hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="9bba8-126">Note that WCF allows you toohave multiple network endpoints over hello same service, so you can retain hello existing internal endpoints while adding relay endpoints for external access at hello same time.</span></span>

<span data-ttu-id="9bba8-127">I den här uppgiften att skapa en enkel WCF-tjänst och Lägg till en relay-lyssnaren tooit.</span><span class="sxs-lookup"><span data-stu-id="9bba8-127">In this task, you build a simple WCF service and add a relay listener tooit.</span></span> <span data-ttu-id="9bba8-128">Den här övningen förutsätter bekant med Visual Studio och därför igenom inte alla hello information om hur du skapar ett projekt.</span><span class="sxs-lookup"><span data-stu-id="9bba8-128">This exercise assumes some familiarity with Visual Studio, and therefore does not walk through all hello details of creating a project.</span></span> <span data-ttu-id="9bba8-129">Övningen fokuserar istället på hello-koden.</span><span class="sxs-lookup"><span data-stu-id="9bba8-129">Instead, it focuses on hello code.</span></span>

<span data-ttu-id="9bba8-130">Slutför hello följa proceduren tooset konfigurera miljön innan du påbörjar de här stegen:</span><span class="sxs-lookup"><span data-stu-id="9bba8-130">Before starting these steps, complete hello following procedure tooset up your environment:</span></span>

1. <span data-ttu-id="9bba8-131">Skapa ett konsolprogram som innehåller två projekt, ”klient” och ”tjänst” i hello lösning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9bba8-131">Within Visual Studio, create a console application that contains two projects, "Client" and "Service", within hello solution.</span></span>
2. <span data-ttu-id="9bba8-132">Lägg till hello Service Bus-NuGet-paketet tooboth projekt.</span><span class="sxs-lookup"><span data-stu-id="9bba8-132">Add hello Service Bus NuGet package tooboth projects.</span></span> <span data-ttu-id="9bba8-133">Det här paketet lägger till alla hello nödvändiga sammansättningen referenser tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="9bba8-133">This package adds all hello necessary assembly references tooyour projects.</span></span>

### <a name="how-toocreate-hello-service"></a><span data-ttu-id="9bba8-134">Hur toocreate hello service</span><span class="sxs-lookup"><span data-stu-id="9bba8-134">How toocreate hello service</span></span>
<span data-ttu-id="9bba8-135">Först skapar du själva hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9bba8-135">First, create hello service itself.</span></span> <span data-ttu-id="9bba8-136">Alla WCF-tjänster består av minst tre separata delar:</span><span class="sxs-lookup"><span data-stu-id="9bba8-136">Any WCF service consists of at least three distinct parts:</span></span>

* <span data-ttu-id="9bba8-137">Definition av ett kontrakt som beskriver vilka meddelanden som utbyts och vilka åtgärder som toobe anropas.</span><span class="sxs-lookup"><span data-stu-id="9bba8-137">Definition of a contract that describes what messages are exchanged and what operations are toobe invoked.</span></span>
* <span data-ttu-id="9bba8-138">Implementering av kontraktet.</span><span class="sxs-lookup"><span data-stu-id="9bba8-138">Implementation of that contract.</span></span>
* <span data-ttu-id="9bba8-139">Värden som är värd hello WCF-tjänst och visar flera slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="9bba8-139">Host that hosts hello WCF service and exposes several endpoints.</span></span>

<span data-ttu-id="9bba8-140">hello kodexempel i det här avsnittet adressen var och en av dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="9bba8-140">hello code examples in this section address each of these components.</span></span>

<span data-ttu-id="9bba8-141">hello kontraktet definierar en enda åtgärd `AddNumbers`, som adderar två tal och returnerar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="9bba8-141">hello contract defines a single operation, `AddNumbers`, that adds two numbers and returns hello result.</span></span> <span data-ttu-id="9bba8-142">Hej `IProblemSolverChannel` gränssnittet aktiverar hello klienten toomore enkelt hantera hello proxylivslängden.</span><span class="sxs-lookup"><span data-stu-id="9bba8-142">hello `IProblemSolverChannel` interface enables hello client toomore easily manage hello proxy lifetime.</span></span> <span data-ttu-id="9bba8-143">Att skapa ett sådant gränssnitt anses vara bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="9bba8-143">Creating such an interface is considered a best practice.</span></span> <span data-ttu-id="9bba8-144">Det är en bra idé tooput det här kontraktet definition i en separat fil så att du kan referera till filen från både ”klient” och ”tjänst” projekt, men du kan också kopiera hello koden till båda projekten.</span><span class="sxs-lookup"><span data-stu-id="9bba8-144">It's a good idea tooput this contract definition into a separate file so that you can reference that file from both your "Client" and "Service" projects, but you can also copy hello code into both projects.</span></span>

```csharp
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

<span data-ttu-id="9bba8-145">Med hello kontrakt på plats är hello-implementeringen:</span><span class="sxs-lookup"><span data-stu-id="9bba8-145">With hello contract in place, hello implementation is as follows:</span></span>

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a><span data-ttu-id="9bba8-146">Konfigurera en tjänstevärd programmässigt</span><span class="sxs-lookup"><span data-stu-id="9bba8-146">Configure a service host programmatically</span></span>
<span data-ttu-id="9bba8-147">Med hello kontraktet och implementering på plats kan värd du nu hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9bba8-147">With hello contract and implementation in place, you can now host hello service.</span></span> <span data-ttu-id="9bba8-148">Värdhanteringen utförs i en [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) objekt, som tar hand om hanteringen av hello-tjänsten och värdar hello slutpunkter som lyssnar efter meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9bba8-148">Hosting occurs inside a [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) object, which takes care of managing instances of hello service and hosts hello endpoints that listen for messages.</span></span> <span data-ttu-id="9bba8-149">hello konfigurerar följande kod hello-tjänsten med både en vanlig, lokal slutpunkt och en relay endpoint tooillustrate hello utseende, sida vid sida av interna och externa slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="9bba8-149">hello following code configures hello service with both a regular local endpoint and a relay endpoint tooillustrate hello appearance, side by side, of internal and external endpoints.</span></span> <span data-ttu-id="9bba8-150">Ersätt hello sträng *namnområde* med ditt namnområdesnamn och *yourKey* med hello SAS-nyckel som du fick i hello förra steget av installationen.</span><span class="sxs-lookup"><span data-stu-id="9bba8-150">Replace hello string *namespace* with your namespace name and *yourKey* with hello SAS key that you obtained in hello previous setup step.</span></span>

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();

sh.Close();
```

<span data-ttu-id="9bba8-151">I exemplet hello skapar du två slutpunkter som finns på hello samma kontrakt implementering.</span><span class="sxs-lookup"><span data-stu-id="9bba8-151">In hello example, you create two endpoints that are on hello same contract implementation.</span></span> <span data-ttu-id="9bba8-152">En lokal och en projiceras via Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="9bba8-152">One is local and one is projected through Azure Relay.</span></span> <span data-ttu-id="9bba8-153">hello viktigaste skillnaderna mellan dem är bindningarna hello: [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) för hello lokala punkten och [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) för hello relay slutpunkt och hello adresser.</span><span class="sxs-lookup"><span data-stu-id="9bba8-153">hello key differences between them are hello bindings; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) for hello local one and [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) for hello relay endpoint and hello addresses.</span></span> <span data-ttu-id="9bba8-154">hello lokala slutpunkten har en lokal nätverksadress med en separat port.</span><span class="sxs-lookup"><span data-stu-id="9bba8-154">hello local endpoint has a local network address with a distinct port.</span></span> <span data-ttu-id="9bba8-155">hello relay slutpunkten har en slutpunktsadress som består av hello sträng `sb`, ditt namnområdesnamn och hello sökvägen ”solver”.</span><span class="sxs-lookup"><span data-stu-id="9bba8-155">hello relay endpoint has an endpoint address composed of hello string `sb`, your namespace name, and hello path "solver."</span></span> <span data-ttu-id="9bba8-156">Detta resulterar i hello URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifierar hello tjänsteslutpunkten som en Service Bus (relay) TCP-slutpunkt med ett fullständigt kvalificerat externa DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="9bba8-156">This results in hello URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifying hello service endpoint as a Service Bus (relay) TCP endpoint with a fully qualified external DNS name.</span></span> <span data-ttu-id="9bba8-157">Om du placerar hello koden som ska ersätta platshållarna hello i hello `Main` funktion av hello **Service** program du har en fungerande tjänst.</span><span class="sxs-lookup"><span data-stu-id="9bba8-157">If you place hello code replacing hello placeholders into hello `Main` function of hello **Service** application, you will have a functional service.</span></span> <span data-ttu-id="9bba8-158">Om du vill att din tjänst toolisten enbart på hello relay bort hello lokala slutpunktsdeklarationen.</span><span class="sxs-lookup"><span data-stu-id="9bba8-158">If you want your service toolisten exclusively on hello relay, remove hello local endpoint declaration.</span></span>

### <a name="configure-a-service-host-in-hello-appconfig-file"></a><span data-ttu-id="9bba8-159">Konfigurera en tjänstevärd i hello App.config-fil</span><span class="sxs-lookup"><span data-stu-id="9bba8-159">Configure a service host in hello App.config file</span></span>
<span data-ttu-id="9bba8-160">Du kan också konfigurera hello värden med hjälp av hello App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="9bba8-160">You can also configure hello host using hello App.config file.</span></span> <span data-ttu-id="9bba8-161">hello koden för tjänstevärden i det här fallet visas i hello nästa exempel.</span><span class="sxs-lookup"><span data-stu-id="9bba8-161">hello service hosting code in this case appears in hello next example.</span></span>

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();
sh.Close();
```

<span data-ttu-id="9bba8-162">hello slutpunktsdefinitionerna flyttas till hello App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="9bba8-162">hello endpoint definitions move into hello App.config file.</span></span> <span data-ttu-id="9bba8-163">hello NuGet-paketet har redan lagt till en uppsättning definitioner toohello App.config-filen som är hello krävs configuration tillägg för Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="9bba8-163">hello NuGet package has already added a range of definitions toohello App.config file, which are hello required configuration extensions for Azure Relay.</span></span> <span data-ttu-id="9bba8-164">Hej följande exempel, som är hello exakt motsvarighet till föregående kod hello bör visas direkt under hello **system.serviceModel** element.</span><span class="sxs-lookup"><span data-stu-id="9bba8-164">hello following example, which is hello exact equivalent of hello previous code, should appear directly beneath hello **system.serviceModel** element.</span></span> <span data-ttu-id="9bba8-165">I det här kodexemplet förutsätter vi att namnområdet för ditt C#-projekt har namnet **Tjänst**.</span><span class="sxs-lookup"><span data-stu-id="9bba8-165">This code example assumes that your project C# namespace is named **Service**.</span></span>
<span data-ttu-id="9bba8-166">Ersätt hello platshållarna med namnet för relay-namnområdet och SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="9bba8-166">Replace hello placeholders with your relay namespace name and SAS key.</span></span>

```xml
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://<namespaceName>.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

<span data-ttu-id="9bba8-167">När du har gjort ändringarna hello-tjänsten startar som förut men med två live-slutpunkter: en lokal och en som lyssnar i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="9bba8-167">After you make these changes, hello service starts as it did before, but with two live endpoints: one local and one listening in hello cloud.</span></span>

### <a name="create-hello-client"></a><span data-ttu-id="9bba8-168">Skapa hello-klient</span><span class="sxs-lookup"><span data-stu-id="9bba8-168">Create hello client</span></span>
#### <a name="configure-a-client-programmatically"></a><span data-ttu-id="9bba8-169">Konfigurera en klient programmässigt</span><span class="sxs-lookup"><span data-stu-id="9bba8-169">Configure a client programmatically</span></span>
<span data-ttu-id="9bba8-170">tooconsume hello tjänsten som du kan skapa en WCF-klient som använder en [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objekt.</span><span class="sxs-lookup"><span data-stu-id="9bba8-170">tooconsume hello service, you can construct a WCF client using a [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) object.</span></span> <span data-ttu-id="9bba8-171">Service Bus använder en tokenbaserad säkerhetsmodell som implementeras med hjälp av SAS.</span><span class="sxs-lookup"><span data-stu-id="9bba8-171">Service Bus uses a token-based security model implemented using SAS.</span></span> <span data-ttu-id="9bba8-172">Hej [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) klassen representerar en leverantör av säkerhetstoken med inbyggda fabriksmetoder som returnerar vissa välkända tokenleverantörer.</span><span class="sxs-lookup"><span data-stu-id="9bba8-172">hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) class represents a security token provider with built-in factory methods that return some well-known token providers.</span></span> <span data-ttu-id="9bba8-173">hello följande exempel används hello [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metoden toohandle hello förvärv av hello lämplig SAS-token.</span><span class="sxs-lookup"><span data-stu-id="9bba8-173">hello following example uses hello [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) method toohandle hello acquisition of hello appropriate SAS token.</span></span> <span data-ttu-id="9bba8-174">är de som erhålls från hello portal som beskrivs i föregående avsnitt i hello hello namn och nyckel.</span><span class="sxs-lookup"><span data-stu-id="9bba8-174">hello name and key are those obtained from hello portal as described in hello previous section.</span></span>

<span data-ttu-id="9bba8-175">Första, referens eller kopiering hello `IProblemSolver` minimera koden från hello-tjänsten till ditt klientprojekt.</span><span class="sxs-lookup"><span data-stu-id="9bba8-175">First, reference or copy hello `IProblemSolver` contract code from hello service into your client project.</span></span>

<span data-ttu-id="9bba8-176">Ersätt sedan hello koden i hello `Main` metod för hello-klienten, Ersätt återigen platshållartexten hello med relay namnområdet och SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="9bba8-176">Then, replace hello code in hello `Main` method of hello client, again replacing hello placeholder text with your relay namespace and SAS key.</span></span>

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "<namespaceName>", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

<span data-ttu-id="9bba8-177">Du kan nu skapa hello klienten och hello-tjänsten och köra dem (kör hello tjänsten först), och hello klienten anropar hello-tjänsten och skriver ut **9**.</span><span class="sxs-lookup"><span data-stu-id="9bba8-177">You can now build hello client and hello service, run them (run hello service first), and hello client calls hello service and prints **9**.</span></span> <span data-ttu-id="9bba8-178">Du kan köra hello klienten och servern på olika datorer, även över nätverk och hello kommunikation fortfarande fungerar.</span><span class="sxs-lookup"><span data-stu-id="9bba8-178">You can run hello client and server on different machines, even across networks, and hello communication will still work.</span></span> <span data-ttu-id="9bba8-179">hello klientkoden kan också köra hello molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="9bba8-179">hello client code can also run in hello cloud or locally.</span></span>

#### <a name="configure-a-client-in-hello-appconfig-file"></a><span data-ttu-id="9bba8-180">Konfigurera en klient i hello App.config-fil</span><span class="sxs-lookup"><span data-stu-id="9bba8-180">Configure a client in hello App.config file</span></span>
<span data-ttu-id="9bba8-181">hello följande kod visar hur tooconfigure hello klienten med hjälp av hello App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="9bba8-181">hello following code shows how tooconfigure hello client using hello App.config file.</span></span>

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

<span data-ttu-id="9bba8-182">hello slutpunktsdefinitionerna flyttas till hello App.config-fil.</span><span class="sxs-lookup"><span data-stu-id="9bba8-182">hello endpoint definitions move into hello App.config file.</span></span> <span data-ttu-id="9bba8-183">hello följande exempel, som är hello samma som hello koden som listades tidigare, bör visas direkt under hello `<system.serviceModel>` element.</span><span class="sxs-lookup"><span data-stu-id="9bba8-183">hello following example, which is hello same as hello code listed previously, should appear directly beneath hello `<system.serviceModel>` element.</span></span> <span data-ttu-id="9bba8-184">Här, som tidigare måste du ersätta hello platshållarna med relay namnområdet och SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="9bba8-184">Here, as before, you must replace hello placeholders with your relay namespace and SAS key.</span></span>

```xml
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://<namespaceName>.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a><span data-ttu-id="9bba8-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9bba8-185">Next steps</span></span>
<span data-ttu-id="9bba8-186">Nu när du har lärt dig hello grunderna i Azure Relay följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="9bba8-186">Now that you've learned hello basics of Azure Relay, follow these links toolearn more.</span></span>

* [<span data-ttu-id="9bba8-187">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="9bba8-187">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="9bba8-188">Azure Service Bus-Arkitekturöversikt</span><span class="sxs-lookup"><span data-stu-id="9bba8-188">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* <span data-ttu-id="9bba8-189">Hämta Service Bus-exempel från [Azure-exempel] [ Azure samples] eller se hello [översikt över Service Bus-exempel][overview of Service Bus samples].</span><span class="sxs-lookup"><span data-stu-id="9bba8-189">Download Service Bus samples from [Azure samples][Azure samples] or see hello [overview of Service Bus samples][overview of Service Bus samples].</span></span>

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
