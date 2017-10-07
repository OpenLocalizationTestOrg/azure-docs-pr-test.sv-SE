---
title: aaaUse Azure-mallar toocreate HDInsight och Data Lake Store | Microsoft Docs
description: "Använda Azure Resource Manager mallar toocreate och använda HDInsight-kluster med Azure Data Lake Store"
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/04/2017
ms.author: nitinme
ms.openlocfilehash: eb88a626f2837dcc29295f3f73a91757059c3bb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a>Skapa ett HDInsight-kluster med Data Lake Store med Azure Resource Manager-mall
> [!div class="op_single_selector"]
> * [Använda portalen](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Med hjälp av PowerShell (för standardlagring)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Med hjälp av PowerShell (för ytterligare lagringsutrymme)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Med Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Lär dig hur toouse Azure PowerShell tooconfigure ett HDInsight-kluster med Azure Data Lake Store **som ytterligare lagringsutrymme**.

För stöds klustertyper användas Data Lake Store som standardlagring eller ytterligare storage-konto. När du använder Data Lake Store som ytterligare lagringsutrymme hello standardkontot för lagring för hello kluster kommer fortfarande att Azure Storage BLOB (WASB) och hello kluster-relaterade filer (till exempel loggar osv.) skrivs fortfarande toohello standardlagring medan hello data som du vill tooprocess kan lagras i ett Data Lake Store-konto. Med Data Lake Store som ett ytterligare storage-konto inte påverkar prestanda eller hello möjlighet tooread/skrivning toohello lagring från hello kluster.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Med hjälp av Data Lake Store för HDInsight klusterlagring

Här följer några viktiga överväganden när för att använda HDInsight med Data Lake Store:

* Alternativet toocreate HDInsight-kluster med åtkomst tooData Datasjölager som standardlagring är tillgänglig för HDInsight version 3.5 och 3,6.

* Alternativet toocreate HDInsight-kluster med åtkomst tooData Datasjölager som ytterligare lagring är tillgänglig för HDInsight version 3.2, 3.4, 3.5 och 3,6.

I den här artikeln etablera vi ett Hadoop-kluster med Data Lake Store som ytterligare lagringsutrymme. Anvisningar för hur toocreate ett Hadoop-kluster med Data Lake Store som standardlagring finns [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).

## <a name="prerequisites"></a>Krav
Innan du påbörjar den här självstudien måste du ha hello följande:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Installera Azure PowerShell 1.0 eller senare**. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).
* **Azure Active Directory Service Principal**. Stegen i den här kursen ger instruktioner för hur toocreate ett huvudnamn för tjänsten i Azure AD. Du måste dock vara en kan toocreate för Azure AD-administratör toobe på ett huvudnamn för tjänsten. Om du är administratör för Azure AD kan du hoppa över det här kravet och fortsätt med hello kursen.

    **Om du inte är en Azure AD-administratör**, kommer du inte att kunna tooperform hello steg krävs toocreate ett huvudnamn för tjänsten. I sådana fall måste din Azure AD-administratör först skapa ett huvudnamn för tjänsten innan du kan skapa ett HDInsight-kluster med Data Lake Store. Dessutom hello tjänstens huvudnamn måste skapas med hjälp av ett certifikat, enligt beskrivningen i [skapa ett huvudnamn för tjänsten med certifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a>Skapa ett HDInsight-kluster med Azure Data Lake Store
hello Resource Manager-mall och hello förutsättningarna för att använda hello-mallen finns på GitHub på [distribuera ett HDInsight Linux-kluster med nya Data Lake Store](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage). Följ hello instruktionerna på den här länken toocreate ett HDInsight-kluster med Azure Data Lake Store som hello ytterligare lagringsutrymme.

hello instruktionerna på hello länken ovan kräver PowerShell. Innan du börjar med dessa instruktioner måste du kontrollera att du loggar in tooyour Azure-konto. Öppna ett nytt Azure PowerShell-fönster på skrivbordet och ange hello följande kodavsnitt. När du tillfrågas toolog i, se till att logga in som en hello prenumerationens admininistratörer/ägare:

```
# Log in tooyour Azure account
Login-AzureRmAccount

# List all hello subscriptions associated tooyour account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-toohello-azure-data-lake-store"></a>Överför exempel data toohello Azure Data Lake Store
hello Resource Manager-mall skapas ett nytt Data Lake Store-konto och associerar den med hello HDInsight-kluster. Nu måste du överföra några exempel på en data-toohello Data Lake Store. Du behöver dessa data senare i hello självstudiekursen toorun jobb från ett HDInsight-kluster som har åtkomst till data i hello Data Lake Store. Anvisningar för hur tooupload data finns i [överför en fil tooyour Datasjölager](data-lake-store-get-started-portal.md#uploaddata). Om du vill söka efter vissa exempel data tooupload, kan du få hello **Ambulansdata** mapp från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData).

## <a name="set-relevant-acls-on-hello-sample-data"></a>Ange relevant ACL: er på hello exempeldata
toomake hello exempeldata som du överför är åtkomlig från hello HDInsight-kluster, måste du se till att hello Azure AD-program som används tooestablish identitet mellan hello HDInsight-kluster och Data Lake Store har du åtkomst toohello fil/mapp försöker tooaccess. toodo, utför följande steg hello.

1. Hitta hello namn hello Azure AD-program som är associerat med HDInsight-kluster och hello Data Lake Store. Enkelriktade toolook för hello namn är tooopen hello HDInsight-klusterbladet som du skapade med hello Resource Manager-mall, klickar du på hello **kluster-AAD-identitet** fliken och Sök efter hello värdet för **tjänstens huvudnamn Visningsnamn**.
2. Nu kan ange åtkomst toothis Azure AD-program på hello filen/mappen som du vill tooaccess från hello HDInsight-kluster. tooset hello höger ACL: er på hello fil/mapp i Data Lake Store finns [skydda data i Data Lake Store](data-lake-store-secure-data.md#filepermissions).

## <a name="run-test-jobs-on-hello-hdinsight-cluster-toouse-hello-data-lake-store"></a>Kör testjobb på hello HDInsight-kluster toouse hello Data Lake Store
När du har konfigurerat ett HDInsight-kluster, kan du köra testjobb på hello klustret tootest som hello HDInsight klustret kan komma åt Data Lake Store. toodo så vi ska köra en Hive-jobb som skapar en tabell med hello exempeldata som du överfört tidigare tooyour Data Lake Store.

I det här avsnittet kommer du att SSH i ett kluster i HDInsight Linux och köra hello en exempelfråga Hive. Om du använder en Windows-klient, bör du använda **PuTTY**, som kan hämtas från [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Läs mer om hur du använder PuTTY [använda SSH med Linux-baserade Hadoop i HDInsight från Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. När du är ansluten, starta hello Hive CLI med hello följande kommando:

   ```
   hive
   ```
2. Med hjälp av hello CLI, ange hello följande instruktioner toocreate en ny tabell med namnet **fordon** med hello exempeldata i hello Data Lake Store:

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   Du bör se en utdata liknande toohello följande:

   ```
   1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
   1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
   1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
   1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
   1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
   1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
   1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
   1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
   1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
   1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1
   ```


## <a name="access-data-lake-store-using-hdfs-commands"></a>Åtkomst till Data Lake Store med hjälp av HDFS-kommandon
När du har konfigurerat hello HDInsight klustret toouse Data Lake Store kan använda du hello HDFS shell-kommandon tooaccess hello store.

I det här avsnittet kommer du att SSH i ett kluster i HDInsight Linux och köra hello HDFS-kommandon. Om du använder en Windows-klient, bör du använda **PuTTY**, som kan hämtas från [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Läs mer om hur du använder PuTTY [använda SSH med Linux-baserade Hadoop i HDInsight från Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

När du är ansluten, Använd hello följande HDFS filesystem kommandot toolist hello filer i hello Data Lake Store.

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

Du bör nu se hello-fil som du överfört tidigare toohello Data Lake Store.

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

Du kan också använda hello `hdfs dfs -put` kommandot tooupload vissa filer toohello Data Lake Store och sedan använda `hdfs dfs -ls` tooverify hello filer har laddats.


## <a name="next-steps"></a>Nästa steg
* [Kopiera data från Azure Storage BLOB tooData Datasjölager](data-lake-store-copy-data-wasb-distcp.md)
