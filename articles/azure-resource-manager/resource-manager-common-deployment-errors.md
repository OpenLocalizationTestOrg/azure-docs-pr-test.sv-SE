---
title: aaaTroubleshoot vanliga fel i Azure-distribution | Microsoft Docs
description: "Beskriver hur tooresolve vanliga fel när du distribuerar resurser tooAzure med Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "distributionsfel för azure-distribution distribuera tooazure"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: tomfitz
ms.openlocfilehash: 8571e9941879eb5586e4258a785b6a09247da771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a><span data-ttu-id="6f934-104">Felsöka vanliga Azure-distribution med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6f934-104">Troubleshoot common Azure deployment errors with Azure Resource Manager</span></span>
<span data-ttu-id="6f934-105">Det här avsnittet beskrivs hur du kan lösa några vanliga Azure-distributionsfel som kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="6f934-105">This topic describes how you can resolve some common Azure deployment errors you may encounter.</span></span>

<span data-ttu-id="6f934-106">följande felkoder hello beskrivs i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="6f934-106">hello following error codes are described in this topic:</span></span>

* [<span data-ttu-id="6f934-107">AccountNameInvalid</span><span class="sxs-lookup"><span data-stu-id="6f934-107">AccountNameInvalid</span></span>](#accountnameinvalid)
* [<span data-ttu-id="6f934-108">Auktoriseringen misslyckades</span><span class="sxs-lookup"><span data-stu-id="6f934-108">Authorization failed</span></span>](#authorization-failed)
* [<span data-ttu-id="6f934-109">BadRequest</span><span class="sxs-lookup"><span data-stu-id="6f934-109">BadRequest</span></span>](#badrequest)
* [<span data-ttu-id="6f934-110">DeploymentFailed</span><span class="sxs-lookup"><span data-stu-id="6f934-110">DeploymentFailed</span></span>](#deploymentfailed)
* [<span data-ttu-id="6f934-111">DisallowedOperation</span><span class="sxs-lookup"><span data-stu-id="6f934-111">DisallowedOperation</span></span>](#disallowedoperation)
* [<span data-ttu-id="6f934-112">InvalidContentLink</span><span class="sxs-lookup"><span data-stu-id="6f934-112">InvalidContentLink</span></span>](#invalidcontentlink)
* [<span data-ttu-id="6f934-113">InvalidTemplate</span><span class="sxs-lookup"><span data-stu-id="6f934-113">InvalidTemplate</span></span>](#invalidtemplate)
* [<span data-ttu-id="6f934-114">MissingSubscriptionRegistration</span><span class="sxs-lookup"><span data-stu-id="6f934-114">MissingSubscriptionRegistration</span></span>](#noregisteredproviderfound)
* [<span data-ttu-id="6f934-115">NotFound</span><span class="sxs-lookup"><span data-stu-id="6f934-115">NotFound</span></span>](#notfound)
* [<span data-ttu-id="6f934-116">NoRegisteredProviderFound</span><span class="sxs-lookup"><span data-stu-id="6f934-116">NoRegisteredProviderFound</span></span>](#noregisteredproviderfound)
* [<span data-ttu-id="6f934-117">OperationNotAllowed</span><span class="sxs-lookup"><span data-stu-id="6f934-117">OperationNotAllowed</span></span>](#quotaexceeded)
* [<span data-ttu-id="6f934-118">ParentResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="6f934-118">ParentResourceNotFound</span></span>](#parentresourcenotfound)
* [<span data-ttu-id="6f934-119">QuotaExceeded</span><span class="sxs-lookup"><span data-stu-id="6f934-119">QuotaExceeded</span></span>](#quotaexceeded)
* [<span data-ttu-id="6f934-120">RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="6f934-120">RequestDisallowedByPolicy</span></span>](#requestdisallowedbypolicy)
* [<span data-ttu-id="6f934-121">ResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="6f934-121">ResourceNotFound</span></span>](#notfound)
* [<span data-ttu-id="6f934-122">SkuNotAvailable</span><span class="sxs-lookup"><span data-stu-id="6f934-122">SkuNotAvailable</span></span>](#skunotavailable)
* [<span data-ttu-id="6f934-123">StorageAccountAlreadyExists</span><span class="sxs-lookup"><span data-stu-id="6f934-123">StorageAccountAlreadyExists</span></span>](#storagenamenotunique)
* [<span data-ttu-id="6f934-124">StorageAccountAlreadyTaken</span><span class="sxs-lookup"><span data-stu-id="6f934-124">StorageAccountAlreadyTaken</span></span>](#storagenamenotunique)

## <a name="deploymentfailed"></a><span data-ttu-id="6f934-125">DeploymentFailed</span><span class="sxs-lookup"><span data-stu-id="6f934-125">DeploymentFailed</span></span>

<span data-ttu-id="6f934-126">Felet indikerar en allmän distributionsfel men det är inte hello felkoden måste toostart felsökning.</span><span class="sxs-lookup"><span data-stu-id="6f934-126">This error code indicates a general deployment error, but it is not hello error code you need toostart troubleshooting.</span></span> <span data-ttu-id="6f934-127">hello felkoden som hjälper dig att lösa problemet hello faktiskt är vanligtvis en nivå under det här felet.</span><span class="sxs-lookup"><span data-stu-id="6f934-127">hello error code that actually helps you resolve hello issue is usually one level below this error.</span></span> <span data-ttu-id="6f934-128">Hello följande bild visar exempelvis hello **RequestDisallowedByPolicy** felkod som är under hello distributionsfel.</span><span class="sxs-lookup"><span data-stu-id="6f934-128">For example, hello following image shows hello **RequestDisallowedByPolicy** error code that is under hello deployment error.</span></span>

![Visa felkod](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a><span data-ttu-id="6f934-130">SkuNotAvailable</span><span class="sxs-lookup"><span data-stu-id="6f934-130">SkuNotAvailable</span></span>

<span data-ttu-id="6f934-131">När du distribuerar en resurs (vanligtvis en virtuell dator), får hello följande kod och fel felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="6f934-131">When deploying a resource (typically a virtual machine), you may receive hello following error code and error message:</span></span>

```
Code: SkuNotAvailable
Message: hello requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy tooa different location.
```

<span data-ttu-id="6f934-132">Du får detta felmeddelande när hello resursen SKU som du har valt (till exempel VM-storlek) inte är tillgänglig för hello-plats som du har valt.</span><span class="sxs-lookup"><span data-stu-id="6f934-132">You receive this error when hello resource SKU you have selected (such as VM size) is not available for hello location you have selected.</span></span> <span data-ttu-id="6f934-133">tooresolve det här problemet behöver du toodetermine som SKU: er är tillgängliga i en region.</span><span class="sxs-lookup"><span data-stu-id="6f934-133">tooresolve this issue, you need toodetermine which SKUs are available in a region.</span></span> <span data-ttu-id="6f934-134">Du kan använda PowerShell, hello-portalen eller en REST-åtgärden toofind tillgängliga SKU: er.</span><span class="sxs-lookup"><span data-stu-id="6f934-134">You can use PowerShell, hello portal, or a REST operation toofind available SKUs.</span></span>

- <span data-ttu-id="6f934-135">PowerShell, Använd [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) och filtrera efter plats.</span><span class="sxs-lookup"><span data-stu-id="6f934-135">For PowerShell, use [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) and filter by location.</span></span> <span data-ttu-id="6f934-136">Du måste ha hello senaste versionen av PowerShell för det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="6f934-136">You must have hello latest version of PowerShell for this command.</span></span>

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
  ```

  <span data-ttu-id="6f934-137">hello resultatet innehåller en lista över SKU: er för hello platsen och eventuella begränsningar för den SKU.</span><span class="sxs-lookup"><span data-stu-id="6f934-137">hello results include a list of SKUs for hello location and any restrictions for that SKU.</span></span>

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 southcentralus
  virtualMachines      Standard_A1 southcentralus
  virtualMachines      Standard_A2 southcentralus
  ```

- <span data-ttu-id="6f934-138">toouse hello [portal](https://portal.azure.com), logga in toohello portal och lägga till en resurs via hello-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="6f934-138">toouse hello [portal](https://portal.azure.com), log in toohello portal and add a resource through hello interface.</span></span> <span data-ttu-id="6f934-139">När du ställer in hello värden du se hello tillgängliga SKU: er för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="6f934-139">As you set hello values, you see hello available SKUs for that resource.</span></span> <span data-ttu-id="6f934-140">Du behöver inte toocomplete hello distribution.</span><span class="sxs-lookup"><span data-stu-id="6f934-140">You do not need toocomplete hello deployment.</span></span>

    ![tillgängliga SKU: er](./media/resource-manager-common-deployment-errors/view-sku.png)

- <span data-ttu-id="6f934-142">toouse hello REST API för virtuella datorer, skicka hello följande begäran:</span><span class="sxs-lookup"><span data-stu-id="6f934-142">toouse hello REST API for virtual machines, send hello following request:</span></span>

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  <span data-ttu-id="6f934-143">Tillgängliga SKU: er och regioner returneras i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="6f934-143">It returns available SKUs and regions in hello following format:</span></span>

  ```json
  {
    "value": [
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A0",
        "tier": "Standard",
        "size": "A0",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A1",
        "tier": "Standard",
        "size": "A1",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      ...
    ]
  }    
  ```

<span data-ttu-id="6f934-144">Om du toofind en lämplig SKU i den regionen eller en annan region som uppfyller behoven för din verksamhet kan skicka en [SKU begäran](https://aka.ms/skurestriction) tooAzure Support.</span><span class="sxs-lookup"><span data-stu-id="6f934-144">If you are unable toofind a suitable SKU in that region or an alternative region that meets your business needs, submit a [SKU request](https://aka.ms/skurestriction) tooAzure Support.</span></span>

## <a name="disallowedoperation"></a><span data-ttu-id="6f934-145">DisallowedOperation</span><span class="sxs-lookup"><span data-stu-id="6f934-145">DisallowedOperation</span></span>

```
Code: DisallowedOperation
Message: hello current subscription type is not permitted tooperform operations on any provider 
namespace. Please use a different subscription.
```

<span data-ttu-id="6f934-146">Om du får detta felmeddelande du använder en prenumeration som inte är tillåtet tooaccess alla Azure-tjänster än Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6f934-146">If you receive this error, you are using a subscription that is not permitted tooaccess any Azure services other than Azure Active Directory.</span></span> <span data-ttu-id="6f934-147">Den här typen av prenumeration kan uppstå när du behöver tooaccess hello klassiska portalen men är inte tillåtna toodeploy resurser.</span><span class="sxs-lookup"><span data-stu-id="6f934-147">You might have this type of subscription when you need tooaccess hello classic portal but are not permitted toodeploy resources.</span></span> <span data-ttu-id="6f934-148">tooresolve det här problemet måste du använda en prenumeration som har behörigheten toodeploy resurser.</span><span class="sxs-lookup"><span data-stu-id="6f934-148">tooresolve this issue, you must use a subscription that has permission toodeploy resources.</span></span>  

<span data-ttu-id="6f934-149">tooview tillgängliga prenumerationer med PowerShell, Använd:</span><span class="sxs-lookup"><span data-stu-id="6f934-149">tooview your available subscriptions with PowerShell, use:</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="6f934-150">Och tooset hello aktuell prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="6f934-150">And, tooset hello current subscription, use:</span></span>

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

<span data-ttu-id="6f934-151">tooview tillgängliga prenumerationer med Azure CLI 2.0, Använd:</span><span class="sxs-lookup"><span data-stu-id="6f934-151">tooview your available subscriptions with Azure CLI 2.0, use:</span></span>

```azurecli
az account list
```

<span data-ttu-id="6f934-152">Och tooset hello aktuell prenumeration, Använd:</span><span class="sxs-lookup"><span data-stu-id="6f934-152">And, tooset hello current subscription, use:</span></span>

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a><span data-ttu-id="6f934-153">InvalidTemplate</span><span class="sxs-lookup"><span data-stu-id="6f934-153">InvalidTemplate</span></span>
<span data-ttu-id="6f934-154">Det här felet kan bero på flera olika typer av fel.</span><span class="sxs-lookup"><span data-stu-id="6f934-154">This error can result from several different types of errors.</span></span>

- <span data-ttu-id="6f934-155">Syntaxfel</span><span class="sxs-lookup"><span data-stu-id="6f934-155">Syntax error</span></span>

   <span data-ttu-id="6f934-156">Om du får ett felmeddelande som anger hello mallen misslyckades verifieringen kanske syntax problem i mallen.</span><span class="sxs-lookup"><span data-stu-id="6f934-156">If you receive an error message that indicates hello template failed validation, you may have a syntax problem in your template.</span></span>

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   <span data-ttu-id="6f934-157">Det här felet beror på enkelt toomake malluttryck kan vara komplicerade.</span><span class="sxs-lookup"><span data-stu-id="6f934-157">This error is easy toomake because template expressions can be intricate.</span></span> <span data-ttu-id="6f934-158">Till exempel innehåller hello följande namntilldelning för ett lagringskonto en uppsättning av hakparenteser, tre funktioner, tre uppsättningar av parenteser, en uppsättning av enkla citattecken och en egenskap:</span><span class="sxs-lookup"><span data-stu-id="6f934-158">For example, hello following name assignment for a storage account contains one set of brackets, three functions, three sets of parentheses, one set of single quotes, and one property:</span></span>

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   <span data-ttu-id="6f934-159">Om du inte anger hello matchande syntax genererar hello mallen ett värde som skiljer sig från din avsikt.</span><span class="sxs-lookup"><span data-stu-id="6f934-159">If you do not provide hello matching syntax, hello template produces a value that is different than your intention.</span></span>

   <span data-ttu-id="6f934-160">Granska noggrant hello uttryckssyntax när du får den här typen av fel.</span><span class="sxs-lookup"><span data-stu-id="6f934-160">When you receive this type of error, carefully review hello expression syntax.</span></span> <span data-ttu-id="6f934-161">Överväg att använda en JSON-redigerare som [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) eller [Visual Studio Code](resource-manager-vs-code.md), vilket kan varna dig om syntaxfel.</span><span class="sxs-lookup"><span data-stu-id="6f934-161">Consider using a JSON editor like [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) or [Visual Studio Code](resource-manager-vs-code.md), which can warn you about syntax errors.</span></span>

- <span data-ttu-id="6f934-162">Felaktiga längder</span><span class="sxs-lookup"><span data-stu-id="6f934-162">Incorrect segment lengths</span></span>

   <span data-ttu-id="6f934-163">En annan ogiltig mall-fel uppstår när hello resursnamnet inte är i rätt format för hello.</span><span class="sxs-lookup"><span data-stu-id="6f934-163">Another invalid template error occurs when hello resource name is not in hello correct format.</span></span>

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'hello template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   <span data-ttu-id="6f934-164">En nivå rotresurs måste ha ett mindre segment i hello namn än i hello resurstypen.</span><span class="sxs-lookup"><span data-stu-id="6f934-164">A root level resource must have one less segment in hello name than in hello resource type.</span></span> <span data-ttu-id="6f934-165">Varje segment differentierade med ett snedstreck.</span><span class="sxs-lookup"><span data-stu-id="6f934-165">Each segment is differentiated by a slash.</span></span> <span data-ttu-id="6f934-166">I följande exempel hello, hello typen har två segment och hello namn har ett segment så att den är en **giltigt namn**.</span><span class="sxs-lookup"><span data-stu-id="6f934-166">In hello following example, hello type has two segments and hello name has one segment, so it is a **valid name**.</span></span>

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   <span data-ttu-id="6f934-167">Men hello nästa exempel är **inte ett giltigt namn** eftersom den har hello samma antal segment som hello-typen.</span><span class="sxs-lookup"><span data-stu-id="6f934-167">But hello next example is **not a valid name** because it has hello same number of segments as hello type.</span></span>

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   <span data-ttu-id="6f934-168">För underordnade resurser hello typ och namn har hello samma antal segment.</span><span class="sxs-lookup"><span data-stu-id="6f934-168">For child resources, hello type and name have hello same number of segments.</span></span> <span data-ttu-id="6f934-169">Det här antalet segment är meningsfullt eftersom hello fullständigt namn och typ för hello underordnade innehåller hello överordnade namn och typen.</span><span class="sxs-lookup"><span data-stu-id="6f934-169">This number of segments makes sense because hello full name and type for hello child includes hello parent name and type.</span></span> <span data-ttu-id="6f934-170">Hello fullständiga namnet måste därför fortfarande ett mindre segment än hello fullständig typen.</span><span class="sxs-lookup"><span data-stu-id="6f934-170">Therefore, hello full name still has one less segment than hello full type.</span></span>

  ```json
  "resources": [
      {
          "type": "Microsoft.KeyVault/vaults",
          "name": "contosokeyvault",
          ...
          "resources": [
              {
                  "type": "secrets",
                  "name": "appPassword",
                  ...
              }
          ]
      }
  ]
  ```

   <span data-ttu-id="6f934-171">Hämtning hello segment rätt kan vara svårt med Resource Manager-typer som tillämpas på resursleverantörer.</span><span class="sxs-lookup"><span data-stu-id="6f934-171">Getting hello segments right can be tricky with Resource Manager types that are applied across resource providers.</span></span> <span data-ttu-id="6f934-172">Till exempel kräver använda en resurs Lås tooa webbplats en typ med fyra segment.</span><span class="sxs-lookup"><span data-stu-id="6f934-172">For example, applying a resource lock tooa web site requires a type with four segments.</span></span> <span data-ttu-id="6f934-173">Hello namn är därför tre segment:</span><span class="sxs-lookup"><span data-stu-id="6f934-173">Therefore, hello name is three segments:</span></span>

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- <span data-ttu-id="6f934-174">Kopiera index är inte den förväntade</span><span class="sxs-lookup"><span data-stu-id="6f934-174">Copy index is not expected</span></span>

   <span data-ttu-id="6f934-175">Du får detta **InvalidTemplate** fel när du har tillämpat hello **kopiera** elementet tooa tillhör hello-mall som inte stöder det här elementet.</span><span class="sxs-lookup"><span data-stu-id="6f934-175">You encounter this **InvalidTemplate** error when you have applied hello **copy** element tooa part of hello template that does not support this element.</span></span> <span data-ttu-id="6f934-176">Du kan bara använda hello kopiera elementet tooa resurstypen.</span><span class="sxs-lookup"><span data-stu-id="6f934-176">You can only apply hello copy element tooa resource type.</span></span> <span data-ttu-id="6f934-177">Du kan inte använda egenskapen tooa inom en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="6f934-177">You cannot apply copy tooa property within a resource type.</span></span> <span data-ttu-id="6f934-178">Till exempel du gäller kopiera tooa virtuell dator, men du kan inte använda den toohello OS-diskar för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6f934-178">For example, you apply copy tooa virtual machine, but you cannot apply it toohello OS disks for a virtual machine.</span></span> <span data-ttu-id="6f934-179">I vissa fall kan konvertera du en underordnad resurs tooa överordnade resurs toocreate en kopia skapas.</span><span class="sxs-lookup"><span data-stu-id="6f934-179">In some cases, you can convert a child resource tooa parent resource toocreate a copy loop.</span></span> <span data-ttu-id="6f934-180">Mer information om hur du använder kopia finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="6f934-180">For more information about using copy, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

- <span data-ttu-id="6f934-181">Parametern är inte giltig</span><span class="sxs-lookup"><span data-stu-id="6f934-181">Parameter is not valid</span></span>

   <span data-ttu-id="6f934-182">Om du anger ett värde som inte är ett av dessa värden hello mall anger tillåtna värden för en parameter, visas ett meddelande liknande toohello följande fel:</span><span class="sxs-lookup"><span data-stu-id="6f934-182">If hello template specifies permitted values for a parameter, and you provide a value that is not one of those values, you receive a message similar toohello following error:</span></span>

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'hello provided value {parameter value}
  for hello template parameter {parameter name} is not valid. hello parameter value is not
  part of hello allowed values
  ``` 

   <span data-ttu-id="6f934-183">Kontrollera hello tillåtna värden i hello mallen och ange en under distributionen.</span><span class="sxs-lookup"><span data-stu-id="6f934-183">Double check hello allowed values in hello template, and provide one during deployment.</span></span>

- <span data-ttu-id="6f934-184">Cirkulärt beroende har upptäckts</span><span class="sxs-lookup"><span data-stu-id="6f934-184">Circular dependency detected</span></span>

   <span data-ttu-id="6f934-185">Du får detta felmeddelande när resurser beroende av varandra på ett sätt som förhindrar hello distribution från att starta.</span><span class="sxs-lookup"><span data-stu-id="6f934-185">You receive this error when resources depend on each other in a way that prevents hello deployment from starting.</span></span> <span data-ttu-id="6f934-186">En kombination av beroenden gör två eller flera resurs vänta tills andra resurser som väntar på också.</span><span class="sxs-lookup"><span data-stu-id="6f934-186">A combination of interdependencies makes two or more resource wait for other resources that are also waiting.</span></span> <span data-ttu-id="6f934-187">Till exempel resource1 beror på resource3 resource2 beror på resource1 och resource3 beror på resource2.</span><span class="sxs-lookup"><span data-stu-id="6f934-187">For example, resource1 depends on resource3, resource2 depends on resource1, and resource3 depends on resource2.</span></span> <span data-ttu-id="6f934-188">Vanligtvis kan du lösa problemet genom att ta bort onödiga beroenden.</span><span class="sxs-lookup"><span data-stu-id="6f934-188">You can usually solve this problem by removing unnecessary dependencies.</span></span> 

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a><span data-ttu-id="6f934-189">NotFound och ResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="6f934-189">NotFound and ResourceNotFound</span></span>
<span data-ttu-id="6f934-190">När mallen innehåller hello namnet på en resurs som inte kan matchas, visas ett felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="6f934-190">When your template includes hello name of a resource that cannot be resolved, you receive an error similar to:</span></span>

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

<span data-ttu-id="6f934-191">Om du försöker toodeploy hello resursen i hello mall saknas kontrollerar du om du behöver tooadd ett beroende.</span><span class="sxs-lookup"><span data-stu-id="6f934-191">If you are attempting toodeploy hello missing resource in hello template, check whether you need tooadd a dependency.</span></span> <span data-ttu-id="6f934-192">Hanteraren för filserverresurser optimerar distribution genom att skapa resurser parallellt, när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="6f934-192">Resource Manager optimizes deployment by creating resources in parallel, when possible.</span></span> <span data-ttu-id="6f934-193">Om en resurs måste distribueras efter en annan resurs, behöver du toouse hello **dependsOn** element i din mall toocreate en beroende hello andra resurser.</span><span class="sxs-lookup"><span data-stu-id="6f934-193">If one resource must be deployed after another resource, you need toouse hello **dependsOn** element in your template toocreate a dependency on hello other resource.</span></span> <span data-ttu-id="6f934-194">Hello App Service-plan måste till exempel finnas när du distribuerar ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="6f934-194">For example, when deploying a web app, hello App Service plan must exist.</span></span> <span data-ttu-id="6f934-195">Om du inte har angett att hello webbprogrammet beror på hello programtjänstplanen Resource Manager skapar båda resurserna på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="6f934-195">If you have not specified that hello web app depends on hello App Service plan, Resource Manager creates both resources at hello same time.</span></span> <span data-ttu-id="6f934-196">Du får ett felmeddelande om att hello App Service-plan resursen inte kan hittas, eftersom det inte finns ännu vid försök tooset en egenskap på hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="6f934-196">You receive an error stating that hello App Service plan resource cannot be found, because it does not exist yet when attempting tooset a property on hello web app.</span></span> <span data-ttu-id="6f934-197">Du kan förhindra att det här felet genom att ange hello beroende i hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="6f934-197">You prevent this error by setting hello dependency in hello web app.</span></span>

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

<span data-ttu-id="6f934-198">Förslag på felsökning för beroende finns [Kontrollera distributionsordningen](#check-deployment-sequence).</span><span class="sxs-lookup"><span data-stu-id="6f934-198">For suggestions on troubleshooting dependency errors, see [Check deployment sequence](#check-deployment-sequence).</span></span>

<span data-ttu-id="6f934-199">Du kan också se felet när hello resursen finns i en annan resursgrupp än hello något som distribueras.</span><span class="sxs-lookup"><span data-stu-id="6f934-199">You also see this error when hello resource exists in a different resource group than hello one being deployed to.</span></span> <span data-ttu-id="6f934-200">I så fall använder hello [resourceId funktionen](resource-group-template-functions-resource.md#resourceid) tooget hello fullständigt kvalificerade namnet på hello resursen.</span><span class="sxs-lookup"><span data-stu-id="6f934-200">In that case, use hello [resourceId function](resource-group-template-functions-resource.md#resourceid) tooget hello fully qualified name of hello resource.</span></span>

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

<span data-ttu-id="6f934-201">Om du försöker toouse hello [referens](resource-group-template-functions-resource.md#reference) eller [listKeys](resource-group-template-functions-resource.md#listkeys) funktioner med en resurs som inte kan lösas, du får hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="6f934-201">If you attempt toouse hello [reference](resource-group-template-functions-resource.md#reference) or [listKeys](resource-group-template-functions-resource.md#listkeys) functions with a resource that cannot be resolved, you receive hello following error:</span></span>

```
Code=ResourceNotFound;
Message=hello Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

<span data-ttu-id="6f934-202">Leta efter ett uttryck som innehåller hello **referens** funktion.</span><span class="sxs-lookup"><span data-stu-id="6f934-202">Look for an expression that includes hello **reference** function.</span></span> <span data-ttu-id="6f934-203">Kontrollera att hello parametervärden är korrekta.</span><span class="sxs-lookup"><span data-stu-id="6f934-203">Double check that hello parameter values are correct.</span></span>

## <a name="parentresourcenotfound"></a><span data-ttu-id="6f934-204">ParentResourceNotFound</span><span class="sxs-lookup"><span data-stu-id="6f934-204">ParentResourceNotFound</span></span>

<span data-ttu-id="6f934-205">När en resurs är en överordnad tooanother resurs, måste hello överordnade resursen finnas innan du skapar hello underordnade resursen.</span><span class="sxs-lookup"><span data-stu-id="6f934-205">When one resource is a parent tooanother resource, hello parent resource must exist before creating hello child resource.</span></span> <span data-ttu-id="6f934-206">Om den inte ännu finns, visas hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="6f934-206">If it does not yet exist, you receive hello following error:</span></span>

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

<span data-ttu-id="6f934-207">hello ingår hello underordnade resursen hello överordnade namn.</span><span class="sxs-lookup"><span data-stu-id="6f934-207">hello name of hello child resource includes hello parent name.</span></span> <span data-ttu-id="6f934-208">Till exempel kan en SQL-databas definieras som:</span><span class="sxs-lookup"><span data-stu-id="6f934-208">For example, a SQL Database might be defined as:</span></span>

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

<span data-ttu-id="6f934-209">Men om du inte anger ett beroende på hello överordnade resursen hello underordnade resursen kan distribueras innan hello överordnade.</span><span class="sxs-lookup"><span data-stu-id="6f934-209">But, if you do not specify a dependency on hello parent resource, hello child resource may get deployed before hello parent.</span></span> <span data-ttu-id="6f934-210">tooresolve det här felet är ett beroende.</span><span class="sxs-lookup"><span data-stu-id="6f934-210">tooresolve this error, include a dependency.</span></span>

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />

## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a><span data-ttu-id="6f934-211">StorageAccountAlreadyExists och StorageAccountAlreadyTaken</span><span class="sxs-lookup"><span data-stu-id="6f934-211">StorageAccountAlreadyExists and StorageAccountAlreadyTaken</span></span>
<span data-ttu-id="6f934-212">Du måste ange ett namn för hello-resurs som är unik i Azure för storage-konton.</span><span class="sxs-lookup"><span data-stu-id="6f934-212">For storage accounts, you must provide a name for hello resource that is unique across Azure.</span></span> <span data-ttu-id="6f934-213">Om du inte anger ett unikt namn får ett felmeddelande som:</span><span class="sxs-lookup"><span data-stu-id="6f934-213">If you do not provide a unique name, you receive an error like:</span></span>

```
Code=StorageAccountAlreadyTaken
Message=hello storage account named mystorage is already taken.
```

<span data-ttu-id="6f934-214">Du kan skapa ett unikt namn genom att sammanbinda en namngivningskonvention med hello resultatet av hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funktion.</span><span class="sxs-lookup"><span data-stu-id="6f934-214">You can create a unique name by concatenating your naming convention with hello result of hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function.</span></span>

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

<span data-ttu-id="6f934-215">Om du distribuerar ett lagringskonto med hello med samma namn som ett befintligt lagringskonto i din prenumeration, men ger en annan plats, du får ett felmeddelande som anger hello storage-konto finns redan i en annan plats.</span><span class="sxs-lookup"><span data-stu-id="6f934-215">If you deploy a storage account with hello same name as an existing storage account in your subscription, but provide a different location, you receive an error indicating hello storage account already exists in a different location.</span></span> <span data-ttu-id="6f934-216">Ta antingen bort hello befintligt lagringskonto eller ange hello samma plats som hello befintligt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6f934-216">Either delete hello existing storage account, or provide hello same location as hello existing storage account.</span></span>

## <a name="accountnameinvalid"></a><span data-ttu-id="6f934-217">AccountNameInvalid</span><span class="sxs-lookup"><span data-stu-id="6f934-217">AccountNameInvalid</span></span>
<span data-ttu-id="6f934-218">Du ser hello **AccountNameInvalid** fel vid försök toogive en storage-konto ett namn som innehåller förbjudet tecken.</span><span class="sxs-lookup"><span data-stu-id="6f934-218">You see hello **AccountNameInvalid** error when attempting toogive a storage account a name that includes prohibited characters.</span></span> <span data-ttu-id="6f934-219">Lagringskontonamn måste vara mellan 3 och 24 tecken långt och innehålla siffror och gemena bokstäver.</span><span class="sxs-lookup"><span data-stu-id="6f934-219">Storage account names must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span> <span data-ttu-id="6f934-220">Hej [uniqueString](resource-group-template-functions-string.md#uniquestring) funktionen returnerar 13 tecken.</span><span class="sxs-lookup"><span data-stu-id="6f934-220">hello [uniqueString](resource-group-template-functions-string.md#uniquestring) function returns 13 characters.</span></span> <span data-ttu-id="6f934-221">Om du sammanfoga ett prefix toohello **uniqueString** leda, ange ett prefix som är 11 tecken eller mindre.</span><span class="sxs-lookup"><span data-stu-id="6f934-221">If you concatenate a prefix toohello **uniqueString** result, provide a prefix that is 11 characters or less.</span></span>

## <a name="badrequest"></a><span data-ttu-id="6f934-222">BadRequest</span><span class="sxs-lookup"><span data-stu-id="6f934-222">BadRequest</span></span>

<span data-ttu-id="6f934-223">Status BadRequest kan uppstå när du anger ett ogiltigt värde för en egenskap.</span><span class="sxs-lookup"><span data-stu-id="6f934-223">You may encounter a BadRequest status when you provide an invalid value for a property.</span></span> <span data-ttu-id="6f934-224">Du anger ett felaktigt värde för SKU för ett lagringskonto måste misslyckas hello distributionen.</span><span class="sxs-lookup"><span data-stu-id="6f934-224">For example, if you provide an incorrect SKU value for a storage account, hello deployment fails.</span></span> <span data-ttu-id="6f934-225">toodetermine giltiga värden för egenskapen titta på hello [REST API](/rest/api) för hello resurstyp som du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="6f934-225">toodetermine valid values for property, look at hello [REST API](/rest/api) for hello resource type you are deploying.</span></span>

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a><span data-ttu-id="6f934-226">NoRegisteredProviderFound och MissingSubscriptionRegistration</span><span class="sxs-lookup"><span data-stu-id="6f934-226">NoRegisteredProviderFound and MissingSubscriptionRegistration</span></span>
<span data-ttu-id="6f934-227">När du distribuerar resurs, kan du får hello följande felkod och meddelande:</span><span class="sxs-lookup"><span data-stu-id="6f934-227">When deploying resource, you may receive hello following error code and message:</span></span>

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

<span data-ttu-id="6f934-228">Eller, du kan få en liknande meddelande om att:</span><span class="sxs-lookup"><span data-stu-id="6f934-228">Or, you may receive a similar message that states:</span></span>

```
Code: MissingSubscriptionRegistration
Message: hello subscription is not registered toouse namespace {resource-provider-namespace}
```

<span data-ttu-id="6f934-229">Felen visas i någon av tre orsaker:</span><span class="sxs-lookup"><span data-stu-id="6f934-229">You receive these errors for one of three reasons:</span></span>

1. <span data-ttu-id="6f934-230">Hej resursprovidern har inte registrerats för din prenumeration</span><span class="sxs-lookup"><span data-stu-id="6f934-230">hello resource provider has not been registered for your subscription</span></span>
2. <span data-ttu-id="6f934-231">API-versionen stöds inte för hello resurstyp</span><span class="sxs-lookup"><span data-stu-id="6f934-231">API version not supported for hello resource type</span></span>
3. <span data-ttu-id="6f934-232">Plats stöds inte för hello resurstyp</span><span class="sxs-lookup"><span data-stu-id="6f934-232">Location not supported for hello resource type</span></span>

<span data-ttu-id="6f934-233">hello felmeddelande bör du få förslag på hello stöds platser och API-versioner.</span><span class="sxs-lookup"><span data-stu-id="6f934-233">hello error message should give you suggestions for hello supported locations and API versions.</span></span> <span data-ttu-id="6f934-234">Du kan ändra din mall tooone av hello föreslagna värden.</span><span class="sxs-lookup"><span data-stu-id="6f934-234">You can change your template tooone of hello suggested values.</span></span> <span data-ttu-id="6f934-235">De flesta leverantörer registreras automatiskt av hello Azure-portalen eller hello-kommandoradsgränssnittet du använder, men inte alla.</span><span class="sxs-lookup"><span data-stu-id="6f934-235">Most providers are registered automatically by hello Azure portal or hello command-line interface you are using, but not all.</span></span> <span data-ttu-id="6f934-236">Om du inte har använt en viss resurs-providern före eventuellt du tooregister providern.</span><span class="sxs-lookup"><span data-stu-id="6f934-236">If you have not used a particular resource provider before, you may need tooregister that provider.</span></span> <span data-ttu-id="6f934-237">Du kan identifiera information om resursproviders via PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6f934-237">You can discover more about resource providers through PowerShell or Azure CLI.</span></span>

<span data-ttu-id="6f934-238">**Portal**</span><span class="sxs-lookup"><span data-stu-id="6f934-238">**Portal**</span></span>

<span data-ttu-id="6f934-239">Du kan se hello registreringsstatus och registrera en resursleverantörens namnrymd hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="6f934-239">You can see hello registration status and register a resource provider namespace through hello portal.</span></span>

1. <span data-ttu-id="6f934-240">Din prenumeration, Välj **resursproviders**.</span><span class="sxs-lookup"><span data-stu-id="6f934-240">For your subscription, select **Resource providers**.</span></span>

   ![Välj resursprovidrar](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. <span data-ttu-id="6f934-242">Titta på hello lista över resursproviders och välj eventuellt hello **registrera** länk tooregister hello resursprovidern av typen hello försöker toodeploy.</span><span class="sxs-lookup"><span data-stu-id="6f934-242">Look at hello list of resource providers, and if necessary, select hello **Register** link tooregister hello resource provider of hello type you are trying toodeploy.</span></span>

   ![Visa en lista över resursprovidrar](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

<span data-ttu-id="6f934-244">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="6f934-244">**PowerShell**</span></span>

<span data-ttu-id="6f934-245">toosee din registreringsstatus, Använd **Get-AzureRmResourceProvider**.</span><span class="sxs-lookup"><span data-stu-id="6f934-245">toosee your registration status, use **Get-AzureRmResourceProvider**.</span></span>

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

<span data-ttu-id="6f934-246">tooregister en provider, Använd **registrera AzureRmResourceProvider** och ange hello namn av hello resursprovidern gärna tooregister.</span><span class="sxs-lookup"><span data-stu-id="6f934-246">tooregister a provider, use **Register-AzureRmResourceProvider** and provide hello name of hello resource provider you wish tooregister.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

<span data-ttu-id="6f934-247">tooget hello stöds platser för en viss typ av resurs, Använd:</span><span class="sxs-lookup"><span data-stu-id="6f934-247">tooget hello supported locations for a particular type of resource, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

<span data-ttu-id="6f934-248">tooget hello API-versioner som stöds för en viss typ av resurs, Använd:</span><span class="sxs-lookup"><span data-stu-id="6f934-248">tooget hello supported API versions for a particular type of resource, use:</span></span>

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

<span data-ttu-id="6f934-249">**Azure CLI**</span><span class="sxs-lookup"><span data-stu-id="6f934-249">**Azure CLI**</span></span>

<span data-ttu-id="6f934-250">toosee om hello provider är registrerad, använda hello `azure provider list` kommando.</span><span class="sxs-lookup"><span data-stu-id="6f934-250">toosee whether hello provider is registered, use hello `azure provider list` command.</span></span>

```azurecli
az provider list
```

<span data-ttu-id="6f934-251">tooregister en resursleverantör, använda hello `azure provider register` kommando och ange hello *namnområde* tooregister.</span><span class="sxs-lookup"><span data-stu-id="6f934-251">tooregister a resource provider, use hello `azure provider register` command, and specify hello *namespace* tooregister.</span></span>

```azurecli
az provider register --namespace Microsoft.Cdn
```

<span data-ttu-id="6f934-252">Använd toosee hello stöds platser och API-versioner för en resurstyp:</span><span class="sxs-lookup"><span data-stu-id="6f934-252">toosee hello supported locations and API versions for a resource type, use:</span></span>

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded" />

## <a name="quotaexceeded-and-operationnotallowed"></a><span data-ttu-id="6f934-253">QuotaExceeded och OperationNotAllowed</span><span class="sxs-lookup"><span data-stu-id="6f934-253">QuotaExceeded and OperationNotAllowed</span></span>
<span data-ttu-id="6f934-254">Problem kan uppstå när distributionen överskrider kvoten som kan vara per resursgrupp, prenumerationer, konton och andra scope.</span><span class="sxs-lookup"><span data-stu-id="6f934-254">You might have issues when deployment exceeds a quota, which could be per resource group, subscriptions, accounts, and other scopes.</span></span> <span data-ttu-id="6f934-255">Din prenumeration kan till exempel vara konfigurerade toolimit hello antalet kärnor för en region.</span><span class="sxs-lookup"><span data-stu-id="6f934-255">For example, your subscription may be configured toolimit hello number of cores for a region.</span></span> <span data-ttu-id="6f934-256">Om du försöker toodeploy en virtuell dator med fler kärnor än hello tillåtna belopp, felmeddelande ett anger hello kvot har överskridits.</span><span class="sxs-lookup"><span data-stu-id="6f934-256">If you attempt toodeploy a virtual machine with more cores than hello permitted amount, you receive an error stating hello quota has been exceeded.</span></span>
<span data-ttu-id="6f934-257">För fullständig kvotinformation finns i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="6f934-257">For complete quota information, see [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md).</span></span>

<span data-ttu-id="6f934-258">tooexamine din prenumeration kvoter för kärnor, kan du använda hello `azure vm list-usage` i hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6f934-258">tooexamine your subscription's quotas for cores, you can use hello `azure vm list-usage` command in hello Azure CLI.</span></span> <span data-ttu-id="6f934-259">hello som följande exempel visar att hello kärnkvot för ett kostnadsfritt utvärderingskonto är 4:</span><span class="sxs-lookup"><span data-stu-id="6f934-259">hello following example illustrates that hello core quota for a free trial account is 4:</span></span>

```azurecli
az vm list-usage --location "South Central US"
```

<span data-ttu-id="6f934-260">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="6f934-260">Which returns:</span></span>

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

<span data-ttu-id="6f934-261">Om du distribuerar en mall som skapar fler än fyra kärnor i hello västra USA region, får du ett distribueringsfel som ser ut som:</span><span class="sxs-lookup"><span data-stu-id="6f934-261">If you deploy a template that creates more than four cores in hello West US region, you get a deployment error that looks like:</span></span>

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

<span data-ttu-id="6f934-262">I PowerShell kan du använda hello **Get-AzureRmVMUsage** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6f934-262">Or in PowerShell, you can use hello **Get-AzureRmVMUsage** cmdlet.</span></span>

```powershell
Get-AzureRmVMUsage
```

<span data-ttu-id="6f934-263">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="6f934-263">Which returns:</span></span>

```powershell
...
CurrentValue : 0
Limit        : 4
Name         : {
                 "value": "cores",
                 "localizedValue": "Total Regional Cores"
               }
Unit         : null
...
```

<span data-ttu-id="6f934-264">I dessa fall bör du gå toohello portal och filen en stöd problemet tooraise din kvot för hello region som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="6f934-264">In these cases, you should go toohello portal and file a support issue tooraise your quota for hello region into which you want toodeploy.</span></span>

> [!NOTE]
> <span data-ttu-id="6f934-265">Kom ihåg att för resursgrupper hello kvot för varje enskild region, inte för hello hela prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="6f934-265">Remember that for resource groups, hello quota is for each individual region, not for hello entire subscription.</span></span> <span data-ttu-id="6f934-266">Om du behöver toodeploy 30 kärnor i västra USA kan ha du tooask för 30 Resource Manager kärnor i USA, västra.</span><span class="sxs-lookup"><span data-stu-id="6f934-266">If you need toodeploy 30 cores in West US, you have tooask for 30 Resource Manager cores in West US.</span></span> <span data-ttu-id="6f934-267">Om du behöver toodeploy 30 kärnor i någon av hello regioner toowhich har du åtkomst till, bör du be om 30 Resource Manager kärnor i alla regioner.</span><span class="sxs-lookup"><span data-stu-id="6f934-267">If you need toodeploy 30 cores in any of hello regions toowhich you have access, you should ask for 30 Resource Manager cores in all regions.</span></span>
>
>

## <a name="invalidcontentlink"></a><span data-ttu-id="6f934-268">InvalidContentLink</span><span class="sxs-lookup"><span data-stu-id="6f934-268">InvalidContentLink</span></span>
<span data-ttu-id="6f934-269">När du felmeddelande hello:</span><span class="sxs-lookup"><span data-stu-id="6f934-269">When you receive hello error message:</span></span>

```
Code=InvalidContentLink
Message=Unable toodownload deployment content from ...
```

<span data-ttu-id="6f934-270">Du har troligen försökt toolink tooa kapslad mall som inte är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="6f934-270">You have most likely attempted toolink tooa nested template that is not available.</span></span> <span data-ttu-id="6f934-271">Kontrollera hello URI som du angav för hello kapslade mallen.</span><span class="sxs-lookup"><span data-stu-id="6f934-271">Double check hello URI you provided for hello nested template.</span></span> <span data-ttu-id="6f934-272">Hello-mallen finns i ett lagringskonto, ifall hello URI är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="6f934-272">If hello template exists in a storage account, make sure hello URI is accessible.</span></span> <span data-ttu-id="6f934-273">Du kan behöva toopass en SAS-token.</span><span class="sxs-lookup"><span data-stu-id="6f934-273">You may need toopass a SAS token.</span></span> <span data-ttu-id="6f934-274">Mer information finns i [Använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6f934-274">For more information, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="requestdisallowedbypolicy"></a><span data-ttu-id="6f934-275">RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="6f934-275">RequestDisallowedByPolicy</span></span>
<span data-ttu-id="6f934-276">Du får detta felmeddelande när prenumerationen innehåller en resursprincip som förhindrar att en åtgärd som du försöker tooperform under distributionen.</span><span class="sxs-lookup"><span data-stu-id="6f934-276">You receive this error when your subscription includes a resource policy that prevents an action you are trying tooperform during deployment.</span></span> <span data-ttu-id="6f934-277">Leta efter hello Principidentifierare i hello felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="6f934-277">In hello error message, look for hello policy identifier.</span></span>

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

<span data-ttu-id="6f934-278">I **PowerShell**, ange den Principidentifierare som hello **Id** parametern tooretrieve information om hello-princip som blockerats av din distribution.</span><span class="sxs-lookup"><span data-stu-id="6f934-278">In **PowerShell**, provide that policy identifier as hello **Id** parameter tooretrieve details about hello policy that blocked your deployment.</span></span>

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

<span data-ttu-id="6f934-279">I **Azure CLI**, ange hello namnet på definitionen av hello principen:</span><span class="sxs-lookup"><span data-stu-id="6f934-279">In **Azure CLI**, provide hello name of hello policy definition:</span></span>

```azurecli
az policy definition show --name regionPolicyAssignment
```

<span data-ttu-id="6f934-280">Mer information finns i följande artiklar hello:</span><span class="sxs-lookup"><span data-stu-id="6f934-280">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="6f934-281">Fel med RequestDisallowedByPolicy</span><span class="sxs-lookup"><span data-stu-id="6f934-281">RequestDisallowedByPolicy error</span></span>](resource-manager-policy-requestdisallowedbypolicy-error.md)
- <span data-ttu-id="6f934-282">[Använda princip toomanage resurser och styr åtkomsten](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="6f934-282">[Use Policy toomanage resources and control access](resource-manager-policy.md).</span></span>

## <a name="authorization-failed"></a><span data-ttu-id="6f934-283">Auktoriseringen misslyckades</span><span class="sxs-lookup"><span data-stu-id="6f934-283">Authorization failed</span></span>
<span data-ttu-id="6f934-284">Du får ett fel under distributionen hello konto eller tjänstens huvudnamn försöker toodeploy hello resurser har inte åtkomst tooperform åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="6f934-284">You may receive an error during deployment because hello account or service principal attempting toodeploy hello resources does not have access tooperform those actions.</span></span> <span data-ttu-id="6f934-285">Azure Active Directory kan du eller din administratör toocontrol vilka identiteter kan komma åt vilka resurser med en bra precision.</span><span class="sxs-lookup"><span data-stu-id="6f934-285">Azure Active Directory enables you or your administrator toocontrol which identities can access what resources with a great degree of precision.</span></span> <span data-ttu-id="6f934-286">Till exempel om ditt konto har tilldelats rollen för läsare av toohello, är du inte kan toocreate resurser.</span><span class="sxs-lookup"><span data-stu-id="6f934-286">For example, if your account is assigned toohello Reader role, you are not able toocreate resources.</span></span> <span data-ttu-id="6f934-287">I så fall visas ett felmeddelande om att auktorisering misslyckades.</span><span class="sxs-lookup"><span data-stu-id="6f934-287">In that case, you see an error message indicating that authorization failed.</span></span>

<span data-ttu-id="6f934-288">Mer information om rollbaserad åtkomstkontroll finns [rollbaserad åtkomstkontroll i](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="6f934-288">For more information about role-based access control, see [Azure Role-Based Access Control](../active-directory/role-based-access-control-configure.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="6f934-289">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6f934-289">Next steps</span></span>
* <span data-ttu-id="6f934-290">toolearn om granskning åtgärder, se [granskningsåtgärder med Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="6f934-290">toolearn about auditing actions, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="6f934-291">toolearn om åtgärder toodetermine hello fel under distributionen, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="6f934-291">toolearn about actions toodetermine hello errors during deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
