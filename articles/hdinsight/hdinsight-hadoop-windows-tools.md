---
title: aaaUse en Windows-dator med Hadoop i HDInsight - Azure | Microsoft Docs
description: "Arbeta från en Windows-dator i Hadoop på HDInsight. Hantera och fråga kluster med PowerShell-, Visual Studio- och Linux-verktyg. Utveckla stordatalösningar med .NET."
services: hdinsight
keywords: "hadoop på windows, hadoop för windows"
author: cjgronlund
manager: jhubbard
ms.author: cgronlun
ms.date: 05/17/2017
ms.topic: article
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 7c93f16e93349c0b8fb1abd55320c2c172b93aa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="work-in-hello-hadoop-ecosystem-on-hdinsight-from-a-windows-pc"></a>Arbeta i hello Hadoop-ekosystemet i HDInsight från en Windows-dator

Läs mer om utveckling och hantering av alternativen på hello Windows-dator för att arbeta i hello Hadoop-ekosystemet i HDInsight. 

HDInsight är baserad på Apache Hadoop och Hadoop-komponenter, öppen källkod tekniker som utvecklats på Linux. HDInsight version 3.4 och högre använder hello Ubuntu Linux-distribution som hello underliggande OS för hello-kluster. Du kan dock arbeta med HDInsight från en Windows-klienten eller Windows-utvecklingsmiljö.

## <a name="use-powershell-for-deployment-and-management-tasks"></a>Använda PowerShell för distribution och hantering av uppgifter
Azure PowerShell är en skriptmiljö som du kan använda toocontrol och automatisera distribution och hantering av uppgifter i HDInsight från Windows.

Exempel på uppgifter som du kan göra med PowerShell:

* [Skapa kluster med hjälp av PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Köra Hive-frågor med hjälp av PowerShell](hdinsight-hadoop-use-hive-powershell.md)
* [Hantera kluster med PowerShell](hdinsight-administer-use-powershell.md)

Följ stegen för[installera och konfigurera Azure Powershell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooget hello senaste versionen. Om du har skript som behöver ändras toobe toouse hello nya cmdlets för Azure Resource Manager, se [migrera tooAzure Resource Manager-baserade utvecklingsverktyg för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="utilities-you-can-run-in-a-browser"></a>Verktyg som du kan köra i en webbläsare
hello har följande verktyg ett webbgränssnitt som körs i en webbläsare:
* **[Azure Cloud Shell (förhandsgranskning)](https://docs.microsoft.com/azure/cloud-shell/quickstart)**  är en interaktiv, kommandoradsverktyg skal som körs i webbläsaren och inifrån hello Azure-portalen.
* **[Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md)**  är en hantering och övervakning av verktyg som är tillgängliga i hello Azure-portalen som kan använda toomanage olika typer av jobb, som:
    * [Använda Ambari med hello REST API](hdinsight-hadoop-manage-ambari-rest-api.md)
    * [Hive-vyn i Ambari](hdinsight-hadoop-use-hive-ambari-view.md)
    * [Tez-vyn i Ambari](hdinsight-debug-ambari-tez-view.md)

## <a name="data-lake-hadoop-tools-for-visual-studio"></a>Data Lake (Hadoop)-verktyg för Visual Studio
Använd Data Lake-verktyg för Visual Studio toodeploy och hantera Storm-topologier. Data Lake-verktyg installeras också hello SCP.NET SDK, som gör att du toodevelop C# Storm-topologier med Visual Studio.

Innan du går vidare toohello följande exempel, [installera och försök Data Lake-verktyg för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md). 

Exempel på uppgifter som du kan göra med Visual Studio och Data Lake-verktyg för Visual Studio:
* [Distribuera och hantera Storm-topologier från Visual Studio](hdinsight-storm-deploy-monitor-topology-linux.md)
* [Utveckla C#-topologier för Storm med Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md). hello bits innehåller exempel mallar för Storm-topologier kan du ansluta toodatabases, till exempel Azure Cosmos DB och SQL-databas.

## <a name="visual-studio-and-hello-net-sdk"></a>Visual Studio och hello .NET SDK 

Du kan använda Visual Studio med hello .NET SDK toomanage kluster och utveckla program för stordata. Du kan använda andra IDEs för hello följande uppgifter, men exempel visas i Visual Studio.

Exempel på uppgifter som du kan göra med hello .NET SDK i Visual Studio:
* [Skapa kluster och arbeta i HDInsight från .NET Framework-program](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)
* [Köra Hive-frågor med hello .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md)
* [Använda C# användardefinierade funktioner med Hive och Pig strömning på Hadoop](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

> Tips om du kör .NET lösningar med Windows-baserade HDInsight-kluster, är det en bra tillfälle tooplan en migrering tooLinux-baserade kluster. Mer information finns i [migrera .NET lösning för Windows-baserade HDInsight tooLinux-baserat HDInsight](hdinsight-hadoop-migrate-dotnet-to-linux.md).

## <a name="intellij-idea-and-eclipse-ide-for-spark-clusters"></a>Intellij IDEA och Eclipse IDE för Spark-kluster
Båda [Intellij IDEA](https://www.jetbrains.com/idea/download) och hello [Eclipse IDE](https://www.eclipse.org/downloads/) användas för att:
* Utveckla och skicka Scala Spark-program på ett HDInsight Spark-kluster.
* Få åtkomst till Spark-kluster.
* Utveckla och köra ett Scala Spark-program lokalt.

Dessa artiklar visa hur: 
* Intellij IDEA: [skapa Spark-program med hjälp av hello Azure Toolkit för plugin-programmet Intellij och hello Scala SDK.](hdinsight-apache-spark-intellij-tool-plugin.md)
* IDE- eller Scala IDE för Eclipse-solförmörkelse: [skapa Spark-program och hello Azure Toolkit för Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) 


## <a name="notebooks-on-spark-for-data-scientists"></a>Datorer på Spark för datavetare 
Apache Spark-kluster i HDInsight innehåller Zeppelin-anteckningsböcker och kernlar som kan användas med Jupyter-anteckningsböcker. 

* [Lär dig hur toouse kärnor på Spark-kluster med Jupyter-anteckningsböcker tootest Spark-program](hdinsight-apache-spark-zeppelin-notebook.md)
* [Lär dig hur toouse Zeppelin-anteckningsböcker på Spark-kluster toorun Spark jobb](hdinsight-apache-spark-jupyter-notebook-kernels.md) 


## <a name="run-linux-based-tools-and-technologies-on-windows"></a>Kör Linux-baserat verktyg och tekniker i Windows

Om du får en situation där du måste använda ett verktyg eller en teknik som endast är tillgängliga på Linux, bör du hello följande alternativ:

* **Bash (beta) i Windows 10** ger en Linux-undersystemet i Windows. Bash kan toodirectly som kör Linux-verktyg utan att behöva toomaintain en dedikerad Linux-installation. [Installera och köra hello Bash beta på Windows 10](https://msdn.microsoft.com/commandline/wsl/install_guide)
* **Docker för Windows** ger åtkomst toomany Linux-baserat verktyg och kan köras direkt från Windows. Du kan till exempel använda Docker toorun hello Beeline klienten för Hive direkt från Windows. Du kan också använda Docker toorun en lokal Jupyter-anteckningsbok och fjärransluta tooSpark på HDInsight. [Kom igång med Docker för Windows](https://docs.docker.com/docker-for-windows/)
* **[MobaXTerm](http://mobaxterm.mobatek.net/)**  tillåter toographically Bläddra hello klustret filsystemet via en SSH-anslutning.

## <a name="next-steps"></a>Nästa steg
Om du är ny tooworking i Linux-baserade kluster, se hello följer artiklar:
* [Konfigurera Hadoop, Kafka, Spark eller andra kluster](hdinsight-hadoop-provision-linux-clusters.md)
* [Tips för HDInsight-kluster på Linux](hdinsight-hadoop-linux-information.md)