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
# <a name="enable-communication-for-role-instances-in-azure"></a>Aktivera kommunikation för rollinstanser i azure
Molntjänstroller kommunicerar via interna och externa anslutningar. Externa anslutningar kallas **inkommande slutpunkter** medan interna anslutningar kallas **interna slutpunkter**. Det här avsnittet beskrivs hur toomodify hello [service definition](cloud-services-model-and-package.md#csdef) toocreate slutpunkter.

## <a name="input-endpoint"></a>Slutpunkten för indata
slutpunkten för indata hello används när du vill tooexpose en port toohello utanför. Du anger hello protokolltyp och hello port för hello-slutpunkt som gäller för både hello externa och interna portar för hello slutpunkt. Om du vill kan du ange en annan Intern port för hello slutpunkten med hello [lokal port](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) attribut.

Hej indataslutpunkten kan använda hello följande protokoll: **http, https, tcp, udp**.

toocreate en slutpunkt för indata, lägga till hello **InputEndpoint** underordnade element toohello **slutpunkter** element av en webb-eller arbetarroll.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Instansen slutpunkten för indata
Instansen inkommande slutpunkter är liknande tooinput slutpunkter men kan du mappa specifika offentliga portar för varje enskild rollinstans med hjälp av vidarebefordrade portar på hello belastningsutjämnare. Du kan ange en offentlig port eller ett portintervall.

slutpunkten för indata hello instans kan bara använda **tcp** eller **udp** som hello-protokoll.

toocreate en instans slutpunkt för indata lägga till hello **InstanceInputEndpoint** underordnade element toohello **slutpunkter** element av en webb-eller arbetarroll.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Intern slutpunkt
Interna slutpunkter som är tillgängliga för instans-instans-kommunikation. hello port är valfri och om det utelämnas används en dynamisk port är tilldelad toohello slutpunkt. Ett portintervall kan användas. Det finns en gräns på fem interna slutpunkter per roll.

hello intern slutpunkt kan använda hello följande protokoll: **http, tcp, udp, alla**.

toocreate en intern slutpunkt för indata, lägga till hello **InternalEndpoint** underordnade element toohello **slutpunkter** element av en webb-eller arbetarroll.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Du kan också använda ett portintervall.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Vs för Worker-roller. Webbroller
Det finns en mindre skillnad med slutpunkter när du arbetar med både worker och webbtjänst roller. Hej webbroll måste ha minst en enda inkommande slutpunkt med hello **HTTP** protokoll.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after hello first InputEndPoint -->
</Endpoints>
```

## <a name="using-hello-net-sdk-tooaccess-an-endpoint"></a>Med hjälp av hello .NET SDK tooaccess en slutpunkt
hello Azure hanteras Library ger metoder för rollen instanser toocommunicate vid körning. Du kan hämta information om hello förekomsten av andra rollinstanser och deras slutpunkter och information om hello aktuella rollinstansen från kod som körs i en rollinstans.

> [!NOTE]
> Du kan bara hämta information om rollinstanser som körs i Molntjänsten och som definierar minst en intern slutpunkt. Du kan inte hämta data om rollinstanser som körs i en annan tjänst.
> 
> 

Du kan använda hello [instanser](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) egenskapen tooretrieve instanser av en roll. Först använda hello [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) tooreturn en referens toohello aktuella roll instans och sedan använda hello [rollen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) egenskapen tooreturn rollen referens toohello sig själv.

När du ansluter tooa rollinstansen programmässigt via hello .NET SDK är relativt enkelt tooaccess hello slutpunktsinformation. När du har redan anslutit tooa serverroll miljö, kan du få hello-port för en viss slutpunkt med den här koden:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Hej **instanser** egenskapen returnerar en mängd **RoleInstance** objekt. Den här samlingen alltid innehåller aktuella hello-instansen. Om hello roll inte definierar en intern slutpunkt inkluderar hello samling hello aktuella instansen men inga andra instanser. hello antalet rollinstanser i hello samlingen kommer alltid att 1 i hello fall där ingen intern slutpunkt har definierats för hello roll. Om hello rollen definierar en intern slutpunkt, dess instanserna är synlig vid körning och hello antalet instanser i hello samlingen motsvarar toohello antalet instanser som angetts för rollen hello i konfigurationsfilen för hello-tjänsten.

> [!NOTE]
> hello Azure hanterade biblioteket ger inte möjlighet att fastställa hello hälsotillståndet för andra rollinstanser, men du implementera sådana hälsa bedömningar själv om din tjänst måste den här funktionen. Du kan använda [Azure Diagnostics](cloud-services-dotnet-diagnostics.md) tooobtain information om hur du kör rollinstanser.
> 
> 

toodetermine hello portnumret för en intern slutpunkt på en rollinstans som du kan använda hello [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) egenskapen tooreturn ett katalogobjekt som innehåller endpoint namn och deras motsvarande IP-adresser och portar. Hej [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) -egenskap returnerar hello IP-adress och port för den angivna slutpunkten. Hej **PublicIPEndpoint** -egenskap returnerar hello-port för en slutpunkt för Utjämning av nätverksbelastning. hello IP-Adressdelen av hello **PublicIPEndpoint** egenskapen används inte.

Här är ett exempel som itererar rollinstanser.

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

Här är ett exempel på en arbetsroll som hämtar hello slutpunkten exponeras via hello tjänstdefinitionen och börjar lyssna efter anslutningar.

> [!WARNING]
> Den här koden fungerar endast för en distribuerad tjänst. När du kör i hello Azure Compute Emulator tjänsten konfigurationselement som skapar direkt port slutpunkter (**InstanceInputEndpoint** element) ignoreras.
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

## <a name="network-traffic-rules-toocontrol-role-communication"></a>Nätverkskommunikation trafik regler toocontrol roll
När du har definierat interna slutpunkter som du kan lägga till nätverket trafik regler (baserat på hello-slutpunkter som du skapade) toocontrol hur rollinstanser kan kommunicera med varandra. hello visar följande diagram några vanliga scenarier för att styra rollen kommunikation:

![Nätverk trafik regler scenarier](./media/cloud-services-enable-communication-role-instances/scenarios.png "nätverk trafik regler scenarier")

hello visar följande kodexempel rolldefinitioner för hello roller som visas i föregående diagram för hello. Varje rolldefinitionen innehåller minst en intern slutpunkt som definierats:

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
> Begränsning för kommunikation mellan roller kan uppstå med interna slutpunkter för både fasta och tilldelas automatiskt portar.
> 
> 

Som standard när en intern slutpunkt har definierats, kan kommunikation flöda från någon roll toohello interna slutpunkten för en roll utan begränsningar. toorestrict kommunikation, måste du lägga till en **NetworkTrafficRules** elementet toohello **ServiceDefinition** element i hello tjänstdefinitionsfilen.

### <a name="scenario-1"></a>Scenario 1
Tillåt endast nätverkstrafik från **WebRole1** för**WorkerRole1**.

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

### <a name="scenario-2"></a>Scenario 2
Tillåter endast nätverkstrafik från **WebRole1** för**WorkerRole1** och **WorkerRole2**.

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

### <a name="scenario-3"></a>Scenario 3
Tillåter endast nätverkstrafik från **WebRole1** för**WorkerRole1**, och **WorkerRole1** för**WorkerRole2**.

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

### <a name="scenario-4"></a>Scenario 4
Tillåter endast nätverkstrafik från **WebRole1** för**WorkerRole1**, **WebRole1** för**WorkerRole2**, och  **WorkerRole1** för**WorkerRole2**.

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

En XML-Schemareferens för hello-element som används ovan kan hittas [här](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Nästa steg
Läs mer om hello Molntjänsten [modellen](cloud-services-model-and-package.md).

