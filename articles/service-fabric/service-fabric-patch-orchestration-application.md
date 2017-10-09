---
title: aaaAzure Service Fabric korrigering orchestration program | Microsoft Docs
description: "Programmet tooautomate operativsystem system korrigering på ett Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: 
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/9/2017
ms.author: nachandr
ms.openlocfilehash: fbb89aa2ea418181ee908a01850178c113c462fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="patch-hello-windows-operating-system-in-your-service-fabric-cluster"></a>Korrigering av hello Windows-operativsystem i Service Fabric-kluster

hello korrigering orchestration programmet är ett Azure Service Fabric som automatiserar operativsystemet korrigering på ett Service Fabric-kluster i Azure utan driftavbrott.

hello korrigering orchestration app innehåller hello följande:

- **Automatisk uppdatering operativsysteminstallation**. Uppdateringar av operativsystemet hämtas och installeras automatiskt. Noder i klustret startas om vid behov utan klusterdriftstopp.

- **Klustermedveten uppdatering och hälsa integration**. När uppdateringar, övervakar hello korrigering orchestration app hello hälsotillstånd hello klusternoder. Klusternoder är uppgraderade en nod i taget eller en domän i taget. Om hello hälsotillstånd hello klustret stängs av på grund av toohello uppdatering process, är korrigering stoppad tooprevent aggravating hello problem.

## <a name="internal-details-of-hello-app"></a>Intern information hello-appen

hello korrigering orchestration app består av följande delkomponenter hello:

- **Coordinator-tjänsten**: tillståndskänslig tjänsten ansvarar för att:
    - Samordna hello Windows Update-jobbet på hello hela klustret.
    - Lagra hello resultatet av slutförda Windows Update-åtgärder.
- **Nod-agenttjänsten**: tillståndslösa tjänsten körs på alla klusternoder för Service Fabric. hello tjänsten ansvarar för:
    - Startprogram för hello nod Agent NTService.
    - Övervakning av hello nod Agent NTService.
- **Noden Agent NTService**: den här Windows NT-tjänsten körs på en högre nivå behörighet (SYSTEM). Däremot körs hello nod Agent-tjänsten och hello Coordinator-tjänsten på en lägre nivå behörighet (NETWORK SERVICE). hello tjänsten ansvarar för att utföra hello följa Windows Update-jobb på alla noder i klustret hello:
    - Inaktiverar automatisk Windows Update på hello-nod.
    - Hämta och installera Windows Update har enligt toohello princip hello användare angetts.
    - Starta om hello datorn efter installationen av Windows Update.
    - Överför hello resultat av Windows-uppdateringar toohello Coordinator-tjänsten.
    - Reporting systemhälsorapporter om en åtgärd misslyckades efter överbelastar alla nya försök.

> [!NOTE]
> hello korrigering orchestration app använder hello Service Fabric reparera manager system service toodisable eller aktivera hello nod och utföra hälsokontroller. hello reparera aktivitet som skapats av hello korrigering orchestration app spårar hello Windows Update status för varje nod.

## <a name="prerequisites"></a>Krav

### <a name="minimum-supported-service-fabric-runtime-version"></a>Minimal stödd version av Service Fabric runtime

#### <a name="azure-clusters"></a>Azure-kluster
hello korrigering orchestration app måste köras på Azure kluster som har Service Fabric runtime version version 5.5 eller senare.

#### <a name="standalone-on-premises-clusters"></a>Fristående lokala kluster
hello korrigering orchestration app måste köras på fristående kluster som har Service Fabric runtime version v5.6 eller senare.

### <a name="enable-hello-repair-manager-service-if-its-not-running-already"></a>Aktivera hello reparera manager-tjänsten (om den inte redan körs)

hello korrigering orchestration appen kräver hello reparera manager system service toobe aktiverad på hello klustret.

#### <a name="azure-clusters"></a>Azure-kluster

Azure-kluster i hello silver hållbarhetsnivån ha hello reparera manager-tjänsten som är aktiverad som standard. Azure-kluster i hello guld hållbarhetsnivån kanske eller kanske inte hello manager reparation aktiverad, beroende på när dessa kluster skapades. Azure-kluster i hello Brons hållbarhetsnivån, som standard inte hello reparera manager-tjänsten aktiverad. Om hello-tjänsten redan är aktiverat, kan du se den i avsnittet hello system services i hello Service Fabric Explorer.

##### <a name="azure-portal"></a>Azure Portal
Du kan aktivera reparera manager från Azure-portalen för närvarande hello för att skapa klustret. Välj `Include Repair Manager` alternativ `Add on features` när hello klusterkonfigurationen.
![Bild för att aktivera reparera Manager från Azure-portalen](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-template"></a>Azure Resource Manager-mall
Du kan också använda hello [Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) tooenable hello reparera manager-tjänsten på nya och befintliga Service Fabric-kluster. Hämta hello mall för hello kluster som du vill toodeploy. Du kan använda hello exempelmallarna, eller så kan du skapa en anpassad mall för hanteraren för filserverresurser. 

tooenable hello reparera manager service med [Azure Resource Manager-mall](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):

1. Kontrollera först att hello `apiversion` har angetts för`2017-07-01-preview` för hello `Microsoft.ServiceFabric/clusters` resurs, som visas i följande fragment hello. Om det skiljer sig, måste du tooupdate hello `apiVersion` toohello värdet `2017-07-01-preview`:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Nu aktivera hello reparera manager-tjänsten genom att lägga till följande hello `addonFeatures` efter hello `fabricSettings` avsnitt:

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. När du har uppdaterat mallen för klustret med de här ändringarna, använda dem och låt hello uppgraderingen är klar. Du kan nu se hello reparera manager system-tjänsten körs i klustret. Den kallas `fabric:/System/RepairManagerService` under hello system tjänster i hello Service Fabric Explorer. 

### <a name="standalone-on-premises-clusters"></a>Fristående lokala kluster

Du kan använda hello [konfigurationsinställningar för Windows-kluster för fristående](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) tooenable hello reparera manager-tjänsten på nya och befintliga Service Fabric-klustret.

tooenable hello reparera manager-tjänsten:

1. Kontrollera först att hello `apiversion` i [allmänna klusterkonfigurationer](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) har angetts för`04-2017` eller senare:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. Aktivera nu reparera manager-tjänsten genom att lägga till följande hello `addonFeaturres` efter hello `fabricSettings` enligt nedan:

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. Uppdatera din klustermanifestet med dessa ändringar, med hello uppdateras klustermanifestet [skapa ett nytt kluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) eller [uppgradera hello klusterkonfigurationen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration). När hello kluster körs med uppdaterade klustermanifestet, du kan nu se hello reparera manager system-tjänsten körs i klustret, som kallas `fabric:/System/RepairManagerService`under system services-avsnittet i hello Service Fabric explorer.

### <a name="disable-automatic-windows-update-on-all-nodes"></a>Inaktivera automatisk uppdatering för Windows på alla noder

Automatiska uppdateringar kan leda tooavailability förlust eftersom flera klusternoder kan du starta om hello samma tid. hello korrigering orchestration app som standard försöker toodisable hello automatisk Windows Update på varje nod i klustret. Vi rekommenderar dock inställningen hello Windows Update-princip för ”avisering före hämtningen” uttryckligen om hello inställningarna hanteras av en administratör eller en Grupprincip.

### <a name="optional-enable-azure-diagnostics"></a>Valfritt: Aktivera Azure-diagnostik

Kluster som kör Service Fabric runtime version `5.6.220.9494` och ovan samla in korrigering orchestration app loggarna som en del av Service Fabric-loggarna.
Du kan hoppa över det här steget om klustret körs i Service Fabric runtime version `5.6.220.9494` och högre.

För kluster som kör Service Fabric runtime version mindre än `5.6.220.9494`, loggar för hello korrigering orchestration app samlas lokalt på varje hello klusternoder.
Vi rekommenderar att du konfigurerar Azure Diagnostics tooupload loggar från alla noder tooa central plats.

Information om hur du aktiverar Azure-diagnostik finns [samla in loggar med hjälp av Azure-diagnostik](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).

Loggar för hello korrigering orchestration app genereras på hello följande fasta providern ID: N:

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

I Resource Manager mallen goto `EtwEventSourceProviderConfiguration` avsnittet `WadCfg` och Lägg till följande poster hello:

```json
  {
    "provider": "e39b723c-590c-4090-abb0-11e3e6616346",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
      "eventDestination": "PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "fc0028ff-bfdc-499f-80dc-ed922c52c5e9",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "24afa313-0d3b-4c7c-b485-1047fd964b60",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "05dc046c-60e9-4ef7-965e-91660adffa68",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  }
```

> [!NOTE]
> Om din Service Fabric-klustret har flera nodtyper och sedan hello föregående avsnitt måste läggas till för alla hello `WadCfg` avsnitt.

## <a name="download-hello-app-package"></a>Hämta hello app-paket

Hämta hello program från hello [Hämta länk](https://go.microsoft.com/fwlink/P/?linkid=849590).

## <a name="configure-hello-app"></a>Konfigurera hello app

Hej hello korrigering orchestration appens beteende kan vara konfigurerade toomeet dina behov. Åsidosätta standardvärden för hello genom att passera i hello programmet parametern under skapa program eller uppdatering. Parametrar för program kan anges genom att ange `ApplicationParameter` toohello `Start-ServiceFabricApplicationUpgrade` eller `New-ServiceFabricApplication` cmdlets.

|**Parametern**        |**Typ**                          | **Detaljer**|
|:-|-|-|
|MaxResultsToCache    |Lång                              | Maximalt antal Windows Update-resultat, som ska cachelagras. <br>Standardvärdet är 3000 under förutsättning att den: <br> -Antalet noder är 20. <br> -Antalet uppdateringar som händer på en nod per månad är fem. <br> -Antalet resultat per åtgärd kan vara 10. <br> -Resultat för hello senaste tre månaderna ska lagras. |
|TaskApprovalPolicy   |Enum <br> {NodeWise, UpgradeDomainWise}                          |TaskApprovalPolicy anger hello policy som är toobe som används för uppdateringar för Windows hello Coordinator-tjänsten tooinstall hello Service Fabric-noder i klustret.<br>                         Tillåtna värden är: <br>                                                           <b>NodeWise</b>. Windows Update är installerade en nod i taget. <br>                                                           <b>UpgradeDomainWise</b>. Windows Update är installerade en domän i taget. (På hello maximala, alla hello noder som tillhör tooan uppgraderingsdomänen kan gå för Windows Update.)
|LogsDiskQuotaInMB   |Lång  <br> (Standard: 1024)               |Maximal storlek för korrigering orchestration app loggar i MB, vilket kan sparas lokalt på noder.
| WUQuery               | Sträng<br>(Standard ”: IsInstalled = 0”)                | Fråga tooget Windows-uppdateringar. Mer information finns i [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
| InstallWindowsOSOnlyUpdates | bool <br> (standard: True)                 | Den här flaggan kan Windows-operativsystem system uppdateringar toobe installerad.            |
| WUOperationTimeOutInMinutes | int <br>(Standard: 90).                   | Anger hello timeout för någon Windows Update-åtgärd (Sök eller ladda ned eller installera). Om hello-åtgärden inte slutfördes inom Hej angiven tidsgräns, den har avbrutits.       |
| WURescheduleCount     | int <br> (Standard: 5).                  | hello maximalt antal gånger hello service schemaläggs automatiskt på nytt hello Windows update om en åtgärd misslyckas så.          |
| WURescheduleTimeInMinutes | int <br>(Standard: 30). | hello intervall på vilka hello service schemaläggs automatiskt på nytt hello Windows update om felet kvarstår. |
| WUFrequency           | Kommaavgränsad sträng (standard: ”vecka, Onsdag 7:00:00”)     | hello frekvensen för att installera Windows Update. hello format och möjliga värden är: <br>-: Som mm: ss varje månad, DD, till exempel varje månad, 5, 12: 22:32. <br> -Varje vecka, dag,: mm: ss, för exempelvis varje vecka, tisdag, 12:22:32.  <br> -Varje dag,: mm: ss, till exempel dagligen, 12:22:32.  <br> -Ingen anger att Windows Update inte utföras.  <br><br> Observera att hello alltid i UTC.|
| AcceptWindowsUpdateEula | bool <br>(Standard: true) | Genom att ange den här flaggan accepterar hello programmet hello slutanvändarens License avtal för Windows Update för hello ägare av hello-datorn.              |

> [!TIP]
> Om du vill att Windows Update toohappen omedelbart, `WUFrequency` relativa toohello tidpunkten för distribution av programmet. Anta exempelvis att du har en fem-nodtest kluster och planera toodeploy hello app vid cirka 17:00:00 UTC. Om du anta att distribution eller uppgradering av programmet hello tar 30 minuter vid hello maximala, Ställ in hello WUFrequency som ”varje dag, 17:30:00”.

## <a name="deploy-hello-app"></a>Distribuera hello app

1. Slut alla hello förkraven tooprepare hello klustret.
2. Distribuera hello korrigering orchestration-appen som andra Service Fabric-app. Du kan distribuera hello appen med hjälp av PowerShell. Gör så hello i [distribuera och ta bort program med hjälp av PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).
3. tooconfigure hello-programmet vid hello i distributionen, pass hello `ApplicationParamater` toohello `New-ServiceFabricApplication` cmdlet. Vi har samlat hello skriptet Deploy.ps1 tillsammans med programmet hello för din bekvämlighet. toouse hello skript:

    - Ansluta tooa Service Fabric-kluster med hjälp av `Connect-ServiceFabricCluster`.
    - Kör hello PowerShell-skriptet Deploy.ps1 med lämpliga hello `ApplicationParameter` värde.

> [!NOTE]
> Tänk hello hello skript och hello programmappen PatchOrchestrationApplication samma katalog.

## <a name="upgrade-hello-app"></a>Uppgradera hello app

tooupgrade en befintlig korrigering orchestration-app med hjälp av PowerShell, gör hello i [uppgradering av Service Fabric-programmet med PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).

## <a name="remove-hello-app"></a>Ta bort hello appen

tooremove hello program, följ hello stegen i [distribuera och ta bort program med hjälp av PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).

Vi har samlat hello skriptet Undeploy.ps1 tillsammans med programmet hello för din bekvämlighet. toouse hello skript:

  - Ansluta tooa Service Fabric-kluster med hjälp av ```Connect-ServiceFabricCluster```.

  - Köra PowerShell-skript för hello Undeploy.ps1.

> [!NOTE]
> Tänk hello hello skript och hello programmappen PatchOrchestrationApplication samma katalog.

## <a name="view-hello-windows-update-results"></a>Visa hello Windows Update-resultat

hello korrigering orchestration app exponerar REST API: er toodisplay hello historiska resultat toohello användare. Ett exempel på hello resultatet JSON:
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2017-05-21T11:46:52.1953713Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of hello issues that are included in this update, see hello associated Microsoft Knowledge Base article. After you install this update, you may have toorestart your system.",
            "ResultCode": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```
Om ingen uppdatering har schemalagts ännu är hello resultatet JSON tom.

Logga in toohello klustret tooquery Windows Update resultat. Sedan reda på hello replik adressen för primär hello av hello Coordinator-tjänsten och tryck hello URL från hello webbläsare: http://&lt;REPLIK-IP-&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.

hello REST-slutpunkt för hello Coordinator-tjänsten har en dynamisk port. toocheck hello exakta URL finns toohello Service Fabric Explorer. Till exempel hello resultat är tillgängliga på `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.

![Bild av REST-slutpunkt](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


Om hello omvänd proxy är aktiverat på hello klustret kan du komma åt hello URL: en från utanför hello-kluster.
Hej slutpunkt som behöver toobe träffar är http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.

tooenable hello omvänd proxy på hello klustret gör hello i [omvänd proxy i Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy). 

> 
> [!WARNING]
> Alla micro tjänster i hello kluster som Exponerar en HTTP-slutpunkten är adresserbara från utanför hello kluster när hello omvänd proxy har konfigurerats.

## <a name="diagnosticshealth-events"></a>Diagnostik/hälsotillstånd händelser

### <a name="collect-patch-orchestration-app-logs"></a>Samla in korrigering orchestration app loggar

Korrigering orchestration app loggarna har samlats in som en del av Service Fabric-loggar från körningsversion `5.6.220.9494` och högre.
För kluster som kör Service Fabric runtime version mindre än `5.6.220.9494`, loggar samlas in med något av följande metoder hello.

#### <a name="locally-on-each-node"></a>Lokalt på varje nod

Loggarna samlas lokalt på varje klusternod för Service Fabric Service Fabric runtime-versionen om är mindre än `5.6.220.9494`. hello plats tooaccess hello loggar är \[Service Fabric\_Installation\_enhet\]:\\PatchOrchestrationApplication\\loggar.

Till exempel om Service Fabric är installerat på enhet D hello sökvägen är D:\\PatchOrchestrationApplication\\loggar.

#### <a name="central-location"></a>Central plats

Om Azure-diagnostik är konfigurerad som en del av nödvändiga steg, är loggar för hello korrigering orchestration app tillgängliga i Azure Storage.

### <a name="health-reports"></a>Hälsorapporter

hello korrigering orchestration app publicerar också hälsorapporter mot hello Coordinator-tjänsten eller hello nod Agent-tjänsten i hello följande fall:

#### <a name="a-windows-update-operation-failed"></a>En Windows Update-åtgärden misslyckades

Om en Windows Update-åtgärden misslyckas på en nod, skapas en hälsorapport mot hello nod Agent-tjänsten. Information om hello hälsorapport innehålla hello problematiska nodnamn.

När uppdatering har slutförts på hello problematiska nod, avmarkeras automatiskt hello rapporten.

#### <a name="hello-node-agent-ntservice-is-down"></a>hello nod Agent NTService är nere

Om hello nod Agent NTService är igång på en nod kan genereras en varning nivå hälsorapport mot hello nod Agent-tjänsten.

#### <a name="hello-repair-manager-service-is-not-enabled"></a>hello reparera manager-tjänsten har inte aktiverats

Om hello reparera manager-tjänsten inte kan hittas på hello klustret, skapas en varningsnivå hälsorapport för hello Coordinator-tjänsten.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

FRÅGOR. **Varför ser jag min kluster i ett feltillstånd när hello korrigering orchestration app körs?**

A. Under installationen hello hello korrigering orchestration app inaktiverar eller startar om noder som tillfälligt kan resultera i hello hälsotillstånd hello klustret gå.

Baserat på hello princip för hello program, antingen en nod kan gå under en uppdatering åtgärd *eller* en hel uppgraderingsdomän kan gå samtidigt.

Hello slutet av hello Windows Update-installation, hello noder är reenabled efter omstart.

I följande exempel hello, gick hello klustret tooan feltillstånd tillfälligt eftersom två noder har ned och hello MaxPercentageUnhealthNodes princip har överskridits. hello fel är tillfällig tills hello korrigering åtgärden pågår.

![Bild av feltillstånd kluster](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Om hello problemet kvarstår läser du avsnittet om felsökning toohello.

FRÅGOR. **Korrigering av orchestration appar är i varningstillstånd**

A. Kontrollera toosee om en hälsorapport bokförd mot hello program är hello grundläggande orsaken. Vanligtvis innehåller hello varning information om hello problem. Om hello problemet är tillfälligt, är hello program förväntade tooauto återskapa från det här läget.

FRÅGOR. **Vad gör jag om min klustret är i feltillstånd och toodo en brådskande operativsystemet uppdatering måste?**

A. hello korrigering orchestration appen installeras inte uppdateringar medan hello klustret är i feltillstånd. Försök toobring klustret tooa felfritt tillstånd toounblock hello korrigering orchestration app arbetsflödet.

FRÅGOR. **Varför korrigering genom datorkluster tar så lång tid toorun?**

A. hello-tid som krävs av hello korrigering orchestration app är främst beroende hello följande faktorer:

- hello princip hello Coordinator-tjänsten. 
  - Hej standardprincipen `NodeWise`, resulterar i korrigering endast en nod i taget. Särskilt i hello fallet med större kluster, rekommenderar vi att du använder hello `UpgradeDomainWise` princip tooachieve snabbare korrigering genom datorkluster.
- hello antalet uppdateringar som är tillgängliga för hämtning och installation. 
- Hej toodownload Genomsnittlig tid som krävs och installerar en uppdatering som inte får överstiga några timmar.
- Hej hello VM prestanda och bandbredd.

FRÅGOR. **Varför ser vissa uppdateringar i Windows Update resultaten via REST-api, men inte under hello Windows Update-historiken på datorn?**

A. Vissa produktuppdateringar måste toobe checkats in deras respektive uppdatering/korrigering historik. Exempel: Uppdateringar för Windows Defender inte visas i Windows Update-historiken på Windows Server 2016.

## <a name="disclaimers"></a>Ansvarsfriskrivningar

- hello korrigering orchestration app accepterar hello slutanvändarens License avtal för Windows Update för hello användares räkning. Du kan också kan hello inställningen inaktiveras i hello konfigurationen av programmet hello.

- hello korrigering orchestration appen samlar in telemetri tootrack användnings- och prestandadata. Hej programtelemetri följer hello inställningen hello Service Fabric runtime telemetri inställning (som är aktiverad som standard).

## <a name="troubleshooting"></a>Felsökning

### <a name="a-node-is-not-coming-back-tooup-state"></a>En nod kommer inte tillbaka tooup tillstånd

**hello nod kan ha fastnat i tillståndet inaktivera eftersom**:

En säkerhets-kontroll väntar. tooremedy den här situationen kan se till att tillräckligt många noder är tillgängliga i felfritt tillstånd.

**hello nod kan ha fastnat i ett inaktiverat tillstånd eftersom**:

- hello-nod har inaktiverats manuellt.
- hello-nod har inaktiverats på grund av tooan pågående Azure-infrastrukturen jobb.
- hello-nod har inaktiverats tillfälligt av hello korrigering orchestration app toopatch hello nod.

**hello nod kan ha fastnat i ned-läge eftersom**:

- hello nod placerades i tillståndet nedåt manuellt.
- hello nod genomgår en omstart (som kan utlösas av hello korrigering orchestration app).
- hello nod förfaller tooa felaktiga VM eller datorn eller nätverket anslutningsproblem.

### <a name="updates-were-skipped-on-some-nodes"></a>Uppdateringar hoppades över på vissa noder

hello korrigering orchestration app försöker tooinstall en Windows update bl.a toohello omplanering princip. hello-tjänsten försöker toorecover hello nod och hoppa över hello update bl.a toohello princip för program.

I sådana fall genereras en varning nivå hälsorapport mot hello nod Agent-tjänsten. hello resultat för Windows Update innehåller också hello möjlig anledning till hello-fel.

### <a name="hello-health-of-hello-cluster-goes-tooerror-while-hello-update-installs"></a>hello hälsotillstånd hello klustret blir tooerror medan hello uppdateringen har installerats

En felaktig Windows update kan skada hello hälsotillståndet för ett program eller ett kluster på en viss nod eller en domän. hello korrigering orchestration app upphör alla efterföljande Windows Update-åtgärden tills hello klustret är felfri igen.

En administratör måste ingripa och ta reda på varför programmet hello eller kluster fick dåligt hälsotillstånd på grund av tooWindows uppdatering.

## <a name="release-notes-"></a>Viktig information:

### <a name="version-110"></a>Version 1.1.0
- Offentliga utgåvan

### <a name="version-111"></a>Version 1.1.1
- Fast ett programfel i SetupEntryPoint av NodeAgentService som förhindrade installationen av NodeAgentNTService.

### <a name="version-120-latest"></a>Version 1.2.0 (senaste)

- Felkorrigeringar runt system starta om arbetsflödet.
- Buggfix vid skapande av RM uppgifter på grund av toowhich hälsokontroll under förberedelserna reparera uppgifter händer inte som förväntat.
- Ändrade hello startläge för windows-tjänst POANodeSvc från automatisk toodelayed-automatiskt.
