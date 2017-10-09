---
title: aaaAzure Service Fabric-programmodell | Microsoft Docs
description: "Hur toomodel och beskriver program och tjänster i Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: 54c4d026e7d556be5f697d4a6f2ee886687e1c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="model-an-application-in-service-fabric"></a><span data-ttu-id="f9e76-103">Modellen är ett program i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f9e76-103">Model an application in Service Fabric</span></span>
<span data-ttu-id="f9e76-104">Den här artikeln innehåller en översikt över hello Azure Service Fabric-programmodell och hur toodefine ett program och tjänsten via manifest filer.</span><span class="sxs-lookup"><span data-stu-id="f9e76-104">This article provides an overview of hello Azure Service Fabric application model and how toodefine an application and service via manifest files.</span></span>

## <a name="understand-hello-application-model"></a><span data-ttu-id="f9e76-105">Förstå hello programmodell</span><span class="sxs-lookup"><span data-stu-id="f9e76-105">Understand hello application model</span></span>
<span data-ttu-id="f9e76-106">Ett program är en samling de tjänster som utför en viss funktion eller funktioner.</span><span class="sxs-lookup"><span data-stu-id="f9e76-106">An application is a collection of constituent services that perform a certain function or functions.</span></span> <span data-ttu-id="f9e76-107">En utför en fullständig och fristående funktion och kan starta och köra oberoende av andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="f9e76-107">A service performs a complete and standalone function and can start and run independently of other services.</span></span>  <span data-ttu-id="f9e76-108">En tjänst består av koden, konfiguration och data.</span><span class="sxs-lookup"><span data-stu-id="f9e76-108">A service is composed of code, configuration, and data.</span></span> <span data-ttu-id="f9e76-109">Koden består av hello körbara binärfilerna för varje tjänst, konfigurationen består av service-inställningar som kan läsas in vid körning och data består av godtycklig statiska data toobe förbrukas av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f9e76-109">For each service, code consists of hello executable binaries, configuration consists of service settings that can be loaded at run time, and data consists of arbitrary static data toobe consumed by hello service.</span></span> <span data-ttu-id="f9e76-110">Varje komponent i den här modellen hierarkiska programmet kan vara en ny version och uppgraderade oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="f9e76-110">Each component in this hierarchical application model can be versioned and upgraded independently.</span></span>

![hello programmodell för Service Fabric][appmodel-diagram]

<span data-ttu-id="f9e76-112">En typ av program består av ett paket av tjänsttyper är en kategorisering av ett program.</span><span class="sxs-lookup"><span data-stu-id="f9e76-112">An application type is a categorization of an application and consists of a bundle of service types.</span></span> <span data-ttu-id="f9e76-113">En service är en kategorisering av en tjänst.</span><span class="sxs-lookup"><span data-stu-id="f9e76-113">A service type is a categorization of a service.</span></span> <span data-ttu-id="f9e76-114">hello kategorisering kan ha olika inställningar och konfigurationer, men hello core funktioner förblir hello samma.</span><span class="sxs-lookup"><span data-stu-id="f9e76-114">hello categorization can have different settings and configurations, but hello core functionality remains hello same.</span></span> <span data-ttu-id="f9e76-115">hello instanser av en tjänst är hello olika service configuration variationer av hello samma typen tjänst.</span><span class="sxs-lookup"><span data-stu-id="f9e76-115">hello instances of a service are hello different service configuration variations of hello same service type.</span></span>  

<span data-ttu-id="f9e76-116">Klasser (eller ”typer”) för program och tjänster beskrivs via XML-filer (applikationsmanifest och service manifest).</span><span class="sxs-lookup"><span data-stu-id="f9e76-116">Classes (or "types") of applications and services are described through XML files (application manifests and service manifests).</span></span>  <span data-ttu-id="f9e76-117">hello manifest är hello mallar som program kan initieras från avbildningsarkivet hello klustret.</span><span class="sxs-lookup"><span data-stu-id="f9e76-117">hello manifests are hello templates against which applications can be instantiated from hello cluster's image store.</span></span> <span data-ttu-id="f9e76-118">hello schemadefinition för hello ServiceManifest.xml och ApplicationManifest.xml installeras med hello Service Fabric-SDK och verktyg för*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="f9e76-118">hello schema definition for hello ServiceManifest.xml and ApplicationManifest.xml file is installed with hello Service Fabric SDK and tools too*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

<span data-ttu-id="f9e76-119">hello kod för olika programinstanser körs som separata processer även när hos hello samma Service Fabric-nod.</span><span class="sxs-lookup"><span data-stu-id="f9e76-119">hello code for different application instances run as separate processes even when hosted by hello same Service Fabric node.</span></span> <span data-ttu-id="f9e76-120">Dessutom hello livscykeln för varje instans av programmet kan hanteras (till exempel uppgraderas) oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="f9e76-120">Furthermore, hello lifecycle of each application instance can be managed (for example, upgraded) independently.</span></span> <span data-ttu-id="f9e76-121">hello följande diagram visar hur programtyper består av tjänsttyper, som i sin tur består av koden, konfiguration och data paket.</span><span class="sxs-lookup"><span data-stu-id="f9e76-121">hello following diagram shows how application types are composed of service types, which in turn are composed of code, configuration, and data packages.</span></span> <span data-ttu-id="f9e76-122">diagram över toosimplify hello, bara hello kod-config-data paket för `ServiceType4` visas, även om varje typ av tjänst skulle lägga till några eller alla pakettyper.</span><span class="sxs-lookup"><span data-stu-id="f9e76-122">toosimplify hello diagram, only hello code/config/data packages for `ServiceType4` are shown, though each service type would include some or all those package types.</span></span>

![Service Fabric programtyper och typer av tjänster][cluster-imagestore-apptypes]

<span data-ttu-id="f9e76-124">Två olika finns används toodescribe program och tjänster: hello tjänstmanifestet och programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="f9e76-124">Two different manifest files are used toodescribe applications and services: hello service manifest and application manifest.</span></span> <span data-ttu-id="f9e76-125">Manifest beskrivs i detalj i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="f9e76-125">Manifests are covered in detail in hello following sections.</span></span>

<span data-ttu-id="f9e76-126">Det kan finnas en eller flera instanser av en typ av aktiva i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="f9e76-126">There can be one or more instances of a service type active in hello cluster.</span></span> <span data-ttu-id="f9e76-127">Till exempel uppnå tillståndskänslig tjänstinstanser eller repliker hög tillförlitlighet genom att replikera tillstånd mellan repliker som finns på olika noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="f9e76-127">For example, stateful service instances, or replicas, achieve high reliability by replicating state between replicas located on different nodes in hello cluster.</span></span> <span data-ttu-id="f9e76-128">Replikering ger redundans för hello service toobe tillgängliga i stort sett även om en nod i ett kluster misslyckas.</span><span class="sxs-lookup"><span data-stu-id="f9e76-128">Replication essentially provides redundancy for hello service toobe available even if one node in a cluster fails.</span></span> <span data-ttu-id="f9e76-129">En [partitionerad service](service-fabric-concepts-partitioning.md) ytterligare dividerar dess tillstånd (och åtkomstläge mönster toothat) över noderna i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="f9e76-129">A [partitioned service](service-fabric-concepts-partitioning.md) further divides its state (and access patterns toothat state) across nodes in hello cluster.</span></span>

<span data-ttu-id="f9e76-130">hello visar följande diagram hello förhållandet mellan program och instanser av tjänsten, partitioner och repliker.</span><span class="sxs-lookup"><span data-stu-id="f9e76-130">hello following diagram shows hello relationship between applications and service instances, partitions, and replicas.</span></span>

![Partitioner och repliker i en tjänst][cluster-application-instances]

> [!TIP]
> <span data-ttu-id="f9e76-132">Du kan visa hello layouten för program i ett kluster med hello Service Fabric Explorer-verktyg som är tillgängliga på http://&lt;yourclusteraddress&gt;: 19080/Explorer.</span><span class="sxs-lookup"><span data-stu-id="f9e76-132">You can view hello layout of applications in a cluster using hello Service Fabric Explorer tool available at http://&lt;yourclusteraddress&gt;:19080/Explorer.</span></span> <span data-ttu-id="f9e76-133">Mer information finns i [visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="f9e76-133">For more information, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
> 
> 

## <a name="describe-a-service"></a><span data-ttu-id="f9e76-134">Beskriv en tjänst</span><span class="sxs-lookup"><span data-stu-id="f9e76-134">Describe a service</span></span>
<span data-ttu-id="f9e76-135">hello tjänstmanifestet definieras deklarativt hello service-typen och versionen.</span><span class="sxs-lookup"><span data-stu-id="f9e76-135">hello service manifest declaratively defines hello service type and version.</span></span> <span data-ttu-id="f9e76-136">Det anger tjänstmetadata som typ av tjänst, hälsa egenskaper, belastningsutjämning mått, binärfiler och konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="f9e76-136">It specifies service metadata such as service type, health properties, load-balancing metrics, service binaries, and configuration files.</span></span>  <span data-ttu-id="f9e76-137">Placera ett annat sätt beskriver hello koden, konfiguration och data paket som ska utgöra en service paketet toosupport en eller flera typer av tjänster.</span><span class="sxs-lookup"><span data-stu-id="f9e76-137">Put another way, it describes hello code, configuration, and data packages that compose a service package toosupport one or more service types.</span></span> <span data-ttu-id="f9e76-138">Här är ett enkelt exempel service manifest:</span><span class="sxs-lookup"><span data-stu-id="f9e76-138">Here is a simple example service manifest:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```

<span data-ttu-id="f9e76-139">**Version** attribut är Ostrukturerade strängar och inte tolkas av hello system.</span><span class="sxs-lookup"><span data-stu-id="f9e76-139">**Version** attributes are unstructured strings and not parsed by hello system.</span></span> <span data-ttu-id="f9e76-140">Version-attribut är används tooversion varje komponent för uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="f9e76-140">Version attributes are used tooversion each component for upgrades.</span></span>

<span data-ttu-id="f9e76-141">**ServiceTypes** deklarerar vilka typer av tjänster som stöds av **CodePackages** i manifestet.</span><span class="sxs-lookup"><span data-stu-id="f9e76-141">**ServiceTypes** declares what service types are supported by **CodePackages** in this manifest.</span></span> <span data-ttu-id="f9e76-142">När en tjänst instansieras mot en av dessa tjänsttyper, aktiveras alla kod paket som deklarerats i manifestet genom att köra deras startpunkter.</span><span class="sxs-lookup"><span data-stu-id="f9e76-142">When a service is instantiated against one of these service types, all code packages declared in this manifest are activated by running their entry points.</span></span> <span data-ttu-id="f9e76-143">hello resulterande processer är förväntade tooregister hello stöds tjänsttyper vid körning.</span><span class="sxs-lookup"><span data-stu-id="f9e76-143">hello resulting processes are expected tooregister hello supported service types at run time.</span></span> <span data-ttu-id="f9e76-144">Tjänsttyper deklareras på hello manifestet nivå och inte hello kod package-nivå.</span><span class="sxs-lookup"><span data-stu-id="f9e76-144">Service types are declared at hello manifest level and not hello code package level.</span></span> <span data-ttu-id="f9e76-145">Så när det finns flera paket i koden, är alla aktiverade när hello görs en sökning efter någon av hello deklarerats tjänsttyper.</span><span class="sxs-lookup"><span data-stu-id="f9e76-145">So when there are multiple code packages, they are all activated whenever hello system looks for any one of hello declared service types.</span></span>

<span data-ttu-id="f9e76-146">**SetupEntryPoint** är en privilegierade startpunkt som körs med samma autentiseringsuppgifter som Service Fabric hello (vanligtvis hello *LocalSystem* konto) innan andra startpunkt.</span><span class="sxs-lookup"><span data-stu-id="f9e76-146">**SetupEntryPoint** is a privileged entry point that runs with hello same credentials as Service Fabric (typically hello *LocalSystem* account) before any other entry point.</span></span> <span data-ttu-id="f9e76-147">hello körbara filen som anges av **EntryPoint** är vanligtvis hello tidskrävande tjänstvärden.</span><span class="sxs-lookup"><span data-stu-id="f9e76-147">hello executable specified by **EntryPoint** is typically hello long-running service host.</span></span> <span data-ttu-id="f9e76-148">hello förekomsten av en separat installationsprogrammet startpunkt slipper toorun hello tjänstvärden med höga privilegier för längre tid.</span><span class="sxs-lookup"><span data-stu-id="f9e76-148">hello presence of a separate setup entry point avoids having toorun hello service host with high privileges for extended periods of time.</span></span> <span data-ttu-id="f9e76-149">hello körbara filen som anges av **EntryPoint** körs efter **SetupEntryPoint** avslutas korrekt.</span><span class="sxs-lookup"><span data-stu-id="f9e76-149">hello executable specified by **EntryPoint** is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="f9e76-150">Om hello processen någonsin avslutar eller kraschar, hello resulterande processen övervakas och startas om (med början igen **SetupEntryPoint**).</span><span class="sxs-lookup"><span data-stu-id="f9e76-150">If hello process ever terminates or crashes, hello resulting process is monitored and restarted (beginning again with **SetupEntryPoint**).</span></span>  

<span data-ttu-id="f9e76-151">Vanliga scenarier där **SetupEntryPoint** är när du kör en körbar fil innan hello-tjänsten startar eller du utföra en åtgärd med förhöjda privilegier.</span><span class="sxs-lookup"><span data-stu-id="f9e76-151">Typical scenarios for using **SetupEntryPoint** are when you run an executable before hello service starts or you perform an operation with elevated privileges.</span></span> <span data-ttu-id="f9e76-152">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f9e76-152">For example:</span></span>

* <span data-ttu-id="f9e76-153">Ställa in och initierar miljövariabler som hello körbara behöver.</span><span class="sxs-lookup"><span data-stu-id="f9e76-153">Setting up and initializing environment variables that hello service executable needs.</span></span> <span data-ttu-id="f9e76-154">Detta är inte begränsad tooonly körbara filer skrivs via hello Service Fabric programmeringsmodeller.</span><span class="sxs-lookup"><span data-stu-id="f9e76-154">This is not limited tooonly executables written via hello Service Fabric programming models.</span></span> <span data-ttu-id="f9e76-155">Exempelvis måste npm.exe miljövariabler som konfigurerats för att distribuera ett node.js-program.</span><span class="sxs-lookup"><span data-stu-id="f9e76-155">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="f9e76-156">Konfigurera åtkomstkontroll genom att installera security-certifikat.</span><span class="sxs-lookup"><span data-stu-id="f9e76-156">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="f9e76-157">Mer information om hur tooconfigure hello **SetupEntryPoint** finns [konfigurera hello princip för en startpunkt för installationen av tjänsten](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="f9e76-157">For more details on how tooconfigure hello **SetupEntryPoint** see [Configure hello policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<span data-ttu-id="f9e76-158">**EnvironmentVariables** innehåller en lista över miljövariabler som ställs in för den här koden paketet.</span><span class="sxs-lookup"><span data-stu-id="f9e76-158">**EnvironmentVariables** provides a list of environment variables that are set for this code package.</span></span> <span data-ttu-id="f9e76-159">Environmentment variabler kan åsidosättas i hello `ApplicationManifest.xml` tooprovide olika värden för olika tjänstinstanser.</span><span class="sxs-lookup"><span data-stu-id="f9e76-159">Environmentment variables can be overridden in hello `ApplicationManifest.xml` tooprovide different values for different service instances.</span></span> 

<span data-ttu-id="f9e76-160">**DataPackage** deklarerar en mapp med namnet med hello **namn** attributet som innehåller godtyckliga statiska data toobe förbrukas av hello process under körning.</span><span class="sxs-lookup"><span data-stu-id="f9e76-160">**DataPackage** declares a folder, named by hello **Name** attribute, that contains arbitrary static data toobe consumed by hello process at run time.</span></span>

<span data-ttu-id="f9e76-161">**ConfigPackage** deklarerar en mapp med namnet med hello **namn** attributet som innehåller en *Settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="f9e76-161">**ConfigPackage** declares a folder, named by hello **Name** attribute, that contains a *Settings.xml* file.</span></span> <span data-ttu-id="f9e76-162">hello inställningsfilen innehåller avsnitt av inställningar för användardefinierade, nyckel / värde-par som hello processen läser tillbaka vid körning.</span><span class="sxs-lookup"><span data-stu-id="f9e76-162">hello settings file contains sections of user-defined, key-value pair settings that hello process reads back at run time.</span></span> <span data-ttu-id="f9e76-163">Under en uppgradering, om det bara hello **ConfigPackage** **version** har ändrats sedan hello körs processen inte startas om.</span><span class="sxs-lookup"><span data-stu-id="f9e76-163">During an upgrade, if only hello **ConfigPackage** **version** has changed, then hello running process is not restarted.</span></span> <span data-ttu-id="f9e76-164">I stället meddelar ett återanrop hello process som konfigurationsinställningarna har ändrats så att de kan läsas dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="f9e76-164">Instead, a callback notifies hello process that configuration settings have changed so they can be reloaded dynamically.</span></span> <span data-ttu-id="f9e76-165">Här är ett exempel *Settings.xml* fil:</span><span class="sxs-lookup"><span data-stu-id="f9e76-165">Here is an example *Settings.xml* file:</span></span>

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> <span data-ttu-id="f9e76-166">Ett service manifest kan innehålla flera koden, konfiguration och data paket.</span><span class="sxs-lookup"><span data-stu-id="f9e76-166">A service manifest can contain multiple code, configuration, and data packages.</span></span> <span data-ttu-id="f9e76-167">Var och en av dem kan vara en ny version oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="f9e76-167">Each of those can be versioned independently.</span></span>
> 
> 

<!--
For more information about other features supported by service manifests, refer toohello following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a><span data-ttu-id="f9e76-168">Beskriv ett program</span><span class="sxs-lookup"><span data-stu-id="f9e76-168">Describe an application</span></span>
<span data-ttu-id="f9e76-169">hello programmanifestet beskriver deklarativt hello programtypen och versionen.</span><span class="sxs-lookup"><span data-stu-id="f9e76-169">hello application manifest declaratively describes hello application type and version.</span></span> <span data-ttu-id="f9e76-170">Den anger service sammansättning metadata, till exempel stabila namn, instans antal/replikering faktor, säkerhetsisolering/princip, placeringen, configuration åsidosättningar, och partitionsschemat ingående tjänsttyper.</span><span class="sxs-lookup"><span data-stu-id="f9e76-170">It specifies service composition metadata such as stable names, partitioning scheme, instance count/replication factor, security/isolation policy, placement constraints, configuration overrides, and constituent service types.</span></span> <span data-ttu-id="f9e76-171">hello belastningsutjämning domäner där programmet hello placeras beskrivs också.</span><span class="sxs-lookup"><span data-stu-id="f9e76-171">hello load-balancing domains into which hello application is placed are also described.</span></span>

<span data-ttu-id="f9e76-172">Därför ett programmanifest beskrivs element på programnivå hello och refererar till ett eller flera service visar toocompose en typ av program.</span><span class="sxs-lookup"><span data-stu-id="f9e76-172">Thus, an application manifest describes elements at hello application level and references one or more service manifests toocompose an application type.</span></span> <span data-ttu-id="f9e76-173">Här är ett enkelt exempel programmanifestet:</span><span class="sxs-lookup"><span data-stu-id="f9e76-173">Here is a simple example application manifest:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

<span data-ttu-id="f9e76-174">Som service manifest **Version** attribut är Ostrukturerade strängar och inte tolkas av hello system.</span><span class="sxs-lookup"><span data-stu-id="f9e76-174">Like service manifests, **Version** attributes are unstructured strings and are not parsed by hello system.</span></span> <span data-ttu-id="f9e76-175">Version-attribut är också används tooversion varje komponent för uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="f9e76-175">Version attributes are also used tooversion each component for upgrades.</span></span>

<span data-ttu-id="f9e76-176">**ServiceManifestImport** innehåller referenser tooservice manifest som ska utgöra den här tillämpningstypen.</span><span class="sxs-lookup"><span data-stu-id="f9e76-176">**ServiceManifestImport** contains references tooservice manifests that compose this application type.</span></span> <span data-ttu-id="f9e76-177">Importerade service manifest avgör vilka typer av tjänster är giltiga i den här tillämpningstypen.</span><span class="sxs-lookup"><span data-stu-id="f9e76-177">Imported service manifests determine what service types are valid within this application type.</span></span> <span data-ttu-id="f9e76-178">Inom hello ServiceManifestImport, kan du åsidosätta konfigurationsvärden i Settings.xml och miljövariabler i ServiceManifest.xml filer.</span><span class="sxs-lookup"><span data-stu-id="f9e76-178">Within hello ServiceManifestImport, you override configuration values in Settings.xml and environment variables in ServiceManifest.xml files.</span></span> 


<span data-ttu-id="f9e76-179">**DefaultServices** deklarerar tjänstinstanser som skapas automatiskt när ett program instansieras mot den här tillämpningstypen.</span><span class="sxs-lookup"><span data-stu-id="f9e76-179">**DefaultServices** declares service instances that are automatically created whenever an application is instantiated against this application type.</span></span> <span data-ttu-id="f9e76-180">Standardtjänsterna bara i informationssyfte och fungerar som vanligt tjänster i varje avseende när de har skapats.</span><span class="sxs-lookup"><span data-stu-id="f9e76-180">Default services are just a convenience and behave like normal services in every respect after they have been created.</span></span> <span data-ttu-id="f9e76-181">De uppgraderas tillsammans med andra tjänster i hello programinstansen och kan också tas bort.</span><span class="sxs-lookup"><span data-stu-id="f9e76-181">They are upgraded along with any other services in hello application instance and can be removed as well.</span></span>

> [!NOTE]
> <span data-ttu-id="f9e76-182">Ett programmanifest kan innehålla flera service manifest import och standardtjänster.</span><span class="sxs-lookup"><span data-stu-id="f9e76-182">An application manifest can contain multiple service manifest imports and default services.</span></span> <span data-ttu-id="f9e76-183">Varje tjänst manifestimporten kan vara en ny version oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="f9e76-183">Each service manifest import can be versioned independently.</span></span>
> 
> 

<span data-ttu-id="f9e76-184">hur toomaintain olika program och tjänstparametrar för enskilda miljöer, se toolearn [hantera programparametrar för miljöer med flera](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="f9e76-184">toolearn how toomaintain different application and service parameters for individual environments, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

<!--
For more information about other features supported by application manifests, refer toohello following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a><span data-ttu-id="f9e76-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f9e76-185">Next steps</span></span>
<span data-ttu-id="f9e76-186">[Paketerar ett program](service-fabric-package-apps.md) och gör den redo toodeploy.</span><span class="sxs-lookup"><span data-stu-id="f9e76-186">[Package an application](service-fabric-package-apps.md) and make it ready toodeploy.</span></span>

<span data-ttu-id="f9e76-187">[Distribuera och ta bort program] [ 10] beskriver hur toouse PowerShell toomanage programinstanser.</span><span class="sxs-lookup"><span data-stu-id="f9e76-187">[Deploy and remove applications][10] describes how toouse PowerShell toomanage application instances.</span></span>

<span data-ttu-id="f9e76-188">[Hantera programparametrar för miljöer med flera] [ 11] beskriver hur tooconfigure parametrar och miljövariabler för olika programinstanser.</span><span class="sxs-lookup"><span data-stu-id="f9e76-188">[Managing application parameters for multiple environments][11] describes how tooconfigure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="f9e76-189">[Konfigurera säkerhetsprinciper för ditt program] [ 12] beskriver hur toorun services under principer toorestrict åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f9e76-189">[Configure security policies for your application][12] describes how toorun services under security policies toorestrict access.</span></span>

<span data-ttu-id="f9e76-190">[Program värd modeller] [ 13] beskriver relationen mellan repliker (eller instanser) av en distribuerad tjänst och värdprocessen för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f9e76-190">[Application hosting models][13] describe relationship between replicas (or instances) of a deployed service and service-host process.</span></span>

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
