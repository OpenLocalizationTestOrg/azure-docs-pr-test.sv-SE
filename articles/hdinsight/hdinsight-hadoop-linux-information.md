---
title: "aaaTips för användning av Hadoop på Linux-baserade HDInsight - Azure | Microsoft Docs"
description: "Hämta implementering tips om att använda kluster för Linux-baserat HDInsight (Hadoop) på en välbekant miljö för Linux körs i hello Azure-molnet."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c41c611c-5798-4c14-81cc-bed1e26b5609
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: a555622605079c9beae88ece872042e36d540c7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="information-about-using-hdinsight-on-linux"></a>Information om hur du använder HDInsight på Linux

Azure HDInsight-kluster tillhandahåller Hadoop på en välbekant miljö för Linux, körs i hello Azure-molnet. För de flesta saker, bör det fungerar precis som andra Hadoop på Linux-installationen. Det här dokumentet visar specifika skillnader som du bör vara medveten om.

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Krav

Många av hello stegen i det här dokumentet använder hello följande verktyg som kan behöva toobe som installerats på datorn.

* [cURL](https://curl.haxx.se/) -används toocommunicate med webbtjänster
* [jq](https://stedolan.github.io/jq/) -används tooparse JSON-dokument
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (förhandsvisning) – Använd tooremotely hantera Azure-tjänster

## <a name="users"></a>Användare

Om inte [domänanslutna](hdinsight-domain-joined-introduction.md), HDInsight ska betraktas som en **enanvändarläge** system. En SSH-användarkontot skapas med hello kluster med administratörsbehörighet för nivån. Ytterligare SSH-konton kan skapas, men de har också administratören åtkomst toohello klustret.

Domänanslutna HDInsight har stöd för flera användare och mer detaljerade inställningar för behörighet och roll. Mer information finns i [hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-manage.md).

## <a name="domain-names"></a>Domännamn

hello fullständigt kvalificerade namn (FQDN) toouse för domänen när du ansluter toohello kluster från hello internet är  **&lt;klusternamn >. azurehdinsight.net** eller (SSH)  **&lt;klusternamn-ssh >. azurehdinsight.net**.

Varje nod i klustret hello har internt, ett namn som tilldelas under klusterkonfigurationen. toofind hello klusternamn, se hello **värdar** sida på hello Ambari-Webbgränssnittet. Du kan också använda hello följande tooreturn en lista över värdar från hello Ambari REST API:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Ersätt **lösenord** med hello lösenordet för hello administratörskonto och **KLUSTERNAMN** med hello namnet på klustret. Det här kommandot returnerar ett JSON-dokument som innehåller en lista över hello värdar i klustret hello. Jq är används tooextract hello `host_name` elementvärde för varje värd.

Om du behöver toofind hello namnet hello noden för en viss tjänst, kan du fråga Ambari för den komponenten. Till exempel använda toofind hello värdar för hello HDFS namn nod hello följande kommando:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Det här kommandot returnerar ett JSON-dokument som beskriver hello-tjänsten och sedan jq hämtar ut endast hello `host_name` värdet för hello-värdar.

## <a name="remote-access-tooservices"></a>Fjärråtkomst tooservices

* **Ambari (webb)** -https://&lt;klusternamn >. azurehdinsight.net

    Autentisera med hjälp av hello klustret administratörsanvändare och lösenordet och logga sedan in tooAmbari.

    Autentisering är klartext - alltid Använd HTTPS toohelp se till att hello anslutningen är säker.

    > [!IMPORTANT]
    > Vissa av hello web användargränssnitt som är tillgängliga via Ambari åt noder med hjälp av ett internt domännamn. Interna domänens namn inte är offentligt tillgänglig över hello internet. Felmeddelandet ”Det gick inte att hitta” fel vid försök tooaccess vissa funktioner via hello Internet.
    >
    > toouse hello fullständiga funktionaliteten hos hello Ambari-webbgränssnittet, använda en SSH-tunnel tooproxy web trafik toohello klustrets huvudnod. Se [använda SSH-tunnlar tooaccess Ambari web UI, resurshanteraren, jobbhistorik, NameNode, Oozie och andra webb-användargränssnitt](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** -https://&lt;klusternamn >.azurehdinsight.net/ambari

    > [!NOTE]
    > Autentisera med hello klustret administratörsanvändare och lösenord.
    >
    > Autentisering är klartext - alltid Använd HTTPS toohelp se till att hello anslutningen är säker.

* **WebHCat (Templeton)** -https://&lt;klusternamn >.azurehdinsight.net/templeton

    > [!NOTE]
    > Autentisera med hello klustret administratörsanvändare och lösenord.
    >
    > Autentisering är klartext - alltid Använd HTTPS toohelp se till att hello anslutningen är säker.

* **SSH** - &lt;klusternamn >-ssh.azurehdinsight.net på port 22 eller 23. Port 22 är används tooconnect toohello primära headnode, medan 23 används tooconnect toohello sekundär. Mer information om hello huvudnoderna finns [tillgänglighet och tillförlitlighet för Hadoop-kluster i HDInsight](hdinsight-high-availability-linux.md).

    > [!NOTE]
    > Du kan bara komma åt hello-klustrets huvudnoder via SSH från en klientdator. När du är ansluten, du kan sedan komma åt hello arbetarnoder med hjälp av SSH från en headnode.

## <a name="file-locations"></a>Sökvägar

Hadoop-relaterade filer kan hittas på hello klusternoder på `/usr/hdp`. Den här katalogen innehåller hello följande undermappar:

* **2.2.4.9-1**: hello katalognamnet är hello hello Hortonworks Data Platform används av HDInsight-version. hello numret på ditt kluster är annorlunda än hello en som visas här.
* **aktuella**: den här katalogen innehåller länkar toosubdirectories under hello **2.2.4.9-1** directory. Denna katalog finns så att du inte har tooremember hello-versionsnumret.

Exempeldata och JAR-filer kan hittas på Hadoop Distributed File System på `/example` och`/HdiSamples`

## <a name="hdfs-azure-storage-and-data-lake-store"></a>HDFS och Azure Storage Data Lake Store

I de flesta Hadoop distributioner stöds HDFS av lokal lagring på hello datorer i hello kluster. Använder lokal lagring kan vara kostsamma för en molnbaserad lösning där du debiteras timvis eller per minut för beräkningsresurser.

HDInsight använder antingen blobbar i Azure Storage eller Azure Data Lake Store som hello standardlagringsplatsen. De här tjänsterna är hello följande fördelar:

* Billig långsiktig lagring
* Åtkomst från externa tjänster, till exempel webbplatser, filen överför/hämta verktyg, SDK: er med olika språk och webbläsare

> [!WARNING]
> HDInsight stöder endast __allmänna__ Azure Storage-konton. Den stöder för närvarande inte hello __Blob storage__ kontotyp.

Ett Azure Storage-konto kan innehålla upp too4.75 TB, även om enskilda blobbar (eller filer från ett HDInsight-perspektiv) kan du bara gå in too195 GB. Azure Data Lake Store kan utökas dynamiskt toohold utförs BILJONTALS av filer, enskilda filer som är större än en petabyte. Mer information finns i [förstå blobbar](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) och [Datasjölager](https://azure.microsoft.com/services/data-lake-store/).

När du använder antingen Azure Storage eller Azure Data Lake Store kan du inte toodo något speciellt från HDInsight tooaccess hello data. Till exempel hello följande kommando visar filer i hello `/example/data` mappen oavsett om den är lagrad på Azure Storage eller Azure Data Lake Store:

    hdfs dfs -ls /example/data

### <a name="uri-and-scheme"></a>URI: N och schema

Vissa kommandon måste du kanske toospecify hello schemat som en del av hello URI vid åtkomst till en fil. Hello Storm-HDFS komponenten kräver till exempel toospecify hello schemat. När du använder icke-förvalt lagring (storage läggas till som ”ytterligare” toohello lagringsklustret), måste du alltid använda hello schemat som en del av hello URI.

När du använder __Azure Storage__, med någon av följande URI-scheman hello:

* `wasb:///`: Åtkomst standardlagring använder okrypterad kommunikation.

* `wasbs:///`: Standard lagring med krypterad kommunikation.  Hej wasbs scheman stöds endast från HDInsight version 3,6 och senare.

* `wasb://<container-name>@<account-name>.blob.core.windows.net/`: Används vid kommunikation med ett icke-förvalt storage-konto. Till exempel om du har ett konto för ytterligare lagringsutrymme eller när åtkomst till data lagras i ett offentligt tillgänglig storage-konto.

När du använder __Datasjölager__, med någon av följande URI-scheman hello:

* `adl:///`: Komma åt hello standard Data Lake Store för hello-kluster.

* `adl://<storage-name>.azuredatalakestore.net/`: Används vid kommunikation med en icke-förvalt Data Lake Store. Används också tooaccess data utanför hello rotkatalog ditt HDInsight-kluster.

> [!IMPORTANT]
> När du använder Data Lake Store som hello standardlagringsplatsen för HDInsight, måste du ange en sökväg i hello store toouse som hello rot HDInsight lagring. hello standardsökvägen är `/clusters/<cluster-name>/`.
>
> När du använder `/` eller `adl:///` tooaccess data, du kan bara komma åt data som lagras i hello rot (till exempel `/clusters/<cluster-name>/`) hello-klustret. tooaccess data var som helst i hello store använder hello `adl://<storage-name>.azuredatalakestore.net/` format.

### <a name="what-storage-is-hello-cluster-using"></a>Vilka lagring hello klustret använder

Du kan använda Ambari tooretrieve hello standard lagringskonfiguration för hello-kluster. Använd hello följande kommando tooretrieve HDFS konfigurationsinformation med curl och filtrera den med hjälp av [jq](https://stedolan.github.io/jq/):

```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```

> [!NOTE]
> Detta returnerar hello första tillämpas toohello konfigurationsservern (`service_config_version=1`), som innehåller den här informationen. Du kan behöva toolist alla konfigurationsversionerna toofind hello senaste.

Det här kommandot returnerar ett värde liknande toohello följande URI: er:

* `wasb://<container-name>@<account-name>.blob.core.windows.net`Om du använder ett Azure Storage-konto.

    hello kontonamnet är hello namnet på hello Azure Storage-konto, hello behållarens namn är hello blob-behållare som är hello roten för hello klusterlagringen.

* `adl://home`Om du använder Azure Data Lake Store. tooget hello Data Lake Store-namn, Använd hello följande REST-anrop:

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'```

    Det här kommandot returnerar hello efter värdnamn: `<data-lake-store-account-name>.azuredatalakestore.net`.

    tooget hello katalog i hello-butiken som är hello rot för HDInsight, Använd hello följande REST-anrop:

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'```

    Det här kommandot returnerar en sökväg liknande toohello följande sökväg: `/clusters/<hdinsight-cluster-name>/`.

Du kan också hitta hello storage-informationen med hello Azure-portalen med hjälp av hello följande steg:

1. I hello [Azure-portalen](https://portal.azure.com/), Välj ditt HDInsight-kluster.

2. Från hello **egenskaper** väljer **Lagringskonton**. hello storage-informationen för hello kluster visas.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>Hur kommer jag åt filer från utanför HDInsight

Det finns olika sätt tooaccess data från utanför hello HDInsight-kluster. hello följande är några länkar tooutilities och SDK: er som kan använda toowork med dina data:

Om du använder __Azure Storage__, se följande länkar för olika sätt att du kan komma åt dina data hello:

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2): kommandoradsgränssnittet kommandon för att arbeta med Azure. När installationen är klar använder du hello `az storage` kommandot för hjälp med att använda lagring, eller `az storage blob` för blob-fil.
* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): en python-skript för att arbeta med blobbar i Azure Storage.
* Olika SDK:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Storage REST API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Om du använder __Azure Data Lake Store__, se följande länkar för olika sätt att du kan komma åt dina data hello:

* [Webbläsare](../data-lake-store/data-lake-store-get-started-portal.md)
* [PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [Azure CLI 2.0](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [WebHDFS REST API](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [Data Lake-verktyg för Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling"></a>Skalning av klustret

hello klustret skalning funktionen kan du toodynamically ändra hello antalet datanoder som används av ett kluster. Du kan utföra skalning åtgärder när andra jobb eller processer som körs på ett kluster.

hello olika klustertyper påverkas av skalning enligt följande:

* **Hadoop**: vid skalning av hello antalet noder i ett kluster, hello tjänster i hello klustret startas om. Skalning åtgärder kan orsaka jobb körs eller väntar toofail på hello hello skalning åtgärden har slutförts. Du kan skicka hello jobb när hello åtgärden är klar.
* **HBase**: regionala servrar balanseras automatiskt inom några minuter efter hello skalning åtgärden har slutförts. toomanually saldo regionala servrar, Använd hello följande steg:

    1. Ansluta toohello HDInsight-kluster med SSH. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

    2. Använd följande toostart hello HBase-gränssnittet hello:

            hbase shell

    3. När hello HBase-gränssnittet har lästs in, använder du följande toomanually saldo hello regionala servrar hello:

            balancer

* **Storm**: du bör balansera alla Storm-topologier som körs när en skalning åtgärden har utförts. Ombalansering kan hello topologi tooreadjust parallellitet inställningar baserat på hello nya antalet noder i klustret hello. toorebalance topologier som körs, med någon av följande alternativ för hello:

    * **SSH**: ansluta toohello server och Använd hello följande kommando toorebalance en topologi:

            storm rebalance TOPOLOGYNAME

        Du kan också ange parametrar toooverride hello parallellitet tips som ursprungligen hello-topologi. Till exempel `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` Omkonfigurerar hello topologi too5 arbetsprocesser, 3 Utförarna för hello blå kanal komponent och 10 Utförarna för hello gul bult komponent.

    * **Storm-Användargränssnittet**: Använd hello följande steg toorebalance en topologi med hello Storm-Användargränssnittet.

        1. Öppna **https://CLUSTERNAME.azurehdinsight.net/stormui** i webbläsaren, där KLUSTERNAMN är hello namnet på Storm-kluster. Om du uppmanas ange hello HDInsight-kluster administratör (admin) namn och lösenord som du angav när du skapar hello kluster.
        2. Välj hello topologi du vill toorebalance och sedan välja hello **balansera** knappen. Ange hello fördröjning innan hello balansera åtgärden utförs.

Specifik information om att skala HDInsight-kluster, se:

* [Hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Hantera Hadoop-kluster i HDInsight med hjälp av Azure PowerShell](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hur installerar jag Hue (eller andra Hadoop-komponenter)?

HDInsight är en hanterad tjänst. Om Azure upptäcker ett problem med hello klustret, det kan ta bort hello misslyckas nod och skapa en nod tooreplace den. Du kan manuellt installera saker på hello klustret, är inte beständiga när den här åtgärden genomförs. Använd i stället [HDInsight-skriptåtgärder](hdinsight-hadoop-customize-cluster.md). En skriptåtgärd kan vara används toomake hello följande ändringar:

* Installera och konfigurera en tjänst eller en webbplats till exempel Spark eller Hue.
* Installera och konfigurera en komponent som kräver konfigurationsändringar på flera noder i klustret hello. Till exempel en obligatorisk miljövariabel, skapa en loggningskatalog eller skapa en konfigurationsfil.

Skriptåtgärder är Bash-skript. hello-skript kan körs under klusteretablering, och använda tooinstall och konfigurera ytterligare komponenter på hello klustret. Exempelskript tillhandahålls för att installera hello följande komponenter:

* [Hue](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Information om hur du utvecklar dina egna skriptåtgärder finns i [Utveckling av skriptåtgärder med HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="jar-files"></a>JAR-filer

Vissa Hadoop-teknik finns i självständiga jar-filer som innehåller funktioner som används som en del av ett MapReduce-jobb, eller från inuti Pig- eller Hive. När dessa kan installeras med hjälp av skriptåtgärder, de ofta kräver inte några inställningar och kan vara överförda toohello när du har etablerat och används direkt. Om du vill toomake till hello komponenten FRISKRIVNINGEN återställning av avbildning av hello kluster, kan du lagra hello jar-filen i hello standardlagring för klustret (WASB eller ADL).

Om du vill toouse hello senaste versionen av till exempel [DataFu](http://datafu.incubator.apache.org/), du kan hämta en jar som innehåller hello projektet och överföra den toohello HDInsight-kluster. Följ sedan hello DataFu dokumentationen om hur toouse Pig eller Hive.

> [!IMPORTANT]
> Vissa komponenter som är fristående jar-filer med HDInsight, men är inte i hello sökväg. Om du letar efter en viss komponent kan använda du hello Följ toosearch för den i klustret:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Det här kommandot returnerar hello sökvägen till några matchande jar-filer.

toouse en annan version av en komponent, överför hello version du behöver och använda i dina jobb.

> [!WARNING]
> Komponenter som ingår i hello HDInsight-kluster stöds fullt ut och Microsoft-supporten hjälper tooisolate och lösa problem relaterade toothese komponenter.
>
> Anpassade komponenter får stöd för kommersiellt rimliga toohelp du toofurther hello felsökning. Detta kan resultera i att lösa problemet hello eller be tooengage tillgängliga kanaler för hello öppnas teknikerna där djup expertis för att teknik finns. Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Nästa steg

* [Migrera från Windows-baserade HDInsight tooLinux-baserade](hdinsight-migrate-from-windows-to-linux.md)
* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce-jobb med HDInsight](hdinsight-use-mapreduce.md)
