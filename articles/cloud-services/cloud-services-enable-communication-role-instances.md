---
title: "aaaCommunication för roller i molntjänster | Microsoft Docs"
description: "Rollinstanser i molntjänster kan ha slutpunkter (http, https, tcp, udp) har definierats för dem som kommunicerar med hello utanför eller mellan andra rollinstanser."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7008a083-acbe-4fb8-ae60-b837ef971ca1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: 1fb39215ceb8a3f0381ef5e108c1149de115ff8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-communication-for-role-instances-in-azure"></a><span data-ttu-id="68dd5-103">Aktivera kommunikation för rollinstanser i azure</span><span class="sxs-lookup"><span data-stu-id="68dd5-103">Enable communication for role instances in azure</span></span>
<span data-ttu-id="68dd5-104">Molntjänstroller kommunicerar via interna och externa anslutningar.</span><span class="sxs-lookup"><span data-stu-id="68dd5-104">Cloud service roles communicate through internal and external connections.</span></span> <span data-ttu-id="68dd5-105">Externa anslutningar kallas **inkommande slutpunkter** medan interna anslutningar kallas **interna slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="68dd5-105">External connections are called **input endpoints** while internal connections are called **internal endpoints**.</span></span> <span data-ttu-id="68dd5-106">Det här avsnittet beskrivs hur toomodify hello [service definition](cloud-services-model-and-package.md#csdef) toocreate slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="68dd5-106">This topic describes how toomodify hello [service definition](cloud-services-model-and-package.md#csdef) toocreate endpoints.</span></span>

## <a name="input-endpoint"></a><span data-ttu-id="68dd5-107">Slutpunkten för indata</span><span class="sxs-lookup"><span data-stu-id="68dd5-107">Input endpoint</span></span>
<span data-ttu-id="68dd5-108">slutpunkten för indata hello används när du vill tooexpose en port toohello utanför.</span><span class="sxs-lookup"><span data-stu-id="68dd5-108">hello input endpoint is used when you want tooexpose a port toohello outside.</span></span> <span data-ttu-id="68dd5-109">Du anger hello protokolltyp och hello port för hello-slutpunkt som gäller för både hello externa och interna portar för hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="68dd5-109">You specify hello protocol type and hello port of hello endpoint which then applies for both hello external and internal ports for hello endpoint.</span></span> <span data-ttu-id="68dd5-110">Om du vill kan du ange en annan Intern port för hello slutpunkten med hello [lokal port](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) attribut.</span><span class="sxs-lookup"><span data-stu-id="68dd5-110">If you want, you can specify a different internal port for hello endpoint with hello [localPort](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) attribute.</span></span>

<span data-ttu-id="68dd5-111">Hej indataslutpunkten kan använda hello följande protokoll: **http, https, tcp, udp**.</span><span class="sxs-lookup"><span data-stu-id="68dd5-111">hello input endpoint can use hello following protocols: **http, https, tcp, udp**.</span></span>

<span data-ttu-id="68dd5-112">toocreate en slutpunkt för indata, lägga till hello **InputEndpoint** underordnade element toohello **slutpunkter** element av en webb-eller arbetarroll.</span><span class="sxs-lookup"><span data-stu-id="68dd5-112">toocreate an input endpoint, add hello **InputEndpoint** child element toohello **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a><span data-ttu-id="68dd5-113">Instansen slutpunkten för indata</span><span class="sxs-lookup"><span data-stu-id="68dd5-113">Instance input endpoint</span></span>
<span data-ttu-id="68dd5-114">Instansen inkommande slutpunkter är liknande tooinput slutpunkter men kan du mappa specifika offentliga portar för varje enskild rollinstans med hjälp av vidarebefordrade portar på hello belastningsutjämnare.</span><span class="sxs-lookup"><span data-stu-id="68dd5-114">Instance input endpoints are similar tooinput endpoints but allows you map specific public-facing ports for each individual role instance by using port forwarding on hello load balancer.</span></span> <span data-ttu-id="68dd5-115">Du kan ange en offentlig port eller ett portintervall.</span><span class="sxs-lookup"><span data-stu-id="68dd5-115">You can specify a single public-facing port, or a range of ports.</span></span>

<span data-ttu-id="68dd5-116">slutpunkten för indata hello instans kan bara använda **tcp** eller **udp** som hello-protokoll.</span><span class="sxs-lookup"><span data-stu-id="68dd5-116">hello instance input endpoint can only use **tcp** or **udp** as hello protocol.</span></span>

<span data-ttu-id="68dd5-117">toocreate en instans slutpunkt för indata lägga till hello **InstanceInputEndpoint** underordnade element toohello **slutpunkter** element av en webb-eller arbetarroll.</span><span class="sxs-lookup"><span data-stu-id="68dd5-117">toocreate an instance input endpoint, add hello **InstanceInputEndpoint** child element toohello **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a><span data-ttu-id="68dd5-118">Intern slutpunkt</span><span class="sxs-lookup"><span data-stu-id="68dd5-118">Internal endpoint</span></span>
<span data-ttu-id="68dd5-119">Interna slutpunkter som är tillgängliga för instans-instans-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="68dd5-119">Internal endpoints are available for instance-to-instance communication.</span></span> <span data-ttu-id="68dd5-120">hello port är valfri och om det utelämnas används en dynamisk port är tilldelad toohello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="68dd5-120">hello port is optional and if omitted, a dynamic port is assigned toohello endpoint.</span></span> <span data-ttu-id="68dd5-121">Ett portintervall kan användas.</span><span class="sxs-lookup"><span data-stu-id="68dd5-121">A port range can be used.</span></span> <span data-ttu-id="68dd5-122">Det finns en gräns på fem interna slutpunkter per roll.</span><span class="sxs-lookup"><span data-stu-id="68dd5-122">There is a limit of five internal endpoints per role.</span></span>

<span data-ttu-id="68dd5-123">hello intern slutpunkt kan använda hello följande protokoll: **http, tcp, udp, alla**.</span><span class="sxs-lookup"><span data-stu-id="68dd5-123">hello internal endpoint can use hello following protocols: **http, tcp, udp, any**.</span></span>

<span data-ttu-id="68dd5-124">toocreate en intern slutpunkt för indata, lägga till hello **InternalEndpoint** underordnade element toohello **slutpunkter** element av en webb-eller arbetarroll.</span><span class="sxs-lookup"><span data-stu-id="68dd5-124">toocreate an internal input endpoint, add hello **InternalEndpoint** child element toohello **Endpoints** element of either a web or worker role.</span></span>

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

<span data-ttu-id="68dd5-125">Du kan också använda ett portintervall.</span><span class="sxs-lookup"><span data-stu-id="68dd5-125">You can also use a port range.</span></span>

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a><span data-ttu-id="68dd5-126">Vs för Worker-roller. Webbroller</span><span class="sxs-lookup"><span data-stu-id="68dd5-126">Worker roles vs. Web roles</span></span>
<span data-ttu-id="68dd5-127">Det finns en mindre skillnad med slutpunkter när du arbetar med både worker och webbtjänst roller.</span><span class="sxs-lookup"><span data-stu-id="68dd5-127">There is one minor difference with endpoints when working with both worker and web roles.</span></span> <span data-ttu-id="68dd5-128">Hej webbroll måste ha minst en enda inkommande slutpunkt med hello **HTTP** protokoll.</span><span class="sxs-lookup"><span data-stu-id="68dd5-128">hello web role must have at minimum a single input endpoint using hello **HTTP** protocol.</span></span>

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after hello first InputEndPoint -->
</Endpoints>
```

## <a name="using-hello-net-sdk-tooaccess-an-endpoint"></a><span data-ttu-id="68dd5-129">Med hjälp av hello .NET SDK tooaccess en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="68dd5-129">Using hello .NET SDK tooaccess an endpoint</span></span>
<span data-ttu-id="68dd5-130">hello Azure hanteras Library ger metoder för rollen instanser toocommunicate vid körning.</span><span class="sxs-lookup"><span data-stu-id="68dd5-130">hello Azure Managed Library provides methods for role instances toocommunicate at runtime.</span></span> <span data-ttu-id="68dd5-131">Du kan hämta information om hello förekomsten av andra rollinstanser och deras slutpunkter och information om hello aktuella rollinstansen från kod som körs i en rollinstans.</span><span class="sxs-lookup"><span data-stu-id="68dd5-131">From code running within a role instance, you can retrieve information about hello existence of other role instances and their endpoints, as well as information about hello current role instance.</span></span>

> [!NOTE]
> <span data-ttu-id="68dd5-132">Du kan bara hämta information om rollinstanser som körs i Molntjänsten och som definierar minst en intern slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="68dd5-132">You can only retrieve information about role instances that are running in your cloud service and that define at least one internal endpoint.</span></span> <span data-ttu-id="68dd5-133">Du kan inte hämta data om rollinstanser som körs i en annan tjänst.</span><span class="sxs-lookup"><span data-stu-id="68dd5-133">You cannot obtain data about role instances running in a different service.</span></span>
> 
> 

<span data-ttu-id="68dd5-134">Du kan använda hello [instanser](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) egenskapen tooretrieve instanser av en roll.</span><span class="sxs-lookup"><span data-stu-id="68dd5-134">You can use hello [Instances](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) property tooretrieve instances of a role.</span></span> <span data-ttu-id="68dd5-135">Först använda hello [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) tooreturn en referens toohello aktuella roll instans och sedan använda hello [rollen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) egenskapen tooreturn rollen referens toohello sig själv.</span><span class="sxs-lookup"><span data-stu-id="68dd5-135">First use hello [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) tooreturn a reference toohello current role instance, and then use hello [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) property tooreturn a reference toohello role itself.</span></span>

<span data-ttu-id="68dd5-136">När du ansluter tooa rollinstansen programmässigt via hello .NET SDK är relativt enkelt tooaccess hello slutpunktsinformation.</span><span class="sxs-lookup"><span data-stu-id="68dd5-136">When you connect tooa role instance programmatically through hello .NET SDK, it's relatively easy tooaccess hello endpoint information.</span></span> <span data-ttu-id="68dd5-137">När du har redan anslutit tooa serverroll miljö, kan du få hello-port för en viss slutpunkt med den här koden:</span><span class="sxs-lookup"><span data-stu-id="68dd5-137">For example, after you've already connected tooa specific role environment, you can get hello port of a specific endpoint with this code:</span></span>

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

<span data-ttu-id="68dd5-138">Hej **instanser** egenskapen returnerar en mängd **RoleInstance** objekt.</span><span class="sxs-lookup"><span data-stu-id="68dd5-138">hello **Instances** property returns a collection of **RoleInstance** objects.</span></span> <span data-ttu-id="68dd5-139">Den här samlingen alltid innehåller aktuella hello-instansen.</span><span class="sxs-lookup"><span data-stu-id="68dd5-139">This collection always contains hello current instance.</span></span> <span data-ttu-id="68dd5-140">Om hello roll inte definierar en intern slutpunkt inkluderar hello samling hello aktuella instansen men inga andra instanser.</span><span class="sxs-lookup"><span data-stu-id="68dd5-140">If hello role does not define an internal endpoint, hello collection includes hello current instance but no other instances.</span></span> <span data-ttu-id="68dd5-141">hello antalet rollinstanser i hello samlingen kommer alltid att 1 i hello fall där ingen intern slutpunkt har definierats för hello roll.</span><span class="sxs-lookup"><span data-stu-id="68dd5-141">hello number of role instances in hello collection will always be 1 in hello case where no internal endpoint is defined for hello role.</span></span> <span data-ttu-id="68dd5-142">Om hello rollen definierar en intern slutpunkt, dess instanserna är synlig vid körning och hello antalet instanser i hello samlingen motsvarar toohello antalet instanser som angetts för rollen hello i konfigurationsfilen för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="68dd5-142">If hello role defines an internal endpoint, its instances are discoverable at runtime, and hello number of instances in hello collection will correspond toohello number of instances specified for hello role in hello service configuration file.</span></span>

> [!NOTE]
> <span data-ttu-id="68dd5-143">hello Azure hanterade biblioteket ger inte möjlighet att fastställa hello hälsotillståndet för andra rollinstanser, men du implementera sådana hälsa bedömningar själv om din tjänst måste den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="68dd5-143">hello Azure Managed Library does not provide a means of determining hello health of other role instances, but you can implement such health assessments yourself if your service needs this functionality.</span></span> <span data-ttu-id="68dd5-144">Du kan använda [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) tooobtain information om hur du kör rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="68dd5-144">You can use [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) tooobtain information about running role instances.</span></span>
> 
> 

<span data-ttu-id="68dd5-145">toodetermine hello portnumret för en intern slutpunkt på en rollinstans som du kan använda hello [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) egenskapen tooreturn ett katalogobjekt som innehåller endpoint namn och deras motsvarande IP-adresser och portar.</span><span class="sxs-lookup"><span data-stu-id="68dd5-145">toodetermine hello port number for an internal endpoint on a role instance, you can use hello [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) property tooreturn a Dictionary object that contains endpoint names and their corresponding IP addresses and ports.</span></span> <span data-ttu-id="68dd5-146">Hej [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) -egenskap returnerar hello IP-adress och port för den angivna slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="68dd5-146">hello [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) property returns hello IP address and port for a specified endpoint.</span></span> <span data-ttu-id="68dd5-147">Hej **PublicIPEndpoint** -egenskap returnerar hello-port för en slutpunkt för Utjämning av nätverksbelastning.</span><span class="sxs-lookup"><span data-stu-id="68dd5-147">hello **PublicIPEndpoint** property returns hello port for a load balanced endpoint.</span></span> <span data-ttu-id="68dd5-148">hello IP-Adressdelen av hello **PublicIPEndpoint** egenskapen används inte.</span><span class="sxs-lookup"><span data-stu-id="68dd5-148">hello IP address portion of hello **PublicIPEndpoint** property is not used.</span></span>

<span data-ttu-id="68dd5-149">Här är ett exempel som itererar rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="68dd5-149">Here is an example that iterates role instances.</span></span>

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

<span data-ttu-id="68dd5-150">Här är ett exempel på en arbetsroll som hämtar hello slutpunkten exponeras via hello tjänstdefinitionen och börjar lyssna efter anslutningar.</span><span class="sxs-lookup"><span data-stu-id="68dd5-150">Here is an example of a worker role that gets hello endpoint exposed through hello service definition and starts listening for connections.</span></span>

> [!WARNING]
> <span data-ttu-id="68dd5-151">Den här koden fungerar endast för en distribuerad tjänst.</span><span class="sxs-lookup"><span data-stu-id="68dd5-151">This code will only work for a deployed service.</span></span> <span data-ttu-id="68dd5-152">När du kör i hello Azure Compute Emulator tjänsten konfigurationselement som skapar direkt port slutpunkter (**InstanceInputEndpoint** element) ignoreras.</span><span class="sxs-lookup"><span data-stu-id="68dd5-152">When running in hello Azure Compute Emulator, service configuration elements that create direct port endpoints (**InstanceInputEndpoint** elements) are ignored.</span></span>
> 
> 

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;

        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;

        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);

        // Bind socket listener toointernal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block hello thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set hello maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see hello MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-toocontrol-role-communication"></a><span data-ttu-id="68dd5-153">Nätverkskommunikation trafik regler toocontrol roll</span><span class="sxs-lookup"><span data-stu-id="68dd5-153">Network traffic rules toocontrol role communication</span></span>
<span data-ttu-id="68dd5-154">När du har definierat interna slutpunkter som du kan lägga till nätverket trafik regler (baserat på hello-slutpunkter som du skapade) toocontrol hur rollinstanser kan kommunicera med varandra.</span><span class="sxs-lookup"><span data-stu-id="68dd5-154">After you define internal endpoints, you can add network traffic rules (based on hello endpoints that you created) toocontrol how role instances can communicate with each other.</span></span> <span data-ttu-id="68dd5-155">hello visar följande diagram några vanliga scenarier för att styra rollen kommunikation:</span><span class="sxs-lookup"><span data-stu-id="68dd5-155">hello following diagram shows some common scenarios for controlling role communication:</span></span>

<span data-ttu-id="68dd5-156">![Nätverk trafik regler scenarier](./media/cloud-services-enable-communication-role-instances/scenarios.png "nätverk trafik regler scenarier")</span><span class="sxs-lookup"><span data-stu-id="68dd5-156">![Network Traffic Rules Scenarios](./media/cloud-services-enable-communication-role-instances/scenarios.png "Network Traffic Rules Scenarios")</span></span>

<span data-ttu-id="68dd5-157">hello visar följande kodexempel rolldefinitioner för hello roller som visas i föregående diagram för hello.</span><span class="sxs-lookup"><span data-stu-id="68dd5-157">hello following code example shows role definitions for hello roles shown in hello previous diagram.</span></span> <span data-ttu-id="68dd5-158">Varje rolldefinitionen innehåller minst en intern slutpunkt som definierats:</span><span class="sxs-lookup"><span data-stu-id="68dd5-158">Each role definition includes at least one internal endpoint defined:</span></span>

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [!NOTE]
> <span data-ttu-id="68dd5-159">Begränsning för kommunikation mellan roller kan uppstå med interna slutpunkter för både fasta och tilldelas automatiskt portar.</span><span class="sxs-lookup"><span data-stu-id="68dd5-159">Restriction of communication between roles can occur with internal endpoints of both fixed and automatically assigned ports.</span></span>
> 
> 

<span data-ttu-id="68dd5-160">Som standard när en intern slutpunkt har definierats, kan kommunikation flöda från någon roll toohello interna slutpunkten för en roll utan begränsningar.</span><span class="sxs-lookup"><span data-stu-id="68dd5-160">By default, after an internal endpoint is defined, communication can flow from any role toohello internal endpoint of a role without any restrictions.</span></span> <span data-ttu-id="68dd5-161">toorestrict kommunikation, måste du lägga till en **NetworkTrafficRules** elementet toohello **ServiceDefinition** element i hello tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="68dd5-161">toorestrict communication, you must add a **NetworkTrafficRules** element toohello **ServiceDefinition** element in hello service definition file.</span></span>

### <a name="scenario-1"></a><span data-ttu-id="68dd5-162">Scenario 1</span><span class="sxs-lookup"><span data-stu-id="68dd5-162">Scenario 1</span></span>
<span data-ttu-id="68dd5-163">Tillåt endast nätverkstrafik från **WebRole1** för**WorkerRole1**.</span><span class="sxs-lookup"><span data-stu-id="68dd5-163">Only allow network traffic from **WebRole1** too**WorkerRole1**.</span></span>

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a><span data-ttu-id="68dd5-164">Scenario 2</span><span class="sxs-lookup"><span data-stu-id="68dd5-164">Scenario 2</span></span>
<span data-ttu-id="68dd5-165">Tillåter endast nätverkstrafik från **WebRole1** för**WorkerRole1** och **WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="68dd5-165">Only allows network traffic from **WebRole1** too**WorkerRole1** and **WorkerRole2**.</span></span>

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a><span data-ttu-id="68dd5-166">Scenario 3</span><span class="sxs-lookup"><span data-stu-id="68dd5-166">Scenario 3</span></span>
<span data-ttu-id="68dd5-167">Tillåter endast nätverkstrafik från **WebRole1** för**WorkerRole1**, och **WorkerRole1** för**WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="68dd5-167">Only allows network traffic from **WebRole1** too**WorkerRole1**, and **WorkerRole1** too**WorkerRole2**.</span></span>

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a><span data-ttu-id="68dd5-168">Scenario 4</span><span class="sxs-lookup"><span data-stu-id="68dd5-168">Scenario 4</span></span>
<span data-ttu-id="68dd5-169">Tillåter endast nätverkstrafik från **WebRole1** för**WorkerRole1**, **WebRole1** för**WorkerRole2**, och  **WorkerRole1** för**WorkerRole2**.</span><span class="sxs-lookup"><span data-stu-id="68dd5-169">Only allows network traffic from **WebRole1** too**WorkerRole1**, **WebRole1** too**WorkerRole2**, and **WorkerRole1** too**WorkerRole2**.</span></span>

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

<span data-ttu-id="68dd5-170">En XML-Schemareferens för hello-element som används ovan kan hittas [här](https://msdn.microsoft.com/library/azure/gg557551.aspx).</span><span class="sxs-lookup"><span data-stu-id="68dd5-170">An XML schema reference for hello elements used above can be found [here](https://msdn.microsoft.com/library/azure/gg557551.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="68dd5-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68dd5-171">Next steps</span></span>
<span data-ttu-id="68dd5-172">Läs mer om hello Molntjänsten [modellen](cloud-services-model-and-package.md).</span><span class="sxs-lookup"><span data-stu-id="68dd5-172">Read more about hello Cloud Service [model](cloud-services-model-and-package.md).</span></span>

