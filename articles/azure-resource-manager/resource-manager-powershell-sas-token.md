---
title: aaaDeploy Azure-mall med SAS-token och PowerShell | Microsoft Docs
description: "Använda Azure Resource Manager och Azure PowerShell toodeploy resurser tooAzure från en mall som är skyddat med SAS-token."
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
ms.openlocfilehash: b95e096591d6213f8ef79235c8cd85705c4b79ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a><span data-ttu-id="080e1-103">Distribuera privata Resource Manager-mall med SAS-token och Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="080e1-103">Deploy private Resource Manager template with SAS token and Azure PowerShell</span></span>

<span data-ttu-id="080e1-104">När mallen finns i ett lagringskonto, kan du begränsa åtkomst toohello mallen och ange en signatur (SAS) delad åtkomst-token under distributionen.</span><span class="sxs-lookup"><span data-stu-id="080e1-104">When your template resides in a storage account, you can restrict access toohello template and provide a shared access signature (SAS) token during deployment.</span></span> <span data-ttu-id="080e1-105">Det här avsnittet beskrivs hur toouse Azure PowerShell med Resource Manager mallar tooprovide en SAS-token under distributionen.</span><span class="sxs-lookup"><span data-stu-id="080e1-105">This topic explains how toouse Azure PowerShell with Resource Manager templates tooprovide a SAS token during deployment.</span></span> 

## <a name="add-private-template-toostorage-account"></a><span data-ttu-id="080e1-106">Lägga till privata mallen toostorage konto</span><span class="sxs-lookup"><span data-stu-id="080e1-106">Add private template toostorage account</span></span>

<span data-ttu-id="080e1-107">Du kan lägga till mallar tooa storage-konto och länka toothem under distributionen med en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="080e1-107">You can add your templates tooa storage account and link toothem during deployment with a SAS token.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="080e1-108">Genom att följa hello stegen nedan är hello blob som innehåller hello mall tillgänglig tooonly hello Kontoägare.</span><span class="sxs-lookup"><span data-stu-id="080e1-108">By following hello steps below, hello blob containing hello template is accessible tooonly hello account owner.</span></span> <span data-ttu-id="080e1-109">När du skapar en SAS-token för hello blob är hello blob tillgänglig tooanyone med den URI.</span><span class="sxs-lookup"><span data-stu-id="080e1-109">However, when you create a SAS token for hello blob, hello blob is accessible tooanyone with that URI.</span></span> <span data-ttu-id="080e1-110">Om en annan användare spärras hello URI, är den användaren kan tooaccess hello-mallen.</span><span class="sxs-lookup"><span data-stu-id="080e1-110">If another user intercepts hello URI, that user is able tooaccess hello template.</span></span> <span data-ttu-id="080e1-111">Med hjälp av en SAS-token är ett bra sätt för att begränsa åtkomst tooyour mallar bör, men du inte känsliga data som lösenord direkt i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="080e1-111">Using a SAS token is a good way of limiting access tooyour templates, but you should not include sensitive data like passwords directly in hello template.</span></span>
> 
> 

<span data-ttu-id="080e1-112">hello följande exempel ställer in en behållare för privat lagring-konto och överför en mall:</span><span class="sxs-lookup"><span data-stu-id="080e1-112">hello following example sets up a private storage account container and uploads a template:</span></span>
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a><span data-ttu-id="080e1-113">Ange SAS-token under distributionen</span><span class="sxs-lookup"><span data-stu-id="080e1-113">Provide SAS token during deployment</span></span>
<span data-ttu-id="080e1-114">toodeploy en privat mall i ett lagringskonto genererar en SAS-token och inkludera den i hello URI för hello mall.</span><span class="sxs-lookup"><span data-stu-id="080e1-114">toodeploy a private template in a storage account, generate a SAS token and include it in hello URI for hello template.</span></span> <span data-ttu-id="080e1-115">Ange hello utgången tid tooallow tillräckligt med tid toocomplete hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="080e1-115">Set hello expiry time tooallow enough time toocomplete hello deployment.</span></span>
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get hello URI with hello SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

<span data-ttu-id="080e1-116">Ett exempel på hur du använder en SAS-token med länkade mallar finns [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="080e1-116">For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="080e1-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="080e1-117">Next steps</span></span>
* <span data-ttu-id="080e1-118">En introduktion toodeploying mallar, se [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="080e1-118">For an introduction toodeploying templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="080e1-119">En fullständig exempelskript som distribuerar en mall finns [skript för distribuera Resource Manager-mallar](resource-manager-samples-powershell-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="080e1-119">For a complete sample script that deploys a template, see [Deploy Resource Manager template script](resource-manager-samples-powershell-deploy.md)</span></span>
* <span data-ttu-id="080e1-120">toodefine parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="080e1-120">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="080e1-121">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="080e1-121">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

