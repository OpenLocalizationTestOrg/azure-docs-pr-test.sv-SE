---
title: "aaaGet igång med exempel HBase på HDInsight - Azure | Microsoft Docs"
description: "Följ den här toostart för exempel av Apache HBase med hadoop i HDInsight. Skapa tabeller från hello HBase-gränssnittet och fråga dem med Hive."
keywords: hbasecommand,hbase example
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 4d6a2658-6b19-4268-95ee-822890f5a33a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: jgao
ms.openlocfilehash: 43419780142b320b16180a2b1f25020dee2f7a11
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a>Kom igång med ett Apache HBase-exempel i HDInsight

Lär dig hur toocreate ett HBase-kluster i HDInsight, skapa HBase-tabeller och frågetabeller med Hive. Allmän HBase-information finns i [HDInsight HBase-översikt][hdinsight-hbase-overview].

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Krav
Innan du försöker exemplet HBase, måste du ha hello följande objekt:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
* [curl](http://curl.haxx.se/download.html).

## <a name="create-hbase-cluster"></a>Skapa HBase-kluster
hello följande procedur används en mall för Azure Resource Manager-toocreate en version 3.4 Linux-baserade HBase-kluster och hello beroende Azure standardkontot för lagring. toounderstand hello parametrar som används i hello proceduren och andra metoder, se [skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicka på hello följande bild tooopen hello mallen i hello Azure-portalen. hello-mallen finns i en offentlig blob-behållare. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Från hello **anpassad distribution** bladet ange hello följande värden:
   
   * **Prenumerationen**: Välj din Azure-prenumeration som används toocreate hello klustret.
   * **Resursgrupp**: Skapa en Azure Resource Management-grupp eller använd en befintlig.
   * **Plats**: Ange hello plats hello resursgruppen. 
   * **Klusternamn**: Ange ett namn för hello HBase-kluster.
   * **Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är **admin**.
   * **SSH-användarnamn och lösenord**: hello Standardanvändarnamnet är **sshuser**.  Du kan byta namn.
     
     Andra parametrar är valfria.  
     
     Varje kluster är beroende av ett Azure Storage-konto. När du har tagit bort ett kluster behåller hello data i hello storage-konto. hello klustret standard lagringskontonamnet är hello klusternamnet med tillägget ”store”. Det är hårdkodat i hello mallsektion variabler.
3. Välj **acceptera toohello villkoren ovan**, och klicka sedan på **inköp**. Det tar cirka 20 minuter toocreate ett kluster.

> [!NOTE]
> När ett HBase-kluster har tagits bort kan du skapa ett annat HBase-kluster med hjälp av hello samma standardblob-behållare. hello nya klustret hämtar hello HBase-tabeller som du skapade i hello ursprungliga klustret. tooavoid inkonsekvenser, rekommenderar vi att du inaktiverar hello HBase-tabeller innan du tar bort hello klustret.
> 
> 

## <a name="create-tables-and-insert-data"></a>Skapa tabeller och infoga data
Du kan använda SSH tooconnect tooHBase kluster och sedan använda HBase Shell toocreate HBase-tabeller, infoga data och fråga efter data. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

För de flesta visas data i tabellformat hello:

![HDInsight HBase-tabelldata][img-hbase-sample-data-tabular]

Hej samma data ser ut som i HBase (en implementering av BigTable):

![HDInsight HBase BigTable-data][img-hbase-sample-data-bigtable]


**toouse hello HBase-gränssnittet**

1. Kör hello följande HBase-kommando från SSH:
   
    ```bash
    hbase shell
    ```

2. Skapa en HBase med två kolumnserier:

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. Infoga data:
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![HDInsight Hadoop HBase shell][img-hbase-shell]
4. Hämta en rad
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    Hello samma resultat visas som med hello skanningskommandot eftersom det är bara en rad.
   
    Mer information om hello HBase-tabellschemat finns [introduktion tooHBase schemat Design][hbase-schema]. Fler HBase-kommandon finns i [referensguiden för Apache HBase][hbase-quick-start].
5. Avsluta hello shell
   
    ```hbaseshell
    exit
    ```

**toobulk Läs in data i HBase-tabellen för hello kontakter**

HBase innehåller flera metoder för att läsa in data i tabeller.  Mer information finns i [Massinläsning](http://hbase.apache.org/book.html#arch.bulk.load).

En exempeldatafil finns i en offentlig blob-behållare, *wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  hello är hello datafilen:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

Du kan om du vill skapa en textfil och överföra hello filen tooyour egna storage-konto. Hello instruktioner finns i [överföra data för Hadoop-jobb i HDInsight][hdinsight-upload-data].

> [!NOTE]
> Den här proceduren använder hello kontakter HBase-tabell som du har skapat i hello föregående procedur.
> 

1. Kör följande kommando tootransform hello filen tooStoreFiles och lagra vid en relativ sökväg som anges av Dimporttsv.bulk.output hello från SSH.  Om du är i HBase Shell Använd hello avsluta kommandot tooexit.

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. Kör följande kommando tooupload hello data från/storedatafileoutput toohello HBase-tabellen hello:
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. Du kan öppna hello HBase-gränssnittet och använda hello genomsökning kommandot toolist hello innehållet.

## <a name="use-hive-tooquery-hbase"></a>Använda Hive tooquery HBase

Du kan fråga efter data i HBase-tabeller med hjälp av Hive. I det här avsnittet skapar du en Hive-tabell som mappar toohello HBase-tabellen och använder den tooquery hello data i HBase-tabellen.

1. Öppna **PuTTY**, och Anslut toohello klustret.  Se hello instruktioner i hello föregående procedur.
2. Använd följande kommando toostart Beeline hello från hello SSH-sessionen:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Mer information om Beeline finns i [Use Hive with Hadoop in HDInsight with Beeline](hdinsight-hadoop-use-hive-beeline.md) (Använda Hive med Hadoop i HDInsight med Beeline).
       
3. Kör följande HiveQL-skript toocreate hello en Hive-tabell som mappar toohello HBase-tabellen. Kontrollera att du har skapat hello exempeltabell hänvisade till tidigare i den här kursen med hjälp av hello HBase-gränssnittet innan du kör instruktionen.

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. Kör följande HiveQL-skript tooquery hello data i hello HBase-tabellen hello:

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a>Använd HBase REST API:er med Curl

hello REST API skyddas via [grundläggande autentisering](http://en.wikipedia.org/wiki/Basic_access_authentication). Du bör alltid göra begäranden genom att använda säker HTTP (HTTPS) toohelp se till att dina autentiseringsuppgifter skickas toohello server på ett säkert sätt.

2. Använd hello följande kommando toolist hello befintliga HBase-tabeller:

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

3. Använd följande kommando toocreate en ny HBase-tabell med två kolumnserier hello:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    hello schemat tillhandahålls i hello JSon-format.
4. Använd följande kommando tooinsert hello vissa data:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    Du måste base64 koda hello värdena som anges i hello -d växel. I exemplet hello:
   
   * MTAwMA ==: 1000
   * UGVyc29uYWw6TmFtZQ==: Personal:Name
   * Sm9obiBEb2xl: John Dole
     
     [FALSKT radnyckel](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) kan du tooinsert (batch) flera värden.
5. Använd följande kommando tooget en rad hello:
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

Mer information om HBase Rest finns i [Referensguiden för Apache HBase](https://hbase.apache.org/book.html#_rest).

> [!NOTE]
> Thrift stöds inte av HBase i HDInsight.
>
> När du använder Curl eller annan REST-kommunikation med WebHCat, måste du autentisera hello begäranden genom att tillhandahålla hello användarnamn och lösenord för hello HDInsight-klustrets administratör. Du måste också använda hello klustrets namn som en del av hello identifierare URI (Uniform Resource) används toosend hello begäranden toohello server:
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    Du bör få ett svar liknande toohello efter svar:
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a>Kontrollera klusterstatus
HBase i HDInsight levereras med ett webbgränssnitt för övervakning av kluster. Du kan använda hello Webbgränssnittet för att begära statistik eller information om regioner.

**tooaccess hello HBase Master UI**

1. Logga in på hello hello Ambari-Webbgränssnittet på https://&lt;klusternamn >. azurehdinsight.net.
2. Klicka på **HBase** hello vänstra menyn.
3. Klicka på **snabblänkar** hello hello sida, punkt toohello active Zookeeper nod länk, och klicka sedan på **HBase Master UI**.  hello UI öppnas i en annan flik i webbläsaren:

  ![HDInsight HBase HMaster UI](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  Hej HBase Master UI innehåller hello följande avsnitt:

  - regionservrar
  - huvudservrar för säkerhetskopiering
  - tabeller
  - uppgifter
  - attribut för programvara

## <a name="delete-hello-cluster"></a>Ta bort hello kluster
tooavoid inkonsekvenser, rekommenderar vi att du inaktiverar hello HBase-tabeller innan du tar bort hello klustret.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg
I den här artikeln får du lära dig hur hello toocreate ett HBase-kluster och hur toocreate tabellerna och vyerna hello data i dessa tabeller från HBase-gränssnittet. Du också lära dig hur toouse registreringsdata fråga på data i HBase-tabeller och hur toouse hello HBase C# REST API: er toocreate en HBase-tabell och hämta data från hello tabell.

Det finns fler toolearn:

* [HDInsight HBase-översikt][hdinsight-hbase-overview]: HBase är en NoSQL-databas av Apachetyp med öppen källkod som bygger på Hadoop och som ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och semistrukturerade data.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
