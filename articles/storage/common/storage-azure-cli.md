---
title: aaaUsing hello Azure CLI 2.0 med Azure Storage | Microsoft Docs
description: "Lär dig hur toouse hello Azure-kommandoradsgränssnittet (Azure CLI) 2.0 med Azure Storage toocreate och hantera storage-konton och arbeta med Azure-blobbar och filer. hello Azure CLI 2.0 är ett verktyg för flera plattformar som skrivits i Python."
services: storage
documentationcenter: na
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 06/02/2017
ms.author: marsma
ms.openlocfilehash: 14e6eb0c913676380c90a72563276245e7f08aa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cli-20-with-azure-storage"></a><span data-ttu-id="0ddea-104">Med Azure Storage hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0ddea-104">Using hello Azure CLI 2.0 with Azure Storage</span></span>

<span data-ttu-id="0ddea-105">hello öppen källkod, plattformsoberoende Azure CLI 2.0 innehåller en uppsättning kommandon för att arbeta med hello Azure-plattformen.</span><span class="sxs-lookup"><span data-stu-id="0ddea-105">hello open-source, cross-platform Azure CLI 2.0 provides a set of commands for working with hello Azure platform.</span></span> <span data-ttu-id="0ddea-106">Det ger mycket hello samma funktioner som finns i hello [Azure-portalen](https://portal.azure.com), inklusive omfattande dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="0ddea-106">It provides much of hello same functionality found in hello [Azure portal](https://portal.azure.com), including rich data access.</span></span>

<span data-ttu-id="0ddea-107">I den här guiden visar vi dig hur toouse hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooperform flera uppgifter arbeta med resurserna i Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0ddea-107">In this guide, we show you how toouse hello [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooperform several tasks working with resources in your Azure Storage account.</span></span> <span data-ttu-id="0ddea-108">Vi rekommenderar att du hämtar och installerar eller uppgraderar toohello senaste versionen av hello CLI 2.0 innan du använder den här guiden.</span><span class="sxs-lookup"><span data-stu-id="0ddea-108">We recommend that you download and install or upgrade toohello latest version of hello CLI 2.0 before using this guide.</span></span>

<span data-ttu-id="0ddea-109">hello exemplen i hello handboken förutsätter hello användning av hello Bash shell på Ubuntu, men andra plattformar ska utföra på samma sätt.</span><span class="sxs-lookup"><span data-stu-id="0ddea-109">hello examples in hello guide assume hello use of hello Bash shell on Ubuntu, but other platforms should perform similarly.</span></span> 

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="prerequisites"></a><span data-ttu-id="0ddea-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0ddea-110">Prerequisites</span></span>
<span data-ttu-id="0ddea-111">Den här handboken förutsätts att du förstår hello grundläggande begrepp för Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="0ddea-111">This guide assumes that you understand hello basic concepts of Azure Storage.</span></span> <span data-ttu-id="0ddea-112">Det förutsätts även att du är kan toosatisfy hello skapa krav som anges nedan för Azure och hello Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0ddea-112">It also assumes that you're able toosatisfy hello account creation requirements that are specified below for Azure and hello Storage service.</span></span>

### <a name="accounts"></a><span data-ttu-id="0ddea-113">Konton</span><span class="sxs-lookup"><span data-stu-id="0ddea-113">Accounts</span></span>
* <span data-ttu-id="0ddea-114">**Azure-konto**: Om du inte redan har en Azure-prenumeration [skapa ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="0ddea-114">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="0ddea-115">**Lagringskonto**: Se [skapar ett lagringskonto](storage-create-storage-account.md#create-a-storage-account) i [Om Azure-lagringskonton](storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="0ddea-115">**Storage account**: See [Create a storage account](storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](storage-create-storage-account.md).</span></span>

### <a name="install-hello-azure-cli-20"></a><span data-ttu-id="0ddea-116">Installera hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0ddea-116">Install hello Azure CLI 2.0</span></span>

<span data-ttu-id="0ddea-117">Hämta och installera hello Azure CLI 2.0 genom att följa hello instruktionerna [installera Azure CLI 2.0](/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="0ddea-117">Download and install hello Azure CLI 2.0 by following hello instructions outlined in [Install Azure CLI 2.0](/cli/azure/install-az-cli2).</span></span>

> [!TIP]
> <span data-ttu-id="0ddea-118">Om du har problem med installation hello kolla hello [felsökning för installationen](/cli/azure/install-az-cli2#installation-troubleshooting) avsnittet hello artikel och hello [installera felsökning](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) guide på GitHub.</span><span class="sxs-lookup"><span data-stu-id="0ddea-118">If you have trouble with hello installation, check out hello [Installation Troubleshooting](/cli/azure/install-az-cli2#installation-troubleshooting) section of hello article, and hello [Install Troubleshooting](https://github.com/Azure/azure-cli/blob/master/doc/install_troubleshooting.md) guide on GitHub.</span></span>
>

## <a name="working-with-hello-cli"></a><span data-ttu-id="0ddea-119">Arbeta med hello CLI</span><span class="sxs-lookup"><span data-stu-id="0ddea-119">Working with hello CLI</span></span>

<span data-ttu-id="0ddea-120">När du har installerat hello CLI, kan du använda hello `az` i din kommandoradsgränssnittet (Bash, Terminal, Kommandotolken) tooaccess hello Azure CLI-kommandona.</span><span class="sxs-lookup"><span data-stu-id="0ddea-120">Once you've installed hello CLI, you can use hello `az` command in your command-line interface (Bash, Terminal, Command Prompt) tooaccess hello Azure CLI commands.</span></span> <span data-ttu-id="0ddea-121">Typen hello `az` kommandot toosee en fullständig lista över grundläggande hello-kommandon (hello följande exempel på utdata har trunkerats):</span><span class="sxs-lookup"><span data-stu-id="0ddea-121">Type hello `az` command toosee a full list of hello base commands (hello following example output has been truncated):</span></span>

```
     /\
    /  \    _____   _ _ __ ___
   / /\ \  |_  / | | | \'__/ _ \
  / ____ \  / /| |_| | | |  __/
 /_/    \_\/___|\__,_|_|  \___|


Welcome toohello cool new Azure CLI!

Here are hello base commands:

    account          : Manage subscriptions.
    acr              : Manage Azure container registries.
    acs              : Manage Azure Container Services.
    ad               : Synchronize on-premises directories and manage Azure Active Directory
                       resources.
    ...
```

<span data-ttu-id="0ddea-122">Köra hello-kommandot i din kommandoradsgränssnittet `az storage --help` toolist hello `storage` kommandot undergrupper.</span><span class="sxs-lookup"><span data-stu-id="0ddea-122">In your command-line interface, execute hello command `az storage --help` toolist hello `storage` command subgroups.</span></span> <span data-ttu-id="0ddea-123">hello beskrivningar av hello undergrupper ger en översikt över hello funktioner hello Azure CLI tillhandahåller för att arbeta med dina lagringsresurser.</span><span class="sxs-lookup"><span data-stu-id="0ddea-123">hello descriptions of hello subgroups provide an overview of hello functionality hello Azure CLI provides for working with your storage resources.</span></span>

```
Group
    az storage: Durable, highly available, and massively scalable cloud storage.

Subgroups:
    account  : Manage storage accounts.
    blob     : Object storage for unstructured data.
    container: Manage blob storage containers.
    cors     : Manage Storage service Cross-Origin Resource Sharing (CORS).
    directory: Manage file storage directories.
    entity   : Manage table storage entities.
    file     : File shares that use hello standard SMB 3.0 protocol.
    logging  : Manage Storage service logging information.
    message  : Manage queue storage messages.
    metrics  : Manage Storage service metrics.
    queue    : Use queues tooeffectively scale applications according tootraffic.
    share    : Manage file shares.
    table    : NoSQL key-value storage using semi-structured datasets.
```

## <a name="connect-hello-cli-tooyour-azure-subscription"></a><span data-ttu-id="0ddea-124">Ansluta hello CLI tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0ddea-124">Connect hello CLI tooyour Azure subscription</span></span>

<span data-ttu-id="0ddea-125">toowork med hello resurser i din Azure-prenumeration måste du först logga i tooyour Azure-konto med `az login`.</span><span class="sxs-lookup"><span data-stu-id="0ddea-125">toowork with hello resources in your Azure subscription, you must first log in tooyour Azure account with `az login`.</span></span> <span data-ttu-id="0ddea-126">Det finns flera sätt som du kan logga in:</span><span class="sxs-lookup"><span data-stu-id="0ddea-126">There are several ways you can log in:</span></span>

* <span data-ttu-id="0ddea-127">**Interaktiv inloggning**:`az login`</span><span class="sxs-lookup"><span data-stu-id="0ddea-127">**Interactive login**: `az login`</span></span>
* <span data-ttu-id="0ddea-128">**Logga in med användarnamn och lösenord**:`az login -u johndoe@contoso.com -p VerySecret`</span><span class="sxs-lookup"><span data-stu-id="0ddea-128">**Log in with user name and password**: `az login -u johndoe@contoso.com -p VerySecret`</span></span>
  * <span data-ttu-id="0ddea-129">Detta fungerar inte med Microsoft-konton eller konton som använder multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="0ddea-129">This doesn't work with Microsoft accounts or accounts that use multi-factor authentication.</span></span>
* <span data-ttu-id="0ddea-130">**Logga in med ett huvudnamn för tjänsten**:`az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span><span class="sxs-lookup"><span data-stu-id="0ddea-130">**Log in with a service principal**: `az login --service-principal -u http://azure-cli-2016-08-05-14-31-15 -p VerySecret --tenant contoso.onmicrosoft.com`</span></span>

## <a name="azure-cli-20-sample-script"></a><span data-ttu-id="0ddea-131">Azure CLI 2.0 exempelskript</span><span class="sxs-lookup"><span data-stu-id="0ddea-131">Azure CLI 2.0 sample script</span></span>

<span data-ttu-id="0ddea-132">Nu ska arbetar vi med ett litet kommandoskript som utfärdar några grundläggande Azure CLI 2.0 kommandon toointeract med Azure Storage-resurser.</span><span class="sxs-lookup"><span data-stu-id="0ddea-132">Next, we'll work with a small shell script that issues a few basic Azure CLI 2.0 commands toointeract with Azure Storage resources.</span></span> <span data-ttu-id="0ddea-133">hello skriptet först skapar en ny behållare på ditt lagringskonto och överför en befintlig fil (som en blob) toothat behållare.</span><span class="sxs-lookup"><span data-stu-id="0ddea-133">hello script first creates a new container in your storage account, then uploads an existing file (as a blob) toothat container.</span></span> <span data-ttu-id="0ddea-134">Den visar en lista över alla blobbar i behållaren hello och slutligen hämtar hello filen tooa mål på den lokala datorn som du anger.</span><span class="sxs-lookup"><span data-stu-id="0ddea-134">It then lists all blobs in hello container, and finally, downloads hello file tooa destination on your local computer that you specify.</span></span>

```bash
#!/bin/bash
# A simple Azure Storage example script

export AZURE_STORAGE_ACCOUNT=<storage_account_name>
export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

export container_name=<container_name>
export blob_name=<blob_name>
export file_to_upload=<file_to_upload>
export destination_file=<destination_file>

echo "Creating hello container..."
az storage container create --name $container_name

echo "Uploading hello file..."
az storage blob upload --container-name $container_name --file $file_to_upload --name $blob_name

echo "Listing hello blobs..."
az storage blob list --container-name $container_name --output table

echo "Downloading hello file..."
az storage blob download --container-name $container_name --name $blob_name --file $destination_file --output table

echo "Done"
```

<span data-ttu-id="0ddea-135">**Konfigurera och köra hello-skript**</span><span class="sxs-lookup"><span data-stu-id="0ddea-135">**Configure and run hello script**</span></span>

1. <span data-ttu-id="0ddea-136">Öppna valfri textredigerare, och sedan kopiera och klistra in hello föregående skript i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="0ddea-136">Open your favorite text editor, then copy and paste hello preceding script into hello editor.</span></span>

2. <span data-ttu-id="0ddea-137">Därefter uppdatera hello skript variabler tooreflect konfigurationsinställningarna.</span><span class="sxs-lookup"><span data-stu-id="0ddea-137">Next, update hello script's variables tooreflect your configuration settings.</span></span> <span data-ttu-id="0ddea-138">Ersätt hello värden enligt följande:</span><span class="sxs-lookup"><span data-stu-id="0ddea-138">Replace hello following values as specified:</span></span>

   * <span data-ttu-id="0ddea-139">**\<storage_account_name\>**  hello namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0ddea-139">**\<storage_account_name\>** hello name of your storage account.</span></span>
   * <span data-ttu-id="0ddea-140">**\<storage_account_key\>**  hello primära eller sekundära åtkomstnyckeln för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0ddea-140">**\<storage_account_key\>** hello primary or secondary access key for your storage account.</span></span>
   * <span data-ttu-id="0ddea-141">**\<container_name\>**  ett namn hello nya behållare toocreate, till exempel ”azure-cli-exemplet-container”.</span><span class="sxs-lookup"><span data-stu-id="0ddea-141">**\<container_name\>** A name hello new container toocreate, such as "azure-cli-sample-container".</span></span>
   * <span data-ttu-id="0ddea-142">**\<blob_name\>**  ett namn för hello i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="0ddea-142">**\<blob_name\>** A name for hello destination blob in hello container.</span></span>
   * <span data-ttu-id="0ddea-143">**\<file_to_upload\>**  hello sökväg toosmall fil på den lokala datorn som ”~ / images/HelloWorld.png”.</span><span class="sxs-lookup"><span data-stu-id="0ddea-143">**\<file_to_upload\>** hello path toosmall file on your local computer, such as "~/images/HelloWorld.png".</span></span>
   * <span data-ttu-id="0ddea-144">**\<destination_file\>**  hello mål filsökväg som ”~ / downloadedImage.png”.</span><span class="sxs-lookup"><span data-stu-id="0ddea-144">**\<destination_file\>** hello destination file path, such as "~/downloadedImage.png".</span></span>

3. <span data-ttu-id="0ddea-145">När du har uppdaterat hello nödvändiga variabler, spara hello skript och avsluta redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="0ddea-145">After you've updated hello necessary variables, save hello script and exit your editor.</span></span> <span data-ttu-id="0ddea-146">hello nästa steg förutsätter att du har namngett skriptet **my_storage_sample.sh**.</span><span class="sxs-lookup"><span data-stu-id="0ddea-146">hello next steps assume you've named your script **my_storage_sample.sh**.</span></span>

4. <span data-ttu-id="0ddea-147">Markera hello skriptet som körbara, om det behövs:`chmod +x my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="0ddea-147">Mark hello script as executable, if necessary: `chmod +x my_storage_sample.sh`</span></span>

5. <span data-ttu-id="0ddea-148">Kör hello-skriptet.</span><span class="sxs-lookup"><span data-stu-id="0ddea-148">Execute hello script.</span></span> <span data-ttu-id="0ddea-149">Till exempel i Bash:`./my_storage_sample.sh`</span><span class="sxs-lookup"><span data-stu-id="0ddea-149">For example, in Bash: `./my_storage_sample.sh`</span></span>

<span data-ttu-id="0ddea-150">Du bör se utdata liknande toohello nedan och hello  **\<destination_file\>**  du angav i hello skript ska visas på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="0ddea-150">You should see output similar toohello following, and hello **\<destination_file\>** you specified in hello script should appear on your local computer.</span></span>

```
Creating hello container...
{
  "created": true
}
Uploading hello file...
Percent complete: %100.0
Listing hello blobs...
Name       Blob Type      Length  Content Type              Last Modified
---------  -----------  --------  ------------------------  -------------------------
README.md  BlockBlob        6700  application/octet-stream  2017-05-12T20:54:59+00:00
Downloading hello file...
Name
---------
README.md
Done
```

> [!TIP]
> <span data-ttu-id="0ddea-151">hello föregående utdata är i **tabell** format.</span><span class="sxs-lookup"><span data-stu-id="0ddea-151">hello preceding output is in **table** format.</span></span> <span data-ttu-id="0ddea-152">Du kan ange vilka utdata format toouse genom att ange hello `--output` argument i CLI-kommandon eller Ställ in den globalt med `az configure`.</span><span class="sxs-lookup"><span data-stu-id="0ddea-152">You can specify which output format toouse by specifying hello `--output` argument in your CLI commands, or set it globally using `az configure`.</span></span>
>

## <a name="manage-storage-accounts"></a><span data-ttu-id="0ddea-153">Hantera lagringskonton</span><span class="sxs-lookup"><span data-stu-id="0ddea-153">Manage storage accounts</span></span>

### <a name="create-a-new-storage-account"></a><span data-ttu-id="0ddea-154">Skapa ett nytt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="0ddea-154">Create a new storage account</span></span>
<span data-ttu-id="0ddea-155">toouse Azure Storage, du behöver ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0ddea-155">toouse Azure Storage, you need a storage account.</span></span> <span data-ttu-id="0ddea-156">Du kan skapa ett nytt Azure Storage-konto när du har konfigurerat datorn för[ansluter tooyour prenumeration](#connect-to-your-azure-subscription).</span><span class="sxs-lookup"><span data-stu-id="0ddea-156">You can create a new Azure Storage account after you've configured your computer too[connect tooyour subscription](#connect-to-your-azure-subscription).</span></span>

```azurecli
az storage account create \
    --location <location> \
    --name <account_name> \
    --resource-group <resource_group> \
    --sku <account_sku>
```

* <span data-ttu-id="0ddea-157">`--location`[Krävs]: plats.</span><span class="sxs-lookup"><span data-stu-id="0ddea-157">`--location` [Required]: Location.</span></span> <span data-ttu-id="0ddea-158">Till exempel ”USA, västra”.</span><span class="sxs-lookup"><span data-stu-id="0ddea-158">For example, "West US".</span></span>
* <span data-ttu-id="0ddea-159">`--name`[Krävs]: hello lagringskontonamn.</span><span class="sxs-lookup"><span data-stu-id="0ddea-159">`--name` [Required]: hello storage account name.</span></span> <span data-ttu-id="0ddea-160">hello-namnet måste bestå av 3 too24 tecken och Använd endast gemener alfanumeriska tecken.</span><span class="sxs-lookup"><span data-stu-id="0ddea-160">hello name must be 3 too24 characters in length, and use only lowercase alphanumeric characters.</span></span>
* <span data-ttu-id="0ddea-161">`--resource-group`[Krävs]: namnet på resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="0ddea-161">`--resource-group` [Required]: Name of resource group.</span></span>
* <span data-ttu-id="0ddea-162">`--sku`[Krävs]: hello SKU-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="0ddea-162">`--sku` [Required]: hello storage account SKU.</span></span> <span data-ttu-id="0ddea-163">Tillåtna värden:</span><span class="sxs-lookup"><span data-stu-id="0ddea-163">Allowed values:</span></span>
  * `Premium_LRS`
  * `Standard_GRS`
  * `Standard_LRS`
  * `Standard_RAGRS`
  * `Standard_ZRS`

### <a name="set-default-azure-storage-account-environment-variables"></a><span data-ttu-id="0ddea-164">Ange standardkontot för Azure storage miljövariabler</span><span class="sxs-lookup"><span data-stu-id="0ddea-164">Set default Azure storage account environment variables</span></span>
<span data-ttu-id="0ddea-165">Du kan ha flera lagringskonton i Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="0ddea-165">You can have multiple storage accounts in your Azure subscription.</span></span> <span data-ttu-id="0ddea-166">tooselect en av dem toouse för alla efterföljande lagring kommandon, kan du ange de här miljövariablerna:</span><span class="sxs-lookup"><span data-stu-id="0ddea-166">tooselect one of them toouse for all subsequent storage commands, you can set these environment variables:</span></span>

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

<span data-ttu-id="0ddea-167">Det är ett annat sätt tooset standardkontot för lagring med hjälp av en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="0ddea-167">Another way tooset a default storage account is by using a connection string.</span></span> <span data-ttu-id="0ddea-168">Först hämta hello anslutningssträngen med hello `show-connection-string` kommando:</span><span class="sxs-lookup"><span data-stu-id="0ddea-168">First, get hello connection string with hello `show-connection-string` command:</span></span>

```azurecli
az storage account show-connection-string \
    --name <account_name> \
    --resource-group <resource_group>
```

<span data-ttu-id="0ddea-169">Sedan kopiera hello utdata anslutningssträngen och ange hello `AZURE_STORAGE_CONNECTION_STRING` miljövariabeln (du kanske måste tooenclose hello anslutningssträngen med citattecken):</span><span class="sxs-lookup"><span data-stu-id="0ddea-169">Then copy hello output connection string and set hello `AZURE_STORAGE_CONNECTION_STRING` environment variable (you might need tooenclose hello connection string in quotes):</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING="<connection_string>"
```

> [!NOTE]
> <span data-ttu-id="0ddea-170">Alla exempel i hello följande avsnitt i den här artikeln förutsätter att du har ställt in hello `AZURE_STORAGE_ACCOUNT` och `AZURE_STORAGE_ACCESS_KEY` miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="0ddea-170">All examples in hello following sections of this article assume that you've set hello `AZURE_STORAGE_ACCOUNT` and `AZURE_STORAGE_ACCESS_KEY` environment variables.</span></span>
>

## <a name="create-and-manage-blobs"></a><span data-ttu-id="0ddea-171">Skapa och hantera blobbar</span><span class="sxs-lookup"><span data-stu-id="0ddea-171">Create and manage blobs</span></span>
<span data-ttu-id="0ddea-172">Azure Blob storage är en tjänst för att lagra stora mängder Ostrukturerade data, till exempel text eller binära data som kan nås från var som helst i hello world via HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0ddea-172">Azure Blob storage is a service for storing large amounts of unstructured data, such as text or binary data, that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="0ddea-173">Det här avsnittet förutsätter att du redan är bekant med Azure Blob storage-koncept.</span><span class="sxs-lookup"><span data-stu-id="0ddea-173">This section assumes that you are already familiar with Azure Blob storage concepts.</span></span> <span data-ttu-id="0ddea-174">Detaljerad information finns i [komma igång med Azure Blob storage med hjälp av .NET](../blobs/storage-dotnet-how-to-use-blobs.md) och [Blob-Tjänstkoncept](/rest/api/storageservices/blob-service-concepts).</span><span class="sxs-lookup"><span data-stu-id="0ddea-174">For detailed information, see [Get started with Azure Blob storage using .NET](../blobs/storage-dotnet-how-to-use-blobs.md) and [Blob Service Concepts](/rest/api/storageservices/blob-service-concepts).</span></span>

### <a name="create-a-container"></a><span data-ttu-id="0ddea-175">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="0ddea-175">Create a container</span></span>
<span data-ttu-id="0ddea-176">Varje blobb i Azure-lagring måste vara i en behållare.</span><span class="sxs-lookup"><span data-stu-id="0ddea-176">Every blob in Azure storage must be in a container.</span></span> <span data-ttu-id="0ddea-177">Du kan skapa en behållare med hello `az storage container create` kommando:</span><span class="sxs-lookup"><span data-stu-id="0ddea-177">You can create a container by using hello `az storage container create` command:</span></span>

```azurecli
az storage container create --name <container_name>
```

<span data-ttu-id="0ddea-178">Du kan ange en av tre nivåer för läsbehörighet för en ny behållare genom att ange hello valfria `--public-access` argument:</span><span class="sxs-lookup"><span data-stu-id="0ddea-178">You can set one of three levels of read access for a new container by specifying hello optional  `--public-access` argument:</span></span>

* <span data-ttu-id="0ddea-179">`off`(standard): behållardata privata toohello konto äger.</span><span class="sxs-lookup"><span data-stu-id="0ddea-179">`off` (default): Container data is private toohello account owner.</span></span>
* <span data-ttu-id="0ddea-180">`blob`: Offentlig läsbehörighet för blobbar.</span><span class="sxs-lookup"><span data-stu-id="0ddea-180">`blob`: Public read access for blobs.</span></span>
* <span data-ttu-id="0ddea-181">`container`: Offentliga Läs- och åtkomst toohello hela behållaren.</span><span class="sxs-lookup"><span data-stu-id="0ddea-181">`container`: Public read and list access toohello entire container.</span></span>

<span data-ttu-id="0ddea-182">Mer information finns i [Hantera anonym läsbehörighet toocontainers och blobbar](../blobs/storage-manage-access-to-resources.md).</span><span class="sxs-lookup"><span data-stu-id="0ddea-182">For more information, see [Manage anonymous read access toocontainers and blobs](../blobs/storage-manage-access-to-resources.md).</span></span>

### <a name="upload-a-blob-tooa-container"></a><span data-ttu-id="0ddea-183">Överför en tooa blob-behållare</span><span class="sxs-lookup"><span data-stu-id="0ddea-183">Upload a blob tooa container</span></span>
<span data-ttu-id="0ddea-184">Azure Blob storage stöder block, Lägg till och sidblobbar.</span><span class="sxs-lookup"><span data-stu-id="0ddea-184">Azure Blob storage supports block, append, and page blobs.</span></span> <span data-ttu-id="0ddea-185">Överför blobbar tooa behållaren med hjälp av hello `blob upload` kommando:</span><span class="sxs-lookup"><span data-stu-id="0ddea-185">Upload blobs tooa container by using hello `blob upload` command:</span></span>

```azurecli
az storage blob upload \
    --file <local_file_path> \
    --container-name <container_name> \
    --name <blob_name>
```

 <span data-ttu-id="0ddea-186">Som standard hello `blob upload` kommandot överför *.vhd filer toopage blobbar eller blockblobbar på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="0ddea-186">By default, hello `blob upload` command uploads *.vhd files toopage blobs, or block blobs otherwise.</span></span> <span data-ttu-id="0ddea-187">toospecify en annan skriver när du laddar upp en blobb kan du använda hello `--type` argumentet--tillåtna värden är `append`, `block`, och `page`.</span><span class="sxs-lookup"><span data-stu-id="0ddea-187">toospecify another type when you upload a blob, you can use hello `--type` argument--allowed values are `append`, `block`, and `page`.</span></span>

 <span data-ttu-id="0ddea-188">Mer information om hello annan blob-typer finns [förstå Blockblobbar, Tilläggsblobbar och Sidblobbar](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span><span class="sxs-lookup"><span data-stu-id="0ddea-188">For more information on hello different blob types, see [Understanding Block Blobs, Append Blobs, and Page Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).</span></span>


### <a name="download-a-blob-from-a-container"></a><span data-ttu-id="0ddea-189">Hämta en blob från en behållare</span><span class="sxs-lookup"><span data-stu-id="0ddea-189">Download a blob from a container</span></span>
<span data-ttu-id="0ddea-190">Det här exemplet visar hur toodownload en blob från en behållare:</span><span class="sxs-lookup"><span data-stu-id="0ddea-190">This example demonstrates how toodownload a blob from a container:</span></span>

```azurecli
az storage blob download \
    --container-name mycontainer \
    --name myblob.png \
    --file ~/mydownloadedblob.png
```

### <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="0ddea-191">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="0ddea-191">List hello blobs in a container</span></span>

<span data-ttu-id="0ddea-192">Visa hello blobbar i en behållare med hello [az storage blob listan](/cli/azure/storage/blob#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="0ddea-192">List hello blobs in a container with hello [az storage blob list](/cli/azure/storage/blob#list) command.</span></span>

```azurecli
az storage blob list \
    --container-name mycontainer \
    --output table
```

### <a name="copy-blobs"></a><span data-ttu-id="0ddea-193">Kopiera BLOB</span><span class="sxs-lookup"><span data-stu-id="0ddea-193">Copy blobs</span></span>
<span data-ttu-id="0ddea-194">Du kan kopiera blobar i eller mellan lagringskonton och regioner asynkront.</span><span class="sxs-lookup"><span data-stu-id="0ddea-194">You can copy blobs within or across storage accounts and regions asynchronously.</span></span>

<span data-ttu-id="0ddea-195">hello visar exemplet nedan hur toocopy blobbar från en lagring kontot tooanother.</span><span class="sxs-lookup"><span data-stu-id="0ddea-195">hello following example demonstrates how toocopy blobs from one storage account tooanother.</span></span> <span data-ttu-id="0ddea-196">Vi först skapa en behållare i hello källa storage-konto, ange offentlig läsbehörighet för dess blobbar.</span><span class="sxs-lookup"><span data-stu-id="0ddea-196">We first create a container in hello source storage account, specifying public read-access for its blobs.</span></span> <span data-ttu-id="0ddea-197">Därefter överföra vi en fil toohello behållare och slutligen kopiera hello blob från behållaren till en behållare i hello destinationslagringskontot.</span><span class="sxs-lookup"><span data-stu-id="0ddea-197">Next, we upload a file toohello container, and finally, copy hello blob from that container into a container in hello destination storage account.</span></span>

```azurecli
# Create container in source account
az storage container create \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --name sourcecontainer \
    --public-access blob

# Upload blob toocontainer in source account
az storage blob upload \
    --account-name sourceaccountname \
    --account-key sourceaccountkey \
    --container-name sourcecontainer \
    --file ~/Pictures/sourcefile.png \
    --name sourcefile.png

# Copy blob from source account toodestination account (destcontainer must exist)
az storage blob copy start \
    --account-name destaccountname \
    --account-key destaccountkey \
    --destination-blob destfile.png \
    --destination-container destcontainer \
    --source-uri https://sourceaccountname.blob.core.windows.net/sourcecontainer/sourcefile.png
```

<span data-ttu-id="0ddea-198">I ovanstående exempel hello finnas hello målbehållare redan i hello destinationslagringskontot för hello Kopiera åtgärden toosucceed.</span><span class="sxs-lookup"><span data-stu-id="0ddea-198">In hello above example, hello destination container must already exist in hello destination storage account for hello copy operation toosucceed.</span></span> <span data-ttu-id="0ddea-199">Dessutom hello källa blob anges i hello `--source-uri` argument måste inkludera en token för delad åtkomst signatur (SAS), eller vara allmänt tillgänglig i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="0ddea-199">Additionally, hello source blob specified in hello `--source-uri` argument must either include a shared access signature (SAS) token, or be publicly accessible, as in this example.</span></span>

### <a name="delete-a-blob"></a><span data-ttu-id="0ddea-200">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="0ddea-200">Delete a blob</span></span>
<span data-ttu-id="0ddea-201">toodelete blob använda hello `blob delete` kommando:</span><span class="sxs-lookup"><span data-stu-id="0ddea-201">toodelete a blob, use hello `blob delete` command:</span></span>

```azurecli
az storage blob delete --container-name <container_name> --name <blob_name>
```

## <a name="create-and-manage-file-shares"></a><span data-ttu-id="0ddea-202">Skapa och hantera filresurser</span><span class="sxs-lookup"><span data-stu-id="0ddea-202">Create and manage file shares</span></span>
<span data-ttu-id="0ddea-203">Azure File storage erbjuder delad lagring för program som använder hello Server Message Block (SMB) protokollet.</span><span class="sxs-lookup"><span data-stu-id="0ddea-203">Azure File storage offers shared storage for applications using hello Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="0ddea-204">Microsoft Azure-datorer och molntjänster, samt lokala program kan dela fildata via monterade resurser.</span><span class="sxs-lookup"><span data-stu-id="0ddea-204">Microsoft Azure virtual machines and cloud services, as well as on-premises applications, can share file data via mounted shares.</span></span> <span data-ttu-id="0ddea-205">Du kan hantera filresurser och fildata via hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0ddea-205">You can manage file shares and file data via hello Azure CLI.</span></span> <span data-ttu-id="0ddea-206">Mer information om Azure File storage finns [Kom igång med Azure File storage i Windows](../storage-dotnet-how-to-use-files.md) eller [hur toouse Azure File storage med Linux](../storage-how-to-use-files-linux.md).</span><span class="sxs-lookup"><span data-stu-id="0ddea-206">For more information on Azure File storage, see [Get started with Azure File storage on Windows](../storage-dotnet-how-to-use-files.md) or [How toouse Azure File storage with Linux](../storage-how-to-use-files-linux.md).</span></span>

### <a name="create-a-file-share"></a><span data-ttu-id="0ddea-207">Skapa en filresurs</span><span class="sxs-lookup"><span data-stu-id="0ddea-207">Create a file share</span></span>
<span data-ttu-id="0ddea-208">En filresurs i Azure är en SMB-filresurs i Azure.</span><span class="sxs-lookup"><span data-stu-id="0ddea-208">An Azure File share is an SMB file share in Azure.</span></span> <span data-ttu-id="0ddea-209">Alla kataloger och filer måste skapas i en filresurs.</span><span class="sxs-lookup"><span data-stu-id="0ddea-209">All directories and files must be created in a file share.</span></span> <span data-ttu-id="0ddea-210">Ett konto kan innehålla ett obegränsat antal resurser och en resurs kan lagra ett obegränsat antal filer, upp toohello kapacitetsbegränsningar för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0ddea-210">An account can contain an unlimited number of shares, and a share can store an unlimited number of files, up toohello capacity limits of hello storage account.</span></span> <span data-ttu-id="0ddea-211">hello följande exempel skapar en filresurs med namnet **minresurs**.</span><span class="sxs-lookup"><span data-stu-id="0ddea-211">hello following example creates a file share named **myshare**.</span></span>

```azurecli
az storage share create --name myshare
```

### <a name="create-a-directory"></a><span data-ttu-id="0ddea-212">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="0ddea-212">Create a directory</span></span>
<span data-ttu-id="0ddea-213">En katalog innehåller en hierarkisk struktur i ett Azure-filresursen.</span><span class="sxs-lookup"><span data-stu-id="0ddea-213">A directory provides a hierarchical structure in an Azure file share.</span></span> <span data-ttu-id="0ddea-214">hello följande exempel skapas en katalog med namnet **MinMapp** i hello filresurs.</span><span class="sxs-lookup"><span data-stu-id="0ddea-214">hello following example creates a directory named **myDir** in hello file share.</span></span>

```azurecli
az storage directory create --name myDir --share-name myshare
```

<span data-ttu-id="0ddea-215">En katalogsökväg kan innehålla flera nivåer, till exempel **dir1/dir2**.</span><span class="sxs-lookup"><span data-stu-id="0ddea-215">A directory path can include multiple levels, for example **dir1/dir2**.</span></span> <span data-ttu-id="0ddea-216">Dock måste du kontrollera att alla överordnade kataloger finns innan du skapar en underkatalog.</span><span class="sxs-lookup"><span data-stu-id="0ddea-216">However, you must ensure that all parent directories exist before creating a subdirectory.</span></span> <span data-ttu-id="0ddea-217">Till exempel för sökvägen **dir1/dir2**, måste du först skapa directory **dir1**, skapa katalog **dir2**.</span><span class="sxs-lookup"><span data-stu-id="0ddea-217">For example, for path **dir1/dir2**, you must first create directory **dir1**, then create directory **dir2**.</span></span>

### <a name="upload-a-local-file-tooa-share"></a><span data-ttu-id="0ddea-218">Ladda upp en lokal fil tooa resurs</span><span class="sxs-lookup"><span data-stu-id="0ddea-218">Upload a local file tooa share</span></span>
<span data-ttu-id="0ddea-219">hello följande exempel överför en fil från **~/temp/samplefile.txt** tooroot av hello **minresurs** filresurs.</span><span class="sxs-lookup"><span data-stu-id="0ddea-219">hello following example uploads a file from **~/temp/samplefile.txt** tooroot of hello **myshare** file share.</span></span> <span data-ttu-id="0ddea-220">Hej `--source` argumentet anger hello befintliga lokala filen tooupload.</span><span class="sxs-lookup"><span data-stu-id="0ddea-220">hello `--source` argument specifies hello existing local file tooupload.</span></span>

```azurecli
az storage file upload --share-name myshare --source ~/temp/samplefile.txt
```

<span data-ttu-id="0ddea-221">Du kan ange en sökväg i hello resursen tooupload hello filen tooan befintliga directory hello resursen som directory skapas:</span><span class="sxs-lookup"><span data-stu-id="0ddea-221">As with directory creation, you can specify a directory path within hello share tooupload hello file tooan existing directory within hello share:</span></span>

```azurecli
az storage file upload --share-name myshare/myDir --source ~/temp/samplefile.txt
```

<span data-ttu-id="0ddea-222">En fil i hello resursen kan vara upp too1 TB i storlek.</span><span class="sxs-lookup"><span data-stu-id="0ddea-222">A file in hello share can be up too1 TB in size.</span></span>

### <a name="list-hello-files-in-a-share"></a><span data-ttu-id="0ddea-223">Lista över hello filer i en resurs</span><span class="sxs-lookup"><span data-stu-id="0ddea-223">List hello files in a share</span></span>
<span data-ttu-id="0ddea-224">Du kan lista filer och kataloger på en resurs med hjälp av hello `az storage file list` kommando:</span><span class="sxs-lookup"><span data-stu-id="0ddea-224">You can list files and directories in a share by using hello `az storage file list` command:</span></span>

```azurecli
# List hello files in hello root of a share
az storage file list --share-name myshare --output table

# List hello files in a directory within a share
az storage file list --share-name myshare/myDir --output table

# List hello files in a path within a share
az storage file list --share-name myshare --path myDir/mySubDir/MySubDir2 --output table
```

### <a name="copy-files"></a><span data-ttu-id="0ddea-225">Kopiera filer</span><span class="sxs-lookup"><span data-stu-id="0ddea-225">Copy files</span></span>      
<span data-ttu-id="0ddea-226">Du kan kopiera en fil för tooanother, en tooa blob-fil eller en tooa blob-fil.</span><span class="sxs-lookup"><span data-stu-id="0ddea-226">You can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="0ddea-227">Till exempel toocopy en katalog i tooa i en annan resurs:</span><span class="sxs-lookup"><span data-stu-id="0ddea-227">For example, toocopy a file tooa directory in a different share:</span></span>        
        
```azurecli
az storage file copy start \
--source-share share1 --source-path dir1/file.txt \
--destination-share share2 --destination-path dir2/file.txt     
```

## <a name="next-steps"></a><span data-ttu-id="0ddea-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ddea-228">Next steps</span></span>
<span data-ttu-id="0ddea-229">Här följer några ytterligare resurser för att lära dig mer om hur du arbetar med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="0ddea-229">Here are some additional resources for learning more about working with hello Azure CLI 2.0.</span></span>

* [<span data-ttu-id="0ddea-230">Kom igång med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0ddea-230">Get started with Azure CLI 2.0</span></span>](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [<span data-ttu-id="0ddea-231">Azure CLI 2.0-kommandoreferens</span><span class="sxs-lookup"><span data-stu-id="0ddea-231">Azure CLI 2.0 command reference</span></span>](/cli/azure)
* [<span data-ttu-id="0ddea-232">Azure CLI 2.0 på GitHub</span><span class="sxs-lookup"><span data-stu-id="0ddea-232">Azure CLI 2.0 on GitHub</span></span>](https://github.com/Azure/azure-cli)
