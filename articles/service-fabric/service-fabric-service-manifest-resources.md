---
title: "aaaSpecifying Service Fabric-Tjänsteslutpunkter | Microsoft Docs"
description: Hur toodescribe endpoint resurser i en service manifest, inklusive hur tooset av HTTPS-slutpunkter
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: da36cbdb-6531-4dae-88e8-a311ab71520d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: subramar
ms.openlocfilehash: a4ebee353ce5cf86583673674246094f03f368be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="specify-resources-in-a-service-manifest"></a>Ange resurser i ett tjänstmanifest
## <a name="overview"></a>Översikt
hello tjänstmanifestet kan resurser som används av hello service toobe deklarerats eller har ändrats utan att ändra hello kompilerad kod. Azure Service Fabric stöder konfiguration av slutpunkten resurser för hello-tjänsten. Hej toohello resurser som har angetts i hello tjänstmanifestet kan kontrolleras via hello säkerhetsgrupp i hello programmanifestet. hello deklarationen av resurser kan dessa resurser toobe ändras vid tidpunkten för distribution, vilket innebär att hello tjänsten inte behöver toointroduce en ny konfiguration mekanism. hello schemadefinition för hello ServiceManifest.xml fil har installerats med hello Service Fabric-SDK och verktyg för*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

## <a name="endpoints"></a>Slutpunkter
När en resurs för slutpunkten har definierats i hello tjänstmanifestet tilldelar Service Fabric portar från hello reserverade portintervall för programmet när en port inte är anges explicit. Titta exempelvis på hello endpoint *ServiceEndpoint1* anges i hello manifestet fragment som efter denna punkt. Tjänster kan dessutom också begära en specifik port i en resurs. Tjänsten repliker som körs på olika noder kan tilldelas olika portnummer, medan repliker av en tjänst som körs på hello samma nod resursen hello port. hello service repliker kan sedan använda dessa portar som behövs för replikering och lyssnar efter förfrågningar från klienter.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Se för[konfigurera tillståndskänsliga Reliable Services](service-fabric-reliable-services-configuration.md) tooread mer om refererar till slutpunkter från hello paketet inställningar konfigurationsfilen (settings.xml).

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Exempel: Om du anger en HTTP-slutpunkt för tjänsten
hello följande tjänstmanifestet definierar en resurs för TCP-slutpunkt och två http-slutpunkt resurser i hello &lt;resurser&gt; element.

HTTP-slutpunkter är automatiskt ACL skulle av Service Fabric.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is hello name of your ServiceType.
         This name must match hello string used in hello RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is hello contents of hello Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by hello replicator for replicating hello state of your service.
           This endpoint is configured through hello ReplicatorSettings config section in hello Settings.xml
           file under hello ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Exempel: Om du anger en HTTPS-slutpunkt för tjänsten
hello HTTPS-protokollet ger serverautentisering och används också för att kryptera kommunikationen mellan klient-server. tooenable HTTPS på din Service Fabric-tjänsten, ange hello-protokollet i hello *resurser -> slutpunkter Endpoint ->* avsnitt i hello tjänstmanifestet, enligt tidigare för hello slutpunkten *ServiceEndpoint3* .

> [!NOTE]
> En tjänst-protokollet kan inte ändras under uppgradering av programmet. Om den har ändrats under uppgraderingen, är en ny ändring.
> 
> 

Här är ett exempel ApplicationManifest måste tooset för HTTPS. hello tumavtrycket för certifikatet måste anges. Hej EndpointRef är en referens tooEndpointResource i ServiceManifest, som du angett hello HTTPS-protokollet. Du kan lägga till fler än en EndpointCertificate.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import hello ServiceManifest from hello ServicePackage. hello ServiceManifestName and ServiceManifestVersion
       should match hello Name and Version attributes of hello ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- hello section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         hello attribute ServiceTypeName below must match hello name defined in hello imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_ ]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```

## <a name="overriding-endpoints-in-servicemanifestxml"></a>Åsidosättning av slutpunkterna i ServiceManifest.xml

Lägg till ett ResourceOverrides avsnitt som är ett objekt på samma nivå tooConfigOverrides avsnitt i hello ApplicationManifest. Du kan ange hello åsidosättande för hello slutpunkter i hello resurser avsnittet anges i hello tjänstmanifestet i det här avsnittet.

I ordning toooverride slutpunkt i ServiceManifest med ApplicationParameters ändra hello ApplicationManifest enligt följande:

Lägga till ett nytt avsnitt ”ResourceOverrides” Hej ServiceManifestImport avsnitt

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="ServiceEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
        <Endpoint Name="ServiceEndpoint1" Port="[Port1]" Protocol="[Protocol1] "/>
      </Endpoints>
    </ResourceOverrides>
        <Policies>
           <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint"/>
        </Policies>
  </ServiceManifestImport>
```

I hello parametrar lägga till nedan:

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

Vid distribution av hello programmet nu kan du skicka in dessa värden som ApplicationParameters till exempel:

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

Obs: Om hello värden tillhandahåller hello ApplicationParameters är tom vi gå tillbaka toohello standard värden som anges i hello ServiceManifest för hello motsvarande EndPointName.

Exempel:

Om i hello ServiceManifest som du har angett

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

Hej Port1 och Protocol1 värde för programmet parametrarna är null eller tomt. ServiceFabric beslutar fortfarande hello port. Och hello protokoll kommer tcp.

Anta att du anger ett felaktigt värde. Precis som för Port angav du ett strängvärde ”Foo” i stället för int.  Nya ServiceFabricApplication kommandot misslyckas med felmeddelandet: hello override-parametern med 'ServiceEndpoint1' namnattribut 'Port1 ”i avsnittet 'ResourceOverrides' är ogiltig. hello-värde som angetts är ”Foo” och krävs är 'int'.
