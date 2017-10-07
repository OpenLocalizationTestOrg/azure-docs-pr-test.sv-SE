---
title: "aaaGet igång med Hadoop och Hive i Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate HDInsight-kluster och frågar efter data med Hive."
keywords: "hadoop komma igång, hadoop linux, hadoop Snabbstart, komma igång, hive hive-Snabbstart"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6a12ed4c-9d49-4990-abf5-0a79fdfca459
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: jgao
ms.openlocfilehash: 3d96d78121200ebda3626dd2c3885e3ddacd546d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight"></a>Hadoop-självstudiekurs: Komma igång med Hadoop i HDInsight

Lär dig hur toocreate [Hadoop](http://hadoop.apache.org/) kluster i HDInsight och hur toorun Hive-jobb i HDInsight. [Apache Hive](https://hive.apache.org/) är hello populäraste komponenten i hello Hadoop-ekosystemet. För närvarande innehåller HDInsight [sju olika klustertyper](hdinsight-hadoop-introduction.md#overview). Varje typ av kluster har stöd för olika komponentuppsättningar. Samtliga klustertyper stöder Hive. En lista över stödda komponenter i HDInsight finns [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md)  

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Krav
Innan du börjar den här vägledningen måste du ha:

* **Azure-prenumeration**: toocreate ett kostnadsfritt en månad konto, bläddra för[azure.microsoft.com/free](https://azure.microsoft.com/free).

## <a name="create-cluster"></a>Skapa kluster

De flesta Hadoop-jobb är batchjobb. Du skapar ett kluster, kör vissa jobb och ta bort hello-klustret. I det här avsnittet skapar du ett Hadoop-kluster i HDInsight med en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md). Du behöver inte ha någon erfarenhet av Resource Manager-mallar för att kunna följa de här självstudierna. För andra metoder för att skapa kluster och förstå hello egenskaper som används i den här självstudiekursen, se [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md). Använd hello selector på hello överkant hello sidan toochoose dina alternativ för att skapa klustret.

hello Resource Manager-mallen som används i den här kursen finns i [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). 

1. Klicka på hello efter bilden toosign i tooAzure och öppna hello Resource Manager-mall i hello Azure-portalen. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Ange eller välj hello följande värden:
   
    ![HDInsight Linux komma igång Resource Manager-mall i portal](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "distribuera Hadoop-kluster i HDInsigut med hello Azure-portalen och en resource manager-Gruppmall").
   
    * **Prenumeration**: Välj din Azure-prenumeration.
    * **Resursgrupp**: Skapa en resursgrupp eller välj en befintlig resursgrupp.  En resursgrupp är en behållare med Azure-komponenter.  I det här fallet innehåller hello resursgruppen hello HDInsight-kluster och hello beroende Azure Storage-konto. 
    * **Plats**: Välj en Azure-plats där du vill att toocreate klustret.  Välj en plats närmare tooyou för bättre prestanda. 
    * **Klustertyp**: Välj **hadoop** för den här självstudien.
    * **Klusternamn**: Ange ett namn för hello Hadoop-kluster.
    * **Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är **admin**.
    * **SSH-användarnamn och lösenord**: hello Standardanvändarnamnet är **sshuser**.  Du kan byta namn. 
     
    Vissa egenskaper har hårdkodad i hello mallen.  Du kan konfigurera dessa värden från hello mall.

    * **Plats**: hello platsen för hello kluster och hello beroende konto lagringsresurs hello samma plats som hello resursgrupp.
    * **Klusterversion**: 3.5
    * **OS-typ**: Linux
    * **Antal arbetsnoder**: 2

     Varje kluster är beroende av ett [Azure Storage-konto](hdinsight-hadoop-use-blob-storage.md) eller ett [Azure Data Lake-konto](hdinsight-hadoop-use-data-lake-store.md). Det kallas hello standardkontot för lagring. HDInsight-kluster och dess standardkontot för lagring måste finnas i hello samma Azure-region. Ta bort kluster tar inte bort hello storage-konto. 
     
     Fler förklaringar av dessa egenskaper finns i [Skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3. Välj **acceptera toohello villkoren ovan** och **PIN-kod toodashboard**, och klicka sedan på **inköp**. Du bör se en ny panel med titeln **skicka malldistribution** på hello portalens instrumentpanel. Det tar cirka 20 minuter toocreate ett kluster. När hello klustret har skapats är hello rubriktexten på panelen hello ändrade toohello resursgruppens namn du angett. Och hello portalen öppnar automatiskt hello resursgrupp i ett nytt blad. Du kan se både hello och hello standardlagring i listan.
   
    ![HDInsight Linux komma igång resursgrupp](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-resource-group.png "Azure HDInsight klusterresursgrupp").

4. Klicka på hello klustrets namn tooopen hello kluster i ett nytt blad.

   ![HDInsight Linux komma igång klusterinställningar](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png "HDInsight-klusteregenskaper")


## <a name="run-hive-queries"></a>Köra Hive-frågor
[Apache Hive](hdinsight-use-hive.md) är hello populäraste komponenten som används i HDInsight. Det finns många sätt toorun Hive-jobb i HDInsight. I den här kursen använder du hello Ambari Hive-vyn från hello-portalen. Andra metoder för att skicka Hive-jobb beskrivs i [Använda Hive-data i HDInsight](hdinsight-use-hive.md).

1. Hello föregående skärmbild, klicka på **Klusterinstrumentpanel**, och klicka sedan på **instrumentpanelen för HDInsight-klustret**.  Du kan även bläddra för **https://&lt;klusternamn >. azurehdinsight.net**, där &lt;klusternamn > är hello-kluster som du skapade i hello föregående avsnitt tooopen Ambari.
2. Ange hello Hadoop-användarnamn och lösenord som du angav i hello föregående avsnitt. hello Standardanvändarnamnet är **admin**.
3. Öppna **Hive-vy** som visas i följande skärmbild hello:
   
    ![Välja Ambari-vyer](./media/hdinsight-hadoop-linux-tutorial-get-started/selecthiveview.png "HDInsight Hive Viewer menyn").
4. I hello **frågeredigeraren** på hello-sidan, klistra in hello följande HiveQL-instruktioner i kalkylbladet med hello:
   
        SHOW TABLES;
   
   > [!NOTE]
   > Hive kräver semikolon.       
   > 
   > 
5. Klicka på **Kör**. En **Frågeprocessresultat** avsnitt bör visas under hello frågeredigeraren och visa information om hello jobb. 
   
    När hello frågan har slutförts hello **Frågeprocessresultat** avsnitt visar hello resultaten av hello igen. En tabell med namnet **hivesampletable** bör visas. Det här exemplet Hive-tabell kommer med alla hello HDInsight-kluster.
   
    ![HDInsight Hive-vyer](./media/hdinsight-hadoop-linux-tutorial-get-started/hiveview.png "HDInsight Hive visa frågeredigeraren").
6. Upprepa steg 4 och steg 5 toorun hello följande fråga:
   
        SELECT * FROM hivesampletable;
   
   > [!TIP]
   > Obs hello **spara resultaten** listrutan hello övre vänstra hörnet hello **Frågeprocessresultat** avsnittet; du kan använda den här hello nedladdningsresultaten tooeither eller spara dem tooHDInsight lagring som en CSV-fil.
   > 
   > 
7. Klicka på **historik** tooget en lista över hello jobb.

När du har slutfört ett Hive-jobb, kan du [exportera hello resultat tooAzure SQL database eller SQL Server-databas](hdinsight-use-sqoop-mac-linux.md), du kan också [visualisera hello resultat i Excel](hdinsight-connect-excel-power-query.md). Mer information om hur du använder Hive i HDInsight finns [använda Hive och HiveQL med Hadoop i HDInsight tooanalyze en exempelfil Apache log4j](hdinsight-use-hive.md).

## <a name="clean-up-hello-tutorial"></a>Rensa hello självstudiekursen
Du kanske vill toodelete hello klustret när du har slutfört hello kursen. Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används. Du debiteras också för ett HDInsight-kluster, även när det inte används. Eftersom hello avgifterna för hello klustret är flera gånger större än hello avgifterna för lagring, är det ekonomiskt meningsfullt toodelete kluster när de inte används. 

> [!NOTE]
> Med hjälp av [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md), du kan skapa HDInsight-kluster på begäran och konfigurera en TimeToLive inställningen för ta bort hello kluster automatiskt. 
> 
> 

**toodelete hello klustret och/eller hello standardkontot för lagring**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på hello sida vid sida med hello resursgruppens namn du använde när du skapade hello hello portalens instrumentpanel och.
3. Klicka på **ta bort** på hello resurs bladet toodelete hello resursgrupp, som innehåller hello kluster och hello standardkontot för lagring, eller klicka på hello klusternamn på hello **resurser** panelen och klicka sedan på **Ta bort** på hello klusterbladet. Obs hello resursgruppen tar bort hello storage-konto. Om du vill tookeep hello storage-konto, Välj toodelete hello klustret.

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg
I den här kursen har du fått veta hur toocreate en Linux-baserat HDInsight-kluster med hjälp av en Resource Manager-mall och hur tooperform grundläggande Hive-frågor.

toolearn mer information om hur du analyserar data med HDInsight, se hello följande artiklar:

* toolearn mer information om hur du använder Hive med HDInsight, inklusive hur tooperform Hive-frågor från Visual Studio finns [använda Hive med HDInsight][hdinsight-use-hive].
* toolearn om Pig, ett språk används tootransform data, se [använda Pig med HDInsight][hdinsight-use-pig].
* toolearn om MapReduce, ett hur toowrite program som bearbetar data i Hadoop, se [använda MapReduce med HDInsight][hdinsight-use-mapreduce].
* toolearn om hur du använder hello HDInsight Tools för Visual Studio tooanalyze data i HDInsight, se [komma igång med Visual Studio Hadoop-verktygen för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Om du är klar toostart arbetar med dina egna data och behöver tooknow mer om hur HDInsight lagrar data eller hur tooget data till HDInsight finns hello följande:

* Mer information om hur HDInsight använder Azure Storage finns i [Använda Azure Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Mer information om hur tooupload data tooHDInsight finns [överför data tooHDInsight][hdinsight-upload-data].

Om du vill läsa mer om att skapa eller hantera HDInsight-kluster toolearn finns hello följande:

* toolearn om hur du hanterar ditt Linux-baserade HDInsight-kluster finns [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).
* toolearn mer om hello-alternativ som du kan välja när du skapar ett HDInsight-kluster finns [skapa HDInsight i Linux med anpassade alternativ](hdinsight-hadoop-provision-linux-clusters.md).
* Om du är bekant med Linux och Hadoop, men vill tooknow närmare information om Hadoop på hello HDInsight, se [arbeta med HDInsight på Linux](hdinsight-hadoop-linux-information.md). Detta ger information som:
  
  * URL: er för tjänster som ligger på hello klustret, till exempel Ambari och WebHCat
  * hello platsen för Hadoop-filer och exempel på hello lokalt filsystem
  * hello användning av Azure Storage (WASB) i stället för HDFS som hello standard datalager

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


