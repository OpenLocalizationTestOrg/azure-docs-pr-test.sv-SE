---
title: aaaDeploy Azure-mall med SAS-token och Azure CLI | Microsoft Docs
description: "Använda Azure Resource Manager och Azure CLI toodeploy resurser tooAzure från en mall som är skyddat med SAS-token."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 59c64616d6e1f5e456d88a72854d0ed99e1bdc0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a><span data-ttu-id="1960a-103">Distribuera privata Resource Manager-mall med SAS-token och Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1960a-103">Deploy private Resource Manager template with SAS token and Azure CLI</span></span>

<span data-ttu-id="1960a-104">När mallen finns i ett lagringskonto, kan du begränsa åtkomst toohello mallen och ange en signatur (SAS) delad åtkomst-token under distributionen.</span><span class="sxs-lookup"><span data-stu-id="1960a-104">When your template resides in a storage account, you can restrict access toohello template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="1960a-105">Det här avsnittet beskrivs hur toouse Azure PowerShell med Resource Manager mallar tooprovide en SAS-token under distributionen.</span><span class="sxs-lookup"><span data-stu-id="1960a-105">This topic explains how toouse Azure PowerShell with Resource Manager templates tooprovide a SAS token during deployment.</span></span> 

## <a name="add-private-template-toostorage-account"></a><span data-ttu-id="1960a-106">Lägga till privata mallen toostorage konto</span><span class="sxs-lookup"><span data-stu-id="1960a-106">Add private template toostorage account</span></span>

<span data-ttu-id="1960a-107">Du kan lägga till mallar tooa storage-konto och länka toothem under distributionen med en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="1960a-107">You can add your templates tooa storage account and link toothem during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1960a-108">Genom att följa hello stegen nedan är hello blob som innehåller hello mall tillgänglig tooonly hello Kontoägare.</span><span class="sxs-lookup"><span data-stu-id="1960a-108">By following hello steps below, hello blob containing hello template is accessible tooonly hello account owner.</span></span> <span data-ttu-id="1960a-109">När du skapar en SAS-token för hello blob är hello blob tillgänglig tooanyone med den URI.</span><span class="sxs-lookup"><span data-stu-id="1960a-109">However, when you create a SAS token for hello blob, hello blob is accessible tooanyone with that URI.</span></span> <span data-ttu-id="1960a-110">Om en annan användare spärras hello URI, är den användaren kan tooaccess hello-mallen.</span><span class="sxs-lookup"><span data-stu-id="1960a-110">If another user intercepts hello URI, that user is able tooaccess hello template.</span></span> <span data-ttu-id="1960a-111">Med hjälp av en SAS-token är ett bra sätt för att begränsa åtkomst tooyour mallar bör, men du inte känsliga data som lösenord direkt i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="1960a-111">Using a SAS token is a good way of limiting access tooyour templates, but you should not include sensitive data like passwords directly in hello template.</span></span>
> 
> 

<span data-ttu-id="1960a-112">hello följande exempel ställer in en behållare för privat lagring-konto och överför en mall:</span><span class="sxs-lookup"><span data-stu-id="1960a-112">hello following example sets up a private storage account container and uploads a template:</span></span>
   
```azurecli
az group create --name "ManageGroup" --location "South Central US"
az storage account create \
    --resource-group ManageGroup \
    --location "South Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
az storage blob upload \
    --container-name templates \
    --file vmlinux.json \
    --name vmlinux.json \
    --connection-string $connection
```

### <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="1960a-113">Ange SAS-token under distributionen</span><span class="sxs-lookup"><span data-stu-id="1960a-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="1960a-114">toodeploy en privat mall i ett lagringskonto genererar en SAS-token och inkludera den i hello URI för hello mall.</span><span class="sxs-lookup"><span data-stu-id="1960a-114">toodeploy a private template in a storage account, generate a SAS token and include it in hello URI for hello template.</span></span> <span data-ttu-id="1960a-115">Ange hello utgången tid tooallow tillräckligt med tid toocomplete hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="1960a-115">Set hello expiry time tooallow enough time toocomplete hello deployment.</span></span>
   
```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name vmlinux.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name vmlinux.json \
    --output tsv \
    --connection-string $connection)
az group deployment create --resource-group ExampleGroup --template-uri $url?$token
```

<span data-ttu-id="1960a-116">Ett exempel på hur du använder en SAS-token med länkade mallar finns [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1960a-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1960a-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1960a-117">Next steps</span></span>
* <span data-ttu-id="1960a-118">En introduktion toodeploying mallar, se [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="1960a-118">For an introduction toodeploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy-cli.md).</span></span>
* <span data-ttu-id="1960a-119">En fullständig exempelskript som distribuerar en mall finns [skript för distribuera Resource Manager-mallar](resource-manager-samples-cli-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="1960a-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-cli-deploy.md)</span></span>
* <span data-ttu-id="1960a-120">toodefine parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="1960a-120">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="1960a-121">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="1960a-121">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
