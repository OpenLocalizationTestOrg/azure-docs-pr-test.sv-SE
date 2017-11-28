---
title: Azure-resurs-providers och resurstyper | Microsoft Docs
description: "Beskriver resursleverantörer som har stöd för hanteraren för filserverresurser, deras scheman och tillgängliga API-versioner och regioner som kan vara värd för resurser."
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
ms.openlocfilehash: 6a9128f45d4199404019cee594842d59c7f1aaf3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="resource-providers-and-types"></a><span data-ttu-id="5f067-103">Resursproviders och typer</span><span class="sxs-lookup"><span data-stu-id="5f067-103">Resource providers and types</span></span>

<span data-ttu-id="5f067-104">När du distribuerar resurser kan behöver du ofta hämta information om resursproviders och typer.</span><span class="sxs-lookup"><span data-stu-id="5f067-104">When deploying resources, you frequently need to retrieve information about the resource providers and types.</span></span> <span data-ttu-id="5f067-105">I den här artikeln får du lära dig att:</span><span class="sxs-lookup"><span data-stu-id="5f067-105">In this article, you learn to:</span></span>

* <span data-ttu-id="5f067-106">Visa alla providrar i Azure</span><span class="sxs-lookup"><span data-stu-id="5f067-106">View all resource providers in Azure</span></span>
* <span data-ttu-id="5f067-107">Kontrollera registreringsstatus av en resursprovider</span><span class="sxs-lookup"><span data-stu-id="5f067-107">Check registration status of a resource provider</span></span>
* <span data-ttu-id="5f067-108">En registerresursleverantören</span><span class="sxs-lookup"><span data-stu-id="5f067-108">Register a resource provider</span></span>
* <span data-ttu-id="5f067-109">Visa resurstyper för en resursprovider</span><span class="sxs-lookup"><span data-stu-id="5f067-109">View resource types for a resource provider</span></span>
* <span data-ttu-id="5f067-110">Visa giltiga platser för en resurstyp</span><span class="sxs-lookup"><span data-stu-id="5f067-110">View valid locations for a resource type</span></span>
* <span data-ttu-id="5f067-111">Visa giltiga API-versioner för en resurstyp</span><span class="sxs-lookup"><span data-stu-id="5f067-111">View valid API versions for a resource type</span></span>

<span data-ttu-id="5f067-112">Du kan utföra dessa steg via portalen, PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="5f067-112">You can perform these steps through the portal, PowerShell, or Azure CLI.</span></span>

## <a name="powershell"></a><span data-ttu-id="5f067-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5f067-113">PowerShell</span></span>

<span data-ttu-id="5f067-114">Om du vill se alla providrar i Azure och registreringsstatus för din prenumeration, använder du:</span><span class="sxs-lookup"><span data-stu-id="5f067-114">To see all resource providers in Azure, and the registration status for your subscription, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

<span data-ttu-id="5f067-115">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="5f067-115">Which returns results similar to:</span></span>

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="5f067-116">Registrera en resursleverantör konfigurerar din prenumeration för att arbeta med resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="5f067-116">Registering a resource provider configures your subscription to work with the resource provider.</span></span> <span data-ttu-id="5f067-117">Omfattningen för registrering är alltid prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="5f067-117">The scope for registration is always the subscription.</span></span> <span data-ttu-id="5f067-118">Många resursproviders registreras automatiskt som standard.</span><span class="sxs-lookup"><span data-stu-id="5f067-118">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="5f067-119">Du kan dock behöva registrera manuellt vissa resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="5f067-119">However, you may need to manually register some resource providers.</span></span> <span data-ttu-id="5f067-120">Om du vill registrera en resursleverantör, du måste ha behörighet att utföra den `/register/action` åtgärden för resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="5f067-120">To register a resource provider, you must have permission to perform the `/register/action` operation for the resource provider.</span></span> <span data-ttu-id="5f067-121">Den här åtgärden ingår i rollerna deltagare och ägare.</span><span class="sxs-lookup"><span data-stu-id="5f067-121">This operation is included in the Contributor and Owner roles.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="5f067-122">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="5f067-122">Which returns results similar to:</span></span>

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

<span data-ttu-id="5f067-123">Du kan inte avregistrera en resursleverantör när du har fortfarande resurstyper från resursprovidern i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5f067-123">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="5f067-124">Informationen för en viss resurs-provider, Använd:</span><span class="sxs-lookup"><span data-stu-id="5f067-124">To see information for a particular resource provider, use:</span></span>

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

<span data-ttu-id="5f067-125">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="5f067-125">Which returns results similar to:</span></span>

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

<span data-ttu-id="5f067-126">Om du vill visa resurstyperna för en resursleverantör, använder du:</span><span class="sxs-lookup"><span data-stu-id="5f067-126">To see the resource types for a resource provider, use:</span></span>

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

<span data-ttu-id="5f067-127">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="5f067-127">Which returns:</span></span>

```powershell
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="5f067-128">API-versionen motsvarar en version av REST-API: et som ges ut av resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="5f067-128">The API version corresponds to a version of REST API operations that are released by the resource provider.</span></span> <span data-ttu-id="5f067-129">Som en resursleverantör aktiverar nya funktioner, släpper en ny version av REST API.</span><span class="sxs-lookup"><span data-stu-id="5f067-129">As a resource provider enables new features, it releases a new version of the REST API.</span></span> 

<span data-ttu-id="5f067-130">Om du vill hämta tillgängliga API-versioner för en resurstyp, använder du:</span><span class="sxs-lookup"><span data-stu-id="5f067-130">To get the available API versions for a resource type, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

<span data-ttu-id="5f067-131">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="5f067-131">Which returns:</span></span>

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="5f067-132">Hanteraren för filserverresurser stöds i alla regioner, men de resurser som du distribuerar stöds inte i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="5f067-132">Resource Manager is supported in all regions, but the resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="5f067-133">Dessutom kan finnas det begränsningar för din prenumeration som hindrar dig från att använda vissa regioner som har stöd för resursen.</span><span class="sxs-lookup"><span data-stu-id="5f067-133">In addition, there may be limitations on your subscription that prevent you from using some regions that support the resource.</span></span> 

<span data-ttu-id="5f067-134">Använd följande för att få placeringar som stöds för en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="5f067-134">To get the supported locations for a resource type, use.</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

<span data-ttu-id="5f067-135">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="5f067-135">Which returns:</span></span>

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a><span data-ttu-id="5f067-136">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5f067-136">Azure CLI</span></span>
<span data-ttu-id="5f067-137">Om du vill se alla providrar i Azure och registreringsstatus för din prenumeration, använder du:</span><span class="sxs-lookup"><span data-stu-id="5f067-137">To see all resource providers in Azure, and the registration status for your subscription, use:</span></span>

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

<span data-ttu-id="5f067-138">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="5f067-138">Which returns results similar to:</span></span>

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

<span data-ttu-id="5f067-139">Registrera en resursleverantör konfigurerar din prenumeration för att arbeta med resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="5f067-139">Registering a resource provider configures your subscription to work with the resource provider.</span></span> <span data-ttu-id="5f067-140">Omfattningen för registrering är alltid prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="5f067-140">The scope for registration is always the subscription.</span></span> <span data-ttu-id="5f067-141">Många resursproviders registreras automatiskt som standard.</span><span class="sxs-lookup"><span data-stu-id="5f067-141">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="5f067-142">Du kan dock behöva registrera manuellt vissa resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="5f067-142">However, you may need to manually register some resource providers.</span></span> <span data-ttu-id="5f067-143">Om du vill registrera en resursleverantör, du måste ha behörighet att utföra den `/register/action` åtgärden för resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="5f067-143">To register a resource provider, you must have permission to perform the `/register/action` operation for the resource provider.</span></span> <span data-ttu-id="5f067-144">Den här åtgärden ingår i rollerna deltagare och ägare.</span><span class="sxs-lookup"><span data-stu-id="5f067-144">This operation is included in the Contributor and Owner roles.</span></span>

```azurecli
az provider register --namespace Microsoft.Batch
```

<span data-ttu-id="5f067-145">Som returnerar ett meddelande som registrering är pågående.</span><span class="sxs-lookup"><span data-stu-id="5f067-145">Which returns a message that registration is on-going.</span></span>

<span data-ttu-id="5f067-146">Du kan inte avregistrera en resursleverantör när du har fortfarande resurstyper från resursprovidern i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5f067-146">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="5f067-147">Informationen för en viss resurs-provider, Använd:</span><span class="sxs-lookup"><span data-stu-id="5f067-147">To see information for a particular resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch
```

<span data-ttu-id="5f067-148">Som returnerar resultat liknar:</span><span class="sxs-lookup"><span data-stu-id="5f067-148">Which returns results similar to:</span></span>

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

<span data-ttu-id="5f067-149">Om du vill visa resurstyperna för en resursleverantör, använder du:</span><span class="sxs-lookup"><span data-stu-id="5f067-149">To see the resource types for a resource provider, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

<span data-ttu-id="5f067-150">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="5f067-150">Which returns:</span></span>

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

<span data-ttu-id="5f067-151">API-versionen motsvarar en version av REST-API: et som ges ut av resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="5f067-151">The API version corresponds to a version of REST API operations that are released by the resource provider.</span></span> <span data-ttu-id="5f067-152">Som en resursleverantör aktiverar nya funktioner, släpper en ny version av REST API.</span><span class="sxs-lookup"><span data-stu-id="5f067-152">As a resource provider enables new features, it releases a new version of the REST API.</span></span> 

<span data-ttu-id="5f067-153">Om du vill hämta tillgängliga API-versioner för en resurstyp, använder du:</span><span class="sxs-lookup"><span data-stu-id="5f067-153">To get the available API versions for a resource type, use:</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

<span data-ttu-id="5f067-154">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="5f067-154">Which returns:</span></span>

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

<span data-ttu-id="5f067-155">Hanteraren för filserverresurser stöds i alla regioner, men de resurser som du distribuerar stöds inte i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="5f067-155">Resource Manager is supported in all regions, but the resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="5f067-156">Dessutom kan finnas det begränsningar för din prenumeration som hindrar dig från att använda vissa regioner som har stöd för resursen.</span><span class="sxs-lookup"><span data-stu-id="5f067-156">In addition, there may be limitations on your subscription that prevent you from using some regions that support the resource.</span></span> 

<span data-ttu-id="5f067-157">Använd följande för att få placeringar som stöds för en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="5f067-157">To get the supported locations for a resource type, use.</span></span>

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

<span data-ttu-id="5f067-158">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="5f067-158">Which returns:</span></span>

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a><span data-ttu-id="5f067-159">Portalen</span><span class="sxs-lookup"><span data-stu-id="5f067-159">Portal</span></span>

<span data-ttu-id="5f067-160">Om du vill se alla providrar i Azure och registreringsstatus för din prenumeration, Välj **prenumerationer**.</span><span class="sxs-lookup"><span data-stu-id="5f067-160">To see all resource providers in Azure, and the registration status for your subscription, select **Subscriptions**.</span></span>

![Välj prenumerationer](./media/resource-manager-supported-services/select-subscriptions.png)

<span data-ttu-id="5f067-162">Välj prenumerationen att visa.</span><span class="sxs-lookup"><span data-stu-id="5f067-162">Choose the subscription to view.</span></span>

![Ange prenumeration](./media/resource-manager-supported-services/subscription.png)

<span data-ttu-id="5f067-164">Välj **resursproviders** och visa listan över tillgängliga resursproviders.</span><span class="sxs-lookup"><span data-stu-id="5f067-164">Select **Resource providers** and view the list of available resource providers.</span></span>

![Visa resursprovidrar](./media/resource-manager-supported-services/show-resource-providers.png)

<span data-ttu-id="5f067-166">Registrera en resursleverantör konfigurerar din prenumeration för att arbeta med resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="5f067-166">Registering a resource provider configures your subscription to work with the resource provider.</span></span> <span data-ttu-id="5f067-167">Omfattningen för registrering är alltid prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="5f067-167">The scope for registration is always the subscription.</span></span> <span data-ttu-id="5f067-168">Många resursproviders registreras automatiskt som standard.</span><span class="sxs-lookup"><span data-stu-id="5f067-168">By default, many resource providers are automatically registered.</span></span> <span data-ttu-id="5f067-169">Du kan dock behöva registrera manuellt vissa resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="5f067-169">However, you may need to manually register some resource providers.</span></span> <span data-ttu-id="5f067-170">Om du vill registrera en resursleverantör, du måste ha behörighet att utföra den `/register/action` åtgärden för resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="5f067-170">To register a resource provider, you must have permission to perform the `/register/action` operation for the resource provider.</span></span> <span data-ttu-id="5f067-171">Den här åtgärden ingår i rollerna deltagare och ägare.</span><span class="sxs-lookup"><span data-stu-id="5f067-171">This operation is included in the Contributor and Owner roles.</span></span> <span data-ttu-id="5f067-172">Om du vill registrera en resursleverantör, Välj **registrera**.</span><span class="sxs-lookup"><span data-stu-id="5f067-172">To register a resource provider, select **Register**.</span></span>

![registerresursleverantören](./media/resource-manager-supported-services/register-provider.png)

<span data-ttu-id="5f067-174">Du kan inte avregistrera en resursleverantör när du har fortfarande resurstyper från resursprovidern i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5f067-174">You cannot unregister a resource provider when you still have resource types from that resource provider in your subscription.</span></span>

<span data-ttu-id="5f067-175">Om du vill se informationen för en viss resurs-provider väljer **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="5f067-175">To see information for a particular resource provider, select **More services**.</span></span>

![Välj fler tjänster](./media/resource-manager-supported-services/more-services.png)

<span data-ttu-id="5f067-177">Sök efter **Resursläsaren** och välj bland de tillgängliga alternativen.</span><span class="sxs-lookup"><span data-stu-id="5f067-177">Search for **Resource Explorer** and select it from the available options.</span></span>

![Välj resursläsaren](./media/resource-manager-supported-services/select-resource-explorer.png)

<span data-ttu-id="5f067-179">Välj **Providers**.</span><span class="sxs-lookup"><span data-stu-id="5f067-179">Select **Providers**.</span></span>

![Välj providers](./media/resource-manager-supported-services/select-providers.png)

<span data-ttu-id="5f067-181">Välj resursprovidern och resurstypen som du vill visa.</span><span class="sxs-lookup"><span data-stu-id="5f067-181">Select the resource provider and resource type that you want to view.</span></span>

![Välj resurstyp](./media/resource-manager-supported-services/select-resource-type.png)

<span data-ttu-id="5f067-183">Hanteraren för filserverresurser stöds i alla regioner, men de resurser som du distribuerar stöds inte i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="5f067-183">Resource Manager is supported in all regions, but the resources you deploy might not be supported in all regions.</span></span> <span data-ttu-id="5f067-184">Dessutom kan finnas det begränsningar för din prenumeration som hindrar dig från att använda vissa regioner som har stöd för resursen.</span><span class="sxs-lookup"><span data-stu-id="5f067-184">In addition, there may be limitations on your subscription that prevent you from using some regions that support the resource.</span></span> <span data-ttu-id="5f067-185">Resursläsaren visar giltiga platser för resurstypen.</span><span class="sxs-lookup"><span data-stu-id="5f067-185">The resource explorer displays valid locations for the resource type.</span></span>

![Visa platser](./media/resource-manager-supported-services/show-locations.png)

<span data-ttu-id="5f067-187">API-versionen motsvarar en version av REST-API: et som ges ut av resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="5f067-187">The API version corresponds to a version of REST API operations that are released by the resource provider.</span></span> <span data-ttu-id="5f067-188">Som en resursleverantör aktiverar nya funktioner, släpper en ny version av REST API.</span><span class="sxs-lookup"><span data-stu-id="5f067-188">As a resource provider enables new features, it releases a new version of the REST API.</span></span> <span data-ttu-id="5f067-189">Resursläsaren visar giltiga API-versioner för resurstypen.</span><span class="sxs-lookup"><span data-stu-id="5f067-189">The resource explorer displays valid API versions for the resource type.</span></span>

![Visa API-versioner](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a><span data-ttu-id="5f067-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5f067-191">Next steps</span></span>
* <span data-ttu-id="5f067-192">Läs om hur du skapar Resource Manager-mallar i [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5f067-192">To learn about creating Resource Manager templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="5f067-193">Läs om hur du distribuerar resurser i [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="5f067-193">To learn about deploying resources, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>
* <span data-ttu-id="5f067-194">Åtgärder för en resursleverantör finns [Azure REST API](/rest/api/).</span><span class="sxs-lookup"><span data-stu-id="5f067-194">To view the operations for a resource provider, see [Azure REST API](/rest/api/).</span></span>

