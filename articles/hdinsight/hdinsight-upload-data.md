---
title: "aaaUpload data för Hadoop-jobb i HDInsight | Microsoft Docs"
description: "Lär dig hur tooupload och komma åt data för Hadoop-jobb i HDInsight med hello Azure CLI, Azure Lagringsutforskaren, Azure PowerShell, kommandorad för hello Hadoop eller Sqoop."
keywords: "etl-hadoop, hämta data till hadoop, Läs in data för hadoop"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 56b913ee-0f9a-4e9f-9eaf-c571f8603dd6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: jgao
ms.openlocfilehash: 15da602085d41c19789e34800f3d9e238d7d1de8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Ladda upp data för Hadoop-jobb i HDInsight
Azure HDInsight ger en komplett Hadoop distributed file system (HDFS) över Azure Blob storage. Den är utformad som en HDFS tillägget tooprovide en sömlös upplevelse toocustomers. Den gör hello fullständig uppsättning komponenter i hello Hadoop-ekosystemet toooperate direkt på hello data som den hanterar. Azure Blob storage och HDFS är distinkt filsystem som är optimerade för lagring av data och beräkningar på dessa data. Information om hello fördelarna med att använda Azure Blob storage finns [använda Azure Blob storage med HDInsight][hdinsight-storage].

**Förutsättningar**

Observera följande krav innan du börjar hello:

* Ett Azure HDInsight-kluster. Instruktioner finns i [komma igång med Azure HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision].

## <a name="why-blob-storage"></a>Varför blob storage?
Azure HDInsight-kluster är vanligtvis distribuerats toorun MapReduce-jobb och hello kluster tas bort när de här jobben har slutförts. Behålla hello data i hello HDFS kluster när beräkningar har slutförts skulle vara en kostsam sätt toostore dessa data. Azure Blob storage är en hög tillgänglighet, skalbara, hög kapacitet, låg kostnad och delbart lagringsalternativ för data som är toobe bearbetas med HDInsight. Lagra data i en blob kan hello HDInsight-kluster som används för beräkning toobe publicerat på ett säkert sätt utan att förlora data.

### <a name="directories"></a>Kataloger
Azure Blob storage-behållare som lagrar data som nyckel/värde-par och det finns ingen Kataloghierarki. Men hello ”/” tecken kan användas i hello nyckelnamn toomake visas den som om en fil lagras i en katalogstruktur. HDInsight ser dessa som om de faktiska kataloger.

Nyckeln för en blob kan till exempel vara *input/log1.txt*. Ingen verklig ”input” katalog finns, men på grund av toohello förekomst av hello tecknet ”/” hello nyckelnamn, den har hello utseendet på en sökväg till filen.

Om du använder Azure Explorer-verktyg kan du märka vissa 0 byte-filer. Filerna har två syften:

* Om det finns tomma mappar, markera de av hello finns hello-mappen. Azure Blob storage är tillräckligt smarta tooknow att om en blob som kallas foo-fältet finns det finns en mapp med namnet **foo**. Men hello endast sätt toosignify kallas för en tom mapp **foo** av är att ha särskilda 0 byte filen på plats.
* De innehåller särskilda metadata som behövs av hello Hadoop filsystem, särskilt hello behörigheter och ägare för hello mappar.

## <a name="command-line-utilities"></a>Kommandoradsverktyg
Microsoft tillhandahåller hello följande verktyg toowork med Azure Blob storage:

| Verktyget | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure-kommandoradsgränssnittet][azurecli] |✔ |✔ |✔ |
| [Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] | | |✔ |
| [Hadoop-kommando](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Medan hello Azure CLI, Azure PowerShell och AzCopy kan alla användas utanför Azure, hello Hadoop kommandot är bara tillgängligt på hello HDInsight-kluster och kan bara läsa in data från hello lokala filsystem i Azure Blob storage.
>
>

### <a id="xplatcli"></a>Azure CLI
hello Azure CLI är ett verktyg för flera plattformar som du kan använda toomanage Azure services. Använd följande steg tooupload data tooAzure Blob storage hello:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Installera och konfigurera hello Azure CLI för Mac, Linux och Windows](../cli-install-nodejs.md).
2. Öppna en kommandotolk, bash eller andra gränssnitt och använda hello följande tooauthenticate tooyour Azure-prenumeration.

        azure login

    När du uppmanas ange hello användarnamn och lösenord för din prenumeration.
3. Ange hello efter kommandot toolist hello storage-konton för din prenumeration:

        azure storage account list
4. Markera hello storage-konto som innehåller hello blob som du vill toowork med och använda följande kommando tooretrieve hello nyckeln för det här kontot hello:

        azure storage account keys list <storage-account-name>

    Detta bör returnera **primära** och **sekundära** nycklar. Kopiera hello **primära** nyckelvärdet eftersom den används i hello nästa steg.
5. Använd hello efter kommandot tooretrieve en lista över blobbbehållare i hello storage-konto:

        azure storage container list -a <storage-account-name> -k <primary-key>
6. Använd följande kommandon tooupload hello och hämta filer toohello blob:

   * tooupload en fil:

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * toodownload en fil:

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> Om du kommer alltid att arbeta med hello samma lagringskonto som du kan ange hello följande miljövariabler i stället för att ange hello konto och nyckel för varje kommando:
>
> * **AZURE\_lagring\_konto**: hello lagringskontonamn
> * **AZURE\_lagring\_åtkomst\_NYCKELN**: hello lagringskontonyckel
>
>

### <a id="powershell"></a>Azure PowerShell
Azure PowerShell är en skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av dina arbetsbelastningar i Azure. Information om hur du konfigurerar din arbetsstation toorun Azure PowerShell finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**tooupload en lokal fil tooAzure Blob storage**

1. Öppna hello Azure PowerShell-konsolen som finns beskrivet i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).
2. Ange hello värdena för hello fem första variabler i hello följande skript:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get hello storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create hello storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy hello file from local workstation toohello Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. Klistra in hello skript i hello Azure PowerShell-konsolen toorun som toocopy hello-filen.

Till exempel PowerShell-skript har skapats toowork med HDInsight, se [HDInsight tools](https://github.com/blackmist/hdinsight-tools).

### <a id="azcopy"></a>AzCopy
AzCopy är ett kommandoradsverktyg som är utformade toosimplify hello aktiviteten för att överföra data till och från ett Azure Storage-konto. Du kan använda den som ett fristående verktyg eller innehålla verktyget i ett befintligt program. [Hämta AzCopy][azure-azcopy-download].

Hej AzCopy syntaxen är:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Mer information finns i [AzCopy - överföring/hämta filer för Azure-BLOB][azure-azcopy].

### <a id="commandline"></a>Hadoop-kommandorad
Hej Hadoop kommandoraden är bara användbara för att lagra data i blob-lagring när data hello finns redan på hello klustrets huvudnod.

I ordning toouse hello Hadoop-kommandot, måste du först ansluta toohello headnode med någon av följande metoder hello:

* **Windows-baserade HDInsight**: [ansluta med hjälp av fjärrskrivbord](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **Linux-baserat HDInsight**: ansluta via SSH ([hello SSH-kommandot](hdinsight-hadoop-linux-use-ssh-unix.md) eller [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))

När du är ansluten, kan du använda följande syntax tooupload en fil toostorage hello.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Till exempel, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Eftersom hello standardfilsystem för HDInsight i Azure Blob storage är /example/data.txt i Azure Blob storage. Du kan också se toohello filen som:

    wasb:///example/data/data.txt

eller

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

En lista med andra Hadoop-kommandon som fungerar med filer, finns [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> På HBase-kluster hello standardblockstorleken används när data skrivs är 256KB. När det här fungerar bra när du använder HBase APIs eller REST API: er med hjälp av hello `hadoop` eller `hdfs dfs` kommandon toowrite data är större än ~ 12 GB resulterar i ett fel. Se hello [skrivning till blob storage-undantag](#storageexception) avsnittet nedan för mer information.
>
>

## <a name="graphical-clients"></a>Grafisk klienter
Det finns också flera program som ger ett grafiskt gränssnitt för att arbeta med Azure Storage. hello nedan följer en lista över några av dessa program:

| Client | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio-verktygen för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |✔ |✔ |✔ |
| [Azure Storage Explorer](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Molnet lagring Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

### <a name="visual-studio-tools-for-hdinsight"></a>Visual Studio-verktygen för HDInsight
Mer information finns i [analysera hello länkade resurser](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

### <a id="storageexplorer"></a>Azure Lagringsutforskaren
*Azure Lagringsutforskaren* är användbart för att kontrollera och ändra hello data i BLOB. Det är ett kostnadsfritt, Öppna källa verktyg som kan hämtas från [http://storageexplorer.com/](http://storageexplorer.com/). hello källkoden är tillgänglig från den här länken samt.

Innan du använder verktyget hello, måste du känna till din Azure storage-konto och nyckel. Anvisningar om hur du får den här informationen finns hello ”så här: visa, kopiera och generera lagring åtkomstnycklar” avsnittet av [skapa, hantera eller ta bort ett lagringskonto][azure-create-storage-account].

1. Kör Azure Lagringsutforskaren. Om det är hello första gången du har kört hello Lagringsutforskaren, du uppmanas att hello **_Storage kontonamn** och **lagringskontonyckel**. Om du har kört den innan du använder hello **Lägg till** knappen tooadd ett nytt lagringskontonamn och nyckel.

    Ange hello namn och nyckel för hello storage-konto som används av ditt HDInsight-kluster och välj sedan **Spara & Öppna**.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. Hello namnet på hello-behållare som är kopplad till ditt HDInsight-kluster på hello listan behållare toohello kvar för hello-gränssnittet. Detta är hello namnet på hello HDInsight-kluster som standard men kan skilja sig om du har angett ett specifikt namn när du skapar hello kluster.
3. Välj hello överför ikonen hello verktygsfältet.

    ![Verktygsfält med överför ikonen markerat](./media/hdinsight-upload-data/toolbar.png)
4. Ange en fil tooupload och klicka sedan på **öppna**. När du uppmanas, Välj **överför** tooupload hello filen toohello rot hello lagringsbehållaren. Om du vill tooupload hello tooa specifika sökväg, ange hello sökväg i hello **mål** fältet och välj sedan **överför**.

    ![Överför fildialogruta](./media/hdinsight-upload-data/fileupload.png)

    När hello-filen har överförts, kan du använda den från jobb på hello HDInsight-kluster.

## <a name="mount-azure-blob-storage-as-local-drive"></a>Montera Azure Blob Storage som lokal enhet
Se [montera Azure Blob Storage som lokala enhet](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

## <a name="services"></a>Tjänster
### <a name="azure-data-factory"></a>Azure Data Factory
hello Azure Data Factory-tjänsten är en helt hanterad tjänst för att skapa lagring, databehandling och data movement datatjänster till produktion pipeline-effektiviserad, skalbara och tillförlitliga data.

Azure Data Factory kan vara används toomove data till Azure Blob storage eller toocreate data pipelines som använder direkt HDInsight funktioner som Hive och svin.

Mer information finns i hello [dokumentation för Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

### <a id="sqoop"></a>Apache Sqoop
Sqoop är ett verktyg tootransfer data mellan Hadoop och relationsdatabaser. Du kan använda den tooimport data från ett relationella databashanteringssystem (RDBMS) som SQL Server, MySQL eller Oracle till hello Hadoop distributed file system (HDFS), transformera hello data i Hadoop med MapReduce eller Hive och sedan exportera hello data tillbaka till en RDBMS.

Mer information finns i [använda Sqoop med HDInsight][hdinsight-use-sqoop].

## <a name="development-sdks"></a>SDK: er för utveckling
Azure Blob storage kan även nås med ett Azure SDK från hello följande programmeringsspråk:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Mer information om hur du installerar hello Azure SDK finns [hämtar Azure](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Felsökning
### <a id="storageexception"></a>Skrivning till blob Storage-undantag
**Symptom**: när du använder hello `hadoop` eller `hdfs dfs` kommandon toowrite filer som är ~ 12 GB eller större på ett HBase-kluster som kan uppstå hello följande fel:

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: hello request body is too large and exceeds hello maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Orsak**: HBase på HDInsight-kluster tooa standardblockstorleken på 256 KB när du skriver tooAzure lagring. När det här fungerar för HBase APIs eller REST API: er det kommer att resultera i ett fel när du använder hello `hadoop` eller `hdfs dfs` kommandoradsverktyg.

**Lösning**: Använd `fs.azure.write.request.size` toospecify en större blockstorlek. Du kan göra detta på grundval av per tillfälle genom att använda hello `-D` parameter. hello följande är ett exempel som använder den här parametern med hello `hadoop` kommando:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Du kan också öka hello värdet för `fs.azure.write.request.size` globalt med Ambari. hello följande steg kan vara används toochange hello värdet i hello Ambari-Webbgränssnittet:

1. Gå toohello Ambari-Webbgränssnittet för klustret i webbläsaren. Detta är https://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är hello namnet på klustret.

    När du uppmanas ange hello admin namn och lösenord för hello klustret.
2. Hello vänster sida av hello-skärmen, Välj **HDFS**, och välj sedan hello **konfigurationerna** fliken.
3. I hello **Filter...**  anger `fs.azure.write.request.size`. Visas hello fältet och det aktuella värdet hello mitten av hello-sidan.
4. Ändra hello värdet från 262144 (256KB) toohello nytt värde. Till exempel 4194304 (4MB).

![Bild av ändra hello-värdet via Ambari-Webbgränssnittet](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Läs mer om hur du använder Ambari [hantera HDInsight-kluster med Ambari-Webbgränssnittet för hello](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Nästa steg
Nu när du förstår hur tooget data till HDInsight, läsa hello följande artiklar toolearn hur tooperform analys:

* [Kom igång med Azure HDInsight][hdinsight-get-started]
* [Skicka Hadoop-jobb via programmering][hdinsight-submit-jobs]
* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Pig med HDInsight][hdinsight-use-pig]

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]:../storage/common/storage-create-storage-account.md
[azure-azcopy-download]:../storage/common/storage-use-azcopy.md
[azure-azcopy]:../storage/common/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
