---
title: "Azure CLI skript exempel - lägga till ett program i Batch | Microsoft Docs"
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
ms.openlocfilehash: 5d057eaf32867aedc95d58c5185e2be1f9385ec0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="adding-applications-to-azure-batch-with-azure-cli"></a><span data-ttu-id="5213b-103">Lägga till program till Azure Batch med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5213b-103">Adding applications to Azure Batch with Azure CLI</span></span>

<span data-ttu-id="5213b-104">Det här skriptet visar hur du konfigurerar ett program för användning med en Azure Batch-pool eller en uppgift.</span><span class="sxs-lookup"><span data-stu-id="5213b-104">This script demonstrates how to set up an application for use with an Azure Batch pool or task.</span></span> <span data-ttu-id="5213b-105">Paketera din körbar fil, tillsammans med eventuella beroenden till en ZIP-fil om du vill konfigurera ett program.</span><span class="sxs-lookup"><span data-stu-id="5213b-105">To set up an application, package your executable, together with any dependencies, into a .zip file.</span></span> <span data-ttu-id="5213b-106">I det här exemplet kallas den körbara zip-filen ' min-program-exe.zip'.</span><span class="sxs-lookup"><span data-stu-id="5213b-106">In this example the executable zip file is called 'my-application-exe.zip'.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5213b-107">Krav</span><span class="sxs-lookup"><span data-stu-id="5213b-107">Prerequisites</span></span>

- <span data-ttu-id="5213b-108">Installera Azure CLI med hjälp av instruktionerna i den [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="5213b-108">Install the Azure CLI using the instructions provided in the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli), if you have not already done so.</span></span>
- <span data-ttu-id="5213b-109">Skapa ett Batch-konto om du inte redan har ett.</span><span class="sxs-lookup"><span data-stu-id="5213b-109">Create a Batch account if you don't already have one.</span></span> <span data-ttu-id="5213b-110">Se [skapa ett batchkonto med Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) för ett exempelskript som skapar ett konto.</span><span class="sxs-lookup"><span data-stu-id="5213b-110">See [Create a Batch account with the Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) for a sample script that creates an account.</span></span>

## <a name="sample-script"></a><span data-ttu-id="5213b-111">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="5213b-111">Sample script</span></span>

<span data-ttu-id="5213b-112">[!code-azurecli[huvudsakliga](../../../cli_scripts/batch/add-application/add-application.sh "Lägg till program")]</span><span class="sxs-lookup"><span data-stu-id="5213b-112">[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]</span></span>

## <a name="clean-up-application"></a><span data-ttu-id="5213b-113">Rensa program</span><span class="sxs-lookup"><span data-stu-id="5213b-113">Clean up application</span></span>

<span data-ttu-id="5213b-114">När du har kört ovan exempelskript kör du följande kommandon för att ta bort programmet och alla dess överförda programpaket.</span><span class="sxs-lookup"><span data-stu-id="5213b-114">After you run the above sample script, run the following commands to remove the application and all of its uploaded application packages.</span></span>

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a><span data-ttu-id="5213b-115">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="5213b-115">Script explanation</span></span>

<span data-ttu-id="5213b-116">Det här skriptet använder följande kommandon för att skapa ett program och ladda upp ett programpaket.</span><span class="sxs-lookup"><span data-stu-id="5213b-116">This script uses the following commands to create an application and upload an application package.</span></span>
<span data-ttu-id="5213b-117">Varje kommando i tabellen länkar till kommando-specifik dokumentation.</span><span class="sxs-lookup"><span data-stu-id="5213b-117">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="5213b-118">Kommando</span><span class="sxs-lookup"><span data-stu-id="5213b-118">Command</span></span> | <span data-ttu-id="5213b-119">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="5213b-119">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5213b-120">Skapa AZ batch-program</span><span class="sxs-lookup"><span data-stu-id="5213b-120">az batch application create</span></span>](https://docs.microsoft.com/cli/azure/batch/application#create) | <span data-ttu-id="5213b-121">Skapar ett program.</span><span class="sxs-lookup"><span data-stu-id="5213b-121">Creates an application.</span></span>  |
| [<span data-ttu-id="5213b-122">AZ batch programmet uppsättningen</span><span class="sxs-lookup"><span data-stu-id="5213b-122">az batch application set</span></span>](https://docs.microsoft.com/cli/azure/batch/application#set) | <span data-ttu-id="5213b-123">Uppdaterar egenskaperna för ett program.</span><span class="sxs-lookup"><span data-stu-id="5213b-123">Updates properties of an application.</span></span>  |
| [<span data-ttu-id="5213b-124">Skapa AZ batch-programpaket</span><span class="sxs-lookup"><span data-stu-id="5213b-124">az batch application package create</span></span>](https://docs.microsoft.com/cli/azure/batch/application/package#create) | <span data-ttu-id="5213b-125">Lägger till ett programpaket till det angivna programmet.</span><span class="sxs-lookup"><span data-stu-id="5213b-125">Adds an application package to the specified application.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="5213b-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5213b-126">Next steps</span></span>

<span data-ttu-id="5213b-127">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5213b-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5213b-128">Ytterligare Batch CLI skriptexempel finns i den [Azure Batch CLI dokumentationen](../batch-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5213b-128">Additional Batch CLI script samples can be found in the [Azure Batch CLI documentation](../batch-cli-samples.md).</span></span>
