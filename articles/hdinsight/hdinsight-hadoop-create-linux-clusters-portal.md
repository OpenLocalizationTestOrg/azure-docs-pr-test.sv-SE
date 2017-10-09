---
title: "Hadoop-aaaCreate kluster med en webbläsare - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate Hadoop, HBase, Storm eller Spark-kluster på Linux för HDInsight med hjälp av en webbläsare och hello Azure preview portal."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 697278cf-0032-4f7c-b9b2-a84c4347659e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 63027e35e2d66dd76218aff3e0c085fc811736ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-hello-azure-portal"></a>Skapa Linux-baserade kluster i HDInsight med hello Azure-portalen
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

hello Azure-portalen är ett webbaserat verktyg för tjänster och resurser som finns i hello Microsoft Azure-molnet. I den här artikeln du lära dig hur toocreate Linux-baserat HDInsight-kluster med hjälp av hello portal.

## <a name="prerequisites"></a>Krav
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **En modern webbläsare**. hello Azure-portalen använder HTML5 och Javascript och fungerar inte i äldre webbläsare.

## <a name="create-clusters"></a>Skapa kluster
hello Azure-portalen visar de flesta egenskaper för hello-klustret. Med Azure Resource Manager-mall kan dölja du en stor mängd information. Mer information finns i [skapa Linux-baserade Hadoop-kluster i HDInsight med hjälp av Azure Resource Manager-mallar](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på  **+** , klickar du på **Intelligence + analys**, och klicka sedan på **HDInsight**.
   
    ![Skapa ett nytt kluster i hello Azure-portalen](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster.png "skapar ett nytt kluster i hello Azure-portalen")

3. I hello **HDInsight** bladet, klickar du på **anpassad (storlek, inställningar, appar)**, klickar du på **grunderna**, och ange sedan hello följande information.

    ![Skapa ett nytt kluster i hello Azure-portalen](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-basics.png "skapar ett nytt kluster i hello Azure-portalen")

    * Ange **klusternamnet**: Det här namnet måste vara globalt unikt.

    * Från hello **prenumeration** listrutan, Välj hello Azure-prenumeration som ska användas för hello klustret.

    * Klicka på **kluster typen**, och välj sedan:
   
        * **Klustret typen**: Välj om du inte vet vilka toochoose **Hadoop**. Det är hello populäraste typ av kluster.
     
            > [!IMPORTANT]
            > HDInsight-kluster har på en mängd olika typer som motsvarar toohello arbetsbelastning eller teknik som hello klustret är inställd för. Det finns ingen metod som stöds toocreate ett kluster som kombinerar flera typer, till exempel Storm och HBase på ett kluster. 
            > 
            > 
        
        * **Operativsystem**: Välj **Linux**.
        
        * **Version**: Använd hello standardversionen om du inte vet vilka toochoose. Mer information finns i [HDInsight-klusterversioner](hdinsight-component-versioning.md).
        * **Klustret nivå**: Azure HDInsight tillhandahåller hello molntjänster för stordata i två kategorier: standardnivån och Premium-nivån. Mer information finns i [Klusternivåer](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).

    * För **klustret inloggning användarnamn** och **klustret inloggningslösenordet**, ange hello användarnamn och lösenord för hello administratörsanvändaren.

    * Ange en **SSH-användarnamn** och om du vill toohave hello SSH lösenord är detsamma som hello administratörslösenord du angav tidigare, Välj hello **använda samma lösenord som klusterinloggning** kryssrutan. Om inte, kan du ange antingen en **lösenord** eller **offentliga nyckel**, som kommer att använda tooauthenticate hello SSH-användare. Hello rekommenderad metod är att använda en offentlig nyckel. Klicka på **Välj** vid konfiguration av hello nedre toosave hello autentiseringsuppgifter.
   
        Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

    * För **resursgruppen**, ange om du vill toocreate en ny resursgrupp eller använda en befintlig.

    * Ange ett datacenter **plats** där hello klustret kommer att skapas.

    * Klicka på **Nästa**.

4. På hello **lagring** bladet, ange om du vill Azure Storage (WASB) eller Data Lake Store som standardlagring. Titta på hello tabellen nedan för mer information.

    ![Skapa ett nytt kluster i hello Azure-portalen](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-storage.png "skapar ett nytt kluster i hello Azure-portalen")

    | Lagring                                      | Beskrivning |
    |----------------------------------------------|-------------|
    | **Azure Storage-Blobbar som standardlagring**   | <ul><li>För **primära lagringstyp**väljer **Azure Storage**. Efter det för **urvalsmetod**, kan du välja **Mina prenumerationer** om du vill toospecify ett lagringskonto som är en del av din Azure-prenumeration och välj sedan hello storage-kontot. Annars klickar du på **åtkomstnyckeln** och ge hello information för hello storage-konto som du vill toochoose från utanför Azure-prenumerationen.</li><li>För **standardbehållaren**, du kan välja toogo med hello behållaren standardnamn föreslås av hello-portalen eller ange en egen.</li><li>Om du använder WASB som standardlagring kan du (valfritt) på **ytterligare Lagringskonton** toospecify ytterligare lagringskonton tooassociate med hello-kluster. I hello **Azure Lagringsnycklar** bladet, klickar du på **lägga till en nyckel för säkerhetslagring**, och du kan ange ett lagringskonto från Azure-prenumerationer eller andra prenumerationer (genom att tillhandahålla hello storage-konto snabbtangent).</li><li>Om du använder WASB som standardlagring kan du (valfritt) på **Data Lake Store-åtkomst** toospecify Azure Data Lake Store som ytterligare lagringsutrymme. Mer information finns i [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</li></ul> |
    | **Azure Data Lake Store som standardlagring** | För **primära lagringstyp**väljer **Datasjölager** och läser sedan jobbhistorikposten toohello artikel [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) för instruktioner. |
    | **Externa metastores**                      | Alternativt kan ange du en SQL-databas toosave Hive och Oozie metadata som associeras med hello klustret. För **Välj en SQL-databas för Hive** Välj en SQL-databas och ange sedan hello användarnamn/lösenord för hello-databasen. Upprepa dessa steg för Oozie-metadata.<br><br>Vissa överväganden när du använder Azure SQL-databas för metastores. <ul><li>hello Azure SQL-databas som används för hello metastore måste tillåta anslutningen tooother Azure-tjänster, inklusive Azure HDInsight. Klicka på servernamnet för hello hello Azure SQL database-instrumentpanelen, hello höger. Detta är hello servern vilken hello SQL databasinstansen körs. När du är på hello server vyn klickar du på **konfigurera**, och sedan för **Azure Services**, klickar du på **Ja**, och klicka sedan på **spara**.</li><li>När du skapar en metastore måste du inte använda ett databasnamn som innehåller tankstreck eller bindestreck, eftersom detta kan orsaka hello klustret skapas processen toofail.</li></ul>                                                                                                                                                                       |

    Klicka på **Nästa**. 

    > [!WARNING]
    > Med ett konto för ytterligare lagringsutrymme på en annan plats än hello HDInsight-kluster stöds inte.

5. Du kan också klicka på **program** tooinstall program som fungerar med HDInsight-kluster. Dessa program kan utvecklas av Microsoft, oberoende programvaruleverantörer och av dig själv. Mer information finns i [installera HDInsight-program](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).


6. Klicka på **klusterstorleken** toodisplay information om hello-noder som kommer att skapas för det här klustret. Ange hello antalet arbetarnoder som du behöver för hello klustret. hello uppskattade kostnaden för hello kluster visas inom hello-bladet.
   
    ![Noden priser nivåer bladet](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-nodes.png "ange antalet noder i klustret")
   
   > [!IMPORTANT]
   > Om du planerar på mer än 32 arbetarnoder i klustret har skapats eller genom att skala hello klustret när den har skapats, måste du markera en head-nodstorlek med minst 8 kärnor och 14GB RAM-minne.
   > 
   > Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
   Klicka på **nästa** toosave hello nod priser konfiguration.

7. Klicka på **avancerade inställningar** tooconfigure andra valfria inställningar, till exempel med hjälp av **skriptåtgärder** toocustomize en kluster-tooinstall anpassade komponenter eller ansluta till en **virtuellt nätverk**. Titta på hello tabellen nedan för mer information.

    ![Noden priser nivåer bladet](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-advanced.png "ange antalet noder i klustret")

    | Alternativ | Beskrivning |
    |--------|-------------|
    | **Skriptåtgärder** | Använd det här alternativet om du vill toouse ett anpassat skript toocustomize ett kluster som hello klustret skapas. Mer information om skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md). |
    | **Virtual Network** | Välj en Azure virtuella nätverk och hello undernätet om du vill tooplace hello kluster till ett virtuellt nätverk. Information om hur du använder HDInsight med ett virtuellt nätverk, inklusive konfigurationskraven för hello virtuella nätverk finns i [utöka HDInsight funktioner med hjälp av ett Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md). |

    Klicka på **Nästa**.

8. På hello **sammanfattning** bladet verifiera hello information som du angav tidigare och klicka sedan på **skapa**.

    ![Noden priser nivåer bladet](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-summary.png "ange antalet noder i klustret")
    
    > [!NOTE]
    > Det tar en stund innan hello klustret toobe skapas vanligtvis cirka 15 minuter. Använda hello panelen på hello startsidan eller hello **meddelanden** transaktionen på hello vänsterkant hello sidan toocheck på hello etableringsprocessen.
    > 
    > 
12. När hello-processen är klar klickar du på hello panelen för hello kluster från hello startsidan toolaunch hello klusterbladet. Hej klusterbladet ger hello följande information.
    
    ![Klusterbladet](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-completed.png "egenskaper för kluster")
    
    Använd hello följande toounderstand hello ikoner hello överst i det här bladet.
    
    * **Översikt över** innehåller alla hello viktig information om hello-klustret, till exempel hello namn, hello resursgrupp som det hör till hello plats, hello operativsystemet URL för hello instrumentpanelen för klustret, osv.
    * **Instrumentpanelen** leder toohello Ambari-portalen som associerats med hello kluster.
    * **Secure Shell**: Information som behövs för tooaccess hello kluster med SSH.
    * **Kluster för** kan du öka hello antalet arbetarnoder som associerats med hello kluster.
    * **Ta bort**: tar bort hello HDInsight-kluster.
    

## <a name="customize-clusters"></a>Anpassa kluster
* Se [anpassa HDInsight-kluster med Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-hello-cluster"></a>Ta bort hello kluster
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat ett HDInsight-kluster använder du följande toolearn hur hello toowork med ditt kluster:

### <a name="hadoop-clusters"></a>Hadoop-kluster
* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-kluster
* [Kom igång med HBase på HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Utveckla Java-program för HBase i HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-kluster
* [Utveckla Java-topologier för Storm på HDInsight](hdinsight-storm-develop-java-topology.md)
* [Använda Python komponenter i Storm på HDInsight](hdinsight-storm-develop-python-topology.md)
* [Distribuera och övervaka topologier med Storm på HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark-kluster
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)

