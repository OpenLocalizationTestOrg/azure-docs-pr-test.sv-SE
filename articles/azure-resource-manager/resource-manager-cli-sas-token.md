---
title: Distribuera Azure-mall med SAS-token och Azure CLI | Microsoft Docs
description: "Använd Azure Resource Manager och Azure CLI för att distribuera resurser till Azure från en mall som är skyddat med SAS-token."
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
ms.openlocfilehash: 22387aadd8f53a65efb76a29a9403c46a2c25954
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a><span data-ttu-id="0572c-103">Distribuera privata Resource Manager-mall med SAS-token och Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0572c-103">Deploy private Resource Manager template with SAS token and Azure CLI</span></span>

<span data-ttu-id="0572c-104">När mallen finns i ett lagringskonto kan du begränsa åtkomsten till mallen och erbjuder en token för delad åtkomst-signatur (SAS) under distributionen.</span><span class="sxs-lookup"><span data-stu-id="0572c-104">When your template resides in a storage account, you can restrict access to the template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="0572c-105">Det här avsnittet beskriver hur du använder Azure PowerShell med Resource Manager-mallar för att ange en SAS-token under distributionen.</span><span class="sxs-lookup"><span data-stu-id="0572c-105">This topic explains how to use Azure PowerShell with Resource Manager templates to provide a SAS token during deployment.</span></span> 

## <a name="add-private-template-to-storage-account"></a><span data-ttu-id="0572c-106">Lägg till privata mall i storage-konto</span><span class="sxs-lookup"><span data-stu-id="0572c-106">Add private template to storage account</span></span>

<span data-ttu-id="0572c-107">Du kan lägga till dina mallar i ett lagringskonto och en länk till dem under distributionen med en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="0572c-107">You can add your templates to a storage account and link to them during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0572c-108">Blob som innehåller mallen som är tillgänglig för endast kontoägaren genom att följa stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="0572c-108">By following the steps below, the blob containing the template is accessible to only the account owner.</span></span> <span data-ttu-id="0572c-109">Men när du skapar en SAS-token för blobben är blob tillgänglig för alla med den URI.</span><span class="sxs-lookup"><span data-stu-id="0572c-109">However, when you create a SAS token for the blob, the blob is accessible to anyone with that URI.</span></span> <span data-ttu-id="0572c-110">Om en annan användare spärras URI: N, har som användare tillgång till mallen.</span><span class="sxs-lookup"><span data-stu-id="0572c-110">If another user intercepts the URI, that user is able to access the template.</span></span> <span data-ttu-id="0572c-111">Använda en SAS-token är ett bra sätt att begränsa åtkomst till dina mallar bör, men du inte känsliga data som lösenord direkt i mallen.</span><span class="sxs-lookup"><span data-stu-id="0572c-111">Using a SAS token is a good way of limiting access to your templates, but you should not include sensitive data like passwords directly in the template.</span></span>
> 
> 

<span data-ttu-id="0572c-112">I följande exempel ställer in en behållare för privat lagring-konto och överför en mall:</span><span class="sxs-lookup"><span data-stu-id="0572c-112">The following example sets up a private storage account container and uploads a template:</span></span>
   
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

### <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="0572c-113">Ange SAS-token under distributionen</span><span class="sxs-lookup"><span data-stu-id="0572c-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="0572c-114">Generera en SAS-token för att distribuera en privat mallen i ett lagringskonto och inkludera den i URI: N för mallen.</span><span class="sxs-lookup"><span data-stu-id="0572c-114">To deploy a private template in a storage account, generate a SAS token and include it in the URI for the template.</span></span> <span data-ttu-id="0572c-115">Ange förfallotiden så att tillräckligt med tid att slutföra distributionen.</span><span class="sxs-lookup"><span data-stu-id="0572c-115">Set the expiry time to allow enough time to complete the deployment.</span></span>
   
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

<span data-ttu-id="0572c-116">Ett exempel på hur du använder en SAS-token med länkade mallar finns [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="0572c-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0572c-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0572c-117">Next steps</span></span>
* <span data-ttu-id="0572c-118">En introduktion till att distribuera mallar finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="0572c-118">For an introduction to deploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy-cli.md).</span></span>
* <span data-ttu-id="0572c-119">En fullständig exempelskript som distribuerar en mall finns [skript för distribuera Resource Manager-mallar](resource-manager-samples-cli-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="0572c-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-cli-deploy.md)</span></span>
* <span data-ttu-id="0572c-120">Om du vill definiera parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="0572c-120">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="0572c-121">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="0572c-121">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
