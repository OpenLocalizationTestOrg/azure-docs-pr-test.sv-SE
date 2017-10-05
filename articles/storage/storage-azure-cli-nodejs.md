---
title: "Med hjälp av Azure CLI 1.0 med Azure Storage | Microsoft Docs"
description: "Lär dig hur du använder Azure-kommandoradsgränssnittet (Azure CLI) 1.0 med Azure Storage för att skapa och hantera storage-konton och arbeta med Azure-blobbar och filer. Azure CLI är ett verktyg för flera plattformar"
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: b502232a-e8f6-4d6c-befd-3476592e0e35
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.openlocfilehash: b246d8813a41d353a9c0fa31fe838e025fc93046
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-azure-cli-10-with-azure-storage"></a><span data-ttu-id="b797b-104">Med hjälp av Azure CLI 1.0 med Azure Storage</span><span class="sxs-lookup"><span data-stu-id="b797b-104">Using the Azure CLI 1.0 with Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="b797b-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="b797b-105">Overview</span></span>

<span data-ttu-id="b797b-106">Azure CLI tillhandahåller en uppsättning med öppen källkod, plattformsoberoende kommandon för att arbeta med Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="b797b-106">The Azure CLI provides a set of open source, cross-platform commands for working with the Azure Platform.</span></span> <span data-ttu-id="b797b-107">Det ger mycket av samma funktioner som finns i den [Azure-portalen](https://portal.azure.com) samt som omfattande funktioner för dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="b797b-107">It provides much of the same functionality found in the [Azure portal](https://portal.azure.com) as well as rich data access functionality.</span></span>

<span data-ttu-id="b797b-108">I den här guiden kommer vi hur du använder [Azure-kommandoradsgränssnittet (Azure CLI)](../cli-install-nodejs.md) att utföra olika uppgifter för utveckling och administration med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b797b-108">In this guide, we'll explore how to use [Azure Command-Line Interface (Azure CLI)](../cli-install-nodejs.md) to perform a variety of development and administration tasks with Azure Storage.</span></span> <span data-ttu-id="b797b-109">Vi rekommenderar att du hämtar och installerar eller uppgraderar till senaste Azure CLI innan du använder den här guiden.</span><span class="sxs-lookup"><span data-stu-id="b797b-109">We recommend that you download and install or upgrade to the latest Azure CLI before using this guide.</span></span>

<span data-ttu-id="b797b-110">Den här handboken förutsätts att du förstår de grundläggande principerna för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b797b-110">This guide assumes that you understand the basic concepts of Azure Storage.</span></span> <span data-ttu-id="b797b-111">Handboken innehåller ett antal skript för att demonstrera användningen av Azure CLI med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b797b-111">The guide provides a number of scripts to demonstrate the usage of the Azure CLI with Azure Storage.</span></span> <span data-ttu-id="b797b-112">Glöm inte att uppdatera variablerna skript baserat på din konfiguration innan du kör varje skript.</span><span class="sxs-lookup"><span data-stu-id="b797b-112">Be sure to update the script variables based on your configuration before running each script.</span></span>

> [!NOTE]
> <span data-ttu-id="b797b-113">Handboken innehåller Azure CLI kommando- och exempel för klassiska lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="b797b-113">The guide provides the Azure CLI command and script examples for classic storage accounts.</span></span> <span data-ttu-id="b797b-114">Se [med hjälp av Azure CLI för Mac, Linux och Windows med Azure Resource Manager](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) för Azure CLI-kommandona för Resource Manager storage-konton.</span><span class="sxs-lookup"><span data-stu-id="b797b-114">See [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) for Azure CLI commands for Resource Manager storage accounts.</span></span>
>
>

[!INCLUDE [storage-cli-versions](../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a><span data-ttu-id="b797b-115">Komma igång med Azure Storage- och Azure CLI på 5 minuter</span><span class="sxs-lookup"><span data-stu-id="b797b-115">Get started with Azure Storage and the Azure CLI in 5 minutes</span></span>
<span data-ttu-id="b797b-116">Den här guiden använder Ubuntu exempel, men andra operativsystemplattformar ska utföra på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="b797b-116">This guide uses Ubuntu for examples, but other OS platforms should perform similarly.</span></span>

<span data-ttu-id="b797b-117">**Ny till Azure:** hämta Microsoft Azure-prenumeration och ett Microsoft-konto som är kopplade till den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="b797b-117">**New to Azure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="b797b-118">Mer information om köpalternativ för Azure finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/), [köpa alternativ](https://azure.microsoft.com/pricing/purchase-options/), och [Medlemserbjudanden](https://azure.microsoft.com/pricing/member-offers/) (för medlemmar i MSDN, Microsoft Partner Network, BizSpark och att andra Microsoft-program).</span><span class="sxs-lookup"><span data-stu-id="b797b-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="b797b-119">Se [Tilldela administratörsroller i Azure Active Directory (AD Azure)](https://msdn.microsoft.com/library/azure/hh531793.aspx) för mer information om Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="b797b-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="b797b-120">**När du har skapat ett Microsoft Azure-prenumeration och konto:**</span><span class="sxs-lookup"><span data-stu-id="b797b-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="b797b-121">Hämta och installera Azure CLI följa anvisningarna i [installerar Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="b797b-121">Download and install the Azure CLI following the instructions outlined in [Install the Azure CLI](../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="b797b-122">När du har installerat Azure CLI kommer du att kunna använda azure kommando från din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) att komma åt Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="b797b-122">Once the Azure CLI has been installed, you will be able to use the azure command from your command-line interface (Bash, Terminal, Command prompt) to access the Azure CLI commands.</span></span> <span data-ttu-id="b797b-123">Typ av _azure_ kommandot och du bör se följande utdata.</span><span class="sxs-lookup"><span data-stu-id="b797b-123">Type the _azure_ command and you should see the following output.</span></span>

    ![Azure kommandoutdata][Image1]
3. <span data-ttu-id="b797b-125">Skriv i kommandoradsgränssnittet, `azure storage` för att visa en lista över alla kommandon för azure-lagring och få en första intryck av funktionerna Azure CLI tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="b797b-125">In the command-line interface, type `azure storage` to list out all the azure storage commands and get a first impression of the functionalities the Azure CLI provides.</span></span> <span data-ttu-id="b797b-126">Du kan skriva kommandonamn med **-h** parameter (till exempel `azure storage share create -h`) visas detaljerad information om kommandosyntax.</span><span class="sxs-lookup"><span data-stu-id="b797b-126">You can type command name with **-h** parameter (for example, `azure storage share create -h`) to see details of command syntax.</span></span>
4. <span data-ttu-id="b797b-127">Nu får du ett enkelt skript som visar grundläggande Azure CLI-kommandona för att få åtkomst till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="b797b-127">Now, we'll give you a simple script that shows basic Azure CLI commands to access Azure Storage.</span></span> <span data-ttu-id="b797b-128">Skriptet först ber dig att ange två variabler för ditt lagringskonto och en nyckel.</span><span class="sxs-lookup"><span data-stu-id="b797b-128">The script will first ask you to set two variables for your storage account and key.</span></span> <span data-ttu-id="b797b-129">Skriptet kommer sedan skapa en ny behållare i den här nya storage-konto och ladda upp en befintlig bildfil (blob) till behållaren.</span><span class="sxs-lookup"><span data-stu-id="b797b-129">Then, the script will create a new container in this new storage account and upload an existing image file (blob) to that container.</span></span> <span data-ttu-id="b797b-130">När skriptet visar alla blobbar i behållaren, hämtas bildfilen i målkatalogen som finns på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="b797b-130">After the script lists all blobs in that container, it will download the image file to the destination directory which exists on the local computer.</span></span>

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating the container..."
    azure storage container create $container_name

    echo "Uploading the image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing the blobs..."
    azure storage blob list $container_name

    echo "Downloading the image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. <span data-ttu-id="b797b-131">Öppna din önskade textredigerare (vim till exempel) i den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="b797b-131">In your local computer, open your preferred text editor (vim for example).</span></span> <span data-ttu-id="b797b-132">Ange skriptet ovan i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="b797b-132">Type the above script into your text editor.</span></span>
6. <span data-ttu-id="b797b-133">Nu kan behöver du uppdatera skriptvariabler baserat på inställningarna.</span><span class="sxs-lookup"><span data-stu-id="b797b-133">Now, you need to update the script variables based on your configuration settings.</span></span>

   * <span data-ttu-id="b797b-134">**< Storage_account_name >** använder det angivna namnet i skriptet eller ange ett nytt namn för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b797b-134">**<storage_account_name>** Use the given name in the script or enter a new name for your storage account.</span></span> <span data-ttu-id="b797b-135">**Viktigt:** namnet på lagringskontot måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="b797b-135">**Important:** The name of the storage account must be unique in Azure.</span></span> <span data-ttu-id="b797b-136">Det måste vara gemena för!</span><span class="sxs-lookup"><span data-stu-id="b797b-136">It must be lowercase, too!</span></span>
   * <span data-ttu-id="b797b-137">**< Storage_account_key >** åtkomstnyckeln för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b797b-137">**<storage_account_key>** The access key of your storage account.</span></span>
   * <span data-ttu-id="b797b-138">**< Container_name >** använder det angivna namnet i skriptet eller ange ett nytt namn för din behållare.</span><span class="sxs-lookup"><span data-stu-id="b797b-138">**<container_name>** Use the given name in the script or enter a new name for your container.</span></span>
   * <span data-ttu-id="b797b-139">**< Image_to_upload >** som anger en sökväg till en bild på den lokala datorn ”: ~ / images/HelloWorld.png”.</span><span class="sxs-lookup"><span data-stu-id="b797b-139">**<image_to_upload>** Enter a path to a picture on your local computer, such as: "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="b797b-140">**< Destination_folder >** ange en sökväg till en lokal katalog att lagra filer som hämtas från Azure Storage, exempelvis: ”~/downloadImages”.</span><span class="sxs-lookup"><span data-stu-id="b797b-140">**<destination_folder>** Enter a path to a local directory to store files downloaded from Azure Storage, such as: "~/downloadImages".</span></span>
7. <span data-ttu-id="b797b-141">När du har uppdaterat de nödvändiga variablerna i vim trycker du på tangentkombinationer `ESC`, `:`, `wq!` spara skriptet.</span><span class="sxs-lookup"><span data-stu-id="b797b-141">After you've updated the necessary variables in vim, press key combinations `ESC`, `:`, `wq!` to save the script.</span></span>
8. <span data-ttu-id="b797b-142">Om du vill köra det här skriptet, Skriv namnet på skriptfilen i bash-konsolen.</span><span class="sxs-lookup"><span data-stu-id="b797b-142">To run this script, simply type the script file name in the bash console.</span></span> <span data-ttu-id="b797b-143">När skriptet körs, bör du ha en lokal målmapp som innehåller hämtade avbildningsfilen.</span><span class="sxs-lookup"><span data-stu-id="b797b-143">After this script runs, you should have a local destination folder that includes the downloaded image file.</span></span> <span data-ttu-id="b797b-144">Följande skärmbild visar ett exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="b797b-144">The following screenshot shows an example output:</span></span>

<span data-ttu-id="b797b-145">När skriptet körs, bör du ha en lokal målmapp som innehåller hämtade avbildningsfilen.</span><span class="sxs-lookup"><span data-stu-id="b797b-145">After the script runs, you should have a local destination folder that includes the downloaded image file.</span></span>

## <a name="manage-storage-accounts-with-the-azure-cli"></a><span data-ttu-id="b797b-146">Hantera storage-konton med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b797b-146">Manage storage accounts with the Azure CLI</span></span>
### <a name="connect-to-your-azure-subscription"></a><span data-ttu-id="b797b-147">Ansluta till din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b797b-147">Connect to your Azure subscription</span></span>
<span data-ttu-id="b797b-148">Medan de flesta av kommandona lagring fungerar utan en Azure-prenumeration rekommenderar vi att du ansluter till din prenumeration från Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b797b-148">While most of the storage commands will work without an Azure subscription, we recommend you to connect to your subscription from the Azure CLI.</span></span> <span data-ttu-id="b797b-149">För att konfigurera Azure CLI för att arbeta med din prenumeration, följer du stegen i [Anslut till en Azure-prenumeration från Azure CLI](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="b797b-149">To configure the Azure CLI to work with your subscription, follow the steps in [Connect to an Azure subscription from the Azure CLI](../xplat-cli-connect.md).</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="b797b-150">Skapa ett nytt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="b797b-150">Create a new storage account</span></span>
<span data-ttu-id="b797b-151">För att använda Azure storage, behöver du ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b797b-151">To use Azure storage, you will need a storage account.</span></span> <span data-ttu-id="b797b-152">Du kan skapa ett nytt Azure storage-konto när du har konfigurerat datorn för att ansluta till din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b797b-152">You can create a new Azure storage account after you have configured your computer to connect to your subscription.</span></span>

```azurecli
azure storage account create <account_name>
```

<span data-ttu-id="b797b-153">Namnet på ditt lagringskonto måste vara mellan 3 och 24 tecken långt och innehålla siffror och gemena bokstäver.</span><span class="sxs-lookup"><span data-stu-id="b797b-153">The name of your storage account must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a><span data-ttu-id="b797b-154">Ange ett standardkonto för Azure storage i miljövariabler</span><span class="sxs-lookup"><span data-stu-id="b797b-154">Set a default Azure storage account in environment variables</span></span>
<span data-ttu-id="b797b-155">Du kan ha flera lagringskonton i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b797b-155">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="b797b-156">Du kan välja en av dem och ange den i miljövariabler för kommandona för lagring i samma session.</span><span class="sxs-lookup"><span data-stu-id="b797b-156">You can choose one of them and set it in the environment variables for all the storage commands in the same session.</span></span> <span data-ttu-id="b797b-157">På så sätt kan du köra Azure CLI-kommandona lagring utan att ange lagringskontot och nyckeln explicit.</span><span class="sxs-lookup"><span data-stu-id="b797b-157">This enables you to run the Azure CLI storage commands without specifying the storage account and key explicitly.</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="b797b-158">Ett annat sätt att ange standardkontot för lagring med hjälp av anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="b797b-158">Another way to set a default storage account is using connection string.</span></span> <span data-ttu-id="b797b-159">Hämta det första anslutningssträngen med kommandot:</span><span class="sxs-lookup"><span data-stu-id="b797b-159">Firstly get the connection string by command:</span></span>

```azurecli
azure storage account connectionstring show <account_name>
```

<span data-ttu-id="b797b-160">Kopiera anslutningssträngen utdata och ange miljövariabeln:</span><span class="sxs-lookup"><span data-stu-id="b797b-160">Then copy the output connection string and set it to environment variable:</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a><span data-ttu-id="b797b-161">Skapa och hantera blobbar</span><span class="sxs-lookup"><span data-stu-id="b797b-161">Create and manage blobs</span></span>
<span data-ttu-id="b797b-162">Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i världen via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b797b-162">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in the world via HTTP or HTTPS.</span></span> <span data-ttu-id="b797b-163">Det här avsnittet förutsätter att du redan är bekant med Azure Blob storage-koncept.</span><span class="sxs-lookup"><span data-stu-id="b797b-163">This section assumes that you are already familiar with the Azure Blob storage concepts.</span></span> <span data-ttu-id="b797b-164">Detaljerad information finns i [komma igång med Azure Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md) och [Blob-Tjänstkoncept](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="b797b-164">For detailed information, see [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="b797b-165">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="b797b-165">Create a container</span></span>
<span data-ttu-id="b797b-166">Varje blobb i Azure-lagring måste vara i en behållare.</span><span class="sxs-lookup"><span data-stu-id="b797b-166">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="b797b-167">Du kan skapa en privat behållaren med hjälp av den `azure storage container create` kommando:</span><span class="sxs-lookup"><span data-stu-id="b797b-167">You can create a private container using the `azure storage container create` command:</span></span>

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> <span data-ttu-id="b797b-168">Det finns tre nivåer av anonym läsbehörighet: **av**, **Blob**, och **behållare**.</span><span class="sxs-lookup"><span data-stu-id="b797b-168">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="b797b-169">Om du vill förhindra anonym åtkomst till blobbar, kan du ange parametern behörighet **av**.</span><span class="sxs-lookup"><span data-stu-id="b797b-169">To prevent anonymous access to blobs, set the Permission parameter to **Off**.</span></span> <span data-ttu-id="b797b-170">Som standard den nya behållaren är privat och kan endast användas av ägare.</span><span class="sxs-lookup"><span data-stu-id="b797b-170">By default, the new container is private and can be accessed only by the account owner.</span></span> <span data-ttu-id="b797b-171">Du vill tillåta anonym offentlig läsbehörighet till blob-resurser, men inte till metadata för behållaren eller i listan över blobbar i behållaren anger parametern behörighet **Blob**.</span><span class="sxs-lookup"><span data-stu-id="b797b-171">To allow anonymous public read access to blob resources, but not to container metadata or to the list of blobs in the container, set the Permission parameter to **Blob**.</span></span> <span data-ttu-id="b797b-172">Om du vill ge fullständig offentlig läsbehörighet till blob resurser, metadata för behållaren och listan över blobbar i behållaren, ange parametern behörighet **behållare**.</span><span class="sxs-lookup"><span data-stu-id="b797b-172">To allow full public read access to blob resources, container metadata, and the list of blobs in the container, set the Permission parameter to **Container**.</span></span> <span data-ttu-id="b797b-173">Mer information finns i [Hantera anonym läsbehörighet till behållare och blobbar](storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="b797b-173">For more information, see [Manage anonymous read access to containers and blobs](storage-manage-access-to-resources.md).</span></span>
>
>

### <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="b797b-174">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="b797b-174">Upload a blob into a container</span></span>
<span data-ttu-id="b797b-175">Azure Blob Storage stöder blockblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="b797b-175">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="b797b-176">Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="b797b-176">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="b797b-177">Om du vill överföra blobbar i en behållare, kan du använda den `azure storage blob upload`.</span><span class="sxs-lookup"><span data-stu-id="b797b-177">To upload blobs in to a container, you can use the `azure storage blob upload`.</span></span> <span data-ttu-id="b797b-178">Som standard överför det här kommandot lokala filer till en blockblobb.</span><span class="sxs-lookup"><span data-stu-id="b797b-178">By default, this command uploads the local files to a block blob.</span></span> <span data-ttu-id="b797b-179">Om du vill ange en typ av blob, du kan använda den `--blobtype` parameter.</span><span class="sxs-lookup"><span data-stu-id="b797b-179">To specify the type for the blob, you can use the `--blobtype` parameter.</span></span>

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a><span data-ttu-id="b797b-180">Ladda ned blobbar från en behållare</span><span class="sxs-lookup"><span data-stu-id="b797b-180">Download blobs from a container</span></span>
<span data-ttu-id="b797b-181">I följande exempel visar hur du ladda ned blobbar från en behållare.</span><span class="sxs-lookup"><span data-stu-id="b797b-181">The following example demonstrates how to download blobs from a container.</span></span>

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a><span data-ttu-id="b797b-182">Kopiera BLOB</span><span class="sxs-lookup"><span data-stu-id="b797b-182">Copy blobs</span></span>
<span data-ttu-id="b797b-183">Du kan kopiera blobar i eller mellan lagringskonton och regioner asynkront.</span><span class="sxs-lookup"><span data-stu-id="b797b-183">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="b797b-184">Följande exempel visar hur du kopierar blobar från ett lagringskonto till ett annat.</span><span class="sxs-lookup"><span data-stu-id="b797b-184">The following example demonstrates how to copy blobs from one storage account to another.</span></span> <span data-ttu-id="b797b-185">I det här exemplet skapar vi en behållare där blobbar är offentligt, anonymt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="b797b-185">In this sample we create a container where blobs are publicly, anonymously accessible.</span></span>

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

<span data-ttu-id="b797b-186">Det här exemplet utför en asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="b797b-186">This sample performs an asynchronous copy.</span></span> <span data-ttu-id="b797b-187">Du kan övervaka statusen för varje kopieringen genom att köra den `azure storage blob copy show` igen.</span><span class="sxs-lookup"><span data-stu-id="b797b-187">You can monitor the status of each copy operation by running the `azure storage blob copy show` operation.</span></span>

<span data-ttu-id="b797b-188">Observera att Käll-URL som angetts för kopieringen måste antingen vara allmänt tillgänglig eller inkluderar en SAS (signatur för delad åtkomst)-token.</span><span class="sxs-lookup"><span data-stu-id="b797b-188">Note that the source URL provided for the copy operation must either be publicly accessible, or include a SAS (shared access signature) token.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="b797b-189">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="b797b-189">Delete a blob</span></span>
<span data-ttu-id="b797b-190">Om du vill ta bort en blobb, använder den under kommando:</span><span class="sxs-lookup"><span data-stu-id="b797b-190">To delete a blob, use the below command:</span></span>

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="b797b-191">Skapa och hantera filresurser</span><span class="sxs-lookup"><span data-stu-id="b797b-191">Create and manage file shares</span></span>
<span data-ttu-id="b797b-192">Azure File storage erbjuder delad lagring för program som använder SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="b797b-192">Azure File storage offers shared storage for applications using the standard SMB protocol.</span></span> <span data-ttu-id="b797b-193">Microsoft Azure-datorer och molntjänster, samt lokala program kan dela fildata via monterade resurser.</span><span class="sxs-lookup"><span data-stu-id="b797b-193">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="b797b-194">Du kan hantera filresurser och fildata via Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="b797b-194">You can manage file shares and file data via the Azure CLI.</span></span> <span data-ttu-id="b797b-195">Mer information om Azure File storage finns [Kom igång med Azure File storage i Windows](storage-dotnet-how-to-use-files.md) eller [använda Azure File storage med Linux](storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="b797b-195">For more information on Azure File storage, see [Get started with Azure File storage on Windows](storage-dotnet-how-to-use-files.md) or [How to use Azure File storage with Linux](storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="b797b-196">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="b797b-196">Create a file share</span></span>
<span data-ttu-id="b797b-197">En filresurs i Azure är en SMB-filresurs i Azure.</span><span class="sxs-lookup"><span data-stu-id="b797b-197">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="b797b-198">Alla kataloger och filer måste skapas i en filresurs.</span><span class="sxs-lookup"><span data-stu-id="b797b-198">All directories and files must be created in a file share.</span></span> <span data-ttu-id="b797b-199">Ett konto kan innehålla ett obegränsat antal resurser och en resurs kan lagra ett obegränsat antal filer, upp till lagringskontot kapacitetsgränser.</span><span class="sxs-lookup"><span data-stu-id="b797b-199">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up to the capacity limits of the storage account.</span></span> <span data-ttu-id="b797b-200">Följande exempel skapar en filresurs med namnet **minresurs**.</span><span class="sxs-lookup"><span data-stu-id="b797b-200">The following example creates a file share named **myshare**.</span></span>

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="b797b-201">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="b797b-201">Create a directory</span></span>
<span data-ttu-id="b797b-202">En katalog innehåller ett valfritt hierarkisk struktur för ett Azure-filresursen.</span><span class="sxs-lookup"><span data-stu-id="b797b-202">A directory provides an optional hierarchical structure for an Azure file share.</span></span> <span data-ttu-id="b797b-203">I följande exempel skapas en katalog med namnet **MinMapp** i filresursen.</span><span class="sxs-lookup"><span data-stu-id="b797b-203">The following example creates a directory named **myDir** in the file share.</span></span>

```azurecli
azure storage directory create myshare myDir
```

<span data-ttu-id="b797b-204">Observera att sökvägen kan innehålla flera nivåer *t.ex.*, **en / b**.</span><span class="sxs-lookup"><span data-stu-id="b797b-204">Note that directory path can include multiple levels, *e.g.*, **a/b**.</span></span> <span data-ttu-id="b797b-205">Dock måste du kontrollera att alla överordnade kataloger finns.</span><span class="sxs-lookup"><span data-stu-id="b797b-205">However, you must ensure that all parent directories exist.</span></span> <span data-ttu-id="b797b-206">Till exempel för sökvägen **en / b**, måste du skapa katalog **en** först, sedan att skapa katalog **b**.</span><span class="sxs-lookup"><span data-stu-id="b797b-206">For example, for path **a/b**, you must create directory **a** first, then create directory **b**.</span></span>

### <a name="upload-a-local-file-to-directory"></a><span data-ttu-id="b797b-207">Ladda upp en lokal fil till katalogen</span><span class="sxs-lookup"><span data-stu-id="b797b-207">Upload a local file to directory</span></span>
<span data-ttu-id="b797b-208">I följande exempel överför en fil från **~/temp/samplefile.txt** till den **MinMapp** directory.</span><span class="sxs-lookup"><span data-stu-id="b797b-208">The following example uploads a file from **~/temp/samplefile.txt** to the **myDir** directory.</span></span> <span data-ttu-id="b797b-209">Ändra sökvägen till filen så att den pekar på en giltig fil på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="b797b-209">Edit the file path so that it points to a valid file on your local machine:</span></span>

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

<span data-ttu-id="b797b-210">Observera att en fil i resursen kan vara upp till 1 TB i storlek.</span><span class="sxs-lookup"><span data-stu-id="b797b-210">Note that a file in the share can be up to 1 TB in size.</span></span>

### <a name="list-the-files-in-the-share-root-or-directory"></a><span data-ttu-id="b797b-211">Visa en lista med filerna i resursen rot- eller directory</span><span class="sxs-lookup"><span data-stu-id="b797b-211">List the files in the share root or directory</span></span>
<span data-ttu-id="b797b-212">Du kan visa en lista över filer och underkataloger i en rot-resurs eller en katalog med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="b797b-212">You can list the files and subdirectories in a share root or a directory using the following command:</span></span>

```azurecli
azure storage file list myshare myDir
```

<span data-ttu-id="b797b-213">Observera att katalognamnet är obligatoriskt för åtgärden lista.</span><span class="sxs-lookup"><span data-stu-id="b797b-213">Note that the directory name is optional for the listing operation.</span></span> <span data-ttu-id="b797b-214">Om det utelämnas används listar kommandot innehållet i rotkatalogen för resursen.</span><span class="sxs-lookup"><span data-stu-id="b797b-214">If omitted, the command lists the contents of the root directory of the share.</span></span>

### <a name="copy-files"></a><span data-ttu-id="b797b-215">Kopiera filer</span><span class="sxs-lookup"><span data-stu-id="b797b-215">Copy files</span></span>
<span data-ttu-id="b797b-216">Från och med version 0.9.8 av Azure CLI kan kopiera du en fil till en annan fil, en fil till en blobb eller en blobb till en fil.</span><span class="sxs-lookup"><span data-stu-id="b797b-216">Beginning with version 0.9.8 of Azure CLI, you can copy a file to another file, a file to a blob, or a blob to a file.</span></span> <span data-ttu-id="b797b-217">Nedan ser du hur du utför dessa kopieringsåtgärder med hjälp av CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="b797b-217">Below we demonstrate how to perform these copy operations using CLI commands.</span></span> <span data-ttu-id="b797b-218">Du kopierar en fil till den nya katalogen:</span><span class="sxs-lookup"><span data-stu-id="b797b-218">To copy a file to the new directory:</span></span>

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

<span data-ttu-id="b797b-219">Att kopiera en blobb till en katalog:</span><span class="sxs-lookup"><span data-stu-id="b797b-219">To copy a blob to a file directory:</span></span>

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a><span data-ttu-id="b797b-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b797b-220">Next Steps</span></span>

<span data-ttu-id="b797b-221">Du kan hitta Azure CLI 1.0 kommandoreferens för att arbeta med lagringsresurser här:</span><span class="sxs-lookup"><span data-stu-id="b797b-221">You can find Azure CLI 1.0 command reference for working with Storage resources here:</span></span>

* [<span data-ttu-id="b797b-222">Azure CLI-kommandona i Resource Manager-läge</span><span class="sxs-lookup"><span data-stu-id="b797b-222">Azure CLI commands in Resource Manager mode</span></span>](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [<span data-ttu-id="b797b-223">Azure CLI-kommandona i Azure Service Management-läge</span><span class="sxs-lookup"><span data-stu-id="b797b-223">Azure CLI commands in Azure Service Management mode</span></span>](../cli-install-nodejs.md)

<span data-ttu-id="b797b-224">Du kan också vilja försök den [Azure CLI 2.0](storage-azure-cli.md), våra nästa generations CLI som skrivits i Python, för användning med Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="b797b-224">You may also like to try the [Azure CLI 2.0](storage-azure-cli.md), our next-generation CLI written in Python, for use with the Resource Manager deployment model.</span></span>

[Image1]: ./media/storage-azure-cli/azure_command.png
