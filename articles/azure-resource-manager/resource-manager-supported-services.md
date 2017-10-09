---
title: aaaAzure resursproviders och resurstyper | Microsoft Docs
description: "Beskriver hello resursproviders som har stöd för hanteraren för filserverresurser, scheman och tillgängliga API-versioner och hello regioner som kan vara värd för hello resurser."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 23db1d3808a20166f3b44ec801e1bcc46fbb9bd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resource-providers-and-types"></a><span data-ttu-id="9ef7d-103">Resursproviders och typer</span><span class="sxs-lookup"><span data-stu-id="9ef7d-103">Resource providers and types</span></span>

<span data-ttu-id="9ef7d-104">När du distribuerar resurser kan behöver du ofta tooretrieve information om hello resursproviders och typer.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-104">When deploying resources, you frequently need tooretrieve information about hello resource providers and types.</span></span> <span data-ttu-id="9ef7d-105">I den här artikeln får du lära dig att:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-105">In this article, you learn to:</span></span>

* <span data-ttu-id="9ef7d-106">Visa alla providrar i Azure</span><span class="sxs-lookup"><span data-stu-id="9ef7d-106">View all resource providers in Azure</span></span>
* <span data-ttu-id="9ef7d-107">Kontrollera registreringsstatus av en resursprovider</span><span class="sxs-lookup"><span data-stu-id="9ef7d-107">Check registration status of a resource provider</span></span>
* <span data-ttu-id="9ef7d-108">En registerresursleverantören</span><span class="sxs-lookup"><span data-stu-id="9ef7d-108">Register a resource provider</span></span>
* <span data-ttu-id="9ef7d-109">Visa resurstyper för en resursprovider</span><span class="sxs-lookup"><span data-stu-id="9ef7d-109">View resource types for a resource provider</span></span>
* <span data-ttu-id="9ef7d-110">Visa giltiga platser för en resurstyp</span><span class="sxs-lookup"><span data-stu-id="9ef7d-110">View valid locations for a resource type</span></span>
* <span data-ttu-id="9ef7d-111">Visa giltiga API-versioner för en resurstyp</span><span class="sxs-lookup"><span data-stu-id="9ef7d-111">View valid API versions for a resource type</span></span>

<span data-ttu-id="9ef7d-112">Du kan utföra dessa steg till hello-portalen, PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-112">You can perform these steps through hello portal, PowerShell, or Azure CLI.</span></span>

## <a name="powershell"></a><span data-ttu-id="9ef7d-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ef7d-113">PowerShell</span></span>

<span data-ttu-id="9ef7d-114">toosee alla providrar i Azure och hello registreringsstatus för din prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-114">toosee all resource providers in Azure, and hello registration status for your subscription, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

<span data-ttu-id="9ef7d-115">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-115">Which returns results similar to:</span></span>

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="9ef7d-116">Registrera en resursleverantör konfigurerar din prenumeration toowork med hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-116">Registering a resource provider configures your subscription toowork with hello resource provider.</span></span> <span data-ttu-id="9ef7d-117">hello omfånget för registrering är alltid hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-117">hello scope for registration is always hello subscription.</span></span> <span data-ttu-id="9ef7d-118">Många resursproviders registreras automatiskt som standard.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-118">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="9ef7d-119">Du kan dock behöva toomanually registrera vissa resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-119">However, you may need toomanually register some resource providers.</span></span> <span data-ttu-id="9ef7d-120">tooregister en resursleverantör, måste du ha behörigheten tooperform hello `/register/action` åtgärden för hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-120">tooregister a resource provider, you must have permission tooperform hello `/register/action` operation for hello resource provider.</span></span> <span data-ttu-id="9ef7d-121">Den här åtgärden finns i hello deltagare och ägare roller.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-121">This operation is included in hello Contributor and Owner roles.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="9ef7d-122">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-122">Which returns results similar to:</span></span>

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

<span data-ttu-id="9ef7d-123">Du kan inte avregistrera en resursleverantör när du har fortfarande resurstyper från resursprovidern i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-123">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="9ef7d-124">toosee information för en viss resurs-provider, Använd:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-124">toosee information for a particular resource provider, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="9ef7d-125">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-125">Which returns results similar to:</span></span>

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

<span data-ttu-id="9ef7d-126">toosee hello resurstyper för en resurs-provider, Använd:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-126">toosee hello resource types for a resource provider, use:</span></span>

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

<span data-ttu-id="9ef7d-127">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-127">Which returns:</span></span>

```powershell
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="9ef7d-128">hello API-versionen motsvarar tooa version av REST API-åtgärder som ges ut av hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-128">hello API version corresponds tooa version of REST API operations that are released by hello resource provider.</span></span> <span data-ttu-id="9ef7d-129">Som en resursleverantör aktiverar nya funktioner, släpper en ny version av hello REST API.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-129">As a resource provider enables new features, it releases a new version of hello REST API.</span></span> 

<span data-ttu-id="9ef7d-130">Använd tooget hello tillgängliga API-versioner för en resurstyp:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-130">tooget hello available API versions for a resource type, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

<span data-ttu-id="9ef7d-131">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-131">Which returns:</span></span>

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="9ef7d-132">Hanteraren för filserverresurser stöds i alla regioner, men hello-resurser som du distribuerar stöds inte i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-132">Resource Manager is supported in all regions, but hello resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="9ef7d-133">Dessutom kan finnas det begränsningar för din prenumeration som hindrar dig från att använda vissa områden som stöder hello resurs.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-133">In addition, there may be limitations on your subscription that prevent you from using some regions that support hello resource.</span></span> 

<span data-ttu-id="9ef7d-134">Använd tooget hello stöds platser för en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-134">tooget hello supported locations for a resource type, use.</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

<span data-ttu-id="9ef7d-135">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-135">Which returns:</span></span>

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a><span data-ttu-id="9ef7d-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="9ef7d-136">Azure CLI</span></span>
<span data-ttu-id="9ef7d-137">toosee alla providrar i Azure och hello registreringsstatus för din prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-137">toosee all resource providers in Azure, and hello registration status for your subscription, use:</span></span>

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

<span data-ttu-id="9ef7d-138">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-138">Which returns results similar to:</span></span>

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="9ef7d-139">Registrera en resursleverantör konfigurerar din prenumeration toowork med hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-139">Registering a resource provider configures your subscription toowork with hello resource provider.</span></span> <span data-ttu-id="9ef7d-140">hello omfånget för registrering är alltid hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-140">hello scope for registration is always hello subscription.</span></span> <span data-ttu-id="9ef7d-141">Många resursproviders registreras automatiskt som standard.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-141">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="9ef7d-142">Du kan dock behöva toomanually registrera vissa resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-142">However, you may need toomanually register some resource providers.</span></span> <span data-ttu-id="9ef7d-143">tooregister en resursleverantör, måste du ha behörigheten tooperform hello `/register/action` åtgärden för hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-143">tooregister a resource provider, you must have permission tooperform hello `/register/action` operation for hello resource provider.</span></span> <span data-ttu-id="9ef7d-144">Den här åtgärden finns i hello deltagare och ägare roller.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-144">This operation is included in hello Contributor and Owner roles.</span></span>

```azurecli
az provider register --namespace Microsoft.Batch
```

<span data-ttu-id="9ef7d-145">Som returnerar ett meddelande som registrering är pågående.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-145">Which returns a message that registration is on-going.</span></span>

<span data-ttu-id="9ef7d-146">Du kan inte avregistrera en resursleverantör när du har fortfarande resurstyper från resursprovidern i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-146">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="9ef7d-147">toosee information för en viss resurs-provider, Använd:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-147">toosee information for a particular resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch
```

<span data-ttu-id="9ef7d-148">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-148">Which returns results similar to:</span></span>

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

<span data-ttu-id="9ef7d-149">toosee hello resurstyper för en resurs-provider, Använd:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-149">toosee hello resource types for a resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

<span data-ttu-id="9ef7d-150">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-150">Which returns:</span></span>

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="9ef7d-151">hello API-versionen motsvarar tooa version av REST API-åtgärder som ges ut av hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-151">hello API version corresponds tooa version of REST API operations that are released by hello resource provider.</span></span> <span data-ttu-id="9ef7d-152">Som en resursleverantör aktiverar nya funktioner, släpper en ny version av hello REST API.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-152">As a resource provider enables new features, it releases a new version of hello REST API.</span></span> 

<span data-ttu-id="9ef7d-153">Använd tooget hello tillgängliga API-versioner för en resurstyp:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-153">tooget hello available API versions for a resource type, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

<span data-ttu-id="9ef7d-154">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-154">Which returns:</span></span>

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="9ef7d-155">Hanteraren för filserverresurser stöds i alla regioner, men hello-resurser som du distribuerar stöds inte i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-155">Resource Manager is supported in all regions, but hello resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="9ef7d-156">Dessutom kan finnas det begränsningar för din prenumeration som hindrar dig från att använda vissa områden som stöder hello resurs.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-156">In addition, there may be limitations on your subscription that prevent you from using some regions that support hello resource.</span></span> 

<span data-ttu-id="9ef7d-157">Använd tooget hello stöds platser för en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-157">tooget hello supported locations for a resource type, use.</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

<span data-ttu-id="9ef7d-158">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="9ef7d-158">Which returns:</span></span>

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a><span data-ttu-id="9ef7d-159">Portalen</span><span class="sxs-lookup"><span data-stu-id="9ef7d-159">Portal</span></span>

<span data-ttu-id="9ef7d-160">toosee alla providrar i Azure och hello registreringsstatus för din prenumeration, Välj **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-160">toosee all resource providers in Azure, and hello registration status for your subscription, select **Subscriptions**.</span></span>

![Välj prenumerationer](./media/resource-manager-supported-services/select-subscriptions.png)

<span data-ttu-id="9ef7d-162">Välj hello prenumeration tooview.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-162">Choose hello subscription tooview.</span></span>

![Ange prenumeration](./media/resource-manager-supported-services/subscription.png)

<span data-ttu-id="9ef7d-164">Välj **resursproviders** och visa hello en lista över tillgängliga resursproviders.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-164">Select **Resource providers** and view hello list of available resource providers.</span></span>

![Visa resursprovidrar](./media/resource-manager-supported-services/show-resource-providers.png)

<span data-ttu-id="9ef7d-166">Registrera en resursleverantör konfigurerar din prenumeration toowork med hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-166">Registering a resource provider configures your subscription toowork with hello resource provider.</span></span> <span data-ttu-id="9ef7d-167">hello omfånget för registrering är alltid hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-167">hello scope for registration is always hello subscription.</span></span> <span data-ttu-id="9ef7d-168">Många resursproviders registreras automatiskt som standard.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-168">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="9ef7d-169">Du kan dock behöva toomanually registrera vissa resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-169">However, you may need toomanually register some resource providers.</span></span> <span data-ttu-id="9ef7d-170">tooregister en resursleverantör, måste du ha behörigheten tooperform hello `/register/action` åtgärden för hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-170">tooregister a resource provider, you must have permission tooperform hello `/register/action` operation for hello resource provider.</span></span> <span data-ttu-id="9ef7d-171">Den här åtgärden finns i hello deltagare och ägare roller.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-171">This operation is included in hello Contributor and Owner roles.</span></span> <span data-ttu-id="9ef7d-172">Välj tooregister en resursleverantör **registrera**.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-172">tooregister a resource provider, select **Register**.</span></span>

![registerresursleverantören](./media/resource-manager-supported-services/register-provider.png)

<span data-ttu-id="9ef7d-174">Du kan inte avregistrera en resursleverantör när du har fortfarande resurstyper från resursprovidern i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-174">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="9ef7d-175">toosee information för en viss resurs-provider, Välj **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-175">toosee information for a particular resource provider, select **More services**.</span></span>

![Välj fler tjänster](./media/resource-manager-supported-services/more-services.png)

<span data-ttu-id="9ef7d-177">Sök efter **Resursläsaren** och välj den hello tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-177">Search for **Resource Explorer** and select it from hello available options.</span></span>

![Välj resursläsaren](./media/resource-manager-supported-services/select-resource-explorer.png)

<span data-ttu-id="9ef7d-179">Välj **Providers**.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-179">Select **Providers**.</span></span>

![Välj providers](./media/resource-manager-supported-services/select-providers.png)

<span data-ttu-id="9ef7d-181">Välj hello resursprovidern och resurs skriver du som du vill tooview.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-181">Select hello resource provider and resource type that you want tooview.</span></span>

![Välj resurstyp](./media/resource-manager-supported-services/select-resource-type.png)

<span data-ttu-id="9ef7d-183">Hanteraren för filserverresurser stöds i alla regioner, men hello-resurser som du distribuerar stöds inte i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-183">Resource Manager is supported in all regions, but hello resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="9ef7d-184">Dessutom kan finnas det begränsningar för din prenumeration som hindrar dig från att använda vissa områden som stöder hello resurs.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-184">In addition, there may be limitations on your subscription that prevent you from using some regions that support hello resource.</span></span> <span data-ttu-id="9ef7d-185">Hej resursläsaren visar giltiga platser för hello resurstypen.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-185">hello resource explorer displays valid locations for hello resource type.</span></span>

![Visa platser](./media/resource-manager-supported-services/show-locations.png)

<span data-ttu-id="9ef7d-187">hello API-versionen motsvarar tooa version av REST API-åtgärder som ges ut av hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-187">hello API version corresponds tooa version of REST API operations that are released by hello resource provider.</span></span> <span data-ttu-id="9ef7d-188">Som en resursleverantör aktiverar nya funktioner, släpper en ny version av hello REST API.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-188">As a resource provider enables new features, it releases a new version of hello REST API.</span></span> <span data-ttu-id="9ef7d-189">Hej resursläsaren visar giltiga API-versioner för hello resurstypen.</span><span class="sxs-lookup"><span data-stu-id="9ef7d-189">hello resource explorer displays valid API versions for hello resource type.</span></span>

![Visa API-versioner](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a><span data-ttu-id="9ef7d-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ef7d-191">Next steps</span></span>
* <span data-ttu-id="9ef7d-192">toolearn om hur du skapar Resource Manager-mallar finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9ef7d-192">toolearn about creating Resource Manager templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="9ef7d-193">toolearn om hur du distribuerar resurser, se [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="9ef7d-193">toolearn about deploying resources, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="9ef7d-194">tooview hello åtgärder för en resursleverantör finns [Azure REST API](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="9ef7d-194">tooview hello operations for a resource provider, see [Azure REST API](/rest/api/).</span></span>

