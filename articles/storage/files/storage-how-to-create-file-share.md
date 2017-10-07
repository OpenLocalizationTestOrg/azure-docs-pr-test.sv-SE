---
title: aaaHow toocreate en filresurs i Azure | Microsoft Docs
description: "Hur toocreate en Azure-filresurs i Azure File storage med hjälp av hello Azure-portalen, PowerShell och hello Azure CLI."
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
ms.openlocfilehash: 816694e411a993dae881816fc62173e2b7afe990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-file-share-in-azure-file-storage"></a><span data-ttu-id="45102-103">Skapa en filresurs i Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="45102-103">Create a File Share in Azure File storage</span></span>
<span data-ttu-id="45102-104">Du kan skapa Azure-filresurser med hjälp av [Azure-portalen](https://portal.azure.com/)hello Azure Storage PowerShell-cmdlets, hello Azure Storage-klientbibliotek eller hello Azure Storage REST API. I kursen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="45102-104">You can create Azure File shares using [Azure portal](https://portal.azure.com/), hello Azure Storage PowerShell cmdlets, hello Azure Storage client libraries, or hello Azure Storage REST API.In this tutorial, you will learn:</span></span>
* [<span data-ttu-id="45102-105">Hur toocreate en Azure-fil dela med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="45102-105">How toocreate an Azure File share using hello Azure portal</span></span>](#Create file share through hello Portal)
* [<span data-ttu-id="45102-106">Hur toocreate en Azure-filresurs med hjälp av Powershell</span><span class="sxs-lookup"><span data-stu-id="45102-106">How toocreate an Azure File share using Powershell</span></span>](#Create file share using PowerShell)
* [<span data-ttu-id="45102-107">Hur toocreate en Azure-filresurs med hjälp av CLI</span><span class="sxs-lookup"><span data-stu-id="45102-107">How toocreate an Azure File share using CLI</span></span>](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a><span data-ttu-id="45102-108">Krav</span><span class="sxs-lookup"><span data-stu-id="45102-108">Prerequisites</span></span>
<span data-ttu-id="45102-109">toocreate en Azure-filresurs som du kan använda ett lagringskonto som redan finns eller [skapar ett nytt Azure storage-konto](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="45102-109">toocreate an Azure File share, you can use a storage account that already exists, or [create a new Azure storage account](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).</span></span> <span data-ttu-id="45102-110">toocreate en Azure-filresurs med PowerShell, behöver du hello kontonyckel och namnet på ditt lagringskonto...</span><span class="sxs-lookup"><span data-stu-id="45102-110">toocreate an Azure File share with PowerShell, you will need hello account key and name of your storage account..</span></span> <span data-ttu-id="45102-111">Du behöver lagringskontonyckel om du planerar toouse Powershell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="45102-111">You will need Storage account key if you plan toouse Powershell or CLI.</span></span>

## <a name="create-file-share-through-hello-portal"></a><span data-ttu-id="45102-112">Skapa filresurs via hello Portal</span><span class="sxs-lookup"><span data-stu-id="45102-112">Create file share through hello Portal</span></span>
1. <span data-ttu-id="45102-113">**Gå tooStorage kontobladet på Azure Portal**:</span><span class="sxs-lookup"><span data-stu-id="45102-113">**Go tooStorage Account blade on Azure Portal**:</span></span>    
    <span data-ttu-id="45102-114">![Bladet Lagringskonto](./media/storage-how-to-create-file-share/create-file-share-portal1.png)</span><span class="sxs-lookup"><span data-stu-id="45102-114">![Storage Account Blade](./media/storage-how-to-create-file-share/create-file-share-portal1.png)</span></span>

2. <span data-ttu-id="45102-115">**Klicka på knappen Lägg till filresurs**:</span><span class="sxs-lookup"><span data-stu-id="45102-115">**Click on add File Share button**:</span></span>    
    <span data-ttu-id="45102-116">![Klicka på hello filen resursen knappen Lägg till](./media/storage-how-to-create-file-share/create-file-share-portal2.png)</span><span class="sxs-lookup"><span data-stu-id="45102-116">![Click hello add file share button](./media/storage-how-to-create-file-share/create-file-share-portal2.png)</span></span>

3. <span data-ttu-id="45102-117">**Ange namn och kvot. Kvoten kan för närvarande maximalt vara 5TB**:</span><span class="sxs-lookup"><span data-stu-id="45102-117">**Provide Name and Quota. Quota currently can be maximum 5TB**:</span></span>    
    <span data-ttu-id="45102-118">![Ange ett namn och en önskad kvot för hello ny filresurs](./media/storage-how-to-create-file-share/create-file-share-portal3.png)</span><span class="sxs-lookup"><span data-stu-id="45102-118">![Provide a name and a desired quota for hello new file share](./media/storage-how-to-create-file-share/create-file-share-portal3.png)</span></span>

4. <span data-ttu-id="45102-119">**Se den nya filresursen**: ![Se den nya filresursen](./media/storage-how-to-create-file-share/create-file-share-portal4.png)</span><span class="sxs-lookup"><span data-stu-id="45102-119">**View your new file share**:  ![View your new file share](./media/storage-how-to-create-file-share/create-file-share-portal4.png)</span></span>

5. <span data-ttu-id="45102-120">**Ladda upp en fil**: ![Ladda upp en fil](./media/storage-how-to-create-file-share/create-file-share-portal5.png)</span><span class="sxs-lookup"><span data-stu-id="45102-120">**Upload a file**:  ![Upload a file](./media/storage-how-to-create-file-share/create-file-share-portal5.png)</span></span>

6. <span data-ttu-id="45102-121">**Bläddra till filresursen och hantera dina kataloger och filer**: ![Bläddra i filresurs](./media/storage-how-to-create-file-share/create-file-share-portal6.png)</span><span class="sxs-lookup"><span data-stu-id="45102-121">**Browse into your file share and manage your directories and files**:  ![Browse file share](./media/storage-how-to-create-file-share/create-file-share-portal6.png)</span></span>


## <a name="create-file-share-through-powershell"></a><span data-ttu-id="45102-122">Skapa filresurs via PowerShell</span><span class="sxs-lookup"><span data-stu-id="45102-122">Create file share through PowerShell</span></span>
<span data-ttu-id="45102-123">tooprepare toouse PowerShell, hämta och installera hello Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="45102-123">tooprepare toouse PowerShell, download and install hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="45102-124">Se [hur tooinstall och konfigurera Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) för hello installera installationsplatser och installationsanvisningar.</span><span class="sxs-lookup"><span data-stu-id="45102-124">See [How tooinstall and configure Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) for hello install point and installation instructions.</span></span>

> [!Note]  
> <span data-ttu-id="45102-125">Det rekommenderas att du hämtar och installerar eller uppgraderar toohello senaste Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="45102-125">It's recommended that you download and install or upgrade toohello latest Azure PowerShell module.</span></span>

1. <span data-ttu-id="45102-126">**Skapa en kontext för ditt lagringskonto och nyckeln** hello kontexten innehåller hello lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="45102-126">**Create a context for your storage account and key** hello context encapsulates hello storage account name and account key.</span></span> <span data-ttu-id="45102-127">Anvisningar för hur du kopierar din kontonyckel från [Azure Portal](https://portal.azure.com/) finns i [Visa och kopiera åtkomstnycklar för lagring](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="45102-127">For instructions on copying your account key from the [Azure portal](https://portal.azure.com/), see [View and copy storage access keys](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).</span></span>

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. <span data-ttu-id="45102-128">**Skapa en ny filresurs**:</span><span class="sxs-lookup"><span data-stu-id="45102-128">**Create a new file share**:</span></span>    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> <span data-ttu-id="45102-129">hello namnet på filresursen får bara innehålla gemener.</span><span class="sxs-lookup"><span data-stu-id="45102-129">hello name of your file share must be all lowercase.</span></span> <span data-ttu-id="45102-130">Mer information om hur du namnger filresurser och filer finns i [Namnge och referera till resurser, kataloger, filer och Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span><span class="sxs-lookup"><span data-stu-id="45102-130">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>

## <a name="create-file-share-through-command-line-interface-cli"></a><span data-ttu-id="45102-131">Skapa filresurs via kommandoradsgränssnittet (CLI)</span><span class="sxs-lookup"><span data-stu-id="45102-131">Create file share through Command Line Interface (CLI)</span></span>
1. <span data-ttu-id="45102-132">**tooprepare toouse en kommandoradsgränssnittet (CLI), hämta och installera hello Azure CLI.**</span><span class="sxs-lookup"><span data-stu-id="45102-132">**tooprepare toouse a Command Line Interface (CLI), download and install hello Azure CLI.**</span></span>  
    <span data-ttu-id="45102-133">Se [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) (Installera Azure CLI 2.0) och [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) (Kom igång med Azure CLI 2.0).</span><span class="sxs-lookup"><span data-stu-id="45102-133">See [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) and [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span>

2. <span data-ttu-id="45102-134">**Skapa en anslutning sträng toohello storage-konto där du vill att toocreate hello resursen.**</span><span class="sxs-lookup"><span data-stu-id="45102-134">**Create a connection string toohello storage account where you want toocreate hello share.**</span></span>  
    <span data-ttu-id="45102-135">Ersätt ```<storage-account>``` och ```<resource_group>``` med ditt konto namn och resursen lagringsgruppen i hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="45102-135">Replace ```<storage-account>``` and ```<resource_group>``` with your storage account name and resource group in hello following example.</span></span>

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve hello connection string."
    fi
    ```

3. <span data-ttu-id="45102-136">**Skapa en filresurs**</span><span class="sxs-lookup"><span data-stu-id="45102-136">**Create file share**</span></span>
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a><span data-ttu-id="45102-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="45102-137">Next steps</span></span>
* [<span data-ttu-id="45102-138">Anslut och montera filresurs – Windows</span><span class="sxs-lookup"><span data-stu-id="45102-138">Connect and Mount File Share - Windows</span></span>](storage-how-to-use-files-windows.md)
* [<span data-ttu-id="45102-139">Anslut och montera filresurs – Linux</span><span class="sxs-lookup"><span data-stu-id="45102-139">Connect and Mount File Share - Linux</span></span>](../storage-how-to-use-files-linux.md)
* [<span data-ttu-id="45102-140">Anslut och montera filresurs – macOS</span><span class="sxs-lookup"><span data-stu-id="45102-140">Connect and Mount File Share - macOS</span></span>](storage-how-to-use-files-mac.md)

<span data-ttu-id="45102-141">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="45102-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="45102-142">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="45102-142">FAQ</span></span>](../storage-files-faq.md)
* [<span data-ttu-id="45102-143">Felsökning i Windows</span><span class="sxs-lookup"><span data-stu-id="45102-143">Troubleshooting on Windows</span></span>](storage-troubleshoot-windows-file-connection-problems.md)      
* [<span data-ttu-id="45102-144">Felsökning i Linux</span><span class="sxs-lookup"><span data-stu-id="45102-144">Troubleshooting on Linux</span></span>](storage-troubleshoot-linux-file-connection-problems.md)   