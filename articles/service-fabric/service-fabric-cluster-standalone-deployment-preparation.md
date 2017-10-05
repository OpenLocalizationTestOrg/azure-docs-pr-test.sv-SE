---
title: "Förberedelse av Azure Service Fabric fristående klustret distribution | Microsoft Docs"
description: "Dokumentation som rör förbereda miljön och skapa klusterkonfigurationen beaktas innan du distribuerar ett kluster som är avsedda för hantering av en produktions-arbetsbelastning."
services: service-fabric
documentationcenter: .net
author: maburlik
manager: timlt
editor: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 1/17/2017
ms.author: maburlik;chackdan
ms.openlocfilehash: f332193f9a53260173a1010b8bf9f08726bea427
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
<a id="preparemachines"></a>

## <a name="plan-and-prepare-your-service-fabric-standalone-cluster-deployment"></a>Planera och förbereda distributionen av Service Fabric fristående kluster
Utför följande steg innan du skapar klustret.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Steg 1: Planera infrastrukturen för kluster
Du håller på att skapa ett Service Fabric-kluster på datorer som du äger, så kan du bestämma vilka typer av fel som du vill att klustret att överleva. Till exempel du behöver separata power linjer eller Internet-anslutningar till dessa datorer? Dessutom kan du överväga att den fysiska säkerheten för dessa datorer. Var finns datorerna och som behöver åtkomst till dem? När du har gjort dessa beslut mappa du logiskt datorerna till de olika feldomäner (se steg 4). Planera för driftskluster infrastrukturen är mer komplicerat än för testkluster.

### <a name="step-2-prepare-the-machines-to-meet-the-prerequisites"></a>Steg 2: Förbered datorer för att uppfylla kraven
Krav för varje dator som du vill lägga till i klustret:

* Minst 16 GB RAM rekommenderas.
* Minst 40 GB ledigt diskutrymme rekommenderas.
* En 4 kärnor eller större processor rekommenderas.
* Anslutning till ett säkert nätverk eller nätverk för alla datorer.
* Windows Server 2012 R2 eller Windows Server 2016. 
* [.NET framework 4.5.1 eller senare](https://www.microsoft.com/download/details.aspx?id=40773), fullständig installation.
* [Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
* Den [RemoteRegistry tjänsten](https://technet.microsoft.com/library/cc754820) måste köras på alla datorer.

Klusteradministratören distribuera och konfigurera klustret måste ha [administratörsbehörighet](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) på varje dator. Du kan inte installera Service Fabric på en domänkontrollant.

### <a name="step-3-determine-the-initial-cluster-size"></a>Steg 3: Bestäm inledande klusterstorleken
Varje nod i ett fristående Service Fabric-kluster har Service Fabric runtime distribueras och är medlem i klustret. I en typisk Produktionsdistribution finns en nod per operativsysteminstans (fysisk eller virtuell). Klusterstorleken bestäms av dina affärsbehov. Du måste dock ha en klustrets minsta storlek på tre noder (datorer eller virtuella datorer).
Du kan ha fler än en nod för utveckling, på en viss dator. Service Fabric stöder bara en nod per fysisk eller virtuell dator i en produktionsmiljö.

### <a name="step-4-determine-the-number-of-fault-domains-and-upgrade-domains"></a>Steg 4: Ta reda på antalet feldomäner och uppgradera domäner
En *feldomän* (FD) är en fysisk enhet till felet och är direkt relaterat till den fysiska infrastrukturen i datacenter. En feldomän består av maskinvarukomponenter (datorer, växlar, nätverk och mer) som delar en enskild felpunkt. Även om det finns ingen 1:1-mappning mellan feldomäner och rack, kan löst tala varje rack betraktas som en feldomän. När du överväger noderna i klustret, rekommenderar vi starkt att noderna fördelas mellan minst tre feldomäner.

Du kan välja namn för varje FD när du anger FDs i ClusterConfig.json. Service Fabric stöder hierarkiska FDs, så du kan din infrastruktur-topologi i dem.  Till exempel är följande FDs giltiga:

* ”faultDomain” ”: fd: / Dator1-Room1/Rack1”
* ”faultDomain” ”: fd: / FD1”
* ”faultDomain” ”: fd: / Room1/Rack1/PDU1/M1”

En *uppgraderingsdomänen* (UD) är en logisk enhet för noder. Under Service Fabric styrd uppgraderingar (en uppgradering av programmet eller en uppgradering av klustret) tas alla noder i ett UD uppgradering medan noder i andra UDs ska vara tillgängliga för att uppfylla begäranden. Firmware-uppgraderingar som du utför på dina datorer inte respektera UDs, så måste du göra dem något datorer samtidigt.

Det enklaste sättet att tänka på de här koncepten är att tänka på FDs som oplanerade fel och UDs som planerat underhåll.

Du kan välja namn för varje UD när du anger UDs i ClusterConfig.json. Till exempel är följande namn giltiga:

* ”upgradeDomain”: ”UD0”
* ”upgradeDomain”: ”UD1A”
* ”upgradeDomain”: ”DomainRed”
* ”upgradeDomain”: ”Blue”

Mer detaljerad information om uppgraderingsdomäner och feldomäner finns [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-the-service-fabric-standalone-package-for-windows-server"></a>Steg 5: Hämta det fristående Service Fabric-paketet för Windows Server
[Hämta länk - Service Fabric fristående paket - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) och packa upp paketet till en dator för distribution som inte ingår i klustret eller till en av de datorer som ska ingå i klustret.

### <a name="step-6-modify-cluster-configuration"></a>Steg 6: Ändra klusterkonfigurationen
Du måste skapa en fristående klustret ClusterConfig.json konfigurationsfil, som beskriver specificering av klustret för att skapa en fristående kluster. Du kan basera konfigurationsfilen på mallarna på på länken nedan. <br>
[Fristående klusterkonfigurationer](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

Mer information om avsnitten i den här filen finns [konfigurationsinställningar för Windows-kluster för fristående](service-fabric-cluster-manifest.md).

Öppna en av filerna ClusterConfig.json från paketet som du hämtade och ändra följande inställningar:
| **Konfigurationsinställningen** | **Beskrivning** |
| --- | --- |
| **Nodetypes får** |Nodtyper kan du dela klusternoderna i olika grupper. Ett kluster måste ha minst en NodeType. Alla noder i en grupp har följande gemensamma egenskaper: <br> **Namnet** -detta är namnet på nodtypen. <br>**Slutpunktsportar** – dessa är olika namngivna slutpunkter (portar) som är associerade med den här nodtypen. Du kan använda alla portnummer som du vill, förutsatt att de inte står i konflikt med något annat i manifestet inte och redan används av andra program som körs på datorn och VM. <br> **Placeringsegenskaper** -dessa beskriver egenskaper för den här nodtypen som du använder som placeringen för systemtjänster eller dina tjänster. Dessa egenskaper är användardefinierade nyckel/värde-par som ger extra metadata för en viss nod. Exempel på nodegenskaper skulle vara om noden har en hårddisk eller grafikkort, antalet spindlar i dess hårddisk, kärnor och andra fysiska egenskaper. <br> **Kapaciteter** -nod kapaciteter definiera namn och en viss resurs mängden att en viss nod har tillgängliga för användning. En nod kan till exempel ange att den har kapacitet för ett mått som kallas ”MemoryInMb” och att den har 2 048 MB tillgängligt som standard. Dessa kapacitet för att säkerställa att tjänster som kräver särskild mängder resurser placeras på noder som har resurser som är tillgängliga i belopp som krävs vid körning.<br>**IsPrimary** - om du har mer än en NodeType definierats se till att endast en har angetts till primära med värdet *SANT*, vilket är där systemet services körs. Alla andra nodtyper ska anges till värdet *FALSKT* |
| **Noder** |Det här är information för varje nod som ingår i klustret (nodtypen nodnamnet, IP-adress, feldomän och uppgraderingsdomän för noden). Datorerna du vill att klustret ska skapas på måste anges här med sina IP-adresser. <br> Om du använder samma IP-adress för alla noder, skapas ett kluster med en ruta, som du kan använda för testning. Använd inte en ruta kluster för att distribuera produktionsarbetsbelastningar. |

När klusterkonfigurationen har haft alla inställningar som konfigureras i miljön, kan den testas mot klustermiljön (steg 7).

<a id="environmentsetup"></a>

### <a name="step-7-environment-setup"></a>Steg 7. Konfigurera miljön

När en Klusteradministratör konfigurerar en fristående Service Fabric-klustret, är miljön måste konfigureras med följande kriterier: <br>
1. Användaren som skapar klustret ska ha administratörsrättigheter säkerhetsrättigheter för att alla datorer som listas som noder i klustret konfigurationsfilen.
2. Datorn där klustret har skapats, samt varje kluster nod dator måste du:
* Har avinstallerat Service Fabric SDK
* Ha Service Fabric runtime avinstalleras 
* Har aktiverat Windows-brandväggen (mpssvc)
* Har aktiverat tjänsten Remote Registry (remoteregistry)
* Har aktiverat delning (SMB)-fil
* Har nödvändiga portarna som öppnas, baserat på klustret configuration portar
* Har nödvändiga portarna för Windows-SMB- och Remote Registry-tjänsten: 135, 137, 138, 139 och 445
* Ha en nätverksanslutning till varandra
3. Ingen av de kluster nod datorerna ska vara en domänkontrollant.
4. Om klustret som ska distribueras är en säker kluster, validera den säkerhet som behövs krav är placera och är korrekt konfigurerade mot konfigurationen.
5. Om klustret datorer inte komma åt internet, kan du ange följande i klusterkonfigurationen:
* Inaktivera telemetri: Under *egenskaper* ange *”enableTelemetry”: false*
* Inaktivera automatisk hämtning av Fabric-version & meddelanden att den aktuella versionen av klustret närmar sig slutet av stödet: Under *egenskaper* ange *”fabricClusterAutoupgradeEnabled”: false*
* Du kan också om internet nätverksåtkomst är begränsat till vitt visas domäner, domäner nedan krävs för automatisk uppgradering: go.microsoft.com download.microsoft.com

6. Ange rätt Service Fabric antivirus undantag:

| **Antivirusprogram exkluderade kataloger** |
| --- |
| Program Files\Microsoft Service Fabric |
| FabricDataRoot (från klusterkonfigurationen) |
| FabricLogRoot (från klusterkonfigurationen) |

| **Antivirusprogram undantagna processer** |
| --- |
| Fabric.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |

### <a name="step-8-validate-environment-using-testconfiguration-script"></a>Steg 8. Validera miljö med hjälp av TestConfiguration skript
Skriptet TestConfiguration.ps1 kan hittas i fristående paketet. Den används som en Best Practices Analyzer för att verifiera några av villkoren ovan och ska användas som en förstånd-kontroll för att verifiera om ett kluster kan distribueras på en given miljö. Om det inte finns några fel, finns i listan under [Miljökonfiguration](service-fabric-cluster-standalone-deployment-preparation.md) för felsökning. 

Det här skriptet kan köras på en dator som har administratörsåtkomst till alla datorer som noder i klustret konfigurationsfilen. Den dator som det här skriptet körs på behöver inte vara en del av klustret.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
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

Den här konfigurationen tester modulen kan för närvarande inte valideras säkerhetskonfigurationen så detta måste göras oberoende av varandra.  

> [!NOTE]
> Kontinuerligt genomför vi förbättringar så att den här modulen stabilare, så om det finns en felaktig eller saknas fall som du tror inte för närvarande fångas av TestConfiguration, meddela oss via vårt [stöder kanaler](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).   
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Skapa ett fristående kluster som körs på Windows Server](service-fabric-cluster-creation-for-windows-server.md)
