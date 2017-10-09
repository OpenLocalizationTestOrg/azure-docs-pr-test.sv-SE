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
# <a name="manage-application-parameters-for-multiple-environments"></a>Hantera programparametrar för miljöer med flera
Du kan skapa Azure Service Fabric-kluster genom att använda var som helst från en toomany tusentals datorer. När programmet binärfiler kan köras utan ändringar i den här brett spektrum av miljöer, vill du ofta tooconfigure hello program på olika sätt, beroende på hello antalet datorer som du distribuerar till.

Ett enkelt exempel bör du `InstanceCount` för den tillståndslösa tjänsten. När du kör program i Azure, vanligtvis vill du tooset parametern toohello särskilda värdet ”1”. Den här konfigurationen garanterar att din tjänst körs på varje nod i hello kluster (eller alla noder i hello nodtypen om du har angett en begränsning för placering). Den här konfigurationen är dock inte lämpligt för en enskild dator klustret eftersom du inte kan ha flera processer som lyssnar på hello samma slutpunkt på en enskild dator. I stället kan du vanligtvis ange `InstanceCount` för ”1”.

## <a name="specifying-environment-specific-parameters"></a>Ange miljöspecifikt parametrar
hello lösning toothis konfigurationsproblem är en uppsättning parametrar standardtjänster och parametern-programfilerna Fyll i de parametervärden för en given miljö. Standardtjänster och programparametrar är konfigurerade i hello programmet och service manifest. hello schemadefinition för hello ServiceManifest.xml och ApplicationManifest.xml filer installeras med hello Service Fabric-SDK och verktyg för*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Standardtjänster
Service Fabric-program består av en uppsättning instanser av tjänsten. När det är möjligt för toocreate ett tomt program och skapa alla tjänstinstanser dynamiskt har de flesta program en uppsättning kärntjänster som alltid ska skapas när programmet hello instansieras. Dessa är refererad tooas ”standardtjänster”. De har angetts i hello applikationsmanifestet med platshållare för per miljö configuration ingår i hakparenteser:

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

Var och en av hello namngivna parametrar måste definieras inom hello parametrar element i programmanifestet hello:

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

hello DefaultValue-attributet anger hello värdet toobe används i hello frånvaron av en mer specifik parameter för en given miljö.

> [!NOTE]
> Alla service-instansparametrar är inte lämplig för konfigurationen av per miljö. Hej LowKey i hello-exemplet ovan, och HighKey värden för hello service partitioneringsschema definieras uttryckligen för alla instanser av tjänsten hello eftersom hello partitionsintervall är en funktion av hello data domän, inte hello miljö.
> 
> 

### <a name="per-environment-service-configuration-settings"></a>Konfigurationsinställningar för per miljö
Hej [Service Fabric programmodell](service-fabric-application-model.md) aktiverar services tooinclude configuration paket som innehåller anpassade nyckel-värdepar som är läsbart vid körning. hello värden för dessa inställningar kan också skiljas åt av miljö genom att ange en `ConfigOverride` i hello programmanifestet.

Anta att du har följande inställning i hello Config\Settings.xml-filen för hello hello `Stateful1` tjänsten:

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
toooverride värdet för en specifik Programmiljö-par, skapa en `ConfigOverride` när du importerar hello tjänstmanifestet i hello programmanifestet.

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
Den här parametern kan sedan konfigureras av miljö som ovan. Du kan göra detta genom att deklarera under hello parametrar i programmanifestet hello och ange miljö-specifika värden i hello parametern programfiler.

> [!NOTE]
> Hello fall av konfigurationsinställningar, finns det tre platser där du kan ange hello-värdet för en nyckel: hello konfigurationspaket för tjänsten och hello programmanifestet hello parameterfil för programmet. Service Fabric kommer alltid välja från hello programmet parameterfilen först (om det angetts), sedan hello programmanifestet och slutligen hello konfigurationspaket.
> 
> 

### <a name="setting-and-using-environment-variables"></a>Ställa in och använda miljövariabler 
Du kan ange och ange miljövariabler i hello ServiceManifest.xml filen och åsidosätta dessa i hello ApplicationManifest.xml fil på grundval av per instans.
hello exemplet nedan visar två miljövariabler, ange en med ett värde och hello andra åsidosätts. Du kan använda programmet parametrar tooset miljövariabler värdena i hello samma sätt som de har använts för config åsidosättningar.

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
toooverride hello miljövariabler i hello ApplicationManifest.xml referens hello kodpaketet i hello ServiceManifest med hello `EnvironmentOverrides` element.

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 Du kan komma åt hello miljövariabler från kod när hello namngivna tjänstinstansen har skapats. t.ex. I C# kan du göra följande hello

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a>Miljövariabler för Service Fabric
Service Fabric har inbyggda miljövariabler som anges för varje service-instans. hello fullständig lista över miljövariabler är nedan, där hello i fetstil är hello som du ska använda i din tjänst hello andra som används av Service Fabric runtime. 

* Fabric_ApplicationHostId
* Fabric_ApplicationHostType
* Fabric_ApplicationId
* **Fabric_ApplicationName**
* Fabric_CodePackageInstanceId
* **Fabric_CodePackageName**
* **Fabric_Endpoint_ [YourServiceName] TypeEndpoint**
* **Fabric_Folder_App_Log**
* **Fabric_Folder_App_Temp**
* **Fabric_Folder_App_Work**
* **Fabric_Folder_Application**
* Fabric_NodeId
* **Fabric_NodeIPOrFQDN**
* **Fabric_NodeName**
* Fabric_RuntimeConnectionAddress
* Fabric_ServicePackageInstanceId
* Fabric_ServicePackageName
* Fabric_ServicePackageVersionInstance
* FabricPackageFileName

hello kod belows visar hur toolist hello miljövariabler för Service Fabric
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
hello följande är exempel på miljövariabler för en typ av program som kallas `GuestExe.Application` med en typ av kallas `FrontEndService` när det körs på den lokala dev-datorn.

* **Fabric_ApplicationName = fabric:/GuestExe.Application**
* **Fabric_CodePackageName = Code**
* **Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**
* **Fabric_NodeIPOrFQDN = localhost**
* **Fabric_NodeName = _Node_2**

### <a name="application-parameter-files"></a>Parametern programfiler
hello Service Fabric application-projekt kan innehålla en eller flera parametern programfiler. Var och en av dem definierar hello specifika värden för hello-parametrar som definieras i programmanifestet hello:

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
Som standard innehåller tre parametern programfiler, Local.1Node.xml, Local.5Node.xml och Cloud.xml i ett nytt program:

![Parametern programfiler i Solution Explorer][app-parameters-solution-explorer]

toocreate en parameterfil, kopiera och klistra in en befintlig bara och ge det ett nytt namn.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Identifiera miljö-specifika parametrar under distributionen
Vid tidpunkten för distribution måste toochoose hello lämpliga parametern filen tooapply med ditt program. Du kan göra detta via hello publicera dialogrutan i Visual Studio eller PowerShell.

### <a name="deploy-from-visual-studio"></a>Distribuera från Visual Studio
Du kan välja hello listan över tillgängliga parametern filer när du publicerar ditt program i Visual Studio.

![Välj en parameterfil i hello publicera dialogrutan][publishdialog]

### <a name="deploy-from-powershell"></a>Distribuera från PowerShell
Hej `Deploy-FabricApplication.ps1` PowerShell-skript som ingår i hello programmet projektmall accepterar en publiceringsprofil som en parameter och hello PublishProfile innehåller en referens toohello programmet parameterfil.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Nästa steg
toolearn mer om några av hello grundläggande begrepp som diskuteras i det här avsnittet finns hello [teknisk översikt för Service Fabric](service-fabric-technical-overview.md). Information om andra app-hanteringsfunktioner som är tillgängliga i Visual Studio finns [hantera din Service Fabric-program i Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
