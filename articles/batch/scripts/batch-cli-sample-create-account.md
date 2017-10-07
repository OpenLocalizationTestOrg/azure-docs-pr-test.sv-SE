---
title: aaaAzure CLI skriptexempel - skapa ett Batch-konto | Microsoft Docs
description: "Azure CLI-skript exempel – skapa ett Batch-konto"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: 62b640eebbafdd1081822a638fd0720121ef067a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-batch-account-with-hello-azure-cli"></a><span data-ttu-id="5b1d5-103">Skapa ett batchkonto med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5b1d5-103">Create a Batch account with hello Azure CLI</span></span>

<span data-ttu-id="5b1d5-104">Det här skriptet skapar ett Azure Batch-konto och visar hur olika egenskaper för hello-konto kan efterfrågas och uppdateras.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-104">This script creates an Azure Batch account and shows how various properties of hello account can be queried and updated.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b1d5-105">Krav</span><span class="sxs-lookup"><span data-stu-id="5b1d5-105">Prerequisites</span></span>

<span data-ttu-id="5b1d5-106">Installera hello Azure CLI med hjälp av anvisningarna i hello hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-106">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>

## <a name="batch-account-sample-script"></a><span data-ttu-id="5b1d5-107">Exempelskript för batch-konto</span><span class="sxs-lookup"><span data-stu-id="5b1d5-107">Batch account sample script</span></span>

<span data-ttu-id="5b1d5-108">När du skapar en Batch-kontot tilldelas som standard dess datornoderna internt av hello Batch-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-108">When you create a Batch account, by default its compute nodes are assigned internally by hello Batch service.</span></span> <span data-ttu-id="5b1d5-109">Allokerade datornoderna blir ämne tooa separat kärnkvot och hello kan autentiseras antingen via autentiseringsuppgifterna för delad nyckel eller ett Azure Active Dirctory-token.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-109">Allocated compute nodes will be subject tooa separate core quota and hello account can be authenticated either via Shared Key credentials or an Azure Active Dirctory token.</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account.sh "Create Account")]

## <a name="batch-account-using-user-subscription-sample-script"></a><span data-ttu-id="5b1d5-110">Batch-kontot med användaren exempelskript för prenumeration</span><span class="sxs-lookup"><span data-stu-id="5b1d5-110">Batch account using user subscription sample script</span></span>

<span data-ttu-id="5b1d5-111">Du kan också välja toohave Batch skapa dess compute-noder i din egen Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-111">You can also opt toohave Batch create its compute nodes in your own Azure subscription.</span></span>
<span data-ttu-id="5b1d5-112">Konton som allokerar beräkna noder till din prenumeration måste autentiseras via Azure Active Directory-token och hello datornoderna allokerade räknas som en prenumerationens kvoter.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-112">Accounts that allocate compute nodes into your subscription must be authenticated via an Azure Active Directory token and hello compute nodes allocated will count towards your subscription quota.</span></span> <span data-ttu-id="5b1d5-113">toocreate ett konto i det här läget, en måste ange en Key Vault-referens när du skapar hello-konto.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-113">toocreate an account in this mode, one must specify a Key Vault reference when creating hello account.</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/create-account/create-account-user-subscription.sh  "Create Account using User Subscription")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5b1d5-114">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="5b1d5-114">Clean up deployment</span></span>

<span data-ttu-id="5b1d5-115">När du har kört antingen hello ovan exempelskript kör hello efter kommandot tooremove resursgruppen och alla relaterade resurser (inklusive Batch-konton, Azure Storage-konton och Azure nyckelvalv).</span><span class="sxs-lookup"><span data-stu-id="5b1d5-115">After you run either of hello above sample scripts, run hello following command tooremove the resource group and all related resources (including Batch accounts, Azure Storage accounts and Azure key vaults).</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="5b1d5-116">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="5b1d5-116">Script explanation</span></span>

<span data-ttu-id="5b1d5-117">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, Batch-kontot och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-117">This script uses hello following commands toocreate a resource group, Batch account, and all related resources.</span></span> <span data-ttu-id="5b1d5-118">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-118">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="5b1d5-119">Kommando</span><span class="sxs-lookup"><span data-stu-id="5b1d5-119">Command</span></span> | <span data-ttu-id="5b1d5-120">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="5b1d5-120">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5b1d5-121">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="5b1d5-121">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5b1d5-122">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-122">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5b1d5-123">Skapa AZ batch-kontot</span><span class="sxs-lookup"><span data-stu-id="5b1d5-123">az batch account create</span></span>](https://docs.microsoft.com/cli/azure/batch/account#create) | <span data-ttu-id="5b1d5-124">Skapar hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-124">Creates hello Batch account.</span></span>  |
| [<span data-ttu-id="5b1d5-125">AZ batch inställt</span><span class="sxs-lookup"><span data-stu-id="5b1d5-125">az batch account set</span></span>](https://docs.microsoft.com/cli/azure/batch/account#set) | <span data-ttu-id="5b1d5-126">Uppdaterar egenskaperna för hello Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-126">Updates properties of hello Batch account.</span></span>  |
| [<span data-ttu-id="5b1d5-127">Visa för AZ batch-konto</span><span class="sxs-lookup"><span data-stu-id="5b1d5-127">az batch account show</span></span>](https://docs.microsoft.com/cli/azure/batch/account#show) | <span data-ttu-id="5b1d5-128">Hämtar information om hello angetts Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-128">Retrieves details of hello specified Batch account.</span></span>  |
| [<span data-ttu-id="5b1d5-129">AZ batch-kontot nycklar lista</span><span class="sxs-lookup"><span data-stu-id="5b1d5-129">az batch account keys list</span></span>](https://docs.microsoft.com/cli/azure/batch/account/keys#list) | <span data-ttu-id="5b1d5-130">Hämtar hello snabbtangenter för hello angetts Batch-kontot.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-130">Retrieves hello access keys of hello specified Batch account.</span></span>  |
| [<span data-ttu-id="5b1d5-131">logga in AZ batch-konto</span><span class="sxs-lookup"><span data-stu-id="5b1d5-131">az batch account login</span></span>](https://docs.microsoft.com/cli/azure/batch/account#login) | <span data-ttu-id="5b1d5-132">Autentiseras mot hello angetts Batch-kontot för ytterligare CLI interaktion.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-132">Authenticates against hello specified Batch account for further CLI interaction.</span></span>  |
| [<span data-ttu-id="5b1d5-133">Skapa AZ storage-konto</span><span class="sxs-lookup"><span data-stu-id="5b1d5-133">az storage account create</span></span>](https://docs.microsoft.com/cli/azure/storage/account#create) | <span data-ttu-id="5b1d5-134">Skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-134">Creates a storage account.</span></span> |
| [<span data-ttu-id="5b1d5-135">Skapa AZ keyvault</span><span class="sxs-lookup"><span data-stu-id="5b1d5-135">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="5b1d5-136">Skapar ett nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-136">Creates a key vault.</span></span> |
| [<span data-ttu-id="5b1d5-137">Ange en AZ keyvault-princip</span><span class="sxs-lookup"><span data-stu-id="5b1d5-137">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="5b1d5-138">Uppdatera hello säkerhetsprincipen för hello angivna nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-138">Update hello security policy of hello specified key vault.</span></span> |
| [<span data-ttu-id="5b1d5-139">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="5b1d5-139">az group delete</span></span>](https://docs.microsoft.com/cli/azure/group#delete) | <span data-ttu-id="5b1d5-140">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="5b1d5-140">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5b1d5-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b1d5-141">Next steps</span></span>

<span data-ttu-id="5b1d5-142">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5b1d5-142">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5b1d5-143">Ytterligare Batch CLI skriptexempel finns i hello [Azure Batch CLI dokumentationen](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5b1d5-143">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
