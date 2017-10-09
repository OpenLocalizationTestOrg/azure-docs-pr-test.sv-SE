---
title: "aaaBest metoder för att skapa Resource Manager-mallar | Microsoft Docs"
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
ms.openlocfilehash: ec9bbe218c4f2c6a92ca44b5e9c9c71029e22151
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a><span data-ttu-id="56181-103">Metodtips för att skapa Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="56181-103">Best practices for creating Azure Resource Manager templates</span></span>
<span data-ttu-id="56181-104">Dessa riktlinjer kan hjälpa dig att skapa Azure Resource Manager-mallar som är tillförlitlig och enkel toouse.</span><span class="sxs-lookup"><span data-stu-id="56181-104">These guidelines can help you create Azure Resource Manager templates that are reliable and easy toouse.</span></span> <span data-ttu-id="56181-105">hello riktlinjer är bara förslag.</span><span class="sxs-lookup"><span data-stu-id="56181-105">hello guidelines are only suggestions.</span></span> <span data-ttu-id="56181-106">De är inte krav, om inget annat anges.</span><span class="sxs-lookup"><span data-stu-id="56181-106">They are not requirements, except where noted.</span></span> <span data-ttu-id="56181-107">Ditt scenario kan kräva en variant av en av hello följande metoder eller exempel.</span><span class="sxs-lookup"><span data-stu-id="56181-107">Your scenario might require a variation of one of hello following approaches or examples.</span></span>

## <a name="resource-names"></a><span data-ttu-id="56181-108">Resursnamn</span><span class="sxs-lookup"><span data-stu-id="56181-108">Resource names</span></span>
<span data-ttu-id="56181-109">I allmänhet kan arbeta du med tre typer av resursnamn i Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="56181-109">Generally, you work with three types of resource names in Resource Manager:</span></span>

* <span data-ttu-id="56181-110">Resursnamn som måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="56181-110">Resource names that must be unique.</span></span>
* <span data-ttu-id="56181-111">Resursnamn som inte krävs toobe unika, men du väljer tooprovide ett namn som hjälper dig att identifiera en resurs baserat på kontext.</span><span class="sxs-lookup"><span data-stu-id="56181-111">Resource names that are not required toobe unique, but you choose tooprovide a name that can help you identify a resource based on context.</span></span>
* <span data-ttu-id="56181-112">Resursnamn som kan vara generiska.</span><span class="sxs-lookup"><span data-stu-id="56181-112">Resource names that can be generic.</span></span>

 <span data-ttu-id="56181-113">Information om begränsningar för resursen finns [rekommenderas namnkonventionerna för Azure-resurser](../guidance/guidance-naming-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="56181-113">For information about resource name restrictions, see [Recommended naming conventions for Azure resources](../guidance/guidance-naming-conventions.md).</span></span>

### <a name="unique-resource-names"></a><span data-ttu-id="56181-114">Unikt namn</span><span class="sxs-lookup"><span data-stu-id="56181-114">Unique resource names</span></span>
<span data-ttu-id="56181-115">Du måste ange ett unikt resursnamn för någon resurstyp av som har en slutpunkt för åtkomst av data.</span><span class="sxs-lookup"><span data-stu-id="56181-115">You must provide a unique resource name for any resource type that has a data access endpoint.</span></span> <span data-ttu-id="56181-116">Några vanliga typer av resurser som kräver ett unikt namn är:</span><span class="sxs-lookup"><span data-stu-id="56181-116">Some common resource types that require a unique name include:</span></span>

* <span data-ttu-id="56181-117">Azure Storage<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="56181-117">Azure Storage<sup>1</sup></span></span> 
* <span data-ttu-id="56181-118">Funktionen Web Apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="56181-118">Web Apps feature of Azure App Service</span></span>
* <span data-ttu-id="56181-119">SQL Server</span><span class="sxs-lookup"><span data-stu-id="56181-119">SQL Server</span></span>
* <span data-ttu-id="56181-120">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="56181-120">Azure Key Vault</span></span>
* <span data-ttu-id="56181-121">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="56181-121">Azure Redis Cache</span></span>
* <span data-ttu-id="56181-122">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="56181-122">Azure Batch</span></span>
* <span data-ttu-id="56181-123">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="56181-123">Azure Traffic Manager</span></span>
* <span data-ttu-id="56181-124">Azure Search</span><span class="sxs-lookup"><span data-stu-id="56181-124">Azure Search</span></span>
* <span data-ttu-id="56181-125">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="56181-125">Azure HDInsight</span></span>

<span data-ttu-id="56181-126"><sup>1</sup> lagringskontonamn måste också vara gemener, 24 tecken eller mindre, och inte har någon bindestreck.</span><span class="sxs-lookup"><span data-stu-id="56181-126"><sup>1</sup> Storage account names also must be lowercase, 24 characters or less, and not have any hyphens.</span></span>

<span data-ttu-id="56181-127">Om du anger en parameter för en resurs måste ange du ett unikt namn när du distribuerar hello resurs.</span><span class="sxs-lookup"><span data-stu-id="56181-127">If you provide a parameter for a resource name, you must provide a unique name when you deploy hello resource.</span></span> <span data-ttu-id="56181-128">Du kan skapa en variabel som använder hello [uniqueString()](resource-group-template-functions-string.md#uniquestring) fungerar toogenerate ett namn.</span><span class="sxs-lookup"><span data-stu-id="56181-128">Optionally, you can create a variable that uses hello [uniqueString()](resource-group-template-functions-string.md#uniquestring) function toogenerate a name.</span></span> 

<span data-ttu-id="56181-129">Du också vill tooadd ett prefix eller suffix toohello **uniqueString** resultat.</span><span class="sxs-lookup"><span data-stu-id="56181-129">You also might want tooadd a prefix or suffix toohello **uniqueString** result.</span></span> <span data-ttu-id="56181-130">Ändra hello unikt namn kan hjälpa dig att enkelt identifiera hello resurstyp från hello namn.</span><span class="sxs-lookup"><span data-stu-id="56181-130">Modifying hello unique name can help you more easily identify hello resource type from hello name.</span></span> <span data-ttu-id="56181-131">Du kan till exempel generera ett unikt namn för ett lagringskonto med hjälp av följande variabel hello:</span><span class="sxs-lookup"><span data-stu-id="56181-131">For example, you can generate a unique name for a storage account by using hello following variable:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a><span data-ttu-id="56181-132">Resursnamn för identifiering</span><span class="sxs-lookup"><span data-stu-id="56181-132">Resource names for identification</span></span>
<span data-ttu-id="56181-133">Vissa typer av resurser du kanske vill tooname, men deras namn har inte unika toobe.</span><span class="sxs-lookup"><span data-stu-id="56181-133">Some resource types you might want tooname, but their names do not have toobe unique.</span></span> <span data-ttu-id="56181-134">Du kan ange ett namn som identifierar både hello resurstyp och hello resurs sammanhang för dessa typer av resurser.</span><span class="sxs-lookup"><span data-stu-id="56181-134">For these resource types, you can provide a name that identifies both hello resource context and hello resource type.</span></span> <span data-ttu-id="56181-135">Ange ett beskrivande namn som hjälper dig att identifiera hello resurs i en lista över resurser.</span><span class="sxs-lookup"><span data-stu-id="56181-135">Provide a descriptive name that helps you identify hello resource in a list of resources.</span></span> <span data-ttu-id="56181-136">Om du behöver toouse ett annat resursnamn för olika distributioner kan använda du en parameter för hello namn:</span><span class="sxs-lookup"><span data-stu-id="56181-136">If you need toouse a different resource name for different deployments, you can use a parameter for hello name:</span></span>

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "hello name of hello VM toocreate."
        }
    }
}
```

<span data-ttu-id="56181-137">Om du inte behöver toopass i ett namn under distributionen kan du använda en variabel:</span><span class="sxs-lookup"><span data-stu-id="56181-137">If you do not need toopass in a name during deployment, you can use a variable:</span></span> 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

<span data-ttu-id="56181-138">Du kan också använda ett hårdkodat värde:</span><span class="sxs-lookup"><span data-stu-id="56181-138">You also can use a hard-coded value:</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a><span data-ttu-id="56181-139">Allmänt namn</span><span class="sxs-lookup"><span data-stu-id="56181-139">Generic resource names</span></span>
<span data-ttu-id="56181-140">Du kan använda ett allmänt namn som är hårdkodat i hello mallen för resurstyper som främst öppnas från en annan resurs.</span><span class="sxs-lookup"><span data-stu-id="56181-140">For resource types that you mostly access through a different resource, you can use a generic name that is hard-coded in hello template.</span></span> <span data-ttu-id="56181-141">Du kan till exempel ange ett standard, allmän namn på brandväggsregler på en SQLServer:</span><span class="sxs-lookup"><span data-stu-id="56181-141">For example, you can set a standard, generic name for firewall rules on a SQL server:</span></span>

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a><span data-ttu-id="56181-142">Parametrar</span><span class="sxs-lookup"><span data-stu-id="56181-142">Parameters</span></span>
<span data-ttu-id="56181-143">hello följande information kan vara användbar när du arbetar med parametrar:</span><span class="sxs-lookup"><span data-stu-id="56181-143">hello following information can be helpful when you work with parameters:</span></span>

* <span data-ttu-id="56181-144">Minimera din användning av parametrar.</span><span class="sxs-lookup"><span data-stu-id="56181-144">Minimize your use of parameters.</span></span> <span data-ttu-id="56181-145">Använd om möjligt en variabel eller ett litteralvärde.</span><span class="sxs-lookup"><span data-stu-id="56181-145">Whenever possible, use a variable or a literal value.</span></span> <span data-ttu-id="56181-146">Använd parametrarna endast för dessa scenarier:</span><span class="sxs-lookup"><span data-stu-id="56181-146">Use parameters only for these scenarios:</span></span>
   
   * <span data-ttu-id="56181-147">Inställningar som du vill toouse variationer av enligt tooenvironment (SKU, storlek, kapacitet).</span><span class="sxs-lookup"><span data-stu-id="56181-147">Settings that you want toouse variations of according tooenvironment (SKU, size, capacity).</span></span>
   * <span data-ttu-id="56181-148">Resursnamn som du vill toospecify för enkel identifiering.</span><span class="sxs-lookup"><span data-stu-id="56181-148">Resource names that you want toospecify for easy identification.</span></span>
   * <span data-ttu-id="56181-149">Värden som du använder ofta toocomplete andra aktiviteter (till exempel en administratörsanvändarnamnet).</span><span class="sxs-lookup"><span data-stu-id="56181-149">Values that you use frequently toocomplete other tasks (such as an admin user name).</span></span>
   * <span data-ttu-id="56181-150">Hemligheter (till exempel lösenord).</span><span class="sxs-lookup"><span data-stu-id="56181-150">Secrets (such as passwords).</span></span>
   * <span data-ttu-id="56181-151">hello tal eller uppsättning värden toouse när du skapar flera instanser av en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="56181-151">hello number or array of values toouse when you create multiple instances of a resource type.</span></span>
* <span data-ttu-id="56181-152">Slitbanor användningsfall för parameternamn.</span><span class="sxs-lookup"><span data-stu-id="56181-152">Use camel case for parameter names.</span></span>
* <span data-ttu-id="56181-153">Ange en beskrivning av varje parameter i hello metadata:</span><span class="sxs-lookup"><span data-stu-id="56181-153">Provide a description of every parameter in hello metadata:</span></span>

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "hello type of hello new storage account created toostore hello VM disks."
           }
       }
   }
   ```

* <span data-ttu-id="56181-154">Ange standardvärden för parametrar (förutom för lösenord och SSH-nycklar):</span><span class="sxs-lookup"><span data-stu-id="56181-154">Define default values for parameters (except for passwords and SSH keys):</span></span>
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "hello type of hello new storage account created toostore hello VM disks."
            }
        }
   }
   ```

* <span data-ttu-id="56181-155">Använd **SecureString** för alla lösenord och hemligheter:</span><span class="sxs-lookup"><span data-stu-id="56181-155">Use **SecureString** for all passwords and secrets:</span></span> 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "hello value of hello secret toostore in hello vault."
           }
       }
   }
   ```

* <span data-ttu-id="56181-156">Använd inte en parameter toospecify plats när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="56181-156">Whenever possible, don't use a parameter toospecify location.</span></span> <span data-ttu-id="56181-157">Använd i stället hello **plats** egenskapen hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="56181-157">Instead, use hello **location** property of hello resource group.</span></span> <span data-ttu-id="56181-158">Med hjälp av hello **resourceGroup () .location** uttryck för alla resurser som resurser i hello mallen har distribuerats i hello samma plats som hello resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="56181-158">By using hello **resourceGroup().location** expression for all your resources, resources in hello template are deployed in hello same location as hello resource group:</span></span>
   
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
   
   <span data-ttu-id="56181-159">Om en resurstyp stöds i endast ett begränsat antal platser, kanske du vill toospecify en giltig plats direkt i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="56181-159">If a resource type is supported in only a limited number of locations, you might want toospecify a valid location directly in hello template.</span></span> <span data-ttu-id="56181-160">Om du måste använda en **plats** parameter, dela att parametervärdet så mycket som möjligt med resurser som är sannolikt toobe i hello samma plats.</span><span class="sxs-lookup"><span data-stu-id="56181-160">If you must use a **location** parameter, share that parameter value as much as possible with resources that are likely toobe in hello same location.</span></span> <span data-ttu-id="56181-161">Detta minimerar hello antalet gånger som användarna ombeds tooprovide platsinformation.</span><span class="sxs-lookup"><span data-stu-id="56181-161">This minimizes hello number of times users are asked tooprovide location information.</span></span>
* <span data-ttu-id="56181-162">Undvik att använda en parameter eller variabel för hello API-version för en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="56181-162">Avoid using a parameter or variable for hello API version for a resource type.</span></span> <span data-ttu-id="56181-163">Egenskaper för resursen och värdena kan variera efter versionsnummer.</span><span class="sxs-lookup"><span data-stu-id="56181-163">Resource properties and values can vary by version number.</span></span> <span data-ttu-id="56181-164">IntelliSense i en redigerare kan inte fastställa hello rätt schema när hello API-version anges tooa parametern eller variabeln.</span><span class="sxs-lookup"><span data-stu-id="56181-164">IntelliSense in a code editor cannot determine hello correct schema when hello API version is set tooa parameter or variable.</span></span> <span data-ttu-id="56181-165">I stället hello hårdkoda API-versionen i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="56181-165">Instead, hard-code hello API version in hello template.</span></span>

## <a name="variables"></a><span data-ttu-id="56181-166">Variabler</span><span class="sxs-lookup"><span data-stu-id="56181-166">Variables</span></span>
<span data-ttu-id="56181-167">hello följande information kan vara användbar när du arbetar med variabler:</span><span class="sxs-lookup"><span data-stu-id="56181-167">hello following information can be helpful when you work with variables:</span></span>

* <span data-ttu-id="56181-168">Använda variabler för värden att du behöver toouse mer än en gång i en mall.</span><span class="sxs-lookup"><span data-stu-id="56181-168">Use variables for values that you need toouse more than once in a template.</span></span> <span data-ttu-id="56181-169">Om ett värde används bara en gång, gör ett hårdkodat värde din mall enklare tooread.</span><span class="sxs-lookup"><span data-stu-id="56181-169">If a value is used only once, a hard-coded value makes your template easier tooread.</span></span>
* <span data-ttu-id="56181-170">Du kan inte använda hello [referens](resource-group-template-functions-resource.md#reference) funktion i hello **variabler** avsnitt i hello mall.</span><span class="sxs-lookup"><span data-stu-id="56181-170">You cannot use hello [reference](resource-group-template-functions-resource.md#reference) function in hello **variables** section of hello template.</span></span> <span data-ttu-id="56181-171">Hej **referens** funktionen hämtar sitt värde från hello resursens runtime-tillståndet.</span><span class="sxs-lookup"><span data-stu-id="56181-171">hello **reference** function derives its value from hello resource's runtime state.</span></span> <span data-ttu-id="56181-172">Dock matchas variabler under hello inledande tolkning av hello mallen.</span><span class="sxs-lookup"><span data-stu-id="56181-172">However, variables are resolved during hello initial parsing of hello template.</span></span> <span data-ttu-id="56181-173">Skapa värden som behöver hello **referens** funktionen direkt i hello **resurser** eller **matar ut** avsnitt i hello mall.</span><span class="sxs-lookup"><span data-stu-id="56181-173">Construct values that need hello **reference** function directly in hello **resources** or **outputs** section of hello template.</span></span>
* <span data-ttu-id="56181-174">Variabler för resursnamn som måste vara unika, enligt beskrivningen i [resursnamn](#resource-names).</span><span class="sxs-lookup"><span data-stu-id="56181-174">Include variables for resource names that must be unique, as described in [Resource names](#resource-names).</span></span>
* <span data-ttu-id="56181-175">Du kan gruppera variabler i komplexa objekt.</span><span class="sxs-lookup"><span data-stu-id="56181-175">You can group variables into complex objects.</span></span> <span data-ttu-id="56181-176">Använd hello **variable.subentry** formatera tooreference ett värde från ett komplext objekt.</span><span class="sxs-lookup"><span data-stu-id="56181-176">Use hello **variable.subentry** format tooreference a value from a complex object.</span></span> <span data-ttu-id="56181-177">Gruppera variabler kan hjälpa dig att spåra relaterade variabler.</span><span class="sxs-lookup"><span data-stu-id="56181-177">Grouping variables can help you track related variables.</span></span> <span data-ttu-id="56181-178">Det förbättrar också läsbarhet hello mallen.</span><span class="sxs-lookup"><span data-stu-id="56181-178">It also improves readability of hello template.</span></span> <span data-ttu-id="56181-179">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="56181-179">Here's an example:</span></span>
   
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
   > <span data-ttu-id="56181-180">Ett komplext objekt får inte innehålla ett uttryck som refererar till ett värde från ett komplext objekt.</span><span class="sxs-lookup"><span data-stu-id="56181-180">A complex object cannot contain an expression that references a value from a complex object.</span></span> <span data-ttu-id="56181-181">Definiera en separat variabel för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="56181-181">Define a separate variable for this purpose.</span></span>
   > 
   > 
   
     <span data-ttu-id="56181-182">Avancerade exempel på hur komplexa objekt som variabler finns i [dela tillstånd i Azure Resource Manager-mallar](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="56181-182">For advanced examples of using complex objects as variables, see [Share state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="resources"></a><span data-ttu-id="56181-183">Resurser</span><span class="sxs-lookup"><span data-stu-id="56181-183">Resources</span></span>
<span data-ttu-id="56181-184">hello följande information kan vara användbar när du arbetar med resurser:</span><span class="sxs-lookup"><span data-stu-id="56181-184">hello following information can be helpful when you work with resources:</span></span>

* <span data-ttu-id="56181-185">toohelp andra deltagare förstå hello syftet hello resurs, ange **kommentarer** för varje resurs i mallen för hello:</span><span class="sxs-lookup"><span data-stu-id="56181-185">toohelp other contributors understand hello purpose of hello resource, specify **comments** for each resource in hello template:</span></span>
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used toostore hello VM disks.",
         ...
     }
   ]
   ```

* <span data-ttu-id="56181-186">Du kan använda taggar tooadd metadata tooresources.</span><span class="sxs-lookup"><span data-stu-id="56181-186">You can use tags tooadd metadata tooresources.</span></span> <span data-ttu-id="56181-187">Använda metadata tooadd information om dina resurser.</span><span class="sxs-lookup"><span data-stu-id="56181-187">Use metadata tooadd information about your resources.</span></span> <span data-ttu-id="56181-188">Du kan till exempel lägga till metadata toorecord faktureringsinformation för en resurs.</span><span class="sxs-lookup"><span data-stu-id="56181-188">For example, you can add metadata toorecord billing details for a resource.</span></span> <span data-ttu-id="56181-189">Mer information finns i [med hjälp av taggar tooorganize resurserna i Azure](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="56181-189">For more information, see [Using tags tooorganize your Azure resources](resource-group-using-tags.md).</span></span>
* <span data-ttu-id="56181-190">Om du använder en *offentlig slutpunkt* i mallen (till exempel Azure Blob storage offentlig slutpunkt), *gör inte hårdkoda* hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="56181-190">If you use a *public endpoint* in your template (such as an Azure Blob storage public endpoint), *do not hard-code* hello namespace.</span></span> <span data-ttu-id="56181-191">Använd hello **referens** funktionen toodynamically hämta hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="56181-191">Use hello **reference** function toodynamically retrieve hello namespace.</span></span> <span data-ttu-id="56181-192">Du kan använda den här metoden toodeploy hello mallen toodifferent allmänna namnutrymmet miljöer utan att manuellt ändra hello slutpunkt i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="56181-192">You can use this approach toodeploy hello template toodifferent public namespace environments without manually changing hello endpoint in hello template.</span></span> <span data-ttu-id="56181-193">Ange hello API-version toohello samma version som du använder för hello storage-konto i mallen:</span><span class="sxs-lookup"><span data-stu-id="56181-193">Set hello API version toohello same version that you are using for hello storage account in your template:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="56181-194">Om hello storage-konto är distribuerat i hello samma mall som du skapar, behöver du inte toospecify hello leverantörens namnrymd när du refererar till hello resurs.</span><span class="sxs-lookup"><span data-stu-id="56181-194">If hello storage account is deployed in hello same template that you are creating, you do not need toospecify hello provider namespace when you reference hello resource.</span></span> <span data-ttu-id="56181-195">Detta är hello förenklad syntax:</span><span class="sxs-lookup"><span data-stu-id="56181-195">This is hello simplified syntax:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="56181-196">Om du har andra värden i mallen som är konfigurerade toouse offentliga namnområde kan ändra dessa värden tooreflect hello samma **referens** funktion.</span><span class="sxs-lookup"><span data-stu-id="56181-196">If you have other values in your template that are configured toouse a public namespace, change these values tooreflect hello same **reference** function.</span></span> <span data-ttu-id="56181-197">Du kan till exempel ange hello **storageUri** -egenskapen för hello diagnostikprofilen för virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="56181-197">For example, you can set hello **storageUri** property of hello virtual machine diagnostics profile:</span></span>
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   <span data-ttu-id="56181-198">Du kan också referera till ett befintligt lagringskonto i en annan resursgrupp:</span><span class="sxs-lookup"><span data-stu-id="56181-198">You also can reference an existing storage account that is in a different resource group:</span></span>

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* <span data-ttu-id="56181-199">Tilldela offentliga IP-adresser tooa virtuella datorn endast när ett program kräver.</span><span class="sxs-lookup"><span data-stu-id="56181-199">Assign public IP addresses tooa virtual machine only when an application requires it.</span></span> <span data-ttu-id="56181-200">inkommande NAT-regler, en virtuell nätverksgateway eller en jumpbox Använd tooconnect tooa virtuell dator (VM) för felsökning eller för att hantera administrativa syften.</span><span class="sxs-lookup"><span data-stu-id="56181-200">tooconnect tooa virtual machine (VM) for debugging, or for management or administrative purposes, use inbound NAT rules, a virtual network gateway, or a jumpbox.</span></span>
   
     <span data-ttu-id="56181-201">Mer information om hur du ansluter datorer toovirtual finns:</span><span class="sxs-lookup"><span data-stu-id="56181-201">For more information about connecting toovirtual machines, see:</span></span>
   
   * [<span data-ttu-id="56181-202">Köra virtuella datorer för en arkitektur med flera nivåer i Azure</span><span class="sxs-lookup"><span data-stu-id="56181-202">Run VMs for an N-tier architecture in Azure</span></span>](../guidance/guidance-compute-n-tier-vm.md)
   * [<span data-ttu-id="56181-203">Konfigurera WinRM-åtkomst för virtuella datorer i Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="56181-203">Set up WinRM access for VMs in Azure Resource Manager</span></span>](../virtual-machines/windows/winrm.md)
   * [<span data-ttu-id="56181-204">Tillåt extern åtkomst tooyour VM med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="56181-204">Allow external access tooyour VM by using hello Azure portal</span></span>](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [<span data-ttu-id="56181-205">Tillåt extern åtkomst tooyour VM med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="56181-205">Allow external access tooyour VM by using PowerShell</span></span>](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [<span data-ttu-id="56181-206">Tillåt extern åtkomst tooyour Linux VM med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="56181-206">Allow external access tooyour Linux VM by using Azure CLI</span></span>](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* <span data-ttu-id="56181-207">Hej **domainNameLabel** -egenskapen för offentliga IP-adresser måste vara unika.</span><span class="sxs-lookup"><span data-stu-id="56181-207">hello **domainNameLabel** property for public IP addresses must be unique.</span></span> <span data-ttu-id="56181-208">Hej **domainNameLabel** värdet måste vara mellan 3 och 63 tecken långt och följa hello regler som anges av den här reguljärt uttryck: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span><span class="sxs-lookup"><span data-stu-id="56181-208">hello **domainNameLabel** value must be between 3 and 63 characters long, and follow hello rules specified by this regular expression: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span></span> <span data-ttu-id="56181-209">Eftersom hello **uniqueString** funktionen genererar en sträng som 13 tecken, hello **dnsPrefixString** parametern är begränsad too50 tecken:</span><span class="sxs-lookup"><span data-stu-id="56181-209">Because hello **uniqueString** function generates a string that is 13 characters long, hello **dnsPrefixString** parameter is limited too50 characters:</span></span>

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "hello DNS label for hello public IP address. It must be lowercase. It should match hello following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* <span data-ttu-id="56181-210">När du lägger till ett lösenord tooa-tillägget för anpassat skript använder hello **commandToExecute** egenskap i hello **protectedSettings** egenskapen:</span><span class="sxs-lookup"><span data-stu-id="56181-210">When you add a password tooa custom script extension, use hello **commandToExecute** property in hello **protectedSettings** property:</span></span>
   
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
   > <span data-ttu-id="56181-211">tooensure hemligheter är krypterad när de överförs som parametrar tooVMs-tillägg, använda hello **protectedSettings** -egenskapen för hello relevanta tillägg.</span><span class="sxs-lookup"><span data-stu-id="56181-211">tooensure that secrets are encrypted when they are passed as parameters tooVMs and extensions, use hello **protectedSettings** property of hello relevant extensions.</span></span>
   > 
   > 

## <a name="outputs"></a><span data-ttu-id="56181-212">utdata</span><span class="sxs-lookup"><span data-stu-id="56181-212">Outputs</span></span>
<span data-ttu-id="56181-213">Om du använder en mall toocreate offentliga IP-adresser kan innehålla en **matar ut** avsnitt som returnerar information om hello IP-adress och hello fullständigt kvalificerade domännamnet (FQDN).</span><span class="sxs-lookup"><span data-stu-id="56181-213">If you use a template toocreate public IP addresses, include an **outputs** section that returns details of hello IP address and hello fully qualified domain name (FQDN).</span></span> <span data-ttu-id="56181-214">Du kan använda utdata värden tooeasily hämta information om offentlig IP-adresser och FQDN efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="56181-214">You can use output values tooeasily retrieve details about public IP addresses and FQDNs after deployment.</span></span> <span data-ttu-id="56181-215">När du refererar till hello resurs, använda hello API-version som du använde toocreate den:</span><span class="sxs-lookup"><span data-stu-id="56181-215">When you reference hello resource, use hello API version that you used toocreate it:</span></span> 

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

## <a name="single-template-vs-nested-templates"></a><span data-ttu-id="56181-216">Samma mall kontra kapslade mallar</span><span class="sxs-lookup"><span data-stu-id="56181-216">Single template vs. nested templates</span></span>
<span data-ttu-id="56181-217">toodeploy din lösning du kan använda en mall eller en Huvudmall med flera kapslade mallar.</span><span class="sxs-lookup"><span data-stu-id="56181-217">toodeploy your solution, you can use either a single template or a main template with multiple nested templates.</span></span> <span data-ttu-id="56181-218">Kapslade mallar är gemensamma för mer avancerade scenarier.</span><span class="sxs-lookup"><span data-stu-id="56181-218">Nested templates are common for more advanced scenarios.</span></span> <span data-ttu-id="56181-219">Använda en kapslad mall får du hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="56181-219">Using a nested template gives you hello following advantages:</span></span>

* <span data-ttu-id="56181-220">Du kan dela upp en lösning i riktade komponenter.</span><span class="sxs-lookup"><span data-stu-id="56181-220">You can break down a solution into targeted components.</span></span>
* <span data-ttu-id="56181-221">Du kan återanvända kapslade mallar med olika huvudsakliga mallar.</span><span class="sxs-lookup"><span data-stu-id="56181-221">You can reuse nested templates with different main templates.</span></span>

<span data-ttu-id="56181-222">Om du väljer toouse kapslade mallar hjälper hello följande riktlinjer dig att standardisera utformningen mallen.</span><span class="sxs-lookup"><span data-stu-id="56181-222">If you choose toouse nested templates, hello following guidelines can help you standardize your template design.</span></span> <span data-ttu-id="56181-223">Dessa riktlinjer är baserade på [mönster för att utforma Azure Resource Manager-mallar](best-practices-resource-manager-design-templates.md).</span><span class="sxs-lookup"><span data-stu-id="56181-223">These guidelines are based on [patterns for designing Azure Resource Manager templates](best-practices-resource-manager-design-templates.md).</span></span> <span data-ttu-id="56181-224">Vi rekommenderar en design som har hello följande mallar:</span><span class="sxs-lookup"><span data-stu-id="56181-224">We recommend a design that has hello following templates:</span></span>

* <span data-ttu-id="56181-225">**Huvudmall** (azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="56181-225">**Main template** (azuredeploy.json).</span></span> <span data-ttu-id="56181-226">Använd för hello indataparametrar.</span><span class="sxs-lookup"><span data-stu-id="56181-226">Use for hello input parameters.</span></span>
* <span data-ttu-id="56181-227">**Delade resurser mallen**.</span><span class="sxs-lookup"><span data-stu-id="56181-227">**Shared resources template**.</span></span> <span data-ttu-id="56181-228">Använd toodeploy delade resurser med andra resurser (till exempel virtuella nätverk och tillgänglighet uppsättningar).</span><span class="sxs-lookup"><span data-stu-id="56181-228">Use toodeploy shared resources that all other resources use (for example, virtual network and availability sets).</span></span> <span data-ttu-id="56181-229">Använd hello **dependsOn** uttryck tooensure att distribuera den här mallen innan andra mallar.</span><span class="sxs-lookup"><span data-stu-id="56181-229">Use hello **dependsOn** expression tooensure that this template is deployed before other templates.</span></span>
* <span data-ttu-id="56181-230">**Valfria resurser mallen**.</span><span class="sxs-lookup"><span data-stu-id="56181-230">**Optional resources template**.</span></span> <span data-ttu-id="56181-231">Använd tooconditionally distribuera resurser baserat på en parameter (till exempel jumpbox).</span><span class="sxs-lookup"><span data-stu-id="56181-231">Use tooconditionally deploy resources based on a parameter (for example, a jumpbox).</span></span>
* <span data-ttu-id="56181-232">**Medlemmen resurser mallen**.</span><span class="sxs-lookup"><span data-stu-id="56181-232">**Member resources template**.</span></span> <span data-ttu-id="56181-233">Varje instans i en program-nivå har sin egen konfiguration.</span><span class="sxs-lookup"><span data-stu-id="56181-233">Each instance type within an application tier has its own configuration.</span></span> <span data-ttu-id="56181-234">Du kan definiera olika instans typer i en nivå.</span><span class="sxs-lookup"><span data-stu-id="56181-234">Within a tier, you can define different instance types.</span></span> <span data-ttu-id="56181-235">(Till exempel hello första instansen skapar ett kluster, och ytterligare instanser läggs toohello befintligt kluster.) Varje instans har sin egen Distributionsmall.</span><span class="sxs-lookup"><span data-stu-id="56181-235">(For example, hello first instance creates a cluster, and additional instances are added toohello existing cluster.) Each instance type has its own deployment template.</span></span>
* <span data-ttu-id="56181-236">**Skript**.</span><span class="sxs-lookup"><span data-stu-id="56181-236">**Scripts**.</span></span> <span data-ttu-id="56181-237">Allmänt återanvändbara skript kan användas för varje instanstyp (till exempel initiera och formatera ytterligare diskar).</span><span class="sxs-lookup"><span data-stu-id="56181-237">Widely reusable scripts are applicable for each instance type (for example, initialize and format additional disks).</span></span> <span data-ttu-id="56181-238">Anpassade skript som du skapar för en viss anpassning syfte är olika, baserat på hello instanstyp.</span><span class="sxs-lookup"><span data-stu-id="56181-238">Custom scripts that you create for a specific customization purpose are different, based on hello instance type.</span></span>

![Kapslad mall](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

<span data-ttu-id="56181-240">Mer information finns i [använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="56181-240">For more information, see [Use linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="conditionally-link-toonested-templates"></a><span data-ttu-id="56181-241">Villkorligt länka toonested mallar</span><span class="sxs-lookup"><span data-stu-id="56181-241">Conditionally link toonested templates</span></span>
<span data-ttu-id="56181-242">Du kan använda parametern tooconditionally länk toonested mallar.</span><span class="sxs-lookup"><span data-stu-id="56181-242">You can use a parameter tooconditionally link toonested templates.</span></span> <span data-ttu-id="56181-243">hello parametern blir en del av hello URI för hello mallen:</span><span class="sxs-lookup"><span data-stu-id="56181-243">hello parameter becomes part of hello URI for hello template:</span></span>

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

## <a name="template-format"></a><span data-ttu-id="56181-244">Mallformat</span><span class="sxs-lookup"><span data-stu-id="56181-244">Template format</span></span>
<span data-ttu-id="56181-245">Det är en bra idé toopass din mall med en JSON-verifieraren.</span><span class="sxs-lookup"><span data-stu-id="56181-245">It's a good practice toopass your template through a JSON validator.</span></span> <span data-ttu-id="56181-246">Med hjälp av en systemhälsoverifierare kan du ta bort överflödig kommatecken, parenteser och hakparenteser som kan orsaka fel under distributionen.</span><span class="sxs-lookup"><span data-stu-id="56181-246">A validator can help you remove extraneous commas, parentheses, and brackets that might cause an error during deployment.</span></span> <span data-ttu-id="56181-247">Försök [JSONLint](http://jsonlint.com/) eller ett linter paket för din favorit redigerar miljö (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="56181-247">Try [JSONLint](http://jsonlint.com/) or a linter package for your favorite editing environment (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span></span>

<span data-ttu-id="56181-248">Det är också en bra idé tooformat din JSON för bättre läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="56181-248">It's also a good idea tooformat your JSON for better readability.</span></span> <span data-ttu-id="56181-249">Du kan använda en JSON-Formateraren paketet för din lokala redigeraren.</span><span class="sxs-lookup"><span data-stu-id="56181-249">You can use a JSON formatter package for your local editor.</span></span> <span data-ttu-id="56181-250">I Visual Studio tooformat hello dokumentet trycker du på **Ctrl + K, Ctrl + D**.</span><span class="sxs-lookup"><span data-stu-id="56181-250">In Visual Studio, tooformat hello document, press **Ctrl+K, Ctrl+D**.</span></span> <span data-ttu-id="56181-251">I Visual Studio-koden trycker du på **Alt + Skift + F**.</span><span class="sxs-lookup"><span data-stu-id="56181-251">In Visual Studio Code, press **Alt+Shift+F**.</span></span> <span data-ttu-id="56181-252">Om din lokala redigeraren inte formateras hello dokumentet, kan du använda en [online formaterare](https://www.bing.com/search?q=json+formatter).</span><span class="sxs-lookup"><span data-stu-id="56181-252">If your local editor doesn't format hello document, you can use an [online formatter](https://www.bing.com/search?q=json+formatter).</span></span>

## <a name="next-steps"></a><span data-ttu-id="56181-253">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="56181-253">Next steps</span></span>
* <span data-ttu-id="56181-254">Anvisningar om att utforma din lösning för virtuella datorer, se [kör en virtuell Windows-dator i Azure](../guidance/guidance-compute-single-vm.md) och [kör ett Linux-VM i Azure](../guidance/guidance-compute-single-vm-linux.md).</span><span class="sxs-lookup"><span data-stu-id="56181-254">For guidance on architecting your solution for virtual machines, see [Run a Windows VM in Azure](../guidance/guidance-compute-single-vm.md) and [Run a Linux VM in Azure](../guidance/guidance-compute-single-vm-linux.md).</span></span>
* <span data-ttu-id="56181-255">Anvisningar om hur du skapar ett lagringskonto finns [Azure Storage checklistan för prestanda och skalbarhet](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="56181-255">For guidance on setting up a storage account, see [Azure Storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>
* <span data-ttu-id="56181-256">toolearn om hur företaget kan använda Resource Manager tooeffectively hantera prenumerationer, se [Azure enterprise kodskelett: normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="56181-256">toolearn about how an enterprise can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold: Prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

