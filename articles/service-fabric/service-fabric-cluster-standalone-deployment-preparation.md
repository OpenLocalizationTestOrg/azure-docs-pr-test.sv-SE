---
title: "aaaAzure Service Fabric fristående kluster distribution förberedelse | Microsoft Docs"
description: "Dokumentationen relaterade toopreparing hello miljö och skapa hello klusterkonfigurationen toobe anses vara tidigare toodeploying ett kluster som är avsedda för att hantera en produktion arbetsbelastning."
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
ms.openlocfilehash: e503c61a64b408af3f22bd75ab02f1c34ac9f380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
<a id="preparemachines"></a>

## <a name="plan-and-prepare-your-service-fabric-standalone-cluster-deployment"></a>Planera och förbereda distributionen av Service Fabric fristående kluster
Utför följande steg innan du skapar klustret hello.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Steg 1: Planera infrastrukturen för kluster
Du håller toocreate Service Fabric-kluster på datorer som du äger, så att du kan bestämma vilka typer av fel som du vill hello klustret toosurvive. Till exempel behöver du ha separata power rader eller Internetanslutningar angivna toothese datorer? Dessutom kan du överväga att hello fysisk säkerhet för dessa datorer. Var finns hello datorer och som behöver åtkomst toothem? När du gör dessa beslut mappa du logiskt hello datorer toohello olika feldomäner (se steg 4). hello infrastruktur planera för driftskluster är mer komplicerat än för testkluster.

### <a name="step-2-prepare-hello-machines-toomeet-hello-prerequisites"></a>Steg 2: Förbered hello datorer toomeet hello krav
Krav för varje dator som du vill tooadd toohello klustret:

* Minst 16 GB RAM rekommenderas.
* Minst 40 GB ledigt diskutrymme rekommenderas.
* En 4 kärnor eller större processor rekommenderas.
* Anslutningen tooa säkert nätverk eller nätverk för alla datorer.
* Windows Server 2012 R2 eller Windows Server 2016. 
* [.NET framework 4.5.1 eller senare](https://www.microsoft.com/download/details.aspx?id=40773), fullständig installation.
* [Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
* Hej [RemoteRegistry tjänsten](https://technet.microsoft.com/library/cc754820) måste köras på alla hello-datorer.

hello Klusteradministratören distribuera och konfigurera hello klustret måste ha [administratörsbehörighet](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) på varje hello datorer. Du kan inte installera Service Fabric på en domänkontrollant.

### <a name="step-3-determine-hello-initial-cluster-size"></a>Steg 3: Bestäm hello inledande klusterstorleken
Varje nod i ett fristående Service Fabric-kluster har hello Service Fabric runtime distribueras och är medlem i hello kluster. I en typisk Produktionsdistribution finns en nod per operativsysteminstans (fysisk eller virtuell). hello klusterstorleken bestäms av dina affärsbehov. Du måste dock ha en klustrets minsta storlek på tre noder (datorer eller virtuella datorer).
Du kan ha fler än en nod för utveckling, på en viss dator. Service Fabric stöder bara en nod per fysisk eller virtuell dator i en produktionsmiljö.

### <a name="step-4-determine-hello-number-of-fault-domains-and-upgrade-domains"></a>Steg 4: Fastställa hello antalet feldomäner och uppgradera domäner
En *feldomän* (FD) är en fysisk enhet till felet och är direkt relaterade toohello fysisk infrastruktur i hello datacenter. En feldomän består av maskinvarukomponenter (datorer, växlar, nätverk och mer) som delar en enskild felpunkt. Även om det finns ingen 1:1-mappning mellan feldomäner och rack, kan löst tala varje rack betraktas som en feldomän. När du överväger hello noderna i klustret, rekommenderar vi starkt att hello noder fördelas mellan minst tre feldomäner.

Du kan välja hello namn för varje FD när du anger FDs i ClusterConfig.json. Service Fabric stöder hierarkiska FDs, så du kan din infrastruktur-topologi i dem.  Till exempel är hello följande FDs giltiga:

* ”faultDomain” ”: fd: / Dator1-Room1/Rack1”
* ”faultDomain” ”: fd: / FD1”
* ”faultDomain” ”: fd: / Room1/Rack1/PDU1/M1”

En *uppgraderingsdomänen* (UD) är en logisk enhet för noder. Under Service Fabric styrd uppgraderingar (en uppgradering av programmet eller en uppgradering av klustret) tas alla noder i ett UD ned tooperform hello uppgraderingen medan noder i andra UDs förblir tillgängliga tooserve begäranden. hello firmware-uppgraderingar som du utför på dina datorer inte respektera UDs, så måste du göra dem något datorer samtidigt.

hello enklaste sättet toothink om de här koncepten är tooconsider FDs som hello enhet för oplanerade fel och UDs som hello enhet för planerat underhåll.

Du kan välja hello namn för varje UD när du anger UDs i ClusterConfig.json. Till exempel är hello följande namn giltiga:

* ”upgradeDomain”: ”UD0”
* ”upgradeDomain”: ”UD1A”
* ”upgradeDomain”: ”DomainRed”
* ”upgradeDomain”: ”Blue”

Mer detaljerad information om uppgraderingsdomäner och feldomäner finns [som beskriver ett Service Fabric-kluster](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-hello-service-fabric-standalone-package-for-windows-server"></a>Steg 5: Hämta hello Service Fabric fristående paketet för Windows Server
[Hämta länk - Service Fabric fristående paket - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) och packa hello paketet, antingen tooa distribution datorn som är inte en del av hello kluster eller tooone hello-datorer som ska ingå i klustret.

### <a name="step-6-modify-cluster-configuration"></a>Steg 6: Ändra klusterkonfigurationen
toocreate ett fristående kluster som du har toocreate en fristående klustret ClusterConfig.json konfigurationsfil, som beskriver hello specificering av hello klustret. Du kan basera hello konfigurationsfilen på hello mallar finns på hello länken nedan. <br>
[Fristående klusterkonfigurationer](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

Mer information om hello avsnitt i den här filen finns [konfigurationsinställningar för Windows-kluster för fristående](service-fabric-cluster-manifest.md).

Öppna en hello ClusterConfig.json filer från hello-paketet som du hämtade och ändra hello följande inställningar:
| **Konfigurationsinställningen** | **Beskrivning** |
| --- | --- |
| **Nodetypes får** |Nodtyper Tillåt tooseparate klusternoderna i olika grupper. Ett kluster måste ha minst en NodeType. Alla noder i en grupp har hello följande gemensamma egenskaper: <br> **Namnet** -detta är namnet hello nodtypen. <br>**Slutpunktsportar** – dessa är olika namngivna slutpunkter (portar) som är associerade med den här nodtypen. Du kan använda alla portnummer som du vill, förutsatt att de inte står i konflikt med något annat i manifestet inte och redan används av andra program som körs på hello dator och VM. <br> **Placeringsegenskaper** -dessa beskriver egenskaper för den här nodtypen som du använder som placeringen för hello systemtjänster eller dina tjänster. Dessa egenskaper är användardefinierade nyckel/värde-par som ger extra metadata för en viss nod. Exempel på nodegenskaper skulle vara om hello-noden har en hårddisk eller grafikkort, hello antalet spindlar i dess hårddisk, kärnor och andra fysiska egenskaper. <br> **Kapaciteter** -noden kapaciteter definiera hello namn och mängd för en viss resurs att en viss nod har tillgängliga för användning. En nod kan till exempel ange att den har kapacitet för ett mått som kallas ”MemoryInMb” och att den har 2 048 MB tillgängligt som standard. Dessa kapaciteter som används vid körning tooensure att tjänster som kräver särskild mängder resurser placeras på hello noder som har resurser som är tillgängliga i hello nödvändiga belopp.<br>**IsPrimary** - om du har fler än en NodeType definierats är Kontrollera att endast en tooprimary med hello värdet *SANT*, vilket är där hello system services körs. Alla andra nodtyper ska anges toohello värdet *FALSKT* |
| **Noder** |Dessa är hello information för varje hello-noder som tillhör hello kluster (nodtypen, nodnamn, IP-adress, feldomän och uppgraderingsdomän för hello nod). hello datorer du vill hello klustret toobe skapas på behovet av toobe i den här listan med sina IP-adresser. <br> Om du använder hello samma IP-adress för alla hello noder och ett kluster med en ruta skapas, som du kan använda för testning. Använd inte en ruta kluster för att distribuera produktionsarbetsbelastningar. |

När hello klusterkonfigurationen har alla inställningarna toohello miljö, kan den testas mot hello klustermiljön (steg 7).

<a id="environmentsetup"></a>

### <a name="step-7-environment-setup"></a>Steg 7. Konfigurera miljön

När en Klusteradministratör konfigurerar en fristående Service Fabric-klustret, är hello miljö behov toobe med hello följande kriterier: <br>
1. hello användare skapar hello kluster ska ha behörighet på säkerhet privilegier tooall datorer som listas som noder i konfigurationsfilen för hello klustret.
2. Datorn från vilken hello klustret har skapats, samt varje kluster nod dator måste du:
* Har avinstallerat Service Fabric SDK
* Ha Service Fabric runtime avinstalleras 
* Har hello Windows-brandväggen (mpssvc) aktiverat
* Har aktiverat hello Remote Registry-tjänsten (remoteregistry)
* Har aktiverat delning (SMB)-fil
* Har nödvändiga portarna som öppnas, baserat på klustret configuration portar
* Har nödvändiga portarna för Windows-SMB- och Remote Registry-tjänsten: 135, 137, 138, 139 och 445
* Network connectivity tooone har en annan
3. Ingen av hello kluster nod datorerna ska vara en domänkontrollant.
4. Om säker klustret har hello klustret toobe distribueras kan validera hello säkerhet som behövs krav är placera och är korrekt konfigurerade mot hello konfiguration.
5. Om hello klustret datorer inte komma åt internet, klusterkonfiguration ange hello följande i hello:
* Inaktivera telemetri: Under *egenskaper* ange *”enableTelemetry”: false*
* Inaktivera automatisk hämtning av Fabric-version & meddelanden som hello aktuella kluster av version närmar sig slutet av stödet: Under *egenskaper* ange *”fabricClusterAutoupgradeEnabled”: false*
* Du kan också om internet nätverksåtkomst är begränsad toowhite visas domäner, hello domäner nedan krävs för automatisk uppgradering: go.microsoft.com download.microsoft.com

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
Hej TestConfiguration.ps1 skript finns i hello fristående paketet. Det är används som en Best Practices Analyzer toovalidate några av hello villkoren ovan och bör användas som en förstånd Kontrollera toovalidate om ett kluster kan distribueras på en given miljö. Om det inte finns några fel, se toohello listan under [Miljökonfiguration](service-fabric-cluster-standalone-deployment-preparation.md) för felsökning. 

Det här skriptet kan köras på en dator som har administratören åtkomst tooall hello datorer som listas som noder i konfigurationsfilen för hello-klustret. hello-dator som det här skriptet körs på har inte toobe tillhör hello kluster.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
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

Den här konfigurationen tester modulen kan för närvarande inte valideras hello säkerhetskonfiguration så att det har toobe klar oberoende.  

> [!NOTE]
> Kontinuerligt genomför vi förbättringar toomake den här modulen mer robust, så om det finns en felaktig eller saknat case som du tror att för närvarande inte fångas av TestConfiguration, meddela oss via vårt [stöder kanaler](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).   
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Skapa ett fristående kluster som körs på Windows Server](service-fabric-cluster-creation-for-windows-server.md)
