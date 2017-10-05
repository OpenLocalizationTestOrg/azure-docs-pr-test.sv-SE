---
title: "Så här skapar du en Azure-filresurs | Microsoft Docs"
description: "Så här skapar du en Azure-filresurs i Azure File Storage med hjälp av Azure Portal, PowerShell och Azure CLI."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 10da4d903eaab290a6cca2c4f674548a43a70c53
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a><span data-ttu-id="bf60e-103">Skapa en filresurs i Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="bf60e-103">Create a File Share in Azure File storage</span></span>
<span data-ttu-id="bf60e-104">Du kan skapa Azure-filresurser med [Azure Portal](https://portal.azure.com/), PowerShell-cmdlets för Azure Storage, klientbiblioteken för Azure Storage eller Azure Storage REST-API:et. I den här självstudien lär du dig:</span><span class="sxs-lookup"><span data-stu-id="bf60e-104">You can create Azure File shares using [Azure portal](https://portal.azure.com/), the Azure Storage PowerShell cmdlets, the Azure Storage client libraries, or the Azure Storage REST API.In this tutorial, you will learn:</span></span>
* [<span data-ttu-id="bf60e-105">Hur du skapar en Azure-filresurs med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bf60e-105">How to create an Azure File share using the Azure portal</span></span>](#Create file share through the Portal)
* [<span data-ttu-id="bf60e-106">Hur du skapar en Azure-filresurs med hjälp av Powershell</span><span class="sxs-lookup"><span data-stu-id="bf60e-106">How to create an Azure File share using Powershell</span></span>](#Create file share using PowerShell)
* [<span data-ttu-id="bf60e-107">Hur du skapar en Azure-filresurs med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="bf60e-107">How to create an Azure File share using CLI</span></span>](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a><span data-ttu-id="bf60e-108">Krav</span><span class="sxs-lookup"><span data-stu-id="bf60e-108">Prerequisites</span></span>
<span data-ttu-id="bf60e-109">Om du vill skapa en Azure-filresurs kan du använda ett lagringskonto som redan finns eller [skapa ett nytt Azure Storage-konto](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="bf60e-109">To create an Azure File share, you can use a storage account that already exists, or [create a new Azure storage account](storage-create-storage-account.md).</span></span> <span data-ttu-id="bf60e-110">Om du vill skapa en Azure-filresurs med PowerShell så behöver du kontonyckeln och namnet på ditt lagringskonto...</span><span class="sxs-lookup"><span data-stu-id="bf60e-110">To create an Azure File share with PowerShell, you will need the account key and name of your storage account..</span></span> <span data-ttu-id="bf60e-111">Du behöver din lagringskontonyckel om du avser att använda Powershell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="bf60e-111">You will need Storage account key if you plan to use Powershell or CLI.</span></span>

## <a name="create-file-share-through-the-portal"></a><span data-ttu-id="bf60e-112">Skapa filresurs via portalen</span><span class="sxs-lookup"><span data-stu-id="bf60e-112">Create file share through the Portal</span></span>
1. <span data-ttu-id="bf60e-113">**Gå till bladet Lagringskonto i Azure Portal**:</span><span class="sxs-lookup"><span data-stu-id="bf60e-113">**Go to Storage Account blade on Azure Portal**:</span></span>    
    <span data-ttu-id="bf60e-114">![Bladet Lagringskonto](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)</span><span class="sxs-lookup"><span data-stu-id="bf60e-114">![Storage Account Blade](media/storage-file-how-to-create-file-share/create-file-share-portal1.png)</span></span>

2. <span data-ttu-id="bf60e-115">**Klicka på knappen Lägg till filresurs**:</span><span class="sxs-lookup"><span data-stu-id="bf60e-115">**Click on add File Share button**:</span></span>    
    <span data-ttu-id="bf60e-116">![Klicka på knappen för att lägga till filresurs](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)</span><span class="sxs-lookup"><span data-stu-id="bf60e-116">![Click the add file share button](media/storage-file-how-to-create-file-share/create-file-share-portal2.png)</span></span>

3. <span data-ttu-id="bf60e-117">**Ange namn och kvot. Kvoten kan för närvarande maximalt vara 5TB**:</span><span class="sxs-lookup"><span data-stu-id="bf60e-117">**Provide Name and Quota. Quota currently can be maximum 5TB**:</span></span>    
    <span data-ttu-id="bf60e-118">![Ange ett namn och en önskad kvot för den nya filresursen](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)</span><span class="sxs-lookup"><span data-stu-id="bf60e-118">![Provide a name and a desired quota for the new file share](media/storage-file-how-to-create-file-share/create-file-share-portal3.png)</span></span>

4. <span data-ttu-id="bf60e-119">**Se den nya filresursen**: ![Se den nya filresursen](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)</span><span class="sxs-lookup"><span data-stu-id="bf60e-119">**View your new file share**:  ![View your new file share](media/storage-file-how-to-create-file-share/create-file-share-portal4.png)</span></span>

5. <span data-ttu-id="bf60e-120">**Ladda upp en fil**: ![Ladda upp en fil](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)</span><span class="sxs-lookup"><span data-stu-id="bf60e-120">**Upload a file**:  ![Upload a file](media/storage-file-how-to-create-file-share/create-file-share-portal5.png)</span></span>

6. <span data-ttu-id="bf60e-121">**Bläddra till filresursen och hantera dina kataloger och filer**: ![Bläddra i filresurs](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)</span><span class="sxs-lookup"><span data-stu-id="bf60e-121">**Browse into your file share and manage your directories and files**:  ![Browse file share](media/storage-file-how-to-create-file-share/create-file-share-portal6.png)</span></span>


## <a name="create-file-share-through-powershell"></a><span data-ttu-id="bf60e-122">Skapa filresurs via PowerShell</span><span class="sxs-lookup"><span data-stu-id="bf60e-122">Create file share through PowerShell</span></span>
<span data-ttu-id="bf60e-123">Innan du kan använda PowerShell laddar du ned och installerar Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="bf60e-123">To prepare to use PowerShell, download and install the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="bf60e-124">Information om installationsplatser och installationsanvisningar finns i [Installera och konfigurera Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).</span><span class="sxs-lookup"><span data-stu-id="bf60e-124">See [How to install and configure Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) for the install point and installation instructions.</span></span>

> [!Note]  
> <span data-ttu-id="bf60e-125">Vi rekommenderar att du laddar ned och installerar eller uppgradera till den senaste Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="bf60e-125">It's recommended that you download and install or upgrade to the latest Azure PowerShell module.</span></span>

1. <span data-ttu-id="bf60e-126">**Skapa en kontext för ditt lagringskonto och nyckel** Kontexten innehåller lagringskontots namn och kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="bf60e-126">**Create a context for your storage account and key** The context encapsulates the storage account name and account key.</span></span> <span data-ttu-id="bf60e-127">Anvisningar för hur du kopierar din kontonyckel från [Azure Portal](https://portal.azure.com/) finns i [Visa och kopiera åtkomstnycklar för lagring](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="bf60e-127">For instructions on copying your account key from the [Azure portal](https://portal.azure.com/), see [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span></span>

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. <span data-ttu-id="bf60e-128">**Skapa en ny filresurs**:</span><span class="sxs-lookup"><span data-stu-id="bf60e-128">**Create a new file share**:</span></span>    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> <span data-ttu-id="bf60e-129">Namnet på filresursen får bara innehålla gemener.</span><span class="sxs-lookup"><span data-stu-id="bf60e-129">The name of your file share must be all lowercase.</span></span> <span data-ttu-id="bf60e-130">Mer information om hur du namnger filresurser och filer finns i [Namnge och referera till resurser, kataloger, filer och Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span><span class="sxs-lookup"><span data-stu-id="bf60e-130">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>

## <a name="create-file-share-through-command-line-interface-cli"></a><span data-ttu-id="bf60e-131">Skapa filresurs via kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="bf60e-131">Create file share through Command Line Interface (CLI)</span></span>
1. <span data-ttu-id="bf60e-132">**Om du vill förbereda för att använda ett kommandoradsgränssnitt (CLI) laddar du ned Azure CLI och installerar det.**</span><span class="sxs-lookup"><span data-stu-id="bf60e-132">**To prepare to use a Command Line Interface (CLI), download and install the Azure CLI.**</span></span>  
    <span data-ttu-id="bf60e-133">Se [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) (Installera Azure CLI 2.0) och [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) (Kom igång med Azure CLI 2.0).</span><span class="sxs-lookup"><span data-stu-id="bf60e-133">See [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) and [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span>

2. <span data-ttu-id="bf60e-134">**Skapa en anslutningssträng för lagringskontot där du vill skapa resursen.**</span><span class="sxs-lookup"><span data-stu-id="bf60e-134">**Create a connection string to the storage account where you want to create the share.**</span></span>  
    <span data-ttu-id="bf60e-135">Ersätt ```<storage-account>``` och ```<resource_group>``` med lagringskontots namn och resursgrupp i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="bf60e-135">Replace ```<storage-account>``` and ```<resource_group>``` with your storage account name and resource group in the following example.</span></span>

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

3. <span data-ttu-id="bf60e-136">**Skapa en filresurs**</span><span class="sxs-lookup"><span data-stu-id="bf60e-136">**Create file share**</span></span>
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a><span data-ttu-id="bf60e-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bf60e-137">Next steps</span></span>
* [<span data-ttu-id="bf60e-138">Anslut och montera filresurs – Windows</span><span class="sxs-lookup"><span data-stu-id="bf60e-138">Connect and Mount File Share - Windows</span></span>](storage-file-how-to-use-files-windows.md)
* [<span data-ttu-id="bf60e-139">Anslut och montera filresurs – Linux</span><span class="sxs-lookup"><span data-stu-id="bf60e-139">Connect and Mount File Share - Linux</span></span>](storage-how-to-use-files-linux.md)
* [<span data-ttu-id="bf60e-140">Anslut och montera filresurs – macOS</span><span class="sxs-lookup"><span data-stu-id="bf60e-140">Connect and Mount File Share - macOS</span></span>](storage-file-how-to-use-files-mac.md)

<span data-ttu-id="bf60e-141">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="bf60e-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="bf60e-142">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="bf60e-142">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="bf60e-143">Felsökning</span><span class="sxs-lookup"><span data-stu-id="bf60e-143">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)