---
title: "Ange slutpunkter för Service Fabric | Microsoft Docs"
description: "Så här beskriver slutpunkt resurser i en tjänstmanifestet, inklusive hur du konfigurerar HTTPS-slutpunkter"
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
ms.openlocfilehash: 08141edfbc8be9bf7bf303419e1e482d5f884860
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="specify-resources-in-a-service-manifest"></a><span data-ttu-id="9c1fe-103">Ange resurser i ett tjänstmanifest</span><span class="sxs-lookup"><span data-stu-id="9c1fe-103">Specify resources in a service manifest</span></span>
## <a name="overview"></a><span data-ttu-id="9c1fe-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="9c1fe-104">Overview</span></span>
<span data-ttu-id="9c1fe-105">Service manifest kan resurser som används av tjänsten ska deklareras eller har ändrats utan att ändra den kompilerade koden.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-105">The service manifest allows resources that are used by the service to be declared/changed without changing the compiled code.</span></span> <span data-ttu-id="9c1fe-106">Azure Service Fabric stöder konfiguration av slutpunkten resurser för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-106">Azure Service Fabric supports configuration of endpoint resources for the service.</span></span> <span data-ttu-id="9c1fe-107">Åtkomst till resurser som har angetts i tjänstmanifestet kan kontrolleras via säkerhetsgrupp i programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-107">The access to the resources that are specified in the service manifest can be controlled via the SecurityGroup in the application manifest.</span></span> <span data-ttu-id="9c1fe-108">Deklarationen av resurser kan dessa resurser som ska ändras vid tidpunkten för distribution, vilket innebär att tjänsten inte behöver införa en ny konfiguration mekanism.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-108">The declaration of resources allows these resources to be changed at deployment time, meaning the service doesn't need to introduce a new configuration mechanism.</span></span> <span data-ttu-id="9c1fe-109">Schemadefinitionen för filen ServiceManifest.xml installeras med Fabric-SDK och verktyg för *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-109">The schema definition for the ServiceManifest.xml file is installed with the Service Fabric SDK and tools to *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

## <a name="endpoints"></a><span data-ttu-id="9c1fe-110">Slutpunkter</span><span class="sxs-lookup"><span data-stu-id="9c1fe-110">Endpoints</span></span>
<span data-ttu-id="9c1fe-111">När en resurs för slutpunkten har definierats i service manifest tilldelar Service Fabric portar från portintervall reserverade programmet när en port inte är anges explicit.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-111">When an endpoint resource is defined in the service manifest, Service Fabric assigns ports from the reserved application port range when a port isn't specified explicitly.</span></span> <span data-ttu-id="9c1fe-112">Titta exempelvis på slutpunkten *ServiceEndpoint1* angetts i manifestet fragment som efter denna punkt.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-112">For example, look at the endpoint *ServiceEndpoint1* specified in the manifest snippet provided after this paragraph.</span></span> <span data-ttu-id="9c1fe-113">Tjänster kan dessutom också begära en specifik port i en resurs.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-113">Additionally, services can also request a specific port in a resource.</span></span> <span data-ttu-id="9c1fe-114">Tjänsten repliker som körs på olika noder kan tilldelas olika portnummer, medan repliker av en tjänst som körs på samma nod dela porten.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-114">Service replicas running on different cluster nodes can be assigned different port numbers, while replicas of a service running on the same node share the port.</span></span> <span data-ttu-id="9c1fe-115">Tjänsten replikerna kan sedan använda dessa portar som behövs för replikering och lyssnar efter förfrågningar från klienter.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-115">The service replicas can then use these ports as needed for replication and listening for client requests.</span></span>

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="9c1fe-116">Referera till [konfigurera tillståndskänsliga Reliable Services](service-fabric-reliable-services-configuration.md) att läsa mer om refererar till slutpunkter från paketinställningar config-fil (settings.xml).</span><span class="sxs-lookup"><span data-stu-id="9c1fe-116">Refer to [Configuring stateful Reliable Services](service-fabric-reliable-services-configuration.md) to read more about referencing endpoints from the config package settings file (settings.xml).</span></span>

## <a name="example-specifying-an-http-endpoint-for-your-service"></a><span data-ttu-id="9c1fe-117">Exempel: Om du anger en HTTP-slutpunkt för tjänsten</span><span class="sxs-lookup"><span data-stu-id="9c1fe-117">Example: specifying an HTTP endpoint for your service</span></span>
<span data-ttu-id="9c1fe-118">Följande tjänstmanifestet definierar en resurs för TCP-slutpunkt, och två http-slutpunkt resurser i den &lt;resurser&gt; element.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-118">The following service manifest defines one TCP endpoint resource and two HTTP endpoint resources in the &lt;Resources&gt; element.</span></span>

<span data-ttu-id="9c1fe-119">HTTP-slutpunkter är automatiskt ACL skulle av Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-119">HTTP endpoints are automatically ACL'd by Service Fabric.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
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

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a><span data-ttu-id="9c1fe-120">Exempel: Om du anger en HTTPS-slutpunkt för tjänsten</span><span class="sxs-lookup"><span data-stu-id="9c1fe-120">Example: specifying an HTTPS endpoint for your service</span></span>
<span data-ttu-id="9c1fe-121">HTTPS-protokollet ger serverautentisering och används också för att kryptera kommunikationen mellan klient-server.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-121">The HTTPS protocol provides server authentication and is also used for encrypting client-server communication.</span></span> <span data-ttu-id="9c1fe-122">Om du vill aktivera HTTPS för Service Fabric-tjänsten, ange protokollet i den *resurser -> slutpunkter Endpoint ->* avsnitt i tjänstmanifestet enligt tidigare för slutpunkten *ServiceEndpoint3*.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-122">To enable HTTPS on your Service Fabric service, specify the protocol in the *Resources -> Endpoints -> Endpoint* section of the service manifest, as shown earlier for the endpoint *ServiceEndpoint3*.</span></span>

> [!NOTE]
> <span data-ttu-id="9c1fe-123">En tjänst-protokollet kan inte ändras under uppgradering av programmet.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-123">A service’s protocol cannot be changed during application upgrade.</span></span> <span data-ttu-id="9c1fe-124">Om den har ändrats under uppgraderingen, är en ny ändring.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-124">If it is changed during upgrade, it is a breaking change.</span></span>
> 
> 

<span data-ttu-id="9c1fe-125">Här är ett exempel ApplicationManifest som du måste ange för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-125">Here is an example ApplicationManifest that you need to set for HTTPS.</span></span> <span data-ttu-id="9c1fe-126">Tumavtrycket för certifikatet måste anges.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-126">The thumbprint for your certificate must be provided.</span></span> <span data-ttu-id="9c1fe-127">EndpointRef är en referens till EndpointResource i ServiceManifest, som du anger HTTPS-protokollet.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-127">The EndpointRef is a reference to EndpointResource in ServiceManifest, for which you set the HTTPS protocol.</span></span> <span data-ttu-id="9c1fe-128">Du kan lägga till fler än en EndpointCertificate.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-128">You can add more than one EndpointCertificate.</span></span>  

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
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
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

## <a name="overriding-endpoints-in-servicemanifestxml"></a><span data-ttu-id="9c1fe-129">Åsidosättning av slutpunkterna i ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="9c1fe-129">Overriding Endpoints in ServiceManifest.xml</span></span>

<span data-ttu-id="9c1fe-130">Lägg till ett ResourceOverrides avsnitt som kommer att vara på samma nivå som ConfigOverrides avsnitt i ApplicationManifest.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-130">In the ApplicationManifest add a ResourceOverrides section which will be a sibling to ConfigOverrides section.</span></span> <span data-ttu-id="9c1fe-131">I det här avsnittet kan du ange åsidosättningen för slutpunkter i avsnittet resurser som angetts i manifestet för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-131">In this section you can specify the override for the Endpoints section in the resources section specified in the Service manifest.</span></span>

<span data-ttu-id="9c1fe-132">För att åsidosätta slutpunkt i ServiceManifest med ApplicationParameters ändring i ApplicationManifest som följande:</span><span class="sxs-lookup"><span data-stu-id="9c1fe-132">In order to override EndPoint in ServiceManifest using ApplicationParameters change the ApplicationManifest as following:</span></span>

<span data-ttu-id="9c1fe-133">Lägg till ett nytt avsnitt ”ResourceOverrides” i avsnittet ServiceManifestImport</span><span class="sxs-lookup"><span data-stu-id="9c1fe-133">In the ServiceManifestImport section add a new section "ResourceOverrides"</span></span>

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

<span data-ttu-id="9c1fe-134">Lägg till nedan i parametrarna:</span><span class="sxs-lookup"><span data-stu-id="9c1fe-134">In the Parameters add below:</span></span>

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

<span data-ttu-id="9c1fe-135">Vid distribution av programmet nu kan du skicka in dessa värden som ApplicationParameters till exempel:</span><span class="sxs-lookup"><span data-stu-id="9c1fe-135">While deploying the application now you can pass in these values as ApplicationParameters for example:</span></span>

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

<span data-ttu-id="9c1fe-136">Obs: Om värdena tillhandahåller ApplicationParameters är tom vi gå tillbaka till standardvärdet som angetts i ServiceManifest för motsvarande EndPointName.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-136">Note: If the values provide for the ApplicationParameters is empty we go back to the default value provided in the ServiceManifest for the corresponding EndPointName.</span></span>

<span data-ttu-id="9c1fe-137">Exempel:</span><span class="sxs-lookup"><span data-stu-id="9c1fe-137">For example:</span></span>

<span data-ttu-id="9c1fe-138">Om i ServiceManifest som du har angett</span><span class="sxs-lookup"><span data-stu-id="9c1fe-138">If in the ServiceManifest you specified</span></span>

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

<span data-ttu-id="9c1fe-139">Port1 och Protocol1 värde för programmet är null eller tomt.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-139">And the Port1 and Protocol1 value for Application parameters is null or empty.</span></span> <span data-ttu-id="9c1fe-140">Porten är fortfarande besluta ServiceFabric.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-140">The port is still decided by ServiceFabric.</span></span> <span data-ttu-id="9c1fe-141">Och protokoll kommer tcp.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-141">And the Protocol will tcp.</span></span>

<span data-ttu-id="9c1fe-142">Anta att du anger ett felaktigt värde.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-142">Suppose you specify a wrong value.</span></span> <span data-ttu-id="9c1fe-143">Precis som för Port angav du ett strängvärde ”Foo” i stället för int.  Nya ServiceFabricApplication kommandot misslyckas med felmeddelandet: override-parametern med namnet 'ServiceEndpoint1' attribut 'Port1 ”i avsnittet 'ResourceOverrides' är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-143">Like for Port you specified a string value "Foo" instead of an int.  New-ServiceFabricApplication command will fail with an error : The override parameter with name 'ServiceEndpoint1' attribute 'Port1' in section 'ResourceOverrides' is invalid.</span></span> <span data-ttu-id="9c1fe-144">Det angivna värdet är ”Foo” och krävs är 'int'.</span><span class="sxs-lookup"><span data-stu-id="9c1fe-144">The value specified is 'Foo' and required is 'int'.</span></span>
