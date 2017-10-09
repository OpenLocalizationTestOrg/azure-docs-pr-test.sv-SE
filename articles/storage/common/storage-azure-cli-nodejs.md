---
title: aaaUsing hello Azure CLI 1.0 med Azure Storage | Microsoft Docs
description: "Lär dig hur toouse hello Azure-kommandoradsgränssnittet (Azure CLI) 1.0 med Azure Storage toocreate och hantera storage-konton och arbeta med Azure-blobbar och filer. hello Azure CLI är ett verktyg för flera plattformar"
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
ms.openlocfilehash: 25e459403dde631741403c8722ed07beafac35c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-10-with-azure-storage"></a><span data-ttu-id="5f1ea-104">Med Azure Storage hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="5f1ea-104">Using hello Azure CLI 1.0 with Azure Storage</span></span>

## <a name="overview"></a><span data-ttu-id="5f1ea-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="5f1ea-105">Overview</span></span>

<span data-ttu-id="5f1ea-106">hello Azure CLI innehåller en uppsättning med öppen källkod, plattformsoberoende kommandon för att arbeta med hello Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-106">hello Azure CLI provides a set of open source, cross-platform commands for working with hello Azure Platform.</span></span> <span data-ttu-id="5f1ea-107">Det ger mycket hello samma funktioner som finns i hello [Azure-portalen](https://portal.azure.com) samt som omfattande funktioner för dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-107">It provides much of hello same functionality found in hello [Azure portal](https://portal.azure.com) as well as rich data access functionality.</span></span>

<span data-ttu-id="5f1ea-108">I den här handboken vi ska visa hur toouse [Azure-kommandoradsgränssnittet (Azure CLI)](../../cli-install-nodejs.md) tooperform en rad olika uppgifter för utveckling och administration med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-108">In this guide, we'll explore how toouse [Azure Command-Line Interface (Azure CLI)](../../cli-install-nodejs.md) tooperform a variety of development and administration tasks with Azure Storage.</span></span> <span data-ttu-id="5f1ea-109">Vi rekommenderar att du hämtar och installerar eller uppgraderar toohello senaste Azure CLI innan du använder den här guiden.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-109">We recommend that you download and install or upgrade toohello latest Azure CLI before using this guide.</span></span>

<span data-ttu-id="5f1ea-110">Den här handboken förutsätts att du förstår hello grundläggande begrepp för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-110">This guide assumes that you understand hello basic concepts of Azure Storage.</span></span> <span data-ttu-id="5f1ea-111">hello guiden ger ett antal skript toodemonstrate hello användning av hello Azure CLI med Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-111">hello guide provides a number of scripts toodemonstrate hello usage of hello Azure CLI with Azure Storage.</span></span> <span data-ttu-id="5f1ea-112">Vara säker på att tooupdate hello skriptvariabler baserat på din konfiguration innan du kör varje skript.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-112">Be sure tooupdate hello script variables based on your configuration before running each script.</span></span>

> [!NOTE]
> <span data-ttu-id="5f1ea-113">hello guiden visar hello Azure CLI kommando- och exempel för klassiska lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-113">hello guide provides hello Azure CLI command and script examples for classic storage accounts.</span></span> <span data-ttu-id="5f1ea-114">Se [Using hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) för Azure CLI-kommandona för Resource Manager storage-konton.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-114">See [Using hello Azure CLI for Mac, Linux, and Windows with Azure Resource Management](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) for Azure CLI commands for Resource Manager storage accounts.</span></span>
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-hello-azure-cli-in-5-minutes"></a><span data-ttu-id="5f1ea-115">Kom igång med Azure Storage och hello Azure CLI i 5 minuter</span><span class="sxs-lookup"><span data-stu-id="5f1ea-115">Get started with Azure Storage and hello Azure CLI in 5 minutes</span></span>
<span data-ttu-id="5f1ea-116">Den här guiden använder Ubuntu exempel, men andra operativsystemplattformar ska utföra på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-116">This guide uses Ubuntu for examples, but other OS platforms should perform similarly.</span></span>

<span data-ttu-id="5f1ea-117">**Nya tooAzure:** hämta Microsoft Azure-prenumeration och ett Microsoft-konto som är kopplade till den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-117">**New tooAzure:** Get a Microsoft Azure subscription and a Microsoft account associated with that subscription.</span></span> <span data-ttu-id="5f1ea-118">Mer information om köpalternativ för Azure finns [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/), [köpa alternativ](https://azure.microsoft.com/pricing/purchase-options/), och [Medlemserbjudanden](https://azure.microsoft.com/pricing/member-offers/) (för medlemmar i MSDN, Microsoft Partner Network, BizSpark och att andra Microsoft-program).</span><span class="sxs-lookup"><span data-stu-id="5f1ea-118">For information on Azure purchase options, see [Free Trial](https://azure.microsoft.com/pricing/free-trial/), [Purchase Options](https://azure.microsoft.com/pricing/purchase-options/), and [Member Offers](https://azure.microsoft.com/pricing/member-offers/) (for members of MSDN, Microsoft Partner Network, and BizSpark, and other Microsoft programs).</span></span>

<span data-ttu-id="5f1ea-119">Se [Tilldela administratörsroller i Azure Active Directory (AD Azure)](https://msdn.microsoft.com/library/azure/hh531793.aspx) för mer information om Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-119">See [Assigning administrator roles in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) for more information about Azure subscriptions.</span></span>

<span data-ttu-id="5f1ea-120">**När du har skapat ett Microsoft Azure-prenumeration och konto:**</span><span class="sxs-lookup"><span data-stu-id="5f1ea-120">**After creating a Microsoft Azure subscription and account:**</span></span>

1. <span data-ttu-id="5f1ea-121">Hämta och installera hello Azure CLI hello instruktionerna som beskrivs i [installera hello Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5f1ea-121">Download and install hello Azure CLI following hello instructions outlined in [Install hello Azure CLI](../../cli-install-nodejs.md).</span></span>
2. <span data-ttu-id="5f1ea-122">När hello Azure CLI har installerats, kommer du att kunna toouse hello azure kommando från din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) tooaccess hello Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-122">Once hello Azure CLI has been installed, you will be able toouse hello azure command from your command-line interface (Bash, Terminal, Command prompt) tooaccess hello Azure CLI commands.</span></span> <span data-ttu-id="5f1ea-123">Typen hello _azure_ kommandot och du bör se hello följande utdata.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-123">Type hello _azure_ command and you should see hello following output.</span></span>

    ![Azure kommandoutdata](./media/storage-azure-cli/azure_command.png)   
3. <span data-ttu-id="5f1ea-125">Skriv i hello kommandoradsgränssnittet `azure storage` toolist ut alla hello azure storage-kommandon och få en första intryck av hello funktioner hello Azure CLI tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-125">In hello command-line interface, type `azure storage` toolist out all hello azure storage commands and get a first impression of hello functionalities hello Azure CLI provides.</span></span> <span data-ttu-id="5f1ea-126">Du kan skriva kommandonamn med **-h** parameter (till exempel `azure storage share create -h`) toosee information om kommandosyntax.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-126">You can type command name with **-h** parameter (for example, `azure storage share create -h`) toosee details of command syntax.</span></span>
4. <span data-ttu-id="5f1ea-127">Nu får du ett enkelt skript som visar grundläggande Azure CLI-kommandon tooaccess Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-127">Now, we'll give you a simple script that shows basic Azure CLI commands tooaccess Azure Storage.</span></span> <span data-ttu-id="5f1ea-128">hello skriptet först ber dig tooset två variabler för ditt lagringskonto och en nyckel.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-128">hello script will first ask you tooset two variables for your storage account and key.</span></span> <span data-ttu-id="5f1ea-129">Hello skript ska skapa en ny behållare i den här nya storage-konto och sedan ladda upp en befintlig avbildning fil (blob) toothat behållare.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-129">Then, hello script will create a new container in this new storage account and upload an existing image file (blob) toothat container.</span></span> <span data-ttu-id="5f1ea-130">När hello-skriptet visar alla blobbar i behållaren, hämtas hello image-filen toohello målkatalogen som finns på hello lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-130">After hello script lists all blobs in that container, it will download hello image file toohello destination directory which exists on hello local computer.</span></span>

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating hello container..."
    azure storage container create $container_name

    echo "Uploading hello image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing hello blobs..."
    azure storage blob list $container_name

    echo "Downloading hello image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. <span data-ttu-id="5f1ea-131">Öppna din önskade textredigerare (vim till exempel) i den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-131">In your local computer, open your preferred text editor (vim for example).</span></span> <span data-ttu-id="5f1ea-132">Ange hello ovan skriptet i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-132">Type hello above script into your text editor.</span></span>
6. <span data-ttu-id="5f1ea-133">Du måste nu tooupdate hello skriptvariabler baserat på inställningarna.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-133">Now, you need tooupdate hello script variables based on your configuration settings.</span></span>

   * <span data-ttu-id="5f1ea-134">**< Storage_account_name >** använda hello får namn i skriptet hello eller ange ett nytt namn för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-134">**<storage_account_name>** Use hello given name in hello script or enter a new name for your storage account.</span></span> <span data-ttu-id="5f1ea-135">**Viktigt:** hello namnet på hello storage-konto måste vara unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-135">**Important:** hello name of hello storage account must be unique in Azure.</span></span> <span data-ttu-id="5f1ea-136">Det måste vara gemena för!</span><span class="sxs-lookup"><span data-stu-id="5f1ea-136">It must be lowercase, too!</span></span>
   * <span data-ttu-id="5f1ea-137">**< storage_account_key >** hello åtkomstnyckeln för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-137">**<storage_account_key>** hello access key of your storage account.</span></span>
   * <span data-ttu-id="5f1ea-138">**< Container_name >** använda hello får namn i skriptet hello eller ange ett nytt namn för din behållare.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-138">**<container_name>** Use hello given name in hello script or enter a new name for your container.</span></span>
   * <span data-ttu-id="5f1ea-139">**< Image_to_upload >** ange en sökväg tooa bild på den lokala datorn, till exempel ”: ~ / images/HelloWorld.png”.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-139">**<image_to_upload>** Enter a path tooa picture on your local computer, such as: "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="5f1ea-140">**< Destination_folder >** ange sökvägen tooa lokal katalog toostore filer som hämtas från Azure Storage: ”~/downloadImages”.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-140">**<destination_folder>** Enter a path tooa local directory toostore files downloaded from Azure Storage, such as: "~/downloadImages".</span></span>
7. <span data-ttu-id="5f1ea-141">När du har uppdaterat hello nödvändiga variabler i vim trycker du på tangentkombinationer `ESC`, `:`, `wq!` toosave hello skript.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-141">After you've updated hello necessary variables in vim, press key combinations `ESC`, `:`, `wq!` toosave hello script.</span></span>
8. <span data-ttu-id="5f1ea-142">toorun det här skriptet kan bara typen hello filnamn i hello bash-konsolen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-142">toorun this script, simply type hello script file name in hello bash console.</span></span> <span data-ttu-id="5f1ea-143">När skriptet körs, bör du ha en lokal målmapp som innehåller hello hämtade image-filen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-143">After this script runs, you should have a local destination folder that includes hello downloaded image file.</span></span> <span data-ttu-id="5f1ea-144">hello följande skärmbild visar ett exempel på utdata:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-144">hello following screenshot shows an example output:</span></span>

<span data-ttu-id="5f1ea-145">När hello skriptet körs, bör du ha en lokal målmapp som innehåller hello hämtade image-filen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-145">After hello script runs, you should have a local destination folder that includes hello downloaded image file.</span></span>

## <a name="manage-storage-accounts-with-hello-azure-cli"></a><span data-ttu-id="5f1ea-146">Hantera storage-konton med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5f1ea-146">Manage storage accounts with hello Azure CLI</span></span>
### <a name="connect-tooyour-azure-subscription"></a><span data-ttu-id="5f1ea-147">Ansluta tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5f1ea-147">Connect tooyour Azure subscription</span></span>
<span data-ttu-id="5f1ea-148">Medan de flesta kommandon för lagring av hello fungerar utan en Azure-prenumeration rekommenderar vi du tooconnect tooyour prenumeration från hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-148">While most of hello storage commands will work without an Azure subscription, we recommend you tooconnect tooyour subscription from hello Azure CLI.</span></span> <span data-ttu-id="5f1ea-149">tooconfigure hello Azure CLI toowork med din prenumeration gör hello i [ansluta tooan Azure-prenumeration från hello Azure CLI](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="5f1ea-149">tooconfigure hello Azure CLI toowork with your subscription, follow hello steps in [Connect tooan Azure subscription from hello Azure CLI](../../xplat-cli-connect.md).</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="5f1ea-150">Skapa ett nytt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="5f1ea-150">Create a new storage account</span></span>
<span data-ttu-id="5f1ea-151">toouse Azure storage, behöver ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-151">toouse Azure storage, you will need a storage account.</span></span> <span data-ttu-id="5f1ea-152">Du kan skapa ett nytt Azure storage-konto när du har konfigurerat din dator tooconnect tooyour prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-152">You can create a new Azure storage account after you have configured your computer tooconnect tooyour subscription.</span></span>

```azurecli
azure storage account create <account_name>
```

<span data-ttu-id="5f1ea-153">hello namnet på ditt lagringskonto måste vara mellan 3 och 24 tecken långt och innehålla siffror och gemena bokstäver.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-153">hello name of your storage account must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a><span data-ttu-id="5f1ea-154">Ange ett standardkonto för Azure storage i miljövariabler</span><span class="sxs-lookup"><span data-stu-id="5f1ea-154">Set a default Azure storage account in environment variables</span></span>
<span data-ttu-id="5f1ea-155">Du kan ha flera lagringskonton i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-155">You can have multiple storage accounts in your subscription.</span></span> <span data-ttu-id="5f1ea-156">Du kan välja en av dem och ange den i hello miljövariabler för lagring av alla hello-kommandon i hello samma session.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-156">You can choose one of them and set it in hello environment variables for all hello storage commands in hello same session.</span></span> <span data-ttu-id="5f1ea-157">Detta gör att du toorun hello Azure CLI lagring kommandon utan att ange hello storage-konto och nyckeln explicit.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-157">This enables you toorun hello Azure CLI storage commands without specifying hello storage account and key explicitly.</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="5f1ea-158">Ett annat sätt tooset standardkontot för lagring med hjälp av anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-158">Another way tooset a default storage account is using connection string.</span></span> <span data-ttu-id="5f1ea-159">Hämta det första hello anslutningssträngen med kommandot:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-159">Firstly get hello connection string by command:</span></span>

```azurecli
azure storage account connectionstring show <account_name>
```

<span data-ttu-id="5f1ea-160">Kopiera anslutningssträngen för hello utdata och ange den tooenvironment variabeln:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-160">Then copy hello output connection string and set it tooenvironment variable:</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a><span data-ttu-id="5f1ea-161">Skapa och hantera blobbar</span><span class="sxs-lookup"><span data-stu-id="5f1ea-161">Create and manage blobs</span></span>
<span data-ttu-id="5f1ea-162">Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-162">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="5f1ea-163">Det här avsnittet förutsätter att du redan är bekant med hello Azure Blob storage-koncept.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-163">This section assumes that you are already familiar with hello Azure Blob storage concepts.</span></span> <span data-ttu-id="5f1ea-164">Detaljerad information finns i [komma igång med Azure Blob storage med hjälp av .NET](../blobs/storage-dotnet-how-to-use-blobs.md) och [Blob-Tjänstkoncept](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f1ea-164">For detailed information, see [Get started with Azure Blob storage using .NET](../blobs/storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](http://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="5f1ea-165">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="5f1ea-165">Create a container</span></span>
<span data-ttu-id="5f1ea-166">Varje blobb i Azure-lagring måste vara i en behållare.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-166">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="5f1ea-167">Du kan skapa en privat behållare med hello `azure storage container create` kommando:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-167">You can create a private container using hello `azure storage container create` command:</span></span>

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> <span data-ttu-id="5f1ea-168">Det finns tre nivåer av anonym läsbehörighet: **av**, **Blob**, och **behållare**.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-168">There are three levels of anonymous read access: **Off**, **Blob**, and **Container**.</span></span> <span data-ttu-id="5f1ea-169">tooprevent anonym åtkomst till tooblobs parameter för hello behörighet för**av**.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-169">tooprevent anonymous access tooblobs, set hello Permission parameter too**Off**.</span></span> <span data-ttu-id="5f1ea-170">Som standard hello ny behållare är privat och kan endast användas av hello Kontoägare.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-170">By default, hello new container is private and can be accessed only by hello account owner.</span></span> <span data-ttu-id="5f1ea-171">tooallow anonym offentliga läsa åtkomst tooblob resurser, men inte toocontainer metadata eller toohello lista över blobbar i hello behållare genom att ange hello behörighet parametern för**Blob**.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-171">tooallow anonymous public read access tooblob resources, but not toocontainer metadata or toohello list of blobs in hello container, set hello Permission parameter too**Blob**.</span></span> <span data-ttu-id="5f1ea-172">tooallow fullständig offentliga läsa komma åt resurser i tooblob, metadata för behållaren och hello lista över blobbar i hello behållare genom att ange hello behörighet parametern för**behållare**.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-172">tooallow full public read access tooblob resources, container metadata, and hello list of blobs in hello container, set hello Permission parameter too**Container**.</span></span> <span data-ttu-id="5f1ea-173">Mer information finns i [Hantera anonym läsbehörighet toocontainers och blobbar](../blobs/storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="5f1ea-173">For more information, see [Manage anonymous read access toocontainers and blobs](../blobs/storage-manage-access-to-resources.md).</span></span>
>
>

### <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="5f1ea-174">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="5f1ea-174">Upload a blob into a container</span></span>
<span data-ttu-id="5f1ea-175">Azure Blob Storage stöder blockblobbar och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-175">Azure Blob Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="5f1ea-176">Mer information finns i [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f1ea-176">For more information, see [Understanding Block Blobs, Append Blobs, and Page Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).</span></span>

<span data-ttu-id="5f1ea-177">tooupload blobbar i behållaren tooa, kan du använda hello `azure storage blob upload`.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-177">tooupload blobs in tooa container, you can use hello `azure storage blob upload`.</span></span> <span data-ttu-id="5f1ea-178">Som standard överför det här kommandot hello lokala filer tooa blockblob.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-178">By default, this command uploads hello local files tooa block blob.</span></span> <span data-ttu-id="5f1ea-179">toospecify hello typ för hello blob, du kan använda hello `--blobtype` parameter.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-179">toospecify hello type for hello blob, you can use hello `--blobtype` parameter.</span></span>

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a><span data-ttu-id="5f1ea-180">Ladda ned blobbar från en behållare</span><span class="sxs-lookup"><span data-stu-id="5f1ea-180">Download blobs from a container</span></span>
<span data-ttu-id="5f1ea-181">hello som följande exempel visar hur toodownload BLOB-objekt från en behållare.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-181">hello following example demonstrates how toodownload blobs from a container.</span></span>

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a><span data-ttu-id="5f1ea-182">Kopiera BLOB</span><span class="sxs-lookup"><span data-stu-id="5f1ea-182">Copy blobs</span></span>
<span data-ttu-id="5f1ea-183">Du kan kopiera blobar i eller mellan lagringskonton och regioner asynkront.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-183">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="5f1ea-184">hello visar exemplet nedan hur toocopy blobbar från en lagring kontot tooanother.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-184">hello following example demonstrates how toocopy blobs from one storage account tooanother.</span></span> <span data-ttu-id="5f1ea-185">I det här exemplet skapar vi en behållare där blobbar är offentligt, anonymt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-185">In this sample we create a container where blobs are publicly, anonymously accessible.</span></span>

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

<span data-ttu-id="5f1ea-186">Det här exemplet utför en asynkron kopia.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-186">This sample performs an asynchronous copy.</span></span> <span data-ttu-id="5f1ea-187">Du kan övervaka hello status för varje kopieringen genom att köra hello `azure storage blob copy show` igen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-187">You can monitor hello status of each copy operation by running hello `azure storage blob copy show` operation.</span></span>

<span data-ttu-id="5f1ea-188">Observera att hello Käll-URL för hello kopieringsåtgärden måste antingen vara allmänt tillgänglig eller ta en SAS (signatur för delad åtkomst)-token.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-188">Note that hello source URL provided for hello copy operation must either be publicly accessible, or include a SAS (shared access signature) token.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="5f1ea-189">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="5f1ea-189">Delete a blob</span></span>
<span data-ttu-id="5f1ea-190">toodelete en blob med hello nedan kommando:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-190">toodelete a blob, use hello below command:</span></span>

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="5f1ea-191">Skapa och hantera filresurser</span><span class="sxs-lookup"><span data-stu-id="5f1ea-191">Create and manage file shares</span></span>
<span data-ttu-id="5f1ea-192">Azure File storage erbjuder delad lagring för program som använder hello SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-192">Azure File storage offers shared storage for applications using hello standard SMB protocol.</span></span> <span data-ttu-id="5f1ea-193">Microsoft Azure-datorer och molntjänster, samt lokala program kan dela fildata via monterade resurser.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-193">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="5f1ea-194">Du kan hantera filresurser och fildata via hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-194">You can manage file shares and file data via hello Azure CLI.</span></span> <span data-ttu-id="5f1ea-195">Mer information om Azure File storage finns [Kom igång med Azure File storage i Windows](../storage-dotnet-how-to-use-files.md) eller [hur toouse Azure File storage med Linux](../storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="5f1ea-195">For more information on Azure File storage, see [Get started with Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) or [How toouse Azure File storage with Linux](../storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="5f1ea-196">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="5f1ea-196">Create a file share</span></span>
<span data-ttu-id="5f1ea-197">En filresurs i Azure är en SMB-filresurs i Azure.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-197">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="5f1ea-198">Alla kataloger och filer måste skapas i en filresurs.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-198">All directories and files must be created in a file share.</span></span> <span data-ttu-id="5f1ea-199">Ett konto kan innehålla ett obegränsat antal resurser och en resurs kan lagra ett obegränsat antal filer, upp toohello kapacitetsbegränsningar för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-199">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up toohello capacity limits of hello storage account.</span></span> <span data-ttu-id="5f1ea-200">hello följande exempel skapar en filresurs med namnet **minresurs**.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-200">hello following example creates a file share named **myshare**.</span></span>

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="5f1ea-201">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="5f1ea-201">Create a directory</span></span>
<span data-ttu-id="5f1ea-202">En katalog innehåller ett valfritt hierarkisk struktur för ett Azure-filresursen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-202">A directory provides an optional hierarchical structure for an Azure file share.</span></span> <span data-ttu-id="5f1ea-203">hello följande exempel skapas en katalog med namnet **MinMapp** i hello filresurs.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-203">hello following example creates a directory named **myDir** in hello file share.</span></span>

```azurecli
azure storage directory create myshare myDir
```

<span data-ttu-id="5f1ea-204">Observera att sökvägen kan innehålla flera nivåer *t.ex.*, **en / b**.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-204">Note that directory path can include multiple levels, *e.g.*, **a/b**.</span></span> <span data-ttu-id="5f1ea-205">Dock måste du kontrollera att alla överordnade kataloger finns.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-205">However, you must ensure that all parent directories exist.</span></span> <span data-ttu-id="5f1ea-206">Till exempel för sökvägen **en / b**, måste du skapa katalog **en** först, sedan att skapa katalog **b**.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-206">For example, for path **a/b**, you must create directory **a** first, then create directory **b**.</span></span>

### <a name="upload-a-local-file-toodirectory"></a><span data-ttu-id="5f1ea-207">Ladda upp en lokal fil toodirectory</span><span class="sxs-lookup"><span data-stu-id="5f1ea-207">Upload a local file toodirectory</span></span>
<span data-ttu-id="5f1ea-208">hello följande exempel överför en fil från **~/temp/samplefile.txt** toohello **MinMapp** directory.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-208">hello following example uploads a file from **~/temp/samplefile.txt** toohello **myDir** directory.</span></span> <span data-ttu-id="5f1ea-209">Redigera hello sökvägen så att den pekar tooa giltig fil på den lokala datorn:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-209">Edit hello file path so that it points tooa valid file on your local machine:</span></span>

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

<span data-ttu-id="5f1ea-210">Observera att en fil i hello resursen kan vara upp too1 TB i storlek.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-210">Note that a file in hello share can be up too1 TB in size.</span></span>

### <a name="list-hello-files-in-hello-share-root-or-directory"></a><span data-ttu-id="5f1ea-211">Lista över hello filer i hello resursen rot- eller directory</span><span class="sxs-lookup"><span data-stu-id="5f1ea-211">List hello files in hello share root or directory</span></span>
<span data-ttu-id="5f1ea-212">Du kan ange hello filer och underkataloger i en rot-resurs eller en katalog med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-212">You can list hello files and subdirectories in a share root or a directory using hello following command:</span></span>

```azurecli
azure storage file list myshare myDir
```

<span data-ttu-id="5f1ea-213">Observera att hello katalognamnet är valfri för hello lista igen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-213">Note that hello directory name is optional for hello listing operation.</span></span> <span data-ttu-id="5f1ea-214">Om det utelämnas används visar hello kommando hello innehållet hello rotkatalogen hello-resursen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-214">If omitted, hello command lists hello contents of hello root directory of hello share.</span></span>

### <a name="copy-files"></a><span data-ttu-id="5f1ea-215">Kopiera filer</span><span class="sxs-lookup"><span data-stu-id="5f1ea-215">Copy files</span></span>
<span data-ttu-id="5f1ea-216">Från och med version 0.9.8 av Azure CLI kan kopiera du en fil för tooanother, en tooa blob-fil eller en tooa blob-fil.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-216">Beginning with version 0.9.8 of Azure CLI, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="5f1ea-217">Nedan ser du hur tooperform dessa kopiera åtgärder med hjälp av CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-217">Below we demonstrate how tooperform these copy operations using CLI commands.</span></span> <span data-ttu-id="5f1ea-218">toocopy en ny katalog i toohello:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-218">toocopy a file toohello new directory:</span></span>

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

<span data-ttu-id="5f1ea-219">toocopy en blob tooa katalog:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-219">toocopy a blob tooa file directory:</span></span>

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a><span data-ttu-id="5f1ea-220">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f1ea-220">Next Steps</span></span>

<span data-ttu-id="5f1ea-221">Du kan hitta Azure CLI 1.0 kommandoreferens för att arbeta med lagringsresurser här:</span><span class="sxs-lookup"><span data-stu-id="5f1ea-221">You can find Azure CLI 1.0 command reference for working with Storage resources here:</span></span>

* [<span data-ttu-id="5f1ea-222">Azure CLI-kommandona i Resource Manager-läge</span><span class="sxs-lookup"><span data-stu-id="5f1ea-222">Azure CLI commands in Resource Manager mode</span></span>](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [<span data-ttu-id="5f1ea-223">Azure CLI-kommandona i Azure Service Management-läge</span><span class="sxs-lookup"><span data-stu-id="5f1ea-223">Azure CLI commands in Azure Service Management mode</span></span>](../../cli-install-nodejs.md)

<span data-ttu-id="5f1ea-224">Du kan också använda tootry hello [Azure CLI 2.0](../storage-azure-cli.md), våra nästa generations CLI som skrivits i Python, för hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="5f1ea-224">You may also like tootry hello [Azure CLI 2.0](../storage-azure-cli.md), our next-generation CLI written in Python, for use with hello Resource Manager deployment model.</span></span>
