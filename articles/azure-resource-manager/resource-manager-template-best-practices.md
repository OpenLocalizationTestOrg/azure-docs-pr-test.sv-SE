---
title: "Bästa praxis för att skapa Resource Manager-mallar | Microsoft Docs"
description: "Riktlinjer för att förenkla Azure Resource Manager-mallar."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: a23301ba88279af3f7bf4d353ae808e9eeb0900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a><span data-ttu-id="94df1-103">Metodtips för att skapa Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="94df1-103">Best practices for creating Azure Resource Manager templates</span></span>
<span data-ttu-id="94df1-104">Dessa riktlinjer kan hjälpa dig att skapa Azure Resource Manager-mallar som är tillförlitliga och enkla att använda.</span><span class="sxs-lookup"><span data-stu-id="94df1-104">These guidelines can help you create Azure Resource Manager templates that are reliable and easy to use.</span></span> <span data-ttu-id="94df1-105">Riktlinjerna är bara förslag.</span><span class="sxs-lookup"><span data-stu-id="94df1-105">The guidelines are only suggestions.</span></span> <span data-ttu-id="94df1-106">De är inte krav, om inget annat anges.</span><span class="sxs-lookup"><span data-stu-id="94df1-106">They are not requirements, except where noted.</span></span> <span data-ttu-id="94df1-107">Ditt scenario kan kräva en variant av en av följande metoder eller exempel.</span><span class="sxs-lookup"><span data-stu-id="94df1-107">Your scenario might require a variation of one of the following approaches or examples.</span></span>

## <a name="resource-names"></a><span data-ttu-id="94df1-108">Resursnamn</span><span class="sxs-lookup"><span data-stu-id="94df1-108">Resource names</span></span>
<span data-ttu-id="94df1-109">I allmänhet kan arbeta du med tre typer av resursnamn i Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="94df1-109">Generally, you work with three types of resource names in Resource Manager:</span></span>

* <span data-ttu-id="94df1-110">Resursnamn som måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="94df1-110">Resource names that must be unique.</span></span>
* <span data-ttu-id="94df1-111">Resursnamn som inte behöver vara unika, men välja att ange ett namn som hjälper dig att identifiera en resurs baserat på kontext.</span><span class="sxs-lookup"><span data-stu-id="94df1-111">Resource names that are not required to be unique, but you choose to provide a name that can help you identify a resource based on context.</span></span>
* <span data-ttu-id="94df1-112">Resursnamn som kan vara generiska.</span><span class="sxs-lookup"><span data-stu-id="94df1-112">Resource names that can be generic.</span></span>

 <span data-ttu-id="94df1-113">Information om begränsningar för resursen finns [rekommenderas namnkonventionerna för Azure-resurser](../guidance/guidance-naming-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="94df1-113">For information about resource name restrictions, see [Recommended naming conventions for Azure resources](../guidance/guidance-naming-conventions.md).</span></span>

### <a name="unique-resource-names"></a><span data-ttu-id="94df1-114">Unikt namn</span><span class="sxs-lookup"><span data-stu-id="94df1-114">Unique resource names</span></span>
<span data-ttu-id="94df1-115">Du måste ange ett unikt resursnamn för någon resurstyp av som har en slutpunkt för åtkomst av data.</span><span class="sxs-lookup"><span data-stu-id="94df1-115">You must provide a unique resource name for any resource type that has a data access endpoint.</span></span> <span data-ttu-id="94df1-116">Några vanliga typer av resurser som kräver ett unikt namn är:</span><span class="sxs-lookup"><span data-stu-id="94df1-116">Some common resource types that require a unique name include:</span></span>

* <span data-ttu-id="94df1-117">Azure Storage<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="94df1-117">Azure Storage<sup>1</sup></span></span> 
* <span data-ttu-id="94df1-118">Funktionen Web Apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="94df1-118">Web Apps feature of Azure App Service</span></span>
* <span data-ttu-id="94df1-119">SQL Server</span><span class="sxs-lookup"><span data-stu-id="94df1-119">SQL Server</span></span>
* <span data-ttu-id="94df1-120">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="94df1-120">Azure Key Vault</span></span>
* <span data-ttu-id="94df1-121">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="94df1-121">Azure Redis Cache</span></span>
* <span data-ttu-id="94df1-122">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="94df1-122">Azure Batch</span></span>
* <span data-ttu-id="94df1-123">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="94df1-123">Azure Traffic Manager</span></span>
* <span data-ttu-id="94df1-124">Azure Search</span><span class="sxs-lookup"><span data-stu-id="94df1-124">Azure Search</span></span>
* <span data-ttu-id="94df1-125">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="94df1-125">Azure HDInsight</span></span>

<span data-ttu-id="94df1-126"><sup>1</sup> lagringskontonamn måste också vara gemener, 24 tecken eller mindre, och inte har någon bindestreck.</span><span class="sxs-lookup"><span data-stu-id="94df1-126"><sup>1</sup> Storage account names also must be lowercase, 24 characters or less, and not have any hyphens.</span></span>

<span data-ttu-id="94df1-127">Om du anger en parameter för en resurs måste ange du ett unikt namn när du distribuerar resursen.</span><span class="sxs-lookup"><span data-stu-id="94df1-127">If you provide a parameter for a resource name, you must provide a unique name when you deploy the resource.</span></span> <span data-ttu-id="94df1-128">Du kan skapa en variabel som använder den [uniqueString()](resource-group-template-functions-string.md#uniquestring) funktion för att generera ett namn.</span><span class="sxs-lookup"><span data-stu-id="94df1-128">Optionally, you can create a variable that uses the [uniqueString()](resource-group-template-functions-string.md#uniquestring) function to generate a name.</span></span> 

<span data-ttu-id="94df1-129">Du kan också lägga till ett prefix eller suffix till den **uniqueString** resultat.</span><span class="sxs-lookup"><span data-stu-id="94df1-129">You also might want to add a prefix or suffix to the **uniqueString** result.</span></span> <span data-ttu-id="94df1-130">Ändra det unika namnet hjälper dig att enkelt identifiera resurstypen från namn.</span><span class="sxs-lookup"><span data-stu-id="94df1-130">Modifying the unique name can help you more easily identify the resource type from the name.</span></span> <span data-ttu-id="94df1-131">Du kan till exempel generera ett unikt namn för ett lagringskonto med hjälp av följande variabel:</span><span class="sxs-lookup"><span data-stu-id="94df1-131">For example, you can generate a unique name for a storage account by using the following variable:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a><span data-ttu-id="94df1-132">Resursnamn för identifiering</span><span class="sxs-lookup"><span data-stu-id="94df1-132">Resource names for identification</span></span>
<span data-ttu-id="94df1-133">Vissa typer av resurser som du kanske vill namn, men deras namn behöver inte vara unika.</span><span class="sxs-lookup"><span data-stu-id="94df1-133">Some resource types you might want to name, but their names do not have to be unique.</span></span> <span data-ttu-id="94df1-134">Du kan ange ett namn som identifierar både resurs-kontexten och resurstypen för dessa typer av resurser.</span><span class="sxs-lookup"><span data-stu-id="94df1-134">For these resource types, you can provide a name that identifies both the resource context and the resource type.</span></span> <span data-ttu-id="94df1-135">Ange ett beskrivande namn som hjälper dig att identifiera resursen i en lista över resurser.</span><span class="sxs-lookup"><span data-stu-id="94df1-135">Provide a descriptive name that helps you identify the resource in a list of resources.</span></span> <span data-ttu-id="94df1-136">Om du behöver använda ett annat resursnamn för olika distributioner kan du använda en parameter för namnet:</span><span class="sxs-lookup"><span data-stu-id="94df1-136">If you need to use a different resource name for different deployments, you can use a parameter for the name:</span></span>

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

<span data-ttu-id="94df1-137">Om du inte behöver ange ett namn under distributionen kan du använda en variabel:</span><span class="sxs-lookup"><span data-stu-id="94df1-137">If you do not need to pass in a name during deployment, you can use a variable:</span></span> 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

<span data-ttu-id="94df1-138">Du kan också använda ett hårdkodat värde:</span><span class="sxs-lookup"><span data-stu-id="94df1-138">You also can use a hard-coded value:</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a><span data-ttu-id="94df1-139">Allmänt namn</span><span class="sxs-lookup"><span data-stu-id="94df1-139">Generic resource names</span></span>
<span data-ttu-id="94df1-140">Du kan använda ett allmänt namn som är hårdkodat i mallen för resurstyper som främst öppnas från en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="94df1-140">For resource types that you mostly access through a different resource, you can use a generic name that is hard-coded in the template.</span></span> <span data-ttu-id="94df1-141">Du kan till exempel ange ett standard, allmän namn på brandväggsregler på en SQLServer:</span><span class="sxs-lookup"><span data-stu-id="94df1-141">For example, you can set a standard, generic name for firewall rules on a SQL server:</span></span>

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a><span data-ttu-id="94df1-142">Parametrar</span><span class="sxs-lookup"><span data-stu-id="94df1-142">Parameters</span></span>
<span data-ttu-id="94df1-143">Följande information kan vara användbart när du arbetar med parametrar:</span><span class="sxs-lookup"><span data-stu-id="94df1-143">The following information can be helpful when you work with parameters:</span></span>

* <span data-ttu-id="94df1-144">Minimera din användning av parametrar.</span><span class="sxs-lookup"><span data-stu-id="94df1-144">Minimize your use of parameters.</span></span> <span data-ttu-id="94df1-145">Använd om möjligt en variabel eller ett litteralvärde.</span><span class="sxs-lookup"><span data-stu-id="94df1-145">Whenever possible, use a variable or a literal value.</span></span> <span data-ttu-id="94df1-146">Använd parametrarna endast för dessa scenarier:</span><span class="sxs-lookup"><span data-stu-id="94df1-146">Use parameters only for these scenarios:</span></span>
   
   * <span data-ttu-id="94df1-147">Inställningar som du vill använda variationer av enligt miljö (SKU, storlek, kapacitet).</span><span class="sxs-lookup"><span data-stu-id="94df1-147">Settings that you want to use variations of according to environment (SKU, size, capacity).</span></span>
   * <span data-ttu-id="94df1-148">Resursnamn som du vill ange för enkel identifiering.</span><span class="sxs-lookup"><span data-stu-id="94df1-148">Resource names that you want to specify for easy identification.</span></span>
   * <span data-ttu-id="94df1-149">Värden som du använder ofta för att utföra andra åtgärder (till exempel en administratörsanvändarnamnet).</span><span class="sxs-lookup"><span data-stu-id="94df1-149">Values that you use frequently to complete other tasks (such as an admin user name).</span></span>
   * <span data-ttu-id="94df1-150">Hemligheter (till exempel lösenord).</span><span class="sxs-lookup"><span data-stu-id="94df1-150">Secrets (such as passwords).</span></span>
   * <span data-ttu-id="94df1-151">Det antal eller matris med-värden ska användas när du skapar flera instanser av en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="94df1-151">The number or array of values to use when you create multiple instances of a resource type.</span></span>
* <span data-ttu-id="94df1-152">Slitbanor användningsfall för parameternamn.</span><span class="sxs-lookup"><span data-stu-id="94df1-152">Use camel case for parameter names.</span></span>
* <span data-ttu-id="94df1-153">Ange en beskrivning av varje parameter i metadata:</span><span class="sxs-lookup"><span data-stu-id="94df1-153">Provide a description of every parameter in the metadata:</span></span>

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* <span data-ttu-id="94df1-154">Ange standardvärden för parametrar (förutom för lösenord och SSH-nycklar):</span><span class="sxs-lookup"><span data-stu-id="94df1-154">Define default values for parameters (except for passwords and SSH keys):</span></span>
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* <span data-ttu-id="94df1-155">Använd **SecureString** för alla lösenord och hemligheter:</span><span class="sxs-lookup"><span data-stu-id="94df1-155">Use **SecureString** for all passwords and secrets:</span></span> 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* <span data-ttu-id="94df1-156">Använd inte en parameter anger platsen när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="94df1-156">Whenever possible, don't use a parameter to specify location.</span></span> <span data-ttu-id="94df1-157">Använd i stället de **plats** -egenskapen för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="94df1-157">Instead, use the **location** property of the resource group.</span></span> <span data-ttu-id="94df1-158">Med hjälp av den **resourceGroup () .location** uttryck för alla dina resurser, resurserna i mallen distribueras på samma plats som resursgruppen:</span><span class="sxs-lookup"><span data-stu-id="94df1-158">By using the **resourceGroup().location** expression for all your resources, resources in the template are deployed in the same location as the resource group:</span></span>
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   <span data-ttu-id="94df1-159">Om en resurstyp stöds i endast ett begränsat antal platser, kanske du vill ange en giltig plats direkt i mallen.</span><span class="sxs-lookup"><span data-stu-id="94df1-159">If a resource type is supported in only a limited number of locations, you might want to specify a valid location directly in the template.</span></span> <span data-ttu-id="94df1-160">Om du måste använda en **plats** parameter, dela att parametervärdet så mycket som möjligt med resurser som kan förväntas finnas på samma plats.</span><span class="sxs-lookup"><span data-stu-id="94df1-160">If you must use a **location** parameter, share that parameter value as much as possible with resources that are likely to be in the same location.</span></span> <span data-ttu-id="94df1-161">Detta minskar antalet gånger som användare uppmanas att ange platsinformation.</span><span class="sxs-lookup"><span data-stu-id="94df1-161">This minimizes the number of times users are asked to provide location information.</span></span>
* <span data-ttu-id="94df1-162">Undvik att använda en parameter eller variabel för API-version för en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="94df1-162">Avoid using a parameter or variable for the API version for a resource type.</span></span> <span data-ttu-id="94df1-163">Egenskaper för resursen och värdena kan variera efter versionsnummer.</span><span class="sxs-lookup"><span data-stu-id="94df1-163">Resource properties and values can vary by version number.</span></span> <span data-ttu-id="94df1-164">IntelliSense i en redigerare kan inte avgöra korrekt schema när API-version anges som en parameter eller variabel.</span><span class="sxs-lookup"><span data-stu-id="94df1-164">IntelliSense in a code editor cannot determine the correct schema when the API version is set to a parameter or variable.</span></span> <span data-ttu-id="94df1-165">I stället hårdkoda API-version i mallen.</span><span class="sxs-lookup"><span data-stu-id="94df1-165">Instead, hard-code the API version in the template.</span></span>

## <a name="variables"></a><span data-ttu-id="94df1-166">Variabler</span><span class="sxs-lookup"><span data-stu-id="94df1-166">Variables</span></span>
<span data-ttu-id="94df1-167">Följande information kan vara användbart när du arbetar med variabler:</span><span class="sxs-lookup"><span data-stu-id="94df1-167">The following information can be helpful when you work with variables:</span></span>

* <span data-ttu-id="94df1-168">Använda variabler för värden som du behöver använda mer än en gång i en mall.</span><span class="sxs-lookup"><span data-stu-id="94df1-168">Use variables for values that you need to use more than once in a template.</span></span> <span data-ttu-id="94df1-169">Om ett värde används bara en gång, ett hårdkodat värde gör mallen lättare att läsa.</span><span class="sxs-lookup"><span data-stu-id="94df1-169">If a value is used only once, a hard-coded value makes your template easier to read.</span></span>
* <span data-ttu-id="94df1-170">Du kan inte använda den [referens](resource-group-template-functions-resource.md#reference) fungera i den **variabler** avsnitt i mallen.</span><span class="sxs-lookup"><span data-stu-id="94df1-170">You cannot use the [reference](resource-group-template-functions-resource.md#reference) function in the **variables** section of the template.</span></span> <span data-ttu-id="94df1-171">Den **referens** funktionen hämtar sitt värde från resursens runtime-tillståndet.</span><span class="sxs-lookup"><span data-stu-id="94df1-171">The **reference** function derives its value from the resource's runtime state.</span></span> <span data-ttu-id="94df1-172">Variabler är dock lösts vid första parsningen av mallen.</span><span class="sxs-lookup"><span data-stu-id="94df1-172">However, variables are resolved during the initial parsing of the template.</span></span> <span data-ttu-id="94df1-173">Konstruktion värden som måste den **referens** fungera direkt i den **resurser** eller **matar ut** avsnitt i mallen.</span><span class="sxs-lookup"><span data-stu-id="94df1-173">Construct values that need the **reference** function directly in the **resources** or **outputs** section of the template.</span></span>
* <span data-ttu-id="94df1-174">Variabler för resursnamn som måste vara unika, enligt beskrivningen i [resursnamn](#resource-names).</span><span class="sxs-lookup"><span data-stu-id="94df1-174">Include variables for resource names that must be unique, as described in [Resource names](#resource-names).</span></span>
* <span data-ttu-id="94df1-175">Du kan gruppera variabler i komplexa objekt.</span><span class="sxs-lookup"><span data-stu-id="94df1-175">You can group variables into complex objects.</span></span> <span data-ttu-id="94df1-176">Använd den **variable.subentry** format för att referera till ett värde från ett komplext objekt.</span><span class="sxs-lookup"><span data-stu-id="94df1-176">Use the **variable.subentry** format to reference a value from a complex object.</span></span> <span data-ttu-id="94df1-177">Gruppera variabler kan hjälpa dig att spåra relaterade variabler.</span><span class="sxs-lookup"><span data-stu-id="94df1-177">Grouping variables can help you track related variables.</span></span> <span data-ttu-id="94df1-178">Det förbättrar också läsbarhet för mallen.</span><span class="sxs-lookup"><span data-stu-id="94df1-178">It also improves readability of the template.</span></span> <span data-ttu-id="94df1-179">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="94df1-179">Here's an example:</span></span>
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > <span data-ttu-id="94df1-180">Ett komplext objekt får inte innehålla ett uttryck som refererar till ett värde från ett komplext objekt.</span><span class="sxs-lookup"><span data-stu-id="94df1-180">A complex object cannot contain an expression that references a value from a complex object.</span></span> <span data-ttu-id="94df1-181">Definiera en separat variabel för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="94df1-181">Define a separate variable for this purpose.</span></span>
   > 
   > 
   
     <span data-ttu-id="94df1-182">Avancerade exempel på hur komplexa objekt som variabler finns i [dela tillstånd i Azure Resource Manager-mallar](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="94df1-182">For advanced examples of using complex objects as variables, see [Share state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="resources"></a><span data-ttu-id="94df1-183">Resurser</span><span class="sxs-lookup"><span data-stu-id="94df1-183">Resources</span></span>
<span data-ttu-id="94df1-184">Följande information kan vara användbart när du arbetar med resurser:</span><span class="sxs-lookup"><span data-stu-id="94df1-184">The following information can be helpful when you work with resources:</span></span>

* <span data-ttu-id="94df1-185">För att hjälpa andra deltagare förstå syftet med resursen, ange **kommentarer** för varje resurs i mallen:</span><span class="sxs-lookup"><span data-stu-id="94df1-185">To help other contributors understand the purpose of the resource, specify **comments** for each resource in the template:</span></span>
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* <span data-ttu-id="94df1-186">Du kan använda taggar för att lägga till metadata till resurser.</span><span class="sxs-lookup"><span data-stu-id="94df1-186">You can use tags to add metadata to resources.</span></span> <span data-ttu-id="94df1-187">Använda metadata för att lägga till information om dina resurser.</span><span class="sxs-lookup"><span data-stu-id="94df1-187">Use metadata to add information about your resources.</span></span> <span data-ttu-id="94df1-188">Du kan till exempel lägga till metadata för att registrera faktureringsinformation för en resurs.</span><span class="sxs-lookup"><span data-stu-id="94df1-188">For example, you can add metadata to record billing details for a resource.</span></span> <span data-ttu-id="94df1-189">Mer information finns i [med taggar för att organisera dina Azure-resurser](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="94df1-189">For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).</span></span>
* <span data-ttu-id="94df1-190">Om du använder en *offentlig slutpunkt* i mallen (till exempel Azure Blob storage offentlig slutpunkt), *gör inte hårdkoda* namnområdet.</span><span class="sxs-lookup"><span data-stu-id="94df1-190">If you use a *public endpoint* in your template (such as an Azure Blob storage public endpoint), *do not hard-code* the namespace.</span></span> <span data-ttu-id="94df1-191">Använd den **referens** funktion för att dynamiskt hämta namnområdet.</span><span class="sxs-lookup"><span data-stu-id="94df1-191">Use the **reference** function to dynamically retrieve the namespace.</span></span> <span data-ttu-id="94df1-192">Du kan använda den här metoden för att distribuera mallen till olika namnområdes-offentliga miljöer utan att manuellt ändra slutpunkten i mallen.</span><span class="sxs-lookup"><span data-stu-id="94df1-192">You can use this approach to deploy the template to different public namespace environments without manually changing the endpoint in the template.</span></span> <span data-ttu-id="94df1-193">Ange API-versionen till samma version som du använder för lagringskontot i mallen:</span><span class="sxs-lookup"><span data-stu-id="94df1-193">Set the API version to the same version that you are using for the storage account in your template:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="94df1-194">Om lagringskontot distribueras i samma mall som du skapar, behöver du inte ange leverantörens namnrymd när du refererar till resursen.</span><span class="sxs-lookup"><span data-stu-id="94df1-194">If the storage account is deployed in the same template that you are creating, you do not need to specify the provider namespace when you reference the resource.</span></span> <span data-ttu-id="94df1-195">Detta är förenklad syntax:</span><span class="sxs-lookup"><span data-stu-id="94df1-195">This is the simplified syntax:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="94df1-196">Om du har andra värden i mallen som är konfigurerade för att använda en offentlig namnrymd kan ändra dessa värden för att återspegla samma **referens** funktion.</span><span class="sxs-lookup"><span data-stu-id="94df1-196">If you have other values in your template that are configured to use a public namespace, change these values to reflect the same **reference** function.</span></span> <span data-ttu-id="94df1-197">Du kan till exempel ange det **storageUri** -egenskapen för den virtuella dator diagnostikprofilen:</span><span class="sxs-lookup"><span data-stu-id="94df1-197">For example, you can set the **storageUri** property of the virtual machine diagnostics profile:</span></span>
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   <span data-ttu-id="94df1-198">Du kan också referera till ett befintligt lagringskonto i en annan resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="94df1-198">You also can reference an existing storage account that is in a different resource group:</span></span>

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* <span data-ttu-id="94df1-199">Tilldela offentliga IP-adresser till en virtuell dator endast när ett program kräver.</span><span class="sxs-lookup"><span data-stu-id="94df1-199">Assign public IP addresses to a virtual machine only when an application requires it.</span></span> <span data-ttu-id="94df1-200">Använd ingående NAT-regler, en virtuell nätverksgateway eller en jumpbox för att ansluta till en virtuell dator (VM) för felsökning eller för att hantera administrativa syften.</span><span class="sxs-lookup"><span data-stu-id="94df1-200">To connect to a virtual machine (VM) for debugging, or for management or administrative purposes, use inbound NAT rules, a virtual network gateway, or a jumpbox.</span></span>
   
     <span data-ttu-id="94df1-201">Mer information om hur du ansluter till virtuella datorer finns:</span><span class="sxs-lookup"><span data-stu-id="94df1-201">For more information about connecting to virtual machines, see:</span></span>
   
   * [<span data-ttu-id="94df1-202">Köra virtuella datorer för en arkitektur med flera nivåer i Azure</span><span class="sxs-lookup"><span data-stu-id="94df1-202">Run VMs for an N-tier architecture in Azure</span></span>](../guidance/guidance-compute-n-tier-vm.md)
   * [<span data-ttu-id="94df1-203">Konfigurera WinRM-åtkomst för virtuella datorer i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="94df1-203">Set up WinRM access for VMs in Azure Resource Manager</span></span>](../virtual-machines/windows/winrm.md)
   * [<span data-ttu-id="94df1-204">Tillåt extern åtkomst till den virtuella datorn med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="94df1-204">Allow external access to your VM by using the Azure portal</span></span>](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [<span data-ttu-id="94df1-205">Tillåt extern åtkomst till den virtuella datorn med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="94df1-205">Allow external access to your VM by using PowerShell</span></span>](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [<span data-ttu-id="94df1-206">Ge extern åtkomst till din Linux VM med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="94df1-206">Allow external access to your Linux VM by using Azure CLI</span></span>](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* <span data-ttu-id="94df1-207">Den **domainNameLabel** -egenskapen för offentliga IP-adresser måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="94df1-207">The **domainNameLabel** property for public IP addresses must be unique.</span></span> <span data-ttu-id="94df1-208">Den **domainNameLabel** värdet måste vara mellan 3 och 63 tecken långt och följa de regler som anges av den här reguljärt uttryck: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span><span class="sxs-lookup"><span data-stu-id="94df1-208">The **domainNameLabel** value must be between 3 and 63 characters long, and follow the rules specified by this regular expression: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span></span> <span data-ttu-id="94df1-209">Eftersom den **uniqueString** funktionen genererar en sträng som är 13 tecken lång, den **dnsPrefixString** parameter är begränsad till 50 tecken:</span><span class="sxs-lookup"><span data-stu-id="94df1-209">Because the **uniqueString** function generates a string that is 13 characters long, the **dnsPrefixString** parameter is limited to 50 characters:</span></span>

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* <span data-ttu-id="94df1-210">När du lägger till ett lösenord för tillägget för anpassat skript kan använda den **commandToExecute** egenskap i den **protectedSettings** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="94df1-210">When you add a password to a custom script extension, use the **commandToExecute** property in the **protectedSettings** property:</span></span>
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > <span data-ttu-id="94df1-211">Använd för att säkerställa att hemligheter krypteras när de skickas som parametrar till virtuella datorer och tillägg i **protectedSettings** -egenskapen för de relevanta tillägg.</span><span class="sxs-lookup"><span data-stu-id="94df1-211">To ensure that secrets are encrypted when they are passed as parameters to VMs and extensions, use the **protectedSettings** property of the relevant extensions.</span></span>
   > 
   > 

## <a name="outputs"></a><span data-ttu-id="94df1-212">utdata</span><span class="sxs-lookup"><span data-stu-id="94df1-212">Outputs</span></span>
<span data-ttu-id="94df1-213">Om du använder en mall för att skapa offentliga IP-adresser, inkludera en **matar ut** avsnitt som returnerar information om IP-adressen och det fullständigt kvalificerade domännamnet (FQDN).</span><span class="sxs-lookup"><span data-stu-id="94df1-213">If you use a template to create public IP addresses, include an **outputs** section that returns details of the IP address and the fully qualified domain name (FQDN).</span></span> <span data-ttu-id="94df1-214">Du kan använda utdatavärden för att lätt kunna hämta information om offentlig IP-adresser och FQDN efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="94df1-214">You can use output values to easily retrieve details about public IP addresses and FQDNs after deployment.</span></span> <span data-ttu-id="94df1-215">När du refererar till resursen, kan du använda API-version som du använde för att skapa den:</span><span class="sxs-lookup"><span data-stu-id="94df1-215">When you reference the resource, use the API version that you used to create it:</span></span> 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a><span data-ttu-id="94df1-216">Samma mall kontra kapslade mallar</span><span class="sxs-lookup"><span data-stu-id="94df1-216">Single template vs. nested templates</span></span>
<span data-ttu-id="94df1-217">För att distribuera lösningen måste använda du en mall eller en Huvudmall med flera kapslade mallar.</span><span class="sxs-lookup"><span data-stu-id="94df1-217">To deploy your solution, you can use either a single template or a main template with multiple nested templates.</span></span> <span data-ttu-id="94df1-218">Kapslade mallar är gemensamma för mer avancerade scenarier.</span><span class="sxs-lookup"><span data-stu-id="94df1-218">Nested templates are common for more advanced scenarios.</span></span> <span data-ttu-id="94df1-219">Med hjälp av en kapslad mall ger följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="94df1-219">Using a nested template gives you the following advantages:</span></span>

* <span data-ttu-id="94df1-220">Du kan dela upp en lösning i riktade komponenter.</span><span class="sxs-lookup"><span data-stu-id="94df1-220">You can break down a solution into targeted components.</span></span>
* <span data-ttu-id="94df1-221">Du kan återanvända kapslade mallar med olika huvudsakliga mallar.</span><span class="sxs-lookup"><span data-stu-id="94df1-221">You can reuse nested templates with different main templates.</span></span>

<span data-ttu-id="94df1-222">Om du väljer att använda kapslade mallar kan följande riktlinjer hjälpa dig standardisera utformningen mallen.</span><span class="sxs-lookup"><span data-stu-id="94df1-222">If you choose to use nested templates, the following guidelines can help you standardize your template design.</span></span> <span data-ttu-id="94df1-223">Dessa riktlinjer är baserade på [mönster för att utforma Azure Resource Manager-mallar](best-practices-resource-manager-design-templates.md).</span><span class="sxs-lookup"><span data-stu-id="94df1-223">These guidelines are based on [patterns for designing Azure Resource Manager templates](best-practices-resource-manager-design-templates.md).</span></span> <span data-ttu-id="94df1-224">Vi rekommenderar en design som har följande mallar:</span><span class="sxs-lookup"><span data-stu-id="94df1-224">We recommend a design that has the following templates:</span></span>

* <span data-ttu-id="94df1-225">**Huvudmall** (azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="94df1-225">**Main template** (azuredeploy.json).</span></span> <span data-ttu-id="94df1-226">Använd för indataparametrarna.</span><span class="sxs-lookup"><span data-stu-id="94df1-226">Use for the input parameters.</span></span>
* <span data-ttu-id="94df1-227">**Delade resurser mallen**.</span><span class="sxs-lookup"><span data-stu-id="94df1-227">**Shared resources template**.</span></span> <span data-ttu-id="94df1-228">Använda för att distribuera delade resurser med andra resurser (till exempel virtuella nätverk och tillgänglighet uppsättningar).</span><span class="sxs-lookup"><span data-stu-id="94df1-228">Use to deploy shared resources that all other resources use (for example, virtual network and availability sets).</span></span> <span data-ttu-id="94df1-229">Använd den **dependsOn** uttrycket så att den här mallen distribueras innan andra mallar.</span><span class="sxs-lookup"><span data-stu-id="94df1-229">Use the **dependsOn** expression to ensure that this template is deployed before other templates.</span></span>
* <span data-ttu-id="94df1-230">**Valfria resurser mallen**.</span><span class="sxs-lookup"><span data-stu-id="94df1-230">**Optional resources template**.</span></span> <span data-ttu-id="94df1-231">Använda för att distribuera villkorligt resurser baserat på en parameter (till exempel jumpbox).</span><span class="sxs-lookup"><span data-stu-id="94df1-231">Use to conditionally deploy resources based on a parameter (for example, a jumpbox).</span></span>
* <span data-ttu-id="94df1-232">**Medlemmen resurser mallen**.</span><span class="sxs-lookup"><span data-stu-id="94df1-232">**Member resources template**.</span></span> <span data-ttu-id="94df1-233">Varje instans i en program-nivå har sin egen konfiguration.</span><span class="sxs-lookup"><span data-stu-id="94df1-233">Each instance type within an application tier has its own configuration.</span></span> <span data-ttu-id="94df1-234">Du kan definiera olika instans typer i en nivå.</span><span class="sxs-lookup"><span data-stu-id="94df1-234">Within a tier, you can define different instance types.</span></span> <span data-ttu-id="94df1-235">(Till exempel om den första instansen skapar ett kluster och ytterligare instanser läggs till det befintliga klustret.) Varje instans har sin egen Distributionsmall.</span><span class="sxs-lookup"><span data-stu-id="94df1-235">(For example, the first instance creates a cluster, and additional instances are added to the existing cluster.) Each instance type has its own deployment template.</span></span>
* <span data-ttu-id="94df1-236">**Skript**.</span><span class="sxs-lookup"><span data-stu-id="94df1-236">**Scripts**.</span></span> <span data-ttu-id="94df1-237">Allmänt återanvändbara skript kan användas för varje instanstyp (till exempel initiera och formatera ytterligare diskar).</span><span class="sxs-lookup"><span data-stu-id="94df1-237">Widely reusable scripts are applicable for each instance type (for example, initialize and format additional disks).</span></span> <span data-ttu-id="94df1-238">Anpassade skript som du skapar för en viss anpassning syfte är olika, baserat på typen instans.</span><span class="sxs-lookup"><span data-stu-id="94df1-238">Custom scripts that you create for a specific customization purpose are different, based on the instance type.</span></span>

![Kapslad mall](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

<span data-ttu-id="94df1-240">Mer information finns i [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="94df1-240">For more information, see [Use linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="conditionally-link-to-nested-templates"></a><span data-ttu-id="94df1-241">Villkorligt länk till kapslade mallar</span><span class="sxs-lookup"><span data-stu-id="94df1-241">Conditionally link to nested templates</span></span>
<span data-ttu-id="94df1-242">Du kan använda en parameter för att länka villkorligt till kapslade mallar.</span><span class="sxs-lookup"><span data-stu-id="94df1-242">You can use a parameter to conditionally link to nested templates.</span></span> <span data-ttu-id="94df1-243">Parametern blir en del av URI: N för mallen:</span><span class="sxs-lookup"><span data-stu-id="94df1-243">The parameter becomes part of the URI for the template:</span></span>

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a><span data-ttu-id="94df1-244">Mallformat</span><span class="sxs-lookup"><span data-stu-id="94df1-244">Template format</span></span>
<span data-ttu-id="94df1-245">Det är en bra idé att skicka din mall med en JSON-verifieraren.</span><span class="sxs-lookup"><span data-stu-id="94df1-245">It's a good practice to pass your template through a JSON validator.</span></span> <span data-ttu-id="94df1-246">Med hjälp av en systemhälsoverifierare kan du ta bort överflödig kommatecken, parenteser och hakparenteser som kan orsaka fel under distributionen.</span><span class="sxs-lookup"><span data-stu-id="94df1-246">A validator can help you remove extraneous commas, parentheses, and brackets that might cause an error during deployment.</span></span> <span data-ttu-id="94df1-247">Försök [JSONLint](http://jsonlint.com/) eller ett linter paket för din favorit redigerar miljö (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="94df1-247">Try [JSONLint](http://jsonlint.com/) or a linter package for your favorite editing environment (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span></span>

<span data-ttu-id="94df1-248">Det är också en bra idé att formatera din JSON för bättre läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="94df1-248">It's also a good idea to format your JSON for better readability.</span></span> <span data-ttu-id="94df1-249">Du kan använda en JSON-Formateraren paketet för din lokala redigeraren.</span><span class="sxs-lookup"><span data-stu-id="94df1-249">You can use a JSON formatter package for your local editor.</span></span> <span data-ttu-id="94df1-250">I Visual Studio för att formatera dokumentet trycker du på **Ctrl + K, Ctrl + D**.</span><span class="sxs-lookup"><span data-stu-id="94df1-250">In Visual Studio, to format the document, press **Ctrl+K, Ctrl+D**.</span></span> <span data-ttu-id="94df1-251">I Visual Studio-koden trycker du på **Alt + Skift + F**.</span><span class="sxs-lookup"><span data-stu-id="94df1-251">In Visual Studio Code, press **Alt+Shift+F**.</span></span> <span data-ttu-id="94df1-252">Om din lokala redigeraren inte formateras dokumentet, kan du använda en [online formaterare](https://www.bing.com/search?q=json+formatter).</span><span class="sxs-lookup"><span data-stu-id="94df1-252">If your local editor doesn't format the document, you can use an [online formatter](https://www.bing.com/search?q=json+formatter).</span></span>

## <a name="next-steps"></a><span data-ttu-id="94df1-253">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94df1-253">Next steps</span></span>
* <span data-ttu-id="94df1-254">Anvisningar om att utforma din lösning för virtuella datorer, se [kör en virtuell Windows-dator i Azure](../guidance/guidance-compute-single-vm.md) och [kör ett Linux-VM i Azure](../guidance/guidance-compute-single-vm-linux.md).</span><span class="sxs-lookup"><span data-stu-id="94df1-254">For guidance on architecting your solution for virtual machines, see [Run a Windows VM in Azure](../guidance/guidance-compute-single-vm.md) and [Run a Linux VM in Azure](../guidance/guidance-compute-single-vm-linux.md).</span></span>
* <span data-ttu-id="94df1-255">Anvisningar om hur du skapar ett lagringskonto finns [Azure Storage checklistan för prestanda och skalbarhet](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="94df1-255">For guidance on setting up a storage account, see [Azure Storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>
* <span data-ttu-id="94df1-256">Läs om hur företaget kan använda Resource Manager för att effektivt hantera prenumerationer i [Azure enterprise kodskelett: normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="94df1-256">To learn about how an enterprise can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold: Prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

