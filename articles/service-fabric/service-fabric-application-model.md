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
# <a name="model-an-application-in-service-fabric"></a>Modellen är ett program i Service Fabric
Den här artikeln innehåller en översikt över hello Azure Service Fabric-programmodell och hur toodefine ett program och tjänsten via manifest filer.

## <a name="understand-hello-application-model"></a>Förstå hello programmodell
Ett program är en samling de tjänster som utför en viss funktion eller funktioner. En utför en fullständig och fristående funktion och kan starta och köra oberoende av andra tjänster.  En tjänst består av koden, konfiguration och data. Koden består av hello körbara binärfilerna för varje tjänst, konfigurationen består av service-inställningar som kan läsas in vid körning och data består av godtycklig statiska data toobe förbrukas av hello-tjänsten. Varje komponent i den här modellen hierarkiska programmet kan vara en ny version och uppgraderade oberoende av varandra.

![hello programmodell för Service Fabric][appmodel-diagram]

En typ av program består av ett paket av tjänsttyper är en kategorisering av ett program. En service är en kategorisering av en tjänst. hello kategorisering kan ha olika inställningar och konfigurationer, men hello core funktioner förblir hello samma. hello instanser av en tjänst är hello olika service configuration variationer av hello samma typen tjänst.  

Klasser (eller ”typer”) för program och tjänster beskrivs via XML-filer (applikationsmanifest och service manifest).  hello manifest är hello mallar som program kan initieras från avbildningsarkivet hello klustret. hello schemadefinition för hello ServiceManifest.xml och ApplicationManifest.xml installeras med hello Service Fabric-SDK och verktyg för*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

hello kod för olika programinstanser körs som separata processer även när hos hello samma Service Fabric-nod. Dessutom hello livscykeln för varje instans av programmet kan hanteras (till exempel uppgraderas) oberoende av varandra. hello följande diagram visar hur programtyper består av tjänsttyper, som i sin tur består av koden, konfiguration och data paket. diagram över toosimplify hello, bara hello kod-config-data paket för `ServiceType4` visas, även om varje typ av tjänst skulle lägga till några eller alla pakettyper.

![Service Fabric programtyper och typer av tjänster][cluster-imagestore-apptypes]

Två olika finns används toodescribe program och tjänster: hello tjänstmanifestet och programmanifestet. Manifest beskrivs i detalj i följande avsnitt hello.

Det kan finnas en eller flera instanser av en typ av aktiva i hello kluster. Till exempel uppnå tillståndskänslig tjänstinstanser eller repliker hög tillförlitlighet genom att replikera tillstånd mellan repliker som finns på olika noder i klustret hello. Replikering ger redundans för hello service toobe tillgängliga i stort sett även om en nod i ett kluster misslyckas. En [partitionerad service](service-fabric-concepts-partitioning.md) ytterligare dividerar dess tillstånd (och åtkomstläge mönster toothat) över noderna i klustret hello.

hello visar följande diagram hello förhållandet mellan program och instanser av tjänsten, partitioner och repliker.

![Partitioner och repliker i en tjänst][cluster-application-instances]

> [!TIP]
> Du kan visa hello layouten för program i ett kluster med hello Service Fabric Explorer-verktyg som är tillgängliga på http://&lt;yourclusteraddress&gt;: 19080/Explorer. Mer information finns i [visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
> 
> 

## <a name="describe-a-service"></a>Beskriv en tjänst
hello tjänstmanifestet definieras deklarativt hello service-typen och versionen. Det anger tjänstmetadata som typ av tjänst, hälsa egenskaper, belastningsutjämning mått, binärfiler och konfigurationsfiler.  Placera ett annat sätt beskriver hello koden, konfiguration och data paket som ska utgöra en service paketet toosupport en eller flera typer av tjänster. Här är ett enkelt exempel service manifest:

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

**Version** attribut är Ostrukturerade strängar och inte tolkas av hello system. Version-attribut är används tooversion varje komponent för uppgraderingar.

**ServiceTypes** deklarerar vilka typer av tjänster som stöds av **CodePackages** i manifestet. När en tjänst instansieras mot en av dessa tjänsttyper, aktiveras alla kod paket som deklarerats i manifestet genom att köra deras startpunkter. hello resulterande processer är förväntade tooregister hello stöds tjänsttyper vid körning. Tjänsttyper deklareras på hello manifestet nivå och inte hello kod package-nivå. Så när det finns flera paket i koden, är alla aktiverade när hello görs en sökning efter någon av hello deklarerats tjänsttyper.

**SetupEntryPoint** är en privilegierade startpunkt som körs med samma autentiseringsuppgifter som Service Fabric hello (vanligtvis hello *LocalSystem* konto) innan andra startpunkt. hello körbara filen som anges av **EntryPoint** är vanligtvis hello tidskrävande tjänstvärden. hello förekomsten av en separat installationsprogrammet startpunkt slipper toorun hello tjänstvärden med höga privilegier för längre tid. hello körbara filen som anges av **EntryPoint** körs efter **SetupEntryPoint** avslutas korrekt. Om hello processen någonsin avslutar eller kraschar, hello resulterande processen övervakas och startas om (med början igen **SetupEntryPoint**).  

Vanliga scenarier där **SetupEntryPoint** är när du kör en körbar fil innan hello-tjänsten startar eller du utföra en åtgärd med förhöjda privilegier. Exempel:

* Ställa in och initierar miljövariabler som hello körbara behöver. Detta är inte begränsad tooonly körbara filer skrivs via hello Service Fabric programmeringsmodeller. Exempelvis måste npm.exe miljövariabler som konfigurerats för att distribuera ett node.js-program.
* Konfigurera åtkomstkontroll genom att installera security-certifikat.

Mer information om hur tooconfigure hello **SetupEntryPoint** finns [konfigurera hello princip för en startpunkt för installationen av tjänsten](service-fabric-application-runas-security.md)

**EnvironmentVariables** innehåller en lista över miljövariabler som ställs in för den här koden paketet. Environmentment variabler kan åsidosättas i hello `ApplicationManifest.xml` tooprovide olika värden för olika tjänstinstanser. 

**DataPackage** deklarerar en mapp med namnet med hello **namn** attributet som innehåller godtyckliga statiska data toobe förbrukas av hello process under körning.

**ConfigPackage** deklarerar en mapp med namnet med hello **namn** attributet som innehåller en *Settings.xml* fil. hello inställningsfilen innehåller avsnitt av inställningar för användardefinierade, nyckel / värde-par som hello processen läser tillbaka vid körning. Under en uppgradering, om det bara hello **ConfigPackage** **version** har ändrats sedan hello körs processen inte startas om. I stället meddelar ett återanrop hello process som konfigurationsinställningarna har ändrats så att de kan läsas dynamiskt. Här är ett exempel *Settings.xml* fil:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> Ett service manifest kan innehålla flera koden, konfiguration och data paket. Var och en av dem kan vara en ny version oberoende av varandra.
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

## <a name="describe-an-application"></a>Beskriv ett program
hello programmanifestet beskriver deklarativt hello programtypen och versionen. Den anger service sammansättning metadata, till exempel stabila namn, instans antal/replikering faktor, säkerhetsisolering/princip, placeringen, configuration åsidosättningar, och partitionsschemat ingående tjänsttyper. hello belastningsutjämning domäner där programmet hello placeras beskrivs också.

Därför ett programmanifest beskrivs element på programnivå hello och refererar till ett eller flera service visar toocompose en typ av program. Här är ett enkelt exempel programmanifestet:

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

Som service manifest **Version** attribut är Ostrukturerade strängar och inte tolkas av hello system. Version-attribut är också används tooversion varje komponent för uppgraderingar.

**ServiceManifestImport** innehåller referenser tooservice manifest som ska utgöra den här tillämpningstypen. Importerade service manifest avgör vilka typer av tjänster är giltiga i den här tillämpningstypen. Inom hello ServiceManifestImport, kan du åsidosätta konfigurationsvärden i Settings.xml och miljövariabler i ServiceManifest.xml filer. 


**DefaultServices** deklarerar tjänstinstanser som skapas automatiskt när ett program instansieras mot den här tillämpningstypen. Standardtjänsterna bara i informationssyfte och fungerar som vanligt tjänster i varje avseende när de har skapats. De uppgraderas tillsammans med andra tjänster i hello programinstansen och kan också tas bort.

> [!NOTE]
> Ett programmanifest kan innehålla flera service manifest import och standardtjänster. Varje tjänst manifestimporten kan vara en ny version oberoende av varandra.
> 
> 

hur toomaintain olika program och tjänstparametrar för enskilda miljöer, se toolearn [hantera programparametrar för miljöer med flera](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer toohello following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Nästa steg
[Paketerar ett program](service-fabric-package-apps.md) och gör den redo toodeploy.

[Distribuera och ta bort program] [ 10] beskriver hur toouse PowerShell toomanage programinstanser.

[Hantera programparametrar för miljöer med flera] [ 11] beskriver hur tooconfigure parametrar och miljövariabler för olika programinstanser.

[Konfigurera säkerhetsprinciper för ditt program] [ 12] beskriver hur toorun services under principer toorestrict åtkomst.

[Program värd modeller] [ 13] beskriver relationen mellan repliker (eller instanser) av en distribuerad tjänst och värdprocessen för tjänsten.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
