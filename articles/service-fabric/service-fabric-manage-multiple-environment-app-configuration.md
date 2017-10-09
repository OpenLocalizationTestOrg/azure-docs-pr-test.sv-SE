---
title: "aaaManage flera miljöer i Service Fabric | Microsoft Docs"
description: "Service Fabric-program kan köras på kluster i intervallet från en dator toothousands datorer. I vissa fall vill du tooconfigure programmet på olika sätt för de olika miljöer. Den här artikeln beskriver hur toodefine parametrar för olika program per miljö."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: mikkelhegn
ms.openlocfilehash: 2b3327e0e1a3bbd35a50835e720619f308b1b501
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a><span data-ttu-id="f3df8-105">Hantera programparametrar för miljöer med flera</span><span class="sxs-lookup"><span data-stu-id="f3df8-105">Manage application parameters for multiple environments</span></span>
<span data-ttu-id="f3df8-106">Du kan skapa Azure Service Fabric-kluster genom att använda var som helst från en toomany tusentals datorer.</span><span class="sxs-lookup"><span data-stu-id="f3df8-106">You can create Azure Service Fabric clusters by using anywhere from one toomany thousands of machines.</span></span> <span data-ttu-id="f3df8-107">När programmet binärfiler kan köras utan ändringar i den här brett spektrum av miljöer, vill du ofta tooconfigure hello program på olika sätt, beroende på hello antalet datorer som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="f3df8-107">While application binaries can run without modification across this wide spectrum of environments, you often want tooconfigure hello application differently, depending on hello number of machines you're deploying to.</span></span>

<span data-ttu-id="f3df8-108">Ett enkelt exempel bör du `InstanceCount` för den tillståndslösa tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f3df8-108">As a simple example, consider `InstanceCount` for a stateless service.</span></span> <span data-ttu-id="f3df8-109">När du kör program i Azure, vanligtvis vill du tooset parametern toohello särskilda värdet ”1”.</span><span class="sxs-lookup"><span data-stu-id="f3df8-109">When you are running applications in Azure, you generally want tooset this parameter toohello special value of "-1".</span></span> <span data-ttu-id="f3df8-110">Den här konfigurationen garanterar att din tjänst körs på varje nod i hello kluster (eller alla noder i hello nodtypen om du har angett en begränsning för placering).</span><span class="sxs-lookup"><span data-stu-id="f3df8-110">This configuration ensures that your service is running on every node in hello cluster (or every node in hello node type if you have set a placement constraint).</span></span> <span data-ttu-id="f3df8-111">Den här konfigurationen är dock inte lämpligt för en enskild dator klustret eftersom du inte kan ha flera processer som lyssnar på hello samma slutpunkt på en enskild dator.</span><span class="sxs-lookup"><span data-stu-id="f3df8-111">However, this configuration is not suitable for a single-machine cluster since you cannot have multiple processes listening on hello same endpoint on a single machine.</span></span> <span data-ttu-id="f3df8-112">I stället kan du vanligtvis ange `InstanceCount` för ”1”.</span><span class="sxs-lookup"><span data-stu-id="f3df8-112">Instead, you typically set `InstanceCount` too"1".</span></span>

## <a name="specifying-environment-specific-parameters"></a><span data-ttu-id="f3df8-113">Ange miljöspecifikt parametrar</span><span class="sxs-lookup"><span data-stu-id="f3df8-113">Specifying environment-specific parameters</span></span>
<span data-ttu-id="f3df8-114">hello lösning toothis konfigurationsproblem är en uppsättning parametrar standardtjänster och parametern-programfilerna Fyll i de parametervärden för en given miljö.</span><span class="sxs-lookup"><span data-stu-id="f3df8-114">hello solution toothis configuration issue is a set of parameterized default services and application parameter files that fill in those parameter values for a given environment.</span></span> <span data-ttu-id="f3df8-115">Standardtjänster och programparametrar är konfigurerade i hello programmet och service manifest.</span><span class="sxs-lookup"><span data-stu-id="f3df8-115">Default services and application parameters are configured in hello application and service manifests.</span></span> <span data-ttu-id="f3df8-116">hello schemadefinition för hello ServiceManifest.xml och ApplicationManifest.xml filer installeras med hello Service Fabric-SDK och verktyg för*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span><span class="sxs-lookup"><span data-stu-id="f3df8-116">hello schema definition for hello ServiceManifest.xml and ApplicationManifest.xml files is installed with hello Service Fabric SDK and tools too*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.</span></span>

### <a name="default-services"></a><span data-ttu-id="f3df8-117">Standardtjänster</span><span class="sxs-lookup"><span data-stu-id="f3df8-117">Default services</span></span>
<span data-ttu-id="f3df8-118">Service Fabric-program består av en uppsättning instanser av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f3df8-118">Service Fabric applications are made up of a collection of service instances.</span></span> <span data-ttu-id="f3df8-119">När det är möjligt för toocreate ett tomt program och skapa alla tjänstinstanser dynamiskt har de flesta program en uppsättning kärntjänster som alltid ska skapas när programmet hello instansieras.</span><span class="sxs-lookup"><span data-stu-id="f3df8-119">While it is possible for you toocreate an empty application and then create all service instances dynamically, most applications have a set of core services that should always be created when hello application is instantiated.</span></span> <span data-ttu-id="f3df8-120">Dessa är refererad tooas ”standardtjänster”.</span><span class="sxs-lookup"><span data-stu-id="f3df8-120">These are referred tooas "default services".</span></span> <span data-ttu-id="f3df8-121">De har angetts i hello applikationsmanifestet med platshållare för per miljö configuration ingår i hakparenteser:</span><span class="sxs-lookup"><span data-stu-id="f3df8-121">They are specified in hello application manifest, with placeholders for per-environment configuration included in square brackets:</span></span>

```xml
  <DefaultServices>
      <Service Name="Stateful1">
          <StatefulService
              ServiceTypeName="Stateful1Type"
              TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
              MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

              <UniformInt64Partition
                  PartitionCount="[Stateful1_PartitionCount]"
                  LowKey="-9223372036854775808"
                  HighKey="9223372036854775807"
              />
        </StatefulService>
    </Service>
  </DefaultServices>
```

<span data-ttu-id="f3df8-122">Var och en av hello namngivna parametrar måste definieras inom hello parametrar element i programmanifestet hello:</span><span class="sxs-lookup"><span data-stu-id="f3df8-122">Each of hello named parameters must be defined within hello Parameters element of hello application manifest:</span></span>

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

<span data-ttu-id="f3df8-123">hello DefaultValue-attributet anger hello värdet toobe används i hello frånvaron av en mer specifik parameter för en given miljö.</span><span class="sxs-lookup"><span data-stu-id="f3df8-123">hello DefaultValue attribute specifies hello value toobe used in hello absence of a more-specific parameter for a given environment.</span></span>

> [!NOTE]
> <span data-ttu-id="f3df8-124">Alla service-instansparametrar är inte lämplig för konfigurationen av per miljö.</span><span class="sxs-lookup"><span data-stu-id="f3df8-124">Not all service instance parameters are suitable for per-environment configuration.</span></span> <span data-ttu-id="f3df8-125">Hej LowKey i hello-exemplet ovan, och HighKey värden för hello service partitioneringsschema definieras uttryckligen för alla instanser av tjänsten hello eftersom hello partitionsintervall är en funktion av hello data domän, inte hello miljö.</span><span class="sxs-lookup"><span data-stu-id="f3df8-125">In hello example above, hello LowKey and HighKey values for hello service's partitioning scheme are explicitly defined for all instances of hello service since hello partition range is a function of hello data domain, not hello environment.</span></span>
> 
> 

### <a name="per-environment-service-configuration-settings"></a><span data-ttu-id="f3df8-126">Konfigurationsinställningar för per miljö</span><span class="sxs-lookup"><span data-stu-id="f3df8-126">Per-environment service configuration settings</span></span>
<span data-ttu-id="f3df8-127">Hej [Service Fabric programmodell](service-fabric-application-model.md) aktiverar services tooinclude configuration paket som innehåller anpassade nyckel-värdepar som är läsbart vid körning.</span><span class="sxs-lookup"><span data-stu-id="f3df8-127">hello [Service Fabric application model](service-fabric-application-model.md) enables services tooinclude configuration packages that contain custom key-value pairs that are readable at run time.</span></span> <span data-ttu-id="f3df8-128">hello värden för dessa inställningar kan också skiljas åt av miljö genom att ange en `ConfigOverride` i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="f3df8-128">hello values of these settings can also be differentiated by environment by specifying a `ConfigOverride` in hello application manifest.</span></span>

<span data-ttu-id="f3df8-129">Anta att du har följande inställning i hello Config\Settings.xml-filen för hello hello `Stateful1` tjänsten:</span><span class="sxs-lookup"><span data-stu-id="f3df8-129">Suppose that you have hello following setting in hello Config\Settings.xml file for hello `Stateful1` service:</span></span>

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
<span data-ttu-id="f3df8-130">toooverride värdet för en specifik Programmiljö-par, skapa en `ConfigOverride` när du importerar hello tjänstmanifestet i hello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="f3df8-130">toooverride this value for a specific application/environment pair, create a `ConfigOverride` when you import hello service manifest in hello application manifest.</span></span>

```xml
  <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>
```
<span data-ttu-id="f3df8-131">Den här parametern kan sedan konfigureras av miljö som ovan.</span><span class="sxs-lookup"><span data-stu-id="f3df8-131">This parameter can then be configured by environment as shown above.</span></span> <span data-ttu-id="f3df8-132">Du kan göra detta genom att deklarera under hello parametrar i programmanifestet hello och ange miljö-specifika värden i hello parametern programfiler.</span><span class="sxs-lookup"><span data-stu-id="f3df8-132">You can do this by declaring it in hello parameters section of hello application manifest and specifying environment-specific values in hello application parameter files.</span></span>

> [!NOTE]
> <span data-ttu-id="f3df8-133">Hello fall av konfigurationsinställningar, finns det tre platser där du kan ange hello-värdet för en nyckel: hello konfigurationspaket för tjänsten och hello programmanifestet hello parameterfil för programmet.</span><span class="sxs-lookup"><span data-stu-id="f3df8-133">In hello case of service configuration settings, there are three places where hello value of a key can be set: hello service configuration package, hello application manifest, and hello application parameter file.</span></span> <span data-ttu-id="f3df8-134">Service Fabric kommer alltid välja från hello programmet parameterfilen först (om det angetts), sedan hello programmanifestet och slutligen hello konfigurationspaket.</span><span class="sxs-lookup"><span data-stu-id="f3df8-134">Service Fabric will always choose from hello application parameter file first (if specified), then hello application manifest, and finally hello configuration package.</span></span>
> 
> 

### <a name="setting-and-using-environment-variables"></a><span data-ttu-id="f3df8-135">Ställa in och använda miljövariabler</span><span class="sxs-lookup"><span data-stu-id="f3df8-135">Setting and using environment variables</span></span> 
<span data-ttu-id="f3df8-136">Du kan ange och ange miljövariabler i hello ServiceManifest.xml filen och åsidosätta dessa i hello ApplicationManifest.xml fil på grundval av per instans.</span><span class="sxs-lookup"><span data-stu-id="f3df8-136">You can specify and set environment variables in hello ServiceManifest.xml file and then override these in hello ApplicationManifest.xml file on a per instance basis.</span></span>
<span data-ttu-id="f3df8-137">hello exemplet nedan visar två miljövariabler, ange en med ett värde och hello andra åsidosätts.</span><span class="sxs-lookup"><span data-stu-id="f3df8-137">hello example below shows two environment variables, one with a value set and hello other is overridden.</span></span> <span data-ttu-id="f3df8-138">Du kan använda programmet parametrar tooset miljövariabler värdena i hello samma sätt som de har använts för config åsidosättningar.</span><span class="sxs-lookup"><span data-stu-id="f3df8-138">You can use application parameters tooset environment variables values in hello same way that these were used for config overrides.</span></span>

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
<span data-ttu-id="f3df8-139">toooverride hello miljövariabler i hello ApplicationManifest.xml referens hello kodpaketet i hello ServiceManifest med hello `EnvironmentOverrides` element.</span><span class="sxs-lookup"><span data-stu-id="f3df8-139">toooverride hello environment variables in hello ApplicationManifest.xml, reference hello code package in hello ServiceManifest with hello `EnvironmentOverrides` element.</span></span>

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 <span data-ttu-id="f3df8-140">Du kan komma åt hello miljövariabler från kod när hello namngivna tjänstinstansen har skapats.</span><span class="sxs-lookup"><span data-stu-id="f3df8-140">Once hello named service instance is created you can access hello environment variables from code.</span></span> <span data-ttu-id="f3df8-141">t.ex. I C# kan du göra följande hello</span><span class="sxs-lookup"><span data-stu-id="f3df8-141">e.g. In C# you can do hello following</span></span>

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a><span data-ttu-id="f3df8-142">Miljövariabler för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f3df8-142">Service Fabric environment variables</span></span>
<span data-ttu-id="f3df8-143">Service Fabric har inbyggda miljövariabler som anges för varje service-instans.</span><span class="sxs-lookup"><span data-stu-id="f3df8-143">Service Fabric has built in environment variables set for each service instance.</span></span> <span data-ttu-id="f3df8-144">hello fullständig lista över miljövariabler är nedan, där hello i fetstil är hello som du ska använda i din tjänst hello andra som används av Service Fabric runtime.</span><span class="sxs-lookup"><span data-stu-id="f3df8-144">hello full list of environment variables is below, where hello ones in bold are hello ones that you will use in your service, hello other being used by Service Fabric runtime.</span></span> 

* <span data-ttu-id="f3df8-145">Fabric_ApplicationHostId</span><span class="sxs-lookup"><span data-stu-id="f3df8-145">Fabric_ApplicationHostId</span></span>
* <span data-ttu-id="f3df8-146">Fabric_ApplicationHostType</span><span class="sxs-lookup"><span data-stu-id="f3df8-146">Fabric_ApplicationHostType</span></span>
* <span data-ttu-id="f3df8-147">Fabric_ApplicationId</span><span class="sxs-lookup"><span data-stu-id="f3df8-147">Fabric_ApplicationId</span></span>
* <span data-ttu-id="f3df8-148">**Fabric_ApplicationName**</span><span class="sxs-lookup"><span data-stu-id="f3df8-148">**Fabric_ApplicationName**</span></span>
* <span data-ttu-id="f3df8-149">Fabric_CodePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="f3df8-149">Fabric_CodePackageInstanceId</span></span>
* <span data-ttu-id="f3df8-150">**Fabric_CodePackageName**</span><span class="sxs-lookup"><span data-stu-id="f3df8-150">**Fabric_CodePackageName**</span></span>
* <span data-ttu-id="f3df8-151">**Fabric_Endpoint_ [YourServiceName] TypeEndpoint**</span><span class="sxs-lookup"><span data-stu-id="f3df8-151">**Fabric_Endpoint_[YourServiceName]TypeEndpoint**</span></span>
* <span data-ttu-id="f3df8-152">**Fabric_Folder_App_Log**</span><span class="sxs-lookup"><span data-stu-id="f3df8-152">**Fabric_Folder_App_Log**</span></span>
* <span data-ttu-id="f3df8-153">**Fabric_Folder_App_Temp**</span><span class="sxs-lookup"><span data-stu-id="f3df8-153">**Fabric_Folder_App_Temp**</span></span>
* <span data-ttu-id="f3df8-154">**Fabric_Folder_App_Work**</span><span class="sxs-lookup"><span data-stu-id="f3df8-154">**Fabric_Folder_App_Work**</span></span>
* <span data-ttu-id="f3df8-155">**Fabric_Folder_Application**</span><span class="sxs-lookup"><span data-stu-id="f3df8-155">**Fabric_Folder_Application**</span></span>
* <span data-ttu-id="f3df8-156">Fabric_NodeId</span><span class="sxs-lookup"><span data-stu-id="f3df8-156">Fabric_NodeId</span></span>
* <span data-ttu-id="f3df8-157">**Fabric_NodeIPOrFQDN**</span><span class="sxs-lookup"><span data-stu-id="f3df8-157">**Fabric_NodeIPOrFQDN**</span></span>
* <span data-ttu-id="f3df8-158">**Fabric_NodeName**</span><span class="sxs-lookup"><span data-stu-id="f3df8-158">**Fabric_NodeName**</span></span>
* <span data-ttu-id="f3df8-159">Fabric_RuntimeConnectionAddress</span><span class="sxs-lookup"><span data-stu-id="f3df8-159">Fabric_RuntimeConnectionAddress</span></span>
* <span data-ttu-id="f3df8-160">Fabric_ServicePackageInstanceId</span><span class="sxs-lookup"><span data-stu-id="f3df8-160">Fabric_ServicePackageInstanceId</span></span>
* <span data-ttu-id="f3df8-161">Fabric_ServicePackageName</span><span class="sxs-lookup"><span data-stu-id="f3df8-161">Fabric_ServicePackageName</span></span>
* <span data-ttu-id="f3df8-162">Fabric_ServicePackageVersionInstance</span><span class="sxs-lookup"><span data-stu-id="f3df8-162">Fabric_ServicePackageVersionInstance</span></span>
* <span data-ttu-id="f3df8-163">FabricPackageFileName</span><span class="sxs-lookup"><span data-stu-id="f3df8-163">FabricPackageFileName</span></span>

<span data-ttu-id="f3df8-164">hello kod belows visar hur toolist hello miljövariabler för Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f3df8-164">hello code belows shows how toolist hello Service Fabric environment variables</span></span>
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
<span data-ttu-id="f3df8-165">hello följande är exempel på miljövariabler för en typ av program som kallas `GuestExe.Application` med en typ av kallas `FrontEndService` när det körs på den lokala dev-datorn.</span><span class="sxs-lookup"><span data-stu-id="f3df8-165">hello following are examples of environment variables for an application type called `GuestExe.Application` with a service type called `FrontEndService` when run on your local dev machine.</span></span>

* <span data-ttu-id="f3df8-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span><span class="sxs-lookup"><span data-stu-id="f3df8-166">**Fabric_ApplicationName = fabric:/GuestExe.Application**</span></span>
* <span data-ttu-id="f3df8-167">**Fabric_CodePackageName = Code**</span><span class="sxs-lookup"><span data-stu-id="f3df8-167">**Fabric_CodePackageName = Code**</span></span>
* <span data-ttu-id="f3df8-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span><span class="sxs-lookup"><span data-stu-id="f3df8-168">**Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**</span></span>
* <span data-ttu-id="f3df8-169">**Fabric_NodeIPOrFQDN = localhost**</span><span class="sxs-lookup"><span data-stu-id="f3df8-169">**Fabric_NodeIPOrFQDN = localhost**</span></span>
* <span data-ttu-id="f3df8-170">**Fabric_NodeName = _Node_2**</span><span class="sxs-lookup"><span data-stu-id="f3df8-170">**Fabric_NodeName = _Node_2**</span></span>

### <a name="application-parameter-files"></a><span data-ttu-id="f3df8-171">Parametern programfiler</span><span class="sxs-lookup"><span data-stu-id="f3df8-171">Application parameter files</span></span>
<span data-ttu-id="f3df8-172">hello Service Fabric application-projekt kan innehålla en eller flera parametern programfiler.</span><span class="sxs-lookup"><span data-stu-id="f3df8-172">hello Service Fabric application project can include one or more application parameter files.</span></span> <span data-ttu-id="f3df8-173">Var och en av dem definierar hello specifika värden för hello-parametrar som definieras i programmanifestet hello:</span><span class="sxs-lookup"><span data-stu-id="f3df8-173">Each of them defines hello specific values for hello parameters that are defined in hello application manifest:</span></span>

```xml
    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="3" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>
```
<span data-ttu-id="f3df8-174">Som standard innehåller tre parametern programfiler, Local.1Node.xml, Local.5Node.xml och Cloud.xml i ett nytt program:</span><span class="sxs-lookup"><span data-stu-id="f3df8-174">By default, a new application includes three application parameter files, named Local.1Node.xml, Local.5Node.xml, and Cloud.xml:</span></span>

![Parametern programfiler i Solution Explorer][app-parameters-solution-explorer]

<span data-ttu-id="f3df8-176">toocreate en parameterfil, kopiera och klistra in en befintlig bara och ge det ett nytt namn.</span><span class="sxs-lookup"><span data-stu-id="f3df8-176">toocreate a parameter file, simply copy and paste an existing one and give it a new name.</span></span>

## <a name="identifying-environment-specific-parameters-during-deployment"></a><span data-ttu-id="f3df8-177">Identifiera miljö-specifika parametrar under distributionen</span><span class="sxs-lookup"><span data-stu-id="f3df8-177">Identifying environment-specific parameters during deployment</span></span>
<span data-ttu-id="f3df8-178">Vid tidpunkten för distribution måste toochoose hello lämpliga parametern filen tooapply med ditt program.</span><span class="sxs-lookup"><span data-stu-id="f3df8-178">At deployment time, you need toochoose hello appropriate parameter file tooapply with your application.</span></span> <span data-ttu-id="f3df8-179">Du kan göra detta via hello publicera dialogrutan i Visual Studio eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f3df8-179">You can do this through hello Publish dialog in Visual Studio or through PowerShell.</span></span>

### <a name="deploy-from-visual-studio"></a><span data-ttu-id="f3df8-180">Distribuera från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3df8-180">Deploy from Visual Studio</span></span>
<span data-ttu-id="f3df8-181">Du kan välja hello listan över tillgängliga parametern filer när du publicerar ditt program i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f3df8-181">You can choose from hello list of available parameter files when you publish your application in Visual Studio.</span></span>

![Välj en parameterfil i hello publicera dialogrutan][publishdialog]

### <a name="deploy-from-powershell"></a><span data-ttu-id="f3df8-183">Distribuera från PowerShell</span><span class="sxs-lookup"><span data-stu-id="f3df8-183">Deploy from PowerShell</span></span>
<span data-ttu-id="f3df8-184">Hej `Deploy-FabricApplication.ps1` PowerShell-skript som ingår i hello programmet projektmall accepterar en publiceringsprofil som en parameter och hello PublishProfile innehåller en referens toohello programmet parameterfil.</span><span class="sxs-lookup"><span data-stu-id="f3df8-184">hello `Deploy-FabricApplication.ps1` PowerShell script included in hello application project template accepts a publish profile as a parameter and hello PublishProfile contains a reference toohello application parameters file.</span></span>

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a><span data-ttu-id="f3df8-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f3df8-185">Next steps</span></span>
<span data-ttu-id="f3df8-186">toolearn mer om några av hello grundläggande begrepp som diskuteras i det här avsnittet finns hello [teknisk översikt för Service Fabric](service-fabric-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f3df8-186">toolearn more about some of hello core concepts that are discussed in this topic, see hello [Service Fabric technical overview](service-fabric-technical-overview.md).</span></span> <span data-ttu-id="f3df8-187">Information om andra app-hanteringsfunktioner som är tillgängliga i Visual Studio finns [hantera din Service Fabric-program i Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="f3df8-187">For information about other app management capabilities that are available in Visual Studio, see [Manage your Service Fabric applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md).</span></span>

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
