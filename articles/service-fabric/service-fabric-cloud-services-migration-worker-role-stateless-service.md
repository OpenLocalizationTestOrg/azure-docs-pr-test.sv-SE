---
title: aaaConvert Azure Cloud Services appar toomicroservices | Microsoft Docs
description: "Den här guiden jämför Cloud Services Web och arbetsroller och Service Fabric tillståndslösa tjänster toohelp migrera från molntjänster tooService Fabric."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: c43b11623b2ba7f6069782a8b7e030c82572a6e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guide-tooconverting-web-and-worker-roles-tooservice-fabric-stateless-services"></a>Guide tooconverting webb- och arbetsroller tooService Fabric tillståndslösa tjänster
Den här artikeln beskriver hur toomigrate dina Cloud Services webb- och arbetsroller tooService Fabric tillståndslösa tjänster. Detta är hello enklaste migreringsvägen från molntjänster tooService Fabric för program vars övergripande arkitektur försätts toostay ungefär hello samma.

## <a name="cloud-service-project-tooservice-fabric-application-project"></a>Cloud Service-projekt tooService Fabric application-projekt
 Cloud Service-projekt och ett tjänstprogram för Fabric-projekt har en liknande struktur och båda representerar hello distributionsenhet för ditt program – det vill säga de definiera fullständig hello-paket som är distribuerade toorun ditt program. En Cloud Service-projekt innehåller en eller flera rollerna webb eller arbetare. På liknande sätt kan innehåller ett tjänstprogram för Fabric-projekt en eller flera tjänster. 

hello skillnaden är att hello Cloud Service-projekt par hello programdistribution med en distribution av Virtuella datorer och därmed innehåller inställningar för virtuell dator, hello Service Fabric programprojekt bara definierar ett program som ska distribueras tooa uppsättning befintliga virtuella datorer i ett Service Fabric-kluster. hello Service Fabric själva klustret distribueras bara en gång, antingen via en Resource Manager-mall eller hello Azure-portalen och flera Service Fabric tillämpningsprogram kan vara distribuerade tooit.

![Jämförelse mellan Service Fabric och Cloud Services-projekt][3]

## <a name="worker-role-toostateless-service"></a>Worker toostateless rolltjänst
En Arbetsroll representerar begreppsmässigt en tillståndslös arbetsbelastning, vilket innebär att varje förekomst av hello arbetsbelastningen är identiska och begäranden kan vara routade tooany instans när som helst. Varje instans är inte förväntade tooremember hello tidigare begäran. Tillstånd att hello arbetsbelastningar fungerar på hanteras av en extern tillståndslager, till exempel Azure Table Storage eller Azure-dokumentet DB. Den här typen av arbetsbelastning representeras av en tjänst för tillståndslösa i Service Fabric. hello enklaste metoden toomigrating en Arbetsroll tooService Fabric kan göras genom att konvertera Arbetsrollen kod tooa tillståndslösa tjänsten.

![Worker-rollen tooStateless Service][4]

## <a name="web-role-toostateless-service"></a>Webbtjänsten rollen toostateless
Liknande tooWorker roll, en Webbroll representerar också en tillståndslös arbetsbelastning och så begreppsmässigt för kan vara mappade tooa tillståndslösa Service Fabric-tjänsten. Men till skillnad från webbroller stöder Service Fabric inte IIS. toomigrate ett program från en Webbroll tooa tillståndslösa tjänsten kräver första glidande tooa webbramverk som kan finnas själv och är inte beroende av IIS eller System.Web, till exempel ASP.NET Core 1.

| **Programmet** | **Stöds** | **Sökväg för migrering** |
| --- | --- | --- |
| ASP.NET Web Forms |Nej |Konvertera tooASP.NET Core 1 MVC |
| ASP.NET MVC |Med migrering |Uppgradera tooASP.NET Core 1 MVC |
| ASP.NET Webb-API |Med migrering |Använda automatisk värdbaserade servern eller ASP.NET Core 1 |
| ASP.NET Core 1 |Ja |Saknas |

## <a name="entry-point-api-and-lifecycle"></a>Posten punkt API och livscykel
Worker-rollen och Service Fabric-API: er erbjuder liknande startpunkter: 

| **Startpunkt** | **Worker-rollen** | **Service Fabric-tjänsten** |
| --- | --- | --- |
| Bearbetning av |`Run()` |`RunAsync()` |
| Starta VM |`OnStart()` |Saknas |
| Stoppa VM |`OnStop()` |Saknas |
| Öppna lyssnaren för klientbegäranden |Saknas |<ul><li> `CreateServiceInstanceListener()`för tillståndslösa</li><li>`CreateServiceReplicaListener()`för stateful</li></ul> |

### <a name="worker-role"></a>Worker-rollen
```C#

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a>Service Fabric tillståndslösa tjänsten
```C#

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

Båda har en primär ”kör” åsidosättning toobegin behandling. Service Fabric services kombinera `Run`, `Start`, och `Stop` till en enda kontaktpunkt `RunAsync`. Din tjänst ska börja arbeta när `RunAsync` startar och ska sluta fungera när hello `RunAsync` metodens CancellationToken signaleras. 

Det finns flera viktiga skillnader mellan hello livscykel och livslängden för arbetsroller och Service Fabric-tjänster:

* **Livscykel:** hello största skillnaden är att en Arbetsroll är en virtuell dator och dess livscykel är därför bundet toohello VM, som innefattar händelser för när hello VM startar och stoppar. En Service Fabric-tjänst har en livscykel som skiljer sig från hello VM livscykel, så att den inte innehåller händelser för när hello värddator VM eller datorn startar och stoppa, eftersom de inte är relaterade.
* **Livslängd:** Arbetsrollen-instansen kommer att återanvändas om hello `Run` metoden avslutar. Hej `RunAsync` metod i en tjänst för Service Fabric men kan köra toocompletion och hello tjänstinstans ska fortsätta. 

Service Fabric ger en startpunkt för valfri kommunikation installationsprogrammet för tjänster som lyssnar efter förfrågningar från klienter. Både hello RunAsync och kommunikation startpunkten är valfria åsidosättningar i Service Fabric-services - tjänsten kan välja tooonly lyssna tooclient begäranden eller endast köra en loop i bearbetning eller båda - vilket är anledningen till hello RunAsync-metoden tillåts tooexit utan Starta om hello tjänstinstansen eftersom den kan fortsätta toolisten för klientbegäranden.

## <a name="application-api-and-environment"></a>Program-API och miljö
hello molntjänster miljö API innehåller information och funktioner för hello aktuella VM-instans, samt information om andra instanser för VM-roll. Service Fabric innehåller information som rör tooits runtime och viss information om hello nod en tjänst körs på. 

| **Miljö aktivitet** | **Cloud Services** | **Service Fabric** |
| --- | --- | --- |
| Konfigurationsinställningar och ändringsmeddelande |`RoleEnvironment` |`CodePackageActivationContext` |
| Lokal lagring |`RoleEnvironment` |`CodePackageActivationContext` |
| Slutpunkten |`RoleInstance` <ul><li>Aktuell instans:`RoleEnvironment.CurrentRoleInstance`</li><li>Andra roller och instans:`RoleEnvironment.Roles`</li> |<ul><li>`NodeContext`för den aktuella noden adressen</li><li>`FabricClient`och `ServicePartitionResolver` för identifiering av tjänst slutpunkt</li> |
| Miljö emulering |`RoleEnvironment.IsEmulated` |Saknas |
| Samtidiga Ändringshändelse |`RoleEnvironment` |Saknas |

## <a name="configuration-settings"></a>Konfigurationsinställningar
Konfigurationsinställningarna i molntjänster har angetts för en VM-roll och tillämpa tooall instanser av den Virtuella datorrollen. De här inställningarna är nyckel / värde-par i ServiceConfiguration.*.cscfg filer och kan nås direkt via RoleEnvironment. I Service Fabric inställningarna individuellt tooeach service och tooeach program snarare än tooa VM, eftersom en virtuell dator kan vara värd för flera tjänster och program. En tjänst består av tre paket:

* **Kod:** innehåller hello service körbara filer, binärfiler, DLL-filer och alla andra filer som en tjänst behöver toorun.
* **Config:** alla konfigurationsfiler och inställningarna för en tjänst.
* **Data:** Statiska filer som är associerade med hello-tjänsten.

Var och en av dessa paket kan vara oberoende versionsnummer och uppgraderade. Liknande tooCloud tjänster, en konfigurationspaketet kan nås via programmering genom en API och händelser som är tillgängliga toonotify hello tjänsten av en ändring för config-paketet. En Settings.xml-fil kan användas för nyckel / värde-konfiguration och Programmeringsåtkomst liknande toohello app inställningar för en App.config-fil. Men till skillnad från molntjänster, kan ett Service Fabric config-paket innehålla konfigurationsfiler i valfritt format, oavsett om det är XML, JSON, YAML eller ett anpassat binärt format. 

### <a name="accessing-configuration"></a>Åtkomst till konfiguration
#### <a name="cloud-services"></a>Molntjänster
Konfigurationsinställningar från ServiceConfiguration.*.cscfg kan nås via `RoleEnvironment`. De här inställningarna är globalt tillgänglig tooall rollinstanser i hello distribution för samma tjänst i molnet.

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a>Service Fabric
Varje tjänst har sin egen enskilda konfigurationspaket. Det finns ingen inbyggd mekanism för globala inställningar nås av alla program i ett kluster. När du använder Service Fabric särskilda Settings.xml konfigurationsfilen inom ett konfigurationspaket kan värden i Settings.xml åsidosättas på hello programnivå gör det möjligt på programnivå konfigurationsinställningar.

Konfigurationsinställningarna är öppningar i varje tjänstinstans via hello tjänsten `CodePackageActivationContext`.

```C#

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a>Update-konfigurationshändelser
#### <a name="cloud-services"></a>Molntjänster
Hej `RoleEnvironment.Changed` händelsen är används toonotify alla rollinstanser när en ändring inträffar i hello miljö, till exempel en konfigurationsändring. Detta är används tooconsume konfigurationsuppdateringar utan återvinning rollinstanser eller omstart av en arbetsprocess.

```C#

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get hello list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a>Service Fabric
Varje hello tre pakettyper i en tjänst - kod, konfiguration och Data - ha händelser som meddelar en tjänstinstans när ett paket uppdateras, lägga till eller tas bort. En tjänst kan innehålla flera paket för varje typ. En tjänst kan till exempel ha flera config paket, varje enskilt version och kan uppgraderas. 

Dessa händelser finns tillgängliga tooconsume ändringar i servicepaket utan att starta om hello tjänstinstansen.

```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a>Start-uppgifter
Start-aktiviteter är åtgärder som vidtas innan ett program startas. En startaktivitet är brukar användas toorun installationsprogrammet skript med utökade privilegier. Både molntjänster och Service Fabric stöder uppstart uppgifter. hello största skillnaden är att i Cloud Services, en startaktivitet är bundet tooa VM eftersom den är en del av en rollinstans i Service Fabric en startaktivitet är bundet tooa tjänsten, som inte är bundet tooany viss VM.

| Molntjänster | Service Fabric |
| --- | --- | --- |
| Platsen för konfigurationen |ServiceDefinition.csdef |
| Privilegier |”begränsad” eller ”utökade” |
| Sekvensering |”enkla”, ”bakgrund”, ”förgrunden” |

### <a name="cloud-services"></a>Molntjänster
I Cloud Services konfigureras en startpunkt för start per roll i ServiceDefinition.csdef. 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a>Service Fabric
I Service Fabric konfigureras en startpunkt för start per tjänst i ServiceManifest.xml:

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a>En anteckning om utvecklingsmiljö
Både molntjänster och Service Fabric är integrerade med Visual Studio med projektmallar och stöd för felsökning, konfigurera och distribuera både lokalt och tooAzure. Både molntjänster och Service Fabric ger också en körningsmiljö för lokal utveckling. hello är skillnaden att när hello Molntjänsten development runtime emulerar hello Azure-miljön där den körs, Service Fabric inte använder en emulator - hello fullständig Service Fabric runtime används. hello hello Service Fabric-miljö som du kör på utvecklingsdatorn lokala är samma miljö som körs i produktion.

## <a name="next-steps"></a>Nästa steg
Läs mer om Service Fabric Reliable Services och hello viktiga skillnader mellan Cloud Services och Service Fabric application arkitektur toounderstand hur tootake nytta av hello fullständig uppsättning funktioner för Service Fabric.

* [Komma igång med Service Fabric Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Konceptuell guiden toohello skillnaderna mellan molntjänster och Service Fabric](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
