---
title: installationen av aaaCluster av Hadoop, Spark, Kafka, HBase eller R - Server i Azure HDInsight | Microsoft Docs
description: "Konfigurera Hadoop, Kafka, Spark, HBase, R Server eller Storm-kluster för HDInsight från en webbläsare, hello Azure CLI, Azure PowerShell, REST eller SDK."
keywords: "konfiguration av hadoop, kafka kluster installationsprogrammet, spark-kluster installationsprogrammet vad är i hadoop-kluster"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 23a01938-3fe5-4e2e-8e8b-3368e1bbe2ca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/06/2017
ms.author: jgao
ms.openlocfilehash: 80ec59d8a39f7fccb940503fd2dc3ae5afee6bcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-clusters-in-hdinsight-with-hadoop-spark-kafka-and-more"></a>Ställ in kluster i HDInsight Hadoop, Spark, Kafka och mycket mer

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Lär dig hur tooset upp och konfigurera kluster i HDInsight med Hadoop, Spark, Kafka, interaktiva Hive, HBase, R Server eller Storm. Dessutom lär du dig hur toocustomize kluster och lägga till skydd genom att ansluta till dem tooa domän.

Ett Hadoop-kluster består av flera virtuella datorer (noder) som används för distribuerad bearbetning av uppgifter. Azure HDInsight hanterar implementeringsinformation om installation och konfiguration av enskilda noder, så du behöver bara tooprovide allmänna konfigurationsinformation. 

> [!IMPORTANT]
>HDInsight-kluster faktureringen påbörjas så snart ett kluster skapas och stoppas när hello kluster har tagits bort. Fakturering är proportionerligt per minut, så du bör alltid ta bort klustret när den inte längre används. Lär dig hur för[tar bort ett kluster.](hdinsight-delete-cluster.md)
>

## <a name="cluster-setup-methods"></a>Installationsmetoder för kluster
hello följande tabell visar hello olika metoder du kan använda tooset upp ett HDInsight-kluster.

| Kluster som skapas med | Webbläsare | Kommandorad | REST API | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |
| [Azure Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Snabbregistrering: grundläggande konfiguration
Den här artikeln vägleder dig genom installationen i hello [Azure-portalen](https://portal.azure.com), där du kan skapa ett HDInsight-kluster med *Snabbregistrering* eller *anpassad*. 

Följ instruktionerna på hello skärmen toodo en grundläggande konfiguration. Information ges nedan för:

* [Resursgruppens namn](#resource-group-name)
* [Klustertyper och konfiguration](#cluster-types) 
* [Klustrets inloggningsnamn och SSH-användarnamn](#cluster-login-and-ssh-username)
* [Plats](#location)

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight 3.3 pensionering](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>

## <a name="resource-group-name"></a>Resursgruppens namn 

[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) hjälper dig att arbeta med hello resurser i ditt program som en grupp, enligt tooas en Azure-resursgrupp. Du kan distribuera, uppdatera, övervaka eller ta bort alla hello resurser i programmet i en enda, samordnad åtgärd.

## <a name="cluster-types"></a>Klustertyper och konfiguration
Azure HDInsight innehåller för närvarande hello följande kluster typer, var och en med en uppsättning komponenter tooprovide vissa funktioner.

> [!IMPORTANT]
> HDInsight-kluster finns i olika typer för en enda arbetsbelastning eller teknik. Det finns ingen metod som stöds toocreate ett kluster som kombinerar flera typer, till exempel Storm och HBase på ett kluster. Om din lösning kräver tekniker som är fördelade på flera typer för HDInsight-kluster, en [virtuella Azure-nätverket](https://docs.microsoft.com/azure/virtual-network) kan ansluta klustertyper hello krävs. 
>
>

| Klustertyp | Funktioner |
| --- | --- |
| [Hadoop](hdinsight-hadoop-introduction.md) |Batch-fråga och analys av lagrade data |
| [HBase](hdinsight-hbase-overview.md) |För stora mängder schemalös, NoSQL-data bearbetades. |
| [Storm](hdinsight-storm-overview.md) |Händelsebearbetning i realtid |
| [Spark](hdinsight-apache-spark-overview.md) |Minnesintern bearbetning, interaktiva frågor dataströmmen micro batch-bearbetning |
| [Kafka (förhandsgranskning)](hdinsight-apache-kafka-introduction.md) | En distribuerad strömmande plattform som kan använda toobuild realtid strömmande data pipelines och program |
| [R Server](hdinsight-hadoop-r-server-overview.md) |Olika stordata statistik, förutsägelsemodellering och maskininlärning funktioner |
| [Interaktiva Hive (förhandsgranskning)](hdinsight-hadoop-use-interactive-hive.md) |Cachelagra i minnet för interaktiva och snabbare Hive-frågor |

### <a name="number-of-nodes-for-each-cluster-type"></a>Antalet noder för varje typ av kluster
Varje typ av kluster har sin egen antal noder, terminologi för noderna och standard VM-storlek. I följande tabell hello, är hello antalet noder för varje nodtyp inom parentes.

| Typ | Noder | Diagram |
| --- | --- | --- |
| Hadoop |Huvudnod (2), datanoden (1 +) |![HDInsight Hadoop klusternoder](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |HEAD-server (2), region-server (1 +), master/ZooKeeper-noden (3) |![HDInsight HBase klusternoder](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nimbus-noden (2), handledarens server (1 +), ZooKeeper-noden (3) |![Storm på HDInsight-klusternoder](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-storm-cluster-type-setup.png) |
| Spark |Huvudnod (2), arbetsnoden (1 +), ZooKeeper-noden (3) (kostnadsfritt för A1 ZooKeeper VM-storlek) |![HDInsight Spark klusternoder](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-spark-cluster-type-setup.png) |

Mer information finns i [standard nod konfiguration och virtuella storlekar för kluster](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) i ”vad är hello Hadoop-komponenterna och versioner i HDInsight”?

### <a name="hdinsight-version"></a>HDInsight-version
Välj hello version av HDInsight för det här klustret. Mer information finns i [stöds HDInsight-versioner](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="cluster-tiers"></a>Klustret nivå: tjänstnivåer för HDInsight

Azure HDInsight tillhandahåller hello molntjänster för stordata i två tjänstnivåer: Standard och Premium.  Mer information finns i [HDInsight Standard och HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).

hello visar följande skärmbild hello Azure portal-information för att välja klustertyper.

![HDInsight premium-konfiguration](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)


## <a name="cluster-login-and-ssh-user-name"></a>Klustrets inloggningsnamn och SSH-användarnamn
Du kan konfigurera två användarkonton när klustret skapas med HDInsight-kluster:

* HTTP-användare: hello Standardanvändarnamnet är *admin*. Grundläggande konfiguration för hello används på hello Azure-portalen. Den kallas ibland ”Cluster användare”.
* SSH-användare (Linux-kluster): används tooconnect toohello klustret via SSH. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

## <a name="location"></a>Plats (regioner) för kluster och lagring

Du behöver inte toospecify hello klusterplatsen explicit: hello klustret är i hello samma plats som hello standardlagring. En lista över regioner som stöds, klickar du på hello **Region** listrutan på [HDInsight priser](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Storage-slutpunkter för kluster

Även om en lokal installation av Hadoop använder hello Hadoop Distributed File System (HDFS) för lagring på hello kluster, i hello anslutna moln som du använder lagring slutpunkter toocluster. HDInsight-kluster använder antingen [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) eller [blobbar i Azure Storage](hdinsight-hadoop-use-blob-storage.md). Med hjälp av Azure Storage eller Azure Data Lake Store innebär kan du ta bort hello HDInsight-kluster som används för beräkning samtidigt behålla dina data. 

> [!WARNING]
> Med ett konto för ytterligare lagringsutrymme på en annan plats än hello HDInsight-kluster stöds inte.

Under konfigurationen för hello standardslutpunkten lagring anger du en blob-behållare för ett Azure Storage-konto eller ett Data Lake Store. hello standardlagring innehåller program- och loggar. Alternativt kan ange du ytterligare länkade Azure Storage-konton och Data Lake Store-konton som hello klustret kan komma åt. Hej HDInsight-kluster och hello beroende lagring konton måste finnas i hello samma Azure-plats.

![Inställningar för lagring: HDFS-kompatibla lagring slutpunkter](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-creation-storage.png)

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


### <a name="optional-metastores"></a>Valfritt metastores
Du kan skapa valfria Hive- eller Oozie metastores. Men inte alla klustertyper stöder metastores och Azure SQL Data Warehouse är inte kompatibelt med metastores. 

> [!IMPORTANT]
> När du skapar en anpassad metastore, Använd inte bindestreck, bindestreck eller blanksteg i hello databasnamnet. Detta kan orsaka hello klustret skapas processen toofail.

### <a name="use-hiveoozie-metastore"></a>Hive metastore

Om du vill tooretain Hive-tabeller när du tar bort ett HDInsight-kluster, kan du använda en anpassad metastore. Du kan sedan koppla hello metastore tooanother HDInsight-kluster.

Ett HDInsight-metastore som har skapats för ett HDInsight-kluster av version kan inte delas mellan olika versioner av HDInsight-kluster. En lista över HDInsight-versioner, se [stöds HDInsight-versioner](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Oozie metastore

tooincrease prestanda när du använder Oozie, använda en anpassad metastore. En metastore kan också ge åtkomst tooOozie jobbdata när du tar bort klustret. 

> [!IMPORTANT]
> Du kan inte återanvända en anpassad Oozie metastore. toouse en anpassad Oozie metastore måste du ange en tom Azure SQL Database när du skapar hello HDInsight-kluster.

## <a name="configure-cluster-size"></a>Konfigurera klustrets storlek

Du debiteras för användning av noden för länge hello klustret finns. Faktureringen påbörjas när ett kluster skapas och stoppas när hello kluster har tagits bort. Kluster kan inte frigöra allokerade eller spärra.

hello kostnaden för HDInsight-kluster bestäms av hello antal noder och hello storlekar för virtuella datorer för hello noder. 

Olika klustertyper har olika nodtyper, antal noder och noden storlekar:
* Standardtyp för Hadoop-kluster: 
    * Två *huvudnoder*  
    * Fyra *datanoder*
* Standardtyp för storm-kluster: 
    * Två *Nimbus-noder*
    * Tre *ZooKeeper-noder*
    * Fyra *handledarens noder* 

Om du försöker bara i HDInsight, rekommenderar vi att du använder en datanod. Läs mer om prissättningen för HDInsight [HDInsight priser](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> storleksgräns för hello klustret varierar mellan olika Azure-prenumerationer. Kontakta [Azure faktureringssupporten](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) tooincrease hello gränsen.
>

När du använder hello Azure portal tooconfigure hello kluster hello nodstorlek är tillgänglig via hello **nods prisnivå** bladet. Hello-portalen kan du också se hello kostnaden som är associerad med hello annan nod. 

![HDInsight VM nod storlekar](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Storlekar för virtuella datorer 
När du distribuerar kluster, Välj beräkningsresurser baserat på hello lösning du planerar toodeploy. hello används följande virtuella datorer för HDInsight-kluster:
* A och D1 4 serien VMs: [General-purpose Linux VM-storlekar](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* D11 14 serien VM: [minnesoptimerade Linux VM-storlekar](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

toofind ut vilket värde som du ska använda toospecify en VM-storlek när du skapar ett kluster med hjälp av hello olika SDK eller när du använder Azure PowerShell, se [VM-storlekar toouse för HDInsight-kluster](../cloud-services/cloud-services-sizes-specs.md#size-tables). Använd hello värdet från den här länkade artikeln i hello **storlek** kolumn med hello tabeller.

> [!IMPORTANT]
> Om du behöver mer än 32 worker-noder i ett kluster, måste du välja en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne.
>
>

Mer information finns i [storlekar för virtuella datorer](../virtual-machines/windows/sizes.md). Mer information om priser för hello olika storlekar finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="custom-cluster-setup"></a>Anpassad konfiguration
Anpassade kluster installationen bygger på hello snabbt skapa inställningar och lägger till hello följande alternativ:
- [HDInsight-program](#hdinsight-applications)
- [Klusterstorleken](#cluster-size)
- Avancerade inställningar
  - [Skriptåtgärder](#customize-clusters-using-script-action)
  - [Virtuellt nätverk](#use-virtual-network)

## <a name="install-hdinsight-applications-on-clusters"></a>Installera HDInsight-program i kluster

Ett HDInsight-program är ett program som användarna kan installera på ett Linux-baserat HDInsight-kluster. Du kan använda program, under förutsättning av Microsoft, tredje part eller som du utvecklar själv. Mer information finns i [installera Hadoop-program från tredje part på Azure HDInsight](hdinsight-apps-install-applications.md).

De flesta av hello HDInsight-program är installerade på en tom kantnod.  En tom kantnod är en virtuell Linux-dator med hello samma klientverktyg installeras och konfigureras som hello huvudnod. Du kan använda hello kantnod för att komma åt hello kluster, testa ditt klientprogram och värd för ditt program. Mer information finns i [använda tom edge-noder i HDInsight](hdinsight-apps-use-edge-node.md).

## <a name="advanced-settings-script-actions"></a>Avancerade inställningar: Script åtgärder

Du kan installera ytterligare komponenter eller anpassa klustrets konfiguration med hjälp av skript när du skapar. Dessa skript anropas **skriptåtgärd**, vilket är ett konfigurationsalternativ som kan användas från hello Azure-portalen, HDInsight Windows PowerShell-cmdlets eller hello HDInsight .NET SDK. Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

Vissa inbyggda Java-komponenter som Mahout och kaskad, kan köras på hello klustret som Java Arkiv (JAR) filer. Dessa JAR-filer kan vara distribuerade tooAzure lagring och skickades tooHDInsight kluster med Hadoop-jobbet skicka mekanismer. Mer information finns i [skicka Hadoop-jobb via programmering](hdinsight-submit-hadoop-jobs-programmatically.md).

> [!NOTE]
> Om du har problem med distribution av JAR-filer tooHDInsight kluster eller anropa JAR-filer på HDInsight-kluster, kontakta [Microsoft-supporten](https://azure.microsoft.com/support/options/).
>
> Sammanhängande stöds inte av HDInsight och är inte tillgänglig för Microsofts Support. Listor med stödda komponenter finns [vad är nytt i hello-klusterversioner som tillhandahålls av HDInsight](hdinsight-component-versioning.md).
>
>

Ibland vill du tooconfigure hello följande konfigurationsfiler under skapandeprocessen hello:

* clusterIdentity.xml
* Core-site.xml
* gateway.XML
* hbase env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-plats
* oozie-site.xml
* oozie env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Mer information finns i [anpassa HDInsight-kluster med Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Avancerade inställningar: utöka kluster med ett virtuellt nätverk
Om din lösning kräver tekniker som är fördelade på flera typer för HDInsight-kluster, en [virtuella Azure-nätverket](https://docs.microsoft.com/azure/virtual-network) kan ansluta klustertyper hello krävs. Den här konfigurationen tillåter hello kluster och all kod som du distribuerar toothem, toodirectly kommunicera med varandra.

Mer information om hur du använder Azure-nätverk med HDInsight finns [utöka HDInsight med virtuella Azure-nätverk](hdinsight-extend-hadoop-virtual-network.md).

Ett exempel på hur du använder två typer av klustret i Azure-nätverk finns [analysera sensordata med Storm och HBase](hdinsight-storm-sensor-data-analysis.md). Läs mer om hur du använder HDInsight med ett virtuellt nätverk, inklusive konfigurationskraven för hello virtuellt nätverk, [utöka HDInsight funktioner med hjälp av Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

## <a name="troubleshoot-access-control-issues"></a>Felsökning av problem med åtkomstkontroll

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg

- [Vad är HDInsight, hello Hadoop-ekosystemet och Hadoop-kluster?](hdinsight-hadoop-introduction.md)
- [Komma igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Fungera med Hadoop i HDInsight från en Windows-dator](hdinsight-hadoop-windows-tools.md)
