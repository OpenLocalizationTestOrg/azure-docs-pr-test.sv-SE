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
# <a name="specify-resources-in-a-service-manifest"></a><span data-ttu-id="1fea8-103">Ange resurser i ett tjänstmanifest</span><span class="sxs-lookup"><span data-stu-id="1fea8-103">Specify resources in a service manifest</span></span>
## <a name="overview"></a><span data-ttu-id="1fea8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1fea8-104">Overview</span></span>
<span data-ttu-id="1fea8-105">hello tjänstmanifestet kan resurser som används av hello service toobe deklarerats eller har ändrats utan att ändra hello kompilerad kod.</span><span class="sxs-lookup"><span data-stu-id="1fea8-105">hello service manifest allows resources that are used by hello service toobe declared/changed without changing hello compiled code.</span></span> <span data-ttu-id="1fea8-106">Azure Service Fabric stöder konfiguration av slutpunkten resurser för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1fea8-106">Azure Service Fabric supports configuration of endpoint resources for hello service.</span></span> <span data-ttu-id="1fea8-107">Hej toohello resurser som har angetts i hello tjänstmanifestet kan kontrolleras via hello säkerhetsgrupp i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="1fea8-107">hello access toohello resources that are specified in hello service manifest can be controlled via hello SecurityGroup in hello application manifest.</span></span> <span data-ttu-id="1fea8-108">hello deklarationen av resurser kan dessa resurser toobe ändras vid tidpunkten för distribution, vilket innebär att hello tjänsten inte behöver toointroduce en ny konfiguration mekanism.</span><span class="sxs-lookup"><span data-stu-id="1fea8-108">hello declaration of resources allows these resources toobe changed at deployment time, meaning hello service doesn't need toointroduce a new configuration mechanism.</span></span> <span data-ttu-id="1fea8-109">hello schemadefinition för hello ServiceManifest.xml fil har installerats med hello Service Fabric-SDK och verktyg för*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="1fea8-109">hello schema definition for hello ServiceManifest.xml file is installed with hello Service Fabric SDK and tools too*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

## <a name="endpoints"></a><span data-ttu-id="1fea8-110">Slutpunkter</span><span class="sxs-lookup"><span data-stu-id="1fea8-110">Endpoints</span></span>
<span data-ttu-id="1fea8-111">När en resurs för slutpunkten har definierats i hello tjänstmanifestet tilldelar Service Fabric portar från hello reserverade portintervall för programmet när en port inte är anges explicit.</span><span class="sxs-lookup"><span data-stu-id="1fea8-111">When an endpoint resource is defined in hello service manifest, Service Fabric assigns ports from hello reserved application port range when a port isn't specified explicitly.</span></span> <span data-ttu-id="1fea8-112">Titta exempelvis på hello endpoint *ServiceEndpoint1* anges i hello manifestet fragment som efter denna punkt.</span><span class="sxs-lookup"><span data-stu-id="1fea8-112">For example, look at hello endpoint *ServiceEndpoint1* specified in hello manifest snippet provided after this paragraph.</span></span> <span data-ttu-id="1fea8-113">Tjänster kan dessutom också begära en specifik port i en resurs.</span><span class="sxs-lookup"><span data-stu-id="1fea8-113">Additionally, services can also request a specific port in a resource.</span></span> <span data-ttu-id="1fea8-114">Tjänsten repliker som körs på olika noder kan tilldelas olika portnummer, medan repliker av en tjänst som körs på hello samma nod resursen hello port.</span><span class="sxs-lookup"><span data-stu-id="1fea8-114">Service replicas running on different cluster nodes can be assigned different port numbers, while replicas of a service running on hello same node share hello port.</span></span> <span data-ttu-id="1fea8-115">hello service repliker kan sedan använda dessa portar som behövs för replikering och lyssnar efter förfrågningar från klienter.</span><span class="sxs-lookup"><span data-stu-id="1fea8-115">hello service replicas can then use these ports as needed for replication and listening for client requests.</span></span>

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="1fea8-116">Se för[konfigurera tillståndskänsliga Reliable Services](service-fabric-reliable-services-configuration.md) tooread mer om refererar till slutpunkter från hello paketet inställningar konfigurationsfilen (settings.xml).</span><span class="sxs-lookup"><span data-stu-id="1fea8-116">Refer too[Configuring stateful Reliable Services](service-fabric-reliable-services-configuration.md) tooread more about referencing endpoints from hello config package settings file (settings.xml).</span></span>

## <a name="example-specifying-an-http-endpoint-for-your-service"></a><span data-ttu-id="1fea8-117">Exempel: Om du anger en HTTP-slutpunkt för tjänsten</span><span class="sxs-lookup"><span data-stu-id="1fea8-117">Example: specifying an HTTP endpoint for your service</span></span>
<span data-ttu-id="1fea8-118">hello följande tjänstmanifestet definierar en resurs för TCP-slutpunkt och två http-slutpunkt resurser i hello &lt;resurser&gt; element.</span><span class="sxs-lookup"><span data-stu-id="1fea8-118">hello following service manifest defines one TCP endpoint resource and two HTTP endpoint resources in hello &lt;Resources&gt; element.</span></span>

<span data-ttu-id="1fea8-119">HTTP-slutpunkter är automatiskt ACL skulle av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="1fea8-119">HTTP endpoints are automatically ACL'd by Service Fabric.</span></span>

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

## <a name="example-specifying-an-https-endpoint-for-your-service"></a><span data-ttu-id="1fea8-120">Exempel: Om du anger en HTTPS-slutpunkt för tjänsten</span><span class="sxs-lookup"><span data-stu-id="1fea8-120">Example: specifying an HTTPS endpoint for your service</span></span>
<span data-ttu-id="1fea8-121">hello HTTPS-protokollet ger serverautentisering och används också för att kryptera kommunikationen mellan klient-server.</span><span class="sxs-lookup"><span data-stu-id="1fea8-121">hello HTTPS protocol provides server authentication and is also used for encrypting client-server communication.</span></span> <span data-ttu-id="1fea8-122">tooenable HTTPS på din Service Fabric-tjänsten, ange hello-protokollet i hello *resurser -> slutpunkter Endpoint ->* avsnitt i hello tjänstmanifestet, enligt tidigare för hello slutpunkten *ServiceEndpoint3* .</span><span class="sxs-lookup"><span data-stu-id="1fea8-122">tooenable HTTPS on your Service Fabric service, specify hello protocol in hello *Resources -> Endpoints -> Endpoint* section of hello service manifest, as shown earlier for hello endpoint *ServiceEndpoint3*.</span></span>

> [!NOTE]
> <span data-ttu-id="1fea8-123">En tjänst-protokollet kan inte ändras under uppgradering av programmet.</span><span class="sxs-lookup"><span data-stu-id="1fea8-123">A service’s protocol cannot be changed during application upgrade.</span></span> <span data-ttu-id="1fea8-124">Om den har ändrats under uppgraderingen, är en ny ändring.</span><span class="sxs-lookup"><span data-stu-id="1fea8-124">If it is changed during upgrade, it is a breaking change.</span></span>
> 
> 

<span data-ttu-id="1fea8-125">Här är ett exempel ApplicationManifest måste tooset för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1fea8-125">Here is an example ApplicationManifest that you need tooset for HTTPS.</span></span> <span data-ttu-id="1fea8-126">hello tumavtrycket för certifikatet måste anges.</span><span class="sxs-lookup"><span data-stu-id="1fea8-126">hello thumbprint for your certificate must be provided.</span></span> <span data-ttu-id="1fea8-127">Hej EndpointRef är en referens tooEndpointResource i ServiceManifest, som du angett hello HTTPS-protokollet.</span><span class="sxs-lookup"><span data-stu-id="1fea8-127">hello EndpointRef is a reference tooEndpointResource in ServiceManifest, for which you set hello HTTPS protocol.</span></span> <span data-ttu-id="1fea8-128">Du kan lägga till fler än en EndpointCertificate.</span><span class="sxs-lookup"><span data-stu-id="1fea8-128">You can add more than one EndpointCertificate.</span></span>  

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

## <a name="overriding-endpoints-in-servicemanifestxml"></a><span data-ttu-id="1fea8-129">Åsidosättning av slutpunkterna i ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="1fea8-129">Overriding Endpoints in ServiceManifest.xml</span></span>

<span data-ttu-id="1fea8-130">Lägg till ett ResourceOverrides avsnitt som är ett objekt på samma nivå tooConfigOverrides avsnitt i hello ApplicationManifest.</span><span class="sxs-lookup"><span data-stu-id="1fea8-130">In hello ApplicationManifest add a ResourceOverrides section which will be a sibling tooConfigOverrides section.</span></span> <span data-ttu-id="1fea8-131">Du kan ange hello åsidosättande för hello slutpunkter i hello resurser avsnittet anges i hello tjänstmanifestet i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1fea8-131">In this section you can specify hello override for hello Endpoints section in hello resources section specified in hello Service manifest.</span></span>

<span data-ttu-id="1fea8-132">I ordning toooverride slutpunkt i ServiceManifest med ApplicationParameters ändra hello ApplicationManifest enligt följande:</span><span class="sxs-lookup"><span data-stu-id="1fea8-132">In order toooverride EndPoint in ServiceManifest using ApplicationParameters change hello ApplicationManifest as following:</span></span>

<span data-ttu-id="1fea8-133">Lägga till ett nytt avsnitt ”ResourceOverrides” Hej ServiceManifestImport avsnitt</span><span class="sxs-lookup"><span data-stu-id="1fea8-133">In hello ServiceManifestImport section add a new section "ResourceOverrides"</span></span>

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

<span data-ttu-id="1fea8-134">I hello parametrar lägga till nedan:</span><span class="sxs-lookup"><span data-stu-id="1fea8-134">In hello Parameters add below:</span></span>

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

<span data-ttu-id="1fea8-135">Vid distribution av hello programmet nu kan du skicka in dessa värden som ApplicationParameters till exempel:</span><span class="sxs-lookup"><span data-stu-id="1fea8-135">While deploying hello application now you can pass in these values as ApplicationParameters for example:</span></span>

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

<span data-ttu-id="1fea8-136">Obs: Om hello värden tillhandahåller hello ApplicationParameters är tom vi gå tillbaka toohello standard värden som anges i hello ServiceManifest för hello motsvarande EndPointName.</span><span class="sxs-lookup"><span data-stu-id="1fea8-136">Note: If hello values provide for hello ApplicationParameters is empty we go back toohello default value provided in hello ServiceManifest for hello corresponding EndPointName.</span></span>

<span data-ttu-id="1fea8-137">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1fea8-137">For example:</span></span>

<span data-ttu-id="1fea8-138">Om i hello ServiceManifest som du har angett</span><span class="sxs-lookup"><span data-stu-id="1fea8-138">If in hello ServiceManifest you specified</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

<span data-ttu-id="1fea8-139">Hej Port1 och Protocol1 värde för programmet parametrarna är null eller tomt.</span><span class="sxs-lookup"><span data-stu-id="1fea8-139">And hello Port1 and Protocol1 value for Application parameters is null or empty.</span></span> <span data-ttu-id="1fea8-140">ServiceFabric beslutar fortfarande hello port.</span><span class="sxs-lookup"><span data-stu-id="1fea8-140">hello port is still decided by ServiceFabric.</span></span> <span data-ttu-id="1fea8-141">Och hello protokoll kommer tcp.</span><span class="sxs-lookup"><span data-stu-id="1fea8-141">And hello Protocol will tcp.</span></span>

<span data-ttu-id="1fea8-142">Anta att du anger ett felaktigt värde.</span><span class="sxs-lookup"><span data-stu-id="1fea8-142">Suppose you specify a wrong value.</span></span> <span data-ttu-id="1fea8-143">Precis som för Port angav du ett strängvärde ”Foo” i stället för int.  Nya ServiceFabricApplication kommandot misslyckas med felmeddelandet: hello override-parametern med 'ServiceEndpoint1' namnattribut 'Port1 ”i avsnittet 'ResourceOverrides' är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="1fea8-143">Like for Port you specified a string value "Foo" instead of an int.  New-ServiceFabricApplication command will fail with an error : hello override parameter with name 'ServiceEndpoint1' attribute 'Port1' in section 'ResourceOverrides' is invalid.</span></span> <span data-ttu-id="1fea8-144">hello-värde som angetts är ”Foo” och krävs är 'int'.</span><span class="sxs-lookup"><span data-stu-id="1fea8-144">hello value specified is 'Foo' and required is 'int'.</span></span>
