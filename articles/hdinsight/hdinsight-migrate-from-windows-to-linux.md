---
title: "aaaMigrate från Windows-baserade HDInsight tooLinux-baserat HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toomigrate från en Windows-baserade HDInsight-kluster tooa Linux-baserade HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: ff35be59-bae3-42fd-9edc-77f0041bab93
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 7e5e536e8672d7e7c3086c6860cec062d05eda65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-a-windows-based-hdinsight-cluster-tooa-linux-based-cluster"></a>Migrera från en Windows-baserade HDInsight-kluster tooa Linux-baserade kluster

Det här dokumentet innehåller information om hello skillnaderna mellan HDInsight på Windows och Linux, och hur toomigrate befintligt arbetsbelastningar tooa Linux-baserade kluster.

När Windows-baserade HDInsight erbjuder ett enkelt sätt toouse Hadoop i molnet hello, måste toomigrate tooa Linux-baserade kluster. Till exempel tootake nytta av Linux-baserat verktyg och tekniker som krävs för din lösning. Många saker i hello Hadoop-ekosystemet utvecklas på Linux-baserade system och får inte vara tillgängliga för användning med Windows-baserade HDInsight. Många böcker, videor och andra utbildningsmaterial du dessutom förutsätter att du använder ett Linux-system när du arbetar med Hadoop.

> [!NOTE]
> HDInsight-kluster använda Ubuntu långsiktigt stöd (LTS) som hello operativsystem för hello noder i klustret hello. Information om hello version av Ubuntu tillgänglig med HDInsight, tillsammans med andra komponenten versionsinformation finns [HDInsight komponenten versioner](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Migreringsåtgärder

hello allmänna arbetsflödet för migrering är som följer.

![Diagram över arbetsflödet för migrering](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1. Läs avsnitten i det här dokumentet toounderstand ändringar som kan krävas när du migrerar befintliga arbetsflödet, jobb, etc. tooa Linux-baserade kluster.

2. Skapa ett Linux-baserade kluster som en test/kvalitet försäkran miljö. Mer information om hur du skapar ett Linux-baserade kluster finns [skapa Linux-baserade kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3. Kopiera befintliga jobb, datakällor, och egenskaperna toohello nya miljön.

4. Verifieringen testning toomake till att dina jobb fungerar som förväntat på hello nya klustret.

När du har kontrollerat att allt fungerar som förväntat, Schemalägg driftstopp för hello migrering. Utför följande åtgärder hello under den här driftstörningen:

1. Säkerhetskopiera alla tillfälligt data som lagras lokalt på hello klusternoder. Till exempel om du har data som lagras direkt på en huvudnod.

2. Ta bort hello Windows-baserade kluster.

3. Skapa ett Linux-baserade kluster med hello samma datalager som hello Windows-baserade kluster som används som standard. hello Linux-baserade kluster kan fortsätta arbeta mot ditt befintliga produktionsdata.

4. Importera alla tillfälligt säkerhetskopierade data.

5. Starta jobb/fortsätta att bearbeta med hello nytt kluster.

### <a name="copy-data-toohello-test-environment"></a>Kopiera data toohello testmiljö

Det finns många metoder toocopy hello data och jobb, men hello två beskrivs i det här avsnittet är hello enklaste metoderna toodirectly flytta filer tooa testklustret.

#### <a name="hdfs-copy"></a>HDFS-kopia

Använd följande steg toocopy data från hello klustret toohello test produktionskluster hello. De här stegen använder hello `hdfs dfs` verktyg som ingår i HDInsight.

1. Hitta hello storage-konto och standard behållare för befintliga klustret. följande exempel hello använder PowerShell tooretrieve informationen:

    ```powershell
    $clusterName="Your existing HDInsight cluster name"
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
    write-host "Default container: $clusterInfo.DefaultStorageContainer"
    ```

2. toocreate en testmiljö åtgärderna hello i hello skapa Linux-baserade kluster i HDInsight-dokument. Stoppa innan du skapar hello kluster och i stället väljer **valfri konfiguration**.

3. Hello valfri konfiguration bladet välj **länkade Lagringskonton**.

4. Välj **lägga till en nyckel för säkerhetslagring**, och när du uppmanas, Välj hello storage-konto som returnerades av hello PowerShell-skript i steg 1. Klicka på **Välj** på varje blad. Skapa slutligen hello klustret.

5. När hello klustret har skapats, ansluta tooit med **SSH.** Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

6. Använd hello följande kommando från SSH-session för hello toocopy filer från hello länkade storage-konto toohello nya standardkontot för lagring. Ersätta BEHÅLLAREN med hello behållaren information som returnerades av PowerShell. Ersätt __konto__ med hello kontonamn. Ersätt hello sökvägen toodata med hello sökvägen tooa datafilen.

    ```bash
    hdfs dfs -cp wasb://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location
    ```

    > [!NOTE]
    > Om hello katalogstruktur som innehåller hello data inte finns på hello testmiljö, kan du skapa den med hello följande kommando:

    ```bash
    hdfs dfs -mkdir -p /new/path/to/create
    ```

    Hej `-p` växeln gör det möjligt för hello skapa alla kataloger i hello sökvägen.

#### <a name="direct-copy-between-blobs-in-azure-storage"></a>Direkt kopiera mellan blobbar i Azure Storage

Alternativt kanske du toouse hello `Start-AzureStorageBlobCopy` Azure PowerShell-cmdlet toocopy blobbar mellan lagringskonton utanför HDInsight. Mer information finns i hello hur toomanage Azure BLOB-avsnittet i med hjälp av Azure PowerShell med Azure Storage.

## <a name="client-side-technologies"></a>Tekniker på klientsidan

Klientsidans tekniker som [Azure PowerShell-cmdlets](/powershell/azureps-cmdlets-docs), [Azure CLI](../cli-install-nodejs.md), eller hello [.NET SDK för Hadoop](https://hadoopsdk.codeplex.com/) fortsätta toowork Linux-baserade kluster. Dessa tekniker förlitar sig på REST API: er som är hello samma på båda klustertyper OS.

## <a name="server-side-technologies"></a>Tekniker för serversidan

hello följande tabell innehåller råd om migrera server-komponenter som är specifika för Windows.

| Om du använder den här tekniken... | Åtgärda... |
| --- | --- |
| **PowerShell** (-skript, inklusive skriptåtgärder användas när klustret skapas) |Skriv om som Bash-skript. Skriptåtgärder, se [anpassa Linux-baserade HDInsight med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md) och [för Linux-baserade HDInsight utveckling av skriptåtgärder](hdinsight-hadoop-script-actions-linux.md). |
| **Azure CLI** (serversidan skript) |Hello Azure CLI är tillgängliga på Linux, kommer det inte förinstallerat på HDInsight hello klustrets huvudnoder. Mer information om hur du installerar hello Azure CLI finns [Kom igång med Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli). |
| **.NET-komponenter** |.NET stöds på Linux-baserat HDInsight via [Mono](https://mono-project.com). Mer information finns i [migrera .NET lösningar tooLinux-baserat HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md). |
| **Win32-komponenter eller andra endast Windows-teknik** |Riktlinjer är beroende av hello-komponent eller teknik. Du kanske kan toofind en version som är kompatibel med Linux eller du kan behöva toofind en alternativ lösning eller skriva om den här komponenten. |

> [!IMPORTANT]
> Hej HDInsight management SDK är inte helt kompatibel med Mono. Det bör inte användas som en del av lösningar distribueras toohello HDInsight-klustret just nu.

## <a name="cluster-creation"></a>Skapa ett kluster

Det här avsnittet innehåller information om skillnaderna i klustret har skapats.

### <a name="ssh-user"></a>SSH användare

Linux-baserade HDInsight-kluster använder hello **SSH (Secure Shell)** protokollet tooprovide fjärråtkomst toohello klusternoder. Till skillnad från Remote Desktop för Windows-baserade kluster anger de flesta SSH-klienter inte ett grafiskt användargränssnitt. SSH-klienter innehåller en kommandorad som gör att du toorun kommandon på hello klustret. Vissa klienter (exempelvis [MobaXterm](http://mobaxterm.mobatek.net/)) ger ett grafiskt system webbläsaren tillägg tooa remote kommandoraden.

När klustret skapas måste du ange en SSH-användare och antingen en **lösenord** eller **offentliga nyckelcertifikat** för autentisering.

Du bör använda certifikat med offentlig nyckel, eftersom det är säkrare än att använda ett lösenord. Autentisering med datorcertifikat fungerar genom att generera ett signerat offentliga/privata nyckelpar och sedan tillhandahålla hello offentliga nyckel när du skapar hello kluster. När du ansluter toohello servern med hjälp av SSH ger hello privata nyckel på hello klient autentisering för hello-anslutning.

Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

### <a name="cluster-customization"></a>Anpassning av kluster

**Script åtgärder** används med Linux-baserade kluster måste skrivas i Bash-skript. Medan skriptåtgärder kan användas när klustret skapas för Linux-baserade kluster som de kan även vara används tooperform anpassning när ett kluster som är igång och körs. Mer information finns i [anpassa Linux-baserade HDInsight med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md) och [för Linux-baserade HDInsight utveckling av skriptåtgärder](hdinsight-hadoop-script-actions-linux.md).

En annan anpassningsfunktion är **bootstrap**. För Windows-kluster för kan den här funktionen du toospecify hello platsen för ytterligare bibliotek för användning med Hive. När klustret skapas dessa bibliotek är automatiskt tillgängliga för användning med Hive-frågor utan hello måste toouse `ADD JAR`.

hello Bootstrap funktionen för Linux-baserade kluster ger inte den här funktionen. Använd i stället skriptåtgärd dokumenterade i [lägga till Hive-bibliotek när klustret skapas](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Virtuella nätverk

Windows-baserade HDInsight-kluster endast fungerar med klassiska virtuella nätverk, medan Linux-baserade HDInsight-kluster kräver Resource Manager virtuella nätverk. Om du har resurser i ett klassiskt virtuellt nätverk som hello Linux HDInsight-kluster måste ansluta till, se [ansluta ett klassiskt virtuellt nätverk tooa Resource Manager för virtuella nätverk](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Mer information om konfigurationskrav för att använda virtuella Azure-nätverk med HDInsight finns [utöka HDInsight funktioner med hjälp av ett virtuellt nätverk](hdinsight-extend-hadoop-virtual-network.md).

## <a name="management-and-monitoring"></a>Hantering och övervakning

Många av hello web användargränssnitt som du har använt med Windows-baserade HDInsight, till exempel jobbhistorik eller Yarn-Användargränssnittet är tillgängliga via Ambari. Hello Ambari Hive-vy innehåller dessutom en sätt toorun Hive-frågor med hjälp av webbläsaren. Hej Ambari-Webbgränssnittet är tillgänglig på Linux-baserade kluster på https://CLUSTERNAME.azurehdinsight.net.

Mer information om att arbeta med Ambari finns hello följande dokument:

* [Ambari Web](hdinsight-hadoop-manage-ambari.md)
* [Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari aviseringar

Ambari har en varning som anger om potentiella problem med hello-kluster. Aviseringar visas som röd eller gul poster i hello Ambari-Webbgränssnittet, men du kan också hämta dem via hello REST API.

> [!IMPORTANT]
> Ambari-aviseringar anger om det *kan* vara ett problem, inte att det *är* problem. Exempelvis kan du få en varning att HiveServer2 inte kan nås, även om du har åtkomst till den normalt.
>
> Många aviseringar implementeras som intervallbaserad frågor mot en tjänst och förväntar sig ett svar inom en viss tidsperiod. Så hello varningen innebär inte nödvändigtvis att hello-tjänsten har stoppats, precis som returnerade resultat inom hello förväntades tidsintervall.

Du bör utvärdera om en avisering har pågått under en längre tid eller speglar problem som har rapporterats innan du vidtar åtgärden på den.

## <a name="file-system-locations"></a>Sökvägar för system

hello filsystem för Linux-kluster är placerade annorlunda än Windows-baserade HDInsight-kluster. Använd hello följande tabell toofind används vanligtvis för filer.

| Jag behöver toofind... | Det finns... |
| --- | --- |
| Konfiguration |`/etc`. Till exempel, `/etc/hadoop/conf/core-site.xml` |
| Loggfiler |`/var/logs` |
| Hortonworks Data Platform (HDP) |`/usr/hdp`. Det finns två kataloger finns här, ett som är hello aktuella HDP versionen och `current`. Hej `current` katalogen innehåller symboliska länkar toofiles och kataloger i hello version nummer katalogen. Hej `current` katalog som är ett bekvämt sätt att komma åt HDP filer eftersom hello ändras versionsnumret som hello HDP version är uppdaterad. |
| hadoop-streaming.jar |`/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

I allmänhet om du vet hello namnet på hello-fil kan använda du hello följande kommando från SSH-session toofind hello sökväg till en fil:

    find / -name FILENAME 2>/dev/null

Du kan också använda jokertecken med hello filnamn. Till exempel `find / -name *streaming*.jar 2>/dev/null` returnerar hello sökvägen tooany jar-filer som innehåller hello word strömning som en del av hello filnamn.

## <a name="hive-pig-and-mapreduce"></a>Hive, Pig och MapReduce

Pig och MapReduce arbetsbelastningar är liknande på Linux-baserade kluster. Linux-baserade HDInsight-kluster kan dock skapas med nyare versioner av Hadoop Hive och Pig. Dessa skillnader mellan versioner införa ändringar i hur din befintliga lösningar-funktion. Mer information om hello versioner av komponenter som ingår i HDInsight finns [HDInsight component-versioning](hdinsight-component-versioning.md).

Linux-baserade HDInsight ger inte remote desktop funktioner. Du kan i stället använda SSH tooremotely ansluter toohello head klusternoder. Mer information finns i följande dokument hello:

* [Använda Hive med SSH](hdinsight-hadoop-use-hive-ssh.md)
* [Använda Pig med SSH](hdinsight-hadoop-use-pig-ssh.md)
* [Använda MapReduce med SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Hive

> [!IMPORTANT]
> Om du använder en extern Hive metastore bör du säkerhetskopiera hello metastore innan du använder med Linux-baserade HDInsight. Linux-baserat HDInsight finns i nyare versioner av Hive, som kan ha inkompatibiliteter med metastores som skapats i tidigare versioner.

hello följande diagram ger vägledning om hur du migrerar dina Hive-arbetsbelastningar.

| På Windows-baserade jag använda... | På Linux-baserade... |
| --- | --- |
| **Hive-redigeraren** |[Hive-vyn i Ambari](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`tooenable Tez |Tez är motorn för körning av hello standard för Linux-baserade kluster, så att hello Ange instruktionen längre behövs inte. |
| C# användardefinierade funktioner | Mer information om verifiering C#-komponenter med Linux-baserat HDInsight finns [migrera .NET lösningar tooLinux-baserat HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-filer eller skript på hello servern anropas som en del av ett Hive-jobb |Använd Bash-skript |
| `hive`kommando från fjärrskrivbord |Använd [Beeline](hdinsight-hadoop-use-hive-beeline.md) eller [Hive från en SSH-session](hdinsight-hadoop-use-hive-ssh.md) |

### <a name="pig"></a>Pig

| På Windows-baserade jag använda... | På Linux-baserade... |
| --- | --- |
| C# användardefinierade funktioner | Mer information om verifiering C#-komponenter med Linux-baserat HDInsight finns [migrera .NET lösningar tooLinux-baserat HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-filer eller skript på hello servern anropas som en del av ett Pig-jobb |Använd Bash-skript |

### <a name="mapreduce"></a>MapReduce

| På Windows-baserade jag använda... | På Linux-baserade... |
| --- | --- |
| C# mapper och reducer komponenter | Mer information om verifiering C#-komponenter med Linux-baserat HDInsight finns [migrera .NET lösningar tooLinux-baserat HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md) |
| CMD-filer eller skript på hello servern anropas som en del av ett Hive-jobb |Använd Bash-skript |

## <a name="oozie"></a>Oozie

> [!IMPORTANT]
> Om du använder en extern Oozie metastore bör du säkerhetskopiera hello metastore innan du använder med Linux-baserade HDInsight. Linux-baserat HDInsight finns i nyare versioner av Oozie som kan ha inkompatibiliteter med metastores som skapats i tidigare versioner.

Shell-åtgärder för att tillåta Oozie arbetsflöden. Shell-åtgärder använder hello standardgränssnitt för hello operativsystemet toorun kommandoradsverktyget kommandon. Om du har Oozie arbetsflöden som förlitar sig på hello Windows-gränssnittet, måste du skriva om hello arbetsflöden toorely på hello Linux shell-miljö (Bash). Läs mer om hur du använder shell åtgärder med Oozie [Oozie shell-tillägg för åtgärd](http://oozie.apache.org/docs/3.3.0/DG_ShellActionExtension.html).

Om du har Oozie arbetsflöden som förlitar sig på C#-program som anropas via gränssnittet åtgärder måste du verifiera dessa program i en Linux-miljö. Mer information finns i [migrera .NET lösningar tooLinux-baserat HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="storm"></a>Storm

| På Windows-baserade jag använda... | På Linux-baserade... |
| --- | --- |
| Storm-instrumentpanelen |hello Storm-instrumentpanelen är inte tillgänglig. Se [distribuera och hantera Storm-topologier på Linux-baserade HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) för sätt toosubmit topologier |
| Storm UI |hello Storm-Användargränssnittet är tillgänglig på https://CLUSTERNAME.azurehdinsight.net/stormui |
| Visual Studio toocreate, distribuera och hantera topologier för C# eller hybrid |Visual Studio kan använda toocreate, distribuera och hantera C# (SCP.NET) eller hybridtopologier på Linux-baserade Storm på HDInsight-kluster som skapas efter 2016/10/28. |

## <a name="hbase"></a>HBase

På Linux-baserade kluster hello znode överordnade för HBase är `/hbase-unsecure`. Ange ett värde i hello konfiguration för alla Java-klientprogram som använder interna HBase Java API.

Se [skapar ett Java-baserade HBase-program](hdinsight-hbase-build-java-maven.md) för en exempel-klient som anger det här värdet.

## <a name="spark"></a>Spark

Spark-kluster var tillgängliga på Windows-kluster under förhandsgranskningen. Spark GA är bara tillgänglig med Linux-baserade kluster. Det finns ingen migreringssökväg från en Windows-baserad Spark preview klustret tooa versionen Linux-baserade Spark-kluster.

## <a name="known-issues"></a>Kända problem

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory anpassade .NET-aktiviteter

Azure Data Factory anpassade .NET aktiviteter stöds inte på Linux-baserade HDInsight-kluster. I stället bör du använda någon av följande metoder tooimplement anpassade aktiviteter som en del av din ADF pipeline hello.

* Köra .NET-aktiviteter i Azure Batch-pool. I avsnittet hello används Azure Batch länkade tjänsten av [använda anpassade aktiviteter i ett Azure Data Factory-pipelinen](../data-factory/data-factory-use-custom-activities.md)
* Implementera hello aktiviteten som en MapReduce activity. Mer information finns i [anropa MapReduce program från Data Factory](../data-factory/data-factory-map-reduce.md).

### <a name="line-endings"></a>Radbrytningar

I allmänhet använder radbrytningar på Windows-datorer CRLF, medan Linux-baserade datorer använder LF. Om du skapar eller förväntar dig data med CRLF radbrytningar behöva toomodify hello producenter och konsumenter toowork med hello LF rad avslutas.

Till exempel returnerar med hjälp av Azure PowerShell tooquery HDInsight på Windows-baserade kluster data med CRLF. hello returnerar samma fråga med ett Linux-baserade kluster LF. Du bör testa toosee om hello rad avslutas orsakar problem med din solutuion innan du migrerar tooa Linux-baserade kluster.

Om du har skript som körs direkt på hello Linux-klusternoder, bör du alltid använda LF som hello rad avslutas. Om du använder CRLF får du felmeddelanden när du kör hello skript på en Linux-baserade kluster.

Om du vet att hello skript inte innehåller strängar med inbäddade CR tecken, kan du massimportera ändra hello radbrytningar med någon av följande metoder hello:

* **Innan du laddar upp toohello klustret**: Använd hello följande PowerShell-instruktioner toochange hello radbrytningar från CRLF tooLF innan du laddar upp hello skriptet toohello klustret.

    ```powershell
    $original_file ='c:\path\to\script.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)
    ```

* **Efter överföring toohello klustret**: Använd hello följande kommando från en SSH-session toohello Linux-baserade kluster toomodify hello skriptet.

    ```bash
    hdfs dfs -get wasb:///path/to/script.py oldscript.py
    tr -d '\r' < oldscript.py > script.py
    hdfs dfs -put -f script.py wasb:///path/to/script.py
    ```

## <a name="next-steps"></a>Nästa steg

* [Lär dig hur toocreate Linux-baserade HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md)
* [Använda SSH tooconnect tooHDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Hantera en Linux-baserade kluster med Ambari](hdinsight-hadoop-manage-ambari.md)
