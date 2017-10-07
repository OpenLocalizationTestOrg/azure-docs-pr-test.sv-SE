---
title: aaaMonitor och hantera Hadoop med Ambari REST API - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Ambari toomonitor och hantera Hadoop-kluster i Azure HDInsight. I detta dokument kan du lära dig hur toouse hello Ambari REST API som ingår i HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 1866a77c8e402231bccbcfba7174253aca41339b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-rest-api"></a>Hantera HDInsight-kluster med hjälp av hello Ambari REST API

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Lär dig hur toouse hello Ambari REST API toomanage och övervaka Hadoop-kluster i Azure HDInsight.

Apache Ambari förenklar hello hantering och övervakning av ett Hadoop-kluster genom att tillhandahålla en enkel toouse web användargränssnitt och REST API. Ambari ingår i HDInsight-kluster som använder hello Linux-operativsystem. Du kan använda Ambari toomonitor hello klustret och gör ändringar i konfigurationen.

## <a id="whatis"></a>Vad är Ambari

[Apache Ambari](http://ambari.apache.org) ger webbgränssnitt som kan använda tooprovision, hantera och övervaka Hadoop-kluster. Utvecklare kan integrera dessa funktioner i sina program med hjälp av hello [Ambari REST API: er](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari är som standard med Linux-baserade HDInsight-kluster.

## <a name="how-toouse-hello-ambari-rest-api"></a>Hur toouse hello Ambari REST API

> [!IMPORTANT]
> hello information och exempel i det här dokumentet kräver ett HDInsight-kluster som använder Linux-operativsystem. Mer information finns i [komma igång med HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

hello exemplen i det här dokumentet tillhandahålls för både hello Bourne shell (bash) och PowerShell. hello bash exempel har testats med GNU bash 4.3.11, men bör arbeta tillsammans med andra Unix-gränssnitt. hello PowerShell-exemplen har testats med PowerShell 5.0, men ska fungera med PowerShell 3.0 eller senare.

Om du använder hello __Bourne shell__ (Bash), måste du ha hello följande installerat:

* [cURL](http://curl.haxx.se/): cURL är ett verktyg som kan använda toowork med REST API: er från hello kommandorad. I detta dokument är det använda toocommunicate med hello Ambari REST API.

Om du använder Bash eller PowerShell, du måste ha [jq](https://stedolan.github.io/jq/) installerad. Jq är ett verktyg för att arbeta med JSON-dokument. Den används i **alla** hello Bash exempel och **en** av hello PowerShell-exemplen.

### <a name="base-uri-for-ambari-rest-api"></a>Bas-URI för Ambari Rest API

hello bas-URI för hello Ambari REST API på HDInsight är https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, där **KLUSTERNAMN** är hello namnet på klustret.

> [!IMPORTANT]
> Medan hello klusternamnet i hello fullständigt kvalificerade namn (FQDN) domändelen av hello URI (CLUSTERNAME.azurehdinsight.net) är skiftlägeskänslig, andra förekomster i hello URI är skiftlägeskänsliga. Om klustret har namnet till exempel `MyCluster`, hello följande är giltiga URI: er:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> hello följande URI returneras ett fel eftersom hello andra förekomsten av hello namn inte hello korrigera fallet.
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a>Autentisering

Ansluta tooAmbari på HDInsight kräver HTTPS. Använd hello admin kontonamn (hello standardvärdet är **admin**) och lösenord som du angav när klustret skapas.

## <a name="examples-authentication-and-parsing-json"></a>Exempel: Autentisering och parsa JSON

hello som följande exempel visar hur toomake en GET-begäran mot hello basera Ambari REST API:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> hello Bash exemplen i det här dokumentet att Hej följande förutsättningar:
>
> * hello inloggningsnamn för hello kluster är hello standardvärdet för `admin`.
> * `$PASSWORD`innehåller hello lösenord för hello HDInsight inloggningen kommando. Du kan ange ett värde med hjälp av `PASSWORD='mypassword'`.
> * `$CLUSTERNAME`innehåller hello hello klustrets namn. Du kan ange ett värde med hjälp av`set CLUSTERNAME='clustername'`

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> hello PowerShell-exemplen i det här dokumentet att Hej följande förutsättningar:
>
> * `$creds`är ett autentiseringsuppgiftobjekt som innehåller hello admin inloggningsnamn och lösenord för hello-kluster. Du kan ange ett värde med hjälp av `$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"` och ge hello autentiseringsuppgifter när du tillfrågas.
> * `$clusterName`är en sträng som innehåller hello hello klustrets namn. Du kan ange ett värde med hjälp av `$clusterName="clustername"`.

Båda exempel returnera ett JSON-dokument som börjar med liknande information toohello som följande exempel:

```json
{
"href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
"Clusters" : {
    "cluster_id" : 2,
    "cluster_name" : "CLUSTERNAME",
    "health_report" : {
    "Host/stale_config" : 0,
    "Host/maintenance_state" : 0,
    "Host/host_state/HEALTHY" : 7,
    "Host/host_state/UNHEALTHY" : 0,
    "Host/host_state/HEARTBEAT_LOST" : 0,
    "Host/host_state/INIT" : 0,
    "Host/host_status/HEALTHY" : 7,
    "Host/host_status/UNHEALTHY" : 0,
    "Host/host_status/UNKNOWN" : 0,
    "Host/host_status/ALERT" : 0
    ...
```

### <a name="parsing-json-data"></a>Parsning av JSON-data

hello följande exempel används `jq` tooparse hello svar JSON-dokumentet och visa endast hello `health_report` information från hello resultat.

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

PowerShell 3.0 och senare innehåller hello `ConvertFrom-Json` cmdlet, som konverterar hello JSON-dokumentet till ett objekt som är enklare toowork med från PowerShell. hello följande exempel används `ConvertFrom-Json` toodisplay endast hello `health_report` information från hello resultat.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> När de flesta exemplen i det här dokumentet används `ConvertFrom-Json` toodisplay element från hello svarsdokument hello [uppdatering Ambari configuration](#example-update-ambari-configuration) exempel används jq. Jq används i det här exemplet tooconstruct en ny mall från svar hello JSON-dokument.

En fullständig hello REST API-referens, se [Ambari API-referens V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="example-get-hello-fqdn-of-cluster-nodes"></a>Exempel: Hämta hello FQDN för klusternoder

När du arbetar med HDInsight kan behöva tooknow hello fullständigt kvalificerade domännamnet (FQDN) på en klusternod. Du kan enkelt hämta hello FQDN för hello olika noder i hello-kluster med hjälp av hello följande exempel:

* **Alla noder**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* **HEAD-noder**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Arbetsnoder**

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Zookeeper-noder**

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-hello-internal-ip-address-of-cluster-nodes"></a>Exempel: Hämta hello interna IP-adressen för klusternoder

> [!IMPORTANT]
> hello IP-adresser som returneras av hello exemplen i det här avsnittet är inte direkt tillgänglig över hello internet. De är bara tillgängliga i hello Azure Virtual Network som innehåller hello HDInsight-kluster.
>
> Mer information om att arbeta med HDInsight och virtuella nätverk finns [utöka HDInsight funktioner med hjälp av en anpassad Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

toofind hello IP-adress, måste du veta hello interna fullständigt kvalificerade domännamnet (FQDN) för hello klusternoder. När du har hello FQDN, kan du hämta hello hello värdens IP-adress. hello följande exempel först fråga Ambari för hello FQDN för alla noder i hello-värden och fråga Ambari för hello IP-adress för varje värd.

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts"
$resp = Invoke-WebRequest -Uri $uri -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

## <a name="example-get-hello-default-storage"></a>Exempel: Hämta hello standardlagring

När du skapar ett HDInsight-kluster måste du använda ett Azure Storage-konto eller ett Data Lake Store som hello standardlagring för hello klustret. Du kan använda Ambari tooretrieve informationen efter hello klustret har skapats. Till exempel om du vill tooread/skriva data toohello behållare utanför HDInsight.

hello hämtar följande exempel hello standardkonfigurationen för lagring från hello klustret:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]
> De här exemplen returnera hello första tillämpas toohello konfigurationsservern (`service_config_version=1`) som innehåller den här informationen. Om du hämtar ett värde som har ändrats efter att klustret har skapats kan du behöver toolist hello konfigurationsversionerna och hämta hello senaste.

hello returvärdet är liknande tooone av hello följande exempel:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`-Det här värdet anger hello klustret använder ett Azure Storage-konto för standardlagring. Hej `ACCOUNTNAME` värdet är hello namnet på hello storage-konto. Hej `CONTAINER` del är hello namnet på hello blob-behållaren i hello storage-konto. hello-behållaren är hello roten hello HDFS kompatibel lagringen för hello klustret.

* `adl://home`-Det här värdet anger hello klustret använder en Azure Data Lake Store för standardlagring.

    toofind hello Data Lake Store kontonamn, Använd hello följande exempel:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    hello returvärdet är liknande för`ACCOUNTNAME.azuredatalakestore.net`, där `ACCOUNTNAME` är hello namnet på hello Data Lake Store-konto.

    toofind hello katalog i Data Lake Store som innehåller hello lagringen för hello klustret, Använd hello följande exempel:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    hello returvärdet är liknande för`/clusters/CLUSTERNAME/`. Det här värdet är en sökväg i hello Data Lake Store-konto. Den här sökvägen är hello roten hello HDFS-kompatibla filsystem för hello klustret. 

> [!NOTE]
> Hej `Get-AzureRmHDInsightCluster` cmdlet som tillhandahålls av [Azure PowerShell](/powershell/azure/overview) också returnerar hello storage-informationen för hello-kluster.


## <a name="example-get-configuration"></a>Exempel: Get-konfiguration

1. Hämta hello-konfigurationer som är tillgängliga för klustret.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    Det här exemplet returnerar ett JSON-dokument som innehåller aktuella hello-konfiguration (identifieras av hello *taggen* värde) för hello-komponenter installeras på hello-kluster. hello är följande exempel ett utdrag ur hello data som returneras från en typ av Spark-kluster.
   
   ```json
   "spark-metrics-properties" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-fairscheduler" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-sparkconf" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   }
   ```

2. Få hello-konfiguration för hello-komponent som du är intresserad av. Följande exempel Ersätt i hello `INITIAL` med hello Taggvärde som returneras från hello tidigare begäran.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    Det här exemplet returnerar ett JSON-dokument som innehåller hello aktuella konfiguration för hello `core-site` komponent.

## <a name="example-update-configuration"></a>Exempel: Uppdateringskonfiguration

1. Hämta hello aktuella konfigurationen som Ambari lagrar som hello ”desired configuration”:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    Det här exemplet returnerar ett JSON-dokument som innehåller aktuella hello-konfiguration (identifieras av hello *taggen* värde) för hello-komponenter installeras på hello-kluster. hello är följande exempel ett utdrag ur hello data som returneras från en typ av Spark-kluster.
   
    ```json
    "spark-metrics-properties" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-fairscheduler" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-sparkconf" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    }
    ```
   
    I den här listan måste toocopy hello namnet på hello-komponenten (till exempel **spark\_thrift\_sparkconf** och hello **taggen** värde.

2. Hämta hello konfiguration för hello komponent och tagg med hjälp av hello följande kommandon:
   
    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
    | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```powershell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=spark-thrift-sparkconf&tag=INITIAL" `
        -Credential $creds
    $resp.Content | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    > [!NOTE]
    > Ersätt **spark-thrift-sparkconf** och **inledande** med hello-komponenten och tagg som du vill tooretrieve hello konfiguration för.
   
    Jq är används tooturn hello data som hämtats från HDInsight till en ny konfigurationsmall för. Mer specifikt utför de här exemplen hello följande åtgärder:
   
    * Skapar ett unikt värde som innehåller hello strängen ”version” och hello datum, som lagras i `newtag`.

    * Skapar ett rot-dokument för hello nya önskad konfiguration.

    * Hämtar hello innehållet i hello `.items[]` matrisen och lägger till den under hello **desired_config** element.

    * Tar bort hello `href`, `version`, och `Config` element, som de här elementen inte behövs toosubmit en ny konfiguration.

    * Lägger till en `tag` element med värdet `version#################`. Hej sifferdelen baseras på hello aktuellt datum. Varje konfiguration måste ha en unik tagg.
     
    Slutligen hello data sparas toohello `newconfig.json` dokumentet. hello dokumentstruktur ska se ut ungefär toohello följande exempel:
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. Öppna hello `newconfig.json` dokument och ändra/Lägg till värden i hello `properties` objekt. hello följande exempel ändringar hello värdet för `"spark.yarn.am.memory"` från `"1g"` för`"3g"`. Det ger också `"spark.kryoserializer.buffer.max"` med värdet `"256m"`.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Spara hello-filen när du är klar med ändringarna.

4. Använd följande kommandon toosubmit hello uppdateras configuration tooAmbari hello.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```
   
    Dessa kommandon skicka hello innehållet i hello **newconfig.json** filen toohello klustret som hello nya önskad konfiguration. hello-begäran returnerar ett JSON-dokument. Hej **versionTag** element i det här dokumentet ska matcha hello-version du har skickats och hello **konfigurationerna** objektet innehåller hello konfigurationsändringar som du begärde.

### <a name="example-restart-a-service-component"></a>Exempel: Starta om en tjänstkomponent

Du tittar på hello Ambari-webbgränssnittet visar hello Spark-tjänst nu att den behöver toobe startas om innan hello nya konfigurationen börjar gälla. Använd hello följande steg toorestart hello-tjänsten.

1. Använd hello följande tooenable Underhållsläge för hello Spark-tjänst:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```
   
    Dessa kommandon skickar en JSON-dokument toohello server som aktiverar underhållsläge. Du kan verifiera att hello-tjänsten är nu i underhållsläge med hello följande begäran:
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    hello returvärdet är `ON`.

2. Använd sedan hello följande tooturn av hello-tjänsten:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```
    
    hello svaret är liknande toohello följande exempel:
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    > [!IMPORTANT]
    > Hej `href` värdet som returneras av den här URI: N använder hello interna IP-adress hello klusternod. toouse från utanför hello klustret ersätta hello '10.0.0.18:8080' delen med hello domännamn för hello-klustret. 
    
    Hej efter kommandona hämta hello status för hello begäran:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Ett svar av `COMPLETED` anger hello begäran har slutförts.

3. Använda hello följande toostart hello tjänsten när hello tidigare begäran har slutförts.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```
   
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```
    hello-tjänsten använder hello ny konfiguration.

4. Använd slutligen hello följande tooturn av underhållsläget.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a>Nästa steg

En fullständig hello REST API-referens, se [Ambari API-referens V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

