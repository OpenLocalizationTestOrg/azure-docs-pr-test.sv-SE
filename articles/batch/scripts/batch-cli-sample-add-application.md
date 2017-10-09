---
title: "aaaAzure CLI skriptexempel - lägga till ett program i Batch | Microsoft Docs"
description: "Azure CLI skript exempel - lägga till ett program i Batch"
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
ms.openlocfilehash: cb33b3a7b30610011b19954a987995cc5f0257c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-applications-tooazure-batch-with-azure-cli"></a><span data-ttu-id="93289-103">Lägga till program tooAzure Batch med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="93289-103">Adding applications tooAzure Batch with Azure CLI</span></span>

<span data-ttu-id="93289-104">Det här skriptet visar hur tooset skapat ett program för användning med en Azure Batch-pool eller en uppgift.</span><span class="sxs-lookup"><span data-stu-id="93289-104">This script demonstrates how tooset up an application for use with an Azure Batch pool or task.</span></span> <span data-ttu-id="93289-105">tooset upp ett program, paketera din körbar fil, tillsammans med eventuella beroenden till en ZIP-fil.</span><span class="sxs-lookup"><span data-stu-id="93289-105">tooset up an application, package your executable, together with any dependencies, into a .zip file.</span></span> <span data-ttu-id="93289-106">I det här exemplet hello körbara zip-fil som kallas ”min-program-exe.zip'.</span><span class="sxs-lookup"><span data-stu-id="93289-106">In this example hello executable zip file is called 'my-application-exe.zip'.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93289-107">Krav</span><span class="sxs-lookup"><span data-stu-id="93289-107">Prerequisites</span></span>

- <span data-ttu-id="93289-108">Installera hello Azure CLI med hjälp av anvisningarna i hello hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="93289-108">Install hello Azure CLI using hello instructions provided in hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="93289-109">Skapa ett Batch-konto om du inte redan har ett.</span><span class="sxs-lookup"><span data-stu-id="93289-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="93289-110">Se [skapa ett batchkonto med hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) för ett exempelskript som skapar ett konto.</span><span class="sxs-lookup"><span data-stu-id="93289-110">See [Create a Batch account with hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>

## <a name="sample-script"></a><span data-ttu-id="93289-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="93289-111">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a><span data-ttu-id="93289-112">Rensa program</span><span class="sxs-lookup"><span data-stu-id="93289-112">Clean up application</span></span>

<span data-ttu-id="93289-113">När du har kört hello ovan exempelskript kör du följande kommandon tooremove hello programmet och alla dess överförda programpaket.</span><span class="sxs-lookup"><span data-stu-id="93289-113">After you run hello above sample script, run hello following commands tooremove the application and all of its uploaded application packages.</span></span>

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a><span data-ttu-id="93289-114">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="93289-114">Script explanation</span></span>

<span data-ttu-id="93289-115">Det här skriptet använder följande kommandon toocreate hello ett program och ladda upp ett programpaket.</span><span class="sxs-lookup"><span data-stu-id="93289-115">This script uses hello following commands toocreate an application and upload an application package.</span></span>
<span data-ttu-id="93289-116">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="93289-116">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="93289-117">Kommando</span><span class="sxs-lookup"><span data-stu-id="93289-117">Command</span></span> | <span data-ttu-id="93289-118">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="93289-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="93289-119">Skapa AZ batch-program</span><span class="sxs-lookup"><span data-stu-id="93289-119">az batch application create</span></span>](https://docs.microsoft.com/cli/azure/batch/application#create) | <span data-ttu-id="93289-120">Skapar ett program.</span><span class="sxs-lookup"><span data-stu-id="93289-120">Creates an application.</span></span>  |
| [<span data-ttu-id="93289-121">AZ batch programmet uppsättningen</span><span class="sxs-lookup"><span data-stu-id="93289-121">az batch application set</span></span>](https://docs.microsoft.com/cli/azure/batch/application#set) | <span data-ttu-id="93289-122">Uppdaterar egenskaperna för ett program.</span><span class="sxs-lookup"><span data-stu-id="93289-122">Updates properties of an application.</span></span>  |
| [<span data-ttu-id="93289-123">Skapa AZ batch-programpaket</span><span class="sxs-lookup"><span data-stu-id="93289-123">az batch application package create</span></span>](https://docs.microsoft.com/cli/azure/batch/application/package#create) | <span data-ttu-id="93289-124">Lägger till ett program paketet toohello angivna program.</span><span class="sxs-lookup"><span data-stu-id="93289-124">Adds an application package toohello specified application.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="93289-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93289-125">Next steps</span></span>

<span data-ttu-id="93289-126">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="93289-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="93289-127">Ytterligare Batch CLI skriptexempel finns i hello [Azure Batch CLI dokumentationen](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="93289-127">Additional Batch CLI script samples can be found in hello [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
