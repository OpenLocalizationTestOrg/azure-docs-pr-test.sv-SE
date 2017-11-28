---
title: "Kom igång med Azure Relay WCF vidarebefordrar i .NET | Microsoft Docs"
description: "Lär dig hur du använder Azure Relay WCF reläer för att ansluta två appar som finns på olika platser."
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
ms.openlocfilehash: 1af1ac78398d65e6a87f0d24d6198f3dfbc82ffd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-relay-wcf-relays-with-net"></a><span data-ttu-id="282ef-103">Hur du använder Azure Relay WCF vidarebefordrar med .NET</span><span class="sxs-lookup"><span data-stu-id="282ef-103">How to use Azure Relay WCF relays with .NET</span></span>
<span data-ttu-id="282ef-104">Den här artikeln beskriver hur du använder tjänsten Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="282ef-104">This article describes how to use the Azure Relay service.</span></span> <span data-ttu-id="282ef-105">Exemplen är skrivna i C# och använder API:et Windows Communication Foundation (WCF) med tillägg som finns i Service Bus-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="282ef-105">The samples are written in C# and use the Windows Communication Foundation (WCF) API with extensions contained in the Service Bus assembly.</span></span> <span data-ttu-id="282ef-106">Mer information om Azure relay finns i [översikt över Azure Relay](relay-what-is-it.md).</span><span class="sxs-lookup"><span data-stu-id="282ef-106">For more information about Azure relay, see the [Azure Relay overview](relay-what-is-it.md).</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a><span data-ttu-id="282ef-107">Vad är WCF Relay?</span><span class="sxs-lookup"><span data-stu-id="282ef-107">What is WCF Relay?</span></span>

<span data-ttu-id="282ef-108">Azure [ *vidarebefordrande WCF* ](relay-what-is-it.md) kan du skapa hybridprogram som körs i både ett Azure-datacenter och din egen lokala företagsmiljö.</span><span class="sxs-lookup"><span data-stu-id="282ef-108">The Azure [*WCF Relay*](relay-what-is-it.md) service enables you to build hybrid applications that run in both an Azure datacenter and your own on-premises enterprise environment.</span></span> <span data-ttu-id="282ef-109">Tjänsten relay förenklar detta genom att på ett säkert sätt exponera tjänster för Windows Communication Foundation (WCF) som finns i ett företagsnätverk mot det offentliga molnet, utan att behöva öppna en brandväggsanslutning eller kräva störande ändringar i företagets nätverksinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="282ef-109">The relay service facilitates this by enabling you to securely expose Windows Communication Foundation (WCF) services that reside within a corporate enterprise network to the public cloud, without having to open a firewall connection, or requiring intrusive changes to a corporate network infrastructure.</span></span>

![WCF Relay-begrepp](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

<span data-ttu-id="282ef-111">Du kan värden WCF-tjänster i din befintliga företagsmiljö Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="282ef-111">Azure Relay enables you to host WCF services within your existing enterprise environment.</span></span> <span data-ttu-id="282ef-112">Du kan sedan delegera lyssningen för inkommande sessioner och förfrågningar till de här WCF-tjänster till den vidarebefordrande tjänsten som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="282ef-112">You can then delegate listening for incoming sessions and requests to these WCF services to the relay service running within Azure.</span></span> <span data-ttu-id="282ef-113">Tack vare detta kan du exponera dessa tjänster för programkoden som körs i Azure, eller för mobila arbetare eller miljöer för extranätpartner.</span><span class="sxs-lookup"><span data-stu-id="282ef-113">This enables you to expose these services to application code running in Azure, or to mobile workers or extranet partner environments.</span></span> <span data-ttu-id="282ef-114">Du kan på ett säkert sätt styra åtkomsten till dessa tjänster på en detaljerad nivå Relay.</span><span class="sxs-lookup"><span data-stu-id="282ef-114">Relay enables you to securely control who can access these services at a fine-grained level.</span></span> <span data-ttu-id="282ef-115">Det ger dig ett kraftfullt och säkert sätt att exponera programfunktioner och data från dina befintliga företagslösningar och dra nytta av dessa från molnet.</span><span class="sxs-lookup"><span data-stu-id="282ef-115">It provides a powerful and secure way to expose application functionality and data from your existing enterprise solutions and take advantage of it from the cloud.</span></span>

<span data-ttu-id="282ef-116">Den här artikeln beskriver hur du använder Azure Relay för att skapa en WCF-webbtjänst exponeras med hjälp av en bindning, TCP-kanal som implementerar en säker konversation mellan två parter.</span><span class="sxs-lookup"><span data-stu-id="282ef-116">This article discusses how to use Azure Relay to create a WCF web service, exposed using a TCP channel binding, that implements a secure conversation between two parties.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a><span data-ttu-id="282ef-117">Hämta Service Bus-NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="282ef-117">Get the Service Bus NuGet package</span></span>
<span data-ttu-id="282ef-118">[Service Bus-NuGet-paketet](https://www.nuget.org/packages/WindowsAzure.ServiceBus) är det enklaste sättet att komma åt Service Bus-API:et och att konfigurera din app med alla Service Bus-beroenden.</span><span class="sxs-lookup"><span data-stu-id="282ef-118">The [Service Bus NuGet package](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is the easiest way to get the Service Bus API and to configure your application with all of the Service Bus dependencies.</span></span> <span data-ttu-id="282ef-119">Om du vill installera NuGet-paketet i ditt projekt gör du följande:</span><span class="sxs-lookup"><span data-stu-id="282ef-119">To install the NuGet package in your project, do the following:</span></span>

1. <span data-ttu-id="282ef-120">Högerklicka på **Referenser** i Solution Explorer och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="282ef-120">In Solution Explorer, right-click **References**, then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="282ef-121">Sök efter "Service Bus" och markera posten **Microsoft Azure Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="282ef-121">Search for "Service Bus" and select the **Microsoft Azure Service Bus** item.</span></span> <span data-ttu-id="282ef-122">Klicka på **Installera** för att slutföra installationen och stäng sedan följande dialogruta:</span><span class="sxs-lookup"><span data-stu-id="282ef-122">Click **Install** to complete the installation, then close the following dialog box:</span></span>
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a><span data-ttu-id="282ef-123">Exponera och använda en SOAP-webbtjänst med TCP</span><span class="sxs-lookup"><span data-stu-id="282ef-123">Expose and consume a SOAP web service with TCP</span></span>
<span data-ttu-id="282ef-124">Om du vill exponera en befintlig WCF SOAP-webbtjänst för extern användning, måste du göra ändringar i tjänstebindningarna och i adresserna.</span><span class="sxs-lookup"><span data-stu-id="282ef-124">To expose an existing WCF SOAP web service for external consumption, you must make changes to the service bindings and addresses.</span></span> <span data-ttu-id="282ef-125">Det här kan kräva att du även gör ändringar i konfigurationsfilen, eller i koden, beroende på hur du har skapat och konfigurerat dina WCF-tjänster.</span><span class="sxs-lookup"><span data-stu-id="282ef-125">This may require changes to your configuration file or it could require code changes, depending on how you have set up and configured your WCF services.</span></span> <span data-ttu-id="282ef-126">Observera att WCF gör att du kan ha flera nätverksslutpunkter inom samma tjänst så att du kan behålla de befintliga, interna slutpunkterna när du lägger till relay slutpunkter för extern åtkomst på samma gång.</span><span class="sxs-lookup"><span data-stu-id="282ef-126">Note that WCF allows you to have multiple network endpoints over the same service, so you can retain the existing internal endpoints while adding relay endpoints for external access at the same time.</span></span>

<span data-ttu-id="282ef-127">I den här uppgiften att skapa en enkel WCF-tjänst och Lägg till en relay-lyssnare.</span><span class="sxs-lookup"><span data-stu-id="282ef-127">In this task, you build a simple WCF service and add a relay listener to it.</span></span> <span data-ttu-id="282ef-128">Den här övningen förutsätter att du är bekant med Visual Studio och går därför inte igenom alla detaljer om hur man skapar ett projekt.</span><span class="sxs-lookup"><span data-stu-id="282ef-128">This exercise assumes some familiarity with Visual Studio, and therefore does not walk through all the details of creating a project.</span></span> <span data-ttu-id="282ef-129">Övningen fokuserar istället på koden.</span><span class="sxs-lookup"><span data-stu-id="282ef-129">Instead, it focuses on the code.</span></span>

<span data-ttu-id="282ef-130">Innan du börjar med dessa steg måste du slutföra följande procedur för att göra justeringar i din miljö:</span><span class="sxs-lookup"><span data-stu-id="282ef-130">Before starting these steps, complete the following procedure to set up your environment:</span></span>

1. <span data-ttu-id="282ef-131">Skapa ett konsolprogram i Visual Studio som innehåller två projekt, ”Klient” och ”Tjänst”, inne i lösningen.</span><span class="sxs-lookup"><span data-stu-id="282ef-131">Within Visual Studio, create a console application that contains two projects, "Client" and "Service", within the solution.</span></span>
2. <span data-ttu-id="282ef-132">Lägga till Service Bus NuGet-paketet i båda projekten.</span><span class="sxs-lookup"><span data-stu-id="282ef-132">Add the Service Bus NuGet package to both projects.</span></span> <span data-ttu-id="282ef-133">Detta paket lägger till alla nödvändiga sammansättningsreferenser i dina projekt.</span><span class="sxs-lookup"><span data-stu-id="282ef-133">This package adds all the necessary assembly references to your projects.</span></span>

### <a name="how-to-create-the-service"></a><span data-ttu-id="282ef-134">Så här skapar du tjänsten</span><span class="sxs-lookup"><span data-stu-id="282ef-134">How to create the service</span></span>
<span data-ttu-id="282ef-135">Först skapar du själva tjänsten.</span><span class="sxs-lookup"><span data-stu-id="282ef-135">First, create the service itself.</span></span> <span data-ttu-id="282ef-136">Alla WCF-tjänster består av minst tre separata delar:</span><span class="sxs-lookup"><span data-stu-id="282ef-136">Any WCF service consists of at least three distinct parts:</span></span>

* <span data-ttu-id="282ef-137">Definitionen av ett kontrakt, som beskriver vilka meddelanden som ska bytas ut och vilka åtgärder som ska anropas.</span><span class="sxs-lookup"><span data-stu-id="282ef-137">Definition of a contract that describes what messages are exchanged and what operations are to be invoked.</span></span>
* <span data-ttu-id="282ef-138">Implementering av kontraktet.</span><span class="sxs-lookup"><span data-stu-id="282ef-138">Implementation of that contract.</span></span>
* <span data-ttu-id="282ef-139">Värden som används som värd för WCF-tjänsten och exponerar ett flera slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="282ef-139">Host that hosts the WCF service and exposes several endpoints.</span></span>

<span data-ttu-id="282ef-140">Kodexemplet i det här avsnittet tar upp var och en av dessa komponenter i detalj.</span><span class="sxs-lookup"><span data-stu-id="282ef-140">The code examples in this section address each of these components.</span></span>

<span data-ttu-id="282ef-141">Kontraktet definierar en enda åtgärd `AddNumbers`, som adderar två tal och returnerar resultatet.</span><span class="sxs-lookup"><span data-stu-id="282ef-141">The contract defines a single operation, `AddNumbers`, that adds two numbers and returns the result.</span></span> <span data-ttu-id="282ef-142">Gränssnittet `IProblemSolverChannel` gör att klienten enklare kan hantera proxylivslängden.</span><span class="sxs-lookup"><span data-stu-id="282ef-142">The `IProblemSolverChannel` interface enables the client to more easily manage the proxy lifetime.</span></span> <span data-ttu-id="282ef-143">Att skapa ett sådant gränssnitt anses vara bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="282ef-143">Creating such an interface is considered a best practice.</span></span> <span data-ttu-id="282ef-144">Det är en bra idé att placera kontraktdefinitionen i en separat fil så att du kan referera till filen från både ”Klient”- och ”Tjänst”-projektet, men du kan också kopiera koden till båda projekten.</span><span class="sxs-lookup"><span data-stu-id="282ef-144">It's a good idea to put this contract definition into a separate file so that you can reference that file from both your "Client" and "Service" projects, but you can also copy the code into both projects.</span></span>

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

<span data-ttu-id="282ef-145">Med kontrakt på plats är implementeringen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="282ef-145">With the contract in place, the implementation is as follows:</span></span>

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a><span data-ttu-id="282ef-146">Konfigurera en tjänstevärd programmässigt</span><span class="sxs-lookup"><span data-stu-id="282ef-146">Configure a service host programmatically</span></span>
<span data-ttu-id="282ef-147">När kontraktet är på plats och implementeringen har genomförts, kan du nu vara värd för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="282ef-147">With the contract and implementation in place, you can now host the service.</span></span> <span data-ttu-id="282ef-148">Värdhanteringen utförs inne i ett [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx)-objekt som tar hand om hanteringen av tjänsteinstanserna och fungerar som värd för de slutpunkter som lyssnar efter meddelanden.</span><span class="sxs-lookup"><span data-stu-id="282ef-148">Hosting occurs inside a [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) object, which takes care of managing instances of the service and hosts the endpoints that listen for messages.</span></span> <span data-ttu-id="282ef-149">Följande kod konfigurerar tjänsten med både en vanlig, lokal slutpunkt och en relay-slutpunkt för att visa utseende, sida vid sida av interna och externa slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="282ef-149">The following code configures the service with both a regular local endpoint and a relay endpoint to illustrate the appearance, side by side, of internal and external endpoints.</span></span> <span data-ttu-id="282ef-150">Ersätt strängen *namnområde* med ditt namnområdesnamn och *yourKey* med den SAS-nyckel som du fick i det förra steget av installationen.</span><span class="sxs-lookup"><span data-stu-id="282ef-150">Replace the string *namespace* with your namespace name and *yourKey* with the SAS key that you obtained in the previous setup step.</span></span>

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

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

<span data-ttu-id="282ef-151">I det här exemplet skapar du två slutpunkter som ligger på samma kontraktsimplementering.</span><span class="sxs-lookup"><span data-stu-id="282ef-151">In the example, you create two endpoints that are on the same contract implementation.</span></span> <span data-ttu-id="282ef-152">En lokal och en projiceras via Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="282ef-152">One is local and one is projected through Azure Relay.</span></span> <span data-ttu-id="282ef-153">De viktigaste skillnaderna mellan dem är bindningarna: [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) för den lokala och [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) för relay-slutpunkten och adresserna.</span><span class="sxs-lookup"><span data-stu-id="282ef-153">The key differences between them are the bindings; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) for the local one and [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) for the relay endpoint and the addresses.</span></span> <span data-ttu-id="282ef-154">Den lokala slutpunkten har en lokal nätverksadress med en separat port.</span><span class="sxs-lookup"><span data-stu-id="282ef-154">The local endpoint has a local network address with a distinct port.</span></span> <span data-ttu-id="282ef-155">Relay-slutpunkten har en slutpunktsadress som består av strängen `sb`, ditt namnområdesnamn och sökvägen ”solver”.</span><span class="sxs-lookup"><span data-stu-id="282ef-155">The relay endpoint has an endpoint address composed of the string `sb`, your namespace name, and the path "solver."</span></span> <span data-ttu-id="282ef-156">Detta resulterar i URI: N `sb://[serviceNamespace].servicebus.windows.net/solver`, identifierar tjänsteslutpunkten som en Service Bus (relay) TCP-slutpunkt med ett fullständigt kvalificerat externa DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="282ef-156">This results in the URI `sb://[serviceNamespace].servicebus.windows.net/solver`, identifying the service endpoint as a Service Bus (relay) TCP endpoint with a fully qualified external DNS name.</span></span> <span data-ttu-id="282ef-157">Om du placerar ut koden som ska ersätta platshållarna i `Main`-funktionen i **Tjänsteprogrammet** kommer du att få en fungerande tjänst.</span><span class="sxs-lookup"><span data-stu-id="282ef-157">If you place the code replacing the placeholders into the `Main` function of the **Service** application, you will have a functional service.</span></span> <span data-ttu-id="282ef-158">Om du vill att din tjänst endast ska lyssna på vidarebefordran kan du ta bort den lokala slutpunktsdeklarationen.</span><span class="sxs-lookup"><span data-stu-id="282ef-158">If you want your service to listen exclusively on the relay, remove the local endpoint declaration.</span></span>

### <a name="configure-a-service-host-in-the-appconfig-file"></a><span data-ttu-id="282ef-159">Konfigurera en tjänstevärd i filen App.config</span><span class="sxs-lookup"><span data-stu-id="282ef-159">Configure a service host in the App.config file</span></span>
<span data-ttu-id="282ef-160">Du kan också konfigurera värden med hjälp av filen App.config.</span><span class="sxs-lookup"><span data-stu-id="282ef-160">You can also configure the host using the App.config file.</span></span> <span data-ttu-id="282ef-161">Koden för tjänstevärden i det här fallet visas i nästa exempel.</span><span class="sxs-lookup"><span data-stu-id="282ef-161">The service hosting code in this case appears in the next example.</span></span>

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

<span data-ttu-id="282ef-162">Slutpunktsdefinitionerna flyttas till filen App.config.</span><span class="sxs-lookup"><span data-stu-id="282ef-162">The endpoint definitions move into the App.config file.</span></span> <span data-ttu-id="282ef-163">NuGet-paketet har redan lagts till en uppsättning definitioner i filen App.config som är nödvändig konfiguration-tillägg för Azure Relay.</span><span class="sxs-lookup"><span data-stu-id="282ef-163">The NuGet package has already added a range of definitions to the App.config file, which are the required configuration extensions for Azure Relay.</span></span> <span data-ttu-id="282ef-164">Följande exempel, som är en exakt motsvarighet till föregående kod, bör visas direkt under elementet **system.serviceModel**.</span><span class="sxs-lookup"><span data-stu-id="282ef-164">The following example, which is the exact equivalent of the previous code, should appear directly beneath the **system.serviceModel** element.</span></span> <span data-ttu-id="282ef-165">I det här kodexemplet förutsätter vi att namnområdet för ditt C#-projekt har namnet **Tjänst**.</span><span class="sxs-lookup"><span data-stu-id="282ef-165">This code example assumes that your project C# namespace is named **Service**.</span></span>
<span data-ttu-id="282ef-166">Ersätt platshållarna med namnet för relay-namnområdet och SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="282ef-166">Replace the placeholders with your relay namespace name and SAS key.</span></span>

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

<span data-ttu-id="282ef-167">När du har gjort dessa ändringar, startar tjänsten som förut men med två live-slutpunkter: en lokal och en som lyssnar i molnet.</span><span class="sxs-lookup"><span data-stu-id="282ef-167">After you make these changes, the service starts as it did before, but with two live endpoints: one local and one listening in the cloud.</span></span>

### <a name="create-the-client"></a><span data-ttu-id="282ef-168">Skapa klienten</span><span class="sxs-lookup"><span data-stu-id="282ef-168">Create the client</span></span>
#### <a name="configure-a-client-programmatically"></a><span data-ttu-id="282ef-169">Konfigurera en klient programmässigt</span><span class="sxs-lookup"><span data-stu-id="282ef-169">Configure a client programmatically</span></span>
<span data-ttu-id="282ef-170">Om du vill använda tjänsten, kan du skapa en WCF-klient som använder ett [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx)-objekt.</span><span class="sxs-lookup"><span data-stu-id="282ef-170">To consume the service, you can construct a WCF client using a [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) object.</span></span> <span data-ttu-id="282ef-171">Service Bus använder en tokenbaserad säkerhetsmodell som implementeras med hjälp av SAS.</span><span class="sxs-lookup"><span data-stu-id="282ef-171">Service Bus uses a token-based security model implemented using SAS.</span></span> <span data-ttu-id="282ef-172">Klassen [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) representerar en leverantör av säkerhetstoken med inbyggda fabriksmetoder som returnerar ett antal välkända tokenleverantörer.</span><span class="sxs-lookup"><span data-stu-id="282ef-172">The [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) class represents a security token provider with built-in factory methods that return some well-known token providers.</span></span> <span data-ttu-id="282ef-173">I följande exempel används metoden [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) för att hantera förvärvet av lämplig SAS-token.</span><span class="sxs-lookup"><span data-stu-id="282ef-173">The following example uses the [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) method to handle the acquisition of the appropriate SAS token.</span></span> <span data-ttu-id="282ef-174">Namnet och nyckeln är de som du fick från portalen, enligt beskrivningen i det förra avsnittet.</span><span class="sxs-lookup"><span data-stu-id="282ef-174">The name and key are those obtained from the portal as described in the previous section.</span></span>

<span data-ttu-id="282ef-175">Först måste du referera eller kopiera kontraktkoden `IProblemSolver` från tjänsten till ditt klientprojekt.</span><span class="sxs-lookup"><span data-stu-id="282ef-175">First, reference or copy the `IProblemSolver` contract code from the service into your client project.</span></span>

<span data-ttu-id="282ef-176">Ersätt Koden i den `Main` metoden för klienten, Ersätt återigen platshållartexten med relay namnområdet och SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="282ef-176">Then, replace the code in the `Main` method of the client, again replacing the placeholder text with your relay namespace and SAS key.</span></span>

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

<span data-ttu-id="282ef-177">Du kan nu skapa klienten och tjänsten och köra dem (kör tjänsten först). Klienten anropar tjänsten och skriver ut **9**.</span><span class="sxs-lookup"><span data-stu-id="282ef-177">You can now build the client and the service, run them (run the service first), and the client calls the service and prints **9**.</span></span> <span data-ttu-id="282ef-178">Du kan köra klient och servern på olika datorer, även över nätverk. Kommunikationen kommer ändå att fungera.</span><span class="sxs-lookup"><span data-stu-id="282ef-178">You can run the client and server on different machines, even across networks, and the communication will still work.</span></span> <span data-ttu-id="282ef-179">Klientkoden kan också köras i molnet eller lokalt.</span><span class="sxs-lookup"><span data-stu-id="282ef-179">The client code can also run in the cloud or locally.</span></span>

#### <a name="configure-a-client-in-the-appconfig-file"></a><span data-ttu-id="282ef-180">Konfigurera en klient i filen App.config</span><span class="sxs-lookup"><span data-stu-id="282ef-180">Configure a client in the App.config file</span></span>
<span data-ttu-id="282ef-181">Följande kod visar hur du konfigurerar klienten med hjälp av filen App.config.</span><span class="sxs-lookup"><span data-stu-id="282ef-181">The following code shows how to configure the client using the App.config file.</span></span>

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

<span data-ttu-id="282ef-182">Slutpunktsdefinitionerna flyttas till filen App.config.</span><span class="sxs-lookup"><span data-stu-id="282ef-182">The endpoint definitions move into the App.config file.</span></span> <span data-ttu-id="282ef-183">I följande exempel, som är samma som koden som listades tidigare, bör visas direkt under den `<system.serviceModel>` element.</span><span class="sxs-lookup"><span data-stu-id="282ef-183">The following example, which is the same as the code listed previously, should appear directly beneath the `<system.serviceModel>` element.</span></span> <span data-ttu-id="282ef-184">Här, precis som tidigare, måste du ersätta platshållarna med relay namnområdet och SAS-nyckel.</span><span class="sxs-lookup"><span data-stu-id="282ef-184">Here, as before, you must replace the placeholders with your relay namespace and SAS key.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="282ef-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="282ef-185">Next steps</span></span>
<span data-ttu-id="282ef-186">Nu när du har lärt dig grunderna i Azure Relay, kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="282ef-186">Now that you've learned the basics of Azure Relay, follow these links to learn more.</span></span>

* [<span data-ttu-id="282ef-187">Vad är Azure Relay?</span><span class="sxs-lookup"><span data-stu-id="282ef-187">What is Azure Relay?</span></span>](relay-what-is-it.md)
* [<span data-ttu-id="282ef-188">Azure Service Bus-Arkitekturöversikt</span><span class="sxs-lookup"><span data-stu-id="282ef-188">Azure Service Bus architectural overview</span></span>](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* <span data-ttu-id="282ef-189">Hämta Service Bus-exempel från [Azure-exempel] [ Azure samples] eller finns den [översikt över Service Bus-exempel][overview of Service Bus samples].</span><span class="sxs-lookup"><span data-stu-id="282ef-189">Download Service Bus samples from [Azure samples][Azure samples] or see the [overview of Service Bus samples][overview of Service Bus samples].</span></span>

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
