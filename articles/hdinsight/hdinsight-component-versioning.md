---
title: aaaHadoop komponenter och versioner - Azure HDInsight | Microsoft Docs
description: "Lär dig hello Hadoop-komponenter och -versioner i HDInsight och hello servicenivåer finns i molnet vid distribution av Hortonworks Data Platform."
keywords: hadoop-versioner, komponenterna i hadoop-ekosystemet, hadoop-komponenter, hur toocheck hadoop-version
services: hdinsight
editor: cgronlun
manager: asadk
author: bprakash
tags: azure-portal
documentationcenter: 
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: bprakash
ms.openlocfilehash: b661d901b0113458c3501ec06454fc8841189672
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-hello-hadoop-components-and-versions-available-with-hdinsight"></a>Vad är hello Hadoop-komponenterna och versioner som är tillgängliga med HDInsight?

Lär dig mer om hello Apache Hadoop-ekosystemet komponenter och -versioner i Microsoft Azure HDInsight samt hello servicenivåer för Standard- och Premium. Lär dig också hur toocheck Hadoop komponenten versioner i HDInsight. 

Varje HDInsight-version är en cloud fördelning av en version av Hortonworks Data Platform (HDP).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Tillgängliga med olika versioner av HDInsight Hadoop-komponenter
Azure HDInsight har stöd för flera Hadoop-klusterversioner som kan distribueras när som helst. Varje version alternativ skapas en viss version av hello HDP distribution och en uppsättning komponenter som ingår i distributionen. Från och med den 17 februari 2017 hello klustret standardversionen används av Azure HDInsight är 3.5 och baseras på HDP 2.5.

hello komponenten versioner som är associerade med HDInsight-kluster-versioner finns i hello i den följande tabellen. 

> [!NOTE]
> hello standardversionen för hello HDInsight-tjänst kan ändras utan föregående meddelande. Om du har ett beroende ange hello HDInsight version när du skapar ditt kluster med hello .NET SDK med Azure PowerShell och Azure CLI.

| Komponent | HDInsight 3,6 (standard) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 | HDInsight 3.1 | HDInsight 3.0 |
| --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks Data Platform |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop och YARN |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive och HCatalog |1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez Hive2 | 0.8.4 |-|-|-|-|-|-|
| Apache Ranger | 0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.1.0 (endast Linux) |1.6.2 + 2.0 (endast Linux) |1.6.0 (endast Linux) |1.5.2 (Linux experiment build endast) |1.3.1 (endast Windows) |-|-|
| Apache Kafka | 0.10.0 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.5.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |3.2.8 |-|-|-|

## <a name="check-for-current-hadoop-component-version-information"></a>Sök efter aktuell versionsinformation för Hadoop-komponent

Hej Hadoop-ekosystemet komponenten versioner som är associerade med HDInsight-kluster-versioner kan ändra med uppdateringar tooHDInsight. toocheck hello Hadoop-komponenter och tooverify vilka versioner som används för ett kluster använder du hello Ambari REST API. Hej **GetComponentInformation** kommando hämtar information om tjänstens komponenter. Mer information finns i hello [Ambari dokumentationen][ambari-docs].

I Windows-kluster är ett annat sätt toocheck hello komponentversionen är toolog i tooa kluster med hjälp av fjärrskrivbord och undersök hello innehållet i hello C:\apps\dist\ katalogen.

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [Windows pensionering på HDInsight](#hdinsight-windows-retirement).

### <a name="release-notes"></a>Viktig information

Se [HDInsight viktig information](hdinsight-release-notes.md) för ytterligare viktig information på hello senaste versionerna av HDInsight.

## <a name="supported-hdinsight-versions"></a>HDInsight-versioner som stöds
hello visar följande tabell hello versioner av HDInsight som är tillgängliga på hello Azure-portalen. Hej HDP versioner som motsvarar tooeach HDInsight-version visas tillsammans med hello produkten frisläppningsdatum. hello stöd för förfallodatum och tillbakadragning datum finns också när de är kända.

> [!NOTE]
> När du har stöd för en version har gått ut, kanske den inte tillgängliga via klassiska hello Microsoft Azure-portalen. Kluster-versioner som fortfarande toobe med hello `Version` parameter i hello Windows PowerShell [ny AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) kommando och hello .NET SDK tills hello version datumet för tillbakadragandet.
> 
> Hög tillgänglighet kluster med två noder som head distribueras som standard för HDInsight version 2.1 och senare. De är inte tillgängliga för version 1.6 HDInsight-kluster.

| HDInsight-version | HDP version | VM OS | Hög tillgänglighet | Utgivningsdatum | Tillgänglighet på hello Azure-portalen | Stöd för förfallodatum | Datumet för tillbakadragandet |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3,6 |HDP 2.6 |Ubuntu 16 |Ja |4 april 2017 |Ja | | |
| HDInsight 3.5 |HDP 2,5 |Ubuntu 16 |Ja |30 september 2016 |Ja |5 september 2017 |Den 31 maj 2018 |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |Ja |Den 29 mars 2016 |Ja |Den 29 december 2016 |9 januari 2018 |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |Ja |Den 2 december 2015 |Ja |27 juni 2016 |31 juli 2018 |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |Ja |Den 2 december 2015 |Ja |27 juni 2016 |31 juli 2017 |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS eller Windows Server 2012 R2 |Ja |18 februari 2015 |Nej |Den 1 mars 2016 |1 april 2017 |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |Ja |24 juni 2014 |Nej |18 maj 2015 |30 juni 2016 |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |Ja |11 februari 2014 |Nej |17 september 2014 |30 juni 2015 |
| HDInsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |Ja |28 oktober 2013 |Nej |12 maj 2014 |Den 31 maj 2015 |
| HDInsight 1.6 |HDP 1.1 | |Nej |28 oktober 2013 |Nej |26 april 2014 |Den 31 maj 2015 |

## <a name="hdinsight-windows-retirement"></a>HDInsight Windows ur bruk
Microsoft Azure HDInsight version 3.3 var hello senaste versionen av HDInsight på Windows. hello är datumet för tillbakadragandet för HDInsight på Windows 31 juli 2018. Om du har alla HDInsight-kluster på Windows 3.3 eller tidigare, måste du migrera tooHDInsight på Linux (HDInsight version 3.5 eller senare) innan den 31 juli 2018. Migrera toohello Linux OS kan du tooretain hello möjlighet toocreate eller ändra storlek på dina HDInsight-kluster. Stöd för HDInsight version 3.3 i Windows har upphört att gälla på 27 juni 2016.

Från och med HDInsight version 3.4, har Microsoft släppt HDInsight endast på hello Operativsystemet Linux. Därför kan är vissa av hello komponenter i HDInsight tillgängliga för Linux endast. Dessa omfattar Apache Ranger, Kafka, interaktiva Hive, Spark HDInsight-program och Azure Data Lake Store som hello primära filsystem. Framtida versioner av HDInsight är bara tillgängliga på hello Operativsystemet Linux. Det blir inga kommande versioner av HDInsight på Windows. 

## <a name="faqs"></a>Vanliga frågor och svar

### <a name="what-is-hello-timeline-for-retiring-hdinsight-on-windows"></a>Vad är hello tidslinjen för att ta bort HDInsight på Windows?
Den 31 juli 2018 är hello datumet för tillbakadragandet för HDInsight på Windows. Om hello planerade datumet för tillbakadragandet är olika för din region, du meddelas separat. 

### <a name="what-is-hello-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>Vad är hello effekten av att ta bort HDInsight på Windows för befintliga kunder?
När HDInsight på Windows har dragits tillbaka, kan du skapa ett nytt kluster i HDInsight Windows eller ändra storlek på ett befintligt HDInsight Windows-kluster. Stöd för HDInsight version 3.3 upphörde 27 juni 2016. Det finns därför ingen support eller felkorrigeringar för HDInsight 3.3 eller tidigare versioner. Framtida versioner av HDInsight är bara tillgängliga på hello Operativsystemet Linux. Det blir inga kommande versioner av HDInsight på Windows.
 
### <a name="which-versions-of-hdinsight-on-windows-are-affected"></a>Vilka versioner av HDInsight på Windows påverkas?
Azure HDInsight version 3.3 är hello senaste versionen av HDInsight för Windows. Innan HDInsight på Windows har dragits tillbaka migreras alla HDInsight Windows kluster 3.3 eller tidigare måste du ha tooHDInsight på Linux version 3.5 eller senare. Migrera din tooHDInsight kluster på Linux kan du tooretain hello möjlighet toocreate nya kluster eller ändra storlek på befintliga kluster. 

### <a name="what-do-i-need-toodo"></a>Vad gör jag behöver toodo?
Migrera HDInsight Windows-kluster tooa stöds HDInsight Linux klustret innan den 31 juli 2018. Läs mer i hello [HDInsight migrering dokumentet](https://docs.microsoft.com/en-gb/azure/hdinsight/hdinsight-migrate-from-windows-to-linux). Mer information om Azure HDInsight versioner finns hello lista över [versioner stöds](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions). 

### <a name="where-do-i-find-hello-cluster-os-type"></a>Var hittar jag hello klustret OS-typen?
Gå toohello översiktssidan för HDInsight-kluster i hello Azure-portalen, och leta upp **kluster typen** under **Essentials**. hello OS klustertyper visas på sidan. 

### <a name="i-cant-migrate-tooan-hdinsight-linux-cluster-by-july-31-2018-what-is-hello-impact-toomy-hdinsight-windows-cluster"></a>Jag kan inte migrera tooan HDInsight Linux-kluster av 31 juli 2018. Vad är hello påverkan toomy HDInsight Windows-kluster?
Hej HDInsight Windows-kluster kör som-är, men du kan inte skapa ett nytt kluster i HDInsight Windows eller ändra storlek på ett befintligt HDInsight Windows-kluster. 

### <a name="my-cluster-has-a-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>Min klustret har en .NET-beroendet. Hur löser det här beroendet av Linux?
Du kan lösa dina beroende för Linux-kluster med hjälp av hello [monoljud projekt](http://www.mono-project.com/). Den här öppen källkod implementering av .NET är tillgängligt för kluster i HDInsight Linux. Läs mer i hello [HDInsight migrering dokumentet](https://docs.microsoft.com/en-gb/azure/hdinsight/hdinsight-migrate-from-windows-to-linux). 

### <a name="im-a-new-customer-for-hdinsight-on-windows-how-can-i-create-an-hdinsight-windows-cluster"></a>Jag är en ny kund för HDInsight på Windows. Hur kan jag skapa en HDInsight Windows-kluster?
Från och med den 3 juli 2017 kan endast befintliga HDInsight Windows-kunder skapa nya HDInsight fönster kluster. Nya kunder kan inte skapa ett kluster i HDInsight Windows hello Azure-portalen med hjälp av PowerShell eller hello SDK. Vi rekommenderar att nya kunder skapar ett Linux HDInsight-kluster. Befintliga kunder kan skapa nya HDInsight Windows kluster för tills hello HDInsight på Windows datumet för tillbakadragandet. 

### <a name="is-there-a-pricing-impact-associated-with-moving-from-hdinsight-on-windows-toohdinsight-on-linux"></a>Finns det en prisnivå inverkan som är associerade med flytta från HDInsight på Windows tooHDInsight på Linux?
Nej, hello priser är hello samma för HDInsight på antingen OS. 

### <a name="what-are-hello-customer-advantages-associated-with-hello-move-tooonly-using-hdinsight-on-linux"></a>Vad är hello Kundfördelar som är associerade med hello flytta tooonly med HDInsight på Linux?
* Snabbare tid till marknad för öppen källkod stordata tekniker via hello HDInsight-tjänst
* En stor community och ekosystem för support
* Möjlighet tooexercise aktiv utveckling av hello öppen källkod för Hadoop och andra tekniker för stordata

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-what-is-available-in-hdinsight-on-windows"></a>HDInsight på Linux kan ge ytterligare funktioner utöver vad som är tillgängliga i HDInsight på Windows?
Från och med HDInsight version 3.4, har Microsoft släppt HDInsight endast på hello Operativsystemet Linux. Därför kan är vissa av hello komponenter i HDInsight tillgängliga för Linux endast. Dessa omfattar Apache Ranger, Kafka, interaktiva Hive, Spark HDInsight-program och Azure Data Lake Store som hello primära filsystem. 

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>Servicenivåavtalet för HDInsight-kluster-versioner
hello servicenivåavtal (SLA) har definierats i en _stöd fönstret_. hello stöd fönstret är hello tidsperiod som ett HDInsight-kluster av version stöds av Microsofts kundservice och Support. Om hello-versionen har en _stöd för förfallodatum_ som har skickats, hello HDInsight-kluster är utanför hello stöd fönster. Mer information om versioner som stöds finns i hello lista över [HDInsight-kluster-versioner som stöds](https://docs.microsoft.com/en-gb/azure/hdinsight/hdinsight-migrate-from-windows-to-linux). hello support upphör att gälla för en angiven HDInsight version X (när det finns en nyare version X + 1) beräknas som hello senare av:  

* Formeln 1: Lägg till 180 dagar toohello datum när hello HDInsight-klusterversionen X släpptes.
* Formeln 2: Lägg till 90 dagar toohello datum när hello HDInsight-klusterversionen X + 1 görs tillgängliga i Azure-portalen.

Hej _datumet för tillbakadragandet_ är hello datum efter vilket hello-kluster av version inte kan skapas på HDInsight. Du kan inte ändra storlek ett HDInsight-kluster efter datumet för tillbakadragandet startar den 31 juli 2017. 

> [!NOTE]
> HDInsight Windows-kluster (inklusive versioner 2.1, 3.0, 3.1, 3.2 eller 3.3) körs på Azure OS-Gästfamiljen version 4, som använder hello 64-bitars version av Windows Server 2012 R2. Azure OS-Gästfamiljen version 4 stöder hello .NET Framework version 4.0, 4.5, 4.5.1 och 4.5.2.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks viktig information som är associerade med HDInsight-versioner

hello avsnittet länkar toorelease anteckningar för hello Hortonworks Data Platform distributioner och Apache-komponenter som används med HDInsight.
* HDInsight-kluster av version 3,6 använder ett Hadoop-distributionen som baseras på [Hortonworks Data Platform 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* HDInsight-kluster av version 3.5 använder ett Hadoop-distributionen som baseras på [Hortonworks Data Platform 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). HDInsight-kluster av version 3.5 är hello _standard_ Hadoop-kluster som skapas i hello Azure-portalen.
* HDInsight-kluster av version 3.4 använder ett Hadoop-distributionen som baseras på [Hortonworks Data Platform 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* HDInsight-kluster av version 3.3 använder ett Hadoop-distributionen som baseras på [Hortonworks Data Platform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Apache Storm viktig information](https://storm.apache.org/2015/11/05/storm0100-released.html) är tillgängliga på hello Apache-webbplatsen.
  * [Apache Hive viktig information](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) är tillgängliga på hello Apache-webbplatsen.
* HDInsight-kluster av version 3.2 använder ett Hadoop-distributionen som baseras på [Hortonworks Data Platform 2.2][hdp-2-2].

  * Viktig information för specifika Apache-komponenter finns på följande sätt: [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [vanliga](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), och [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight-kluster av version 3.1 använder ett Hadoop-distributionen som baseras på [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. 3.1 HDInsight-kluster som skapats före November, 7, 2014, baserat på [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* HDInsight-kluster av version 3.0 använder ett Hadoop-distributionen som baseras på [Hortonworks Data Platform 2.0][hdp-2-0-8].
* HDInsight-kluster av version 2.1 använder ett Hadoop-distributionen som baseras på [Hortonworks Data Platform 1.3][hdp-1-3-0].
* HDInsight-kluster av version 1.6 använder ett Hadoop-distributionen som baseras på [Hortonworks Data Platform 1.1][hdp-1-1-0].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard och HDInsight Premium

Azure HDInsight tillhandahåller hello molntjänster för stordata i två kategorier: _Standard_ och _Premium_. hello följande tabell innehåller funktioner som är tillgängliga _endast_ i HDInsight Premium. Funktioner som inte uttryckligen beskrivs i hello tabell är tillgängliga i både HDInsight Standard och Premium.

> [!NOTE]
> Hej HDInsight Premium erbjudande är för närvarande under förhandsgranskning och kan bara Linux-kluster.

| HDInsight Premium-funktion | Beskrivning |
| --- | --- |
| Domänanslutna HDInsight-kluster |Ansluta till HDInsight-kluster tooAzure Active Directory (AD Azure) domäner för säkerhet på företagsnivå. Du kan konfigurera en lista över anställda från ditt företag som kan autentisera till Azure AD toolog på tooan HDInsight-kluster i HDInsight Premium. Hej enterprise administratör kan konfigurera rollbaserad åtkomstkontroll för Hive-säkerhet med hjälp av [Apache Ranger](http://hortonworks.com/apache/ranger/) och begränsa data access toouse endast så mycket som behövs. Slutligen Hej administratör kan granska data som nås av anställda och ändringar tooaccess styr principer, vilket bidrar till att en hög grad av styrning av företagets resurser. Mer information finns i [konfigurera domänanslutna HDInsight-kluster](hdinsight-domain-joined-configure.md). |

### <a name="cluster-types-supported-in-hdinsight-premium"></a>Klustertyper som stöds i HDInsight Premium
hello visar följande tabell hello klustret typer som stöds i HDInsight Premium.

| Klustertyp | Standard | Premium (förhandsgranskning) |
| --- | --- | --- |
| Hadoop |Ja |Ja (endast HDInsight 3,6) |
| Spark |Ja |Nej |
| HBase |Ja |Nej |
| Storm |Ja |Nej |
| R-server |Ja |Nej |
| Interaktiva Hive (förhandsgranskning) |Ja |Nej |
| Kafka (förhandsgranskning) |Ja |Nej | 

### <a name="support-for-azure-data-lake-store-in-hdinsight-premium"></a>Stöd för Azure Data Lake Store i HDInsight Premium

HDInsight Premium kluster stöder inte användning av Azure Data Lake Store som primär lagring. Du kan dock använda Azure Data Lake Store som tilläggslagring med HDInsight Premium-kluster.

### <a name="pricing-and-sla"></a>Priser och SLA
Information om priser och SLA för HDInsight Premium finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Standard nod konfiguration och virtuella storlekar för kluster
hello följande tabeller listan hello standard (VM) storlekar för virtuella datorer för HDInsight-kluster.

> [!IMPORTANT]
> Om du behöver mer än 32 worker-noder i ett kluster, måste du välja en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne.
> 
> 

* Alla regioner utom södra och västra Japan som stöds:

  | Klustertyp | Hadoop | HBase | Storm | Spark | R-server |
  | --- | --- | --- | --- | --- | --- |
  | Huvud: standard VM-storlek |D3 v2 |D3 v2 |A3 |D12 v2 |D12 v2 |
  | Huvud: rekommenderade storlekar på VM |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |A3, A4, A5 |D12 v2, D13 v2, D14 v2 |D12 v2, D13 v2, D14 v2 |
  | Worker: standard VM-storlek |D3 v2 |D3 v2 |D3 v2 |Windows: D12 v2; Linux: D4 v2 |Windows: D12 v2; Linux: D4 v2 |
  | Worker: rekommenderade storlekar på VM |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |Windows: D12 v2 D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |Windows: D12 v2 D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
  | ZooKeeper: standard VM-storlek | |A3 |A2 | | |
  | ZooKeeper: rekommenderade storlekar på VM | |A3, A4, A5 |A2, A3, A4 | | |
  | Kant: standard VM-storlek | | | | |Windows: D12 v2; Linux: D4 v2 |
  | Edge: rekommenderas VM-storlek | | | | |Windows: D12 v2 D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
* Södra och västra Japan endast (ingen v2-storlekar):

  | Klustertyp | Hadoop | HBase | Storm | Spark | R-server |
  | --- | --- | --- | --- | --- | --- |
  | Huvud: standard VM-storlek |D3 |D3 |A3 |D12 |D12 |
  | Huvud: rekommenderade storlekar på VM |D3 D4, D12 |D3 D4, D12 |A3, A4, A5 |D12 D13 D14 |D12 D13 D14 |
  | Worker: standard VM-storlek |D3 |D3 |D3 |Windows: D12; Linux: D4 |Windows: D12; Linux: D4 |
  | Worker: rekommenderade storlekar på VM |D3 D4, D12 |D3 D4, D12 |D3 D4, D12 |Windows: D12 D13 D14; Linux: D4, D12 D13 D14 |Windows: D12 D13 D14; Linux: D4, D12 D13 D14 |
  | ZooKeeper: standard VM-storlek | |A2 |A2 | | |
  | ZooKeeper: rekommenderade storlekar på VM | |A2, A3, A4 |A2, A3, A4 | | |
  | Kant: standard VM-storlekar | | | | |Windows: D12; Linux: D4 |
  | Edge: rekommenderade storlekar på VM | | | | |Windows: D12 D13 D14; Linux: D4, D12 D13 D14 |

> [!NOTE]
> - HEAD kallas *Nimbus* hello Storm kluster, typen.
> - Worker kallas *handledarens* hello Storm kluster, typen.
> - Worker kallas *Region* hello HBase kluster, typen.

## <a name="next-steps"></a>Nästa steg
- [Klustret installationsprogrammet för Hadoop, Spark och mycket mer på HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Fungera med Hadoop i HDInsight från en Windows-dator](hdinsight-hadoop-windows-tools.md)

[Supported HDInsight versions]:(#supported-hdinsight-versions)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
