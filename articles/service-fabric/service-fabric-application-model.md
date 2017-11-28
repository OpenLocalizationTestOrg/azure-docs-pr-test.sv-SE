---
title: Azure Service Fabric-programmodell | Microsoft Docs
description: "Så här modell och beskriver program och tjänster i Service Fabric."
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
ms.openlocfilehash: e30482427b88eb3e58d39075c7f0734664b79aa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="model-an-application-in-service-fabric"></a><span data-ttu-id="ee3c2-103">Modellen är ett program i Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ee3c2-103">Model an application in Service Fabric</span></span>
<span data-ttu-id="ee3c2-104">Den här artikeln innehåller en översikt över Azure Service Fabric-programmodell och hur du definierar ett program och tjänsten via manifest-filer.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-104">This article provides an overview of the Azure Service Fabric application model and how to define an application and service via manifest files.</span></span>

## <a name="understand-the-application-model"></a><span data-ttu-id="ee3c2-105">Förstå programmet modellen</span><span class="sxs-lookup"><span data-stu-id="ee3c2-105">Understand the application model</span></span>
<span data-ttu-id="ee3c2-106">Ett program är en samling de tjänster som utför en viss funktion eller funktioner.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-106">An application is a collection of constituent services that perform a certain function or functions.</span></span> <span data-ttu-id="ee3c2-107">En utför en fullständig och fristående funktion och kan starta och köra oberoende av andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-107">A service performs a complete and standalone function and can start and run independently of other services.</span></span>  <span data-ttu-id="ee3c2-108">En tjänst består av koden, konfiguration och data.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-108">A service is composed of code, configuration, and data.</span></span> <span data-ttu-id="ee3c2-109">Koden består av körbara binärfilerna för varje tjänst, konfigurationen består av service-inställningar som kan läsas in vid körning och data består av godtycklig statiska data som ska konsumeras av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-109">For each service, code consists of the executable binaries, configuration consists of service settings that can be loaded at run time, and data consists of arbitrary static data to be consumed by the service.</span></span> <span data-ttu-id="ee3c2-110">Varje komponent i den här modellen hierarkiska programmet kan vara en ny version och uppgraderade oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-110">Each component in this hierarchical application model can be versioned and upgraded independently.</span></span>

![Programmodell Service Fabric][appmodel-diagram]

<span data-ttu-id="ee3c2-112">En typ av program består av ett paket av tjänsttyper är en kategorisering av ett program.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-112">An application type is a categorization of an application and consists of a bundle of service types.</span></span> <span data-ttu-id="ee3c2-113">En service är en kategorisering av en tjänst.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-113">A service type is a categorization of a service.</span></span> <span data-ttu-id="ee3c2-114">Kategorisering kan ha olika inställningar och konfigurationer, men huvudfunktionerna är densamma.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-114">The categorization can have different settings and configurations, but the core functionality remains the same.</span></span> <span data-ttu-id="ee3c2-115">Instanser av en tjänst finns olika service configuration varianter av samma service-typen.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-115">The instances of a service are the different service configuration variations of the same service type.</span></span>  

<span data-ttu-id="ee3c2-116">Klasser (eller ”typer”) för program och tjänster beskrivs via XML-filer (applikationsmanifest och service manifest).</span><span class="sxs-lookup"><span data-stu-id="ee3c2-116">Classes (or "types") of applications and services are described through XML files (application manifests and service manifests).</span></span>  <span data-ttu-id="ee3c2-117">Manifesten är de mallar som program kan initieras från avbildningsarkivet i klustret.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-117">The manifests are the templates against which applications can be instantiated from the cluster's image store.</span></span> <span data-ttu-id="ee3c2-118">Schemadefinitionen för filen ServiceManifest.xml och ApplicationManifest.xml har installerats med Service Fabric-SDK och verktyg för *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-118">The schema definition for the ServiceManifest.xml and ApplicationManifest.xml file is installed with the Service Fabric SDK and tools to *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

<span data-ttu-id="ee3c2-119">Koden för olika programinstanser köra som separata processer även om de finns på samma Service Fabric-noden.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-119">The code for different application instances run as separate processes even when hosted by the same Service Fabric node.</span></span> <span data-ttu-id="ee3c2-120">Dessutom livscykeln för varje instans av programmet kan hanteras (till exempel uppgraderas) oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-120">Furthermore, the lifecycle of each application instance can be managed (for example, upgraded) independently.</span></span> <span data-ttu-id="ee3c2-121">Följande diagram visar hur programtyper består av tjänsttyper, som i sin tur består av koden, konfiguration och data paket.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-121">The following diagram shows how application types are composed of service types, which in turn are composed of code, configuration, and data packages.</span></span> <span data-ttu-id="ee3c2-122">För att förenkla diagrammet bara kod/config/data paket för `ServiceType4` visas, även om varje typ av tjänst skulle lägga till några eller alla pakettyper.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-122">To simplify the diagram, only the code/config/data packages for `ServiceType4` are shown, though each service type would include some or all those package types.</span></span>

![Service Fabric programtyper och typer av tjänster][cluster-imagestore-apptypes]

<span data-ttu-id="ee3c2-124">Två olika manifest-filer används för att beskriva program och tjänster: tjänstmanifestet och programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-124">Two different manifest files are used to describe applications and services: the service manifest and application manifest.</span></span> <span data-ttu-id="ee3c2-125">Manifest beskrivs i detalj i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-125">Manifests are covered in detail in the following sections.</span></span>

<span data-ttu-id="ee3c2-126">Det kan finnas en eller flera instanser av en typ av aktiva i klustret.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-126">There can be one or more instances of a service type active in the cluster.</span></span> <span data-ttu-id="ee3c2-127">Till exempel uppnå tillståndskänslig tjänstinstanser eller repliker hög tillförlitlighet genom att replikera tillstånd mellan repliker som finns på olika noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-127">For example, stateful service instances, or replicas, achieve high reliability by replicating state between replicas located on different nodes in the cluster.</span></span> <span data-ttu-id="ee3c2-128">Replikering i stort sett ger redundans för tjänsten ska vara tillgängliga även om en nod i ett kluster misslyckas.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-128">Replication essentially provides redundancy for the service to be available even if one node in a cluster fails.</span></span> <span data-ttu-id="ee3c2-129">En [partitionerad service](service-fabric-concepts-partitioning.md) ytterligare dividerar dess tillstånd (och åtkomstmönster till det aktuella tillståndet) mellan noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-129">A [partitioned service](service-fabric-concepts-partitioning.md) further divides its state (and access patterns to that state) across nodes in the cluster.</span></span>

<span data-ttu-id="ee3c2-130">Följande diagram visar relationen mellan program och instanser av tjänsten, partitioner och repliker.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-130">The following diagram shows the relationship between applications and service instances, partitions, and replicas.</span></span>

![Partitioner och repliker i en tjänst][cluster-application-instances]

> [!TIP]
> <span data-ttu-id="ee3c2-132">Du kan visa layouten för program i ett kluster med verktyget Service Fabric-Utforskaren på http://&lt;yourclusteraddress&gt;: 19080/Explorer.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-132">You can view the layout of applications in a cluster using the Service Fabric Explorer tool available at http://&lt;yourclusteraddress&gt;:19080/Explorer.</span></span> <span data-ttu-id="ee3c2-133">Mer information finns i [visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="ee3c2-133">For more information, see [Visualizing your cluster with Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).</span></span>
> 
> 

## <a name="describe-a-service"></a><span data-ttu-id="ee3c2-134">Beskriv en tjänst</span><span class="sxs-lookup"><span data-stu-id="ee3c2-134">Describe a service</span></span>
<span data-ttu-id="ee3c2-135">Service manifest definieras deklarativt service-typen och versionen.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-135">The service manifest declaratively defines the service type and version.</span></span> <span data-ttu-id="ee3c2-136">Det anger tjänstmetadata som typ av tjänst, hälsa egenskaper, belastningsutjämning mått, binärfiler och konfigurationsfiler.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-136">It specifies service metadata such as service type, health properties, load-balancing metrics, service binaries, and configuration files.</span></span>  <span data-ttu-id="ee3c2-137">Placera ett annat sätt beskriver koden, konfiguration och data paket som ska utgöra ett servicepaket för att stödja en eller flera typer av tjänster.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-137">Put another way, it describes the code, configuration, and data packages that compose a service package to support one or more service types.</span></span> <span data-ttu-id="ee3c2-138">Här är ett enkelt exempel service manifest:</span><span class="sxs-lookup"><span data-stu-id="ee3c2-138">Here is a simple example service manifest:</span></span>

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

<span data-ttu-id="ee3c2-139">**Version** attribut är Ostrukturerade strängar och inte tolkas av systemet.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-139">**Version** attributes are unstructured strings and not parsed by the system.</span></span> <span data-ttu-id="ee3c2-140">Version-attribut kan användas för varje komponent för uppgradering till version.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-140">Version attributes are used to version each component for upgrades.</span></span>

<span data-ttu-id="ee3c2-141">**ServiceTypes** deklarerar vilka typer av tjänster som stöds av **CodePackages** i manifestet.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-141">**ServiceTypes** declares what service types are supported by **CodePackages** in this manifest.</span></span> <span data-ttu-id="ee3c2-142">När en tjänst instansieras mot en av dessa tjänsttyper, aktiveras alla kod paket som deklarerats i manifestet genom att köra deras startpunkter.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-142">When a service is instantiated against one of these service types, all code packages declared in this manifest are activated by running their entry points.</span></span> <span data-ttu-id="ee3c2-143">De resulterande processerna förväntas registrera stöds tjänsttyper vid körning.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-143">The resulting processes are expected to register the supported service types at run time.</span></span> <span data-ttu-id="ee3c2-144">Tjänsttyper deklarerats i manifestet nivå och inte koden package-nivå.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-144">Service types are declared at the manifest level and not the code package level.</span></span> <span data-ttu-id="ee3c2-145">Så när det finns flera paket i koden, aktiveras de alla varje gång systemet söker efter en deklarerad tjänsttyp.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-145">So when there are multiple code packages, they are all activated whenever the system looks for any one of the declared service types.</span></span>

<span data-ttu-id="ee3c2-146">**SetupEntryPoint** är en privilegierade startpunkt som körs med samma autentiseringsuppgifter som Service Fabric (vanligtvis den *LocalSystem* konto) innan andra startpunkt.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-146">**SetupEntryPoint** is a privileged entry point that runs with the same credentials as Service Fabric (typically the *LocalSystem* account) before any other entry point.</span></span> <span data-ttu-id="ee3c2-147">Den körbara filen som anges av **EntryPoint** är vanligtvis tjänstvärden tidskrävande.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-147">The executable specified by **EntryPoint** is typically the long-running service host.</span></span> <span data-ttu-id="ee3c2-148">Förekomsten av en separat installationsprogrammet startpunkt slipper du köra tjänstvärden med höga privilegier för längre tid.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-148">The presence of a separate setup entry point avoids having to run the service host with high privileges for extended periods of time.</span></span> <span data-ttu-id="ee3c2-149">Den körbara filen som anges av **EntryPoint** körs efter **SetupEntryPoint** avslutas korrekt.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-149">The executable specified by **EntryPoint** is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="ee3c2-150">Om processen någonsin avslutar eller kraschar, resulterande processen övervakas och startas om (med början igen **SetupEntryPoint**).</span><span class="sxs-lookup"><span data-stu-id="ee3c2-150">If the process ever terminates or crashes, the resulting process is monitored and restarted (beginning again with **SetupEntryPoint**).</span></span>  

<span data-ttu-id="ee3c2-151">Vanliga scenarier där **SetupEntryPoint** är när du kör en körbar fil innan du startar tjänsten eller du utföra en åtgärd med förhöjda privilegier.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-151">Typical scenarios for using **SetupEntryPoint** are when you run an executable before the service starts or you perform an operation with elevated privileges.</span></span> <span data-ttu-id="ee3c2-152">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ee3c2-152">For example:</span></span>

* <span data-ttu-id="ee3c2-153">Ställa in och initierar miljövariabler som behöver körbar fil.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-153">Setting up and initializing environment variables that the service executable needs.</span></span> <span data-ttu-id="ee3c2-154">Detta är inte begränsad till endast körbara filer skrivs via Service Fabric programmeringsmodeller.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-154">This is not limited to only executables written via the Service Fabric programming models.</span></span> <span data-ttu-id="ee3c2-155">Exempelvis måste npm.exe miljövariabler som konfigurerats för att distribuera ett node.js-program.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-155">For example, npm.exe needs some environment variables configured for deploying a node.js application.</span></span>
* <span data-ttu-id="ee3c2-156">Konfigurera åtkomstkontroll genom att installera security-certifikat.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-156">Setting up access control by installing security certificates.</span></span>

<span data-ttu-id="ee3c2-157">Mer information om hur du konfigurerar den **SetupEntryPoint** finns [konfigurera principen för en startpunkt för installationen av tjänsten](service-fabric-application-runas-security.md)</span><span class="sxs-lookup"><span data-stu-id="ee3c2-157">For more details on how to configure the **SetupEntryPoint** see [Configure the policy for a service setup entry point](service-fabric-application-runas-security.md)</span></span>

<span data-ttu-id="ee3c2-158">**EnvironmentVariables** innehåller en lista över miljövariabler som ställs in för den här koden paketet.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-158">**EnvironmentVariables** provides a list of environment variables that are set for this code package.</span></span> <span data-ttu-id="ee3c2-159">Environmentment variabler kan åsidosättas i den `ApplicationManifest.xml` att ange olika värden för olika tjänstinstanser.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-159">Environmentment variables can be overridden in the `ApplicationManifest.xml` to provide different values for different service instances.</span></span> 

<span data-ttu-id="ee3c2-160">**DataPackage** deklarerar en mapp med namnet med den **namn** attributet som innehåller godtyckliga statiska data som ska konsumeras av processen vid körning.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-160">**DataPackage** declares a folder, named by the **Name** attribute, that contains arbitrary static data to be consumed by the process at run time.</span></span>

<span data-ttu-id="ee3c2-161">**ConfigPackage** deklarerar en mapp med namnet med den **namn** attributet som innehåller en *Settings.xml* fil.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-161">**ConfigPackage** declares a folder, named by the **Name** attribute, that contains a *Settings.xml* file.</span></span> <span data-ttu-id="ee3c2-162">Filen innehåller avsnitt av inställningar för användardefinierade, nyckel / värde-par som processen läser tillbaka vid körning.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-162">The settings file contains sections of user-defined, key-value pair settings that the process reads back at run time.</span></span> <span data-ttu-id="ee3c2-163">Under en uppgradering, om det bara den **ConfigPackage** **version** har ändrats sedan processen inte startas om.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-163">During an upgrade, if only the **ConfigPackage** **version** has changed, then the running process is not restarted.</span></span> <span data-ttu-id="ee3c2-164">I stället meddelar ett återanrop processen som konfigurationsinställningarna har ändrats så att de kan läsas dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-164">Instead, a callback notifies the process that configuration settings have changed so they can be reloaded dynamically.</span></span> <span data-ttu-id="ee3c2-165">Här är ett exempel *Settings.xml* fil:</span><span class="sxs-lookup"><span data-stu-id="ee3c2-165">Here is an example *Settings.xml* file:</span></span>

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> <span data-ttu-id="ee3c2-166">Ett service manifest kan innehålla flera koden, konfiguration och data paket.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-166">A service manifest can contain multiple code, configuration, and data packages.</span></span> <span data-ttu-id="ee3c2-167">Var och en av dem kan vara en ny version oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-167">Each of those can be versioned independently.</span></span>
> 
> 

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a><span data-ttu-id="ee3c2-168">Beskriv ett program</span><span class="sxs-lookup"><span data-stu-id="ee3c2-168">Describe an application</span></span>
<span data-ttu-id="ee3c2-169">Applikationsmanifestet beskriver deklarativt programtypen och versionen.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-169">The application manifest declaratively describes the application type and version.</span></span> <span data-ttu-id="ee3c2-170">Den anger service sammansättning metadata, till exempel stabila namn, instans antal/replikering faktor, säkerhetsisolering/princip, placeringen, configuration åsidosättningar, och partitionsschemat ingående tjänsttyper.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-170">It specifies service composition metadata such as stable names, partitioning scheme, instance count/replication factor, security/isolation policy, placement constraints, configuration overrides, and constituent service types.</span></span> <span data-ttu-id="ee3c2-171">Belastningsutjämning domänerna som programmet ska placeras beskrivs också.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-171">The load-balancing domains into which the application is placed are also described.</span></span>

<span data-ttu-id="ee3c2-172">Därför ett programmanifest beskrivs element på programnivå och refererar till en eller flera service manifest för att skapa en typ av program.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-172">Thus, an application manifest describes elements at the application level and references one or more service manifests to compose an application type.</span></span> <span data-ttu-id="ee3c2-173">Här är ett enkelt exempel programmanifestet:</span><span class="sxs-lookup"><span data-stu-id="ee3c2-173">Here is a simple example application manifest:</span></span>

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

<span data-ttu-id="ee3c2-174">Som service manifest **Version** attribut är Ostrukturerade strängar och inte tolkas av systemet.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-174">Like service manifests, **Version** attributes are unstructured strings and are not parsed by the system.</span></span> <span data-ttu-id="ee3c2-175">Version-attribut kan också användas för varje komponent för uppgradering till version.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-175">Version attributes are also used to version each component for upgrades.</span></span>

<span data-ttu-id="ee3c2-176">**ServiceManifestImport** innehåller referenser till service manifest som ska utgöra den här tillämpningstypen.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-176">**ServiceManifestImport** contains references to service manifests that compose this application type.</span></span> <span data-ttu-id="ee3c2-177">Importerade service manifest avgör vilka typer av tjänster är giltiga i den här tillämpningstypen.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-177">Imported service manifests determine what service types are valid within this application type.</span></span> <span data-ttu-id="ee3c2-178">Inom ServiceManifestImport, kan du åsidosätta konfigurationsvärden i Settings.xml och miljövariabler i ServiceManifest.xml filer.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-178">Within the ServiceManifestImport, you override configuration values in Settings.xml and environment variables in ServiceManifest.xml files.</span></span> 


<span data-ttu-id="ee3c2-179">**DefaultServices** deklarerar tjänstinstanser som skapas automatiskt när ett program instansieras mot den här tillämpningstypen.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-179">**DefaultServices** declares service instances that are automatically created whenever an application is instantiated against this application type.</span></span> <span data-ttu-id="ee3c2-180">Standardtjänsterna bara i informationssyfte och fungerar som vanligt tjänster i varje avseende när de har skapats.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-180">Default services are just a convenience and behave like normal services in every respect after they have been created.</span></span> <span data-ttu-id="ee3c2-181">De uppgraderas tillsammans med andra tjänster i programinstansen och kan också tas bort.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-181">They are upgraded along with any other services in the application instance and can be removed as well.</span></span>

> [!NOTE]
> <span data-ttu-id="ee3c2-182">Ett programmanifest kan innehålla flera service manifest import och standardtjänster.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-182">An application manifest can contain multiple service manifest imports and default services.</span></span> <span data-ttu-id="ee3c2-183">Varje tjänst manifestimporten kan vara en ny version oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-183">Each service manifest import can be versioned independently.</span></span>
> 
> 

<span data-ttu-id="ee3c2-184">Information om hur du underhåller olika program och tjänstparametrar för enskilda miljöer finns [hantera programparametrar för miljöer med flera](service-fabric-manage-multiple-environment-app-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="ee3c2-184">To learn how to maintain different application and service parameters for individual environments, see [Managing application parameters for multiple environments](service-fabric-manage-multiple-environment-app-configuration.md).</span></span>

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a><span data-ttu-id="ee3c2-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee3c2-185">Next steps</span></span>
<span data-ttu-id="ee3c2-186">[Paketerar ett program](service-fabric-package-apps.md) och gör den redo att distribuera.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-186">[Package an application](service-fabric-package-apps.md) and make it ready to deploy.</span></span>

<span data-ttu-id="ee3c2-187">[Distribuera och ta bort program] [ 10] beskriver hur du använder PowerShell för att hantera programinstanser.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-187">[Deploy and remove applications][10] describes how to use PowerShell to manage application instances.</span></span>

<span data-ttu-id="ee3c2-188">[Hantera programparametrar för miljöer med flera] [ 11] beskriver hur du konfigurerar parametrar och miljövariabler för olika programinstanser.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-188">[Managing application parameters for multiple environments][11] describes how to configure parameters and environment variables for different application instances.</span></span>

<span data-ttu-id="ee3c2-189">[Konfigurera säkerhetsprinciper för ditt program] [ 12] beskriver hur du kör tjänster under säkerhetsprinciper för att begränsa åtkomsten.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-189">[Configure security policies for your application][12] describes how to run services under security policies to restrict access.</span></span>

<span data-ttu-id="ee3c2-190">[Program värd modeller] [ 13] beskriver relationen mellan repliker (eller instanser) av en distribuerad tjänst och värdprocessen för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ee3c2-190">[Application hosting models][13] describe relationship between replicas (or instances) of a deployed service and service-host process.</span></span>

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
