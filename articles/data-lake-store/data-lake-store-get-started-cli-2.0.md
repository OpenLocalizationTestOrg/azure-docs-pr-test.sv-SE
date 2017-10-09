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
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20"></a><span data-ttu-id="669fd-103">Komma igång med Azure Data Lake Store med hjälp av Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="669fd-103">Get started with Azure Data Lake Store using Azure CLI 2.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="669fd-104">Portal</span><span class="sxs-lookup"><span data-stu-id="669fd-104">Portal</span></span>](data-lake-store-get-started-portal.md)
> * [<span data-ttu-id="669fd-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="669fd-105">PowerShell</span></span>](data-lake-store-get-started-powershell.md)
> * [<span data-ttu-id="669fd-106">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="669fd-106">.NET SDK</span></span>](data-lake-store-get-started-net-sdk.md)
> * [<span data-ttu-id="669fd-107">Java SDK</span><span class="sxs-lookup"><span data-stu-id="669fd-107">Java SDK</span></span>](data-lake-store-get-started-java-sdk.md)
> * [<span data-ttu-id="669fd-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="669fd-108">REST API</span></span>](data-lake-store-get-started-rest-api.md)
> * [<span data-ttu-id="669fd-109">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="669fd-109">Azure CLI 2.0</span></span>](data-lake-store-get-started-cli-2.0.md)
> * [<span data-ttu-id="669fd-110">Node.js</span><span class="sxs-lookup"><span data-stu-id="669fd-110">Node.js</span></span>](data-lake-store-manage-use-nodejs.md)
> * [<span data-ttu-id="669fd-111">Python</span><span class="sxs-lookup"><span data-stu-id="669fd-111">Python</span></span>](data-lake-store-get-started-python.md)
>
>

<span data-ttu-id="669fd-112">Lär dig hur toouse Azure CLI 2.0 toocreate ett Azure Data Lake lagrar konto och utföra grundläggande åtgärder, till exempel skapa mappar, ladda upp och hämta datafiler, ta bort ditt konto, osv. Mer information om Data Lake Store finns i [Översikt över Data Lake Store](data-lake-store-overview.md).</span><span class="sxs-lookup"><span data-stu-id="669fd-112">Learn how toouse Azure CLI 2.0 toocreate an Azure Data Lake Store account and perform basic operations such as create folders, upload and download data files, delete your account, etc. For more information about Data Lake Store, see [Overview of Data Lake Store](data-lake-store-overview.md).</span></span>

<span data-ttu-id="669fd-113">hello Azure CLI 2.0 är Azures nya kommandoradsverktyget upplevelsen för att hantera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="669fd-113">hello Azure CLI 2.0 is Azure's new command-line experience for managing Azure resources.</span></span> <span data-ttu-id="669fd-114">Den kan användas i Mac OS, Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="669fd-114">It can be used on macOS, Linux, and Windows.</span></span> <span data-ttu-id="669fd-115">Mer information finns i [Översikt över Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="669fd-115">For more information, see [Overview of Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview).</span></span> <span data-ttu-id="669fd-116">Du kan också titta på hello [referens för Azure Data Lake Store CLI 2.0](https://docs.microsoft.com/cli/azure/dls) för en fullständig lista över kommandon och syntax.</span><span class="sxs-lookup"><span data-stu-id="669fd-116">You can also look at hello [Azure Data Lake Store CLI 2.0 reference](https://docs.microsoft.com/cli/azure/dls) for a complete list of commands and syntax.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="669fd-117">Krav</span><span class="sxs-lookup"><span data-stu-id="669fd-117">Prerequisites</span></span>
<span data-ttu-id="669fd-118">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="669fd-118">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="669fd-119">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="669fd-119">**An Azure subscription**.</span></span> <span data-ttu-id="669fd-120">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="669fd-120">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="669fd-121">**Azure CLI 2.0** – Anvisningar finns i [Installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="669fd-121">**Azure CLI 2.0** - See [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) for instructions.</span></span>

## <a name="authentication"></a><span data-ttu-id="669fd-122">Autentisering</span><span class="sxs-lookup"><span data-stu-id="669fd-122">Authentication</span></span>

<span data-ttu-id="669fd-123">Den här artikeln använder en enklare metod för autentisering med Data Lake Store där du loggar in som en användare av slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="669fd-123">This article uses a simpler authentication approach with Data Lake Store where you log in as an end-user user.</span></span> <span data-ttu-id="669fd-124">hello åtkomst nivå tooData Lake Store-konto och filsystem regleras sedan hello åtkomstnivån för hello inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="669fd-124">hello access level tooData Lake Store account and file system is then governed by hello access level of hello logged in user.</span></span> <span data-ttu-id="669fd-125">Men det finns andra metoder som korrekt tooauthenticate med Data Lake Store, som är **slutanvändarens autentisering** eller **tjänst-till-tjänst autentisering**.</span><span class="sxs-lookup"><span data-stu-id="669fd-125">However, there are other approaches as well tooauthenticate with Data Lake Store, which are **end-user authentication** or **service-to-service authentication**.</span></span> <span data-ttu-id="669fd-126">Anvisningar och mer information om hur tooauthenticate, se [slutanvändarens autentisering](data-lake-store-end-user-authenticate-using-active-directory.md) eller [tjänst-till-tjänst autentisering](data-lake-store-authenticate-using-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="669fd-126">For instructions and more information on how tooauthenticate, see [End-user authentication](data-lake-store-end-user-authenticate-using-active-directory.md) or [Service-to-service authentication](data-lake-store-authenticate-using-active-directory.md).</span></span>


## <a name="log-in-tooyour-azure-subscription"></a><span data-ttu-id="669fd-127">Logga in tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="669fd-127">Log in tooyour Azure subscription</span></span>

1. <span data-ttu-id="669fd-128">Logga in till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="669fd-128">Log into your Azure subscription.</span></span>

    ```azurecli
    az login
    ```

    <span data-ttu-id="669fd-129">Du kan hämta en kod toouse i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="669fd-129">You get a code toouse in hello next step.</span></span> <span data-ttu-id="669fd-130">Använd en web webbläsare tooopen hello sidan https://aka.ms/devicelogin och ange hello koden tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="669fd-130">Use a web browser tooopen hello page https://aka.ms/devicelogin and enter hello code tooauthenticate.</span></span> <span data-ttu-id="669fd-131">Du kan ange toolog in med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="669fd-131">You are prompted toolog in using your credentials.</span></span>

2. <span data-ttu-id="669fd-132">När du loggar in hello hello fönster visar alla Azure-prenumerationer som är kopplade till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="669fd-132">Once you log in, hello window lists all hello Azure subscriptions that are associated with your account.</span></span> <span data-ttu-id="669fd-133">Använd följande kommando toouse en viss prenumeration hello.</span><span class="sxs-lookup"><span data-stu-id="669fd-133">Use hello following command toouse a specific subscription.</span></span>
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a><span data-ttu-id="669fd-134">Skapa ett Azure Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="669fd-134">Create an Azure Data Lake Store account</span></span>

1. <span data-ttu-id="669fd-135">Skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="669fd-135">Create a new resource group.</span></span> <span data-ttu-id="669fd-136">Hello följande kommando, anger du hello parametervärden som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="669fd-136">In hello following command, provide hello parameter values you want toouse.</span></span> <span data-ttu-id="669fd-137">Om hello platsnamnet innehåller blanksteg måste du placera det inom citattecken.</span><span class="sxs-lookup"><span data-stu-id="669fd-137">If hello location name contains spaces, put it in quotes.</span></span> <span data-ttu-id="669fd-138">Till exempel "USA, östra 2".</span><span class="sxs-lookup"><span data-stu-id="669fd-138">For example "East US 2".</span></span> 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. <span data-ttu-id="669fd-139">Skapa hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="669fd-139">Create hello Data Lake Store account.</span></span>
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a><span data-ttu-id="669fd-140">Skapa mappar i ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="669fd-140">Create folders in a Data Lake Store account</span></span>

<span data-ttu-id="669fd-141">Du kan skapa mappar under toomanage din Azure Data Lake Store-konto och lagra data.</span><span class="sxs-lookup"><span data-stu-id="669fd-141">You can create folders under your Azure Data Lake Store account toomanage and store data.</span></span> <span data-ttu-id="669fd-142">Använd hello efter kommandot toocreate en mapp som kallas **mynewfolder** hello roten i hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="669fd-142">Use hello following command toocreate a folder called **mynewfolder** at hello root of hello Data Lake Store.</span></span>

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> <span data-ttu-id="669fd-143">Hej `--folder` parametern säkerställer att hello kommando skapar en mapp.</span><span class="sxs-lookup"><span data-stu-id="669fd-143">hello `--folder` parameter ensures that hello command creates a folder.</span></span> <span data-ttu-id="669fd-144">Om den här parametern inte finns skapar hello kommando en tom fil med namnet mynewfolder hello roten i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="669fd-144">If this parameter is not present, hello command creates an empty file called mynewfolder at hello root of hello Data Lake Store account.</span></span>
> 
>

## <a name="upload-data-tooa-data-lake-store-account"></a><span data-ttu-id="669fd-145">Ladda upp data tooa Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="669fd-145">Upload data tooa Data Lake Store account</span></span>

<span data-ttu-id="669fd-146">Du kan ladda upp data tooData Lake Store direkt på hello nivå eller tooa rotmapp som du skapade i hello-konto.</span><span class="sxs-lookup"><span data-stu-id="669fd-146">You can upload data tooData Lake Store directly at hello root level or tooa folder that you created within hello account.</span></span> <span data-ttu-id="669fd-147">hello fragmenten nedan visar hur tooupload mappen vissa exempeldata toohello (**mynewfolder**) du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="669fd-147">hello snippets below demonstrate how tooupload some sample data toohello folder (**mynewfolder**) you created in hello previous section.</span></span>

<span data-ttu-id="669fd-148">Om du vill söka efter vissa exempel data tooupload, kan du få hello **Ambulansdata** mapp från hello [Azure Data Lake Git-lagringsplatsen](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span><span class="sxs-lookup"><span data-stu-id="669fd-148">If you are looking for some sample data tooupload, you can get hello **Ambulance Data** folder from hello [Azure Data Lake Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).</span></span> <span data-ttu-id="669fd-149">Hämta hello-filen och lagra den på en lokal katalog på din dator, till exempel C:\sampledata\.</span><span class="sxs-lookup"><span data-stu-id="669fd-149">Download hello file and store it in a local directory on your computer, such as  C:\sampledata\.</span></span>

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> <span data-ttu-id="669fd-150">Du måste ange hello fullständiga sökvägen och filnamnet för hello för hello mål.</span><span class="sxs-lookup"><span data-stu-id="669fd-150">For hello destination, you must specify hello complete path including hello file name.</span></span>
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a><span data-ttu-id="669fd-151">Visa en lista över filer i ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="669fd-151">List files in a Data Lake Store account</span></span>

<span data-ttu-id="669fd-152">Använd hello följande kommando toolist hello filer i ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="669fd-152">Use hello following command toolist hello files in a Data Lake Store account.</span></span>

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
```

<span data-ttu-id="669fd-153">hello resultatet av detta bör vara liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="669fd-153">hello output of this should be similar toohello following:</span></span>

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

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a><span data-ttu-id="669fd-154">Byt namn på, ladda ned och ta bort data från ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="669fd-154">Rename, download, and delete data from a Data Lake Store account</span></span> 

* <span data-ttu-id="669fd-155">**toorename en fil**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="669fd-155">**toorename a file**, use hello following command:</span></span>
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* <span data-ttu-id="669fd-156">**toodownload en fil**, Använd följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="669fd-156">**toodownload a file**, use hello following command.</span></span> <span data-ttu-id="669fd-157">Kontrollera att hello målsökväg som du anger redan finns.</span><span class="sxs-lookup"><span data-stu-id="669fd-157">Make sure hello destination path you specify already exists.</span></span>
  
    ```azurecli     
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > <span data-ttu-id="669fd-158">hello-kommando skapar hello målmappen om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="669fd-158">hello command creates hello destination folder if it does not exist.</span></span>
    > 
    >

* <span data-ttu-id="669fd-159">**toodelete en fil**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="669fd-159">**toodelete a file**, use hello following command:</span></span>
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    <span data-ttu-id="669fd-160">Om du vill toodelete hello mapp **mynewfolder** och hello **vehicle1_09142014_copy.csv** tillsammans i ett kommando, Använd hello--recurse parametern</span><span class="sxs-lookup"><span data-stu-id="669fd-160">If you want toodelete hello folder **mynewfolder** and hello file **vehicle1_09142014_copy.csv** together in one command, use hello --recurse parameter</span></span>

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a><span data-ttu-id="669fd-161">Arbeta med behörigheter och ACL:er för ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="669fd-161">Work with permissions and ACLs for a Data Lake Store account</span></span>

<span data-ttu-id="669fd-162">I det här avsnittet du lär dig mer om hur toomanage ACL: er och behörigheter som använder Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="669fd-162">In this section you learn about how toomanage ACLs and permissions using Azure CLI 2.0.</span></span> <span data-ttu-id="669fd-163">Detaljerad information om hur ACL:er implementeras i Azure Data Lake Store finns i [Åtkomstkontroll i Azure Data Lake Store](data-lake-store-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="669fd-163">For a detailed discussion on how ACLs are implemented in Azure Data Lake Store, see [Access control in Azure Data Lake Store](data-lake-store-access-control.md).</span></span>

* <span data-ttu-id="669fd-164">**tooupdate hello ägaren av en filmapp**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="669fd-164">**tooupdate hello owner of a file/folder**, use hello following command:</span></span>

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* <span data-ttu-id="669fd-165">**tooupdate hello behörigheter för en filmapp**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="669fd-165">**tooupdate hello permissions for a file/folder**, use hello following command:</span></span>

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* <span data-ttu-id="669fd-166">**tooget hello ACL: er för sökvägens**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="669fd-166">**tooget hello ACLs for a given path**, use hello following command:</span></span>

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
    ```

    <span data-ttu-id="669fd-167">hello utdata ska vara liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="669fd-167">hello output should be similar toohello following:</span></span>

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

* <span data-ttu-id="669fd-168">**tooset en post för en ACL**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="669fd-168">**tooset an entry for an ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* <span data-ttu-id="669fd-169">**tooremove en post för en ACL**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="669fd-169">**tooremove an entry for an ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* <span data-ttu-id="669fd-170">**tooremove en hel standard ACL**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="669fd-170">**tooremove an entire default ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* <span data-ttu-id="669fd-171">**tooremove en hel icke-förvalt ACL**, använda hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="669fd-171">**tooremove an entire non-default ACL**, use hello following command:</span></span>

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a><span data-ttu-id="669fd-172">Ta bort ett Data Lake Store-konto</span><span class="sxs-lookup"><span data-stu-id="669fd-172">Delete a Data Lake Store account</span></span>
<span data-ttu-id="669fd-173">Använd hello efter kommandot toodelete ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="669fd-173">Use hello following command toodelete a Data Lake Store account.</span></span>

```azurecli
az dls account delete --account mydatalakestore
```

<span data-ttu-id="669fd-174">När du uppmanas, anger **Y** toodelete hello-konto.</span><span class="sxs-lookup"><span data-stu-id="669fd-174">When prompted, enter **Y** toodelete hello account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="669fd-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="669fd-175">Next steps</span></span>

* [<span data-ttu-id="669fd-176">Azure Data Lake Store CLI 2.0-referens</span><span class="sxs-lookup"><span data-stu-id="669fd-176">Azure Data Lake Store CLI 2.0 reference</span></span>](https://docs.microsoft.com/cli/azure/dls)
* [<span data-ttu-id="669fd-177">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="669fd-177">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="669fd-178">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="669fd-178">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="669fd-179">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="669fd-179">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md
