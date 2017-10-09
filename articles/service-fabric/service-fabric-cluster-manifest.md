---
title: "aaaConfigure din fristående Azure Service Fabric-kluster | Microsoft Docs"
description: "Lär dig hur tooconfigure din fristående eller privata Service Fabric-klustret."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dekapur
ms.openlocfilehash: ce2ad387162a05668bbd3a271c754776fe471850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuration-settings-for-standalone-windows-cluster"></a>Konfigurationsinställningar för fristående Windows-kluster
Den här artikeln beskriver hur ett fristående Service Fabric med tooconfigure hello ***ClusterConfig.JSON*** fil. Du kan använda den här filen toospecify information, till exempel hello Service Fabric-noder och deras IP-adresser, olika typer av noder på hello kluster, hello säkerhetskonfigurationer samt hello nätverkstopologi vad gäller fel/uppgradera domäner för din fristående kluster.

När du [hämta hello fristående Service Fabric-paket](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), några exempel på hello ClusterConfig.JSON filen är hämtade tooyour arbete datorn. hello prov med *DevCluster* i sina namn kan du skapa ett kluster med alla tre noder på hello detsamma datorn som logiska noder. Minst en nod måste vara markerad som en primär nod utanför dessa. Det här klustret är användbar för en miljö för utveckling eller testning och kan inte användas som ett produktionskluster. hello prov med *MultiMachine* i sina namn kan du skapa ett produktionskluster kvalitet, med varje nod på en separat dator.

1. *ClusterConfig.Unsecure.DevCluster.JSON* och *ClusterConfig.Unsecure.MultiMachine.JSON* visar hur toocreate ett oskyddat test eller produktion kluster respektive. 
2. *ClusterConfig.Windows.DevCluster.JSON* och *ClusterConfig.Windows.MultiMachine.JSON* visa hur toocreate test- eller klustret skyddas med [Windows-säkerhet](service-fabric-windows-cluster-windows-security.md).
3. *ClusterConfig.X509.DevCluster.JSON* och *ClusterConfig.X509.MultiMachine.JSON* visa hur toocreate test- eller klustret skyddas med [X509 certifikatbaserad säkerhet](service-fabric-windows-cluster-x509-security.md). 

Nu kommer vi att undersöka hello olika delar av en ***ClusterConfig.JSON*** filen enligt nedan.

## <a name="general-cluster-configurations"></a>Allmän klusterkonfigurationer
Detta omfattar hello bred klustret specifika konfigurationer som visas i hello JSON kodfragmentet nedan.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "01-2017",

Du kan ge något eget namn tooyour Service Fabric-kluster genom att tilldela den toohello **namn** variabeln. Hej **clusterConfigurationVersion** är hello versionsnumret för klustret, bör du öka den varje gång du uppgraderar Service Fabric-klustret. Dock bör du lämna hello **apiVersion** toohello standardvärdet.

<a id="clusternodes"></a>

## <a name="nodes-on-hello-cluster"></a>Noder i klustret hello
Du kan konfigurera hello noder på Service Fabric-kluster med hjälp av hello **noder** avsnitt som hello följande fragment visas.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Ett Service Fabric-kluster måste innehålla minst 3 noder. Du kan lägga till fler noder toothis avsnittet enligt din konfiguration. hello i den följande tabellen beskrivs hello konfigurationsinställningarna för varje nod.

| **Nodkonfiguration** | **Beskrivning** |
| --- | --- |
| Nodnamn |Du kan ge någon toohello nod för eget namn. |
| IP-adress |Ta reda på hello IP-adressen för noden genom att öppna ett kommandofönster och skriva `ipconfig`. Anteckna hello IPV4-adress och tilldela den toohello **iPAddress** variabeln. |
| nodeTypeRef |Varje nod kan tilldelas en annan nod-typen. Hej [nodtyper](#nodetypes) definieras i hello nedan. |
| faultDomain |Fel domäner aktivera administratörer toodefine hello fysiska noder som kan misslyckas på hello samma tid på grund av tooshared fysiska beroenden. |
| upgradeDomain |Uppgraderingsdomäner beskriver uppsättningar med noder som är avstängd för Service Fabric uppgraderas på om hello samma tid. Du kan välja vilka noder tooassign toowhich uppgraderingsdomäner, som inte begränsas av alla fysiska krav. |

## <a name="cluster-properties"></a>Egenskaper för klustret
Hej **egenskaper** avsnitt i hello ClusterConfig.JSON är används tooconfigure hello klustret på följande sätt.

<a id="reliability"></a>

### <a name="reliability"></a>Tillförlitlighet
hello begreppet **reliabilityLevel** definierar hello antal repliker eller instanser av hello Service Fabric-systemtjänster som kan köras på hello primära klusternoder hello. Den avgör hello tillförlitlighet för dessa tjänster och därför hello klustret. hello-värdet är beräknade hello systemet när klustret skapas och uppgradering.

### <a name="diagnostics"></a>Diagnostik
Hej **diagnosticsStore** avsnittet kan du tooconfigure parametrar tooenable diagnostik och felsökning nod eller fel i kluster, enligt följande kodavsnitt hello. 

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

Hej **metadata** är en beskrivning av kluster-diagnostik och kan anges enligt din konfiguration. Dessa variabler bidra till att samla in ETW spårningsloggar, krascher Dumpar samt prestandaräknare. Läs [spårloggen finns det](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) och [ETW-spårning](https://msdn.microsoft.com/library/ms751538.aspx) för mer information om ETW spårningsloggar. Alla loggar inklusive [krascher Dumpar](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) och [prestandaräknare](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) kan vara riktad toohello **connectionString** mapp på din dator. Du kan också använda *AzureStorage* för att lagra diagnostik. Nedan visas ett exempel fragment.

    "diagnosticsStore": {
        "metadata":  "Please replace hello diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Säkerhet
Hej **säkerhet** avsnittet krävs för en säker fristående Service Fabric-klustret. hello följande fragment visas en del av det här avsnittet.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

Hej **metadata** är en beskrivning av säker klustret och kan anges enligt din konfiguration. Hej **ClusterCredentialType** och **ServerCredentialType** bestämmer hello typ av säkerhet som implementerar hello kluster och hello noder. Du kan välja tooeither *X509* för en certifikatbaserad säkerhet eller *Windows* för ett Azure Active Directory-baserad säkerhet. Hej resten av hello **säkerhet** avsnittet baseras på hello typ av hello säkerhet. Läs [certifikat baserade säkerhet i ett fristående kluster](service-fabric-windows-cluster-x509-security.md) eller [Windows-säkerhet i ett kluster med fristående](service-fabric-windows-cluster-windows-security.md) information om hur toofill ut hello resten av hello **säkerhet**avsnitt.

<a id="nodetypes"></a>

### <a name="node-types"></a>Nodtyper
Hej **nodetypes får** beskrivs hello typ av hello-noder som klustret har. Minst en nodtyp måste anges för ett kluster, enligt hello kodfragmentet nedan. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

Hej **namn** är hello eget namn för den här typen av viss nod. toocreate en nod i den här nodtypen tilldela sitt eget namn toohello **nodeTypeRef** variabel för noden, som tidigare nämnts [ovan](#clusternodes). Definiera hello anslutningens slutpunkter som ska användas för varje nodtyp. Du kan välja ett annat portnummer för dessa slutpunkter för anslutningen, så länge som de inte står i konflikt med andra slutpunkter i det här klustret. I ett kluster med flera noder finns en eller flera primära noder (d.v.s. **isPrimary** ställa in också*SANT*), beroende på hello [ **reliabilityLevel** ](#reliability). Läs [Service Fabric-kluster kapacitetsplaneringsöverväganden](service-fabric-cluster-capacity.md) information om **nodetypes får** och **reliabilityLevel**, och tooknow vad är primär och hello icke-primär nodtyper. 

#### <a name="endpoints-used-tooconfigure-hello-node-types"></a>Slutpunkter används tooconfigure hello nodtyper
* *clientConnectionEndpointPort* hello-porten som används av hello klient tooconnect toohello kluster när du använder hello-klientens API: er. 
* *clusterConnectionEndpointPort* är hello port där hello noder kommunicerar med varandra.
* *leaseDriverEndpointPort* hello-porten som används av klustret hello lån drivrutinen toofind ut om hello noder som fortfarande är aktiva. 
* *serviceConnectionEndpointPort* hello-port som används av hello program och tjänster som distribueras på en nod, toocommunicate med hello Service Fabric-klienten på just den noden.
* *httpGatewayEndpointPort* hello-porten som används av hello Service Fabric Explorer tooconnect toohello klustret.
* *ephemeralPorts* åsidosätta hello [dynamiska portar som används av hello OS](https://support.microsoft.com/kb/929851). Service Fabric använder en del av dessa som programmet portar och hello återstående blir tillgänglig för hello OS. Den kommer även mappa det här intervallet toohello befintligt intervall finns i hello OS, så du kan använda hello-intervall som anges i hello exempelfiler JSON för alla ändamål. Du måste toomake att hello skillnaden mellan hello start- och hello portar är minst 255. Om denna skillnad är för låg eftersom det här intervallet delas med hello operativsystem kan du stöta på konflikter. Se hello konfigureras dynamiskt portintervall genom att köra `netsh int ipv4 show dynamicport tcp`.
* *applicationPorts* är hello-portar som används av hello Service Fabric-program. portintervall för hello programmet bör vara tillräckligt stor toocover hello endpoint krav för dina program. Det här intervallet ska vara exklusiv från hello dynamiskt portintervall på hello datorn, d.v.s. hello *ephemeralPorts* intervall som angetts i hello konfiguration.  Service Fabric använda dessa när nya portarna som krävs, samt ta hand om öppna hello-brandväggen för dessa portar. 
* *reverseProxyEndpointPort* är valfria omvänd proxy slutpunkt. Se [Service Fabric omvänd Proxy](service-fabric-reverseproxy.md) för mer information. 

### <a name="log-settings"></a>Logginställningar
Hej **fabricSettings** avsnittet kan du tooset hello rotkataloger för hello Service Fabric-data och loggfiler. Du kan anpassa dessa endast när hello inledande klustret skapas. Nedan visas ett exempel fragment av det här avsnittet.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Vi rekommenderar att du använder en icke-OS-enhet som hello FabricDataRoot och FabricLogRoot eftersom det ger högre tillförlitlighet mot OS kraschar. Observera att om du har ändrat endast hello dataroten sedan hello loggen rot placeras på en nivå under hello data rot.

### <a name="stateful-reliable-service-settings"></a>Tillståndskänsliga Reliable tjänstinställningar
Hej **KtlLogger** avsnittet kan du tooset hello konfigurationsinställningar för Reliable Services. Mer information om dessa inställningar finns [konfigurera tillståndskänsliga reliable services](service-fabric-reliable-services-configuration.md).
hello exemplet nedan visar hur skapade toochange hello hello delade transaktionsloggen som hämtar tooback några tillförlitliga samlingar för tillståndskänsliga tjänster.

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="add-on-features"></a>Tilläggsfunktioner
tooconfigure tilläggsfunktioner hello apiVersion måste vara konfigurerade som ”04-2017' eller högre och addonFeatures behöver toobe konfigureras:

    "apiVersion": "04-2017",
    "properties": {
      "addOnFeatures": [
          "DnsService",
          "RepairManager"
      ]
    }

### <a name="container-support"></a>Stöd för behållaren
tooenable behållaren stöd för både windows server-behållare och hyper-v-behållare för fristående kluster, hello 'DnsService'-tillägg-funktionen måste toobe aktiverat.


## <a name="next-steps"></a>Nästa steg
När du har en hel ClusterConfig.JSON-fil som är konfigurerade enligt din fristående konfiguration kan du distribuera klustret med följande hello artikel [skapa ett fristående Service Fabric-kluster](service-fabric-cluster-creation-for-windows-server.md) och gå sedan vidare för[visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

