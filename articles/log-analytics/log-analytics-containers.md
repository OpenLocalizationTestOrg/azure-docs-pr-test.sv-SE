---
title: "aaaContainer lösning för övervakning i Azure Log Analytics | Microsoft Docs"
description: "hello behållaren övervakning lösning i logganalys hjälper dig se och hantera Docker- och Windows behållaren värdar i en enda plats."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte;banders
ms.openlocfilehash: 2eed1dd81c22faef78a375fca3ebece9e5300c09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="container-monitoring-solution-in-log-analytics"></a>Lösning för övervakning av behållare i logganalys

![Behållare symbol](./media/log-analytics-containers/containers-symbol.png)

Den här artikeln beskriver hur tooset in och använda hello behållaren övervakning lösning i logganalys, vilket gör det möjligt att visa och hantera Docker- och Windows behållaren värdar i en enda plats. Docker är ett system för virtualisering av programvara används toocreate behållare som automatiserar software distribution tootheir IT infrastruktur.

Hej lösning visar vilka behållare som körs, vilka behållaren image som de körs och där behållare körs. Du kan visa detaljerad granskningsinformation visar kommandon som används i behållare. Och du kan felsöka behållare genom att visa och söka centraliserad loggar utan tooremotely vyn Docker eller Windows-värdar. Du kan hitta behållare som kan vara mycket brus och mycket överflödiga resurser på en värd. Och du kan visa centraliserad CPU, minne, lagring och nätverksinformation om användning och prestanda för behållare. På datorer som kör Windows, kan du centralisera och jämför loggar från Windows Server Hyper-V och Docker-behållare. hello-lösningen stöder följande behållare orchestrators hello:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift


hello visar följande diagram hello relationer mellan olika värdar för behållaren och agenter med OMS.

![Behållare diagram](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a>Systemkrav

Granska hello följande information tooverify hello förutsättningar vara uppfyllda innan du börjar.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Behållaren övervakningslösning som stöd för Docker Orchestrator och OS-plattformen
hello hello följande tabell beskrivs Docker orchestration och operativsystem som stöd för behållaren inventering, prestanda och loggar med logganalys övervakning.   

| | ACS | Linux | Windows | Behållare<br>Inventering | Bild<br>Inventering | Node<br>Inventering | Behållare<br>Prestanda | Behållare<br>Händelse | Händelse<br>Logga | Behållare<br>Logga |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Kubernetes | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Mesosphere<br>DC/OS | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; |
| Docker<br>Swarm | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Tjänst<br>Fabric | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Red Hat öppna<br>SKIFT | | &#8226; | | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; | | &#8226; |
| Windows Server<br>(fristående) | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Linux-server<br>(fristående) | | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |


### <a name="docker-versions-supported-on-linux"></a>Docker-versioner som stöds på Linux

- Docker 1.11 too1.13
- Docker CE och EE v17.06

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>x64 Linux-distributioner som stöds som behållare värdar

- Ubuntu 14.04 LTS och 16.04 LTS
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE LEAP 42.2
- CentOS 7.2 och 7.3
- SLES 12
- RHEL 7.2 och 7.3
- Red Hat OpenShift behållare plattform (OCP) 3.4 och 3.5
- ACS Mesosphere DC/OS 1.7.3 too1.8.8
- ACS-Kubernetes 1.4.5 too1.6
- ACS-Docker Swarm

### <a name="supported-windows-operating-system"></a>Windows-operativsystem som stöds

- Windows Server 2016
- Windows 10 årsdagar Edition (Professional eller Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Docker-versioner som stöds i Windows

- Docker 1.12 och 1.13
- Docker 17.03.0 och senare

## <a name="installing-and-configuring-hello-solution"></a>Installera och konfigurera hello lösning
Använd följande information tooinstall hello och konfigurera hello lösning.

1. Lägg till hello behållaren övervakning lösning tooyour OMS-arbetsyta från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) eller genom att använda hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).

2. Installera och använda Docker med en OMS-agent.  Baserat på ditt operativsystem kan välja du mellan hello följande metoder:

  * Installera på Linux operativsystem som stöds och kör Docker och sedan installera och konfigurera hello [OMS-Agent för Linux](log-analytics-agent-linux.md).  
  * Virtuell CoreOS, det går inte att du kör hello OMS-Agent för Linux. I stället kan du köra en av version av hello OMS-Agent för Linux. Granska [Linux behållaren värdar inklusive virtuell CoreOS](#for-all-linux-container-hosts-including-coreos) eller [Azure Government Linux behållaren värdar inklusive virtuell CoreOS](#for-all-azure-government-linux-container-hosts-including-coreos) om du arbetar med behållare i Azure Government-molnet.
  * Installera hello Docker-motorn och klienten på Windows Server 2016 och Windows 10, och sedan ansluta agenten toogather information och skicka det tooLog Analytics.  

### <a name="container-services"></a>Behållartjänster

- Om du har en miljö med Red Hat OpenShift granska [konfigurera en OMS-agent för Red Hat OpenShift](#configure-an-oms-agent-for-red-hat-openshift).
- Om du har ett Kubernetes-kluster med hello Azure Container Service granska [övervaka ett Azure Container Service-kluster med Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).
- Om du har ett Azure Container Service DC/OS-kluster, mer information finns i [övervaka ett Azure Container Service DC/OS-kluster med Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).
- Om du har en miljö med Docker Swarm läge, mer information finns i [konfigurera en OMS-agent för Docker Swarm](#configure-an-oms-agent-for-docker-swarm).
- Om du använder behållare med Service Fabric mer information finns i [översikt av Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- Granska hello [Docker-motorn på Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) artikel för ytterligare information om hur tooinstall och konfigurera Docker-motorer på Windows-datorer.

> [!IMPORTANT]
> Docker måste köras **innan** du installerar hello [OMS-Agent för Linux](log-analytics-agent-linux.md) på behållaren-värdar. Om du redan har installerat hello agenten innan du installerar Docker måste tooreinstall hello OMS-Agent för Linux. Mer information om Docker finns hello [Docker webbplats](https://www.docker.com).


## <a name="linux-container-hosts"></a>Linux-behållaren värdar

När du har installerat Docker använder du följande inställningar för din behållaren tooconfigure hello värdagenten för användning med Docker hello. Du måste först din OMS arbetsyte-ID och nyckel som du hittar i hello Azure-portalen. I arbetsytan, klickar du på **Snabbstart** > **datorer** tooview din **arbetsyte-ID** och **primärnyckel**.  Kopiera och klistra in både i din favorit-redigeraren.

### <a name="for-all-linux-container-hosts-except-coreos"></a>För alla Linux-behållaren värdar utom virtuell CoreOS

- Mer information och anvisningar om hur tooinstall hello OMS-Agent för Linux finns [ansluta din Linux-datorer tooOperations Management Suite (OMS)](log-analytics-agent-linux.md).

### <a name="for-all-linux-container-hosts-including-coreos"></a>För alla värdar för Linux behållare inklusive virtuell CoreOS

Starta hello OMS-behållare som du vill toomonitor. Ändra och använda hello följande exempel:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a>För alla värdar för Azure Government Linux behållare inklusive virtuell CoreOS

Starta hello OMS-behållare som du vill toomonitor. Ändra och använda hello följande exempel:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-linux-agent-tooone-in-a-container"></a>Växlar från att använda en installerade Linux-agenten tooone i en behållare
Om du tidigare använt hello direkt installerad agent och vill ta tooinstead används en agent som körs i en behållare, måste du först bort hello OMS-Agent för Linux. Se [avinstallerar hello OMS-Agent för Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux) toounderstand hur toosuccessfully avinstallera hello agent.  

### <a name="configure-an-oms-agent-for-docker-swarm"></a>Konfigurera en OMS-agent för Docker Swarm

Du kan köra hello OMS-Agent som en global tjänst på Docker Swarm. Använd följande information toocreate OMS-agenttjänsten hello. Du behöver tooinsert din OMS arbetsyte-ID och primärnyckel.

- Kör följande hello på hello huvudnod.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="configure-an-oms-agent-for-red-hat-openshift"></a>Konfigurera en OMS-Agent för Red Hat OpenShift
Det finns tre sätt tooadd hello OMS-Agent tooRed Hat OpenShift toostart att samla in behållaren övervakningsdata.

* [Installera hello OMS-Agent för Linux](log-analytics-agent-linux.md) direkt på varje nod i OpenShift  
* [Aktivera Log Analytics VM-tillägget](log-analytics-azure-vm-extension.md) på varje nod för OpenShift som finns i Azure  
* Installera hello OMS-Agent som en OpenShift daemon-uppsättning  

I det här avsnittet beskriver vi hello steg krävs tooinstall hello OMS-Agent som en OpenShift daemon-uppsättning.  

1. Logga in toohello OpenShift nod och kopiera hello yaml huvudfilen [ocp-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) från GitHub tooyour nod och ändra hello värdet med din OMS arbetsyte-ID och primärnyckel.
2. Kör följande kommandon toocreate hello ett projekt för OMS och ange hello användarkonto.

    ```
    oadm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. toodeploy hello daemon-mängd kör hello följande:

    `oc create -f ocp-omsagent.yaml`

5. tooverify som den är konfigurerad och fungerar korrekt, skriver du hello följande:

    `oc describe daemonset omsagent`  

    och hello utdata bör likna:

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

Om du vill toouse hemligheter toosecure din OMS arbetsyte-ID och primärnyckel när du använder hello OMS-Agent daemon set yaml-fil, utföra hello följande steg.

1. Logga in toohello OpenShift nod och kopiera hello yaml huvudfilen [ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) och hemlighet genererar skript [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) från GitHub.  Det här skriptet genererar hello hemligheter yaml-filen för OMS arbetsyte-ID och primärnyckel toosecure din secrete information.  
2. Kör följande kommandon toocreate hello ett projekt för OMS och ange hello användarkonto. hello hemlighet genererar skript begär din OMS arbetsyte-ID <WSID> och primärnyckel <KEY> och efter slutförande, skapar hello ocp-secret.yaml fil.  

    ```
    oadm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oadm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

4. Distribuera hello hemliga fil genom att köra hello följande:

    `oc create -f ocp-secret.yaml`

5. Kontrollera distributionen genom att köra hello följande:

    `oc describe secret omsagent-secret`  

    och hello utdata bör likna:  

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

6. Distribuera hello OMS-Agent daemon set yaml fil genom att köra hello följande:

    `oc create -f ocp-ds-omsagent.yaml`  

7. Kontrollera distributionen genom att köra hello följande:

    `oc describe ds oms`

    och hello utdata bör likna:

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  

    Type:   Opaque  

     Data  
     ====  
     KEY:    89 bytes  
     WSID:   37 bytes  
    ```

### <a name="secure-your-secret-information-for-docker-swarm-and-kubernetes"></a>Skydda din hemliga information för Docker Swarm och Kubernetes

Du kan skydda din hemliga OMS arbetsyte-ID och primärnycklar för Docker Swarm och Kubernetes behållartjänster.

#### <a name="secure-secrets-for-docker-swarm"></a>Säker hemligheter för Docker Swarm

För Docker Swarm när hello hemligheten för arbetsyte-ID och primärnyckel har skapats kan du köra hello skapa hello Docker-tjänst för OMSagent. Använd följande information toocreate hello din hemliga information.

1. Kör följande hello på hello huvudnod.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Kontrollera att hemligheter har skapats korrekt.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Container kör hello efter kommandot toomount hello hemligheter toohello OMS-Agent.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a>Säker hemligheter för Kubernetes med yaml-filer

För Kubernetes använder du en toogenerate hello hemligheter yaml-skriptfil för ditt arbetsyte-ID och primärnyckel. Vid hello [OMS Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) , finns på filer som du kan använda med eller utan din hemliga information.

- Hej DaemonSet för standard OMS-Agent har inte hemlig information (omsagent.yaml)
- hello OMS-agenten DaemonSet yaml filen använder hemlig information (omsagent-ds-secrets.yaml) med hemliga skript toogenerate hello hemligheter yaml (omsagentsecret.yaml)-fil för generation.

Du kan välja toocreate omsagent DaemonSets med eller utan hemligheter.

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a>Standardfilen OMSagent DaemonSet yaml utan hemligheter

- För hello OMS-agenten DaemonSet yaml standardfil, ersätter du hello `<WSID>` och `<KEY>` tooyour WSID och nyckel. Kopiera filen hello tooyour huvudnod och kör hello följande:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a>Standardfilen OMSagent DaemonSet yaml med hemligheter

1. toouse OMS-agenten DaemonSet med hemliga information, skapa hello hemligheter först.
    1. Kopiera hello skript och hemliga mallfilen och kontrollera att de är på hello samma katalog.
        - Hemligt genererar skript - hemlighet gen.sh
        - Hemlig mall - hemlighet template.yaml
    2. Kör hello skript som hello följande exempel. hello skriptet begär hello OMS arbetsyte-ID och primärnyckel och när du har angett dem hello skriptet skapar en hemlig yaml-fil så att du kan köra den.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Skapa hello hemligheter baljor genom att köra hello följande:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. tooverify kör hello följande:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        Resultatet bör likna:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        Resultatet bör likna:

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Skapa din omsagent daemon set genom att köra``` sudo kubectl create -f omsagent-ds-secrets.yaml ```

2. Kontrollera att hello OMS-agenten DaemonSet körs liknande toohello följande:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Använd en toogenerate hello hemligheter yaml-skriptfil för arbetsyte-ID och primärnyckel för Kubernetes. Använd hello följande exempelinformationen med hello [omsagent yaml filen](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) toosecure din hemliga information.

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

## <a name="windows-container-hosts"></a>Värdar för Windows-behållare

### <a name="preparation-before-installing-windows-agents"></a>Förberedelser innan du installerar Windows-agenter

Innan du installerar agenter på datorer som kör Windows, behöver du tooconfigure hello Docker-tjänst. hello konfiguration kan hello Windows-agenten eller hello logganalys virtuella tillägget toouse hello Docker TCP socket så att hello agenter kan fjärransluta till Docker-daemon för hello och toocapture data för övervakning.

#### <a name="toostart-docker-and-verify-its-configuration"></a>toostart Docker och verifiera konfigurationen

Det finns steg som krävs för tooset in TCP namngiven pipe för Windows Server:

1. Aktivera TCP pipe och namngiven pipe i Windows PowerShell.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. Konfigurera Docker med hello konfigurationsfilen för TCP pipe och namngiven pipe. hello-konfigurationsfilen finns på C:\ProgramData\docker\config\daemon.json.

    Hej daemon.json filen måste hello följande:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Mer information om hello Docker daemon konfiguration används med Windows-behållare finns [Docker-motorn på Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


### <a name="install-windows-agents"></a>Installera Windows-agenter

tooenable Windows och Hyper-V-behållare som övervakning, installera hello Microsoft Monitoring Agent (MMA) på Windows-datorer som är värdar för behållaren. Windows-datorer i din lokala miljö, se [ansluta Windows-datorer tooLog Analytics](log-analytics-windows-agents.md). För virtuella datorer körs i Azure och Anslut dem tooLog Analytics med hjälp av hello [tillägg för virtuell dator](log-analytics-azure-vm-extension.md).

Du kan övervaka Windows-behållare som körs på Service Fabric. Dock endast [virtuella datorer som körs i Azure](log-analytics-azure-vm-extension.md) och [Windows-datorer i din lokala miljö](log-analytics-windows-agents.md) stöds för närvarande för Service Fabric.

Du kan kontrollera att hello behållaren övervakning är korrekt inställda för Windows. toocheck om hello management pack var download korrekt, leta efter *ContainerManagement.xxx*. hello filer måste vara i hello C:\Program Files\Microsoft övervakning Agent\Agent\Health State\Management servicepack mapp.


## <a name="solution-components"></a>Lösningskomponenter

Om du använder Windows-agenter är sedan hello följande hanteringspaket installerat på varje dator med en agent när du lägger till den här lösningen. Ingen konfiguration eller underhåll krävs för hello management pack.

- *ContainerManagement.xxx* installerats i C:\Program Files\Microsoft övervakning Agent\Agent\Health State\Management servicepack

## <a name="container-data-collection-details"></a>Behållaren information för samlingen
hello behållaren övervakning samlar in olika mått och logga prestandadata från behållaren värdar och behållare med agenter som du aktiverar.

Data samlas in varje tre minuter av hello följande typer av agenten.

- [OMS-Agent för Linux](log-analytics-linux-agents.md)
- [Windows-agenten](log-analytics-windows-agents.md)
- [Log Analytics VM-tillägget](log-analytics-azure-vm-extension.md)


### <a name="container-records"></a>Behållaren innehåller

hello visas följande tabell exempel på poster som samlas in av hello behållaren övervakning lösningen och hello-datatyper som visas i sökresultaten för loggen.

| Datatyp | Datatypen i loggen Sök | Fält |
| --- | --- | --- |
| Prestanda för värdar och -behållare | `Type=Perf` | % Processortid för datorn, objektnamn, CounterName &#40; disken läser MB, Disk skriver MB minne användning MB nätverket tar emot byte, nätverket skicka byte, Processor användning sek nätverket &#41; CounterValue, TimeGenerated, räknarsökväg, SourceSystem |
| Behållaren inventering | `Type=ContainerInventory` | TimeGenerated, dator, behållarnamn ContainerHostname, bild, ImageTag, ContainerState, ExitCode, EnvironmentVar, kommandot, CreatedTime, StartedTime, FinishedTime, SourceSystem, behållar-ID, ImageID |
| Behållaren image inventering | `Type=ContainerImageInventory` | TimeGenerated, dator, bild, ImageTag, ImageSize, VirtualSize, körs, pausas, stoppas, misslyckades, SourceSystem, ImageID, TotalContainer |
| Behållaren logg | `Type=ContainerLog` | TimeGenerated, dator, avbildnings-ID, behållarnamn LogEntrySource, LogEntry, SourceSystem, behållar-ID |
| Behållaren loggfiler | `Type=ContainerServiceLog`  | TimeGenerated, dator, TimeOfCommand, bild, kommandot, SourceSystem, behållar-ID |
| Behållaren nod inventering | `Type=ContainerNodeInventory_CL`| TimeGenerated, dator, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Kubernetes inventering | `Type=KubePodInventory_CL` | TimeGenerated, dator, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, SourceSystem |
| Behållaren process | `Type=ContainerProcess_CL` | TimeGenerated, dator, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Kubernetes händelser | `Type=KubeEvents_CL` | TimeGenerated, dator, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, SourceSystem, meddelande |

Etiketter för läggs*PodLabel* datatyper är egna etiketter. hello är tillagda PodLabel etiketter hello tabell exempel. Därför `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` kommer skiljer sig åt i din miljö datauppsättning och allmänt likna `PodLabel_yourlabel_s`.


## <a name="monitor-containers"></a>Övervakaren behållare
När du har hello lösning aktiverat i hello OMS-portalen hello **behållare** innehåller översiktlig information om dina behållaren värdar och hello-behållare som körs på värdar.

![Behållare sida vid sida](./media/log-analytics-containers/containers-title.png)

hello innehåller en översikt över hur många behållare som du har i hello-miljö och om de är misslyckades, igång eller stoppad.

### <a name="using-hello-containers-dashboard"></a>Med hjälp av instrumentpanelen för hello-behållare
Klicka på hello **behållare** panelen. Därifrån ser du vyer som är ordnad efter:

- **Behållaren händelser** – visar status för behållaren och datorer med misslyckade behållare.
- **Loggar för behållaren** – visar ett diagram över behållaren loggfiler som skapas med tiden och en lista med datorer med hello högsta antal loggfiler.
- **Kubernetes händelser** -visas ett diagram över Kubernetes händelser som genererats med tiden och en lista över hello orsaker varför skida genererat hello händelser. *Den här datamängden används endast i Linux-miljöer.*
- **Kubernetes Namespace inventering** – visar hello antalet namnområden och skida och deras hierarki. *Den här datamängden används endast i Linux-miljöer.*
- **Behållaren nod inventering** -visar hello antalet orchestration-typer som används på behållaren noder/värdar. hello datorn noder/värdarna visas också av hello antal behållare. *Den här datamängden används endast i Linux-miljöer.*
- **Behållaren bilder inventering** -visar hello Totalt antal behållare bilder som används och antalet bildtyper. hello antalet avbildningar som också anges med hello bildtagg.
- **Behållare Status** -visar hello Totalt antal behållare noder/värddatorer behållare som körs. Datorer visas också av hello antalet värdar som körs.
- **Behållaren processen** – visar ett linjediagram för behållaren processer som körs över tid. Behållare visas också genom att köra kommandot/processen i behållare. *Den här datamängden används endast i Linux-miljöer.*
- **Behållaren CPU-prestanda** – visar ett linjediagram av hello genomsnittliga processoranvändningen över tid för datorn är noder/värd. Även baserat visar hello datorn noder/värdar på Genomsnittlig CPU-användning.
- **Behållaren minnesprestanda** – visar ett linjediagram minnesanvändningen över tid. Visar också minne användning baserat på instansnamn.
- **Datorprestanda** -visar linjediagram hello procent av CPU-prestanda över tiden, procent av minnesanvändning över tid och MB ledigt diskutrymme över tid. Du kan hovra över en rad i en diagrammet tooview mer information.


Varje område av hello instrumentpanelen är en bild av en sökning som körs på insamlade data.

![Behållare instrumentpanelen](./media/log-analytics-containers/containers-dash01.png)

![Behållare instrumentpanelen](./media/log-analytics-containers/containers-dash02.png)

I hello **behållare Status** området klickar du på hello ytan, enligt nedan.

![Behållare status](./media/log-analytics-containers/containers-status.png)

Sök i loggfilen öppnas och visar information om hello tillstånd för en behållare.

![Logga Sök efter behållare](./media/log-analytics-containers/containers-log-search.png)

Härifrån kan du redigera hello Sök frågan toomodify den toofind hello specifik information du är intresserad av. Mer information om loggen sökningar finns [logga sökningar i logganalys](log-analytics-log-searches.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Felsöka genom att söka efter en misslyckad behållare

Logganalys markerar en behållare som **misslyckades** om den har avslutats med slutkoden noll. Du kan se en översikt över hello fel och problem i hello miljö hello **misslyckades behållare** område.

### <a name="toofind-failed-containers"></a>toofind misslyckades behållare
1. Klicka på hello **behållare Status** område.  
   ![behållare status](./media/log-analytics-containers/containers-status.png)
2. Sök i loggfilen öppnas och visar hello tillståndet för din behållare, liknande toohello följande.  
   ![behållare tillstånd](./media/log-analytics-containers/containers-log-search.png)
3. Klicka sedan på hello visar det sammanlagda värdet för misslyckade behållare tooview ytterligare information. Expandera **visa fler** tooview hello image-ID.  
   ![misslyckade behållare](./media/log-analytics-containers/containers-state-failed.png)  
4. Skriv sedan följande hello i hello sökfråga. `Type=ContainerInventory <ImageID>`toosee information om hello avbildning, till exempel avbildningens storlek och antal bilder har stoppats och misslyckade.  
   ![misslyckade behållare](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Sökloggar för behållardata
När du felsöker ett visst fel kan det hjälpa toosee där uppstår i din miljö. hello följande typer av loggen kan du skapa frågor tooreturn hello information som du vill.


- **ContainerImageInventory** – Använd den här typen när du försöker toofind information som är ordnad efter avbildningen och tooview avbildningsinformationen som bild-ID: N eller storlekar.
- **ContainerInventory** – Använd den här typen om du vill ha information om behållarens plats deras namn är och vad de kör bilder.
- **ContainerLog** – Använd den här typen om du vill toofind specifika informationen i felloggen och poster.
- **ContainerNodeInventory_CL** Använd den här typen om du vill hello information om värd-nod där behållare finns. Det ger dig Docker-version, orchestration-typ., lagring och nätverksinformation.
- **ContainerProcess_CL** Använd den här typen tooquickly finns hello process som körs i hello behållaren.
- **ContainerServiceLog** – Använd den här typen när du försöker toofind information om redovisningsspårning för hello Docker-daemon, till exempel starta, stoppa, ta bort eller pull-kommandon.
- **KubeEvents_CL** använder den här typen toosee hello Kubernetes händelser.
- **KubePodInventory_CL** Använd den här typen om du vill toounderstand hello klusterinformation hierarki.


### <a name="toosearch-logs-for-container-data"></a>toosearch loggar för behållardata
* Välj en bild som du vet misslyckades nyligen och hitta hello felloggar för den. Börja med att hitta ett behållarnamn som körs på avbildningen med en **ContainerInventory** sökning. Till exempel söka efter`Type=ContainerInventory ubuntu Failed`  
    ![Sök efter Ubuntu behållare](./media/log-analytics-containers/search-ubuntu.png)

  Hej namnet på behållaren hello bredvid för**namn**, och Sök efter dessa loggar. I det här exemplet är det `Type=ContainerLog cranky_stonebreaker`.

**Visa information om prestanda**

När du från tooconstruct frågor kan hjälpa det toosee vad du kan göra först. Till exempel toosee Sök alla prestandadata, försök en bred fråga genom att skriva hello följande fråga.

```
Type=Perf
```

![behållare prestanda](./media/log-analytics-containers/containers-perf01.png)

Du kan definiera hello prestandadata det uppstår tooa specifik behållare genom att skriva hello namnet på den toohello höger i frågan.

```
Type=Perf <containerName>
```

Som visar hello lista över prestandamått som samlas in för en enskild behållare.

![behållare prestanda](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Exempel loggen sökfrågor
Ofta användbara toobuild frågar börjar med ett exempel eller två och sedan ändra dem toofit din miljö. Som en startpunkt som du kan experimentera med hello **exempelfrågor** område toohelp du skapa mer avancerade frågor.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Behållare frågor](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Spara loggfilen sökresultat
Spara frågor är en funktion som standard i logganalys. Genom att spara dem, har du de som du har hittat användbar praktiska för framtida användning.

När du skapar en fråga som vara användbara sparar du genom att klicka på **Favoriter** hello överst i hello loggen söksidan. Du kan enkelt använda det senare från hello **min instrumentpanel** sidan.

## <a name="next-steps"></a>Nästa steg
* [Söka i loggar](log-analytics-log-searches.md) tooview detaljerad dataposter för behållaren.
