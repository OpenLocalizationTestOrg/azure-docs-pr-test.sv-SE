---
title: "Överföra data för Hadoop-jobb i HDInsight | Microsoft Docs"
description: "Lär dig mer om att överföra och komma åt data för Hadoop-jobb i HDInsight med hjälp av Azure CLI, Azure Lagringsutforskaren, Azure PowerShell, kommandorad för Hadoop eller Sqoop."
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
ms.openlocfilehash: 6867f96c8ea0e31ed0e682cef48e7aa5e3f65f86
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Ladda upp data för Hadoop-jobb i HDInsight
Azure HDInsight ger en komplett Hadoop distributed file system (HDFS) över Azure Blob storage. Den är utformad som ett HDFS-tillägg för att förse kunder en sömlös upplevelse. Den gör en fullständig uppsättning komponenter i Hadoop-ekosystemet att arbeta direkt med de data som den hanterar. Azure Blob storage och HDFS är distinkt filsystem som är optimerade för lagring av data och beräkningar på dessa data. Information om fördelarna med att använda Azure Blob storage finns [använda Azure Blob storage med HDInsight][hdinsight-storage].

**Förutsättningar**

Observera följande krav innan du börjar:

* Ett Azure HDInsight-kluster. Instruktioner finns i [komma igång med Azure HDInsight] [ hdinsight-get-started] eller [etablera HDInsight-kluster][hdinsight-provision].

## <a name="why-blob-storage"></a>Varför blob storage?
Azure HDInsight-kluster distribueras vanligtvis för att köra MapReduce-jobb och kluster tas bort när de här jobben har slutförts. Behålla data i HDFS är kluster när beräkningar har slutförts en kostsam sätt att lagra dessa data. Azure Blob storage är en hög tillgänglighet, skalbara, hög kapacitet, låg kostnad och delbart lagringsalternativ för data som ska bearbetas med HDInsight. Lagra data i en blob kan HDInsight-kluster som används för beräkning frigörs utan att förlora data på ett säkert sätt.

### <a name="directories"></a>Kataloger
Azure Blob storage-behållare som lagrar data som nyckel/värde-par och det finns ingen Kataloghierarki. Men tecknet ”/” kan användas i nyckelnamnet så att det visas som om en fil lagras i en katalogstruktur. HDInsight ser dessa som om de faktiska kataloger.

Nyckeln för en blob kan till exempel vara *input/log1.txt*. Det finns inga faktiska ”input” katalog, men på grund av förekomsten av ”/”-tecken i namnet har utseendet på en sökväg till filen.

Om du använder Azure Explorer-verktyg kan du märka vissa 0 byte-filer. Filerna har två syften:

* Om det finns tomma mappar, markera de av finns i mappen. Azure Blob storage är smarta du behöver veta om det finns en blob som kallas foo-fältet, det finns en mapp med namnet **foo**. Men det enda sättet för att ange en tom mapp att kallas **foo** av är att ha särskilda 0 byte filen på plats.
* De innehåller särskilda metadata som krävs av Hadoop-filsystem, särskilt behörigheter och ägare för mappar.

## <a name="command-line-utilities"></a>Kommandoradsverktyg
Microsoft tillhandahåller följande verktyg för att arbeta med Azure Blob storage:

| Verktyget | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure-kommandoradsgränssnittet][azurecli] |✔ |✔ |✔ |
| [Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] | | |✔ |
| [Hadoop-kommando](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Medan Azure CLI, Azure PowerShell och AzCopy kan alla användas utanför Azure, Hadoop kommandot är endast tillgängligt för HDInsight-klustret och kan bara läsa in data från det lokala filsystemet i Azure Blob storage.
>
>

### <a id="xplatcli"></a>Azure CLI
Azure CLI är ett plattformsoberoende verktyg som hjälper dig att hantera Azure-tjänster. Använd följande steg för att överföra data till Azure Blob storage:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Installera och konfigurera Azure CLI för Mac, Linux och Windows](../cli-install-nodejs.md).
2. Öppna en kommandotolk, bash eller andra gränssnitt och Använd följande för att autentisera till Azure-prenumeration.

        azure login

    När du uppmanas, anger du användarnamn och lösenord för din prenumeration.
3. Ange följande kommando för att visa en lista med lagringskonton för din prenumeration:

        azure storage account list
4. Välj lagringskonto som innehåller blob som du vill arbeta med och sedan använder du följande kommando för att hämta nyckeln för det här kontot:

        azure storage account keys list <storage-account-name>

    Detta bör returnera **primära** och **sekundära** nycklar. Kopiera den **primära** nyckelvärdet eftersom den används i nästa steg.
5. Använd följande kommando för att hämta en lista över blobbbehållare i storage-konto:

        azure storage container list -a <storage-account-name> -k <primary-key>
6. Ladda upp och laddar ned filer till blob med hjälp av följande kommandon:

   * Att överföra en fil:

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * Att hämta en fil:

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> Om du kommer alltid att arbeta med samma lagringskonto, kan du ange följande miljövariabler i stället för att ange konto och nyckel för varje kommando:
>
> * **AZURE\_lagring\_konto**: lagringskontonamnet
> * **AZURE\_lagring\_åtkomst\_NYCKELN**: lagringskontots åtkomstnyckel
>
>

### <a id="powershell"></a>Azure PowerShell
Azure PowerShell är en skriptmiljö som du kan använda för att styra och automatisera distributionen och hanteringen av dina arbetsbelastningar i Azure. Information om hur du konfigurerar din arbetsstation för att köra Azure PowerShell finns i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Att överföra en lokal fil till Azure Blob storage**

1. Öppna Azure PowerShell-konsol som finns beskrivet i [installera och konfigurera Azure PowerShell](/powershell/azure/overview).
2. Ange värden för de första fem variablerna i skriptet med följande:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. Klistra in skriptet i Azure PowerShell-konsolen för att köra den att kopiera filen.

Till exempel PowerShell-skript som skapats för att arbeta med HDInsight, se [HDInsight tools](https://github.com/blackmist/hdinsight-tools).

### <a id="azcopy"></a>AzCopy
AzCopy är ett kommandoradsverktyg som förenklar uppgiften att överföra data till och från ett Azure Storage-konto. Du kan använda den som ett fristående verktyg eller innehålla verktyget i ett befintligt program. [Hämta AzCopy][azure-azcopy-download].

AzCopy-syntaxen är:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Mer information finns i [AzCopy - överföring/hämta filer för Azure-BLOB][azure-azcopy].

### <a id="commandline"></a>Hadoop-kommandorad
Hadoop-kommandoraden är bara användbara för att lagra data i blob-lagring när data finns redan på klustrets huvudnod.

För att kunna använda kommandot Hadoop, måste du först ansluta till headnode med någon av följande metoder:

* **Windows-baserade HDInsight**: [ansluta med hjälp av fjärrskrivbord](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **Linux-baserat HDInsight**: ansluta via SSH ([SSH-kommandot](hdinsight-hadoop-linux-use-ssh-unix.md) eller [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))

När du är ansluten, kan du använda följande syntax för att överföra en fil till lagring.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Till exempel, `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Eftersom filsystemet för HDInsight i Azure Blob storage är /example/data.txt i Azure Blob storage. Du kan också gå till filen som:

    wasb:///example/data/data.txt

eller

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

En lista med andra Hadoop-kommandon som fungerar med filer, finns [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> I HBase-kluster standard blockstorlek som används när data skrivs är 256KB. När det här fungerar bra när du använder HBase APIs eller REST API: er med hjälp av den `hadoop` eller `hdfs dfs` kommandon för att skriva data som är större än ~ 12 GB resulterar i ett fel. Finns det [skrivning till blob storage-undantag](#storageexception) avsnittet nedan för mer information.
>
>

## <a name="graphical-clients"></a>Grafisk klienter
Det finns också flera program som ger ett grafiskt gränssnitt för att arbeta med Azure Storage. Följande är en lista över några av dessa program:

| Client | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio-verktygen för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |✔ |✔ |✔ |
| [Azure Storage Explorer](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Molnet lagring Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

### <a name="visual-studio-tools-for-hdinsight"></a>Visual Studio-verktygen för HDInsight
Mer information finns i [navigera de länkade resurserna](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

### <a id="storageexplorer"></a>Azure Lagringsutforskaren
*Azure Lagringsutforskaren* är användbart för att kontrollera och ändra data i BLOB. Det är ett kostnadsfritt, Öppna källa verktyg som kan hämtas från [http://storageexplorer.com/](http://storageexplorer.com/). Källkoden är tillgänglig från den här länken samt.

Innan du använder verktyget, måste du känna till din Azure storage-konto och nyckel. Anvisningar om hur du får den här informationen finns i ”så här: visa, kopiera och generera lagring åtkomstnycklar” avsnitt i [skapa, hantera eller ta bort ett lagringskonto][azure-create-storage-account].

1. Kör Azure Lagringsutforskaren. Om det här är första gången du har kört Lagringsutforskaren, du uppmanas att den **_Storage kontonamn** och **lagringskontonyckel**. Om du har kört den innan du använder den **Lägg till** för att lägga till ett nytt lagringskontonamn och nyckel.

    Ange namnet och nyckeln för storage-konto som används av ditt HDInsight-kluster och välj sedan **Spara & Öppna**.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]
2. I listan över behållare till vänster om gränssnittet, klickar du på namnet på behållaren som är kopplad till ditt HDInsight-kluster. Detta är namnet på HDInsight-klustret som standard men kan skilja sig om du har angett ett specifikt namn när du skapar klustret.
3. I verktygsfältet, väljer du ikonen överföringen.

    ![Verktygsfält med överför ikonen markerat](./media/hdinsight-upload-data/toolbar.png)
4. Ange en fil för att ladda upp och klicka sedan på **öppna**. När du uppmanas, Välj **överför** att överföra filen till roten på lagringsbehållaren. Om du vill överföra filen till en specifik sökväg anger du sökvägen i den **mål** fältet och välj sedan **överför**.

    ![Överför fildialogruta](./media/hdinsight-upload-data/fileupload.png)

    När filen har överförts, kan du använda den från jobb i HDInsight-klustret.

## <a name="mount-azure-blob-storage-as-local-drive"></a>Montera Azure Blob Storage som lokal enhet
Se [montera Azure Blob Storage som lokala enhet](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

## <a name="services"></a>Tjänster
### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory-tjänsten är en helt hanterad tjänst för att skapa lagring, databehandling och data movement datatjänster till produktion pipeline-effektiviserad, skalbara och tillförlitliga data.

Azure Data Factory kan användas för att flytta data till Azure Blob storage, eller för att skapa data pipelines som använder HDInsight-funktioner, till exempel Hive och Pig direkt.

Mer information finns i [dokumentation för Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

### <a id="sqoop"></a>Apache Sqoop
Sqoop är ett verktyg som utformats för att överföra data mellan Hadoop och relationsdatabaser. Du kan använda den för att importera data från ett relationella databashanteringssystem (RDBMS), exempelvis SQL Server, MySQL eller Oracle i Hadoop distributed file system (HDFS) Transformera data i Hadoop med MapReduce eller Hive och exportera data till en RDBMS.

Mer information finns i [använda Sqoop med HDInsight][hdinsight-use-sqoop].

## <a name="development-sdks"></a>SDK: er för utveckling
Azure Blob storage kan även nås med ett Azure SDK från följande programmeringsspråk:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Mer information om hur du installerar Azure SDK: erna finns [hämtar Azure](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Felsökning
### <a id="storageexception"></a>Skrivning till blob Storage-undantag
**Symptom**: när du använder den `hadoop` eller `hdfs dfs` kommandon för att skriva filer som är ~ 12 GB eller större på ett HBase-kluster kan du stöta på följande fel:

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
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Orsak**: HBase på HDInsight-kluster standard till en blockstorlek på 256 KB när du skriver till Azure-lagring. När det här fungerar för HBase APIs eller REST API: er det kommer att resultera i ett fel när du använder den `hadoop` eller `hdfs dfs` kommandoradsverktyg.

**Lösning**: Använd `fs.azure.write.request.size` att ange en större blockstorlek. Du kan göra detta på grundval av per användning med hjälp av den `-D` parameter. Följande är ett exempel som använder den här parametern med det `hadoop` kommando:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Du kan också öka värdet för `fs.azure.write.request.size` globalt med Ambari. Följande steg kan användas för att ändra värdet i Ambari-Webbgränssnittet:

1. Gå till Ambari-Webbgränssnittet för klustret i din webbläsare. Detta är https://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är namnet på klustret.

    När du uppmanas, anger du admin namn och lösenord för klustret.
2. Vänster sida av skärmen, Välj **HDFS**, och välj sedan den **konfigurationerna** fliken.
3. I den **Filter...**  anger `fs.azure.write.request.size`. Detta visas i fältet och det aktuella värdet i mitten på sidan.
4. Ändra värdet från 262144 (256KB) till det nya värdet. Till exempel 4194304 (4MB).

![Bild av ändra värdet via Ambari-Webbgränssnittet](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Läs mer om hur du använder Ambari [hantera HDInsight-kluster med Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Nästa steg
Nu när du vet hur du hämta data till HDInsight kan du läsa följande artiklar om du vill lära dig att utföra analyser av:

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
