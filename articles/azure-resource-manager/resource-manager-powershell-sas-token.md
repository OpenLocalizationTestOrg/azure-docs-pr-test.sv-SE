---
title: Distribuera Azure-mall med SAS-token och PowerShell | Microsoft Docs
description: "Använd Azure Resource Manager och Azure PowerShell för att distribuera resurser till Azure från en mall som är skyddat med SAS-token."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 1e3cea027b599e2b1af1ced0fdf14e2cc8a0db82
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a><span data-ttu-id="c2ba2-103">Distribuera privata Resource Manager-mall med SAS-token och Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c2ba2-103">Deploy private Resource Manager template with SAS token and Azure PowerShell</span></span>

<span data-ttu-id="c2ba2-104">När mallen finns i ett lagringskonto kan du begränsa åtkomsten till mallen och erbjuder en token för delad åtkomst-signatur (SAS) under distributionen.</span><span class="sxs-lookup"><span data-stu-id="c2ba2-104">When your template resides in a storage account, you can restrict access to the template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="c2ba2-105">Det här avsnittet beskriver hur du använder Azure PowerShell med Resource Manager-mallar för att ange en SAS-token under distributionen.</span><span class="sxs-lookup"><span data-stu-id="c2ba2-105">This topic explains how to use Azure PowerShell with Resource Manager templates to provide a SAS token during deployment.</span></span> 

## <a name="add-private-template-to-storage-account"></a><span data-ttu-id="c2ba2-106">Lägg till privata mall i storage-konto</span><span class="sxs-lookup"><span data-stu-id="c2ba2-106">Add private template to storage account</span></span>

<span data-ttu-id="c2ba2-107">Du kan lägga till dina mallar i ett lagringskonto och en länk till dem under distributionen med en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="c2ba2-107">You can add your templates to a storage account and link to them during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c2ba2-108">Blob som innehåller mallen som är tillgänglig för endast kontoägaren genom att följa stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="c2ba2-108">By following the steps below, the blob containing the template is accessible to only the account owner.</span></span> <span data-ttu-id="c2ba2-109">Men när du skapar en SAS-token för blobben är blob tillgänglig för alla med den URI.</span><span class="sxs-lookup"><span data-stu-id="c2ba2-109">However, when you create a SAS token for the blob, the blob is accessible to anyone with that URI.</span></span> <span data-ttu-id="c2ba2-110">Om en annan användare spärras URI: N, har som användare tillgång till mallen.</span><span class="sxs-lookup"><span data-stu-id="c2ba2-110">If another user intercepts the URI, that user is able to access the template.</span></span> <span data-ttu-id="c2ba2-111">Använda en SAS-token är ett bra sätt att begränsa åtkomst till dina mallar bör, men du inte känsliga data som lösenord direkt i mallen.</span><span class="sxs-lookup"><span data-stu-id="c2ba2-111">Using a SAS token is a good way of limiting access to your templates, but you should not include sensitive data like passwords directly in the template.</span></span>
> 
> 

<span data-ttu-id="c2ba2-112">I följande exempel ställer in en behållare för privat lagring-konto och överför en mall:</span><span class="sxs-lookup"><span data-stu-id="c2ba2-112">The following example sets up a private storage account container and uploads a template:</span></span>
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="c2ba2-113">Ange SAS-token under distributionen</span><span class="sxs-lookup"><span data-stu-id="c2ba2-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="c2ba2-114">Generera en SAS-token för att distribuera en privat mallen i ett lagringskonto och inkludera den i URI: N för mallen.</span><span class="sxs-lookup"><span data-stu-id="c2ba2-114">To deploy a private template in a storage account, generate a SAS token and include it in the URI for the template.</span></span> <span data-ttu-id="c2ba2-115">Ange förfallotiden så att tillräckligt med tid att slutföra distributionen.</span><span class="sxs-lookup"><span data-stu-id="c2ba2-115">Set the expiry time to allow enough time to complete the deployment.</span></span>
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get the URI with the SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

<span data-ttu-id="c2ba2-116">Ett exempel på hur du använder en SAS-token med länkade mallar finns [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c2ba2-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="c2ba2-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c2ba2-117">Next steps</span></span>
* <span data-ttu-id="c2ba2-118">En introduktion till att distribuera mallar finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="c2ba2-118">For an introduction to deploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="c2ba2-119">En fullständig exempelskript som distribuerar en mall finns [skript för distribuera Resource Manager-mallar](resource-manager-samples-powershell-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="c2ba2-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-powershell-deploy.md)</span></span>
* <span data-ttu-id="c2ba2-120">Om du vill definiera parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="c2ba2-120">To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="c2ba2-121">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="c2ba2-121">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

