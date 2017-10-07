---
title: aaaUse hello Azure portal toocreate Azure HDInsight-kluster med Data Lake Store | Microsoft Docs
description: "Använda hello Azure portal toocreate och använda HDInsight-kluster med Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: nitinme
ms.openlocfilehash: f23113d444a3c5a01894dba29f75f3621b2d16bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-by-using-hello-azure-portal"></a>Skapa HDInsight-kluster med Data Lake Store med hjälp av hello Azure-portalen
> [!div class="op_single_selector"]
> * [Använd hello Azure-portalen](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Använd PowerShell (för standardlagring)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Använd PowerShell (för ytterligare lagringsutrymme)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Använd Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Lär dig hur toouse hello Azure portal toocreate ett HDInsight-kluster med ett Azure Data Lake Store-konto som hello standardlagring eller ett ytterligare lagringsutrymme. Även om ytterligare lagringsutrymme är valfri för ett HDInsight-kluster, är det rekommenderade toostore företagsdata i hello ytterligare storage-konton.

## <a name="prerequisites"></a>Krav
Se till att du har uppfyllt hello följande krav innan du börjar den här kursen:

* **En Azure-prenumeration**. Gå för[hämta kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Azure Data Lake Store-konto**. Följ instruktionerna för hello från [Kom igång med Azure Data Lake Store med hjälp av hello Azure-portalen](data-lake-store-get-started-portal.md). Du måste också skapa en rotmapp på hello-konto.  I den här självstudiekursen en rotmapp kallas __/kluster__ används.
* **Ett Azure Active Directory-tjänstens huvudnamn**. Den här självstudiekursen innehåller instruktioner om hur toocreate ett huvudnamn för tjänsten i Azure Active Directory (AD Azure). Dock toocreate ett huvudnamn för tjänsten, måste du vara administratör för Azure AD. Om du är administratör kan du hoppa över det här kravet och fortsätt med hello kursen.

    >[!NOTE]
    >Du kan skapa en tjänst huvudnamn endast om du är administratör för Azure AD. Azure AD-administratör skapa en tjänst huvudnamn innan du kan skapa ett HDInsight-kluster med Data Lake Store. Dessutom hello tjänstens huvudnamn måste skapas med ett certifikat, enligt beskrivningen i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).
    >

## <a name="create-an-hdinsight-cluster"></a>Skapa ett HDInsight-kluster

I det här avsnittet skapar du ett HDInsight-kluster med Data Lake Store-konton som standard hello eller hello ytterligare lagringsutrymme. Den här artikeln fokuserar endast hello en del av konfigurationen Data Lake Store-konton.  Hello allmänna klustret skapas information och procedurer finns [skapa Hadoop-kluster i HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

### <a name="create-a-cluster-with-data-lake-store-as-default-storage"></a>Skapa ett kluster med Data Lake Store som standardlagring

**toocreate ett HDInsight-kluster med ett Data Lake Store som hello standardkontot för lagring**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Följ [Skapa kluster](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) hello allmän information om hur du skapar HDInsight-kluster.
3. På hello **lagring** bladet under **primära lagringstyp**väljer **Datasjölager**, och ange sedan hello följande information:

    ![Lägg till tjänstens huvudnamn tooHDInsight klustret](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "Lägg till service principal tooHDInsight-kluster")

    - **Välj Data Lake Store-konto**: Välj ett befintligt Data Lake Store-konto. Ett befintligt Data Lake Store-konto krävs.  Se [krav](#prereuisites).
    - **Rotsökvägen**: Ange en sökväg där hello klusterspecifika filer är toobe lagras. På skärmbilden hello är __/kluster-myhdiadlcluster__, i vilken hello __/kluster__ mappen måste finnas och hello skapar portalen *myhdicluster* mapp.  Hej *myhdicluster* är hello klusternamnet.
    - **Data Lake Store-åtkomst**: Konfigurera åtkomst mellan hello Data Lake Store-konto och HDInsight-kluster. Instruktioner finns i [konfigurera Data Lake Store-åtkomst](#configure-data-lake-store-access).
    - **Ytterligare lagringskonton**: Lägg till Azure Storage-konton som ytterligare lagringsutrymme konton för hello klustret. tooadd ytterligare Data Lake lagrar görs genom att ge hello klustret behörigheter på data i flera datasjölagerkonton när du konfigurerar ett Data Lake Store-konto som hello primära lagringstyp. Läs mer i [Konfigurera åtkomst till Data Lake Store](#configure-data-lake-store-access).

4. På hello **Data Lake Store-åtkomst**, klickar du på **Välj**, och sedan fortsätta med skapa ett kluster, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md).


### <a name="create-a-cluster-with-data-lake-store-as-additional-storage"></a>Skapa ett kluster med Data Lake Store som ytterligare lagringsutrymme

hello skapar instruktionerna nedan ett HDInsight-kluster med ett Azure Storage-konto som hello standardlagring och ett Data Lake Store-konto som ett ytterligare lagringsutrymme.
**toocreate ett HDInsight-kluster med ett Data Lake Store som hello standardkontot för lagring**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Följ [Skapa kluster](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) hello allmän information om hur du skapar HDInsight-kluster.
3. På hello **lagring** bladet under **primära lagringstyp**väljer **Azure Storage**, och ange sedan hello följande information:

    ![Lägg till tjänstens huvudnamn tooHDInsight klustret](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Lägg till service principal tooHDInsight-kluster")

    - **Urvalsmetod**: Använd någon av följande alternativ för hello:

        * toospecify ett lagringskonto som är en del av din Azure-prenumeration, Välj **Mina prenumerationer**, och välj sedan hello storage-konto.
        * toospecify ett lagringskonto som ligger utanför din Azure-prenumeration, Välj **åtkomstnyckeln**, och ange sedan hello information för hello utanför storage-konto.

    - **Standardbehållaren**: Använd antingen standardvärdet hello eller ange ett eget namn.

    - Ytterligare lagringskonton: lägga till fler Azure Storage-konton som hello ytterligare lagringsutrymme.
    - Data Lake Store-åtkomst: Konfigurera åtkomst mellan hello Data Lake Store-konto och HDInsight-kluster. Instruktioner finns i [konfigurera Data Lake Store-åtkomst](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Konfigurera Data Lake Store-åtkomst 

I det här avsnittet konfigurerar du Data Lake Store-åtkomst från HDInsight-kluster med en Azure Active Directory-tjänstens huvudnamn. 

### <a name="specify-a-service-principal"></a>Ange ett huvudnamn för tjänsten

Från hello Azure-portalen, kan du använda ett befintligt huvudnamn för tjänsten eller skapa en ny.

**toocreate ett huvudnamn för tjänsten från hello Azure-portalen**

1. Klicka på **Data Lake Store-åtkomst** från hello Store-bladet.
2. På hello **Data Lake Store-åtkomst** bladet, klickar du på **Skapa nytt**.
3. Klicka på **tjänstens huvudnamn**, och följ sedan hello instruktioner toocreate ett huvudnamn för tjänsten.
4. Hämta hello certifikatet om du väljer toouse den igen i hello framtida. Hämtar hello certifikat är användbart om du vill toouse hello samma service principal när du skapar ytterligare HDInsight-kluster.

    ![Lägg till tjänstens huvudnamn tooHDInsight klustret](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Lägg till service principal tooHDInsight-kluster")

4. Klicka på **åtkomst** tooconfigure hello åtkomst.  Se [konfigurera filbehörigheter](#configure-file-permissions).


**toouse en befintlig tjänstens huvudnamn från hello Azure-portalen**

1. Klicka på **Data Lake Store-åtkomst**.
1. På hello **Data Lake Store-åtkomst** bladet, klickar du på **använda befintliga**.
2. Klicka på **tjänstens huvudnamn**, och välj sedan ett huvudnamn för tjänsten. 
3. Överför hello-certifikatet (.pfx-fil) som är kopplad till ditt valda tjänstens huvudnamn och ange sedan hello certifikatlösenord.

    ![Lägg till tjänstens huvudnamn tooHDInsight klustret](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Lägg till service principal tooHDInsight-kluster")

4. Klicka på **åtkomst** tooconfigure hello åtkomst.  Se [konfigurera filbehörigheter](#configure-file-permissions).


### <a name="configure-file-permissions"></a>Konfigurera behörigheter för filer

hello konfigurerar varierar beroende på om hello används som hello standardlagring eller ett ytterligare storage-konto:

- Används som standardlagring

    - behörigheten på hello rotnivå hello Data Lake Store-konto
    - behörigheten på hello rotnivå hello HDInsight klusterlagringen. Till exempel hello __/kluster__ mappen som används för tidigare i kursen hello.
- Använda som ett ytterligare lagringsutrymme

    - Behörigheten på hello mappar där du behöver filåtkomst.

**tooassign behörigheten på hello rotnivå för Data Lake Store-konto**

1. På hello **Data Lake Store-åtkomst** bladet, klickar du på **åtkomst**. Hej **Välj filbehörigheter** blad öppnas. Den listar alla hello datasjölagerkonton i prenumerationen.
2. Hovra (inte på) hello muspekaren över hello namnet på hello Data Lake Store-konto toomake hello kryssrutan visas, och sedan väljer hello kryssrutan.

    ![Lägg till tjänstens huvudnamn tooHDInsight klustret](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Lägg till service principal tooHDInsight-kluster")

  Som standard __läsa__, __skriva__, AND __EXECUTE__ är alla markerade.

3. Klicka på **Välj** på hello längst ned på sidan för hello.
4. Klicka på **kör** tooassign behörighet.
5. Klicka på **Klar**.

**tooassign behörigheten på hello rotnivå för HDInsight-kluster**

1. På hello **Data Lake Store-åtkomst** bladet, klickar du på **åtkomst**. Hej **Välj filbehörigheter** blad öppnas. Den listar alla hello datasjölagerkonton i prenumerationen.
1. Från hello **Välj filbehörigheter** bladet, klickar du på hello Data Lake Store namn tooshow dess innehåll.
2. Välj lagringsroten för hello HDInsight-kluster genom att markera kryssrutan hello hello vänster hello-mappen. Enligt toohello skärmbild tidigare hello klustret lagringsroten är __/kluster__ mappen som du angav när du väljer hello Data Lake Store som standardlagring.
3. Ange hello behörigheter för hello mapp.  Som standard, läsa, skriva och köra är alla markerade.
4. Klicka på **Välj** på hello längst ned på sidan för hello.
5. Klicka på **Run** (Kör).
6. Klicka på **Klar**.

Om du använder Data Lake Store som ytterligare lagringsutrymme måste du tilldela behörigheten endast för hello mappar som du vill tooaccess från hello HDInsight-kluster. Till exempel i hello skärmbilden nedan du ge åtkomst endast för**hdiaddonstorage** mapp i ett Data Lake Store-konto.

![Tilldela service principal behörigheter toohello HDInsight-kluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "tilldela service principal behörigheter toohello HDInsight-kluster")


## <a name="verify-cluster-set-up"></a>Kontrollera konfigurera klustret

När hello klustret installationen är klar kontrollerar du dina resultat genom att göra någon eller båda av följande hello på hello klusterbladet:

* tooverify som hello associerade lagringsmedia för hello klustret är hello Data Lake Store-konto som du har angett klickar du på **lagringskonton** hello vänster.

    ![Lägg till tjänstens huvudnamn tooHDInsight klustret](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "Lägg till service principal tooHDInsight-kluster")

* tooverify som hello huvudnamn för tjänsten är korrekt associerad med hello HDInsight-kluster, klickar du på **Data Lake Store-åtkomst** hello vänster.

    ![Lägg till tjänstens huvudnamn tooHDInsight klustret](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Lägg till service principal tooHDInsight-kluster")


## <a name="examples"></a>Exempel

När du har ställt in hello kluster med Data Lake Store som lagringen finns toothese exempel på hur toouse HDInsight-kluster tooanalyze hello data som lagras i Data Lake Store.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-primary-storage"></a>Köra en Hive-fråga mot data i ett Data Lake Store (som primär lagring)

toorun en Hive-fråga använda hello Hive vyer gränssnittet i hello Ambari-portalen. Anvisningar för hur toouse Ambari Hive-vyer finns [Använd hello Hive-vy med Hadoop i HDInsight](../hdinsight/hdinsight-hadoop-use-hive-ambari-view.md).

När du arbetar med data i ett Data Lake Store finns några strängar toochange.

Om du använder, till exempel hello-klustret som du skapade med Data Lake Store som primär lagring hello sökvägen toohello data är: *adl: / / < data_lake_store_account_name > /azuredatalakestore.net/path/to/file*. En Hive-fråga toocreate en tabell från exempeldata som lagras i hello Data Lake Store-konto som ser ut så hello följande instruktion:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsstorage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Beskrivningar:
* `adl://hdiadlstorage.azuredatalakestore.net/`är hello roten hello Data Lake Store-konto.
* `/clusters/myhdiadlcluster`är hello klusterdata som du angav när du skapar klustret hello hello roten.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/`är hello platsen för hello exempelfilen som du använde i hello-frågan.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-additional-storage"></a>Köra en Hive-fråga mot data i ett Data Lake Store (som ytterligare lagringsutrymme)

Om hello-klustret som du skapade använder Blob storage som standardlagring, ingår inte hello exempeldata i hello Azure Data Lake Store-konto som används som ytterligare lagringsutrymme. I så fall först överföra hello data från Blob storage toohello Data Lake Store och kör sedan hello frågor som visas i föregående exempel hello.

Information om hur toocopy från Blob storage tooa Data Lake datalager finns hello följande artiklar:

* [Använd Distcp toocopy data mellan Azure Storage-blobbar och Data Lake Store](data-lake-store-copy-data-wasb-distcp.md)
* [Använd AdlCopy toocopy data från Azure Storage-blobbar tooData Datasjölager](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-store-with-a-spark-cluster"></a>Använd Data Lake Store med ett Spark-kluster
Du kan använda en Spark-kluster toorun Spark-jobb på data som lagras i ett Data Lake Store. Mer information finns i [använda HDInsight Spark-kluster tooanalyze data i Data Lake Store](../hdinsight/hdinsight-apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-store-in-a-storm-topology"></a>Använd Data Lake Store i en Storm-topologi
Du kan använda hello Data Lake Store toowrite data från en Storm-topologi. Anvisningar för hur tooachieve i det här scenariot finns [Använd Azure Data Lake Store med Apache Storm med HDInsight](../hdinsight/hdinsight-storm-write-data-lake-store.md).

## <a name="see-also"></a>Se även
* [PowerShell: Skapa ett HDInsight-kluster toouse Data Lake Store](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
