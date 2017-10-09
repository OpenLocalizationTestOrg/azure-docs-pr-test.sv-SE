---
title: "aaaCreate ett fristående Azure Service Fabric-kluster | Microsoft Docs"
description: "Skapa ett Azure Service Fabric-kluster på en dator (fysiska eller virtuella) med Windows Server, om den är lokalt eller i något moln."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik;dekapur
ms.openlocfilehash: 444970816290a0448d88a8b2082c75eb7a64cb44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a>Skapa ett fristående kluster som körs på Windows Server
Du kan använda Azure Service Fabric toocreate Service Fabric-kluster på alla virtuella datorer eller datorer som kör Windows Server. Det innebär att distribuera och köra Service Fabric-program i en miljö som innehåller en uppsättning sammankopplade Windows Server-datorer, att den lokala eller med en molntjänstleverantör för. Service Fabric ger en installationsprogrammet paketet toocreate Service Fabric-kluster kallas hello fristående installationspaketet för Windows Server.

Den här artikeln vägleder dig genom hello steg för att skapa en fristående Service Fabric-klustret.

> [!NOTE]
> Det här fristående Windows Server-paketet är kommersiellt tillgängliga och kan användas för Produktionsdistribution. Det här paketet kan innehålla nya Service Fabric-funktioner som finns i ”förhandsversion”. Rulla nedåt för ”[Förhandsgranska funktioner som ingår i det här paketet](#previewfeatures_anchor)”. avsnittet för hello lista av hello förhandsgranskningsfunktioner. Du kan [hämta en kopia av hello EULA](http://go.microsoft.com/fwlink/?LinkID=733084) nu.
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-hello-service-fabric-for-windows-server-package"></a>Få support för hello Service Fabric för Windows Server-paket
* Be hello community om hello Service Fabric fristående paketet för Windows Server i hello [Azure Service Fabric-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).
* Skapa ett ärende för [Professional stöd för Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).  Mer information om Professional Support från Microsoft [här](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
* Du kan också få stöd för det här paketet som en del av [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).
* Mer information, se [Azure Service Fabric supportalternativ](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).
* toocollect loggar för support, kör hello [logginsamlaren för Service Fabric fristående](service-fabric-cluster-standalone-package-contents.md).

<a id="downloadpackage"></a>

## <a name="download-hello-service-fabric-for-windows-server-package"></a>Hämta hello Service Fabric för Windows Server
toocreate hello kluster, Använd hello Service Fabric för Windows Server-paket (Windows Server 2012 R2 och senare) finns här: <br>
[Hämta länk - Service Fabric fristående paket - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)

Hitta information på innehållet i paketet hello [här](service-fabric-cluster-standalone-package-contents.md).

hello Service Fabric runtime-paketet hämtas automatiskt när klustret skapas. Om hur du distribuerar från en dator inte ansluten toohello internet, ladda ned hello runtime-paketet out of band härifrån: <br>
[Hämta länk - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)

Hitta fristående klusterkonfigurationen exempel på: <br>
[Exempel på fristående klusterkonfigurationen](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

<a id="createcluster"></a>

## <a name="create-hello-cluster"></a>Skapa hello-kluster
Service Fabric kan vara distribuerade tooa utveckling av en dator med hjälp av hello *ClusterConfig.Unsecure.DevCluster.json* -fil som ingår i [exempel](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

Packa upp hello fristående paketet tooyour datorn, kopiera hello exempel config-fil toohello lokala datorn och sedan kör hello *CreateServiceFabricCluster.ps1* skript via en administratör PowerShell-session från hello fristående paketmappen:
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a>Steg 1A: skapa ett oskyddat lokal utveckling-kluster
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Se hello Miljökonfiguration avsnitt i [planera och förbereda distributionen av klustret](service-fabric-cluster-standalone-deployment-preparation.md) för felsökningsinformation.

Om du är klar utvecklingsscenarier som körs, kan du ta bort hello Service Fabric-klustret hello datorn genom att referera toosteps i avsnittet ”[tar bort ett kluster](#removecluster_anchor)”. 

### <a name="step-1b-create-a-multi-machine-cluster"></a>Steg 1B: skapa ett kluster för flera datorer
När du har gått igenom hello planering och förberedelser detaljerad på hello nedanstående länk, är du redo toocreate produktion klustret med klustret konfigurationsfilen. <br>
[Planera och förbereda distributionen av kluster](service-fabric-cluster-standalone-deployment-preparation.md)

1. Verifiera hello-konfigurationsfilen som du har skrivit genom att köra hello *TestConfiguration.ps1* skriptet från hello fristående paketmappen:  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    Du bör se utdata som nedan. Om hello nedre fältet ”godkänd” returneras som ”True”, hälsokontroller har gått och hello klustret verkar toobe distribuerbara baserat på inkommande hello-konfigurationen.

    ```
    Trace folder already exists. Traces will be written tooexisting trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
    Running Best Practices Analyzer...
    Best Practices Analyzer completed successfully.
    
    LocalAdminPrivilege        : True
    IsJsonValid                : True
    IsCabValid                 : True
    RequiredPortsOpen          : True
    RemoteRegistryAvailable    : True
    FirewallAvailable          : True
    RpcCheckPassed             : True
    NoConflictingInstallations : True
    FabricInstallable          : True
    Passed                     : True
    ```

2. Skapa hello kluster: kör hello *CreateServiceFabricCluster.ps1* skriptet toodeploy hello Service Fabric-kluster på varje dator i hello konfiguration. 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> Distribution spårningar skrivs toohello VM/datorn där du körde hello CreateServiceFabricCluster.ps1 PowerShell-skript. Dessa återfinns i hello undermapp DeploymentTraces, i hello directory från vilka hello skriptet kördes. toosee om Service Fabric har distribuerats korrekt tooa datorn, hitta hello installerat filer i hello FabricDataRoot directory, enligt anvisningarna i konfigurationsfilen för hello klustret FabricSettings avsnitt (som standard c:\ProgramData\SF). FabricHost.exe och Fabric.exe processer kan också ses körs i Aktivitetshanteraren.
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a>Steg 1C: skapa ett kluster som offline (internet frånkopplad)
hello Service Fabric runtime-paketet hämtas automatiskt när klustret skapas. Distribuera ett kluster toomachines inte när anslutna toohello internet, behöver du toodownload hello Service Fabric runtime paketet separat och ange hello sökvägen tooit när klustret skapas.
hello runtime-paketet kan laddas ned separat, från en annan dator ansluten toohello internet, vid [Hämta länk - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354). Kopiera hello runtime paketet toowhere du distribuerar hello offline kluster från och skapa hello kluster genom att köra `CreateServiceFabricCluster.ps1` med hello `-FabricRuntimePackagePath` parametern ingår, enligt nedan: 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
där `.\ClusterConfig.json` och `.\MicrosoftAzureServiceFabric.cab` är hello sökvägar toohello klusterkonfigurationen respektive hello runtime CAB-fil.


### <a name="step-2-connect-toohello-cluster"></a>Steg 2: Anslut toohello kluster
tooconnect tooa säker kluster, se [Service fabric ansluta toosecure klustret](service-fabric-connect-to-secure-cluster.md).

tooconnect tooan oskyddade kluster, kör hello följande PowerShell-kommando:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
Exempel:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a>Steg 3: Ta fram Service Fabric Explorer
Nu kan du ansluta toohello klustret med Service Fabric Explorer antingen direkt från en av hello datorerna http://localhost:19080/Explorer/index.html eller via fjärranslutning med http://<*IPAddressofaMachine*>: 19080 / Explorer/index.HTML.

## <a name="add-and-remove-nodes"></a>Lägga till och ta bort noder
Du kan lägga till eller ta bort noder tooyour fristående Service Fabric-klustret när ditt företag behöver ändras. Se [lägga till eller ta bort noder fristående Service Fabric-klustret tooa](service-fabric-cluster-windows-server-add-remove-nodes.md) detaljerade anvisningar.

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a>Ta bort ett kluster
tooremove ett kluster som kör hello *RemoveServiceFabricCluster.ps1* PowerShell-skript från hello paketmappen och skicka hello sökvägen toohello JSON-konfigurationsfil. Du kan du ange en plats för hello logg hello borttagningen av.

Det här skriptet kan köras på en dator som har administratören åtkomst tooall hello datorer som listas som noder i konfigurationsfilen för hello-klustret. hello-dator som det här skriptet körs på har inte toobe tillhör hello kluster.

```
# Removes Service Fabric from each machine in hello configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from hello current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-tooopt-out-of-it"></a>Telemetridata som samlas in och hur tooopt ur den
Som standard hello produkt samlar in telemetri om hello Service Fabric användning tooimprove hello produkt. hello Best Practice Analyzer som körs som en del av installationen hello kontrollerar anslutningen för[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Om det inte går att nå misslyckas hello installationen om du avanmäler dig från telemetri.

1. hello telemetri pipeline försöker tooupload hello följande data för[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) varje gång om dagen. Det är en bästa överföring och har ingen inverkan på hello klusterfunktionaliteten. hello telemetri skickas endast från hello nod som kör hello failover manager primär. Inga andra noder skicka ut telemetri.
2. hello telemetri består av hello följande:

* Antal tjänster
* Antal ServiceTypes
* Antal program
* Antal ApplicationUpgrades
* Antal FailoverUnits
* Antal InBuildFailoverUnits
* Antal UnhealthyFailoverUnits
* Antal repliker
* Antal InBuildReplicas
* Antal StandByReplicas
* Antal OfflineReplicas
* CommonQueueLength
* QueryQueueLength
* FailoverUnitQueueLength
* CommitQueueLength
* Antalet noder
* IsContextComplete: True/False
* ClusterId: Detta är ett GUID som genereras slumpmässigt för varje kluster
* ServiceFabricVersion
* Hello virtuella datorn eller datorn från vilken hello telemetri överförs IP-adress

toodisable telemetri, Lägg till följande hello för*egenskaper* i klustret config: *enableTelemetry: false*.

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a>Förhandsgranskningsfunktioner som ingår i det här paketet
Ingen.


> [!NOTE]
> Från och med hello nya [GA-versionen av hello fristående kluster för Windows Server (version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), du kan uppgradera ditt kluster toofuture versioner, manuellt eller automatiskt. Se för[uppgradera en fristående Service Fabric-kluster version](service-fabric-cluster-upgrade-windows-server.md) dokument för mer information.
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Distribuera och ta bort program med hjälp av PowerShell](service-fabric-deploy-remove-applications.md)
* [Konfigurationsinställningar för fristående Windows-kluster](service-fabric-cluster-manifest.md)
* [Lägg till eller ta bort noder tooa fristående Service Fabric-kluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Uppgradera en fristående Service Fabric-kluster-version](service-fabric-cluster-upgrade-windows-server.md)
* [Skapa ett fristående Service Fabric-kluster med Azure virtuella datorer som kör Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Skydda ett fristående kluster på Windows med hjälp av Windows-säkerhet](service-fabric-windows-cluster-windows-security.md)
* [Skydda ett fristående kluster på Windows med X509 certifikat](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
