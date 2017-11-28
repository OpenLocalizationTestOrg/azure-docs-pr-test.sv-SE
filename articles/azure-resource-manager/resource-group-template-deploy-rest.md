---
title: aaaDeploy resurser med REST-API och mall | Microsoft Docs
description: "Använd Azure Resource Manager och hanteraren för filserverresurser REST API toodeploy en tooAzure resurser. hello resurser har definierats i en Resource Manager-mall."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: tomfitz
ms.openlocfilehash: 347ad3bdb604429e7291297d448688204af69b46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a><span data-ttu-id="122c1-104">Distribuera resurser med Resource Manager-mallar och Resource Manager REST API</span><span class="sxs-lookup"><span data-stu-id="122c1-104">Deploy resources with Resource Manager templates and Resource Manager REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="122c1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="122c1-105">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="122c1-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="122c1-106">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="122c1-107">Portal</span><span class="sxs-lookup"><span data-stu-id="122c1-107">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="122c1-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="122c1-108">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="122c1-109">Den här artikeln förklarar hur toouse hello Resource Manager REST-API med Resource Manager mallar toodeploy tooAzure dina resurser.</span><span class="sxs-lookup"><span data-stu-id="122c1-109">This article explains how toouse hello Resource Manager REST API with Resource Manager templates toodeploy your resources tooAzure.</span></span>  

> [!TIP]
> <span data-ttu-id="122c1-110">För hjälp med felsökning av ett fel under distributionen, se:</span><span class="sxs-lookup"><span data-stu-id="122c1-110">For help with debugging an error during deployment, see:</span></span>
> 
> * <span data-ttu-id="122c1-111">[Visa distributionsåtgärder](resource-manager-deployment-operations.md) toolearn om att få information som hjälper dig felsöka din fel</span><span class="sxs-lookup"><span data-stu-id="122c1-111">[View deployment operations](resource-manager-deployment-operations.md) toolearn about getting information that helps you troubleshoot your error</span></span>
> * <span data-ttu-id="122c1-112">[Felsök vanliga fel när du distribuerar tooAzure resurser med Azure Resource Manager](resource-manager-common-deployment-errors.md) toolearn hur tooresolve vanliga distributionsfel</span><span class="sxs-lookup"><span data-stu-id="122c1-112">[Troubleshoot common errors when deploying resources tooAzure with Azure Resource Manager](resource-manager-common-deployment-errors.md) toolearn how tooresolve common deployment errors</span></span>
> 
> 

<span data-ttu-id="122c1-113">Mallen kan vara en lokal fil eller en extern fil som är tillgängliga via en URI.</span><span class="sxs-lookup"><span data-stu-id="122c1-113">Your template can be either a local file or an external file that is available through a URI.</span></span> <span data-ttu-id="122c1-114">När mallen finns i ett lagringskonto, kan du begränsa åtkomst toohello mallen och ange en signatur (SAS) delad åtkomst-token under distributionen.</span><span class="sxs-lookup"><span data-stu-id="122c1-114">When your template resides in a storage account, you can restrict access toohello template and provide a shared access signature (SAS) token during deployment.</span></span>

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-hello-rest-api"></a><span data-ttu-id="122c1-115">Distribuera med hello REST API</span><span class="sxs-lookup"><span data-stu-id="122c1-115">Deploy with hello REST API</span></span>
1. <span data-ttu-id="122c1-116">Ange [gemensamma parametrar och rubriker](https://docs.microsoft.com/rest/api/index), inklusive autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="122c1-116">Set [common parameters and headers](https://docs.microsoft.com/rest/api/index), including authentication tokens.</span></span>
2. <span data-ttu-id="122c1-117">Om du inte har en befintlig resursgrupp kan du skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="122c1-117">If you do not have an existing resource group, create a resource group.</span></span> <span data-ttu-id="122c1-118">Ange ditt prenumerations-ID, hello namnet på hello ny resursgrupp och plats som du behöver för din lösning.</span><span class="sxs-lookup"><span data-stu-id="122c1-118">Provide your subscription ID, hello name of hello new resource group, and location that you need for your solution.</span></span> <span data-ttu-id="122c1-119">Mer information finns i [skapa en resursgrupp](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).</span><span class="sxs-lookup"><span data-stu-id="122c1-119">For more information, see [Create a resource group](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).</span></span>
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. <span data-ttu-id="122c1-120">Verifiera distributionen innan du kör den genom att köra hello [validera en malldistribution](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) igen.</span><span class="sxs-lookup"><span data-stu-id="122c1-120">Validate your deployment before executing it by running hello [Validate a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) operation.</span></span> <span data-ttu-id="122c1-121">När du testar hello distribution, ange parametrar exakt samma sätt som när du kör hello-distribution (visas i nästa steg i hello).</span><span class="sxs-lookup"><span data-stu-id="122c1-121">When testing hello deployment, provide parameters exactly as you would when executing hello deployment (shown in hello next step).</span></span>
4. <span data-ttu-id="122c1-122">Skapa en distribution.</span><span class="sxs-lookup"><span data-stu-id="122c1-122">Create a deployment.</span></span> <span data-ttu-id="122c1-123">Ange ditt prenumerations-ID, hello namnet på hello resursgrupp, hello namnet på hello distribution och en länk tooyour mall.</span><span class="sxs-lookup"><span data-stu-id="122c1-123">Provide your subscription ID, hello name of hello resource group, hello name of hello deployment, and a link tooyour template.</span></span> <span data-ttu-id="122c1-124">Information om hello mallfilen finns [parameterfilen](#parameter-file).</span><span class="sxs-lookup"><span data-stu-id="122c1-124">For information about hello template file, see [Parameter file](#parameter-file).</span></span> <span data-ttu-id="122c1-125">Läs mer om hello REST API toocreate en resursgrupp, [skapar en för malldistribution](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate).</span><span class="sxs-lookup"><span data-stu-id="122c1-125">For more information about hello REST API toocreate a resource group, see [Create a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate).</span></span> <span data-ttu-id="122c1-126">Meddelande hello **läge** har angetts för**stegvis**.</span><span class="sxs-lookup"><span data-stu-id="122c1-126">Notice hello **mode** is set too**Incremental**.</span></span> <span data-ttu-id="122c1-127">Ange toorun en fullständig distribution **läge** för**Slutför**.</span><span class="sxs-lookup"><span data-stu-id="122c1-127">toorun a complete deployment, set **mode** too**Complete**.</span></span> <span data-ttu-id="122c1-128">Var försiktig när du använder hello fullständig läget som av misstag kan du ta bort resurser som inte finns i mallen.</span><span class="sxs-lookup"><span data-stu-id="122c1-128">Be careful when using hello complete mode as you can inadvertently delete resources that are not in your template.</span></span>
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0"
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0"
              }
            }
          }
   
      <span data-ttu-id="122c1-129">Även om du vill toolog svar innehåll, begär innehåll eller båda **debugSetting** i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="122c1-129">If you want toolog response content, request content, or both, include **debugSetting** in hello request.</span></span>
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      <span data-ttu-id="122c1-130">Du kan ställa in din lagring konto toouse signatur (SAS) för delade åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="122c1-130">You can set up your storage account toouse a shared access signature (SAS) token.</span></span> <span data-ttu-id="122c1-131">Mer information finns i [delegera åtkomst med en signatur för delad åtkomst](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).</span><span class="sxs-lookup"><span data-stu-id="122c1-131">For more information, see [Delegating Access with a Shared Access Signature](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).</span></span>
5. <span data-ttu-id="122c1-132">Hämta hello status för hello malldistribution.</span><span class="sxs-lookup"><span data-stu-id="122c1-132">Get hello status of hello template deployment.</span></span> <span data-ttu-id="122c1-133">Mer information finns i [få information om en malldistribution](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).</span><span class="sxs-lookup"><span data-stu-id="122c1-133">For more information, see [Get information about a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).</span></span>
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a><span data-ttu-id="122c1-134">Parameterfilen</span><span class="sxs-lookup"><span data-stu-id="122c1-134">Parameter file</span></span>

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a><span data-ttu-id="122c1-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="122c1-135">Next steps</span></span>
* <span data-ttu-id="122c1-136">toolearn om hantering av asynkrona REST-åtgärder, se [spåra asynkrona åtgärder i Azure](resource-manager-async-operations.md).</span><span class="sxs-lookup"><span data-stu-id="122c1-136">toolearn about handling asynchronous REST operations, see [Track asynchronous Azure operations](resource-manager-async-operations.md).</span></span>
* <span data-ttu-id="122c1-137">Ett exempel för att distribuera resurser via hello .NET-klientbibliotek finns [distribuera resurser med hjälp av .NET-bibliotek och en mall](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="122c1-137">For an example of deploying resources through hello .NET client library, see [Deploy resources using .NET libraries and a template](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="122c1-138">toodefine parametrar i mallen, se [Webbsidemallar](resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="122c1-138">toodefine parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).</span></span>
* <span data-ttu-id="122c1-139">Anvisningar om hur du distribuerar din lösning toodifferent miljöer finns [utvecklings- och testmiljöer i Microsoft Azure](solution-dev-test-environments.md).</span><span class="sxs-lookup"><span data-stu-id="122c1-139">For guidance on deploying your solution toodifferent environments, see [Development and test environments in Microsoft Azure](solution-dev-test-environments.md).</span></span>
* <span data-ttu-id="122c1-140">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="122c1-140">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

