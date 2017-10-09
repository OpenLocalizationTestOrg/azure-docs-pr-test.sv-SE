---
title: "aaaUse Azure kommandoradsverktyget 2.0 gränssnittet tooget igång med Azure Data Lake Store | Microsoft Docs"
description: "Använda kommandoraden för Azure plattformsoberoende 2.0 toocreate ett Data Lake Store-konto och utföra grundläggande åtgärder"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 4ffa0f4a-1cca-46ac-803d-1fc8538c685b
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 374dcd6cdbc13ad19f6c65502329986ecae60ef2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20"></a>Komma igång med Azure Data Lake Store med hjälp av Azure CLI 2.0
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Lär dig hur toouse Azure CLI 2.0 toocreate ett Azure Data Lake lagrar konto och utföra grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta datafiler, ta bort ditt konto, osv. Mer information om Data Lake Store finns i [Översikt över Data Lake Store](data-lake-store-overview.md).

hello Azure CLI 2.0 är Azures nya kommandoradsverktyget upplevelsen för att hantera Azure-resurser. Den kan användas i Mac OS, Linux och Windows. Mer information finns i [Översikt över Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview). Du kan också titta på hello [referens för Azure Data Lake Store CLI 2.0](https://docs.microsoft.com/cli/azure/dls) för en fullständig lista över kommandon och syntax.


## <a name="prerequisites"></a>Krav
Du måste ha hello följande innan du börjar den här artikeln:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Azure CLI 2.0** – Anvisningar finns i [Installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="authentication"></a>Autentisering

Den här artikeln använder en enklare metod för autentisering med Data Lake Store där du loggar in som en användare av slutanvändaren. hello åtkomst nivå tooData Lake Store-konto och filsystem regleras sedan hello åtkomstnivån för hello inloggad användare. Men det finns andra metoder som korrekt tooauthenticate med Data Lake Store, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**. Anvisningar och mer information om hur tooauthenticate, se [slutanvändarens autentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [tjänst-till-tjänst autentisering](data-lake-store-authenticate-using-active-directory.md).


## <a name="log-in-tooyour-azure-subscription"></a>Logga in tooyour Azure-prenumeration

1. Logga in till din Azure-prenumeration.

    ```azurecli
    az login
    ```

    Du kan hämta en kod toouse i hello nästa steg. Använd en web webbläsare tooopen hello sidan https://aka.ms/devicelogin och ange hello koden tooauthenticate. Du kan ange toolog in med dina autentiseringsuppgifter.

2. När du loggar in hello hello fönster visar alla Azure-prenumerationer som är kopplade till ditt konto. Använd följande kommando toouse en viss prenumeration hello.
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a>Skapa ett Azure Data Lake Store-konto

1. Skapa en ny resursgrupp. Hello följande kommando, anger du hello parametervärden som du vill toouse. Om hello platsnamnet innehåller blanksteg måste du placera det inom citattecken. Till exempel "USA, östra 2". 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. Skapa hello Data Lake Store-konto.
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a>Skapa mappar i ett Data Lake Store-konto

Du kan skapa mappar under toomanage din Azure Data Lake Store-konto och lagra data. Använd hello efter kommandot toocreate en mapp som kallas **mynewfolder** hello roten i hello Data Lake Store.

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> Hej `--folder` parametern säkerställer att hello kommando skapar en mapp. Om den här parametern inte finns skapar hello kommando en tom fil med namnet mynewfolder hello roten i hello Data Lake Store-konto.
> 
>

## <a name="upload-data-tooa-data-lake-store-account"></a>Ladda upp data tooa Data Lake Store-konto

Du kan ladda upp data tooData Lake Store direkt på hello nivå eller tooa rotmapp som du skapade i hello-konto. hello fragmenten nedan visar hur tooupload mappen vissa exempeldata toohello (**mynewfolder**) du skapade i föregående avsnitt i hello.

Om du vill söka efter vissa exempel data tooupload, kan du få hello **Ambulansdata** mapp från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Hämta hello-filen och lagra den på en lokal katalog på din dator, till exempel C:\sampledata\.

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> Du måste ange hello fullständiga sökvägen och filnamnet för hello för hello mål.
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a>Visa en lista över filer i ett Data Lake Store-konto

Använd hello följande kommando toolist hello filer i ett Data Lake Store-konto.

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
```

hello resultatet av detta bör vara liknande toohello följande:

    [
        {
            "accessTime": 1491323529542,
            "aclBit": false,
            "blockSize": 268435456,
            "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "length": 1589881,
            "modificationTime": 1491323531638,
            "msExpirationTime": 0,
            "name": "mynewfolder/vehicle1_09142014.csv",
            "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "pathSuffix": "vehicle1_09142014.csv",
            "permission": "770",
            "replication": 1,
            "type": "FILE"
        }
    ]

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a>Byt namn på, ladda ned och ta bort data från ett Data Lake Store-konto 

* **toorename en fil**, använda hello följande kommando:
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* **toodownload en fil**, Använd följande kommando hello. Kontrollera att hello målsökväg som du anger redan finns.
  
    ```azurecli     
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > hello-kommando skapar hello målmappen om det inte finns.
    > 
    >

* **toodelete en fil**, använda hello följande kommando:
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    Om du vill toodelete hello mapp **mynewfolder** och hello **vehicle1_09142014_copy.csv** tillsammans i ett kommando, Använd hello--recurse parametern

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a>Arbeta med behörigheter och ACL:er för ett Data Lake Store-konto

I det här avsnittet du lär dig mer om hur toomanage ACL: er och behörigheter som använder Azure CLI 2.0. Detaljerad information om hur ACL:er implementeras i Azure Data Lake Store finns i [Åtkomstkontroll i Azure Data Lake Store](data-lake-store-access-control.md).

* **tooupdate hello ägaren av en filmapp**, använda hello följande kommando:

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* **tooupdate hello behörigheter för en filmapp**, använda hello följande kommando:

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* **tooget hello ACL: er för sökvägens**, använda hello följande kommando:

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
    ```

    hello utdata ska vara liknande toohello följande:

        {
            "entries": [
            "user::rwx",
            "group::rwx",
            "other::---"
          ],
          "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "permission": "770",
          "stickyBit": false
        }

* **tooset en post för en ACL**, använda hello följande kommando:

    ```azurecli
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* **tooremove en post för en ACL**, använda hello följande kommando:

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* **tooremove en hel standard ACL**, använda hello följande kommando:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* **tooremove en hel icke-förvalt ACL**, använda hello följande kommando:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a>Ta bort ett Data Lake Store-konto
Använd hello efter kommandot toodelete ett Data Lake Store-konto.

```azurecli
az dls account delete --account mydatalakestore
```

När du uppmanas, anger **Y** toodelete hello-konto.

## <a name="next-steps"></a>Nästa steg

* [Azure Data Lake Store CLI 2.0-referens](https://docs.microsoft.com/cli/azure/dls)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md
